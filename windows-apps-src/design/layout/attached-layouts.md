---
Description: ItemsRepeater 컨트롤과 같은 컨테이너에서 사용할 연결된 레이아웃을 정의할 수 있습니다.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5be16e22a30f0b366ad55f323a0f3f2aa2b7b837
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220306"
---
# <a name="attached-layouts"></a>연결된 레이아웃

레이아웃 논리를 다른 개체에 위임하는 컨테이너(예: 패널)는 연결된 레이아웃 개체를 사용하여 해당 자식 요소에 레이아웃 동작을 제공합니다.  연결된 레이아웃 모델을 통해 애플리케이션은 유연하게 런타임에 항목의 레이아웃을 변경하거나, UI의 여러 부분(예: 열에 맞춰 정렬된 것처럼 나타나는 테이블 행 항목) 간의 레이아웃 측면을 보다 쉽게 공유할 수 있습니다.

이 항목에서는 연결된 레이아웃(가상화 및 비가상화) 생성과 관련된 항목, 이해해야 하는 개념 및 클래스, 이러한 항목 중에서 결정할 때 고려해야 할 장단점을 다룹니다.

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되어 있습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리 개요](/uwp/toolkits/winui/)를 참조하세요. |

> **중요 API**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](../controls-and-patterns/items-repeater.md)
> * [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)(미리 보기)

## <a name="key-concepts"></a>주요 개념

레이아웃을 수행하려면 모든 요소에 대해 다음 두 가지 질문에 답변해야 합니다.

1. 이 요소의 ***크기***는 얼마나 되나요?

2. 이 요소는 어느 ***위치***에 배치되나요?

이러한 질문의 답변이 될 수 있는 XAML의 레이아웃 시스템은 [사용자 지정 패널](./custom-panels-overview.md)을 다루면서 간략히 설명됩니다.

### <a name="containers-and-context"></a>컨테이너 및 컨텍스트

개념적으로 XAML의 [패널](/uwp/api/windows.ui.xaml.controls.panel)은 프레임워크에서 다음과 같은 두 가지 중요한 역할을 합니다.

1. 자식 요소를 포함하고 요소 트리에서 분기를 도입할 수 있습니다.
2. 해당 자식에 특정 레이아웃 전략을 적용합니다.

이러한 이유로 XAML의 패널은 레이아웃과 비슷하지만, 기술적으로는 레이아웃 이상의 기능을 수행합니다.

[ItemsRepeater](../controls-and-patterns/items-repeater.md)도 패널처럼 동작하지만, 패널과 달리 프로그래밍 방식으로 UIElement 자식을 추가 또는 제거할 수 있도록 하는 Children 속성을 노출하지 않습니다.  대신, 자식 항목의 수명은 프레임워크에 의해 자동으로 관리되어 데이터 항목의 컬렉션에 해당합니다.  패널에서 파생되지 않지만 패널처럼 동작하고 프레임워크에서 처리됩니다.

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)은 패널에서 파생된 컨테이너로, 연결된 [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) 개체에 논리를 위임합니다.  LayoutPanel는 *미리 보기*로 제공되며 현재는 WinUI 패키지의 *시험판*에서만 사용할 수 있습니다.

#### <a name="containers"></a>컨테이너

개념적으로 [패널](/uwp/api/windows.ui.xaml.controls.panel)은 [배경](/uwp/api/windows.ui.xaml.controls.panel.background)에 대한 픽셀을 렌더링하는 기능도 제공하는 요소의 컨테이너입니다.  패널은 사용하기 쉬운 패키지에서 일반적인 레이아웃 논리를 캡슐화하는 방법을 제공합니다.

**연결된 레이아웃** 개념은 컨테이너의 역할과 레이아웃의 역할을 더 명확히 구분할 수 있도록 합니다.  컨테이너가 레이아웃 논리를 다른 개체에 위임하는 경우 아래 코드 조각과 같이 해당 개체를 연결된 레이아웃으로 지칭합니다. LayoutPanel과 같은 [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)에서 상속하는 컨테이너는 XAML의 레이아웃 프로세스에 입력을 제공하는 공용 속성(예: Height 및 Width)을 자동으로 노출합니다.

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

레이아웃 프로세스 중에 컨테이너는 연결된 *UniformGridLayout*을 사용하여 해당 자식을 측정하고 정렬합니다.

#### <a name="per-container-state"></a>컨테이너 단위 상태

