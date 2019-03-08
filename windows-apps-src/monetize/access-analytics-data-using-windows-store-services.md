---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Microsoft Store 분석 API를 사용 하 여 프로그래밍 방식으로 또는 사용자의 조직에 등록 된 앱에 대 한 분석 데이터를 검색 하려면 ' s Windows 파트너 센터 계정.
title: 스토어 서비스를 사용하여 분석 데이터에 액세스
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 72e0941bb42a2a507af652758432ce51212c1042
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592658"
---
# <a name="access-analytics-data-using-store-services"></a>스토어 서비스를 사용하여 분석 데이터에 액세스

사용 된 *Microsoft Store 분석 API* 귀하나 귀하 조직의 Windows 파트너 센터 계정에 등록 된 앱에 대 한 분석 데이터를 프로그래밍 방식으로 검색할 합니다. 이 API를 사용하면 앱 및 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 구입, 오류, 앱 평점 및 리뷰에 대한 데이터를 검색할 수 있습니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
2.  Microsoft Store 분석 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 분석 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
3.  [Microsoft Store 분석 API를 호출](#call-the-windows-store-analytics-api)합니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>1단계: Microsoft Store 분석 API를 사용 하기 위한 필수 구성 요소를 완료 합니다.

Microsoft Store 분석 API를 호출하는 코드를 작성하기 전에 다음과 같은 필수 조건을 완료했는지 확인합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](https://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 수이 고, 그렇지 [파트너 센터에서 새 Azure AD 만들기](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 추가 요금 없이 합니다.

* 파트너 센터 계정을 사용 하 여 Azure AD 응용 프로그램을 연결 하 고, ID 및 클라이언트 응용 프로그램 ID를 테 넌 트를 검색 하 고, 키를 생성 해야 합니다. Azure AD 응용 프로그램은 Microsoft Store 분석 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.
    > [!NOTE]
    > 이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

파트너 센터 계정을 사용 하 여 Azure AD 응용 프로그램을 연결 및 필요한 값을 검색 합니다.

1.  파트너 센터에서 [조직의 Azure AD 디렉터리를 사용 하 여 조직의 파트너 센터 계정을 연결](../publish/associate-azure-ad-with-partner-center.md)합니다.

2.  다음으로, 합니다 **사용자** 페이지에 **계정 설정** 파트너 센터의 섹션 [Azure AD 응용 프로그램을 추가](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) 앱 또는 서비스를 사용 하 여 나타내는 파트너 센터 계정에 대 한 분석 데이터에 액세스 합니다. 이 응용 프로그램에 **관리자** 역할을 할당하도록 합니다. Azure AD 디렉터리에서 아직 응용 프로그램이 없는 경우 [새 파트너 센터에서 Azure AD 응용 프로그램](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)합니다.

3.  **사용자** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [AD 응용 프로그램 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store 분석 API에서 메서드를 호출하기 전에 먼저 API에 있는 각 메서드의 **Authorization** 헤더에 전달하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 API에 대한 추가 호출에 계속 사용할 수 있도록 해당 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지침에 따라 HTTP POST를 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점에 보냅니다. 다음은 샘플 요청입니다.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

에 대 한 합니다 *테 넌 트\_id* POST 값 및 *클라이언트\_id* 및 *클라이언트\_비밀* 매개 변수를 지정 된 테 넌 트 ID, 클라이언트 ID 및 이전 섹션에서 파트너 센터에서 검색 하는 응용 프로그램에 대 한 키입니다. *resource* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>3단계: Microsoft Store 분석 API를 호출 합니다.

Azure AD 액세스 토큰이 있으면 Microsoft Store 분석 API를 호출할 준비가 된 것입니다. 액세스 토큰을 각 메서드의 **Authorization** 헤더로 전달해야 합니다.

### <a name="methods-for-uwp-apps"></a>UWP 앱의 메서드

다음 analytics 메서드는 파트너 센터에서 UWP 앱에서 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 합병, 변환, 설치 및 사용 |  <ul><li>[인 앱 구매 가져오기](get-app-acquisitions.md)</li><li>[앱 취득 깔때기 데이터 가져오기](get-acquisition-funnel-data.md)</li><li>[앱 변환 채널에서 가져오기](get-app-conversions-by-channel.md)</li><li>[추가 기능 구매 가져오기](get-in-app-acquisitions.md)</li><li>[추가 기능 구매 구독 가져오기](get-subscription-acquisitions.md)</li><li>[채널에서 추가 기능 변환 가져오기](get-add-on-conversions-by-channel.md)</li><li>[앱 설치 가져오기](get-app-installs.md)</li><li>[앱의 일일 사용량 가져오기](get-app-usage-daily.md)</li><li>[월별 앱 사용량 가져오기](get-app-usage-monthly.md)</li></ul> |
| 앱 오류 | <ul><li>[오류 보고 데이터 가져오기](get-error-reporting-data.md)</li><li>[앱에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md)</li><li>[앱에서 오류에 대 한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[앱에서 오류에 대 한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[앱에 대 한 insights 데이터를 가져오기](get-insights-data-for-your-app.md)</li></ul>  |
| 평점 및 리뷰 | <ul><li>[앱 등급을 가져와서](get-app-ratings.md)</li><li>[앱 검토를 가져오기](get-app-reviews.md)</li></ul> |
| 앱 내 광고 및 광고 캠페인 | <ul><li>[Ad 성능 데이터 가져오기](get-ad-performance-data.md)</li><li>[광고 캠페인 성능 데이터 가져오기](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>데스크톱 응용 프로그램의 메서드

[Windows 데스크톱 응용 프로그램 프로그램](https://msdn.microsoft.com/library/windows/desktop/mt826504)에 속하는 개발자 계정으로 다음과 같은 분석 메서드를 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 설치 |  <ul><li>[데스크톱 응용 프로그램 설치 가져오기](get-desktop-app-installs.md)</li></ul> |
| 블록 |  <ul><li>[데스크톱 응용 프로그램에 대 한 업그레이드 요소 가져오기](get-desktop-block-data.md)</li><li>[데스크톱 응용 프로그램에 대 한 업그레이드 블록 세부 정보 가져오기](get-desktop-block-data-details.md)</li></ul> |
| 응용 프로그램 오류 |  <ul><li>[오류 보고 데스크톱 응용 프로그램에 대 한 데이터 가져오기](get-desktop-application-error-reporting-data.md)</li><li>[데스크톱 응용 프로그램에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)</li><li>[데스크톱 응용 프로그램에서 오류에 대 한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[데스크톱 응용 프로그램에서 오류에 대 한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[데스크톱 응용 프로그램에 대 한 insights 데이터를 가져오기](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Xbox Live 서비스의 메서드

다음 추가 메서드는 [Xbox Live 서비스](../xbox-live/developer-program-overview.md)를 사용하는 게임의 개발자 계정에서 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 일반 분석 |  <ul><li>[Xbox Live analytics 데이터 가져오기](get-xbox-live-analytics.md)</li><li>[Xbox Live 성과 데이터 가져오기](get-xbox-live-achievements-data.md)</li><li>[Xbox Live 동시 사용 현황 데이터 가져오기](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| 상태 분석 |  <ul><li>[Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)</li></ul> |
| 커뮤니티 분석 |  <ul><li>[Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)</li><li>[Xbox Live club 데이터 가져오기](get-xbox-live-club-data.md)</li><li>[Xbox Live 멀티 플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Xbox One 게임의 메서드

다음과 같은 추가 메서드는 Xbox 개발자 포털 (XDP)를 통해 수집 된 게임을 Xbox One을 사용 하 여 개발자 계정에서 사용할 수 있는 되며 XDP 분석 대시보드에서 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 구입 |  <ul><li>[Xbox One 게임 획득 가져오기](get-xbox-one-game-acquisitions.md)</li><li>[Xbox One 추가 기능 구매 가져오기](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| 오류 |  <ul><li>[오류가 Xbox One에 대 한 보고 데이터 게임](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[게임에 Xbox One에 오류에 대 한 세부 정보를 가져오기](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Xbox One에 오류에 대 한 스택 추적을 게임 가져오기](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Xbox One 게임에서 오류에 대 한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>하드웨어 및 드라이버의 메서드

개발자 계정에 속하는 합니다 [Windows 하드웨어 대시보드 프로그램](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) 추가 하드웨어 및 드라이버에 대 한 분석 데이터를 검색 하는 메서드 집합에 대 한 액세스 권한이 있습니다. 자세한 내용은 [Hardware dashboard API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api)합니다.

## <a name="code-example"></a>코드 예제

다음 코드 예제는 Azure AD 액세스 토큰을 얻고 C# 콘솔 앱에서 Microsoft Store 분석 API를 호출하는 방법을 보여 줍니다. 이 코드 예제를 사용하려면 시나리오에 맞는 적절한 값을 *tenantId*, *clientId*, *clientSecret* 및 *appID* 변수에 할당합니다. 이 예제에서 Microsoft Store 분석 API가 반환한 JSON 데이터를 역직렬화하려면 Newtonsoft의 [Json.NET 패키지](https://www.newtonsoft.com/json)가 필요합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>오류 응답

Microsoft Store 분석 API는 오류 코드와 메시지를 포함하는 오류 응답을 JSON 개체에 반환합니다. 다음 예제에서는 잘못된 매개 변수로 인해 발생한 오류 응답을 보여 줍니다.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```
