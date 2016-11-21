---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "앱과 추가 기능 카탈로그가 있는 경우 Windows 스토어 컬렉션 API 및 Windows 스토어 구매 API를 사용하여 서비스에서 이러한 제품에 대한 소유권 정보에 액세스할 수 있습니다."
title: "서비스에서 제품 보기 및 권한 부여"
translationtype: Human Translation
ms.sourcegitcommit: 819843c8ba1e4a073f70f7de36fe98dd4087cdc6
ms.openlocfilehash: 86d7383a7288434ec613244e4c2ba610889a45fb

---

# 서비스에서 제품 보기 및 권한 부여

앱과 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 카탈로그가 있는 경우 *Windows 스토어 컬렉션 API* 및 *Windows 스토어 구매 API*를 사용하여 서비스에서 이러한 제품에 대한 소유권 정보에 액세스할 수 있습니다.

이러한 API는 플랫폼 간 서비스에서 지원하는 추가 기능 카탈로그와 개발자가 사용하도록 설계된 REST 메서드로 구성됩니다. 이러한 API를 사용하여 다음 작업을 수행할 수 있습니다.

-   Windows 스토어 컬렉션 API: 지정된 사용자가 소유한 앱 및 추가 기능을 쿼리하거나 소모성 제품을 처리됨으로 보고합니다.
-   Windows 스토어 구매 API: 지정된 사용자에게 무료 앱 또는 추가 기능을 허가합니다.

## Windows 스토어 컬렉션 API 및 Windows 스토어 구매 API 사용


