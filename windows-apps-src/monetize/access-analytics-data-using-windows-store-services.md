---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Microsoft Store analytics API를 사용 하 여 또는 조직의 Windows 파트너 센터 계정에 등록 된 앱에 대 한 분석 데이터를 프로그래밍 방식으로 검색할 수 있습니다.
title: 저장소 서비스를 사용 하 여 분석 데이터 액세스
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store analytics API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8becf9149d0afa888d0024619df06f2103c7bf8b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158647"
---
# <a name="access-analytics-data-using-store-services"></a>저장소 서비스를 사용 하 여 분석 데이터 액세스

*Microsoft Store ANALYTICS API* 를 사용 하 여 또는 조직의 Windows 파트너 센터 계정에 등록 된 앱에 대 한 분석 데이터를 프로그래밍 방식으로 검색할 수 있습니다. 이 API를 사용 하면 앱 및 추가 기능 (앱 내 제품이 나 IAP 라고도 함)의 데이터를 검색 하 고, 오류, 앱 등급 및 검토를 수행할 수 있습니다. 이 API는 Azure Active Directory (Azure AD)를 사용 하 여 앱 또는 서비스에서 호출을 인증 합니다.

다음 단계는 종단 간 프로세스를 설명 합니다.

1.  모든 [필수 구성 요소](#prerequisites)를 완료 했는지 확인 합니다.
2.  Microsoft Store analytics API에서 메서드를 호출 하기 전에 [AZURE AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 분석 API에 대 한 호출에서이 토큰을 사용 하는 데 60 분이 소요 됩니다. 토큰이 만료 된 후 새 토큰을 생성할 수 있습니다.
3.  [Microsoft Store ANALYTICS API를 호출](#call-the-windows-store-analytics-api)합니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>1 단계: Microsoft Store analytics API를 사용 하기 위한 필수 구성 요소 완료

Microsoft Store analytics API를 호출 하는 코드 작성을 시작 하기 전에 다음 필수 구성 요소를 완료 했는지 확인 합니다.

* 사용자(또는 조직)는 Azure AD 디렉터리가 있어야 하고 디렉터리에 대한 [전역 관리자](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 권한이 있어야 합니다. Microsoft에서 이미 Microsoft 365 또는 다른 비즈니스 서비스를 사용 하는 경우 Azure AD 디렉터리가 이미 있습니다. 그렇지 않으면 추가 비용 없이 [파트너 센터에서 새 AZURE AD를 만들](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 수 있습니다.

* Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고, 응용 프로그램에 대 한 테 넌 트 ID와 클라이언트 ID를 검색 하 고, 키를 생성 해야 합니다. Azure AD 응용 프로그램은 Microsoft Store analytics API를 호출 하려는 응용 프로그램 또는 서비스를 나타냅니다. API에 전달하는 Azure AD 액세스 토큰을 얻으려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.
    > [!NOTE]
    > 이 작업은 한 번만 수행 하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키가 있으면 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.

Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고 필요한 값을 검색 하려면 다음을 수행 합니다.

1.  파트너 센터에서 [조직의 파트너 센터 계정을 조직의 Azure AD 디렉터리에 연결합니다](../publish/associate-azure-ad-with-partner-center.md).

2.  그런 다음 파트너 센터의 **계정 설정** 섹션에 있는 **사용자** 페이지에서 파트너 센터 계정에 대 한 분석 데이터에 액세스 하는 데 사용할 앱 또는 서비스를 나타내는 [Azure AD 응용 프로그램을 추가](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) 합니다. 이 애플리케이션을 **관리자** 역할에 할당해야 합니다. 애플리케이션이 아직 Azure AD 디렉터리에 존재하지 않는 경우 [파트너 센터에서 새 Azure AD 애플리케이션 만들 수 있습니다](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  **사용자** 페이지로 돌아가 Azure AD 애플리케이션의 이름을 클릭하여 애플리케이션 설정으로 이동한 다음 **테넌트 ID**와 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 나가면 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [Azure AD 애플리케이션 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store analytics API에서 메서드를 호출 하기 전에 먼저 API의 각 메서드에 대 한 **인증** 헤더에 전달 하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후에는 API에 대 한 추가 호출에서 토큰을 계속 사용할 수 있도록 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용 하 여 서비스 호출](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) 에 대 한 지침에 따라 HTTP POST를 끝점으로 보냅니다 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` . 샘플 요청은 다음과 같습니다.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

POST URI와 클라이언트 * \_ id* 및 *클라이언트 \_ 암호* 매개 변수의 *테 넌 트 \_ Id* 값에 대해 이전 섹션의 파트너 센터에서 검색 한 응용 프로그램의 테 넌 트 id, 클라이언트 id 및 키를 지정 합니다. *리소스* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

액세스 토큰이 만료 되 면 [여기](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens)에 설명 된 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>3 단계: Microsoft Store 분석 API 호출

Azure AD 액세스 토큰이 있으면 Microsoft Store analytics API를 호출할 준비가 된 것입니다. 각 메서드의 **인증** 헤더에 액세스 토큰을 전달 해야 합니다.

### <a name="methods-for-uwp-apps-and-games"></a>UWP 앱 및 게임 방법
앱 및 게임 합병 및 추가 기능에는 다음 메서드를 사용할 수 있습니다. 

* [게임 및 앱에 대한 취득 데이터 가져오기](acquisitions-data.md)
* [게임 및 앱에 대한 추가 기능 취득 데이터 가져오기](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>UWP 앱에 대 한 메서드 

파트너 센터의 UWP 앱에서 사용할 수 있는 분석 메서드는 다음과 같습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 인수, 변환, 설치 및 사용 |  <ul><li>[앱 인수 가져오기](get-app-acquisitions.md) (레거시)</li><li>[앱 취득 깔때기 데이터 가져오기](get-acquisition-funnel-data.md) (레거시)</li><li>[채널별 앱 변환 가져오기](get-app-conversions-by-channel.md)</li><li>[추가 기능 구입 가져오기](get-in-app-acquisitions.md)</li><li>[구독 추가 기능 취득 가져오기](get-subscription-acquisitions.md)</li><li>[채널별 추가 기능 변환 가져오기](get-add-on-conversions-by-channel.md)</li><li>[앱 설치 가져오기](get-app-installs.md)</li><li>[일일 앱 사용 현황 가져오기](get-app-usage-daily.md)</li><li>[월별 앱 사용 현황 가져오기](get-app-usage-monthly.md)</li></ul> |
| 앱 오류 | <ul><li>[오류 보고 데이터 가져오기](get-error-reporting-data.md)</li><li>[앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md)</li><li>[앱에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[앱의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| 자세한 정보 | <ul><li>[앱에 대한 인사이트 데이터 가져오기](get-insights-data-for-your-app.md)</li></ul>  |
| 등급 및 리뷰 | <ul><li>[앱 평점 가져오기](get-app-ratings.md)</li><li>[앱 리뷰 가져오기](get-app-reviews.md)</li></ul> |
| 앱 내 광고 및 광고 캠페인 | <ul><li>[광고 성과 데이터 가져오기](get-ad-performance-data.md)</li><li>[광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>데스크톱 응용 프로그램에 대 한 메서드

[Windows 데스크톱 응용](/windows/desktop/appxpkg/windows-desktop-application-program)프로그램에 속하는 개발자 계정에서 사용할 수 있는 분석 메서드는 다음과 같습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 설치 항목 |  <ul><li>[데스크톱 애플리케이션 설치 가져오기](get-desktop-app-installs.md)</li></ul> |
| 블록 |  <ul><li>[데스크톱 애플리케이션에 대한 업그레이드 차단 가져오기](get-desktop-block-data.md)</li><li>[데스크톱 애플리케이션에 대한 업그레이드 차단 세부 정보 가져오기](get-desktop-block-data-details.md)</li></ul> |
| 애플리케이션 오류 |  <ul><li>[데스크톱 애플리케이션에 대한 오류 보고 데이터 가져오기](get-desktop-application-error-reporting-data.md)</li><li>[데스크톱 애플리케이션에서 오류 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)</li><li>[데스크톱 애플리케이션에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[데스크톱 애플리케이션의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| 자세한 정보 | <ul><li>[데스크톱 애플리케이션에 대한 인사이트 데이터 가져오기](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Xbox Live 서비스에 대 한 메서드

[Xbox Live 서비스](/gaming/xbox-live/developer-program-overview.md)를 사용 하는 게임이 포함 된 개발자 계정에서 사용할 수 있는 추가 방법은 다음과 같습니다.

| 시나리오       | 메서드      |
|---------------|--------------------|
| 일반 분석 |  <ul><li>[Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)</li><li>[Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)</li><li>[Xbox Live 동시 사용량 현황 데이터 가져오기](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| 상태 분석 |  <ul><li>[Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)</li></ul> |
| 커뮤니티 분석 |  <ul><li>[Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)</li><li>[Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)</li><li>[Xbox Live 멀티 플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>하드웨어 및 드라이버에 대 한 메서드

[Windows 하드웨어 대시보드 프로그램](/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) 에 속하는 개발자 계정은 하드웨어 및 드라이버에 대 한 분석 데이터를 검색 하는 추가 방법 집합에 액세스할 수 있습니다. 자세한 내용은 [하드웨어 대시보드 API](/windows-hardware/drivers/dashboard/dashboard-api)를 참조 하세요.

## <a name="code-example"></a>코드 예제

다음 코드 예제에서는 Azure AD 액세스 토큰을 가져오고 c # 콘솔 앱에서 Microsoft Store analytics API를 호출 하는 방법을 보여 줍니다. 이 코드 예제를 사용 하려면 사용자의 시나리오에 맞게 *tenantId*, *clientId*, *clientSecret*및 *appID* 변수를 적절 한 값에 할당 합니다. 이 예에서는 Newtonsoft.json의 [Json.NET 패키지가](https://www.newtonsoft.com/json) MICROSOFT STORE analytics API에서 반환 된 Json 데이터를 deserialize 해야 합니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>오류 응답

Microsoft Store analytics API는 오류 코드 및 메시지를 포함 하는 JSON 개체에 오류 응답을 반환 합니다. 다음 예에서는 잘못 된 매개 변수로 인해 발생 하는 오류 응답을 보여 줍니다.

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