연결된 레이아웃을 사용하여 레이아웃 개체의 단일 인스턴스는 아래 코드 조각과 같은 많은 컨테이너에  연결될 수 있습니다. 따라서 호스트 컨테이너에 종속되거나 호스트 컨테이너를 직접 참조해서는 안 됩니다.  예:

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

이러한 상황에서 *ExampleLayout*은 레이아웃 계산에서 사용하는 상태와 한 패널의 요소 레이아웃이 다른 패널에 영향을 미치지 않도록 하기 위해 해당 상태를 저장할 위치를 신중하게 고려해야 합니다.  해당 MeasureOverride 및 ArrangeOverride 논리가 해당 정적 속성의 값에 따라 달라지는 사용자 지정 패널과 유사합니다.

#### <a name="layoutcontext"></a>LayoutContext

[LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)는 이러한 문제를 처리하기 위한 것입니다.  이 항목은 자식 요소를 검색하는 경우처럼, 두 요소 간에 직접적인 종속성을 적용하지 않으면서 호스트 컨테이너와 상호 작용할 수 있도록 하는 연결된 레이아웃을 제공합니다. 또한 이 컨텍스트는 컨테이너의 자식 요소와 관련되어야 하는 모든 상태를 레이아웃에 저장할 수 있도록 합니다.

간단한 비가상화 레이아웃은 상태를 유지 관리하지 않아도 되므로 문제가 되지 않는 경우가 많습니다. 그러나 Grid와 같은 보다 복잡한 레이아웃은 값을 다시 계산하지 않도록 측정과 정렬 호출 간에 상태를 유지하도록 선택할 수 있습니다.

레이아웃을 가상화할 경우 반복적인 레이아웃 단계 간 뿐만 아니라 측정과 정렬 간에 상태를 유지 관리해야 하는 경우가 많습니다.

#### <a name="initializing-and-uninitializing-per-container-state"></a>컨테이너 단위 상태 초기화 및 초기화 해제

레이아웃이 컨테이너에 연결되면 해당 [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) 메서드가 호출되고 상태를 저장하기 위해 개체를 초기화할 수 있는 기회가 제공됩니다.

마찬가지로 컨테이너에서 레이아웃을 제거하면 [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) 메서드가 호출됩니다.  이를 통해 레이아웃에서는 해당 컨테이너와 연결된 모든 상태를 정리할 수 있는 기회가 제공됩니다.

컨텍스트에 대한 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 속성을 사용하여 레이아웃의 상태 개체를 컨테이너에 저장하고 컨테이너에서 검색할 수 있습니다.

### <a name="ui-virtualization"></a>UI 가상화

UI 가상화는 _필요할 때까지_ UI 개체의 생성을 지연시키는 것을 의미합니다.  성능 최적화를 나타냅니다.  스크롤할 수 없는 시나리오의 경우 앱과 관련된 여러 가지 요인을 기준으로_필요할 때_를 결정합니다.  이러한 경우 앱은 [x:Load](../../xaml-platform/x-load-attribute.md)를 사용하는 것이 좋습니다. 레이아웃에서 특별한 처리는 필요하지 않습니다.

목록과 같은 스크롤 기반 시나리오에서는 _필요할 때_를 결정할 때 레이아웃 프로세스 중 배치된 위치가 많은 영향을 미치며, 특별한 고려 사항도 적용됩니다.  이 문서에서는 이 시나리오를 중점적으로 다룹니다.

> [!NOTE]
> 이 문서에서는 다루지 않았지만 스크롤 시나리오에서 UI 가상화를 사용하도록 설정하는 것과 동일한 기능을 비스크롤 시나리오에도 적용할 수 있습니다.  예를 들어, 표시되는 명령의 수명을 관리하고, 표시되는 영역과 오버플로 메뉴 간에 요소를 재활용/이동하여 사용 가능한 공간 변화에 응답하는 데이터 기반 도구 모음 컨트롤이 있습니다.

## <a name="getting-started"></a>시작하기

먼저, 만들어야 하는 레이아웃이 UI 가상화를 지원해야 하는지 여부를 결정합니다.

**유의할 사항...**

