---
Description: 타사 개발자는 WNS(Windows 푸시 알림 서비스)를 사용하여 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. WNS는 에너지 효율적이며 신뢰할 수 있는 방법으로 사용자에게 새 업데이트를 전달하는 메커니즘을 제공합니다.
title: WNS(Windows 푸시 알림 서비스) 개요
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1f53dd0538e4564c50fb5cbcb6986f5cf9661cae
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463826"
---
# <a name="windows-push-notification-services-wns-overview"></a>WNS(Windows 푸시 알림 서비스) 개요
 

WNS (Windows Push Notification Services)를 사용 하면 타사 개발자가 자신의 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. WNS는 에너지 효율적이며 신뢰할 수 있는 방법으로 사용자에게 새 업데이트를 전달하는 메커니즘을 제공합니다.

## <a name="how-it-works"></a>작동 방법


다음 다이어그램은 푸시 알림 보내기를 위한 전체 데이터 흐름을 보여 줍니다. 이 작업은 다음 단계로 이루어져 있습니다.

1.  앱이 WNS에서 푸시 알림 채널을 요청 합니다.
2.  Windows가 WNS에 알림 채널을 만들도록 요청합니다. 이 채널은 URI(Uniform Resource Identifier) 형태로 호출 디바이스에 반환됩니다.
3.  알림 채널 URI는 WNS에서 앱으로 반환 됩니다.
4.  앱은 이 URI를 고유 클라우드 서비스로 보냅니다. 알림을 보낼 때 URI를 액세스할 수 있도록 고유 클라우드 서비스에 URI를 저장합니다. URI는 앱과 고유 서비스 간의 인터페이스입니다. 안전한 보안 웹 표준을 사용하여 이 인터페이스를 구현하는 것은 사용자의 책임입니다.
5.  클라우드 서비스는 보낼 업데이트가 있는 경우 채널 URI를 사용하여 WNS에 알립니다. SSL(Secure Sockets Layer)을 통해 알림 페이로드를 포함한 HTTP POST 요청을 실행하는 방법으로 이 작업을 수행합니다. 이 단계에는 인증이 필요합니다.
6.  WNS는 요청을 받아 적절한 디바이스로 알림을 라우트합니다.

![푸시 알림에 대한 WNS 데이터 흐름 다이어그램](images/wns-diagram-01.jpg)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>앱을 등록하고 클라우드 서비스용 자격 증명 받기


WNS를 사용하여 알림을 보내려면 먼저 스토어 대시보드에 앱을 등록해야 합니다. 이렇게 하면 클라우드 서비스가 WNS로 인증하는 데 사용할 자격 증명을 앱에 제공합니다. 이러한 자격 증명은 패키지 SID(보안 식별자)와 비밀 키로 구성됩니다. 이 등록을 수행 하려면 [파트너 센터](https://partner.microsoft.com/dashboard)에 로그인 합니다. 앱을 만든 후에는 **앱 관리 - WNS/MPNS** 페이지의 지침에 따라 자격 증명을 검색할 수 있습니다. Live Services 솔루션을 사용하려면 이 페이지의 **라이브 서비스 사이트** 링크를 따라 이동합니다.

각 앱에는 해당 클라우드 서비스에 사용할 고유 자격 증명 집합이 있습니다. 이 자격 증명은 다른 앱에 알림을 보내는 데 사용할 수 없습니다.

앱 등록 방법에 대한 자세한 내용은 [WNS(Windows 알림 서비스)를 사용하여 인증하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))을 참조하세요.

## <a name="requesting-a-notification-channel"></a>알림 채널 요청


푸시 알림을 받을 수 있는 앱이 실행되면 먼저 [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_)를 통해 알림 채널을 요청해야 합니다. 전체 설명 및 코드 예는 [알림 채널을 요청, 생성 및 저장하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))을 참조하세요. 이 API는 호출 응용 프로그램 및 해당 타일과 고유하게 연결되어 모든 알림 형식이 이를 통해 전송될 수 있는 채널 URI를 반환합니다.

앱이 채널 URI를 만든 후에는 이 URI와 관련되어야 하는 모든 앱 특정 메타데이터와 함께 채널 URI를 해당 클라우드 서비스로 보냅니다.

### <a name="important-notes"></a>중요 정보

