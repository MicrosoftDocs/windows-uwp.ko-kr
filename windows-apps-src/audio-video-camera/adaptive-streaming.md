---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: 이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠의 재생을 지원합니다.
title: 적응 스트리밍
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef8e3ab4abd9ee9159dc7d5aa757f55e00817a51
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7423780"
---
# <a name="adaptive-streaming"></a>적응 스트리밍


이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠의 재생을 지원합니다. Windows 10 버전 1803부터 부드러운 스트리밍이 **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 에 의해 지원됩니다.

지원되는 HLS 프로토콜 태그 목록은 [HLS 태그 지원](hls-tag-support.md)을 참조하세요. 

지원되는 DASH 프로필의 목록을 보려면 [DASH 프로필 지원](dash-profile-support.md)을 참조하세요. 

> [!NOTE] 
> 이 문서의 코드는 UWP [적응 스트리밍 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)에서 조정되었습니다.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>MediaPlayer 및 MediaPlayerElement의 간단한 적응 스트리밍

UWP 앱에서 적응 스트리밍 미디어를 재생하려면 DASH 또는 HLS 매니페스트 파일을 가리키는 **Uri** 개체를 만듭니다. [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 클래스의 인스턴스를 만듭니다. [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)를 호출하여 새 **MediaSource** 개체를 만들고 **MediaPlayer**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 속성으로 설정합니다. [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play)를 호출하여 미디어 콘텐츠 재생을 시작합니다.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

위의 예제는 미디어 콘텐츠의 오디오를 재생하지만 UI에서 콘텐츠를 자동으로 렌더링하지는 않습니다. 비디오 콘텐츠를 재생하는 대부분의 앱은 XAML 페이지의 콘텐츠를 렌더링하려고 합니다.  이렇게 하려면 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤을 XAML 페이지에 추가합니다.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

[**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)를 호출하여 DASH 또는 HLS 매니페스트 파일의 URI에서 **MediaSource**를 만듭니다. 그런 다음 **MediaPlayerElement**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) 속성을 설정합니다. **MediaPlayerElement**에서 자동으로 콘텐츠의 새 **MediaPlayer** 개체를 만듭니다. **MediaPlayer**에서 **Play**를 호출하여 콘텐츠 재생을 시작할 수 있습니다.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> Windows 10 버전 1607부터는 **MediaPlayer** 클래스를 사용하여 미디어 항목을 재생하는 것이 좋습니다. **MediaPlayerElement**는 XAML 페이지의 **MediaPlayer** 콘텐츠를 렌더링하는 데 사용되는 간단한 XAML 컨트롤입니다. **MediaElement** 컨트롤은 이전 버전과의 호환성을 위해 계속 지원됩니다. **MediaPlayer** 및 **MediaPlayerElement**를 사용하여 미디어 콘텐츠를 재생하는 방법은 [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조하세요. **MediaSource** 및 관련 API를 미디어 콘텐츠와 함께 사용하는 방법은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조하세요.

## <a name="adaptive-streaming-with-adaptivemediasource"></a>AdaptiveMediaSource를 이용한 적응 스트리밍

사용자 지정 HTTP 헤더를 제공하거나, 현재 다운로드 및 재생 비트 전송률을 모니터링하거나, 시스템이 적응 스트리밍의 비트 전송률을 전환할 때 확인할 비율을 조정하는 등의 추가적인 고급 적응 스트리밍 기능이 앱에 필요하면 **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 개체를 사용합니다.

적응 스트리밍 API는 [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279) 네임스페이스에 있습니다. 이 문서의 예제는 다음 네임스페이스의 API를 사용합니다.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>URI에서 AdaptiveMediaSource를 초기화합니다.

[**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261)를 호출하여 적응 스트리밍 매니페스트 파일의 URI를 통해 **AdaptiveMediaSource**를 초기화합니다. 이 메서드에서 반환된 [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) 값을 통해 미디어 원본이 성공적으로 생성되었는지 알 수 있습니다. 이럴 경우, [**MediaSource.CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource)를 호출하고 이를 미디어 플레이어의 [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source) 속성에 할당하여 **MediaSource** 개체를 만드는 방식으로 개체를 **MediaPlayer**의 스트림 원본으로 설정할 수 있습니다. 이 예제에서는 [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) 속성을 쿼리하여 이 스트림에 지원되는 최대 비트 전송률을 확인하고 해당 값을 초기 비트 전송률로 설정합니다. 이 예제는 이 문서에서 나중에 설명할 여러 **AdaptiveMediaSource** 이벤트에 대한 처리기 또한 등록합니다.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>HttpClient를 사용하는 AdaptiveMediaSource 초기화

매니페스트 파일을 가져오기 위해 사용자 지정 HTTP 헤더를 설정해야 하면 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 개체를 만들고, 원하는 헤더를 설정하고, 개체를 **CreateFromUriAsync**의 오버로드에 전달합니다.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

시스템이 서버에서 리소스를 검색하려고 하면 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) 이벤트가 발생합니다. 이벤트 처리기에 전달된 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935)는 리소스 형식 및 URI 같은 요청되는 리소스에 대한 정보를 제공하는 속성을 표시합니다.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>DownloadRequested 이벤트를 사용하여 리소스 요청 속성 수정

