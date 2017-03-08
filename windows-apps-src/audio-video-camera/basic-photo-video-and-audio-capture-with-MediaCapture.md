---
author: drewbatgit
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: "이 문서에서는 MediaCapture 클래스를 사용하여 사진과 비디오를 캡처하는 가장 간단한 방법을 보여 줍니다."
title: "MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8918b120394def3ba12d5932dc66cb38279cc124
ms.lasthandoff: 02/08/2017

---

# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 클래스를 사용하여 사진과 비디오를 캡처하는 가장 간단한 방법을 보여 줍니다. **MediaCapture** 클래스는 캡처 파이프라인에 대한 하위 수준 제어를 제공하고 고급 캡처 시나리오를 가능하게 하는 강력한 API 집합을 표시하지만, 이 문서는 기본 미디어 캡처를 쉽고 빠르게 앱에 추가할 수 있도록 돕기 위한 것입니다. **MediaCapture**에서 제공하는 더 많은 기능에 대해 알아보려면 [**카메라**](camera.md)를 참조하세요.

단순히 사진이나 비디오를 캡처하고 다른 미디어 캡처 기능을 추가하지 않으려는 경우 또는 고유한 카메라 UI를 만들지 않으려는 경우 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI) 클래스를 사용하는 것이 좋습니다. 이 클래스를 사용하면 단순히 Windows 기본 제공 카메라 앱을 실행하고 캡처된 사진 또는 비디오 파일을 받을 수 있습니다. 자세한 내용은 [**Windows 기본 제공 카메라 UI를 사용하여 사진 및 비디오 캡처**](capture-photos-and-video-with-cameracaptureui.md)를 참조하세요.

이 문서의 코드는 [**카메라 시작 키트**](https://go.microsoft.com/fwlink/?linkid=619479) 샘플에서 조정되었습니다. 샘플을 다운로드하여 상황에 맞게 사용되는 코드를 참조하거나 자체 앱을 처음 빌드하기 시작할 때 샘플을 사용할 수 있습니다.

## <a name="add-capability-declarations-to-the-app-manifest"></a>앱 매니페스트에 접근 권한 값 선언 추가

앱에서 장치의 카메라에 액세스해야 하는 경우 앱에 *webcam* and *microphone* 장치 기능이 사용된다고 선언해야 합니다. 캡처한 사진 또는 비디오를 사용자의 사진 라이브러리에 저장하려는 경우에도 *picturesLibrary* 및 *videosLibrary* 접근 권한 값을 선언해야 합니다.

**앱 매니페스트에 접근 권한 값을 추가하려면**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.
4.  사진과 비디오 라이브러리에 액세스하려면 **사진 라이브러리**의 확인란과 **비디오 라이브러리**의 확인란을 선택합니다.


## <a name="initialize-the-mediacapture-object"></a>MediaCapture 개체 초기화
이 문서에 설명된 모든 캡처 메서드의 경우 생성자를 호출한 다음 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync)를 호출하여 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 개체를 초기화하는 첫 번째 단계를 수행해야 합니다. **MediaCapture** 개체는 앱의 여러 위치에서 액세스되므로 개체가 포함될 클래스 변수를 선언합니다.  캡처 작업이 실패할 경우 알림을 받을 **MediaCapture** 개체의 [**Failed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.Failed) 이벤트 처리기를 구현합니다.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>카메라 미리 보기 설정
카메라 미리 보기를 표시하지 않고 **MediaCapture**를 사용하여 사진, 비디오 및 오디오를 캡처할 수 있지만 일반적으로 사용자가 캡처되는 내용을 볼 수 있도록 미리 보기 스트림을 표시하는 것이 좋습니다. 또한 자동 초점, 자동 노출, 자동 화이트 밸런스 등의 몇 가지 **MediaCapture** 기능을 사용하려면 미리 보기 스트림을 실행해야 합니다. 카메라 미리 보기를 설정하는 방법은 [**카메라 미리 보기 표시**](simple-camera-preview-access.md)를 참조하세요.

## <a name="capture-a-photo-to-a-softwarebitmap"></a>SoftwareBitmap에 사진 캡처
[**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) 클래스는 여러 기능에 공통된 이미지 표현을 제공하기 위해 Windows 10에서 도입되었습니다. 사진을 파일에 캡처하지 않고 사진을 캡처한 다음 XAML에서 표시하는 등 캡처한 이미지를 앱에서 즉시 사용하려는 경우 **SoftwareBitmap**에 캡처해야 합니다. 나중에 이미지를 디스크에 저장할 수 있습니다.

