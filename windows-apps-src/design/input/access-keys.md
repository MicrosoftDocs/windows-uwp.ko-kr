---
author: Karl-Bridge-Microsoft
Description: Learn how to improve both the usability and the accessibility of your UWP app by providing an intuitive way for users to quickly navigate and interact with an app's visible UI through a keyboard instead of a pointer device (such as touch or mouse).
title: 선택키 설계 지침
label: Access keys design guidelines
keywords: 키보드, 선택키, 키팁, 키 팁, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 조작
template: detail.hbs
ms.author: kbridge
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8e842d6c5b8e62a9c043c97849fdf17f524ccfc7
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536413"
---
# <a name="access-keys"></a>선택키

선택키는 사용자가 터치 또는 마우스 같은 포인터 장치 대신 키보드를 통해 앱의 시각적 UI를 신속하게 탐색하고 상호 작용할 수 있는 직관적인 방법을 제공하여 Windows 응용 프로그램의 사용 편의성과 접근성을 개선하는 바로 가기 키입니다.

바로 가기 키를 사용하여 Windows 응용 프로그램에서 일반적인 작업을 호출하는 방법에 대한 자세한 내용은 [바로 가기 키](keyboard-accelerators.md) 항목을 참조하세요. 

> [!NOTE]
> 키보드는 특정 장애([키보드 접근성](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility) 참조)가 있는 사용자에게 필수이며, 앱과 좀 더 효과적으로 상호 작용하는 수단으로 키보드를 선호하는 사용자에게도 중요한 도구입니다.

UWP(유니버설 Windows 플랫폼)는 모든 플랫폼 컨트롤에서 키 팁이라고 하는 시각적 큐를 통해 키보드 기반 선택키 및 연결된 UI 피드백에 대한 기본 지원을 제공합니다.

## <a name="overview"></a>개요

선택키는 Alt 키와 하나 이상의 영숫자 키 조합(*니모닉*이라고도 함)으로 구성되며 일반적으로 동시에 누르지 않고 순차적으로 누릅니다.

키 팁은 사용자가 Alt 키를 누를 때 선택키를 지원하는 컨트롤 옆에 표시되는 배지입니다. 각 키 팁에는 연결된 컨트롤을 활성화하는 영숫자 키가 포함되어 있습니다.

> [!NOTE]
> 바로 가기 키는 단일 영숫자 문자를 사용하는 선택키에 대해 자동으로 지원됩니다. 예를 들어 Word에서 Alt + F를 동시에 누르면 파일 메뉴가 열리고 키 팁은 표시되지 않습니다.

Alt 키를 누르면 선택키 기능이 초기화되고 현재 사용 가능한 모든 키 조합이 키 팁에 표시됩니다. 이후의 키 입력은 선택키 프레임워크를 통해 처리됩니다. 유효한 선택키를 누르거나 Enter, Esc, Tab 또는 화살표 키를 눌러 선택키를 비활성화하고 키 입력 처리를 앱에 반환할 때까지 잘못된 키가 거부됩니다.

Microsoft Office 앱은 선택키에 대한 다양한 지원을 제공합니다. 다음 이미지는 선택키가 활성화된 Word의 홈 탭을 보여 줍니다(숫자 및 여러 키 입력을 지원).

![Microsoft Word의 선택키에 대한 키 팁 배지](images/accesskeys/keytip-badges-word.png)

_Microsoft Word의 선택키에 대한 키 팁 배지_

컨트롤에 선택키를 추가하려면 **AccessKey 속성**을 사용합니다. 이 속성의 값은 선택키 순서, 바로 가기(단일 영숫자인 경우) 및 키 팁을 지정합니다.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>선택키를 사용해야 하는 경우

적합한 경우 UI 어디서나 선택키를 지정하고 모든 사용자 지정 컨트롤에서 선택키를 지원하는 것이 좋습니다.

