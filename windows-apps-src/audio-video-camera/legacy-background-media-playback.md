---
author: drewbatgit
ms.assetid: 3848cd72-eccd-400e-93ff-13649cd81b6c
description: 이 문서에서는 재생을 위한 레거시 백그라운드 미디어 모델을 사용하여 앱에 대한 지원을 제공하고 새 모델로의 마이그레이션을 위한 지침을 제공합니다.
title: 레거시 백그라운드 미디어 재생
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 68695125c2056adca8186120db7875cb3a68baf8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.locfileid: "219057"
---
# <a name="legacy-background-media-playback"></a>레거시 백그라운드 미디어 재생

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 UWP 앱에 백그라운드 오디오 지원을 추가하기 위한 레거시 두 프로세스 모델을 설명합니다. Windows 10 버전 1607부터 백그라운드 오디오에 대해 더 간단하게 구현할 수 있는 단일 프로세스 모델이 도입되었습니다. 백그라운드 오디오에 대한 현재 권장 사항은 [백그라운드에서 미디어 재생](background-audio.md)을 참조하세요. 이 문서는 레거시 두 프로세스 모델을 사용하여 이미 개발된 앱에 대한 지원을 제공하도록 작성되었습니다.

## <a name="background-audio-architecture"></a>백그라운드 오디오 아키텍처

백그라운드 재생을 수행하는 앱은 두 개의 프로세스로 구성됩니다. 첫 번째 프로세스는 포그라운드에서 실행되는, 앱 UI 및 클라이언트 논리를 포함하는 메인 앱입니다. 두 번째 프로세스는 모든 UWP 앱 백그라운드 작업처럼 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)를 구현하는 백그라운드 재생 작업입니다. 백그라운드 작업에서 오디오 재생 논리 및 백그라운드 서비스가 포함됩니다. 백그라운드 작업이 시스템 미디어 전송 컨트롤을 통해 시스템과 통신합니다.

다음 다이어그램은 시스템 설계 방법을 대략적으로 보여 줍니다.

![Windows 10 백그라운드 오디오 아키텍처](images/backround-audio-architecture-win10.png)
## <a name="mediaplayer"></a>MediaPlayer

[**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 네임스페이스는 백그라운드에서 오디오를 재생하는 데 사용되는 API를 포함합니다. 앱마다 재생이 진행되는 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535)의 단일 인스턴스가 있습니다. 백그라운드 오디오 앱은 **MediaPlayer** 클래스에서 현재 트랙 설정, 재생 시작, 일시 중지, 빨리 감기, 되감기 등을 설정하기 위한 메서드를 호출하고 속성을 설정합니다. 미디어 플레이어 개체 인스턴스는 항상 [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528) 속성을 통해 액세스합니다.

## <a name="mediaplayer-proxy-and-stub"></a>MediaPlayer 프록시 및 스텁

앱의 백그라운드 프로세스에서 **BackgroundMediaPlayer.Current**에 액세스되면 **MediaPlayer** 인스턴스가 백그라운드 작업 호스트에서 활성화되며 직접 조작할 수 있습니다.

포그라운드 응용 프로그램에서 **BackgroundMediaPlayer.Current**에 액세스되면 반환되는 **MediaPlayer** 인스턴스는 실제로 백그라운드 프로세스에서 스텁과 통신하는 프록시입니다. 이 스텁은 백그라운드 프로세스에서도 호스트되는 실제 **MediaPlayer** 인스턴스와 통신합니다.

