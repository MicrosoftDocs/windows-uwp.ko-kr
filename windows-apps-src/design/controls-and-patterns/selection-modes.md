---
description: 선택 모드를 사용하면 사용자가 단일 항목 또는 여러 항목을 선택하여 관련 작업을 수행할 수 있습니다.
title: 선택 모드
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1f3ec8d816ecd6de0cb08ca685a69cc693f4f47c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035196"
---
# <a name="selection-mode-overview"></a>선택 모드 개요

선택 모드를 통해 사용자는 단일 항목 또는 여러 항목에 대한 작업을 수행할 수 있습니다. 이 모드는 Ctrl 키 또는 Shift 키를 누른 채 항목을 클릭하거나 갤러리 보기에서 항목의 대상을 롤오버하여 상황에 맞는 메뉴를 통해 호출할 수 있습니다. 선택 모드가 활성화되면 각 목록 항목 옆에 확인란이 표시되고 화면의 위쪽이나 아래쪽에 작업이 표시될 수 있습니다.

## <a name="different-selection-modes"></a>다양한 선택 모드
다음과 같은 세 가지 선택 모드가 있습니다.

- 단일 - 사용자가 한 번에 하나의 항목만 선택할 수 있습니다.
- 다중 - 사용자가 보조 키를 사용하지 않고 여러 항목을 선택할 수 있습니다.
- 확장 - 사용자가 보조 키를 사용하여 여러 항목을 선택할 수 있습니다. 예를 들어 Shift 키를 누른 상태로 선택할 수 있습니다.

## <a name="selection-mode-examples"></a>선택 모드 예제
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>단일 선택 모드를 사용하도록 설정된 ListView는 다음과 같습니다.
![단일 선택 모드를 사용하도록 설정된 목록 보기](images/listview-selection-single.png)

Art Venere 항목이 선택되었으며, 현재 Mitsue Tollner 항목을 가리키고 있습니다.

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>다중 선택 모드를 사용하도록 설정된 ListView는 다음과 같습니다.
![다중 선택 모드를 사용하도록 설정된 목록 보기](images/listview-selection-multiple.png)

3개의 항목이 선택되었으며, 현재 마우스로 Donette Foller 항목을 가리키고 있습니다.

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>단일 선택 모드를 사용하도록 설정된 GridView는 다음과 같습니다.
![단일 선택 모드를 사용하도록 설정된 그리드 보기](images/gridview-selection-single.png)

항목 7이 선택되었으며, 현재 마우스로 항목 3을 가리키고 있습니다.

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>다중 선택 모드를 사용하도록 설정된 GridView는 다음과 같습니다.
![다중 선택 모드를 사용하도록 설정된 그리드 보기](images/gridview-selection-multiple.png)

항목 2, 4, 5가 선택되었으며, 현재 마우스로 항목 7을 가리키고 있습니다.

## <a name="behavior-and-interaction"></a>동작 및 상호 작용
항목의 아무 곳이나 탭하면 선택됩니다. 명령 모음 작업을 탭하면 선택한 모든 항목에 영향을 줍니다. 항목을 선택하지 않으면 “모두 선택"을 제외한 명령 모음 작업이 비활성화되어야 합니다.

선택 모드에는 빠른 해제 모델이 없습니다. 즉, 선택 모드가 활성화된 프레임의 외부를 탭해도 모드가 취소되지 않습니다. 따라서 실수로 모드를 비활성화하는 것을 방지할 수 있습니다. 뒤로 단추를 클릭하면 다중 선택 모드가 해제됩니다.

작업이 선택된 경우 시각적 확인을 표시합니다. 특정 작업, 특히 삭제와 같은 파괴적인 작업에 대해서는 확인 대화 상자를 표시하는 것이 좋습니다.

선택 모드는 해당 모드가 활성화된 페이지로 제한되며, 해당 페이지 외부의 항목에는 영향을 줄 수 없습니다.

선택 모드의 진입점은 이 모드의 영향을 받는 콘텐츠와 나란히 배치되어야 합니다.

명령 모음 권장 사항에 대해서는 [명령 모음에 대한 지침](app-bars.md)을 참조하세요.

선택 모드 및 특정 컨트롤의 선택 이벤트 처리에 대한 자세한 내용은 해당 컨트롤에 대한 지침 페이지를 참조하세요.
