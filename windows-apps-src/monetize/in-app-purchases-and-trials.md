---
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: UWP 앱에서 앱 내 구매 및 평가판 기능을 구현 하 고 테스트 하는 데 필요한 기본 작업 및 개념에 대해 알아봅니다.
title: 앱에서 바로 구매 및 평가판
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10, uwp, 앱 내 구매, IAPs, 추가 기능, 평가판, 소비재, 내구성이 있는 구독
ms.localizationpriority: medium
ms.openlocfilehash: fea4b222d604c2da1b97d692658abd8561ab1501
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942807"
---
# <a name="in-app-purchases-and-trials"></a>앱에서 바로 구매 및 평가판

Windows SDK는 다음 기능을 구현 하 여 UWP (유니버설 Windows 플랫폼) 앱에서 더 많은 비용을 생성 하는 데 사용할 수 있는 Api를 제공 합니다.

* **앱에서 바로 구매** &nbsp; &nbsp; 앱이 무료로 제공 되는지 여부에 관계 없이 앱 내에서 콘텐츠 또는 새로운 앱 기능 (예: 게임의 다음 수준 잠금 해제)을 판매할 수 있습니다.

* **평가판 기능** &nbsp; &nbsp; [파트너 센터에서 무료 평가판으로 앱을 구성](../publish/set-app-pricing-and-availability.md#free-trial)하는 경우 평가판 기간 동안 일부 기능을 제외 하거나 제한 하 여 앱의 전체 버전을 구입 하도록 고객에 게 유도할 수 있습니다. 고객이 앱을 구입 하기 전에 평가판 동안에만 표시 되는 배너 또는 워터 마크와 같은 기능을 사용 하도록 설정할 수도 있습니다.

이 문서에서는 앱에서 바로 구매와 평가판이 UWP 앱에서 작동하는 방식에 대해 간략하게 보여 줍니다.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>사용할 네임 스페이스 선택

앱이 대상으로 하는 Windows 10 버전에 따라 앱 내 구매 및 평가판 기능을 UWP 앱에 추가 하는 데 사용할 수 있는 두 가지 네임 스페이스가 있습니다. 이러한 네임 스페이스의 Api는 동일한 목표를 제공 하지만, 매우 다르게 디자인 되었으며 두 Api 간에 코드가 호환 되지 않습니다.

* **[Windows. 서비스 저장소](https://docs.microsoft.com/uwp/api/windows.services.store)** &nbsp; &nbsp; Windows 10 버전 1607부터 앱은이 네임 스페이스의 API를 사용 하 여 앱 내 구매 및 평가판을 구현할 수 있습니다. 앱 프로젝트가 **Windows 10 기념일 Edition (10.0;)을 대상으로 하는 경우이 네임 스페이스의 멤버를 사용 하는 것이 좋습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. 이 네임 스페이스는 저장소 관리 사용 가능 추가 기능과 같은 최신 추가 기능 형식을 지원 하며, 파트너 센터 및 스토어에서 지원 되는 이후 유형의 제품과 기능과 호환 되도록 설계 되었습니다. 이 네임 스페이스에 대 한 자세한 내용은이 문서의 [Windows. 서비스 네임 스페이스를 사용 하 여 앱 내 구매 및 평가판](#api_intro) 을 참조 하세요.

* **[Windows ApplicationModel. 저장소](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store)** &nbsp; &nbsp; 모든 버전의 Windows 10에서는이 네임 스페이스의 앱 내 구매 및 평가판에 대 한 이전 API도 지원 합니다. **Windows. ApplicationModel. store** 네임 스페이스에 대 한 자세한 내용은 [앱 내 구매 및 windows.](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)

> [!IMPORTANT]
> 더 이상 새 기능을 사용 하 여 **windows. ApplicationModel. store** 네임 스페이스를 업데이트 하지 않으며, 앱에 가능한 경우 대신 **windows. store** 네임 스페이스를 사용 하는 것이 좋습니다. [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop) 를 사용 하는 windows 데스크톱 응용 프로그램 또는 파트너 센터에서 개발 샌드박스를 사용 하는 앱 또는 게임 (예: Xbox Live와 통합 되는 게임의 경우)은 **windows** 데스크톱 응용 프로그램에서 지원 되지 않습니다.

<span id="concepts" />

## <a name="basic-concepts"></a>기본 개념

저장소에서 제공 되는 모든 항목은 일반적으로 *제품*이라고 합니다. 대부분의 개발자는 *앱* 및 *추가*기능과 같은 형식의 제품만 사용 합니다.

추가 기능은 앱의 컨텍스트에서 사용할 수 있는 제품 또는 기능입니다. 예를 들어 앱 또는 게임에서 사용 되는 통화, 게임을 위한 새 지도 또는 무기, 광고 없이 앱을 사용할 수 있는 기능, 해당 유형의 콘텐츠를 제공할 수 있는 앱에 대 한 음악 또는 비디오와 같은 디지털 콘텐츠 등이 있습니다. 모든 앱과 추가 기능에는 사용자에 게 앱 또는 추가 기능을 사용할 수 있는 권한이 있는지 여부를 나타내는 관련 라이선스가 있습니다. 사용자가 앱 또는 추가 기능을 평가판으로 사용할 수 있는 경우 라이선스는 평가판에 대 한 추가 정보도 제공 합니다.

앱의 고객에 게 추가 기능을 제공 하려면 저장소에서이를 인식할 수 있도록 [파트너 센터에서 앱에 대 한 추가 기능을 정의](../publish/add-on-submissions.md) 해야 합니다. 그런 다음 앱은 **windows** 의 api를 사용 하 여 앱 내 구매로 사용자에 게 판매할 추가 기능을 제공할 수 **있습니다.**

UWP 앱은 다음과 같은 유형의 추가 기능을 제공할 수 있습니다.

| 추가 기능 유형 |  Description  |
|---------|-------------------|
| 지속성  |  [파트너 센터에서 지정](../publish/enter-iap-properties.md)하는 수명 동안 지속 되는 추가 기능입니다. <p/><p/>기본적으로 지 속성 추가 기능은 만료 되지 않습니다 .이 경우 한 번만 구매할 수 있습니다. 추가 기능에 특정 기간을 지정 하는 경우 사용자는 만료 된 후 추가 기능을 다시 구매할 수 있습니다. |
| 개발자 관리 가능  |  사용 된 후 다시 구매할 수 있는 추가 기능입니다. 사용자는 추가 기능이 나타내는 항목의 사용자 잔액을 추적 해야 합니다.<p/><p/>사용자가 추가 기능과 연결 된 항목을 사용 하는 경우 사용자는 사용자의 균형을 유지 하 고 사용자가 모든 항목을 사용한 후 저장소에 대 한 추가 기능 구매를 보고 해야 합니다. 사용자는 앱이 이전 추가 기능 구매를 충족 된 것으로 보고 하기 전까지 추가 기능을 다시 구매할 수 없습니다. <p/><p/>예를 들어, 추가 기능이 게임에서 100 동전을 나타내며 사용자가 10kb를 사용 하는 경우 앱 이나 서비스는 사용자에 대해 새로운 남은 잔액을 90로 유지 해야 합니다. 사용자가 모든 100을 사용한 후 앱에서 추가 기능을 충족 된 것으로 보고 해야 합니다. 그러면 사용자가 100 동전 추가 기능을 다시 구매할 수 있습니다.    |
| 저장소 관리 가능  |  언제 든 지 구매, 사용 및 구매할 수 있는 추가 기능입니다. 저장소는 추가 기능이 나타내는 항목의 사용자 잔액을 추적 합니다.<p/><p/>사용자가 추가 기능과 연결 된 항목을 사용 하는 경우 해당 항목을 저장소에 대해 충족 된 것으로 보고 하 고, 스토어에서 사용자의 잔액을 업데이트 합니다. 사용자는 원하는 횟수 만큼 추가 기능을 구매할 수 있습니다 (항목을 먼저 사용할 필요는 없음). 앱은 언제 든 지 사용자의 현재 잔액을 쿼리할 수 있습니다. <p/><p/> 예를 들어 추가 기능이 게임에서 100 동전의 초기 수량을 표시 하 고 사용자가 50을 사용 하는 경우 앱은 추가 기능에 대 한 50 단위가 충족 된 상점에 보고 하 고, 저장소는 남은 잔액을 업데이트 합니다. 사용자가 추가 기능을 다시 구매 하 여 더 100 동전을 획득 하는 경우에는 이제 150 동전 합계가 발생 합니다. <p/><p/>**Note** &nbsp; 참고 &nbsp; 스토어에서 관리 되는 소모품을 사용 하려면 앱 **이 Windows 10 기념일 Edition (10.0;)을 대상으로 해야 합니다. 빌드 14393)** 이상 릴리스를 사용 하 고 Visual Studio의 이후 버전을 사용 **Windows.Services.Store** 해야 하며, **windows.**  |
| 구독 | 추가 기능을 계속 사용 하기 위해 고객이 되풀이 간격으로 계속 청구 되는 지 속성 추가 기능입니다. 고객은 언제 든 지 구독을 취소 하 여 추가 요금을 피할 수 있습니다. <p/><p/>**Note** &nbsp; 참고 &nbsp; 구독 추가 기능을 사용 하려면 앱 **이 Windows 10 기념일 Edition (10.0;)을 대상으로 해야 합니다. 빌드 14393)** 이상 릴리스를 사용 하 고 Visual Studio의 이후 버전을 사용 **Windows.Services.Store** 해야 하며, **windows.**  |