1.  키를 한 번에 하나만 누를 수 있거나 마우스 사용에 어려움이 있는 사용자를 포함하여 거동 장애가 있는 사용자를 위해 **선택키로 앱의 접근성을 향상**합니다.

    잘 디자인된 키보드 UI는 소프트웨어 접근성의 중요한 요소입니다. 시각 장애나 특정 거동 장애가 있는 사용자는 키보드 UI를 사용하여 앱을 탐색하고 기능을 조작할 수 있습니다. 이러한 사용자는 마우스를 작동할 수 없으며 다양한 보조 기술(예: 키보드 향상 도구, 화상 키보드, 화면 확대기, 화면 낭독 프로그램 및 음성 입력 유틸리티)을 대신 사용할 수 있습니다. 이러한 사용자에게는 포괄적인 명령 범위가 매우 중요합니다.

2.  키보드로 조작하는 것을 선호하는 고급 사용자를 위해 **선택키로 앱의 사용 편의성을 향상**합니다.

    숙련된 사용자는 키보드 기반 명령을 더 빠르게 입력할 수 있고 키보드에서 손을 떼지 않아도 되기 때문에 키보드를 선호하는 경향이 강합니다. 이러한 사용자에게는 효율성과 일관성이 매우 중요합니다. 포괄성은 가장 자주 사용하는 명령에만 중요합니다.

## <a name="set-access-key-scope"></a>선택키 범위 설정

화면에 선택키를 지원하는 요소가 많이 있는 경우 선택키 범위를 조정하여 **인지적 부하**를 줄이는 것이 좋습니다. 이렇게 하면 화면의 선택키 수가 최소화되어 쉽게 찾을 수 있으며, 효율성과 생산성이 향상됩니다.

예를 들어 Microsoft Word는 두 가지 선택키 범위를 제공합니다. 하나는 리본 탭의 주 범위이고 다른 하나는 선택한 탭의 명령에 대한 보조 범위입니다.

다음 이미지는 Word의 두 선택키 범위를 보여 줍니다. 첫 번째는 사용자가 탭 및 다른 최상위 수준 명령을 선택할 수 있는 주 선택키를 보여 주고, 두 번째는 홈 탭의 선택키를 보여 줍니다.

![Microsoft Word의 주 선택키](images/accesskeys/primary-access-keys-word.png)
_Microsoft Word의 주 선택키_

![Microsoft Word의 보조 선택키](images/accesskeys/secondary-access-keys-word.png)
_Microsoft Word의 보조 선택키_

선택키는 여러 범위의 요소에 대해 중복 가능합니다. 위 예제에서 "2"는 주 범위의 실행 취소 그리고 보조 범위의 "기울임꼴"에 대한 선택키입니다.

다음은 선택키 범위를 정의하는 방법입니다.

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![CommandBar에 대한 기본 선택키](images/accesskeys/primary-access-keys-commandbar.png)

_CommandBar 주 범위 및 지원되는 선택키_

![CommandBar에 대한 보조 선택키](images/accesskeys/secondary-access-keys-commandbar.png)

_CommandBar 보조 범위 및 지원되는 선택키_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 크리에이터스 업데이트 이상

Windows 10 Fall Creators Update 이전에는 CommandBar 등 일부 컨트롤은 기본 선택키 범위를 지원하지 않았습니다.

다음 예제는 Word의 리본처럼 부모 명령이 호출되면 선택키가 제공되는 CommandBar의 SecondaryCommands를 지원하는 방법을 보여 줍니다.

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>선택키 충돌 방지

동일한 범위에 있는 두 개 이상의 요소가 중복되는 선택키를 갖고 있거나 동일한 영숫자 문자로 시작하는 경우 선택키 충돌이 발생합니다.

시스템에서는 시각적 트리에 추가된 첫 번째 요소의 선택키를 처리하고 나머지는 무시하여 중복 선택키를 해결합니다.

