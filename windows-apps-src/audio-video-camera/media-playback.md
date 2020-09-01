---
ms.assetid: 25553c4d-fa4f-4130-af9b-97f993fefd43
description: 이 섹션에서는 오디오 및 비디오를 재생하는 유니버설 Windows 앱을 만드는 방법에 대한 정보를 제공합니다.
title: 미디어 재생
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9aa5f3b355186810d4b337237245203592f3bc42
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163777"
---
# <a name="media-playback"></a>미디어 재생


이 섹션에서는 오디오 및 비디오를 재생하는 유니버설 Windows 앱을 만드는 방법에 대한 정보를 제공합니다. 

## <a name="media-playback-developer-features"></a>미디어 재생 개발자 기능

다음 표에서는 앱에 미디어 재생 기능을 추가 하기 위한 자세한 지침을 제공 하는 방법 문서를 나열 합니다.
 
| 항목                                                                                             | 설명                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md) | 이 문서에서는 UWP 앱 용 미디어 재생 시스템의 새로운 기능 및 향상 된 기능을 활용 하는 방법을 보여 줍니다. Windows 10 버전 1607부터 미디어 재생에 대 한 권장 모범 사례는 미디어 재생을 위해 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 대신 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스를 사용 하는 것입니다. 경량 XAML 컨트롤인 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)는 xaml 페이지에서 미디어 콘텐츠를 렌더링할 수 있도록 하기 위해 도입 되었습니다. **MediaPlayer** 는 시스템 미디어 전송 컨트롤과의 자동 통합 및 백그라운드 오디오에 대 한 간단한 프로세스 모델을 비롯 한 몇 가지 이점을 제공 합니다. 또한이 문서에서는 비디오를 MediaTimelineController 화면에 렌더링 하는 방법과 여러 미디어 플레이어를 동기화 하는 데 [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) 를 사용 하는 방법을 보여 줍니다.                                                                                                          |
| [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)                         | 이 문서에서는 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 클래스를 사용 하는 방법을 보여 줍니다 .이 클래스는 로컬 또는 원격 파일과 같은 여러 원본의 미디어를 참조 하 고 재생 하는 일반적인 방법을 제공 하 고, 기본 미디어 형식에 관계 없이 미디어 데이터에 액세스 하기 위한 공통 모델을 노출 합니다. [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 클래스는 **MediaSource**의 기능을 확장 하 여 미디어 항목에 포함 된 여러 오디오, 비디오 및 메타 데이터 트랙에서 관리 하 고 선택할 수 있습니다. [**Mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 를 사용 하면 하나 이상의 미디어 재생 항목에서 재생 목록을 만들 수 있습니다.                                                                                                               |
| [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)                               | 이 문서에서는 SMTC (시스템 미디어 전송 제어)와 앱을 통합 하는 방법을 보여 줍니다. Windows 10 버전 1607부터 미디어를 재생 하기 위해 만든 **MediaPlayer** 의 모든 인스턴스가 smtc에 의해 자동으로 표시 됩니다. 이 문서에서는 SMTC에 재생 중인 콘텐츠에 대 한 메타 데이터를 제공 하는 방법과 SMTC 컨트롤의 기본 동작을 보강 하거나 완전히 재정의 하는 방법을 보여 줍니다.                                   |
| [시스템에서 지원하는 시간이 제한된 메타데이터 신호](system-supported-metadata-cues.md)                               | 이 문서에서는 미디어 파일 또는 스트림에 포함 될 수 있는 몇 가지 형식의 시간 메타 데이터를 활용 하는 방법을 설명 합니다.                                   |
| [미디어 휴지 만들기, 예약 및 관리](create-schedule-and-manage-media-breaks.md)                                                                             | 이 문서에서는 미디어 재생 앱에 대 한 미디어 나누기를 만들고 예약 하 고 관리 하는 방법을 보여 줍니다. Windows 10, 버전 1607부터 [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) 클래스를 사용 하 여 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer)로 재생 하는 모든 [**mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 에 미디어 나누기를 빠르고 쉽게 추가할 수 있습니다. 미디어 나누기는 일반적으로 미디어 콘텐츠에 오디오 또는 비디오 광고를 삽입 하는 데 사용 됩니다. 하나 이상의 미디어 분리를 예약 하면 시스템은 재생 중 지정 된 시간에 미디어 콘텐츠를 자동으로 재생 합니다. **MediaBreakManager** 는 응용 프로그램이 미디어 중단의 시작, 종료 또는 사용자에 의해 생략 될 때 반응할 수 있도록 이벤트를 제공 합니다. 미디어 나누기에 대 한 [**Mediaplaybacksession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 에 액세스 하 여 다운로드 및 버퍼링 진행률 업데이트와 같은 이벤트를 모니터링할 수도 있습니다.                                                                                                                     |
| [백그라운드에서 미디어 재생](background-audio.md)                                                                             | 이 문서에서는 앱이 포그라운드에서 백그라운드에서 이동할 때 미디어가 계속 재생 되도록 앱을 구성 하는 방법을 보여 줍니다. 즉, 사용자가 앱을 최소화 하거나, 홈 화면으로 반환 하거나, 다른 방식으로 앱에서 벗어난 경우에도 앱에서 오디오를 계속 재생할 수 있습니다. Windows 10 버전 1607을 사용 하는 경우 레거시 2 프로세스 모델 보다 훨씬 빠르고 쉽게 구현할 수 있는 백그라운드 미디어 재생용 새 단일 프로세스 모델이 도입 되었습니다. 이 문서에는 백그라운드에서 실행 하는 동안 앱의 메모리 사용을 [**관리 하는**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 새 응용 프로그램 수명 [**주기 이벤트를 처리 하는**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 방법에 대 한 정보가 포함 되어 있습니다.                                                                                                                    |
| [적응 스트리밍](adaptive-streaming.md)                                                       | 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에 적응 스트리밍 멀티미디어 콘텐츠 재생을 추가 하는 방법을 설명 합니다. 이 기능은 현재 HLS (Http 라이브 스트리밍) 및 HTTP (대시) 콘텐츠를 통한 동적 스트리밍을 재생할 수 있도록 지원 합니다.                                          |
| [미디어 캐스팅](media-casting.md)                                                                 | 이 문서에서는 유니버설 Windows 앱에서 원격 장치로 미디어를 캐스팅 하는 방법을 보여 줍니다.                                                                                                                                                                                                       |
| [원격 Bluetooth 연결 디바이스에서 오디오 재생 사용](enable-remote-audio-playback.md)                                                                 | 이 문서에서는 audio [Playbackconnection](/uwp/api/windows.media.audio.audioplaybackconnection) 을 사용 하 여 bluetooth로 연결 된 원격 장치가 로컬 컴퓨터에서 오디오를 재생할 수 있도록 하는 방법을 보여 줍니다 .이 경우 bluetooth 스피커 처럼 작동 하도록 PC를 구성 하 고 사용자가 휴대폰에서 오디오를 소리로 전환할 수 있습니다.                                                                                                                                                                                                       |
| [PlayReady DRM](playready-client-sdk.md)                                                          | 이 항목에서는 유니버설 Windows 플랫폼 (UWP) 앱에 PlayReady로 보호 된 미디어 콘텐츠를 추가 하는 방법에 대해 설명 합니다.                                                                                                                                                                                |
| [PlayReady 암호화된 미디어 확장](playready-encrypted-media-extension.md)                     | 이 섹션에서는 이전 Windows 8.1 버전에서 Windows 10 버전으로 변경 된 내용을 지원 하도록 PlayReady 웹 앱을 수정 하는 방법을 설명 합니다.                                                                                                                                       |





## <a name="media-playback-sdk-samples"></a>미디어 재생 SDK 샘플

다음 SDK 샘플은 Windows 10의 UWP 앱에서 사용할 수 있는 미디어 재생 기능을 보여 줍니다. 이 샘플을 사용 하 여 컨텍스트에서 사용 되는 미디어 재생 Api를 확인 하거나 사용자 고유의 앱에 대 한 시작 점으로 사용 합니다.

* [적응 스트리밍 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)
* [배경 오디오 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)
* [시스템 미디어 전송 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)                                                                                               
 