**MediaCapture** 개체를 초기화한 후 [**LowLagPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture) 클래스를 사용하여 사진을 **SoftwareBitmap**에 캡처할 수 있습니다. 원하는 이미지 형식을 지정하는 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) 개체를 전달하고 [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync)를 호출하여 이 클래스의 인스턴스를 가져옵니다. [**CreateUncompressed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateUncompressed)는 지정된 픽셀 형식으로 압축되지 않은 인코딩을 만듭니다. [**CapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto) 개체를 반환하는 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync)를 호출하여 사진을 캡처합니다. [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto.Frame) 속성, [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) 속성을 차례로 액세스하여 **SoftwareBitmap**을 가져옵니다.

필요한 경우 **CaptureAsync**를 반복해서 호출하여 여러 사진을 캡처할 수 있습니다. 캡처를 마쳤으면 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync)를 호출하여 **LowLagPhotoCapture** 세션을 종료하고 관련된 리소스를 해제합니다. **FinishAsync**를 호출한 후 사진 캡처를 다시 시작하려면 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync)를 호출하기 전에 [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync)를 다시 호출하여 캡처 세션을 다시 초기화해야 합니다.

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

XAML 페이지에 표시하는 방법을 비롯한 **SoftwareBitmap** 개체 작업에 대한 자세한 내용은 [**비트맵 이미지 만들기, 편집 및 저장**](imaging.md)을 참조하세요.

## <a name="capture-a-photo-to-a-file"></a>파일에 사진 캡처
일반적인 사진 앱은 캡처한 사진을 디스크 또는 클라우드 저장소에 저장하며 사진 방향 등의 메타데이터를 파일에 추가해야 합니다. 다음 예제에서는 사진을 파일에 캡처하는 방법을 보여 줍니다. 나중에 이미지 파일에서 **SoftwareBitmap**을 만들 수 있습니다. 

이 예제에 나와 있는 방법에서는 사진을 메모리 내 스트림에 캡처한 다음 스트림에서 디스크의 파일로 사진을 코드 변환합니다. 이 예제에서는 [**GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.GetLibraryAsync)를 사용하여 사용자의 사진 라이브러리를 가져온 다음 [**SaveFolder**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.SaveFolder) 속성을 사용하여 참조 기본 저장 폴더를 가져옵니다. 이 폴더에 액세스하려면 앱 매니페스트에 **사진 라이브러리** 접근 권한 값을 추가해야 합니다. [**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFolder.CreateFileAsync)는 사진이 저장되는 새 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile)을 만듭니다.

[**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream)을 만든 다음 [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync)를 호출하여 사진을 스트림에 캡처하고 스트림 및 사용해야 하는 이미지 형식을 지정하는 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) 개체를 전달합니다. 직접 개체를 초기화하여 사용자 지정 인코딩 속성을 만들 수 있지만, 클래스에서 일반적인 인코딩 형식에 대해 [**ImageEncodingProperties.CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateJpeg) 등의 정적 메서드를 제공합니다. [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile.OpenAsync)를 호출하여 출력 파일에 대한 파일 스트림을 만듭니다. 메모리 내 스트림에서 이미지를 디코드하는 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder)를 만든 다음 [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.CreateForTranscodingAsync)를 호출하여 이미지를 파일로 인코드하는 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder)를 만듭니다.

필요에 따라 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapPropertySet) 개체를 만든 다음 이미지 인코더에서 [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252.aspx)를 호출하여 사진에 대한 메타데이터를 이미지 파일에 포함할 수 있습니다. 인코딩 속성에 대한 자세한 내용은 [**이미지 메타데이터**](image-metadata.md)를 참조하세요. 적절한 장치 방향 처리는 대부분의 사진 앱에서 필수입니다. 자세한 내용은 [**MediaCapture를 사용하여 장치 방향 처리**](handle-device-orientation-with-mediacapture.md)를 참조하세요.

마지막으로, 인코더 개체에서 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync)를 호출하여 메모리 내 스트림에서 파일로 사진을 코드 변환합니다.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

파일 및 폴더 작업에 대한 자세한 내용은 [**파일, 폴더 및 라이브러리**](https://msdn.microsoft.com/windows/uwp/files/index)를 참조하세요.

## <a name="capture-a-video"></a>비디오 캡처
[**LowLagMediaRecording**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording) 클래스를 사용하여 비디오 캡처를 신속하게 앱에 추가합니다. 먼저, 개체에 대한 클래스 변수를 선언합니다.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

그런 다음 비디오를 저장할 **StorageFile** 개체를 만듭니다. 이 예제와 같이 사용자의 비디오 라이브러리에 저장하려면 앱 매니페스트에 **비디오 라이브러리** 접근 권한 값을 추가해야 합니다. [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync)를 호출하여 미디어 기록을 초기화하고 저장소 파일 및 비디오에 대한 인코딩을 지정하는 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile) 개체를 전달합니다. 클래스는 일반적인 비디오 인코딩 프로필을 만들기 위한 정적 메서드(예: [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4))를 제공합니다.

