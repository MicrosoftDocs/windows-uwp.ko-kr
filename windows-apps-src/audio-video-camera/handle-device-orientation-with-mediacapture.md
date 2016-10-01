---
author: drewbatgit
ms.assetid: 
description: "이 문서에서는 도우미 클래스를 사용하여 사진 및 비디오를 캡처할 때 디바이스 방향을 처리하는 방법을 보여 줍니다."
title: "MediaCapture를 사용하여 디바이스 방향 처리"
translationtype: Human Translation
ms.sourcegitcommit: 9f1d65d73bdf50697d75b0d57429aed66898e1b5
ms.openlocfilehash: eb6487e7f2c19a8227320c5a7f087e4b3c3c6270

---

# MediaCapture를 사용하여 디바이스 방향 처리
앱이 사용자 디바이스의 파일에 저장하거나 온라인으로 공유하는 등 앱 외부에서 보려는 사진 또는 비디오를 캡처하는 경우 다른 앱이나 디바이스에서 이미지를 표시할 때 올바른 방향으로 표시되도록 적절한 방향 메타데이터를 사용하여 이미지를 인코드하는 것이 중요합니다. 디바이스 섀시 방향, 디스플레이 방향, 섀시의 카메라 배치(전면 또는 후면 카메라) 등 고려할 여러 가지 변수가 있기 때문에 미디어 파일에 포함할 올바른 방향 데이터를 결정하는 작업은 복잡할 수 있습니다. 

방향 처리 프로세스를 단순화하려면 이 문서의 끝에 전체 정의가 나와 있는 도우미 클래스 **CameraRotationHelper**를 사용하는 것이 좋습니다. 이 클래스를 프로젝트에 추가한 다음 이 문서의 단계에 따라 카메라 앱에 방향 지원을 추가할 수 있습니다. 도우미 클래스는 사용자 관점에서 올바르게 렌더링되도록 카메라 UI에서 컨트롤을 쉽게 회전하는 데에도 도움이 됩니다.

> [!NOTE] 
> 이 문서는 [**MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md) 문서에서 설명한 코드 및 개념을 토대로 작성되었습니다. 앱에 방향 지원을 추가하기 전에 **MediaCapture** 클래스 사용의 기본 개념을 파악하는 것이 좋습니다.

## 이 문서에서 사용된 네임스페이스
이 문서의 예제 코드는 코드에 포함해야 하는 다음 네임스페이스의 API를 사용합니다. 

[!code-cs[OrientationUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOrientationUsing)]

앱에 방향 지원을 추가하는 과정의 첫 번째 단계는 디바이스를 회전할 때 자동으로 회전하지 않도록 디스플레이를 잠그는 것입니다. 자동 UI 회전은 대부분의 앱 유형에서 잘 작동하지만 카메라 미리 보기가 회전하는 경우 사용자에게 불편합니다. [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences) 속성을 [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations)로 설정하여 디스플레이 방향을 잠급니다. 

[!code-cs[AutoRotationPreference](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAutoRotationPreference)]

## 카메라 디바이스 위치 추적
캡처된 미디어의 올바른 방향을 계산하려면 앱에서 섀시의 카메라 디바이스 위치를 확인해야 합니다. USB 웹캠과 같이 카메라가 디바이스 외부에 있는지 여부를 추적하는 부울 멤버 변수를 추가합니다. 미리 보기를 미러해야 하는지 여부(전면 카메라가 사용되는 경우)를 추적하는 다른 부울 변수를 추가합니다. 또한 선택한 카메라를 나타내는 **DeviceInformation** 개체를 저장하기 위한 변수를 추가합니다.

[!code-cs[CameraDeviceLocationBools](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCameraDeviceLocationBools)]
[!code-cs[DeclareCameraDevice](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareCameraDevice)]

## 카메라 디바이스 선택 및 MediaCapture 개체 초기화
[**MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md) 문서에서는 몇 줄의 코드만으로 **MediaCapture** 개체를 초기화하는 방법을 보여 줍니다. 카메라 방향을 지원하려면 초기화 프로세스에 몇 단계를 더 추가합니다.

