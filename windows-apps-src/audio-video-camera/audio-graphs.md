---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: 이 문서에서는 Windows. Media. Audio 네임 스페이스의 Api를 사용 하 여 오디오 라우팅, 혼합 및 처리 시나리오에 대 한 오디오 그래프를 만드는 방법을 보여 줍니다.
title: 오디오 그래프
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 72a6fe2e704bc419306c74f410ed51e8e8560fa6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362976"
---
# <a name="audio-graphs"></a>오디오 그래프



이 문서에서는 [**Windows. Media. audio**](/uwp/api/Windows.Media.Audio) 네임 스페이스의 api를 사용 하 여 오디오 라우팅, 혼합 및 처리 시나리오에 대 한 오디오 그래프를 만드는 방법을 보여 줍니다.

*오디오 그래프* 는 오디오 데이터 흐름을 통해 상호 연결 된 오디오 노드 집합입니다. 

- 오디오 *입력 노드* 는 오디오 입력 장치, 오디오 파일 또는 사용자 지정 코드에서 그래프에 오디오 데이터를 제공 합니다. 

- *오디오 출력 노드* 는 그래프에 의해 처리 되는 오디오의 대상입니다. 오디오를 그래프에서 오디오 출력 장치, 오디오 파일 또는 사용자 지정 코드로 라우팅할 수 있습니다. 

- *서브 믹스 노드* 는 하나 이상의 노드에서 오디오를 가져오고이를 그래프의 다른 노드로 라우팅할 수 있는 단일 출력으로 결합 합니다. 

모든 노드를 만들고 연결을 설정한 후에는 오디오 그래프를 시작 하 고, 입력 노드에서 모든 서브 믹스 노드를 통해 출력 노드로 오디오 데이터 흐름이 시작 됩니다. 이 모델은 장치 마이크에서 오디오 파일로 기록 하거나, 파일에서 장치의 스피커로 오디오를 재생 하거나, 여러 원본의 오디오를 빠르고 쉽게 구현 하는 등의 시나리오를 가능 하 게 합니다.

오디오 그래프에 오디오 효과를 추가 하 여 추가 시나리오를 사용할 수 있습니다. 오디오 그래프의 모든 노드는 노드를 통과 하는 오디오에서 오디오 처리를 수행 하는 오디오 효과를 0 개 이상 채울 수 있습니다. 몇 줄의 코드로 오디오 노드에 연결할 수 있는 echo, 이퀄라이저, 제한, 반향 등의 몇 가지 기본 효과가 있습니다. 기본 제공 효과와 정확히 동일 하 게 작동 하는 사용자 지정 오디오 효과를 만들 수도 있습니다.

> [!NOTE]
> [오디오 그래프 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AudioCreation) 은이 개요에서 설명 하는 코드를 구현 합니다. 샘플을 다운로드 하 여 컨텍스트에서 코드를 확인 하거나 사용자 고유의 앱에 대 한 시작 점으로 사용할 수 있습니다.

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>Windows 런타임 오디오 그래프 또는 XAudio2 선택

Windows 런타임 audio graph Api는 COM 기반 [XAudio2 api](/windows/desktop/xaudio2/xaudio2-apis-portal)를 사용 하 여 구현할 수 있는 기능을 제공 합니다. 다음은 XAudio2와 다른 Windows 런타임 오디오 그래프 프레임 워크의 기능입니다.

Windows 런타임 오디오 그래프 Api:

-   XAudio2 보다 훨씬 더 쉽게 사용할 수 있습니다.
-   C + +에 대해 지원 되는 것 외에도 c #에서 사용할 수 있습니다.
-   압축 된 파일 형식을 포함 하 여 오디오 파일을 직접 사용할 수 있습니다. XAudio2는 오디오 버퍼에 대해서만 작동 하며 파일 i/o 기능을 제공 하지 않습니다.
-   Windows 10에서 대기 시간이 짧은 오디오 파이프라인을 사용할 수 있습니다.
-   기본 끝점 매개 변수가 사용 되는 경우 자동 끝점 전환을 지원 합니다. 예를 들어 사용자가 장치 스피커에서 헤드셋으로 전환 하면 오디오가 자동으로 새 입력으로 리디렉션됩니다.

## <a name="audiograph-class"></a>오디오 그래프 클래스

[**오디오 그래프**](/uwp/api/Windows.Media.Audio.AudioGraph) 클래스는 그래프를 구성 하는 모든 노드의 부모입니다. 이 개체를 사용 하 여 모든 오디오 노드 형식의 인스턴스를 만들 수 있습니다. 그래프에 대 한 구성 설정을 포함 하는 [**오디오 Graphsettings**](/uwp/api/Windows.Media.Audio.AudioGraphSettings) 개체를 초기화 한 다음, [**오디오 그래프. createasync**](/uwp/api/windows.media.audio.audiograph.createasync)를 호출 하 여 **오디오 그래프** 클래스의 인스턴스를 만듭니다. 반환 된 [**createaudio Graphresult**](/uwp/api/Windows.Media.Audio.CreateAudioGraphResult) 는 생성 된 오디오 그래프에 대 한 액세스를 제공 하거나 오디오 그래프 만들기에 실패 하는 경우 오류 값을 제공 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareAudioGraph":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetInitAudioGraph":::

