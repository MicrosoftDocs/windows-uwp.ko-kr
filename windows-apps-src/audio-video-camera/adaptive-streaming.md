---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠의 재생을 지원합니다."
title: "적응 스트리밍"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3afd0440d8e552ebc3459c5fe30dd766db3ae8b9
ms.lasthandoff: 02/07/2017

---

# <a name="adaptive-streaming"></a>적응 스트리밍

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠의 재생을 지원합니다.

지원되는 HLS 프로토콜 태그 목록은 [HLS 태그 지원](hls-tag-support.md)을 참조하세요. 

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

사용자 지정 HTTP 헤더를 제공하거나, 현재 다운로드 및 재생 비트 전송률을 모니터링하거나, 시스템이 적응 스트리밍의 비트 전송률을 전환할 때 확인할 비율을 조정하는 등의 추가적인 고급 적응 스트리밍 기능이 앱에 필요하면 [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 개체를 사용합니다.

적응 스트리밍 API는 [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279) 네임스페이스에 있습니다.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

[**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261)를 호출하여 적응 스트리밍 매니페스트 파일의 URI를 통해 **AdaptiveMediaSource**를 초기화합니다. 이 메서드에서 반환된 [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) 값을 통해 미디어 원본이 성공적으로 생성되었는지 알 수 있습니다. 이 경우 [**SetMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn652653)를 호출하여 개체를 **MediaPlayer**에 대한 스트림 소스로 설정할 수 있습니다. 이 예제에서는 [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) 속성을 쿼리하여 이 스트림에 지원되는 최대 비트 전송률을 확인하고 해당 값을 초기 비트 전송률로 설정합니다. 이 예제에서는 이 문서에서 나중에 설명하는 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) 및 [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) 이벤트에 대한 처리기도 등록합니다.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

매니페스트 파일을 가져오기 위해 사용자 지정 HTTP 헤더를 설정해야 하면 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 개체를 만들고, 원하는 헤더를 설정하고, 개체를 **CreateFromUriAsync**의 오버로드에 전달합니다.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

시스템이 서버에서 리소스를 검색하려고 하면 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) 이벤트가 발생합니다. 이벤트 처리기에 전달된 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935)는 리소스 형식 및 URI 같은 요청되는 리소스에 대한 정보를 제공하는 속성을 표시합니다.

**DownloadRequested** 이벤트 처리기를 사용하여 이벤트 인수에서 제공된 [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) 개체의 속성을 업데이트하는 방식으로 리소스 요청을 수정할 수 있습니다. 아래 예제에서 리소스가 검색되는 원본 URI는 결과 개체의 [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) 속성을 업데이트해서 수정합니다.

결과 개체의 [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) 또는 [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) 속성을 설정하여 요청된 리소스의 콘텐츠를 재정의할 수 있습니다. 아래 예제에서 매니페스트 리소스의 콘텐츠는 **Buffer** 속성을 설정해서 바꿉니다. 원격 서버의 데이터 검색 또는 비동기 사용자 인증과 같이 비동기적으로 가져온 데이터를 사용하여 리소스 요청을 업데이트할 경우 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936)을 호출하여 지연을 가져오고, 작업이 완료되면 [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934)를 호출하여 다운로드 요청 작업이 계속될 수 있음을 시스템에 알려야 합니다.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

**AdaptiveMediaSource** 개체는 다운로드나 재생 비트 전송률이 변경될 때 대응할 수 있는 이벤트를 제공합니다. 이 예제에서 현재 비트 전송률은 간단히 UI에서 업데이트됩니다. 시스템이 적응형 스트림의 비트 전송률을 전환할 때 확인할 비율을 수정할 수 있습니다. 자세한 내용은 [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697) 속성을 참조하세요.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [HLS 태그 지원](hls-tag-support.md) 
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [백그라운드에서 미디어 재생](background-audio.md) 






