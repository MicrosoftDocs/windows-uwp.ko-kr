---
description: 액셀러레이터 키를 사용 하 여 Windows 앱의 유용성 및 접근성을 향상 시키는 방법에 대해 알아봅니다.
title: 바로 가기 키
label: Keyboard accelerators
template: detail.hbs
keywords: 키보드, 액셀러레이터 키, 액셀러레이터 키, 바로 가기 키, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 상호 작용, 게임 패드, 원격
ms.date: 09/24/2020
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 6f712dd8e845b3beb52981be4a17df128c48845d
ms.sourcegitcommit: c5fdcc0779d4b657669948a4eda32ca3ccc7889b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102784744"
---
# <a name="keyboard-accelerators"></a>바로 가기 키

![표면 키보드의 주인공 이미지](images/accelerators/accelerators_hero2.png)

액셀러레이터 키 (또는 키보드 액셀러레이터)는 사용자가 응용 프로그램 UI를 탐색 하지 않고도 일반 작업 또는 명령을 호출할 수 있는 직관적인 방법을 제공 하 여 Windows 응용 프로그램의 유용성 및 접근성을 개선 하는 바로 가기 키입니다.

바로 가기 키를 사용 하 여 Windows 응용 프로그램의 UI를 탐색 하는 방법에 대 한 자세한 내용은 [액세스 키](access-keys.md) 항목을 참조 하세요.

> [!NOTE]
> 키보드는 특정 장애가 있는 사용자에 게 중요 하며 ( [키보드 접근성](../accessibility/keyboard-accessibility.md)참조) 앱과 상호 작용 하는 보다 효율적인 방법으로 선호 하는 사용자를 위한 중요 한 도구 이기도 합니다.

## <a name="overview"></a>개요

액셀러레이터 키에는 일반적으로 f 12의 함수 키, 하나 이상의 보조키와 쌍을 이루는 표준 키의 일부 조합 (CTRL, Shift)이 포함 됩니다.

> [!NOTE]
> UWP 플랫폼 컨트롤에는 기본 제공 키보드 액셀러레이터 키가 있습니다. 예를 들어 ListView는 목록에 있는 모든 항목을 선택 하기 위해 Ctrl + A를 지원 하 고, RichEditBox는 텍스트 상자에 탭을 삽입 하기 위해 Ctrl + Tab을 지원 합니다. 이러한 기본 제공 키보드 액셀러레이터는 **컨트롤 액셀러레이터** 라고 하며 포커스가 요소 또는 자식 중 하나에 있는 경우에만 실행 됩니다. 여기서 설명 하는 키보드 액셀러레이터 Api를 사용 하 여 정의한 액셀러레이터를 **앱 가속기** 라고 합니다.

키보드 액셀러레이터는 모든 작업에 사용할 수 없지만 메뉴에 노출 된 명령과 연결 되는 경우가 많습니다 (메뉴 항목 내용으로 지정 해야 함). 액셀러레이터는 동일한 메뉴 항목이 없는 작업과 연결 될 수도 있습니다. 그러나 사용자는 응용 프로그램 메뉴를 사용 하 여 사용 가능한 명령 집합을 검색 하 고 학습할 수 있으므로 가능한 한 빨리 액셀러레이터를 검색 해야 합니다. 레이블 또는 설정 된 패턴을 사용 하면이 작업을 쉽게 수행할 수 있습니다.

![메뉴 항목 레이블에 있는 키보드 액셀러레이터의 스크린샷](images/accelerators/accelerators_menuitemlabel.png)  
*메뉴 항목 레이블에 설명 된 키보드 액셀러레이터*

## <a name="when-to-use-keyboard-accelerators"></a>키보드 액셀러레이터를 사용 하는 경우

UI에서 적절 한 위치에 키보드 액셀러레이터 키를 지정 하 고 모든 사용자 지정 컨트롤에서 액셀러레이터 키를 지원 하는 것이 좋습니다.

