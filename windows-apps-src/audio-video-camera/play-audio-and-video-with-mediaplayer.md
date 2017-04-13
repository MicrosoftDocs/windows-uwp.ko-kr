---
author: drewbatgit
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: "이 문서에서는 MediaPlayer를 사용하여 유니버설 Windows 앱에서 미디어를 재생하는 방법을 보여 줍니다."
title: "MediaPlayer를 사용하여 오디오 및 비디오 재생"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 79eabb1341d9a14ec924a1933917e92af797b65a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="play-audio-and-video-with-mediaplayer"></a>MediaPlayer를 사용하여 오디오 및 비디오 재생

이 문서에서는 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 클래스를 사용하여 유니버설 Windows 앱에서 미디어를 재생하는 방법을 보여 줍니다. Windows10 버전 1607에서는 백그라운드 오디오에 대해 간소화된 단일 프로세스 디자인, SMTC(시스템 미디어 전송 컨트롤)와 자동 통합, 여러 미디어 플레이어를 동기화하는 기능, Windows.UI.Composition 표면 기능 및 콘텐츠에서 미디어 중단을 만들고 예약하는 편리한 인터페이스를 포함하여 미디어 재생 API가 크게 개선되었습니다. 이러한 개선된 기능을 활용하려면 미디어 재생을 위해 **MediaElement** 대신 **MediaPlayer** 클래스를 사용하는 것이 좋습니다. XAML 페이지에서 미디어 콘텐츠를 렌더링할 수 있도록 간단한 XAML 컨트롤인 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)가 도입되었습니다. 이제 **MediaElement**를 통해 제공된 많은 재생 컨트롤 및 상태 API가 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 개체를 통해 사용 가능합니다. **MediaElement**는 이전 버전과의 호환성을 지원하기 위해 계속 작동되지만 이 클래스에 추가 기능은 제공되지 않습니다.

이 문서에서는 일반적인 미디어 재생 앱에서 사용하는 **MediaPlayer** 기능을 단계별로 안내합니다. **MediaPlayer**는 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 클래스를 모든 미디어 항목의 컨테이너로 사용합니다. 이 클래스를 사용하면 로컬 파일, 메모리 스트림 및 네트워크 소스를 비롯한 다양한 소스에서 모두 동일한 인터페이스를 사용하여 미디어를 로드 및 재생할 수 있습니다. [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)과 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList) 등, 재생 목록과 같은 고급 기능과 여러 오디오, 비디오와 미디어 소스를 관리하는 기능 및 메타데이터 추적 기능을 제공하는 **MediaSource**에서 작동하는 상위 수준 클래스도 있습니다. **MediaSource** 및 관련 API에 대한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조하세요.


##<a name="play-a-media-file-with-mediaplayer"></a>MediaPlayer를 사용하여 미디어 파일 재생  
**MediaPlayer**를 사용한 기본 미디어 재생은 매우 간단히 구현할 수 있습니다. 먼저 **MediaPlayer** 클래스의 새 인스턴스를 만듭니다. 앱에는 한 번에 여러 개의 **MediaPlayer** 인스턴스가 활성화될 수 있습니다. 다음으로 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 또는 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)와 같이 [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource)를 구현하는 개체에 플레이어의 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 속성을 설정합니다. 이 예제에서는 앱의 로컬 저장소에 있는 파일에서 **MediaSource**가 생성되고 소스에서 **MediaPlaybackItem**이 생성된 다음 플레이어의 **Source** 속성에 할당됩니다.

**MediaElement**와 달리 **MediaPlayer**는 기본적으로 자동으로 재생을 시작하지 않습니다. [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play)를 호출하거나 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 속성을 true로 설정하여 재생을 시작할 수 있습니다. 또는 기본 제공 미디어 컨트롤을 사용하여 사용자가 재생을 시작할 때까지 기다릴 수 있습니다.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

앱의 **MediaPlayer** 사용이 끝나면 [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) 메서드(C#에서 **Dispose**에 프로젝션됨)를 호출하여 플레이어에서 사용한 리소스를 정리해야 합니다.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##<a name="use-mediaplayerelement-to-render-video-in-xaml"></a>MediaPlayerElement를 사용하여 XAML에서 비디오 렌더링
XAML에 표시하지 않고 **MediaPlayer**에서 미디어를 재생할 수는 있지만 많은 미디어 재생 앱은 XAML 페이지에서 미디어를 렌더링하려고 합니다. 이렇게 하려면 경량 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤을 사용합니다. **MediaElement**와 마찬가지로 **MediaPlayerElement**를 사용하여 기본 제공 전송 컨트롤의 표시 여부를 지정할 수 있습니다.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

[**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764)를 호출하면 요소가 바인딩되는 **MediaPlayer** 인스턴스를 설정할 수 있습니다.

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

**MediaPlayerElement**에 재생 소스를 설정할 수도 있으며 요소는 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer) 속성을 사용하여 액세스할 수 있는 **MediaPlayer** 인스턴스를 자동으로 만듭니다.

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled)를 false로 설정하여 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)의 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)를 비활성화하면, **MediaPlayer** 및 [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls)(**MediaPlayerElement**에서 제공) 간 연결이 끊어지므로 기본 제공 전송 컨트롤이 플레이어의 재생을 더 이상 자동으로 제어할 수 없게 됩니다. 대신 고유한 컨트롤을 구현하여 **MediaPlayer**를 제어해야 합니다.

