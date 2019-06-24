---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: 앱과 추가 기능 카탈로그가 있는 경우 Microsoft Store 컬렉션 API 및 Microsoft Store 구매 API를 사용하여 서비스에서 이러한 제품에 대한 소유권 정보에 액세스할 수 있습니다.
title: 서비스에서 제품 권한 관리
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 컬렉션 API, Microsoft Store 구매 API, 제품 보기, 제품 권한 부여
ms.localizationpriority: medium
ms.openlocfilehash: 184937133b85ae2cac7a21bb6002af70b06d34da
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319920"
---
# <a name="manage-product-entitlements-from-a-service"></a>서비스에서 제품 권한 관리

앱과 추가 기능 카탈로그가 있는 경우 *Microsoft Store 컬렉션 API* 및 *Microsoft Store 구매 API*를 사용하여 서비스에서 이러한 제품에 대한 소유권 정보에 액세스할 수 있습니다. *권한*은 Microsoft Store를 통해 게시된 앱 또는 추가 기능을 사용하는 고객의 권리를 나타냅니다.

이러한 API는 플랫폼 간 서비스에서 지원하는 추가 기능 카탈로그와 개발자가 사용하도록 설계된 REST 메서드로 구성됩니다. 이러한 API를 사용하여 다음 작업을 수행할 수 있습니다.

-   Microsoft Store 컬렉션 API: [사용자가 소유 하는 제품에 대 한 쿼리](query-for-products.md) 하 고 [충족 하는 대로 사용할 수 있는 제품을 보고](report-consumable-products-as-fulfilled.md)합니다.
-   Microsoft Store 구매 API: [무료 제품을 사용자에 게 부여할](grant-free-products.md), [사용자에 대 한 구독을 얻으려면](get-subscriptions-for-a-user.md), 및 [사용자에 대 한 구독의 청구 상태 변경](change-the-billing-state-of-a-subscription-for-a-user.md)합니다.

