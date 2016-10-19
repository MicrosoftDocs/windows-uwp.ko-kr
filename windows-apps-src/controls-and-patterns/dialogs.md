---
author: mijacobs
Description: "대화 상자 및 플라이아웃은 사용자가 요청할 경우나 알림 또는 승인이 필요한 문제가 발생할 경우 나타나는 임시 UI 요소를 표시합니다."
title: "대화 상자 및 플라이아웃"
label: Dialogs
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: ff9940c06276165dc139e120c4e9cdeb005ff125

---
# 대화 상자 및 플라이아웃

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

대화 상자 및 플라이아웃은 사용자로부터 알림, 승인 또는 추가 정보를 요청하는 문제가 발생할 경우 나타나는 임시 UI 요소입니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx">ContentDialog 클래스</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn279496">Flyout 클래스</a></li>
</ul>

</div>
</div>



<!--
<table>
<tr>
<th>Dialogs</th><th>Flyouts</th>
</tr>
<tr>
<td>![Example of a full-button dialog](images/controls_dialog_twobutton.png)</td>
<td>![Example of a flyout](images/flyout-example.png)</td>
</tr>
<tr>
<td>Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.  </td>
<td>A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a hidden control, show more detail about an item, or ask the user to confirm an action. Flyouts can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
</td>
</tr>
</table>
-->

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>대화 상자</b> <br/><br/>
   ![전체 단추 대화 상자 예제](images/controls_dialog_twobutton.png)</p>
<p>대화 상자는 상황에 맞는 앱 정보를 제공하는 모달 UI 오버레이입니다. 대화 상자는 명시적으로 닫을 때까지 앱 창의 조작을 차단합니다. 종종 사용자의 작업을 요청하기도 합니다.   
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>플라이아웃</b> <br/><br/>
   ![플라이아웃의 예제](images/flyout-example.png)</p>
<p>플라이아웃은 사용자가 수행하는 작업과 관련된 UI를 표시하는 경량의 상황에 맞는 팝업입니다. 이 기능은 배치 및 크기 조정 논리를 포함하며, 숨겨진 컨트롤을 표시하거나, 항목에 대한 세부 정보를 표시하거나, 사용자에게 작업 확인을 요청하는 데 사용할 수 있습니다. 
</p><p>대화 상자와 달리 플라이아웃은 플라이아웃 바깥쪽 아무 곳이나 탭 또는 클릭하거나, Esc 키 또는 뒤로 단추를 누르거나, 앱 창 크기를 조정하거나, 디바이스의 방향을 변경하여 신속하게 해제할 수 있습니다.
</p><br/>

  </div>
</div>
</div>

## 올바른 컨트롤인가요?

* 대화 상자 및 플라이아웃을 사용하여 사용자에게 중요한 정보를 알리거나 확인 또는 추가 정보를 요청한 후에 작업을 완료할 수 있습니다. 
* [도구 설명](tooltips.md) 또는 [상황에 맞는 메뉴](menus.md) 대신에 플라이아웃을 사용하지 마세요. 도구 설명을 사용하여 지정된 시간 후에 숨겨지는 간단한 설명을 표시합니다. 복사 및 붙여넣기 등 UI 요소에 관련된 상황별 작업을 위한 상황에 맞는 메뉴를 사용합니다.  


대화 상자 및 플라이아웃을 통해 사용자가 중요한 정보를 알 수 있지만 사용자 환경을 방해할 수도 있습니다. 대화 상자는 모달(차단)이므로 대화 상자를 조작할 때까지 아무 작업도 수행하지 못하도록 하여 사용자를 중단시킵니다. 플라이아웃은 덜 방해되는 환경을 제공하지만 너무 많은 플라이아웃이 표시되면 주의가 분산될 수 있습니다. 

