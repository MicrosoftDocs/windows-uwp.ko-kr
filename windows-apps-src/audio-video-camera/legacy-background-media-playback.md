---
ms.assetid: 3848cd72-eccd-400e-93ff-13649cd81b6c
description: 이 문서에서는 재생을 위해 레거시 백그라운드 미디어 모델을 사용 하는 앱에 대 한 지원을 제공 하 고 새 모델로 마이그레이션하기 위한 지침을 제공 합니다.
title: 레거시 백그라운드 미디어 재생
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5fd7615204d16b3c082a78ee1c65180623048043
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163927"
---
# <a name="legacy-background-media-playback"></a>레거시 백그라운드 미디어 재생


이 문서에서는 UWP 앱에 배경 오디오 지원을 추가 하는 데 사용할 수 있는 레거시 2 프로세스 모델에 대해 설명 합니다. Windows 10, 버전 1607부터 구현 하기가 훨씬 더 간단한 배경 오디오에 대 한 단일 프로세스 모델입니다. 백그라운드 오디오에 대한 현재 권장 사항은 [백그라운드에서 미디어 재생](background-audio.md)을 참조하세요. 이 문서는 레거시 2 프로세스 모델을 사용 하 여 이미 개발 된 앱에 대 한 지원을 제공 하기 위한 것입니다.

> [!NOTE]
> Windows, 버전 1703부터 **BackgroundMediaPlayer** 는 더 이상 사용 되지 않으며 이후 버전의 windows에서 사용할 수 없습니다.

## <a name="background-audio-architecture"></a>배경 오디오 아키텍처

백그라운드 재생을 수행 하는 앱은 두 프로세스로 구성 됩니다. 첫 번째 프로세스는 포그라운드로 실행 되는 앱 UI와 클라이언트 논리를 포함 하는 주 앱입니다. 두 번째 프로세스는 모든 UWP 앱 백그라운드 작업과 마찬가지로 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 를 구현 하는 백그라운드 재생 작업입니다. 백그라운드 작업은 오디오 재생 논리 및 백그라운드 서비스를 포함 합니다. 백그라운드 작업은 시스템 미디어 전송 컨트롤을 통해 시스템과 통신 합니다.

다음 다이어그램은 시스템이 설계 되는 방법에 대 한 개요입니다.

![Windows 10 백그라운드 오디오 아키텍처](images/backround-audio-architecture-win10.png)
## <a name="mediaplayer"></a>MediaPlayer

[**Windows 비디오 재생**](/uwp/api/Windows.Media.Playback) 네임 스페이스는 배경에서 오디오를 재생 하는 데 사용 되는 api를 포함 합니다. 재생을 수행 하는 앱 당 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 의 단일 인스턴스가 있습니다. 배경 오디오 앱은 메서드를 호출 하 고 **MediaPlayer** 클래스의 속성을 설정 하 여 현재 추적, 시작 재생, 일시 중지, 빨리 감기, 되감기 등을 설정 합니다. Media player 개체 인스턴스는 항상 [**BackgroundMediaPlayer**](/uwp/api/windows.media.playback.backgroundmediaplayer.current) 속성을 통해 액세스 됩니다.

## <a name="mediaplayer-proxy-and-stub"></a>MediaPlayer 프록시 및 스텁

앱의 백그라운드 프로세스에서 **BackgroundMediaPlayer** 에 액세스 하는 경우에는 **MediaPlayer** 인스턴스가 백그라운드 작업 호스트에서 활성화 되 고 직접 조작할 수 있습니다.

포그라운드 응용 프로그램에서 **BackgroundMediaPlayer** 에 액세스 하는 경우 반환 되는 **MediaPlayer** 인스턴스는 실제로 백그라운드 프로세스의 스텁을 통신 하는 프록시입니다. 이 스텁은 백그라운드 프로세스 에서도 호스트 되는 실제 **MediaPlayer** 인스턴스와 통신 합니다.

