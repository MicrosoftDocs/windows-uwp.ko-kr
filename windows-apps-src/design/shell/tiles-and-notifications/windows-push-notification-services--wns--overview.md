---
description: 타사 개발자는 WNS(Windows 푸시 알림 서비스)를 사용하여 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. WNS는 에너지 효율적이며 신뢰할 수 있는 방법으로 사용자에게 새 업데이트를 전달하는 메커니즘을 제공합니다.
title: WNS(Windows 푸시 알림 서비스) 개요
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 03/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bd910c42743577a83491386f5c667dd09722ba9b
ms.sourcegitcommit: 8171695ade04a762f19723f0b88e46e407375800
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/05/2020
ms.locfileid: "89494379"
---
# <a name="windows-push-notification-services-wns-overview"></a>WNS(Windows 푸시 알림 서비스) 개요 

WNS (Windows Push Notification Services)를 사용 하면 타사 개발자가 자신의 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. WNS는 에너지 효율적이며 신뢰할 수 있는 방법으로 사용자에게 새 업데이트를 전달하는 메커니즘을 제공합니다.

## <a name="how-it-works"></a>작동 방식

다음 다이어그램은 푸시 알림을 보내기 위한 전체 데이터 흐름을 보여 줍니다. 여기에는 다음 단계가 포함 됩니다.

1.  앱이 WNS에서 푸시 알림 채널을 요청 합니다.
2.  Windows에서 알림 채널을 만들도록 WNS를 요청 합니다. 이 채널은 URI (Uniform Resource Identifier) 형식으로 호출 장치에 반환 됩니다.
3.  알림 채널 URI는 WNS에서 앱으로 반환 됩니다.
4.  앱은 자신의 클라우드 서비스에 URI를 보냅니다. 그런 다음 사용자의 클라우드 서비스에 URI를 저장 하 여 알림을 보낼 때 URI에 액세스할 수 있도록 합니다. URI는 앱과 고유 서비스 간의 인터페이스입니다. 안전한 보안 웹 표준을 사용하여 이 인터페이스를 구현하는 것은 사용자의 책임입니다.
5.  클라우드 서비스에 보낼 업데이트가 있으면 채널 URI를 사용 하 여 WNS에 알립니다. 이 작업은 알림 페이로드를 비롯 하 여 SSL(Secure Sockets Layer) (SSL)를 통해 HTTP POST 요청을 실행 하 여 수행 됩니다. 이 단계에는 인증이 필요 합니다.
6.  WNS에서 요청을 수신 하 고 해당 장치에 알림을 라우팅합니다.

![푸시 알림에 대 한 wns 데이터 흐름 다이어그램](images/wns-diagram-01.jpg)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>앱 등록 및 클라우드 서비스에 대 한 자격 증명 수신

WNS를 사용 하 여 알림을 보내려면 먼저 스토어 대시보드에 앱을 등록 해야 합니다. 

각 앱에는 클라우드 서비스에 대 한 자체 자격 증명 집합이 있습니다. 이러한 자격 증명을 사용 하 여 다른 앱에 알림을 보낼 수는 없습니다.

### <a name="step-1-register-your-app-with-the-dashboard"></a>1 단계: 대시보드에 앱 등록