먼저 [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync)를 호출하고 디바이스 선택기 [**DeviceClass.VideoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceClass)를 전달하여 사용 가능한 모든 비디오 캡처 디바이스 목록을 가져옵니다. 그런 다음 목록에서 카메라의 패널 위치가 알려져 있고 제공된 값과 일치하는 첫 번째 디바이스(이 예제에서는 전면 카메라)를 선택합니다. 원하는 패널에 카메라가 없는 경우 사용 가능한 첫 번째 또는 기본 카메라가 사용됩니다.

카메라 디바이스를 발견한 경우 새 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings) 개체가 생성되고 [**VideoDeviceId**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.VideoDeviceId) 속성이 선택한 디바이스로 설정됩니다. MediaCapture 개체를 만든 다음 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync)를 호출하고 설정 개체를 전달하여 선택한 카메라를 사용하도록 시스템에 알립니다.

마지막으로, 선택한 디바이스 패널이 null 또는 알 수 없음인지 확인합니다. null 또는 알 수 없음이면 외부 카메라이므로 카메라 회전은 디바이스 회전과 관련이 없습니다. 패널이 알려져 있고 디바이스 섀시의 전면에 있으면 미리 보기를 미러해야 하므로 이를 추적하는 변수가 설정됩니다.

[!code-cs[InitMediaCaptureWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCaptureWithOrientation)]
## CameraRotationHelper 클래스 초기화

이제 **CameraRotationHelper** 클래스를 사용합니다. 개체를 저장할 클래스 멤버 변수를 선언합니다. 생성자를 호출하고 선택한 카메라의 엔클로저 위치를 전달합니다. 도우미 클래스는 이 정보를 사용하여 캡처된 미디어, 미리 보기 스트림 및 UI의 올바른 방향을 계산합니다. UI 또는 미리 보기 스트림의 방향을 업데이트해야 하는 경우 발생하는 도우미 클래스의 **OrientationChanged** 이벤트 처리기를 등록합니다.

[!code-cs[DeclareRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareRotationHelper)]

[!code-cs[InitRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitRotationHelper)]

## 카메라 미리 보기 스트림에 방향 데이터 추가
미리 보기 스트림의 메타데이터에 올바른 방향을 추가해도 미리 보기가 사용자에게 표시되는 모양에는 영향을 주지 않지만 시스템이 미리 보기 스트림에서 캡처된 프레임을 올바르게 인코드하는 데 도움이 됩니다.

[**MediaCapture.StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartPreviewAsync)를 호출하여 카메라 미리 보기를 시작합니다. 이 작업을 수행하기 전에 멤버 변수를 검사하여 미리 보기를 미러해야 하는지 여부(전면 카메라)를 확인합니다. 미러해야 하는 경우 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.CaptureElement)(이 예제에서는 *PreviewControl*로 명명됨)의 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FlowDirection)로 설정합니다. 미리 보기를 시작한 후 도우미 메서드 **SetPreviewRotationAsync**를 호출하여 미리 보기 회전을 설정합니다. 다음은 이 메서드의 구현입니다.

[!code-cs[StartPreviewWithRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewWithRotationAsync)]

휴대폰 방향이 변경될 경우 미리 보기 스트림을 다시 초기화하지 않고 업데이트될 수 있도록 별도 메서드에서 미리 보기 회전을 설정합니다. 카메라가 디바이스 외부에 있으면 아무 작업도 수행되지 않습니다. 외부 카메라가 아닌 경우 **CameraRotationHelper** 메서드 **GetCameraPreviewOrientation**이 호출되고 미리 보기 스트림의 적절한 방향을 반환합니다. 

메타데이터를 설정하기 위해 [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.VideoDeviceController.GetMediaStreamProperties)를 호출하여 미리 보기 스트림 속성이 검색됩니다. 비디오 스트림 회전의 MFT(Media Foundation Transform) 특성을 나타내는 GUID를 만듭니다. C++에서는 [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh162880.aspx) 상수를 사용할 수 있지만 C#에서는 GUID 값을 수동으로 지정해야 합니다. 