공유하려는 정보가 사용자를 중단시킬 만큼 중요한 정보인지 고려해야 합니다. 또한 정보를 표시해야 하는 빈도도 고려해야 합니다. 대화 상자나 알림을 몇 분 마다 표시할 경우에는 기본 UI에 이 정보에 대한 공간을 대신에 할당할 수 있습니다. 예를 들어 채팅 클라이언트에서 친구가 로그인 할 때마다 플라이아웃을 표시하지 않고 온라인 상태인 친구의 목록을 표시하고 친구가 로그온할 때 강조 표시할 수 있습니다. 

플라이아웃 및 대화 상자는 흔히 파일 삭제 등과 같이 작업을 실행하기 전에 확인하는 데 사용됩니다. 사용자가 특정 작업을 자주 수행할 경우에는 매번 작업을 확인하지 않고 실수하면 작업을 취소할 수 있는 방법을 제공하는 것이 좋습니다. 



## 대화 상자와 플라이아웃

대화 상자 또는 플라이아웃을 사용하기로 결정했으면 무엇을 사용할 것인지 선택해야 합니다. 

<!--
Dialogs are modal, which means they block all interaction with the app until the user selects a dialog button. To visually reinforce their modal behavior, dialogs draw an overlay layer which partially obscures the temporarily unreachable app UI.

A flyout is a light dismiss control, meaning that users can choose from a variety of actions to quickly dismiss it. These interactions are intended to be lightweight and non-blocking. Light dismiss actions include

* Clicking or tap outside the transient UI
* Pressing the Escape key
* Pressing the Back button
* Resizing the app window
* Changing device orientation

-->

대화 상자는 상호 작용을 차단하고 플라이아웃은 그러지 않으므로 질문에 대답하거나 소량의 특정 정보에 중점을 두는 상황에서는 대화 상자를 예약해야 합니다. 반면 무언가에 주의를 집중시키지만 사용자가 무시해도 되는 경우 플라이아웃을 사용할 수 있습니다. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>대화 상자는 다음과 같은 작업에 사용됩니다.</b> <br/>
<ul>
<li>사용자가 계속하기 전에 읽고 **반드시** 승인해야 하는 중요한 정보 표시 예를 들면 다음과 같습니다.
<ul>
  <li>사용자의 보안이 손상될 수 있는 경우</li>
  <li>사용자가 중요한 자산을 영구적으로 변경하려는 경우</li>
  <li>사용자가 중요한 자산을 삭제하려는 경우</li>
  <li>앱에서 바로 구매를 확인하려면</li>
</ul>

</li>
<li>연결 오류 등 전체 앱 상황에 적용되는 오류 메시지입니다.</li>
<li>앱에서 사용자 대신 선택할 수 없는 경우와 같이 앱에서 사용자에게 차단 질문을 해야 하는 경우의 질문 차단 질문은 무시하거나 연기할 수 없으며 사용자에게 잘 정의된 선택 항목을 제공해야 합니다.</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>플라이아웃은 다음과 같은 작업에 사용됩니다.</b> <br/>
<ul>
<li>작업을 완료할 수 있기 전에 필요한 추가 정보 수집</li>
<li>특정 시간에만 관련되는 정보 표시 예를 들어 사진 갤러리 앱에서 이미지 미리 보기를 클릭할 때 플라이아웃을 사용하여 큰 버전의 이미지를 표시할 수 있습니다.</li>
<li>소멸될 수 있는 작업과 관련된 경고 및 확인</li>
<li>페이지에 항목에 대한 세부 정보 또는 긴 설명과 같은 추가 정보 표시</li>
</ul></p>
  </div>
</div>
</div>



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

## 대화 상자 만들기
대화 상자를 만들려면 [ContentDialog 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)를 사용합니다. 코드나 태그에서 대화 상자를 만들 수 있습니다. 일반적으로 XAML에서 UI 요소를 정의하는 것이 더 쉽지만 간단한 대화 상자의 경우 코드를 사용하는 것이 실제로 더 쉽습니다. 이 예제에서는 WiFi 연결이 없음을 사용자에게 알리는 대화 상자를 만든 다음 [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) 메서드를 사용하여 대화 상자를 표시합니다.

