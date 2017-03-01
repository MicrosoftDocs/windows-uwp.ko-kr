---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "앱과 추가 기능 카탈로그가 있는 경우 Windows 스토어 컬렉션 API 및 Windows 스토어 구매 API를 사용하여 서비스에서 이러한 제품의 소유권 정보에 액세스할 수 있습니다."
title: "서비스에서 제품 권한 관리"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 스토어 컬렉션 API, Windows 스토어 구매 API, 제품 보기, 제품 권한 부여"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7f4f74c887509e772fd01dbdcfe28d86c583fbc1
ms.lasthandoff: 02/07/2017

---

# <a name="manage-product-entitlements-from-a-service"></a>서비스에서 제품 권한 관리

앱과 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 카탈로그가 있는 경우 *Windows 스토어 컬렉션 API* 및 *Windows 스토어 구매 API*를 사용하여 서비스에서 이러한 제품의 권한 정보에 액세스할 수 있습니다. *권한*은 Windows 스토어를 통해 게시된 앱 또는 추가 기능을 사용하는 고객의 권리를 나타냅니다.

이러한 API는 플랫폼 간 서비스에서 지원하는 추가 기능 카탈로그와 개발자가 사용하도록 설계된 REST 메서드로 구성됩니다. 이러한 API를 사용하여 다음 작업을 수행할 수 있습니다.

-   Windows 스토어 컬렉션 API: [사용자가 소유한 제품을 쿼리](query-for-products.md)하고 [소모품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)합니다.
-   Windows 스토어 구매 API: [사용자에게 무료 제품에 대한 권리를 부여](grant-free-products.md)합니다.

>**참고**&nbsp;&nbsp;Windows 스토어 컬렉션 API 및 구매 API는 고객 소유권 정보에 액세스하기 위해 Azure AD(Azure Active Directory) 인증을 사용합니다. 이러한 API를 사용하려면 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다.

## <a name="overview"></a>개요

다음 단계에서는 Windows 스토어 컬렉션 API 및 구매 API를 사용하는 모든 과정을 설명합니다.

