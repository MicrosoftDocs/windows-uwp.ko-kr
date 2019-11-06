---
Description: '컨테이너에서 사용할 연결 된 레이아웃을 정의할 수 있습니다 (예: 배치 Repeater 컨트롤).'
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dc23e86f85c5db3dd10c5cec152047be387d4513
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282294"
---
# <a name="attached-layouts"></a>연결 된 레이아웃

레이아웃 논리를 다른 개체에 위임 하는 컨테이너 (예: Panel)는 연결 된 레이아웃 개체를 사용 하 여 해당 자식 요소에 대 한 레이아웃 동작을 제공 합니다.  연결 된 레이아웃 모델을 통해 응용 프로그램은 런타임에 항목의 레이아웃을 변경 하거나, UI의 여러 부분 (예: 열에 정렬 된 것으로 표시 되는 테이블의 행에 있는 항목) 간의 레이아웃 측면을 보다 쉽게 공유할 수 있습니다.

이 항목에서는 연결 된 레이아웃 (가상화 및 비 가상화)을 만드는 데 관련 된 내용과 이해 해야 하는 개념 및 클래스와 이러한 개념을 결정할 때 고려해 야 할 장단점에 대해 다룹니다.

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 UWP 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되었습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

> **중요 API**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (미리 보기)

## <a name="key-concepts"></a>주요 개념

레이아웃을 수행 하려면 모든 요소에 대해 두 가지 질문에 답변해 야 합니다.

1. 이 요소의 ***크기*** 는 어떻게 되나요?

2. 이 요소의 ***위치*** 는 어떻게 되나요?

이러한 질문에 대답 하는 XAML의 레이아웃 시스템은 [사용자 지정 패널](/windows/uwp/design/layout/custom-panels-overview)에 대 한 설명의 일부로 간략하게 설명 되어 있습니다.

### <a name="containers-and-context"></a>컨테이너 및 컨텍스트

개념적으로 XAML [패널](/uwp/api/windows.ui.xaml.controls.panel) 은 프레임 워크의 두 가지 중요 한 역할을 채웁니다.

1. 자식 요소를 포함 하 고 요소 트리에서 분기를 도입할 수 있습니다.
2. 해당 자식에 특정 레이아웃 전략을 적용 합니다.

이러한 이유로 XAML의 패널은 레이아웃에 일반적으로 사용 되지만 기술적으로는 레이아웃 보다 더 많이 사용 됩니다.

항목 [리피터](/windows/uwp/design/controls-and-patterns/items-repeater) 는 패널 처럼 동작 하지만 패널과 달리 프로그래밍 방식으로 UIElement 자식을 추가 하거나 제거할 수 있는 자식 속성을 노출 하지 않습니다.  대신, 자식 항목의 수명은 프레임 워크에 의해 자동으로 관리 되어 데이터 항목의 컬렉션에 해당 합니다.  패널에서 파생 되지 않지만 패널에서 파생 되 고 프레임 워크에서 처리 됩니다.

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) 는 패널에서 파생 된 컨테이너 이며, 연결 된 [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) 개체에 논리를 위임 합니다.  LayoutPanel는 *미리 보기* 상태 이며 현재 WinUI 패키지의 *시험판* 삭제 에서만 사용할 수 있습니다.

#### <a name="containers"></a>컨테이너

개념적으로 [패널](/uwp/api/windows.ui.xaml.controls.panel) 은 [배경의](/uwp/api/windows.ui.xaml.controls.panel.background)픽셀을 렌더링 하는 기능이 있는 요소의 컨테이너입니다.  패널은 사용 하기 쉬운 패키지에서 일반적인 레이아웃 논리를 캡슐화 하는 방법을 제공 합니다.

**연결 된 레이아웃** 의 개념은 컨테이너와 레이아웃의 두 역할을 구분 하는 것이 더 명확 하 게 해줍니다.  컨테이너가 레이아웃 논리를 다른 개체에 위임 하는 경우 아래 코드 조각과 같이 해당 개체를 연결 된 레이아웃으로 호출 합니다. LayoutPanel와 같이 [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)에서 상속 되는 컨테이너는 XAML의 레이아웃 프로세스에 입력을 제공 하는 공용 속성 (예: 높이 및 너비)을 자동으로 노출 합니다.

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

