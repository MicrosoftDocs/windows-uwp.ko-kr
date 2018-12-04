---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: 이 문서에서는 MediaCapture와 함께 MediaFrameReader를 사용하여 캡처 원본에서 오디오 데이터가 포함된 AudioFrame을 가져오는 방법을 설명합니다.
title: MediaFrameReader를 사용하여 오디오 프레임 처리
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f85570d5c66db1641ec6352526d4db6213e199b4
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8477067"
---
# <a name="process-audio-frames-with-mediaframereader"></a>MediaFrameReader를 사용하여 오디오 프레임 처리

이 문서에서는 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)와 함께 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader)를 사용하여 미디어 프레임 원본에서 오디오 데이터를 가져오는 방법을 설명합니다. **MediaFrameReader**를 사용하여 색, 적외선 또는 깊이 카메라 등에서 이미지 데이터를 가져오는 방법에 대해 자세히 알아보려면  [Process media frames with MediaFrameReader](process-media-frames-with-mediaframereader.md)를 참조하세요. 이 문서에서는 프레임 판독기 사용 패턴에 대한 일반적인 개요를 제공하고 **MediaFrameSourceGroup**를 사용하여 여러 소스에서 동시에 프레임을 검색하는 등과 같이 **MediaFrameReader** 클래스의 몇 가지 추가 기능에 대해 설명합니다. 

> [!NOTE] 
> 이 문서에 설명된 기능은 Windows 10 버전 1803부터 사용할 수 있습니다.

> [!NOTE] 
> 유니버설 Windows 앱 샘플에서는 **MediaFrameReader**를 사용하여 색, 깊이, 적외선 카메라 등 다른 프레임 원본의 프레임을 표시하는 방법을 보여 줍니다. 자세한 내용은 [카메라 프레임 샘플](http://go.microsoft.com/fwlink/?LinkId=823230)을 참조하세요.

## <a name="setting-up-your-project"></a>프로젝트 설정
오디오 프레임을 획득하는 프로세스는 다른 유형의 미디어 프레임을 획득하는 것과 대체로 동일합니다. **MediaCapture**를 사용하는 앱의 경우 카메라 장치에 액세스하기 전에 앱에서 *웹캠* 기능을 사용하도록 선언해야 합니다. 앱이 오디오 장치에서 캡처하는 경우 *마이크* 장치 기능도 선언해야 합니다. 

**앱 매니페스트에 접근 권한 값 추가**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.
4.  사진과 비디오 라이브러리에 액세스하려면 **사진 라이브러리**의 확인란과 **비디오 라이브러리**의 확인란을 선택합니다.



## <a name="select-frame-sources-and-frame-source-groups"></a>프레임 원본과 프레임 원본 그룹 선택

오디오 프레임을 캡처하는 첫 단계는 마이크나 다른 오디오 캡처 디바이스와 같은 오디오 데이터 소스를 나타내는 [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource)를 초기화하는 것입니다. 이 작업을 하려면 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 개체의 새로운 인스턴스를 만들어야 합니다. 이 예제에서는 **MediaCapture**의 초기화 설정만이 캡처 디바이스에서 오디오를 스트리밍해야 함을 나타내는[**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) 설정입니다. 

[**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync)를 호출한 후 [**FrameSources**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.framesources) 속성으로 액세스 가능한 미디어 프레임 소스 목록을 가져올 수 있습니다. 이 예제에서는 Linq 쿼리를 사용하여 모든 프레임 소스를 선택하며, 이 경우에 프레임 소스를 나타내는 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)에는 **Audio**의 [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype)가 있고, 이것은 미디어 소스가 오디오 데이터를 생성함을 나타냅니다.

쿼리에서 하나 이상의 프레임 소스를 반환하는 경우 [**CurrentFormat**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) 속성을 선택하여 이 예제에서와 같이 필요한 오디오 포맷 즉, float 오디오 데이터를 지원하는지 확인할 수 있습니다. [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties)를 확인하여 필요한 오디오 인코딩을 소스에서 지원하는지 확인합니다.

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>MediaFrameReader 만들기 및 시작

새로운 **MediaFrameReader** 인스턴스를 가져오려면 [**MediaCapture.CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_)를 호출하여 이전 단계에서 선택한 **MediaFrameSource** 개체를 전달합니다. 기본적으로 오디오 프레임은 버퍼링된 모드에서 가져오므로 프레임이 삭제될 가능성이 적습니다. 그러나 오디오 프레임을 충분히 빠르게 처리하지 않고 시스템의 할당된 메모리 버퍼를 채우는 경우에도 여전히 발생할 수 있습니다.

