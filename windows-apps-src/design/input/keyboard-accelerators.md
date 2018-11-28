---
Description: Learn how accelerator keys can improve the usability and accessibility of UWP apps.
title: 바로 가기 키
label: Keyboard accelerators
template: detail.hbs
keywords: 키보드, 바로 가기, 바로 가기 키, 키보드 바로 가기 키, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 조작, 게임 패드, 원격
ms.date: 10/10/2017
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 6f764d15c1bf5a52a6a48a45856daf9031bbd346
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7831628"
---
# <a name="keyboard-accelerators"></a>바로 가기 키

![Surface 키보드](images/accelerators/accelerators_hero2.png)

바로 가기 키는 사용자가 앱 UI를 탐색하지 않고도 일반적인 동작이나 명령을 호출할 수 있는 직관적인 방법을 제공하여 Windows 응용 프로그램의 사용 편의성과 접근성을 향상시키는 키보드 바로 가기 키입니다.

키보드 바로 가기 키를 사용하여 Windows 응용 프로그램의 UI를 탐색하는 방법에 대한 자세한 내용은 [선택키](access-keys.md)를 참조하세요.

> [!NOTE]
> 키보드는 특정 장애([키보드 접근성](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility) 참조)가 있는 사용자에게 필수이며, 앱과 좀 더 효과적으로 상호 작용하는 수단으로 키보드를 선호하는 사용자에게도 중요한 도구입니다.

## <a name="overview"></a>개요

가속기는 일반적으로 기능 키 F1 ~ F12 또는 하나 이상의 보조 키(CTRL, Shift)와 쌍을 이루는 표준 키의 조합을 포함합니다.

> [!NOTE]
> UWP 플랫폼 컨트롤에는 키보드 가속기가 기본으로 제공됩니다. 예를 들어 ListView는 목록의 모든 항목을 선택하기 위해 Ctrl+A를 지원하고 RichEditBox는 텍스트 상자에 Tab을 삽입하기 위해 Ctrl+Tab을 지원합니다. 이러한 기본 제공 키보드 가속기는 **컨트롤 가속기**라고 하며 포커스가 요소 또는 자식 요소 중 하나에 있는 경우에만 실행됩니다. 여기에 설명된 키보드 가속기 API를 사용하여 정의한 가속기를 **앱 가속기**라고 합니다.

키보드 가속기는 모든 작업에 사용할 수 있는 것은 아니지만 종종 메뉴에 표시되는 명령과 연결됩니다(메뉴 항목 콘텐츠로 지정해야 함).가속기는 동일한 메뉴 항목이 없는 작업과 연결할 수도 있습니다. 그러나 사용자가 응용 프로그램의 메뉴를 사용하여 사용 가능한 명령 집합을 검색하고 알아보므로 가능한 쉽게 바로 가기 검색을 시도해야 합니다(레이블 또는 설정된 패턴을 사용하면 도움이 될 수 있음).

![메뉴 항목 레이블에 설명된 바로 가기 키](images/accelerators/accelerators_menuitemlabel.png)  
*메뉴 항목 레이블에 설명된 바로 가기 키*

## <a name="when-to-use-keyboard-accelerators"></a>키보드 가속기를 사용해야 하는 경우

적합한 경우 UI 어디서나 키보드 가속기를 지정하고 모든 사용자 지정 컨트롤에서 가속기를 지원하는 것이 좋습니다.

- 키보드 가속기로 앱 한 번에 하나의 키를 누를 수 있거나 마우스 * 사용에 어려움이 있는 사용자를 포함 하 여 거동 장애가 있는 더 많은 accessiblefor 사용자

  잘 디자인된 키보드 UI는 소프트웨어 접근성의 중요한 요소입니다. 시각 장애나 특정 거동 장애가 있는 사용자는 키보드 UI를 사용하여 앱을 탐색하고 기능을 조작할 수 있습니다. 이러한 사용자는 마우스를 작동할 수 없으며 다양한 보조 기술(예: 키보드 향상 도구, 화상 키보드, 화면 확대기, 화면 낭독 프로그램 및 음성 입력 유틸리티)을 대신 사용할 수 있습니다. 이러한 사용자에게는 포괄적인 명령 범위가 매우 중요합니다.

