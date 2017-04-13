---
author: shawjohn
Description: "Windows 앱에서 실행되는 앱용 광고 캠페인을 만든 후 다른 채널을 사용하여 앱을 홍보할 수도 있습니다."
title: "사용자 지정 앱 홍보 캠페인 만들기"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 사용자 지정, 앱, 홍보, 캠페인"
ms.openlocfilehash: 1e56b1df4b5501ded5db799c3ad67f97c0e1cfcd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-a-custom-app-promotion-campaign"></a>사용자 지정 앱 홍보 캠페인 만들기

Windows 앱에서 실행되는 [앱용 광고 캠페인](create-an-ad-campaign-for-your-app.md)을 만든 후 다른 채널을 사용하여 앱을 홍보할 수도 있습니다. 예를 들어 타사 앱 마케팅 공급자를 사용하여 앱을 홍보하거나 소셜 미디어 사이트에 앱의 링크를 게시할 수 있습니다. 이러한 활동을 *사용자 지정 캠페인*이라고 합니다.

앱의 사용자 지정 캠페인을 실행하는 경우 각 URL에 서로 다른 캠페인 ID를 포함하여 각 사용자 지정 캠페인에 대한 Windows 스토어 앱 URL을 만들어 각 캠페인의 상대적인 성과를 추적할 수 있습니다. Windows 10을 실행하는 고객이 캠페인 ID가 포함된 URL을 클릭하면 Microsoft에서 이 클릭을 해당 사용자 지정 캠페인과 연결하고 이 데이터를 제공합니다.

