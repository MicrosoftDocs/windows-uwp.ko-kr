---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠를 지원합니다."
title: "적응 스트리밍"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8ebf90b02fcfbb4349ba2b303d9c91727b731ad7

---

# 적응 스트리밍

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠를 지원합니다.

## MediaElement를 이용한 간단한 적응 스트리밍

XAML 기반 앱에서 적응 스트리밍 멀티미디어를 표시하려면 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 컨트롤을 페이지에 추가합니다.

[!code-xml[MediaElementXAML](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml#SnippetMediaElementXAML)]

**MediaElement**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) 속성을 DASH 또는 HLS 매니페스트 파일의 URI로 설정합니다.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetManifestSource)]

## AdaptiveMediaSource를 이용한 적응 스트리밍

사용자 지정 HTTP 헤더를 제공하거나, 현재 다운로드 및 재생 비트 전송률을 모니터링하거나, 시스템이 적응 스트리밍의 비트 전송률을 전환할 때 확인할 비율을 조정하는 등의 추가적인 고급 적응 스트리밍 기능이 앱에 필요하면 [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 개체를 사용합니다.

적응 스트리밍 API는 [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279) 네임스페이스에 있습니다.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

[
            **CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261)를 호출하여 적응 스트리밍 매니페스트 파일의 URI를 통해 **AdaptiveMediaSource**를 초기화합니다. 이 메서드에서 반환된 [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) 값을 통해 미디어 원본이 성공적으로 생성되었는지 알 수 있습니다. 이 경우 [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029)를 호출하여 개체를 **MediaElement**에 대한 스트림 소스로 설정할 수 있습니다. 이 예제에서는 [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) 속성을 쿼리하여 이 스트림에 지원되는 최대 비트 전송률을 확인하고 해당 값을 초기 비트 전송률로 설정합니다. 이 예제에서는 이 문서에서 나중에 설명하는 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) 및 [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) 이벤트에 대한 처리기도 등록합니다.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

매니페스트 파일을 가져오기 위해 사용자 지정 HTTP 헤더를 설정해야 하면 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 개체를 만들고, 원하는 헤더를 설정하고, 개체를 **CreateFromUriAsync**의 오버로드에 전달합니다.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

시스템이 서버에서 리소스를 검색하려고 하면 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) 이벤트가 발생합니다. 이벤트 처리기에 전달된 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935)는 리소스 형식 및 URI 같은 요청되는 리소스에 대한 정보를 제공하는 속성을 표시합니다.

**DownloadRequested** 이벤트 처리기를 사용하여 이벤트 인수에서 제공된 [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) 개체의 속성을 업데이트하는 방식으로 리소스 요청을 수정할 수 있습니다. 아래 예제에서 리소스가 검색되는 원본 URI는 결과 개체의 [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) 속성을 업데이트해서 수정합니다.

결과 개체의 [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) 또는 [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) 속성을 설정하여 요청된 리소스의 콘텐츠를 재정의할 수 있습니다. 아래 예제에서 매니페스트 리소스의 콘텐츠는 **Buffer** 속성을 설정해서 바꿉니다. 원격 서버의 데이터 검색 또는 비동기 사용자 인증과 같이 비동기적으로 가져온 데이터를 사용하여 리소스 요청을 업데이트할 경우 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936)을 호출하여 지연을 가져오고, 작업이 완료되면 [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934)를 호출하여 다운로드 요청 작업이 계속될 수 있음을 시스템에 알려야 합니다.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

**AdaptiveMediaSource** 개체는 다운로드나 재생 비트 전송률이 변경될 때 대응할 수 있는 이벤트를 제공합니다. 이 예제에서 현재 비트 전송률은 간단히 UI에서 업데이트됩니다. 시스템이 적응형 스트림의 비트 전송률을 전환할 때 확인할 비율을 수정할 수 있습니다. 자세한 내용은 [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697) 속성을 참조하세요.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

 

 







<!--HONumber=Jun16_HO4-->