<span />

> [!NOTE]
> 패키지가 포함 된 내구성이 있는 추가 기능 (다운로드 가능한 콘텐츠 또는 DLC 라고도 함)과 같은 다른 유형의 추가 기능은 제한 된 개발자 에게만 제공 되며이 설명서에서는 다루지 않습니다.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Windows. Store 네임 스페이스를 사용 하 여 앱에서 바로 구매 및 평가판 사용

이 섹션에서는 [Windows. Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임 스페이스에 대 한 중요 한 작업 및 개념에 대 한 개요를 제공 합니다. 이 네임 스페이스는 **Windows 10 기념일 Edition (10.0;)을 대상으로 하는 앱 에서만 사용할 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 버전 (Windows 10, 버전 1607에 해당). 가능 하면 앱에서 [windows. ApplicationModel. store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 네임 스페이스 대신 **windows. store** 네임 스페이스를 사용 하는 것이 좋습니다. **Windows.** e s e. Store 네임 스페이스에 대 한 자세한 내용은 [이 문서](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)를 참조 하세요.

**이 섹션의 내용**

* [비디오](#video)
* [StoreContext 클래스 시작](#get-started-storecontext)
* [앱에서 바로 구매 구현](#implement-iap)
* [평가판 기능 구현](#implement-trial)
* [앱 내 구매 또는 평가판 구현 테스트](#testing)
* [앱에서 바로 구매 수신 확인](#receipts)
* [데스크톱 브리지로 StoreContext 클래스 사용](#desktop)
* [제품, Sku 및 전반](#products-skus)
* [저장소 Id](#store-ids)

<span id="video" />

### <a name="video"></a>동영상

[Windows. Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임 스페이스를 사용 하 여 앱에서 앱 내 구매를 구현 하는 방법에 대 한 개요는 다음 비디오를 시청 하세요.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>StoreContext 클래스 시작

**Windows. Store** 네임 스페이스에 대 한 기본 진입점은 [storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스입니다. 이 클래스는 현재 앱 및 사용 가능한 추가 기능에 대 한 정보를 가져오고 현재 앱 또는 해당 추가 기능에 대 한 라이선스 정보를 가져오고 현재 사용자에 대 한 앱 또는 추가 기능을 구입 하 고 기타 작업을 수행 하는 데 사용할 수 있는 메서드를 제공 합니다. [Storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 개체를 가져오려면 다음 중 하나를 수행 합니다.

* 단일 사용자 앱 (즉, 앱을 시작한 사용자의 컨텍스트에서만 실행 되는 앱)에서 정적 [Getdefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) 메서드를 사용 하 여 사용자에 대 한 Microsoft Store 관련 데이터에 액세스 하는 데 사용할 수 있는 **storecontext** 개체를 가져옵니다. 대부분의 UWP(유니버설 Windows 플랫폼) 앱은 단일 사용자 앱입니다.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* [다중 사용자 앱](../xbox-apps/multi-user-applications.md)에서 정적 [getforuser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) 메서드를 사용 하 여 응용 프로그램을 사용 하는 동안 Microsoft 계정 로그인 된 특정 사용자에 대 한 Microsoft Store 관련 데이터에 액세스 하는 데 사용할 수 있는 **storecontext** 개체를 가져옵니다. 다음 예제에서는 사용 가능한 첫 번째 사용자에 대 한 **Storecontext** 개체를 가져옵니다.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop) 를 사용 하는 Windows 데스크톱 응용 프로그램은이 개체를 사용 하기 전에 [storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 개체를 구성 하기 위해 추가 단계를 수행 해야 합니다. 자세한 내용은 [이 섹션](#desktop)을 참조하세요.

[Storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 개체를 만든 후에는이 개체의 메서드를 호출 하 여 현재 앱 및 해당 추가 기능에 대 한 제품 정보를 저장 하 고, 현재 앱 및 해당 추가 기능에 대 한 라이선스 정보를 검색 하 고, 현재 사용자에 대 한 앱 또는 추가 기능을 구입 하 고, 기타 작업을 수행할 수 있습니다. 이 개체를 사용 하 여 수행할 수 있는 일반적인 태스크에 대 한 자세한 내용은 다음 문서를 참조 하세요.

* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱에 구독 추가 기능을 사용하도록 설정](enable-subscription-add-ons-for-your-app.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)

**Windows. store** 네임 스페이스에서 **storecontext** 및 기타 형식을 사용 하는 방법을 보여 주는 샘플 앱은 [store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조 하세요.

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>앱에서 바로 구매 구현

**Windows. Store** 네임 스페이스를 사용 하 여 앱의 고객에 게 앱 내 구매를 제공 하려면 다음을 수행 합니다.

1. 앱이 고객이 구매할 수 있는 추가 기능을 제공 하는 경우 [파트너 센터에서 앱에 대 한 추가 기능 제출을 만듭니다 ](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions).

2. 앱에 코드를 작성 하 여 앱에서 [제공 하는 추가 기능 또는 앱에서 제공 하는 추가 기능에 대 한 제품 정보를 검색](get-product-info-for-apps-and-add-ons.md) 한 다음 [라이선스가 활성화 되어 있는지](get-license-info-for-apps-and-add-ons.md) 여부 (사용자에 게 앱 또는 추가 기능을 사용할 수 있는 라이선스가 있는지 여부)를 확인 합니다. 라이선스가 활성화 되지 않은 경우 앱 내 구매로 사용자에 게 판매할 앱 또는 추가 기능을 제공 하는 UI를 표시 합니다.

3. 사용자가 앱 또는 추가 기능을 구입 하도록 선택 하는 경우 적절 한 방법을 사용 하 여 제품을 구입 합니다.

    * 사용자가 앱 또는 내구성이 있는 추가 기능을 구입 하는 경우 앱 [내 앱 및 추가 기능 사용](enable-in-app-purchases-of-apps-and-add-ons.md)의 지침을 따르세요.
    * 사용자가 사용 가능 추가 기능을 구입 하는 경우 [사용 가능한 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)의 지침을 따릅니다.
    * 사용자가 구독 추가 기능을 구입 하는 경우 [앱에 대 한 구독 추가 기능 사용](enable-subscription-add-ons-for-your-app.md)의 지침을 따르세요.

4. 이 문서의 [테스트 지침](#testing) 에 따라 구현을 테스트 합니다.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>평가판 기능 구현

**Windows. Store** 네임 스페이스를 사용 하 여 앱 평가판의 기능을 제외 하거나 제한 하려면 다음을 수행 합니다.

1. [파트너 센터에서 무료 평가판으로 앱을 구성](../publish/set-app-pricing-and-availability.md#free-trial)합니다.

2. 앱에 코드를 작성 하 여 앱에서 제공 하는 [추가 기능 또는 앱에 대 한 제품 정보를 검색](get-product-info-for-apps-and-add-ons.md) 한 다음 [앱과 연결 된 라이선스가 평가판 라이선스 인지 여부를 확인](get-license-info-for-apps-and-add-ons.md)합니다.

3. 평가판 인 경우 앱의 특정 기능을 제외 하거나 제한 한 다음, 사용자가 정식 라이선스를 구매 하면 기능을 사용 하도록 설정 합니다. 자세한 내용 및 코드 예제는 [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)을 참조 하세요.

4. 이 문서의 [테스트 지침](#testing) 에 따라 구현을 테스트 합니다.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>앱 내 구매 또는 평가판 구현 테스트

앱에서 **Windows. store** 네임 스페이스의 api를 사용 하 여 앱 내 구매 또는 평가판 기능을 구현 하는 경우 앱을 스토어에 게시 하 고 개발 장치에 앱을 다운로드 하 여 테스트에 라이선스를 사용 해야 합니다. 다음 프로세스에 따라 코드를 테스트 합니다.

1. 앱이 아직 게시 되지 않은 상태에서 스토어에서 사용할 수 있는 경우 앱이 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족 하는지 확인 하 고, 파트너 센터에서 [앱을 제출](https://docs.microsoft.com/windows/uwp/publish/app-submissions) 하 고, 앱이 인증 프로세스를 통과 하는지 확인 합니다. 앱을 테스트 하는 동안 [스토어에서 검색할 수 없도록 앱을 구성할](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) 수 있습니다. [패키지를 항공편](../publish/package-flights.md)으로 적절 하 게 구성 하는 방법에 유의 하세요. 잘못 구성 된 패키지 항공편은 다운로드 하지 못할 수 있습니다.

2. 다음 작업을 완료 했는지 확인 합니다.

    * 앱에서 [Storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스 및 기타 관련 형식을 사용 하 여 [앱 내 구매](#implement-iap) 또는 [평가판 기능](#implement-trial)을 구현 하는 코드를 응용 프로그램에 작성 **합니다.**
    * 앱이 고객이 구매할 수 있는 추가 기능을 제공 하는 경우 [파트너 센터에서 앱에 대 한 추가 기능 제출을 만듭니다](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions).
    * 앱 평가판에서 일부 기능을 제외 하거나 제한 하려면 [파트너 센터에서 무료 평가판으로 앱을 구성](../publish/set-app-pricing-and-availability.md#free-trial)하세요.

3. Visual Studio에서 프로젝트를 열고 **프로젝트 메뉴**를 클릭 한 다음 **저장**을 가리키고 **스토어와 앱 연결**을 클릭 합니다. 마법사의 지침을 완료 하 여 테스트에 사용 하려는 파트너 센터 계정의 앱과 앱 프로젝트를 연결 합니다.
    > [!NOTE]
    > 프로젝트를 스토어의 앱에 연결 하지 않는 경우 [Storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 메서드는 반환 값의 **extendederror** 속성을 오류 코드 값 0x803F6107로 설정 합니다. 이 값은 스토어에 앱에 대 한 정보가 없음을 나타냅니다.
4. 아직 수행 하지 않은 경우 이전 단계에서 지정한 스토어에서 앱을 설치 하 고 앱을 한 번 실행 한 다음이 앱을 닫습니다. 이렇게 하면 앱에 대 한 유효한 라이선스가 개발 장치에 설치 됩니다.

5. Visual Studio에서 프로젝트의 실행 또는 디버깅을 시작 합니다. 코드는 로컬 프로젝트에 연결 된 스토어 앱에서 앱 및 추가 기능 데이터를 검색 해야 합니다. 앱을 다시 설치할지 묻는 메시지가 표시 되 면 지침에 따라 프로젝트를 실행 하거나 디버그 합니다.
    > [!NOTE]
    > 이러한 단계를 완료 한 후에는 앱의 코드를 계속 업데이트 한 다음 새 앱 패키지를 스토어에 제출 하지 않고 개발 컴퓨터에서 업데이트 된 프로젝트를 디버그할 수 있습니다. 테스트에 사용할 로컬 라이선스를 얻기 위해 응용 프로그램의 스토어 버전을 개발 컴퓨터에 한 번 다운로드 하기만 하면 됩니다. 테스트를 완료 한 후 앱에서 앱 구매 또는 평가판 관련 기능을 고객에 게 제공 하려는 경우에만 새 앱 패키지를 스토어에 제출 해야 합니다.

앱에서 **Windows.** c a s e. store 네임 스페이스를 사용 하는 경우 앱을 스토어에 제출 하기 전에 앱에서 [currentappsimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 클래스를 사용 하 여 테스트 중에 라이선스 정보를 시뮬레이션할 수 있습니다. 자세한 내용은 [CurrentApp 및 Currentapp시뮬레이터 클래스 시작](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes)을 참조 하세요.  

> [!NOTE]
> **Windows. Store** 네임 스페이스는 테스트 하는 동안 라이선스 정보를 시뮬레이트하는 데 사용할 수 있는 클래스를 제공 하지 않습니다. 앱 내 구매 또는 평가판을 구현 하기 위해 **Windows. store** 네임 스페이스를 사용 하는 경우 앱을 스토어에 게시 하 고 개발 장치에 앱을 다운로드 하 여 위에서 설명한 대로 테스트에 라이선스를 사용 해야 합니다.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>앱에서 바로 구매 수신 확인

응용 프로그램 코드에서 성공적인 구매를 위해 트랜잭션 수신을 수신 하는 데 사용할 수 있는 API를 제공 하지 **않습니다.** 이는 [클라이언트 쪽 API를 사용 하 여 트랜잭션 수신을 검색 하](use-receipts-to-verify-product-purchases.md)는 응용 프로그램에서 사용 하는 다른 환경 **입니다.**

**Windows. Store** 네임 스페이스를 사용 하 여 앱 내 구매를 구현 하는 경우 지정 된 고객이 앱 또는 추가 기능을 구입 했는지 여부를 확인 하려는 경우 [Microsoft Store 컬렉션 REST API](view-and-grant-products-from-a-service.md)에서 [products에 대 한 쿼리 메서드](query-for-products.md) 를 사용할 수 있습니다. 이 메서드에 대 한 반환 데이터는 지정 된 고객에 게 지정 된 제품에 대 한 자격이 있는지 여부를 확인 하 고 사용자가 제품을 획득 한 트랜잭션에 대 한 데이터를 제공 합니다. Microsoft Store collection API는 Azure AD 인증을 사용 하 여이 정보를 검색 합니다.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>데스크톱 브리지로 StoreContext 클래스 사용

[데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop) 를 사용 하는 데스크톱 응용 프로그램은 [storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스를 사용 하 여 앱 내 구매 및 평가판을 구현할 수 있습니다. 그러나 Win32 데스크톱 응용 프로그램 또는 렌더링 프레임 워크 (예: WPF 응용 프로그램)와 연결 된 창 핸들 (HWND)이 있는 데스크톱 응용 프로그램이 있는 경우 응용 프로그램에서 개체에 표시 되는 모달 대화 상자의 소유자 창인 응용 프로그램 창을 지정 하도록 **Storecontext** 개체를 구성 해야 합니다.

많은 **storecontext** 멤버 (및 **storecontext** 개체를 통해 액세스 되는 다른 관련 형식의 멤버)는 제품 구매와 같은 저장 관련 작업을 위해 사용자에 게 모달 대화 상자를 표시 합니다. 데스크톱 응용 프로그램에서 **Storecontext** 개체를 구성 하 여 모달 대화 상자의 소유자 창을 지정 하지 않는 경우이 개체는 부정확 한 데이터 또는 오류를 반환 합니다.

데스크톱 브리지를 사용 하는 데스크톱 응용 프로그램에서 **Storecontext** 개체를 구성 하려면 다음 단계를 수행 합니다.

1. 다음 중 하나를 수행 하 여 앱이 [Iinitializewithwindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) 인터페이스에 액세스할 수 있도록 합니다.

    * 응용 프로그램이 c # 또는 Visual Basic와 같은 관리 되는 언어로 작성 된 경우 다음 c # 예제와 같이 [ComImport](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) 특성을 사용 하 여 앱 코드에서 **Iinitializewithwindow** 인터페이스를 선언 합니다. 이 예제에서는 코드 파일에 **t e m** 네임 스페이스에 대 한 **using** 문이 있다고 가정 합니다.

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * 응용 프로그램이 c + +로 작성 되는 경우 코드에 shobjidl .h 헤더 파일에 대 한 참조를 추가 합니다. 이 헤더 파일에는 **Iinitializewithwindow** 인터페이스의 선언이 포함 되어 있습니다.

2. 이 문서 앞부분에서 설명한 대로 [Getdefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) 메서드 (또는 앱이 [다중 사용자 앱](../xbox-apps/multi-user-applications.md)인 경우 [getforuser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) )를 사용 하 여 [storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 개체를 가져온 다음이 개체를 [iinitializewithwindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) 개체로 캐스팅 합니다. 그런 다음 [IInitializeWithWindow.Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize) 메서드를 호출 하 고, **storecontext** 메서드로 표시 되는 모달 대화 상자의 소유자가 될 창의 핸들을 전달 합니다. 다음 c # 예제에서는 응용 프로그램의 주 창 핸들을 메서드에 전달 하는 방법을 보여 줍니다.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>제품, Sku 및 전반

상점의 모든 제품에는 하나 이상의 *sku*가 있으며 각 sku에는 하나 이상의 *가용성이*있습니다. 이러한 개념은 파트너 센터의 대부분의 개발자 로부터 추상화 되며 대부분의 개발자는 앱 또는 추가 기능에 대 한 Sku 또는 전반를 정의 하지 않습니다. 그러나 **전반 네임 스페이스의 제품** 에 대 한 개체 모델에는 sku 및 포함 되기 때문에 이러한 개념에 대 한 기본적인 이해는 일부 시나리오에 유용할 수 있습니다.

| Object |  Description  |
|---------|-------------------|
| 제품  |  *제품은* 앱 또는 추가 기능을 포함 하 여 스토어에서 사용할 수 있는 모든 유형의 제품을 의미 합니다. <p/><p/> 상점의 각 제품에는 해당 하는 저장소 [제품](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 개체가 있습니다. 이 클래스는 제품의 상점 ID, 매장 목록의 이미지 및 비디오, 가격 정보 등의 데이터에 액세스 하는 데 사용할 수 있는 속성을 제공 합니다. 또한 제품을 구매 하는 데 사용할 수 있는 메서드를 제공 합니다. |
| SKU |  *SKU* 는 고유한 설명, 가격 및 기타 고유한 제품 정보를 포함 하는 제품의 특정 버전입니다. 각 앱 또는 추가 기능에는 기본 SKU가 있습니다. 대부분의 개발자가 앱에 대 한 Sku를 여러 개 보유 하 게 되는 유일한 경우는 앱의 전체 버전을 게시 하 고 평가판 버전을 게시 하는 경우입니다. 스토어 카탈로그에서는 이러한 각 버전이 동일한 앱의 다른 SKU입니다. <p/><p/> 일부 게시자는 자신의 Sku를 정의할 수 있습니다. 예를 들어, 대량 게임 게시자는 다른 모든 시장에서 레드 피를 표시 하는 다른 SKU와 빨강 블러드를 허용 하지 않는 시장의 녹색 블러드를 표시 하는 하나의 SKU가 있는 게임을 릴리스할 수 있습니다. 또는 디지털 비디오 콘텐츠를 판매 하는 게시자는 비디오에 대해 두 개의 Sku, 즉 높은 정의 버전용 SKU와 표준 정의 버전의 다른 sku를 게시할 수 있습니다. <p/><p/> 상점의 각 SKU에는 해당 [StoreSku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) 개체가 있습니다. 모든 사용자 [제품](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 에는 제품의 sku에 액세스 하는 데 사용할 수 있는 [sku](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) 속성이 있습니다. |
| 가용성  |  *가용성* 은 고유한 가격 책정 정보를 포함 하는 특정 버전의 SKU입니다. 각 SKU에는 기본 가용성이 있습니다. 일부 게시자는 지정 된 SKU에 대해 다양 한 가격 옵션을 도입 하기 위해 고유한 전반를 정의 하는 기능을 제공 합니다. <p/><p/> 저장소의 각 가용성에는 해당 하는 저장소 [가용성](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) 개체가 있습니다. 모든 [StoreSku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) 에는 SKU에 대 한 전반에 액세스 하는 데 사용할 수 있는 [전반](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) 속성이 있습니다. 대부분의 개발자는 각 SKU에 단일 기본 가용성이 있습니다.  |

<span id="store_ids" />

### <a name="store-ids"></a>저장소 Id

스토어의 모든 앱, 추가 기능 또는 기타 제품에는 연결 된 **저장소 id** ( *제품 저장소 id*라고도 함)가 있습니다. 앱 또는 추가 기능에 대 한 작업을 수행 하기 위해 많은 Api에 저장소 ID가 필요 합니다.

저장소에 있는 모든 제품의 저장소 ID는와 같은 12 자의 영숫자 문자열입니다 ```9NBLGGH4R315``` . 매장에서 제품의 상점 ID를 가져오는 방법에는 여러 가지가 있습니다.

* 앱의 경우 파트너 센터의 [앱 id 페이지](../publish/view-app-identity-details.md) 에서 상점 id를 가져올 수 있습니다.
* 추가 기능의 경우 파트너 센터의 추가 기능 개요 페이지에서 상점 ID를 가져올 수 있습니다.
* 제품의 경우 제품을 나타내는 StoreId [product](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 개체의 [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) 속성을 사용 하 여 프로그래밍 방식으로 상점 ID를 가져올 수도 있습니다.

Sku 및 전반이 포함 된 제품의 경우 Sku와 전반에는 다른 형식의 고유한 매장 Id도 있습니다.

| Object |  저장소 ID 형식  |
|---------|-------------------|
| SKU |  SKU의 저장소 ID에는 형식이 있습니다 ```<product Store ID>/xxxx``` ```xxxx``` . 여기서은 제품의 SKU를 식별 하는 4 자리 영숫자 문자열입니다. 예: ```9NBLGGH4R315/000N``` 이 ID는 [StoreSku](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) 개체의 [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid) 속성에서 반환 되며, *SKU 저장소 id*라고도 합니다. |
| 가용성  |  가용성의 저장소 ID에는 형식이 있습니다. ```<product Store ID>/xxxx/yyyyyyyyyyyy``` 여기서 ```xxxx``` 은 제품의 sku를 식별 하는 4 자리 영숫자 문자열이 며 ```yyyyyyyyyyyy``` sku의 가용성을 식별 하는 12 자의 영숫자 문자열입니다. 예: ```9NBLGGH4R315/000N/4KW6QZD2VN6X``` 이 ID는 [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid)  [가용성](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) 개체의 속성에 의해 반환 되 고 때로는 *가용성 저장소 id*라고도 합니다.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>코드에서 추가 기능에 대 한 제품 Id를 사용 하는 방법

앱의 컨텍스트에서 추가 기능을 고객에 게 제공 하려면 파트너 센터에서 [추가 기능 제출을 만들](../publish/add-on-submissions.md) 때 추가 기능에 대 한 [고유한 제품 ID를 입력](../publish/set-your-add-on-product-id.md#product-id) 해야 합니다. 이 제품 id를 사용 하 여 코드에서 추가 기능을 참조할 수 있습니다. 그러나 제품 ID를 사용할 수 있는 특정 시나리오는 앱에서 앱을 구매할 때 사용 하는 네임 스페이스에 따라 다릅니다.

> [!NOTE]
> 파트너 센터에서 추가 기능에 대해 입력 하는 제품 ID는 추가 기능의 [저장소 id](#store-ids)와 다릅니다. 저장소 ID는 파트너 센터에 의해 생성 됩니다.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Windows. Store 네임 스페이스를 사용 하는 앱

앱이 **StoreLicense 네임 스페이스** 를 사용 하는 경우 제품 ID를 사용 하 여 추가 기능을 나타내는 컴퓨터 [제품](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) 또는 추가 기능의 라이선스를 나타내는 [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) 를 쉽게 식별할 수 있습니다. 제품 ID는 StoreLicense 제품에 의해 노출 됩니다 [. inappoffertoken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) 및 [. inappoffertoken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken) 속성.

> [!NOTE]
> 제품 ID는 코드에서 추가 기능을 식별 하는 데 유용 하지만, 대부분의 경우에는 **Windows. store** 네임 스페이스의 작업에서 제품 id 대신 추가 기능 [저장소 ID](#store-ids) 를 사용 합니다. 예를 들어 앱에 대 한 하나 이상의 알려진 추가 기능을 프로그래밍 방식으로 검색 하려면 추가 기능에 대 한 저장소 Id (제품 Id 아님)를 [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync) 메서드에 전달 합니다. 마찬가지로, 사용할 수 있는 추가 기능을 충족 된 것으로 보고 하려면 제품 ID가 아닌 추가 기능 저장소 ID를 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 메서드에 전달 합니다.

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Windows ApplicationModel. Store 네임 스페이스를 사용 하는 앱

앱에서 **Windows. ApplicationModel. Store** 네임 스페이스를 사용 하는 경우 대부분의 작업에 대해 파트너 센터에서 추가 기능에 할당 한 제품 ID를 사용 해야 합니다. 예를 들면

* 제품 ID를 사용 하 여 추가 기능을 나타내는 제품 [목록](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting) 또는 추가 기능의 라이선스를 나타내는 제품 [라이선스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) 를 식별할 수 있습니다. 제품 ID는 제품 [목록. productid](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) 및 제품 [라이선스 productid](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId) 속성에 의해 노출 됩니다.

* [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 메서드를 사용 하 여 사용자에 대 한 추가 기능을 구매할 때 제품 ID를 지정 합니다. 자세한 내용은 [앱에서 바로 제품 구매 사용](enable-in-app-product-purchases.md)을 참조하세요.

* [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) 메서드를 사용 하 여 사용 가능한 추가 기능을 충족 된 것으로 보고할 때 제품 ID를 지정 합니다. 자세한 내용은 [사용 가능한 앱 내 제품 구매 사용](enable-consumable-in-app-product-purchases.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱에 구독 추가 기능을 사용하도록 설정](enable-subscription-add-ons-for-your-app.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [Microsoft Store 작업에 대한 오류 코드](error-codes-for-store-operations.md)
* [앱에서 Windows. ApplicationModel. Store 네임 스페이스를 사용 하 여 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
