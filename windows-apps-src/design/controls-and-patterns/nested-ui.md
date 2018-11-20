---
author: muhsinking
Description: Use nested UI to enable multiple actions on a list item
title: 목록 항목의 중첩된 UI
label: Nested UI in list items
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 60a29717-56f2-4388-a9ff-0098e34d5896
pm-contact: chigy
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f9bb6daeb01e264cf9cdb0fa9ee9c66738fec972
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7286949"
---
# <a name="nested-ui-in-list-items"></a>목록 항목의 중첩된 UI

 

중첩된 UI는 중첩된 실행 가능한 컨트롤을 독립적인 포커스를 가질 수도 있는 컨테이너 내에 묶어 표시하는 UI(사용자 인터페이스)입니다.

중첩된 UI를 사용하여 중요 작업 실행을 가속화하는 추가 옵션을 사용자에게 제공할 수 있습니다. 그러나 표시하는 작업이 많을수록 UI가 복잡해집니다. 이 UI 패턴을 사용할 경우 더 많은 주의가 필요합니다. 이 문서에서는 특정 UI에 가장 적합한 작업 과정을 결정하는 지침을 제공합니다.

> **중요 API**: [ListView 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [GridView 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

이 문서에서는 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 및 [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) 항목에서 중첩된 UI를 생성하는 방법에 대해 설명합니다. 이 섹션에서는 다른 중첩 UI 사례에 대해 다루지 않지만 이러한 개념에 대해서는 설명합니다. 시작하기 전에 UI에 있는 ListView 또는 GridView 컨트롤 사용에 대한 일반 지침을 잘 알고 있어야 합니다. 이에 대한 내용은 [목록](lists.md) 및 [ListView 및 GridView](listview-and-gridview.md) 문서를 참조하세요.

이 문서에서 사용되는 용어인 *목록*, *목록 항목* 및 *중첩된 UI*의 정의는 다음과 같습니다.
- *목록*은 목록 보기 또는 그리드 보기에 포함된 항목의 컬렉션을 의미합니다.
- *목록 항목*은 사용자가 목록에서 작업할 수 있는 개별 항목을 의미합니다.
- *중첩된 UI*는 목록 항목 자체에 대해 수행할 수 있는 작업과는 별개로 사용자가 작업할 수 있는 목록 항목 내의 UI 요소를 의미합니다.

![중첩된 UI 부분](images/nested-ui-example-1.png)

> 참고&nbsp;&nbsp; ListView 및 GridView 모두 [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx) 클래스에서 파생되므로 동일한 기능을 갖지만 데이터를 다르게 표시합니다. 이 문서에서 목록에 대한 설명은 ListView 및 GridView 컨트롤에 모두 적용됩니다.

## <a name="primary-and-secondary-actions"></a>기본 및 보조 작업

목록을 사용하여 UI를 만들 때 사용자가 이러한 목록 항목을 사용하여 수행할 수 있는 작업을 고려합니다.  

- 사용자가 항목을 클릭하여 작업을 수행할 수 있나요?
    - 일반적으로 목록 항목을 클릭하면 작업이 시작되지만 꼭 그렇지는 않습니다.
- 사용자가 둘 이상의 작업을 수행할 수 있나요?
    - 예를 들어 목록에서 메일을 탭하면 해당 메일이 열립니다. 그러나 사용자가 메일을 먼저 열지 않고 삭제하려는 경우도 있습니다. 이 경우 사용자가 목록에서 이 작업에 직접 액세스하면 편리합니다.
- 사용자에게 작업을 어떻게 표시해야 하나요?
    - 모든 입력 유형을 고려하세요. 일부 중첩된 UI 형식은 한 가지 입력 방법에서는 제대로 작동하지만 다른 방법에서는 제대로 작동하지 않는 경우가 있습니다.  

*기본 작업*은 사용자가 목록 항목을 누를 때 발생하는 작업입니다.

*보조 작업*은 일반적으로 목록 항목과 관련된 바로 가기입니다. 이러한 바로 가기는 목록 관리 또는 목록 항목과 관련된 작업일 수 있습니다.

## <a name="options-for-secondary-actions"></a>보조 작업에 대한 옵션

목록 UI를 만들 때는 먼저 UWP를 지원하는 모든 입력 방법을 고려해야 합니다. 다른 종류의 입력에 대한 자세한 내용은 [입력 지침서](../input/index.md)를 참조하세요.

UWP에서 지원되는 모든 입력을 앱에서 지원하는 것이 확인된 후에는 앱의 보조 작업이 기본 목록에 바로 가기로 표시될 정도로 중요한지 결정해야 합니다. 표시하는 작업이 많을수록 UI가 복잡해집니다. 보조 작업을 기본 목록 UI에 반드시 표시해야 하는지 아니면 다른 곳에 배치할 수도 있는지 결정합니다.

항상 모든 입력에서 이러한 작업에 액세스할 수 있어야 하는 경우에는 기본 목록 UI에 추가 작업을 표시할 수 있습니다.

보조 작업을 반드시 기본 목록 UI에 배치할 필요가 없다고 결정한 경우에는 다른 여러 가지 방법으로 사용자에게 표시할 수 있습니다. 다음은 보조 작업을 배치할 수 있는 몇 가지 옵션입니다.

### <a name="put-secondary-actions-on-the-detail-page"></a>세부 정보 페이지에 보조 작업 배치

목록 항목을 누를 때 이동할 페이지에 보조 작업을 배치합니다. 마스터/세부 정보 패턴을 사용하는 경우 세부 정보 페이지에 보조 작업을 배치하는 것이 좋습니다.

자세한 내용은 [마스터/세부 정보 패턴](master-details.md)을 참조하세요.

### <a name="put-secondary-actions-in-a-context-menu"></a>상황에 맞는 메뉴에 보조 작업 배치

사용자가 마우스 오른쪽 단추를 클릭하거나 길게 누르기를 통해 액세스할 수 있는 상황에 맞는 메뉴에 보조 작업을 배치합니다. 이렇게 하면 사용자가 세부 정보 페이지를 로드하지 않고 메일을 삭제하는 등의 작업을 수행할 수 있습니다. 세부 정보 페이지에 이러한 옵션을 배치하면 상황에 맞는 메뉴가 기본 UI가 아닌 바로 가기가 되므로 유용합니다.

게임 패드 또는 리모콘을 사용하여 입력할 때 보조 작업을 표시하려면 상황에 맞는 메뉴를 사용하는 것이 좋습니다.

자세한 내용은 [상황에 맞는 메뉴와 플라이아웃](menus.md)을 참조하세요.

### <a name="put-secondary-actions-in-hover-ui-to-optimize-for-pointer-input"></a>가리키기 UI에 보조 작업을 배치하여 포인터 입력 최적화

마우스와 펜 등의 포인터 입력을 통해 앱을 자주 사용하고 이러한 입력에서만 보조 작업을 쉽게 사용할 수 있게 하려면 가리킬 때에만 보조 작업을 표시할 수 있습니다. 이 바로 가기는 포인터 입력이 사용될 때만 표시되므로 다른 입력 유형을 지원하려면 다른 옵션을 사용해야 합니다.

![가리킬 때 중첩된 UI 표시](images/nested-ui-hover.png)


자세한 내용은 [마우스 조작](../input/mouse-interactions.md)을 참조하세요.

## <a name="ui-placement-for-primary-and-secondary-actions"></a>기본 및 보조 작업에 대한 UI 배치

보조 작업을 기본 목록 UI에 표시하려는 경우 다음 지침이 권장됩니다.

기본 및 보조 작업으로 목록 항목을 만드는 경우 기본 작업을 왼쪽에, 보조 작업을 오른쪽에 배치합니다. 왼쪽에서 오른쪽으로 읽는 문화권에서는 사용자가 목록 항목의 왼쪽에 있는 작업을 기본 작업으로 연결합니다.

이 예제에서는 항목이 가로 방향으로 더 많이 나열되는(높이보다 너비가 더 긴) UI에 대해 설명합니다. 그러나 목록 항목이 정사각형에 가깝거나 너비보다 높이가 더 길 수도 있습니다. 일반적으로 이러한 경우에는 그리드를 사용합니다. 이러한 항목의 경우 목록이 세로 방향으로 스크롤되지 않으면 목록 항목의 오른쪽이 아닌 아래쪽에 보조 작업을 배치할 수 있습니다.

## <a name="consider-all-inputs"></a>모든 입력 고려

중첩된 UI를 사용할 경우 모든 입력 유형으로 사용자 환경을 평가해야 합니다. 앞서 언급했듯이 중첩된 UI는 몇 가지 입력 유형에서는 올바르게 작동합니다. 그러나 일부 유형에서는 올바르게 작동하지 않을 수 있습니다. 특히 키보드, 컨트롤러 및 원격 입력은 중첩된 UI 요소에 액세스하는 것이 어려울 수 있습니다. UWP가 모든 입력 유형에 대해 올바르게 작동하려면 아래 지침을 따라야 합니다.

## <a name="nested-ui-handling"></a>중첩된 UI 처리

목록 항목에 둘 이상의 작업이 중첩된 경우 이 지침을 참조하여 키보드, 게임 패드, 리모콘 또는 기타 포인터 방식이 아닌 입력을 사용하여 탐색을 처리하는 것이 좋습니다.

### <a name="nested-ui-where-list-items-perform-an-action"></a>목록 항목이 작업을 수행하는 중첩된 UI

중첩된 요소가 있는 목록 UI에서 호출, 선택(단일 또는 다중) 또는 끌어서 놓기 등의 작업을 지원하는 경우 이러한 화살표 지정 기술을 사용하여 중첩된 UI 요소를 탐색하는 것이 좋습니다.

![중첩된 UI 부분](images/nested-ui-navigation.png)

**게임 패드**

게임 패드에서 입력하는 경우 다음 사용자 환경을 제공합니다.

- **A**에서 오른쪽 방향 키를 누르면 포커스가 **B**로 이동합니다.
- **B**에서 오른쪽 방향 키를 누르면 포커스가 **C**로 이동합니다.
- **C**에서는 오른쪽 방향 키가 작동하지 않거나 포커스 가능 UI 요소가 목록 오른쪽에 있는 경우 포커스가 이 요소로 이동합니다.
- **C**에서 왼쪽 방향 키를 누르면 포커스가 **B**로 이동합니다.
- **B**에서 왼쪽 방향 키를 누르면 포커스가 **A**로 이동합니다.
- **A**에서는 왼쪽 방향 키가 작동하지 않거나 포커스 가능 UI 요소가 목록 오른쪽에 있는 경우 포커스가 이 요소로 이동합니다.
- **A**, **B** 또는 **C**에서 아래쪽 방향 키를 누르면 포커스가 **D**로 이동합니다.
- 목록 항목 왼쪽에 있는 UI 요소에서 오른쪽 방향 키를 누르면 포커스가 **A**로 이동합니다.
- 목록 항목 오른쪽에 있는 UI 요소에서 왼쪽 방향 키를 누르면 포커스가 **A**로 이동합니다.

**키보드**

키보드에서 입력하는 경우에는 다음과 같이 작동합니다.

- **A**에서 Tab 키를 누르면 포커스가 **B**로 이동합니다.
- **B**에서 Tab 키를 누르면 포커스가 **C**로 이동합니다.
- **C**에서 Tab 키를 누르면 포커스가 탭 순서상 다음 포커스 가능 UI 요소로 이동합니다.
- **C**에서 Shift+Tab을 누르면 포커스가 **B**로 이동합니다.
- **B**에서 Shift+Tab 또는 왼쪽 화살표 키를 누르면 포커스가 **A**로 이동합니다.
- **A**에서 Shift+Tab을 누르면 포커스가 탭 순서상 다음 포커스 가능 UI 요소로 이동합니다.
- **A**, **B** 또는 **C**에서 아래쪽 화살표 키를 누르면 포커스가 **D**로 이동합니다.
- 목록 항목 왼쪽에 있는 UI 요소에서 Tab 키를 누르면 포커스가 **A**로 이동합니다.
- 목록 항목 오른쪽에 있는 UI 요소에서 Shift+Tab을 누르면 포커스가 **C**로 이동합니다.

이 UI를 구현하려면 목록에서 [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx)를 **true**로 설정합니다. [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx)는 임의의 값으로 설정할 수 있습니다.

이를 구현하는 코드에 대해서는 이 문서의 [예제](#example) 섹션을 참조하세요.

### <a name="nested-ui-where-list-items-do-not-perform-an-action"></a>목록 항목이 작업을 수행하지 않는 중첩된 UI

목록 보기에서 가상화 및 최적화된 스크롤 동작을 제공하지만 목록 항목과 연결된 작업이 없는 경우가 있습니다. 이러한 UI는 일반적으로 요소를 그룹화하여 집합으로 스크롤하는 용도로만 목록 항목을 사용합니다.

이러한 종류의 UI는 사용자가 작업을 수행할 수 있는 것보다 더 많은 요소가 중첩되어 있으므로 이전 예제보다 훨씬 더 복잡할 수 있습니다.

![중첩된 UI 부분](images/nested-ui-grouping.png)


이 UI를 구현하려면 목록에서 다음 속성을 설정합니다.
- [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx)를 **None**으로 설정합니다.
- [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx)를 **false**로 설정합니다.
- [IsFocusEngagementEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isfocusengagementenabled.aspx)를 **true**로 설정합니다.

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="False" >
    <ListView.ItemContainerStyle>
         <Style TargetType="ListViewItem">
             <Setter Property="IsFocusEngagementEnabled" Value="True"/>
         </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

목록 항목에서 작업을 수행하지 않는 경우 이 지침을 사용하여 게임 패드 또는 키보드로 탐색을 처리하는 것이 좋습니다.

**게임 패드**

게임 패드에서 입력하는 경우 다음 사용자 환경을 제공합니다.

- 목록 항목에서 아래쪽 방향 키를 누르면 포커스가 다음 목록 항목으로 이동합니다.
- 목록 항목에서는 왼쪽/오른쪽 방향 키가 작동하지 않거나 포커스 가능 UI 요소가 목록 오른쪽에 있는 경우 포커스가 이 요소로 이동합니다.
- 목록 항목에서 'A' 단추를 누르면 중첩된 UI에서 위쪽/왼쪽 아래/오른쪽 순서로 포커스가 이동합니다.
- 중첩된 UI 내에서는 XY 포커스 탐색 모델을 따릅니다.  포커스는 현재 목록 항목 내에 있는 중첩된 UI로만 이동하며 사용자가 'B' 단추를 누르면 포커스가 다시 목록 항목으로 이동합니다.

**키보드**

키보드에서 입력하는 경우에는 다음과 같이 작동합니다.

- 목록 항목에서 아래쪽 화살표 키를 누르면 포커스가 다음 목록 항목으로 이동합니다.
- 목록 항목에서는 왼쪽/오른쪽 화살표 키를 눌러도 작동하지 않습니다.
- 목록 항목에서 Tab 키를 누르면 포커스가 중첩된 UI 항목 사이에서 다음 탭 정지로 이동합니다.
- 중첩된 UI 항목 중 하나에서 Tab 키를 누르면 탭 순서에 따라 중첩된 UI 항목 사이를 이동합니다.  중첩된 UI 항목 사이를 모두 이동한 후에는 탭 순서상 ListView 다음인 컨트롤로 포커스가 이동합니다.
- Shift+Tab을 누르면 탭 동작이 반대 방향으로 수행됩니다.

## <a name="example"></a>예제

이 예제에서는 [목록 항목이 작업을 수행하는 중첩된 UI](#nested-ui-where-list-items-perform-an-action)를 구현하는 방법을 보여 줍니다.

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="True"
          ChoosingItemContainer="listview1_ChoosingItemContainer"/>
```

```csharp
private void OnListViewItemKeyDown(object sender, KeyRoutedEventArgs e)
{
    // Code to handle going in/out of nested UI with gamepad and remote only.
    if (e.Handled == true)
    {
        return;
    }

    var focusedElementAsListViewItem = FocusManager.GetFocusedElement() as ListViewItem;
    if (focusedElementAsListViewItem != null)
    {
        // Focus is on the ListViewItem.
        // Go in with Right arrow.
        Control candidate = null;

        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                var rawPixelsPerViewPixel = DisplayInformation.GetForCurrentView().RawPixelsPerViewPixel;
                GeneralTransform generalTransform = focusedElementAsListViewItem.TransformToVisual(null);
                Point startPoint = generalTransform.TransformPoint(new Point(0, 0));
                Rect hintRect = new Rect(startPoint.X * rawPixelsPerViewPixel, startPoint.Y * rawPixelsPerViewPixel, 1, focusedElementAsListViewItem.ActualHeight * rawPixelsPerViewPixel);
                candidate = FocusManager.FindNextFocusableElement(FocusNavigationDirection.Right, hintRect) as Control;
                break;
        }

        if (candidate != null)
        {
            candidate.Focus(FocusState.Keyboard);
            e.Handled = true;
        }
    }
    else
    {
        // Focus is inside the ListViewItem.
        FocusNavigationDirection direction = FocusNavigationDirection.None;
        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadUp:
            case Windows.System.VirtualKey.GamepadLeftThumbstickUp:
                direction = FocusNavigationDirection.Up;
                break;
            case Windows.System.VirtualKey.GamepadDPadDown:
            case Windows.System.VirtualKey.GamepadLeftThumbstickDown:
                direction = FocusNavigationDirection.Down;
                break;
            case Windows.System.VirtualKey.GamepadDPadLeft:
            case Windows.System.VirtualKey.GamepadLeftThumbstickLeft:
                direction = FocusNavigationDirection.Left;
                break;
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                direction = FocusNavigationDirection.Right;
                break;
            default:
                break;
        }

        if (direction != FocusNavigationDirection.None)
        {
            Control candidate = FocusManager.FindNextFocusableElement(direction) as Control;
            if (candidate != null)
            {
                ListViewItem listViewItem = sender as ListViewItem;

                // If the next focusable candidate to the left is outside of ListViewItem,
                // put the focus on ListViewItem.
                if (direction == FocusNavigationDirection.Left &&
                    !listViewItem.IsAncestorOf(candidate))
                {
                    listViewItem.Focus(FocusState.Keyboard);
                }
                else
                {
                    candidate.Focus(FocusState.Keyboard);
                }
            }

            e.Handled = true;
        }
    }
}

private void listview1_ChoosingItemContainer(ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    if (args.ItemContainer == null)
    {
        args.ItemContainer = new ListViewItem();
        args.ItemContainer.KeyDown += OnListViewItemKeyDown;
    }
}
```

```csharp
// DependencyObjectExtensions.cs definition.
public static class DependencyObjectExtensions
{
    public static bool IsAncestorOf(this DependencyObject parent, DependencyObject child)
    {
        DependencyObject current = child;
        bool isAncestor = false;

        while (current != null && !isAncestor)
        {
            if (current == parent)
            {
                isAncestor = true;
            }

            current = VisualTreeHelper.GetParent(current);
        }

        return isAncestor;
    }
}
```
