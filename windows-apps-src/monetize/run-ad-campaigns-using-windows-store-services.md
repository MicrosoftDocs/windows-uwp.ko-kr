---
author: Xansky
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Microsoft Store 프로 모션 API를 사용 하 여 프로그래밍 방식으로 사용자 또는 사용자 조직의 파트너 센터 계정에 등록 된 앱에 대 한 홍보용 광고 캠페인을 관리 합니다.
title: 스토어 서비스를 사용하여 광고 캠페인 실행
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 프로모션 API, 광고 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: 81e77fb01347c2a6ec2630e08f3a00ee6cf47de7
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7575265"
---
# <a name="run-ad-campaigns-using-store-services"></a>스토어 서비스를 사용하여 광고 캠페인 실행

*Microsoft Store 프로 모션 API* 사용 하 여 프로그래밍 방식으로 사용자 또는 사용자 조직의 파트너 센터 계정에 등록 된 앱에 대 한 홍보용 광고 캠페인을 관리 합니다. 이 API를 사용하여 대상 지정 및 창작 광고와 같은 캠페인 및 기타 관련 자산을 만들고 업데이트하고 모니터링할 수 있습니다. 이 API는 사용자 대량의 캠페인을 만들고 파트너 센터를 사용 하지 않고 하려는 개발자에 게 특히 유용 합니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
2.  Microsoft Store 프로모션 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 프로모션 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
3.  [Microsoft Store 프로모션 API를 호출합니다](#call-the-windows-store-promotions-api).

또는 만들기 하 고 파트너 센터를 사용 하 여 광고 캠페인 및 파트너 센터에서 API에 액세스할 수도 있습니다 Microsoft Store 프로 모션을 통해 프로그래밍 방식으로 만드는 광고 캠페인도 관리할 수 있습니다. 파트너 센터에서 광고 캠페인을 관리 하는 방법에 대 한 자세한 내용은 [앱에 대 한 광고 캠페인 만들기](../publish/create-an-ad-campaign-for-your-app.md)를 참조 하세요.

> [!NOTE]
> 파트너 센터 계정이 있는 개발자 Microsoft Store 프로 모션 API를 사용 하 여 자신의 앱에 대 한 광고 캠페인을 관리할 수 있습니다. 광고회사는 광고주를 대리하여 광고 캠페인을 실행하기 위해 이 API에 대 한 액세스를 요청할 수도 있습니다. 이 API에 대한 자세한 내용을 알기 원하거나 이 API에 대한 액세스를 요청하려는 광고회사는 storepromotionsapi@microsoft.com으로 요청을 보내시기 바랍니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>1단계: Microsoft Store 프로모션 API를 사용하기 위한 필수 조건 완료

Microsoft Store 프로모션 API를 호출하는 코드 작성을 시작하기 전에 다음 필수 조건을 완료했는지 확인합니다.

* 있습니다 수를 성공적으로 만들고이 API를 사용 하 여 광고 캠페인을 시작 하기 전에 먼저 [파트너 센터에서 **광고 캠페인** 페이지를 사용 하 여 하나의 유료 광고 캠페인 만들기](../publish/create-an-ad-campaign-for-your-app.md), 해야 하 고이 페이지에서 하나 이상의 결제 방법을 추가 해야 합니다. 이렇게 하면 이 API를 사용하여 광고 캠페인의 청구 가능한 배달 라인을 성공적으로 만들 수 있습니다. 이 API를 사용 하 여 만든 광고 캠페인의 배달 라인 됩니다 파트너 센터에서 **광고 캠페인** 페이지에서 선택 된 기본 결제 방법 요금을 청구 자동으로 합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 그렇지 않으면 추가 요금 없이 [파트너 센터에서 Azure AD를 새로 만들](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 수 있습니다.

* Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고, ID 및 클라이언트 ID는 응용 프로그램에 대 한 테 넌 트를 검색 하 고, 키를 생성 해야 합니다. Azure AD 응용 프로그램은 Microsoft Store 프로모션 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.
    > [!NOTE]
    > 이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

파트너 센터 계정과 Azure AD 응용 프로그램 연결에 필요한 값을 검색.

1.  파트너 센터에서는 [조직의 파트너 센터 계정을 조직의 Azure AD 디렉터리와 연결](../publish/associate-azure-ad-with-dev-center.md)합니다.

2.  파트너 센터, [Azure AD 응용 프로그램을 추가](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) 앱을 나타내는 또는 파트너 센터 계정에 대 한 프로 모션 캠페인을 관리 하는 데 사용할 서비스의 **계정 설정** 섹션에서 **사용자가** 페이지에서 다음으로. 이 응용 프로그램에 **관리자** 역할을 할당하도록 합니다. 응용 프로그램이 아직 Azure AD 디렉터리에 수 있는 경우 [새 파트너 센터에서 Azure AD 응용 프로그램](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)합니다. 

3.  **사용자** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [AD 응용 프로그램 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store 프로모션 API에서 메서드를 호출하기 전에 먼저 API에 있는 각 메서드의 **Authorization** 헤더에 전달하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 API에 대한 추가 호출에 계속 사용할 수 있도록 해당 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지침에 따라 HTTP POST를 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점에 보냅니다. 다음은 샘플 요청입니다.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

POST URI 및 *client\_id* 및 *client\_secret* 매개 변수에서 *tenant\_id* 값에 대 한 테 넌 트 ID, 클라이언트 ID 및 이전 섹션에서 파트너 센터에서 검색 하는 응용 프로그램에 대 한 키를 지정 합니다. *resource* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>3단계: Microsoft Store 프로모션 API 호출

Azure AD 액세스 토큰이 있으면 Microsoft Store 프로모션 API를 호출할 준비가 된 것입니다. 액세스 토큰을 각 메서드의 **Authorization** 헤더로 전달해야 합니다.

Microsoft Store 프로모션 API 컨텍스트에서 광고 캠페인은 캠페인에 대한 고급 정보를 포함하는 *캠페인* 개체와 광고 캠페인에 대한 *배달 라인*, *타기팅 프로필* 및 *크리에이티브*를 나타내는 추가 개체로 구성됩니다. API는 이러한 개체 유형으로 그룹화된 일련의 메소드를 포함합니다. 캠페인을 만들려면 일반적으로 이들 각 개체에 대해 다른 POST 메소드를 호출합니다. 또한 API는 모든 개체를 검색하는 데 사용할 수 있는 GET 메소드와 캠페인, 배달 라인 및 타기팅 프로필 개체를 편집하는 데 사용할 수 있는 PUT 메서드도 제공합니다.

이러한 개체 및 관련 메서드에 대한 자세한 내용은 다음 표를 참조하세요.


| 개체       | 설명   |
|---------------|-----------------|
| 캠페인 |  이 개체는 광고 캠페인을 나타내며, 광고 캠페인의 개체 모델 계층에서 상단에 위치합니다. 이 개체는 실행 중인 캠페인의 유형(유료, 하우스 또는 커뮤니티), 캠페인 목표, 캠페인의 배달 라인 및 기타 세부 정보를 식별합니다. 각 캠페인은 한 앱에만 연결할 수 있습니다.<br/><br/>이 개체와 관련된 메서드에 대한 자세한 내용은 [광고 캠페인 관리](manage-ad-campaigns.md)를 참조하세요.<br/><br/>**참고**&nbsp;&nbsp;광고 캠페인을 만든 후 [Microsoft Store 분석 API](access-analytics-data-using-windows-store-services.md)의 [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md) 메서드를 사용하여 캠페인의 성과 데이터를 검색할 수 있습니다.  |
| 배달 라인 | 각 캠페인에는 인벤토리를 구매하고 광고를 전달하는 데 사용되는 배달 라인이 하나 이상 있습니다. 각 배달 라인에 대해 타기팅을 설정하고, 입찰 가격을 설정할 수 있으며, 예산을 설정하고 사용할 크리에이티브와 연결하여 지출할 금액을 결정할 수 있습니다.<br/><br/>이 개체와 관련된 메서드에 대한 자세한 내용은 [광고 캠페인 배달 라인 관리](manage-delivery-lines-for-ad-campaigns.md)를 참조하세요. |
| 타기팅 프로필 | 각 배달 라인에는 타깃으로 설정할 사용자, 지역 및 인벤토리 유형을 지정하는 타기팅 프로필이 하나씩 있습니다. 타기팅 프로필은 템플릿으로 만들고 배달 라인 사이에서 공유할 수 있습니다.<br/><br/>이 개체와 관련된 메서드에 대한 자세한 내용은 [광고 캠페인 타기팅 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)를 참조하세요. |
| 크리에이티브 | 모든 배달 라인에는 캠페인의 일부로서 고객에게 표시되는 광고를 나타내는 크리에이티브가 하나 이상 있습니다. 크리에이티브는 하나 이상의 배달 라인과 연결될 수 있으며, 항상 동일한 앱을 나타내는 경우 여러 광고 캠페인에서 공유될 수도 있습니다.<br/><br/>이 개체와 관련된 메서드에 대한 자세한 내용은 [광고 캠페인 크리에이티브 관리](manage-creatives-for-ad-campaigns.md)를 참조하세요. |


다음 다이어그램은 캠페인, 배달 라인, 타기팅 프로필 및 크리에이티브 간 관계를 보여 줍니다.

![광고 캠페인 계층](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>코드 예제

다음 코드 예제는 Azure AD 액세스 토큰을 얻고 C# 콘솔 앱에서 Microsoft Store 프로모션 API를 호출하는 방법을 보여 줍니다. 이 코드 예제를 사용하려면 시나리오에 맞는 적절한 값을 *tenantId*, *clientId*, *clientSecret* 및 *appID* 변수에 할당합니다. 이 예제에서 Microsoft Store 프로모션 API가 반환한 JSON 데이터를 역직렬화하려면 Newtonsoft의 [Json.NET 패키지](http://www.newtonsoft.com/json)가 필요합니다.

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>관련 항목

* [광고 캠페인 관리](manage-ad-campaigns.md)
* [광고 캠페인 배달 라인 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인 타기팅 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [광고 캠페인 크리에이티브 관리](manage-creatives-for-ad-campaigns.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)


 