사용자 지정 캠페인과 연결되는 데이터에는 앱의 방문자 수와 *변환*의 두 가지 주요 유형이 있습니다. 변환은 고객이 사용자 지정 캠페인을 통해 홍보된 URL에서 앱에 대한 Windows 스토어 페이지를 클릭한 결과로 이루어진 앱 구입입니다. 변환에 대한 자세한 내용은 이 항목의 [앱 구입이 변환으로 인정되는 방법 이해](#understanding-how-app-acquisitions-qualify-as-conversions)를 참조하세요.

앱에 대한 사용자 지정 캠페인 성과 데이터는 다음과 같은 방식으로 검색할 수 있습니다.

* 개발자 센터 대시보드의 [채널 및 변환 보고서](channels-and-conversions-report.md)에서 앱 또는 추가 기능에 대한 페이지 보기 및 변환에 대한 데이터를 볼 수 있습니다.
* 앱이 UWP(유니버설 Windows 플랫폼) 앱인 경우 Windows SDK의 API를 사용하여 변환으로 이어진 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다.

> **중요**&nbsp;&nbsp;이 데이터는 Windows 10을 실행하는 고객에 대해서만 추적됩니다. 다른 운영 체제를 사용하는 고객도 앱 목록 링크를 따라갈 수 있지만 해당 고객의 활동에 대한 데이터는 포함되지 않습니다.

## <a name="example-custom-campaign-scenario"></a>예제 사용자 지정 캠페인 시나리오

새 게임 빌드를 완료하고 플레이어에게 기존 게임을 홍보하려는 게임 개발자를 예로 들어 보겠습니다. 이 개발자는 게임의 Windows 스토어 페이지 링크를 포함하여 새 게임 릴리스에 대한 알림을 자신의 Facebook 페이지에 게시합니다. 또한 이 개발자는 많은 플레이어가 Twitter에서 자신을 팔로잉하므로 게임의 Windows 스토어 페이지 링크가 포함된 알림을 트윗합니다.

이러한 각 홍보 채널의 성공을 추적하기 위해 이 개발자는 게임의 Windows 스토어 페이지 URL에 대한 두 가지 변형을 만듭니다.

* Facebook 페이지에 게시할 URL에는 사용자 지정 캠페인 ID `my-facebook-campaign`을 포함합니다.
* Twitter에 게시할 URL에는 사용자 지정 캠페인 ID `my-twitter-campaign`을 포함합니다.

Facebook 및 Twitter 팔로워가 URL을 클릭하면 Microsoft에서 각 클릭을 추적하여 해당 사용자 지정 캠페인에 연결합니다. 이후의 게임 다운로드 및 추가 기능 구매는 사용자 지정 캠페인에 연결되고 변환으로 보고됩니다.

<span id="conversions" />
## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>앱 구입이 변환으로 인정되는 방법 이해

*변환*은 고객이 사용자 지정 캠페인을 통해 홍보된 URL에서 앱에 대한 Windows 스토어 페이지를 클릭한 결과로 이루어진 앱 구입입니다. Windows 개발자 센터 대시보드의 [채널 및 변환 보고서](channels-and-conversions-report.md)에서 변환으로 인정되는 조건과 [프로그래밍 방식으로 캠페인 ID 검색](#programmatically)에서 변환으로 인정되는 시나리오는 서로 다릅니다.

### <a name="qualifying-conversions-for-the-dashboard-report"></a>대시보드 보고서에 대한 적격 변환

다음 시나리오는 [채널 및 변환 보고서](channels-and-conversions-report.md)에서 변환으로 인정됩니다.

* *정식 Microsoft 계정이 있거나 없는* 고객이 사용자 지정 캠페인 ID가 포함된 앱 URL을 클릭하고 앱의 Windows 스토어 페이지로 이동합니다. 이 고객이 사용자 지정 캠페인 ID가 포함된 Windows 스토어 URL을 처음 클릭한 후 24시간 이내에 앱을 구입합니다.

* 고객이 사용자 지정 캠페인 ID가 포함된 Windows 스토어 URL을 클릭한 것과 다른 컴퓨터 또는 장치에서 앱을 구입할 경우 고객에게 Microsoft 계정이 있어야 변환으로 계산됩니다.

> **참고**&nbsp;&nbsp;사용자 지정 캠페인에 대한 변환으로 계산되는 앱 구입의 경우 해당 앱의 모든 추가 기능 구매는 동일한 사용자 지정 캠페인에 대한 변환으로도 계산됩니다.

### <a name="qualifying-conversions-for-programmatically-retrieving-the-campaign-id"></a>프로그래밍 방식의 캠페인 ID 검색에 대한 적격 변환

앱과 연결된 캠페인 ID를 프로그래밍 방식으로 검색할 때 변환으로 인정되려면 다음 조건을 충족해야 합니다.

* **Windows 10 버전 1607 이상**을 실행하는 장치: *정식 Microsoft 계정이 있거나 없는* 고객이 사용자 지정 캠페인 ID가 포함된 앱 URL을 클릭하고 앱의 Windows 스토어 페이지로 이동합니다. 고객은 URL을 클릭한 후 Windows 스토어 페이지 보기에서 앱을 구입합니다.

* **Windows 10 버전 1511 이하**를 실행하는 장치: *정식 Microsoft 계정이 있는* 고객이 사용자 지정 캠페인 ID가 포함된 앱 URL을 클릭하고 앱의 Windows 스토어 페이지로 이동합니다. 고객은 URL을 클릭한 후 Windows 스토어 페이지 보기에서 앱을 구입합니다. 이러한 버전의 Windows 10에서는 프로그래밍 방식으로 캠페인 ID를 검색할 때 변환으로 인정되려면 사용자에게 정식 Microsoft 계정이 있어야 합니다.

>**참조**&nbsp;&nbsp;고객이 페이지를 벗어났다가 24시간 내에 같은 컴퓨터나 장치 또는 다른 컴퓨터나 장치(고객에게 Microsoft 계정이 있는 경우)에서 페이지로 돌아와 앱을 구입하는 경우 이는 [채널 및 변환 보고서](channels-and-conversions-report.md)에서 변환으로 인정됩니다. 그러나 캠페인 ID를 프로그래밍 방식으로 검색하는 경우에는 변환으로 인정되지 않습니다.

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>앱의 Windows 스토어 페이지 URL에 사용자 지정 캠페인 ID를 포함

사용자 지정 캠페인 ID를 사용하여 앱의 Windows 스토어 페이지 URL을 만들려면

1.  사용자 지정 캠페인에 대한 ID 문자열을 만듭니다. 이 문자열은 최대 100자를 포함할 수 있지만 식별하기 쉽도록 짧은 캠페인 ID를 정의하는 것이 좋습니다.

  >**참고**&nbsp;&nbsp;캠페인 ID 문자열이 변환 보고서나 채널 내의 다른 개발자에게 표시될 수 있습니다. 고객이 캠페인 ID를 클릭하여 스토어에 들어갔지만 같은 세션 내에서 다른 개발자의 앱을 구입한 경우가 여기에 해당합니다. 이 경우 캠페인 ID 이름이 다른 개발자에게 표시되더라고 얼마나 많은 개발자 앱의 변환이 해당 캠페인 ID의 첫 클릭으로 인해 발생했는지만 개발자에게 표시됩니다. 캠페인 ID를 클릭하여 얼마나 많은 사용자가 앱을 구입했는지에 대한 데이터를 표시되지 않습니다.

2.  앱의 Windows 스토어 페이지 URL을 HTML 또는 프로토콜 형식으로 가져옵니다.

    * 고객이 브라우저에서 앱의 Windows 스토어 페이지를 탐색할 수 있도록 하려면 HTTP 형식을 사용합니다. 이 URL은 Windows 스토어 앱이 설치된 경우 앱 목록에서 Windows 스토어 앱을 시작합니다. 이 URL 형식은 **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**입니다. 예를 들어 Skype의 HTTP URL은 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`입니다. HTML 형식의 URL은 [개발자 센터 대시보드의 **앱 ID** 페이지](link-to-your-app.md)에서 볼 수 있습니다. 참고로 HTTP 형식의 URL은 Windows 7 이상을 실행하는 컴퓨터와 태블릿 및 Windows Phone 8 이상을 실행하는 휴대폰의 브라우저에서 Windows 스토어를 탐색하는 데 사용될 수 있습니다.

    * Windows 스토어 앱이 설치된 장치나 컴퓨터에서 실행되는 다른 Windows 앱에서 앱을 홍보하고 고객이 Windows 스토어 앱에서 앱 페이지를 열기를 원하는 경우 프로토콜 형식을 사용합니다. 이 URL 형식은 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**입니다. 예를 들어 Skype의 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`입니다.

3.  앱의 URL 끝에 다음 문자열을 추가합니다.

    * HTTP 형식 URL의 경우 **`?cid=*my custom campaign ID*`**를 추가합니다. 예를 들어 Skype에서 값이 **custom\_campaign**인 캠페인 ID를 소개하는 경우 해당 캠페인 ID를 포함하는 새 HTTP URL은 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`입니다.

    * 프로토콜 형식 URL의 경우 **`&cid=*my custom campaign ID*`**를 추가합니다. 예를 들어 Skype에서 값이 **custom\_campaign**인 캠페인 ID를 소개하는 경우 해당 캠페인 ID를 포함하는 새 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`입니다.

<span id="programmatically" />
## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>프로그래밍 방식으로 앱의 사용자 지정 캠페인 ID 검색

앱이 UWP 앱인 경우 Windows SDK의 API를 사용하여 앱과 연결된 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다. 이러한 API는 많은 분석 및 수익 창출 시나리오를 가능하게 합니다. 예를 들어 현재 사용자가 Facebook 캠페인을 통해 검색한 후 앱을 다운로드했는지 확인한 다음 그에 따라 앱 환경을 사용자 지정할 수 있습니다. 또는 타사 앱 마케팅 공급자를 사용하는 경우 해당 공급자에게 데이터를 다시 보낼 수 있습니다.

이러한 API는 고객이 캠페인 ID가 포함된 URL을 클릭하고 앱의 Windows 스토어 페이지로 이동한 후 이 페이지를 나가지 않고 앱을 받은 경우에만 캠페인 ID 문자열을 반환합니다. 사용자가 페이지를 벗어났다가 나중에 돌아와 앱을 받은 경우에는 이러한 API를 사용했을 때 [변환으로 인정](#conversions)되지 않습니다.

앱이 대상으로 하는 Windows 10의 버전에 따라 사용할 수 있는 API가 다릅니다.

* Windows 10 버전 1607 이상: **Windows.Services.Store** 네임스페이스의 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스를 사용합니다. 이 API를 사용하는 경우, 사용자가 정식 Microsoft 계정이 있는지에 관계없이 모든 [적격 취득](#conversions)의 사용자 지정 캠페인 ID를 검색할 수 있습니다.
* Windows 10 버전 1511 이하: **Windows.ApplicationModel.Store** 네임스페이스의 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 클래스를 사용합니다. 이 API를 사용하는 경우, 사용자가 정식 Microsoft 계정이 있는 [적격 취득](#conversions)의 사용자 지정 캠페인 ID만 검색할 수 있습니다.

>**참고**&nbsp;&nbsp;**Windows.ApplicationModel.Store** 네임스페이스는 모든 버전의 Windows 10에서 사용할 수 있지만, 앱이 Windows 10 버전 1607 이상을 대상으로 할 경우 **Windows.Services.Store** 네임스페이스의 API를 사용하는 것이 좋습니다. 이러한 네임스페이스 차이점에 대한 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md#choose-namespace)을 참조하세요. 다음 코드 예제는 같은 프로젝트에서 두 API 모두를 사용하도록 코드를 구성하는 방법을 보여줍니다.

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

2. 그런 다음, 현재 사용자가 정식 Microsoft 계정이 있는 경우에 대해 사용자 지정 캠페인 ID 검색을 시도합니다. 이를 위해 코드는 현재 앱 SKU를 나타내는 [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) 개체를 가져온 다음, 캠페인 ID가 있을 경우 이를 검색하기 위해 [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_CampaignId) 속성에 액세스합니다.

2. 그런 다음 현재 사용자가 정식 Microsoft 계정이 없는 경우에 대한 캠페인 ID 검색을 시도합니다. 이 경우 캠페인 ID는 앱 라이선스에 포함되어 있습니다. 코드는 [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppLicenseAsync) 메서드를 사용하여 라이선스를 검색하고 라이선스의 JSON 콘텐츠를 구문 분석하여 *customPolicyField1*이라는 이름의 키 값을 찾습니다. 이 값은 캠페인 ID를 포함합니다.

3. **Windows.Services.Store** 네임스페이스의 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스를 사용할 수 없는 경우, 코드는 대신 **Windows.ApplicationModel.Store** 네임스페이스(이 네임스페이스는 버전 1511 이하를 포함하여 모든 버전의 Windows 10에서 사용할 수 있음)의 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getapppurchasecampaignidasync.aspx) 메서드를 사용하여 사용자 지정 캠페인 ID를 검색합니다. 이 메서드를 사용하는 경우, 사용자가 정식 Microsoft 계정이 있는 [적격 취득](#conversions)의 사용자 지정 캠페인 ID만 검색할 수 있음을 참고하십시오.

  >**참고**&nbsp;&nbsp;**Windows.ApplicationModel.Store** 네임스페이스에는 스토어에 앱을 제출하기 전에 스토어 작업을 시뮬레이트하는 특수 클래스인 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator)가 포함되어 있습니다. 이 클래스는 [Windows.StoreProxy.xml이라는 로컬 파일](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator)에서 데이터를 검색합니다. 이전 코드 예제에서 **CurrentApp**과 **CurrentAppSimulator** 모두를 프로젝트의 디버그 및 비 디버그 코드 사용하는 방법을 볼 수 있습니다. 디버그 환경에서 이 코드를 테스트하려면, 다음 예제와 같이 개발 컴퓨터의 WindowsStoreProxy.xml 파일에 **AppPurchaseCampaignId** 요소를 추가합니다. 앱을 실행하면 [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator#Windows_ApplicationModel_Store_CurrentAppSimulator_GetAppPurchaseCampaignIdAsync) 메서드는 항상 이 값을 반환합니다.

  ``` xml
  <CurrentApp>
      ...
      <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
  </CurrentApp>
  ```

  >**참고**&nbsp;&nbsp;**Windows.Services.Store** 네임스페이스는 테스트 중에 라이선스 정보를 시뮬레이션하는 데 사용할 수 있는 클래스를 제공하지 않습니다. 대신에 개발자가 스토어에 앱을 게시하고 개발 장치에 앱을 다운로드하여 해당 라이선스를 테스트에 사용해야 합니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md#testing)을 참조하세요.

## <a name="test-your-custom-campaign"></a>사용자 지정 캠페인 테스트

사용자 지정 캠페인 URL을 홍보하기 전에 다음을 수행하여 사용자 지정 캠페인을 테스트하는 것이 좋습니다.

1.  테스트에 사용할 컴퓨터 또는 장치에서 Microsoft 계정에 로그인합니다.
2.  사용자 지정 캠페인 URL을 클릭합니다. Windows 스토어에서 앱 페이지를 올바르게 로드하는지 확인한 다음 Windows 스토어 앱 또는 브라우저 페이지를 닫습니다.
3.  URL을 여러 번 클릭하고 앱 페이지를 방문할 때마다 Windows 스토어 앱 또는 브라우저 페이지를 닫습니다. 여러 번의 앱 페이지 방문 중에서 앱을 구입하여 변환을 생성합니다. URL을 클릭한 총 횟수를 계산합니다.
4. 예상된 페이지 내용과 변환이 [채널 및 변환 보고서](channels-and-conversions-report.md)에 표시되는지 확인하고, 앱 코드를 테스트하여 캠페인 ID를 검색할 수 있는지 확인합니다.