1. 비가상화 레이아웃은 제작하기가 더 쉽습니다. 항목 수가 항상 작은 경우에는 비가상화 레이아웃을 제작하는 것이 좋습니다.
2. 플랫폼은 일반적인 요구를 해결하기 위해 [ItemsRepeater](../controls-and-patterns/items-repeater.md#change-the-layout-of-items) 및 [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)에서 작동하는 연결된 레이아웃 세트를 제공합니다.  사용자 지정 레이아웃을 정의해야 할지를 결정하기 전에 해당 내용을 숙지하세요.
3. 가상화 레이아웃에서는 비가상화 레이아웃에 비해 CPU와 메모리 비용/복잡성/오버헤드가 추가됩니다.  경험에 따르면 레이아웃에서 관리해야 하는 자식 항목이 뷰포트의 3배 크기 영역에 잘 맞게 되면 일반적으로 가상화 레이아웃에서 많은 이점을 얻을 수 없습니다. 3배 크기는 이 문서 뒷부분에서 더 자세히 설명하겠지만, 그 근거는 Windows에서의 스크롤 작업이 갖는 특성과 가상화에 미치는 영향에 있습니다.

> [!TIP]
> 참고로, [ListView](/uwp/api/windows.ui.xaml.controls.listview)(및 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater))의 기본 설정은 항목 수가 현재 뷰포트의 3배 크기를 채울만큼 충분해질 때까지 재활용은 시작되지 않게 하는 것입니다.

**기본 형식 선택**

![연결된 레이아웃 계층 구조](images/xaml-attached-layout-hierarchy.png)

기본 [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) 형식에는 연결된 레이아웃을 제작하기 위한 시작점으로 사용되는 두 개의 파생 형식이 있습니다.

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>비가상화 레이아웃

비가상화 레이아웃을 만드는 방법은 [사용자 지정 패널](./custom-panels-overview.md)을 만든 모든 사용자에게 친숙할 것입니다.  동일한 개념이 적용됩니다.  주요 차이점은 [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)를 사용하여 [Children](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) 컬렉션에 액세스하며, 레이아웃은 상태를 저장하도록 선택할 수 있다는 것입니다.