- 키보드 액셀러레이터를 사용 하면 한 번에 하나의 키만 누르거나 마우스를 사용 하는 데 어려움이 있는 사용자를 포함 하 여 화물 차 장애가 있는 사용자가 앱에 보다 쉽게 액세스할 수 있습니다. * *

  잘 디자인 된 키보드 UI는 소프트웨어 접근성의 중요 한 측면입니다. 시각 장애나 특정 거동 장애가 있는 사용자는 키보드 UI를 사용하여 앱을 탐색하고 기능을 조작할 수 있습니다. 이러한 사용자는 마우스를 작동 하지 않고 키보드 기능 도구, 화상 키보드, 화면 enlargers, 화면 판독기, 음성 입력 유틸리티 등의 다양 한 보조 기술을 사용 하는 것이 좋습니다. 이러한 사용자에 게는 포괄적인 명령 범위를 적용 하는 것이 중요 합니다.

- 키보드 액셀러레이터를 사용 하면 키보드를 사용 하는 것을 선호 하는 고급 사용자가 앱을 보다 쉽게 사용할 수 있습니다.

  키보드 기반 명령을 빠르게 입력할 수 있고 키보드에서 손을 제거 하지 않아도 되는 경우 숙련 된 사용자에 게 키보드 사용에 대 한 강력한 기본 설정이 사용 됩니다. 이러한 사용자의 경우 효율성과 일관성은 매우 중요 합니다. comprehensiveness는 가장 자주 사용 되는 명령에만 중요 합니다.

## <a name="specify-a-keyboard-accelerator"></a>키보드 액셀러레이터 지정

[KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) api를 사용 하 여 UWP 앱에서 바로 가기 키를 만듭니다. 이러한 Api를 사용 하면 여러 개의 KeyDown 이벤트를 처리 하 여 누른 키 조합을 검색할 필요가 없으며, 앱 리소스의 액셀러레이터를 지역화할 수 있습니다.

앱에서 가장 일반적인 작업에 대해 키보드 액셀러레이터를 설정 하 고 메뉴 항목 레이블이나 도구 설명을 사용 하 여 문서를 문서화 하는 것이 좋습니다. 이 예제에서는 이름 바꾸기 및 복사 명령에 대해서만 키보드 액셀러레이터 키를 선언 합니다.

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

![도구 설명에 있는 키보드 액셀러레이터의 스크린샷](images/accelerators/accelerators_tooltip.png)  
***도구 설명에 설명 된 키보드 액셀러레이터***

