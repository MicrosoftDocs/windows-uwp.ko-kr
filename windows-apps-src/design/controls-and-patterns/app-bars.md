---
author: QuinnRadich
Description: Command bars give users easy access to your app's most common tasks.
title: 명령 모음
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d3c8aad90e028ece42128e86f5e255be7fd29177
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6987812"
---
# <a name="command-bar"></a>명령 모음

명령 모음을 통해 앱에서 가장 많이 수행하는 작업에 쉽게 액세스할 수 있습니다. 명령 모음은 앱 수준의 명령이나 페이지 고유의 명령에 액세스할 수 있도록 해주며, 어떠한 탐색 패턴에서도 사용이 가능합니다.

> **중요 API**: [CommandBar 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), [AppBarButton 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx), [AppBarSeparator 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

![아이콘이 있는 명령 모음의 예](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

CommandBar 컨트롤은 유연하고 가벼운 범용 컨트롤로서 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx) 및 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx) 컨트롤과 같은 간단한 명령뿐만 아니라 이미지 또는 텍스트 블록과 같은 복잡한 콘텐츠도 표시할 수 있습니다.

> [!NOTE]
> XAML은 [AppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar) 컨트롤과 [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) 컨트롤을 모두 제공합니다. AppBar를 사용하는 유니버설 Windows 8 앱을 업그레이드하는 경우 AppBar만 사용해야 하며 변경은 최소화해야 합니다. Windows 10의 새로운 앱의 경우 CommandBar 컨트롤을 대신 사용하는 것이 좋습니다. 이 문서에서는 CommandBar 컨트롤을 사용한다고 가정합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우는 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/CommandBar">앱을 열고 작동 중인 CommandBar를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Microsoft 사진 앱의 확장된 명령 모음입니다.

![Microsoft 사진 앱의 명령 모음](images/control-examples/command-bar-photos.png)

Windows Phone의 Outlook 일정 명령 모음입니다.

![Outlook 일정 앱의 명령 모음](images/control-examples/command-bar-calendar-phone.png)

## <a name="anatomy"></a>구조

기본적으로 명령 모음에는 아이콘 단추 행 및 줄임표 \[•••\]로 표현되는 선택적 "자세히 보기" 단추가 표시됩니다. 다음은 나중에 표시된 예제 코드로 만든 명령 모음입니다. 닫힘 컴팩트 상태로 표시됩니다.

![닫힌 명령 모음](images/command-bar-compact.png)

