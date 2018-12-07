---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: 이 문서에서는 UWP 앱이 오디오 스트림 수준에서 시스템이 시작한 변경 사항을 검색하고 이에 대응하는 방법에 대해 설명합니다.
title: 오디오 상태 변경 사항 검색 및 대응
ms.date: 04/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 69eeb82fd9a1e043e99b7fe0d635ca750779eda5
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8808478"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>오디오 상태 변경 사항 검색 및 대응
Windows 10, 버전 1803부터는 시스템이 앱이 사용 중인 오디오 스트림의 오디오 레벨을 낮추거나 음을 소거하면 앱이 이를 검색할 수 있습니다. 캡처 및 렌더 스트림, 특정 오디오 디바이스 및 오디오 범주, 또는 미디어 재생을 위해 앱이 사용하고 있는 [**MediaPlayer**](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) 개체에 대한 알림을 받을 수 있습니다. 예를 들어 시스템은 알람이 울리는 동안 오디오 재생 수준을 낮추거나 '더킹(duck)'할 수 있습니다. 앱이 앱 매니페스트에서 *backgroundMediaPlayback* 기능을 선언하지 않은 경우 앱이 백그라운드로 전환할 때 앱의 음이 소거됩니다. 

오디오 상태 변경을 처리하기 위한 패턴은 지원되는 모든 오디오 스트림에서 동일합니다. 먼저 [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor) 클래스의 인스턴스를 만듭니다. 다음 예에서 앱은 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 클래스를 사용하여 게임 채팅을 위한 오디오를 캡처합니다. 팩터리 메서드를 호출하여 기본 통신 디바이스의 게임 채팅 오디오 캡처 스트림과 연결된 오디오 상태 모니터를 가져옵니다.  그 다음 [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 이벤트에 대한 처리기를 등록하며, 이 이벤트는 시스템이 관련 스트림의 오디오 레벨을 변경할 때 발생합니다.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

**SoundLevelChanged** 이벤트 처리기에서 스트림에 대 한 새로운 오디오 레벨 결정에 **AudioStateMonitor** 보낸 처리기에 전달 된 [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) 속성을 확인 합니다. 이 예제에서 앱은 사운드 레벨 음이 소거되면 오디오 캡처를 중지하고 오디오 레벨이 전체 볼륨으로 돌아가면 캡처를 다시 시작합니다.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

**MediaCapture**를 사용한 오디오 캡처에 대한 자세한 내용은 [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)를 참조하세요.

[**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 클래스의 모든 인스턴스에는 관련 **AudioStateMonitor**가 있으며, 이것을 사용하여 시스템이 현재 재생 중인 콘텐츠의 볼륨 레벨을 변경하면 이를 감지할 수 있습니다. 재생할 콘텐츠의 유형에 따라 오디오 상태 변경 사항을 다르게 처리하도록 결정할 수 있습니다. 예를 들어, 오디오가 낮아지면 팟캐스트의 재생을 일시 중지하지만 콘텐츠가 음악이면 계속 재생하도록 결정할 수 있습니다. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

**MediaPlayer** 사용 방법은 [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조하세요. 

## <a name="related-topics"></a>관련 항목