---
description: 이동과 스크롤을 통해 사용자는 화면 경계를 벗어나 확장된 콘텐츠에 액세스할 수 있습니다.
title: 스크롤 뷰어 컨트롤
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scrollbars
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: Abarlow, pagildea
design-contact: ksulliv
dev-contact: regisb
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f0bd0fa72587d83e9dca28b18688f1c667033070
ms.sourcegitcommit: c5fdcc0779d4b657669948a4eda32ca3ccc7889b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102784854"
---
# <a name="scroll-viewer-controls"></a>스크롤 뷰어 컨트롤



한 영역에 모두 표시할 수 없을 정도로 UI 콘텐츠가 너무 많은 경우 스크롤 뷰어 컨트롤을 사용합니다.

> **중요 API**: [ScrollViewer 클래스](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [ScrollBar 클래스](/uwp/api/windows.ui.xaml.controls.primitives.scrollbar)

스크롤 뷰어를 사용하면 콘텐츠가 뷰포트(표시 영역)의 경계를 넘어 확장될 수 있습니다. 사용자는 마우스 또는 펜 커서를 사용하여 스크롤 뷰어의 스크롤 막대를 조작하거나 터치, 마우스 휠, 키보드 또는 게임 패드를 통해 스크롤 뷰어 표면을 조작하여 이 콘텐츠에 도달합니다. 이 이미지는 스크롤 뷰어 컨트롤의 몇 가지 예를 보여 줍니다.

![표준 스크롤 막대 컨트롤을 보여 주는 스크린샷](images/ScrollBar_Standard.jpg)

스크롤 뷰어의 스크롤 막대는 다음 그림과 같이 상황에 따라 이동 표시기(왼쪽)와 기본 스크롤 막대(오른쪽)라는 두 가지 시각화를 사용합니다.

![표준 스크롤 막대 및 이동 표시기 컨트롤의 모양 샘플](images/SCROLLBAR.png)

스크롤 뷰어는 사용자의 입력 방식을 인식하고, 표시할 시각화를 결정하는 데 사용합니다.

* 스크롤 막대를 직접 조작하지 않고 터치 등으로 영역을 스크롤하는 경우 현재 스크롤 위치 표시와 함께 이동 표시기가 나타납니다.
* 마우스나 펜 커서가 이동 표시기 위로 이동하면 이동 표시기가 기존 스크롤 막대로 변형됩니다.  스크롤 막대 위치 조정 컨트롤을 끌어 스크롤 영역을 조작합니다.

<!--
<div class="microsoft-internal-note">
See complete redlines in [UNI]http://uni/DesignDepot.FrontEnd/#/ProductNav/3378/0/dv/?t=Windows|Controls|ScrollControls&f=RS2
</div>
-->

![스크롤 막대의 기능](images/conscious-scroll.gif)

> [!NOTE]
> 스크롤 막대는 ScrollViewer 내의 콘텐츠 맨 위에 16px로 오버레이되어 표시됩니다. 훌륭한 UX 디자인을 만들려면 스크롤 막대가 오버레이되어 대화형 콘텐츠가 잘 보이지 않는 경우가 없도록 합니다. 또한 UX가 겹치지 않도록 하려면 뷰포트의 가장자리에 스크롤 막대를 위한 16px 안쪽 여백을 남겨 둡니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/ScrollViewer">앱을 열고 ScrollViewer의 기능을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-scroll-viewer"></a>스크롤 뷰어 만들기

페이지에 세로 스크롤을 추가하려면 스크롤 뷰어에 페이지 콘텐츠를 래핑하세요.

```xaml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1">

    <ScrollViewer>
        <StackPanel>
            <TextBlock Text="My Page Title" Style="{StaticResource TitleTextBlockStyle}"/>
            <!-- more page content -->
        </StackPanel>
    </ScrollViewer>
</Page>
```

이 XAML은 수평 스크롤을 사용하도록 설정하고, 스크롤 뷰어에 이미지를 배치하며, 확대/축소를 사용하도록 설정하는 방법을 보여 줍니다.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## <a name="scrollviewer-in-a-control-template"></a>컨트롤 템플릿의 ScrollViewer

ScrollViewer 컨트롤은 다른 컨트롤의 복합 파트로 사용되는 것이 일반적입니다. ScrollViewer 파트는 호스트 컨트롤의 레이아웃 공간이 확장된 콘텐츠 크기보다 더 작게 제한되는 경우에만 지원용 [ScrollContentPresenter](/uwp/api/Windows.UI.Xaml.Controls.ScrollContentPresenter) 클래스와 더불어 스크롤 막대와 함께 뷰포트를 표시합니다. 종종 목록에도 이러한 경우가 발생하므로 [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) 템플릿에는 항상 ScrollViewer가 포함됩니다. [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 및 [RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)의 템플릿에도 ScrollViewer가 포함됩니다.

**ScrollViewer** 파트가 컨트롤에 있는 경우 대체로 호스트 컨트롤에 콘텐츠를 스크롤할 수 있게 하는 특정 입력 이벤트 및 조작에 대한 기본 제공 이벤트 처리가 있습니다. 예를 들어 GridView가 살짝 밀기 제스처를 해석하고 이로 인해 콘텐츠가 가로로 스크롤됩니다. 호스트 컨트롤이 수신하는 입력 이벤트와 원시 조작은 컨트롤에 의해 처리된 것으로 간주되며 [PointerPressed](/uwp/api/windows.ui.xaml.uielement.pointerpressed)와 같은 하위 수준 이벤트가 발생하지 않고 부모 컨테이너로 버블링되지도 않습니다. 컨트롤 클래스 및 이벤트에 대한 **On**_Event_ 가상 메서드를 재정의하거나 컨트롤을 다시 템플릿으로 작성하여 기본 제공 컨트롤 처리의 일부를 변경할 수 있습니다. 하지만 두 경우 모두 일반적으로 컨트롤이 이벤트와 사용자의 입력 동작 및 제스처에 예상된 방식으로 반응하도록 하는 원래 기본 동작을 재현하는 것은 쉽지 않습니다. 따라서 해당 입력 이벤트를 반드시 발생해야 하는지 여부를 고려해야 합니다. 컨트롤에 의해 처리되지 않는 다른 입력 이벤트 또는 제스처가 있는지 조사하고 앱 또는 컨트롤 조작 디자인에 사용하는 것이 좋습니다.

ScrollViewer를 포함하는 컨트롤이 ScrollViewer 파트 내에 있는 일부 동작과 속성에 영향을 줄 수 있도록 ScrollViewer는 스타일에서 설정하고 템플릿 바인딩에 사용할 수 있는 많은 XAML 연결 속성을 정의합니다. 연결된 속성에 대한 자세한 내용은 [연결된 속성 개요](../../xaml-platform/attached-properties-overview.md)를 참조하세요.

**ScrollViewer XAML 연결 속성**

ScrollViewer는 다음과 같은 XAML 연결 속성을 정의합니다.

- [ScrollViewer.BringIntoViewOnFocusChange](/uwp/api/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange)
- [ScrollViewer.HorizontalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility)
- [ScrollViewer.HorizontalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode)
- [ScrollViewer.IsDeferredScrollingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled)
- [ScrollViewer.IsHorizontalRailEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled)
- [ScrollViewer.IsScrollInertiaEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled)
- [ScrollViewer.IsVerticalRailEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled)
- [ScrollViewer.IsVerticalScrollChainingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled)
- [ScrollViewer.IsZoomChainingEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled)
- [ScrollViewer.IsZoomInertiaEnabled](/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled)
- [ScrollViewer.VerticalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty)
- [ScrollViewer.VerticalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode)
- [ScrollViewer.ZoomMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode)

이러한 XAML 연결 속성은 ScrollViewer가 암시적이며(예: ScrollViewer가 ListView 또는 GridView의 기본 템플릿에 있는 경우) 템플릿 파트에 액세스하지 않고 컨트롤의 스크롤 동작에 영향을 주려는 경우에 사용됩니다.

예를 들어 ListView의 기본 제공 스크롤 뷰어에 세로 스크롤 막대가 항상 표시되도록 하는 방법은 다음과 같습니다.

```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

예제 코드와 같이 ScrollViewer가 XAML에서 명시적인 경우에는 연결된 속성 구문을 사용할 필요가 없습니다. 특성 구문을 사용하면 됩니다(예: `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`).


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 가능하면 가로가 아닌 세로 스크롤로 디자인합니다.
- 하나의 뷰포트 경계(세로 또는 가로)를 넘어가는 콘텐츠 영역에는 단일 축 이동을 사용합니다. 두 뷰포트 경계(세로 또는 가로)를 모두 넘어가는 콘텐츠 영역에는 2축 이동을 사용합니다.
- 목록 보기, 그리드 보기, 콤보 상자, 목록 상자, 텍스트 입력 상자 및 허브 컨트롤에서 기본 제공 스크롤 기능을 사용합니다. 이러한 컨트롤을 사용하면 항목이 너무 많아 한 번에 모두 표시할 수 없는 경우 사용자가 항목 목록을 가로질러 가로로 또는 세로로 스크롤할 수 있습니다.
- 사용자가 화면에 맞게 크기 조정된 이미지가 아니라 전체 크기 이미지를 가로질러 이동 및 확대/축소할 수 있게 하려는 경우와 같이 사용자가 더 큰 영역에서 양방향으로 이동하고 확대/축소할 수도 있게 하려면 스크롤 뷰어 내부에 이미지를 배치합니다.
- 사용자가 긴 텍스트 줄을 스크롤할 경우 세로로만 스크롤하도록 스크롤 뷰어를 구성합니다.
- 스크롤 뷰어를 사용하여 개체를 하나만 포함합니다. 하나의 개체는 레이아웃 패널이 될 수 있고, 여기에는 고유한 개체가 제한 없이 포함됩니다.
- ScrollViewer 또는 ListView와 같이 스크롤할 수 있는 보기에서 [UIElement](/uwp/api/Windows.UI.Xaml.UIElement)에 대한 포인터 이벤트를 처리해야 하는 경우에는 [UIElement.CancelDirectmanipulation()](/uwp/api/windows.ui.xaml.uielement.canceldirectmanipulations)을 호출하여 보기의 요소에 대한 조작 이벤트 지원을 명시적으로 해제해야 합니다. 보기에서 조작 이벤트를 다시 사용하도록 설정하려면 [UIElement.TryStartDirectManipulation()](/uwp/api/windows.ui.xaml.uielement.trystartdirectmanipulation)을 호출합니다.



## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-topics"></a>관련 항목

**개발자용(XAML)**

* [ScrollViewer 클래스](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)
