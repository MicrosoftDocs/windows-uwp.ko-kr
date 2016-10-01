---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "UWP 앱에서 ‘앱에서 바로 구매’ 및 평가판을 사용하도록 설정하는 방법을 알아봅니다."
title: "앱에서 바로 구매 및 평가판"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99143d48a5f2155b0a47008574d0a78243dea925

---

# 앱에서 바로 구매 및 평가판

Windows SDK는 앱으로 수익을 창출하고 새로운 기능을 추가할 수 있도록 UWP(유니버설 Windows 플랫폼) 앱에 앱에서 바로 구매 및 평가판 기능을 추가하는 데 사용할 수 있는 API를 제공합니다. 이러한 API를 통해 앱에 대한 라이선스 정보에 액세스할 수도 있습니다.

이러한 시나리오에서 Windows 10은 다음 두 가지 API를 제공합니다.

* 모든 버전의 Windows 10은 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스에서 앱에서 바로 구매 및 라이선스 정보를 위한 API를 지원합니다.

* Windows 10 버전 1607부터는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에 앱에서 바로 구매 및 라이선스 정보를 위한 대체 API가 있습니다.  

두 네임스페이스의 API는 동일한 역할을 하지만 완전히 다르게 디자인되었으며 두 API 간에 코드가 호환되지 않습니다. 앱이 Windows 10 버전 1607 이상을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스를 사용하는 것이 좋습니다. 이 네임스페이스는 스토어 관리 소모성 추가 기능 등의 최신 추가 기능 유형을 지원하며 Windows 개발자 센터 및 스토어에서 지원하는 이후 제품 및 기능 유형과 호환되도록 설계되었습니다. **Windows.Services.Store** 네임스페이스에서는 성능도 향상되었습니다.

이 문서에서는 UWP 앱용 앱에서 바로 구매를 소개하고 Windows 10 버전 1607부터 사용할 수 있는 **Windows.Services.Store** 네임스페이스를 간략하게 설명합니다. **Windows.ApplicationModel.Store** 네임스페이스의 멤버를 사용하는 방법에 대한 자세한 내용은 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.


## UWP 앱의 앱에서 바로 구매 개요

이 섹션에서는 앱에서 바로 구매 및 평가판이 스토어의 UWP 앱에서 작동하는 방식에 대한 핵심 개념을 설명합니다. 이러한 개념의 대부분은 **Windows.Services.Store** 및 **Windows.ApplicationModel.Store** 네임스페이스 둘 다에 적용됩니다.

스토어에서 제공하는 각 항목을 일반적으로 *제품*이라고 합니다. 대부분의 개발자는 *앱*과 *추가 기능*(앱에서 바로 구매 제품 또는 IAP라고도 함) 유형의 제품을 사용합니다. 추가 기능은 앱의 컨텍스트에서 고객에게 제공하는 제품 또는 기능을 가리킵니다. 추가 기능은 앱에서 고객에게 제공하는 모든 기능을 나타낼 수 있습니다. 예를 들어 앱 또는 게임에서 사용할 통화, 게임용 새로운 지도 또는 무기, 광고 없이 앱을 사용하는 기능, 해당 형식의 콘텐츠를 제공할 수 있는 앱을 위한 음악, 비디오 등의 디지털 콘텐츠 등이 포함됩니다.

모든 앱과 추가 기능에는 사용자가 앱 또는 추가 기능을 사용할 자격이 있는지 여부를 나타내는 관련 라이선스가 있습니다. 사용자가 앱 또는 추가 기능을 평가판으로 사용할 자격이 있으면 라이선스에서 평가판에 대한 추가 정보도 제공합니다.

앱에서 고객에게 추가 기능을 제공하려면 먼저 [개발자 센터 대시보드에서 앱용 추가 기능을 정의](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)합니다. 그런 다음, 사용자에게 추가 기능이 나타내는 기능을 사용할 라이선스가 있는지 확인하고 라이선스가 아직 없는 경우 추가 기능을 앱에서 바로 구매로 사용자에게 판매용으로 제공하는 코드를 앱에서 작성합니다. Windows 10 버전 1607 이상을 대상으로 하는 앱에서 **Windows.Services.Store** 네임스페이스를 사용하여 관련 작업을 보여 주는 예제는 다음 문서를 참조하세요.

* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)

**Windows.ApplicationModel.Store** 네임스페이스를 사용하여 관련 작업을 보여 주는 예제는 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.

모든 개발자는 다음과 같은 유형의 추가 기능을 만들 수 있습니다.

