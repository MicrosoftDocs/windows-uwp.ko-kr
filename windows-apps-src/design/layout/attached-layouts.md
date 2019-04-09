---
Description: ItemsRepeater 컨트롤과 같은 컨테이너 사용에 대 한 연결 된 레이아웃을 정의할 수 있습니다.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ff73b13acb5f5970bb79755b0bf5706fb12545a
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913993"
---
# <a name="attached-layouts"></a>연결 된 레이아웃

다른 개체에 레이아웃 논리를 위임 하는 컨테이너 (예: 제어판) 요소 자식에 대 한 레이아웃 동작을 제공 하는 연결 된 레이아웃 개체에 의존 합니다.  UI (예: 테이블의 열에 정렬 되는 행의 항목)의 여러 부분 간에 레이아웃 요소를 보다 쉽게 공유할 또는 연결 된 레이아웃 모델을 런타임 시 항목의 레이아웃을 변경 하려면 응용 프로그램에 대 한 유연성을 제공 합니다.

이 항목에서는 연결 된 레이아웃 (가상화 및 비 가상화), 고 개념을 이해 해야 하는 클래스를 만드는 관련 된 내용을 다룹니다 및 절충 해야 서로 결정할 때 고려해 야 합니다.

| **Windows UI Library 가져오기** |
| - |
| 이 컨트롤은 Windows UI 라이브러리, 새로운 컨트롤 및 UWP 앱 용 UI 기능을 포함 하는 NuGet 패키지의 일부로 포함 합니다. 설치 지침을 비롯 한 자세한 내용은 참조는 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)합니다. |

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

레이아웃을 수행 하려면 모든 요소에 대 한 두 가지 질문에 대답할 수는 필요 합니다.

1. 어떤 ***크기*** 이 요소 수는?

2. 항목은 ***위치*** 이 요소의 수 있습니까?

이러한 질문에 대 한 답을 XAML의 레이아웃 시스템에 대 한 설명의 일부로 간략하게 나옵니다 [사용자 지정 패널](/windows/uwp/design/layout/custom-panels-overview)합니다.

### <a name="containers-and-context"></a>컨테이너 및 컨텍스트

개념적으로 XAML의 [패널](/uwp/api/windows.ui.xaml.controls.panel) 프레임 워크의 두 가지 중요 한 역할을 채웁니다.

1. 자식 요소를 포함할 수 있습니다 하 고 요소 트리의 분기를 소개 합니다.
2. 특정 레이아웃 전략을 해당 자식에 적용 됩니다.

따라서 XAML에서 패널 레이아웃 있지만 엄밀히 말해 동의어 이었습니다 않으면 이상의 레이아웃 합니다.

합니다 [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) 도 동작 UIElement 자식 프로그래밍 방식으로 추가 또는 제거를 허용 하는 Children 속성 패널 같은 하지만 패널 달리를 표시 하지 않습니다.  대신, 자식의 수명 데이터 항목의 컬렉션에 해당 하는 프레임 워크에서 자동으로 관리 됩니다.  패널에서 파생 되지 않으므로, 하지만 작동 하 고 패널 같은 프레임 워크에 의해 처리 됩니다.

> [!NOTE]
> 합니다 [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) 컨테이너에 연결 된 해당 논리를 위임 하는 패널에서 파생 됩니다 [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) 개체입니다.  LayoutPanel 중인 *미리 보기* 되며 현재에 사용할 수 있습니다 합니다 *시험판* WinUI 패키지의 삭제 합니다.

#### <a name="containers"></a>컨테이너

개념상 [패널](/uwp/api/windows.ui.xaml.controls.panel) 픽셀을 렌더링 하는 기능이 있는 요소의 컨테이너인를 [백그라운드](/uwp/api/windows.ui.xaml.controls.panel.background)합니다.  패널에는 사용 하기 쉬운 패키지의 일반적인 레이아웃 논리를 캡슐화 하는 방법을 제공 합니다.