-   앱의 알림 채널 URI가 항상 동일하게 유지된다고 보장하지 않습니다. 앱이 실행될 때마다 새 채널을 요청하고 URI 변경 시 해당 서비스를 업데이트하는 것이 좋습니다. 개발자는 채널 URI를 수정하지 않아야 하며 블랙 박스 문자열로 간주해야 합니다. 이때 채널 URI는 30일이 경과하면 만료됩니다. Windows 10 앱이 백그라운드에서 주기적으로 해당 채널을 갱신 하는 경우 Windows 8.1에 대 한 [푸시 및 정기 알림 샘플](https://code.msdn.microsoft.com/windowsapps/push-and-periodic-de225603) 을 다운로드 하 고 해당 소스 코드 및/또는이에 대해 설명 하는 패턴을 다시 사용할 수 있습니다.
-   클라우드 서비스와 클라이언트 앱 간의 인터페이스는 개발자가 구현합니다. 앱은 고유 서비스를 사용하여 인증 프로세스를 거치고 HTTPS 같은 보안 프로토콜을 통해 데이터를 전송하는 것이 좋습니다.
-   클라우드 서비스는 항상 채널 URI가 "notify.windows.com" 도메인을 사용하는지 확인합니다. 다른 도메인에서는 이 서비스가 알림을 채널로 푸시하지 않아야 합니다. 앱에 대한 콜백이 손상되는 경우 악의적인 공격자가 채널 URI를 제출하여 WNS를 스푸핑할 수 있습니다. 도메인을 검사하지 않으면 클라우드 서비스에서 자신도 모르게 이러한 공격자에게 정보를 공개할 수도 있습니다.
-   클라우드 서비스에서 만료된 채널에 알림을 전달하려고 시도하면 WNS에서 [응답 코드 410](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))을 반환합니다. 이 코드에 대처하려면 서비스에서 해당 URI에 더 이상 알림을 보내려고 시도하지 않아야 합니다.

## <a name="authenticating-your-cloud-service"></a>클라우드 서비스 인증


알림을 보내려면 WNS를 통해 클라우드 서비스를 인증해야 합니다. 이 프로세스의 첫 번째 단계는 Microsoft Store 대시보드를 사용하여 앱을 등록하는 것입니다. 등록 프로세스 중 패키지 SID(보안 식별자) 및 비밀 키를 앱에 제공합니다. 이 정보는 클라우드 서비스가 WNS로 인증하는 데 사용됩니다.

WNS 인증 체계는 [OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-v2-23) 프로토콜의 클라이언트 자격 증명 프로필을 사용하여 구현됩니다. 클라우드 서비스는 WNS를 통해 자격 증명(패키지 SID 및 비밀 키)을 제공하여 인증합니다. 그러면 액세스 토큰을 받습니다. 이러한 액세스 토큰을 통해 클라우드 서비스는 알림을 보낼 수 있습니다. WNS로 보내는 모든 알림 요청에는 이 토큰이 필요합니다.

정보 체인을 자세히 보면 다음과 같습니다.

1.  클라우드 서비스는 OAuth 2.0 프로토콜에 따라 HTTPS를 통해 WNS로 자격 증명을 보냅니다. 이렇게 하면 WNS에서 서비스가 인증됩니다.
2.  인증에 성공하면 WNS는 액세스 토큰을 반환합니다. 만료 시까지 이 액세스 토큰은 모든 후속 알림 요청에 사용됩니다.

![클라우드 서비스 인증에 대한 WNS 다이어그램](images/wns-diagram-02.jpg)

WNS를 통한 인증에서는 클라우드 서비스가 SSL(Secure Sockets Layer)을 통해 HTTP 요청을 제출합니다. 매개 변수는 "application/x-www-for-urlencoded" 형식으로 제공됩니다. "클라이언트\_id" 필드에 패키지 SID를 제공 하 고 "클라이언트\_비밀" 필드에 비밀 키를 제공 합니다. 구문 정보는 [액세스 토큰 요청](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)) 참조를 확인하세요.

이는 사용자의 코드에서 성공적으로 사용할 수 있는 잘라내기 및 붙여넣기 코드가 아닌 예제 일 뿐입니다 **.  **

 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

WNS는 클라우드 서비스를 인증하고 인증에 성공하면 "200 OK" 응답을 보냅니다. "application/json" 미디어 형식을 사용하여 HTTP 응답 본문에 포함된 매개 변수에 액세스 토큰이 반환됩니다. 서비스에서 액세스 토큰을 받으면 알림을 보낼 준비가 완료됩니다.

다음은 액세스 토큰을 포함하여 성공적인 인증 응답을 보여 주는 예입니다. 구문 정보는 [푸시 알림 서비스 요청 및 응답 헤더](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))를 참조하세요.