**DownloadRequested** 이벤트 처리기를 사용하여 이벤트 인수에서 제공된 [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) 개체의 속성을 업데이트하는 방식으로 리소스 요청을 수정할 수 있습니다. 아래 예제에서 리소스가 검색되는 원본 URI는 결과 개체의 [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) 속성을 업데이트해서 수정합니다. 또한 바이트 범위 오프셋 및 미디어 세그먼트의 길이를 다시 작성하거나 아래의 예제에서와 같이 리소스 URI를 변경하여 전체 리소스를 다운로드하고 바이트 범위 오프셋 및 길이를 null로 설정할 수 있습니다.

결과 개체의 [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) 또는 [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) 속성을 설정하여 요청된 리소스의 콘텐츠를 재정의할 수 있습니다. 아래 예제에서 매니페스트 리소스의 콘텐츠는 **Buffer** 속성을 설정해서 바꿉니다. 원격 서버의 데이터 검색 또는 비동기 사용자 인증과 같이 비동기적으로 가져온 데이터를 사용하여 리소스 요청을 업데이트할 경우 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936)을 호출하여 지연을 가져오고, 작업이 완료되면 [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934)를 호출하여 다운로드 요청 작업이 계속될 수 있음을 시스템에 알려야 합니다.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>비트 전송률 이벤트를 사용하여 비트 전송률 변경 관리 및 응답

**AdaptiveMediaSource** 개체는 다운로드나 재생 비트 전송률이 변경될 때 대응할 수 있는 이벤트를 제공합니다. 이 예제에서 현재 비트 전송률은 간단히 UI에서 업데이트됩니다. 시스템이 적응형 스트림의 비트 전송률을 전환할 때 확인할 비율을 수정할 수 있습니다. 자세한 내용은 [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697) 속성을 참조하세요.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>다운로드 완료 및 실패 이벤트 처리
요청된 리소스의 다운로드가 실패할 때 **AdaptiveMediaSource** 개체는 [**DownloadFailed**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) 이벤트를 발생시킵니다. 이 이벤트를 사용하여 실패에 대한 응답으로 UI를 업데이트할 수 있습니다. 또한 이벤트를 사용하여 다운로드 작업 및 실패에 대한 통계 정보를 기록할 수 있습니다. 

이벤트 처리기에 전달된 [**AdaptiveMediaSourceDownloadFailedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) 개체는 응답 유형 및 원본 URI와 같은 실패한 리소스 다운로드 및 실패가 발생한 스트림 내의 위치에 대한 메타데이터를 포함합니다. [**RequestId**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId)는 여러 이벤트에서 개별 요청에 대한 상태 정보의 상관 관계를 지정하는 데 사용할 수 있는 요청에 대한 시스템에서 생성된 고유 식별자를 가져옵니다.

[**통계**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) 속성은 [**AdaptiveMediaSourceDownloadStatistics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) 개체를 반환합니다. 이벤트 시 수신된 바이트 수와 다운로드 작업 시 다양한 지표의 타이밍에 대한 자세한 정보를 제공합니다. 적응 스트리밍 구현에서 성능 문제를 식별하기 위해 이 정보를 기록할 수 있습니다.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