개념이 **레이아웃 연결** 에서는 컨테이너의 두 가지 역할 및 레이아웃을 명확 하 게 구분 합니다.  컨테이너를 다른 개체의 레이아웃 논리를 위임 하는 경우 호출 개체 연결 된 레이아웃 아래 코드 조각에 표시 된 대로 합니다. 상속 되는 컨테이너 [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement), LayoutPanel, 같은 XAML의 레이아웃 프로세스 (예: 높이 및 너비)에 대 한 입력을 제공 하는 공용 속성을 자동으로 노출 합니다.

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

레이아웃 프로세스 컨테이너에 연결 된 의존 *UniformGridLayout* 를 측정 하 여 자식을 정렬 합니다.

#### <a name="per-container-state"></a>컨테이너 당 상태

연결 된 레이아웃을 사용 하 여 레이아웃 개체의 단일 인스턴스를 사용 하 여 연결 가능성이 *많은* 아래 코드 조각에서와 같이 컨테이너; 따라서 해야 하지 종속 또는 직접 호스트 컨테이너를 참조 합니다.  예를 들어 다음과 같은 가치를 제공해야 합니다.

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

이 상황 *ExampleLayout* 레이아웃 계산에 해당 상태 저장 된 요소는 다른 패널에 대 한 레이아웃에 영향을 주지 않으려면 사용 되는 상태를 주의 깊게 고려해 야 합니다.  값에 따라 달라 집니다 인 MeasureOverride 및 ArrangeOverride 논리 사용자 지정 패널에 유사한 것 해당 *정적* 속성입니다.

#### <a name="layoutcontext"></a>LayoutContext

용도 [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) 이러한 과제를 사용 하 여 처리 하는 것입니다.  연결 된 레이아웃 둘 사이 직접적인 종속성을 도입 하지 않고 자식 요소를 검색 하는 등의 호스트 컨테이너와 상호 작용 하는 기능을 제공 합니다. 또한 컨텍스트는 레이아웃을 컨테이너의 자식 요소에 관련 될 수 있는 필요한 상태 저장을 수 있습니다.

간단 하 고 가상화 되지 않은 레이아웃 종종 필요가 없습니다 비-문제를 쉽게 하는 모든 상태를 유지 합니다. 그러나 그리드와 같은 더 복잡 한 레이아웃을 측정값 간의 상태를 유지 관리 하 고 다시 값을 계산 하지 않으려면 호출을 정렬할 수 있습니다.

레이아웃 가상화 *종종* 두 측정값 간의 일부 상태를 유지 관리 하 고 반복적인 레이아웃 전달 간에 정렬 해야 합니다.

#### <a name="initializing-and-uninitializing-per-container-state"></a>초기화 및 초기화 해제 컨테이너 당 상태

레이아웃 컨테이너에 연결 된 경우 해당 [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) 메서드 호출 되 고 상태를 저장 하는 개체를 초기화할 수 있는 기회를 제공 합니다.

마찬가지로, 레이아웃 중에서 제거 되 면 컨테이너는 [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) 메서드가 호출 됩니다.  이렇게 하면 레이아웃 컨테이너를 사용 하 여 연결 해야 하는 모든 상태를 정리할 수 있습니다.

레이아웃의 상태 개체를 사용 하 여 저장 하 고 사용 하 여 컨테이너에서 검색할 수는 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 컨텍스트의 속성입니다.

### <a name="ui-virtualization"></a>UI 가상화

UI 가상화 될 때까지 UI 개체의 생성을 지연 의미 _필요할 때_합니다.  이 성능 최적화.  스크롤할 수 없는 시나리오 결정 _필요할 때_ 많은 앱 관련 된 작업에 기반 할 수 있습니다.  이러한 경우 앱 사용을 고려해 야 합니다 [x: 로드](../../xaml-platform/x-load-attribute.md)합니다. 레이아웃에서 특수 한 처리는 필요 하지 않습니다.

