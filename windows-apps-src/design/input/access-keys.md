---
Description: '사용자가 포인터 장치 (예: 터치 또는 마우스) 대신 키보드를 통해 앱의 표시 되는 UI를 신속 하 게 탐색 하 고 상호 작용할 수 있는 직관적인 방법을 제공 하 여 Windows 앱의 유용성 및 내게 필요한 옵션을 개선 하는 방법에 대해 알아봅니다.'
title: 액세스 키 디자인 지침
label: Access keys design guidelines
keywords: 키보드, 액세스 키, keytip, 키 팁, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 상호 작용
template: detail.hbs
ms.date: 06/08/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c0d5808c462beb72341fd83c6fc4c1cfc0178b2f
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970978"
---
# <a name="access-keys"></a>액세스 키

액세스 키는 사용자가 포인터 장치 (예: 터치 또는 마우스) 대신 키보드를 통해 앱의 표시 되는 UI를 신속 하 게 탐색 하 고 상호 작용할 수 있는 직관적인 방법을 제공 하 여 Windows 응용 프로그램의 유용성 및 접근성을 개선 하는 바로 가기 키입니다.

바로 가기 키를 사용 하 여 Windows 응용 프로그램에서 일반적인 작업 호출에 대 한 자세한 내용은 [액셀러레이터 키](keyboard-accelerators.md) 항목을 참조 하세요. 

> [!NOTE]
> 키보드는 특정 장애가 있는 사용자에 게 중요 하며 ( [키보드 접근성](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)참조) 앱과 상호 작용 하는 보다 효율적인 방법으로 선호 하는 사용자를 위한 중요 한 도구 이기도 합니다.

Windows 앱은 키 팁 이라는 시각적 신호를 통해 키보드 기반 액세스 키 및 연결 된 UI 피드백 모두에 대해 플랫폼 컨트롤에서 기본 제공 되는 지원을 제공 합니다.

## <a name="overview"></a>개요

액세스 키는 Alt 키와 하나 이상의 영숫자 키 (일반적으로 *니모닉*이라고 함)의 조합입니다.

키 팁은 사용자가 Alt 키를 누를 때 액세스 키를 지 원하는 컨트롤 옆에 표시 되는 배지입니다. 각 키 팁에는 연결 된 컨트롤을 활성화 하는 영숫자 키가 포함 되어 있습니다.

> [!NOTE]
> 키보드 바로 가기 키는 영숫자 문자가 하나인 선택키가 자동으로 지원 됩니다. 예를 들어, Word에서 Alt + F를 동시에 누르면 주요 팁을 표시 하지 않고 파일 메뉴가 열립니다.

Alt 키를 누르면 액세스 키 기능이 초기화 되 고 키 팁에 현재 사용 가능한 키 조합이 모두 표시 됩니다. 다음 키 입력은 유효한 액세스 키를 누를 때까지 잘못 된 키를 거부 하는 액세스 키 프레임 워크에서 처리 되며, 액세스 키를 비활성화 하 고 앱에 대 한 키 입력 처리를 반환 하기 위해 Enter, Esc, Tab 또는 화살표 키를 누를 때까지 키를 누를 수 있습니다.

Microsoft Office 앱은 액세스 키에 대 한 광범위 한 지원을 제공 합니다. 다음 이미지는 선택 된 액세스 키가 있는 Word의 홈 탭을 보여 줍니다 (숫자 및 여러 키 입력에 대 한 지원).

![Microsoft Word의 액세스 키에 대 한 키 팁 배지](images/accesskeys/keytip-badges-word.png)

_Microsoft Word의 액세스 키에 대 한 KeyTip 배지_

컨트롤에 액세스 키를 추가 하려면 **AccessKey 속성**을 사용 합니다. 이 속성의 값은 액세스 키 시퀀스, 바로 가기 (단일 영숫자) 및 키 팁을 지정 합니다.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>액세스 키를 사용 하는 경우

UI에 적절 한 액세스 키를 지정 하 고 모든 사용자 지정 컨트롤의 액세스 키를 지 원하는 것이 좋습니다.