여러 선택키가 동일한 문자로 시작하는 경우(예: “A”, “A1”, “AB”) 시스템에서는 단일 문자 선택키를 처리하고 나머지는 무시합니다.

고유한 선택키를 사용하거나 명령의 범위를 지정하여 충돌을 방지해야 합니다.

## <a name="choose-access-keys"></a>선택키를 선택

선택키를 선택할 때 다음 사항을 고려해야 합니다.

-   단일 문자를 사용하여 키 입력을 최소화하고 바로 가기 키를 기본적으로 지원(Alt + AccessKey)
-   문자를 3개 이상 사용하지 말 것
-   선택키 충돌 방지
-   문자 "I"와 숫자 "1" 또는 문자 "O"와 숫자 "0"처럼 다른 문자와 구분하기 어려운 문자를 사용하지 말 것
-   Word처럼 많이 사용되는 앱의 잘 알려진 선례를 사용할 것(“파일”은 “F”, “홈”은 “H” 등)
-   명령 이름의 첫 번째 문자 또는 명령과 밀접하게 관련되어 기억하기 쉬운 문자를 사용할 것
    -   첫 번째 문자가 이미 할당된 경우 명령 이름의 첫 번째 문자와 최대한 가까운 문자를 사용할 것(삽입은 "N")
    -   명령 이름의 고유한 자음 사용(보기는 "W")
    -   명령 이름의 모음 사용.

## <a name="localize-access-keys"></a>선택키 지역화

또한 앱이 여러 언어로 지역화되는 경우 **선택키 지역화를 고려**해야 합니다. 예를 들어 en-US에서는 “Home”에 "H"를 사용하고 es-ES에서는 “Incio”에 “I”를 사용합니다.

다음과 같이 태그에 x:Uid 확장을 사용하여 지역화된 리소스를 적용합니다.

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
각 언어에 대한 리소스는 프로젝트의 해당 문자열 폴더에 추가됩니다.

![영어 및 스페인어 리소스 문자열 폴더](images/accesskeys/resource-string-folders.png)

_영어 및 스페인어 리소스 문자열 폴더_

지역화된 선택키는 프로젝트 resources.resw 파일에 지정됩니다.

![resources.resw 파일에 지정된 AccessKey 속성 지정](images/accesskeys/resource-resw-file.png)

_resources.resw 파일에 지정된 AccessKey 속성 지정_

자세한 내용은 [UI 리소스 번역](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965329(v=win.10).aspx)을 참조하세요.

## <a name="key-tip-positioning"></a>키 팁 위치 지정

키 팁은 다른 UI 요소의 존재, 다른 키 팁 및 화면 가장자리를 고려하여 해당 UI 요소에 대한 부동 배지로 표시됩니다.

일반적으로 기본 키 팁 위치면 충분하며 기본 키 팁 위치는 적응형 UI에 대한 기본 지원을 제공합니다.

![자동 키 팁 배치의 예](images/accesskeys/auto-keytip-position.png)

_자동 키 팁 배치의 예_

그러나 키 팁 위치를 보다 세밀하게 제어해야 하는 경우 다음 사항을 따르는 것이 좋습니다.

1.  **명확한 연관성 원칙**: 사용자가 컨트롤을 키 팁과 쉽게 연결할 수 있습니다.

    a.  키 팁은 선택키를 갖고 있는 요소(소유자)와 **가까이** 있어야 합니다.  
    b.  키 팁은 선택키를 갖고 있는 **활성화된 요소를 포함하면 안 됩니다**.   
    c.  키 팁을 소유자와 가까운 곳에 배치할 수 없는 경우 소유자와 겹쳐야 합니다. 

2.  **검색 기능**: 사용자가 키 팁을 사용하여 신속하게 컨트롤을 검색할 수 있습니다.

    a.  키 팁은 절대로 다른 키 팁과 **겹치면** 안 됩니다.  

