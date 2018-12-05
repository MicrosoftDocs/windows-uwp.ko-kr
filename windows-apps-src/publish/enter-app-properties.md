---
Description: The App properties page of the app submission process lets you define your app's category and indicate hardware preferences or other declarations.
title: 앱 속성 입력
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, 게임 설정, 표시 모드, 시스템 요구 사항, 하드웨어 요구 사항, 최소 하드웨어, 권장 하드웨어, 개인 정보 취급 방침, 지원 연락처 정보, 앱 웹 사이트, 지원 정보
ms.localizationpriority: medium
ms.openlocfilehash: 80220f8402b225691a2e4eb3202f1f04d48e06b4
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750476"
---
# <a name="enter-app-properties"></a>앱 속성 입력

[앱 제출 프로세스](app-submissions.md)의 **속성** 페이지에서 앱 범주를 정의하고 기타 정보 및 선언을 입력할 수 있습니다. 이 페이지에서 앱에 대한 완전하고 정확한 세부 정보를 제공해야 합니다.


## <a name="category-and-subcategory"></a>범주 및 하위 범주

Microsoft Store에서 앱을 분류하는 데 사용해야 하는 범주 및 하위 범주/장르(해당하는 경우)를 지정합니다. 앱을 제출하려면 범주를 지정해야 합니다.

자세한 내용은 [범주 및 하위 범주 테이블](category-and-subcategory-table.md)을 참조하세요.


## <a name="support-info"></a>지원 정보

이 섹션에는 고객이 앱과 지원을 받을 수 있는 방법에 대해 자세히 이해하는 데 도움이 되는 정보가 제공됩니다.

### <a name="privacy-policy-url"></a>개인 정보 취급 방침 URL

앱이 개인 정보 관련 법률 및 규정을 준수하는지 확인하고, 필요할 경우 유효한 개인 정보 취급 방침 URL을 제공하는 것은 개발자의 책임입니다.

이 섹션에서는 앱이 [개인 정보](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information)를 액세스, 수집 또는 전송하는지 여부를 표시해야 합니다. **예**라고 답하려면 개인 정보 취급 방침 URL이 필요합니다. 그렇지 않은 경우에는 선택 사항입니다(앱에서 개인 정보 취급 방침이 필요하다고 판단되지만 없는 경우에는 제출이 인증 실패할 수 있음).

> [!NOTE]
> 패키지가 개인 정보의 액세스, 전송 또는 수집을 허용하는 [기능](../packaging/app-capability-declarations.md)을 선언하는 것으로 감지가 되면 이 질문을 **예**로 표시하고 개인 정보 취급 방침 URL을 입력해야 합니다.

앱에서 개인 정보 취급 압침이 필요한지 여부를 판단하는 데 도움을 얻으려면 [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) 및 [Microsoft Store 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information)을 검토하세요. 

> [!NOTE]
> Microsoft는 앱에 대한 기본 개인 정보 취급 방침을 제공하지 않습니다. 마찬가지로, 사용자 앱에는 Microsoft 개인 정보 취급 방침이 적용되지 않습니다. 


### <a name="website"></a>웹 사이트

앱에 대한 웹 페이지의 URL을 입력합니다. 이 URL은 Store의 앱 웹 목록이 아니라 사용자 웹 사이트의 페이지를 가리켜야 합니다. 이 필드는 선택 사항이지만 권장 사항이기도 합니다.

### <a name="support-contact-info"></a>지원 연락처 정보

고객이 앱에 대한 지원을 받을 수 있는 웹 페이지의 URL(또는 지원을 받기 위해 연락할 메일 주소)을 입력합니다. 고객이 필요할 경우 지원을 받을 수 있는 방법을 알 수 있도록 모든 제출에 이 정보를 포함시키는 것이 좋습니다. Microsoft는 고객에게 앱에 대한 지원을 제공하지 않습니다.

> [!IMPORTANT]
> Xbox에서 앱이나 게임을 사용할 수 있는 경우에는 **지원 연락처 정보** 필드가 필요합니다. 그렇지 않은 경우에는 선택 사항이면서 권장 사항입니다.


## <a name="game-settings"></a>게임 설정

제품 범주를 **게임**으로 선택했을 때만 표시되는 범주입니다. 게임이 지원하는 기능을 지정할 수 있습니다. 이 섹션에 제공 된 정보는 제품의 스토어에 표시를 나열 합니다.

게임이 멀티 플레이어 옵션을 지원하는 경우, 세션의 최소/최대 플레이어 수를 지정해야 합니다. 최소/최대 플레이어의 수는 1,000명 이상 입력할 수 없습니다.

**플랫폼 간 멀티 플레이어**란 Windows 10 PC와 Xbox의 플레이어 간 멀티플레이어 세션을 지원한다는 의미입니다.


## <a name="display-mode"></a>디스플레이 모드