| 추가 기능 유형 |  설명  |
|---------|-------------------|
| 지속형  |  [Windows 개발자 센터 대시보드](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties)에서 지정한 수명 동안 지속되는 추가 기능입니다. <p/><p/>기본적으로 지속형 추가 기능은 만료되지 않으므로 한 번만 구매할 수 있습니다. 추가 기능에 대해 특정 지속 기간을 지정하면 만료 후에 사용자가 추가 기능을 다시 구매할 수 있습니다.  |
| 개발자 관리 소모성  |  구매하고 사용한 후 다시 구매할 수 있는 추가 기능입니다. 이 유형의 추가 기능은 일반적으로 앱에서 바로 구매 통화에 사용됩니다. <p/><p/>이 소모성 유형의 경우 개발자가 추가 기능이 나타내는 항목의 사용자 잔액을 추적하고 사용자가 항목을 모두 사용한 후 추가 기능 구매를 처리된 것으로 스토어에 보고해야 합니다. 사용자는 앱에서 이전 추가 기능 구매를 처리된 것으로 보고할 때까지 추가 기능을 다시 구매할 수 없습니다. <p/><p/>예를 들어 게임에서 추가 기능이 100개 동전을 나타내고 사용자가 10개 동전을 사용한 경우 앱 또는 서비스에서 사용자의 남은 새 잔액인 90개 동전을 유지 관리해야 합니다. 사용자가 100개 동전을 모두 사용한 후 앱에서 추가 기능을 처리된 것으로 보고해야 하며, 그러면 사용자가 100개 동전 추가 기능을 다시 구매할 수 있습니다.    |
| 스토어 관리 소모성  |  구매하고 사용한 후 다시 구매할 수 있는 추가 기능입니다. 이 유형의 추가 기능은 일반적으로 앱에서 바로 구매 통화에 사용됩니다.<p/><p/>이 소모성 유형의 경우 스토어에서 추가 기능이 나타내는 항목의 사용자 잔액을 추적합니다. 사용자는 항목을 사용할 때 해당 항목을 처리된 것으로 스토어에 보고해야 하며, 스토어에서 사용자 잔액을 업데이트합니다. 앱은 언제든지 사용자의 현재 잔액을 쿼리할 수 있습니다. 사용자는 모든 항목을 사용한 후 추가 기능을 다시 구매할 수 있습니다.  <p/><p/> 예를 들어 게임에서 추가 기능이 초기 수량인 100개 동전을 나타내고 사용자가 10개 동전을 사용한 경우 앱은 추가 기능의 10개 단위가 처리되었다고 스토어에 보고하고 스토어에서 남은 잔액을 업데이트합니다. 사용자는 100개 동전을 모두 사용한 후 100개 동전 추가 기능을 다시 구매할 수 있습니다. <p/><p/> **스토어 관리 소모성은 Windows 10 버전 1607부터 사용할 수 있습니다. Windows 개발자 센터 대시보드에서 스토어 관리 소모성을 만드는 기능이 곧 제공될 예정입니다.**  |

<span />

>**참고**&nbsp;&nbsp;패키지를 사용한 지속형 추가 기능(다운로드 가능한 콘텐츠 또는 DLC라고도 함) 등 다른 유형의 추가 기능은 제한된 일부 개발자만 사용할 수 있으며 이 설명서에서 다루지 않습니다.

<span id="api_intro" />
## Windows.Services.Store 네임스페이스 소개

**Windows.Services.Store** 네임스페이스의 기본 진입점은 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스입니다. 이 클래스는 현재 앱과 사용 가능한 추가 기능에 대한 정보 가져오기, 현재 사용자를 위한 앱 또는 추가 기능 구매, 현재 앱 또는 추가 기능에 대한 라이선스 정보 가져오기 및 기타 작업에 사용할 수 있는 메서드를 제공합니다. [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 가져오려면 다음 중 하나를 수행합니다.

* 단일 사용자 앱(즉, 앱을 실행한 사용자의 컨텍스트에서만 실행되는 앱)에서는 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 메서드를 통해 사용자에 대한 Windows 스토어 관련 데이터를 액세스 및 관리하는 데 사용할 수 있는 **StoreContext** 개체를 가져옵니다. 대부분의 UWP(유니버설 Windows 플랫폼) 앱은 단일 사용자 앱입니다.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* 다중 사용자 앱에서는 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 메서드를 통해 앱을 사용하는 동안 해당 Microsoft 계정으로 로그인한 특정 사용자에 대한 Windows 스토어 관련 데이터를 액세스 및 관리하는 데 사용할 수 있는 **StoreContext** 개체를 가져옵니다. 다중 사용자 앱에 대한 자세한 내용은 [다중 사용자 응용 프로그램 소개](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)를 참조하세요. 다음 예제에서는 사용 가능한 첫 번째 사용자에 대한 **StoreContext** 개체를 가져옵니다.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

[StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)가 있으면 메서드 호출을 시작하여 현재 사용자를 위한 앱 또는 추가 기능을 구매하고 기타 작업을 수행할 수 있습니다. 자세한 내용은 다음 문서를 참조하세요.

* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)