3.  **쉬운 검사:** 사용자가 키 팁을 간단하게 살펴볼 수 있습니다.

    a.  키 팁을 다른 키 팁 및 UI 요소와 일직선으로 **정렬**해야 합니다.
    b.  키 팁을 최대한 많이 **그룹화**해야 합니다. 

### <a name="relative-position"></a>상대 위치

**KeyTipPlacementMode** 속성을 사용하여 요소 또는 그룹별로 키 팁의 배치를 사용자 지정합니다.

배치 모드는 위, 아래, 오른쪽, 왼쪽, 숨김, 가운데 및 자동입니다.

![키 팁 배치 모드](images/accesskeys/keytip-postion-modes.png)

_키 팁 배치 모드_

컨트롤의 중심선을 사용하여 키 팁의 세로 및 가로 정렬을 계산합니다.

다음 예제는 StackPanel 컨테이너의 KeyTipPlacementMode 속성을 사용하여 컨트롤 그룹의 키 팁 배치를 설정하는 방법을 보여 줍니다.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>오프셋

키 팁 위치를 훨씬 세밀하게 제어하려면 요소의 KeyTipHorizontalOffset 및 KeyTipVerticalOffset 속성을 사용합니다.

> [!NOTE]
> KeyTipPlacementMode가 자동으로 설정되면 오프셋을 설정할 수 없습니다.

KeyTipHorizontalOffset 속성은 키 팁을 왼쪽 또는 오른쪽으로 얼마나 이동할지를 나타냅니다. 다음 예제는 단추의 키 팁 오프셋을 설정하는 방법을 보여 줍니다.

![키 팁 배치 모드](images/accesskeys/keytip-offsets.png)

_키 팁의 세로 및 가로 오프셋 설정_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>화면 가장자리 맞춤 {#screen-edge-alignment .ListParagraph}

키 팁이 완전하게 표시되도록 화면 가장자리를 기준으로 키 팁의 위치가 자동으로 조정됩니다. 이 동작이 발생할 때 컨트롤과 키 팁 정렬 지점 간의 거리가 가로 및 세로 오프셋에 지정된 값과 다를 수 있습니다.

![키 팁 배치 모드](images/accesskeys/keytips-screen-edge.png)

_화면 가장자리를 사용하면 키 팁이 자동으로 위치를 변경합니다._

## <a name="key-tip-style"></a>스타일 키 팁

고대비를 비롯한 플랫폼 테마에 대한 기본 키 팁 지원을 사용하는 것이 좋습니다.

사용자 고유의 키 팁 스타일을 지정해야 하는 경우 KeyTipFontSize(글꼴 크기), KeyTipFontFamily(글꼴 패밀리), KeyTipBackground(배경), KeyTipForeground(전경), KeyTipPadding(안쪽 여백), KeyTipBorderBrush(테두리 색), KeyTipBorderThemeThickness(테두리 두께) 등의 응용 프로그램 리소스를 사용합니다.

![키 팁 배치 모드](images/accesskeys/keytip-customization.png)

_키 팁 사용자 지정 옵션_

이 예제에서는 이러한 응용 프로그램 리소스를 변경하는 방법을 보여 줍니다.

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>선택키 및 내레이터

XAML 프레임워크는 UI 자동화 클라이언트가 사용자 인터페이스의 요소에 대한 정보를 검색할 수 있게 해주는 자동화 속성을 노출합니다.

UIElement 또는 TextElement 컨트롤에서 AccessKey 속성을 지정하는 경우 [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) 속성을 사용하여 이 값을 가져올 수 있습니다. 내레이터 같은 접근성 클라이언트의 경우 요소가 포커스를 받을 때마다 이 속성 값을 읽습니다.

## <a name="related-articles"></a>관련 문서

* [키보드 조작](keyboard-interactions.md)
* [바로 가기 키](keyboard-accelerators.md)

**샘플**
* [XAML 컨트롤 갤러리 (일명 XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


