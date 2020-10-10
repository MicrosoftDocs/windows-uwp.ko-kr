---
description: Windows 앱에서 실행 되는 앱에 대 한 광고 캠페인을 만드는 것 외에도 다른 채널을 사용 하 여 앱을 승격할 수 있습니다.
title: 사용자 지정 앱 홍보 캠페인 만들기
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 사용자 지정, 앱, 판촉, 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: c04a9705797e69543e9aec8f4e6205c815f76800
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878436"
---
# <a name="create-a-custom-app-promotion-campaign"></a>사용자 지정 앱 홍보 캠페인 만들기

Windows 앱에서 실행 되는 [앱에 대 한 광고 캠페인](../monetize/index.md) 을 만들 뿐만 아니라 다른 채널을 사용 하 여 앱을 승격할 수도 있습니다. 예를 들어 타사 앱 마케팅 공급자를 사용 하 여 앱을 홍보 하거나 소셜 미디어 사이트에서 앱에 대 한 링크를 게시할 수 있습니다. 이러한 작업을 *사용자 지정 캠페인*이라고 합니다.

앱에 대 한 사용자 지정 캠페인을 실행 하는 경우 각 URL에 다른 *캠페인 ID*가 포함 된 각 사용자 지정 캠페인에 대해 다른 url을 만들어 각 캠페인의 상대적 성능을 추적할 수 있습니다. Windows 10을 실행 하는 고객이 캠페인 ID를 포함 하는 URL을 클릭 하면 해당 사용자를 해당 사용자 지정 캠페인과 연결 하 고 [파트너 센터](https://partner.microsoft.com/dashboard)에서이 데이터를 사용할 수 있도록 합니다.

> [!IMPORTANT]
> 이 데이터는 Windows 10의 고객에 대해서만 추적 됩니다. 다른 운영 체제를 사용 하는 고객은 여전히 앱 목록에 대 한 링크를 팔 로우 할 수 있지만 이러한 고객의 활동에 대 한 데이터는 포함 되지 않습니다.

사용자 지정 캠페인과 연결 된 데이터에는 두 가지 기본 유형이 있습니다. 앱의 스토어 목록에 대 한 *페이지 보기* 와 *변환이*있습니다. 변환은 사용자 지정 캠페인 ID를 포함 하는 URL에서 앱의 스토어 목록 페이지를 보는 고객이 생성 한 앱 취득입니다. 변환에 대 한 자세한 내용은이 항목에서 [앱 가져오기가 변환으로 한정 되는 방법 이해](#understanding-how-acquisitions-qualify-as-conversions) 를 참조 하세요.

다음과 같은 방법으로 앱에 대 한 사용자 지정 캠페인 성능 데이터를 검색할 수 있습니다.

* 응용 프로그램 페이지 보기에서 앱 또는 추가 기능에 대 한 데이터를 볼 수 있으며,이는 [구매 보고서](acquisitions-report.md)의 캠페인 ID 및 **총 캠페인 변환** 차트를 **기준으로 응용 프로그램 페이지 보기** 및 변환에 대 한 데이터를 볼 수 있습니다.
* 앱이 UWP (유니버설 Windows 플랫폼) 앱 인 경우 Windows SDK Api를 사용 하 여 변환 된 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다.

## <a name="example-custom-campaign-scenario"></a>사용자 지정 캠페인 시나리오 예제

새 게임 빌드를 완료 하 고 기존 게임 플레이어로 홍보 하 고 싶은 게임 개발자를 생각해 보세요. 그녀는 게임의 스토어 목록에 대 한 링크를 포함 하 여 Facebook 페이지에 새 게임 릴리스에 대 한 공지를 게시 합니다. 그녀의 많은 플레이어가 Twitter를 팔 로우 하 여 게임 매장 목록에 대 한 링크와 함께 공지를 트 윗 합니다.

개발자는 이러한 각 프로 모션 채널의 성공 여부를 추적 하기 위해 게임의 저장소 목록에 대 한 두 가지 형식의 URL을 만듭니다.

* Facebook 페이지에 게시할 URL에 사용자 지정 캠페인 ID가 포함 됩니다. `my-facebook-campaign`

* Twitter에 게시할 URL에는 사용자 지정 캠페인 ID가 포함 됩니다. `my-twitter-campaign`

Facebook 및 Twitter 팔로 워가 Url을 클릭 하면 Microsoft에서 각 클릭을 추적 하 여 해당 사용자 지정 캠페인과 연결 합니다. 이후의 선별 된 게임과 추가 구매 구매는 사용자 지정 캠페인과 연결 되며 변환으로 보고 됩니다.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>변환으로 인 한 한정 방법 이해

사용자 지정 캠페인 *변환은* 고객이 사용자 지정 캠페인을 통해 승격 된 URL을 클릭 하면 발생 하는 취득입니다. **응용 프로그램 페이지 보기** 에 대 한 변환 및 [구매 보고서](acquisitions-report.md) 의 캠페인 id 및 **총 캠페인 변환** 차트의 변환으로 한정 하 고, [프로그래밍 방식으로 캠페인 id를 검색](#programmatically)하기 위한 변환으로 정규화 하는 시나리오가 있습니다.

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>합병 보고서의 정규화 된 변환

다음 시나리오는 [구매 보고서](acquisitions-report.md)의 캠페인 ID 및 **총 캠페인 변환** 차트로 **앱 페이지 보기 및 변환** 에 대 한 변환으로 한정 됩니다.

* 인식 된 *Microsoft 계정 있거나 없는* 고객은 사용자 지정 캠페인 ID를 포함 하 고 앱의 스토어 목록으로 리디렉션되는 앱 URL을 클릭 합니다. 그런 다음, 동일한 고객이 사용자 지정 캠페인 ID로 Microsoft Store URL을 처음 클릭 하면 24 시간 이내에 앱을 가져옵니다.

* 고객이 사용자 지정 캠페인 ID를 사용 하 여 URL을 클릭 한 장치와 다른 장치에서 앱을 획득 하는 경우에는 사용자가 URL을 클릭 했을 때와 동일한 Microsoft 계정로 로그인 한 경우에만 변환이 계산 됩니다.

> [!NOTE]
> 사용자 지정 캠페인에 대 한 변환으로 계산 되는 앱 합병의 경우 해당 앱의 추가 구매는 동일한 사용자 지정 캠페인에 대 한 변환으로도 계산 됩니다.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>프로그래밍 방식으로 캠페인 ID를 검색할 때 한정 변환

앱과 연결 된 캠페인 ID를 프로그래밍 방식으로 검색 하는 경우 변환으로 한정 하려면 다음 조건을 충족 해야 합니다.

* **Windows 10, 버전 1607**이상을 실행 하는 장치에서: 고객이 (인식 된 Microsoft 계정에 로그인 했는지 여부에 관계 없이) 사용자 지정 캠페인 ID를 포함 하 고 앱의 스토어 목록 페이지로 리디렉션되는 URL을 클릭 합니다. URL을 클릭 한 결과로 고객이 스토어 목록을 보는 동안 앱을 획득 합니다.

* **Windows 10, 버전 1511 또는 이전 버전**을 실행 하는 장치에서: 고객이 인식 된 Microsoft 계정로 로그인 해야 하는 경우 사용자 지정 캠페인 ID가 포함 된 URL을 클릭 하 고 앱의 스토어 목록 페이지로 리디렉션됩니다. URL을 클릭 한 결과로 고객이 스토어 목록을 보는 동안 앱을 획득 합니다. 이러한 버전의 Windows 10에서는 사용자가 프로그래밍 방식으로 캠페인 ID를 검색 하는 경우 변환으로 한정 하기 위해 인식 된 Microsoft 계정로 로그인 해야 합니다.

> [!NOTE]
> 고객이 스토어 목록 페이지를 떠나면 서 24 시간 (동일한 장치 또는 동일한 Microsoft 계정로 로그인 했을 때 다른 장치에 있는 경우)을 사용 하 여 페이지에 반환 하 고 앱을 가져오면 **앱 페이지 보기 및** 변환 [보고서](acquisitions-report.md)의 캠페인 ID 및 **총 캠페인 변환** 차트를 통해 변환으로 간주 **됩니다** . 그러나이는 프로그래밍 방식으로 캠페인 ID를 검색 하는 경우 변환으로 한정 **되지** 않습니다.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>앱의 Microsoft Store 페이지 URL에 사용자 지정 캠페인 ID 포함

사용자 지정 캠페인 ID를 사용 하 여 앱에 대 한 Microsoft Store 페이지 URL을 만들려면 다음을 수행 합니다.

1.  사용자 지정 캠페인에 대 한 ID 문자열을 만듭니다. 쉽게 식별할 수 있는 짧은 캠페인 Id를 정의 하는 것이 좋지만이 문자열에는 최대 100 자까지 사용할 수 있습니다.

   > [!NOTE]
   > 캠페인 ID 문자열은 자신의 앱에 대 한 [합병 보고서](acquisitions-report.md) 를 볼 때 다른 개발자에 게 표시 될 수 있습니다. 고객이 사용자 지정 캠페인 ID를 클릭 하 여 스토어에 진입 하 고 동일한 세션 내에서 다른 개발자의 앱을 구매 하는 경우 해당 변환을 캠페인 ID로 지정할 때 이러한 상황이 발생할 수 있습니다. 해당 개발자는 캠페인 id의 이름을 포함 하 여 캠페인 ID의 초기 클릭으로 인 한 앱의 변환 수를 확인 하지만 캠페인 ID를 클릭 한 후에는 자신의 앱 (또는 다른 개발자의 앱)을 구매한 사용자의 수에 대 한 데이터를 볼 수 없습니다.

2.  HTML 또는 프로토콜 형식으로 앱 스토어 목록에 대 한 링크를 가져옵니다.

    * 고객이 모든 운영 체제의 브라우저에서 앱의 웹 기반 상점 목록으로 이동 하려면 HTML URL을 사용 합니다. Windows 장치에서 스토어 앱이 시작 되 고 앱의 목록도 표시 됩니다. 이 URL의 형식은 **`https://www.microsoft.com/store/apps/*your app ID*`** 입니다. 예를 들어 Skype의 HTML URL은 `https://www.microsoft.com/store/apps/9wzdncrfj364` 입니다. [앱 id](view-app-identity-details.md#link-to-your-apps-listing) 페이지에서이 URL을 찾을 수 있습니다.

    * UWP 앱이 설치 된 장치 또는 컴퓨터에서 실행 되는 다른 Windows 앱에서 앱을 승격 하거나 고객이 Microsoft Store를 지 원하는 장치에 있는 경우 프로토콜 형식을 사용 합니다. 이 링크는 브라우저를 열지 않고도 앱의 스토어 목록으로 바로 이동 합니다. 이 URL의 형식은 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`** 입니다. 예를 들어 Skype의 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364` 입니다.

3.  앱에 대 한 URL의 끝에 다음 문자열을 추가 합니다.

    * HTML 형식 URL의 경우를 추가 **`?cid=*my custom campaign ID*`** 합니다. 예를 들어, Skype에서 **사용자 지정 \_ 캠페인**값을 사용 하 여 캠페인 id를 도입 하는 경우 캠페인 id를 포함 하는 새 URL은 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign` 입니다.

    * 프로토콜 형식 URL의 경우 append를 추가 **`&cid=*my custom campaign ID*`** 합니다. 예를 들어, Skype에서 **사용자 지정 \_ 캠페인**값을 사용 하 여 캠페인 id를 도입 하는 경우 캠페인 id를 포함 하는 새 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign` 입니다.

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>앱에 대 한 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색

앱이 UWP 앱 인 경우 Windows SDK Api를 사용 하 여 앱의 획득에 연결 된 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다. 이러한 Api는 다양 한 분석과 수익 화 시나리오를 가능 하 게 합니다. 예를 들어, Facebook 캠페인을 통해 검색 한 후 현재 사용자가 앱을 확보 했는지 확인 한 후 앱 환경을 적절 하 게 사용자 지정할 수 있습니다. 또는 타사 앱 마케팅 공급자를 사용 하는 경우 공급자에 게 데이터를 다시 보낼 수 있습니다.

이러한 Api는 고객이 포함 된 캠페인 ID를 사용 하 여 URL을 클릭 하 고 앱에 대 한 Microsoft Store 페이지를 본 다음 스토어 목록 페이지를 떠나지 않고 앱을 획득 하는 경우에만 캠페인 ID 문자열을 반환 합니다. 사용자가 페이지를 떠나면 나중에 앱을 반환 하 고 가져오는 경우 이러한 Api를 사용 하는 경우 [변환으로 한정](#conversions) 되지 않습니다.

앱이 대상으로 하는 Windows 10 버전에 따라 다양 한 Api를 사용 하 여 사용할 수 있습니다.

* Windows 10, 버전 1607 이상: **windows. Store** 네임 스페이스에서 [**Storecontext**](/uwp/api/windows.services.store.storecontext) 클래스를 사용 합니다. 이 API를 사용 하는 경우 사용자가 인식 된 Microsoft 계정에 로그인 했는지 여부에 관계 없이 모든 [정규화](#conversions)된 획득에 대 한 사용자 지정 캠페인 id를 검색할 수 있습니다.

* Windows 10, 버전 1511 또는 이전 버전: **Windows ApplicationModel. Store** 네임 스페이스의 [**Currentapp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스를 사용 합니다. 이 API를 사용 하는 경우 사용자가 인식 된 Microsoft 계정를 사용 하 여 로그인 한 [정규화](#conversions) 된 획득에 대 한 사용자 지정 캠페인 id만 검색할 수 있습니다.

> [!NOTE]
> Windows 10의 모든 버전에서는 **windows.** n e t. store 네임 스페이스를 사용할 수 있지만, 앱이 windows 10, 버전 1607 이상을 대상으로 하는 경우에는 **windows. store** 네임 스페이스에서 api를 사용 하는 것이 좋습니다. 이러한 네임 스페이스 간의 차이점에 대 한 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md#choose-namespace)을 참조 하세요. 다음 코드 예제에서는 동일한 프로젝트에서 두 Api를 모두 사용 하도록 코드를 구성 하는 방법을 보여 줍니다.

### <a name="code-example"></a>코드 예제

다음 코드 예제에서는 사용자 지정 캠페인 ID를 검색 하는 방법을 보여 줍니다. 이 예제에서는 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 사용 하 여 **Windows** 의 두 api 집합을 모두 사용 **합니다.** 이 프로세스를 수행 하 여 모든 버전의 Windows 10에서 코드를 실행할 수 있습니다. 이 코드를 사용 하려면 프로젝트의 대상 OS 버전은 **Windows 10 기념일 Edition (10.0;) 이어야 합니다. 빌드 14394)** 이상, 최소 OS 버전이 이전 버전인 경우에도 가능 합니다.

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

이 코드는 다음을 수행합니다.

1. 먼저 현재 장치에서 Storecontext 클래스의 [**Storecontext**](/uwp/api/windows.services.store.storecontext) 클래스를 사용할 수 있는지 **확인 합니다.** 이는 장치에서 windows 10, 버전 1607 이상을 실행 하 고 있음을 의미 합니다. 이 경우 코드는이 클래스를 사용 하는 것으로 진행 합니다.

2. 그런 다음 현재 사용자에 게 인식 된 Microsoft 계정 있는 경우 사용자 지정 캠페인 ID를 가져오려고 시도 합니다. 이렇게 하려면 코드에서 현재 앱 SKU를 나타내는 [**StoreSku**](/uwp/api/Windows.Services.Store.StoreSku) 개체를 가져온 다음 [**CampaignId**](/uwp/api/windows.services.store.storecollectiondata.CampaignId) 속성에 액세스 하 여 캠페인 ID (있는 경우)를 검색 합니다.
3. 그런 다음 코드는 현재 사용자에 게 인식 된 Microsoft 계정 없는 경우의 캠페인 ID를 검색 하려고 합니다. 이 경우 캠페인 ID는 앱 라이선스에 포함 됩니다. 이 코드는 [**GetAppLicenseAsync**](/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) 메서드를 사용 하 여 라이선스를 검색 한 다음 *customPolicyField1*라는 키 값에 대 한 라이선스의 JSON 콘텐츠를 구문 분석 합니다. 이 값에는 캠페인 ID가 포함 됩니다.

4. GetAppPurchaseCampaignIdAsync 네임 스페이스의 [**Storecontext**](/uwp/api/windows.services.store.storecontext) 클래스를 사용할 수 없는 경우,이 코드는 사용자 지정 캠페인 ID를 검색 하기 위해 **Windows. Applicationmodel. store** **네임 스페이스에서** [**GetAppPurchaseCampaignIdAsync**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) 메서드를 사용 하도록 대체 합니다 .이 네임 스페이스는 버전 1511 및 이전 버전을 포함 하 여 모든 버전의 windows 10에서 사용할 수 있습니다. 이 메서드를 사용 하는 경우 사용자에 게 인식 된 Microsoft 계정 있는 [정규화](#conversions) 된 획득에 대 한 사용자 지정 캠페인 id만 검색할 수 있습니다.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Windows. ApplicationModel. Store 네임 스페이스에 대 한 프록시 파일의 캠페인 ID를 지정 합니다.

응용 프로그램을 스토어에 제출 하기 전에 코드를 테스트 하기 위한 저장소 작업을 시뮬레이트하는 특수 클래스인 [**Currentappsimulator**](/uwp/api/windows.applicationmodel.store.currentappsimulator)가 **있습니다.** 이 클래스 [는 Windows.StoreProxy.xml 파일 이라는 로컬 파일](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator)에서 데이터를 검색 합니다. 위의 코드 예제는 프로젝트에서 디버그 및 비 디버그 코드에 **Currentapp** 및 **currentapp시뮬레이터** 를 모두 사용 하는 방법을 보여 줍니다. 디버그 환경에서이 코드를 테스트 하려면 다음 예제와 같이 개발 컴퓨터의 WindowsStoreProxy.xml 파일에 **AppPurchaseCampaignId** 요소를 추가 합니다. 앱을 실행 하는 경우 [**GetAppPurchaseCampaignIdAsync**](/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) 메서드는 항상이 값을 반환 합니다.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

**Windows. Store** 네임 스페이스는 테스트 하는 동안 라이선스 정보를 시뮬레이트하는 데 사용할 수 있는 클래스를 제공 하지 않습니다. 대신 앱을 스토어에 게시 하 고 해당 앱을 개발 장치에 다운로드 하 여 테스트를 위해 라이선스를 사용 해야 합니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md#testing)을 참조 하세요.

## <a name="test-your-custom-campaign"></a>사용자 지정 캠페인 테스트

사용자 지정 캠페인 URL의 수준을 올리려면 먼저 다음을 수행 하 여 사용자 지정 캠페인을 테스트 하는 것이 좋습니다.

1.  테스트를 위해 사용 하는 장치에서 Microsoft 계정에 로그인 합니다.

2.  사용자 지정 캠페인 URL을 클릭 합니다. 앱 페이지로 이동한 다음 UWP 앱 또는 브라우저 페이지를 닫아야 합니다.

3.  URL을 몇 번 더 클릭 하 여 앱의 페이지에 방문한 후 UWP 앱 또는 브라우저 페이지를 닫습니다. 앱 페이지를 방문 하 **는 동안 앱** 을 획득 하 여 변환을 생성 합니다. URL을 클릭 한 총 횟수를 계산 합니다.

4. 예상 되는 페이지 보기 및 변환이 **응용 프로그램 페이지 보기** 에 표시 되 고, 변환 [보고서](acquisitions-report.md)의 캠페인 id 및 **총 캠페인 변환** 차트에 의해 변환 되는지 확인 하 고, 위에 설명 된 API를 사용 하 여 캠페인 id를 성공적으로 검색할 수 있는지 여부를 확인 하기 위해 앱의 코드를 테스트 합니다.