Windows 스토어 컬렉션 API 및 구매 API는 고객 소유권 정보에 액세스하기 위해 Azure AD(Active Directory) 인증을 사용합니다. 이러한 API를 호출하기 전에 Azure AD 메타데이터를 Windows 개발자 센터 대시보드에 있는 응용 프로그램에 적용시키고 필요한 여러 액세스 토큰 및 키를 생성해야 합니다. 다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [Azure AD에서 웹 응용 프로그램을 구성합니다](#step-1). 이 응용 프로그램은 Azure AD 컨텍스트에서 플랫폼 간 서비스를 나타냅니다.
2.  [Windows 개발자 센터 대시보드에서 Azure AD 클라이언트 ID와 응용 프로그램을 연결합니다](#step-2).
3.  서비스에서 게시자 ID를 나타내는 [Azure AD 액세스 토큰을 생성](#step-3)합니다.
4.  Windows 앱의 클라이언트 쪽 코드에서 현재 사용자의 ID를 나타내는 [Windows 스토어 ID 키를 생성](#step-4)하고 Windows 스토어 ID 키를 다시 사용자의 서비스로 전달합니다.
5.  필요한 Azure AD 액세스 토큰 및 Windows 스토어 ID 키가 있으면 [서비스에서 Windows 스토어 컬렉션 API를 호출합니다](#step-5).

다음 섹션에서는 이러한 각 단계에 대한 자세한 내용을 제공합니다.

<span id="step-1"/>
### 1단계: Azure AD에서 웹 응용 프로그램 구성

1.  [응용 프로그램과 Azure Active Directory 통합](http://go.microsoft.com/fwlink/?LinkId=722502)의 지침에 따라 웹 응용 프로그램을 Azure AD에 추가합니다.

    > **참고**&nbsp;&nbsp;**응용 프로그램 정보 제공 페이지**에서 **웹 응용 프로그램 및/또는 웹 API**를 선택했는지 확인합니다. 이 작업을 통해 응용 프로그램에 대한 키(*클라이언트 암호*라고도 함)를 가져올 수 있습니다. Windows 스토어 컬렉션 API를 호출하려면 이후 단계에서 Azure AD의 액세스 토큰을 요청할 때 클라이언트 암호를 제공해야 합니다.

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

    이러한 문자열은 응용 프로그램이 지원하는 대상 그룹을 나타냅니다. 이후 단계에서 이러한 대상 값에 각각 연결된 Azure AD 액세스 토큰을 만듭니다. 응용 프로그램 매니페스트를 다운로드하는 방법에 대한 자세한 내용은 [Azure Active Directory 응용 프로그램 매니페스트 이해]( http://go.microsoft.com/fwlink/?LinkId=722500)를 참조하세요.

5.  [Azure 관리 포털](http://manage.windowsazure.com/)에서 응용 프로그램 매니페스트를 저장하고 응용 프로그램에 업로드합니다.

<span id="step-2"/>
### 2단계: Windows 개발자 센터 대시보드에서 Azure AD 클라이언트 ID를 응용 프로그램에 연결

Windows 스토어 컬렉션 API는 Azure AD 클라이언트 ID에 연결한 앱 및 추가 기능에 대한 사용자 소유권 정보에만 액세스 권한을 제공합니다.

1.  [Windows 개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인하고 앱을 선택합니다.
2.  **서비스** &gt; **제품 컬렉션 및 구매** 페이지로 이동하고 Azure AD 클라이언트 ID를 사용 가능한 필드 중 하나에 입력합니다.

<span id="step-3"/>
### 3단계: Azure AD에서 액세스 토큰 검색

Windows 스토어 ID 키를 검색하거나 Windows 스토어 컬렉션 API 또는 구매 API를 호출하기 전에 먼저 Azure AD에서 게시자 ID를 나타내는 세 개의 액세스 토큰을 요청해야 합니다. 이러한 각각의 액세스 토큰은 다른 대상 그룹 URI와 연결되며 각 토큰은 다른 API 호출에 사용됩니다. 각 토큰의 수명은 60분이며 만료된 이후 새로 고칠 수 있습니다.

액세스 토큰을 만들려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지침에 따라 서비스에서 OAuth 2.0 API를 사용합니다. 각 토큰에 대해 다음 매개 변수 데이터를 지정합니다.

-   *client\_id* 및 *client\_secret* 매개 변수에 대해 [Azure 관리 포털](http://manage.windowsazure.com/)에서 가져온 응용 프로그램에 대한 클라이언트 ID 및 클라이언트 암호를 지정합니다. 두 매개 변수 모두 Windows 스토어 컬렉션 API 또는 구매 API가 필요로 하는 인증 수준의 액세스 토큰을 생성하기 위해 필요합니다.
-   *resource* 매개 변수에 대해 다음 앱 ID URI(이전에 응용 프로그램 매니페스트의 `"identifierUris"` 섹션에 추가한 URI와 동일함) 중 하나를 지정합니다. 이 프로세스가 끝날 때 세 개의 액세스 토큰이 있어야 하며 각 토큰에는 이러한 앱 ID URI와 연결된 하나의 URI가 있어야 합니다.
    -   `https://onestore.microsoft.com/b2b/keys/create/collections`: 이후 단계에서 이 URI를 사용하여 만든 액세스 토큰을 사용하여 Windows 스토어 컬렉션 API와 함께 사용할 수 있는 Windows 스토어 ID 키를 요청합니다.
    -   `https://onestore.microsoft.com/b2b/keys/create/purchase`: 이후 단계에서 이 URI를 사용하여 만든 액세스 토큰을 사용하여 Windows 스토어 구매 API와 함께 사용할 수 있는 Windows 스토어 ID 키를 요청합니다.
    -   `https://onestore.microsoft.com`: 이후 단계에서 이 URI를 사용하여 만든 액세스 토큰을 사용하여 Windows 스토어 컬렉션 API 또는 구매 API를 직접 호출합니다.

    > **중요**&nbsp;&nbsp;사용자 서비스에 안전하게 저장된 액세스 토큰을 가진 `https://onestore.microsoft.com` 대상 그룹만 사용합니다. 이 대상 그룹의 액세스 토큰을 서비스 외부에 노출시키면 서비스 재생 공격에 취약해질 수 있습니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다. 액세스 토큰의 구조에 대한 자세한 내용은 [지원되는 토큰 및 클레임 유형](http://go.microsoft.com/fwlink/?LinkId=722501)을 참조하세요.

> **중요**&nbsp;&nbsp;앱이 아닌 서비스의 컨텍스트에서만 Azure AD 액세스 토큰을 만들어야 합니다. 앱에 전송되면 클라이언트 암호가 손상될 수 있습니다.

<span id="step-4"/>
### 4단계: 앱의 클라이언트 쪽 코드에서 Windows 스토어 ID 키 생성

Windows 스토어 컬렉션 API 또는 구매 API를 호출하려면 Windows 스토어 ID 키를 가져와야 합니다. 이것은 액세스하려는 제품 소유권 정보의 소유자인 사용자의 ID를 나타내는 JWT(JSON 웹 토큰)입니다. 이 키의 클레임에 대한 자세한 내용은 [Windows 스토어 ID 키에서 클레임](#claims)을 참조하세요.

현재 Windows 스토어 ID 키를 얻는 유일한 방법은 Windows 스토어에 현재 로그인한 사용자의 ID를 검색하여 앱의 클라이언트 쪽 코드에서 UWP(유니버설 Windows 플랫폼) API를 호출하는 것입니다. Windows 스토어 ID 키를 생성하려면:

1.  서비스에서 다음 액세스 토큰 중 하나를 클라이언트 앱에 전달합니다.

  * Windows 스토어 컬렉션 API와 함께 사용할 수 있는 Windows 스토어 ID 키를 얻으려면 `https://onestore.microsoft.com/b2b/keys/create/collections` 대상 URI를 사용하여 생성한 Azure AD 액세스 토큰을 전달합니다.

  * Windows 스토어 구매 API와 함께 사용할 수 있는 Windows 스토어 ID 키를 얻으려면 `https://onestore.microsoft.com/b2b/keys/create/purchase` 대상 URI를 사용하여 생성한 Azure AD 액세스 토큰을 전달합니다.

2.  앱 코드에서 다음 메서드 중 하나를 호출하여 Windows 스토어 ID 키를 검색합니다.

  * 앱이 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스의 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 Windows 스토어 컬렉션 API를 사용하려고 하면 [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx) 메서드를 사용하고, Windows 스토어 구매 API를 사용하려고 하면 [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx) 메서드를 사용합니다.

  * 앱이 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스의 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 클래스를 사용하여 앱에서 바로 구매를 관리하는 경우 Windows 스토어 컬렉션 API를 사용하려고 하면 [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674) 메서드를 사용하고, Windows 스토어 구매 API를 사용하려고 하면 [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675) 메서드를 사용합니다.

    모든 메서드에서 Azure AD 액세스 토큰을 해당 메서드의 *serviceTicket* 매개 변수에 전달합니다. 서비스의 컨텍스트에서 현재 사용자를 식별하는 *publisherUserId* 매개 변수에 ID를 선택적으로 전달할 수 있습니다. 서비스에 대한 사용자 ID를 유지하는 경우 이 매개 변수를 사용하여 이러한 사용자 ID를 Windows 스토어 컬렉션 API 또는 구매 API 호출과 연결할 수 있습니다.

    >**참고**&nbsp;&nbsp;**Windows.Services.Store** 및 **Windows.ApplicationModel.Store** 네임스페이스 간 차이점에 대한 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

3.  앱에서 Windows 스토어 ID 키를 성공적으로 검색한 후 키를 서비스로 다시 전달합니다.

> **참고**&nbsp;&nbsp;각 Windows 스토어 ID 키는 90일 동안 유효합니다. 키가 만료된 후 [키를 갱신](renew-a-windows-store-id-key.md)할 수 있습니다. 새로 만들기보다는 Windows 스토어 ID 키를 갱신하는 것이 좋습니다.

<span id="step-5"/>
### 5단계: 서비스에서 Windows 스토어 컬렉션 API 또는 구매 API 호출

서비스에 특정 사용자의 제품 소유권 정보에 액세스할 수 있게 하는 Windows 스토어 ID 키를 만든 후 서비스에서 Windows 스토어 컬렉션 API 또는 구매 API를 호출할 수 있습니다. 시나리오에 적용되는 지침을 사용합니다.

-   [제품에 대한 쿼리](query-for-products.md)
-   [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
-   [무료 제품에 대한 권한 부여](grant-free-products.md)

각 시나리오에 대해 다음 정보를 API로 전달합니다.

-   `https://onestore.microsoft.com` 대상 그룹 URI를 사용하여 앞에서 만든 Azure AD 액세스 토큰입니다. 이 토큰은 게시자 ID를 나타냅니다. 이 토큰을 요청 헤더에 전달합니다.
-   앱의 클라이언트 쪽 코드에서 검색된 Windows 스토어 ID 키입니다. 이 키는 액세스하려는 제품 소유권 정보의 소유자인 사용자의 ID를 나타냅니다.

<span id="claims"/>
## Windows 스토어 ID 키 클레임

Windows 스토어 ID 키는 액세스하려는 제품 소유권 정보의 소유자인 사용자의 ID를 나타내는 JWT(JSON 웹 토큰)입니다. Base64를 사용하여 디코드하면 Windows 스토어 ID 키는 다음 클레임을 포함합니다.

| 클레임 이름            | 설명          |
|---------------|-------------|
| iat                 | 키가 발급된 시간을 식별합니다. 이 클레임은 토큰의 수명을 확인하는 데 사용할 수 있습니다. 이 값은 시기 시간으로 표현됩니다.           |
| iss                    | 발급자를 식별합니다. 이것은 *aud* 클레임과 같은 값입니다.      |
| aud           | 대상 그룹을 식별합니다. 값은 `https://collections.mp.microsoft.com/v6.0/keys` 또는 `https://purchase.mp.microsoft.com/v6.0/keys` 중 하나여야 합니다.  |
| exp        | 키 갱신을 제외한 작업을 처리할 때 키가 더 이상 허용되지 않게 되는 만료 시간을 식별합니다. 이 클레임의 값은 시기 시간으로 표시됩니다.       |
| nbf                | 처리를 위해 토큰이 허용되는 시간을 식별합니다. 이 클레임의 값은 시기 시간으로 표시됩니다. |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`   | 개발자를 식별하는 클라이언트 ID입니다.     |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`    | Windows 스토어 서비스에만 사용되는 정보를 포함한 불투명 페이로드(암호화되고 Base64 인코딩된)입니다.   |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`     | 서비스의 컨텍스트에서 현재 사용자를 식별하는 사용자 ID입니다. 이것은 [키를 생성할 때 사용하는 메서드](view-and-grant-products-from-a-service.md#step-4)의 선택적 *publisherUserId* 매개 변수로 전달하는 동일한 값입니다. |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri` | 키를 갱신하는 데 사용할 수 있는 URI입니다.                                                                                                                 
디코드된 Windows 스토어 ID 키 헤더의 예는 다음과 같습니다.

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

## 관련 항목

* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)
* [Windows 스토어 ID 키 갱신](renew-a-windows-store-id-key.md)
* [응용 프로그램과 Azure Active Directory 통합](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Azure Active Directory 응용 프로그램 매니페스트 이해]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [지원되는 토큰 및 클레임 유형](http://go.microsoft.com/fwlink/?LinkId=722501)
 

 



<!--HONumber=Nov16_HO1-->