1.  **액세스 키** 를 사용 하면 한 번에 하나의 키만 누르거나 마우스를 사용 하는 데 어려움이 있는 사용자를 포함 하 여 화물 차 장애가 있는 사용자가 앱에 보다 쉽게 액세스할 수 있습니다.

    잘 디자인 된 키보드 UI는 소프트웨어 접근성의 중요 한 측면입니다. 시각 장애나 특정 거동 장애가 있는 사용자는 키보드 UI를 사용하여 앱을 탐색하고 기능을 조작할 수 있습니다. 이러한 사용자는 마우스를 작동 하지 않고 키보드 기능 도구, 화상 키보드, 화면 enlargers, 화면 판독기, 음성 입력 유틸리티 등의 다양 한 보조 기술을 사용 하는 것이 좋습니다. 이러한 사용자에 게는 포괄적인 명령 범위를 적용 하는 것이 중요 합니다.

2.  액세스 키를 사용 하면 키보드를 사용 하는 것을 선호 하는 고급 사용자가 **앱을 보다 쉽게 사용할** 수 있습니다.

    키보드 기반 명령을 빠르게 입력할 수 있고 키보드에서 손을 제거 하지 않아도 되는 경우 숙련 된 사용자에 게 키보드 사용에 대 한 강력한 기본 설정이 사용 됩니다. 이러한 사용자의 경우 효율성과 일관성은 매우 중요 합니다. comprehensiveness는 가장 자주 사용 되는 명령에만 중요 합니다.

## <a name="set-access-key-scope"></a>액세스 키 범위 설정

화면에 액세스 키를 지 원하는 요소가 여러 개 있는 경우 액세스 키의 범위를 지정 하 여 **인지 부하**를 줄이는 것이 좋습니다. 이렇게 하면 화면에 액세스 키 수가 최소화 되어 쉽게 찾을 수 있으며 효율성과 생산성이 향상 됩니다.

예를 들어 Microsoft Word는 리본 탭의 기본 범위와 선택 된 탭의 명령에 대 한 보조 범위의 두 가지 선택 키 범위를 제공 합니다.

다음 이미지는 Word의 두 가지 액세스 키 범위를 보여 줍니다. 첫 번째는 사용자가 탭 및 다른 최상위 명령을 선택할 수 있도록 하는 기본 액세스 키를 표시 하 고, 두 번째는 홈 탭에 대 한 보조 액세스 키를 표시 합니다.

![Microsoft word의 기본 액세스 키](images/accesskeys/primary-access-keys-word.png)
_microsoft word의 기본 액세스_ 키

![Microsoft word의 보조 액세스 키](images/accesskeys/secondary-access-keys-word.png)
microsoft_word의 보조 액세스_ 키

여러 범위의 요소에 대해 액세스 키를 복제할 수 있습니다. 앞의 예제에서 "2"는 주 범위의 실행 취소에 대 한 선택 키이 고, 보조 범위의 "기울임꼴" 이기도 합니다.

여기서는 액세스 키 범위를 정의 하는 방법을 보여 줍니다.

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

![CommandBar의 기본 액세스 키](images/accesskeys/primary-access-keys-commandbar.png)

_명령 모음 주 범위 및 지원 되는 액세스 키_

![CommandBar의 보조 액세스 키](images/accesskeys/secondary-access-keys-commandbar.png)

_CommandBar 보조 범위 및 지원 되는 액세스 키_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 크리에이터 업데이트 및 이전 버전

Windows 10 구성 작성자 업데이트 이전에는 CommandBar와 같은 일부 컨트롤에서 기본 제공 액세스 키 범위를 지원 하지 않았습니다.

다음 예제에서는 액세스 키를 사용 하 여 CommandBar SecondaryCommands를 지 원하는 방법을 보여 줍니다 .이 키는 부모 명령이 호출 된 후에 사용할 수 있습니다 (Word의 리본과 유사).

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

## <a name="avoid-access-key-collisions"></a>액세스 키 충돌 방지

액세스 키 충돌은 동일한 범위에 있는 둘 이상의 요소에 중복 된 액세스 키가 있거나 동일한 영숫자 문자로 시작 하는 경우에 발생 합니다.