목록 등 스크롤 기반 시나리오에서 결정 _필요할 때_ 대 "는 것을 사용자에 게 표시"는 특별 한 고려 하며 레이아웃 과정 배치 된 위치에 따라 달라 집니다.  이 시나리오는이 문서에 대 한 포커스입니다.

> [!NOTE]
> 이 문서에 포함 되지 않지만 스크롤할 수 없는 시나리오에서 시나리오를 스크롤 UI 가상화를 사용 하도록 설정 하는 동일한 기능을 적용할 수 있습니다.  예를 들어, 데이터 기반 도구 모음 컨트롤을 소개 하며 재활용 / 표시 영역을 오버플로 메뉴로 사이의 요소를 이동 하 여 사용 가능한 공간 변경 내용에 응답 명령의 수명을 관리 하는 합니다.

## <a name="getting-started"></a>시작하기

먼저 만들려는 레이아웃 UI 가상화를 지원 하는지 여부를 결정 합니다.

**몇 가지 주의 해야 하는 중...**

1. 비 가상화 레이아웃은 작성자에 게 쉽게 합니다. 항목의 수를 항상 작은 수 없는 경우 다음 가상화 되지 않은 레이아웃을 작성 것이 좋습니다.
2. 플랫폼을 사용 하는 연결 된 레이아웃의 집합을 제공 합니다 [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) 하 고 [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) 일반적인 요구 사항에 맞게 합니다.  와 기능을 살펴보고 사용자 지정 레이아웃을 정의 해야 결정 하기 전에 합니다.
3. 가상화 레이아웃에는 항상 추가 CPU 및 메모리 비용/복잡성/오버 헤드는 가상화 되지 않은 레이아웃에 비해 몇 가지 있습니다.  일반적인 경험상 자식을 레이아웃 관리 해야 하는 경우 영역에서에 맞는 것 만큼 뷰포트 크기 x 3는 다음 가상화 레이아웃에서 훨씬 향상 수 있습니다. 3 x 크기가이 문서 뒷부분에서 자세히 설명 되어 이지만 Windows 가상화에 미치는 영향에 스크롤의 비동기 특성 때문입니다.

> [!TIP]
> 기본 설정에 대 한 참조 지점으로의 [ListView](/uwp/api/windows.ui.xaml.controls.listview) (및 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater))는 현재 뷰포트 크기 x 3을 입력 하기에 충분 항목 수가 될 때까지 시작 되지 않으면 재활용 됩니다.

**기본 형식 선택**

![연결 된 레이아웃 계층](images/xaml-attached-layout-hierarchy.png)

기본 [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layout) 형식은 연결 된 레이아웃을 작성 하기 위한 시작 지점으로 사용 되는 두 개의 파생된 형식:

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>가상화 되지 않은 레이아웃

만들어진 모든 사용자에 게 익숙하게 느껴질 것 이외의 가상화 레이아웃 만들기에 대 한 접근 방식은 [사용자 지정 패널](/windows/uwp/design/layout/custom-panels-overview)합니다.  동일한 개념이 적용 됩니다.  기본 차이점은는 [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) 는 액세스 하는 [자식](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) 컬렉션 및 레이아웃 상태를 저장 하도록 선택할 수.

