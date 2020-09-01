---
Description: 액세스 가능한 Windows 앱을 만들려는 경우 방지할 수 있는 방법을 나열 합니다.
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: 피해야 할 접근성 사례
label: Accessibility practices to avoid
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2858abc469e0a22d97d8b833f2f347f9bc75d9ce
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173977"
---
# <a name="accessibility-practices-to-avoid"></a>피해야 할 접근성 사례

액세스할 수 있는 Windows 앱을 만들려는 경우 다음을 방지 하는 방법 목록을 참조 하세요. 

* Microsoft UI 자동화 지원이 이미 구현 된 **기본 Windows 컨트롤 또는 컨트롤을 사용할 수 있는 경우 사용자 지정 UI 요소를 빌드하지 마세요** . 표준 Windows 컨트롤은 기본적으로 액세스할 수 있으며, 일반적으로 응용 프로그램에 관련 된 몇 가지 내게 필요한 옵션 특성만 추가 해야 합니다. 이와 대조적으로 실제 사용자 지정 컨트롤에 대 한 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 지원 구현은 약간 더 관련이 있습니다 ( [사용자 지정 자동화 피어](custom-automation-peers.md)참조).
* **정적 텍스트 또는 다른 비 대화형 요소를 탭 순서에 배치 하지** 않습니다. 예를 들어, 대화형이 아닌 요소에 대해 [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 속성을 설정 합니다. 비 대화형 요소는 탭 순서에 있는 경우 사용자에 대 한 키보드 탐색의 효율성을 줄이기 때문에 키보드 접근성 지침에 대 한 것입니다. 많은 보조 기술에서 탭 순서를 사용 하 고 보조 기술 사용자에 게 앱의 인터페이스를 제공 하는 방법에 대 한 논리의 일부로 요소에 초점을 맞출 수 있는 기능을 사용 합니다. 탭 순서의 텍스트 전용 요소는 탭 순서 (단추, 확인란, 텍스트 입력 필드, 콤보 상자, 목록 등)에서 대화형 요소만 필요로 하는 사용자를 혼동할 수 있습니다.
* 프레젠테이션 순서가 자식 요소 선언 순서 (사실상 논리적 순서)와 다른 경우가 많기 때문에 [**캔버스**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 요소와 같은 **UI 요소의 절대 위치 지정을 사용 하지 마십시오** . 가능 하면 UI 요소를 문서 또는 논리적 순서로 정렬 하 여 화면 판독기가 올바른 순서로 해당 요소를 읽을 수 있도록 합니다. UI 요소의 표시 순서가 문서 또는 논리적 순서와 다를 수 있는 경우 명시적 탭 인덱스 값 ( [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex)설정)을 사용 하 여 올바른 읽기 순서를 정의 합니다.
* **정보를 전달 하는 유일한 방법으로 색을 사용 하지 마세요.** 색이 블라인드 인 사용자는 색 상태 표시기와 같이 색을 통해서만 전달 되는 정보를 받을 수 없습니다. 정보에 액세스할 수 있도록 다른 시각적 신호 (특히 텍스트)를 포함 합니다.
* 응용 프로그램 기능에 꼭 필요한 경우가 아니면 **전체 앱 캔버스를 자동으로 새로 고치지 마세요** . 페이지 콘텐츠를 자동으로 새로 고쳐야 하는 경우 페이지의 특정 영역만 업데이트 합니다. 보조 기술은 일반적으로 효과적인 변경이 최소화 된 경우에도 새로 고쳐진 앱 캔버스가 완전히 새로운 구조 라고 가정 합니다. 보조 기술 사용자에 대 한이 비용은 새로 고쳐진 앱에 대 한 문서 보기 또는 설명을 다시 만들고 사용자에 게 다시 제공 해야 합니다.
  
  사용자가 시작 하는 고의적인 페이지 탐색은 응용 프로그램 구조를 새로 고치기 위한 합법적인 사례입니다. 그러나 탐색을 시작 하는 UI 항목이 올바르게 식별 되었는지 확인 하거나 이름을 지정 하 여 컨텍스트를 변경 하 고 페이지를 다시 로드 합니다.

  > [!NOTE]
  > 영역 내의 콘텐츠를 새로 고치는 경우 해당 요소의 [**AccessibilityProperties.LiveSetting**](/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty) 접근성 속성을 기본 설정이 아닌 **Polite** 또는 **Assertive** 중 하나로 설정하는 것이 좋습니다. 일부 보조 기술은이 설정을 라이브 지역의 액세스 가능한 리치 (리치 인터넷 응용 프로그램) 개념에 매핑할 수 있으므로 사용자에 게 콘텐츠 영역이 변경 되었음을 알릴 수 있습니다.

* **초당 세 번 이상 깜박이는 UI 요소를 사용 하지 마십시오.** 깜박이는 요소로 인해 일부 사용자가 seizures 수 있습니다. 플래시 하는 UI 요소를 사용 하지 않는 것이 좋습니다.
* **사용자 컨텍스트를 변경 하거나 기능을 자동으로 활성화 하지 않습니다.** 컨텍스트 또는 활성화 변경은 사용자가 포커스가 있는 UI 요소에 대해 직접 작업을 수행 하는 경우에만 발생 합니다. 사용자 컨텍스트의 변경 내용에는 포커스 변경, 새 콘텐츠 표시, 다른 페이지로 이동 등이 있습니다. 사용자를 포함 하지 않고 컨텍스트를 변경 하는 것은 장애가 있는 사용자를 위해 세분화 될 수 있습니다. 이 요구 사항에 대 한 예외로는 하위 메뉴 표시, 폼 유효성 검사, 다른 컨트롤에 도움말 텍스트 표시, 비동기 이벤트에 대 한 응답으로 컨텍스트 변경 등이 있습니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [스토어의 접근성](accessibility-in-the-store.md)
* [접근성 검사 목록](accessibility-checklist.md)