시스템은 시각적 트리에 추가 된 첫 번째 요소의 액세스 키를 처리 하 여 중복 된 액세스 키를 확인 하 고 다른 모든 항목은 무시 합니다.

여러 액세스 키가 같은 문자 (예: "A", "A1" 및 "AB")로 시작 하는 경우 시스템은 단일 문자 액세스 키를 처리 하 고 다른 모든 항목을 무시 합니다.

고유 액세스 키를 사용 하거나 명령 범위를 지정 하 여 충돌을 방지 합니다.

## <a name="choose-access-keys"></a>액세스 키 선택

액세스 키를 선택할 때 다음 사항을 고려 하세요.

-   단일 문자를 사용 하 여 키 입력을 최소화 하 고 기본적으로 액셀러레이터 키 지원 (Alt + AccessKey)
-   문자를 세 개 이상 사용 하지 마십시오.
-   액세스 키 충돌 방지
-   문자 "I", 숫자 "1", 문자 "O" 및 숫자 "0"과 같이 다른 문자와 구별 하기 어려운 문자를 사용 하지 않습니다.
-   Word ("File"의 경우 "H", "Home"의 경우 "H")와 같이 인기 있는 다른 앱의 잘 알려진 참조 되는 참조를 사용 합니다.
-   명령 이름의 첫 문자 또는 다시 사용 하는 데 도움이 되는 명령과 가까운 연결을 사용 하는 문자 사용
    -   첫 글자를 이미 할당 한 경우에는 명령 이름의 첫 글자 (삽입의 경우 "N")와 최대한 가까운 문자를 사용 합니다.
    -   명령 이름에서 특수 자음 사용 (보기의 경우 "W")
    -   명령 이름의 모음을 사용 합니다.

## <a name="localize-access-keys"></a>액세스 키 지역화

앱이 여러 언어로 지역화 될 경우에는 **액세스 키를 지역화**하는 것도 고려해 야 합니다. 예를 들어 en-us의 "Home"의 경우 "H"의 경우이 고 "Incio"의 경우 "I"는 es입니다.

태그에서 x:Uid 확장을 사용 하 여 다음과 같이 지역화 된 리소스를 적용 합니다.

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
각 언어에 대 한 리소스는 프로젝트의 해당 문자열 폴더에 추가 됩니다.

![영어 및 스페인어 리소스 문자열 폴더](images/accesskeys/resource-string-folders.png)

_영어 및 스페인어 리소스 문자열 폴더_

지역화 된 액세스 키는 프로젝트 리소스에서 지정 됩니다. resw 파일:

![리소스. resw 파일에 지정 된 AccessKey 속성을 지정 합니다.](images/accesskeys/resource-resw-file.png)

_리소스. resw 파일에 지정 된 AccessKey 속성을 지정 합니다._

자세한 내용은 [UI 리소스 변환](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10)) (영문)을 참조 하세요.

## <a name="key-tip-positioning"></a>키 팁 위치 지정

키 팁은 다른 UI 요소, 기타 주요 팁 및 화면 가장자리의 존재를 고려 하 여 해당 UI 요소에 상대적인 부동 배지로 표시 됩니다.

일반적으로 기본 키 팁 위치는 충분 하며 기본 제공 된 적응 UI 지원 기능을 제공 합니다.

![자동 키 팁 배치의 예](images/accesskeys/auto-keytip-position.png)

_자동 키 팁 배치의 예_

그러나 키 팁 위치 지정을 더 많이 제어 해야 하는 경우에는 다음을 수행 하는 것이 좋습니다.

1.  **명확한 연결 원칙**: 사용자가 컨트롤을 키 팁에 쉽게 연결할 수 있습니다.

    a.  키 팁은 액세스 키 (소유자)가 있는 요소와 **가까워야** 합니다.  
    b.  키 팁은 액세스 키가 있는 **활성화 된 요소** 를 포함 하지 않아야 합니다.   
    c.  키 팁을 소유자에 게 가까이 배치할 수 없으면 소유자와 겹칩니다. 

2.  검색 **기능: 사용자**는 키 팁을 사용 하 여 컨트롤을 신속 하 게 검색할 수 있습니다.

    a.  키 팁은 다른 주요 팁과 **겹치지** 않습니다.  