1. 기본 형식 [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)(Panel 대신)에서 파생됩니다.
2. *(선택 사항)* 변경될 때 레이아웃을 무효화하는 종속성 속성을 정의합니다.
3. _(**신규**/선택 사항)_ [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)의 일부로 레이아웃에 필요한 모든 상태 개체를 초기화합니다. 컨텍스트와 함께 제공되는 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)를 사용하여 호스트 컨테이너와 함께 스태시합니다.
4. [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride)를 재정의하고 모든 자식 항목에 대해 [Measure](/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 호출합니다.
5. [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride)를 재정의하고 모든 자식 항목에 대해 [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 호출합니다.
6. *(**신규**/선택 사항)* 저장된 모든 상태를 [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)의 일부로 정리합니다.

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>예제: 간단한 스택 레이아웃(다양한 크기의 항목)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

다음은 다양한 크기의 항목에 대한 기본 비가상화 스택 레이아웃입니다. 레이아웃의 동작을 조정하는 속성은 없습니다. 다음 구현에서는 레이아웃이 컨테이너에서 제공하는 컨텍스트 개체에 의존하는 방법을 보여 줍니다.

1. 자식 수를 가져옵니다.
2. 인덱스를 사용하여 각 자식 요소에 액세스합니다.

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>가상화 레이아웃

비가상화 레이아웃과 마찬가지로 가상화 레이아웃의 개략적인 단계도 동일합니다.  대체적으로 이러한 복잡성에 따라 뷰포트 내에 포함하여 구현해야 하는 요소가 결정됩니다.

1. 기본 형식 [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)에서 파생됩니다.
2. (선택 사항) 변경될 때 레이아웃을 무효화하는 종속성 속성을 정의합니다.
3. [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)의 일부로 레이아웃에 필요한 모든 상태 개체를 초기화합니다. 컨텍스트와 함께 제공되는 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)를 사용하여 호스트 컨테이너와 함께 스태시합니다.
4. [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)를 재정의하고 구현해야 하는 각 자식 항목에 대한 [Measure](/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 호출합니다.
   1. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드는 프레임워크에서 준비한 UIElement를 검색하는 데 사용됩니다(예: 데이터 바인딩 적용).
5. [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)를 재정의하고 구현된 각 자식 항목에 대해 [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 호출합니다.
6. (선택 사항) 저장된 모든 상태를 [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)의 일부로 정리합니다.

> [!TIP]
> [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)에서 반환되는 값은 가상화된 콘텐츠의 크기로 사용됩니다.

가상화 레이아웃을 제작할 때 고려할 두 가지 일반적인 방법이 있습니다.  어떤 방법을 선택할지는 "요소의 크기를 결정하는 방법"에 따라 크게 좌우됩니다.  데이터 세트의 항목 인덱스를 아는 것만으로 충분하거나, 데이터 자체가 최종 크기를 나타내는 경우 **데이터 종속**으로 간주할 수 있습니다.  이 경우 좀 더 간단하게 만들 수 있습니다.  그러나 항목 크기를 결정하는 유일한 방법이 UI를 만들고 측정하는 것이라면 **콘텐츠 종속**이라고 말할 수 있습니다.  이러한 방법은 좀 더 복잡합니다.

### <a name="the-layout-process"></a>레이아웃 프로세스

데이터 종속 레이아웃을 만들거나 콘텐츠 종속 레이아웃을 만드는 경우 레이아웃 프로세스와 Windows 비동기 스크롤의 영향을 이해하는 것이 중요합니다.

시작부터 화면에 UI를 표시할 때까지 프레임워크에서 수행하는 단계에 대한 자세한 보기는 다음과 같습니다.

1. 태그를 구문 분석합니다.

2. 요소의 트리를 생성합니다.

3. 레이아웃 단계를 수행합니다.

4. 렌더링 단계를 수행합니다.

UI 가상화를 사용하면 2단계에서 일반적으로 수행되는 요소 생성 작업은 지연되거나 뷰포트를 채울만큼 충분한 콘텐츠가 생성된 것으로 확인되면 일찍 종료됩니다. 가상화 컨테이너(예: ItemsRepeater)는 이 프로세스를 구동하기 위해 연결된 레이아웃을 따릅니다. 이 컨테이너는 연결된 레이아웃에 가상화 레이아웃에 필요한 추가 정보를 표시하는 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)를 제공합니다.

**RealizationRect(즉, 뷰포트)**

Windows 상의 스크롤은 UI 스레드에 대해 비동기적으로 수행됩니다. 프레임워크의 레이아웃에 의해 제어되지 않습니다.  대신 시스템의 Compositor에서 상호 작용 및 이동이 발생합니다. 이 방법의 장점은 콘텐츠 이동을 항상 60fps에서 수행할 수 있다는 것입니다.  그러나 문제는 레이아웃에 표시되는 "뷰포트"가 실제로 화면에 표시되는 결과에 비해 약간 이전 콘텐츠일 수 있다는 것입니다. 사용자가 빠르게 스크롤하면 UI 스레드가 새 콘텐츠를 생성하고 "검은색으로 이동"하는 속도보다 빨라질 수 있습니다. 이러한 이유로, 가상화 레이아웃을 통해 뷰포트보다 큰 영역을 채우기에 충분한 준비된 요소의 추가 버퍼를 생성해야 하는 경우가 종종 있습니다. 스크롤하는 동안 과도한 부하가 발생하는 경우에도 사용자에게 콘텐츠가 표시됩니다.

![구현 Rect](images/xaml-attached-layout-realizationrect.png)

요소를 만드는 데 비용이 많이 들기 때문에 가상화 컨테이너(예: [ItemsRepeater](../controls-and-patterns/items-repeater.md))는 처음에는 뷰포트와 일치하는 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)를 연결된 레이아웃에 제공합니다. 유휴 시간에 컨테이너는 점점 커지는 구현 Rect를 사용하여 레이아웃을 반복적으로 호출함으로써 준비된 콘텐츠의 버퍼를 늘릴 수 있습니다. 이 동작은 빠른 시작 시간과 양호한 이동 환경 간에 균형을 맞추려고 시도하는 성능 최적화입니다. ItemsRepeater가 생성하는 최대 버퍼 크기는 [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 및 [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 속성에 의해 제어됩니다.

**요소 다시 사용(재활용)**

레이아웃은 실행될 때마다 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)를 채울 수 있게 요소의 크기 및 위치를 조정하게 됩니다. 기본적으로 [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)은 각 레이아웃 단계가 끝날 때 사용되지 않은 모든 요소를 재활용합니다.

[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 및 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)의 일부로 레이아웃에 전달되는 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)는 가상화 레이아웃에 필요한 추가 정보를 제공합니다. 가장 일반적으로 사용되는 항목 중 일부는 다음을 수행하는 기능을 제공합니다.

1. 데이터의 항목 수를 쿼리합니다([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) 메서드를 사용하여 특정 항목을 검색합니다.
3. 레이아웃이 구현된 요소로 채울 뷰포트 및 버퍼를 나타내는 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)를 검색합니다.
4. [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드를 사용하여 특정 항목에 대한 UIElement를 요청합니다.

지정된 인덱스에 대한 요소를 요청하면 해당 요소가 해당 레이아웃 단계에 대해 "사용 중"으로 표시됩니다. 해당 요소가 아직 없는 경우에는 구현되고 자동으로 사용할 수 있게 준비됩니다(예: DataTemplate에 정의된 UI 트리 팽창, 데이터 바인딩 처리 등).  그렇지 않으면 기존 인스턴스의 풀에서 검색됩니다.

각 측정 단계가 끝난 후에 "사용 중"으로 표시되지 않은 구현된 모든 기존 요소는 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드를 통해 요소가 검색될 때 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 옵션이 사용되지 않는 한, 자동으로 재사용 가능한 것으로 간주됩니다. 이 프레임워크는 자동으로 재활용 풀로 이동되어 사용할 수 있게 됩니다. 이후에 다른 컨테이너에서 사용할 수 있습니다. 요소를 다시 부모로 지정할 때 다소 비용이 발생하므로 프레임워크는 가능한 경우 이를 방지하려고 시도합니다.

가상화 레이아웃이 각 측정이 시작될 때 구현 Rect에 더 이상 속하지 않는 요소를 알게 되면 재사용을 최적화할 수 있습니다. 즉, 프레임워크의 기본 동작에 의존하지 않습니다. 레이아웃은 [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) 메서드를 사용하여 요소를 선점적으로 재활용 풀로 이동할 수 있습니다.  새 요소를 요청하기 전에 이 메서드를 호출하면 레이아웃에서 나중에 요소와 연결되지 않은 인덱스에 대해 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 요청을 실행할 때 해당 기존 요소를 사용할 수 있게 됩니다.

VirtualizingLayoutContext는 콘텐츠 종속 레이아웃을 만드는 레이아웃 작성자를 위해 디자인된 두 가지 추가 속성을 제공합니다. 이 속성에 대해서는 나중에 자세히 설명합니다.

1. 레이아웃에 선택적 _input_을 제공하는 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)입니다.
2. 레이아웃의 선택적인 _output_인 [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)입니다.

## <a name="data-dependent-virtualizing-layouts"></a>데이터 종속 가상화 레이아웃

표시할 콘텐츠를 측정할 필요가 없이, 모든 항목의 크기를 알고 있으면 가상화 레이아웃을 사용하는 것이 더 쉽습니다.  이 문서에서는 일반적으로 데이터 검사와 관련이 있으므로 이 범주의 가상화 레이아웃을 간단히 **데이터 레이아웃**으로 나타냅니다.  데이터를 기준으로 하는 앱은 데이터의 일부가 이전에 디자인에 따라 결정되었으므로 알려진 크기의 시각적 표현을 선택할 수 있습니다.

일반적인 방법은 레이아웃에서 다음을 수행하는 것입니다.

1. 모든 항목의 크기와 위치를 계산합니다.
2. [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)의 일부로 다음을 수행합니다.
   1. [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)를 사용하여 뷰포트 내에 표시되어야 하는 항목을 결정합니다.
   2. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드를 사용하여 항목을 나타내야 하는 UIElement를 검색합니다.
   3. 미리 계산된 크기를 사용하여 UIElement를 [측정](/uwp/api/windows.ui.xaml.uielement.measure)합니다.
3. [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)의 일부로, 구현된 각 UIElement를 미리 계산된 위치로 [정렬](/uwp/api/windows.ui.xaml.uielement.arrange)합니다.

> [!NOTE]
> 데이터 레이아웃 방법은 _데이터 가상화_와 호환되지 않는 경우가 많습니다.  특히 메모리에 로드되는 유일한 데이터는 사용자에게 표시되는 콘텐츠를 채우는 데 필요한 데이터입니다.  데이터 가상화는 해당 데이터가 상주하는 위치로 스크롤할 때의 데이터에 대한 지연 또는 증분 로드를 나타내지 않습니다.  대신, 뷰 밖으로 스크롤할 때 항목이 메모리에서 해제되는 경우를 나타냅니다.  데이터 레이아웃이 데이터 레이아웃의 일부로 모든 데이터 항목을 검사하도록 하면 데이터 가상화가 예상대로 작동하지 않게 됩니다.  예외는 모든 항목의 크기가 동일하다고 가정하는 UniformGridLayout과 같은 레이아웃입니다.

> [!TIP]
> 다양한 상황에서 다른 사용자가 사용할 수 있는 컨트롤 라이브러리에 대해 사용자 지정 컨트롤을 만드는 경우 데이터 레이아웃이 적절하지 않을 수 있습니다.

### <a name="example-xbox-activity-feed-layout"></a>예제: Xbox 작업 피드 레이아웃

Xbox 작업 피드용 UI는 각 줄에 넓은 타일과 2개의 좁은 타일이 표시되고, 다음 줄에서는 이러한 순서가 반전되는 반복 패턴을 사용합니다. 이 레이아웃에서 모든 항목의 크기는 데이터 세트에서의 항목 위치와 타일의 알려진 크기(넓은 및 좁은) 함수입니다.

![Xbox 작업 피드](images/xaml-attached-layout-activityfeedscreenshot.png)

아래 코드는 **데이터 레이아웃**에 대해 수행할 수 있는 일반적인 방법을 설명하는 작업 피드에 대한 사용자 지정 가상화 UI를 살펴봅니다.

<table>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고, 이 샘플 레이아웃에서 실제로 작동하는 <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>를 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>구현

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(선택 사항) Item-UIElement 매핑 관리

기본적으로 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)는 구현된 요소와 해당 요소가 표시하는 데이터 원본의 인덱스 간 매핑을 유지 관리합니다.  레이아웃은 기본 자동 재생 동작을 방해하는 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드를 통해 요소를 검색할 때 항상 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 옵션을 요청하여 이 매핑 자체를 관리하도록 선택할 수 있습니다.  스크롤이 한 방향으로 제한되고, 고려하는 항목이 항상 인접해 있는 경우에만(즉, 첫 번째 및 마지막 요소의 인덱스를 아는 것만으로 구현해야 하는 모든 요소를 충분히 알 수 있음) 레이아웃에서 이 작업을 수행하도록 선택할 수 있습니다.