레이아웃 프로세스 중에 컨테이너는 연결 된 *UniformGridLayout* 를 사용 하 여 자식을 측정 하 고 정렬 합니다.

#### <a name="per-container-state"></a>컨테이너 단위 상태

연결 된 레이아웃을 사용 하 여 레이아웃 개체의 단일 인스턴스는 아래 코드 조각과 같은 *많은* 컨테이너와 연결 될 수 있습니다. 따라서 호스트 컨테이너에 종속 되거나 직접 참조 해서는 안 됩니다.  예를 들어 다음과 같은 가치를 제공해야 합니다.

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

이러한 상황에서 *ExampleLayout* 는 레이아웃 계산에 사용 되는 상태를 신중 하 게 고려해 야 하며, 한 패널의 요소에 대 한 레이아웃에 영향을 주지 않도록 해당 상태가 저장 되는 위치를 신중 하 게 고려해 야 합니다.  System.windows.frameworkelement.measureoverride 및 System.windows.frameworkelement.arrangeoverride 논리는 해당 *정적* 속성의 값에 따라 달라 지는 사용자 지정 패널과 유사 합니다.

#### <a name="layoutcontext"></a>LayoutContext

[Layoutcontext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) 의 용도는 이러한 문제를 처리 하는 것입니다.  이 클래스는 자식 요소를 검색 하는 것과 같은 호스트 컨테이너와 상호 작용할 수 있는 기능을 연결 된 레이아웃에 제공 합니다. 또한 컨텍스트는 컨테이너의 자식 요소와 관련 될 수 있는 필요한 모든 상태를 레이아웃에 저장할 수 있도록 합니다.

간단 하 고 비 가상화 레이아웃은 상태를 유지 관리 하지 않아도 되 고 문제가 되지 않는 경우가 많습니다. 그러나 Grid와 같은 보다 복잡 한 레이아웃은 값을 다시 계산 하지 않도록 측정값과 정렬 호출 사이의 상태를 유지 하도록 선택할 수 있습니다.

레이아웃을 가상화 하는 *경우가 종종* 측정값과 정렬 뿐만 아니라 반복적인 레이아웃 전달 사이에 몇 가지 상태를 유지 해야 합니다.

#### <a name="initializing-and-uninitializing-per-container-state"></a>컨테이너 별 상태 초기화 및 초기화

레이아웃이 컨테이너에 연결 되 면 해당 [Initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) 메서드가 호출 되 고 상태를 저장 하기 위해 개체를 초기화할 수 있는 기회를 제공 합니다.

마찬가지로 컨테이너에서 레이아웃을 제거 하는 경우 [Uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) 메서드가 호출 됩니다.  이렇게 하면 해당 컨테이너와 연결 된 모든 상태를 정리할 수 있는 기회가 레이아웃에 제공 됩니다.

