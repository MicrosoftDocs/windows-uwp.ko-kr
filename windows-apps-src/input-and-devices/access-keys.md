---
author: Karl-Bridge-Microsoft
Description: "사용자가 키보드를 사용하여 UI 요소를 탐색할 수 있도록 탭 탐색과 선택키를 사용한 키보드 액세스를 가능하게 합니다."
title: "선택키"
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Access keys
template: detail.hbs
keywords: "선택키, 키보드, 접근성"
translationtype: Human Translation
ms.sourcegitcommit: 2b6b1d7b1755aad4d75a29413d989c6e8112128a
ms.openlocfilehash: dfe89e4d4fd089dde6b7b307325b8fe43de82c10

---

# <a name="access-keys"></a>선택키

행동 장애가 있는 사용자와 같이 마우스를 사용하기 어려운 사용자는 키보드를 사용하여 앱을 탐색 및 조작하는 경우가 많습니다.  XAML 프레임워크를 사용하면 탭 탐색과 선택키를 통해 키보드에서 UI 요소에 액세스할 수 있습니다.

- 탭 탐색은 사용자가 키보드의 Tab 키와 화살표 키를 사용하여 UI 요소 간에 포커스를 이동할 수 있는 기본 키보드 접근성 어포던스(기본적으로 사용)입니다.
- 선택키는 키보드 한정자(Alt 키)와 하나 이상의 영숫자 키(일반적으로 명령과 연결된 문자)의 조합을 사용하여 앱 명령에 빠르게 액세스할 수 있도록 앱에서 구현하는 보충 접근성 어포던스입니다. 일반적인 선택키에는 파일 메뉴를 여는 _Alt+F_, 왼쪽에 맞추는 _Alt+AL_ 등이 있습니다.  