#### <a name="example-xbox-activity-feed-measure"></a>예제: Xbox 작업 피드 측정값

아래 코드 조각은 매핑을 관리하기 위해 이전 샘플에서 MeasureOverride에 추가할 수 있는 추가 논리를 보여 줍니다.

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>콘텐츠 종속 가상화 레이아웃

정확한 크기를 알기 위해 항목에 대한 UI 콘텐츠를 먼저 측정해야 하는 경우 **콘텐츠 종속 레이아웃**입니다.  레이아웃에 따라 항목 크기가 조정되지 않고, 각 항목이 자체적으로 크기를 조정해야 하는 레이아웃으로도 간주할 수 있습니다. 이 범주에 포함되는 가상화 레이아웃이 여기에 더 가깝습니다.

> [!NOTE]
> 콘텐츠 종속 레이아웃은 데이터 가상화를 중단하지 않습니다.

### <a name="estimations"></a>예측

콘텐츠 종속 레이아웃은 구현되지 않은 콘텐츠의 크기과 구현된 콘텐츠의 위치를 둘 다 추정하기 위해 예측에 의존합니다. 이러한 예측에 따라 변경이 진행되면 구현된 콘텐츠가 스크롤 가능 영역 내에서 위치를 정기적으로 이동하게 됩니다. 이러한 현상이 완화되지 않으면 혼란스럽고 정리되지 않은 환경이 제공될 수 있습니다. 잠재적인 문제 및 완화 방법은 여기에 설명되어 있습니다.