포그라운드 및 백그라운드 프로세스에서는 백그라운드 프로세스에서만 액세스할 수 있는 [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 및 [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635)를 제외하고 **MediaPlayer** 인스턴스의 속성 대부분에 액세스할 수 있습니다. 포그라운드 앱 및 백그라운드 프로세스 모두 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/dn652609), [**MediaEnded**](https://msdn.microsoft.com/library/windows/apps/dn652603) 및 [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/dn652606)와 같은 미디어 관련 이벤트의 알림을 받을 수 있습니다.

## <a name="playback-lists"></a>재생 목록

백그라운드 오디오 응용 프로그램에 대한 일반적인 시나리오는 여러 항목을 차례대로 재생하는 것입니다. 이 시나리오는 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 개체를 사용하여 백그라운드 프로세스에서 가장 쉽게 수행됩니다. 이 개체를 [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 속성에 할당하여 **MediaPlayer**의 소스로 설정할 수 있습니다.

백그라운드 프로세스에서 설정된 포그라운드 프로세스에서 **MediaPlaybackList**에 액세스할 수는 없습니다.

## <a name="system-media-transport-controls"></a>시스템 미디어 전송 컨트롤

사용자는 앱의 UI를 직접 사용하지 않고 Bluetooth 디바이스, SmartGlass, 시스템 미디어 전송 컨트롤과 같은 수단을 통해 오디오 재생을 제어할 수 있습니다. 백그라운드 작업은 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 클래스를 사용하여 이러한 사용자가 시작한 이벤트를 구독합니다.

백그라운드 프로세스 내에서 **SystemMediaTransportControls** 인스턴스를 가져오려면 [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) 속성을 사용합니다. 포그라운드 앱은 [**SystemMediaTransportControls.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708)를 호출하여 이 클래스의 인스턴스를 가져오지만 반환되는 인스턴스는 백그라운드 작업과 관련이 없는 포그라운드 전용 인스턴스입니다.

## <a name="sending-messages-between-tasks"></a>작업 간에 메시지 보내기

백그라운드 오디오 앱의 두 프로세스 간에 통신이 필요한 경우가 있습니다. 예를 들어 새 트랙 재생이 시작될 때 백그라운드 작업이 포그라운드 작업에 알림을 보낸 다음 새 노래 제목을 포그라운드 작업에 보내 화면에 표시하도록 할 수 있습니다.

간단한 통신 메커니즘을 사용하여 포그라운드 및 백그라운드 프로세스에서 이벤트를 발생시킬 수 있습니다. [**SendMessageToForeground**](https://msdn.microsoft.com/library/windows/apps/dn652533) 및 [**SendMessageToBackground**](https://msdn.microsoft.com/library/windows/apps/dn652532) 메서드는 각각 해당 프로세스에서 이벤트를 호출합니다. [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) 및 [**MessageReceivedFromForeground**](https://msdn.microsoft.com/library/windows/apps/dn652531) 이벤트를 구독하여 메시지를 받을 수 있습니다.

그러면 데이터가 메시지 전송 메서드에 인수로 전달되며, 해당 메시지 전송 메서드가 메시지 수신 이벤트 처리기로 전달될 수 있습니다. [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 클래스를 사용하여 데이터를 전달합니다. 이 클래스는 문자열을 키로 포함하고 기타 값 형식을 값으로 포함하는 사전입니다. 정수, 문자열 및 부울 같은 간단한 값 형식을 전달할 수 있습니다.

## <a name="background-task-life-cycle"></a>백그라운드 작업 수명 주기

백그라운드 작업의 수명은 앱의 현재 재생 상태와 밀접하게 연결됩니다. 예를 들어 사용자가 오디오 재생을 일시 중지하면 시스템에서는 상황에 따라 앱을 종료하거나 취소할 수 있습니다. 오디오를 재생하지 않고 일정 시간이 경과되면 시스템에서 백그라운드 작업이 자동으로 종료됩니다.

[**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드는 백그라운드 작업은 앱이 포그라운드 앱에서 실행되는 코드의 [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528)에 처음으로 액세스할 때와 [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) 이벤트에 대한 처리기를 등록할 때 중에서 먼저 일어나는 작업 후에 호출됩니다. 포그라운드 앱이 백그라운드 프로세스에서 보낸 메시지를 누락하지 않도록 하기 위해 **BackgroundMediaPlayer.Current**를 처음 호출하기 전에 메시지 수신 처리기를 등록하는 것이 좋습니다.

백그라운드 작업을 계속 유지하려면 앱이 **Run** 메서드 내에서 [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499)을 요청하고 작업 인스턴스가 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 또는 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) 이벤트를 수신할 때 [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)를 호출해야 합니다. 리소스가 사용되고 앱의 백그라운드 작업이 시스템에 의해 종료될 수 있으므로 **Run** 메서드에서 루핑하거나 기다리지는 마세요.

백그라운드 작업은 **Run** 메서드가 완료되고 지연이 요청되지 않을 때 **Completed** 이벤트를 가져옵니다. 경우에 따라 앱이 **Canceled** 이벤트를 가져올 때 **Completed** 이벤트도 따라올 수 있습니다. 작업은 **Run**가 실행되는 동안 **Canceled** 이벤트를 수신할 수 있으므로 이러한 잠재적인 동시 상황을 관리해야 합니다.

다음과 같이 백그라운드 작업을 취소할 수 있는 상황이 있을 수 있습니다.

-   독점 하위 정책을 적용하는 시스템에서 오디오 재생 기능을 가진 새 앱을 시작합니다. 아래의 [백그라운드 오디오 작업 수명에 대한 시스템 정책](#system-policies-for-background-audio-task-lifetime)을 참조하세요.

-   백그라운드 작업이 시작되었으나 음악이 아직 재생되지 않고 이후 포그라운드 앱이 일시 중단된 경우

-   수신 전화 통화 또는 VoIP 전화와 같은 기타 미디어 중단

다음과 같이 예고 없이 백그라운드 작업을 취소할 수 있는 상황

-   VoIP 전화가 오고 시스템의 메모리가 부족하여 백그라운드 작업을 유지할 수 없습니다.

-   리소스 정책에 위반된 경우

-   작업 취소 또는 완료가 정상적으로 종료되지 않은 경우

## <a name="system-policies-for-background-audio-task-lifetime"></a>백그라운드 오디오 작업 수명에 대한 시스템 정책

다음과 같은 정책은 시스템이 백그라운드 오디오 작업의 수명을 관리하는 방법을 확인하는 데 도움이 됩니다.

### <a name="exclusivity"></a>독점

이 하위 정책은 사용하도록 설정하는 경우 백그라운드 오디오 작업 수를 한 번에 1 이하로 제한합니다. 이 정책은 모바일 및 기타 비데스크톱 SKU에서 사용하도록 설정됩니다.

### <a name="inactivity-timeout"></a>비활성 시간 제한

리소스 제약으로 인해 시스템은 일정 기간의 비활성 상태 후 백그라운드 작업을 종료할 수 있습니다.

백그라운드 작업은 다음 조건이 모두 충족되는 경우 "비활성"으로 간주됩니다.

-   포그라운드 앱은 표시되지 않습니다(일시 중단 또는 중단됨).

-   백그라운드 미디어 플레이어는 재생 상태가 아닙니다.

이러한 조건을 모두 만족할 경우 백그라운드 미디어 시스템 정책이 타이머를 시작합니다. 타이머가 만료될 때 어떤 조건도 변경되지 않으면 백그라운드 미디어 시스템 정책은 백그라운드 작업을 종료합니다.

### <a name="shared-lifetime"></a>공유 수명

이 옵션을 사용하도록 설정하면 이 하위 정책은 강제로 백그라운드 작업이 포그라운드 작업의 수명에 종속되도록 합니다. 포그라운드 작업이 사용자 또는 시스템에 의해 종료되면 백그라운드 작업도 종료됩니다.

그러나 포그라운드가 백그라운드에 종속되는 것은 아닙니다. 백그라운드 작업이 종료되어도 포그라운드 작업을 강제로 종료되지는 않습니다.

다음 표에는 어떤 정책이 어떤 디바이스 유형에 적용되는지 나와 있습니다.

| 하위 정책             | 바탕 화면  | 모바일   | 기타    |
|------------------------|----------|----------|----------|
| **독점**        | 사용 안 함 | 사용  | 사용  |
| **비활성 시간 제한** | 사용 안 함 | 사용  | 사용 안 함 |
| **공유 수명**    | 사용  | 사용 안 함 | 사용 안 함 |


 

 