**Windows.Services.Store** 네임스페이스를 사용하여 평가판 및 앱에서 바로 구매를 구현하는 방법을 보여 주는 전체 샘플은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

<span />
### 제품에 대한 개체 모델, SKU 및 가용성

스토어의 모든 제품에는 *SKU*가 하나 이상 있으며, 각 SKU에 *가용성*이 하나 이상 있습니다. 대부분의 개발자는 Windows 개발자 센터 대시보드에서 이러한 개념을 도외시하며 해당 앱이나 추가 기능에 대한 SKU 또는 가용성을 정의하지 않습니다. 그러나 **Windows.Services.Store** 네임스페이스의 스토어 제품에 대한 개체 모델에는 SKU와 가용성이 포함되어 있으므로 이러한 개념에 대한 기본적인 이해가 도움이 될 수 있습니다.

| 개체 유형 |  설명  |
|---------|-------------------|
| 제품  |  [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 클래스는 앱 또는 추가 기능을 포함하여 스토어에서 사용할 수 있는 모든 유형의 제품을 나타냅니다. 이 클래스는 제품의 스토어 ID, 스토어 목록에 사용할 이미지 및 비디오, 가격 정보 등의 데이터에 액세스하는 데 사용할 수 있는 속성을 제공합니다. 또한 제품을 구매하는 데 사용할 수 있는 메서드를 제공합니다. |
| SKU |  [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 클래스는 제품의 *SKU*를 나타냅니다. SKU는 자체 설명, 가격 및 기타 고유한 제품 정보가 있는 특정 버전의 제품입니다. 앱이나 추가 기능마다 기본 SKU가 있습니다. 대부분의 개발자가 앱용 SKU를 여러 개 사용할 때는 전체 버전의 앱과 평가판을 게시하는 경우뿐입니다(스토어 카탈로그에서 이러한 각 버전은 동일한 앱의 다른 SKU임). <p/><p/> 일부 판매자는 해당 SKU를 정의할 수 있습니다. 예를 들어 대규모 게임 판매자가 빨간색 피를 허용하지 않는 지역/국가에서 녹색 피를 표시하는 SKU와 다른 모든 지역/국가에서 빨간색 피를 표시하는 SKU로 게임을 출시할 수 있습니다. 또는 디지털 비디오 콘텐츠의 판매자가 HD 버전용 SKU와 표준 화질 버전용 SKU의 두 SKU를 게시할 수 있습니다. <p/><p/> 각 제품에는 SKU에 액세스할 수 있는 [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) 속성이 있습니다. |
| 사용 가능성  |  [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 클래스는 SKU의 *가용성*을 나타냅니다. 가용성은 고유한 가격 정보가 있는 특정 버전의 SKU입니다. SKU마다 기본 가용성이 있습니다. 일부 판매자는 고유한 가용성을 정의하여 주어진 SKU에 대해 다른 가격 옵션을 도입할 수 있습니다. <p/><p/> 각 SKU에는 가용성에 액세스할 수 있는 [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) 속성이 있습니다. 대부분의 개발자의 경우 SKU마다 하나의 기본 가용성이 있습니다.  |

<span id="store_ids" />
### 스토어 ID

스토어의 모든 앱과 추가 기능에는 관련 **스토어 ID**가 있습니다. **Windows.Services.Store** 네임스페이스에 있는 대부분의 API는 스토어 ID가 있어야 앱이나 추가 기능에 대한 작업을 수행할 수 있습니다. 제품, SKU 및 가용성은 서로 다른 스토어 ID 형식을 사용합니다.

| 개체 유형 |  스토어 ID 형식  |
|---------|-------------------|
| 제품  |  스토어에 있는 모든 제품의 스토어 ID는 12자의 영숫자 문자열입니다(예: ```9NBLGGH4R315```). 이 스토어 ID는 앱 또는 추가 기능에 대한 Windows 개발자 센터 대시보드 페이지에서 제공되며 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 개체의 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) 속성에 의해 반환됩니다. 이 ID를 *제품 스토어 ID*라고도 합니다. |
| SKU |  SKU의 스토어 ID는 ```<product Store ID>/xxxx``` 형식을 사용합니다. 여기서 ```xxxx```는 제품의 SKU를 식별하는 4자리 영숫자 문자열입니다. 예를 들면 ```9NBLGGH4R315/000N```입니다. 이 ID는 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 개체의 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) 속성에서 반환하며 *SKU 스토어 ID*라고도 합니다. |
| 사용 가능성  |  사용 가능성의 스토어 ID는 ```<product Store ID>/xxxx/yyyyyyyyyyyy``` 형식을 사용합니다. 여기서 ```xxxx```는 제품의 SKU를 식별하는 4자리 영숫자 문자열이고, ```yyyyyyyyyyyy```는 SKU의 가용성을 식별하는 12자리 영숫자 문자열입니다. 예를 들면 ```9NBLGGH4R315/000N/4KW6QZD2VN6X```입니다. 이 ID는 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 개체의 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) 속성에서 반환하며 * 스토어 ID*라고도 합니다.  |

