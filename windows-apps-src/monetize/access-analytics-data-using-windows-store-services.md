---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use the Microsoft Store analytics API to programmatically retrieve analytics data for apps that are registered to your or your organization''s Windows Partner Center account.
title: 스토어 서비스를 사용하여 분석 데이터에 액세스
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 71c59049b76219d6f9360748e9ca11ea84542e47
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259313"
---
# <a name="access-analytics-data-using-store-services"></a>스토어 서비스를 사용하여 분석 데이터에 액세스

Use the *Microsoft Store analytics API* to programmatically retrieve analytics data for apps that are registered to your or your organization's Windows Partner Center account. 이 API를 사용하면 앱 및 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 구입, 오류, 앱 평점 및 리뷰에 대한 데이터를 검색할 수 있습니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
2.  Microsoft Store 분석 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 분석 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
3.  [Microsoft Store 분석 API를 호출](#call-the-windows-store-analytics-api)합니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>단계 1: Microsoft Store 분석 API를 사용하기 위한 필수 조건 완료

Microsoft Store 분석 API를 호출하는 코드를 작성하기 전에 다음과 같은 필수 조건을 완료했는지 확인합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. Otherwise, you can [create a new Azure AD in Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) for no additional charge.

* You must associate an Azure AD application with your Partner Center account, retrieve the tenant ID and client ID for the application and generate a key. Azure AD 응용 프로그램은 Microsoft Store 분석 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.
    > [!NOTE]
    > 이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

To associate an Azure AD application with your Partner Center account and retrieve the required values:

1.  In Partner Center, [associate your organization's Partner Center account with your organization's Azure AD directory](../publish/associate-azure-ad-with-partner-center.md).

2.  Next, from the **Users** page in the **Account settings** section of Partner Center, [add the Azure AD application](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) that represents the app or service that you will use to access analytics data for your Partner Center account. 이 응용 프로그램에 **관리자** 역할을 할당하도록 합니다. If the application doesn't exist yet in your Azure AD directory, you can [create a new Azure AD application in Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  **사용자** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [AD 응용 프로그램 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store 분석 API에서 메서드를 호출하기 전에 먼저 API에 있는 각 메서드의 **Authorization** 헤더에 전달하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 API에 대한 추가 호출에 계속 사용할 수 있도록 해당 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지침에 따라 HTTP POST를 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점에 보냅니다. 다음은 샘플 요청입니다.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

For the *tenant\_id* value in the POST URI and the *client\_id* and *client\_secret* parameters, specify the tenant ID, client ID and the key for your application that you retrieved from Partner Center in the previous section. *resource* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>단계 3: Microsoft Store 분석 API 호출

Azure AD 액세스 토큰이 있으면 Microsoft Store 분석 API를 호출할 준비가 된 것입니다. 액세스 토큰을 각 메서드의 **Authorization** 헤더로 전달해야 합니다.

### <a name="methods-for-uwp-apps-and-games"></a>Methods for UWP apps and games
The following methods are available for apps and games acquisitions and add-on acquisitions: 

* [Get acquisitions data for your games and apps](acquisitions-data.md)
* [Get add-on acquisitions data for your games and apps](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>UWP 앱의 메서드 

The following analytics methods are available for UWP apps in Partner Center.

| 시나리오       | 메서드      |
|---------------|--------------------|
| Acquisitions, conversions, installs, and usage |  <ul><li>[Get app acquisitions](get-app-acquisitions.md) (legacy)</li><li>[Get app acquisition funnel data](get-acquisition-funnel-data.md) (legacy)</li><li>[Get app conversions by channel](get-app-conversions-by-channel.md)</li><li>[Get add-on acquisitions](get-in-app-acquisitions.md)</li><li>[Get subscription add-on acquisitions](get-subscription-acquisitions.md)</li><li>[Get add-on conversions by channel](get-add-on-conversions-by-channel.md)</li><li>[Get app installs](get-app-installs.md)</li><li>[Get daily app usage](get-app-usage-daily.md)</li><li>[Get monthly app usage](get-app-usage-monthly.md)</li></ul> |
| 앱 오류 | <ul><li>[Get error reporting data](get-error-reporting-data.md)</li><li>[Get details for an error in your app](get-details-for-an-error-in-your-app.md)</li><li>[Get the stack trace for an error in your app](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Download the CAB file for an error in your app](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Get insights data for your app](get-insights-data-for-your-app.md)</li></ul>  |
| 평점 및 리뷰 | <ul><li>[Get app ratings](get-app-ratings.md)</li><li>[Get app reviews](get-app-reviews.md)</li></ul> |
| 앱 내 광고 및 광고 캠페인 | <ul><li>[Get ad performance data](get-ad-performance-data.md)</li><li>[Get ad campaign performance data](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>데스크톱 응용 프로그램의 메서드

[Windows 데스크톱 응용 프로그램 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)에 속하는 개발자 계정으로 다음과 같은 분석 메서드를 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 설치 |  <ul><li>[Get desktop application installs](get-desktop-app-installs.md)</li></ul> |
| Blocks |  <ul><li>[Get upgrade blocks for your desktop application](get-desktop-block-data.md)</li><li>[Get upgrade block details for your desktop application](get-desktop-block-data-details.md)</li></ul> |
| 응용 프로그램 오류 |  <ul><li>[Get error reporting data for your desktop application](get-desktop-application-error-reporting-data.md)</li><li>[Get details for an error in your desktop application](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Get the stack trace for an error in your desktop application](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Download the CAB file for an error in your desktop application](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Get insights data for your desktop application](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Xbox Live 서비스의 메서드

다음 추가 메서드는 [Xbox Live 서비스](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md)를 사용하는 게임의 개발자 계정에서 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 일반 분석 |  <ul><li>[Get Xbox Live analytics data](get-xbox-live-analytics.md)</li><li>[Get Xbox Live achievements data](get-xbox-live-achievements-data.md)</li><li>[Get Xbox Live concurrent usage data](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| 상태 분석 |  <ul><li>[Get Xbox Live health data](get-xbox-live-health-data.md)</li></ul> |
| 커뮤니티 분석 |  <ul><li>[Get Xbox Live Game Hub data](get-xbox-live-game-hub-data.md)</li><li>[Get Xbox Live club data](get-xbox-live-club-data.md)</li><li>[Get Xbox Live multiplayer data](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Xbox One 게임의 메서드

The following additional methods are available for use by developer accounts with Xbox One games that were ingested through the Xbox Developer Portal (XDP) and available in the XDP Analytics dashboard.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 취득 |  <ul><li>[Get Xbox One game acquisitions](get-xbox-one-game-acquisitions.md)</li><li>[Get Xbox One add-on acquisitions](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| 오류 |  <ul><li>[Get error reporting data for your Xbox One game](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Get details for an error in your Xbox One game](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Get the stack trace for an error in your Xbox One game](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Download the CAB file for an error in your Xbox One game](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>하드웨어 및 드라이버의 메서드

Developer accounts that belong to the [Windows hardware dashboard program](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) have access to an additional set of methods for retrieving analytics data for hardware and drivers. For more information, see [Hardware dashboard API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>코드 예제

다음 코드 예제는 Azure AD 액세스 토큰을 얻고 C# 콘솔 앱에서 Microsoft Store 분석 API를 호출하는 방법을 보여 줍니다. 이 코드 예제를 사용하려면 시나리오에 맞는 적절한 값을 *tenantId*, *clientId*, *clientSecret* 및 *appID* 변수에 할당합니다. 이 예제에서 Microsoft Store 분석 API가 반환한 JSON 데이터를 역직렬화하려면 Newtonsoft의 [Json.NET 패키지](https://www.newtonsoft.com/json)가 필요합니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

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
