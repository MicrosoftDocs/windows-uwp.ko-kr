---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Microsoft Store 프로 모션 API를 사용 하 여 또는 조직의 파트너 센터 계정에 등록 된 앱에 대 한 프로 모션 광고 캠페인을 프로그래밍 방식으로 관리 합니다.
title: 스토어 서비스를 사용 하 여 ad 캠페인 실행
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 프로 모션 API, ad 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: 9b9cb30d2a87d93df1790fb42ad3b4b243f0f713
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164557"
---
# <a name="run-ad-campaigns-using-store-services"></a>스토어 서비스를 사용 하 여 ad 캠페인 실행

*Microsoft Store 프로 모션 API* 를 사용 하 여 또는 조직의 파트너 센터 계정에 등록 된 앱에 대 한 프로 모션 광고 캠페인을 프로그래밍 방식으로 관리 합니다. 이 API를 사용 하 여 캠페인을 만들고, 업데이트 하 고, 대상 및 creatives 등의 기타 관련 자산을 모니터링할 수 있습니다. 이 API는 특히 파트너 센터를 사용 하지 않고 많은 양의 캠페인을 만드는 개발자에 게 유용 합니다. 이 API는 Azure Active Directory (Azure AD)를 사용 하 여 앱 또는 서비스에서 호출을 인증 합니다.

다음 단계는 종단 간 프로세스를 설명 합니다.