키보드 탐색과 접근성에 대한 자세한 내용은 [키보드 조작](https://msdn.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) 및 [키보드 접근성](https://msdn.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)을 참조하세요. 이 문서에서는 이러한 문서에 설명된 개념을 알고 있다고 가정합니다.

## <a name="access-key-overview"></a>선택키 개요

선택키를 사용하면 화살표 키와 Tab 키를 반복적으로 누르지 않고도 키보드를 사용하여 직접 단추를 호출하거나 포커스를 설정할 수 있습니다. 선택키는 쉽게 검색할 수 있어야 하므로 선택키가 있는 컨트롤 위의 부동 배지 등과 같이 UI에서 직접 문서화해야 합니다.

![Microsoft Word의 선택키 및 연결된 키 팁의 예](images/keyboard/accesskeys-keytips.png)

_그림 1: Microsoft Word의 선택키 및 연결된 키 팁의 예_

선택키는 UI 요소와 연결된 하나 이상의 영숫자 문자입니다. 예를 들어 Microsoft Word에서는 _H_를 홈 탭, _2_를 실행 취소 단추, _JI_를 그리기 탭에 사용합니다.

**선택키 범위**

선택키는 특정 범위에 속합니다. 예를 들어 그림 1에서 _F_, _H_, _N_ 및 _JI_는 페이지 범위에 속합니다.  사용자가 _H_를 누르면 범위가 홈 탭의 범위로 변경되고 해당 선택키가 그림 2와 같이 표시됩니다. 선택키 _V_, _FP_, _FF_ 및 _FS_는 홈 탭의 범위에 속합니다.

![Microsoft Word의 홈 탭 범위에 대한 선택키 및 연결된 키 팁의 예](images/keyboard/accesskeys-keytips-hometab.png)

_그림 2: Microsoft Word의 홈 탭 범위에 대한 선택키 및 연결된 키 팁의 예_

두 요소가 서로 다른 범위에 속하는 경우 동일한 선택키를 가질 수 있습니다. 예를 들어 _2_는 페이지 범위에서 실행 취소의 선택키(그림 1)인 동시에 홈 탭 범위에서 기울임꼴의 선택키(그림 2)입니다. 다른 범위를 지정하지 않을 경우 모든 선택키는 기본 범위에 속합니다.

**선택키 시퀀스**

선택키 조합은 일반적으로 키를 동시에 누르는 대신 한 번에 하나씩 키를 눌러 작업을 수행합니다. 이에 대한 예외는 다음 섹션에서 설명합니다. 작업을 수행하는 데 필요한 키 입력 시퀀스가 _선택키 시퀀스_입니다. 사용자가 Alt 키를 누르면 선택키 시퀀스가 시작됩니다. 사용자가 선택키 시퀀스의 마지막 키를 누르면 선택키가 호출됩니다. 예를 들어 Word에서 보기 탭을 열려면 선택키 시퀀스인 _Alt, W_를 누릅니다.

사용자는 선택키 시퀀스에서 여러 개의 선택키를 호출할 수 있습니다. 예를 들어 Word 문서에서 서식 복사를 열려면 Alt 키를 눌러 시퀀스를 초기화하고 _H_ 키를 눌러 홈 섹션으로 이동한 다음 선택키 범위를 변경하고 _F_, _P_ 키를 차례로 누릅니다. _H_ 및 _FP_ 키는 각각 홈 탭과 서식 복사 단추의 선택키입니다.

호출된 후 선택키 시퀀스를 완료하는 요소도 있고(예: 서식 복사 단추) 완료하지 않는 요소도 있습니다(예: 홈 탭). 선택키를 호출하면 명령이 실행되거나, 포커스가 이동되거나, 선택키 범위가 변경되거나, 다른 연결된 작업이 수행됩니다.

## <a name="access-key-user-interaction"></a>선택키 사용자 조작

선택키 API를 이해하려면 먼저 사용자 조작 모델을 이해해야 합니다. 다음은 선택키 사용자 조작 모델에 대한 요약입니다.

- 사용자가 Alt 키를 누르면 입력 컨트롤에 포커스가 있는 경우에도 선택키 시퀀스가 시작됩니다. 그런 다음 사용자는 선택키를 눌러 연결된 작업을 호출할 수 있습니다. 이 사용자 조작을 사용하는 경우 Alt 키를 누를 때 표시되는 시각적 어포던스(예: 부동 배지)로 사용 가능한 선택키를 UI 내에서 문서화해야 합니다.
- 사용자가 Alt 키와 선택키를 동시에 누르면 선택키가 즉시 호출됩니다. 이는 Alt+_선택키_로 정의된 바로 가기 키를 사용하는 것과 비슷합니다. 이 경우 선택키의 시각적 어포던스가 표시되지 않습니다. 그러나 선택키 호출 시 선택키 범위가 변경될 수 있습니다. 이 경우 선택키 시퀀스가 시작되고 새 범위에 대한 시각적 어포던스가 표시됩니다.
    > [!NOTE]
    > 한 문자로 이루어진 선택키만 이 사용자 조작을 이용할 수 있습니다. 둘 이상의 문자로 이루어진 선택 키에 대해서는 Alt+_선택키_ 조합이 지원되지 않습니다.    
- 일부 문자를 공유하는 여러 개의 다중 문자 선택키가 있을 경우 사용자가 공유 문자를 누르면 선택키가 필터링됩니다. 예를 들어 _A1_, _A2_, _C_라는 세 개의 선택키가 표시되어 있다고 가정합니다. 사용자가 _A_를 누르면 _A1_ 및 _A2_ 선택키만 표시되고 C에 대한 시각적 어포던스는 숨겨집니다.
- Esc 키를 누르면 한 수준의 필터링이 제거됩니다. 예를 들어 _B_, _ABC_, _ACD_, _ABD_라는 선택키가 있고 사용자가 _A_를 누르면 _ABC_, _ACD_ 및 _ABD_만 표시됩니다. 그런 다음 사용자가 _B_를 누르면 _ABC_ 및 _ABD_가 표시됩니다. 사용자가 Esc 키를 누르면 한 수준의 필터링이 제거되고 _ABC_, _ACD_ 및 _ABD_ 선택키가 표시됩니다. 사용자가 Esc 키를 다시 누르면 다른 한 수준의 필터링이 제거되고 _B_, _ABC_, _ACD_, _ABD_ 등의 모든 선택키가 활성화되고 해당 시각적 어포던스가 표시됩니다.
- Esc 키를 누르면 이전 범위로 다시 이동합니다. 많은 명령이 있는 앱에서 탐색하기 쉽도록 선택키가 여러 범위에 속할 수 있습니다. 선택키 시퀀스는 항상 주 범위에서 시작합니다. 특정 UI 요소를 범위 소유자로 지정하는 선택키를 제외하고 모든 선택키는 주 범위에 속합니다. 사용자가 범위 소유자인 요소의 선택키를 호출하면 XAML 프레임워크가 자동으로 범위를 해당 요소로 이동하고 내부 선택키 탐색 스택에 추가합니다. Esc 키를 누르면 선택키 탐색 스택을 뒤로 이동합니다.
- 선택키 시퀀스를 해제하는 방법에는 여러 가지가 있습니다.
    - 사용자는 Alt 키를 눌러 진행 중인 선택키 시퀀스를 해제할 수 있습니다. Alt 키를 누르면 선택키 시퀀스가 시작된다는 것도 기억하세요.
    - 주 범위에 있고 필터링되지 않은 경우 Esc 키를 누르면 선택키 시퀀스가 해제됩니다.
        > [!NOTE]
        > Esc 키 입력은 처리를 위해 UI 계층에도 전달됩니다.
- Tab 키를 누르면 선택키 시퀀스가 해제되고 탭 탐색으로 돌아갑니다.
- Enter 키를 누르면 선택키 시퀀스가 해제되고 포커스가 있는 요소에 키 입력을 보냅니다.
- 화살표 키를 누르면 선택키 시퀀스가 해제되고 포커스가 있는 요소에 키 입력을 보냅니다.
- 마우스 클릭이나 터치와 같은 포인터 아래로 이동 이벤트는 선택키 시퀀스를 해제합니다.
- 기본적으로 선택키를 호출하면 선택키 시퀀스가 해제됩니다.  그러나 [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 속성을 **false**로 설정하여 이 동작을 재정의할 수 있습니다.
- DFA(결정적 유한 오토마톤)가 불가능하면 선택키 충돌이 발생합니다. 선택키 충돌은 바람직하지 않지만 많은 명령, 지역화 문제 또는 선택키의 런타임 생성으로 인해 발생할 수 있습니다.

 충돌이 발생하는 다음 두 가지 경우가 있습니다.
 - 두 UI 요소에 동일한 선택키 값이 있고 동일한 선택키 범위에 속하는 경우. 예를 들어 선택키 _A1_이 `button1`에 사용되고 선택 키 _A1_이 기본 범위에 속하는 `button2`입니다. 이 경우 시스템은 시각적 트리에 추가된 첫 번째 요소의 선택키를 처리하여 충돌을 해결합니다. 나머지는 무시됩니다.
 - 동일한 선택키 범위에 둘 이상의 계산 옵션이 있는 경우. 예를 들어 _A_와 _A1_이 있습니다. 사용자가 _A_를 누를 경우 시스템에 _A_ 선택키를 호출하거나 계속 진행하여 _A1_ 선택키의 A 문자를 사용하는 두 가지 옵션이 있습니다. 이 경우 시스템은 오토마톤을 통해 도달한 첫 번째 선택키 호출만 처리합니다. 예를 들어 _A_와 _A1_이 있을 경우 시스템은 _A_만 호출합니다.
-   사용자가 선택키 시퀀스에서 잘못된 선택키 값을 누르면 아무 작업도 수행되지 않습니다. 선택키 시퀀스에서 유효한 선택키로 간주되는 키에는 다음 두 가지 범주가 있습니다.
 - 선택키 시퀀스를 종료하는 특수 키: Esc, Alt, 화살표 키, Enter, Tab 등입니다.
 - 선택키에 할당된 영숫자 문자

## <a name="access-key-apis"></a>선택키 API

선택키 사용자 조작을 지원하기 위해 XAML 프레임워크는 여기에 설명된 API를 제공합니다.

**AccessKeyManager**

[AccessKeyManager](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.aspx)는 선택키가 표시되거나 숨겨진 경우 UI를 관리하는 데 사용할 수 있는 도우미 클래스입니다. [IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx) 이벤트는 앱이 선택키 시퀀스를 시작하고 종료할 때마다 발생합니다. [IsDisplayModeEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabled.aspx) 속성을 쿼리하여 시각적 어포던스를 표시할지 또는 숨길지를 결정할 수 있습니다.  [ExitDisplayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.exitdisplaymode.aspx)를 호출하여 강제로 선택키 시퀀스를 해제할 수도 있습니다.

> [!NOTE]
> 선택키의 시각적 개체에 대한 기본 제공 구현은 없습니다. 직접 제공해야 합니다.  

**AccessKey**

[AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) 속성을 사용하면 UIElement 또는 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.accesskey.aspx)에 선택키를 지정할 수 있습니다. 두 요소의 선택키와 범위가 같은 경우 시각적 트리에 추가된 첫 번째 요소만 처리됩니다.

XAML 프레임워크에서 선택키를 처리하도록 하려면 시각적 트리에서 UI 요소를 실현해야 합니다. 시각적 트리에 선택키를 가진 요소가 없으면 선택키 이벤트가 발생하지 않습니다.

선택키 API는 생성되기 위해 두 개의 키 입력이 필요한 문자를 지원하지 않습니다. 개별 문자는 특정 언어의 기본 자판 배열에 있는 키와 일치해야 합니다.  

**AccessKeyDisplayRequested/Dismissed**

[AccessKeyDisplayRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplayrequested.aspx) 및 [AccessKeyDisplayDismissed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplaydismissed.aspx) 이벤트는 선택키의 시각적 어포던스를 표시하거나 해제해야 할 때 발생합니다. [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) 속성이 **Collapsed**로 설정된 요소에 대해서는 이러한 이벤트가 발생하지 않습니다. 선택키 시퀀스 중 사용자가 선택키에서 사용되는 문자를 누를 때마다 AccessKeyDisplayRequested 이벤트가 발생합니다. 예를 들어 선택키가 _AB_로 설정된 경우 이 이벤트는 사용자가 Alt 키를 누를 때 발생하고 _A_를 누를 때 다시 발생합니다. 사용자가 _B_를 누르면 AccessKeyDisplayDismissed 이벤트가 발생합니다.

**AccessKeyInvoked**

[AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) 이벤트는 사용자가 선택키의 마지막 문자에 도달할 때 발생합니다. 선택키는 하나 또는 여러 문자로 구성될 수 있습니다. 예를 들어 선택키 _A_와 _BC_의 경우 사용자가 _Alt, A_ 또는 _Alt, B, C_를 누를 때는 이벤트가 발생하지만 _Alt, B_만 누를 때는 발생하지 않습니다. 이 이벤트는 키를 놓을 때가 아니라 키를 누를 때 발생합니다.

**IsAccessKeyScope**

[IsAccessKeyScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.isaccesskeyscope.aspx) 속성을 사용하면 UIElement를 선택키 범위의 루트로 지정할 수 있습니다. 이 요소에 대해서는 AccessKeyDisplayRequested 이벤트가 발생하지만 자식 요소에 대해서는 발생하지 않습니다. 사용자가 이 요소를 호출하면 XAML 프레임워크가 자동으로 범위를 변경하고 자식 요소에서는 AccessKeyDisplayRequested 이벤트를, 다른 UI 요소(부모 요소 포함)에서는 AccessKeyDisplayDismissed 이벤트를 발생합니다.  범위를 변경할 때는 선택키 시퀀스가 종료되지 않습니다.

**AccessKeyScopeOwner**

요소가 시각적 트리에서 부모 요소가 아닌 다른 요소(원본)의 범위에 참여하도록 하려면 [AccessKeyScopeOwner](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyscopeowner.aspx) 속성을 설정합니다. AccessKeyScopeOwner 속성에 바인딩된 요소의 IsAccessKeyScope는 **true**로 설정되어 있어야 합니다. 설정되지 않은 경우 예외가 발생합니다.

**ExitDisplayModeOnAccessKeyInvoked**

기본적으로 요소가 범위 소유자가 아닌 경우 선택키를 호출하면 선택키 시퀀스가 완료되고 [AccessKeyManager.IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx) 이벤트가 발생합니다. [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 속성을 **false**로 설정하여 이 동작을 재정의하고 호출된 후 선택키 시퀀스를 종료하지 않도록 할 수 있습니다. 이 속성은 [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 및 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.exitdisplaymodeonaccesskeyinvoked.aspx) 둘 다에 있습니다.

> [!NOTE]
> 요소가 범위 소유자이면(`IsAccessKeyScope="True"`) 앱이 새 선택키 범위를 시작하며 IsDisplayModeEnabledChanged 이벤트가 발생하지 않습니다.

**지역화**

선택키를 여러 언어로 지역화하고 [ResourceLoader](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.resources.resourceloader.aspx) API를 사용하여 런타임에 로드할 수 있습니다.

## <a name="control-patterns-used-when-an-access-key-is-invoked"></a>선택키를 호출할 때 사용되는 컨트롤 패턴

컨트롤 패턴은 공용 컨트롤 기능을 노출하는 인터페이스 구현입니다. 예를 들어 단추는 **Invoke** 컨트롤 패턴을 구현하고 이 패턴은 **클릭** 이벤트를 발생합니다. 선택키를 호출하면 XAML 프레임워크에서 호출된 요소가 컨트롤 패턴을 구현하는지 여부를 조회하고, 구현하는 경우 컨트롤 패턴을 실행합니다. 요소에 둘 이상의 컨트롤 패턴이 있는 경우 하나만 호출되고 나머지는 무시됩니다. 컨트롤 패턴은 다음 순서로 검색됩니다.

1.  Invoke. 예를 들어 단추입니다.
2.  Toggle. 예를 들어 확인란입니다.
3.  Selection. 예를 들어 RadioButton입니다.
4.  Expand/Collapse. 예를 들어 ComboBox입니다.

컨트롤 패턴을 찾을 수 없는 경우 선택키 호출은 no-op으로 표시되며, 이러한 상황의 디버그를 지원하기 위해 다음과 같은 디버그 메시지가 기록됩니다. "이 구성 요소에 대한 자동화 패턴을 찾을 수 없습니다. AccessKeyInvoked에 대한 이벤트 처리기에서 원하는 동작을 구현하세요. 이벤트 처리기에서 Handled를 true로 설정하면 이 메시지가 표시되지 않습니다."

> [!NOTE]
> 이 메시지를 표시하려면 Visual Studio의 디버그 설정에서 디버거의 응용 프로그램 프로세스 유형이 _혼합(관리/네이티브)_ 또는 _네이티브_여야 합니다.

선택키에서 기본 컨트롤 패턴을 실행하지 않거나 요소에 컨트롤 패턴이 없는 경우 [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) 이벤트를 처리하고 원하는 동작을 구현해야 합니다.
```csharp
private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
{
    args.Handled = true;
    //Do something
}
```

컨트롤 패턴에 대한 자세한 내용은 [UI 자동화 컨트롤 패턴 개요](https://msdn.microsoft.com/library/windows/desktop/ee671194.aspx)를 참조하세요.

## <a name="access-keys-and-narrator"></a>선택키 및 내레이터

Windows 런타임에는 Microsoft UI 자동화 요소의 속성을 노출하는 UI 자동화 공급자가 있습니다. UI 자동화 클라이언트 응용 프로그램은 이러한 속성을 통해 사용자 인터페이스 부분에 대한 정보를 검색할 수 있습니다. [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) 속성을 사용하면 내레이터 등의 클라이언트가 요소와 연결된 선택키를 검색할 수 있습니다. 내레이터는 요소가 포커스를 받을 때마다 이 속성을 읽습니다. AutomationProperties.AccessKey에 값이 없는 경우 XAML 프레임워크는 UIElement 또는 TextElement의 [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) 속성 값을 반환합니다. AccessKey 속성에 이미 값이 있는 경우에는 AutomationProperties.AccessKey를 설정할 필요가 없습니다.

## <a name="example-access-key-for-button"></a>예제: 단추의 선택키

이 예제에서는 단추의 선택키를 만드는 방법을 보여 줍니다. 도구 설명을 시각적 어포던스로 사용하여 선택키가 포함된 부동 배지를 구현합니다.

> [!NOTE]
> 편의상 도구 설명을 사용하지만 고유한 컨트롤을 만들어 표시하는 데 사용하는 것이 좋습니다(예: [팝업](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.popup.aspx)).

XAML 프레임워크에서 Click 이벤트 처리기를 자동으로 호출하므로 AccessKeyInvoked 이벤트를 처리할 필요는 없습니다. 이 예제에서는 [AccessKeyDisplayRequestedEventArgs.PressedKeys](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeydisplayrequestedeventargs.pressedkeys.aspx) 속성을 사용하여 선택키 호출을 위해 남아 있는 문자에 대해서만 시각적 어포던스를 제공합니다. 예를 들어 _A1_, _A2_, _C_라는 세 개의 선택키가 표시되어 있고 사용자가 _A_를 누르면 _A1_ 및 _A2_ 선택키만 필터링되지 않고 _A1_과 _A2_ 대신 _1_과 _2_로 표시됩니다.

```xaml
<StackPanel
        VerticalAlignment="Center"
        HorizontalAlignment="Center"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Button Content="Press"
                AccessKey="PB"
                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                Click="DoSomething" />
        <TextBlock Text="" x:Name="textBlock" />
    </StackPanel>
```

```csharp
 public sealed partial class ButtonSample : Page
    {
        public ButtonSample()
        {
            this.InitializeComponent();
        }

        private void DoSomething(object sender, RoutedEventArgs args)
        {
            textBlock.Text = "Access Key is working!";
        }

        private void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(sender, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        private void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(sender, null);
            }
        }
    }
```

## <a name="example-scoped-access-keys"></a>예제: 범위가 지정된 선택키

이 예제에서는 범위가 지정된 선택키를 만드는 방법을 보여 줍니다. PivotItem의 IsAccessKeyScope 속성은 사용자가 Alt 키를 누를 때 PivotItem 자식 요소의 선택키가 표시되지 않도록 합니다. XAML 프레임워크에서 범위를 자동으로 전환하기 때문에 이러한 선택키는 사용자가 PivotItem을 호출할 때만 표시됩니다. 또한 프레임워크에서 다른 범위의 선택키를 숨깁니다.

이 예제에서는 AccessKeyInvoked 이벤트를 처리하는 방법도 보여 줍니다. PivotItem은 컨트롤 패턴을 구현하지 않으므로 XAML 프레임워크에서 기본적으로 아무 작업도 호출하지 않습니다. 이 구현은 선택키를 사용하여 호출된 PivotItem을 선택하는 방법을 보여 줍니다.

마지막으로, 이 예제에서는 표시 모드가 변경될 때 작업을 수행할 수 있는 IsDisplayModeChanged 이벤트를 보여 줍니다. 이 예제에서는 사용자가 Alt 키를 누를 때까지 피벗 컨트롤이 축소됩니다. 사용자가 피벗 조작을 마치면 다시 축소됩니다. IsDisplayModeEnabled를 사용하여 선택키 표시 모드가 사용하도록 설정되었는지 여부를 확인할 수 있습니다.

```xaml   
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Pivot x:Name="MyPivot" VerticalAlignment="Center" HorizontalAlignment="Center" >
            <Pivot.Items>
                <PivotItem
                    x:Name="PivotItem1"
                    AccessKey="A"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    IsAccessKeyScope="True">
                    <PivotItem.Header>
                        <TextBlock Text="A Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal" >
                        <Button Content="ButtonAA" AccessKey="A"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested" />
                        <Button Content="ButtonAD1" AccessKey="D1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button Content="ButtonAD2" AccessKey="D2"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
                <PivotItem
                    x:Name="PivotItem2"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    AccessKey="B"
                    IsAccessKeyScope="true">
                    <PivotItem.Header>
                        <TextBlock Text="B Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal">
                        <Button AccessKey="B" Content="ButtonBB"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F1" Content="ButtonBF1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F2" Content="ButtonBF2"  
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
            </Pivot.Items>
        </Pivot>
    </Grid>
```

```csharp
public sealed partial class ScopedAccessKeys : Page
    {
        public ScopedAccessKeys()
        {
            this.InitializeComponent();
            AccessKeyManager.IsDisplayModeEnabledChanged += OnDisplayModeEnabledChanged;
            this.Loaded += OnLoaded;
        }

        void OnLoaded(object sender, object e)
        {
            //To let the framework discover the access keys, the elements should be realized
            //on the visual tree. If there are no elements in the visual
            //tree with access key, the framework won't raise the events.
            //In this sample, if you define the Pivot as collapsed on the constructor, the Pivot
            //will have a lazy loading and the access keys won't be enabled.
            //For this reason, we make it visible when creating the object
            //and we collapse it when we load the page.
            MyPivot.Visibility = Visibility.Collapsed;
        }

        void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
        {
            args.Handled = true;
            MyPivot.SelectedItem = sender as PivotItem;
        }
        void OnDisplayModeEnabledChanged(object sender, object e)
        {
            if (AccessKeyManager.IsDisplayModeEnabled)
            {
                MyPivot.Visibility = Visibility.Visible;
            }
            else
            {
                MyPivot.Visibility = Visibility.Collapsed;

            }
        }

        DependencyObject AdjustTarget(UIElement sender)
        {
            DependencyObject target = sender;
            if (sender is PivotItem)
            {
                PivotItem pivotItem = target as PivotItem;
                target = (sender as PivotItem).Header as TextBlock;
            }
            return target;
        }

        void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);
            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(target, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);

            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(target, null);
            }
        }
    }
```



<!--HONumber=Dec16_HO1-->


