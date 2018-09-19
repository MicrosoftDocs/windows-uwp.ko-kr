---
author: JnHs
description: Windows 앱에서 실행되는 앱용 광고 캠페인을 만드는 것 외에 다른 채널을 사용하여 앱을 홍보할 수도 있습니다.
title: 사용자 지정 앱 홍보 캠페인 만들기
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: wdg-dev-content
ms.date: 09/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 사용자 지정, 앱, 홍보, 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: 13ee8d7482a2ce0716d4e133af329cd0ea42c184
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052403"
---
# <a name="create-a-custom-app-promotion-campaign"></a>사용자 지정 앱 홍보 캠페인 만들기

Windows 앱에서 실행되는 [앱용 광고 캠페인](create-an-ad-campaign-for-your-app.md)을 만든 후 다른 채널을 사용하여 앱을 홍보할 수도 있습니다. 예를 들어 타사 앱 마케팅 공급자를 사용하여 앱을 홍보하거나 소셜 미디어 사이트에 앱의 링크를 게시할 수 있습니다. 이러한 활동을 *사용자 지정 캠페인*이라고 합니다.

앱에서 사용자 지정 캠페인을 실행하는 경우, 각 URL에 서로 다른 *캠페인 ID*를 포함하여 각 사용자 지정 캠페인에 대해 다른 URL을 만들어 각 캠페인의 상대적인 성과를 추적할 수 있습니다. Windows 10을 실행하는 고객이 캠페인 ID가 포함된 URL을 클릭하면 Microsoft에서 이 클릭을 해당 사용자 지정 캠페인과 연결하고 이 데이터를 제공합니다.

> [!IMPORTANT]
> Windows 10 고객에 대해서만 이 데이터가 추적됩니다. 다른 운영 체제를 사용하는 고객도 앱 목록 링크를 따라갈 수 있지만 해당 고객의 활동에 대한 데이터는 포함되지 않습니다.