마지막으로, [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync)를 호출하여 비디오 캡처를 시작합니다.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

비디오 녹화를 중지하려면 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync)를 호출합니다.

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

계속해서 **StartAsync** 및 **StopAsync**를 호출하여 추가 비디오를 캡처할 수 있습니다. 비디오 캡처를 마쳤으면 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync)를 호출하여 캡처 세션을 삭제하고 관련된 리소스를 정리합니다. 이 호출 후에는 **StartAsync** 호출 전에 **PrepareLowLagRecordToStorageFileAsync**를 다시 호출하여 캡처 세션을 다시 초기화해야 합니다.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

비디오를 캡처할 때 **MediaCapture** 개체의 [**RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.RecordLimitationExceeded) 이벤트 처리기를 등록해야 합니다. 이 이벤트는 단일 녹화에 대한 제한(현재 3시간)을 초과할 경우 운영 체제에서 발생합니다. 이벤트 처리기에서 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync)를 호출하여 녹화를 완료해야 합니다.

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

## <a name="pause-and-resume-video-recording"></a>비디오 녹화 일시 중지 및 다시 시작
[**PauseAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseAsync)를 호출한 다음 [**ResumeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.ResumeAsync)를 호출하면 비디오 녹화를 일시 중지한 다음 별도의 출력 파일을 만들지 않고 녹화를 다시 시작할 수 있습니다.

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

Windows 10 버전 1607부터 비디오 녹화를 일시 중지하고 녹화가 일시 중지되기 전에 캡처된 마지막 프레임을 받을 수 있습니다. 그런 다음 카메라 미리 보기에 이 프레임을 오버레이하여 사용자가 녹화를 다시 시작하기 전에 카메라를 일시 중지된 프레임에 맞추게 할 수 있습니다. [**PauseWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseWithResultAsync)를 호출하면 [**MediaCapturePauseResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult) 개체가 반환됩니다. [**LastFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult.LastFrame) 속성은 마지막 프레임을 나타내는 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.VideoFrame) 개체입니다. XAML에서 프레임을 표시하려면 비디오 프레임의 **SoftwareBitmap** 표현을 가져옵니다. 현재, 알파 채널이 미리 곱해졌거나 비어 있는 BGRA8 형식의 이미지만 지원되므로 필요한 경우 [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert)를 호출하여 올바른 형식을 가져옵니다.  새 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 개체를 만들고 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource.SetBitmapAsync)를 호출하여 초기화합니다. 마지막으로, XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) 컨트롤의 **Source** 속성을 설정하여 이미지를 표시합니다. 이 방법이 작동하려면 이미지가 **CaptureElement** 컨트롤과 정렬되고 불투명도 값이 1보다 작아야 합니다. UI 스레드에서만 UI를 수정할 수 있으므로 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync) 내에서 이 호출을 수행해야 합니다.

또한 **PauseWithResultAsync**는 녹화된 총 시간을 추적해야 하는 경우를 위해 이전 세그먼트에서 녹화된 비디오의 지속 시간을 반환합니다.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

녹화를 다시 시작할 때 이미지 원본을 null로 설정하여 숨길 수 있습니다.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

[**StopWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopWithResultAsync)를 호출하여 비디오를 중지할 때 결과 프레임을 가져올 수도 있습니다.


## <a name="capture-audio"></a>오디오 캡처 
위에 표시된 것과 동일한 비디오 캡처 기술을 사용하여 오디오 캡처를 신속하게 앱에 추가할 수 있습니다. 아래 예제에서는 응용 프로그램 데이터 폴더에 **StorageFile**을 만듭니다. [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync)를 호출하여 캡처 세션을 초기화하고 파일 및 이 예제에서 [**CreateMp3**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp3) 정적 메서드를 통해 생성된 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile)을 전달합니다. 녹음을 시작하려면 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync)를 호출합니다.

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


오디오 녹음을 중지하려면 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoSequenceCapture.StopAsync)를 호출합니다.

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

**StartAsync** 및 **StopAsync**를 여러 번 호출하여 여러 오디오 파일을 녹음할 수 있습니다. 오디오 캡처를 마쳤으면 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync)를 호출하여 캡처 세션을 삭제하고 관련된 리소스를 정리합니다. 이 호출 후에는 **StartAsync** 호출 전에 **PrepareLowLagRecordToStorageFileAsync**를 다시 호출하여 캡처 세션을 다시 초기화해야 합니다.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [Windows 기본 제공 카메라 UI를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-cameracaptureui.md)
* [MediaCapture를 사용하여 장치 방향 처리](handle-device-orientation-with-mediacapture.md)
* [비트맵 이미지 만들기, 편집 및 저장](imaging.md)
* [파일, 폴더 및 라이브러리](https://msdn.microsoft.com/windows/uwp/files/index)