3.  **간편한 검색:** 사용자는 키 팁을 쉽게 skim 수 있습니다.

    a.  키 팁은 UI 요소와 **나란히 정렬** 되어야 합니다.
    b.  키 팁은 가능한 한 많이 **그룹화** 되어야 합니다. 

### <a name="relative-position"></a>상대 위치

**KeyTipPlacementMode** 속성을 사용 하 여 요소 또는 그룹별 기준으로 키 팁의 배치를 사용자 지정할 수 있습니다.

배치 모드는 위쪽, 아래쪽, 오른쪽, 왼쪽, 숨김, 가운데 및 자동입니다.

![주요 팁 배치 모드](images/accesskeys/keytip-postion-modes.png)

_주요 팁 배치 모드_

컨트롤의 가운데 선은 키 팁의 세로 및 가로 맞춤을 계산 하는 데 사용 됩니다.

다음 예제에서는 StackPanel 컨테이너의 KeyTipPlacementMode 속성을 사용 하 여 컨트롤 그룹의 키 팁 배치를 설정 하는 방법을 보여 줍니다.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>오프셋

키 팁 위치를 보다 세밀 하 게 제어 하기 위해 요소의 KeyTipHorizontalOffset 및 KeyTipVerticalOffset 속성을 사용 합니다.

> [!NOTE]
> KeyTipPlacementMode가 Auto로 설정 된 경우에는 오프셋을 설정할 수 없습니다.

키 팁을 왼쪽 또는 오른쪽으로 이동 하는 정도를 나타내는 KeyTipHorizontalOffset 속성입니다. 예제에서는 단추에 대 한 키 팁 오프셋을 설정 하는 방법을 보여 줍니다.

![주요 팁 배치 모드](images/accesskeys/keytip-offsets.png)

_키 팁에 대 한 세로 및 가로 오프셋 설정_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>화면 가장자리 맞춤 {#screen-가장자리 맞춤입니다. ListParagraph}

키 팁의 위치는 키 팁이 완전히 표시 되도록 화면 가장자리에 따라 자동으로 조정 됩니다. 이 경우 컨트롤과 키 팁 맞춤 지점 간의 거리가 가로 및 세로 오프셋에 지정 된 값과 다를 수 있습니다.

![주요 팁 배치 모드](images/accesskeys/keytips-screen-edge.png)

_화면 가장자리에 따라 키 팁이 자동으로 다시 배치 됩니다._

## <a name="key-tip-style"></a>키 팁 스타일

고대비를 포함 하 여 플랫폼 테마에 대 한 기본 제공 키 팁 지원을 사용 하는 것이 좋습니다.

고유한 키 팁 스타일을 지정 해야 하는 경우 KeyTipFontSize (글꼴 크기), KeyTipFontFamily (글꼴 패밀리), KeyTipBackground (배경), KeyTipForeground (전경), KeyTipPadding (패딩), Keytiporderbrush (테두리 색) 및 KeyTipBorderThemeThickness (테두리 두께)와 같은 응용 프로그램 리소스를 사용 합니다.

![주요 팁 배치 모드](images/accesskeys/keytip-customization.png)

_주요 팁 사용자 지정 옵션_

이 예제에서는 다음 응용 프로그램 리소스를 변경 하는 방법을 보여 줍니다.

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

## <a name="access-keys-and-narrator"></a>액세스 키 및 내레이터

XAML 프레임 워크는 UI 자동화 클라이언트가 사용자 인터페이스의 요소에 대 한 정보를 검색할 수 있도록 하는 자동화 속성을 노출 합니다.

UIElement 또는 TextElement 컨트롤에서 AccessKey 속성을 지정 하는 경우 [Automationproperties. accesskey](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.accesskey) 속성을 사용 하 여이 값을 가져올 수 있습니다. 내레이터와 같은 내게 필요한 옵션 지원 클라이언트는 요소가 포커스를 가질 때마다이 속성의 값을 읽습니다.

## <a name="related-articles"></a>관련된 문서

* [키보드 상호 작용](keyboard-interactions.md)
* [키보드 액셀러레이터](keyboard-accelerators.md)

**샘플**
* [XAML 컨트롤 갤러리 (즉, XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


