---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "UWP 앱에서 ‘앱에서 바로 구매’ 및 평가판을 사용하도록 설정하는 방법을 알아봅니다."
title: "앱에서 바로 구매 및 평가판"
translationtype: Human Translation
ms.sourcegitcommit: ebac7d8edf9787ad810b84f3855fe23ae3007dcf
ms.openlocfilehash: 33004b5229167bedd4cbafe5279709f1225ed02c

---

# <a name="in-app-purchases-and-trials"></a>앱에서 바로 구매 및 평가판

Windows SDK는 UWP(유니버설 Windows 플랫폼) 앱에서 더 많은 수익을 올릴 수 있도록 다음과 같은 기능을 구현하는 데 사용할 수 있는 API를 제공합니다.

* **앱에서 바로 구매**&nbsp;&nbsp;앱이 무료인지 여부와 상관없이, 앱 내에서 바로 콘텐츠 또는 새 앱 기능(예: 게임의 다음 단계 잠금 해제)을 판매할 수 있습니다.

* **평가판 기능**&nbsp;&nbsp;Windows 개발자 센터 대시보드에서 [무료 평가판](../publish/set-app-pricing-and-availability.md#free-trial)으로 앱을 구성하면 평가 기간 동안 일부 기능을 제외하거나 제한하여 고객이 앱 정식 버전을 구매하도록 유도할 수 있습니다. 또한 고객이 앱을 구매하기 전 체험 기간 동안에만 표시되는 배너 또는 워터마크와 같은 기능을 사용하도록 설정할 수도 있습니다.

이 문서에서는 앱에서 바로 구매와 평가판이 UWP 앱에서 작동하는 방식에 대해 간략하게 보여 줍니다.

<span id="choose-namespace" />
## <a name="choose-which-namespace-to-use"></a>사용할 네임스페이스 선택

앱에서 바로 구매와 평가판 기능을 UWP 앱에 추가하는 데 사용할 수 있는 네임스페이스는 앱에서 대상을 지정한 Windows 10 버전에 따라 서로 다른 두 가지가 있습니다. 두 네임스페이스의 API는 동일한 역할을 하지만 완전히 다르게 디자인되었으며 두 API 간에 코드가 호환되지 않습니다.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;Windows 10 버전 1607부터 이 네임스페이스에서 API를 사용하여 앱에서 바로 구매 및 평가판을 구현할 수 있습니다. 앱이 Windows 10 버전 1607 이상 릴리스를 대상으로 하는 경우 이 네임스페이스의 구성원을 사용하는 것이 좋습니다. 이 네임스페이스는 스토어 관리 소모성 추가 기능 등의 최신 추가 기능 유형을 지원하며 Windows 개발자 센터 및 스토어에서 지원하는 이후 제품 및 기능 유형과 호환되도록 설계되었습니다. 이 네임스페이스에 대한 자세한 내용은 이 문서의 [Windows.Services.Store 네임스페이스를 사용하여 앱에서 바로 구매 및 평가판 이용](#api_intro) 섹션을 참조하세요.

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;모든 버전의 Windows 10은 이 네임이 스페이스에서 앱에서 바로 구매 및 평가판을 위한 이전 API도 지원합니다. Windows 10용 UWP 앱이 이 네임스페이스를 사용할 수는 있지만 이 네임스페이스는 나중에 개발자 센터 및 스토어에서 새로운 유형의 제품 및 기능을 지원하도록 업데이트되지 않을 수 있습니다. 이 네임스페이스에 대한 자세한 내용은 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.

<span id="concepts" />
## <a name="basic-concepts"></a>기본 개념

이 섹션에서는 UWP 앱의 앱에서 바로 구매와 평가판에 대한 기본 개념을 소개합니다. 이러한 개념의 대부분은 별도의 설명을 제외하고 **Windows.Services.Store** 및 **Windows.ApplicationModel.Store** 네임스페이스 둘 다에 적용됩니다.

스토어에서 제공하는 각 항목을 일반적으로 *제품*이라고 합니다. 대부분의 개발자는 *앱*과 *추가 기능*(앱에서 바로 구매 제품 또는 IAP라고도 함) 유형의 제품을 사용합니다.

추가 기능은 앱 컨텍스트에서 고객이 사용할 수 있는 제품 또는 기능을 말합니다. 예를 들어 앱 또는 게임에서 사용할 통화, 게임용 새로운 지도 또는 무기, 광고 없이 앱을 사용하는 기능, 앱에서 이용할 수 있는 음악, 비디오 등의 디지털 콘텐츠(앱에서 해당 콘텐츠 유형을 제공하는 경우) 등이 포함됩니다. 모든 앱과 추가 기능에는 사용자가 앱 또는 추가 기능을 사용할 자격이 있는지 여부를 나타내는 관련 라이선스가 있습니다. 사용자가 앱 또는 추가 기능을 평가판으로 사용할 자격이 있으면 라이선스에서 평가판에 대한 추가 정보도 제공합니다.

앱에서 고객에게 추가 기능을 제공하려면 스토어에서도 알고 있도록 [Windows 개발자 센터 대시보드에서 앱에 대한 추가 기능을 정의](../publish/iap-submissions.md)해야 합니다. 그런 다음 **Windows.Services.Store** 또는 **Windows.ApplicationModel.Store** 네임스페이스에서 API를 사용하여 앱에서 바로 구매로 사용자에게 추가 기능을 판매용으로 제공할 수 있습니다.

UWP 앱은 다음 유형의 추가 기능을 제공할 수 있습니다.

| 추가 기능 유형 |  설명  |
|---------|-------------------|
| 지속형  |  [Windows 개발자 센터 대시보드](../publish/enter-iap-properties.md)에서 지정한 수명 동안 지속되는 추가 기능입니다. <p/><p/>기본적으로 지속형 추가 기능은 만료되지 않으므로 한 번만 구매할 수 있습니다. 추가 기능에 대해 특정 지속 기간을 지정하면 만료 후에 사용자가 추가 기능을 다시 구매할 수 있습니다. |
| 개발자 관리 소모성  |  구매하고 사용한 후 다시 구매할 수 있는 추가 기능입니다. 이 유형의 추가 기능은 주로 앱에서 바로 구매 통화에 사용됩니다. <p/><p/>이 소모성 유형의 경우 개발자가 추가 기능이 나타내는 항목의 사용자 잔액을 추적하고 사용자가 항목을 모두 사용한 후 추가 기능 구매를 처리된 것으로 스토어에 보고해야 합니다. 사용자는 앱에서 이전 추가 기능 구매를 처리된 것으로 보고할 때까지 추가 기능을 다시 구매할 수 없습니다. <p/><p/>예를 들어 게임에서 추가 기능이 100개 동전을 나타내고 사용자가 10개 동전을 사용한 경우 앱 또는 서비스에서 사용자의 남은 새 잔액인 90개 동전을 유지 관리해야 합니다. 사용자가 100개 동전을 모두 사용한 후 앱에서 추가 기능을 처리된 것으로 보고해야 하며, 그러면 사용자가 100개 동전 추가 기능을 다시 구매할 수 있습니다.    |
| 스토어 관리 소모성  |  구매하고 사용한 후 다시 구매할 수 있는 추가 기능입니다. 이 유형의 추가 기능은 주로 앱에서 바로 구매 통화에 사용됩니다.<p/><p/>이 소모성 유형의 경우 스토어에서 추가 기능이 나타내는 항목의 사용자 잔액을 추적합니다. 사용자는 항목을 사용할 때 해당 항목을 처리된 것으로 스토어에 보고해야 하며, 스토어에서 사용자 잔액을 업데이트합니다. 앱은 언제든지 사용자의 현재 잔액을 쿼리할 수 있습니다. 사용자는 모든 항목을 사용한 후 추가 기능을 다시 구매할 수 있습니다.  <p/><p/> 예를 들어 게임에서 추가 기능이 초기 수량인 100개 동전을 나타내고 사용자가 10개 동전을 사용한 경우 앱은 추가 기능의 10개 단위가 처리되었다고 스토어에 보고하고 스토어에서 남은 잔액을 업데이트합니다. 사용자는 100개 동전을 모두 사용한 후 100개 동전 추가 기능을 다시 구매할 수 있습니다. <p/><p/>**참고**&nbsp;&nbsp;스토어 관리 소모성은 Windows 10 버전 1607부터 사용할 수 있습니다. 스토어에서 관리하는 소모품을 사용하려면 앱에서 Windows 10 버전 1607 또는 그 이후 버전을 대상으로 지정하고 **Windows.ApplicationModel.Store** 네임스페이스 대신 **Windows.Services.Store** 네임스페이스에서 API를 사용해야 합니다.  |

<span />

>**참고**&nbsp;&nbsp;패키지를 사용한 지속형 추가 기능(다운로드 가능한 콘텐츠 또는 DLC라고도 함) 등 다른 유형의 추가 기능은 제한된 일부 개발자만 사용할 수 있으며 이 설명서에서 다루지 않습니다.

<span id="api_intro" />
## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Windows.Services.Store 네임스페이스를 사용하여 앱에서 바로 구매 및 평가판

이 문서의 나머지 부분에서는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스를 사용하여 앱에서 바로 구매 및 평가판을 구현하는 방법에 대해 설명합니다. 이 네임스페이스는 Windows 10 버전 1607 이상을 대상으로 하는 앱에만 사용 가능하므로 앱에서 가능하다면 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스 대신 이 네임스페이스를 사용하는 것이 좋습니다.

**Windows.ApplicationModel.Store** 네임스페이스에 대한 자세한 내용은 [이 문서](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)를 참조하세요.

### <a name="get-started-with-the-storecontext-class"></a>StoreContext 클래스 시작

**Windows.Services.Store** 네임스페이스의 기본 진입점은 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스입니다. 이 클래스는 현재 앱과 사용 가능한 추가 기능에 대한 정보 가져오기, 현재 앱 또는 추가 기능에 대한 라이선스 정보 가져오기, 현재 사용자를 위한 앱 또는 추가 기능 구매 및 기타 작업에 사용할 수 있는 메서드를 제공합니다. [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 가져오려면 다음 중 하나를 수행합니다.

* 단일 사용자 앱(즉, 앱을 실행한 사용자의 컨텍스트에서만 실행되는 앱)에서는 정적 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 메서드를 통해 사용자에 대한 Windows 스토어 관련 데이터에 액세스하는 데 사용할 수 있는 **StoreContext** 개체를 가져옵니다. 대부분의 UWP(유니버설 Windows 플랫폼) 앱은 단일 사용자 앱입니다.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* [다중 사용자 앱](../xbox-apps/multi-user-applications.md)에서는 정적 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 메서드를 통해 앱을 사용하는 동안 해당 Microsoft 계정으로 로그인한 특정 사용자의 Windows 스토어 관련 데이터를 액세스 및 관리하는 데 사용할 수 있는 **StoreContext** 개체를 가져옵니다. 다음 예제에서는 사용 가능한 첫 번째 사용자에 대한 **StoreContext** 개체를 가져옵니다.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

>**참고**&nbsp;&nbsp;[데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 Windows 데스크톱 응용 프로그램에서는 이 개체를 사용하기 전에 먼저 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성하는 추가 단계를 수행해야 합니다. 자세한 내용은 [이 섹션](#desktop)을 참조하세요.

[StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체가 확보되면 이 개체의 메서드 호출을 시작하여 현재 앱과 추가 기능에 대한 스토어 제품 정보 가져오기, 현재 앱 또는 추가 기능에 대한 라이선스 정보 가져오기, 현재 사용자를 위한 앱 또는 추가 기능 구매 및 기타 작업 수행을 위한 메서드를 제공합니다. 이 개체를 사용하여 수행할 수 있는 일반적인 작업에 대한 자세한 내용은 다음 문서를 참조하세요.

* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)

**StoreContext**를 비롯하여 기타 다른 유형의 **Windows.Services.Store** 네임스페이스를 사용하는 방법을 보여 주는 샘플 앱은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

<span id="implement-iap" />
### <a name="implement-in-app-purchases"></a>앱에서 바로 구매 구현

**Windows.Services.Store** 네임스페이스를 사용하여 앱에서 바로 구매 방식을 고객에게 제공하려면

1. 앱에서 고객이 구매할 수 있는 추가 기능을 제공하는 경우 [Windows 개발자 센터 대시보드에서 앱에 대한 추가 기능 제출을 만듭니다](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. 앱에서 코드를 기록하여 [앱이나 앱이 제공하는 추가 기능에 대한 제품 정보를 검색](get-product-info-for-apps-and-add-ons.md)한 다음 [라이선스가 활성 상태인지 확인](get-license-info-for-apps-and-add-ons.md)(사용자에게 앱이나 추가 기능을 사용할 수 있는 라이선스가 있는지 여부)합니다. 라이선스 활성화되지 않으면 판매를 위해 사용자에게 앱과 추가 기능을 제공하는 UI를 앱에서 바로 구매로 나타냅니다.

3. 사용자가 앱 또는 추가 기능을 구매하기로 선택한 경우 적절한 방법을 사용하여 제품을 구입합니다.

  * 사용자가 앱 또는 지속형 추가 기능을 구매할 경우 [앱에서 앱 및 추가 기능 바로 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md) 지침을 따릅니다.

  * 사용자가 소모성 추가 기능을 구매할 경우 [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)의 지침을 따르세요.

4. 이 문서에서는 [테스트 지침](#testing)에 따라 구현 방식을 테스트합니다.

<span id="implement-trial" />
### <a name="implement-trial-functionality"></a>평가판의 기능 구현

**Windows.Services.Store** 네임스페이스를 사용하여 앱의 평가판 버전에서 기능을 제외하거나 제한하려면

1. [앱을 Windows 개발자 센터 대시보드에서 무료 평가판 앱으로 구성합니다](../publish/set-app-pricing-and-availability.md#free-trial).

2. 앱에서 코드를 기록하여 [앱이나 앱이 제공하는 추가 기능에 대한 제품 정보를 검색](get-product-info-for-apps-and-add-ons.md)한 다음 [앱과 연결된 라이선스가 평가판 라이선스인지 확인](get-license-info-for-apps-and-add-ons.md)합니다.

3. 평가판인 경우 앱의 특정 기능을 제외하거나 제안한 다음 사용자가 정식 라이선스를 구매할 때 해당 기능을 사용하도록 설정합니다. 자세한 내용 및 코드 예제는 [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)을 참조하세요.

4. 이 문서에서는 [테스트 지침](#testing)에 따라 구현 방식을 테스트합니다.

<span id="testing" />
### <a name="test-your-in-app-purchase-or-trial-implementation"></a>앱에서 바로 구매 또는 평가판 구현 테스트

**Windows.Services.Store** 네임스페이스는 테스트 중에 라이선스 정보를 시뮬레이션하는 데 사용할 수 있는 클래스를 제공하지 않습니다. 대신에 개발자가 스토어에 앱을 게시하고 개발 장치에 앱을 다운로드하여 해당 라이선스를 테스트에 사용해야 합니다. 이는 **Windows.ApplicationModel.Store** 네임스페이스를 사용하는 앱과 다른 환경으로, 해당 앱은 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 클래스를 사용하여 테스트 중에 라이선스 정보를 시뮬레이션할 수 있습니다.

앱에서 **Windows.Services.Store** 네임스페이스의 API를 사용하여 앱과 추가 기능에 대한 정보에 액세스하는 경우 코드를 테스트하려면 다음 프로세스를 따르세요.

1. 앱이 아직 게시되지 않고 스토어에서 사용할 수 없는 경우 앱이 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족하는지 확인하고, Windows 개발자 센터 대시보드에 [앱을 제출](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)하고 앱을 스토어에서 사용할 수 있도록 인증 프로세스를 전달해야 합니다. [스토어에서 앱을 숨길](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) 수 있으므로 테스트하는 동안 고객이 사용하지 못할 수 있습니다.

2. 그다음에 아래 작업을 완료해야 합니다.

  * **Windows.Services.Store** 네임스페이스에서 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스 및 기타 관련 형식을 사용하는 앱에서 코드를 작성하여 [앱에서 바로 구매](#implement-iap) 또는 [평가판 기능](#implement-trial)을 구현합니다.

  * 앱에서 고객이 구매할 수 있는 추가 기능을 제공하는 경우 [Windows 개발자 센터 대시보드에서 앱에 대한 추가 기능 제출을 만듭니다](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

  * 앱의 평가판 버전에서 일부 기능을 제외하거나 제한하려면 앱을 [Windows 개발자 센터 대시보드에서 무료 평가판 앱으로 구성](../publish/set-app-pricing-and-availability.md#free-trial)합니다.

3. 프로젝트가 열리면 Visual Studio에서 **프로젝트 메뉴**를 클릭하고 **스토어**를 가리킨 다음 **스토어에 앱 연결**을 클릭합니다. 마법사의 지침에 따라 테스트에 사용하려는 Windows 개발자 센터 계정에서 앱 프로젝트를 앱에 연결합니다.

  >**참고**&nbsp;&nbsp;스토어에서 프로젝트를 앱에 연결하지 않으면 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 메서드가 해당 반환 값의 **ExtendedError** 속성을 오류 코드 값 0x803F6107로 설정합니다. 이 값은 스토어에 앱에 대한 정보가 없음을 나타냅니다.

4. 아직 수행하지 않은 경우 이전 단계에서 지정한 앱을 스토어에서 설치하고 앱을 한 번 실행한 다음 이 앱을 닫습니다. 이렇게 하면 앱에 대한 유효한 라이선스가 개발 장치에 설치됩니다.

5. Visual Studio에서 프로젝트 실행 또는 디버깅을 시작합니다. 코드를 통해 로컬 프로젝트와 연결된 스토어 앱에서 앱 및 추가 기능 데이터를 검색해야 합니다. 앱을 다시 설치하라는 메시지가 표시되면 지침을 따른 다음 프로젝트를 실행하거나 디버그합니다.

>**참고**&nbsp;&nbsp;1~5단계를 완료한 후 앱의 코드를 계속 업데이트할 수 있고, 이후 새로운 앱 패키지를 스토어에 제출하지 않고 개발 컴퓨터에서 업데이트된 프로젝트를 디버그할 수 있습니다. 테스트용으로 사용할 로컬 라이선스를 얻으려면 개발 컴퓨터에 스토어 버전의 앱을 다운로드하기만 하면 됩니다. 테스트를 완료한 후 사용자의 앱에서 고객이 체험판 관련 기능 또는 앱에서 바로 구매 기능을 사용할 수 있게 하려면 스토어에 새로운 앱 패키지를 제출해야 합니다.

<span id="receipts" />
### <a name="receipts-for-in-app-purchases"></a>앱에서 바로 구매에 대한 영수증

**Windows.Services.Store** 네임스페이스는 앱 코드에서 성공한 구매에 대한 거래 영수증을 가져오는 데 사용할 수 있는 API를 제공하지 않습니다. 이는 **Windows.ApplicationModel.Store** 네임스페이스를 사용하는 앱과 다른 환경으로, 해당 앱은 [거래 영수증을 검색할 수 있는 고객 쪽 API를 사용](use-receipts-to-verify-product-purchases.md)할 수 있습니다.

**Windows.Services.Store** 네임스페이스를 사용하여 앱에서 바로 구매를 구현하고 지정된 고객이 앱이나 추가 기능을 구매했는지 확인하려면 [Windows 스토어 컬렉션 REST API](query-for-products.md)에서 [제품 메서드에 대한 쿼리](view-and-grant-products-from-a-service.md)를 사용할 수 있습니다. 이 메서드에 대한 반환 데이터에서는 지정된 고객에게 특정 제품에 대한 자격이 있는지 확인하고, 사용자가 제품을 구입할 때 해당 거래에 대한 데이터를 제공합니다. Windows 스토어 컬렉션 API는 Azure AD 인증을 사용하여 이 정보를 검색합니다.

<span id="desktop" />
### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>데스크톱 브리지를 통하여 StoreContext 클래스 사용

[데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램은 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스를 사용하여 앱에서 바로 구매 및 평가판을 구현할 수 있습니다. 그러나 Win32 데스크톱 응용 프로그램이나, 렌더링 프레임워크(예: WPF 응용 프로그램)와 연결된 창 핸들(HWND)이 있는 데스크톱 응용 프로그램이 있는 경우 응용 프로그램에서 **StoreContext** 개체를 구성하여 개체에서 표시하는 모달 대화 상자에 대한 소유자 창인 응용 프로그램 창을 지정합니다.

많은 **StoreContext** 구성원(및 **StoreContext** 개체를 통해 액세스한 기타 관련된 구성원 유형)들은 제품 구입과 같은 스토어 관련 작업에 대한 모달 대화 상자를 사용자에게 표시합니다. 데스크톱 응용 프로그램에서 **StoreContext** 개체를 구성하지 않고 모달 대화 상자에 대한 소유자 창을 지정한 경우 이 개체는 부정확한 데이터 또는 오류를 반환합니다.

데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 **StoreContext** 개체를 구성하려면 다음 단계를 따릅니다.

  1. 다음 중 하나를 수행하여 앱에서 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 인터페이스에 액세스할 수 있도록 합니다.

    * 응용 프로그램이 C# 또는 Visual Basic 등 관리되는 언어로 작성된 경우 앱 코드의 **IInitializeWithWindow** 인터페이스를 다음 C# 예제에 나타나는 [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) 특성을 사용하여 선언합니다. 이 예제에서는 **System.Runtime.InteropServices** 네임스페이스에 대한 코드 파일에 **using** 문이 있다고 가정합니다.

      > [!div class="tabbedCodeSnippets"]
      ```csharp
      [ComImport]
      [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
      [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
      public interface IInitializeWithWindow
      {
          void Initialize(IntPtr hwnd);
      }
      ```

    * 응용 프로그램이 C++로 작성되었을 경우 코드에 shobjidl.h 헤더 파일에 대한 참조를 추가하면 됩니다. 이 헤더 파일에는 **IInitializeWithWindow** 인터페이스의 선언이 포함되어 있습니다.

  2. [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 가져오려면 이 문서의 앞부분에서 설명한 대로 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 메서드(또는 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) - [다중 사용자 앱](../xbox-apps/multi-user-applications.md)일 경우)를 사용하고 이 개체를 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 개체로 캐스팅합니다. 그런 다음 [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) 메서드를 호출하고 **StoreContext** 메서드에서 표시한 모달 대화 상자의 소유자가 되도록 창의 핸들을 전달합니다. 다음 C# 예제에서는 앱 주 창의 핸들을 메서드에 전달하는 방법을 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />
### <a name="products-skus-and-availabilities"></a>제품, SKU 및 가용성

스토어의 모든 제품에는 *SKU*가 하나 이상 있으며, 각 SKU에 *가용성*이 하나 이상 있습니다. 대부분의 개발자는 Windows 개발자 센터 대시보드에서 이러한 개념을 도외시하며 해당 앱이나 추가 기능에 대한 SKU 또는 가용성을 정의하지 않습니다. 그러나 **Windows.Services.Store** 네임스페이스의 스토어 제품에 대한 개체 모델에는 SKU와 가용성이 포함되어 있으므로 이러한 개념에 대한 기본적인 이해가 도움이 될 수 있습니다.

| 개체 유형 |  설명  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  이 클래스는 앱 또는 추가 기능을 포함하여 스토어에서 사용할 수 있는 모든 유형의 제품을 나타냅니다. 이 클래스는 제품의 스토어 ID, 스토어 목록에 사용할 이미지 및 비디오, 가격 정보 등의 데이터에 액세스하는 데 사용할 수 있는 속성을 제공합니다. 또한 제품을 구매하는 데 사용할 수 있는 메서드를 제공합니다. |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  이 클래스는 제품에 대한 *SKU*를 나타냅니다. SKU는 자체 설명, 가격 및 기타 고유한 제품 정보가 있는 특정 버전의 제품입니다. 앱이나 추가 기능마다 기본 SKU가 있습니다. 대부분의 개발자가 앱용 SKU를 여러 개 사용할 때는 전체 버전의 앱과 평가판을 게시하는 경우뿐입니다(스토어 카탈로그에서 이러한 각 버전은 동일한 앱의 다른 SKU임). <p/><p/> 일부 판매자는 해당 SKU를 정의할 수 있습니다. 예를 들어 대규모 게임 판매자가 빨간색 피를 허용하지 않는 지역/국가에서 녹색 피를 표시하는 SKU와 다른 모든 지역/국가에서 빨간색 피를 표시하는 SKU로 게임을 출시할 수 있습니다. 또는 디지털 비디오 콘텐츠의 판매자가 HD 버전용 SKU와 표준 화질 버전용 SKU의 두 SKU를 게시할 수 있습니다. <p/><p/> 각 제품에는 SKU에 액세스할 수 있는 [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) 속성이 있습니다. |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  이 클래스는 SKU에 대한 *가용성*을 나타냅니다. 가용성은 고유한 가격 정보가 있는 특정 버전의 SKU입니다. SKU마다 기본 가용성이 있습니다. 일부 판매자는 고유한 가용성을 정의하여 주어진 SKU에 대해 다른 가격 옵션을 도입할 수 있습니다. <p/><p/> 각 SKU에는 가용성에 액세스할 수 있는 [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) 속성이 있습니다. 대부분의 개발자의 경우 SKU마다 하나의 기본 가용성이 있습니다.  |

<span id="store_ids" />
### <a name="store-ids"></a>스토어 ID

스토어의 모든 앱과 추가 기능에는 관련 **스토어 ID**가 있습니다. **Windows.Services.Store** 네임스페이스에 있는 대부분의 API는 스토어 ID가 있어야 앱이나 추가 기능에 대한 작업을 수행할 수 있습니다. 제품, SKU 및 가용성은 서로 다른 스토어 ID 형식을 사용합니다.

| 개체 유형 |  스토어 ID 형식  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  스토어에 있는 모든 제품의 스토어 ID는 12자의 영숫자 문자열입니다(예: ```9NBLGGH4R315```). 이 스토어 ID는 앱 또는 추가 기능에 대한 Windows 개발자 센터 대시보드 페이지에서 제공되며 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) 개체의 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 속성에 의해 반환됩니다. 이 ID를 *제품 스토어 ID*라고도 합니다. |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  SKU의 스토어 ID는 ```<product Store ID>/xxxx``` 형식을 사용합니다. 여기서 ```xxxx```는 제품의 SKU를 식별하는 4자리 영숫자 문자열입니다. 예를 들면 ```9NBLGGH4R315/000N```입니다. 이 ID는 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 개체의 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) 속성에서 반환하며 *SKU 스토어 ID*라고도 합니다. |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  사용 가능성의 스토어 ID는 ```<product Store ID>/xxxx/yyyyyyyyyyyy``` 형식을 사용합니다. 여기서 ```xxxx```는 제품의 SKU를 식별하는 4자리 영숫자 문자열이고, ```yyyyyyyyyyyy```는 SKU의 가용성을 식별하는 12자리 영숫자 문자열입니다. 예를 들면 ```9NBLGGH4R315/000N/4KW6QZD2VN6X```입니다. 이 ID는 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 개체의 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) 속성에서 반환하며 * 스토어 ID*라고도 합니다.  |

## <a name="related-topics"></a>관련 항목

* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Dec16_HO4-->


