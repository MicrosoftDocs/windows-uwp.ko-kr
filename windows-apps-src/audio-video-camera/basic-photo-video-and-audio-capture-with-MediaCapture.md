---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: 이 문서에서는 MediaCapture 클래스를 사용 하 여 사진과 비디오를 캡처하는 가장 간단한 방법을 보여 줍니다.
title: MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aecd25eb7f3d9c9f08e2b07d6bd425a00686e0ea
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362956"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처


이 문서에서는 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 클래스를 사용 하 여 사진과 비디오를 캡처하는 가장 간단한 방법을 보여 줍니다. **MediaCapture** 클래스는 캡처 파이프라인에 대 한 하위 수준 제어를 제공 하 고 고급 캡처 시나리오를 사용 하는 강력한 api 집합을 제공 하지만이 문서는 기본 미디어 캡처를 앱에 빠르고 쉽게 추가 하는 데 도움이 됩니다. **MediaCapture** 에서 제공 하는 기능에 대해 자세히 알아보려면 [**카메라**](camera.md)를 참조 하세요.

단순히 사진 또는 비디오를 캡처하고 추가 미디어 캡처 기능을 추가 하지 않으려는 경우 또는 사용자 고유의 카메라 UI를 만들지 않으려는 경우 Windows 기본 제공 카메라 앱을 시작 하 고 캡처된 사진 또는 비디오 파일을 받을 수 있는 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 클래스를 사용 하는 것이 좋습니다. 자세한 내용은 [ **Windows 기본 제공 카메라 UI를 사용 하 여 사진 및 비디오 캡처** 를 참조 하세요.](capture-photos-and-video-with-cameracaptureui.md)

이 문서의 코드는 [**카메라 스타터 키트**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit) 샘플에서 적용 되었습니다. 샘플을 다운로드 하 여 컨텍스트에서 사용 되는 코드를 확인 하거나 사용자 고유의 앱에 대 한 시작 지점으로 샘플을 사용할 수 있습니다.

## <a name="add-capability-declarations-to-the-app-manifest"></a>앱 매니페스트에 기능 선언 추가

앱이 장치 카메라에 액세스 하려면 앱에서 *웹캠* 및 *마이크* 장치 기능을 사용 하도록 선언 해야 합니다. 캡처한 사진과 비디오를 사용자의 사진 또는 비디오 라이브러리에 저장 하려면 *picturesLibrary* 및 *videosLibrary* 기능도 선언 해야 합니다.

**앱 매니페스트에 기능을 추가 하려면**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **기능** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.
4.  사진과 비디오 라이브러리에 액세스하려면 **사진 라이브러리**의 확인란과 **비디오 라이브러리**의 확인란을 선택합니다.


## <a name="initialize-the-mediacapture-object"></a>MediaCapture 개체를 초기화 합니다.
이 문서에서 설명 하는 모든 캡처 메서드는 생성자를 호출 하 고 [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync)를 호출 하 여 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 개체를 초기화 하는 첫 번째 단계를 수행 해야 합니다. **MediaCapture** 개체는 앱의 여러 위치에서 액세스할 수 있으므로 개체를 보유할 클래스 변수를 선언 합니다.  캡처 작업이 실패 하는 경우 알리도록 **MediaCapture** 개체의 [**Failed**](/uwp/api/windows.media.capture.mediacapture.failed) 이벤트에 대 한 처리기를 구현 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareMediaCapture":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitMediaCapture":::

## <a name="set-up-the-camera-preview"></a>카메라 미리 보기 설정
카메라 미리 보기를 표시 하지 않고 **MediaCapture** 를 사용 하 여 사진, 비디오 및 오디오를 캡처할 수 있지만 일반적으로 사용자가 캡처할 항목을 볼 수 있도록 미리 보기 스트림을 표시 하려고 합니다. 또한 몇 가지 **MediaCapture** 기능을 통해 미리 보기 스트림이 자동 포커스, 자동 노출, 자동으로 밸런스를 포함 하 여 enbled 수 있도록 해야 합니다. 카메라 미리 보기를 설정 하는 방법을 보려면 [**카메라 미리 보기 표시**](simple-camera-preview-access.md)를 참조 하세요.