-   모든 오디오 노드 형식은 오디오 \* **그래프** 클래스의 Create 메서드를 사용 하 여 만듭니다.
-   Audio [**graph. Start**](/uwp/api/windows.media.audio.audiograph.start) 메서드는 오디오 그래프가 오디오 데이터의 처리를 시작 하도록 합니다. 오디오 [**그래프. Stop**](/uwp/api/windows.media.audio.audiograph.stop) 메서드는 오디오 처리를 중지 합니다. 그래프의 각 노드는 그래프를 실행 하는 동안 독립적으로 시작 및 중지 될 수 있지만 그래프가 중지 될 때 활성화 되는 노드는 없습니다. [**ResetAllNodes**](/uwp/api/windows.media.audio.audiograph.resetallnodes) 를 설정 하면 그래프의 모든 노드가 현재 오디오 버퍼에 있는 모든 데이터를 삭제 합니다.
-   [**QuantumStarted**](/uwp/api/windows.media.audio.audiograph.quantumstarted) 이벤트는 그래프가 오디오 데이터의 새 퀀텀 처리를 시작할 때 발생 합니다. [**QuantumProcessed**](/uwp/api/windows.media.audio.audiograph.quantumprocessed) 이벤트는 퀀텀 처리가 완료 될 때 발생 합니다.

-   필요한 AudioRenderCategory [**Graphsettings**](/uwp/api/Windows.Media.Audio.AudioGraphSettings) 속성은 [**AudioRenderCategory**](/uwp/api/Windows.Media.Render.AudioRenderCategory)뿐입니다. 이 값을 지정 하면 시스템에서 지정 된 범주에 대 한 오디오 파이프라인을 최적화할 수 있습니다.
-   오디오 그래프의 퀀텀 크기는 한 번에 처리 되는 샘플 수를 결정 합니다. 기본적으로 퀀텀 크기는 기본 샘플링 주기를 기반으로 10 밀리초입니다. [**DesiredSamplesPerQuantum**](/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum) 속성을 설정 하 여 사용자 지정 퀀텀 크기를 지정 하는 경우 [**QuantumSizeSelectionMode**](/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) 속성도 **closesttodesired** 로 설정 하거나 제공 된 값이 무시 되도록 설정 해야 합니다. 이 값을 사용 하는 경우 시스템은 사용자가 지정 하는 것과 최대한 가까운 퀀텀 크기를 선택 합니다. 실제 퀀텀 크기를 확인 하려면 **오디오 그래프가** 만들어진 후 [**SamplesPerQuantum**](/uwp/api/windows.media.audio.audiograph.samplesperquantum) 를 확인 합니다.
-   파일에 오디오 그래프를 사용 하 고 오디오 장치로 출력 하지 않으려는 경우에는 [**DesiredSamplesPerQuantum**](/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum) 속성을 설정 하지 않고 기본 퀀텀 크기를 사용 하는 것이 좋습니다.
-   [**DesiredRenderDeviceAudioProcessing**](/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing) 속성은 기본 렌더링 장치가 오디오 그래프의 출력에서 수행 하는 처리 크기를 결정 합니다. **기본** 설정을 사용 하면 시스템에서 지정 된 오디오 렌더링 범주에 대 한 기본 오디오 처리를 사용할 수 있습니다. 이러한 처리는 일부 장치, 특히 소규모 스피커가 있는 모바일 장치에서 오디오의 소리를 크게 향상 시킬 수 있습니다. **원시** 설정은 수행 된 신호 처리 크기를 최소화 하 여 성능을 향상 시킬 수 있지만 일부 장치의 경우 품질이 저하 될 수 있습니다.
-   [**QuantumSizeSelectionMode**](/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) 를 **lowestlatency**로 설정 하면 오디오 그래프가 자동으로 [**DesiredRenderDeviceAudioProcessing**](/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing)에 대해 **Raw** 를 사용 합니다.
- Windows 10, 버전 1803부터 [**MaxPlaybackSpeedFactor**](/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) 속성을 설정 하 여 [**PlaybackSpeedFactor,**](/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor), [**PlaybackSpeedFactor**](/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor)및 [**MediaSourceInputNode**](/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor) 속성에 사용 되는 최대 값을 설정할 수 있습니다. 오디오 그래프가 1 보다 큰 재생 속도 비율을 지 원하는 경우 충분 한 오디오 데이터 버퍼를 유지 하기 위해 시스템에서 추가 메모리를 할당 해야 합니다. 이러한 이유로 **MaxPlaybackSpeedFactor** 를 앱에 필요한 가장 낮은 값으로 설정 하면 앱의 메모리 소비가 줄어듭니다. 앱이 정상 속도로만 콘텐츠를 재생 하는 경우 MaxPlaybackSpeedFactor를 1로 설정 하는 것이 좋습니다.
-   [**EncodingProperties**](/uwp/api/windows.media.audio.audiographsettings.encodingproperties) 는 그래프에서 사용 되는 오디오 형식을 결정 합니다. 32 비트 부동 소수점 형식만 지원 됩니다.
-   [**Primaryrenderdevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) 는 오디오 그래프의 기본 렌더링 장치를 설정 합니다. 설정 하지 않으면 기본 시스템 장치가 사용 됩니다. 기본 렌더링 장치는 그래프의 다른 노드에 대 한 퀀텀 크기를 계산 하는 데 사용 됩니다. 시스템에 오디오 렌더링 장치가 없으면 오디오 그래프가 생성 되지 않습니다.