WNS를 통해 알림을 보내려면 앱을 파트너 센터 대시보드에 등록 해야 합니다. 그러면 클라우드 서비스에서 WNS로 인증 하는 데 사용할 앱에 대 한 자격 증명을 제공 합니다. 이러한 자격 증명은 패키지 SID (보안 식별자) 및 비밀 키로 구성 됩니다. 이 등록을 수행 하려면 [파트너 센터](https://partner.microsoft.com/dashboard)에 로그인 합니다. 앱을 만든 후 자격 증명을 검색 하는 방법에 대 한 [제품 관리-WNS/MPNS](https://apps.dev.microsoft.com/) for instrunctions을 참조 하세요 (live services 솔루션을 사용 하려는 경우이 페이지의 **live services 사이트** 링크를 따름).

등록하려면 다음을 수행합니다.
1.    파트너 센터의 Windows 스토어 앱 페이지로 이동 하 여 개인 Microsoft 계정 (예: johndoe@outlook.com ,)로 로그인 janedoe@xboxlive.com 합니다.
2.    로그인 했으면 대시보드 링크를 클릭 합니다.
3.    대시보드에서 새 앱 만들기를 선택 합니다.

![wns 앱 등록](../images/wns-create-new-app.png)

4.    앱 이름을 예약 하 여 앱을 만듭니다. 앱에 고유한 이름을 제공 합니다. 이름을 입력 하 고 제품 이름 예약 단추를 클릭 합니다. 이름을 사용할 수 있는 경우 앱 용으로 예약 되어 있습니다. 앱 이름을 성공적으로 예약한 후에는 다른 세부 정보를 수정할 수 있게 됩니다.

![wns 예약 제품 이름](../images/wns-reserve-poduct-name.png)
 
### <a name="step-2-obtain-the-identity-values-and-credentials-for-your-app"></a>2 단계: 앱에 대 한 id 값 및 자격 증명 가져오기

앱의 이름을 예약 하면 Windows 스토어에서 연결 된 자격 증명을 만들었습니다. 또한 응용 프로그램의 매니페스트 파일 (appxmanifest.xml)에 있어야 하는 연결 된 id 값 (이름 및 게시자)을 할당 했습니다. Windows 스토어에 앱을 이미 업로드 한 경우 이러한 값이 매니페스트에 자동으로 추가 됩니다. 앱을 업로드 하지 않은 경우에는 매니페스트에 id 값을 수동으로 추가 해야 합니다.

1.    제품 관리 드롭다운 화살표를 선택 합니다.

![wns 제품 관리](../images/wns-product-management.png)

2.    제품 관리 드롭다운에서 WNS/MPNS 링크를 선택 합니다.

![wns 제품 관리 continuted](../images/wns-product-management2.png)
 
3.    WNS/MPNS 페이지에서 Windows 푸시 Notification Services (WNS) 및 Microsoft Azure Mobile Services 섹션 아래에 있는 라이브 서비스 사이트 링크를 클릭 합니다.

![wns 라이브 서비스](../images/wns-live-services-page.png)
 
4.    응용 프로그램 등록 포털 (이전에는 Live Services 페이지) 페이지에서 앱의 매니페스트에 포함할 identity 요소를 제공 합니다. 여기에는 앱 암호, 패키지 보안 식별자 및 응용 프로그램 Id가 포함 됩니다. 텍스트 편집기에서 매니페스트를 열고 페이지가 지시 하는 대로 해당 요소를 추가 합니다.    

> [!NOTE]
> AAD 계정으로 로그인 하는 경우 앱을 등록 한 Microsoft 계정 소유자에 게 문의 하 여 연결 된 앱 암호를 가져와야 합니다. 이 연락처를 찾는 데 도움이 필요 하면 화면의 오른쪽 위에 있는 기어를 클릭 한 다음 개발자 설정을 클릭 하면 해당 Microsoft 계정으로 앱을 만든 메일 주소가 표시 됩니다.
 
5.    클라우드 서버에 SID 및 클라이언트 암호를 업로드 합니다.

> [!Important]
> 클라우드 서비스에서 SID 및 클라이언트 암호를 안전 하 게 저장 하 고 액세스 해야 합니다. 이 정보를 공개 하거나 도용 하면 공격자가 권한 또는 정보 없이 사용자에 게 알림을 보낼 수 있습니다.

## <a name="requesting-a-notification-channel"></a>알림 채널 요청

푸시 알림을 받을 수 있는 앱이 실행 되는 경우 [**CreatePushNotificationChannelForApplicationAsync**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_)를 통해 알림 채널을 먼저 요청 해야 합니다. 전체 토론 및 예제 코드는 [알림 채널을 요청 하 고 만들고 저장 하는 방법](/previous-versions/windows/apps/hh465412(v=win.10))을 참조 하세요. 이 API는 호출 하는 응용 프로그램과 해당 타일에 고유 하 게 연결 되 고 모든 알림 유형을 보낼 수 있는 채널 URI를 반환 합니다.

앱은 채널 URI를 성공적으로 만든 후이 URI와 연결 되어야 하는 앱 별 메타 데이터와 함께 클라우드 서비스로 보냅니다.

### <a name="important-notes"></a>중요

-   앱에 대 한 알림 채널 URI가 항상 동일 하 게 유지 된다는 것을 보장 하지는 않습니다. 앱이 실행 될 때마다 새 채널을 요청 하 고 URI가 변경 되 면 해당 서비스를 업데이트할 것을 권장 합니다. 개발자는 채널 URI를 수정할 수 없으며 블랙 박스 문자열로 간주 해야 합니다. 지금은 채널 Uri는 30 일 후에 만료 됩니다. Windows 10 앱이 백그라운드에서 주기적으로 해당 채널을 갱신 하는 경우 Windows 8.1에 대 한 [푸시 및 정기 알림 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) 을 다운로드 하 고 해당 소스 코드 및/또는이에 대해 설명 하는 패턴을 다시 사용할 수 있습니다.
-   클라우드 서비스와 클라이언트 앱 간의 인터페이스는 개발자에 의해 구현 됩니다. 앱이 자체 서비스를 사용 하 여 인증 프로세스를 진행 하 고 HTTPS와 같은 보안 프로토콜을 통해 데이터를 전송 하는 것이 좋습니다.
-   클라우드 서비스는 항상 채널 URI가 "notify.windows.com" 도메인을 사용 하는지 확인 하는 것이 중요 합니다. 서비스는 다른 도메인의 채널에 알림을 푸시 하지 않아야 합니다. 앱에 대 한 콜백이 손상 되는 경우 악의적인 공격자가 채널 URI를 전송 하 여 WNS를 스푸핑할 수 있습니다. 도메인을 검사 하지 않으면 클라우드 서비스는 잠재적으로이 공격자에 게 정보를 공개할 수 있습니다.
-   클라우드 서비스가 만료 된 채널에 알림을 배달 하려고 하면 WNS는 [응답 코드 410](/previous-versions/windows/apps/hh465435(v=win.10))을 반환 합니다. 해당 코드에 대 한 응답으로 서비스는 더 이상 해당 URI에 알림을 보내려고 시도 하지 않습니다.

## <a name="authenticating-your-cloud-service"></a>클라우드 서비스 인증

알림을 보내려면 WNS를 통해 클라우드 서비스를 인증 해야 합니다. 이 프로세스의 첫 번째 단계는 Microsoft Store 대시보드를 사용 하 여 앱을 등록할 때 발생 합니다. 등록 프로세스 중에 앱에는 패키지 SID (보안 식별자) 및 비밀 키가 제공 됩니다. 이 정보는 클라우드 서비스에서 WNS를 사용 하 여 인증 하는 데 사용 됩니다.

WNS 인증 체계는 [OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-v2-23) 프로토콜의 클라이언트 자격 증명 프로필을 사용 하 여 구현 됩니다. 클라우드 서비스는 자격 증명 (패키지 SID 및 비밀 키)을 제공 하 여 WNS를 사용 하 여 인증 합니다. 반환 되는 경우 액세스 토큰을 수신 합니다. 이 액세스 토큰을 사용 하면 클라우드 서비스에서 알림을 보낼 수 있습니다. 토큰은 WNS로 전송 되는 모든 알림 요청에 필요 합니다.

개략적인 수준에서 정보 체인은 다음과 같습니다.

1.  클라우드 서비스는 OAuth 2.0 프로토콜을 따라 HTTPS를 통해 WNS에 자격 증명을 보냅니다. 이는 WNS를 사용 하 여 서비스를 인증 합니다.
2.  인증에 성공 하면 WNS는 액세스 토큰을 반환 합니다. 이 액세스 토큰은 만료 될 때까지 이후의 모든 알림 요청에 사용 됩니다.

![클라우드 서비스 인증에 대 한 wns 다이어그램](images/wns-diagram-02.jpg)

WNS를 사용 하는 인증에서 클라우드 서비스는 SSL(Secure Sockets Layer) (SSL)을 통해 HTTP 요청을 제출 합니다. 매개 변수는 "application/x-www-form-urlencoded" 형식으로 제공 됩니다. \_다음 예제에 표시 된 것 처럼 "클라이언트 id" 필드에 패키지 SID를 제공 하 고 "클라이언트 암호" 필드에 비밀 키를 제공 \_ 합니다. 구문에 대 한 자세한 내용은 [액세스 토큰 요청](/previous-versions/windows/apps/hh465435(v=win.10)) 참조를 참조 하세요.

> [!NOTE]
> 이는 사용자의 코드에서 성공적으로 사용할 수 있는 잘라내기 및 붙여넣기 코드가 아닌 예제 일 뿐입니다. 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

WNS는 클라우드 서비스를 인증 하 고, 성공 하면 "200 OK" 응답을 보냅니다. 액세스 토큰은 "application/json" 미디어 유형을 사용 하 여 HTTP 응답 본문에 포함 된 매개 변수에 반환 됩니다. 서비스에서 액세스 토큰을 받은 후 알림을 보낼 준비가 된 것입니다.

다음 예에서는 액세스 토큰을 포함 하 여 성공적인 인증 응답을 보여 줍니다. 구문에 대 한 자세한 내용은 [푸시 알림 서비스 요청 및 응답 헤더](/previous-versions/windows/apps/hh465435(v=win.10))를 참조 하세요.

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

### <a name="important-notes"></a>중요

-   이 절차에서 지원 되는 OAuth 2.0 프로토콜은 draft 버전 V16를 따릅니다.
-   RFC (OAuth Request for Comments)는 "클라이언트" 라는 용어를 사용 하 여 클라우드 서비스를 참조 합니다.
-   OAuth 초안이 종료 되 면이 절차가 변경 될 수 있습니다.
-   여러 알림 요청에 대 한 액세스 토큰을 다시 사용할 수 있습니다. 이렇게 하면 클라우드 서비스에서 한 번만 인증 하 여 많은 알림을 보낼 수 있습니다. 그러나 액세스 토큰이 만료 되 면 클라우드 서비스는 새 액세스 토큰을 받기 위해 다시 인증 해야 합니다.

## <a name="sending-a-notification"></a>알림 보내기


클라우드 서비스는 채널 URI를 사용 하 여 사용자에 대 한 업데이트가 있을 때마다 알림을 보낼 수 있습니다.

위에서 설명한 액세스 토큰은 여러 알림 요청에 다시 사용할 수 있습니다. 클라우드 서버는 모든 알림에 대해 새 액세스 토큰을 요청 하는 데 필요 하지 않습니다. 액세스 토큰이 만료 되 면 알림 요청에서 오류를 반환 합니다. 액세스 토큰이 거부 된 경우 알림을 두 번 이상 다시 전송 하지 않는 것이 좋습니다. 이 오류가 발생 하는 경우 새 액세스 토큰을 요청 하 고 알림을 다시 전송 해야 합니다. 정확한 오류 코드는 [푸시 알림 응답 코드](/previous-versions/windows/apps/hh465435(v=win.10))를 참조 하세요.

1.  클라우드 서비스는 채널 URI에 HTTP POST를 수행 합니다. 이 요청은 SSL을 통해 수행 해야 하며 필요한 헤더와 알림 페이로드를 포함 합니다. 권한 부여 헤더는 권한 부여를 위해 획득 된 액세스 토큰을 포함 해야 합니다.

    예제 요청은 다음과 같습니다. 구문 정보는 [푸시 알림 응답 코드](/previous-versions/windows/apps/hh465435(v=win.10))를 참조하세요.

    알림 페이로드를 작성 하는 방법에 대 한 자세한 내용은 [빠른 시작: 푸시 알림 보내기](/previous-versions/windows/apps/hh868252(v=win.10))를 참조 하세요. 타일, 알림 또는 배지 푸시 알림의 페이로드는 정의 된 각 [적응 타일 스키마](adaptive-tiles-schema.md) 또는 [레거시 타일 스키마](/uwp/schemas/tiles/tiles-xml-schema-portal)를 준수 하는 XML 콘텐츠로 제공 됩니다. 원시 알림의 페이로드에 지정 된 구조체가 없습니다. 엄격 하 게 앱을 정의 합니다.

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

2.  WNS는 알림이 수신 되었으며 다음에 사용할 수 있는 기회에서 배달 됨을 나타내는 데 응답 합니다. 그러나 WNS는 장치 또는 응용 프로그램에서 알림을 수신 했다는 종단 간 확인을 제공 하지 않습니다.

다음 다이어그램은 데이터 흐름을 보여 줍니다.

![알림을 보내기 위한 wns 다이어그램](images/wns-diagram-03.jpg)

### <a name="important-notes"></a>중요

-   WNS는 알림의 안정성 또는 대기 시간을 보장 하지 않습니다.
-   알림에는 기밀 데이터 나 중요 한 데이터가 포함 되어서는 안 됩니다.
-   알림을 보내려면 클라우드 서비스에서 먼저 WNS를 사용 하 여 인증 하 고 액세스 토큰을 받아야 합니다.
-   액세스 토큰을 사용 하면 클라우드 서비스에서 토큰이 생성 된 단일 앱에만 알림을 보낼 수 있습니다. 한 액세스 토큰을 사용 하 여 여러 앱에서 알림을 보낼 수는 없습니다. 따라서 클라우드 서비스에서 여러 앱을 지 원하는 경우 각 채널 URI에 알림을 푸시할 때 앱에 대 한 올바른 액세스 토큰을 제공 해야 합니다.
-   장치가 오프 라인 상태인 경우 기본적으로 WNS는 최대 5 개의 타일 알림 (큐를 사용 하도록 설정 된 경우), 각 채널 URI에 대 한 배지 알림 하나 및 원시 알림은 저장 하지 않습니다. 이 기본 캐싱 동작은 [X WNS 캐시 정책 헤더](/previous-versions/windows/apps/hh465435(v=win.10))를 통해 변경할 수 있습니다. 장치가 오프 라인인 경우에는 알림 알림이 저장 되지 않습니다.
-   알림 콘텐츠가 사용자에 게 개인 설정 된 시나리오에서 WNS는 클라우드 서비스에서 해당 업데이트를 수신 하는 즉시 전송 하도록 권장 합니다. 이 시나리오의 예로는 소셜 미디어 피드 업데이트, 인스턴트 통신 초대, 새 메시지 알림 또는 경고가 있습니다. 또는 동일한 일반 업데이트가 사용자의 많은 하위 집합에 자주 전달 되는 시나리오를 사용할 수 있습니다. 예: 날씨, 재고 및 뉴스 업데이트. WNS 지침은 이러한 업데이트의 빈도가 30 분 마다 최대 1이 되도록 지정 합니다. 최종 사용자 또는 WNS는 자주 사용 하지 않는 일상적인 업데이트를 확인할 수 있습니다.
-   Windows 알림 플랫폼은 소켓을 연결 하 고 정상 상태로 유지 하기 위해 WNS와 주기적인 데이터 연결을 유지 관리 합니다. 알림 채널을 요청 하거나 사용 하는 응용 프로그램이 없으면 소켓이 생성 되지 않습니다.

## <a name="expiration-of-tile-and-badge-notifications"></a>타일 및 배지 알림 만료

기본적으로 타일 및 배지 알림은 다운로드 된 후 3 일 후에 만료 됩니다. 알림이 만료 되 면 타일 또는 큐에서 콘텐츠가 제거 되 고 사용자에 게 더 이상 표시 되지 않습니다. 타일 콘텐츠가 관련 된 것 보다 오래 지속 되지 않도록 모든 타일 및 배지 알림에서 만료 (앱에 맞는 시간 사용)를 설정 하는 것이 가장 좋습니다. 지정 된 수명의 콘텐츠에는 명시적 만료 시간이 필요 합니다. 이를 통해 클라우드 서비스에서 알림 전송을 중지 하거나 오랫동안 네트워크에서 연결을 끊는 경우에도 오래 된 콘텐츠가 제거 됩니다.

클라우드 서비스는 수신 된 후 알림이 유효한 상태로 유지 되는 시간 (초)을 지정 하 여 각 알림에 대해 만료를 설정할 수 있습니다. 자세한 내용은 [푸시 알림 서비스 요청 및 응답 헤더](/previous-versions/windows/apps/hh465435(v=win.10))를 참조 하세요.

예를 들어, 재고 시장의 활성 거래 시간 동안 주가 업데이트에 대 한 만료를 전송 간격의 두 배 (예: 1 시간 마다 알림을 보내는 경우 1 시간 후)로 설정할 수 있습니다. 또 다른 예로, 뉴스 앱은 일일 뉴스 타일 업데이트에 대해 하루에 적절 한 만료 시간을 결정할 수 있습니다.

## <a name="push-notifications-and-battery-saver"></a>푸시 알림 및 배터리 절약

배터리 절약 장치에서 백그라운드 작업을 제한 하 여 배터리 수명을 연장 합니다. Windows 10에서는 배터리가 지정 된 임계값 아래로 떨어지면 사용자가 배터리 보호기를 자동으로 켤 수 있습니다. 배터리 절약이 켜져 있으면 에너지를 절약 하기 위해 푸시 알림 수신이 사용 하지 않도록 설정 됩니다. 그러나이에 대 한 몇 가지 예외가 있습니다. 다음 Windows 10 배터리 보호기 설정 ( **설정** 앱에 있음)을 사용 하면 배터리 절약 모드가 설정 된 경우에도 앱이 푸시 알림을 받을 수 있습니다.

-   **배터리 절약 모드 동안 모든 앱에서 푸시 알림 허용**:이 설정을 사용 하면 배터리 절약 모드가 설정 되어 있는 동안 모든 앱에서 푸시 알림을 받을 수 있습니다. 이 설정은 데스크톱 버전의 Windows 10 (Home, Pro, Enterprise 및 교육용)에만 적용 됩니다.
-   **항상 허용**:이 설정을 사용 하면 배터리 보호기가 설정 되어 있는 동안 특정 앱이 백그라운드에서 실행 될 수 있습니다 (푸시 알림 수신 포함). 이 목록은 사용자가 수동으로 유지 관리 합니다.

이러한 두 설정의 상태를 확인할 수 있는 방법은 없지만 배터리 절약 상태를 확인 하는 방법은 없습니다. Windows 10에서는 [**EnergySaverStatus**](/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus) 속성을 사용 하 여 배터리 절약 상태를 확인 합니다. 앱은 [**EnergySaverStatusChanged**](/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged) 이벤트를 사용 하 여 배터리 절약에 대 한 변경 내용을 수신할 수도 있습니다.

앱이 푸시 알림에 크게 의존 하는 경우 배터리 보호기가 설정 되어 있는 동안 사용자에 게 알림을 수신 하지 못할 수 있음을 알리고 **배터리 절약 설정을**쉽게 조정할 수 있도록 하는 것이 좋습니다. Windows 10의 배터리 보호기 설정 URI 체계를 사용 하 여 `ms-settings:batterysaver-settings` 설정 앱에 대 한 편리한 링크를 제공할 수 있습니다.

> [!TIP]
> 사용자에 게 배터리 절약 시간 설정을 알리는 경우 나중에 메시지를 표시 하지 않도록 하는 방법을 제공 하는 것이 좋습니다. 예를 들어 `dontAskMeAgainBox` 다음 예제의 확인란은 [**LocalSettings**](/uwp/api/Windows.Storage.ApplicationData.LocalSettings)에서 사용자의 기본 설정을 유지 합니다.

다음은 Windows 10에서 배터리 절약이 켜져 있는지 여부를 확인 하는 방법의 예입니다. 이 예제에서는 사용자에 게 알리고 설정 앱을 **배터리 절약 설정**으로 시작 합니다. 를 `dontAskAgainSetting` 사용 하면 사용자가 메시지를 다시 표시 하지 않으려면 메시지를 표시 하지 않을 수 있습니다.

```csharp
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

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.System.h>
#include <winrt/Windows.System.Power.h>
#include <winrt/Windows.UI.Xaml.h>
#include <winrt/Windows.UI.Xaml.Controls.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Storage;
using namespace winrt::Windows::System;
using namespace winrt::Windows::System::Power;
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Controls;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
winrt::fire_and_forget CheckForEnergySaving()
{
    // Get reminder preference from LocalSettings.
    bool dontAskAgain{ false };
    auto localSettings = ApplicationData::Current().LocalSettings();
    IInspectable dontAskSetting = localSettings.Values().Lookup(L"dontAskAgainSetting");
    if (!dontAskSetting)
    {
        // Setting doesn't exist.
        dontAskAgain = false;
    }
    else
    {
        // Retrieve setting value
        dontAskAgain = winrt::unbox_value<bool>(dontAskSetting);
    }

    // Check whether battery saver is on, and whether it's okay to raise dialog.
    if ((PowerManager::EnergySaverStatus() == EnergySaverStatus::On) && (!dontAskAgain))
    {
        // Check dialog results.
        ContentDialogResult dialogResult = co_await saveEnergyDialog().ShowAsync();
        if (dialogResult == ContentDialogResult::Primary)
        {
            // Launch battery saver settings
            // (settings are available only when a battery is present).
            co_await Launcher::LaunchUriAsync(Uri(L"ms-settings:batterysaver-settings"));
        }

        // Save reminder preference.
        if (dontAskAgainBox().IsChecked())
        {
            // Don't raise the dialog again.
            localSettings.Values().Insert(L"dontAskAgainSetting", winrt::box_value(true));
        }
    }
}
```

이 예제에서 제공 되는 [**Contentdialog**](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) 의 XAML입니다.

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
* [빠른 시작: 푸시 알림 보내기](/previous-versions/windows/apps/hh868252(v=win.10))
* [푸시 알림을 통해 배지를 업데이트 하는 방법](/previous-versions/windows/apps/hh465450(v=win.10))
* [알림 채널을 요청, 생성 및 저장 하는 방법](/previous-versions/windows/apps/hh465412(v=win.10))
* [실행 중인 응용 프로그램에 대 한 알림을 가로채는 방법](/previous-versions/windows/apps/jj709907(v=win.10))
* [WNS (Windows 푸시 알림 서비스)를 사용 하 여 인증 하는 방법](/previous-versions/windows/apps/hh465407(v=win.10))
* [푸시 알림 서비스 요청 및 응답 헤더](/previous-versions/windows/apps/hh465435(v=win.10))
* [푸시 알림에 대 한 지침 및 검사 목록]()
* [원시 알림](/previous-versions/windows/apps/hh761488(v=win.10))