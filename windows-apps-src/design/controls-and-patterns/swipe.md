---
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
Description: 살짝 밀기 명령은 상황에 맞는 메뉴를 위한 터치 가속기입니다.
title: 살짝 밀기
label: Swipe
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 315edbddccc51b7e742bf9beffad8497a104ce03
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80614092"
---
# <a name="swipe"></a>살짝 밀기

살짝 밀기 명령은 사용자가 앱 내에서 상태를 변경하지 않고도 공통 메뉴 작업에 터치로 쉽게 액세스할 수 있는 상황에 맞는 메뉴의 가속기입니다.

> **Windows UI 라이브러리 API**: [SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem)
>
> **플랫폼 API**: [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [ListView 클래스](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![실행 및 표시등 테마](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

살짝 밀기 명령을 사용하면 공간이 절약됩니다. 여러 항목에서 연속적으로 사용자 동일한 작업을 수행할 수 있는 경우에 유용한 명령입니다. 또한 페이지 내에서 전체 팝업이나 상태 변경이 필요하지 않은 항목에 대해 "바로 가기"를 제공합니다.

항목 그룹의 크기가 클 때는 살짝 밀기 명령을 사용해야 합니다. 각 항목에는 사용자가 정기적으로 수행하고 싶어하는 작업이 1 ~ 3개 포함되어 있습니다. 이러한 작업은 다음을 포함하지만 이에 국한되지 않습니다.

- 삭제
- 표시 또는 보관
- 저장 또는 다운로드
- 회신

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/SwipeControl">앱을 열고 작동 중인 SwipeControl을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>동영상 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>살짝 밀기는 어떻게 작동합니까?

UWP 살짝 밀기 명령에는 두 가지 모드 즉, [표시](/uwp/api/windows.ui.xaml.controls.swipemode)와 [실행](/uwp/api/windows.ui.xaml.controls.swipemode)이 있습니다. 살짝 밀기는 또한 위쪽, 아래쪽, 왼쪽, 오른쪽 등 4가지 방향을 지원합니다.

### <a name="reveal-mode"></a>표시 모드

표시 모드에서 사용자는 항목을 살짝 밀어서 하나 이상의 명령 메뉴를 열고, 명시적으로 명령을 탭하여 이를 실행해야 합니다. 사용자가 뒤로 살짝 밀기, 탭해서 끄기 또는 열린 살짝 밀기 항목을 스크롤하여 화면 끄기를 통해 메뉴를 다시 닫거나 명령을 선택할 때까지는 항목을 살짝 밀었다 놓아도 메뉴는 열린 상태로 유지됩니다.

![살짝 밀어 표시](images/SwipeCommand-Reveal_v2.gif)

표시 모드는 보다 안전하고 다양한 용도의 살짝 밀기 모드로, 대다수 유형의 메뉴 작업에 사용할 수 있으며 삭제처럼 파괴적일 수 있는 작업에도 사용할 수 있습니다.

사용자가 표시 모드에서 열린 미사용 상태로 표시된 메뉴 옵션 중 하나를 선택하면 해당 항목에 대한 명령이 호출되고 살짝 밀기 컨트롤이 닫힙니다.

### <a name="execute-mode"></a>실행 모드

실행 모드에서 사용자는 열린 항목을 살짝 미는 동작만으로 한 가지 명령을 표시하고 실행할 수 있습니다. 사용자가 임계값을 넘기기 전에 살짝 밀어 끌고 있는 항목을 놓으면 메뉴가 닫히고 명령이 실행되지 않습니다. 사용자가 항목을 살짝 밀고 임계값을 넘겨 놓으면 명령이 즉시 실행됩니다.

![살짝 밀어 실행](images/SwipeCommand_Delete_v2.gif)

사용자가 임계값에 도달한 후에도 손가락을 떼지 않거나 살짝 밀기 항목을 당겨 다시 닫으면 명령이 실행되지 않고 해당 항목에 대한 작업이 수행되지 않습니다.

실행 모드는 항목을 살짝 미는 동안 색상 및 레이블 방향을 통해 풍부한 시각적 피드백을 제공합니다.

실행은 사용자가 수행 중인 작업이 가장 흔한 것일 때 가장 유용합니다.

항목 삭제와 같이 보다 파괴적인 작업에서도 사용할 수 있습니다. 그러나 실행 모드는 표시 모드와 달리 한 방향으로 살짝 미는 한 가지 동작만 필요하기 때문에 사용자는 명시적으로 단추를 클릭해야 합니다.

### <a name="swipe-directions"></a>살짝 밀기 방향

살짝 밀기는 위쪽, 아래쪽, 왼쪽, 오른쪽 등 모든 기본 방향으로 작동합니다. 각각의 살짝 밀기 방향은 자체적인 살짝 밀기 항목이나 콘텐츠를 보유할 수 있지만, 살짝 밀 수 있는 요소 하나당 한 번에 한 방향 인스턴스만 설정할 수 있습니다.

예를 들어 같은 SwipeControl에서 두 개의 [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems)를 정의하는 것이 불가능합니다.

## <a name="how-to-create-a-swipe-command"></a>살짝 밀기 명령을 만드는 방법

살짝 밀기 명령에는 정의가 필요한 두 가지 요소가 포함되어 있습니다.

- [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol)은 콘텐츠를 중심으로 래핑을 수행합니다. ListView 같은 컬렉션의 경우, 이 요소는 DataTemplate 내에 있습니다.
- 살짝 밀기 컨트롤의 방향 컨테이너에 배치된 하나 이상의 [ SwipeItem ](/uwp/api/windows.ui.xaml.controls.swipeitem) 개체인 살짝 밀기 메뉴 항목으로는 [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems) 또는 [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)가 있습니다.

살짝 밀기 콘텐츠는 인라인으로 배치되거나 페이지 또는 앱의 리소스 섹션에 정의될 수 있습니다.

몇 가지 텍스트를 중심으로 래핑된 SwipeControl의 간단한 예는 다음과 같습니다. 여기에는 살짝 밀기 명령을 생성하는 데 필요한 XAML 요소의 계층이 나와 있습니다.

```xaml
<SwipeControl HorizontalAlignment="Center" VerticalAlignment="Center">
    <SwipeControl.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Pin">
                <SwipeItem.IconSource>
                    <SymbolIconSource Symbol="Pin"/>
                </SwipeItem.IconSource>
            </SwipeItem>
        </SwipeItems>
    </SwipeControl.LeftItems>

     <!-- Swipeable content -->
    <Border Width="180" Height="44" BorderBrush="Black" BorderThickness="2">
        <TextBlock Text="Swipe to Pin" Margin="4,8,0,0"/>
    </Border>
</SwipeControl>
```

지금부터는 목록에서 일반적으로 살짝 밀기 명령을 사용하는 방법을 완벽하게 보여주는 예제를 살펴보겠습니다. 이 예제에서는 실행 모드를 사용하는 삭제 명령과 표시 모드를 사용하는 다른 명령의 메뉴를 설정합니다. 두 명령 집합 모두 페이지의 리소스 섹션에서 정의됩니다. ListView의 항목에 살짝 밀기 명령이 적용됩니다.

먼저, 페이지 수준 리소스로 명령을 표현하는 살짝 밀기 항목을 만듭니다. SwipeItem은 아이콘으로 [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource)를 사용합니다. 또한 리소스로 아이콘을 생성합니다.

```xaml
<Page.Resources>
    <SymbolIconSource x:Key="ReplyIcon" Symbol="MailReply"/>
    <SymbolIconSource x:Key="DeleteIcon" Symbol="Delete"/>
    <SymbolIconSource x:Key="PinIcon" Symbol="Pin"/>

    <SwipeItems x:Key="RevealOptions" Mode="Reveal">
        <SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"/>
        <SwipeItem Text="Pin" IconSource="{StaticResource PinIcon}"/>
    </SwipeItems>

    <SwipeItems x:Key="ExecuteDelete" Mode="Execute">
        <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
                   Background="Red"/>
    </SwipeItems>
</Page.Resources>
```

살짝 밀기 콘텐츠의 메뉴 항목을 짧고 간결한 텍스트 레이블로 유지해야 합니다. 이러한 작업은 사용자가 단기간에 여러 번 수행하는 기본 작업이어야 합니다.

컬렉션 또는 ListView에서 살짝 밀기 명령이 작동하도록 설정하는 것은 DataTemplate에서 SwipeControl을 정의한다는 점을 제외하면 단순 살짝 밀기(위의 예)를 정의하는 것과 완전히 같기 때문에 컬렉션의 각 항목에 적용됩니다.

여기에 항목 DataTemplate에 SwipeContro이 적용되는 ListView가 있습니다. LeftItems 및 RightItems 속성은 리소스로 생성한 살짝 밀기 항목을 참조합니다.

```xaml
<ListView x:Name="sampleList" Width="300">
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
        </Style>
    </ListView.ItemContainerStyle>
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <SwipeControl x:Name="ListViewSwipeContainer"
                          LeftItems="{StaticResource RevealOptions}"
                          RightItems="{StaticResource ExecuteDelete}"
                          Height="60">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="{x:Bind}" FontSize="18"/>
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit..." FontSize="12"/>
                    </StackPanel>
                </StackPanel>
            </SwipeControl>
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

## <a name="handle-an-invoked-swipe-command"></a>호출된 살짝 밀기 명령을 처리

살짝 밀기 명령에 대해 작업을 수행하려면 [호출](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked) 이벤트를 처리합니다. (사용자가 명령을 호출하는 방법에 대한 자세한 내용은 이 문서 앞부분의 _살짝 밀기는 어떻게 작동합니까?_ 섹션을 검토하세요.) 일반적으로 살짝 밀기 명령은 ListView나 목록 스타일의 시나리오에 있습니다. 이 경우, 명령이 호출되면 살짝 민 항목에 대해 어떤 작업을 수행하고 싶을 것입니다.

이전에 생성한 살짝 밀기 항목인 _삭제_에서 호출 이벤트를 처리하는 방법은 다음과 같습니다.

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

데이터 항목은 SwipeControl의 DataContext입니다. 여기 나와 있듯이, 코드의 이벤트 인수에서 SwipeControl.DataContext 속성을 가져와서 살짝 민 항목을 액세스할 수 있습니다.

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> 여기서는 편의 상 ListView.Items 컬렉션에 항목들이 직접 추가되었기 때문에 이 항목도 같은 방식으로 삭제됩니다. 대신에 ListView.ItemsSource를 보다 일반적인 컬렉션으로 설정하는 경우에는 소스 컬렉션에서 해당 항목을 삭제해야 합니다.

이러한 특정 인스턴스에서는 해당 목록에서 해당 항목이 삭제되었기 때문에 살짝 민 항목의 최종 표시 상태는 중요하지 않습니다. 그러나 작업을 수행한 다음에 다시 살짝 밀기를 축소하고 싶은 경우에는 [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) 속성을 [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked) 열거값 중 하나로 설정할 수 있습니다.

- **자동**
  - 실행 모드에서는 살짝 밀기를 통해 연 항목이 호출 시에도 열린 상태로 유지됩니다.
  - 표시 모드에서는 살짝 밀기를 통해 연 항목이 호출 시 축소됩니다.
- **닫기**
  - 항목이 호출되면 살짝 밀기 컨트롤은 모드에 관계없이 항상 축소되고 정상 상태로 돌아갑니다.
- **RemainOpen**
  - 항목이 호출되면 살짝 밀기 컨트롤은 모드에 관계없이 항상 열린 상태로 유지됩니다.

여기에서 _회신_ 살짝 밀기 항목은 호출 후 닫기로 설정됩니다.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- FlipViews, 허브 또는 피벗에서 살짝 밀기를 사용하지 마세요. 이들을 결합해서 사용하면 살짝 밀기 방향이 상충하면서 사용자에게 혼동을 줄 수 있습니다.
- 가로 살짝 밀기와 가로 탐색 또는 세로 살짝 밀기와 세로 탐색을 결합해서 사용하지 마세요.
- 사용자가 살짝 밀기 중인 항목이 동일한 작업이고, 살짝 밀기가 가능한 모든 관련 항목에 대해 일관되게 적용하도록 하세요.
- 사용자가 수행하려는 주 작업에서 살짝 밀기를 사용하세요.
- 동일한 작업이 수차례 반복되는 항목에서 살짝 밀기를 사용하세요.
- 가로 살짝 밀기는 가로로 긴 항목에, 세로 살짝 밀기는 세로로 긴 항목에 사용하세요.
- 짧고 간결한 텍스트 레이블을 사용하세요.

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련된 문서

- [목록 보기 및 그리드 보기](listview-and-gridview.md)
- [항목 컨테이너 및 템플릿](item-containers-templates.md)
- [당겨서 새로 고침](pull-to-refresh.md)
