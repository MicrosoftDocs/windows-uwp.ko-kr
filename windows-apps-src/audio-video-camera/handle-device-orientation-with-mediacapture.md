---
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: 이 문서에서는 도우미 클래스를 사용 하 여 사진 및 비디오를 캡처할 때 장치 방향을 처리 하는 방법을 보여 줍니다.
title: MediaCapture를 사용 하 여 장치 방향 처리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18e135e85902bb3e00b7a09b8ee98a4e02b71f15
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362646"
---
# <a name="handle-device-orientation-with-mediacapture"></a>MediaCapture를 사용 하 여 장치 방향 처리
앱이 사용자 장치의 파일에 저장 하거나 온라인으로 공유 하는 등 앱 외부에서 볼 수 있는 사진이 나 비디오를 캡처하는 경우에는 적절 한 방향 메타 데이터를 사용 하 여 이미지를 인코딩하면 다른 응용 프로그램이 나 장치에서 이미지를 표시할 때 올바르게 표시 됩니다. 미디어 파일에 포함할 올바른 방향 데이터를 결정 하는 것은 복잡 한 작업 일 수 있습니다. 예를 들어 장치 섀시의 방향, 디스플레이 방향, 섀시에 카메라 배치 (앞면 또는 후면 카메라)를 비롯 한 여러 가지 사항을 고려해 야 합니다. 

방향 처리 과정을 간소화 하려면이 문서의 끝에 전체 정의가 제공 되는 도우미 클래스 **CameraRotationHelper**를 사용 하는 것이 좋습니다. 이 클래스를 프로젝트에 추가한 다음이 문서의 단계에 따라 카메라 앱에 방향 지원을 추가할 수 있습니다. 또한 도우미 클래스를 사용 하면 사용자의 관점에서 올바르게 렌더링 되도록 카메라 UI에서 컨트롤을 보다 쉽게 회전할 수 있습니다.

> [!NOTE] 
> 이 문서는 [**MediaCapture를 사용 하 여 기본 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md)문서에서 설명 하는 코드 및 개념을 기반으로 합니다. 응용 프로그램에 방향 지원을 추가 하기 전에 **MediaCapture** 클래스를 사용 하는 기본 개념을 숙지 하는 것이 좋습니다.

## <a name="namespaces-used-in-this-article"></a>이 문서에서 사용 되는 네임 스페이스
이 문서의 예제 코드에서는 다음 네임 스페이스의 Api를 사용 하 여 코드에 포함 해야 합니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetOrientationUsing":::

응용 프로그램에 방향 지원을 추가 하는 첫 번째 단계는 장치가 회전 될 때 자동으로 회전 하지 않도록 디스플레이를 잠그는 것입니다. 자동 UI 회전은 대부분의 앱 유형에 적합 하지만 카메라 미리 보기가 회전 하는 경우 사용자에 게는 unintuitive 됩니다. Displayproperties.autorotationpreferences 속성을 [**DisplayInformation**](/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) [**로 설정**](/uwp/api/Windows.Graphics.Display.DisplayOrientations)하 여 표시 방향을 잠급니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetAutoRotationPreference":::

## <a name="tracking-the-camera-device-location"></a>카메라 장치 위치 추적
캡처한 미디어의 올바른 방향을 계산 하려면 앱이 섀시에 있는 카메라 장치의 위치를 확인 해야 합니다. 카메라를 장치 외부 (예: USB 웹 캠)에서 외부에 있는지를 추적 하는 부울 구성원 변수를 추가 합니다. 다른 부울 변수를 추가 하 여 미리 보기가 미러링 되어야 하는지 여부를 추적 합니다 .이 경우 전면 카메라가 사용 됩니다. 또한 선택한 카메라를 나타내는 **Deviceinformation** 개체를 저장 하는 변수를 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCameraDeviceLocationBools":::
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareCameraDevice":::

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>카메라 장치를 선택 하 고 MediaCapture 개체를 초기화 합니다.
[**MediaCapture를 사용한 기본 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md) 문서에서는 몇 줄의 코드를 사용 하 여 **MediaCapture** 개체를 초기화 하는 방법을 보여 줍니다. 카메라 방향을 지원 하기 위해 초기화 프로세스에 몇 가지 단계를 더 추가 합니다.

