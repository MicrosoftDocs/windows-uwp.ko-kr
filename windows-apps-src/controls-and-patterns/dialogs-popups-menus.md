---
author: Jwmsft
redirect_url: https://msdn.microsoft.com/windows/uwp/controls-and-patterns/dialogs
Description: "플라이아웃은 사용자가 현재 수행하고 있는 작업과 관련된 UI를 일시적으로 표시하는 데 사용되는 경량 팝업입니다."
title: "상황에 맞는 메뉴 및 대화 상자"
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 6572acefa25e464b6edaca9fee5b2b3e3b46ff3f

---
# 메뉴, 대화 상자, 플라이아웃 및 팝업

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

메뉴, 대화 상자, 플라이아웃 및 팝업은 사용자가 요청할 경우나 알림 또는 승인이 필요한 문제가 발생할 경우 나타나는 임시 UI 요소를 표시합니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn299030">MenuFlyout 클래스</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn279496">Flyout 클래스</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx">ContentDialog 클래스</a></li>
</ul>

</div>
</div>






상황에 맞는 메뉴는 사용자에게 즉각적인 작업을 제공합니다. 또한 텍스트 명령으로 채울 수 있습니다. 상황에 맞는 메뉴는 메뉴 바깥쪽의 아무 곳이나 탭하거나 클릭하여 빨리 해제할 수 있습니다.

대화 상자는 상황에 맞는 앱 정보를 제공하는 모달 UI 오버레이입니다. 대화 상자는 명시적으로 닫을 때까지 앱 창의 조작을 차단합니다. 종종 사용자의 작업을 요청하기도 합니다.

플라이아웃은 사용자가 수행하는 작업과 관련된 UI를 표시하는 경량의 상황에 맞는 팝업입니다. 이 기능은 배치 및 크기 조정 논리를 포함하며, 숨겨진 컨트롤을 표시하거나, 항목에 대한 세부 정보를 표시하거나, 사용자에게 작업 확인을 요청하는 데 사용할 수 있습니다. 플라이아웃은 플라이아웃 바깥쪽의 아무 곳이나 탭하거나 클릭하여 해제할 수 있습니다.


## 올바른 컨트롤인가요?

상황에 맞는 메뉴는 다음에 사용할 수 있습니다.

-   상황에 맞는 작업
-   작업이 수행되어야 하지만 선택할 수 없는 개체에 대한 명령

대화 상자는 다음에 사용할 수 있습니다.

- 사용자가 계속하기 전에 읽고 승인해야 하는 중요한 정보 표시
- 사용자로부터 명확한 작업을 요청하거나 사용자가 승인해야 하는 중요한 메시지를 전달합니다. 예를 들면 다음과 같습니다.
  - 사용자의 보안이 손상될 수 있는 경우
  - 사용자가 중요한 자산을 영구적으로 변경하려는 경우
  - 사용자가 중요한 자산을 삭제하려는 경우
  - 앱에서 바로 구매를 확인하려면
- 연결 오류 등 전체 앱 상황에 적용되는 오류 메시지입니다.
- 앱에서 사용자 대신 선택할 수 없는 경우와 같이 앱에서 사용자에게 차단 질문을 해야 하는 경우의 질문 차단 질문은 무시하거나 연기할 수 없으며 사용자에게 잘 정의된 선택 항목을 제공해야 합니다.

플라이아웃은 다음에 사용할 수 있습니다.

-   상황에 맞는 임시 UI
-   소멸될 수 있는 작업과 관련된 경고 및 확인
-   페이지에 항목에 대한 세부 정보 또는 긴 설명과 같은 추가 정보 표시


## 예

짧은 간단한 명령 목록으로 구성된 일반적인 단일 창 상황에 맞는 메뉴는 다음과 같습니다. 필요한 경우 스크롤할 수 있습니다. 필요에 따라 구분 기호를 사용하여 유사한 명령을 그룹화합니다.

![일반적인 상황에 맞는 메뉴 예제](images/controls_contextmenu_singlepane.png)

연속 상황에 맞는 메뉴는 보다 포괄적인 명령 컬렉션에 사용됩니다. 여러 플라이아웃 수준으로 표시되며 스크롤이 가능합니다. 필요에 따라 구분 기호를 사용하여 유사한 명령을 그룹화합니다.

![연속 상황에 맞는 메뉴 예제](images/controls_contextmenu_cascading.png)

다음은 전체 화면, 단일 단추 확인 대화 상자 예제입니다. 이 종류의 대화 상자에서는 사용자가 계속 진행하기 위해 단추를 누르기 전에 읽게 되는 적당량의 정보가 표시됩니다.

![전체 페이지, 단일 단추 대화 상자 예제](images/controls_dialog_singlebutton.png)

다음은 사용자에게 A/B 선택 옵션을 제공하는 두 개 단추를 포함하는 대화 상자의 예입니다. 일반적으로 이 대화 상자에는 간단한 정보가 표시됩니다.

