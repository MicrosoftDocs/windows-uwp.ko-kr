---
author: drewbatgit
ms.assetid: 25553c4d-fa4f-4130-af9b-97f993fefd43
description: 이 섹션에서는 오디오 및 비디오를 재생하는 유니버설 Windows 앱을 만드는 방법에 대한 정보를 제공합니다.
title: 미디어 재생
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dbcd2a4f9cec02882c62c7d6493746931b7919a8
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5986613"
---
# <a name="media-playback"></a>미디어 재생


이 섹션에서는 오디오 및 비디오를 재생하는 유니버설 Windows 앱을 만드는 방법에 대한 정보를 제공합니다. 

## <a name="media-playback-developer-features"></a>미디어 재생 개발자 기능

다음 표에서는 앱에 미디어 재생 기능을 추가하는 것에 대한 자세한 지침을 제공하는 방법 문서를 보여 줍니다.
 
| 항목                                                                                             | 설명                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md) | 이 문서는 UWP 앱의 미디어 재생 시스템에 대한 새 기능과 개선 기능을 활용하는 방법을 보여 줍니다. Windows 10 버전 1607부터는 미디어 재생을 위해 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaElement) 대신 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 클래스를 사용하는 것이 좋습니다. XAML 페이지에서 미디어 콘텐츠를 렌더링할 수 있도록 간단한 XAML 컨트롤인 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)가 도입되었습니다. **MediaPlayer**는 시스템 미디어 전송 컨트롤을 사용한 자동 통합 및 백그라운드 오디오를 위한 한 프로세스 모델 등 여러 가지 이점을 제공합니다. 또한 이 문서에서는 Windows.UI.Composition 표면에 비디오를 렌더링하는 방법과 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController)를 사용하여 여러 미디어 플레이어를 동기화하는 방법을 보여 줍니다.                                                                                                          |
| [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)                         | 이 문서에서는 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 클래스 사용 방법을 보여 줍니다. 이 클래스는 로컬 또는 원격 파일과 같은 여러 원본에서 미디어를 참조하고 재생하는 일반적인 방법을 제공하며 기본 미디어 형식에 상관없이 미디어 데이터에 액세스하기 위한 공통 모델을 공개합니다. [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 클래스는 **MediaSource**의 기능을 확장하여 미디어 항목에 포함된 여러 오디오, 비디오 및 메타데이터 트랙에서 관리하고 선택할 수 있습니다. [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)를 사용하면 하나 이상의 미디어 재생 항목에서 재생 목록을 만들 수 있습니다.                                                                                                               |
| [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)                               | 이 문서에서는 앱과 SMTC(시스템 미디어 전송 컨트롤)를 사용하여 앱을 통합하는 방법을 보여 줍니다. Windows 10 버전 1607부터 미디어를 재생하기 위해 만든 모든 **MediaPlayer** 인스턴스가 SMTC에 의해 자동으로 재생됩니다. 이 문서는 재생 중인 콘텐츠에 대한 메타데이터를 SMTC에 제공하고 SMTC 컨트롤의 기본 동작을 늘리거나 완전히 재정의하는 방법을 보여 줍니다.                                   |
| [시스템 지원 시간이 제한된 메타데이터 신호](system-supported-metadata-cues.md)                               | 이 문서는 미디어 파일 또는 스트림에 포함되었을 수 있는 시간이 제한된 메타데이터의 여러 형식을 활용하는 방법에 대해 설명합니다.                                   |
| [미디어 중단 만들기, 예약 및 관리](create-schedule-and-manage-media-breaks.md)                                                                             | 이 문서에서는 미디어 재생 앱에서 미디어 중단을 만들고, 예약하고, 관리하는 방법을 보여 줍니다. Windows 10 버전 1607부터 [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) 클래스를 사용하여 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)에서 재생하는 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)에 미디어 중단을 쉽고 빠르게 추가할 수 있습니다. 미디어 중단은 일반적으로 미디어 콘텐츠에 오디오 또는 비디오 광고를 추가하는 데 사용됩니다. 하나 이상의 미디어 중단을 예약한 후에는 재생 중 시스템에서 자동으로 지정된 시간에 미디어 콘텐츠를 재생합니다. **MediaBreakManager**는 미디어 중단이 시작되거나 종료될 때 또는 사용자가 건너뛸 때 앱에서 반응할 수 있도록 이벤트를 제공합니다. 미디어 중단을 위해 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession)에 액세스하여 다운로드 및 버퍼링 진행 업데이트 등의 이벤트를 모니터링할 수도 있습니다.                                                                                                                     |
| [백그라운드에서 미디어 재생](background-audio.md)                                                                             | 이 문서에서는 앱이 포그라운드에서 백그라운드로 이동될 때 미디어가 계속 재생되도록 앱을 구성하는 방법을 보여 줍니다. 즉, 사용자가 홈 화면에서 반환된 앱을 최소화했거나 다른 방법으로 앱에서 외부로 이동한 후에도 앱은 오디오를 계속 재생할 수 있습니다. Windows 10 버전 1607에서는 백그라운드 미디어 재생을 위한 새로운 단일 프로세스 모델이 도입되어 레거시 두 프로세스 모델보다 훨씬 빠르고 쉽게 구현할 수 있게 되었습니다. 이 문서에는 새 응용 프로그램 수명 주기 이벤트 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 및 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)를 처리하여 백그라운드 실행 중 앱의 메모리 사용을 관리하는 데 대한 정보가 포함되어 있습니다.                                                                                                                    |
| [적응 스트리밍](adaptive-streaming.md)                                                       | 이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠를 지원합니다.                                          |
| [미디어 캐스팅](media-casting.md)                                                                 | 이 문서에서는 유니버설 Windows 앱에서 원격 디바이스로 미디어를 캐스팅하는 방법을 보여 줍니다.                                                                                                                                                                                                       |
| [PlayReady DRM](playready-client-sdk.md)                                                          | 이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에 PlayReady 보호된 미디어 콘텐츠를 추가하는 방법을 설명합니다.                                                                                                                                                                                |
| [PlayReady 암호화된 미디어 확장](playready-encrypted-media-extension.md)                     | 이 섹션에서는 Windows 8.1 버전과 달리 Windows 10 버전에 작성된 변경 사항을 지원하도록 PlayReady 웹앱을 수정하는 방법을 설명합니다.                                                                                                                                       |

## <a name="media-playback-sdk-samples"></a>미디어 재생 SDK 샘플

다음 SDK 샘플은 Windows 10의 UWP 앱에 제공되는 미디어 재생 기능을 보여 줍니다. 이러한 샘플을 사용하여 컨텍스트에서 사용되거나 자체 앱의 시작점으로 사용되는 미디어 재생 API를 볼 수 있습니다.

* [적응 스트리밍 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)
* [백그라운드 오디오 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)
* [시스템 미디어 전송 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)                                                                                               
 