먼저 장치 선택기 FindAllAsync를 전달 하 여 사용 가능한 모든 비디오 캡처 장치 목록을 가져오는 [**Deviceinformation.**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) [**VideoCapture**](/uwp/api/Windows.Devices.Enumeration.DeviceClass) 을 호출 합니다. 그런 다음 목록에서 카메라의 패널 위치가 알려져 있고이 값이 제공 된 값과 일치 하는 첫 번째 장치 (이 예제에서는 전면 카메라)를 선택 합니다. 원하는 패널에 카메라가 없으면 사용 가능한 첫 번째 카메라 또는 기본 카메라를 사용 합니다.

카메라 장치가 발견 되 면 새 [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) 개체가 만들어지고 [**videodeviceid**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videodeviceid) 속성이 선택한 장치로 설정 됩니다. 그런 다음 MediaCapture 개체를 만들고 설정 개체 [**를 전달**](/uwp/api/windows.media.capture.mediacapture.initializeasync)하 여 시스템에 선택한 카메라를 사용 하도록 지시 합니다.

마지막으로, 선택한 장치 패널이 null 인지 알 수 없는 지 확인 합니다. 이 경우 카메라는 외부에 있습니다. 즉, 해당 회전이 장치의 회전과 관련이 없는 것입니다. 패널이 알려져 있고 장치 섀시 앞에 있는 경우 미리 보기를 미러링 해야 하므로이를 추적 하는 변수가 설정 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitMediaCaptureWithOrientation":::
## <a name="initialize-the-camerarotationhelper-class"></a>CameraRotationHelper 클래스를 초기화 합니다.

이제 **CameraRotationHelper** 클래스를 사용 하기 시작 합니다. 클래스 멤버 변수를 선언 하 여 개체를 저장 합니다. 생성자를 호출 하 여 선택한 카메라의 엔클로저 위치를 전달 합니다. 도우미 클래스는이 정보를 사용 하 여 캡처된 미디어, 미리 보기 스트림 및 UI의 올바른 방향을 계산 합니다. UI 또는 미리 보기 스트림의 방향을 업데이트 해야 할 때 발생 하는 도우미 클래스의 **OrientationChanged** 이벤트에 대 한 처리기를 등록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareRotationHelper":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitRotationHelper":::

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>카메라 미리 보기 스트림에 방향 데이터 추가
미리 보기 스트림의 메타 데이터에 올바른 방향을 추가 해도 미리 보기가 사용자에 게 표시 되는 방식에는 영향을 주지 않지만 시스템에서 미리 보기 스트림에서 캡처된 모든 프레임을 올바르게 인코딩할 수 있습니다.

[**MediaCapture.StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync)를 호출하여 카메라 미리 보기를 시작합니다. 이 작업을 수행 하기 전에 멤버 변수를 확인 하 여 미리 보기를 미러링 (전면 카메라의 경우) 해야 하는지 확인 합니다. 이 경우 *PreviewControl* 라는 [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement)의 [**flowdirection**](/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) 속성을이 예제에서 [**RightToLeft**](/uwp/api/Windows.UI.Xaml.FlowDirection)로 설정 합니다. 미리 보기를 시작한 후 **SetPreviewRotationAsync** 도우미 메서드를 호출 하 여 미리 보기 회전을 설정 합니다. 다음은이 메서드의 구현입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartPreviewWithRotationAsync":::

미리 보기 회전을 별도의 방법으로 설정 하 여 미리 보기 스트림을 다시 초기화 하지 않고 전화 방향이 변경 될 때 업데이트 될 수 있도록 합니다. 카메라가 장치 외부에 있는 경우 아무 동작도 수행 되지 않습니다. 그렇지 않으면 **CameraRotationHelper** 메서드 **GetCameraPreviewOrientation** 이 호출 되 고 미리 보기 스트림의 적절 한 방향을 반환 합니다. 

메타 데이터를 설정 하기 위해 [**VideoDeviceController. GetMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties)를 호출 하 여 미리 보기 스트림 속성을 검색 합니다. 다음으로 비디오 스트림 회전의 MFT (미디어 파운데이션 변환) 특성을 나타내는 GUID를 만듭니다. C + +에서는 상수 [**MF_MT_VIDEO_ROTATION**](/windows/desktop/medfound/mf-mt-video-rotation)를 사용할 수 있지만, c #에서는 GUID 값을 수동으로 지정 해야 합니다. 