GUID를 키로 지정하고 미리 보기 회전을 값으로 지정하여 스트림 속성 개체에 속성 값을 추가합니다. 이 속성 값은 시계 반대 방향의 도 단위여야 하므로 **CameraRotationHelper** 메서드 **ConvertSimpleOrientationToClockwiseDegrees**를 사용하여 단순 방향 값이 변환됩니다. 마지막으로, [**SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.SetEncodingPropertiesAsync(Windows.Media.Capture.MediaStreamType,Windows.Media.MediaProperties.IMediaEncodingProperties,Windows.Media.MediaProperties.MediaPropertySet))를 호출하여 새 회전 속성을 스트림에 적용합니다.

[!code-cs[SetPreviewRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSetPreviewRotationAsync)]

**CameraRotationHelper.OrientationChanged** 이벤트 처리기를 추가합니다. 이 이벤트는 미리 보기 스트림을 회전해야 하는지 여부를 알리는 인수를 전달합니다. 디바이스 방향을 앞면이 위로 또는 앞면이 아래로 향하도록 변경한 경우 이 값은 false가 됩니다. 미리 보기를 회전할 필요가 없으면 앞에서 정의한 **SetPreviewRotationAsync**를 호출합니다.

필요한 경우 **OrientationChanged** 이벤트 처리기에서 UI를 업데이트합니다. **GetUIOrientation**을 호출하여 도우미 클래스에서 현재 권장 UI 방향을 가져오고 이 값을 XAML 변형에 사용되는 시계 방향의 도로 변환합니다. 방향 값에서 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.RotateTransform)을 만들고 XAML 컨트롤의 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.RenderTransform) 속성을 설정합니다. UI 레이아웃에 따라 단순히 컨트롤을 회전하는 것뿐 아니라 여기서 추가로 조정해야 할 수도 있습니다. 또한 모든 UI 업데이트는 UI 스레드에서 수행해야 하므로 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority,Windows.UI.Core.DispatchedHandler)) 호출 내부에 이 코드를 배치해야 합니다.  

[!code-cs[HelperOrientationChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetHelperOrientationChanged)]

## 방향 데이터를 사용하여 사진 캡처
[**MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md) 문서에서는 먼저 메모리 내 스트림에 캡처한 다음 디코더를 통해 스트림에서 이미지 데이터를 읽고 인코더를 통해 이미지 데이터를 파일로 코드 변환하여 사진을 파일에 캡처하는 방법을 보여 줍니다. **CameraRotationHelper** 클래스에서 가져온 방향 데이터를 코드 변환 작업 중 이미지 파일에 추가할 수 있습니다.

다음 예제에서는 [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync)를 호출하여 사진을 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream)에 캡처하고 스트림에서 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder)를 만듭니다. [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile)을 만든 다음 열어서 파일에 쓸 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.IRandomAccessStream)을 검색합니다. 

파일을 코드 변환하기 전에 도우미 클래스 메서드 **GetCameraCaptureOrientation**에서 사진 방향을 검색합니다. 이 메서드는 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientation) 개체를 반환하고, 해당 개체는 도우미 메서드 **ConvertSimpleOrientationToPhotoOrientation**을 사용하여 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.FileProperties.PhotoOrientation) 개체로 변환됩니다. 새 **BitmapPropertySet** 개체를 만들고, 키가 "System.Photo.Orientation"이고 값이 **BitmapTypedValue**로 표시된 사진 방향인 속성을 추가합니다. "System.Photo.Orientation"은 이미지 파일에 메타데이터로 추가될 수 있는 많은 Windows 속성 중 하나입니다. 모든 사진 관련 속성 목록은 [**Windows 속성 - 사진**](https://msdn.microsoft.com/en-us/library/windows/desktop/ff516600)을 참조하세요. 이미지의 메타데이터 작업 방법에 대한 자세한 내용은 [**이미지 메타데이터**](image-metadata.md)를 참조하세요.

마지막으로, [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/)를 호출하여 방향 데이터가 포함된 속성 집합을 인코더에 대해 설정하고 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync)를 호출하여 이미지를 코드 변환합니다.

