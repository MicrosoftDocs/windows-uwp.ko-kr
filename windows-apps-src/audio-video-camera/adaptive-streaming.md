---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에 적응 스트리밍 멀티미디어 콘텐츠 재생을 추가 하는 방법을 설명 합니다. 이 기능은 현재 HLS (Http 라이브 스트리밍) 및 HTTP (대시) 콘텐츠를 통한 동적 스트리밍을 재생할 수 있도록 지원 합니다.
title: 적응 스트리밍
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0ecfe0b8e48a0810614cad6fde91d9a429956bf9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161237"
---
# <a name="adaptive-streaming"></a>적응 스트리밍


이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에 적응 스트리밍 멀티미디어 콘텐츠 재생을 추가 하는 방법을 설명 합니다. 이 기능은 http (대시로) 콘텐츠를 통한 동적 스트리밍 및 HLS (Http 라이브 스트리밍) 재생을 지원 합니다. Windows 10, 버전 1803부터 부드러운 스트리밍는  **[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 에서 지원 됩니다.

지원 되는 HLS 프로토콜 태그 목록은 [hls 태그 지원](hls-tag-support.md)을 참조 하세요. 

지원 되는 대시 프로필 목록은 [대시 프로필 지원](dash-profile-support.md)을 참조 하세요. 

> [!NOTE] 
> 이 문서의 코드는 UWP [적응 스트리밍 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)에서 도입 되었습니다.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>MediaPlayer 및 MediaPlayerElement를 사용 하는 단순 적응 스트리밍

UWP 앱에서 적응 스트리밍 미디어를 재생 하려면 대시 또는 HLS 매니페스트 파일을 가리키는 **Uri** 개체를 만듭니다. [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스의 인스턴스를 만듭니다. [**MediaSource**](/uwp/api/windows.media.core.mediasource.createfromuri) 를 호출 하 여 새 **MediaSource** 개체를 만든 다음이를 **MediaPlayer**의 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 속성으로 설정 합니다. [**재생**](/uwp/api/windows.media.playback.mediaplayer.play) 을 호출 하 여 미디어 콘텐츠의 재생을 시작 합니다.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

위의 예제는 미디어 콘텐츠의 오디오를 재생 하지만 UI에서 콘텐츠를 자동으로 렌더링 하지 않습니다. 비디오 콘텐츠를 재생 하는 대부분의 앱은 XAML 페이지에서 콘텐츠를 렌더링 하려고 합니다.  이렇게 하려면 XAML 페이지에 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤을 추가 합니다.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

[**MediaSource**](/uwp/api/windows.media.core.mediasource.createfromuri) 를 호출 하 여 대시 또는 HLS 매니페스트 파일의 URI에서 **MediaSource** 를 만듭니다. 그런 다음 **MediaPlayerElement**의 [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty) 속성을 설정 합니다. **MediaPlayerElement** 는 콘텐츠에 대 한 새 **MediaPlayer** 개체를 자동으로 만듭니다. **MediaPlayer** 에서 **Play** 를 호출 하 여 콘텐츠의 재생을 시작할 수 있습니다.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> Windows 10 버전 1607부터 **MediaPlayer** 클래스를 사용 하 여 미디어 항목을 재생 하는 것이 좋습니다. **MediaPlayerElement** 는 xaml 페이지에서 **MediaPlayer** 의 콘텐츠를 렌더링 하는 데 사용 되는 간단한 XAML 컨트롤입니다. **MediaElement** 컨트롤은 이전 버전과의 호환성을 위해 계속 지원 됩니다. **Mediaplayer** 및 **MediaPlayerElement** 를 사용 하 여 미디어 콘텐츠를 재생 하는 방법에 대 한 자세한 내용은 [Mediaplayer로 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조 하세요. **MediaSource** 및 관련 api를 사용 하 여 미디어 콘텐츠로 작업 하는 방법에 대 한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조 하세요.

## <a name="adaptive-streaming-with-adaptivemediasource"></a>AdaptiveMediaSource를 사용 하는 적응 스트리밍

사용자 지정 HTTP 헤더를 제공 하거나, 현재 다운로드 및 재생 비트 속도를 모니터링 하거나, 시스템에서 적응 스트림의 비트 전송률을 전환 하는 경우를 결정 하는 비율을 조정 하는 등의 고급 적응 스트리밍 기능이 앱에 필요한 경우에는 **[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 개체를 사용 합니다.

적응 스트리밍 Api는 Windows. Media. [**adaptive**](/uwp/api/Windows.Media.Streaming.Adaptive) 네임 스페이스에 있습니다. 이 문서의 예제에서는 다음 네임 스페이스의 Api를 사용 합니다.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>URI에서 AdaptiveMediaSource를 초기화 합니다.

[**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)를 호출 하 여 적응 스트리밍 매니페스트 파일의 URI로 **AdaptiveMediaSource** 를 초기화 합니다. 이 메서드에서 반환 된 [**AdaptiveMediaSourceCreationStatus**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus) 값은 미디어 원본이 성공적으로 만들어졌는지 여부를 알려 줍니다. 이 경우 [**MediaSource. CreateFromAdaptiveMediaSource**](/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource)를 호출 하 여 **MediaSource** 개체를 만든 다음 미디어 플레이어의 [**원본**](/uwp/api/windows.media.playback.mediaplayer.Source) 속성에 할당 하 여 개체를 **MediaPlayer** 의 스트림 원본으로 설정할 수 있습니다. 이 예제에서는 사용할 수 있는 [**bit요율**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates) 속성을 쿼리하여이 스트림에 대해 지원 되는 최대 비트 전송률을 결정 한 다음 해당 값을 초기 비트 전송률로 설정 합니다. 또한이 예제에서는이 문서의 뒷부분에서 설명 하는 몇 가지 **AdaptiveMediaSource** 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>HttpClient를 사용 하 여 AdaptiveMediaSource 초기화

매니페스트 파일을 가져오기 위해 사용자 지정 HTTP 헤더를 설정 해야 하는 경우 [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) 개체를 만들고 원하는 헤더를 설정한 다음 개체를 **CreateFromUriAsync**의 오버 로드에 전달할 수 있습니다.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

시스템이 서버에서 리소스를 검색 하려고 할 때 [**Downloadrequested**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested) 된 이벤트가 발생 합니다. 이벤트 처리기로 전달 되는 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) 는 요청 되는 리소스에 대 한 정보 (예: 리소스의 형식 및 URI)를 제공 하는 속성을 노출 합니다.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>DownloadRequested 이벤트를 사용 하 여 리소스 요청 속성 수정

**Downloadrequested** 이벤트 처리기를 사용 하 여 이벤트 인수로 제공 된 [**AdaptiveMediaSourceDownloadResult**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult) 개체의 속성을 업데이트 하 여 리소스 요청을 수정할 수 있습니다. 아래 예제에서는 결과 개체의 [**ResourceUri**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri) 속성을 업데이트 하 여 리소스를 검색할 URI를 수정 합니다. 미디어 세그먼트에 대 한 바이트 범위 오프셋과 길이를 다시 작성 하거나 아래 예제와 같이 리소스 URI를 변경 하 여 전체 리소스를 다운로드 하 고 바이트 범위 오프셋과 길이를 null로 설정할 수도 있습니다.

결과 개체의 [**Buffer**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer) 또는 [**InputStream**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream) 속성을 설정 하 여 요청 된 리소스의 콘텐츠를 재정의할 수 있습니다. 아래 예제에서는 **Buffer** 속성을 설정 하 여 매니페스트 리소스의 콘텐츠를 대체 합니다. 원격 서버 또는 비동기 사용자 인증에서 데이터를 검색 하는 것과 같이 비동기적으로 가져온 데이터를 사용 하 여 리소스 요청을 업데이트 하는 경우 AdaptiveMediaSourceDownloadRequestedEventArgs를 호출 하 여 지연을 가져온 다음 작업이 완료 되 면 [**완료**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) 를 호출 하 여 다운로드 요청 작업을 계속할 수 있음을 시스템에 알립니다 [**.**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral)

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>비트 전송률 이벤트를 사용 하 여 비트 전송률 변경 내용 관리 및 대응

**AdaptiveMediaSource** 개체는 다운로드 또는 재생 비트 전송률이 변경 될 때 반응할 수 있는 이벤트를 제공 합니다. 이 예제에서는 현재 비트 전송률이 UI에서 단순히 업데이트 됩니다. 시스템이 적응 스트림의 비트 전송률을 전환 하는 시점을 결정 하는 비율을 수정할 수 있습니다. 자세한 내용은 [**AdvancedSettings**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings) 속성을 참조 하세요.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>다운로드 완료 및 실패 이벤트 처리
**AdaptiveMediaSource** 개체는 요청 된 리소스의 다운로드가 실패할 때 [**downloadfailed**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) 이벤트를 발생 시킵니다. 이 이벤트를 사용 하 여 오류에 대 한 응답으로 UI를 업데이트할 수 있습니다. 이벤트를 사용 하 여 다운로드 작업 및 오류에 대 한 통계 정보를 기록할 수도 있습니다. 

이벤트 처리기에 전달 된 [**AdaptiveMediaSourceDownloadFailedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) 개체에는 리소스 유형, 리소스의 URI, 오류가 발생 한 스트림 내의 위치와 같은 실패 한 리소스 다운로드에 대 한 메타 데이터가 포함 됩니다. [**RequestId**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) 는 여러 이벤트에서 개별 요청에 대 한 상태 정보를 상호 연결 하는 데 사용할 수 있는 시스템 생성 고유 식별자를 요청에 대해 가져옵니다.

[**Statistics**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) 속성은 이벤트 시 받은 바이트 수와 다운로드 작업의 여러 마일스 톤에 대 한 타이밍에 대 한 자세한 정보를 제공 하는 [**AdaptiveMediaSourceDownloadStatistics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) 개체를 반환 합니다. 적응 스트리밍 구현에서 성능 문제를 식별 하기 위해이 정보를 기록할 수 있습니다.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


[**Downloadcompleted**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) 이벤트는 리소스 다운로드가 완료 되 고 유사 데이터를 **downloadcompleted** 이벤트와 provdes 때 발생 합니다. 다시 한 번, 단일 요청에 대 한 이벤트의 상관 관계를 지정 하기 위해 **RequestId** 가 제공 됩니다. 또한 다운로드 통계의 로깅을 사용 하도록 설정 하기 위해 **AdaptiveMediaSourceDownloadStatistics** 개체가 제공 됩니다.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>AdaptiveMediaSourceDiagnostics를 사용 하 여 적응 스트리밍 원격 분석 데이터 수집
**AdaptiveMediaSource** 는 [**AdaptiveMediaSourceDiagnostics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics) 개체를 반환 하는 [**진단**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) 속성을 노출 합니다. 이 개체를 사용 하 여 [**DiagnosticAvailable**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) 이벤트에 등록 합니다. 이 이벤트는 원격 분석 수집에 사용 하기 위한 것 이며 런타임에 응용 프로그램 동작을 수정 하는 데 사용 하면 안 됩니다. 이 진단 이벤트는 다양 한 이유로 발생 합니다. 이벤트에 전달 된 [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) 개체의 [**DiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) 속성을 확인 하 여 이벤트가 발생 한 이유를 확인 합니다. 잠재적인 원인에는 요청 된 리소스에 액세스 하는 오류 및 스트리밍 매니페스트 파일을 구문 분석 하는 오류가 포함 됩니다. 진단 이벤트를 트리거할 수 있는 상황 목록은 [**AdaptiveMediaSourceDiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype)를 참조 하세요. 다른 적응 스트리밍 이벤트의 인수와 마찬가지로 **AdaptiveMediaSourceDiagnosticAvailableEventArgs** 는 다른 이벤트 간의 요청 정보를 상호 연결 하는 **RequestId** 속성이을 제공 합니다.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>MediaBinder를 사용 하 여 재생 목록의 항목에 대 한 적응 스트리밍 콘텐츠의 바인딩 지연
[**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder) 클래스를 사용 하 여 [**Mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList)의 미디어 콘텐츠 바인딩을 연기할 수 있습니다. Windows 10, 버전 1703부터 바인딩된 콘텐츠로 [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 를 제공할 수 있습니다. 적응 미디어 원본의 지연 된 바인딩 프로세스는 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)에 설명 된 다른 유형의 미디어 바인딩과 거의 동일 합니다. 

**MediaBinder** 인스턴스를 만들고 앱 정의 [**토큰**](/uwp/api/Windows.Media.Core.MediaBinder.Token) 문자열을 설정 하 여 바인딩할 콘텐츠를 식별 하 고 [**바인딩**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) 이벤트에 등록 합니다. [**MediaSource. CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder)를 호출 하 여 **바인더** 에서 **MediaSource** 를 만듭니다. 그런 다음 **MediaSource** 에서 **Mediaplaybackitem** 을 만들고 재생 목록에 추가 합니다.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

**바인딩** 이벤트 처리기에서 토큰 문자열을 사용 하 여 바인딩할 콘텐츠를 식별 한 다음 **[CreateFromStreamAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** 또는 **[CreateFromUriAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** 의 오버 로드 중 하나를 호출 하 여 적응 미디어 원본을 만듭니다. 이러한 메서드는 비동기 메서드 이므로 먼저 [**MediabindGetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) 메서드를 호출 하 여 시스템에서 작업이 완료 될 때까지 기다렸다가 계속 진행 해야 합니다.  **[SetAdaptiveMediaSource](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)** 를 호출 하 여 적응 미디어 원본을 바인딩된 콘텐츠로 설정 합니다. 마지막으로 작업이 완료 된 후 지연을 호출 하 여 시스템을 계속 하도록 지시 합니다 [**.**](/uwp/api/windows.foundation.deferral.Complete)

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

바인딩된 적응 미디어 소스에 대 한 이벤트 처리기를 등록 하려면 **Mediaplaybacklist**의 [**currentitemchanged**](/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) 이벤트에 대 한 처리기에서이 작업을 수행할 수 있습니다. [**CurrentMediaPlaybackItemChangedEventArgs NewItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) 속성은 목록에서 현재 재생 중인 **Mediaplaybackitem** 을 포함 합니다. **Mediaplaybackitem** 의 [**Source**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) 속성에 액세스 하 고 미디어 원본의 [**AdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) 속성에 액세스 하 여 새 항목을 나타내는 **AdaptiveMediaSource** 의 인스턴스를 가져옵니다. 새 재생 항목이 **AdaptiveMediaSource**가 아닌 경우이 속성은 null이 됩니다. 따라서 개체의 이벤트에 대 한 처리기를 등록 하려고 시도 하기 전에 null을 테스트 해야 합니다.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [HLS 태그 지원](hls-tag-support.md) 
* [대시 프로필 지원](dash-profile-support.md) 
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [백그라운드에서 미디어 재생](background-audio.md) 