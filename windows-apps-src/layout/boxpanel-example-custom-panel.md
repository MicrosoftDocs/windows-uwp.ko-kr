---
author: Jwmsft
Description: "ArrangeOverride 및 MeasureOverride methods 메서드를 구현하고 Children 속성을 사용하는 사용자 지정 Panel 클래스에 대한 코드를 작성하는 방법을 알아봅니다."
MS-HAID: dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "BoxPanel, 예제 사용자 지정 패널"
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 4427219987f0524858233cf382cd13121cf77b07

---

# BoxPanel, 예제 사용자 지정 패널

**중요 API**

-   [**패널**](https://msdn.microsoft.com/library/windows/apps/br227511)
-   [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
-   [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 및 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 메서드를 구현하고 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 속성을 사용하는 사용자 지정 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 클래스에 대한 코드를 작성하는 방법을 알아봅니다. 예제 코드에서는 사용자 지정 패널 구현을 보여 주지만, 각 레이아웃 시나리오에 맞게 패널을 사용자 지정하는 방법에 영향을 주는 레이아웃 개념을 설명하는 데 많은 시간을 할애하지 않습니다. 이러한 레이아웃 개념과 특정 레이아웃 시나리오에 개념을 적용하는 방법에 대한 자세한 내용은 [XAML 사용자 지정 패널 개요](custom-panels-overview.md)를 참조하세요.

*패널*은 XAML 레이아웃 시스템이 실행되고 앱 UI가 렌더링될 때 패널에 포함된 자식 요소에 대한 레이아웃 동작을 제공하는 개체입니다. [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 클래스에서 사용자 지정 클래스를 파생시켜 XAML 레이아웃에 대한 사용자 지정 패널을 정의할 수 있습니다. [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 및 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 메서드를 재정의하고 자식 요소를 측정 및 정렬하는 논리를 제공하여 패널에 대한 동작을 제공합니다. 이 예제는 **Panel**에서 파생됩니다. **Panel**, **ArrangeOverride** 및 **MeasureOverride** 메서드에서 시작하는 경우 시작 동작이 없습니다. 코드에서 자식 요소가 XAML 레이아웃 시스템에 알려지고 UI에 렌더링되는 게이트웨이를 제공합니다. 따라서 코드에서 모든 자식 요소를 고려하고 레이아웃 시스템에 필요한 패턴을 따르는 것이 중요합니다.

## 레이아웃 시나리오

사용자 지정 패널을 정의하는 경우 레이아웃 시나리오를 정의합니다.

레이아웃 시나리오는 다음과 같이 표현됩니다.

-   패널에 자식 요소가 있는 경우 패널이 수행하는 작업
-   패널에 자체 공간에 대한 제약 조건이 있는 경우
-   패널 논리가 최종적으로 자식의 렌더링된 UI 레이아웃을 생성하는 모든 측정, 배치, 위치 및 크기 조정을 결정하는 방식

이 점을 감안하여, 여기에 표시된 `BoxPanel`은 특정 시나리오에서 사용됩니다. 이 예제에서는 코드에 중점을 두기 위해 시나리오를 자세히 설명하지 않고 필요한 단계 및 코딩 패턴에 집중합니다. 먼저 시나리오에 대해 자세히 알아보려면 ["`BoxPanel`에 대한 시나리오"](#scenario)로 건너뛴 다음 코드로 돌아오세요.

## **Panel**에서 파생시켜 시작

[**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)에서 사용자 지정 클래스를 파생시켜 시작합니다. 가장 쉬운 방법은 Microsoft Visual Studio의 **솔루션 탐색기**에서 프로젝트에 대한 **추가** | **새 항목** | **클래스** 상황에 맞는 메뉴 옵션을 사용하여 이 클래스에 대한 별도 코드 파일을 정의하는 것입니다. 클래스(및 파일) 이름을 `BoxPanel`로 지정합니다.

클래스 템플릿 파일은 구체적으로 UWP(유니버설 Windows 플랫폼) 앱용으로 지정되지 않기 때문에 많은 **using** 문으로 시작하지 않습니다. 따라서 **using** 문을 먼저 추가합니다. 필요하지 않은 몇 가지 **using** 문과 함께 템플릿 파일이 시작될 수도 있으며 이 파일은 삭제할 수 있습니다. 다음은 일반적인 사용자 지정 패널 코드에 필요한 형식을 확인할 수 있는 **using** 문의 제안 목록입니다.

```CSharp
using System;
using System.Collections.Generic; // if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; // Point, Size, and Rect
using Windows.UI.Xaml; // DependencyObject, UIElement, and FrameworkElement
using Windows.UI.Xaml.Controls; // Panel
using Windows.UI.Xaml.Media; // if you need Brushes or other utilities
```

이제 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)을 확인할 수 있으므로 `BoxPanel`의 기본 클래스로 만듭니다. 또한 `BoxPanel`을 공용으로 설정합니다.

```CSharp
public class BoxPanel : Panel
{
}
```

클래스 수준에서 여러 논리 함수가 공유하지만 공용 API로 노출할 필요가 없는 **int** 및 **double** 값을 몇 개 정의합니다. 예제에서는 `maxrc`, `rowcount`, `colcount`, `cellwidth`, `cellheight`, `maxcellheight`, `aspectratio`로 이름이 지정됩니다.

이 작업을 수행한 후 전체 코드 파일은 다음과 같습니다. 이제 이유를 알고 있으므로 **using**에 대한 주석을 제거합니다.

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

이제부터 멤버 정의(멤버 재정의 또는 종속성 속성 등의 지원 항목)를 한 번에 하나씩 보여드리겠습니다. 위의 골격에 이러한 항목을 순서에 관계없이 추가할 수 있으며, 최종 코드를 표시할 때까지 코드 조각에 클래스 범위의 정의나 **using** 문을 다시 표시하지 않습니다.

## **MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 &amp;&amp; Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 &amp;&amp; Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that&#39;s our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

필요한 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 구현의 패턴은 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514)에 있는 각 요소의 반복입니다. 각 요소에서 항상 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 메서드를 호출합니다. **Measure**에는 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 형식의 매개 변수가 있습니다. 여기서는 패널에서 특정 자식 요소에 사용할 수 있도록 할 크기를 전달합니다. 따라서 루프를 수행하고 **Measure** 호출을 시작하기 전에 각 셀이 사용할 수 있는 공간 크기를 알아야 합니다. **MeasureOverride** 메서드 자체의 *availableSize* 값을 사용합니다. 호출되는 이 **MeasureOverride**의 트리거인 **Measure**를 호출할 때 패널의 부모가 사용한 크기입니다. 따라서 일반적인 논리는 각 자식 요소가 패널의 전체 *availableSize* 공간을 나누는 체계를 작성하는 것입니다. 그런 다음 나눈 각 크기를 각 자식 요소의 **Measure**에 전달합니다.

`BoxPanel`에서 크기를 나누는 방법은 비교적 간단합니다. 공간을 항목 수로 제어되는 상자 수로 나눕니다. 행 및 열 개수와 사용 가능한 크기에 따라 상자 크기가 지정됩니다. 때때로 정사각형의 행 또는 열 하나는 필요하지 않으므로 삭제되며, 행 : 열 비율 측면에서 패널이 정사각형이 아니라 직사각형이 됩니다. 이 논리에 도달한 방식에 대한 자세한 내용을 보려면 ["BoxPanel에 대한 시나리오"](#scenario)로 건너뛰세요.

그러면 측정 단계에서는 어떤 작업을 수행할까요? [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)가 호출된 각 요소에서 읽기 전용 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 속성의 값을 설정합니다. **DesiredSize**는 정렬 시 및 최종 렌더링에서 가능한 크기나 필수 크기를 전달하기 때문에 정렬 단계에 도달한 후에는 **DesiredSize** 값을 사용하는 것이 중요할 수 있습니다. 고유한 논리에 **DesiredSize**를 사용하지 않는 경우에도 시스템에 필요합니다.

*availableSize*의 높이 구성 요소가 무한대인 경우 이 패널을 사용할 수 있습니다. 이 경우 나눌 알려진 패널 높이가 없습니다. 따라서 측정 단계의 논리에서 각 자식에게 유한 높이가 없다고 알립니다. [**Size.Height**](https://msdn.microsoft.com/library/windows/apps/hh763910)가 무한 크기인 자식에 대한 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 호출에 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)를 전달하면 됩니다. 이는 타당한 동작입니다. **Measure**가 호출된 경우의 논리는 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)가 **Measure**에 전달된 값 또는 명시적으로 설정된 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 및 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 등의 인수를 통한 해당 요소의 실제 크기 중 최소값으로 설정되는 것입니다.

**참고**&nbsp;&nbsp;[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)의 내부 논리에도 이 동작이 있습니다. **StackPanel**은 자식의 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)에 무한대 차원 값을 전달하여 방향 차원에 자식에 대한 제약 조건이 없음을 나타냅니다. 일반적으로 **StackPanel**은 해당 차원에서 증가하는 스택에 모든 자식을 수용하기 위해 동적으로 크기가 조정됩니다.

그러나 패널 자체는 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)에서 무한대 값인 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)를 반환할 수 없으며, 레이아웃 중 예외가 발생합니다. 따라서 논리의 일부는 자식이 요청하는 최대 높이를 확인하고, 패널 자체의 크기 제약 조건에서 가져온 것이 아닐 경우 해당 높이를 셀 높이로 사용하는 것입니다. 다음은 이전 코드에서 참조된 도우미 함수 `LimitUnboundedSize`입니다. 이 함수는 최대 셀 높이를 받아서 반환할 유한 높이를 패널에 제공하는 데 사용할 뿐 아니라 정렬 단계가 시작되기 전에 `cellheight`가 유한 숫자인지 확인합니다.

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## **ArrangeOverride**

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

필요한 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 구현의 패턴은 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514)에 있는 각 요소의 반복입니다. 각 요소에서 항상 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 메서드를 호출합니다.

일반적으로 계산이 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)만큼 많지 않습니다. 자식의 크기는 패널 자체의 **MeasureOverride** 논리 또는 측정 단계 중 설정된 각 자식의 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 값에서 이미 알려져 있습니다. 하지만 각 자식이 표시될 패널 내의 위치를 결정해야 합니다. 일반 패널에서 각 자식은 서로 다른 위치에 렌더링되어야 합니다. 겹치는 요소를 만드는 패널은 일반적인 시나리오에 적합하지 않습니다. 의도한 시나리오인 경우 고의로 겹치는 패널을 만들 수는 있습니다.

이 패널은 행과 열의 개념에 따라 정렬됩니다. 행과 열 수는 이미 계산되었습니다(측정에 필요함). 이제 행과 열의 모양 및 각 셀의 알려진 크기가 이 패널에 포함된 각 요소의 렌더링 위치(`anchorPoint`)를 정의하는 논리에 사용됩니다. 해당 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)와 측정값에서 이미 알려진 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)는 [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)를 구성하는 두 개의 구성 요소로 사용됩니다. **Rect**는 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914)의 입력 형식입니다.

