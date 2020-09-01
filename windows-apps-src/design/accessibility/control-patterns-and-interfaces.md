---
Description: Microsoft UI 자동화 컨트롤 패턴, 클라이언트에서 액세스 하는 데 사용 하는 클래스 및 인터페이스 공급자를 사용 하 여 구현 합니다.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: 컨트롤 패턴 및 인터페이스
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: adbe1556f48e2f9b362faa303be52586714c73d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157027"
---
# <a name="control-patterns-and-interfaces"></a>컨트롤 패턴 및 인터페이스  



Microsoft UI 자동화 컨트롤 패턴, 클라이언트에서 액세스 하는 데 사용 하는 클래스 및 인터페이스 공급자를 사용 하 여 구현 합니다.

이 항목의 표에서는 Microsoft UI 자동화 컨트롤 패턴에 대해 설명 합니다. 이 표에는 ui 자동화 클라이언트가 컨트롤 패턴 및이를 구현 하는 데 사용 하는 인터페이스에 액세스 하는 데 사용 하는 클래스도 나열 되어 있습니다. **컨트롤 패턴** 열은 UI 자동화 클라이언트 관점의 패턴 이름을 [**컨트롤 패턴 가용성 속성 식별자**](/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids)에 나열 된 상수 값으로 표시 합니다. UI 자동화 공급자 관점에서 이러한 각 패턴은 [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 상수 이름입니다. **클래스 공급자 인터페이스** 열에는 공급자가 사용자 지정 XAML 컨트롤에 대해이 패턴을 제공 하기 위해 구현 하는 인터페이스의 이름이 표시 됩니다.

컨트롤 패턴을 노출 하 고 인터페이스를 구현 하는 사용자 지정 자동화 피어를 구현 하는 방법에 대 한 자세한 내용은 [사용자 지정 자동화 피어](custom-automation-peers.md)를 참조 하세요.

컨트롤 패턴을 구현 하는 경우 ui 프레임 워크를 구현 하는 데 사용 되는 UI 프레임 워크에 관계 없이 클라이언트에서 컨트롤 패턴을 사용할 수 있는 몇 가지 기대를 설명 하는 UI 자동화 공급자 설명서도 참조 해야 합니다. 일반 UI 자동화 공급자 설명서에 나열 된 정보 중 일부는 동료를 구현 하 고 해당 패턴을 올바르게 지 원하는 방법에 영향을 줍니다. [UI 자동화 컨트롤 패턴 구현](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)을 참조 하 고 구현 하려는 패턴을 설명 하는 페이지를 봅니다.

| 컨트롤 패턴 | 클래스 공급자 인터페이스 | Description |
|-----------------|--------------------------|-------------|
| **주석** | [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | 문서에 있는 주석의 속성을 노출 하는 데 사용 됩니다. |
| **도킹** | [**IDockProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | 도킹 컨테이너에서 도킹될 수 있는 컨트롤에 사용됩니다. 예를 들면, 도구 모음 또는 도구 팔레트입니다. |
| **끌기** | [**IDragProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | 끌기 가능 컨트롤을 지 원하는 데 사용 되거나 끌기 항목이 있는 컨트롤을 지 원하는 데 사용 됩니다. |
| **DropTarget** | [**IDropTargetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | 끌어서 놓기 작업의 대상이 될 수 있는 컨트롤을 지 원하는 데 사용 됩니다. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | 시각적으로 확장 되어 더 많은 콘텐츠를 표시 하 고 축소 하 여 콘텐츠를 숨기는 컨트롤을 지 원하는 데 사용 됩니다. |
| **눈금** | [**IGridProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | 지정된 셀로 이동 및 크기 조정과 같은 표 기능을 지원하는 컨트롤에 사용됩니다. 표 자체는 레이아웃을 제공 하지만 컨트롤이 아니기 때문에이 패턴을 구현 하지 않습니다. |
| **GridItem** | [**IGridItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | 표 내에서 셀이 있는 컨트롤에 사용됩니다. |
| **호출** | [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button)와 같이 호출할 수 있는 컨트롤에 사용 됩니다. |
| **ItemContainer** | [**IItemContainerProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | 응용 프로그램에서 가상화 된 목록과 같은 컨테이너의 요소를 찾을 수 있습니다. |
| **MultipleView** | [**IMultipleViewProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | 동일한 정보, 데이터 또는 자식 항목 집합의 여러 표현 간을 전환할 수 있는 컨트롤에 사용됩니다. |
| **ObjectModel** | [**IObjectModelProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | 문서의 기본 개체 모델에 대 한 포인터를 노출 하는 데 사용 됩니다. |
| **RangeValue** | [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | 컨트롤에 적용할 수 있는 값의 범위가 있는 컨트롤에 사용됩니다. 예를 들어 연도를 포함 하는 회전자 컨트롤의 범위는 현재 연도에 1900이 고 월을 나타내는 다른 회전자 컨트롤의 범위는 1 ~ 12입니다. |
| **스크롤** | [**IScrollProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | 스크롤할 수 있는 컨트롤에 사용됩니다. 예를 들면, 컨트롤의 볼 수 있는 영역에 표시될 수 있는 것보다 많은 정보가 있는 경우 활성 상태의 스크롤 막대가 있는 컨트롤입니다. |
| **ScrollItem** | [**IScrollItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | 스크롤하는 목록의 개별 항목이 포함된 컨트롤에 사용됩니다. 예를 들면, 스크롤 목록의 개별 항목을 가진 목록 컨트롤(예: 콤보 상자 컨트롤)입니다. |
| **선택 항목** | [**ISelectionProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | 선택 컨테이너 컨트롤에 사용됩니다. 예를 들어 [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 와 [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)가 있습니다. |
| **SelectionItem** | [**ISelectionItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | 목록 상자 및 콤보 상자와 같은 선택 컨테이너 컨트롤의 개별 항목에 사용됩니다. |
| **스프레드시트** | [**ISpreadsheetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | 스프레드시트나 다른 그리드 기반 문서의 내용을 표시 하는 데 사용 됩니다. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | 스프레드시트 또는 다른 그리드 기반 문서에서 셀의 속성을 표시 하는 데 사용 됩니다. |
| **스타일** | [**IStylesProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | 특정 스타일, 채우기 색, 채우기 패턴 또는 셰이프를 포함 하는 UI 요소를 설명 하는 데 사용 됩니다. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | UI 자동화 클라이언트 앱에서 마우스나 키보드 입력을 특정 UI 요소로 전달할 수 있도록 합니다. |
| **테이블** | [**ITableProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | 헤더 정보 및 표가 있는 컨트롤에 사용됩니다. 예를 들어 테이블 형식 달력 컨트롤이 있습니다. |
| **TableItem** | [**ITableItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | 테이블의 항목에 사용됩니다. |
| **Text** | [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | 편집 컨트롤 및 텍스트 정보를 노출하는 문서에 사용됩니다. [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) 및 [**ITextProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider2)도 참조 하세요. |
| **TextChild** | [**ITextChildProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | **Text** 컨트롤 패턴을 지 원하는 요소의 가장 가까운 상위 항목에 액세스 하는 데 사용 됩니다. |
| **TextEdit** | 사용할 수 있는 관리 되는 클래스가 없습니다. | 텍스트를 수정 하는 컨트롤에 대 한 액세스를 제공 합니다. 예를 들어, 자동 수정 기능을 수행 하거나 IME (입력기)를 통해 입력 컴퍼지션을 사용 하도록 설정할 수 있습니다. |
| **TextRange** | [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | [**Itextprovider**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider)를 구현 하는 텍스트 컨테이너의 연속 텍스트 범위에 대 한 액세스를 제공 합니다. [**ITextRangeProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2)도 참조 하세요. |
| **Toggle** | [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | 상태를 전환할 수 있는 컨트롤에 사용됩니다. 예를 들어 [**확인란**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 및 메뉴 항목을 선택할 수 있습니다. |
| **변환** | [**ITransformProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | 크기 조정, 이동 및 회전할 수 있는 컨트롤에 사용됩니다. Transform 컨트롤 패턴은 디자이너, 폼, 그래픽 편집기 및 그리기 애플리케이션에서 일반적으로 사용됩니다. |
| **값** | [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | 클라이언트가 값 범위를 지원하지 않는 컨트롤에 값을 설정하거나 가져올 수 있습니다. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | 가상화 되 고 UI 자동화 요소로 완전히 액세스할 수 있도록 해야 하는 컨테이너 내의 항목을 노출 합니다. |
| **창** | [**IWindowProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | Microsoft Windows 운영 체제에 대 한 기본적인 개념인 windows 관련 정보를 공개 합니다. Windows 컨트롤의 예로는 자식 창 및 대화 상자가 있습니다. |

> [!NOTE]
> 기존 XAML 컨트롤에서 이러한 모든 패턴의 구현을 반드시 찾아야 하는 것은 아닙니다. 일부 패턴에는 일반적인 UI 자동화 프레임워크 패턴 정의에서 패리티를 지원하고 사용자 지정 구현을 통해 해당 패턴을 지원해야 하는 자동화 피어 시나리오를 지원할 용도로만 인터페이스가 있습니다.

> [!NOTE]
> Windows Phone 스토어 앱은 여기에 나열 된 모든 UI 자동화 컨트롤 패턴을 지원 하지 않습니다. **Annotation**, **Dock**, **Drag**, **droptarget**, **objectmodel** 은 지원 되지 않는 패턴 중 일부입니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [사용자 지정 자동화 피어](custom-automation-peers.md)
* [접근성](accessibility.md)