> [!NOTE]
> 구현되었거나 구현되지 않은 모든 항목을 고려하고 모든 항목의 정확한 크기와 위치를 알고 있는 데이터 레이아웃에서는 이러한 문제가 전적으로 방지될 수 있습니다.

**스크롤 앵커**

XAML은 [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 인터페이스를 구현하여 [스크롤 앵커](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)를 지원하도록 함으로써 갑작스러운 뷰포트 변화를 완화하는 메커니즘을 제공합니다. 사용자가 콘텐츠를 조작하는 경우 스크롤 컨트롤은 추적되도록 옵트인(opt-in)된 후보 세트에서 요소를 지속적으로 선택합니다. 앵커 요소의 위치가 레이아웃 중에 이동하는 경우 스크롤 컨트롤이 뷰포트를 유지하기 위해 자동으로 이동합니다.

레이아웃에 제공된 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)의 값은 스크롤 컨트롤에 의해 현재 선택된 앵커 요소를 반영할 수 있습니다. 또는 개발자가 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)에 대해 [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) 메서드를 사용하여 인덱스에 대해 요소가 구현되도록 명시적으로 요청하는 경우, 다음 레이아웃 단계에서 해당 인덱스가 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)로 제공됩니다. 이렇게 하면 개발자가 요소를 구현하고, 이후에 [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) 메서드를 통해 뷰로 가져오도록 요청하는 시나리오에 맞게 레이아웃을 준비할 수 있습니다.

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)는 항목의 위치를 예측할 때 콘텐츠 종속 레이아웃이 먼저 배치해야 하는 데이터 원본 항목의 인덱스입니다. 구현된 다른 항목의 위치를 지정하기 위한 시작점으로 사용해야 합니다.