![전체 단추 대화 상자 예제](images/controls_dialog_twobutton.png)

## 모달 vs 빠른 해제

대화 상자는 모달 형식입니다. 즉 사용자가 대화 상자 단추를 선택할 때까지 모든 앱 조작을 차단합니다. 대화 상자의 모달 동작을 시각적으로 강화하기 위해 대화 상자에서는 일시적으로 연결할 수 없는 앱 UI를 부분적으로 가리는 오버레이 계층을 그립니다.

**참고** 취소가 사용 가능한 대화 상자 옵션에 포함된 경우 앱에서는 사용자가 Esc 키를 눌러 대화 상자를 닫을 수 있도록 선택할 수 있습니다. 이 동작은 컨트롤에 기본 제공되지는 않지만 일반적으로 구현된 바로 가기입니다.

플라이아웃 및 상황에 맞는 메뉴는 빠른 해제 컨트롤입니다. 즉 사용자는 임시 UI를 빠르게 해제할 수 있는 다양한 작업을 선택할 수 있습니다. 이러한 조작은 간단하고 차단되지 않도록 구현되었습니다 빠른 해제 작업은 다음과 같습니다.
- 임시 UI 바깥을 클릭 또는 탭
- Esc 키 누르기
- 뒤로 단추 누르기
- 앱 창 크기 조정
- 디바이스 방향 변경


## 대화 상자 사용 지침

-   대화 상자의 텍스트 첫 줄에서 문제점이나 사용자의 목적을 명확히 식별해야 합니다.
-   대화 상자 제목은 기본 지침이며 선택 사항입니다.
    -   간단한 제목을 사용하여 대화 상자에서 수행해야 하는 작업을 설명할 수 있습니다. 제목이 길 경우 줄 바꿈되지 않고 잘립니다.
    -   대화 상자를 사용하여 단순 메시지, 오류 또는 질문을 제공하는 경우 선택적으로 제목을 생략할 수 있습니다. 내용 텍스트를 사용하여 핵심 정보를 전달합니다.
    -   제목은 단추 선택 항목과 직접 관련된 것이어야 합니다.
-   대화 상자 내용에는 설명 텍스트가 포함되며 필수 사항입니다.
    -   메시지, 오류 또는 차단 질문은 최대한 간결하게 제공합니다.
    -   대화 상자 제목을 사용하는 경우, 콘텐츠 영역을 사용하여 세부 정보를 제공하거나 용어를 정의할 수 있습니다. 제목과 똑같은 내용을 표현만 조금 바꿔 제공하지 마세요.
-   하나 이상의 대화 상자 단추가 표시되어야 합니다.
    -   단추를 사용해야만 대화 상자를 닫을 수 있습니다.
    -   기본 지시 사항이나 내용에 대한 특정 응답을 식별하는 텍스트가 있는 단추를 사용합니다. 예를 들어 "AppName에서 해당 위치에 액세스하도록 하시겠어요?" 뒤에 "허용" 및 "차단" 단추를 제공합니다. 특정 응답은 보다 신속하게 이해할 수 있어서 효율적인 결정을 유도할 수 있습니다.
-   오류 대화 상자에는 오류 메시지가 관련된 정보와 함께 표시됩니다. 오류 대화 상자에서는 “닫기" 또는 유사한 작업을 나타내는 단추만 사용됩니다.
-   유효성 검사 오류(예: 암호 필드의 오류)와 같이 페이지의 특정 위치에 해당하는 오류의 경우에는 대화 상자를 사용하지 마세요. 대신 앱의 캔버스 자체를 사용하여 인라인 오류를 표시합니다.

## 플라이아웃

플라이아웃은 개방형 컨테이너로 임의의 UI와 해당 콘텐츠를 표시할 수 있습니다.  플라이아웃은 고유한 시각적 표시가 없는 단순한 콘텐츠 컨트롤입니다. 플라이아웃의 콘텐츠 주변에는 여백과 스크롤 막대(옵션)가 추가되어 있습니다. 플라이아웃의 스타일을 지정하려면 해당 [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx)을 수정합니다.

다음 코드는 줄 바꿈 단락을 보여 주고, 화면 읽기 프로그램에서 텍스트 블록에 액세스하도록 설정합니다.

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

### 호출 및 배치

플라이아웃과 상황에 맞는 메뉴는 특정 컨트롤에 연결됩니다. 표시되는 경우 호출 개체에 고정하고 개체의 기본 상대 위치(위쪽, 왼쪽, 아래쪽 또는 오른쪽)를 지정해야 합니다. 플라이아웃에는 플라이아웃을 확대하고 앱 창 내 가운데로 지정하는 전체 배치 모드도 있습니다.

[Button 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)에는 사용자가 단추를 클릭하거나 탭할 때 열리는 임시 UI를 지정할 수 있게 하는 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 속성이 있습니다.

````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="Yay!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````


## 관련 문서

**개발자용**
- [**MenuFlyout 클래스**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Flyout 클래스**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog 클래스**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Aug16_HO3-->