<span id="testing" />
### Windows.Services.Store 네임스페이스를 사용하는 앱 테스트

**Windows.Services.Store** 네임스페이스는 테스트 중에 라이선스 정보를 시뮬레이트하는 데 사용할 수 있는 클래스를 제공하지 않습니다. 대신에 개발자가 스토어에 앱을 게시하고 개발 디바이스에 앱을 다운로드하여 해당 라이선스를 테스트에 사용해야 합니다. 이는 **Windows.ApplicationModel.Store** 네임스페이스를 사용하는 앱과 다른 환경으로, 해당 앱은 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 클래스를 사용하여 테스트 중에 라이선스 정보를 시뮬레이트할 수 있습니다.

앱에서 **Windows.Services.Store** 네임스페이스의 API를 사용하여 앱과 추가 기능에 대한 정보에 액세스하는 경우 코드를 테스트하려면 다음 프로세스를 따르세요.

1. 앱이 스토어에서 이미 게시되고 사용할 수 있는 경우 **Windows.Services.Store** 네임스페이스의 API를 사용하도록 이 앱을 업데이트하려면 시작할 준비가 되었습니다. 앱에 대한 추가 기능을 제공하려는 경우 개발자 센터 대시보드에서 [앱에 대한 추가 기능을 정의](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)합니다.

  스토어에서 게시되고 사용할 수 있는 앱이 아직 없는 경우 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족하는 기본 앱을 빌드하고 Windows 개발자 센터 대시보드에 [이 앱을 제출](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)합니다. 앱에 대한 추가 기능을 제공하려는 경우 [앱에 대한 추가 기능을 정의](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)합니다. 필요에 따라 테스트 중에 [스토어에서 앱을 숨길](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) 수 있습니다.

2. 앱에서 **Windows.Services.Store** 네임스페이스의 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 메서드 중 하나를 사용하는 코드를 작성하여 [현재 앱에 사용할 수 있는 추가 기능 가져오기](get-product-info-for-apps-and-add-ons.md), [앱 또는 추가 기능 구매](enable-in-app-purchases-of-apps-and-add-ons.md), [앱에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md) 등의 작업을 수행합니다. 더 많은 예제를 보려면 아래의 관련 항목 섹션을 참조하세요.

3. Visual Studio에서 **프로젝트 메뉴**를 클릭하고 **스토어**를 가리킨 다음 **스토어에 앱 연결**을 클릭합니다. 마법사의 지침에 따라 테스트에 사용하려는 Windows 개발자 센터 계정에서 앱 프로젝트를 앱에 연결합니다.

  >**참고**&nbsp;&nbsp;스토어에서 프로젝트를 앱에 연결하지 않으면 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 메서드가 해당 반환 값의 **ExtendedError** 속성을 오류 코드 값 0x803F6107로 설정합니다. 이 값은 스토어에 앱에 대한 정보가 없음을 나타냅니다.

4. 아직 수행하지 않은 경우 이전 단계에서 지정한 앱을 스토어에서 설치하고 앱을 한 번 실행한 다음 이 앱을 닫습니다. 이렇게 하면 앱에 대한 유효한 라이선스가 개발 디바이스에 설치됩니다.

5. Visual Studio에서 프로젝트 실행 또는 디버깅을 시작합니다. 코드를 통해 로컬 프로젝트와 연결된 스토어 앱에서 앱 및 추가 기능 데이터를 검색해야 합니다. 앱을 다시 설치하라는 메시지가 표시되면 지침을 따른 다음 프로젝트를 실행하거나 디버그합니다.

## 관련 항목

* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Aug16_HO5-->