리소스 다운로드가 완료되고 **DownloadFailed** 이벤트에 유사한 데이터가 제공되면 [**DownloadCompleted**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) 이벤트가 발생합니다. 상관 단일 요청에 대해 이벤트의 상관 관계를 지정하기 위해 다시 한 번 **RequestId**가 제공됩니다. 또한 다운로드 통계 로깅을 활성화하기 위해 **AdaptiveMediaSourceDownloadStatistics** 개체가 제공됩니다.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>AdaptiveMediaSourceDiagnostics로 적응 스트리밍 원격 분석 데이터 수집
**AdaptiveMediaSource**는 [**AdaptiveMediaSourceDiagnostics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics) 개체를 반환하는 [**진단**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource?branch=master.Diagnostics) 속성을 노출합니다. 이 개체를 사용하여 [**DiagnosticAvailable**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) 이벤트를 등록합니다. 이 이벤트는 원격 분석 컬렉션에 대해 사용하도록 하고 실행 시 앱 동작을 수정하는 데 사용하지 않아야 합니다. 이 진단 이벤트는 다양한 이유로 발생합니다. 이벤트가 발생한 이유를 확인하기 위해 이벤트에 전달된 [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) 개체의 [**DiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) 속성을 확인합니다. 가능한 이유에는 요청된 리소스에 액세스하는 오류 및 매니페스트 파일 스트리밍 구문 분석 오류가 포함됩니다. 진단 이벤트를 시작할 수 있는 경우에 대한 목록은 [**AdaptiveMediaSourceDiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype)을 참고하세요. 다른 적응 스트리밍 이벤트에 대한 인수와 같이 **AdaptiveMediaSourceDiagnosticAvailableEventArgs**는 다른 이벤트 간 요청 정보의 상관 관계를 지정하는 **RequestId** 속성을 제공합니다.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>MediaBinder를 사용하여 재생 목록의 항목에 대한 적응 스트리밍 콘텐츠 바인딩 연기
[**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) 클래스로 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)의 미디어 바인딩을 연기할 수 있습니다. Windows 10 버전 1703부터 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource)를 바인딩된 콘텐츠로 제공할 수 있습니다. 적응 미디어 소스의 연기된 바인딩 프로세스는 대체로 다른 종류의 미디어 바인딩과 동일하며 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)에 설명되어 있습니다. 

**MediaBinder** 인스턴스를 만들고 앱 정의 [**토큰**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) 문자열을 설정하여 바인딩할 콘텐츠를 식별하고 [**바인딩**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding) 이벤트를 등록할 수 있습니다. [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder)를 호출하여 **바인더**에서 **MediaSource**를 만듭니다. 그런 다음 **MediaSource**에서 **MediaPlaybackItem**을 만들고 재생 목록에 추가합니다.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

**바인딩** 이벤트 처리기에서 토큰 문자열을 사용하여 바인딩할 콘텐츠를 식별한 다음 **[CreateFromStreamAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** 또는 **[CreateFromUriAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** 의 오버로드 중 하나를 호출하여 적응 미디어 소스를 만듭니다. 이는 비동기 메서드이기 때문에 먼저 [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) 메서드를 호출하여 계속하기 전에 시스템이 작업을 완료할 때까지 기다리도록 해야 합니다.  **[SetAdaptiveMediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)** 를 호출하여 적응 미디어 소스를 바인딩된 콘텐츠로 설정합니다. 마지막으로 작업이 완료된 후 시스템에 계속하도록 지시하기 위해 [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete)를 호출합니다.

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

바인딩된 적응 미디어 원본에 대해 이벤트 처리기를 등록하려면 **MediaPlaybackList**의 [**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) 이벤트에 대한 처리기에서 이를 수행할 수 있습니다. [**CurrentMediaPlaybackItemChangedEventArgs.NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) 속성은 목록의 새로 현재 재생 중인 **MediaPlaybackItem**을 포함합니다. **MediaPlaybackItem**의 [**소스**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) 속성에 액세스하여 새 항목을 나타내는 **AdaptiveMediaSource**의 인스턴스를 가져온 다음 미디어 소스의 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) 속성을 가져옵니다. 새 재생 항목이 **AdaptiveMediaSource**가 아니면 이 속성은 null이므로 모든 개체의 이벤트에 대해 처리기를 등록하기 전에 테스트해야 합니다.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [HLS 태그 지원](hls-tag-support.md) 
* [DASH 프로필 지원](dash-profile-support.md) 
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [백그라운드에서 미디어 재생](background-audio.md) 