오디오 그래프가 기본 오디오 렌더링 장치를 사용 하도록 하거나, [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 를 호출 하 [**Windows.Devices.Enumeration.DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 고 [**windows. GetAudioRenderSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector)에서 반환 되는 오디오 렌더링 장치 선택기를 전달 하 여 시스템의 사용 가능한 오디오 렌더링 장치 목록을 가져올 수 있습니다. 반환 된 **Deviceinformation** 개체 중 하나를 프로그래밍 방식으로 선택 하거나 UI를 표시 하 여 사용자가 장치를 선택 하 고이를 사용 하 여 [**primaryrenderdevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) 속성을 설정할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetEnumerateAudioRenderDevices":::

##  <a name="device-input-node"></a>장치 입력 노드

장치 입력 노드는 마이크와 같이 시스템에 연결 된 오디오 캡처 장치에서 그래프로 오디오를 피드 합니다. [**Createdeviceinputnodeasync**](/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync)를 호출 하 여 시스템의 기본 오디오 캡처 장치를 사용 하는 [**deviceinputnode**](/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) 개체를 만듭니다. 시스템이 지정 된 범주에 대 한 오디오 파이프라인을 최적화할 수 있도록 [**AudioRenderCategory**](/uwp/api/Windows.Media.Render.AudioRenderCategory) 을 제공 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateDeviceInputNode":::

장치 입력 노드에 대 한 특정 오디오 캡처 장치를 지정 하려면 [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 를 호출 하 고 [**GetAudioCaptureSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiocaptureselector)에서 반환 된 오디오 렌더링 장치 선택기를 전달 하 여 시스템의 사용 가능한 오디오 캡처 장치 목록을 가져오려면 [**windows**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) . x s e. m i c. m a c. 반환 된 **Deviceinformation** 개체 중 하나를 프로그래밍 방식으로 선택 하거나 UI를 표시 하 여 사용자가 장치를 선택한 후 [**Createdeviceinputnodeasync**](/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync)에 전달할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetEnumerateAudioCaptureDevices":::

##  <a name="device-output-node"></a>장치 출력 노드

장치 출력 노드는 그래프에서 오디오 렌더링 장치 (예: 스피커 또는 헤드셋)로 오디오를 푸시합니다. [**Createdeviceoutputnodeasync**](/uwp/api/windows.media.audio.audiograph.createdeviceoutputnodeasync)를 호출 하 여 [**deviceoutputnode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) 를 만듭니다. 출력 노드는 오디오 그래프의 [**Primaryrenderdevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) 를 사용 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceOutputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateDeviceOutputNode":::

##  <a name="file-input-node"></a>파일 입력 노드

파일 입력 노드를 사용 하면 오디오 파일의 데이터를 그래프로 공급할 수 있습니다. [**Createfileinputnodeasync**](/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync)를 호출 하 여 [**오디오 fileinputnode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) 를 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFileInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFileInputNode":::

-   파일 입력 노드는 mp3, wav, wma, m4a 등의 파일 형식을 지원 합니다.
-   [**StartTime**](/uwp/api/windows.media.audio.audiofileinputnode.starttime) 속성을 설정 하 여 재생이 시작 되어야 하는 파일에 대 한 시간 오프셋을 지정 합니다. 이 속성이 null 이면 파일의 시작 부분이 사용 됩니다. [**EndTime**](/uwp/api/windows.media.audio.audiofileinputnode.endtime) 속성을 설정 하 여 재생이 종료 되는 파일에 대 한 시간 오프셋을 지정 합니다. 이 속성이 null 이면 파일의 끝이 사용 됩니다. 시작 시간 값은 종료 시간 값 보다 작아야 하며, 종료 시간 값은 오디오 파일의 기간 보다 작거나 같아야 합니다. 이러한 값은 [**duration**](/uwp/api/windows.media.audio.audiofileinputnode.duration) 속성 값을 확인 하 여 확인할 수 있습니다.
-   [**Seek**](/uwp/api/windows.media.audio.audiofileinputnode.seek) 를 호출 하 고 재생 위치를 이동 해야 하는 파일에 대 한 시간 오프셋을 지정 하 여 오디오 파일에서 위치를 찾습니다. 지정 된 값은 [**StartTime**](/uwp/api/windows.media.audio.audiofileinputnode.starttime) 및 [**EndTime**](/uwp/api/windows.media.audio.audiofileinputnode.endtime) 범위 내에 있어야 합니다. 읽기 전용 [**위치**](/uwp/api/windows.media.audio.audiofileinputnode.position) 속성을 사용 하 여 노드의 현재 재생 위치를 가져옵니다.
-   [**LoopCount**](/uwp/api/windows.media.audio.audiofileinputnode.loopcount) 속성을 설정 하 여 오디오 파일의 루핑을 사용 하도록 설정 합니다. Null이 아닌 경우이 값은 초기 재생 후에 파일이 재생 되는 횟수를 나타냅니다. 따라서 예를 들어 **LoopCount** 를 1로 설정 하면 파일은 총 2 회 재생 되 고 5로 설정 하면 파일이 총 6 회 재생 됩니다. **LoopCount** 를 null로 설정 하면 파일이 무기한 반복 됩니다. 반복을 중지 하려면 값을 0으로 설정 합니다.
-   [**PlaybackSpeedFactor**](/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor)를 설정 하 여 오디오 파일을 재생 하는 속도를 조정 합니다. 값이 1 이면 파일의 원래 속도는 0.5이 고 2는 2 배속입니다.

##  <a name="mediasource-input-node"></a>MediaSource 입력 노드

[**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 클래스는 다른 원본의 미디어를 참조 하는 일반적인 방법을 제공 하 고 디스크, 스트림 또는 적응 스트리밍 네트워크 원본에 있는 파일 일 수 있는 기본 미디어 형식에 관계 없이 미디어 데이터에 액세스 하기 위한 공통 모델을 노출 합니다. [* * MediaSourceAudioInputNode](/uwp/api/windows.media.audio.mediasourceaudioinputnode) 노드를 사용 하면 오디오 데이터를 **MediaSource** 에서 오디오 그래프로 보낼 수 있습니다. [**CreateMediaSourceAudioInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_)를 호출 하 고 재생할 콘텐츠를 나타내는 **MediaSource** 개체를 전달 하 여 **MediaSourceAudioInputNode** 를 만듭니다. [**상태**](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status) 속성을 확인 하 여 작업의 상태를 확인 하는 데 사용할 수 있는 [* * CreateMediaSourceAudioInputNodeResult](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult) 이 반환 됩니다. 상태가 **Success**인 경우 [**Node**](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node) 속성에 액세스 하 여 만든 **MediaSourceAudioInputNode** 를 가져올 수 있습니다. 다음 예제에서는 네트워크를 통해 콘텐츠 스트리밍을 나타내는 AdaptiveMediaSource 개체에서 노드를 만드는 방법을 보여 줍니다. **MediaSource**작업에 대 한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조 하세요. 인터넷을 통해 미디어 콘텐츠를 스트리밍하는 방법에 대 한 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조 하세요.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSourceInputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceInputNode":::

재생이 **MediaSource** 콘텐츠의 끝에 도달 했을 때 알림을 받으려면 [**MediaSourceCompleted**](/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted) 이벤트에 대 한 처리기를 등록 합니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetRegisterMediaSourceCompleted":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetMediaSourceCompleted":::

Diskis 파일을 재생 하는 것은 항상 성공적으로 완료 될 수 있지만 네트워크 연결 변경 또는 오디오 그래프의 제어 외부에 있는 기타 문제로 인해 재생 중에 네트워크 원본에서 스트리밍된 미디어가 실패할 수 있습니다. 재생 하는 동안 **MediaSource** 가 재생 되 면 오디오 그래프가 [**UnrecoverableErrorOccurred**](/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred) 이벤트를 발생 시킵니다. 이 이벤트에 대 한 처리기를 사용 하 여 오디오 그래프를 중지 및 삭제 한 다음 그래프를 다시 초기화할 수 있습니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetRegisterUnrecoverableError":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUnrecoverableError":::

##  <a name="file-output-node"></a>파일 출력 노드

파일 출력 노드를 사용 하면 오디오 데이터를 그래프에서 오디오 파일로 보낼 수 있습니다. [**Createfileoutputnodeasync**](/uwp/api/windows.media.audio.audiograph.createfileoutputnodeasync)를 호출 하 여 [**오디오 fileoutputnode**](/uwp/api/Windows.Media.Audio.AudioFileOutputNode) 를 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFileOutputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFileOutputNode":::

-   파일 출력 노드는 mp3, wav, wma, m4a 등의 파일 형식을 지원 합니다.
-   [**오디오 Fileoutputnode**](/uwp/api/windows.media.audio.audiofileoutputnode.stop) 를 호출 해야 합니다. [**FinalizeAsync**](/uwp/api/windows.media.audio.audiofileoutputnode.finalizeasync) 를 호출 하기 전에 노드의 처리를 중지 하려면 중지 하세요. 그렇지 않으면 예외가 throw 됩니다.

##  <a name="audio-frame-input-node"></a>오디오 프레임 입력 노드

오디오 프레임 입력 노드를 사용 하면 사용자의 코드에서 생성 하는 오디오 데이터를 오디오 그래프로 푸시할 수 있습니다. 이렇게 하면 사용자 지정 소프트웨어 신시사이저를 만드는 등의 시나리오를 사용할 수 있습니다. [**Createframeinputnode**](/uwp/api/windows.media.audio.audiograph.createframeinputnode)를 호출 하 여 [**오디오 프레임**](/uwp/api/Windows.Media.Audio.AudioFrameInputNode) 을 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFrameInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFrameInputNode":::

QuantumStarted 이벤트는 오디오 그래프가 다음에 오디오 데이터의 퀀텀 처리를 시작할 준비가 되었을 때 발생 합니다 [**.**](/uwp/api/windows.media.audio.audioframeinputnode.quantumstarted) 처리기 내에서이 이벤트에 대 한 사용자 지정 생성 오디오 데이터를 제공 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetQuantumStarted":::

-   **QuantumStarted** 이벤트 처리기로 전달 되는 [**FrameInputNodeQuantumStartedEventArgs**](/uwp/api/Windows.Media.Audio.FrameInputNodeQuantumStartedEventArgs) 개체는 오디오 그래프가 처리할 퀀텀을 채워야 하는 샘플 수를 나타내는 [**RequiredSamples**](/uwp/api/windows.media.audio.frameinputnodequantumstartedeventargs.requiredsamples) 속성을 노출 합니다.
-   오디오 데이터를 사용 하 여 채워진 오디오 [**프레임**](/uwp/api/Windows.Media.AudioFrame) 개체를 그래프로 전달 하려면 오디오 데이터를 호출 합니다 [**. addframe**](/uwp/api/windows.media.audio.audioframeinputnode.addframe) .
- 오디오 데이터가 포함된 **MediaFrameReader**를 사용하기 위한 새로운 API 세트가 Windows 10, 1803 버전에 도입되었습니다. 이러한 Api를 사용 하면 **addframe** 메서드를 사용 하 여 프레임 코드 **노드에** 전달 될 수 있는 미디어 프레임 소스에서 **오디오 프레임** 개체를 가져올 수 있습니다. 자세한 내용은 [MediaFrameReader를 사용하여 오디오 프레임 처리](process-audio-frames-with-mediaframereader.md)를 참조하세요.
-   **Generate의 odata** 도우미 메서드 구현 예제는 다음과 같습니다.

오디오 데이터를 사용 하 여 오디오 [**프레임**](/uwp/api/Windows.Media.AudioFrame) 을 채우려면 오디오 프레임의 기본 메모리 버퍼에 대 한 액세스 권한을 얻어야 합니다. 이렇게 하려면 네임스페이스 내에 다음 코드를 추가하여 **IMemoryBufferByteAccess** COM 인터페이스를 초기화해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetComImportIMemoryBufferByteAccess":::

다음 코드에서는 오디오 [**프레임**](/uwp/api/Windows.Media.AudioFrame) 을 만들고 오디오 데이터로 채우는 **generate오디오 odata** 도우미 메서드의 구현 예를 보여 줍니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetGenerateAudioData":::

-   이 메서드는 Windows 런타임 형식을 기반으로 하는 원시 버퍼에 액세스 하므로 **unsafe** 키워드를 사용 하 여 선언 해야 합니다. 또한 프로젝트의 **속성** 페이지를 열고, **빌드** 속성 페이지를 클릭 하 고, **안전 하지 않은 코드 허용** 확인란을 선택 하 여 안전 하지 않은 코드를 컴파일할 수 있도록 Microsoft Visual Studio에서 프로젝트를 구성 해야 합니다.
-   원하는 버퍼 크기를 생성자에 전달 하 여 **Windows Media** 네임 스페이스에서 [**오디오 프레임**](/uwp/api/Windows.Media.AudioFrame)의 새 인스턴스를 초기화 합니다. 버퍼 크기는 샘플 수와 각 샘플의 크기를 곱한 값입니다.
-   [**Lockbuffer**](/uwp/api/windows.media.audioframe.lockbuffer)를 호출 하 여 오디오 프레임의 [**AudioBuffer**](/uwp/api/Windows.Media.AudioBuffer) 를 가져옵니다.
-   [**CreateReference**](/uwp/api/windows.media.audiobuffer.createreference)를 호출 하 여 오디오 버퍼에서 [**IMemoryBufferByteAccess**](/previous-versions/mt297505(v=vs.85)) COM 인터페이스의 인스턴스를 가져옵니다.
-   [**IMemoryBufferByteAccess**](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) 를 호출 하 여 원시 오디오 버퍼 데이터에 대 한 포인터를 가져오고 오디오 데이터의 샘플 데이터 형식으로 캐스팅 합니다.
-   데이터를 사용 하 여 버퍼를 채우고 오디오 그래프에 제출할 오디오 [**프레임**](/uwp/api/Windows.Media.AudioFrame) 을 반환 합니다.

##  <a name="audio-frame-output-node"></a>오디오 프레임 출력 노드

오디오 프레임 출력 노드를 사용 하면 사용자가 만든 사용자 지정 코드를 사용 하 여 오디오 그래프에서 오디오 데이터 출력을 수신 하 고 처리할 수 있습니다. 이에 대 한 예제 시나리오는 오디오 출력에 대 한 신호 분석을 수행 하는 것입니다. [**Create프레임**](/uwp/api/windows.media.audio.audiograph.createframeoutputnode)outputnode를 호출 하 여 [**오디오 프레임 outputnode**](/uwp/api/Windows.Media.Audio.AudioFrameOutputNode) 를 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFrameOutputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFrameOutputNode":::

오디오 그래프가 오디오 데이터의 퀀텀 처리를 시작 하면 [**QuantumStarted**](/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted) 이벤트가 발생 합니다. 이 이벤트에 대 한 처리기 내에서 오디오 데이터에 액세스할 수 있습니다. 

> [!NOTE]
> 오디오 그래프와 동기화 된 일반 흐름에서 오디오 프레임을 검색 하려는 경우 동기 **QuantumStarted** 이벤트 처리기 내에서 오디오 프레임을 호출 합니다. [getframe](/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame) **QuantumProcessed** 이벤트는 오디오 엔진이 오디오 처리를 완료 한 후 비동기식으로 발생 합니다. 즉, 해당 주기가 불규칙 한 것일 수 있습니다. 따라서 오디오 프레임 데이터의 동기화 된 처리에 **QuantumProcessed** 이벤트를 사용 하면 안 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetQuantumStartedFrameOutput":::

-   [**Getframe**](/uwp/api/windows.media.audio.audioframeoutputnode.getframe) 을 호출 하 여 그래프에서 오디오 데이터로 채워진 오디오 [**프레임**](/uwp/api/Windows.Media.AudioFrame) 개체를 가져옵니다.
-   **Processframeoutput** 도우미 메서드의 예제 구현은 다음과 같습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetProcessFrameOutput":::

-   위의 오디오 프레임 입력 노드 예제와 마찬가지로 **IMemoryBufferByteAccess** COM 인터페이스를 선언 하 고 기본 오디오 버퍼에 액세스 하기 위해 안전 하지 않은 코드를 허용 하도록 프로젝트를 구성 해야 합니다.
-   [**Lockbuffer**](/uwp/api/windows.media.audioframe.lockbuffer)를 호출 하 여 오디오 프레임의 [**AudioBuffer**](/uwp/api/Windows.Media.AudioBuffer) 를 가져옵니다.
-   [**CreateReference**](/uwp/api/windows.media.audiobuffer.createreference)를 호출 하 여 오디오 버퍼에서 **IMemoryBufferByteAccess** COM 인터페이스의 인스턴스를 가져옵니다.
-   **IMemoryBufferByteAccess** 를 호출 하 여 원시 오디오 버퍼 데이터에 대 한 포인터를 가져오고 오디오 데이터의 샘플 데이터 형식으로 캐스팅 합니다.

## <a name="node-connections-and-submix-nodes"></a>노드 연결 및 서브 믹스 노드

모든 입력 노드 형식은 노드에 의해 생성 된 오디오를 메서드로 전달 되는 노드로 라우팅하는 **Addoutgoingconnection** 메서드를 노출 합니다. 다음 예에서는 오디오 [**파일을 장치**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode)스피커에서 재생 하기 위한 간단한 설정 인 오디오 파일 [**inputnode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) 를 오디오 파일에 연결 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection1":::

입력 노드에서 다른 노드로의 연결을 둘 이상 만들 수 있습니다. 다음 예에서는 [**오디오 Fileinputnode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) 에서 [**filefileoutputnode**](/uwp/api/Windows.Media.Audio.AudioFileOutputNode)로 다른 연결을 추가 합니다. 이제 오디오 파일의 오디오가 장치의 스피커로 재생 되 고 오디오 파일에도 기록 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection2":::

출력 노드가 다른 노드에서 두 개 이상의 연결을 수신할 수도 있습니다. 다음 예에서는 [**오디오**](/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) 에서 오디오 [**deviceoutput**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) 노드에 연결 합니다. 출력 노드에는 파일 입력 노드와 장치 입력 노드의 연결이 있으므로 출력에 두 소스의 오디오가 혼합 되어 포함 됩니다. **Addoutgoingconnection** 은 연결을 통해 전달 되는 신호에 대 한 이득 값을 지정할 수 있는 오버 로드를 제공 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection3":::

출력 노드가 여러 노드에서의 연결을 허용할 수 있지만 혼합을 출력에 전달 하기 전에 하나 이상의 노드에서 중간의 신호를 만들 수 있습니다. 예를 들어, 그래프에서 오디오 신호의 하위 집합에 수준을 설정 하거나 효과를 적용 하는 것이 좋습니다. 이렇게 하려면 [**AudioSubmixNode**](/uwp/api/Windows.Media.Audio.AudioSubmixNode)를 사용 합니다. 하나 이상의 입력 노드나 다른 서브 믹스 노드에서 서브 믹스 노드에 연결할 수 있습니다. 다음 예제에서는 [**CreateSubmixNode**](/uwp/api/windows.media.audio.audiograph.createsubmixnode)를 사용 하 여 새 서브 믹스 노드를 만듭니다. 그런 다음 파일 입력 노드 및 프레임 출력 노드에서 서브 믹스 노드에 연결이 추가 됩니다. 마지막으로, 서브 믹스 노드는 파일 출력 노드에 연결 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateSubmixNode":::

## <a name="starting-and-stopping-audio-graph-nodes"></a>오디오 그래프 노드 시작 및 중지

오디오 [**그래프가 호출 되 면 오디오**](/uwp/api/windows.media.audio.audiograph.start) 그래프가 오디오 데이터 처리를 시작 합니다. 모든 노드 형식은 개별 노드가 데이터 처리를 시작 하거나 중지 하는 **시작** 및 **중지** 메서드를 제공 합니다. 오디오 [**그래프. Stop**](/uwp/api/windows.media.audio.audiograph.stop) 이 호출 되 면 개별 노드의 상태에 관계 없이 모든 노드의 오디오 처리가 모두 중지 되지만 오디오 그래프가 중지 된 동안에는 각 노드의 상태를 설정할 수 있습니다. 예를 들어 그래프가 중지 된 동안 개별 노드에서 **중지** 를 호출한 다음, 시작을 호출 하면 개별 노드가 중지 된 상태로 유지 됩니다 **.**

모든 노드 형식은 **ConsumeInput** 속성을 노출 합니다 .이 속성을 false로 설정 하면 노드가 오디오 처리를 계속할 수 있지만 다른 노드에서 입력 하는 오디오 데이터를 사용 하는 것을 중지 합니다.

모든 노드 형식은 노드가 현재 버퍼에 있는 오디오 데이터를 삭제 하 게 하는 **Reset** 메서드를 노출 합니다.

## <a name="adding-audio-effects"></a>오디오 효과 추가

Audio graph API를 사용 하면 그래프의 모든 노드 형식에 오디오 효과를 추가할 수 있습니다. 출력 노드, 입력 노드 및 서브 믹스 노드는 각각 하드웨어의 기능에 의해서만 제한 되는 오디오 효과를 무제한으로 포함할 수 있습니다. 다음 예에서는 기본 제공 에코 효과를 서브 믹스 노드에 추가 하는 방법을 보여 줍니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddEffect":::

-   모든 오디오 효과는 [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition)을 구현 합니다. 모든 노드는 해당 노드에 적용 되는 효과 목록을 나타내는 **EffectDefinitions** 속성을 노출 합니다. 해당 정의 개체를 목록에 추가 하 여 효과를 추가 합니다.
-   **Windows. Media. Audio** 네임 스페이스에는 몇 가지 효과 정의 클래스가 제공 됩니다. 여기에는 다음이 포함됩니다.
    -   [**EchoEffectDefinition**](/uwp/api/Windows.Media.Audio.EchoEffectDefinition)
    -   [**EqualizerEffectDefinition**](/uwp/api/Windows.Media.Audio.EqualizerEffectDefinition)
    -   [**LimiterEffectDefinition**](/uwp/api/Windows.Media.Audio.LimiterEffectDefinition)
    -   [**ReverbEffectDefinition**](/uwp/api/Windows.Media.Audio.ReverbEffectDefinition)
-   [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) 을 구현 하 고 오디오 그래프의 모든 노드에 적용 하는 고유한 오디오 효과를 만들 수 있습니다.
-   모든 노드 형식은 지정 된 정의를 사용 하 여 추가 된 노드의 **EffectDefinitions** 목록에서 모든 효과를 사용 하지 않도록 설정 하는 **DisableEffectsByDefinition** 메서드를 노출 합니다. **EnableEffectsByDefinition** 는 지정 된 정의를 사용 하 여 효과를 설정 합니다.

## <a name="spatial-audio"></a>공간 오디오
Windows 10, 버전 1607부터 오디오 **그래프** 는 입력 또는 서브 믹스 노드의 오디오를 내보내는 3d 공간에서 위치를 지정 하는 데 사용할 수 있는 공간 오디오를 지원 합니다. 또한 오디오를 내보내는 모양과 방향을 지정 하 고, 노드의 오디오를 Doppler으로 전환 하는 데 사용 되는 속도를 지정 하 고, 오디오가 거리를 감쇠 되거나 약화 하는 방법을 설명 하는 감소 모델을 정의할 수 있습니다. 

송신기를 만들기 위해 먼저 송신기에서 사운드가 투영 되는 셰이프를 만들 수 있습니다 .이 셰이프는 원뿔 또는 전방향 수 있습니다. [**AudioNodeEmitterShape**](/uwp/api/Windows.Media.Audio.AudioNodeEmitterShape) 클래스는 이러한 각 셰이프를 만드는 정적 메서드를 제공 합니다. 다음으로, 감소 모델을 만듭니다. 이는 수신기 로부터의 거리가 늘어나면 송신기의 오디오 볼륨이 감소 하는 방식을 정의 합니다. [**CreateNatural**](/uwp/api/windows.media.audio.audionodeemitterdecaymodel.createnatural) 메서드는 거리 제곱 대칭 대칭 모델을 사용 하 여 자연 스러운 소리 감소를 에뮬레이트하는 감소 모델을 만듭니다. 마지막으로 [**AudioNodeEmitterSettings**](/uwp/api/Windows.Media.Audio.AudioNodeEmitterSettings) 개체를 만듭니다. 현재이 개체는 송신기 오디오의 속도 기반 Doppler 감쇠를 사용 하거나 사용 하지 않도록 설정 하는 데만 사용 됩니다. 방금 만든 초기화 개체를 전달 하 여 [**AudioNodeEmitter**](/uwp/api/windows.media.audio.audionodeemitter.-ctor) 생성자를 호출 합니다. 기본적으로 송신기는 원본에 배치 되지만 [**위치**](/uwp/api/windows.media.audio.audionodeemitter.position) 속성을 사용 하 여 송신기의 위치를 설정할 수 있습니다.

> [!NOTE]
> 오디오 노드 생성기는 mono로 포맷 된 오디오만 샘플 속도로 48kHz로 처리할 수 있습니다. 다른 샘플링 주기를 사용 하 여 스테레오 오디오 또는 오디오를 사용 하려고 하면 예외가 발생 합니다.

원하는 노드 형식에 대해 오버 로드 된 생성 메서드를 사용 하 여 만들 때 오디오 노드에 송신기를 할당 합니다. 이 예제에서는 [**Createfileinputnodeasync**](/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) 를 사용 하 여 지정 된 파일 및 해당 노드에 연결할 [**AudioNodeEmitter**](/uwp/api/Windows.Media.Audio.AudioNodeEmitter) 개체에서 파일 입력 노드를 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateEmitter":::

사용자에 게 그래프에서 오디오를 출력 하는 [**Audiodeviceoutputnode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) 에는 사용자의 3d 공간에서 사용자의 위치, 방향 및 속도를 나타내는 [**수신기**](/uwp/api/windows.media.audio.audiodeviceoutputnode.listener) 속성을 사용 하 여 액세스 하는 수신기 개체가 있습니다. 그래프의 모든 송신기 위치는 내보내기 개체의 위치와 방향을 기준으로 합니다. 기본적으로 수신기는 Z 축을 따라 앞으로 향하는 원점 (0, 0, 0)에 위치 하지만 [**위치**](/uwp/api/windows.media.audio.audionodelistener.position) 및 [**방향**](/uwp/api/windows.media.audio.audionodelistener.orientation) 속성을 사용 하 여 위치와 방향을 설정할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetListener":::

런타임에 송신기의 위치, 속도 및 방향을 업데이트 하 여 3D 공간을 통해 오디오 소스의 움직임을 시뮬레이션할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUpdateEmitter":::

런타임에 수신기 개체의 위치, 속도 및 방향을 업데이트 하 여 사용자가 3D 공간을 통해 이동 하는 것을 시뮬레이션할 수도 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUpdateListener":::

기본적으로 공간 오디오는 해당 셰이프, 속도 및 수신기에 상대적인 위치를 기준으로 오디오를 경우 Microsoft의 HRTF (헤드 전송 함수) 알고리즘을 사용 하 여 계산 됩니다. [**SpatialAudioModel**](/uwp/api/windows.media.audio.audionodeemitter.spatialaudiomodel) 속성을 **FoldDown** 로 설정 하 여 정확 하지 않지만 더 작은 CPU 및 메모리 리소스를 필요로 하는 공간 오디오를 시뮬레이션 하는 간단한 스테레오 혼합 방법을 사용할 수 있습니다.

## <a name="see-also"></a>추가 정보
- [미디어 재생](media-playback.md)
 

 