``` http
 HTTP/1.1 200 OK   
 Cache-Control: no-store
 Content-Length: 422
 Content-Type: application/json
 
 {
     "access_token":"EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=", 
     "token_type":"bearer"
 }
```

### <a name="important-notes"></a>중요 정보

-   이 프로시저에서 지원되는 OAuth 2.0 프로토콜은 초안 버전 V16을 따릅니다.
-   OAuth RFC(Request For Comments)에서는 클라우드 서비스를 가리키는 데 용어 "클라이언트"가 사용됩니다.
-   OAuth 초안이 마무리되면 이 프로시저에 대한 변경이 있을 수 있습니다.
-   액세스 토큰은 여러 알림 요청에 재사용될 수 있습니다. 따라서 클라우드 서비스는 한 번만 인증하면 여러 개의 알림을 보낼 수 있습니다. 그러나 액세스 토큰이 만료되면 클라우드 서비스가 다시 인증하여 새 액세스 토큰을 받아야 합니다.

## <a name="sending-a-notification"></a>알림 보내기


클라우드 서비스는 채널 URI를 사용하여 사용자용 업데이트가 있을 때마다 알림을 보낼 수 있습니다.

위에 설명된 액세스 토큰은 여러 알림 요청에 재사용할 수 있습니다. 클라우드 서버는 알림마다 새 액세스 토큰을 요청할 필요가 없습니다. 액세스 토큰이 만료되면 알림 요청이 오류를 반환합니다. 액세스 토큰이 거부되는 경우 알림을 두 번 이상 다시 보내려고 시도하지 않는 것이 좋습니다. 이러한 오류가 발생하면 새 액세스 토큰을 요청하여 알림을 다시 보내야 합니다. 정확한 오류 코드는 [푸시 알림 응답 코드](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))를 참조하세요.

1.  클라우드 서비스는 채널 URI로 HTTP POST를 수행합니다. 이러한 요청은 SSL을 통해 수행되어야 하며 필요한 헤더 및 알림 페이로드가 요청에 포함됩니다. 인증 헤더에는 인증을 위해 획득한 액세스 토큰이 포함되어야 합니다.

    요청 예는 다음과 같습니다. 구문 정보는 [푸시 알림 응답 코드](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))를 참조하세요.

    알림 페이로드를 작성하는 방법에 대한 자세한 내용은 [빠른 시작: 푸시 알림 보내기](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))를 참조하세요. 타일, 알림 메시지 또는 배지 푸시 알림의 페이로드는 정의된 해당 [적응형 타일 스키마](adaptive-tiles-schema.md) 또는 [레거시 타일 스키마](https://docs.microsoft.com/uwp/schemas/tiles/tiles-xml-schema-portal)를 따르는 XML 콘텐츠로 제공됩니다. 원시 알림의 페이로드에는 지정된 구조가 없습니다. 푸시 알림의 페이로드는 오로지 앱에서 정의됩니다.

    ``` http
     POST https://cloud.notify.windows.com/?token=AQE%bU%2fSjZOCvRjjpILow%3d%3d HTTP/1.1
     Content-Type: text/xml
     X-WNS-Type: wns/tile
     Authorization: Bearer EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=
     Host: cloud.notify.windows.com
     Content-Length: 24

     <body>
     ....
    ```

2.  WNS는 응답을 통해 알림을 받았으며 가능한 다음 기회에 알림을 전달할 것임을 알려줍니다. 그러나 WNS는 디바이스 또는 애플리케이션에서 알림을 받았는지에 대한 엔드투엔드 확인은 제공하지 않습니다.

이 다이어그램은 데이터 흐름을 보여 줍니다.

![알림 보내기를 위한 WNS 다이어그램](images/wns-diagram-03.jpg)

### <a name="important-notes"></a>중요 정보

-   WNS는 알림의 안정성 또는 대기 시간을 보장하지 않습니다.
-   알림에는 비밀 또는 중요한 데이터가 포함되지 않아야 합니다.
-   알림을 보내려면 클라우드 서비스는 먼저 WNS를 사용하여 인증하고 액세스 토큰을 받아야 합니다.
-   클라우드 서비스는 하나의 액세스 토큰을 사용하여 해당 토큰이 만들어진 대상인 하나의 앱으로만 알림을 보낼 수 있습니다. 하나의 액세스 토큰을 사용하여 여러 앱으로 알림을 보낼 수는 없습니다. 따라서 클라우드 서비스가 여러 앱을 지원하는 경우 각 채널 URI에 알림을 푸시할 때 해당 앱의 올바른 액세스 토큰을 제공해야 합니다.
-   디바이스가 오프라인이면 기본적으로 WNS는 각 채널 URI마다 최대 5개의 타일 알림(큐를 사용하는 경우, 사용하지 않는 경우에는 1개의 타일 알림)과 하나의 배지 알림을 저장하며 원시 알림은 저장하지 않습니다. 이 기본 캐싱 동작은 [X-WNS-Cache-Policy 헤더](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))를 통해 변경할 수 있습니다. 디바이스가 오프라인일 때는 알림 메시지가 저장되지 않습니다.
-   알림 콘텐츠가 해당 사용자에 맞게 개인 설정되어 있는 시나리오에서는 클라우드 서비스가 업데이트를 받는 즉시 보내도록 WNS에서 권장합니다. 이러한 시나리오의 예로는 소셜 미디어 피드 업데이트, 인스턴트 커뮤니케이션 초대, 새 메시지 알림 또는 경고가 있습니다. 또는 날씨, 주식, 뉴스 업데이트 같이 동일한 일반 업데이트가 대규모 사용자 하위 집합에 자주 전달되는 시나리오가 있을 수 있습니다. WNS 지침에는 이러한 업데이트의 빈도가 최대 30분마다 하나인 것으로 지정되어 있습니다. 최종 사용자 또는 WNS는 루틴 업데이트가 더 자주 사용되도록 정할 수 있습니다.