- 키보드 가속기 앱 키보드로 조작 하는 것을 선호 하는 자세한 usablefor 고급 사용자를 확인 합니다.

  숙련된 사용자는 키보드 기반 명령을 더 빠르게 입력할 수 있고 키보드에서 손을 떼지 않아도 되기 때문에 키보드를 선호하는 경향이 강합니다. 이러한 사용자에게는 효율성과 일관성이 매우 중요합니다. 포괄성은 가장 자주 사용하는 명령에만 중요합니다.

## <a name="specify-a-keyboard-accelerator"></a>키보드 가속기 지정

[KeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) API를 사용하여 UWP 앱에서 키보드 가속기를 만들 수 있습니다. 이러한 API를 사용하면 여러 개의 KeyDown 이벤트를 처리하여 키 조합을 감지할 필요가 없으며 앱 리소스에서 가속기를 지역화할 수 있습니다.

앱의 가장 일반적인 동작에 키보드 가속기를 설정하고 메뉴 항목 레이블 또는 도구 설명을 사용하여 문서화하는 것이 좋습니다. 이 예에서는 Rename 및 Copy 명령에 대해서만 키보드 가속기를 선언합니다.

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![도구 설명에 설명된 키보드 가속기](images/accelerators/accelerators_tooltip.png)  
***도구 설명에 설명된 키보드 가속기***

[UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 개체에는 [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 컬렉션, [KeyboardAccelerators](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators)가 있으며 여기에서 사용자 지정 KeyboardAccelerator 개체를 지정하고 키보드 가속기에 대한 키 입력을 정의합니다.

-   **[키](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** - 키보드 가속기에 사용되는 [VirtualKey](https://docs.microsoft.com/uwp/api/windows.system.virtualkey).

-   **[보조 키](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** - 키보드 가속기에 사용되는 [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/windows.system.virtualkeymodifiers). 보조 키가 설정되지 않은 경우 기본값은 None입니다.

> [!NOTE]
> 단일 키(A, Delete, F2, 스페이스바, Esc, 멀티미디어 키) 가속기 및 다중 키 가속기(Ctrl+Shift+M)가 지원됩니다. 하지만 게임 패드 가상 키는 지원되지 않습니다.

## <a name="scoped-accelerators"></a>범위가 지정된 가속기

다른 가속기가 앱 전체에서 작동하는 한편 일부 가속기는 특정 범위에서만 작동합니다.

예를 들어 Microsoft Outlook에는 다음 가속기가 포함됩니다.
-   Ctrl+B, Ctrl+I 및 ESC는 전자 메일 보내기 양식의 범위에서만 작동합니다.
-   Ctrl+1 및 Ctrl+2는 앱 전체에서 작동합니다.

### <a name="context-menus"></a>상황에 맞는 메뉴

상황에 맞는 메뉴 작업은 텍스트 편집기에서 선택한 문자 또는 재생 목록의 노래와 같은 특정 영역이나 요소에만 영향을 미칩니다. 이러한 이유로 상황에 맞는 메뉴 항목의 키보드 가속기 범위를 상황에 맞는 메뉴의 부모로 설정하는 것이 좋습니다.

[ScopeOwner](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) 속성을 사용하여 키보드 가속기의 범위를 지정합니다. 이 코드는 범위가 지정된 키보드 가속기를 사용하여 ListView에 상황에 맞는 메뉴를 구현하는 방법을 보여 줍니다.

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

MenuFlyoutItem.KeyboardAccelerators 요소의 ScopeOwner 특성은 가속기를 전역이 아닌 지정된 범위로 표시합니다(기본값은 null 또는 전역). 자세한 내용은 이 항목의 후반부에 있는 **바로 가기 해결** 섹션을 참조하세요.

## <a name="invoke-a-keyboard-accelerator"></a>바로 가기 키 호출 

[KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 개체는 [UI 자동화(UIA) 컨트롤 패턴](https://msdn.microsoft.com/library/windows/desktop/ee671194(v=vs.85).aspx)을 사용하여 바로 가기가 호출될 때 작업을 수행합니다.

UIA [컨트롤 패턴]은 공통적인 컨트롤 기능을 공개합니다. 예를 들어 Button 컨트롤은 [Invoke](https://msdn.microsoft.com/library/windows/desktop/ee671279(v=vs.85).aspx) 컨트롤 패턴을 구현하여 Click 이벤트를 지원합니다(일반적으로 컨트롤은 클릭, 두 번 클릭 또는 Enter, 미리 정의된 키보드 바로 가기 키 또는 다른 키 조합을 눌러서 호출됨). 키보드 가속기를 사용하여 컨트롤을 호출하면 XAML 프레임워크는 컨트롤이 Invoke 컨트롤 패턴을 구현하는지 확인하고, 구현하는 경우 이를 활성화합니다(KeyboardAcceleratorInvoked 이벤트를 수신할 필요는 없음).

다음 예제에서는 Button이 Invoke 패턴을 구현하므로 Control+S가 Click 이벤트를 트리거합니다.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

요소가 여러 컨트롤 패턴을 구현하는 경우 하나만 가속기를 통해 활성화될 수 있습니다. 컨트롤 패턴은 다음과 같이 우선 순위가 지정됩니다.
1.  Invoke(Button)
2.  Toggle(Checkbox)
3.  Selection(ListView)
4.  Expand/Collapse(ComboBox) 

일치하는 항목이 없으면 가속기가 유효하지 않으며 디버그 메시지가 제공됩니다("이 구성 요소에 대한 자동화 패턴을 찾을 수 없습니다. Invoked 이벤트에서 원하는 모든 동작을 구현하세요. 이벤트 처리기에서 Handled를 true로 설정하면 이 메시지가 표시되지 않습니다.")

## <a name="custom-keyboard-accelerator-behavior"></a>사용자 지정 키보드 가속기 동작

[KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 개체의 Invoked 이벤트는 가속기가 실행되면 시작됩니다. [KeyboardAcceleratorInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) 이벤트 개체에는 다음 속성이 포함됩니다.
- **Handled**(Boolean): 이 값을 true로 설정하면 컨트롤 패턴을 트리거하는 이벤트를 방지하고 가속기 이벤트 버블링을 중지합니다. 기본값은 false입니다.
- **Element**(DependencyObject): 가속기가 포함된 개체입니다.

여기에서는 키보드 가속기 컬렉션을 정의하는 방법과 Invoked 이벤트를 처리하는 방법을 보여 줍니다.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>   
```

``` csharp
void SelectAllInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  CustomSelectAll(MyListView);
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  Refresh(MyListView);
  args.Handled = true;
}
```

## <a name="override-default-keyboard-behavior"></a>기본 키보드 동작을 재정의

경우에 따라 백스페이스 키 또는 Enter 키와 같은 특정 키의 기본 동작을 재정의 해야 할 수 있습니다. 예를 들면 

## <a name="disable-a-keyboard-accelerator"></a>키보드 가속기 비활성화 

컨트롤이 비활성화된 경우 연결된 가속기도 비활성화됩니다. 다음 예제에서는 ListView의 IsEnabled 속성이 false로 설정되어 있기 때문에 연결된 Control+A 가속기를 호출할 수 없습니다.

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" 
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

부모 및 자식 컨트롤은 동일한 가속기를 공유할 수 있습니다. 이 경우 자식 컨트롤에 포커스가 있고 액셀러레이터가 비활성화된 경우에도 부모 컨트롤을 호출할 수 있습니다.

## <a name="screen-readers-and-keyboard-accelerators"></a>화면 읽기 프로그램 및 키보드 가속기 

내레이터와 같은 화면 읽기 프로그램은 키보드 가속기 키 조합을 사용자에게 알릴 수 있습니다. 기본적으로 이는 각 보조 키(VirtualModifiers 열거형 순서)와 키("+" 기호로 구분) 순서로 위치합니다. [AcceleratorKey](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties 연결된 속성을 통해 사용자 지정할 수 있습니다. 둘 이상의 가속기가 지정되면 첫 번째 가속기만 알려집니다.

이 예제에서 AutomationProperty.AcceleratorKey는 "Control+Shift+A"라는 문자열을 반환합니다.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> AutomationProperties.AcceleratorKey를 설정해도 키보드 기능이 활성화되지는 않으며 UIA 프레임워크에만 사용되는 키를 나타냅니다.

## <a name="common-keyboard-accelerators"></a>일반적인 바로 가기 키

UWP 응용 프로그램에서 키보드 바로 가기를 일관되게 만드는 것이 좋습니다. 사용자는 키보드 가속기를 기억하고 동일한(또는 유사한) 결과를 기대해야 합니다.

이는 앱 간의 기능 차이로 인해 항상 가능한 것은 아닙니다.

| **편집** | **일반적인 바로 가기 키** |
| ------------- | ----------------------------------- |
| 편집 모드 시작 | Ctrl + E |
| 포커스가 설정된 컨트롤이나 창에 있는 모든 항목 선택 | Ctrl + A |
| 검색 및 바꾸기 | Ctrl + H |
| 실행 취소 | Ctrl + Z |
| 다시 실행 | Ctrl + Y |
| 선택 항목을 삭제하고 클립보드에 복사 | Ctrl + X |
| 클립보드로 선택 영역 복사 | Ctrl + C, Ctrl + Insert |
| 클립보드의 콘텐츠 붙여 넣기 | Ctrl + V, Shift + Insert |
| 클립보드의 콘텐츠 붙여 넣기(옵션 포함) | Ctrl + Alt + V |
| 항목 이름 바꾸기 | F2 |
| 새 항목 추가 | Ctrl + N |
| 새 보조 항목 추가 | Ctrl + Shift + N |
| 선택한 항목 삭제(실행 취소 포함) | Del, Ctrl+D |
| 선택한 항목 삭제(실행 취소 불포함) | Shift + Del |
| 굵게 | Ctrl + B |
| 밑줄 | Ctrl + U |
| 기울임꼴 | Ctrl + I |

| **탐색** | |
| ------------- | ----------------------------------- |
| 포커스가 설정된 컨트롤 또는 창에서 콘텐츠 찾기 | Ctrl + F |
| 다음 검색 결과로 이동 | F3 |

| **다른 작업** | |
| ------------- | ----------------------------------- |
| 즐겨찾기 추가 | Ctrl + D | 
| 새로 고침 | F5 또는 Ctrl + R | 
| 확대 | Ctrl + + | 
| 축소 | Ctrl + - | 
| 기본 보기로 확대/축소 | Ctrl + 0 | 
| 저장 | Ctrl + S | 
| 닫기 | Ctrl + W | 
| 인쇄 | Ctrl + P | 

일부 조합은 Windows의 지역화된 버전에 유효하지 않습니다. 예를 들어, 스페인어 버전의 Windows에서는 '굵게'에 대해 Ctrl+N이 Ctrl+B 대신 사용됩니다. 앱이 지역화되어 있는 경우 지역화된 바로 가기 키를 제공하는 것이 좋습니다.

## <a name="usability-affordances-for-keyboard-accelerators"></a>바로 가기 키의 유용성 기능

### <a name="tooltips"></a>도구 설명

일반적으로 바로 가기 키는 UWP 응용 프로그램의 UI에 직접 설명되어 있지는 않지만, [도구 설명](../controls-and-patterns/tooltips.md)을 통해 사용자에게 보다 잘 표시되도록 할 수 있습니다. 도구 설명은 사용자가 포커스를 이동하거나, 누르고 있거나, 마우스 포인터를 컨트롤 위로 이동하면 자동으로 표시됩니다. 도구 설명을 통해 컨트롤에 연결된 바로 가기 키가 있는지 여부를 확인할 수 있으며, 있는 경우 바로 가기 키 조합이 표시됩니다.

**Windows 10, 버전 1803 (2018 년 4 월 업데이트) 이상**

기본적으로 키보드 가속기를 선언 하는 경우 모든 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) 및 [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) (제외는 해당 키 조합이 표시 도구 설명에 있습니다.

> [!NOTE] 
> 컨트롤에 둘 이상의 가속기 정의 하는 경우 첫 번째만 표시 됩니다.

![바로 가기 키 도구 설명](images/accelerators/accelerators_tooltip_savebutton_small.png)

*도구 설명의 바로 가기 키 조합*

[단추](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)및 [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 개체의 경우 키보드 가속기는 컨트롤의 기본 도구 설명에 추가 됩니다. [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 및 [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) 개체, 키보드 가속기 플라이 아웃 텍스트와 함께 표시 됩니다.

> [!NOTE]
> 도구 설명 지정 (다음 예에서 Button1 참조)이이 동작을 재정의 합니다.

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![바로 가기 키 도구 설명](images/accelerators/accelerators-button-small.png)

*버튼의 기본 도구 설명에 추가 하는 바로 가기 키 조합*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![바로 가기 키 도구 설명](images/accelerators/accelerators-appbarbutton-small.png)

*AppBarButton의 기본 도구 설명에 추가 하는 바로 가기 키 조합*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![바로 가기 키 도구 설명](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*MenuFlyoutItem의 텍스트에 추가 하는 바로 가기 키 조합*

[KeyboardAcceleratorPlacementMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) 속성([자동](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) 또는 [숨김](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode)의 두 값 허용)을 사용하면 표시 동작을 제어할 수 있습니다.    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

경우에 따라 다른 요소(일반적으로 컨테이너 개체)를 기준으로 도구 설명을 표시해야 할 수 있습니다. 예를 들어, 피벗 컨트롤 피벗 헤더가 있는 PivotItem에 대한 도구 설명을 표시하는 피벗 컨트롤이 있습니다. 

다음은 KeyboardAcceleratorPlacementTarget 속성을 사용하여 단추 대신 그리드 컨테이너를 사용하여 저장 단추에 대한 바로 가기 키 조합을 표시하는 방법입니다.

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>레이블

경우에 따라 컨트롤의 레이블을 사용하여 컨트롤에 연결된 바로 가기 키의 유무를 확인하고, 있는 경우 바로 가기 키가 무엇인지를 확인하는 것이 좋을 수 있습니다. 

일부 플랫폼 컨트롤은 기본적으로 이러한 작업을 수행합니다(특히, [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) 및 [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) 개체). 반면, [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 및 [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton)은 [명령 모음](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar)의 오버플로 메뉴에 표시될 때에 이를 수행합니다.

![메뉴 항목 레이블에 설명된 바로 가기 키](images/accelerators/accelerators_menuitemlabel.png)  
*메뉴 항목 레이블에 설명된 바로 가기 키*

[MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 및 [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 컨트롤의 [KeyboardAcceleratorTextOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) 속성을 통해 레이블의 기본 바로 가기 텍스트를 재정의할 수 있습니다(텍스트가 없는 경우 빈 칸 하나 사용). 

> [!NOTE] 
> 시스템에서 연결된 키보드를 검색할 수 없는 경우 재정의 텍스트가 표시되지 않습니다[KeyboardPresent](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) 속성을 통해 이를 자체적으로 확인할 수 있음).

## <a name="advanced-concepts"></a>고급 개념

여기에서는 바로 가기 키의 몇 가지 하위 수준 측면을 검토합니다.

### <a name="when-an-accelerator-is-invoked"></a>가속기가 호출될 때

가속기는 보조 키 및 비보조 키의 두 가지 유형의 키로 구성됩니다. 보조 키는 [VirtualKeyModifiers](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.System.VirtualKeyModifiers)를 통해 공개되는 Shift, Menu, Control 및 Windows 키를 포함합니다. 비보조 키는 Delete, F3, 스페이스바, Esc, 모든 영숫자 및 문장 부호 키와 같은 가상 키입니다. 키보드 가속기는 사용자가 하나 이상의 보조 키를 누른 상태에서 비 보조 키를 누르면 호출됩니다. 예를 들어 사용자가 Ctrl+Shift+M을 누르는 경우, 사용자가 M을 누를 때 프레임워크는 보조 키(Ctrl 및 Shift)를 확인하고 가속기가 있으면 이를 실행합니다.

> [!NOTE]
> 기본적으로 가속기는 자동 반복합니다(예: 사용자가 Ctrl+Shift를 누른 다음 M을 누르고 있으면 M이 해제될 때까지 가속기가 반복적으로 호출됨). 이 동작은 수정할 수 없습니다.

### <a name="input-event-priority"></a>입력 이벤트 우선 순위
입력 이벤트는 앱의 요구 사항에 따라 가로채고 처리할 수 있는 특정 순서로 발생합니다. 

#### <a name="the-keydownkeyup-bubbling-event"></a>KeyDown/KeyUp 버블링 이벤트 

XAML에서는 입력 버블링 파이프라인이 하나만 있는 것처럼 키 입력이 처리됩니다. 이 입력 파이프 라인은 KeyDown/KeyUp 이벤트 및 문자 입력에 사용됩니다. 예를 들어, 요소에 포커스가 있고 사용자가 키를 누른 경우, KeyDown 이벤트가 해당 요소 및 해당 요소의 부모 순서로 발생하며 args.Handled 속성이 true가 될 때까지 트리를 위로 이동합니다.

또한 KeyDown 이벤트는 일부 컨트롤에서 기본 제공 컨트롤 가속기를 구현하는 데 사용됩니다. 컨트롤에 키보드 가속기가 있으면 KeyDown 이벤트를 처리하므로 KeyDown 이벤트 버블링이 발생하지 않습니다. 예를 들어 RichEditBox는 Ctrl+C로 복사를 지원합니다. Ctrl을 누르면 KeyDown 이벤트가 시작되고 버블링이 발생하지만 사용자가 동시에 C를 누르면 KeyDown 이벤트가 Handled로 표시되고 발생하지 않습니다([UIElement.AddHandler](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.AddHandler)의 handledEventsToo 매개 변수가 true로 설정되지 않은 경우).

#### <a name="the-characterreceived-event"></a>CharacterReceived 이벤트

[CharacterReceived](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.CharacterReceived) 이벤트는 TextBox와 같은 텍스트 컨트롤에 대한 [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.KeyDown) 이벤트 이후 실행되며, KeyDown 이벤트 처리기에서 문자 입력을 취소할 수 있습니다.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>PreviewKeyDown 및 PreviewKeyUp 이벤트

미리 보기 입력 이벤트는 다른 이벤트보다 먼저 발생합니다. 이러한 이벤트를 처리하지 않으면 포커스가 있는 요소의 가속기가 실행된 다음 KeyDown 이벤트가 발생합니다. 두 이벤트 모두 처리될 때까지 버블링됩니다.


![키 이벤트 시퀀스](images/accelerators/accelerators_keyevents.png)
***키 이벤트 시퀀스***

이벤트 순서:

Preview KeyDown events …
App accelerator OnKeyDown method KeyDown event App accelerators on the parent OnKeyDown method on the parent KeyDown event on the parent (Bubbles to the root) …
CharacterReceived event PreviewKeyUp events KeyUpEvents

가속기 이벤트가 처리되면 KeyDown 이벤트도 처리된 것으로 표시됩니다. KeyUp 이벤트는 처리되지 않은 상태로 유지됩니다.

### <a name="resolving-accelerators"></a>가속기 해결

키보드 가속기 이벤트는 포커스가 있는 요소에서 루트까지 버블링합니다. 이벤트가 처리되지 않으면 XAML 프레임워크는 버블링 경로 외부의 다른 범위가 지정되지 않은 앱 가속기를 찾습니다.

두 개의 키보드 가속기가 동일한 키 조합으로 정의되면 시각적 트리에서 발견된 첫 번째 키보드 가속기가 호출됩니다.

범위가 지정된 키보드 가속기는 포커스가 특정 범위 안에 있는 경우에만 호출됩니다. 예를 들어, 수십 개의 컨트롤이 포함된 Grid에서는 포커스가 Grid(범위 소유자) 내에 있는 경우에만 키보드 가속기를 컨트롤에 대해 호출할 수 있습니다.

### <a name="scoping-accelerators-programmatically"></a>프로그래밍 방식으로 가속기 범위 지정

[UIElement.TryInvokeKeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) 메서드는 요소의 하위 트리에서 일치하는 모든 가속기를 호출합니다.

[UIElement.OnProcessKeyboardAccelerators](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) 메서드는 키보드 가속기보다 먼저 실행됩니다. 이 메서드는 키, 보조 키 및 키보드 가속기의 처리 여부를 나타내는 부울을 포함하는 [ProcessKeyboardAcceleratorArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs)를 전달합니다. 처리된 것으로 표시된 경우 키보드 가속기가 버블링됩니다(외부 키보드 가속기가 호출되지 않음).

> [!NOTE]
> OnProcessKeyboardAccelerator는 처리되거나 처리되지 않더라도 항상 실행됩니다(OnKeyDown 이벤트와 유사함). 이벤트가 처리된 것으로 표시되었는지 확인해야 합니다.

이 예제에서는 OnProcessKeyboardAccelerators 및 TryInvokeKeyboardAccelerator를 사용하여 키보드 가속기를 Page 개체에 적용합니다.

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>가속기 지역화

모든 키보드 가속기를 지역화하는 것이 좋습니다. 이 작업은 XAML 선언의 표준 UWP 리소스(.resw) 파일과 x:Uid 특성을 사용하여 수행할 수 있습니다. 이 예에서 Windows 런타임은 자동으로 리소스를 로드합니다.

![UWP 리소스 파일을 사용한 키보드 가속기 지역화](images/accelerators/accelerators_localization.png)
***UWP 리소스 파일을 사용한 키보드 가속기 지역화***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>프로그래밍 방식으로 가속기 설정

다음은 가속기를 프로그래밍 방식으로 정의하는 예제입니다.

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked = handler;
    this.KeyAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator는 공유할 수 없으며, 동일한 KeyboardAccelerator를 여러 요소에 추가할 수 없습니다.

### <a name="override-keyboard-accelerator-behavior"></a>바로 가기 키 동작 재정의

[KeyboardAccelerator.Invoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) 이벤트가 기본 KeyboardAccelerator 동작을 재정의하도록 처리할 수 있습니다.

다음은 사용자 지정 ListView 컨트롤에서 "모두 선택" 명령(Ctrl+A 바로 가기 키)을 재정의하는 방법을 보여주는 예입니다. 여기에서는 또한 이벤트 버블링이 추가로 발생하지 않도록 Handled 속성을 true로 설정했습니다.

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>관련 문서

* [키보드 조작](keyboard-interactions.md)
* [선택키](access-keys.md)

**샘플**
* [XAML 컨트롤 갤러리 (일명 XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


 