패널에서 콘텐츠를 잘라야 하는 경우가 있습니다. 이때 잘린 크기는 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)에 있는 크기인데, [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 논리에서 이 크기를 **Measure**에 전달된 값 또는 다른 실제 크기 인수 중 최소값으로 설정하기 때문입니다. 따라서 일반적으로 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 중에 클리핑을 확인할 필요가 없으며, 각 **Arrange** 호출에 **DesiredSize**를 전달하여 클리핑이 발생합니다.

다른 수단을 통해 렌더링 위치를 정의하는 데 필요한 모든 정보를 알고 있는 경우 루프를 수행하는 동안 개수가 항상 필요한 것은 아닙니다. 예를 들어 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 레이아웃 논리에서는 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 컬렉션의 위치가 중요하지 않습니다. **Canvas**에서 각 요소의 위치를 지정하는 데 필요한 모든 정보는 배열 논리의 일부인 자식의 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 및 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) 값을 읽어서 인식됩니다. `BoxPanel` 논리에서는 새 행을 시작하고 *y* 값을 오프셋할 경우를 알 수 있도록 *colcount*에 비교할 개수가 필요합니다.

일반적으로 입력 *finalSize*와 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 구현에서 반환하는 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)는 같습니다. 이유에 대한 자세한 내용은 [XAML 사용자 지정 패널 개요](custom-panels-overview.md)의 "**ArrangeOverride**" 섹션을 참조하세요.