## <a name="expiration-of-tile-and-badge-notifications"></a>타일 및 배지 알림의 만료


기본적으로 타일 및 배지 알림은 다운로드된 후 3일이 경과하면 만료됩니다. 알림이 만료되면 콘텐츠가 타일 또는 큐에서 제거되고 더 이상 사용자에게 표시되지 않습니다. 앱에 적절한 시간을 사용하여 모든 타일 및 배지 알림에 대한 만료를 설정함으로써 타일의 콘텐츠를 관련이 있을 때까지만 유지하는 것이 좋습니다. 명시적 만료 시간은 수명이 정의되어 있는 콘텐츠에 필수적입니다. 이를 지정하면 클라우드 서비스에서 알림 보내기를 중지할 경우나 사용자의 네트워크 연결이 장기간 끊길 경우에 부실 콘텐츠도 제거됩니다.

클라우드 서비스는 X-WNS-TTL HTTP 헤더를 설정하여 알림의 전송 후 유효 기간을 지정함으로써 각 알림에 대해 만료를 설정할 수 있습니다. 자세한 내용은 [푸시 알림 서비스 요청 및 응답 헤더](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))를 참조하세요.

예를 들어 주식시장 거래일 중에 주가 업데이트 만료를 보내기 간격의 두 배(예제: 30분마다 알림을 보낼 경우 접수 1시간 후)로 설정할 수 있습니다. 다른 예로 뉴스 앱에서는 일간 뉴스 타일 업데이트에 적절한 만료 시간을 1일로 결정할 수 있습니다.

## <a name="push-notifications-and-battery-saver"></a>푸시 알림 및 배터리 절약 모드


배터리 절약 모드는 디바이스의 백그라운드 활동을 제한하여 배터리 사용 시간을 연장합니다. Windows 10에서는 배터리가 지정 된 임계값 아래로 떨어지면 사용자가 배터리 보호기를 자동으로 켤 수 있습니다. 배터리 절약 모드가 작동되면 에너지를 절약하기 위해 푸시 알림 받기가 사용되지 않습니다. 그러나 이 경우에는 몇 가지 예외가 있습니다. 다음 Windows 10 배터리 보호기 설정 ( **설정** 앱에 있음)을 사용 하면 배터리 절약 모드가 설정 된 경우에도 앱이 푸시 알림을 받을 수 있습니다.

-   **배터리 절약 모드에 있는 동안 앱에서 푸시 알림 허용**: 이 설정을 사용하면 배터리 절약 모드가 켜져 있는 동안 모든 앱에서 푸시 알림을 받을 수 있습니다. 이 설정은 데스크톱 버전의 Windows 10 (Home, Pro, Enterprise 및 교육용)에만 적용 됩니다.
-   **항상 허용**: 이 설정을 사용하면 배터리 절약 모드가 켜져 있는 동안 특정 앱이 백그라운드에서 실행할 수 있습니다(푸시 알림 받기 포함). 이 목록은 사용자가 수동으로 유지합니다.

