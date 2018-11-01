---
author: adwilso
Description: Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. There are many ways to send the notifications depending on the needs of your application
title: 올바른 푸시 알림 채널 유형 선택
ms.author: mijacobs
ms.date: 07/07/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db2cf1c732669a2ae45b8a5eb427e8864446c800
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5876813"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>올바른 푸시 알림 채널 유형 선택

이 문서에서는 앱에 콘텐츠를 전달하는 데 도움이 되는 세 가지 유형의 UWP 푸시 알림 채널(주, 보조 및 대체)에 대해 다룹니다. 

(푸시 알림을 만드는 방법에 대 한 자세한 내용은 [Windows 푸시 알림 서비스 (WNS) 개요](../tiles-and-notifications/windows-push-notification-services--wns--overview.md)참조). 

## <a name="types-of-push-channels"></a>푸시 채널 유형 

UWP 앱에 알림에 보내는 데 사용할 수 있는 세 가지 유형의 푸시 채널이 있습니다. 채널은 다음과 같습니다. 

[기본 채널](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -"전통적인" 푸시 채널입니다. 스토어에 있는 아무 앱에서나 알림 메시지, 타일, 원시 또는 배지 알림(알림 메시지/타일/배지 설명에 대한 링크)을 보내는 데 사용할 수 있습니다.

[보조 타일 채널](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - 보조 타일에 대한 타일 업데이트를 푸시하는 데 사용됩니다. 사용자의 시작 화면에 고정된 보조 타일에 타일 또는 배지 알림을 보내는 데만 사용할 수 있습니다.

[대체 채널](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - 크리에이터 업데이트에 추가된 새로운 유형의 채널입니다. 스토어에서 등록되지 않은 앱을 포함하여 모든 UWP 앱에 원시 알림을 보낼 수 있습니다. 

> [!NOTE]
> 어떤 푸시 채널을 사용하든, 일단 장치에서 앱이 실행되면 항상 로컬 알림 메시지, 타일 또는 배지 알림을 보낼 수 있습니다. 포그라운드 앱 프로세스 또는 백그라운드 작업에서 로컬 알림을 보낼 수 있습니다. 


## <a name="primary-channels"></a>기본 채널

현재 Windows에서 가장 일반적으로 사용되는 채널로, Microsoft Store를 통해 앱을 배포하는 거의 모든 시나리오에 적합합니다. 앱에 모든 종류의 알림을 보낼 수 있습니다. 

### <a name="what-do-primary-channels-enable"></a>기본 채널로 무엇을 할 수 있을까요?

-   **기본 채널로 타일 또는 배지 업데이트를 보내기.** 사용자가 여러분의 타일을 시작 화면에 고정하기로 선택한다면 여러분에게는 아주 좋은 홍보 기회입니다. 앱 내에서 유용한 정보 또는 경험 미리 알림이 포함된 업데이트를 보내세요. 
-   **알림 메시지 보내기.** 알림 메시지는 사용자 앞에서 즉시 몇 가지 정보를 얻을 수 있는 기회입니다. 셸을 사용하여 대부분의 앱 맨 위에 그리며, 사용자가 나중에 뒤로 이동하여 상호 작용할 수 있도록 알림 센터에 실시간으로 제공됩니다. 
-   **백그라운드 작업을 트리거하는 원시 알림 보내기** 알림을 기반으로 사용자 대신 작업을 수행해야 하는 경우가 종종 있습니다. 원시 알림을 사용하여 앱의 백그라운드 작업을 실행할 수 있습니다. 
-   **TLS을 사용하여 Windows에서 제공하는 전송 중 메시지 암호화.** WNS로 들어오고 사용자 장치로 나가는 메시지가 모두 실시간으로 암호화됩니다.  

### <a name="limitations-of-primary-channels"></a>기본 채널의 제한

-   알림을 푸시하려면 WNS REST API를 사용해야 하는데, 이 API는 장치 공급업체의 표준이 아닙니다. 
-   앱당 채널을 하나만 만들 수 있습니다. 
-   Microsoft Store에 앱을 등록해야 합니다.

### <a name="creating-a-primary-channel"></a>기본 채널 만들기 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>보조 타일 채널

타일 및 배지 업데이트를 보조 타일로 푸시하는 데 사용할 수 있는 채널입니다. 그룹 채팅의 새 메시지나 업데이트된 스포츠 점수처럼 사용자가 앱 내에서 상호 작용할 수 있는 흥미로운 활동 또는 작업에 대해 알리는 데 사용됩니다. 

### <a name="what-do-secondary-tile-channels-enable"></a>보조 타일 채널로 무엇을 할 수 있을까요

-   보조 타일에 타일 또는 배지 알림 보내기. 보조 타일은 사용자를 다시 앱으로 불러 들이는 좋은 방법입니다. 보조 타일은 사용자가 관심을 갖고 있는 정보의 링크이며, 타일에 관련 정보를 표시하면 사용자를 계속 타일로 돌아오게 만들 수 있습니다.
-   다양한 타일 간 채널 분리(및 만료). 사용자가 시작 화면에 고정할 수 있는 다양한 유형의 보조 타일 간에 백 엔드 논리를 분리할 수 있습니다. 
-   TLS을 사용하여 Windows에서 제공하는 전송 중 메시지 암호화. WNS로 들어오고 사용자 장치로 나가는 메시지가 모두 실시간으로 암호화됩니다.  

### <a name="limitations-of-secondary-tile-channels"></a>보조 타일 채널의 제한

-   메시지 알림 또는 원시 알림을 사용할 수 없습니다. 보조 타일로 전송되는 메시지 알림 또는 원시 알림은 시스템에서 무시됩니다.
-   Microsoft Store에 앱을 등록해야 합니다.


### <a name="creating-a-secondary-tile-channel"></a>보조 타일 채널 만들기 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>대체 채널

대체 채널을 사용하면 앱을 Microsoft Store에 등록하거나 앱에 사용되는 기본 채널 이외의 푸시 채널을 만들지 않고도 푸시 알림을 보낼 수 있습니다. 
 
### <a name="what-do-alternate-channels-enable"></a>대체 채널로 무엇을 할 수 있을까요?
-   Windows 장치에서 실행되는 UWP에 원시 푸시 알림을 보냅니다. 대체 채널은 원시 알림만 허용합니다.
-   앱 내의 다양한 기능에 대한 여러 원시 푸시 채널을 만들 수 있습니다. 한 앱에서 최대 1000개의 대체 채널을 만들 수 있으며 각 채널은 30일 동안 유효합니다. 이러한 각 채널은 앱에서 별도로 관리 또는 취소할 수 있습니다.
-   Microsoft Store에 앱을 등록하지 않고 대체 푸시 채널을 만들 수 있습니다. 앱을 Microsoft Store에 등록하지 않고 장치에 설치하더라도 계속해서 푸시 알림을 받을 수 있습니다.
-   서버에서 W3C 표준 REST API 및 VAPID 프로토콜을 사용하여 알림을 푸시할 수 있습니다. 대체 채널은 W3C 표준 프로토콜을 사용하며, 따라서 유지 관리해야 하는 서버 논리를 간소화할 수 있습니다.
-   종합적인 전체 메시지 암호화. 기본 채널이 전송 중 암호화를 제공하지만 보다 강력한 보안을 원하는 경우 대체 채널을 사용하여 앱이 암호화 헤더를 통과하게 하는 방법으로 메시지를 보호할 수 있습니다. 

### <a name="limitations-of-alternate-channels"></a>대체 채널의 제한
-   앱에서 알림 메시지, 타일 또는 배지 유형의 알림을 보낼 수 없습니다. 대체 채널을 사용하면 다른 유형의 알림을 보내는 기능이 제한됩니다. 하지만 앱이 백그라운드 작업에서 로컬 알림을 보내는 기능은 유지됩니다. 
-   기본 또는 보조 타일 채널 이외의 다른 REST API가 필요합니다. 표준 W3C REST API를 사용하기 때문에 앱에서 푸시 알림 메시지 또는 타일 업데이트를 보내려면 다른 논리가 필요합니다.

### <a name="creating-an-alternate-channel"></a>대체 채널 만들기 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>채널 유형 비교
다음은 채널 유형을 비교한 정보입니다.

<table>

<tr class="header">
<th align="left"><b>유형</b></th>
<th align="left"><b>알림 메시지 푸시 여부</b></th>
<th align="left"><b>타일/배지 푸시 여부</b></th>
<th align="left"><b>원시 알림 푸시 여부</b></th>
<th align="left"><b>인증</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>스토어 등록 필요 여부</b></th>
<th align="left"><b>채널</b></th>
<th align="left"><b>암호화</b></th>
</tr>


<tr class="odd">
<td align="left">기본</td>
<td align="left">예</td>
<td align="left">예 - 기본 타일만</td>
<td align="left">예</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">예</td>
<td align="left">앱당 하나</td>
<td align="left">전송 중</td>
</tr>
<tr class="even">
<td align="left">보조 타일</td>
<td align="left">아니요</td>
<td align="left">예 - 보조 타일만</td>
<td align="left">아니요</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">예</td>
<td align="left">보조 타일당 하나</td>
<td align="left">전송 중</td>
</tr>
<tr class="odd">
<td align="left">대체</td>
<td align="left">아니요</td>
<td align="left">아니요</td>
<td align="left">예</td>
<td align="left">VAPID</td>
<td align="left">WebPush W3C 표준</td>
<td align="left">아니요</td>
<td align="left">앱당 1,000개</td>
<td align="left">전송 중 + 헤더 통과를 통한 종합적 암호화 가능(앱 코드 필요)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>올바른 채널 선택

일반적으로 앱에서 기본 채널을 사용하는 것이 좋지만 다음과 같은 몇 가지 예외가 있습니다. 

1. 보조 타일에 타일 업데이트를 푸시하려는 경우 보조 타일 푸시 채널을 사용합니다.
2. 다른 서비스에 채널을 전달하려는 경우(예: 브라우저) 대체 채널을 사용합니다.
3. Windows 스토어에 나열되지 않는 앱(예: LOB 앱)을 만들려는 경우 대체 채널을 사용합니다.
4. 서버에 다시 사용하고 싶은 기존 웹 푸시 코드가 있거나 백 엔드 서비스에 여러 채널을 만들고 싶은 경우 대체 채널을 사용합니다.

## <a name="related-articles"></a>관련 문서

* [로컬 타일 알림 보내기](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [적응형 및 대화형 알림 메시지](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [빠른 시작: 푸시 알림 보내기](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [푸시 알림을 통해 배지를 업데이트하는 방법](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [알림 채널 요청, 만들기 및 저장 방법](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [실행 중인 응용 프로그램에 대한 알림을 가로채는 방법](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [WNS(Windows 푸시 알림 서비스)를 사용하여 인증하는 방법](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [푸시 알림 서비스 요청 및 응답 헤더](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [푸시 알림에 대한 지침 및 검사 목록](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [원시 알림](raw-notification-overview.md)