## <a name="capture-a-photo-to-a-softwarebitmap"></a>에 사진을 캡처합니다.
[**\Bitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 클래스는 여러 기능에서 이미지의 일반적인 표현을 제공 하기 위해 Windows 10에서 도입 되었습니다. 사진을 캡처한 후 응용 프로그램에서 캡처한 이미지를 사용 하는 경우 (예: 파일을 캡처하는 대신 XAML로 표시 하는 경우), 파일을 **캡처하는 대신**에 캡처한 이미지를 사용 해야 합니다. 나중에 이미지를 디스크에 저장할 수도 있습니다.

MediaCapture 개체를 초기화 한 후에는 [**Lowlagphoto capture**](/uwp/api/Windows.Media.Capture.LowLagPhotoCapture) 클래스 **를 사용 하** 여 **MediaCapture** 에 대 한 사진을 캡처할 수 있습니다. [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync)를 호출 하 고 원하는 이미지 형식을 지정 하 여 [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 개체를 전달 하 여이 클래스의 인스턴스를 가져옵니다. [**Createuncompressed**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createuncompressed) 해제는 지정 된 픽셀 형식으로 압축 되지 않은 인코딩을 만듭니다. [**CapturedPhoto**](/uwp/api/Windows.Media.Capture.CapturedPhoto) 개체를 반환 하는 [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync)을 호출 하 여 사진을 캡처합니다. [**Frame**](/uwp/api/windows.media.capture.capturedphoto.frame) 속성 및 [**\bitmap**](/uwp/api/windows.media.capture.capturedframe.softwarebitmap) 속성에 액세스 하 여에 지 속성 **비트맵** 을 가져옵니다.

원할 경우 **CaptureAsync**를 반복 해 서 호출 하 여 여러 사진을 캡처할 수 있습니다. 캡처를 완료 한 후에는 마침 [**비동기**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) 를 호출 하 여 **Lowlag사진 캡처** 세션을 종료 하 고 연결 된 리소스를 해제 합니다. Enter **async**를 호출 하 고 사진을 다시 캡처하기 시작 하는 경우 [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync)를 호출 하기 전에 [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) 를 다시 호출 하 여 캡처 세션을 다시 초기화 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCaptureToSoftwareBitmap":::

Windows, 버전 1803부터 **CaptureAsync** 에서 반환 된 **CapturedFrame** 클래스의 [**BitmapProperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) 속성에 액세스 하 여 캡처된 사진에 대 한 메타 데이터를 검색할 수 있습니다. 이 데이터를 **BitmapEncoder** 에 전달 하 여 메타 데이터를 파일에 저장할 수 있습니다. 이전에는 압축 되지 않은 이미지 형식에 대 한이 데이터에 액세스할 수 있는 방법이 없었습니다. 또한 [**controlvalues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) 속성에 액세스 하 여 캡처된 프레임에 대 한 제어 값 (예: 노출 및 흰색 잔액)을 설명 하는 [**CapturedFrameControlValues**](/uwp/api/windows.media.capture.capturedframecontrolvalues) 개체를 검색할 수 있습니다.

**BitmapEncoder** 사용 및 XAML 페이지에 개체를 표시 하는 방법을 비롯 하 여 **\bitmap** 개체 작업에 대 한 자세한 내용은 [**비트맵 이미지 만들기, 편집 및 저장**](imaging.md)을 참조 하세요. 

캡처 장치 컨트롤 값 설정에 대 한 자세한 내용은 [사진 및 비디오에 대 한 장치 컨트롤 캡처](capture-device-controls-for-photo-and-video-capture.md)를 참조 하세요.

Windows 10, 버전 1803부터 **MediaCapture**에서 반환 하는 **CapturedFrame** 의 [**BitmapProperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) 속성에 액세스 하 여 압축 되지 않은 형식으로 캡처된 사진에 대 한 EXIF 정보 등의 메타 데이터를 가져올 수 있습니다. 이전 릴리스에서는이 데이터를 압축 된 파일 형식으로 캡처된 사진의 헤더 에서만 액세스할 수 있었습니다. 수동으로 이미지 파일을 작성 하는 경우이 데이터를 [**BitmapEncoder**](/uwp/api/windows.graphics.imaging.bitmapencoder) 에 제공할 수 있습니다. 비트맵 인코딩에 대 한 자세한 내용은 [비트맵 이미지 만들기, 편집 및 저장](imaging.md)을 참조 하세요.  [**Controlvalues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) 속성에 액세스 하 여 이미지를 캡처할 때 사용 되는 노출 및 플래시 설정과 같은 프레임 컨트롤 값에도 액세스할 수 있습니다. 자세한 내용은 [사진 및 비디오 캡처를 위한 장치 컨트롤 캡처](capture-device-controls-for-photo-and-video-capture.md)를 참조 하세요.

## <a name="capture-a-photo-to-a-file"></a>파일에 사진 캡처
일반적인 사진 앱은 캡처된 사진을 디스크나 클라우드 저장소에 저장 하 고 사진 방향과 같은 메타 데이터를 파일에 추가 해야 합니다. 다음 예제에서는 파일에 사진을 캡처하는 방법을 보여 줍니다. 나중에 이미지 파일에서 계속 해 서이 **비트맵** 을 만들 수도 있습니다. 

이 예제에 표시 된 기술은 메모리 내 스트림으로 사진을 캡처한 다음 스트림에서 디스크의 파일로 사진을 트랜스 코딩 합니다. 이 예제에서는 [**Getlibraryasync**](/uwp/api/windows.storage.storagelibrary.getlibraryasync) 를 사용 하 여 사용자의 그림 라이브러리를 가져온 다음 [**savefolder**](/uwp/api/windows.storage.storagelibrary.savefolder) 속성을 사용 하 여 참조 기본 저장 폴더를 가져옵니다. 이 폴더에 액세스 하려면 앱 매니페스트에 **그림 라이브러리** 기능을 추가 해야 합니다. [**CreateFileAsync**](/uwp/api/windows.storage.storagefolder.createfileasync) 사진이 저장 될 새 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 를 만듭니다.

[**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) 을 만든 다음 [**CapturePhotoToStreamAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) 를 호출 하 여 스트림에 사진을 캡처하고, 사용 해야 하는 이미지 형식을 지정 하 여 스트림을 전달 하 고 [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 개체를 전달 합니다. 개체를 직접 초기화 하 여 사용자 지정 인코딩 속성을 만들 수 있지만 클래스는 일반적인 인코딩 형식에 [**ImageEncodingProperties**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createjpeg) 와 같은 정적 메서드를 제공 합니다. 다음으로 [**Openasync**](/uwp/api/windows.storage.storagefile.openasync)를 호출 하 여 출력 파일에 대 한 파일 스트림을 만듭니다. [**Bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 을 만들어 메모리 내 스트림에서 이미지를 디코딩하고 [**CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync)를 호출 하 여 이미지를 파일로 인코딩하는 [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) 를 만듭니다.

필요에 따라 [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) 개체를 만든 다음 이미지 인코더에서 [**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) 을 호출 하 여 이미지 파일의 사진에 대 한 메타 데이터를 포함할 수 있습니다. 인코딩 속성에 대 한 자세한 내용은 [**이미지 메타 데이터**](image-metadata.md)를 참조 하세요. 장치 방향을 올바르게 처리 하는 것은 대부분의 사진 앱에 필수적입니다. 자세한 내용은 [**MediaCapture를 사용 하 여 장치 방향 처리**](handle-device-orientation-with-mediacapture.md)를 참조 하세요.

마지막으로 인코더 개체에 대해 [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) 를 호출 하 여 메모리 내 스트림에서 사진을 파일에 트랜스 코딩 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCaptureToFile":::

파일 및 폴더를 사용 하는 방법에 대 한 자세한 내용은 [**파일, 폴더 및 라이브러리**](../files/index.md)를 참조 하십시오.

## <a name="capture-a-video"></a>비디오 캡처
[**Lowlagmediarecording**](/uwp/api/Windows.Media.Capture.LowLagMediaRecording) 클래스를 사용 하 여 앱에 비디오 캡처를 빠르게 추가 합니다. 먼저 개체에 대 한 클래스 변수를로 선언 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetLowLagMediaRecording":::

그런 다음 비디오가 저장 될 **StorageFile** 개체를 만듭니다. 이 예제에 표시 된 것 처럼 사용자의 비디오 라이브러리에 저장 하려면 앱 매니페스트에 **비디오 라이브러리** 기능을 추가 해야 합니다. [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) 를 호출 하 여 미디어 기록을 초기화 하 고, 저장소 파일을 전달 하 고, 비디오에 대 한 인코딩을 지정 하는 [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 개체를 전달 합니다. 클래스는 일반적인 비디오 인코딩 프로필을 만들기 위한 정적 메서드(예: [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4))를 제공합니다.

마지막으로 [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync) 를 호출 하 여 비디오 캡처를 시작 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartVideoCapture":::

비디오 기록을 중지 하려면 [**Stopasync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync)를 호출 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStopRecording":::

**StartAsync** 및 **stopasync** 를 계속 호출 하 여 추가 비디오를 캡처할 수 있습니다. 비디오 캡처를 완료 한 후에는 마침 [**비동기**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) 를 호출 하 여 캡처 세션을 삭제 하 고 연결 된 리소스를 정리 합니다. 이 호출 후에는 **StartAsync**를 호출 하기 전에 **PrepareLowLagRecordToStorageFileAsync** 를 다시 호출 하 여 캡처 세션을 다시 초기화 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetFinishAsync":::

비디오를 캡처할 때 **MediaCapture** 개체의 [**recordlimit ationlimit**](/uwp/api/windows.media.capture.mediacapture.recordlimitationexceeded) 이벤트에 대 한 처리기를 등록 해야 합니다 .이 이벤트는 단일 기록 (현재 3 시간)에 대 한 제한을 초과할 경우 운영 체제에서 발생 합니다. 이벤트에 대 한 처리기에서 [**Stopasync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync)를 호출 하 여 기록을 마무리 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRecordLimitationExceeded":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRecordLimitationExceededHandler":::

### <a name="play-and-edit-captured-video-files"></a>캡처한 비디오 파일 재생 및 편집
비디오를 파일에 캡처한 후에는 파일을 로드 하 여 앱의 UI 내에서 다시 재생할 수 있습니다. **[MediaPlayerElement](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** XAML 컨트롤 및 관련 **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** 를 사용 하 여이 작업을 수행할 수 있습니다. XAML 페이지에서 미디어를 재생 하는 방법에 대 한 자세한 내용은 [MediaPlayer를 사용 하 여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조 하세요.

**[CreateFromFileAsync](/uwp/api/windows.media.editing.mediaclip.createfromfileasync)** 를 호출 하 여 비디오 파일에서 **[mediaclip](/uwp/api/windows.media.editing.mediaclip)** 개체를 만들 수도 있습니다.  **[Mediacomposition](/uwp/api/windows.media.editing.mediacomposition)** 은 **mediac립** 개체의 시퀀스를 정렬 하 고, 비디오 길이를 조정 하 고, 계층을 만들고, 배경 음악을 추가 하 고, 비디오 효과를 적용 하는 등 기본적인 비디오 편집 기능을 제공 합니다 미디어 컴포지션으로 작업 하는 방법에 대 한 자세한 내용은 [미디어 컴포지션 및 편집](media-compositions-and-editing.md)을 참조 하세요.

## <a name="pause-and-resume-video-recording"></a>비디오 녹화 일시 중지 및 다시 시작
[**PauseAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pauseasync) 를 호출 하 고 [**ResumeAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.resumeasync)를 호출 하 여 별도의 출력 파일을 만들지 않고 비디오 기록을 일시 중지 한 후 기록을 다시 시작할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetPauseRecordingSimple":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetResumeRecordingSimple":::

Windows 10 버전 1607부터 비디오 기록을 일시 중지 하 고 기록이 일시 중지 되기 전에 캡처된 마지막 프레임을 받을 수 있습니다. 그런 다음 카메라 미리 보기에서이 프레임을 오버레이 하면 기록을 다시 시작 하기 전에 사용자가 일시 중지 된 프레임을 기준으로 카메라를 정렬할 수 있습니다. [**PauseWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pausewithresultasync) 를 호출 하면 [**MediaCapturePauseResult**](/uwp/api/Windows.Media.Capture.MediaCapturePauseResult) 개체가 반환 됩니다. [**Lastframe**](/uwp/api/windows.media.capture.mediacapturepauseresult.lastframe) 속성은 마지막 프레임을 나타내는 [**videoframe**](/uwp/api/Windows.Media.VideoFrame) 개체입니다. XAML에서 프레임을 표시 하려면 비디오 프레임의 해당 **비트맵** 표현을 가져옵니다. 현재는 미리 곱하기 또는 빈 알파 채널이 있는 BGRA8 형식의 이미지만 지원 되므로 [**필요한 경우 올바른**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 형식을 얻기 위해 호출을 호출 합니다.  새 [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 개체를 만들고 [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) 를 호출 하 여 초기화 합니다. 마지막으로, XAML [**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image) 컨트롤의 **Source** 속성을 설정 하 여 이미지를 표시 합니다. 이 트릭을 사용 하려면 이미지가 **CaptureElement** 컨트롤에 맞게 정렬 되어야 하며 불투명도 값이 1 보다 작아야 합니다. UI 스레드에서만 UI를 수정할 수 있으므로 [**Runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync)내에서이 호출을 수행 해야 합니다.

또한 **PauseWithResultAsync** 는 기록 된 총 시간을 추적 해야 하는 경우 앞 세그먼트에 기록 된 비디오의 기간을 반환 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetPauseCaptureWithResult":::

기록을 다시 시작할 때 이미지의 원본을 null로 설정 하 고 숨길 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetResumeCaptureWithResult":::

[**StopWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopwithresultasync)를 호출 하 여 비디오를 중지 하는 경우 결과 프레임을 가져올 수도 있습니다.


## <a name="capture-audio"></a>오디오 캡처 
비디오 캡처에 대해 위에 나와 있는 것과 동일한 기법을 사용 하 여 오디오 캡처를 앱에 빠르게 추가할 수 있습니다. 아래 예제에서는 응용 프로그램 데이터 폴더에 **StorageFile** 를 만듭니다. [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) 를 호출 하 여 캡처 세션을 초기화 하 고,이 예제에서 생성 된 파일 및 [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 를 [**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3) 정적 메서드로 전달 합니다. 기록을 시작 하려면 [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync)를 호출 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartAudioCapture":::


[**Stopasync**](/uwp/api/windows.media.capture.lowlagphotosequencecapture.stopasync) 를 호출 하 여 오디오 녹음을 중지 합니다.

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)  
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStopRecording":::

**StartAsync** 및 **stopasync** 를 여러 번 호출 하 여 여러 오디오 파일을 기록할 수 있습니다. 오디오 캡처를 완료 한 후에는 마침 [**비동기**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) 를 호출 하 여 캡처 세션을 삭제 하 고 연결 된 리소스를 정리 합니다. 이 호출 후에는 **StartAsync**를 호출 하기 전에 **PrepareLowLagRecordToStorageFileAsync** 를 다시 호출 하 여 캡처 세션을 다시 초기화 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetFinishAsync":::


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>시스템의 오디오 수준 변경 내용 검색 및 응답
Windows 10, 버전 1803부터 앱은 시스템에서 앱의 오디오 캡처 및 오디오 렌더링 스트림의 오디오 수준을 mutes 수 있는 시기를 감지할 수 있습니다. 예를 들어 시스템은 백그라운드에 들어가면 응용 프로그램의 스트림을 음소거 할 수 있습니다. [**AudioStateMonitor**](/uwp/api/windows.media.audio.audiostatemonitor) 클래스를 사용 하면 시스템에서 오디오 스트림의 볼륨을 수정할 때 이벤트를 수신 하도록 등록할 수 있습니다. [**CreateForCaptureMonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring)를 호출 하 여 오디오 캡처 스트림을 모니터링 하기 위한 **AudioStateMonitor** 인스턴스를 가져옵니다. [**Createforrendermonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring)을 호출 하 여 오디오 렌더링 스트림을 모니터링 하기 위한 인스턴스를 가져옵니다. 각 모니터의 [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 이벤트에 대 한 처리기를 등록 하 여 시스템에서 해당 스트림 범주의 오디오를 변경할 때 알림이 표시 되도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetAudioStateMonitorUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetAudioStateVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRegisterAudioStateMonitor":::

캡처 스트림의 **SoundLevelChanged** 처리기에서 **AudioStateMonitor** 발신자의 [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) 속성을 확인 하 여 새 사운드 수준을 확인할 수 있습니다. 캡처 스트림은 시스템에서 "ducked"로 낮추지 말아야 합니다. 음소거 또는 전체 볼륨으로 다시 전환 되어야 합니다. 오디오 스트림이 음소거 된 경우 진행 중인 캡처를 중지할 수 있습니다. 오디오 스트림이 전체 볼륨으로 복원 되 면 캡처를 다시 시작할 수 있습니다. 다음 예제에서는 일부 부울 클래스 변수를 사용 하 여 앱이 현재 오디오를 캡처 중인지와 오디오 상태 변경으로 인해 캡처가 중지 되었는지 여부를 추적 합니다. 이러한 변수는 프로그래밍 방식으로 오디오 캡처를 중지 하거나 시작 하는 것이 적절 한 시기를 결정 하는 데 사용 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCaptureSoundLevelChanged":::

다음 코드 예제에서는 오디오 렌더링에 대 한 **SoundLevelChanged** 처리기의 구현을 보여 줍니다. 앱 시나리오와 재생 중인 콘텐츠 형식에 따라 소리 수준이 ducked 때 오디오 재생을 일시 중지 하는 것이 좋습니다. 미디어 재생에 대 한 음질 수준 변경을 처리 하는 방법에 대 한 자세한 내용은 [MediaPlayer를 사용 하 여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조 하세요.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRenderSoundLevelChanged":::


* [Windows 기본 카메라 UI를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-cameracaptureui.md)
* [MediaCapture를 사용 하 여 장치 방향 처리](handle-device-orientation-with-mediacapture.md)
* [비트맵 이미지 만들기, 편집 및 저장](imaging.md)
* [파일, 폴더 및 라이브러리](../files/index.md)