> [!NOTE]
> Microsoft Store 컬렉션 API 및 구매 API는 고객 소유권 정보에 액세스하기 위해 Azure AD(Active Directory) 인증을 사용합니다. 이러한 API를 사용하려면 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](https://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다.

## <a name="overview"></a>개요

다음 단계에서는 Microsoft Store 컬렉션 API 및 구매 API를 사용하는 모든 과정을 설명합니다.

1.  [Azure AD에서 응용 프로그램 구성](#step-1)합니다.
2.  [파트너 센터에서 앱을 사용 하 여 Azure AD 응용 프로그램 ID를 연결할](#step-2)합니다.
3.  서비스에서 게시자 ID를 나타내는 [Azure AD 액세스 토큰을 만듭니다](#step-3).
4.  클라이언트 Windows 앱에서 [Microsoft Store ID 키를 만들](#step-4) 전달이이 키는 서비스에 다시 확인 하 고 현재 사용자의 id를 나타내는입니다.
5.  필요한 Azure AD 액세스 토큰 및 Microsoft Store ID 키를 획득한 후 [서비스에서 Microsoft Store 컬렉션 API 또는 구매 API를 호출합니다](#step-5).

이 종단 간 프로세스에는 다양 한 작업을 수행 하는 두 개의 소프트웨어 구성 요소 포함 됩니다.

* **서비스**합니다. 비즈니스 환경의 컨텍스트에서 안전 하 게 실행 하는 응용 프로그램 이므로 선택 하는 모든 개발 플랫폼을 사용 하 여 구현할 수 있습니다. 서비스는 Azure AD 액세스 토큰을 만들고 필요한 시나리오에 대 한 Microsoft Store 컬렉션에 대 한 REST Uri를 호출 하기 위한 API 및 구매 API에 대 한 합니다.
* **클라이언트 Windows 앱**합니다. 에 액세스 하는 앱에 대 한 추가 기능 등 고객 자격 정보를 관리 하려는 앱입니다. 이 앱은 Microsoft Store 컬렉션 API를 호출 하 고 서비스에서 API를 구입 해야 하는 Microsoft Store ID 키 생성을 담당 합니다.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>1단계: Azure AD에서 응용 프로그램 구성

Microsoft Store 컬렉션 API를 사용 하거나 API를 구매, Azure AD 웹 응용 프로그램 만들기 하 고, ID 및 응용 프로그램에 대 한 응용 프로그램 ID 테 넌 트를 검색 하 고, 키를 생성 해야 있습니다. Azure AD 웹 응용 프로그램은 Microsoft Store 컬렉션 API를 호출 하거나 API를 구매 하려는 서비스를 나타냅니다. 테 넌 트 ID, 응용 프로그램 ID 및 키 API를 호출 해야 하는 Azure AD 액세스 토큰을 생성 하는 것이 해야 합니다.

> [!NOTE]
> 이 섹션의 작업을 한 번만 수행하면 됩니다. Azure AD 응용 프로그램 매니페스트를 업데이트 후에 테 넌 트 ID, 응용 프로그램 ID 및 클라이언트 암호 다시 사용할 수 있습니다 이러한 값을 만들기 위한 필요할 때 언제 든 Azure AD 액세스 토큰입니다.

1.  아직 수행 하지 않은 경우의 지침을 따릅니다 [Azure Active Directory와 응용 프로그램 통합](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) 등록 하는 **웹 앱 / API** Azure AD를 사용 하 여 응용 프로그램입니다.
    > [!NOTE]
    > 응용 프로그램을 등록할 때 선택 해야 **웹 앱 / API** 키를 검색할 수 있도록 응용 프로그램 유형으로 (라고도 *클라이언트 비밀*) 응용 프로그램에 대 한 합니다. Microsoft Store 컬렉션 API 또는 구매 API를 호출하려면 이후 단계에서 Azure AD의 액세스 토큰을 요청할 때 클라이언트 암호를 제공해야 합니다.

2.  에 [Azure 관리 포털](https://portal.azure.com/), 이동할 **Azure Active Directory**합니다. 디렉터리를 선택, 클릭 **앱 등록** 왼쪽된 탐색 창에서 다음 응용 프로그램을 선택 합니다.
3.  응용 프로그램의 기본 등록 페이지로 이동 합니다. 이 페이지에 복사 합니다 **응용 프로그램 ID** 나중에 사용할 값입니다.
4.  나중에 필요한 키를 만듭니다 (이 모든 호출을 *클라이언트 암호*). 왼쪽된 창에서 클릭 **설정을** 차례로 **키**합니다. 이 페이지에서는 완료 하는 단계 [키를 만들](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis)합니다. 나중에 사용에 대 한이 키를 복사 합니다.
5.  여러 필요한 대상 Uri를 추가 하 여 [응용 프로그램 매니페스트](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). 왼쪽된 창에서 클릭 **매니페스트**합니다. 클릭 **편집**을 대체 합니다 `"identifierUris"` 다음 텍스트를 사용 하 여 섹션 및 클릭 **저장**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    이러한 문자열은 응용 프로그램이 지원하는 대상 그룹을 나타냅니다. 이후 단계에서 이러한 대상 값에 각각 연결된 Azure AD 액세스 토큰을 만듭니다.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>2단계: 파트너 센터에서 클라이언트 앱을 사용 하 여 Azure AD 응용 프로그램 ID에 연결

소유권 및 앱 또는 추가 기능에 대 한 구매를 구성 하는 API를 구매 하거나 Microsoft Store 컬렉션 API를 사용 하 여 파트너 센터에서 앱 (또는 추가 기능을 포함 하는 앱)를 사용 하 여 Azure AD 응용 프로그램 ID를 연결 해야 합니다.

> [!NOTE]
> 이 작업은 한 번만 수행하면 됩니다.

1.  에 로그인 [파트너 센터](https://partner.microsoft.com/dashboard) 앱을 선택 합니다.
2.  로 이동 합니다 **Services** &gt; **제품 컬렉션 및 구매** 페이지 중 하나에 Azure AD 응용 프로그램 ID를 입력 하 고 **클라이언트 ID** 필드입니다.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>3단계: Azure AD 액세스 토큰 만들기

Microsoft Store ID 키를 검색하거나 Microsoft Store 컬렉션 API 또는 구매 API를 호출하려면 서비스에서 게시자 ID를 나타내는 여러 Azure AD 액세스 토큰을 만들어야 합니다. 각 토큰은 서로 다른 API에 사용됩니다. 각 토큰의 수명은 60분이며 만료된 후 새로 고칠 수 있습니다.

> [!IMPORTANT]
> 앱이 아닌 서비스의 컨텍스트에서만 Azure AD 액세스 토큰을 만듭니다. 앱에 전송되면 클라이언트 암호가 손상될 수 있습니다.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>여러 토큰 및 대상 그룹 URI의 이해

Microsoft Store 컬렉션 API 또는 구매 API에서 호출하려는 메서드에 따라 두 가지 또는 세 가지 토큰을 만들어야 합니다. 각 액세스 토큰은 서로 다른 대상 그룹 URI(이전에 개발자가 Azure AD 응용 프로그램 매니페스트의 `"identifierUris"` 섹션에 추가한 것과 동일한 URI)에 연결됩니다.

  * 어떤 경우든 `https://onestore.microsoft.com` 대상 그룹 URI를 사용하여 토큰을 만들어야 합니다. 나중에 Microsoft Store 컬렉션 API 또는 구매 API에서 이 토큰을 메서드의 **권한 부여** 헤더로 전달할 것입니다.
      > [!IMPORTANT]
      > `https://onestore.microsoft.com` 대상 그룹에는 서비스에 안전하게 저장된 액세스 토큰만 사용하세요. 이 대상 그룹의 액세스 토큰을 서비스 외부에 노출시키면 서비스 재생 공격에 취약해질 수 있습니다.

  * Microsoft Store 컬렉션 API에서 메서드를 호출하여 [사용자가 소유한 제품에 대해 쿼리](query-for-products.md)하거나 [소모품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)하려는 경우에도 `https://onestore.microsoft.com/b2b/keys/create/collections` 대상 그룹 URI를 사용하여 토큰을 만들어야 합니다. 이후 단계에서 Windows SDK의 클라이언트 메서드에 이 토큰을 전달하여 Microsoft Store 컬렉션 API와 함께 사용할 수 있는 Microsoft Store ID 키를 요청할 것입니다.

  * Microsoft Store 구매 API의 메서드를 호출하여 [사용자에게 무료 제품에 대한 권한을 부여하고](grant-free-products.md), [사용자의 구독을 가져오고](get-subscriptions-for-a-user.md), [사용자의 구독 청구 상태를 변경하려면](change-the-billing-state-of-a-subscription-for-a-user.md)`https://onestore.microsoft.com/b2b/keys/create/purchase` 대상 그룹 URI를 사용하여 토큰도 만들어야 합니다. 이후 단계에서 Windows SDK의 클라이언트 메서드에 이 토큰을 전달하여 Microsoft Store 구매 API와 함께 사용할 수 있는 Microsoft Store ID 키를 요청할 것입니다.

<span />

### <a name="create-the-tokens"></a>토큰 만들기

액세스 토큰을 만들려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지침에 따라 서비스에서 OAuth 2.0 API를 사용하여 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점으로 HTTP POST를 보냅니다. 다음은 샘플 요청입니다.

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

각 토큰에 대해 다음 매개 변수 데이터를 지정합니다.

* 에 대 한 합니다 *클라이언트\_id* 하 고 *클라이언트\_비밀* 응용 프로그램 ID 및 클라이언트 암호를 에서검색하는응용프로그램에대한매개변수를지정[Azure 관리 포털](https://portal.azure.com/)합니다. Microsoft Store 컬렉션 API 또는 구매 API에서 요구하는 인증 수준의 액세스 토큰을 만들려면 두 매개 변수가 모두 필요합니다.

* *리소스* 매개 변수의 경우 만들려는 액세스 토큰의 종류에 따라 [이전 섹션](#access-tokens)에 나열된 대상 그룹 URI 중 하나를 지정합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다. 액세스 토큰의 구조에 대한 자세한 내용은 [지원되는 토큰 및 클레임 유형](https://go.microsoft.com/fwlink/?LinkId=722501)을 참조하세요.

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>4단계: Microsoft Store ID 키 만들기

Microsoft Store 컬렉션 API 또는 구매 API에서 메서드를 호출하려면 앱에서 Microsoft Store ID 키를 만들고 서비스로 전송해야 합니다. 이 키는 액세스하려는 제품 소유권 정보를 소유한 사용자 ID를 나타내는 JWT(JSON 웹 토큰)입니다. 이 키의 클레임에 대한 자세한 내용은 [Microsoft Store ID 키 클레임](#claims-in-a-microsoft-store-id-key)을 참조하세요.

현재 Microsoft Store ID 키를 만드는 유일한 방법은 앱의 클라이언트 코드에서 UWP(유니버설 Windows 플랫폼) API를 호출하는 것입니다. 생성된 키는 현재 디바이스에서 Microsoft Store에 로그인한 사용자의 ID를 나타냅니다.

> [!NOTE]
> 각 Microsoft Store ID 키는 90일 동안 유효합니다. 키가 만료된 후 [키를 갱신](renew-a-windows-store-id-key.md)할 수 있습니다. 새로 만들기보다는 Microsoft Store ID 키를 갱신하는 것이 좋습니다.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Microsoft Store 컬렉션 API에 대한 Microsoft Store ID 키를 만들려면

다음 단계에 따라 Microsoft Store 컬렉션 API와 함께 사용할 수 있는 Microsoft Store ID 키를 만들어서 [사용자가 소유한 제품에 대해 쿼리](query-for-products.md)하거나 [소모품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)합니다.

1.  서비스에서 대상 그룹 URL 값 `https://onestore.microsoft.com/b2b/keys/create/collections`을 가진 Azure AD 액세스 토큰을 전달합니다. 이는 [이전 3단계](#step-3)에서 만들었던 토큰 중 하나입니다.

2.  앱 코드에서 이러한 메서드 중 하나를 호출하여 Microsoft Store ID 키를 검색합니다.

  * 앱에서 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스에 [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) 메서드를 사용합니다.

  * 앱에서 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 네임스페이스의 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) 메서드를 사용합니다.

    메서드의 *serviceTicket* 매개 변수에 Azure AD 액세스 토큰을 전달합니다. 현재 앱의 게시자로 관리 하는 서비스의 컨텍스트에서 익명 사용자 Id를 유지 관리할 사용자 ID를 전달할 수도 있습니다는 *publisherUserId* 새 Microsoft Store ID를 사용 하 여 현재 사용자를 연결할 매개 변수 (사용자 ID에에서 포함 될 키) 키입니다. 그렇지 않으면 Microsoft Store ID 키를 사용 하 여 사용자 ID를 연결할 필요가 없으면 임의의 문자열 값을 전달할 수는 *publisherUserId* 매개 변수입니다.

3.  앱에서 Microsoft Store ID 키를 성공적으로 만들면 키를 다시 서비스로 전달합니다.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Microsoft Store 구매 API에 대한 Microsoft Store ID 키를 만들려면

이러한 단계에 따라 Microsoft Store 구매 API와 함께 사용하여 [사용자에게 무료 제품에 대한 권한을 부여하고](grant-free-products.md), [사용자의 구독을 가져오고](get-subscriptions-for-a-user.md), [사용자의 구독 청구 상태를 변경](change-the-billing-state-of-a-subscription-for-a-user.md)할 수 있는 Microsoft Store ID 키를 만듭니다.

1.  서비스에서 대상 그룹 URL 값 `https://onestore.microsoft.com/b2b/keys/create/purchase`을 가진 Azure AD 액세스 토큰을 전달합니다. 이는 [이전 3단계](#step-3)에서 만들었던 토큰 중 하나입니다.

2.  앱 코드에서 이러한 메서드 중 하나를 호출하여 Microsoft Store ID 키를 검색합니다.

  * 앱에서 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)의 [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) 메서드를 사용합니다.

  * 앱에서 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 네임스페이스에 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) 메서드를 사용합니다.

    메서드의 *serviceTicket* 매개 변수에 Azure AD 액세스 토큰을 전달합니다. 현재 앱의 게시자로 관리 하는 서비스의 컨텍스트에서 익명 사용자 Id를 유지 관리할 사용자 ID를 전달할 수도 있습니다는 *publisherUserId* 새 Microsoft Store ID를 사용 하 여 현재 사용자를 연결할 매개 변수 (사용자 ID에에서 포함 될 키) 키입니다. 그렇지 않으면 Microsoft Store ID 키를 사용 하 여 사용자 ID를 연결할 필요가 없으면 임의의 문자열 값을 전달할 수는 *publisherUserId* 매개 변수입니다.

3.  앱에서 Microsoft Store ID 키를 성공적으로 만들면 키를 다시 서비스로 전달합니다.

### <a name="diagram"></a>다이어그램

다음 다이어그램에서는 Microsoft Store ID 키를 만드는 과정을 보여 줍니다.

  ![Windows Store ID 키 만들기](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>5단계: Microsoft Store 컬렉션 API를 호출 하거나 서비스에서 API를 구매

서비스에서 특정 사용자의 제품 소유권 정보에 액세스할 수 있게 해주는 Microsoft Store ID 키를 만든 후에는 서비스에서 다음 지침에 따라 Microsoft Store 컬렉션 API 또는 구매 API를 호출할 수 있습니다.

* [제품에 대 한 쿼리](query-for-products.md)
* [충족 하는 대로 소모 성 제품을 보고](report-consumable-products-as-fulfilled.md)
* [권한 부여 무료 제품](grant-free-products.md)
* [사용자에 대 한 구독을 얻으려면](get-subscriptions-for-a-user.md)
* [사용자에 대 한 구독의 청구 상태 변경](change-the-billing-state-of-a-subscription-for-a-user.md)

각 시나리오에 대해 다음 정보를 API로 전달합니다.

-   요청 머리글에서 사용자가 대상 그룹 URI 값 `https://onestore.microsoft.com`이 포함된 Azure AD 액세스 토큰을 전달합니다. 이는 [이전 3단계](#step-3)에서 만들었던 토큰 중 하나입니다. 이 토큰은 게시자 ID를 나타냅니다.
-   요청 본문에서 앱의 클라이언트 측 코드로부터 [4단계 초반](#step-4)에 검색한 Microsoft Store ID 키를 전달합니다. 이 키는 액세스하려는 제품 소유권 정보의 소유자인 사용자의 ID를 나타냅니다.

### <a name="diagram"></a>다이어그램

다음 다이어그램은 서비스에서 Microsoft Store 컬렉션 API 또는 구매 API의에서 메서드를 호출 하는 프로세스를 설명 합니다.

  ![컬렉션 또는 구매 API 호출 합니다.](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Microsoft Store ID 키 클레임

Microsoft Store ID 키는 액세스하려는 제품 소유권 정보의 소유자인 사용자의 ID를 나타내는 JWT(JSON 웹 토큰)입니다. Base64를 사용하여 디코딩하면 Microsoft Store ID 키에 다음 클레임이 포함됩니다.

* `iat`:&nbsp;&nbsp;&nbsp;키 발급 된 시간을 식별 합니다. 이 클레임은 토큰의 수명을 확인하는 데 사용할 수 있습니다. 이 값은 시기 시간으로 표현됩니다.
* `iss`:&nbsp;&nbsp;&nbsp;발급자를 식별 합니다. `aud` 클레임과 같은 값입니다.
* `aud`:&nbsp;&nbsp;&nbsp;대상을 식별 합니다. 값은 `https://collections.mp.microsoft.com/v6.0/keys` 또는 `https://purchase.mp.microsoft.com/v6.0/keys` 중 하나여야 합니다.
* `exp`:&nbsp;&nbsp;&nbsp;또는 그 이후에 키를 더 이상 수락 키 갱신 제외 하 고 아무 것도 처리에 대 한 만료 시간을 식별 합니다. 이 클레임의 값은 시기 시간으로 표시됩니다.
* `nbf`:&nbsp;&nbsp;&nbsp;처리에 대 한 토큰을 수락 됩니다 하는 시간을 식별 합니다. 이 클레임의 값은 시기 시간으로 표시됩니다.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;개발자를 식별 하는 클라이언트 ID입니다.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Microsoft Store 서비스에 사용 될 정보를 포함 하는 불투명 페이로드 (암호화 및 Base64로 인코딩된).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;서비스의 컨텍스트에서 현재 사용자를 식별 하는 사용자 ID입니다. [키를 만들 때 사용하는 메서드](#step-4)의 선택적 *publisherUserId* 매개 변수로 전달하는 값과 동일한 값입니다.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;키를 갱신 하는 데 사용할 수 있는 URI입니다.

다음은 디코딩된 Microsoft Store ID 키 헤더의 예입니다.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

다음은 디코딩된 Microsoft Store ID 키 클레임 집합의 예입니다.

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>관련 항목

* [제품에 대 한 쿼리](query-for-products.md)
* [충족 하는 대로 소모 성 제품을 보고](report-consumable-products-as-fulfilled.md)
* [권한 부여 무료 제품](grant-free-products.md)
* [사용자에 대 한 구독을 얻으려면](get-subscriptions-for-a-user.md)
* [사용자에 대 한 구독의 청구 상태 변경](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Microsoft Store ID 키를 갱신](renew-a-windows-store-id-key.md)
* [Azure Active Directory와 응용 프로그램 통합](https://go.microsoft.com/fwlink/?LinkId=722502)
* [Azure Active Directory 응용 프로그램 매니페스트 이해]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [지원 되는 토큰 및 클레임 유형](https://go.microsoft.com/fwlink/?LinkId=722501)