##<a name="common-mediaplayer-tasks"></a>일반적인 MediaPlayer 작업
이 섹션에서는 **MediaPlayer**의 몇 가지 기능을 사용하는 방법을 보여 줍니다.

###<a name="set-the-audio-category"></a>오디오 범주 설정
시스템이 재생 중인 미디어의 종류를 인식하도록 [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) 열거형 값 중 하나에 **MediaPlayer**의 [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) 속성을 설정합니다. 다른 응용 프로그램이 백그라운드에서 음악을 재생하는 경우 게임 음악은 자동으로 음소거되도록 게임은 해당 음악 스트림을 **GameMedia**로 분류해야 합니다. 음악 또는 동영상 응용 프로그램은 **GameMedia** 스트림보다 우선하도록 **Media** 또는 **Movie**로 스트림을 분류해야 합니다.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###<a name="output-to-a-specific-audio-endpoint"></a>특정 오디오 끝점에 출력
기본적으로 **MediaPlayer**의 오디오 출력은 시스템의 기본 오디오 끝점에 라우팅되지만 **MediaPlayer**가 출력에 사용해야 하는 특정 오디오 끝점을 지정할 수 있습니다. 아래 예제에서 [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector)는 디바이스의 오디오 렌더 범주를 고유하게 식별하는 문자열을 반환합니다. 다음으로 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) 메서드 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync)가 호출되고 선택된 유형의 사용 가능한 모든 디바이스 목록을 가져옵니다. 사용할 디바이스를 프로그래밍 방식으로 결정하거나 반환된 디바이스를 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox)에 추가하여 사용자가 디바이스를 선택할 수도 있습니다.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

디바이스 콤보 상자에 대한 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) 이벤트에서 **ComboBoxItem**의 [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) 속성에 저장된 **MediaPlayer**의 [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) 속성이 선택한 디바이스로 설정됩니다.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###<a name="playback-session"></a>재생 세션
이 문서의 앞에서 설명한 대로 **MediaElement** 클래스에서 노출하는 대부분의 함수는 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 클래스로 이동되었습니다. 여기에는 재생 위치, 플레이어가 일시 중지되었는지 재생 중인지 및 현재 재생 속도 등 플레이어의 재생 상태에 대한 정보가 포함됩니다. 또한 **MediaPlaybackSession**은 재생 중인 콘텐츠의 현재 버퍼링 및 다운로드 상태와 현재 재생 중인 비디오 콘텐츠의 가로 세로 비율 및 실제 크기를 포함하여 상태가 변경되면 알려주는 몇 가지 이벤트도 제공합니다.

다음 예제에서는 콘텐츠에서 10초 앞으로 건너뛰는 단추 클릭 처리기를 구현하는 방법을 보여 줍니다. 먼저 플레이어에 대한 **MediaPlaybackSession** 개체가 [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession) 속성으로 검색됩니다. 다음으로 [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) 속성이 현재 재생 위치 + 10초로 설정됩니다.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

다음 예제에서는 세션의 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) 속성을 설정하여 정상 재생 속도와 2배속 간에 전환하는 토글 단추를 사용하는 방법을 보여 줍니다.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###<a name="pinch-and-zoom-video"></a>손가락을 모아 비디오 확대/축소
**MediaPlayer**는 효과적으로 비디오를 확대할 수 있도록 렌더링해야 하는 비디오 콘텐츠 내에서 소스 사각형을 지정할 수 있습니다. 지정하는 사각형은 정규화된 사각형(0,0,1,1)을 기준으로 합니다. 여기서 0,0은 프레임의 왼쪽 상단이고 1,1은 프레임의 전체 너비 및 높이를 가리킵니다. 따라서 예를 들어 비디오의 오른쪽 위 사분면이 렌더링되도록 확대/축소 사각형을 설정하려면 사각형(.5,0,.5,.5)을 지정합니다.  소스 사각형이 정규화된 사각형(0,0,1,1) 내에 있도록 값을 확인하는 것이 중요합니다. 이 범위를 벗어나는 값을 설정하려고 하면 예외가 발생됩니다.