1. 기본 형식에서 파생 [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (대신 패널).
2. *(선택 사항)*  종속성 속성을 정의할 때 변경 됩니다 레이아웃이 무효화 합니다.
3. _(**새로 만들기**선택적 /)_ 의 일부로 레이아웃에서 필요한 모든 상태 개체를 초기화 합니다 [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)합니다. 호스트 컨테이너를 스 태 시를 사용 하 여 합니다 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 컨텍스트를 사용 하 여 제공 합니다.
4. 재정의 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) 호출을 [측정값](/uwp/api/windows.ui.xaml.uielement.measure) 모든 자식 메서드.
5. 재정의 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) 호출을 [정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 모든 자식 메서드.
6. *(**새로 만들기**/선택 사항)* 의 일부로 저장된 된 상태를 정리 합니다 [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)합니다.

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>예: 간단한 스택 레이아웃 (다양 한 크기의 항목)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

다음은 다양 한 크기의 항목의 기본적인 아닌 가상화 스택 레이아웃입니다. 이 레이아웃의 동작을 조정 하려면 모든 속성에 부족 합니다. 아래 구현은 레이아웃 컨테이너에 의해 제공 되는 컨텍스트 개체에 의존 하는 방법을 보여 줍니다.

1. 하위 항목의 수를 수집 하 고
2. 인덱스에서 각 자식 요소에 액세스 합니다.

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

가상화 되지 않은 레이아웃 마찬가지로 레이아웃의 가상화 단계로 동일합니다.  요소 뷰포트 안에 및 실현할 수 해야 결정 대부분의 복잡성도.

1. 기본 형식에서 파생 [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)합니다.
2. (선택 사항) 종속성 속성을 정의할 때 변경 됩니다 레이아웃이 무효화 합니다.
3. 일부로 레이아웃에서 필요한 모든 상태 개체를 초기화 합니다 [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)합니다. 호스트 컨테이너를 스 태 시를 사용 하 여 합니다 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 컨텍스트를 사용 하 여 제공 합니다.
4. 재정의 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 호출을 [측정값](/uwp/api/windows.ui.xaml.uielement.measure) 실현 해야 하는 각 자식에 대 한 메서드.
   1. 합니다 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드 (예: 데이터 바인딩 적용) 프레임 워크에 의해 준비 된 UIElement 사용 됩니다.
5. 재정의 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) 호출을 [정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 실현된 각 자식에 대 한 메서드.
6. (선택 사항) 모든 정리의 일부로 상태를 저장 합니다 [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)합니다.

> [!TIP]
> 값을 반환 합니다 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) 가상화 된 콘텐츠의 크기로 사용 되는 합니다.

두 가지 일반 가상화 레이아웃을 작성할 때 고려해 야 합니다.  주로 다른 중 하나를 선택할 것인지는 "는 확인 방법을 요소의 크기"에 따라 다릅니다.  최종 크기를 결정 하는 데이터 집합 또는 데이터 자체에 있는 항목의 인덱스를 찾으려면 해당 충분 경우 생각 됩니다 **데이터 종속**합니다.  이들은 더 간단 하 게 만듭니다.  그러나 항목을 만들고 말할는 다음 UI를 측정 하는 것에 대 한 크기를 결정 하는 유일한 방법은 이면 **콘텐츠 종속**합니다.  이러한 옵션은 더 복잡 합니다.

### <a name="the-layout-process"></a>레이아웃 프로세스

데이터 또는 종속 콘텐츠 레이아웃을 만드는 인지 레이아웃 프로세스 및 Windows의 비동기 스크롤의 영향을 이해 해야 합니다.

(초과)에서 시작 화면에서 UI를 표시 하기 위해 프레임 워크에서 수행 된 단계의 단순화 된 보기는:

1. 태그를 구문 분석 합니다.

2. 요소 트리를 생성합니다.

3. 레이아웃 단계를 수행합니다.

4. 렌더링 단계를 수행합니다.

UI 가상화를 사용 하 여 2 단계에서가 정상적으로 수행 하는 요소를 만들기를 지연 시키거나 일찍 종료 되 면 해당 확인 된 충분 한 콘텐츠 만들어져 뷰포트를 입력 합니다. 이 프로세스를 드라이브에 연결 된 레이아웃 (예: ItemsRepeater)는 가상화 컨테이너 설정을 따릅니다. 사용 하 여 연결 된 레이아웃을 제공 하는 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 레이아웃을 가상화 해야 하는 추가 정보를 표시 하는 합니다.

**RealizationRect (즉, 뷰포트)**

