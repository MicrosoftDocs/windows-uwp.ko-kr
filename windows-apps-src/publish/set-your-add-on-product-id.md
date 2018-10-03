---
author: jnHs
Description: When you create a new add-on in the Windows Dev Center dashboard, you need to specify a product type and assign it a product ID.
title: 추가 기능 제품 유형 및 제품 ID 설정
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 추가 기능, iap, 내구성, 소모품, 구독, 제품 종류, 제품 id, 앱에서 바로 구매, 앱에서 바로 구매 제품
ms.localizationpriority: medium
ms.openlocfilehash: 0673048fc9a1ed8fb7c439607ebc4197039699e9
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4263520"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>추가 기능 제품 유형 및 제품 ID 설정

아직 앱을 제출하지 않았더라도 이미 대시보드에서 만든 앱과 추가 기능을 연결해야 합니다. 앱의 **개요** 페이지나 **추가 기능** 페이지에서 **새 추가 기능 만들기** 단추를 찾을 수 있습니다.

**새 추가 기능 만들기**를 선택하고 나면 제품 유형을 지정하고 추가 기능에 대한 제품 ID를 할당하라는 메시지가 표시됩니다.

## <a name="product-type"></a>제품 유형

먼저 제공할 추가 기능 유형을 표시해야 합니다. 이 선택은 고객이 추가 기능을 사용하는 방법을 나타냅니다.

> [!NOTE]
> 이 페이지를 저장하여 추가 기능을 만든 후에는 제품 유형을 변경할 수 없습니다. 잘못된 제품 유형을 선택한 경우 언제든지 진행 중인 추가 기능 제출을 삭제하고 새 추가 기능을 만들어 다시 시작할 수 있습니다.

<span id="durable" />

### <a name="durable"></a>지속형

추가 기능을 보통 단 한 번 구입하는 경우에는 제품 유형으로 **지속형**을 선택합니다. 지속형 추가 기능은 일반적으로 앱에서 추가 기능을 잠금 해제하는 데 사용됩니다.

지속형 추가 기능의 기본 **제품 수명**은 **계속**으로, 이는 추가 기능이 만료되지 않음을 나타냅니다. 추가 기능 제출 프로세스의 [속성](enter-add-on-properties.md) 단계에서 **제품 수명**을 다른 기간으로 설정할 수 있습니다. 이렇게 할 경우 지정한 기간(1-365일 중 선택) 이후에 추가 기능이 만료되고, 고객은 만료 이후에 다시 구입을 할 수 있습니다.

<span id="consumable" />

### <a name="consumable"></a>소모성

추가 기능을 구매, 사용(소비) 및 재구매할 수 있는 경우 **소모성** 제품 유형 중 하나를 선택합니다. 소모성 추가 기능은 일반적으로 고객이 정해진 금액으로 구매하고 다 사용할 수 있는 게임 내 통화(골드, 코인 등) 같은 항목에 사용됩니다. 자세한 내용은 [소모성 추가 기능 구매 사용](../monetize/enable-consumable-add-on-purchases.md)를 참조하세요.

소모성 추가 기능에는 다음 두 가지 유형이 있습니다.
- **개발자 관리 소모성**: 잔액 및 이행이 앱 내에서 관리되어야 합니다. 모든 OS 버전에서 지원됩니다.
- **Microsoft Store 관리 소모성:** Windows 10 버전 1607 이상을 실행하는 모든 고객 장치의 잔액을 Microsoft에서 추적합니다. 이전 버전의 OS에서는 지원되지 않습니다. 이 옵션을 사용하려면 상위 제품이 Windows 10 SDK 버전 14393 이상을 사용하여 컴파일되어야 합니다. 또한 대시보드에서 제출을 만들어 언제든지 작업을 시작할 수는 있지만 상위 제품이 게시될 때까지 Microsoft Store 관리 소모성 추가 기능을 Microsoft Store에 제출할 수 없습니다. 제출의 **속성** 단계에서 Microsoft Store 관리 소모성 추가 기능에 대한 수량을 입력해야 합니다.

<span id="subscription" />

### <a name="subscription"></a>구독

추가 기능에 대해 고객에게 반복적으로 비용을 청구하고 싶은 경우에는 **구독**을 선택합니다.

고객이 처음에 구독 추가 기능을 구입한 이후에 추가 기능을 계속해서 사용할 수 있도록 반복해서 비용이 청구됩니다. 고객은 추가 요금이 부과되지 않도록 언제든 구독을 취소할 수 있습니다. 제출의 **속성** 단계에서 구독 기간과 무료 평가판의 제공 여부를 지정해야 합니다.

구독 추가 기능은 Windows 10 버전 1607 이상을 실행하는 고객에게만 지원됩니다. Windows10 SDK 버전 14393 이상을 사용하여 상위 앱을 컴파일하고 **Windows.ApplicationModel.Store** 네임스페이스 대신 **Windows.Services.Store** 네임스페이스에서 앱에서 바로 구매 API를 사용해야 합니다. 자세한 정보는 [앱에 구독 추가 기능을 사용하도록 설정](../monetize/enable-subscription-add-ons-for-your-app.md)을 참조하세요.

대시보드에서 제출을 만들어 언제든지 작업을 시작할 수는 있지만, 상위 제품을 제출해야 Microsoft Store에 구독 추가 기능을 게시할 수 있습니다.

## <a name="product-id"></a>제품 ID

원하는 제품 형식에 관계 없이 추가 기능 고유의 제품 ID를 입력해야 합니다. 이 이름은 대시보드에서 추가 기능을 식별하는 데 사용되며, 이 식별자를 사용하여 [코드에서 추가 기능을 참조](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code)합니다.

다음은 제품 ID를 선택할 때 유의해야 하는 몇 가지 사항입니다.

-   고객에게 이 제품 ID가 표시되지 않습니다. 나중에 고객에게 표시할 [제목 및 설명](create-add-on-descriptions.md)을 입력할 수 있습니다.
-   게시된 후에는 추가 기능 제품 ID를 변경하거나 삭제할 수 없습니다.
-   제품 ID는 100자 이내여야 합니다.
-   제품 ID에는 다음 문자를 사용할 수 없습니다. **&lt; &gt; \* % & : \\ ? + ,**
-   추가 기능을 모든 OS 버전에서 제공하려면 영숫자 문자, 마침표 및/또는 밑줄만 사용해야 합니다. 다른 종류의 문자를 사용하면 Windows Phone 8.1 이하를 실행하는 고객이 추가 기능을 구매할 수 없습니다.
-   제품 ID는 Microsoft Store 내에서 고유할 필요가 없지만 개발자 계정에서는 고유해야 합니다.
 