멀티 터치 제스처를 사용하여 손가락을 모아 확대/축소를 구현하려면 먼저 지원하려는 제스처를 지정해야 합니다. 이 예제에서는 크기 조정 및 변환 제스처를 요청합니다. 구독한 제스처 중 하나가 발생하면 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) 이벤트가 발생합니다. [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 이벤트는 전체 프레임으로 확대/축소를 다시 설정하는 데 사용됩니다. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

다음으로 현재 확대/축소 소스 사각형을 저장할 **Rect** 개체를 선언합니다.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

**ManipulationDelta** 처리기는 확대/축소 사각형의 배율 또는 변환을 조정합니다. 델타 배율 값이 1이 아니면 사용자가 축소 제스처를 수행했다는 의미입니다. 값이 1보다 큰 경우 콘텐츠를 확대하려면 소스 사각형을 더 작게 만들어야 합니다. 값이 1보다 작은 경우 축소하려면 소스 사각형을 더 크게 만들어야 합니다. 새 배율 값을 설정하기 전에 결과 사각형이 완전히 (0,0,1,1) 한도 내에 있는지 확인합니다.

배율 값이 1이면 변환 제스처가 처리됩니다. 사각형은 단순히 컨트롤의 너비와 높이로 나눈 제스처의 픽셀 수로 변환됩니다. 마찬가지로 결과 사각형이 (0,0,1,1) 범위 안에 있는지 확인합니다.

마지막으로 **MediaPlaybackSession**의 [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect)가 새로 조정된 사각형으로 설정되고 렌더링해야 할 비디오 프레임 내의 영역을 지정합니다.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

[**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 이벤트 처리기에서 소스 사각형을 다시 (0,0,1,1)로 설정하면 전체 비디오 프레임이 렌더링됩니다.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
        
##<a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>MediaPlayerSurface를 사용하여 Windows.UI.Composition 표면에 비디오를 렌더링합니다.
Windows10 버전 1607부터 **MediaPlayer**를 사용하여 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface)에 비디오를 렌더링할 수 있으며 플레이어가 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition) 네임스페이스의 API와 상호 작용할 수 있습니다. 컴퍼지션 프레임워크를 사용하면 XAML과 하위 수준 DirectX 그래픽 API 간에 시각적 계층의 그래픽 작업이 가능합니다. 그러면 모든 XAML 컨트롤에 비디오 렌더링과 같은 시나리오를 사용할 수 있습니다. 컴퍼지션 API 사용에 대한 자세한 내용은 [시각적 계층](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer)을 참조하세요.

다음 예제에서는 비디오 플레이어 콘텐츠를 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas) 컨트롤로 렌더링하는 방법을 보여 줍니다. 이 예제에서 미디어 플레이어 관련 호출은 [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) 및 [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963)입니다. **SetSurfaceSize**는 콘텐츠 렌더링에 할당해야 하는 버퍼의 크기를 시스템에 지시합니다. **GetSurface**는 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor)를 인수로 사용하고 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) 클래스의 인스턴스를 검색합니다. 이 클래스는 표면을 만드는 데 사용된 **Compositor** 및 **MediaPlayer**에 대한 액세스를 제공하고 [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface) 속성을 통해 표면 자체를 노출합니다.

이 예제의 나머지 코드는 비디오가 렌더링되는 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual)을 만들고 시각적 개체를 표시할 캔버스 요소의 크기를 설정합니다. 다음으로 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush)가 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface)에서 만들어지고 시각적 개체의 [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) 속성에 할당됩니다. 다음으로 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual)이 만들어지고 **SpriteVisual**이 해당 시각적 트리의 맨 위에 삽입됩니다. 마지막으로 [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981)을 호출하여 **Canvas**에 컨테이너 시각적 개체를 할당합니다.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
##<a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>MediaTimelineController를 사용하여 여러 플레이어에서 콘텐츠 동기화
이 문서의 앞에서 설명한 대로 앱에는 한 번에 여러 개의 **MediaPlayer** 개체가 활성화되어 있을 수 있습니다. 기본적으로 만든 각 **MediaPlayer**는 독립적으로 작동합니다. 일부 시나리오의 경우 비디오에 대한 설명 트랙 동기화와 같이 여러 플레이어의 재생 속도, 플레이어 상태, 재생 위치를 동기화할 수 있습니다. Windows10 버전 1607부터 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) 클래스를 사용하여 이 동작을 구현할 수 있습니다.

