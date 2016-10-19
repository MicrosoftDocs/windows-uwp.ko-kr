---
author: mcleanbyron
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: "Windows 10 버전 1607 이전 릴리스를 대상으로 하는 UWP 앱에서, 앱에서 바로 구매 및 평가판을 사용하는 방법을 알아봅니다."
title: "Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 649d082cddcf301fe602a5ab99637ad7bea67d49

---

# Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판

Windows SDK는 앱으로 수익을 창출하고 새로운 기능을 추가할 수 있도록 UWP(유니버설 Windows 플랫폼) 앱에 앱에서 바로 구매 및 평가판 기능을 추가하는 데 사용할 수 있는 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스의 멤버를 제공합니다. 이러한 API를 통해 앱에 대한 라이선스 정보에 액세스할 수도 있습니다.

>**참고**&nbsp;&nbsp;앱이 Windows 10 버전 1607 이상을 대상으로 하는 경우 **Windows.ApplicationModel.Store** 네임스페이스 대신 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스의 멤버를 사용하는 것이 좋습니다. **Windows.Services.Store** 네임스페이스는 스토어 관리 소모성 추가 기능 등의 최신 추가 기능 유형을 지원하며 Windows 개발자 센터 및 스토어에서 지원하는 이후 제품 및 기능 유형과 호환되도록 설계되었습니다. 또한 **Windows.Services.Store** 네임스페이스는 보다 나은 성능을 제공하도록 디자인되었습니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

이 섹션의 문서에서는 몇 가지 일반적인 시나리오에서 **Windows.ApplicationModel.Store** 네임스페이스의 멤버를 사용하기 위한 자세한 지침과 코드 예제를 제공합니다. UWP 앱의 앱에서 바로 구매와 관련된 개념의 개요를 보려면 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

**Windows.ApplicationModel.Store** 네임스페이스를 사용하여 평가판 및 앱에서 바로 구매를 구현하는 방법을 보여 주는 전체 샘플은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)을 참조하세요.

## 이 섹션의 내용


| 항목                                                                                                       | 설명                 |
|-------------------------------------------------------------------------------------------------------------|-----------------------------|
| [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)      |  앱이 무료인지 여부와 상관없이, 앱 내에서 바로 콘텐츠, 기타 앱 또는 새 앱 기능(예: 게임의 다음 단계 잠금 해제)을 판매할 수 있습니다. 여기서는 앱에서 이러한 제품을 사용하도록 설정하는 방법을 보여 줍니다.  |
| [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)      | 스토어 상거래 플랫폼을 통해 앱에서 바로 구매 소모성 제품(구매, 사용 및 필요에 따라 다시 구매할 수 있는 항목)을 제공하여 강력하고 안정적인 구매 환경을 고객에게 제공합니다. 이 기능은 특정 회복 아이템을 구매하여 사용할 수 있는 게임 내 통화(금, 동전 등) 등에 특히 유용합니다. |
| [평가판의 기능 제외 또는 제한](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | 평가 기간 동안 고객이 앱을 무료로 사용할 수 있게 하는 경우 평가 기간 동안 일부 기능을 제외하거나 제한하여 고객이 앱 정식 버전으로 업그레이드하도록 유도할 수 있습니다. |
| [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)      |   앱에서 대규모 앱에서 바로 구매 제품 카탈로그를 제공하는 경우 이 항목에 설명된 프로세스를 선택적으로 수행하여 카탈로그를 관리할 수 있습니다.    |
| [확인 메일을 사용하여 제품 구매 검증](use-receipts-to-verify-product-purchases.md)      |   제품 구매를 성공적으로 이행한 각 Windows 스토어 거래에서 고객에게 나열된 제품 및 통화 비용에 대한 정보를 제공하는 거래 영수증을 선택적으로 반환할 수 있습니다. 사용자가 앱을 구매했거나, Windows 스토어에서 앱에서 바로 구매 제품을 구매했는지 확인해야 하는 경우 이 정보에 액세스할 수 있습니다. |



<!--HONumber=Aug16_HO5-->


