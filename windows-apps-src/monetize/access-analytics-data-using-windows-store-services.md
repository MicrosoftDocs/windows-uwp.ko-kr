---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Microsoft Store 분석 API를 사용하여 프로그래밍 방식으로 사용자 또는 사용자 조직의 Windows 개발자 센터 계정에 등록된 앱에 대한 분석 데이터를 검색합니다.
title: 스토어 서비스를 사용하여 분석 데이터에 액세스
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API
ms.localizationpriority: medium
ms.openlocfilehash: 26bed64053e8de9a42ac01ed3262c7b0f41d1d42
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4058387"
---
# <a name="access-analytics-data-using-store-services"></a>스토어 서비스를 사용하여 분석 데이터에 액세스

*Microsoft Store 분석 API*를 사용하여 프로그래밍 방식으로 사용자 또는 사용자 조직의 Windows 개발자 센터 계정에 등록된 앱에 대한 분석 데이터를 검색합니다. 이 API를 사용하면 앱 및 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 구입, 오류, 앱 평점 및 리뷰에 대한 데이터를 검색할 수 있습니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
2.  Microsoft Store 분석 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 분석 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
3.  [Microsoft Store 분석 API를 호출](#call-the-windows-store-analytics-api)합니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>단계 1: Microsoft Store 분석 API를 사용하기 위한 필수 조건 완료

Microsoft Store 분석 API를 호출하는 코드를 작성하기 전에 다음과 같은 필수 조건을 완료했는지 확인합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 아니면 추가 요금 없이 [개발자 센터 내에서 Azure AD를 새로 만들 수 있습니다](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account).

* Azure AD 응용 프로그램을 개발자 센터 계정과 연결하고 응용 프로그램에 대한 테넌트 ID 및 클라이언트 ID를 검색하고 키를 생성해야 합니다. Azure AD 응용 프로그램은 Microsoft Store 분석 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.
    > [!NOTE]
    > 이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

Azure AD 응용 프로그램을 개발자 센터 계정에 연결하고 필요한 값을 검색하려면

1.  개발자 센터에서 [조직의 개발자 센터 계정을 조직의 Azure AD Directory와 연결](../publish/associate-azure-ad-with-dev-center.md)합니다.

2.  그런 다음 개발자 센터의 **계정 설정**의 **사용자** 페이지에서 개발자 센터 계정에 대한 분석 데이터에 액세스하는 데 사용할 앱 또는 서비스를 나타내는 [Azure AD 응용 프로그램을 추가](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-dev-center-account)합니다. 이 응용 프로그램에 **관리자** 역할을 할당하도록 합니다. 응용 프로그램이 아직 Azure AD 디렉터리에 존재하지 않으면 [개발자 센터에서 새 Azure AD 응용 프로그램을 만들](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account) 수 있습니다.

3.  **사용자** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [AD 응용 프로그램 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>단계 2: Azure AD 액세스 토큰 가져오기

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

POST URI의 *tenant\_id* 값, *client\_id* 및 *client\_secret* 매개 변수에는 이전 섹션의 개발자 센터에서 검색한 응용 프로그램의 테넌트 ID, 클라이언트 ID 및 키를 지정합니다. *resource* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>단계 3: Microsoft Store 분석 API 호출

Azure AD 액세스 토큰이 있으면 Microsoft Store 분석 API를 호출할 준비가 된 것입니다. 액세스 토큰을 각 메서드의 **Authorization** 헤더로 전달해야 합니다.

### <a name="methods-for-uwp-apps"></a>UWP 앱의 메서드

다음 분석 메서드를 개발자 센터의 UWP 앱에 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 취득, 변환, 설치 및 사용 |  <ul><li>[앱 취득 가져오기](get-app-acquisitions.md)</li><li>[앱 취득 깔때기 데이터 가져오기](get-acquisition-funnel-data.md)</li><li>[채널 별 앱 변환 가져오기](get-app-conversions-by-channel.md)</li><li>[추가 기능 취득 가져오기](get-in-app-acquisitions.md)</li><li>[구독 추가 기능 취득 가져오기](get-subscription-acquisitions.md)</li><li>[채널별 추가 기능 변환 가져오기](get-add-on-conversions-by-channel.md)</li><li>[앱 설치 받기](get-app-installs.md)</li><li>[일일 앱 사용 현황 가져오기](get-app-usage-daily.md)</li><li>[월별 앱 사용 현황 가져오기](get-app-usage-monthly.md)</li></ul> |
| 앱 오류 | <ul><li>[오류 보고 데이터 가져오기](get-error-reporting-data.md)</li><li>[앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md)</li><li>[앱에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[앱의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| 인 사이트 | <ul><li>[앱에 대한 정보 데이터 가져오기](get-insights-data-for-your-app.md)</li></ul>  |
| 평점 및 리뷰 | <ul><li>[앱 평점 가져오기](get-app-ratings.md)</li><li>[앱 리뷰 가져오기](get-app-reviews.md)</li></ul> |
| 앱 내 광고 및 광고 캠페인 | <ul><li>[광고 성과 데이터 가져오기](get-ad-performance-data.md)</li><li>[광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>데스크톱 응용 프로그램의 메서드

[Windows 데스크톱 응용 프로그램 프로그램](https://msdn.microsoft.com/library/windows/desktop/mt826504)에 속하는 개발자 계정으로 다음과 같은 분석 메서드를 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 설치 |  <ul><li>[데스크톱 응용 프로그램 설치 가져오기](get-desktop-app-installs.md)</li></ul> |
| 블록 |  <ul><li>[데스크톱 응용 프로그램에 대한 업그레이드 차단 가져오기](get-desktop-block-data.md)</li><li>[데스크톱 응용 프로그램에 대한 업그레이드 차단 세부 정보 가져오기](get-desktop-block-data-details.md)</li></ul> |
| 응용 프로그램 오류 |  <ul><li>[데스크톱 응용 프로그램에 대한 오류 보고 데이터 가져오기](get-desktop-application-error-reporting-data.md)</li><li>[데스크톱 응용 프로그램에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)</li><li>[데스크톱 응용 프로그램에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[데스크톱 응용 프로그램의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| 인 사이트 | <ul><li>[데스크톱 응용 프로그램에 대한 정보 데이터 가져오기](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Xbox Live 서비스의 메서드

다음 추가 메서드는 [Xbox Live 서비스](../xbox-live/developer-program-overview.md)를 사용하는 게임의 개발자 계정에서 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 일반 분석 |  <ul><li>[Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)</li><li>[Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)</li><li>[Xbox Live 동시 사용 데이터 가져오기](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| 상태 분석 |  <ul><li>[Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)</li></ul> |
| 커뮤니티 분석 |  <ul><li>[Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)</li><li>[Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)</li><li>[Xbox Live 멀티플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Xbox One 게임의 메서드

다음 추가 메서드는 Xbox 개발자 포털(XDP)을 통해 수집된 Xbox One 게임의 개발자 계정에서 사용할 수 있으며 XDP 분석 개발자 센터 대시보드에서도 사용할 수 있습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 취득 |  <ul><li>[Xbox One 게임 취득 가져오기](get-xbox-one-game-acquisitions.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>하드웨어 및 드라이버의 메서드

[Windows 하드웨어 개발자 센터 프로그램](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) 에 속하는 개발자 계정에는 추가 하드웨어 및 드라이버에 대 한 분석 데이터를 검색 하기 위한 메서드 집합에 액세스할을 수 있습니다. 자세한 내용은 [하드웨어 대시보드 API를](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api)참조 하세요.

## <a name="code-example"></a>코드 예제

다음 코드 예제는 Azure AD 액세스 토큰을 얻고 C# 콘솔 앱에서 Microsoft Store 분석 API를 호출하는 방법을 보여 줍니다. 이 코드 예제를 사용하려면 시나리오에 맞는 적절한 값을 *tenantId*, *clientId*, *clientSecret* 및 *appID* 변수에 할당합니다. 이 예제에서 Microsoft Store 분석 API가 반환한 JSON 데이터를 역직렬화하려면 Newtonsoft의 [Json.NET 패키지](http://www.newtonsoft.com/json)가 필요합니다.

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