또한 명령 모음은 다음과 같이 닫힘, 최소 상태로 표시할 수 있습니다. 자세한 내용은 [열림 및 닫힘 상태](#open-and-closed-states) 섹션을 참조하세요.

![닫힌 명령 모음](images/command-bar-minimal.png)

다음은 열림 상태의 동일한 명령 모음입니다. 레이블로 컨트롤의 주요 부분을 식별합니다.

![닫힌 명령 모음](images/commandbar_anatomy_open.png)

명령 모음은 다음 4개의 기본 영역으로 구분됩니다.
- 콘텐츠 영역은 모음의 왼쪽에 맞춰집니다. [콘텐츠](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) 속성이 채워지면 표시됩니다.
- 기본 명령 영역은 모음의 오른쪽에 맞춰집니다. [기본 명령](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) 속성이 채워지면 표시됩니다.  
- "자세히 보기" \[•••\] 단추는 모음의 오른쪽에 표시됩니다. "자세히 보기" \[•••\] 단추를 누르면 기본 명령 레이블이 표시되고 보조 명령이 있는 경우 오버플로 메뉴가 열립니다. 이 단추는 기본 명령 레이블이나 보조 레이블이 존재하지 않으면 표시되지 않습니다. 기본 작동을 변경하려면 [OverflowButtonVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx) 속성을 사용하세요.
- 오버플로 메뉴는 명령 모음이 열리고 [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 속성이 채워지는 경우에만 표시됩니다. 공간이 제한된 경우 기본 명령이 SecondaryCommands 영역으로 이동합니다. 기본 작동을 변경하려면 [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx) 속성을 사용하세요.

레이아웃은 [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx)이 **RightToLeft**일 때 반대로 됩니다.

## <a name="create-a-command-bar"></a>명령 모음 만들기
이 예제는 이전에 표시된 명령 모음을 만듭니다.

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>명령 및 콘텐츠
CommandBar 컨트롤에는 명령 및 콘텐츠를 추가하는 데 사용할 수 있는 세 가지 속성, 즉 [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx), [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 및 [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) 속성이 있습니다.


### <a name="commands"></a>명령

기본적으로 명령 모음 항목은 **PrimaryCommands** 컬렉션에 추가됩니다. 가장 중요한 명령이 항상 표시되도록 하려면 중요도 순서로 명령을 추가해야 합니다. 사용자가 앱 창의 크기를 조정해서 명령 모음 너비가 변경되면 기본 명령이 명령 모음과 중단점의 오버플로 메뉴 간에 동적으로 이동합니다. 이러한 기본 작동을 변경하려면 [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx) 속성을 사용하세요. 

가장 작은 화면(320epx 너비)에서는 최대 4개의 기본 명령이 명령 모음에 들어갑니다. 

**SecondaryCommands** 컬렉션에 명령을 추가할 수 있고, 이러한 명령은 오버플로 메뉴에 표시됩니다.

!["자세히" 영역과 아이콘이 있는 명령 모음의 예](images/appbar_rs2_overflow_icons.png)

필요에 따라 PrimaryCommands와 SecondaryCommands 사이의 명령을 프로그래밍 방식으로 이동할 수 있습니다.

- *여러 페이지에서 일관되게 표시되는 명령이 있는 경우 해당 명령을 일관된 위치에 유지하는 것이 가장 좋습니다.*
- *수락, 예 및 확인 명령은 거부, 아니요, 취소 명령 왼쪽에 배치하는 것이 좋습니다. 일관성은 사용자에게 신뢰감을 주어 시스템을 둘러보도록 유도하며, 앱 탐색과 관련된 지식을 앱 간에 적용하는 데 도움이 됩니다.*

### <a name="app-bar-buttons"></a>앱 바 단추

PrimaryCommands와 SecondaryCommands는 모두 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 및 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 명령 요소로만 채울 수 있습니다. 

앱 바 단추 컨트롤은 아이콘 및 텍스트 레이블에 따라 구분됩니다. 이러한 컨트롤은 명령 모음에서의 사용에 최적화되어 있으며, 컨트롤이 명령 모음에서 사용되는지 또는 오버플로 메뉴에서 사용되는지에 따라 모양이 변경됩니다.

오버플로 메뉴의 아이콘 크기는 16x16px로 기본 명령 영역의 아이콘(20x20px)보다 작습니다. SymbolIcon, FontIcon 또는 PathIcon을 사용하는 경우 명령이 보조 명령 영업에 진입할 때 화질 손실 없이 아이콘 배율이 올바른 크기로 자동 조정됩니다. 

### <a name="button-labels"></a>단추 레이블
AppBarButton [IsCompact](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) 속성은 레이블의 표시 여부를 결정합니다. CommandBar 컨트롤에서 명령 모음은 열리고 닫힐 때 자동으로 단추의 IsCompact 속성을 덮어씁니다.

앱 바 단추 레이블의 위치를 변경하려면 CommandBar의 [DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) 속성을 사용합니다.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![레이블이 오른쪽에 있는 명령 모음](images/app-bar-labels-on-right.png)

창이 더 커지면 가독성 개선을 위해 레이블을 앱 바 단추 아이콘의 오른쪽으로 이동하는 것이 좋습니다. 레이블이 아래쪽에 있으면 명령 모음을 열어야 레이블 표시되지만 레이블이 오른쪽에 있으면 명령 모음이 닫혀 있더라도 표시됩니다.

오버플로 메뉴에서는 기본적으로 아이콘의 오른쪽으로 레이블 위치가 이동되고 **LabelPosition**이 무시됩니다. [CommandBarOverflowPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) 속성을 [CommandBarOverflowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter)를 대상으로 하는 스타일로 설정하여 스타일을 조정할 수 있습니다. 

단추 레이블은 짧아야 하며, 가급적이면 한 단어여야 합니다. 아이콘 아래의 더 긴 레이블은 여러 줄로 줄바꿈되어 열리는 명령 모음의 전체 높이가 늘어납니다. 단어 분리가 일어나야 하는 문자 경계에서 레이블이 암시를 주도록 텍스트에 소프트 하이픈 문자(0x00AD)를 포함할 수 있습니다. XAML에서 다음과 같이 이스케이프 시퀀스를 사용하여 이를 표시합니다.

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

레이블이 암시된 위치에서 래핑하는 경우 모양은 다음과 같습니다.

![래핑 레이블이 있는 앱 바 단추](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>명령 모음 플라이아웃

회신, 전체 회신 및 전달을 응답 메뉴에 배치하는 등 논리적 그룹을 명령에 사용합니다. 일반적으로 앱 바 단추는 단일 명령을 활성화하지만 사용자 지정 콘텐츠와 함께 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx) 또는 [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx)을 표시하는 데 앱 바 단추를 사용할 수 있습니다.

![명령 모음의 플라이아웃 예](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>기타 콘텐츠

**Content** 속성을 설정하여 콘텐츠 영역에 XAML 요소를 추가할 수 있습니다. 둘 이상의 요소를 추가하려는 경우 패널 컨테이너에 배치하고 패널을 Content 속성의 단일 자식으로 만들어야 합니다.

동적 오버플로가 활성화되면 기본 명령이 오버플로 메뉴로 이동할 수 있으므로 콘텐츠가 잘리지 않게 됩니다. 그렇지 않은 경우 기본 명령이 우선 적용되어 콘텐츠 잘림이 야기될 수 있습니다.

[ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx)가 **Compact**인 경우 콘텐츠가 명령 모음의 컴팩트 크기보다 크면 잘릴 수 있습니다. 표시할 [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx) 및 [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 이벤트를 처리하거나 콘텐츠 영역의 UI 부분이 잘리지 않도록 숨겨야 합니다. 자세한 내용은 [열림 및 닫힘 상태](#open-and-closed-states) 섹션을 참조하세요.


## <a name="open-and-closed-states"></a>열림 및 닫힘 상태

명령 모음은 열거나 닫을 수 있습니다. 기본 명령 단추는 열릴 때 텍스트 레이블로 표시되고, 보조 명령이 있는 경우에는 오버플로 메뉴가 열립니다.

사용자가 "자세히 보기" \[•••\] 단추를 눌러 이러한 상태 간을 전환할 수 있습니다. [IsOpen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx) 속성을 설정하여 프로그래밍 방식으로 상태 간을 전환할 수 있습니다. 

[Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx), [Opened](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx), [Closing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) 및 [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 이벤트를 사용하여 열리거나 닫히는 명령 모음에 응답할 수 있습니다.  
- Opening 및 Closing 이벤트는 전환 애니메이션이 시작하기 전에 발생합니다.
- Opened 및 Closed 이벤트는 전환이 완료된 후 발생합니다.

이 예제에서는 명령 모음의 불투명도를 변경하는 데 Opening 및 Closing 이벤트를 사용합니다. 명령 모음이 닫히면 반투명이므로 앱 백그라운드가 비쳐 보입니다. 명령 모음이 열릴 때 명령 모음은 불투명해지므로 사용자가 명령에 집중할 수 있습니다.

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### <a name="issticky"></a>IsSticky

명령 모음이 열린 상태에서 사용자가 앱의 다른 부분과 상호 작용을 하면 명령 모음이 자동으로 닫힙니다. 이를 *빠른 해제*라고 합니다. [IsSticky](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx) 속성을 설정하여 빠른 해제 동작을 제어할 수 있습니다. `IsSticky="true"`일 때 사용자가 "자세히 보기" \[•••\] 단추를 누르거나 오버플로 메뉴에서 항목을 선택할 때까지 이 모음은 열려 있습니다. 

고정 명령 모음은 빠른 해제에 대한 사용자의 기대를 준수하지 않으므로 사용하지 않는 것이 좋습니다.

### <a name="display-mode"></a>디스플레이 모드

[ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) 속성을 설정하여 닫힘 상태에서 명령 모음이 표시되는 방법을 제어할 수 있습니다. 3개의 닫힌 디스플레이 모드에서 선택할 수 있습니다.
- **컴팩트**: 기본 모드입니다. 콘텐츠, 레이블이 없는 기본 명령 아이콘 및 "자세히 보기" \[•••\] 단추를 표시합니다.
- **최소**: "자세히 보기" \[•••\] 단추 역할을 하는 가는 가로 막대형만 표시합니다. 모음의 아무 곳이나 눌러서 열 수 있습니다.
- **숨김**: 명령 모음이 닫히면 표시되지 않습니다. 이 모드는 인라인 명령 모음을 사용하여 상황에 맞는 명령을 표시하는 데 유용할 수 있습니다. 이 경우 **IsOpen** 속성을 설정하거나 ClosedDisplayMode를 **Minimal** 또는 **Compact**로 변경하여 프로그래밍 방식으로 명령 모음을 열어야 합니다.

여기서 명령 모음은 [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)에 대한 간단한 서식 명령을 저장하는 데 사용합니다. 편집 상자에 포커스가 없으면 서식 명령이 주의가 분산될 수 있으므로 숨겨져 있습니다. 편집 상자를 사용하는 중이면 명령 모음의 ClosedDisplayMode가 Compact로 변경되어 서식 명령이 표시됩니다.

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**참고**&nbsp;&nbsp;편집 명령 구현은 이 예제의 범위를 벗어납니다. 자세한 내용은 [RichEditBox](rich-edit-box.md) 문서를 참조하세요.

최소 및 숨김 모드가 일부 경우에 유용하지만 모든 작업을 숨기면 사용자에게 혼동을 줄 수 있으므로 주의하세요.

사용자에게 힌트를 더 또는 덜 제공하도록 ClosedDisplayMode를 변경하면 주변 요소의 레이아웃에 영향을 줍니다. 반면, CommandBar가 닫힘과 열림 간에 전환하는 경우 다른 요소의 레이아웃에 영향을 주지 않습니다.

## <a name="placement"></a>배치
명령 모음은 앱 창의 맨 위, 앱 창의 맨 아래에 배치하거나 인라인으로 배치할 수 있습니다.

![앱 바 배치의 예 1](images/AppbarGuidelines_Placement1.png)

-   소형 핸드헬드 디바이스의 경우 접근이 쉽도록 화면의 맨 아래쪽에 명령 모음을 배치하는 것이 좋습니다.
-   화면이 더 큰 디바이스에서 명령 모음을 창의 위쪽에 배치하면 더 눈에 띄고 찾기가 쉽습니다.

[DiagonalSizeInInches](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) API를 사용하여 실제 화면 크기를 결정합니다.

단일 보기 화면(왼쪽 예) 및 여러 보기 화면(오른쪽 예)에서는 다음 화면 영역에 명령 모음을 배치할 수 있습니다. 인라인 명령 모음은 작업 공간 내 어디에나 배치할 수 있습니다.

![앱 바 배치의 예 2](images/AppbarGuidelines_Placement2.png)

>**터치 디바이스**: 터치 키보드 또는 SIP(Soft Input Panel)가 나타날 때 명령 모음이 사용자에게 계속 표시되어야 하는 경우 명령 모음을 페이지의 [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) 속성에 할당할 수 있으며 SIP가 있는 경우 명령 모음은 계속 표시되도록 이동하게 됩니다. 그러지 않으면 명령 모음을 인라인으로 앱 콘텐츠 기준으로 배치해야 합니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [XAML 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>관련 문서

* [UWP 앱의 명령 디자인 기본 사항](../basics/commanding-basics.md)
* [CommandBar 클래스](https://msdn.microsoft.com/library/windows/apps/dn279427)