레이아웃의 상태 개체를에 저장 하 고 컨텍스트에서 [Layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 속성을 사용 하 여 컨테이너에서 검색할 수 있습니다.

### <a name="ui-virtualization"></a>UI 가상화

UI 가상화는 _필요할 때_까지 ui 개체 생성을 지연 하는 것을 의미 합니다.  성능 최적화입니다.  스크롤하지 않는 시나리오의 경우, _필요한 경우_ 앱 별 항목 수에 따라 결정 됩니다.  이러한 경우에는 응용 프로그램에서 [X:load](../../xaml-platform/x-load-attribute.md)를 사용 하는 것을 고려해 야 합니다. 레이아웃에서 특별 한 처리가 필요 하지 않습니다.

목록과 같은 스크롤 기반 시나리오에서 _필요_ 에 따라 "사용자에 게 표시 됩니다"를 결정 합니다 .이는 레이아웃 프로세스 중에 배치 된 위치에 따라 달라 지 며 특별 한 고려 사항이 필요 합니다.  이 시나리오는이 문서에서 중점적으로 다룹니다.

> [!NOTE]
> 이 문서에서 다루지 않았지만 스크롤 시나리오에서 UI 가상화를 사용 하도록 설정 하는 것과 동일한 기능을 스크롤하지 않는 시나리오에서 적용할 수 있습니다.  예를 들어 표시 되는 명령의 수명을 관리 하 고 표시 되는 영역과 오버플로 메뉴 간에 요소를 재활용/이동 하 여 사용 가능한 공간의 변경에 응답 하는 데이터 기반 도구 모음 컨트롤이 있습니다.

## <a name="getting-started"></a>시작하기

먼저, 만들어야 하는 레이아웃에서 UI 가상화를 지원 해야 하는지 여부를 결정 합니다.

**몇 가지 사항을 염두에 두어야 합니다.**

1. 비 가상화 레이아웃은 제작 하기가 더 쉽습니다. 항목 수가 항상 작은 경우에는 비 가상화 레이아웃을 작성 하는 것이 좋습니다.
2. 플랫폼은 일반적인 요구를 충족 하기 위해 [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) [와 함께](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) 작동 하는 연결 된 레이아웃 집합을 제공 합니다.  사용자 지정 레이아웃을 정의 해야 하는 것을 결정 하기 전에 해당 내용을 숙지 하세요.
3. 레이아웃을 가상화 하는 경우 비 가상화 레이아웃에 비해 CPU와 메모리 비용/복잡성/오버 헤드가 추가 됩니다.  레이아웃을 관리 하는 데 필요한 자식 항목이 뷰포트의 3 배 크기를 초과 하는 영역에 있을 가능성이 큰 경우 일반적으로 가상화 레이아웃에서 많은 이점을 얻을 수 없습니다. 3 배 크기는이 문서 뒷부분에서 더 자세히 설명 하지만 Windows에서 스크롤 하는 비동기 특성과 가상화에 대 한 영향 때문입니다.

> [!TIP]
> 참고로, [ListView](/uwp/api/windows.ui.xaml.controls.listview) (및 항목 [리피터](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater))의 기본 설정은 항목 수가 현재 뷰포트의 3 배 크기를 채우도록 충분 한 경우까지 재생을 시작 하지 않는다는 점입니다.

**기본 형식 선택**

![연결 된 레이아웃 계층 구조](images/xaml-attached-layout-hierarchy.png)

기본 [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layout) 형식에는 연결 된 레이아웃을 제작 하기 위한 시작점으로 사용 되는 두 개의 파생 형식이 있습니다.

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>비 가상화 레이아웃

비 가상화 레이아웃을 만드는 방법은 [사용자 지정 패널](/windows/uwp/design/layout/custom-panels-overview)을 만든 사람에 게 친숙 해야 합니다.  동일한 개념이 적용 됩니다.  주요 차이점은 [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) 는 [자식](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) 컬렉션에 액세스 하는 데 사용 되 고 레이아웃은 상태를 저장 하도록 선택할 수 있습니다.

1. 기본 형식 [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (패널 대신)에서 파생 됩니다.
2. *(선택 사항)* 변경 될 때 레이아웃을 무효화 하는 종속성 속성을 정의 합니다.
3. _(**신규**/선택적)_ [Initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)의 일부로 레이아웃에 필요한 모든 상태 개체를 초기화 합니다. 컨텍스트와 함께 제공 되는 [Layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 를 사용 하 여 호스트 컨테이너를 사용 하 여 준비 합니다.
4. [System.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) 를 재정의 하 고 모든 자식에 대해 [Measure](/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 호출 합니다.
5. [System.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) 를 재정의 하 고 모든 자식의 [정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 호출 합니다.
6. *(**신규**/선택적)* [Uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)의 일부로 저장 된 상태를 정리 합니다.

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>예: 간단한 스택 레이아웃 (다양 한 크기의 항목)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

다음은 다양 한 크기의 항목에 대 한 기본 비 가상화 스택 레이아웃입니다. 레이아웃의 동작을 조정 하는 속성은 없습니다. 다음 구현에서는 레이아웃이 컨테이너에서 제공 하는 컨텍스트 개체에 의존 하는 방법을 보여 줍니다.

1. 자식 수를 가져옵니다.
2. 인덱스를 사용 하 여 각 자식 요소에 액세스 합니다.

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

## <a name="virtualizing-layouts"></a>레이아웃 가상화

비 가상화 레이아웃과 마찬가지로 가상화 레이아웃에 대 한 개략적인 단계는 동일 합니다.  이러한 복잡성은 주로 뷰포트 내에 포함 되는 요소를 결정 하는 데 있어 실현 해야 합니다.

1. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)기본 형식에서 파생 됩니다.
2. 필드 변경 시 레이아웃이 무효화 되는 종속성 속성을 정의 합니다.
3. [Initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)의 일부로 레이아웃에 필요할 모든 상태 개체를 초기화 합니다. 컨텍스트와 함께 제공 되는 [Layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 를 사용 하 여 호스트 컨테이너를 사용 하 여 준비 합니다.
4. [System.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 를 재정의 하 고 실현 되어야 하는 각 자식에 대해 [Measure](/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 호출 합니다.
   1. [Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드는 프레임 워크에서 준비한 UIElement (예: 데이터 바인딩 적용)를 검색 하는 데 사용 됩니다.
5. [System.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) 를 재정의 하 고 실현 된 각 자식에 대해 [정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 호출 합니다.
6. 필드 [Uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)의 일부로 저장 된 상태를 정리 합니다.

> [!TIP]
> [System.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) 에서 반환 되는 값은 가상화 된 콘텐츠의 크기로 사용 됩니다.

가상화 레이아웃을 제작할 때 고려할 두 가지 일반적인 방법이 있습니다.  하나를 선택 하는지 여부는 "요소의 크기를 결정 하는 방법"에 따라 크게 달라 집니다.  데이터 집합에 있는 항목의 인덱스를 정확히 알고 있거나 데이터 자체가 최종 크기를 결정 하는 경우에는 **데이터에 따라 달라 집니다**.  이러한 단계는 보다 간단 하 게 만들 수 있습니다.  그러나 항목 크기를 확인 하는 유일한 방법은 UI를 만들고 측정 하는 것입니다 .이 경우에는 **내용에 따라 달라 집니다**.  이러한 방법은 더 복잡 합니다.

### <a name="the-layout-process"></a>레이아웃 프로세스

데이터 또는 콘텐츠 종속 레이아웃을 만드는 경우에는 레이아웃 프로세스와 Windows의 비동기 스크롤의 영향을 이해 하는 것이 중요 합니다.

프레임 워크에서 화면에 UI를 표시할 때까지 프레임 워크에서 수행 하는 단계에 대 한 자세한 내용은 다음과 같습니다.

1. 태그를 구문 분석 합니다.

2. 요소의 트리를 생성 합니다.

3. 레이아웃 단계를 수행 합니다.

4. 렌더링 패스를 수행 합니다.

UI 가상화를 사용 하면 2 단계에서 일반적으로 수행 하는 요소를 만드는 작업은 뷰포트를 채우기 위해 충분 한 내용이 생성 된 것으로 확인 되 면 일찍 지연 되거나 종료 됩니다. 가상화 컨테이너 (예: 하위 프로세스)는이 프로세스를 구동 하기 위해 연결 된 레이아웃으로 지연 됩니다. 가상화 레이아웃에 필요한 추가 정보를 제공 하는 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 와 연결 된 레이아웃을 제공 합니다.

**RealizationRect (즉, 뷰포트)**

Windows에서 스크롤하면 UI 스레드에 대해 비동기적으로 수행 됩니다. 프레임 워크의 레이아웃에 의해 제어 되지 않습니다.  대신 시스템의 compositor에서 상호 작용 및 이동이 발생 합니다. 이 방법의 장점은 60 fps에서 항상 콘텐츠 이동을 수행할 수 있다는 것입니다.  그러나이 문제는 레이아웃에 표시 되는 "뷰포트"가 실제로 화면에 표시 되는 항목을 기준으로 약간 다를 수 있다는 것입니다. 사용자가 빠르게 스크롤하면 UI 스레드 속도가 새 콘텐츠를 생성 하 고 "검은색으로 이동" 하는 속도가 빨라질 수 있습니다. 이러한 이유로, 가상화 레이아웃을 통해 뷰포트 보다 큰 영역을 채울 수 있는 준비 된 요소의 추가 버퍼를 생성 해야 하는 경우가 종종 있습니다. 스크롤 하는 동안 과도 한 부하가 발생 하는 경우에도 사용자에 게 콘텐츠가 표시 됩니다.

![인식 rect](images/xaml-attached-layout-realizationrect.png)

요소를 만드는 데 많은 비용이 들기 때문에 컨테이너 (예: 항목 [리피터](/windows/uwp/design/controls-and-patterns/items-repeater))를 가상화 하면 처음에는 뷰포트에 일치 하는 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 연결 된 레이아웃을 제공 합니다. 유휴 시간에 컨테이너는 점점 더 큰 인식 rect를 사용 하 여 레이아웃을 반복 해 서 호출 하 여 준비 된 콘텐츠의 버퍼를 늘릴 수 있습니다. 이 동작은 빠른 시작 시간과 양호한 패닝 환경 간에 균형을 맞추도록 시도 하는 성능 최적화입니다. [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 및 [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 속성에 의해 제어 되는 속성에 의해 제어 되는 최대 버퍼 크기가 제어 됩니다.

**요소 다시 사용 (재생)**

레이아웃은 크기가 조정 되 고 요소가 실행 될 때마다 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 을 채우도록 요소를 배치 합니다. 기본적으로 [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) 는 각 레이아웃 패스의 끝에서 사용 되지 않은 모든 요소를 재활용 합니다.

[System.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 및 [system.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) 의 일부로 레이아웃에 전달 되는 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 는 가상화 레이아웃 요구 사항에 대 한 추가 정보를 제공 합니다. 가장 일반적으로 사용 되는 항목 중 일부는 다음을 수행 하는 기능입니다.

1. 데이터의 항목 수를 쿼리 합니다 ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. [Getitemat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) 메서드를 사용 하 여 특정 항목을 검색 합니다.
3. 레이아웃이 실현 된 요소로 채울 뷰포트 및 버퍼를 나타내는 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 를 검색 합니다.
4. [Getorcreateelement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드를 사용 하 여 특정 항목에 대 한 UIElement를 요청 합니다.

지정 된 인덱스에 대 한 요소를 요청 하면 해당 요소가 해당 레이아웃 패스에 대해 "사용 중"으로 표시 됩니다. 요소가 아직 없는 경우에는 해당 요소가 인식 되 고 자동으로 사용 준비 됩니다. 예를 들어 DataTemplate에 정의 된 UI 트리를 않아서 하 고 데이터 바인딩을 처리 하는 등의 작업을 수행 합니다.  그렇지 않으면 기존 인스턴스의 풀에서 검색 됩니다.

각 측정값이 끝난 후에는 "사용 중"으로 표시되지 않은 모든 기존 요소를 다시 사용할 수 있는 것으로 간주합니다. 단,[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)를 통해 요소를 검색할 때 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)를 사용 하는 옵션이 사용 되지 않는 경우 프레임 워크는 자동으로이를 재활용 풀로 이동 하 여 사용할 수 있도록 합니다. 이후에 다른 컨테이너에서 사용할 수 있습니다. 요소를 다시 부모로 지정할 때 몇 가지 비용이 발생 하므로 프레임 워크는 가능한 경우이를 방지 하려고 시도 합니다.

가상화 레이아웃을 통해 각 측정 시작 부분에서 요소가 인식 영역에 더 이상 포함 되지 않는 것을 알고 있는 경우 다시 사용을 최적화할 수 있습니다. 프레임 워크의 기본 동작에 의존 하지 않습니다. 레이아웃에서는 [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) 메서드를 사용 하 여 미리 요소를 재생 풀로 이동할 수 있습니다.  새 요소를 요청 하기 전에이 메서드를 호출 하면 레이아웃에서 나중에 요소에 연결 되지 않은 인덱스에 대해 [Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 요청을 실행할 때 기존 요소를 사용할 수 있습니다.

VirtualizingLayoutContext는 레이아웃 작성자가 콘텐츠 종속 레이아웃을 만드는 두 가지 추가 속성을 제공 합니다. 이에 대해서는 나중에 자세히 설명 합니다.

1. 레이아웃에 선택적 _입력_ 을 제공 하는 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 입니다.
2. 레이아웃의 선택적인 _출력_ 인 [layoutorigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 입니다.

## <a name="data-dependent-virtualizing-layouts"></a>데이터 종속 가상화 레이아웃

표시할 콘텐츠를 측정할 필요가 없는 모든 항목의 크기를 알고 있으면 가상화 레이아웃을 사용 하는 것이 더 쉽습니다.  이 문서에서는 일반적으로 데이터를 검사 하는 것을 포함 하므로이 범주의 가상화 레이아웃을 **데이터 레이아웃** 으로 참조 합니다.  데이터를 기반으로 하는 앱은 데이터의 일부 또는 이전에 디자인에 의해 결정 된 것 처럼 알려진 크기의 시각적 표현을 선택할 수 있습니다.

일반적인 방법은 레이아웃에 대 한 것입니다.

1. 모든 항목의 크기와 위치를 계산 합니다.
2. [System.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)의 일부로:
   1. [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 를 사용 하 여 뷰포트 내에 표시 되어야 하는 항목을 결정 합니다.
   2. [Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드를 사용 하 여 항목을 나타내야 하는 UIElement를 검색 합니다.
   3. 미리 계산 된 크기를 사용 하 여 UIElement를 [측정](/uwp/api/windows.ui.xaml.uielement.measure) 합니다.
3. [System.windows.frameworkelement.arrangeoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)의 일환으로 각 실현 된 UIElement를 미리 계산 된 위치로 [정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 합니다.

> [!NOTE]
> 데이터 레이아웃 접근 방식은 종종 _데이터 가상화_와 호환 되지 않습니다.  특히 메모리에 로드 된 데이터만 사용자에 게 표시 되는 데이터를 채우는 데 필요한 데이터입니다.  데이터 가상화는 사용자가 해당 데이터가 상주 하는 위치를 아래로 스크롤할 때 지연 또는 증분 데이터 로드를 참조 하지 않습니다.  대신, 항목이 뷰에서 벗어나 스크롤될 때 메모리에서 항목을 해제할 때를 참조 합니다.  데이터 레이아웃의 일부로 모든 데이터 항목을 검사 하는 데이터 레이아웃이 있으면 데이터 가상화가 예상 대로 작동 하지 않습니다.  예외는 모든 항목의 크기가 동일 하다 고 가정 하는 UniformGridLayout와 같은 레이아웃입니다.

> [!TIP]
> 다양 한 상황에서 다른 사용자가 사용할 수 있는 컨트롤 라이브러리에 대 한 사용자 지정 컨트롤을 만드는 경우 데이터 레이아웃이 옵션이 아닐 수 있습니다.

### <a name="example-xbox-activity-feed-layout"></a>예: Xbox 활동 피드 레이아웃

Xbox 활동 피드에 대 한 UI는 반복 패턴을 사용 합니다. 여기에는 각 줄에 넓은 타일이 오고 그 다음에는 두 개의 좁은 타일이 이어지는 줄에서 반전 됩니다. 이 레이아웃에서 모든 항목의 크기는 데이터 집합에서 항목의 위치와 타일에 대해 알려진 크기 (와이드 vs 좁게)의 함수입니다.

![Xbox 활동 피드](images/xaml-attached-layout-activityfeedscreenshot.png)

아래 코드는 **데이터 레이아웃**에 대해 수행할 수 있는 일반적인 접근 방식을 설명 하기 위해 활동 피드에 대 한 사용자 지정 가상화 UI를 안내 합니다.

<table>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 되어 있는 경우 여기를 클릭 하 여 앱을 열고이 샘플 레이아웃을 사용 하 여 항목 <a href="xamlcontrolsgallery:/item/ItemsRepeater">repeater</a> 에서 작업을 확인 합니다.</p>
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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>필드 UIElement 매핑에 대 한 항목 관리

기본적으로 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 는 인식 되는 요소와 해당 요소가 나타내는 데이터 원본의 인덱스 간 매핑을 유지 관리 합니다.  레이아웃은 기본 자동 재생 동작을 방지 하는 [Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드를 통해 요소를 검색할 때 항상 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 옵션을 요청 하 여이 매핑 자체를 관리 하도록 선택할 수 있습니다.  이 작업을 수행 하도록 선택할 수 있습니다. 예를 들어 스크롤이 한 방향으로 제한 되는 경우에만 사용 되 고, 고려 하는 항목은 항상 연속적으로 사용 됩니다. 즉, 첫 번째 및 마지막 요소의 인덱스를 알고 있으면 팅 해야 하는 모든 요소를 쉽게 파악할 수 있습니다. lized).

#### <a name="example-xbox-activity-feed-measure"></a>예: Xbox 활동 피드 측정값

아래 코드 조각은 매핑을 관리 하기 위해 이전 샘플에서 System.windows.frameworkelement.measureoverride에 추가할 수 있는 추가 논리를 보여 줍니다.

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

항목에 대 한 UI 콘텐츠를 먼저 측정 하 여 정확한 크기를 파악 해야 하는 경우 **콘텐츠 종속 레이아웃**입니다.  항목 크기를 나타내는 레이아웃이 아니라 각 항목의 크기를 조정 해야 하는 레이아웃으로 생각할 수도 있습니다. 이 범주에 포함 되는 레이아웃을 가상화 하는 것이 더 복잡 합니다.

> [!NOTE]
> 콘텐츠 종속 레이아웃은 데이터 가상화를 중단 하지 않습니다.

### <a name="estimations"></a>예측이

내용에 따라 달라 지는 레이아웃은 추정 된 콘텐츠의 크기와 실현 된 콘텐츠의 위치를 모두 추측 하는 데 사용 됩니다. 이러한 예측 값이 변경 되 면 인식 된 콘텐츠가 스크롤할 수 있는 영역 내에서 위치를 정기적으로 이동 합니다. 이로 인해 완화 되지 않은 경우 매우 번거로울 수 있으며 사용자 환경이 jarring 수 있습니다. 잠재적인 문제 및 완화 방법은 여기에 설명 되어 있습니다.

> [!NOTE]
> 모든 항목을 고려 하 고 모든 항목에 대 한 정확한 크기를 파악 하 고 해당 위치를 파악 하는 데이터 레이아웃은 이러한 문제를 전적으로 방지할 수 있습니다.

**스크롤 앵커**

XAML은 스크롤 컨트롤이 [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 인터페이스를 구현 하 여 [스크롤 앵커](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 를 지원 하도록 하 여 갑작스러운 뷰포트 변화를 완화 하는 메커니즘을 제공 합니다. 사용자가 콘텐츠를 조작 하는 경우 스크롤 컨트롤은 추적 되도록 옵트인 된 후보 집합에서 요소를 지속적으로 선택 합니다. 앵커 요소의 위치가 레이아웃 중에 이동 하는 경우 scroll 컨트롤이 뷰포트를 자동으로 이동 하 여 뷰포트를 유지 합니다.

레이아웃에 제공 된 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 의 값에 따라 스크롤 컨트롤에서 선택한 앵커 요소가 현재 선택 되어 있을 수 있습니다. 또는 개발자가 범주 [리피터](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)의 [getorcreateelement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) 메서드를 사용 하 여 인덱스에 대해 요소를 인식 하도록 명시적으로 요청 하는 경우 해당 인덱스는 다음 레이아웃 단계에서 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 로 지정 됩니다. 이렇게 하면 개발자가 요소를 인식 한 다음 [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) 메서드를 통해 뷰로 가져오도록 요청 하는 시나리오에 대해 레이아웃을 준비할 수 있습니다.

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 는 항목의 위치를 예측할 때 콘텐츠 종속 레이아웃이 먼저 배치 해야 하는 데이터 소스 항목의 인덱스입니다. 다른 인식 된 항목의 위치를 지정 하기 위한 시작 지점으로 사용 되어야 합니다.

**스크롤 막대에 미치는 영향**

스크롤 앵커를 사용 하는 경우에도 레이아웃의 예상치는 내용 크기의 상당 부분으로 인해 많은 차이가 있을 경우 스크롤 막대에 대 한 엄지 단추의 위치가 가까이 표시 될 수 있습니다.  엄지 단추를 끌 때 마우스 포인터의 위치를 추적 하는 것이 표시 되지 않는 경우 사용자가 jarring 수 있습니다.

레이아웃의 예측이 정확해 지 면 사용자에 게 스크롤 막대의 스크롤 막대가 표시 되지 않을 수 있습니다.

### <a name="layout-corrections"></a>레이아웃 수정

콘텐츠 종속 레이아웃은 해당 추정치를 현실에 합리화 준비 해야 합니다.  예를 들어 사용자가 콘텐츠의 맨 위로 스크롤하면 레이아웃이 매우 첫 번째 요소를 인식 하는 경우 시작 된 요소를 기준으로 요소의 예상 위치는의 원점 이외의 위치에 표시 될 수 있습니다. (x:0 , y:0). 이 경우 레이아웃은 [layoutorigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 속성을 사용 하 여 새 레이아웃 원점으로 계산 된 위치를 설정할 수 있습니다.  순 결과는 스크롤 컨트롤의 뷰포트가 레이아웃에서 보고 되는 콘텐츠의 위치를 고려 하 여 자동으로 조정 되는 스크롤 앵커와 비슷합니다.

![LayoutOrigin 수정](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>연결 끊김 뷰포트

레이아웃의 [system.windows.frameworkelement.measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 메서드에서 반환 되는 크기는 각 연속 레이아웃으로 변경 될 수 있는 콘텐츠 크기에서 가장 적합 한 추측을 나타냅니다.  사용자가 스크롤하면 레이아웃이 업데이트 된 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)지속적으로 다시 평가 됩니다.

사용자가 엄지 단추를 매우 빠르게 끌면 레이아웃의 관점에서 뷰포트를 사용 하 여 이전 위치가 현재 위치와 겹치지 않는 큰 점프를 만들 수 있습니다.  이는 스크롤의 비동기 특성 때문입니다. 레이아웃을 사용 하는 응용 프로그램 에서도 현재 인식 되지 않으며 레이아웃으로 추적 되는 현재 범위 밖에 서 레이아웃 될 것으로 예상 되는 항목에 대 한 요소 보기를 요청 하는 것이 가능 합니다.

레이아웃이 추측을 검색 하는 것이 잘못 되었거나 예기치 않은 뷰포트 시프트를 발견 한 경우에는 해당 시작 위치를 다시 시작 해야 합니다.  XAML 컨트롤의 일부로 제공 되는 가상화 레이아웃은 표시 되는 콘텐츠의 특성에 대 한 제한이 더 적기 때문에 콘텐츠 종속 레이아웃으로 개발 됩니다.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>예: 가변 크기 항목에 대 한 간단한 가상화 스택 레이아웃

아래 샘플에서는 가변 크기 항목에 대 한 간단한 스택 레이아웃을 보여 줍니다.

* UI 가상화를 지원 합니다.
* 예측을 사용 하 여 해당 항목의 크기를 추정 합니다.
* 는 연속 되지 않은 잠재적 불연속 뷰포트 변화를 인식 합니다.
* 이러한 이동에 대 한 고려 사항에 레이아웃 수정을 적용 합니다.

** 사용: 태그 @ no__t-0

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

** 코드 숨김: 주 .cs @ no__t-0

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

** 코드: VirtualizingStackLayout @ no__t-0

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

## <a name="related-articles"></a>관련 문서

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