1.  [Azure AD에서 웹 응용 프로그램을 구성합니다](#step-1).
2.  [Windows 개발자 센터 대시보드에서 Azure AD 클라이언트 ID와 응용 프로그램을 연결합니다](#step-2).
3.  서비스에서 게시자 ID를 나타내는 [Azure AD 액세스 토큰을 만듭니다](#step-3).
4.  Windows 앱의 클라이언트 쪽 코드에서 현재 사용자의 ID를 나타내는 [Windows 스토어 ID 키를 만들](#step-4)고 Windows 스토어 ID 키를 다시 사용자의 서비스로 전달합니다.
5.  필요한 Azure AD 액세스 토큰 및 Windows 스토어 ID 키가 있으면 [서비스에서 Windows 스토어 컬렉션 API를 호출합니다](#step-5).

다음 섹션에서는 이러한 각 단계에 대한 자세한 내용을 제공합니다.

<span id="step-1"/>
### <a name="step-1-configure-a-web-application-in-azure-ad"></a>1단계: Azure AD에서 웹 응용 프로그램 구성

Windows 스토어 컬렉션 API 또는 구매 API를 사용하려면 먼저 Azure AD 웹 응용 프로그램을 만들고, 응용 프로그램의 테넌트 ID와 클라이언트 ID를 검색하고, 키를 생성해야 합니다. Azure AD 응용 프로그램은 개발자가 Windows 스토어 컬렉션 API 또는 구매 API를 호출할 앱 또는 서비스를 나타냅니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.

>**참고**&nbsp;&nbsp;이 섹션의 작업을 한 번만 수행하면 됩니다. Azure AD 응용 프로그램 매니페스트를 업데이트하고 테넌트 ID, 클라이언트 ID 및 클라이언트 암호를 설정한 후에는 언제든지 이러한 값을 다시 사용하여 새로운 Azure AD 액세스 토큰을 만들 수 있습니다.

1.  [응용 프로그램과 Azure Active Directory 통합](http://go.microsoft.com/fwlink/?LinkId=722502)의 지침에 따라 웹 응용 프로그램을 Azure AD에 추가합니다.

    > **참고**&nbsp;&nbsp;**응용 프로그램 정보 제공 페이지**에서 **웹 응용 프로그램 및/또는 웹 API**를 선택합니다. 응용 프로그램의 키(*클라이언트 암호*라고도 함)를 검색하기 위해 꼭 필요한 과정입니다. Windows 스토어 컬렉션 API 또는 구매 API를 호출하려면 이후 과정에서 Azure AD에서 액세스 토큰을 요청할 때 클라이언트 암호를 입력해야 합니다.

2.  [Azure 관리 포털](http://manage.windowsazure.com/)에서 **Active Directory**로 이동합니다. 디렉터리를 선택하고 맨 위의 **응용 프로그램** 탭을 클릭한 다음 응용 프로그램을 선택합니다.
3.  **구성** 탭을 클릭합니다. 이 탭에서 응용 프로그램에 대한 클라이언트 ID를 가져오고 키(이후 단계에서는 *클라이언트 암호*라고 함)를 요청합니다.
4.  화면 맨 아래에서 **매니페스트 관리**를 클릭합니다. Azure AD 응용 프로그램 매니페스트를 다운로드하고 `"identifierUris"` 섹션을 다음 텍스트로 바꿉니다.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    이러한 문자열은 응용 프로그램이 지원하는 대상 그룹을 나타냅니다. 이후 단계에서 이러한 대상 값에 각각 연결된 Azure AD 액세스 토큰을 만듭니다. 응용 프로그램 매니페스트를 다운로드하는 방법에 대한 자세한 내용은 [Azure Active Directory 응용 프로그램 매니페스트 이해](http://go.microsoft.com/fwlink/?LinkId=722500)를 참조하세요.

5.  [Azure 관리 포털](http://manage.windowsazure.com/)에서 응용 프로그램 매니페스트를 저장하고 응용 프로그램에 업로드합니다.

<span id="step-2"/>
### <a name="step-2-associate-your-azure-ad-client-id-with-your-app-in-windows-dev-center"></a>2단계: Windows 개발자 센터에서 Azure AD 클라이언트 ID를 앱에 연결

Windows 스토어 컬렉션 API 또는 구매 API를 사용하여 앱 또는 추가 기능을 실행하려면 Windows 개발자 센터 대시보드에서 Azure AD 클라이언트 ID를 앱에 연결해야 합니다.

>**참고**&nbsp;&nbsp;이 작업은 한 번만 수행하면 됩니다.

1.  [Windows 개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인하고 앱을 선택합니다.
2.  **서비스** &gt; **제품 컬렉션 및 구매** 페이지로 이동하여 제공되는 필드 중 하나에 Azure AD 클라이언트 ID를 입력합니다.

<span id="step-3"/>
### <a name="step-3-create-azure-ad-access-tokens"></a>3단계: Azure AD 액세스 토큰 만들기

Windows 스토어 ID 키를 검색하거나 Windows 스토어 컬렉션 API 또는 구매 API를 호출하려면 서비스에서 게시자 ID를 나타내는 여러 Azure AD 액세스 토큰을 만들어야 합니다. 각 토큰은 서로 다른 API에 사용됩니다. 각 토큰의 수명은 60분이며 만료된 후 새로 고칠 수 있습니다.

<span id="access-tokens" />
#### <a name="understanding-the-different-tokens-and-audience-uris"></a>여러 토큰 및 대상 그룹 URI의 이해

Windows 스토어 컬렉션 API 또는 구매 API에서 호출하려는 메서드에 따라 두 가지 또는 세 가지 토큰을 만들어야 합니다. 각 액세스 토큰은 서로 다른 대상 그룹 URI(이전에 개발자가 Azure AD 응용 프로그램 매니페스트의 `"identifierUris"` 섹션에 추가한 것과 동일한 URI)에 연결됩니다.

  * 어떤 경우든 `https://onestore.microsoft.com` 대상 그룹 URI를 사용하여 토큰을 만들어야 합니다. 나중에 Windows 스토어 컬렉션 API 또는 구매 API에서 이 토큰을 메서드의 **권한 부여** 헤더로 전달할 것입니다.

  > **중요**&nbsp;&nbsp;`https://onestore.microsoft.com` 대상 그룹에는 서비스에 안전하게 저장된 액세스 토큰만 사용하세요. 이 대상 그룹의 액세스 토큰을 서비스 외부에 노출시키면 서비스 재생 공격에 취약해질 수 있습니다.

  * Windows 스토어 컬렉션 API에서 메서드를 호출하여 [사용자가 소유한 제품에 대해 쿼리](query-for-products.md)하거나 [소모품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)하려는 경우에도 `https://onestore.microsoft.com/b2b/keys/create/collections` 대상 그룹 URI를 사용하여 토큰을 만들어야 합니다. 나중에 Windows SDK의 클라이언트 메서드에 이 토큰을 전달하여 Windows 스토어 컬렉션 API와 함께 사용할 수 있는 Windows 스토어 ID 키를 요청할 것입니다.

  * Windows 스토어 구매 API에서 메서드를 호출하여 [사용자에게 무료 제품에 대한 권리를 부여](grant-free-products.md)하려는 경우에도 `https://onestore.microsoft.com/b2b/keys/create/purchase` 대상 그룹 URI를 사용하여 토큰을 만들어야 합니다. 나중에 Windows SDK의 클라이언트 메서드에 이 토큰을 전달하여 Windows 스토어 구매 API와 함께 사용할 수 있는 Windows 스토어 ID 키를 요청할 것입니다.

<span />
#### <a name="how-to-create-the-tokens"></a>토큰을 만드는 방법

액세스 토큰을 만들려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service)의 지침에 따라 서비스에서 OAuth 2.0 API를 사용하여 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점으로 HTTP POST를 보냅니다. 다음은 샘플 요청입니다.

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

* *client\_id* 및 *client\_secret* 매개 변수의 경우 [Azure 관리 포털](http://manage.windowsazure.com)에서 검색한 응용 프로그램의 클라이언트 ID 및 클라이언트 암호를 지정합니다. Windows 스토어 컬렉션 API 또는 구매 API에서 요구하는 인증 수준의 액세스 토큰을 만들려면 두 매개 변수가 모두 필요합니다.

* *리소스* 매개 변수의 경우 만들려는 액세스 토큰의 종류에 따라 [이전 섹션](#access-tokens)에 나열된 대상 그룹 URI 중 하나를 지정합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다. 액세스 토큰의 구조에 대한 자세한 내용은 [지원되는 토큰 및 클레임 유형](http://go.microsoft.com/fwlink/?LinkId=722501)을 참조하세요.

> **중요**&nbsp;&nbsp;앱이 아닌 서비스의 컨텍스트에서만 Azure AD 액세스 토큰을 만들어야 합니다. 앱에 전송되면 클라이언트 암호가 손상될 수 있습니다.

<span id="step-4"/>
### <a name="step-4-create-a-windows-store-id-key"></a>4단계: Windows 스토어 ID 키 만들기

Windows 스토어 컬렉션 API 또는 구매 API에서 메서드를 호출하려면 서비스에서 Windows 스토어 ID 키를 만들어야 합니다. 이것은 액세스하려는 제품 소유권 정보를 소유한 사용자 ID를 나타내는 JWT(JSON 웹 토큰)입니다. 이 키의 클레임에 대한 자세한 내용은 [Windows 스토어 ID 키 클레임](#claims)을 참조하세요.

현재 Windows 스토어 ID 키를 만드는 유일한 방법은 앱의 클라이언트 코드에서 UWP(유니버설 Windows 플랫폼) API를 호출하는 것입니다. 생성된 키는 현재 디바이스에서 Windows 스토어에 로그인한 사용자의 ID를 나타냅니다.

> **참고**&nbsp;&nbsp;각 Windows 스토어 ID 키는 90일 동안 유효합니다. 키가 만료되면 [키를 갱신](renew-a-windows-store-id-key.md)할 수 있습니다. 새로 만들기보다는 Windows 스토어 ID 키를 갱신하는 것이 좋습니다.

<span />
#### <a name="to-create-a-windows-store-id-key-for-the-windows-store-collection-api"></a>Windows 스토어 컬렉션 API에 대한 Windows 스토어 ID 키를 만들려면

다음 단계에 따라 Windows 스토어 컬렉션 API와 함께 사용할 수 있는 Windows 스토어 ID 키를 만들어서 [사용자가 소유한 제품에 대해 쿼리](query-for-products.md)하거나 [소모품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)합니다.

1.  서비스에서 `https://onestore.microsoft.com/b2b/keys/create/collections` 대상 그룹 URI를 사용하여 만든 Azure AD 액세스 토큰을 클라이언트 앱으로 전달합니다.

2.  앱 코드에서 다음 메서드 중 하나를 호출하여 Windows 스토어 ID 키를 검색합니다.

  * 앱에서 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx) 메서드를 사용합니다.

  * 앱에서 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스의 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674) 메서드를 사용합니다.

  메서드의 *serviceTicket* 매개 변수에 Azure AD 액세스 토큰을 전달합니다. 선택 사항으로, 서비스의 컨텍스트에서 현재 사용자를 식별하는 *publisherUserId* 매개 변수에 ID를 전달할 수 있습니다. 서비스의 사용자 ID를 유지하는 경우 이 매개 변수를 사용하여 이러한 사용자 ID와 Windows 스토어 컬렉션 API 호출의 상관 관계를 지정할 수 있습니다.

3.  앱에서 Windows 스토어 ID 키를 성공적으로 검색하면 키를 다시 서비스로 전달합니다.

<span />
#### <a name="to-create-a-windows-store-id-key-for-the-windows-store-purchase-api"></a>Windows 스토어 구매 API에 대한 Windows 스토어 ID 키를 만들려면

다음 단계에 따라 Windows 스토어 구매 API에서 사용할 수 있는 Windows 스토어 ID 키를 만들어서 [사용자에게 무료 제품에 대한 권리를 부여](grant-free-products.md)합니다.

1.  서비스에서 `https://onestore.microsoft.com/b2b/keys/create/purchase` 대상 그룹 URI를 사용하여 만든 Azure AD 액세스 토큰을 클라이언트 앱으로 전달합니다.

2.  앱 코드에서 다음 메서드 중 하나를 호출하여 Windows 스토어 ID 키를 검색합니다.

  * 앱에서 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)의 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx) 메서드를 사용합니다.

  * 앱에서 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스에 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675) 메서드를 사용합니다.

  메서드의 *serviceTicket* 매개 변수에 Azure AD 액세스 토큰을 전달합니다. 선택 사항으로, 서비스의 컨텍스트에서 현재 사용자를 식별하는 *publisherUserId* 매개 변수에 ID를 전달할 수 있습니다. 서비스의 사용자 ID를 유지하는 경우 이 매개 변수를 사용하여 이러한 사용자 ID와 Windows 스토어 컬렉션 API 호출의 상관 관계를 지정할 수 있습니다.

3.  앱에서 Windows 스토어 ID 키를 성공적으로 검색하면 키를 다시 서비스로 전달합니다.

<span id="step-5"/>
### <a name="step-5-call-the-windows-store-collection-api-or-purchase-api-from-your-service"></a>5단계: 서비스에서 Windows 스토어 컬렉션 API 또는 구매 API 호출

서비스에서 특정 사용자의 제품 소유권 정보에 액세스할 수 있게 해주는 Windows 스토어 ID 키를 만든 후에는 서비스에서 다음 지침에 따라 Windows 스토어 컬렉션 API 또는 구매 API를 호출할 수 있습니다.

* [제품에 대한 쿼리](query-for-products.md)
* [소모품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)

각 시나리오에 대해 다음 정보를 API로 전달합니다.

-   `https://onestore.microsoft.com` 대상 그룹 URI를 사용하여 앞에서 만든 Azure AD 액세스 토큰입니다. 이 토큰은 게시자 ID를 나타냅니다. 이 토큰을 요청 헤더에 전달합니다.
-   앱의 클라이언트 쪽 코드에서 검색된 Windows 스토어 ID 키입니다. 이 키는 액세스하려는 제품 소유권 정보의 소유자인 사용자의 ID를 나타냅니다.

<span id="claims"/>
## <a name="claims-in-a-windows-store-id-key"></a>Windows 스토어 ID 키 클레임

Windows 스토어 ID 키는 액세스하려는 제품 소유권 정보의 소유자인 사용자의 ID를 나타내는 JWT(JSON 웹 토큰)입니다. Base64를 사용하여 디코딩하면 Windows 스토어 ID 키에 다음 클레임이 포함됩니다.

* `iat`:&nbsp;&nbsp;&nbsp;키가 발급된 시간을 식별합니다. 이 클레임은 토큰의 수명을 확인하는 데 사용할 수 있습니다. 이 값은 Epoch 시간으로 표시됩니다.
* `iss`:&nbsp;&nbsp;&nbsp;발급자를 식별합니다. `aud` 클레임과 같은 값입니다.
* `aud`:&nbsp;&nbsp;&nbsp;대상 그룹을 식별합니다. 값은 `https://collections.mp.microsoft.com/v6.0/keys` 또는 `https://purchase.mp.microsoft.com/v6.0/keys` 중 하나여야 합니다.
* `exp`:&nbsp;&nbsp;&nbsp;키 갱신을 제외한 작업 처리에 키가 더 이상 허용되지 않는 만료 시간을 식별합니다. 이 클레임의 값은 Epoch 시간으로 표시됩니다.
* `nbf`:&nbsp;&nbsp;&nbsp;처리 작업에 토큰이 허용되는 시간을 식별합니다. 이 클레임의 값은 Epoch 시간으로 표시됩니다.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;개발자를 식별하는 클라이언트 ID입니다.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Windows 스토어 서비스에만 사용되는 정보를 포함한 불투명 페이로드(암호화되고 Base64 인코딩된)입니다.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;서비스의 컨텍스트에서 현재 사용자를 식별하는 사용자 ID입니다. [키를 만들 때 사용하는 메서드](#step-4)의 선택적 *publisherUserId* 매개 변수로 전달하는 값과 동일한 값입니다.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;키를 갱신하는 데 사용할 수 있는 URI입니다.

다음은 디코딩된 Windows 스토어 ID 키 헤더의 예입니다.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

디코드된 Windows 스토어 ID 키 클레임 집합의 예는 다음과 같습니다.

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

* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)
* [Windows 스토어 ID 키 갱신](renew-a-windows-store-id-key.md)
* [응용 프로그램과 Azure Active Directory 통합](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Azure Active Directory 응용 프로그램 매니페스트 이해]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [지원되는 토큰 및 클레임 유형](http://go.microsoft.com/fwlink/?LinkId=722501)