사용자 지정 캠페인과 연결되는 데이터는 앱의 Store 목록에 대한 *페이지 보기*와 *변환* 두 가지 기본 유형이 있습니다. 변환은 고객이 사용자 지정 캠페인 ID가 포함된 URL에서 앱의 스토어 목록 페이지를 클릭한 결과로 이루어진 앱 구입입니다. 변환에 대한 자세한 내용은 이 항목의 [앱 구입이 변환으로 인정되는 방법 이해](#understanding-how-acquisitions-qualify-as-conversions)를 참조하세요.

앱에 대한 사용자 지정 캠페인 성과 데이터는 다음과 같은 방식으로 검색할 수 있습니다.

* 개발자 센터 대시보드의 [구입 보고서](acquisitions-report.md)의 **캠페인 ID별 앱 페이지 보기 및 변환** 및 **총 캠페인 변환** 차트에 나와 있는 앱 또는 추가 기능에 대한 페이지 보기 및 변환과 관련된 데이터를 볼 수 있습니다.
* 앱이 UWP(유니버설 Windows 플랫폼) 앱인 경우 Windows SDK의 API를 사용하여 변환으로 이어진 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다.

## <a name="example-custom-campaign-scenario"></a>예제 사용자 지정 캠페인 시나리오

새 게임 빌드를 완료하고 플레이어에게 기존 게임을 홍보하려는 게임 개발자를 예로 들어 보겠습니다. 이 개발자는 게임의 Store 목록에 대한 링크를 포함하여 새 게임 릴리스에 대한 알림을 자신의 Facebook 페이지에 게시합니다. 또한 이 개발자는 많은 플레이어가 Twitter에서 자신을 팔로잉하므로 게임의 Store 페이지 링크가 포함된 알림을 트윗합니다.

이러한 각 홍보 채널의 성공을 추적하기 위해 이 개발자는 게임의 Store 목록에 대한 두 가지 변형을 만듭니다.

* 개발자가 Facebook 페이지에 게시할 URL에는 사용자 지정 캠페인 ID가 포함됩니다. `my-facebook-campaign`

* Twitter에 게시할 URL에는 사용자 지정 캠페인 ID를 포함됩니다. `my-twitter-campaign`

Facebook 및 Twitter 팔로워가 URL을 클릭하면 Microsoft에서 각 클릭을 추적하여 해당 사용자 지정 캠페인에 연결합니다. 이후의 게임 다운로드 및 추가 기능 구매는 사용자 지정 캠페인에 연결되고 변환으로 보고됩니다.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>앱 구입이 변환으로 인정되는 방법 이해

사용자 지정 캠페인 *변환*은 고객이 사용자 지정 캠페인을 통해 홍보된 URL을 클릭한 결과로 이루어진 구입입니다. 개발자 센터 대시보드의 [구입 보고서](acquisitions-report.md)의 **캠페인 ID별 앱 페이지 보기 및 변환** 및 **총 캠페인 변환** 차트에서 변환으로 인정되는 시나리오와 [프로그래밍 방식으로 캠페인 ID 검색](#programmatically)에서 변환으로 인정되는 시나리오는 서로 다릅니다.

### <a name="qualifying-conversions-in-the-dashboard-report"></a>대시보드 보고서에서 변환으로 인정

아래 시나리오는 [구입 보고서](acquisitions-report.md)의 **캠페인 ID별 앱 페이지 보기 및 변환** 및 **총 캠페인 변환** 차트에서 변환으로 인정됩니다.

* *정식 Microsoft 계정이 있거나 없는* 고객이 사용자 지정 캠페인 ID가 포함된 앱 URL을 클릭하면 앱의 스토어 목록으로 리디렉션이 됩니다. 그리고 동일한 고객이 사용자 지정 캠페인 ID를 통해 Microsoft Store URL을 처음 클릭한 후 24시간 이내에 앱을 구입합니다.

* 고객이 사용자 지정 캠페인 ID가 포함된 URL을 클릭한 것과 다른 장치에서 앱을 구입할 경우, URL을 클릭할 때와 같은 Microsoft 계정으로 고객이 로그인을 해야 변환으로 계산됩니다.

> [!NOTE]
> 사용자 지정 캠페인에 대한 변환으로 계산되는 앱 구입의 경우, 해당 앱의 모든 추가 기능 구매는 동일한 사용자 지정 캠페인에 대한 변환으로도 계산됩니다.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>프로그래밍 방식으로 캠페인 ID를 검색할 때 변환 인정

앱과 연결된 캠페인 ID를 프로그래밍 방식으로 검색할 때 변환으로 인정되려면 다음 조건을 충족해야 합니다.

* **Windows 10 버전 1607 이상**을 실행하는 장치: 고객(정식 Microsoft 계정으로 로그인했든 아니든 관계 없음)이 사용자 지정 캠페인 ID가 포함된 URL을 클릭하면 앱의 스토어 목록 페이지로 리디렉션됩니다. 고객이 URL 클릭 결과로 스토어 목록을 보는 동안 앱을 구입합니다.

* **Windows 10 버전 1511 이하**를 실행하는 장치: 고객(반드시 정식 Microsoft 계정으로 로그인해야 함)이 사용자 지정 캠페인 ID가 포함된 URL을 클릭하면 앱의 스토어 목록 페이지로 리디렉션됩니다. 고객이 URL 클릭 결과로 스토어 목록을 보는 동안 앱을 구입합니다. 이러한 버전의 Windows 10에서는 프로그래밍 방식으로 캠페인 ID를 검색할 때 변환으로 인정되려면 사용자가 정식 Microsoft 계정으로 로그인해야 합니다.

> [!NOTE]
> 고객이 스토어 목록 페이지를 떠나지만 24시간 내에 페이지로 돌아와서(같은 Microsoft 계정으로 로그인할 때 같은 장치나 다른 장치에서) 앱을 구입하는 경우, 이렇게 하면 [구입 보고서](acquisitions-report.md)의 **캠페인 ID별 앱 페이지 보기 및 변환** 및 **총 캠페인 변환**에서 변환으로 인정이 **됩니다**. 그러나 캠페인 ID를 프로그래밍 방식으로 검색하는 경우에는 변환으로 인정되지 **않습니다**.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>앱의 Microsoft Store 페이지 URL에 사용자 지정 캠페인 ID를 포함

사용자 지정 캠페인 ID를 사용하여 앱의 Microsoft Store 페이지 URL을 만들려면

1.  사용자 지정 캠페인에 대한 ID 문자열을 만듭니다. 이 문자열은 최대 100자를 포함할 수 있지만 식별하기 쉽도록 짧은 캠페인 ID를 정의하는 것이 좋습니다.

   > [!NOTE]
   > 앱에 대한 [인수 보고서](acquisitions-report.md)를 볼 때 캠페인 ID 문자열이 다른 개발자에게 표시될 수 있습니다. 고객이 사용자 지정 캠페인 ID를 클릭하여 스토어에 들어가서 같은 세션 내에서 다른 개발자의 앱을 구입했기 때문에 캠페인 ID로 인한 변환으로 인정된 경우가 여기에 해당합니다. 개발자는 캠페인 ID 이름을 포함하여 얼마나 많은 개발자 앱의 변환이 캠페인 ID의 첫 클릭으로 인해 발생했는지 확인할 수 있지만, 캠페인 ID를 클릭한 후에 얼마나 많은 사용자가 개발자 앱(또는 다른 개발자의 앱)을 구매했는지에 대한 데이터는 확인할 수 없습니다.

2.  앱의 Store 목록에 대한 링크를 HTML 또는 프로토콜 형식으로 가져옵니다.

    * 고객이 어떤 운영 체제에서든 브라우저에서 앱의 웹 기반 스토어 목록을 탐색할 수 있도록 하려면 HTML URL을 사용합니다. 또한 Windows 장치에서 스토어 앱이 시작되고 앱 목록을 표시합니다. 이 URL 형식은 **`https://www.microsoft.com/store/apps/*your app ID*`** 입니다. 예를 들어 Skype의 HTML URL은 `https://www.microsoft.com/store/apps/9wzdncrfj364`입니다. [앱 ID](view-app-identity-details.md#link-to-your-apps-listing) 페이지에서 이 URL을 확인할 수 있습니다.

    * UWP 앱이 설치된 장치나 컴퓨터에서 실행되는 다른 Windows 앱 내에서 앱을 홍보하거나 고객이 Microsoft Store를 지원하는 장치에 있다는 것을 알고 있는 경우 프로토콜 형식을 사용합니다. 이 링크를 사용하면 브라우저를 열지 않고도 앱의 스토어 목록에 직접 연결할 수 있습니다. 이 URL 형식은 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`** 입니다. 예를 들어 Skype의 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`입니다.

3.  앱의 URL 끝에 다음 문자열을 추가합니다.

    * HTML 형식 URL의 경우 **`?cid=*my custom campaign ID*`** 를 추가합니다. 예를 들어 Skype에서 값이 **custom\_campaign**인 캠페인 ID를 소개하는 경우 해당 캠페인 ID가 포함된 새 URL은 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`이 됩니다.

    * 프로토콜 형식 URL의 경우 **`&cid=*my custom campaign ID*`** 를 추가합니다. 예를 들어 Skype에서 값이 **custom\_campaign**인 캠페인 ID를 소개하는 경우 해당 캠페인 ID를 포함하는 새 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`입니다.

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>프로그래밍 방식으로 앱의 사용자 지정 캠페인 ID 검색

앱이 UWP 앱인 경우, Windows SDK의 API를 사용하여 앱 구입과 연결된 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다. 이러한 API는 많은 분석 및 수익 창출 시나리오를 가능하게 합니다. 예를 들어 현재 사용자가 Facebook 캠페인을 통해 검색한 후 앱을 다운로드했는지 확인한 다음 그에 따라 앱 환경을 사용자 지정할 수 있습니다. 또는 타사 앱 마케팅 공급자를 사용하는 경우 해당 공급자에게 데이터를 다시 보낼 수 있습니다.

이러한 API는 고객이 캠페인 ID가 포함된 URL을 클릭하고 앱의 Microsoft Store 페이지를 확인한 후 Store 목록 페이지를 나가지 않고 앱을 구입한 경우에만 캠페인 ID 문자열을 반환합니다. 사용자가 페이지를 벗어났다가 나중에 돌아와 앱을 받은 경우에는 이러한 API를 사용했을 때 [변환으로 인정](#conversions)되지 않습니다.

앱이 대상으로 하는 Windows 10의 버전에 따라 사용할 수 있는 API가 다릅니다.

* Windows 10 버전 1607 이상: **Windows.Services.Store** 네임스페이스의 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스를 사용합니다. 이 API를 사용하는 경우, 사용자가 정식 Microsoft 계정으로 로그인을 하는지 여부에 관계 없이 모든 [적격 취득](#conversions)의 사용자 지정 캠페인 ID를 검색할 수 있습니다.

* Windows 10 버전 1511 이하: **Windows.ApplicationModel.Store** 네임스페이스의 [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스를 사용합니다. 이 API를 사용하는 경우, 사용자가 정식 Microsoft 계정으로 로그인한 경우에만 [적격 취득](#conversions)의 사용자 지정 캠페인 ID를 검색할 수 있습니다.

> [!NOTE]
> **Windows.ApplicationModel.Store** 네임스페이스는 모든 버전의 Windows 10에서 사용할 수 있지만, 앱이 Windows 10 버전 1607 이상을 대상으로 할 경우 **Windows.Services.Store** 네임스페이스의 API를 사용하는 것이 좋습니다. 이러한 네임스페이스 차이점에 대한 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md#choose-namespace)을 참조하세요. 다음 코드 예제는 같은 프로젝트에서 두 API 모두를 사용하도록 코드를 구성하는 방법을 보여줍니다.

### <a name="code-example"></a>코드 예제

다음 코드 예제는 사용자 지정 캠페인 ID를 검색하는 방법을 보여줍니다. 이 예제는 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 사용하여 **Windows.Services.Store**와 **Windows.ApplicationModel.Store** 네임스페이스의 API 모두를 사용합니다. 이 프로세스를 따라 모든 버전의 Windows 10에서 코드를 실행할 수 있습니다. 최소 OS 버전이 더 이전 버전일 수는 있지만, 이 코드를 사용하려면 프로젝트의 대상 OS 버전이 **Windows 10 Anniversary Edition(10.0, 빌드 14394)** 이상이어야 합니다.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

이 코드는 다음 작업을 수행합니다.

1. 먼저, 현재 장치에서 **Windows.Services.Store** 네임스페이스의 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스를 사용할 수 있는지 확인합니다. 이는 장치에서 Windows 10 버전 1607을 이상을 실행함을 의미합니다. 그렇다면 코드가 이 클래스 사용하여 진행됩니다.

2. 그런 다음, 현재 사용자가 정식 Microsoft 계정이 있는 경우에 대해 사용자 지정 캠페인 ID 검색을 시도합니다. 이를 위해 코드는 현재 앱 SKU를 나타내는 [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) 개체를 가져온 다음, 캠페인 ID가 있을 경우 이를 검색하기 위해 [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId) 속성에 액세스합니다.
3. 그런 다음 현재 사용자가 정식 Microsoft 계정이 없는 경우에 대한 캠페인 ID 검색을 시도합니다. 이 경우 캠페인 ID는 앱 라이선스에 포함되어 있습니다. 코드는 [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) 메서드를 사용하여 라이선스를 검색하고 라이선스의 JSON 콘텐츠를 구문 분석하여 *customPolicyField1*이라는 이름의 키 값을 찾습니다. 이 값은 캠페인 ID를 포함합니다.

4. **Windows.Services.Store** 네임스페이스의 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스를 사용할 수 없는 경우, 코드는 대신 **Windows.ApplicationModel.Store** 네임스페이스(이 네임스페이스는 버전 1511 이하를 포함하여 모든 버전의 Windows 10에서 사용할 수 있음)의 [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) 메서드를 사용하여 사용자 지정 캠페인 ID를 검색합니다. 이 메서드를 사용하는 경우, 사용자가 정식 Microsoft 계정이 있는 [적격 취득](#conversions)의 사용자 지정 캠페인 ID만 검색할 수 있음을 참고하십시오.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Windows.ApplicationModel.Store 네임스페이스에 대한 프록시 파일에서 캠페인 ID를 지정

**Windows.ApplicationModel.Store** 네임스페이스에는 스토어에 앱을 제출하기 전에 코드 테스트를 위해 스토어 작업을 시뮬레이션하는 특수 클래스인 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator)가 포함되어 있습니다. 이 클래스는 [Windows.StoreProxy.xml이라는 로컬 파일](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator)에서 데이터를 검색합니다. 이전 코드 예제에서 **CurrentApp**과 **CurrentAppSimulator** 모두를 프로젝트의 디버그 및 비 디버그 코드 사용하는 방법을 볼 수 있습니다. 디버그 환경에서 이 코드를 테스트하려면, 다음 예제와 같이 개발 컴퓨터의 WindowsStoreProxy.xml 파일에 **AppPurchaseCampaignId** 요소를 추가합니다. 앱을 실행하면 [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) 메서드는 항상 이 값을 반환합니다.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

**Windows.Services.Store** 네임스페이스는 테스트 중에 라이선스 정보를 시뮬레이션하는 데 사용할 수 있는 클래스를 제공하지 않습니다. 대신에 개발자가 스토어에 앱을 게시하고 개발 장치에 앱을 다운로드하여 해당 라이선스를 테스트에 사용해야 합니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md#testing)을 참조하세요.

## <a name="test-your-custom-campaign"></a>사용자 지정 캠페인 테스트

사용자 지정 캠페인 URL을 홍보하기 전에 다음을 수행하여 사용자 지정 캠페인을 테스트하는 것이 좋습니다.

1.  테스트에 사용하고 있는 장치에서 Microsoft 계정으로 로그인합니다.

2.  사용자 지정 캠페인 URL을 클릭합니다. 앱 페이지로 이동이 되었는지 확인한 다음, UWP 앱 또는 브라우저 페이지를 닫습니다.

3.  URL을 여러 번 클릭하고 앱 페이지를 방문할 때마다 UWP 앱 또는 브라우저 페이지를 닫습니다. 앱 페이지 방문 중 **하나** 동안 앱을 구입하여 변환을 생성합니다. URL을 클릭한 총 횟수를 계산합니다.

4. 예상된 페이지 보기 및 변환이 개발자 센터 대시보드의 [구입 보고서](acquisitions-report.md)의 **캠페인 ID별 앱 페이지 보기 및 변환** 및 **총 캠페인 변환** 차트에 나타나는지 확인하고, 앱이 코드를 테스트하여 위에서 설명한 API를 사용하여 캠페인 ID를 제대로 검색할 수 있는지 확인합니다.