백그라운드 프로세스 에서만 액세스할 수 있는 Mediaplayer 및 [**MediaPlayer.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 를 제외 하 고, 포그라운드 및 백그라운드 프로세스 모두 **mediaplayer** 인스턴스의 속성 대부분에 액세스할 수 있습니다 [**.**](/uwp/api/windows.media.playback.mediaplayer.source) 포그라운드 [**앱 및 백그라운드**](/uwp/api/windows.media.playback.mediaplayer.mediafailed)프로세스는 미디어 관련 [**이벤트에 대**](/uwp/api/windows.media.playback.mediaplayer.mediaopened) [**한 알림을**](/uwp/api/windows.media.playback.mediaplayer.mediaended)받을 수 있습니다.

## <a name="playback-lists"></a>재생 목록

배경 오디오 응용 프로그램의 일반적인 시나리오는 한 행에서 여러 항목을 재생 하는 것입니다. 이는 mediaplayer의 원본으로 설정할 수 있는 [**Mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 개체를 사용 하 여 백그라운드 프로세스에서 가장 쉽게 수행할 수 있습니다 .이 개체는 [**mediaplayer 속성에**](/uwp/api/windows.media.playback.mediaplayer.source) 할당 하 여 **mediaplayer** 에서 원본으로 설정할 수 있습니다.

백그라운드 프로세스에 설정 된 포그라운드 프로세스에서 **Mediaplaybacklist** 에 액세스할 수 없습니다.

## <a name="system-media-transport-controls"></a>시스템 미디어 전송 컨트롤

사용자는 Bluetooth 장치, SmartGlass 및 시스템 미디어 전송 컨트롤과 같은 방법으로 앱 UI를 직접 사용 하지 않고도 오디오 재생을 제어할 수 있습니다. 백그라운드 작업은 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 클래스를 사용 하 여 이러한 사용자가 시작한 시스템 이벤트를 구독 합니다.

백그라운드 프로세스 내에서 **SystemMediaTransportControls** 인스턴스를 가져오려면 [**MediaPlayer.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 속성을 사용 합니다. 포그라운드 앱은 [**SystemMediaTransportControls. GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview)를 호출 하 여 클래스의 인스턴스를 가져오지만 반환 된 인스턴스는 백그라운드 작업과 관련이 없는 전경 전용 인스턴스입니다.

## <a name="sending-messages-between-tasks"></a>작업 간 메시지 보내기

배경 오디오 응용 프로그램의 두 프로세스 간에 통신 하는 경우가 있습니다. 예를 들어 새 트랙이 재생을 시작할 때 백그라운드 작업에서 포그라운드 작업을 알리도록 하 고 새 노래 제목을 전경 작업으로 보내 화면에 표시할 수 있습니다.

간단한 통신 메커니즘은 포그라운드 프로세스와 백그라운드 프로세스 모두에서 이벤트를 발생 시킵니다. [**SendMessageToForeground**](/uwp/api/windows.media.playback.backgroundmediaplayer.sendmessagetoforeground) 및 [**SendMessageToBackground**](/uwp/api/windows.media.playback.backgroundmediaplayer.sendmessagetobackground) 메서드는 각각 해당 프로세스에서 이벤트를 호출 합니다. [**MessageReceivedFromBackground**](/uwp/api/windows.media.playback.backgroundmediaplayer.messagereceivedfrombackground) 및 [**MessageReceivedFromForeground**](/uwp/api/windows.media.playback.backgroundmediaplayer.messagereceivedfromforeground) 이벤트를 구독 하 여 메시지를 받을 수 있습니다.

데이터는 메시지를 받은 이벤트 처리기에 전달 되는 메시지 보내기 메서드에 인수로 전달 될 수 있습니다. [**Valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet) 클래스를 사용 하 여 데이터를 전달 합니다. 이 클래스는 문자열을 키로 포함 하 고 다른 값 형식을 값으로 포함 하는 사전입니다. 정수, 문자열 및 부울과 같은 단순 값 형식을 전달할 수 있습니다.

## <a name="background-task-life-cycle"></a>백그라운드 작업 수명 주기

백그라운드 작업의 수명은 앱의 현재 재생 상태와 밀접 하 게 연관 되어 있습니다. 예를 들어 사용자가 오디오 재생을 일시 중지 하는 경우 시스템은 상황에 따라 응용 프로그램을 종료 하거나 취소할 수 있습니다. 오디오 재생이 없는 일정 시간이 지나면 시스템이 백그라운드 작업을 자동으로 종료할 수 있습니다.

[**IBackgroundTask**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드는 앱이 포그라운드 앱에서 실행 되는 코드에서 [**BackgroundMediaPlayer**](/uwp/api/windows.media.playback.backgroundmediaplayer.current) 에 처음으로 액세스 하거나 [**MessageReceivedFromBackground**](/uwp/api/windows.media.playback.backgroundmediaplayer.messagereceivedfrombackground) 이벤트에 대 한 처리기를 등록 하는 경우 중 먼저 발생 하는 경우에 호출 됩니다. 먼저 **BackgroundMediaPlayer** 를 호출 하기 전에 메시지 수신 처리기에 등록 하는 것이 좋습니다. 이렇게 하면 포그라운드 앱이 백그라운드 프로세스에서 보낸 메시지를 놓치지 않습니다.

백그라운드 작업을 활성 상태로 유지 하려면 앱이 **Run** 메서드 내에서 [**BackgroundTaskDeferral**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral) 를 요청 하 고, 작업 인스턴스가 [**취소**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) 된 이벤트 나 [**완료**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.completed) 된 이벤트를 받을 때 [**BackgroundTaskDeferral**](/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete) 를 호출 해야 합니다. 이로 인해 리소스가 소비 되 고 시스템에서 앱의 백그라운드 작업이 종료 될 수 있으므로 **Run** 메서드를 반복 하거나 대기 하지 마십시오.

**실행** 메서드가 완료 되 고 지연이 요청 되지 않은 경우 백그라운드 작업에서 **완료** 된 이벤트를 가져옵니다. 경우에 따라 앱이 **취소** 된 이벤트를 가져오는 경우 **완료** 된 이벤트 뒤에 올 수도 있습니다. **실행** 이 실행 되는 동안 작업에서 **취소** 된 이벤트를 받을 수 있으므로이 잠재적 동시성를 관리 해야 합니다.

백그라운드 작업을 취소할 수 있는 상황은 다음과 같습니다.

-   독점 성을 하위 정책을 적용 하는 시스템에서 오디오 재생 기능이 있는 새 앱이 시작 됩니다. 아래의 [백그라운드 오디오 작업 수명에 대 한 시스템 정책](#system-policies-for-background-audio-task-lifetime) 섹션을 참조 하세요.

-   백그라운드 작업이 시작 되었지만 음악이 아직 재생 되지 않아 포그라운드 앱이 일시 중지 되었습니다.

-   들어오는 전화 통화 또는 VoIP 통화와 같은 기타 미디어 중단

다음과 같이 백그라운드 작업을 종료할 수 있는 경우는 다음과 같습니다.

-   VoIP 호출이 제공 되며 시스템에 사용 가능한 메모리가 부족 하 여 백그라운드 작업이 활성 상태로 유지 됩니다.

-   리소스 정책을 위반 했습니다.

-   작업 취소 또는 완료가 정상적으로 종료 되지 않습니다.

## <a name="system-policies-for-background-audio-task-lifetime"></a>백그라운드 오디오 작업 수명에 대 한 시스템 정책

다음 정책은 시스템에서 백그라운드 오디오 작업의 수명을 관리 하는 방법을 결정 하는 데 도움이 됩니다.

### <a name="exclusivity"></a>독점 성을

사용 하도록 설정 하는 경우이 하위 정책은 지정 된 시간에 백그라운드 오디오 작업의 수를 최대 1 개로 제한 합니다. 모바일 및 기타 비 데스크톱 Sku에서 사용할 수 있습니다.

### <a name="inactivity-timeout"></a>비활성 시간 제한

리소스 제약 조건으로 인해 시스템은 비활성 기간 후 백그라운드 작업을 종료할 수 있습니다.

다음 조건이 모두 충족 되는 경우 백그라운드 작업은 "비활성"으로 간주 됩니다.

-   포그라운드 앱은 표시 되지 않습니다 (일시 중단 또는 종료 됨).

-   백그라운드 미디어 플레이어의 재생 상태가 아닙니다.

이러한 두 조건이 모두 충족 되 면 백그라운드 미디어 시스템 정책에서 타이머를 시작 합니다. 타이머가 만료 될 때 두 가지 조건이 변경 되지 않은 경우 백그라운드 미디어 시스템 정책에서 백그라운드 작업을 종료 합니다.

### <a name="shared-lifetime"></a>공유 수명

사용 하도록 설정 된 경우이 하위 정책은 백그라운드 작업을 포그라운드 작업의 수명에 따라 강제로 적용 합니다. 사용자 또는 시스템에 의해 포그라운드 작업이 종료 되 면 백그라운드 작업도 종료 됩니다.

그러나이는 전경의 배경에 따라 달라 지는 것을 의미 하지는 않습니다. 백그라운드 작업이 종료 되 면이로 인해 포그라운드 작업이 종료 되지 않습니다.

다음 표에서는 장치 유형에 적용 되는 정책을 보여 줍니다.

| 하위 정책             | 데스크톱  | 모바일   | 기타    |
|------------------------|----------|----------|----------|
| **독점 성을**        | 사용 안 함 | 사용  | 사용  |
| **비활성 시간 제한** | 사용 안 함 | 사용  | 사용 안 함 |
| **공유 수명**    | 사용  | 사용 안 함 | 사용 안 함 |


 

 