**스크롤 막대에 미치는 영향**

스크롤 앵커를 사용하는 경우에도 콘텐츠 크기가 크게 변경되어 레이아웃 예측치가 크게 달라지면 스크롤 막대의 엄지 단추 위치가 빠르게 이동할 수 있습니다.  엄지 단추를 끌 때 마우스 포인터의 위치를 따라가지 않는 것처럼 보이면 사용자에게 혼동을 일으킬 수 있습니다.

레이아웃의 예측이 정확해 질수록 사용자에게 스크롤 막대의 엄지 단추가 빠르게 이동하는 것처럼 보이지 않을 확률이 높습니다.

### <a name="layout-corrections"></a>레이아웃 수정

콘텐츠 종속 레이아웃은 실제를 고려해서 해당 추정치를 준비해야 합니다.  예를 들어, 사용자가 콘텐츠의 맨 위로 스크롤하고 레이아웃이 맨 처음 요소를 구현하면 시작된 요소를 기준으로 하는 요소의 예상 위치가 원점(x:0, y:0) 이외의 위치에 표시될 수 있습니다. 이 경우 레이아웃은 [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 속성을 사용하여 계산한 위치를 새 레이아웃 원점으로 설정할 수 있습니다.  순결과는 스크롤 컨트롤의 뷰포트가 레이아웃이 보고한 콘텐츠의 위치를 고려하여 자동으로 조정되는 스크롤 앵커와 비슷합니다.

![LayoutOrigin 수정](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>연결이 끊어진 뷰포트

레이아웃의 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 메서드에서 반환되는 크기는 각 연속 레이아웃에서 변경될 수 있는 콘텐츠의 크기를 가장 잘 예측하는 것으로 나타냅니다.  사용자가 스크롤하면 레이아웃이 업데이트된 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)를 사용하여 계속 재평가됩니다.

사용자가 엄지 단추를 매우 빠르게 끌면, 레이아웃의 관점에서 이전 위치가 현재 위치와 겹치지 않도록 뷰포트가 크게 이동할 수 있습니다.  이것은 스크롤의 비동기 특성 때문입니다. 또한 레이아웃을 사용하는 앱은 요소가 현재 구현되지 않고 레이아웃으로 추적되는 현재 범위 밖에 놓일 것으로 예상되는 항목에 대한 뷰에 표시되도록 요청할 수 있습니다.

레이아웃이 해당 추측치가 올바르지 않음을 확인하거나 예기치 않은 뷰포트 이동을 알아낼 경우 해당 시작 위치를 다시 지정해야 합니다.  XAML 컨트롤의 일부로 제공되는 가상화 레이아웃은 표시되는 콘텐츠의 특성을 덜 제한하므로 콘텐츠 종속 레이아웃으로 개발됩니다.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>예제: 가변 크기 항목에 대한 간단한 가상화 스택 레이아웃

아래 샘플에서는 가변 크기 항목에 대해 다음과 같은 간단한 스택 레이아웃을 보여 줍니다.

* UI 가상화를 지원합니다.
* 예측을 사용하여 구현되지 않은 항목의 크기를 추정합니다.
* 잠재적인 비연속 뷰포트 변화를 인식합니다.
* 이러한 이동에 대해 고려할 레이아웃 수정 내용을 적용합니다.

**사용법: 태그**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**코드 숨김: Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**코드: VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>관련된 문서

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
