---
author: jnHs
Description: You can set the precise date and time that your app should become available in the Store, giving you greater flexibility and the ability to customize dates for different markets.
title: 정확한 릴리스 일정 구성
ms.author: wdg-dev-content
ms.date: 05/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 일정, 발매 날짜, 날짜, 실행
ms.localizationpriority: medium
ms.openlocfilehash: 84466f907bad7e38506e1bf81b89eb631675093c
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4122352"
---
# <a name="configure-precise-release-scheduling"></a>정확한 릴리스 일정 구성

[가격 책정 및 가용성](set-app-pricing-and-availability.md) 페이지의 **일정** 섹션에서 Microsoft Store에서 앱을 사용할 수 있는 정확한 날짜와 시간을 설정할 수 있기 때문에 유연성이 뛰어나며 시장별로 날짜를 사용자 지정할 수 있습니다.

> [!NOTE]
> 이 항목은 앱을 가리키지만, 추가 기능 제출을 위한 릴리스 일정은 동일한 프로세스를 사용합니다.

Microsoft Store에서 앱을 더 이상 사용할 수 없게 만드는 날짜도 추가로 설정할 수 있습니다. Microsoft Store에서 검색이나 탐색으로 제품을 찾을 수 없지만, 직접 링크가 있는 고객은 제품의 Microsoft Store 목록을 볼 수 있습니다. 그러나 제품을 이미 소유하고 있거나, [홍보 코드](generate-promotional-codes.md)를 갖고 있으며, Windows 10 장치를 사용하고 있는 경우에만 다운로드 할 수 있습니다.