## 구체화: 행 및 열 개수 제어

이 패널을 현재 상태대로 컴파일하고 사용할 수도 있습니다. 그러나 구체화를 하나 더 추가하겠습니다. 이전 코드에서 논리는 가로 세로 비율의 가장 긴 쪽에 추가 행 또는 열을 배치하는 것입니다. 그러나 셀 모양에 대한 제어를 강화하기 위해 패널 자체의 가로 세로 비율이 "세로"인 경우에도 3x4 대신 4x3 셀 집합을 선택하는 것이 좋습니다. 따라서 패널 소비자가 해당 동작을 제어하기 위해 설정할 수 있는 선택적 종속성 속성을 추가하겠습니다. 다음은 기본적인 종속성 속성 정의입니다.

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

또한 다음은 `UseOppositeRCRatio` 사용 시 측정 논리에 미치는 영향입니다. 실제로는 `rowcount` 및 `colcount`가 `maxrc`에서 파생되는 방법과 실제 가로 세로 비율만 변경하며, 이 때문에 각 셀에 해당 크기 차이가 있습니다. `UseOppositeRCRatio`가 **true**인 경우 행과 열 개수에 사용하기 전에 실제 가로 세로 비율 값을 반전합니다.

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## BoxPanel에 대한 시나리오

`BoxPanel`에 대한 특정 시나리오는 공간을 나누는 방법의 주요 결정자 중 하나가 자식 항목 수를 알고 패널에 사용 가능한 알려진 공간을 나누는 것인 패널입니다. 패널은 본질적으로 직사각형 모양입니다. 많은 패널은 직사각형 공간을 추가 직사각형으로 나누는 방식으로 작동하며, [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)도 해당 셀에 대해 이 작업을 수행합니다. **Grid**의 경우 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) 및 [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) 값으로 셀 크기를 설정하며, [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) 및 [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/hh759774) 연결된 속성을 사용하여 요소가 들어가는 정확한 셀을 선언합니다. 일반적으로 **Grid**에서 적절한 레이아웃을 가져오려면 충분한 셀이 있고 각 자식 요소가 해당 셀에 맞게 연결된 속성을 설정하도록 자식 요소 수를 미리 알아야 합니다.