오디오 데이터의 새 프레임을 사용할 수 있을 때 발생하는 [**MediaFrameReader.FrameArrived**](*https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 이벤트에 대한 처리기를 등록합니다. [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync)를 호출하여 오디오 프레임 획득을 시작합니다. 프레임 읽기 프로그램이 시작되지 않으면 [**Success**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus) 이외에 호출에서 반환된 상태 값은 값을 갖습니다.

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

**FrameArrived** 이벤트 처리기에서 최근 미디어 프레임에 대한 참조 검색을 시도하기 위해 보낸 사람으로 전달된 **MediaFrameReader**에서 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe)를 호출합니다. 이 개체는 null일 수 있으므로 개체를 사용하기 전에 항상 확인해야 합니다. **TryAcquireLatestFrame**에서 반환된 **MediaFrameReference**에 래핑된 미디어 프레임의 유형은 획득할 프레임 읽기 프로그램을 구성한 프레임 소스나 소스 유형에 따라 달라집니다. 이 예제에서는 프레임 읽기 프로그램이 오디오 프레임을 획득하도록 설정되었기 때문에 [**AudioMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe) 속성을 사용하여 기본 프레임을 가져옵니다. 

아래 예제에서 이 **ProcessAudioFrame** 도우미 메서드는 [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe)을 가져오는 방법을 보여 주며, 이것은 프레임의 타임스탬프와 같은 정보와 **AudioMediaFrame** 개체에서 불연속 여부를 제공합니다.  오디오 샘플 데이터를 읽거나 처리하려면 **AudioMediaFrame** 개체에서 [**AudioBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer) 개체를 가져오고, [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)를 만들고 나서 COM 메서드 **IMemoryBufferByteAccess::GetBuffer**를 호출하여 데이터를 검색해야 합니다. 기본 버퍼 액세스에 대한 자세한 내용은 코드 목록 아래 나오는 메모를 참조하세요.

이 데이터 형식은 프레임 원본에 따라 달라집니다. 이 예제에서 미디어 프레임 소스를 선택할 때 선택한 프레임 소스가 단일 float 데이터 채널을 사용했음을 확실히 확신했습니다. 나머지 예제 코드는 프레임의 오디오 데이터에 대한 지속 시간과 샘플 수를 결정하는 방법을 보여줍니다.  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> 오디오 데이터에서 작동하려면 원시 메모리 버퍼에 액세스해야 합니다. 이 작업을 하려면 아래 코드 목록을 포함하여 **IMemoryBufferByteAccess** COM 인터페이스를 사용해야 합니다. 원시 버퍼의 작동은 **unsafe** 키워드를 사용하는 메서드에서 수행해야 합니다. 또한 확인란을 선택하여 **프로젝트 -> 속성** 대화 상자의  **빌드** 탭에서 안전 하지 않은 코드를 허용해야 합니다.

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>오디오 데이터에서 MediaFrameReader 사용에 대한 추가 정보

오디오 프레임 소스와 관련된 [**AudioDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.AudioDeviceController)를 검색하려면 [**MediaFrameSource.Controller**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.controller) 속성에 액세스합니다. 이 개체는 캡처 디바이스의 스트림 속성을 가져오거나 설정하거나 캡처 수준을 제어하는 데 사용할 수 있습니다. 다음 예제는 프레임이 프레임 읽기 프로그램을 통해 계속 획득되도록 오디오 디바이스 음을 소거한 것이지만, 모든 샘플의 값은 0입니다.

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

[**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) 개체를 사용하여 미디어 프레임 소스에서 캡처한 오디오 데이터를 [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph)로 전달할 수 있습니다. [**AudioFrameInputNode**](https://docs.microsoft.com/en-us/uwp/api/windows.media.audio.audioframeinputnode)의  [**AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) 메서드에 프레임을 전달합니다. 오디오 그래프를 사용하여 오디오 신호를 캡처하고, 처리하고, 혼합하는 방법에 대한 자세한 내용은 [오디오 그래프](audio-graphs.md)를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [카메라 프레임 샘플](http://go.microsoft.com/fwlink/?LinkId=823230)
* [오디오 그래프](audio-graphs.md)
 