[UIElement](/uwp/api/windows.ui.xaml.uielement) 개체에는 사용자 지정 KeyboardAccelerator 개체를 지정 하 고 키보드 액셀러레이터 키에 대 한 키 입력을 정의 하는 [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 컬렉션인 [KeyboardAccelerators](/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators)이 있습니다.

-   **[키](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** -키보드 액셀러레이터에 사용 되는 [virtualkey](/uwp/api/windows.system.virtualkey) 입니다.

-   **[한정자](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** -키보드 액셀러레이터에 사용 되는 [virtualkeymodifiers](/uwp/api/windows.system.virtualkeymodifiers) 입니다. 한정자를 설정 하지 않으면 기본값은 None입니다.

> [!NOTE]
> 단일 키 (A, Delete, F2, 스페이스바, Esc, 멀티미디어 키) 액셀러레이터 및 멀티 키 액셀러레이터 (Ctrl + Shift + M)가 지원 됩니다. 그러나 게임 패드 가상 키는 지원 되지 않습니다.

## <a name="scoped-accelerators"></a>범위가 지정 된 가속기

일부 액셀러레이터는 특정 범위 에서만 작동 하지만 다른 작업은 앱 전체에서 작동 합니다.

예를 들어 Microsoft Outlook에는 다음과 같은 가속기가 포함 됩니다.
-   Ctrl + B, Ctrl + I 및 ESC는 전자 메일 보내기 양식의 범위 에서만 작동 합니다.
-   Ctrl + 1 및 Ctrl + 2 작업 응용 프로그램 전체

### <a name="context-menus"></a>상황에 맞는 메뉴

상황에 맞는 메뉴 동작은 텍스트 편집기에서 선택한 문자 또는 재생 목록에 있는 노래와 같은 특정 영역이 나 요소에만 영향을 줍니다. 따라서 상황에 맞는 메뉴 항목에 대 한 키보드 액셀러레이터의 범위를 상황에 맞는 메뉴의 부모로 설정 하는 것이 좋습니다.

[ScopeOwner](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) 속성을 사용 하 여 키보드 액셀러레이터의 범위를 지정 합니다. 이 코드는 범위가 지정 된 키보드 액셀러레이터를 사용 하 여 ListView에서 상황에 맞는 메뉴를 구현 하는 방법을 보여 줍니다.

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

KeyboardAccelerators 요소의 ScopeOwner 특성은 액셀러레이터를 전역이 아닌 범위가 지정 된 (기본값은 null 또는 global)로 표시 합니다. 자세한 내용은이 항목의 뒷부분에 나오는 **가속기 확인** 섹션을 참조 하세요.

## <a name="invoke-a-keyboard-accelerator"></a>키보드 액셀러레이터 키 호출 

[KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 개체는 [UIA (UI 자동화) 컨트롤 패턴](/windows/desktop/WinAuto/uiauto-controlpatternsoverview) 을 사용 하 여 액셀러레이터 키가 호출 될 때 작업을 수행 합니다.

UIA [컨트롤 패턴]은 공용 제어 기능을 노출 합니다. 예를 들어 단추 컨트롤은 Click 이벤트를 지원 하기 위해 [Invoke](/windows/desktop/WinAuto/uiauto-implementinginvoke) 컨트롤 패턴을 구현 합니다. 일반적으로 컨트롤은 클릭 하거나, 두 번 클릭 하거나, enter 키, 미리 정의 된 바로 가기 키 또는 키 입력의 다른 조합을 통해 호출 됩니다. 키보드 액셀러레이터 키를 사용 하 여 컨트롤을 호출 하는 경우 XAML 프레임 워크는 컨트롤이 Invoke 컨트롤 패턴을 구현 하는지 확인 하 고, 그럴 경우 활성화 합니다 (KeyboardAcceleratorInvoked 이벤트를 수신 대기 하지 않아도 됨).

다음 예제에서는 단추가 호출 패턴을 구현 하기 때문에 컨트롤 + S가 Click 이벤트를 트리거합니다.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

요소가 여러 컨트롤 패턴을 구현 하는 경우에는 액셀러레이터 키를 통해 하나만 활성화할 수 있습니다. 컨트롤 패턴은 다음과 같이 우선 순위가 지정 됩니다.
1.  Invoke (Button)
2.  토글 (Checkbox)
3.  선택 (ListView)
4.  확장/축소 (ComboBox) 

일치 하는 항목이 없는 경우 액셀러레이터 키가 유효 하지 않고 디버그 메시지가 제공 됩니다 .이 구성 요소에 대 한 자동화 패턴을 찾을 수 없습니다. 호출 된 이벤트에서 원하는 모든 동작을 구현 합니다. 이벤트 처리기에서 처리 됨을 true로 설정 하면이 메시지가 표시 되지 않습니다. ")

## <a name="custom-keyboard-accelerator-behavior"></a>사용자 지정 키보드 액셀러레이터 동작

[KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 개체의 호출 된 이벤트는 액셀러레이터 키가 실행 될 때 발생 합니다. [KeyboardAcceleratorInvokedEventArgs](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) 이벤트 개체는 다음과 같은 속성을 포함 합니다.

- [**Handled**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.handled) (부울):이 값을 True로 설정 하면 컨트롤 패턴을 트리거하는 이벤트가 방지 되 고 액셀러레이터 이벤트 버블링이 중지 됩니다. 기본값은 false입니다.
- [**요소**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.element) (DependencyObject): 액셀러레이터와 연결 된 개체입니다.
- [**KeyboardAccelerator**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.keyboardaccelerator): 호출 된 이벤트를 발생 시키는 데 사용 되는 키보드 액셀러레이터입니다.

여기서는 ListView의 항목에 대 한 키보드 액셀러레이터 컬렉션을 정의 하는 방법과 각 액셀러레이터에 대해 호출 된 이벤트를 처리 하는 방법을 보여 줍니다.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>
```

``` csharp
void SelectAllInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectAll();
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectionMode = ListViewSelectionMode.None;
  MyListView.SelectionMode = ListViewSelectionMode.Multiple;
  args.Handled = true;
}
```

## <a name="override-default-keyboard-behavior"></a>기본 키보드 동작 재정의

일부 컨트롤은 포커스가 있을 때 앱 정의 가속기를 재정의 하는 기본 제공 키보드 액셀러레이터를 지원 합니다. 예를 들어, [텍스트 상자](/uwp/api/windows.ui.xaml.controls.textbox) 에 포커스가 있을 때 컨트롤 + C accelerator는 현재 선택한 텍스트만 복사 합니다. 즉, 앱에서 정의한 액셀러레이터는 무시 되 고 다른 기능은 실행 되지 않습니다.

사용자에 게 친숙 하 고 기대 하는 것으로 인해 기본 제어 동작을 재정의 하지 않는 것이 좋지만 컨트롤의 기본 제공 키보드 액셀러레이터 키를 재정의할 수 있습니다. 다음 예제에서는 [system.windows.forms.control.previewkeydown>](/uwp/api/windows.ui.xaml.uielement.previewkeydown) 이벤트 처리기를 통해 [텍스트 상자](/uwp/api/windows.ui.xaml.controls.textbox) 에 대 한 컨트롤 + C 키보드 액셀러레이터를 재정의 하는 방법을 보여 줍니다. 

``` csharp
 private void TextBlock_PreviewKeyDown(object sender, KeyRoutedEventArgs e)
 {
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(Windows.System.VirtualKey.Control);
    var isCtrlDown = ctrlState == CoreVirtualKeyStates.Down || ctrlState 
        ==  (CoreVirtualKeyStates.Down | CoreVirtualKeyStates.Locked);
    if (isCtrlDown && e.Key == Windows.System.VirtualKey.C)
    {
        // Your custom keyboard accelerator behavior.
        
        e.Handled = true;
    }
 }
```  

## <a name="disable-a-keyboard-accelerator"></a>키보드 액셀러레이터 키 사용 안 함 

컨트롤을 사용할 수 없는 경우 연결 된 액셀러레이터도 사용 하지 않도록 설정 됩니다. 다음 예제에서는 ListView의 IsEnabled 속성이 false로 설정 되어 있으므로 연결 된 컨트롤 + 액셀러레이터를 호출할 수 없습니다.

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
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

부모 및 자식 컨트롤은 같은 액셀러레이터 키를 공유할 수 있습니다. 이 경우 자식에 포커스가 있고 해당 액셀러레이터 키가 사용 되지 않는 경우에도 부모 컨트롤이 호출 될 수 있습니다.

## <a name="screen-readers-and-keyboard-accelerators"></a>화면 판독기 및 키보드 액셀러레이터 

내레이터와 같은 화면 판독기는 사용자에 게 키보드 액셀러레이터 키 조합을 알릴 수 있습니다. 기본적으로이는 각 한정자 (VirtualModifiers 열거형 순서) 다음에 키 ("+" 기호로 구분 됨)가 나옵니다. [AcceleratorKey](/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) automationproperties 연결 된 속성을 통해이를 사용자 지정할 수 있습니다. 하나 이상의 액셀러레이터 키가 지정 된 경우 첫 번째 항목만 알려집니다.

이 예제에서 AutomationProperty AcceleratorKey는 "Control + Shift + A" 문자열을 반환 합니다.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> AcceleratorKey를 설정 하면 키보드 기능이 사용 되지 않습니다. UIA 프레임 워크는 사용 되는 키를 나타냅니다.

## <a name="common-keyboard-accelerators"></a>일반적인 키보드 액셀러레이터

Windows 응용 프로그램에서 바로 가기 키를 일관 되 게 만드는 것이 좋습니다. 사용자는 키보드 액셀러레이터를 기억 하 고 동일 하거나 유사한 결과를 발생 해야 합니다.

앱 간 기능의 차이로 인해 항상 불가능 한 것은 아닙니다.

| **편집 중** | **일반 키보드 액셀러레이터** |
| ------------- | ----------------------------------- |
| 편집 모드 시작 | Ctrl+E |
| 포커스가 있는 컨트롤 또는 창에서 모든 항목 선택 | Ctrl + A |
| 검색 및 바꾸기 | Ctrl+H |
| 실행 취소 | Ctrl + Z |
| 다시 실행 | Ctrl + Y |
| 선택 영역을 삭제 하 고 클립보드에 복사 합니다. | Ctrl + X |
| 선택 영역을 클립보드에 복사 | Ctrl + C, Ctrl + Insert |
| 클립보드의 내용 붙여넣기 | Ctrl + V, Shift + Insert |
| 클립보드의 내용 붙여넣기 (옵션 포함) | Ctrl + Alt + V |
| 항목 이름 바꾸기 | F2 |
| 새 항목 추가 | Ctrl+N |
| 새 보조 항목 추가 | Ctrl + Shift + N |
| 선택한 항목 삭제 (실행 취소 포함) | Del, Ctrl + D |
| 선택한 항목 삭제 (실행 취소 하지 않음) | Shift + Del |
| 굵게 | Ctrl + B |
| 밑줄 | Ctrl + U |
| 기울임꼴 | Ctrl+I |
| **탐색** | |
| 포커스가 있는 컨트롤 또는 창에서 콘텐츠 찾기 | Ctrl+F |
| 다음 검색 결과로 이동 | F3 |
| **기타 작업** | |
| 즐겨찾기 추가 | Ctrl+D | 
| 새로 고침 | F5 또는 Ctrl + R | 
| 확대 | Ctrl + + | 
| 축소 | Ctrl +- | 
| 기본 뷰로 확대/축소 | Ctrl + 0 | 
| 저장 | Ctrl+S | 
| 닫기 | Ctrl+W | 
| 인쇄 | Ctrl+P | 

지역화 된 버전의 Windows에서는 일부 조합이 유효 하지 않습니다. 예를 들어, 스페인어 버전의 Windows에서는 ctrl + N을 Ctrl + B 대신 굵게 사용 합니다. 앱이 지역화 된 경우 지역화 된 키보드 액셀러레이터를 제공 하는 것이 좋습니다.

## <a name="usability-affordances-for-keyboard-accelerators"></a>키보드 액셀러레이터에 대 한 유용성 affordances

### <a name="tooltips"></a>도구 설명

일반적으로 키보드 액셀러레이터는 Windows 응용 프로그램의 UI에 직접 설명 되지 않으므로 사용자가 포커스를 이동할 때 자동으로 표시 되는 [도구 설명](../controls-and-patterns/tooltips.md)및 컨트롤 위로 마우스 포인터를 가져가면 자동으로 검색 기능을 향상 시킬 수 있습니다. 도구 설명에서는 컨트롤에 연결 된 키보드 액셀러레이터 키가 있는지 여부를 확인할 수 있으며,이 경우 액셀러레이터 키 조합이 무엇 인지 확인할 수 있습니다.

**Windows 10, 버전 1803 (4 월 2018 업데이트) 이상**

기본적으로 키보드 액셀러레이터를 선언 하면 [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) 및 [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)를 제외한 모든 컨트롤이 도구 설명에 해당 하는 키 조합을 표시 합니다.

> [!NOTE] 
> 컨트롤에 하나 이상의 액셀러레이터 키가 정의 되어 있으면 첫 번째 항목만 표시 됩니다.

![Ctrl + S 액셀러레이터 키에 대 한 지원을 나타내는 도구 설명이 있는 저장 단추의 스크린샷](images/accelerators/accelerators_tooltip_savebutton_small.png)

*도구 설명의 액셀러레이터 키 콤보*

[단추](/uwp/api/windows.ui.xaml.controls.button), [App바 단추](/uwp/api/windows.ui.xaml.controls.appbarbutton)및 [app togglebutton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 개체의 경우 키보드 액셀러레이터 키가 컨트롤의 기본 도구 설명에 추가 됩니다. [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.appbarbutton) 및 [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) 개체의 경우 플라이 아웃 텍스트와 함께 키보드 액셀러레이터를 표시 합니다.

> [!NOTE]
> 도구 설명 지정 (다음 예제의 Button1 참조)은이 동작을 재정의 합니다.

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

![Windows + B 액셀러레이터 키에 대 한 지원을 나타내는 Button2 위의 도구 설명이 포함 된 Button1, Button2 및 Button3 레이블이 지정 된 세 단추의 스크린샷](images/accelerators/accelerators-button-small.png)

*단추의 기본 도구 설명에 액셀러레이터 키 콤보가 추가 됨*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![디스크 아이콘과 Ctrl + S 액셀러레이터 키가 괄호 안에 추가 된 기본 저장 텍스트가 포함 된 도구 설명이 있는 단추의 스크린샷](images/accelerators/accelerators-appbarbutton-small.png)

*키 입력 콤보가 App바 단추의 기본 도구 설명에 추가 됨*

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

![액셀러레이터 키 combos를 포함 하는 MenuFlyoutItems를 포함 하는 메뉴의 스크린샷](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*MenuFlyoutItem의 텍스트에 추가 된 액셀러레이터 키 콤보*

[Auto](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) 또는 [Hidden](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode)의 두 값을 허용 하는 [KeyboardAcceleratorPlacementMode](/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) 속성을 사용 하 여 프레젠테이션 동작을 제어 합니다.    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

경우에 따라 다른 요소 (일반적으로 컨테이너 개체)를 기준으로 도구 설명을 제공 해야 할 수도 있습니다. 

여기에서는 KeyboardAcceleratorPlacementTarget 속성을 사용 하 여 단추 대신 그리드 컨테이너가 있는 저장 단추의 키보드 액셀러레이터 키 조합을 표시 하는 방법을 보여 줍니다.

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

컨트롤의 레이블을 사용 하 여 컨트롤에 연결 된 키보드 액셀러레이터 키가 있는지 여부를 확인 하는 것이 좋습니다 .이 경우 액셀러레이터 키 조합이 무엇 인지 여부를 확인 하는 것이 좋습니다. 

일부 플랫폼 컨트롤은 기본적으로이 작업을 수행 합니다. 즉, [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) 및 [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) 개체는 기본적으로이 작업을 수행 하 고, [App 단추](/uwp/api/windows.ui.xaml.controls.appbarbutton) 와 [app togglebutton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 은 [CommandBar](/uwp/api/windows.ui.xaml.controls.commandbar)의 오버플로 메뉴에 표시 될 때이를 수행 합니다.

![메뉴 항목 레이블에 설명 된 키보드 액셀러레이터 키입니다.](images/accelerators/accelerators_menuitemlabel.png)  
*메뉴 항목 레이블에 설명 된 키보드 액셀러레이터*

[MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [App 단추](/uwp/api/windows.ui.xaml.controls.appbarbutton)및 [app togglebutton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 컨트롤의 [KeyboardAcceleratorTextOverride](/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) 속성을 통해 레이블의 기본 액셀러레이터 텍스트를 재정의할 수 있습니다 (텍스트 없이 단일 공백을 사용). 

> [!NOTE] 
> 시스템에서 연결 된 키보드를 검색할 수 없는 경우에는 재정의 텍스트가 표시 되지 않습니다 ( [KeyboardPresent](/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) 속성을 통해 직접 확인할 수 있음).

## <a name="advanced-concepts"></a>고급 개념

여기서는 키보드 액셀러레이터의 일부 하위 수준 측면을 검토 합니다.

### <a name="when-an-accelerator-is-invoked"></a>액셀러레이터 키를 호출 하는 경우

액셀러레이터는 두 가지 형식의 키 (한정자 및 비 한정자)로 구성 됩니다. 한정자 키에는 [Virtualkeymodifiers](/uwp/api/Windows.System.VirtualKeyModifiers)를 통해 노출 되는 Shift, Menu, Control 및 Windows 키가 포함 됩니다. 한정자가 아닌 것은 Delete, F3, 스페이스바, Esc, 모든 영숫자 및 문장 부호 키와 같은 가상 키입니다. 키보드 액셀러레이터 키는 하나 이상의 보조키를 누른 채 보조키를 누를 때 호출 됩니다. 예를 들어 사용자가 Ctrl + Shift + M을 누르면 M을 누를 때 프레임 워크에서 한정자 (Ctrl 및 Shift)를 확인 하 고 액셀러레이터 (있는 경우)를 발생 시킵니다.

> [!NOTE]
> 기본적으로 액셀러레이터 autorepeats (예: 사용자가 Ctrl + Shift를 누른 후 M을 누르고 있으면 M이 해제 될 때까지 액셀러레이터 키가 반복적으로 호출 됨). 이 동작은 수정할 수 없습니다.

### <a name="input-event-priority"></a>입력 이벤트 우선 순위
입력 이벤트는 응용 프로그램의 요구 사항에 따라 가로채서 처리할 수 있는 특정 시퀀스에서 발생 합니다. 

#### <a name="the-keydownkeyup-bubbling-event"></a>KeyDown/KeyUp 버블링 이벤트 

XAML에서 입력 버블링 파이프라인은 하나만 있는 것 처럼 키 입력이 처리 됩니다. 이 입력 파이프라인은 KeyDown/KeyUp 이벤트와 문자 입력에 사용 됩니다. 예를 들어 요소에 포커스가 있는 상태에서 사용자가 키를 누르면 요소에서 KeyDown 이벤트가 발생 하 고, 요소의 부모가 오고, 그 뒤에 인수를 사용 해야 합니다. 처리 된 속성이 true입니다.

또한 일부 컨트롤에서 기본 제공 컨트롤 가속기를 구현 하는 데 사용 되는 KeyDown 이벤트입니다. 컨트롤에 키보드 액셀러레이터 키가 있는 경우 KeyDown 이벤트를 처리 합니다. 즉, KeyDown 이벤트 버블링이 없음을 의미 합니다. 예를 들어 RichEditBox는 Ctrl + C를 사용 하 여 복사를 지원 합니다. Ctrl 키를 누르면 KeyDown 이벤트가 발생 하 고 버블링 되지만 사용자가 동시에 C를 누르면 KeyDown 이벤트가 처리 된 것으로 표시 되 고 handledEventsToo ( [UIElement](/uwp/api/windows.ui.xaml.uielement.addhandler) 의 매개 변수가 true로 설정 되지 않은 경우)가 발생 하지 않습니다.

#### <a name="the-characterreceived-event"></a>CharacterReceived 이벤트입니다.

[CharacterReceived](/uwp/api/windows.ui.core.corewindow.CharacterReceived) 이벤트는 TextBox와 같은 텍스트 컨트롤에 대 한 [keydown](/uwp/api/windows.ui.core.corewindow.KeyDown) 이벤트 이후에 발생 하므로 keydown 이벤트 처리기에서 문자 입력을 취소할 수 있습니다.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>System.windows.forms.control.previewkeydown> 및 PreviewKeyUp 이벤트

미리 보기 입력 이벤트는 다른 이벤트 보다 먼저 발생 합니다. 이러한 이벤트를 처리 하지 않으면 포커스가 있는 요소에 대 한 액셀러레이터 키가 실행 된 후 KeyDown 이벤트가 발생 합니다. 두 이벤트 모두 처리 될 때까지 버블링 됩니다.


![키 이벤트 시퀀스 ](images/accelerators/accelerators_keyevents.png)
 ***키 이벤트 시퀀스*** 를 보여 주는 다이어그램

이벤트 순서:

KeyDown 이벤트 미리 보기<br>
…<br>
앱 가속기<br>
OnKeyDown 메서드<br>
KeyDown 이벤트<br>
부모의 앱 가속기<br>
부모에 대 한 OnKeyDown 메서드<br>
부모에 대 한 KeyDown 이벤트<br>
(루트로 거품)<br>
…<br>
CharacterReceived 이벤트<br>
PreviewKeyUp 이벤트<br>
KeyUpEvents<br>

액셀러레이터 이벤트를 처리할 때 KeyDown 이벤트도 처리 된 것으로 표시 됩니다. KeyUp 이벤트는 처리 되지 않은 상태로 유지 됩니다.

### <a name="resolving-accelerators"></a>가속기 확인

위쪽에 포커스가 있는 요소에서 키보드 액셀러레이터 이벤트를 버블링 합니다. 이벤트가 처리 되지 않으면 XAML 프레임 워크는 버블링 경로 외부에서 범위가 없는 다른 앱 가속기를 찾습니다.

동일한 키 조합을 사용 하 여 두 개의 키보드 액셀러레이터 키를 정의한 경우 시각적 트리에서 발견 된 첫 번째 키보드 액셀러레이터 키가 호출 됩니다.

범위가 지정 된 키보드 액셀러레이터는 포커스가 특정 범위 내에 있는 경우에만 호출 됩니다. 예를 들어 수십 개의 컨트롤이 포함 된 표에서 포커스가 그리드 (범위 소유자) 내에 있는 경우에만 컨트롤에 대해 키보드 액셀러레이터를 호출할 수 있습니다.

### <a name="scoping-accelerators-programmatically"></a>프로그래밍 방식으로 가속기 범위 지정

[TryInvokeKeyboardAccelerator](/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) 메서드는 요소의 하위 트리에 있는 일치 하는 액셀러레이터를 모두 호출 합니다.

[OnProcessKeyboardAccelerators](/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) 메서드는 키보드 액셀러레이터 키 보다 먼저 실행 됩니다. 이 메서드는 키, 한정자 및 키보드 액셀러레이터 키가 처리 되는지 여부를 나타내는 부울을 포함 하는 [ProcessKeyboardAcceleratorArgs](/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) 개체를 전달 합니다. 처리 된 것으로 표시 된 경우 키보드 액셀러레이터는 거품 (외부 키보드 액셀러레이터는 호출 되지 않습니다).

> [!NOTE]
> OnProcessKeyboardAccelerators는 처리 여부에 관계 없이 항상 발생 합니다 (OnKeyDown 이벤트와 유사). 이벤트가 처리 된 것으로 표시 되었는지 여부를 확인 해야 합니다.

이 예제에서는 OnProcessKeyboardAccelerators 및 TryInvokeKeyboardAccelerator를 사용 하 여 키보드 액셀러레이터를 Page 개체로 범위를 합니다.

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

### <a name="localize-the-accelerators"></a>액셀러레이터 키 지역화

모든 키보드 액셀러레이터 키를 지역화 하는 것이 좋습니다. XAML 선언에서 표준 UWP 리소스 (. resw) 파일 및 x:Uid 특성을 사용 하 여이 작업을 수행할 수 있습니다. 이 예제에서 Windows 런타임는 리소스를 자동으로 로드 합니다.

![UWP 리소스를 사용한 키보드 액셀러레이터 지역화의 다이어그램 파일 ](images/accelerators/accelerators_localization.png)
 ***키보드 가속기 uwp 리소스 파일을 사용한 지역화***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>프로그래밍 방식으로 가속기 설정

액셀러레이터 키를 프로그래밍 방식으로 정의 하는 예제는 다음과 같습니다.

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
    accelerator.Invoked += handler;
    this.KeyboardAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator는 공유할 수 없습니다. 동일한 KeyboardAccelerator를 여러 요소에 추가할 수 없습니다.

### <a name="override-keyboard-accelerator-behavior"></a>키보드 액셀러레이터 동작 재정의

[KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) 이벤트를 처리 하 여 기본 KeyboardAccelerator 동작을 재정의할 수 있습니다.

이 예제에서는 사용자 지정 ListView 컨트롤에서 "모두 선택" 명령 (Ctrl + A 키보드 액셀러레이터)을 재정의 하는 방법을 보여 줍니다. 또한 처리 된 속성을 true로 설정 하 여 이벤트 버블링을 추가로 중지 합니다.

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

- [키보드 조작](keyboard-interactions.md)
- [액세스 키](access-keys.md)

### <a name="samples"></a>샘플

- [XAML 컨트롤 갤러리](https://github.com/Microsoft/Xaml-Controls-Gallery)
