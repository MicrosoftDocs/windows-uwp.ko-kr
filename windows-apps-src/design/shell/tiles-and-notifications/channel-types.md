---
Description: WNS (Windows Push Notification Services)를 사용 하면 타사 개발자가 자신의 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. 응용 프로그램의 필요에 따라 알림을 보내는 방법에는 여러 가지가 있습니다.
title: 올바른 푸시 알림 채널 유형 선택
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 326012a38f2d4a8cd7d5c406c160db5168c9877d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219226"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>올바른 푸시 알림 채널 유형 선택

이 문서에서는 앱에 콘텐츠를 제공 하는 데 도움이 되는 세 가지 유형의 Windows 푸시 알림 채널 (기본, 보조 및 대체)을 설명 합니다. 

푸시 알림을 만드는 방법에 대 한 자세한 내용은 [WNS (Windows 푸시 Notification Services) 개요](../tiles-and-notifications/windows-push-notification-services--wns--overview.md)를 참조 하세요. 

## <a name="types-of-push-channels"></a>푸시 채널의 유형 

Windows 앱에 알림을 보내는 데 사용할 수 있는 푸시 채널에는 세 가지 유형이 있습니다. 아래에 이 계정과 키의 예제가 나와 있습니다. 

[기본 채널](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -"기존" 푸시 채널입니다. 는 스토어의 모든 앱에서 알림, 타일, raw 또는 배지 알림을 보내는 데 사용할 수 있습니다. [자세한 내용은 여기를 참조](windows-push-notification-services--wns--overview.md)하세요.

[보조 타일 채널](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -타일 업데이트를 보조 타일로 푸시하는 데 사용 됩니다. 사용자의 시작 화면에 고정 된 보조 타일에 타일 또는 배지 알림을 보내는 데만 사용할 수 있습니다.

[대체 채널](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -작성자 업데이트에 추가 된 새 형식의 채널입니다. 저장소에 등록 되지 않은 앱을 포함 하 여 모든 Windows 앱으로 원시 알림을 보낼 수 있습니다. 

> [!NOTE]
> 앱이 장치에서 실행 되는 경우 사용 하는 푸시 채널에 관계 없이 항상 로컬 알림, 타일 또는 배지 알림을 보낼 수 있습니다. 포그라운드 앱 프로세스 또는 백그라운드 작업에서 로컬 알림을 보낼 수 있습니다. 


## <a name="primary-channels"></a>기본 채널

이는 현재 Windows에서 가장 일반적으로 사용 되는 채널 이며 앱이 Microsoft Store를 통해 배포 되는 거의 모든 시나리오에 적합 합니다. 앱에 모든 유형의 알림을 보낼 수 있습니다. 

### <a name="what-do-primary-channels-enable"></a>기본 채널은 어떤 기능을 사용할 수 있나요?

-   **타일 또는 배지 업데이트를 기본 타일로 보냅니다.** 사용자가 타일을 시작 화면에 고정 하도록 선택한 경우에는이를 해제할 수 있습니다. 앱 내에서 유용한 정보 또는 환경 미리 알림이 포함 된 업데이트를 보냅니다. 
-   **알림 메시지를 보내는 중입니다.** 알림 메시지는 사용자 앞에 일부 정보를 즉시 얻을 수 있는 기회입니다. 이러한 항목은 셸을 통해 대부분의 앱을 기반으로 하 고, 사용자가 뒤로 돌아가 나중에 조작할 수 있도록 작업 센터에 살고 있습니다. 
-   **백그라운드 작업을 트리거하는 원시 알림을 보냅니다.** 알림에 따라 사용자를 대신 하 여 일부 작업을 수행 하려는 경우가 있습니다. 원시 알림을 사용 하면 앱의 백그라운드 작업을 실행할 수 있습니다. 
-   **TLS를 사용 하 여 Windows에서 제공 하는 전송의 메시지 암호화.** 메시지는 모두 WNS로 들어오고 사용자의 장치로 이동 하는 통신에서 암호화 됩니다.  

### <a name="limitations-of-primary-channels"></a>기본 채널의 제한 사항

-   에는 장치 공급 업체 간에 표준이 아닌 REST API WNS를 사용 하 여 알림을 푸시 해야 합니다. 
-   앱 당 하나의 채널만 만들 수 있습니다. 
-   앱이 Microsoft Store에 등록 되어 있어야 합니다.

### <a name="creating-a-primary-channel"></a>기본 채널 만들기 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>보조 타일 채널

이러한 채널은 타일 및 배지 업데이트를 보조 타일로 푸시하는 데 사용할 수 있습니다. 앱에서 사용자에 게 그룹 채팅 또는 업데이트 된 스포츠 점수의 새 메시지와 같이 앱에서 상호 작용할 수 있는 관심 있는 작업 또는 정보를 사용자에 게 알리는 데 사용 됩니다. 

### <a name="what-do-secondary-tile-channels-enable"></a>보조 타일 채널은 어떤 기능을 사용할 수 있나요?

-   타일 또는 배지 알림을 보조 타일로 보냅니다. 보조 타일은 사용자를 앱으로 다시 가져오는 좋은 방법입니다. 이러한 정보는 관심 있는 정보에 대 한 딥 링크 이며 타일에 대 한 관련 정보를 배치 하면 다시 다시 가져오는 데 도움이 됩니다.
-   여러 타일 간의 채널 (및 expiries) 분리. 이렇게 하면 사용자가 시작 화면에 고정할 수 있는 다양 한 유형의 보조 타일 사이에서 백 엔드의 논리를 구분할 수 있습니다. 
-   TLS를 사용 하 여 Windows에서 제공 하는 전송의 메시지 암호화. 메시지는 모두 WNS로 들어오고 사용자의 장치로 이동 하는 통신에서 암호화 됩니다.  

### <a name="limitations-of-secondary-tile-channels"></a>보조 타일 채널의 제한 사항

-   알림 또는 원시 알림은 허용 되지 않습니다. 보조 타일로 전송 된 알림 또는 원시 알림은 시스템에서 무시 됩니다.
-   앱이 Microsoft Store에 등록 되어 있어야 합니다.


### <a name="creating-a-secondary-tile-channel"></a>보조 타일 채널 만들기 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>대체 채널

대체 채널을 사용 하면 앱에 사용 되는 기본 채널 외부에서 푸시 채널을 만들거나 Microsoft Store에 등록 하지 않고도 앱이 푸시 알림을 보낼 수 있습니다. 
 
### <a name="what-do-alternate-channels-enable"></a>대체 채널은 어떤 기능을 사용할 수 있나요?
-   모든 Windows 장치에서 실행 되는 Windows에 원시 푸시 알림을 보냅니다. 대체 채널은 원시 알림만 허용 합니다 (하지만 로컬에서 알림 또는 타일 알림을 표시 하는 백그라운드 작업의 절전 모드를 해제할 수 있음).
-   앱이 앱 내에서 다양 한 기능에 대 한 여러 원시 푸시 채널을 만들 수 있습니다. 앱은 최대 1000 개의 대체 채널을 만들 수 있으며 각 채널은 30 일 동안 유효 합니다. 이러한 각 채널은 앱에서 별도로 관리 하거나 해지할 수 있습니다.
-   Microsoft Store를 사용 하 여 앱을 등록 하지 않고도 대체 푸시 채널을 만들 수 있습니다. Microsoft Store에 등록 하지 않고 장치에 앱을 설치 하려는 경우에도 푸시 알림을 받을 수 있습니다.
-   서버는 W3C 표준 REST Api 및 VAPID 프로토콜을 사용 하 여 알림을 푸시할 수 있습니다. 대체 채널은 W3C 표준 프로토콜을 사용 하므로 유지 관리 해야 하는 서버 논리를 단순화할 수 있습니다.
-   전체, 종단 간, 메시지 암호화 전송 중에 기본 채널이 암호화를 제공 하는 반면, 추가 보안을 유지 하려는 경우에는 응용 프로그램이 암호화 헤더를 통과 하 여 메시지를 보호 하는 데 사용할 수 있는 대체 채널이 있습니다. 

### <a name="limitations-of-alternate-channels"></a>대체 채널의 제한 사항
-   앱 서버에서 푸시 알림, 타일 또는 배지 유형 알림을 보낼 수 없습니다. 푸시 원시 알림만 보낼 수 있습니다. 앱은 여전히 백그라운드 작업에서 로컬 알림을 보낼 수 있습니다. 
-   기본 또는 보조 타일 채널과는 다른 REST API 필요 합니다. 표준 W3C REST API를 사용 하면 앱에 푸시 알림 또는 타일 업데이트를 전송 하는 데 다른 논리가 있어야 함을 의미 합니다.

### <a name="creating-an-alternate-channel"></a>대체 채널 만들기 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>채널 형식 비교
다음은 다양 한 유형의 채널을 빠르게 비교 하는 것입니다.

<table>

<tr class="header">
<th align="left"><b>유형</b></th>
<th align="left"><b>푸시 알림</b></th>
<th align="left"><b>타일/배지 푸시</b></th>
<th align="left"><b>원시 알림 푸시</b></th>
<th align="left"><b>인증</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>스토어 등록이 필요 한가요?</b></th>
<th align="left"><b>채널</b></th>
<th align="left"><b>암호화</b></th>
</tr>


<tr class="odd">
<td align="left">기본</td>
<td align="left">예</td>
<td align="left">예-기본 타일에만 해당</td>
<td align="left">예</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">예</td>
<td align="left">앱 당 1 개</td>
<td align="left">전송 중</td>
</tr>
<tr class="even">
<td align="left">보조 타일</td>
<td align="left">아니요</td>
<td align="left">예-보조 타일에만 해당</td>
<td align="left">아니요</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">예</td>
<td align="left">보조 타일 당 하나</td>
<td align="left">전송 중</td>
</tr>
<tr class="odd">
<td align="left">또</td>
<td align="left">아니요</td>
<td align="left">아니요</td>
<td align="left">예</td>
<td align="left">VAPID</td>
<td align="left">WebPush W3C Standard</td>
<td align="left">아니요</td>
<td align="left">앱 당 1000</td>
<td align="left">전송 중-헤더 통과를 사용 하 여 종단 간 암호화 가능 (앱 코드 필요)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>오른쪽 채널 선택

일반적으로 몇 가지 예외를 제외 하 고 앱에서 기본 채널을 사용 하는 것이 좋습니다. 

1. 타일 업데이트를 보조 타일로 푸시하는 경우 보조 타일 푸시 채널을 사용 합니다.
2. 다른 서비스 (예: 브라우저의 경우)에 채널을 전달 하는 경우 대체 채널을 사용 합니다.
3. Windows 스토어에 나열 되지 않는 앱을 만드는 경우 (예: LOB 앱) 대체 채널을 사용 합니다.
4. 서버에 기존 웹 푸시 코드를 다시 사용 하거나 백엔드 서비스에 여러 채널이 필요한 경우 대체 채널을 사용 합니다.

## <a name="related-articles"></a>관련된 문서

* [로컬 타일 알림 보내기](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [적응형 및 대화형 알림 메시지](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [빠른 시작: 푸시 알림 보내기](/previous-versions/windows/apps/hh868252(v=win.10))
* [푸시 알림을 통해 배지를 업데이트 하는 방법](/previous-versions/windows/apps/hh465450(v=win.10))
* [알림 채널을 요청, 생성 및 저장 하는 방법](/previous-versions/windows/apps/hh465412(v=win.10))
* [실행 중인 응용 프로그램에 대 한 알림을 가로채는 방법](/previous-versions/windows/apps/hh465450(v=win.10))
* [WNS (Windows 푸시 알림 서비스)를 사용 하 여 인증 하는 방법](/previous-versions/windows/apps/hh465407(v=win.10))
* [푸시 알림 서비스 요청 및 응답 헤더](/previous-versions/windows/apps/hh465435(v=win.10))
* [푸시 알림에 대 한 지침 및 검사 목록](./windows-push-notification-services--wns--overview.md)
* [원시 알림](raw-notification-overview.md)