이 섹션에서는 제품이 PC 및/또는 HoloLens 디바이스에서 [Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality)에 대해 2D가 아닌 몰입형 보기로 실행되도록 설계되어 있는지 여부를 표시하도록 할 수 있습니다. 몰입형 보기임을 표시하면 다음 또한 수행해야 합니다.
- **속성** 페이지의 아래에 표시되는 [시스템 요구 사항](#system-requirements) 섹션에서 **Windows Mixed Reality 몰입형 헤드셋**에 대해 **최소 하드웨어** 또는 **권장 하드웨어**를 선택 합니다.
- **경계 설정**(PC가 선택된 경우)을 지정하여 앉거나 서서 사용해야 하는지, 또는 사용자가 이를 이동하면서 사용하는 것을 허용(또는 허락)하는지 여부를 알립니다. 

**게임**을 제품의 범주로 선택한 경우 **디스플레이 모드** 선택에 제품이 4K 해상도 비디오 출력, HDR(High Dynamic Range) 동영상 출력 또는 변수 새로 고침 빈도 디스플레이를 지원하는지 여부를 표시할 수 있는 추가 옵션이 표시됩니다.

제품이 이러한 디스플레이 모드 옵션을 모두 지원하지 않는 경우 모든 상자의 선택을 해제합니다.


## <a name="product-declarations"></a>제품 선언

이 섹션에서 확인란을 선택하여 앱에 선언이 적용되는지를 지정할 수 있습니다. 이는 앱 표시 방식, 특정 고객에게 앱을 제공할지 여부 또는 고객의 앱 사용 방식에 영향을 미칠 수 있습니다.

자세한 내용은 [제품 선언](app-declarations.md)을 참조하세요.

## <a name="system-requirements"></a>시스템 요구 사항

이 섹션에서는 앱을 제대로 실행 및 조작하기 위해 특정 하드웨어 기능이 필요하거나 권장되는지를 지정할 수 있습니다. **최소 하드웨어** 및/또는 **권장 하드웨어**를 지정하려는 각 하드웨어 항목에 대한 확인란을 선택(또는 적절한 옵션 지정)합니다.

**권장 하드웨어**를 선택할 경우 해당 항목이 Windows 10 버전 1607 이상 고객용 권장 하드웨어로 제품의 Microsoft Store 목록에 표시됩니다. 이전 OS 버전 고객에게는 이 정보가 표시되지 않습니다.

**최소 하드웨어**를 선택할 경우 해당 항목이 Windows 10 버전 1607 이상 고객용 필수 하드웨어로 제품의 Microsoft Store 목록에 표시됩니다. 이전 OS 버전 고객에게는 이 정보가 표시되지 않습니다. 또한 Microsoft Store는 필수 하드웨어가 없는 장치에서 앱의 목록을 보는 고객에게 경고를 표시할 수도 있습니다. 해당 하드웨어가 없는 장치에서도 앱을 다운로드할 수 있지만 이 장치에서 앱을 평가하거나 리뷰할 수는 없습니다. 

고객별 동작은 특정 요구 사항 및 고객의 Windows 버전에 따라 달라집니다.

- **Windows 10 버전 1607 이상 고객의 경우:**
     - 최소 및 권장 요구 사항이 Microsoft Store 목록에 모두 표시됩니다.
     - Microsoft Store는 모든 최소 요구 사항에 대해 확인하고 요구 사항을 충족하지 않는 장치의 고객에게 경고를 표시합니다.
- **Windows 10 이전 버전 고객의 경우:**
     - 대부분의 고객의 경우 모든 최소 및 권장 하드웨어 요구 사항이 Microsoft Store 목록에 표시됩니다(이전 버전의 Microsoft Store 클라이언트를 보는 고객에게는 최소 하드웨어 요구 사항만 표시됨).
     - Microsoft Store는 **메모리**, **DirectX**, **비디오 메모리**, **그래픽** 및 **프로세서**를 제외하고 **최소 하드웨어**로 지정하는 항목을 확인합니다. 확인되지 않으면 해당 요구 사항을 충족하지 않는 장치에 경고가 표시되지 않습니다. 
- **Windows 8.x 이하 버전 또는 Windows Phone 8.x 이하 버전 고객의 경우:**
     - **터치 스크린**용 **최소 하드웨어** 확인란을 선택하면 이 요구 사항이 앱의 Microsoft Store 목록에 표시되고 터치 스크린이 없는 장치의 고객이 앱을 다운로드하려고 할 경우 경고가 표시됩니다. 다른 요구 사항은 Microsoft Store 목록에서 확인 및 표시되지 않습니다.

Microsoft Store에서 고객의 장치에 선택한 기능이 없음을 검색하지 못할 수 있고 경고가 표시된 경우에도 고객이 여전히 앱을 다운로드할 수 있으므로 지정된 하드웨어에 대한 런타임 검사를 앱에 추가하는 것이 좋습니다. 메모리 또는 DirectX 수준에 대한 최소 요구 사항에 맞지 않는 장치에서 UWP 앱을 완전히 다운로드하지 못하게 하려면 [StoreManifest XML 파일](https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root)에 최소 요구 사항을 지정할 수 있습니다.

> [!TIP]
> 3D 프린터나 USB 장치 등을 올바르게 실행하기 위해 이 섹션에서 설명하지 않은 추가 항목이 제품에 필요한 경우, Store 목록을 작성할 때 [추가 시스템 요구 사항](create-app-store-listings.md#additional-system-requirements)을 입력해도 됩니다.





