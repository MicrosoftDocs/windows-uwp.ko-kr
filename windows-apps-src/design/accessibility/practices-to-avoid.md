---
author: Xansky
Description: Lists the practices to avoid if you want to create an accessible Universal Windows Platform (UWP) app.
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: 피해야 할 접근성 사례
label: Accessibility practices to avoid
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9505dfc666042c22e6f77ed02ffca7c5973d4fba
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6987020"
---
# <a name="accessibility-practices-to-avoid"></a>피해야 할 접근성 사례

UWP(유니버설 Windows 플랫폼) 앱에 접근성을 만들려는 경우 피해야 할 사례를 참조하세요. 

* 기본 Windows 컨트롤이나 Microsoft UI 자동화 지원을 이미 구현한 컨트롤을 사용할 수 있는 경우 **사용자 지정 UI 요소를 빌드하지 않도록 합니다.** 표준 Windows 컨트롤은 기본적으로 접근성이 있으므로 일반적으로 앱과 관련된 몇 가지 접근성 특성만 추가하면 됩니다. 반대로 실제 사용자 지정 컨트롤에 대한 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 지원 구현은 약간 더 많은 작업이 필요합니다([사용자 지정 자동화 피어](custom-automation-peers.md) 참조).
* **정적 텍스트나 다른 비대화형 요소를 탭 순서에 배치하지 마세요**(예: 대화형이 아닌 요소에 대해 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 속성 설정). 비대화형 요소가 탭 순서에 있으면 사용자의 키보드 탐색 효율성이 감소되므로 키보드 접근성 지침에 위배됩니다. 많은 보조 기술은 앱 인터페이스를 보조 기술 사용자에게 제공하는 방법에 대한 논리의 일부로 요소에 집중하는 기능과 탭 순서를 사용합니다. 탭 순서에서 텍스트 전용 요소는 탭 순서에서 대화형 요소(단추, 확인란, 텍스트 입력 필드, 콤보 상자, 목록 등)만 예상하는 사용자에게 혼동을 줄 수 있습니다.
* 프레젠테이션 순서가 자식 요소 선언 순서(실제로는 논리 순서임)와 다른 경우가 자주 있으므로 UI 요소(예: [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267) 요소)의 **절대 위치 지정은 사용하지 마세요**. 가능하면 화면 읽기 프로그램이 올바른 순서로 해당 요소를 읽을 수 있도록 문서 또는 논리 순서로 UI 요소를 정렬하는 것이 좋습니다. UI 요소의 표시 순서를 문서 또는 논리 순서와 다르게 하려면 명시적인 탭 인덱스 값([**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 참조)을 사용하여 올바른 읽기 순서를 정의합니다.
* **정보를 전달하는 유일한 수단으로 색을 사용하지 마세요.** 색맹인 사용자는 색 상태 표시기같이 색을 통해서만 전달되는 정보를 받을 수 없습니다. 다른 시각 신호(예제: 텍스트)를 포함시켜 정보에 대한 접근성을 유지합니다.
* 실제로 앱 기능에 필요하지 않는 한 **전체 앱 캔버스를 자동으로 새로 고치지 마세요.** 페이지 콘텐츠를 자동으로 새로 고쳐야 하면 페이지의 특정 영역만 업데이트합니다. 일반적으로 보조 기술에서는 적용된 변경 내용이 최소인 경우에도 새로 고친 앱 canvas가 완전히 새로운 구조라고 가정합니다. 따라서 보조 기술 사용자는 새로 고친 앱의 문서 보기 또는 설명을 다시 만들어 사용자에게 다시 제공해야 할 수 있습니다.
  
  사용자가 의도적으로 페이지 탐색을 시작한 경우 앱의 구조를 새로 고치는 데 적합합니다. 그러나 탐색을 시작한 UI 항목을 올바르게 식별하거나 해당 항목을 호출하여 컨텍스트가 변경되고 페이지가 다시 로드되었음을 나타내도록 이름을 지정해야 합니다.

  > [!NOTE]
  > 영역 내의 콘텐츠를 새로 고치는 경우 해당 요소의 [**AccessibilityProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516) 접근성 속성을 기본 설정이 아닌 **Polite** 또는 **Assertive** 중 하나로 설정하는 것이 좋습니다. 일부 보조 기술은 라이브 영역의 ARIA(Accessible Rich Internet Applications) 개념에 이 설정을 매핑하여 해당 콘텐츠 영역이 변경되었음을 사용자에게 알릴 수 있습니다.

* **초당 4번 이상 깜박이는 UI 요소를 사용하지 마세요.** 깜박이는 요소로 인해 일부 사용자가 발작을 일으킬 수 있습니다. 깜박이는 UI 요소를 아예 사용하지 않는 것이 가장 좋은 방법입니다.
* **자동으로 사용자 컨텍스트를 변경하거나 기능을 활성화하지 마세요.** 컨텍스트 또는 활성화 변경은 사용자가 포커스가 있는 UI 요소에 직접 작업을 수행할 때만 발생해야 합니다. 사용자 컨텍스트 변경에는 포커스 변경, 새 콘텐츠 표시, 다른 페이지로의 이동이 포함됩니다. 사용자 개입 없이 컨텍스트를 변경할 경우 장애를 가진 사용자는 혼란을 겪을 수 있습니다. 단, 하위 메뉴 표시, 양식 유효성 검사, 다른 컨트롤에 도움말 텍스트 표시, 비동기 이벤트에 대한 응답 컨텍스트 변경 등은 이 요구 사항의 예외입니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [스토어의 접근성](accessibility-in-the-store.md)
* [접근성 검사 목록](accessibility-checklist.md)