```csharp
private async void displayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog()
    {
        Title = "No wifi connection",
        Content = "Check connection and try again",
        PrimaryButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

사용자가 대화 단추를 클릭하면 [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) 메서드는 사용자가 클릭한 단추를 알 수 있도록 [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx)를 반환합니다. 

이 예제의 대화 상자는 질문한 다음 반환된 [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx)를 사용하여 사용자의 응답을 확인합니다. 

```csharp
private async void displayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog()
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Cancel",
        SecondaryButtonText = "Delete file permanently"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();
    
    // Delete the file if the user clicked the second button. 
    /// Otherwise, do nothing. 
    if (result == ContentDialogResult.Secondary)
    {
        // Delete the file. 
    }
}
```


##  플라이아웃 만들기

플라이아웃은 개방형 컨테이너로 임의의 UI와 해당 콘텐츠를 표시할 수 있습니다.  

플라이아웃은 특정 컨트롤에 연결됩니다. [배치](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.placement.aspx) 속성을 사용하여 플라이아웃이 표시되는 위치(위쪽, 왼쪽, 아래쪽, 오른쪽 또는 전체)를 지정할 수 있습니다. [전체 배치 모드](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutplacementmode.aspx)를 선택하면 앱은 플라이아웃을 확대하고 앱 창 내에서 가운데로 지정합니다. 표시되는 경우 호출 개체에 고정하고 개체의 기본 상대 위치(위쪽, 왼쪽, 아래쪽 또는 오른쪽)를 지정해야 합니다. 플라이아웃에는 플라이아웃을 확대하고 앱 창 내 가운데로 지정하는 전체 배치 모드도 있습니다. [단추](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)와 같은 일부 컨트롤은 플라이아웃을 연결하는 데 사용할 수 있는 [플라이아웃](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 속성을 제공합니다. 

이 예제에서는 단추를 누를 때 일부 텍스트를 표시하는 간단한 플라이아웃을 만듭니다. 
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

컨트롤에 플라이아웃 속성이 없으면 대신 [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) 관련 속성을 사용할 수 있습니다. 이 속성을 사용할 경우 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 메서드도 호출해야 플라이아웃을 표시할 수 있습니다. 

이 예제에서는 이미지에 간단한 플라이아웃을 추가합니다. 사용자가 이미지를 탭하면 앱이 플라이아웃을 표시합니다. 

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50" 
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock TextWrapping="Wrap" Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

앞의 예제에서 해당 플라이아웃 인라인이 정의되었습니다. 고정 리소스로 플라이아웃을 정의한 다음 여러 요소와 함께 사용할 수도 있습니다. 이 예제에서는 해당 미리 보기를 탭하는 경우 더 큰 버전의 이미지를 표시하는 보다 복잡한 플라이아웃을 만듭니다. 

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source 
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}" 
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
        <Flyout.FlyoutPresenterStyle>
            <Style TargetType="FlyoutPresenter">
                <Setter Property="ScrollViewer.ZoomMode" Value="Enabled"/>
                <Setter Property="Background" Value="Black"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="BorderThickness" Value="5"/>
                <Setter Property="MinHeight" Value="300"/>
                <Setter Property="MinWidth" Value="300"/>
            </Style>
        </Flyout.FlyoutPresenterStyle>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

## 플라이아웃 스타일 지정
플라이아웃의 스타일을 지정하려면 해당 [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx)을 수정합니다. 이 예제에서는 줄 바꿈 단락을 보여 주고, 화면 읽기 프로그램에서 텍스트 블록에 액세스하도록 설정합니다.

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" 
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## 샘플 다운로드
*   [XAML UI 기본 사항](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    대화형 형식의 모든 XAML 컨트롤을 참조하세요.

## 관련 문서
- [도구 설명](tooltips.md)
- [메뉴 및 상황에 맞는 메뉴](menus.md)
- [**Flyout 클래스**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog 클래스**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Aug16_HO3-->


