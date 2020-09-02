---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: 이 문서에서는 MediaCapture와 함께 MediaFrameReader를 사용 하 여 캡처 원본에서 오디오 데이터를 포함 하는 오디오 프레임을 가져오는 방법을 보여 줍니다.
title: MediaFrameReader를 사용하여 오디오 프레임 처리
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18ef0ee1efb7a69a8b305c9b95e84938fe6fde32
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363896"
---
# <a name="process-audio-frames-with-mediaframereader"></a>MediaFrameReader를 사용하여 오디오 프레임 처리

이 문서에서는 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 와 함께 [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 를 사용 하 여 미디어 프레임 소스에서 오디오 데이터를 가져오는 방법을 보여 줍니다. **MediaFrameReader** 를 사용 하 여 색, 적외선 또는 깊이 카메라와 같은 이미지 데이터를 가져오는 방법에 대해 알아보려면 [MediaFrameReader를 사용 하 여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)를 참조 하세요. 이 문서에서는 프레임 판독기 사용 패턴의 일반적인 개요를 제공 하 고 **MediafMediaFrameReader Esourcegroup** 을 사용 하 여 동시에 여러 소스에서 프레임을 검색 **하는 것** 과 같은 일부 추가 기능을 설명 합니다. 

> [!NOTE] 
> 이 문서에서 설명 하는 기능은 Windows 10 버전 1803부터 사용할 수 있습니다.

> [!NOTE] 
> 유니버설 Windows 앱 샘플에서는 **MediaFrameReader**를 사용하여 색, 깊이, 적외선 카메라 등 다른 프레임 원본의 프레임을 표시하는 방법을 보여 줍니다. 자세한 내용은 [카메라 프레임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)을 참조하세요.

## <a name="setting-up-your-project"></a>프로젝트 설정
오디오 프레임을 확보 하는 프로세스는 대부분 다른 유형의 미디어 프레임을 가져오는 것과 같습니다. **MediaCapture**를 사용하는 앱의 경우 카메라 장치에 액세스하기 전에 앱에서 *웹캠* 기능을 사용하도록 선언해야 합니다. 앱이 오디오 장치에서 캡처하는 경우 *마이크* 장치 기능도 선언해야 합니다. 

**앱 매니페스트에 기능 추가**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **기능** 탭을 선택합니다.
3.  **웹캠** 확인란과 **마이크** 상자를 선택합니다.
4.  사진과 비디오 라이브러리에 액세스하려면 **사진 라이브러리**의 확인란과 **비디오 라이브러리**의 확인란을 선택합니다.



## <a name="select-frame-sources-and-frame-source-groups"></a>프레임 원본과 프레임 원본 그룹 선택

오디오 프레임을 캡처하는 첫 번째 단계는 마이크 또는 기타 오디오 캡처 장치와 같은 오디오 데이터의 원본을 나타내는 [**Mediaf를**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) 초기화 하는 것입니다. 이렇게 하려면 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 개체의 새 인스턴스를 만들어야 합니다. 이 예제에서 **MediaCapture** 에 대 한 유일한 초기화 설정은 캡처 장치에서 오디오를 스트리밍할 것임을 나타내기 위해 [**StreamingCaptureMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) 를 설정 하는 것입니다. 

[**TializeAsyncMediaCapture.Ini**](/uwp/api/windows.media.capture.mediacapture.initializeasync)를 호출한 후 [**FrameSources**](/uwp/api/windows.media.capture.mediacapture.framesources) 속성을 사용 하 여 액세스 가능한 미디어 프레임 소스 목록을 가져올 수 있습니다. 이 예제에서는 Linq 쿼리를 사용 하 여 프레임 소스를 설명 하는 [**Mediaframesourceinfo**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo) 가 미디어 원본에서 오디오 데이터를 생성 함을 나타내는  [**mediastreamtype**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) **오디오**를 사용 하는 모든 프레임 소스를 선택 합니다.

쿼리가 하나 이상의 프레임 원본을 반환 하는 경우 [**Currentformat**](/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) 속성을 확인 하 여 원본에서 원하는 오디오 형식 (이 예제에서는 부동 오디오 데이터)을 지원 하는지 확인할 수 있습니다. [**AudioEncodingProperties**](/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) 를 확인 하 여 원하는 오디오 인코딩이 원본에서 지원 되는지 확인 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitAudioFrameSource":::

## <a name="create-and-start-the-mediaframereader"></a>MediaFrameReader 만들기 및 시작

이전 단계에서 선택한 **MediafMediaFrameReader** 의 개체를 **전달 하 여** [**MediaCapture**](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_)를 호출 하 여 새 인스턴스를 가져옵니다. 기본적으로 오디오 프레임은 버퍼링 모드에서 가져오며,이는 오디오 프레임을 충분히 빠르게 처리 하지 않고 시스템의 안에 메모리 버퍼를 채우는 경우에도 발생할 수 있습니다.

[**MediaFrameReader FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 이벤트에 대 한 처리기를 등록 합니다 .이 이벤트는 시스템에서 오디오 데이터의 새 프레임을 사용할 수 있을 때 발생 합니다. [**StartAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.startasync) 를 호출 하 여 오디오 프레임 획득을 시작 합니다. 프레임 판독기를 시작 하지 못한 경우 호출에서 반환 된 상태 값의 값은 [**Success**](/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus)가 아닙니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCreateAudioFrameReader":::

**FrameArrived** 이벤트 처리기에서 최신 미디어 프레임에 대 한 참조를 검색 하기 위해 처리기에 보낸 사람으로 전달 된 **MediaFrameReader** 개체의 [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) 를 호출 합니다. 이 개체는 null 일 수 있으므로 항상 개체를 사용 하기 전에 확인 해야 합니다. **TryAcquireLatestFrame** 에서 반환 된 **MediaFrameReference** 에 래핑된 미디어 프레임의 입력은 프레임 판독기에서 획득 하도록 구성한 프레임 소스 또는 소스 유형에 따라 달라 집니다. 이 예제의 프레임 판독기는 오디오 프레임을 가져오기 위해 설정 되었으므로 [**AudioMediaFrame**](/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe) 속성을 사용 하 여 기본 프레임을 가져옵니다. 

아래 예제의이 **ProcessAudioMediaFrame frame** 도우미 메서드는 프레임 타임 스탬프와 같은 정보를 제공 하는 [**오디오 프레임**](/uwp/api/windows.media.audioframe) 및이 **프레임의 불연속** 프레임을 가져오는 방법을 보여 줍니다. 오디오 샘플 데이터를 읽거나 처리 하려면 **AudioMediaFrame** 개체에서 [**AudioBuffer**](/uwp/api/windows.media.audiobuffer) 개체를 가져오고 [**IMemoryBufferReference**](/uwp/api/windows.foundation.imemorybufferreference)를 만든 다음 COM 메서드 **IMemoryBufferByteAccess:: getbuffer** 를 호출 하 여 데이터를 검색 해야 합니다. 네이티브 버퍼에 액세스 하는 방법에 대 한 자세한 내용은 코드 목록 아래에 있는 참고를 참조 하세요.

데이터의 형식은 프레임 원본에 따라 달라 집니다. 이 예에서 미디어 프레임 원본을 선택 하는 경우 선택한 프레임 원본이 float 데이터의 단일 채널을 사용 했는지 명확 하 게 결정 합니다. 예제 코드의 나머지 부분에서는 프레임에 있는 오디오 데이터의 기간 및 샘플 수를 확인 하는 방법을 보여 줍니다.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetProcessAudioFrame":::

> [!NOTE] 
> 오디오 데이터에 대해 작업을 수행 하려면 네이티브 메모리 버퍼에 액세스 해야 합니다. 이렇게 하려면 아래 코드 목록을 포함 하 여 **IMemoryBufferByteAccess** COM 인터페이스를 사용 해야 합니다. 네이티브 버퍼에 대 한 작업은 **unsafe** 키워드를 사용 하는 메서드에서 수행 해야 합니다. 또한 **프로젝트-> 속성** 대화 상자의 **빌드** 탭에서 안전 하지 않은 코드를 허용 하는 확인란을 선택 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetIMemoryBufferByteAccess":::

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>오디오 데이터에 MediaFrameReader 사용에 대 한 추가 정보

[**MediafAudioDeviceController**](/uwp/api/windows.media.capture.frames.mediaframesource.controller) 속성에 액세스 하 여 오디오 프레임 원본과 연결 된 [**AudioDeviceController**](/uwp/api/Windows.Media.Devices.AudioDeviceController) 를 검색할 수 있습니다. 이 개체는 캡처 장치의 스트림 속성을 가져오거나 설정 하거나 캡처 수준을 제어 하는 데 사용할 수 있습니다. 다음 예제에서는 프레임 판독기에서 프레임을 계속 얻을 수 있도록 오디오 장치를 mutes 모든 샘플 값은 0입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetAudioDeviceControllerMute":::

오디오 [**프레임**](/uwp/api/windows.media.audioframe) 개체를 사용 하 여 미디어 프레임 소스에서 캡처된 오디오 데이터를 오디오 [**그래프**](/uwp/api/windows.media.audio.audiograph)에 전달할 수 있습니다. 프레임을 [**오디오**](/uwp/api/windows.media.audio.audioframeinputnode)프레임의 [**addframe**](/uwp/api/windows.media.audio.audioframeinputnode.addframe) 메서드에 전달 합니다. 오디오 그래프를 사용 하 여 오디오 신호를 캡처, 처리 및 혼합 하는 방법에 대 한 자세한 내용은 [오디오 그래프](audio-graphs.md)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [카메라 프레임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [오디오 그래프](audio-graphs.md)
 