기본적으로 ([표시 여부](choose-visibility-options.md#discoverability) 섹션에서 **앱을 사용할 수 있지만, Microsoft Store에서 검색할 수 없도록 설정** 옵션을 선택한 경우를 제외하고), 앱 인증과 게시 프로세스가 완료되는 즉시 고객이 앱을 사용할 수 있습니다. 다른 날짜를 선택 하려면 **옵션 표시**를 선택해 이 섹션을 확장하세요.

[표시 여부](choose-visibility-options.md#discoverability) 섹션에서 **앱을 사용할 수 있지만 Microsoft Store에서 검색할 수 없도록 설정** 옵션을 선택했다면 **일정**에서 날짜를 구성할 수 없습니다. 앱이 고객에게 릴리스되지 않아 구성할 릴리스 날짜가 없기 때문입니다.

> [!IMPORTANT]
> 일정 섹션에서 지정한 날짜는 Windows 10 고객에게만 적용됩니다.
>
>앱이 이전 OS 버전을 지원하는 경우, 이후의 릴리스 날짜를 선택했다 하더라도 해당 OS 버전을 이용하는 고객은 인증과 게시 프로세스가 완료된 즉시 앱 목록을 확인할 수 있습니다. 이들 고객에게는 **취득 중지** 날짜가 적용되지 않습니다. ([표시 여부](choose-visibility-options.md#discoverability) 섹션에서 다른 선택으로 업데이트를 제출하건, **앱 개요** 페이지에서 **앱을 사용할 수 없도록 설정**을 선택한 경우를 제외하고)여전히 앱을 취득할 수 있습니다.


## <a name="base-schedule"></a>기본 일정

기본 일정에서 선택한 사항은 나중에 [특정 지역/국가 대해 사용자 지정](#customize-the-schedule-for-specific-markets)을 선택해 특정 지역/국가(또는 지역/국가 그룹)을 대상으로 날짜를 추가한 경우를 제외하고, 앱을 사용할 수 있는 모든 지역/국가에 적용됩니다.

**릴리스** 및 **취득 중지** 두 가지 옵션이 표시됩니다. 

## <a name="release"></a>릴리스

**릴리스** 드롭다운을 이용, Microsoft Store에서 앱을 사용할 수 있는 때를 설정할 수 있습니다. Microsoft Store에서 검색이나 탐색으로 앱을 찾을 수 있고, 고객이 Microsoft Store 목록을 확인해 앱을 취득할 수 있습니다.

>[!NOTE]
> 앱이 Microsoft Store에 게시되어 사용할 수 있는 상태가 된 후에는 **릴리스** 날짜를 선택할 수 없습니다(앱을 이미 출시했기 때문).

다음은 제품 **릴리스** 일정에서 구성할 수 있는 옵션들입니다.
- **가능한 한 빨리**: 제품 인증과 게시가 완료된 즉시 릴리스됩니다. 기본값인 옵션입니다.
- **지정(at)**: 제품을 선택한 날짜와 시간에 릴리스합니다. 두 가지 추가 옵션이 있습니다.
   - **UTC**: 선택한 시간이 UTC(Universal Coordinated Time) 시간이기 때문에 모든 지역/국가에서 동일한 시간에 앱이 릴리스 됩니다.
   - **로컬**: 선택한 시간을 지역/국가의 시간대에 사용합니다. (시간대가 하나 이상인 지역/국가의 경우, 하나의 시간대만 사용합니다. 미국에서는 동부 표준 시간대를 사용합니다.)
- **예약하지 않음**: Microsoft Store에서 앱을 사용할 수 없습니다. 이 옵션을 선택하는 경우, 새로 제출하고 다른 옵션 중 하나를 선택해 나중에 Microsoft Store에서 앱을 사용할 수 있도록 설정할 수 있습니다.


## <a name="stop-acquisition"></a>취득 중지

**취득 중지** 드롭다운을 이용, 새 고객이 Microsoft Store에서 앱을 취득하지 못하고, 발견하지 못하도록 만드는 날짜와 시간을 설정할 수 있습니다. 하나 이상의 앱을 사용하는 것을 조정해야 할 때 등, 새 고객에게 더 이상 앱을 제공하지 않는 시기를 정확히 관리하기 원할 때 유용합니다.

**취득 주지**의 기본값은 '사용하지 않음'으로 설정되어 있습니다. 이를 변경하려면 드롭다운에서 **지정(at)** 을 선택해, 위에서 설명한대로 날짜와 시간을 지정하세요. 선택한 날짜와 시간부터 고객이 더 이상 앱을 취득할 수 없습니다.

[표시 여부](choose-visibility-options.md#discoverability) 섹션에서 **앱을 검색할 수 있지만 사용할 수 없도록 설정** 을 선택하고, **취득 중지: 직접 링크가 있는 고객은 제품의 Microsoft Store 목록을 확인할 수 있지만, 과거 제품을 소유했거나, 판촉 코드가 있고, Windows 10 장치를 사용 중인 경우에만 다운로드 가능**을 선택했을 때와 같은 효과가 있습니다. 앱이 새 고객에게 제공되지 않게 하려면 앱 개요 페이지에서 **앱을 사용할 수 없도록 설정**을 클릭합니다. 자세한 내용은 [Microsoft Store에서 앱 제거](guidance-for-app-package-management.md#removing-an-app-from-the-store)를 참조하세요.

> [!TIP]
> **취득 중지**날짜를 선택했는데, 나중에 앱을 다시 사용할 수 있도록 만들려면 새로 제출하고, **취득 중지**를 **사용 안함**으로 바꿔야 합니다. 업데이트한 제출이 게시된 후 앱이 다시 사용할 수 있는 상태가 됩니다.

## <a name="customize-the-schedule-for-specific-markets"></a>특정 지역/국가의 일정을 사용자 지정 

위에서 선택한 옵션이 앱을 제공하는 모든 지역/국가에 적용되는 것이 기본값입니다. 특정 지역/국가의 가격을 사용자 지정하려면 **특정 지역/국가 사용자 지정**을 클릭합니다. **지역/국가 선택** 팝업 창이 앱을 사용할 수 있도록 설정한 모든 지역/국가 목록을 표시합니다. [시장](define-pricing-and-market-selection.md) 섹션에서 제외한 시장들은 표시가 되지 않습니다. 

특정 지역/국가에 대한 일정을 추가하려면, 지역/국가를 선택한 후 **저장**을 클릭합니다. 위에서 설명한 **릴리스** 및 **취득 중지** 옵션이 표시되지만, 해당 지역/국가에만 선택 사항이 적용될 것입니다.

여러 지역/국가에 적용할 일정을 추가하려면 *지역/국가 그룹*을 생성합니다. 포함시킬 지역/국가를 선택한 후 해당 그룹의 이름을 입력합니다. (이름은 참고용으로 고객은 볼 수 없습니다.) 예를 들어, 북미에 대한 지역/국가 그룹을 생성하고 싶다면 **캐나다**, **멕시코**,**미국**을 선택한 후 **북미** 또는 다른 이름을 지정합니다. 지역/국가 그룹을 생성한 후 **저장**을 클릭합니다. 그러면 위에서 설명한 **릴리스** 및 **취득 중지** 옵션이 표시되지만, 해당 지역/국가 그룹에만 선택 사항이 적용되어 있을 것입니다.

추가로 다른 지역/국가의 일정을 사용자 지정하거나, 추가 지역/국가 그룹을 만들려면 **특정 지역/국가 사용자 지정**을 클릭한 후 위 단계를 반복합니다. 지역/국가 그룹의 지역/국가를 변경하려면 해당 이름을 선택합니다. 시장 그룹(또는 개별 시장)에 대한 사용자 지정 일정을 제거하려면 **제거**를 클릭합니다.

> [!NOTE]
> 하나의 시장은 **일정** 섹션에서 사용하는 시장 그룹 중 하나에만 소속됩니다. 