이러한 두 설정의 상태를 확인할 수 있는 방법은 없지만 배터리 절약 모드의 상태는 확인할 수 있습니다. Windows 10에서는 [**EnergySaverStatus**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus) 속성을 사용 하 여 배터리 절약 상태를 확인 합니다. 앱에서 [**EnergySaverStatusChanged**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged) 이벤트를 사용하여 배터리 절약 모드의 변경에도 수신 대기할 수 있습니다.

앱에서 푸시 알림을 상당한 많이 받는 경우 사용자에게 배터리 절약 모드가 켜져 있는 동안 알림을 받을 수 없음을 알려 **배터리 절약 모드 설정**을 쉽게 조정할 수 있게 합니다. Windows 10의 배터리 보호기 설정 URI 체계를 사용 하 여 `ms-settings:batterysaver-settings`설정 앱에 대 한 편리한 링크를 제공할 수 있습니다.

**팁**   배터리 절약 설정을 사용자에 게 알릴 때 나중에 메시지를 표시 하지 않도록 하는 방법을 제공 하는 것이 좋습니다. 예를 들어, 다음 예제의 `dontAskMeAgainBox` 확인란에는 사용자가 [**LocalSettings**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalSettings)에 지정한 기본 설정을 유지합니다.

 

Windows 10에서 배터리 절약이 켜져 있는지 확인 하는 방법의 예는 다음과 같습니다. 이 예제에서는 설정 앱을 **배터리 절약 모드 설정**으로 시작하고 사용자에게 알립니다. 사용자가 다시 알림을 받고 싶지 않으면 `dontAskAgainSetting`를 사용하여 메시지를 표시하지 않을 수 있습니다.

```cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;
using Windows.System;
using Windows.System.Power;
...
...
async public void CheckForEnergySaving()
{
   //Get reminder preference from LocalSettings
   bool dontAskAgain;
   var localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
   object dontAskSetting = localSettings.Values["dontAskAgainSetting"];
   if (dontAskSetting == null)
   {  // Setting does not exist
      dontAskAgain = false;
   }
   else
   {  // Retrieve setting value
      dontAskAgain = Convert.ToBoolean(dontAskSetting);
   }
   
   // Check if battery saver is on and that it's okay to raise dialog
   if ((PowerManager.EnergySaverStatus == EnergySaverStatus.On)
         && (dontAskAgain == false))
   {
      // Check dialog results
      ContentDialogResult dialogResult = await saveEnergyDialog.ShowAsync();
      if (dialogResult == ContentDialogResult.Primary)
      {
         // Launch battery saver settings (settings are available only when a battery is present)
         await Launcher.LaunchUriAsync(new Uri("ms-settings:batterysaver-settings"));
      }

      // Save reminder preference
      if (dontAskAgainBox.IsChecked == true)
      {  // Don't raise dialog again
         localSettings.Values["dontAskAgainSetting"] = "true";
      }
   }
}
```

다음은 이 예제에 제공된 [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)의 XAML입니다.

```xaml
<ContentDialog x:Name="saveEnergyDialog"
               PrimaryButtonText="Open battery saver settings"
               SecondaryButtonText="Ignore"
               Title="Battery saver is on."> 
   <StackPanel>
      <TextBlock TextWrapping="WrapWholeWords">
         <LineBreak/><Run>Battery saver is on and you may 
          not receive push notifications.</Run><LineBreak/>
         <LineBreak/><Run>You can choose to allow this app to work normally
         while in battery saver, including receiving push notifications.</Run>
         <LineBreak/>
      </TextBlock>
      <CheckBox x:Name="dontAskAgainBox" Content="OK, got it."/>
   </StackPanel>
</ContentDialog>
```

## <a name="related-topics"></a>관련 항목


* [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)
* [빠른 시작: 푸시 알림 보내기](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [푸시 알림을 통해 배지를 업데이트 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465450(v=win.10))
* [알림 채널을 요청, 생성 및 저장 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [실행 중인 응용 프로그램에 대 한 알림을 가로채는 방법](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [WNS (Windows 푸시 알림 서비스)를 사용 하 여 인증 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [푸시 알림 서비스 요청 및 응답 헤더](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [푸시 알림에 대 한 지침 및 검사 목록](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [원시 알림](https://docs.microsoft.com/previous-versions/windows/apps/hh761488(v=win.10))
 

 