1.  모든 [필수 구성 요소](#prerequisites)를 완료 했는지 확인 합니다.
2.  Microsoft Store 프로 모션 API에서 메서드를 호출 하기 전에 [AZURE AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 프로 모션 API에 대 한 호출에서이 토큰을 사용 하는 데 60 분이 소요 됩니다. 토큰이 만료 된 후 새 토큰을 생성할 수 있습니다.
3.  [Microsoft Store 프로 모션 API를 호출](#call-the-windows-store-promotions-api)합니다.

또는 파트너 센터를 사용 하 여 ad 캠페인을 만들고 관리할 수 있으며, Microsoft Store 프로 모션 API를 통해 프로그래밍 방식으로 만드는 모든 ad 캠페인은 파트너 센터 에서도 액세스할 수 있습니다. 파트너 센터에서 ad 캠페인을 관리 하는 방법에 대 한 자세한 내용은 [앱에 대 한 광고 캠페인 만들기](../publish/create-an-ad-campaign-for-your-app.md)를 참조 하세요.

> [!NOTE]
> 파트너 센터 계정이 있는 개발자는 Microsoft Store 프로 모션 API를 사용 하 여 앱에 대 한 광고 캠페인을 관리할 수 있습니다. 또한 미디어 기관은이 API에 대 한 액세스를 요청 하 여 광고주를 대신 하 여 ad 캠페인을 실행할 수 있습니다. 이 API에 대 한 자세한 정보를 알고자 하거나 액세스를 요청 하는 미디어 에이전시 인 경우에 요청을 보냅니다 storepromotionsapi@microsoft.com .

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>1 단계: Microsoft Store 프로 모션 API를 사용 하기 위한 필수 구성 요소 완료

Microsoft Store 프로 모션 API를 호출 하는 코드 작성을 시작 하기 전에 다음 필수 구성 요소를 완료 했는지 확인 합니다.

* 이 API를 사용 하 여 ad 캠페인을 성공적으로 만들고 시작 하려면 먼저 [파트너 센터의 **ad 캠페인** 페이지를 사용 하 여 유료 ad 캠페인을 만들고](../publish/create-an-ad-campaign-for-your-app.md)이 페이지에서 결제 방법을 하나 이상 추가 해야 합니다. 이 작업을 수행한 후에는이 API를 사용 하 여 광고 캠페인에 대해 청구 가능한 배달 줄을 성공적으로 만들 수 있습니다. 이 API를 사용 하 여 만든 ad 캠페인의 배달 줄은 파트너 센터의 **ad 캠페인** 페이지에서 선택한 기본 결제 방법을 자동으로 청구 합니다.

* 사용자(또는 조직)는 Azure AD 디렉터리가 있어야 하고 디렉터리에 대한 [전역 관리자](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 권한이 있어야 합니다. Microsoft에서 이미 Microsoft 365 또는 다른 비즈니스 서비스를 사용 하는 경우 Azure AD 디렉터리가 이미 있습니다. 그렇지 않으면 추가 비용 없이 [파트너 센터에서 새 AZURE AD를 만들](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 수 있습니다.

* Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고, 응용 프로그램에 대 한 테 넌 트 ID와 클라이언트 ID를 검색 하 고, 키를 생성 해야 합니다. Azure AD 응용 프로그램은 Microsoft Store 프로 모션 API를 호출 하려는 응용 프로그램 또는 서비스를 나타냅니다. API에 전달하는 Azure AD 액세스 토큰을 얻으려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.
    > [!NOTE]
    > 이 작업은 한 번만 수행 하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키가 있으면 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.

Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고 필요한 값을 검색 하려면 다음을 수행 합니다.

1.  파트너 센터에서 [조직의 파트너 센터 계정을 조직의 Azure AD 디렉터리에 연결합니다](../publish/associate-azure-ad-with-partner-center.md).

2.  그런 다음 파트너 센터의 **계정 설정** 섹션에 있는 **사용자** 페이지에서 파트너 센터 계정에 대 한 프로 모션 캠페인을 관리 하는 데 사용할 앱 또는 서비스를 나타내는 [Azure AD 응용 프로그램을 추가](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) 합니다. 이 애플리케이션을 **관리자** 역할에 할당해야 합니다. 애플리케이션이 아직 Azure AD 디렉터리에 존재하지 않는 경우 [파트너 센터에서 새 Azure AD 애플리케이션 만들 수 있습니다](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  **사용자** 페이지로 돌아가 Azure AD 애플리케이션의 이름을 클릭하여 애플리케이션 설정으로 이동한 다음 **테넌트 ID**와 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 나가면 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [Azure AD 애플리케이션 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store 프로 모션 API에서 메서드를 호출 하기 전에 먼저 API의 각 메서드에 대 한 **인증** 헤더에 전달 하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후에는 API에 대 한 추가 호출에서 토큰을 계속 사용할 수 있도록 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용 하 여 서비스 호출](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) 에 대 한 지침에 따라 HTTP POST를 끝점으로 보냅니다 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` . 샘플 요청은 다음과 같습니다.

```syntax
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

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>3 단계: Microsoft Store 프로 모션 API 호출

Azure AD 액세스 토큰이 있으면 Microsoft Store 프로 모션 API를 호출할 준비가 된 것입니다. 각 메서드의 **인증** 헤더에 액세스 토큰을 전달 해야 합니다.

Microsoft Store 프로 모션 API의 컨텍스트에서 ad 캠페인은 캠페인에 대 한 개략적인 정보를 포함 하는 *캠페인* 개체와, 광고 캠페인에 대 한 *배달 선*, *대상 프로필*및 *creatives* 를 나타내는 추가 개체를 구성 합니다. API에는 이러한 개체 형식으로 그룹화 되는 다양 한 메서드 집합이 포함 되어 있습니다. 캠페인을 만들려면 일반적으로 이러한 각 개체에 대해 다른 POST 메서드를 호출 합니다. 또한 API는 캠페인, 배달 선 및 대상 프로필 개체를 편집 하는 데 사용할 수 있는 개체 및 PUT 메서드를 검색 하는 데 사용할 수 있는 GET 메서드를 제공 합니다.

이러한 개체 및 관련 메서드에 대 한 자세한 내용은 다음 표를 참조 하십시오.


| 개체       | Description   |
|---------------|-----------------|
| 캠페인 |  이 개체는 ad 캠페인을 나타내며, 광고 캠페인의 개체 모델 계층 구조 맨 위에 있습니다. 이 개체는 실행 중인 캠페인 유형 (유료, 집 또는 커뮤니티), 캠페인 목표, 캠페인의 배달 선 및 기타 세부 정보를 식별 합니다. 각 캠페인은 하나의 앱에만 연결할 수 있습니다.<br/><br/>이 개체와 관련 된 메서드에 대 한 자세한 내용은 [ad 캠페인 관리](manage-ad-campaigns.md)를 참조 하세요.<br/><br/>**Note** &nbsp; 참고 &nbsp; 광고 캠페인을 만든 후 [Microsoft Store 분석 API](access-analytics-data-using-windows-store-services.md)에서 [ad 캠페인 성능 데이터 가져오기](get-ad-campaign-performance-data.md) 방법을 사용 하 여 캠페인에 대 한 성능 데이터를 검색할 수 있습니다.  |
| 배달 라인 | 모든 캠페인에는 재고를 구입 하 고 광고를 배달 하는 데 사용 되는 하나 이상의 배달 회선이 있습니다. 각 배달 라인에 대해 대상을 설정 하 고, 입찰 가격을 설정 하 고, 사용 하려는 creatives에 대 한 예산 및 링크를 설정 하 여 비용을 결정 하는 데 사용할 수 있습니다.<br/><br/>이 개체와 관련 된 메서드에 대 한 자세한 내용은 [ad 캠페인의 배달 선 관리](manage-delivery-lines-for-ad-campaigns.md)를 참조 하세요. |
| 대상 프로필 | 모든 배달 라인에는 대상으로 지정할 사용자, 지역 및 재고 유형을 지정 하는 대상 프로필이 하나 있습니다. 대상 프로필을 템플릿으로 만들고 배달 줄에서 공유할 수 있습니다.<br/><br/>이 개체와 관련 된 메서드에 대 한 자세한 내용은 [ad 캠페인의 대상 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)를 참조 하세요. |
| Creatives | 모든 배달 라인에는 캠페인의 일부로 고객에 게 표시 되는 광고를 나타내는 하나 이상의 creatives 있습니다. Creative는 항상 동일한 앱을 나타내는 ad 캠페인을 통해 하나 이상의 배달 선과 연결 될 수 있습니다.<br/><br/>이 개체와 관련 된 메서드에 대 한 자세한 내용은 [Manage creatives for ad 캠페인과](manage-creatives-for-ad-campaigns.md)항목을 참조 하세요. |


다음 다이어그램은 캠페인, 배달 선, 대상 프로필 및 creatives 간의 관계를 보여 줍니다.

![Ad 캠페인 계층 구조](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>코드 예제

다음 코드 예제에서는 Azure AD 액세스 토큰을 가져오고 c # 콘솔 앱에서 Microsoft Store 프로 모션 API를 호출 하는 방법을 보여 줍니다. 이 코드 예제를 사용 하려면 사용자의 시나리오에 맞게 *tenantId*, *clientId*, *clientSecret*및 *appID* 변수를 적절 한 값에 할당 합니다. 이 예에서는 Newtonsoft.json의 [Json.NET 패키지가](https://www.newtonsoft.com/json) Microsoft Store 프로 모션 API에서 반환 된 Json 데이터를 deserialize 해야 합니다.

[!code-csharp[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>관련 항목

* [광고 캠페인 관리](manage-ad-campaigns.md)
* [광고 캠페인의 배달 선 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인의 대상 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [Ad 캠페인에 대 한 creatives 관리](manage-creatives-for-ad-campaigns.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)


 