GUID를 키로 지정 하 고 미리 보기 회전을 값으로 지정 하 여 스트림 속성 개체에 속성 값을 추가 합니다. 이 속성은 값을 반시계 방향으로 예상 하므로 **CameraRotationHelper** 메서드 **ConvertSimpleOrientationToClockwiseDegrees** 를 사용 하 여 단순 방향 값을 변환 합니다. 마지막으로 [**SetEncodingPropertiesAsync**](/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_SetEncodingPropertiesAsync_Windows_Media_Capture_MediaStreamType_Windows_Media_MediaProperties_IMediaEncodingProperties_Windows_Media_MediaProperties_MediaPropertySet_) 를 호출 하 여 새 회전 속성을 스트림에 적용 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSetPreviewRotationAsync":::

그런 다음 **CameraRotationHelper OrientationChanged** 이벤트에 대 한 처리기를 추가 합니다. 이 이벤트는 미리 보기 스트림을 회전 해야 하는지 여부를 알 수 있는 인수를 전달 합니다. 장치 방향이 왼쪽으로 이동 하도록 변경 된 경우이 값은 false가 됩니다. 미리 보기를 회전 해야 하는 경우 이전에 정의 된 **SetPreviewRotationAsync** 를 호출 합니다.

그런 다음 **OrientationChanged** 이벤트 처리기에서 필요한 경우 UI를 업데이트 합니다. **Getuiorientation** 를 호출 하 여 도우미 클래스에서 현재 권장 되는 UI 방향을 가져오고 XAML 변환에 사용 되는 값을 시계 방향으로 변환 합니다. 방향 값에서 [**system.windows.media.rotatetransform.angle**](/uwp/api/Windows.UI.Xaml.Media.RotateTransform) 를 만들고 XAML 컨트롤의 [**rendertransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) 속성을 설정 합니다. UI 레이아웃에 따라 단순히 컨트롤을 회전 하는 것 외에 추가로 조정 해야 할 수도 있습니다. 또한 ui 스레드에 대 한 모든 업데이트를 수행 해야 하므로 [**Runasync**](/uwp/api/Windows.UI.Core.CoreDispatcher#Windows_UI_Core_CoreDispatcher_RunAsync_Windows_UI_Core_CoreDispatcherPriority_Windows_UI_Core_DispatchedHandler_)에 대 한 호출 내에이 코드를 넣어야 합니다.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetHelperOrientationChanged":::

## <a name="capture-a-photo-with-orientation-data"></a>방향 데이터를 사용 하 여 사진 캡처
[**MediaCapture를 사용 하는 기본 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md) 는 먼저 메모리 내 스트림으로 캡처한 다음 디코더를 사용 하 여 스트림에서 이미지 데이터를 읽고 이미지 데이터를 파일로 트랜스 코딩 하 여 파일에 대 한 사진을 캡처하는 방법을 보여 줍니다. **CameraRotationHelper** 클래스에서 가져온 방향 데이터는 코드 변환 작업을 수행 하는 동안 이미지 파일에 추가할 수 있습니다.

다음 예제에서는 [**CapturePhotoToStreamAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) 에 대 한 호출을 사용 하 여 [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) 에 사진을 캡처하고 스트림에서 [**bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 를 만듭니다. 그런 다음 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 는 파일에 쓸 [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 를 검색 하기 위해 만들어지고 열립니다. 

파일의 코드를 변환 하기 전에 도우미 클래스 메서드 **GetCameraCaptureOrientation**에서 사진의 방향을 검색 합니다. 이 메서드는 도우미 메서드 **ConvertSimpleOrientationToPhotoOrientation**를 사용 하 여 [**사진 방향**](/uwp/api/Windows.Storage.FileProperties.PhotoOrientation) 개체로 변환 되는 [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) 개체를 반환 합니다. 그런 다음 새 **BitmapPropertySet** 개체를 만들고, 키가 "BitmapTypedValue" 인 속성을 추가 하 고, 값을 사진 방향으로 표시 하 여 **BitmapTypedValue**로 표시 합니다. "System.object"는 이미지 파일에 메타 데이터로 추가할 수 있는 많은 Windows 속성 중 하나입니다. 모든 사진 관련 속성 목록은 [**Windows 속성-사진**](/previous-versions/ff516600(v=vs.85))을 참조 하세요. 이미지에서 메타 데이터를 사용 하는 방법에 대 한 자세한 내용은 [**이미지 메타 데이터**](image-metadata.md)를 참조 하세요.

마지막으로, [**SetPropertiesAsync**](../develop/index.md) 에 대 한 호출을 사용 하 여 인코더에 대 한 방향 데이터를 포함 하는 속성 집합을 설정 하 고, [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync)에 대 한 호출을 사용 하 여 이미지를 트랜스 코딩 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCapturePhotoWithOrientation":::

## <a name="capture-a-video-with-orientation-data"></a>방향 데이터를 사용 하 여 비디오 캡처
기본 비디오 캡처는 [**MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처**](basic-photo-video-and-audio-capture-with-mediacapture.md)문서에 설명 되어 있습니다. 캡처된 비디오의 인코딩에 방향 데이터를 추가 하는 것은 미리 보기 스트림에 방향 데이터 추가에 대 한 섹션의 앞부분에서 설명한 것과 동일한 기술을 사용 하 여 수행 됩니다.

다음 예제에서는 캡처된 비디오가 작성 되는 파일을 만듭니다. MP4 인코딩 프로필은 정적 메서드 [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)를 사용 하 여 만듭니다. 회전 속성의 방향이 반시계 방향으로 표시 되어야 **하기 때문에** **ConvertSimpleOrientationToClockwiseDegrees** **CameraRotationHelper** 클래스에서 비디오에 대 한 적절 한 방향은 방향 값을 변환 하기 위해 호출 됩니다. 다음으로 비디오 스트림 회전의 MFT (미디어 파운데이션 변환) 특성을 나타내는 GUID를 만듭니다. C + +에서는 상수 [**MF_MT_VIDEO_ROTATION**](/windows/desktop/medfound/mf-mt-video-rotation)를 사용할 수 있지만, c #에서는 GUID 값을 수동으로 지정 해야 합니다. GUID를 키로 지정 하 고 값을 값으로 지정 하 여 속성 값을 스트림 속성 개체에 추가 합니다. 마지막으로 [**StartRecordToStorageFileAsync**](/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_) 를 호출 하 여 방향 데이터로 인코딩된 비디오 기록을 시작 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartRecordingWithOrientationAsync":::

## <a name="camerarotationhelper-full-code-listing"></a>전체 코드 목록 CameraRotationHelper
다음 코드 조각에서는 하드웨어 방향 센서를 관리 하 고, 사진과 비디오에 대 한 적절 한 방향 값을 계산 하 고, 다양 한 Windows 기능에서 사용 되는 여러 방향 표현 사이를 변환 하는 도우미 메서드를 제공 하는 **CameraRotationHelper** 클래스에 대 한 전체 코드를 나열 합니다. 위의 문서에 나와 있는 지침을 따르는 경우 아무 것도 변경 하지 않고도이 클래스를 프로젝트에 그대로 추가할 수 있습니다. 물론, 특정 시나리오의 요구 사항에 맞게 다음 코드를 자유롭게 사용자 지정할 수 있습니다.

이 도우미 클래스는 장치의 [**SimpleOrientationSensor**](/uwp/api/Windows.Devices.Sensors.SimpleOrientationSensor) 를 사용 하 여 장치 섀시의 현재 방향을 확인 하 고 [**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation) 클래스를 사용 하 여 현재 디스플레이 방향을 결정 합니다. 이러한 각 클래스는 현재 방향이 변경 될 때 발생 하는 이벤트를 제공 합니다. 캡처 장치가 탑재 된 패널 (front, back 또는 external)은 미리 보기 스트림을 미러링 해야 하는지 여부를 결정 하는 데 사용 됩니다. 또한 일부 장치에서 지원 되는 RotationAngleInDegreesClockwise 속성을 사용 하 여 [**EnclosureLocation**](/uwp/api/windows.devices.enumeration.enclosurelocation.rotationangleindegreesclockwise) 가 섀시를 탑재 하는 방향을 결정 합니다.

다음 메서드를 사용 하 여 지정 된 카메라 앱 작업에 대해 권장 되는 방향 값을 가져올 수 있습니다.

* **Getuiorientation** -카메라 UI 요소에 대해 제안 된 방향을 반환 합니다.
* **GetCameraCaptureOrientation** -이미지 메타 데이터로 인코딩할 제안 된 방향을 반환 합니다.
* **GetCameraPreviewOrientation** -미리 보기 스트림에 대해 제안 된 방향을 반환 하 여 자연 스러운 사용자 환경을 제공 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs" id="SnippetCameraRotationHelperFull":::



## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
