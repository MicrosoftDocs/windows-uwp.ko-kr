---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: 이 문서에서는 UWP 앱이 오디오 스트림 수준에서 시스템 시작 변경 내용을 감지 하 고 응답 하는 방법을 설명 합니다.
title: 오디오 상태 변경 감지 및 대응
ms.date: 04/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84dbafa95f0151f8f8774d00ceae4b09b6ec5e88
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170517"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>오디오 상태 변경 감지 및 대응
Windows 10, 버전 1803부터 응용 프로그램은 시스템이 앱에서 사용 하는 오디오 스트림의 오디오 수준을 mutes 수 있는 시기를 감지할 수 있습니다. 캡처 및 렌더링 스트림, 특정 오디오 장치 및 오디오 범주 또는 앱이 미디어 재생에 사용 하는 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 개체에 대 한 알림을 받을 수 있습니다. 예를 들어, 경보가 울림 될 때 시스템은 오디오 재생 수준을 낮출 수 있습니다. 앱이 앱 매니페스트에서 *backgroundMediaPlayback* 기능을 선언 하지 않은 경우 시스템은 백그라운드에 들어가면 앱을 음소거 합니다. 

오디오 상태 변경을 처리 하는 패턴은 지원 되는 모든 오디오 스트림에서 동일 합니다. 먼저 [**AudioStateMonitor**](/uwp/api/windows.media.audio.audiostatemonitor) 클래스의 인스턴스를 만듭니다. 다음 예제에서 앱은 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 클래스를 사용 하 여 게임 채팅을 위한 오디오를 캡처합니다. 팩터리 메서드는 기본 통신 장치의 게임 채팅 오디오 캡처 스트림과 연결 된 오디오 상태 모니터를 가져오기 위해 호출 됩니다.  그런 다음 [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 이벤트에 대 한 처리기가 등록 됩니다 .이 이벤트는 연결 된 스트림의 오디오 수준을 시스템에서 변경할 때 발생 합니다.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

**SoundLevelChanged** 이벤트 처리기에서 처리기에 전달 된 **AudioStateMonitor** 발신자의 [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) 속성을 확인 하 여 스트림의 새 오디오 수준을 결정 합니다. 이 예제에서 앱은 소리 수준이 음소거 상태일 때 오디오 캡처를 중지 하 고 오디오 수준이 전체 볼륨으로 반환 될 때 캡처를 재개 합니다.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

**MediaCapture**로 오디오를 캡처하는 방법에 대 한 자세한 내용은 [MediaCapture를 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)를 참조 하세요.

[**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스의 모든 인스턴스에는 시스템에서 현재 재생 중인 콘텐츠의 볼륨 수준을 변경 하는 경우를 감지 하는 데 사용할 수 있는 **AudioStateMonitor** 가 연결 되어 있습니다. 재생 중인 콘텐츠 형식에 따라 오디오 상태 변경을 다르게 처리할 수 있습니다. 예를 들어 오디오를 낮출 때 포드캐스트 재생을 일시 중지 하도록 결정할 수 있지만 콘텐츠가 음악 인 경우 재생을 계속할 수 있습니다. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

**Mediaplayer**를 사용 하는 방법에 대 한 자세한 내용은 [mediaplayer를 사용 하 여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조 하세요. 

## <a name="related-topics"></a>관련 항목