###<a name="implement-playback-controls"></a>재생 컨트롤 구현
다음 예제는 **MediaTimelineController**를 사용하여 **MediaPlayer**의 두 인스턴스를 제어하는 방법을 보여 줍니다. 먼저 **MediaPlayer**의 각 인스턴스가 인스턴스화되고 **Source**는 미디어 파일로 설정됩니다. 다음 새 **MediaTimelineController**가 만들어집니다. 각 **MediaPlayer**에서 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 속성을 false로 설정하면 각 플레이어에 연결된 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)가 사용할 수 없도록 설정됩니다. 그런 다음 [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) 속성이 타임라인 컨트롤러 개체로 설정됩니다.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**주의** [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)는 **MediaPlayer**와 SMTC(시스템 미디어 전송 컨트롤) 간에 자동 통합을 제공하지만 이 자동 통합은 **MediaTimelineController**로 제어되는 미디어 플레이어와 함께 사용할 수 없습니다. 따라서 플레이어의 타임라인 컨트롤러를 설정하기 전에 미디어 플레이어에 대한 명령 관리자를 사용하지 않도록 설정해야 합니다. 그러지 않으면 예외가 발생하고 "Attaching Media Timeline Controller is blocked because of the current state of the object(개체의 현재 상태 때문에 미디어 타임라인 컨트롤러의 연결이 차단되었습니다.)" 메시지가 표시됩니다. SMTC와 미디어 플레이어 통합에 대한 자세한 내용은 [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)을 참조하세요. **MediaTimelineController**를 사용 중이면 SMTC를 수동으로 계속 제어할 수 있습니다. 자세한 내용은 [시스템 미디어 전송 컨트롤의 수동 제어](system-media-transport-controls.md)를 참조하세요.

하나 이상의 미디어 플레이어에 **MediaTimelineController**를 연결한 후 컨트롤러에서 노출하는 메서드를 사용하여 재생 상태를 제어할 수 있습니다. 다음 예제에서는 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start)를 호출하여 미디어 시작 시 모든 관련 미디어 플레이어의 재생을 시작합니다.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

이 예제는 연결된 모든 미디어 플레이어를 일시 중지 및 다시 시작하는 방법을 보여 줍니다.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

연결된 모든 미디어 플레이어를 빨리 감으려면 재생 속도를 1보다 큰 값으로 설정합니다.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

다음 예제는 연결된 미디어 플레이어 중 하나의 콘텐츠 기간을 기준으로 타임라인 컨트롤러의 현재 재생 위치를 표시하는 **Slider** 컨트롤을 사용하는 방법을 보여 줍니다. 먼저 새 **MediaSource**가 만들어지고 미디어 소스의 [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted)에 대한 처리기가 등록됩니다. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

**OpenOperationCompleted** 처리기는 미디어 소스 콘텐츠의 기간을 검색하는 데 사용됩니다. 기간이 결정되면 **Slider** 컨트롤의 최대값은 미디어 항목의 총 시간(초)으로 설정됩니다. 값은 UI 스레드에서 실행되도록 하는 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)에 대한 호출 내에 설정됩니다.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

다음으로 타임라인 컨트롤러의 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged) 이벤트에 대한 처리기가 등록됩니다. 이 이벤트 처리기는 초당 약 4번 주기적으로 시스템에서 호출합니다.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

**PositionChanged**에 대한 처리기에서 슬라이더 값은 타임라인 컨트롤러의 현재 위치를 반영하도록 업데이트됩니다.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

###<a name="offset-the-playback-position-from-the-timeline-position"></a>타임라인 위치에서 재생 위치 오프셋
경우에 따라 타임라인 컨트롤러와 연결된 하나 이상의 미디어 플레이어의 재생 위치를 다른 플레이어에서 오프셋되도록 할 수 있습니다. 오프셋하려는 **MediaPlayer** 개체의 [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) 속성을 설정하여 이 작업을 수행할 수 있습니다. 다음 예제에서는 두 미디어 플레이어의 콘텐츠 기간을 사용하여 항목의 길이를 더하거나 빼는 두 슬라이더 컨트롤의 최소값과 최대값을 설정합니다.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

각 슬라이더에 대한 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) 이벤트에서 각 플레이어에 대한 **TimelineControllerPositionOffset**이 해당 값으로 설정됩니다.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

플레이어의 오프셋 값이 음수 재생 위치에 매핑되면 클립이 일시 중지되었다가 오프셋이 0이 되면 재생이 시작됩니다. 마찬가지로 오프셋 값이 미디어 항목 기간보다 큰 재생 위치에 매핑되면 단일 미디어 플레이어가 콘텐츠의 끝에 도달할 때와 같이 마지막 프레임이 표시됩니다.

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)
* [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)
* [미디어 중단 만들기, 예약 및 관리](create-schedule-and-manage-media-breaks.md)
* [백그라운드에서 미디어 재생](background-audio.md)





 

 




