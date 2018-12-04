---
Description: Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: 컨트롤 패턴 및 인터페이스
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 87afe086ca28e27a39f5508a2bea5ea9fcb1c6a5
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8466947"
---
# <a name="control-patterns-and-interfaces"></a>컨트롤 패턴 및 인터페이스  



Microsoft UI 자동화 컨트롤 패턴, 클라이언트가 컨트롤 패턴에 액세스하는 데 사용하는 클래스 및 공급자가 컨트롤 패턴을 구현하는 데 사용하는 인터페이스를 나열합니다.

이 항목의 표에서는 Microsoft UI 자동화 컨트롤 패턴에 대해 설명합니다. 이 표에는 UI 자동화 클라이언트가 컨트롤 패턴에 액세스하는 데 사용하는 클래스와 UI 자동화 공급자가 컨트롤 패턴을 구현하는 데 사용하는 인터페이스도 나와 있습니다. **Control pattern** 열은 UI 자동화 클라이언트 관점의 패턴 이름을 [**Control Pattern Availability Property Identifiers**](https://msdn.microsoft.com/library/windows/desktop/Ee671199)에 나열되는 상수 값으로 표시합니다. 이러한 각 패턴은 UI 자동화 공급자 관점에서는 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 상수 이름입니다. **Class provider interface** 열은 공급자가 사용자 지정 XAML 컨트롤에 대해 이 패턴을 제공하기 위해 구현하는 인터페이스의 이름을 표시합니다.

컨트롤 패턴을 표시하고 인터페이스를 구현하는 사용자 지정 자동화 피어를 구현하는 방법에 대한 자세한 내용은 [사용자 지정 자동화 피어](custom-automation-peers.md)를 참조하세요.

컨트롤 패턴을 구현할 때는 구현에 사용되는 UI 프레임워크에 관계없이 고객이 컨트롤 패턴에서 원하는 기대치에 대해 설명하는 UI 자동화 공급자 설명서도 참조해야 합니다. 일반적인 UI 자동화 공급자 설명서에 나온 일부 정보는 피어를 구현하고 해당 패턴을 올바르게 지원하는 방법에 영향을 미칩니다. [UI 자동화 컨트롤 패턴 구현](https://msdn.microsoft.com/library/windows/desktop/Ee671292)을 참조하고 구현하려는 패턴에 대해 설명하는 페이지를 확인하세요.

| 컨트롤 패턴 | 클래스 공급자 인터페이스 | 설명 |
|-----------------|--------------------------|-------------|
| **Annotation** | [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) | 문서에 있는 주석의 속성을 표시하는 데 사용됩니다. |
| **도킹** | [**IDockProvider**](https://msdn.microsoft.com/library/windows/apps/BR242565) | 도킹 컨테이너에 도킹할 수 있는 컨트롤에 사용됩니다. 예를 들어 도구 모음이나 도구 팔레트입니다. |
| **끌기** | [**IDragProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750322) | 끌기 가능 컨트롤이나 끌기 가능 항목이 있는 컨트롤을 지원하는 데 사용됩니다. |
| **DropTarget** | [**IDropTargetProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750327) | 끌어서 놓기 작업의 대상이 될 수 있는 컨트롤을 지원하는 데 사용됩니다. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/BR242568) | 시각적으로 확장하여 더 많은 내용을 표시하고 축소하여 내용을 숨기는 컨트롤을 지원하는 데 사용됩니다. |
| **그리드** | [**IGridProvider**](https://msdn.microsoft.com/library/windows/apps/BR242578) | 크기 조정, 지정한 셀로 이동 등의 그리드 기능을 지원하는 컨트롤에 사용됩니다. 그리드 자체는 레이아웃을 제공하지만 컨트롤이 아니기 때문에 이 패턴을 구현하지 않습니다. |
| **GridItem** | [**IGridItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242572) | 그리드 내에 셀이 있는 컨트롤에 사용됩니다. |
| **Invoke** | [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582) | [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 등 호출할 수 있는 컨트롤에 사용됩니다. |
| **ItemContainer** | [**IItemContainerProvider**](https://msdn.microsoft.com/library/windows/apps/BR242583) | 가상화된 목록 등 응용 프로그램이 컨테이너에서 요소를 찾을 수 있게 합니다. |
| **MultipleView** | [**IMultipleViewProvider**](https://msdn.microsoft.com/library/windows/apps/BR242585) | 동일한 정보, 데이터 또는 자식 세트의 여러 표현을 전환할 수 있는 컨트롤에 사용됩니다. |
| **ObjectModel** | [**IObjectModelProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251815) | 포인터를 문서의 기본 개체 모델에 표시하는 데 사용됩니다. |
| **RangeValue** | [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) | 컨트롤에 적용할 수 있는 값 범위가 있는 컨트롤에 사용됩니다. 예를 들어 연도를 포함하는 회전자 컨트롤에는 1900에서 현재 연도까지 범위가 있을 수 있고 월을 제공하는 다른 회전자 컨트롤에는 1에서 12까지 범위가 있을 수 있습니다. |
| **Scroll** | [**IScrollProvider**](https://msdn.microsoft.com/library/windows/apps/BR242601) | 스크롤할 수 있는 컨트롤에 사용됩니다. 예를 들어 컨트롤의 보이는 영역에 표시될 수 있는 추가 정보가 있을 때 활성화되는 스크롤 막대가 있는 컨트롤입니다. |
| **ScrollItem** | [**IScrollItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242599) | 스크롤되는 목록에 개별 항목이 있는 컨트롤에 사용됩니다. 예를 들어 콤보 상자 컨트롤 등 스크롤 목록에 개별 항목이 있는 목록 컨트롤입니다. |
| **Selection** | [**ISelectionProvider**](https://msdn.microsoft.com/library/windows/apps/BR242616) | 선택 컨테이너 컨트롤에 사용됩니다. 예를 들어 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 및 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/BR209348)입니다. |
| **SelectionItem** | [**ISelectionItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242610) | 목록 상자, 콤보 상자 등 선택 컨테이너 컨트롤의 개별 항목에 사용됩니다. |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251821) | 스프레드시트 또는 기타 그리드 기반 문서의 내용을 표시하는 데 사용됩니다. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251817) | 스프레드시트 또는 기타 그리드 기반 문서에 있는 셀의 속성을 표시하는 데 사용됩니다. |
| **스타일** | [**IStylesProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251823) | 특정 스타일, 채우기 색, 채우기 패턴 또는 도형이 있는 UI 요소를 설명하는 데 사용됩니다. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://msdn.microsoft.com/library/windows/apps/Dn279198) | UI 자동화 클라이언트 앱이 마우스 또는 키보드 입력을 특정 UI 요소로 리디렉션하도록 합니다. |
| **Table** | [**ITableProvider**](https://msdn.microsoft.com/library/windows/apps/BR242623) | 그리드 및 헤더 정보가 있는 컨트롤에 사용됩니다. 예를 들어 표 형식 일정 컨트롤입니다. |
| **TableItem** | [**ITableItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242620) | 표의 항목에 사용됩니다. |
| **Text** | [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) | 텍스트 정보를 표시하는 편집 컨트롤 및 문서에 사용됩니다. [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) 및 [**ITextProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider2)을(를) 참조하세요. |
| **TextChild** | [**ITextChildProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextchildprovider) | **Text** 컨트롤 패턴을 지원하는 요소의 가장 가까운 상위 항목에 액세스하는 데 사용됩니다. |
| **TextEdit** | 사용 가능한 관리되는 클래스가 없음 | 예를 들어 자동 고침을 수행하거나 IME(입력기)를 통한 입력 작성을 지원하는 컨트롤과 같이, 텍스트를 수정하는 컨트롤에 대한 액세스를 제공합니다. |
| **TextRange** | [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) | [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider)을(를) 구현하는 텍스트 컨테이너의 연속 텍스트 범위에 대한 액세스를 제공합니다. [**ITextRangeProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider2)을(를) 참조하세요. |
| **Toggle** | [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653) | 상태를 전환할 수 있는 컨트롤에 사용됩니다. 예를 들어 선택할 수 있는 메뉴 항목 및 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/BR209316)입니다. |
| **Transform** | [**ITransformProvider**](https://msdn.microsoft.com/library/windows/apps/BR242656) | 크기 조정, 이동 및 회전할 수 있는 컨트롤에 사용됩니다. 대체로 Transform 컨트롤 패턴과 디자이너, 양식, 그래픽 편집기 및 그리기 응용 프로그램에서 사용됩니다. |
| **값** | [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) | 클라이언트가 값 범위를 지원하지 않는 컨트롤의 값을 가져오거나 설정할 수 있게 합니다. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242668) | 가상화되었으며 UI 자동화 요소로 액세스할 수 있도록 해야 하는 컨테이너 내부 항목을 표시합니다. |
| **Window** | [**IWindowProvider**](https://msdn.microsoft.com/library/windows/apps/BR242670) | MicrosoftWindows 운영 체제 개념인 창 관련 정보를 노출합니다. 창인 컨트롤의 예로 자식 창과 대화 상자가 있습니다. |

> [!NOTE]
> 기존 XAML 컨트롤에서 이러한 패턴이 모두 구현되지는 않았을 수 있습니다. 일부 패턴에는 일반적인 UI 자동화 프레임워크 패턴 정의에서 패리티를 지원하고 사용자 지정 구현을 통해 해당 패턴을 지원해야 하는 자동화 피어 시나리오를 지원할 용도로만 인터페이스가 있습니다.

> [!NOTE]
> Windows Phone 스토어 앱은 여기에 나열된 일부 UI 자동화 컨트롤 패턴을 지원하지 않습니다. **Annotation**, **Dock**, **Drag**, **DropTarget**, **ObjectModel**은 지원되지 않는 패턴 중 일부입니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [사용자 지정 자동화 피어](custom-automation-peers.md)
* [접근성](accessibility.md) 