[!code-cs[CapturePhotoWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCapturePhotoWithOrientation)]

## 방향 데이터를 사용하여 비디오 캡처
기본적인 비디오 캡처는 [**MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md) 문서에서 설명합니다. 캡처된 비디오의 인코딩에 방향 데이터를 추가하는 경우 앞의 미리 보기 스트림에 방향 데이터 추가 섹션에서 설명한 것과 동일한 기술을 사용합니다.

다음 예제에서는 캡처된 비디오를 쓸 파일을 만듭니다. 정적 메서드 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4)를 사용하여 MP4 인코딩 프로필을 만듭니다. **GetCameraCaptureOrientation**을 호출하여 **CameraRotationHelper** 클래스에서 적절한 비디오 방향을 가져옵니다. 회전 속성에 따라 방향을 시계 반대 방향의 도로 표시해야 하므로 **ConvertSimpleOrientationToClockwiseDegrees** 도우미 메서드를 호출하여 방향 값을 변환합니다. 비디오 스트림 회전의 MFT(Media Foundation Transform) 특성을 나타내는 GUID를 만듭니다. C++에서는 [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh162880.aspx) 상수를 사용할 수 있지만 C#에서는 GUID 값을 수동으로 지정해야 합니다. GUID를 키로 지정하고 회전을 값으로 지정하여 스트림 속성 개체에 속성 값을 추가합니다. 마지막으로, [**StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartRecordToStorageFileAsync(Windows.Media.MediaProperties.MediaEncodingProfile,Windows.Storage.IStorageFile))를 호출하여 방향 데이터로 인코드된 비디오 녹화를 시작합니다.

[!code-cs[StartRecordingWithOrientationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartRecordingWithOrientationAsync)]

## CameraRotationHelper 전체 코드 목록
다음 코드 조각은 하드웨어 방향 센서를 관리하고, 사진 및 비디오의 적절한 방향 값을 계산하고, 각 Windows 기능에서 사용되는 다양한 방향 표현 간에 변환하는 도우미 메서드를 제공하는 **CameraRotationHelper** 클래스의 전체 코드를 보여 줍니다. 위의 문서에 있는 지침을 따를 경우 이 클래스를 변경하지 않고 현재 상태대로 프로젝트에 추가할 수 있습니다. 물론, 특정 시나리오의 요구에 맞게 다음 코드를 원하는 대로 사용자 지정할 수 있습니다.

이 도우미 클래스는 디바이스의 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientationSensor)를 사용하여 디바이스 섀시의 현재 방향을 확인하고, [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation) 클래스를 사용하여 디스플레이의 현재 방향을 확인합니다. 각 클래스는 현재 방향이 변경될 때 발생하는 이벤트를 제공합니다. 캡처 디바이스가 탑재된 패널(전면, 후면 또는 외부)을 사용하여 미리 보기 스트림을 미러해야 하는지 여부를 확인합니다. 또한 일부 디바이스에서 지원되는 [**EnclosureLocation.RotationAngleInDegreesClockwise**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.EnclosureLocation.RotationAngleInDegreesClockwise) 속성을 사용하여 카메라가 섀시에 탑재된 방향을 확인합니다.

다음 메서드를 사용하여 지정한 카메라 앱 작업에 대한 권장 방향 값을 가져올 수 있습니다.

* **GetUIOrientation** - 카메라 UI 요소에 대해 제안된 방향을 반환합니다.
* **GetCameraCaptureOrientation** - 이미지 메타데이터로 인코드하는 경우의 제안된 방향을 반환합니다.
* **GetCameraPreviewOrientation** - 자연스러운 사용자 환경을 제공하기 위해 미리 보기 스트림에 대해 제안된 방향을 반환합니다.

[!code-cs[CameraRotationHelperFull](./code/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs#SnippetCameraRotationHelperFull)]



## 관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 







<!--HONumber=Aug16_HO3-->