그러나 자식 수가 동적인 경우는 어떻게 해야 할까요? 이런 경우도 분명히 있습니다. 앱 코드에서 UI를 업데이트할 만큼 중요한 동적 런타임 상태에 대한 응답으로 컬렉션에 항목을 추가할 수 있습니다. 지원 컬렉션/비즈니스 개체에 대한 데이터 바인딩을 사용하는 경우 해당 업데이트 가져오기 및 UI 업데이트가 자동으로 처리되므로 대체로 이 기법을 사용하는 것이 좋습니다([데이터 바인딩 세부 정보](https://msdn.microsoft.com/library/windows/apps/mt210946) 참조).

그러나 모든 앱 시나리오가 데이터 바인딩에 적합한 것은 아닙니다. 경우에 따라 런타임에 새 UI 요소를 만들고 표시되도록 해야 합니다. `BoxPanel` 이 이 시나리오에 적합합니다. `BoxPanel`은 자식 개수를 계산에 사용하고 모두 들어가도록 기존 자식 요소와 새 자식 요소를 새 레이아웃으로 조정하기 때문에 자식 항목 수의 변경은 문제가 되지 않습니다.

여기에는 나와 있지 않지만 `BoxPanel`을 추가로 확장하는 고급 시나리오는 동적 자식을 수용하고 개별 셀의 크기를 조정하기 위한 더 강력한 요소로 자식의 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)를 사용합니다. 이 시나리오에서는 "불필요하게 사용된" 공간을 좀 더 줄이기 위해 다양한 행 또는 열 크기나 그리드가 아닌 모양을 사용할 수 있습니다. 이 경우 미적 요인과 가장 작은 크기를 위해 다양한 크기와 가로 세로 비율의 여러 직사각형이 컨테이너 직사각형에 모두 들어가도록 하는 방법에 대한 전략이 필요합니다. `BoxPanel` 은 이 작업을 수행하지 않고 공간을 나누는 더 간단한 기법을 사용합니다. `BoxPanel`의 기법은 자식 개수보다 큰 최소 정사각형 수를 결정하는 것입니다. 예를 들어 9개 항목은 3x3 정사각형에 들어갑니다. 10 개 항목에는 4x4 정사각형이 필요합니다. 그러나 공간을 절약하기 위해 시작 정사각형의 행이나 열을 하나 제거하고 항목을 맞출 수도 있습니다. 개수=10 예제에서는 4x3 또는 3x4 직사각형에 들어갑니다.

패널에서 10개 항목에 대해 5x2를 선택하지 않는 이유가 궁금할 수도 있습니다. 이렇게 하면 항목 수에 정확히 맞기 때문입니다. 그러나 실제로 패널은 가로 세로 비율이 비슷한 직사각형으로 크기가 조정됩니다. 최소 정사각형 기법은 일반적인 레이아웃 모양에서 제대로 작동하도록 크기 조정 논리를 보정하고 셀 모양의 가로 세로 비율이 특이한 크기 조정을 권장하지 않는 방법입니다.

**참고**&nbsp;&nbsp;이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

## 관련 항목

**참조**

[**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)

[**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

[**패널**](https://msdn.microsoft.com/library/windows/apps/br227511)

**개념**

[맞춤, 여백 및 안쪽 여백](alignment-margin-padding.md)



<!--HONumber=Aug16_HO3-->