Windows에서 스크롤할 발생을 UI 스레드에 비동기 합니다. 프레임 워크의 레이아웃으로 제어 되지 않습니다.  대신, 상호 작용 및 이동 시스템의 작성자에 발생 합니다. 이 방식의 장점은 콘텐츠 이동을 항상 작업은 60 fps 한다는 점입니다.  문제 인데, "뷰포트" 레이아웃에 표시 되는 화면에 실제로 표시 되는 사항을 기준으로 약간 오래 된 수 있습니다. 신속 하 게를 사용 하는 경우 새로운 콘텐츠 및 "검정색으로 이동"을 생성 하는 UI 스레드의 속도 최적값 수 있습니다. 이러한 이유로 가상화 레이아웃의 뷰포트 보다 큰 영역을 채우는 충분 한 준비 요소의 추가 버퍼를 생성 하는 데 필요한 경우가 있습니다. 사용자를 스크롤 하는 동안 더 많은 부하 상태에서 표시 되 면 아직 콘텐츠를 사용 하 여 합니다.

![인식 rect](images/xaml-attached-layout-realizationrect.png)

요소 생성 비용이 많이 드는 이므로 컨테이너를 가상화 (예: [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater))와 연결 된 레이아웃 하면 처음에 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 뷰포트와 일치 하는 합니다. 유휴 시간에 컨테이너 증가할 수 준비 된 콘텐츠 버퍼를 점점 더 큰 인식 사각형을 사용 하 여 레이아웃에 대 한 반복된 호출 하 여 이 동작은을 빠른 시작 시간 및 우수한 이동 환경을 간의 균형을 이루 려 고 하는 성능 최적화에 설명 합니다. ItemsRepeater 생성 하는 최대 버퍼 크기에 의해 제어 됩니다 해당 [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 하 고 [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 속성입니다.

**다시 사용 하 여 요소 (재생)**

레이아웃 크기에 맞게 요소 위치를 예상 되는 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 실행 될 때마다 합니다. 기본적으로는 [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) 각 레이아웃 단계 끝에 사용 되지 않는 모든 요소를 재활용 합니다.

[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 일환으로 레이아웃에 전달 되는 합니다 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 및 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) 추가 정보를 제공는 가상화 레이아웃 해야합니다. 자주 사용 하는 작업의 수는 다음과 같습니다.

1. 데이터의 항목 수를 쿼리 ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. 특정 사용 하 여 항목을 검색 합니다 [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) 메서드.
3. 검색을 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 뷰포트를 나타내는 및 레이아웃을 사용 하 여 채워야 하는 버퍼에 요소 실현 합니다.
4. 사용 하 여 특정 항목에 대 한 UIElement를 요청 합니다 [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드.

지정된 된 인덱스의 요소를 요청 레이아웃의 성공에 대 한 "사용 중"으로 표시 된 해당 요소의 발생 합니다. 요소가 아직 없는 경우 다음 수 실현 하 고 자동으로 (모든 데이터 바인딩, 등을 처리 하는 datatemplate에서 정의 UI 트리 값 예:.)는 사용할 수 있도록 준비 합니다.  그렇지 않으면 기존 인스턴스의 풀에서 검색 됩니다.

"사용"에 표시 되지 않은 요소 자동으로 다시 사용할 수 있는 것으로 간주 됩니다 실현 모든 기존 각 측정 단계 끝 하지 않는 한 옵션을 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 요소를 통해 검색 된 때 사용한 합니다 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드. 프레임 워크는 자동으로 재활용 풀으로 이동 하 고 사용할 수 있도록 합니다. 이 수 이후에 가져와서 사용에 대 한 다른 컨테이너에서. 이 방지 하려면 가능한 경우 재지정 다음 요소와 관련 된 일부 비용이 없기 때문에 프레임 워크를 가져오려고 시도 합니다.

가상화 레이아웃 요소는 더 이상 인식 rect 속하는 각 측정값의 시작 부분에 알고 해당 다시 사용 하 여 최적화할 수 것입니다. 프레임 워크의 기본 동작에 의존 합니다. 레이아웃 우선적으로 이동할 수 요소 재활용 풀을 사용 하 여 합니다 [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) 메서드.  새 요소를 요청 하기 전에이 메서드를 호출 하면 해당 기존 요소가 레이아웃 나중에 실행 하면 사용할 수는 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 요소와 연결 되어 있지는 인덱스에 대 한 요청입니다.

VirtualizingLayoutContext 종속 콘텐츠 레이아웃 만들기 레이아웃 제작자를 위한 두 가지 추가 속성을 제공 합니다. 설명 자세히 나중입니다.

1. A [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 제공 하는 선택적인 _입력_ 레이아웃 합니다.
2. A [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 즉 선택적 _출력_ 레이아웃입니다.

## <a name="data-dependent-virtualizing-layouts"></a>데이터 종속 가상화 레이아웃

모든 항목의 크기를 측정 하는 콘텐츠를 표시 하지 않고도 이어야 합니다 하는 것이 알고 있는 경우 가상화 레이아웃이 쉽습니다.  이 문서에 간단히 부르는이 범주의 레이아웃으로 가상화 **데이터 레이아웃** 있으므로 만들어져야 하므로, 일반적으로 데이터를 검사 합니다.  앱을 선택할 수는 알려진 크기-시각적 아마도 때문에 데이터를 기반으로 데이터의 해당 부분 또는 디자인을 기준으로 이전 했습니다.

일반적인 방법은 레이아웃을입니다.

1. 크기 및 모든 항목의 위치를 계산 합니다.
2. 일부로 합니다 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. 사용 된 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 뷰포트 내에 항목이 표시 됩니다 확인 하려면.
   2. 항목과 나타내야 하는 ui 요소를 검색 합니다 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 메서드.
   3. [측정값](/uwp/api/windows.ui.xaml.uielement.measure) 미리 계산 된 크기를 사용 하 여 UIElement입니다.
3. 일부로 합니다 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)를 [정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 미리 계산 된 위치가 포함 된 UIElement를 실현 각각.

> [!NOTE]
> 데이터 레이아웃 방식을 자주 사용 하 여 호환 되지 않습니다 _데이터 가상화_합니다.  특히 데이터만 메모리에 로드 하는 위치는 사용자에 게 표시 되는 사항을 입력 하는 데 필요한 해당 데이터입니다.  사용자는 데이터 상주 남아 있는 아래로 스크롤하면 데이터 지연 또는 증분 로드를 데이터 가상화 참조 되지 않습니다.  대신,이 참조 하는 항목 보기에서 벗어나 스크롤 하는 것 처럼 메모리에서 해제 된 경우.  데이터 레이아웃 예상 대로 데이터 레이아웃의 일부 작업에서 데이터 가상화를 방해 하는 대로 모든 데이터 항목을 검사 하는 것입니다.  예외는 모두에 같은 크기를 가정 UniformGridLayout 같은 레이아웃.

> [!TIP]
> 다양 한 상황에서에서 다른 용도로 사용 될 컨트롤 라이브러리에 대 한 사용자 지정 컨트롤을 만드는 경우 데이터 레이아웃에는 선택할 없습니다 수 있습니다.

### <a name="example-xbox-activity-feed-layout"></a>예: 레이아웃 Xbox 작업 피드

Xbox 활동 피드에 UI 각 줄에 와이드 타일을 반전 하는 좁은 두 개의 타일 뒤에 다음 줄에서 반복 패턴을 사용 합니다. 이 레이아웃에서 모든 항목에 대 한 크기가 함수 (와이드 vs 좁힐) 데이터 집합 및 타일에 대 한 알려진된 크기의 항목 위치입니다.

![Xbox 작업 피드](images/xaml-attached-layout-activityfeedscreenshot.png)

무엇을 사용자 지정 가상화 피드 작업에 대 한 UI 설명 하는 것은 일반 방법 안내 아래 코드에 대해 수행할 수 있습니다는 **데이터 레이아웃**합니다.

<table>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 앱을 열고 확인 합니다 <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> 이 샘플 레이아웃을 사용 하 여 실행 중인 합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(선택 사항) UIElement 매핑 항목 관리

기본적으로 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 표시 요소를 나타내는 데이터 원본의 인덱스 사이의 매핑을 유지 관리 합니다.  레이아웃을이 매핑을 자체 항상 하는 옵션을 요청 하 여 관리 하도록 선택할 수 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 를 통해 요소를 검색할 때 합니다 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 기본값을 방지 하는 메서드 자동 재활용 동작입니다.  스크롤 하는 것은 한 방향으로 제한 한 것으로 간주 하는 항목 (즉, 알고 있으면 첫 번째 및 마지막 요소의 인덱스에는 충분 팅 해야 하는 모든 요소를 정확히 알고 연속은 항상 하는 경우에 사용 됩니다 하는 경우 이렇게 하려면 예를 들어 레이아웃을 선택할 수 있습니다. lized)입니다.

#### <a name="example-xbox-activity-feed-measure"></a>예: 측정값 Xbox 작업 피드

아래 코드 조각은 이전 샘플에서 매핑을 관리 하는 MeasureOverride 추가할 수 있는 추가 논리를 보여 줍니다.

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

## <a name="content-dependent-virtualizing-layouts"></a>종속 콘텐츠 가상화 레이아웃

정확한 크기를 파악 하는 항목에 대 한 UI 콘텐츠를 먼저 측정 해야 경우 것을 **종속 콘텐츠 레이아웃**합니다.  또한 해당 레이아웃 항목 크기를 알리는 것이 아니라 각 항목 자체를 조정 해야 합니다는 레이아웃으로 생각할 수 있습니다. 이 범주에 속하는 가상화 레이아웃은 더 복잡 합니다.

> [!NOTE]
> 종속 콘텐츠 레이아웃 안 함 ()를 중단 하지 않아야 데이터 가상화 합니다.

### <a name="estimations"></a>예측

종속 콘텐츠 레이아웃 표시 되지 않은 콘텐츠의 크기와 실현 콘텐츠의 위치를 추측 하는 예측에 의존 합니다. 이러한 예측이 변경 될 때 정기적으로 스크롤 가능한 영역 내에서 위치를 이동 하려면 실현된 콘텐츠를 발생 합니다. 이 매우 불편 하 고 jarring 사용자 경험을 발생 시킬 없으면 완화 합니다. 잠재적인 문제 및 완화 방법 여기에 설명 되어 있습니다.

> [!NOTE]
> 모든 항목을 고려 하 고 정확한 크기를 실현 하는 모든 항목 및 해당 위치를 파악 하는 데이터 레이아웃 전적으로 이러한 문제를 방지할 수 있습니다.

**스크롤 고정**

스크롤 컨트롤 지원 함으로써 갑자기 뷰포트 shifts 완화 하는 메커니즘을 제공 하는 XAML [스크롤 고정](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 구현 하 여 합니다 [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 인터페이스입니다. 사용자가 콘텐츠를 조작 하는 대로 스크롤 컨트롤 추적 하도록 옵트인 된 후보 집합에서 요소를 지속적으로 선택 합니다. 앵커 요소의 위치를 레이아웃 하는 동안 이동 하는 경우 scroll 컨트롤은 자동으로 뷰포트 유지 되도록 뷰포트를 이동 합니다.

값을 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 제공 레이아웃 스크롤 컨트롤에서 선택 하는 현재 선택 된 앵커 요소를 반영할 수 있습니다. 또는 개발자는 요소를 사용 하 여 인덱스에 대 한 실현할 수는 명시적으로 요청 하는 경우는 [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) 메서드는 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), 해당 인덱스도 제공 됩니다는 [ RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 다음 레이아웃 패스에서. 이 통해 개발자는 요소를 인식 하 고 이후에 통해 뷰로 상태로 만들 수를 요청 가능성이 높은 시나리오에 대 한 준비가 레이아웃 합니다 [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) 메서드.

합니다 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 해당 항목의 위치를 추정 하는 경우 종속 콘텐츠 레이아웃을 먼저 배치 해야 하는 데이터 원본에 있는 항목에 대 한 인덱스입니다. 다른 표시 항목 위치 지정에 대 한 시작 지점으로 서비스를 제공 해야 합니다.

**스크롤 막대에 미치는 영향**

스크롤 고정 된 경우에 레이아웃의 예상 달라 지 면 많은, 아마도 때문에 상당한 변형이 콘텐츠의 크기를 스크롤 막대에 대 한 위치 조정 컨트롤의 위치가 잘못 된 경우에 나타날 수 있습니다.  Thumb 끌어 하는 경우 마우스 포인터의 위치를 추적 하지 않습니다. 사용자에 대 한 jarring이 될 수 있습니다.

더 정확한 레이아웃 해당 예측에 있을 수 있습니다 다음 사용자가 높습니다는 파일로 이동 하는 스크롤 막대의 엄지 단추 표시 됩니다.

### <a name="layout-corrections"></a>레이아웃 수정

종속 콘텐츠 레이아웃을 실제로 사용 하 여 해당 예측을 합리화 하도록 준비 해야 합니다.  예를 들어 사용자를 스크롤하면 매우 첫 번째 요소를 인식 하는 콘텐츠 및 레이아웃의 위쪽, 원점 (x: 0 이외의 어딘가에 나타나야 하는 시작 요소를 기준으로 요소의 예상된 위치 발생할 것을 찾을 수합니다 있습니다. y:0). 이 문제가 발생 하는 경우 레이아웃을 사용할 수는 [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 속성을 새 레이아웃 원점으로 해당 계산의 위치를 설정 합니다.  결과는 스크롤 컨트롤의 뷰포트 자동으로 조정 되어 콘텐츠 위치에 대 한 계정 레이아웃에서 보고 한 대로 고정 스크롤 하는 것과 비슷합니다.

![LayoutOrigin 수정](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>연결이 끊긴된 뷰포트

크기를 레이아웃에서 반환 된 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 메서드는 각 연속 레이아웃을 사용 하 여 변경 될 수 있는 콘텐츠의 크기에서 최상의 추측을 나타냅니다.  레이아웃 업데이트를 사용 하 여 지속적으로 재평가 됩니다 사용자가 스크롤하면 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)합니다.

사용자가 thumb 매우 신속 하 게 하는 경우에 크게 표시 레이아웃의 관점에서 뷰포트의 수 이전 위치에 현재 위치에 겹치는 하지 이동 후 합니다.  스크롤의 비동기 특성 때문입니다. 레이아웃 요소 현재 실현 되지 않은 및 범위 밖에 있는 경우 현재 레이아웃에서 추적을 레이아웃 예상 되는 항목에 대 한 보기에 맞게 수정할 수는 요청을 사용 하는 앱도 가능 합니다.

레이아웃에서 해당 추측 올바르지 및/또는 예기치 않은 뷰포트 시프트를 표시를 검색 하는 경우 해당 시작 위치를 회전 해야 합니다.  표시 되는 콘텐츠의 특성에 대 한 제한이 배치 하는 대로 XAML 컨트롤의 일부로 제공 되는 가상화 레이아웃 종속 콘텐츠 레이아웃으로 개발 됩니다.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>예: 가변 크기의 항목에 대 한 간단한 가상화 스택 레이아웃

아래 샘플에서는 가변 크기의 항목에 대 한 간단한 스택 레이아웃을 보여 줍니다.

* UI 가상화를 지원합니다.
* 예상치를 사용 하 여 표시 되지 않은 항목의 크기를 추측 합니다.
* 잠재적인 비연속적 뷰포트 시프트 인식 하 고
* 이러한 이동에 대 한 계정 레이아웃 수정에 적용 됩니다.

**사용량: 마크업**

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

**코드 숨김 파일: Main.cs**

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

## <a name="related-articles"></a>관련 문서

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
