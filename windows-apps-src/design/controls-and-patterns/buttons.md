---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: 단추
label: Buttons
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f663ce9da6453922e35f850a8cd039f33770f434
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494170"
---
# <a name="buttons"></a>단추


단추를 사용하면 즉각적인 작업을 트리거할 수 있습니다.

> **중요 API**: [Button 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), [RepeatButton 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx), [Click 이벤트](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

![단추 예](images/controls/button.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

단추를 사용하면 양식을 전송하는 등의 즉각적인 작업을 시작할 수 있습니다.

다른 페이지를 탐색하는 작업인 경우에는 단추 대신 링크를 사용합니다. 추가 정보는 [하이퍼링크](hyperlinks.md)를 참조하세요.
> 예외: 마법사 탐색인 경우에는 '뒤로' 및 '다음'이라는 레이블이 붙은 단추를 사용합니다. 역방향 탐색 또는 상위 수준 탐색 등의 경우에는 뒤로 단추를 사용합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Button">앱을 열고 작동 중인 단추를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

이 예에서는 위치 액세스를 요청하는 대화 상자에서 허용 및 차단의 두 가지 단추를 사용합니다.

![대화 상자에서 사용되는 단추의 예](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>단추 만들기

이 예제에서는 클릭에 응답하는 단추를 보여 줍니다.

XAML에서 단추를 만듭니다.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

또는 코드에서 단추를 만듭니다.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Click 이벤트를 처리합니다.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>단추 조작

손가락 또는 스타일러스로 단추를 탭하거나 포인터가 단추 위에 있을 때 마우스 왼쪽 단추를 누르면 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 이벤트가 발생합니다. 단추에 키보드 포커스가 있는 경우 Enter 키 또는 스페이스바 키를 눌러도 Click 이벤트가 발생합니다.

단추에는 대신 클릭 동작이 있기 때문에 일반적으로 단추에서 하위 수준의 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 이벤트를 처리할 수 없습니다. 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx)를 참조하세요.

[ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode) 속성을 변경하여 단추가 Click 이벤트를 발생시키는 방법을 변경할 수 있습니다. ClickMode의 기본값은 **Release**이지만, 단추의 ClickMode를 **Hover** 또는 **Press**로 설정할 수 있습니다. ClickMode가 **Hover**인 경우 키보드 또는 터치로 Click 이벤트를 발생시킬 수 없습니다.


### <a name="button-content"></a>단추 콘텐츠

단추는 [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx)입니다. 단추의 XAML 콘텐츠 속성은 [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)이며 XAML에 대해 다음과 같은 구문을 가능하게 합니다. `<Button>A button's content</Button>`. 어떠한 개체라도 단추 콘텐츠로 설정할 수 있습니다. 콘텐츠가 [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx)인 경우 단추에서 렌더링됩니다. 콘텐츠가 다른 유형의 개체인 경우 해당 문자열 표현이 단추에 표시됩니다.

버튼의 콘텐츠는 일반적으로 텍스트입니다. 콘텐츠 텍스트가 있는 단추에서는 다음과 같은 디자인을 추천합니다.
-   단추가 수행하는 작업을 명확히 설명해 주는 간결하고 구체적이며 설명적인 텍스트를 사용합니다. 일반적으로 단추 텍스트 콘텐츠는 한 단어로 된 동사입니다.
-   브랜드 지침에 다른 글꼴을 사용하도록 규정되어 있지 않은 경우 기본 글꼴을 사용하세요.
-   텍스트가 짧은 경우에는 120px의 최소 단추 너비를 사용하여 명령 단추가 좁아지지 않도록 합니다.
- 텍스트가 긴 경우에는 텍스트의 최대 길이를 26자로 제한하여 명령 버튼이 넓어지지 않도록 합니다.
-   단추의 텍스트 콘텐츠가 동적인 경우에는(예: [지역화](../globalizing/globalizing-portal.md)) 단추 크기를 조정하는 방법과 주위의 컨트롤에 나타날 변화를 고려하세요.

<table>
<tr>
<td> <b>다음 문제를 해결해야 합니다.</b><br> 오버플로 텍스트가 포함된 단추 </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>옵션 1:</b><br> 단추 너비를 늘리고, 단추를 스태킹하며, 텍스트 길이가 26자 이상이면 줄바꿈을 합니다. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>옵션 2:</b><br> 단추 높이를 늘리고 텍스트를 줄바꿈 합니다. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

단추의 모양을 만드는 시각 효과를 사용자가 지정할 수도 있습니다. 예를 들어 텍스트를 아이콘으로 바꾸거나 아이콘과 텍스트를 함께 사용할 수 있습니다.

여기에서는 이미지와 텍스트가 포함된 **StackPanel**이 단추의 콘텐츠로 설정되어 있습니다.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

단추는 다음과 같습니다.

![이미지 및 텍스트 콘텐츠가 포함된 단추](images/button-orange.png)

## <a name="create-a-repeat-button"></a>반복 단추 만들기

[RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)은 사용자가 눌렀다가 놓을 때까지 반복해서 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 이벤트를 발생시키는 컨트롤입니다. [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) 속성을 설정하여 RepeatButton이 클릭 동작 반복을 시작하기 전에 눌러진 후 대기해야 하는 시간을 지정합니다. [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) 속성을 설정하여 클릭 동작의 반복 간 시간을 지정합니다. 두 속성에 대한 시간은 밀리초로 지정됩니다.

다음 예제에서는 두 RepeatButton 컨트롤(해당 Click 이벤트는 텍스트 블록에 표시되는 값을 늘리고 줄이는 데 사용함)을 보여 줍니다.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="recommendations"></a>권장 사항
-   단추의 목적과 상태가 사용자에게 명확하게 전달되는지 확인합니다.
-   확인 대화 상자 등에서 동일한 결정에 대한 단추가 여러 개 있을 경우에는 다음과 같은 순서로 커밋 단추를 표시하세요. 여기에서 [그렇게 함] 및 [그렇게 하지 않음]은 기본 지침에 대한 구체적인 응답입니다.
    -   확인/[그렇게 함]/예
    -   [그렇게 하지 않음]/아니요
    -   취소
-   사용자에게 단추를 한 번에 한두 개(예: 수락 및 취소)만 표시합니다. 사용자에게 더 많은 작업을 표시해야 하는 경우 사용자가 하나의 명령 단추로 작업을 선택하여 트리거할 수 있도록 [확인란](checkbox.md) 또는 [라디오 단추](radio-button.md)를 사용하는 것이 좋습니다.
-   앱 내의 여러 페이지에서 작업을 사용할 수 있어야 하는 경우 단추를 여러 페이지에 복제하는 대신 [하단 앱 바](app-bars.md)를 사용하는 것이 좋습니다.

### <a name="recommended-single-button-layout"></a>권장 단일 단추 레이아웃

레이아웃에 단추가 하나만 필요한 경우 컨테이너 컨텍스트에 따라 왼쪽 또는 오른쪽에 맞춰야 합니다.

-   단추가 하나만 있는 대화 상자의 경우 단추를 **오른쪽에 맞춰야** 합니다. 대화 상자에 단추가 하나만 있는 겨우 단추가 안전한 비파괴 작업을 수행하는지 확인하세요. [ContentDialog](dialogs.md)를 사용하고 단일 단추를 지정하면 자동으로 오른쪽에 맞춰집니다.

![대화 상자 내의 단추](images/pushbutton_doc_dialog.png)

-   단추가 알림 메시지, 플라이아웃 또는 목록 보기 항목과 같은 컨테이너 UI 내에 나타나는 경우 컨테이너 내에서 단추를 **오른쪽에 맞춰야**합니다.

![컨테이너 내의 단추](images/pushbutton_doc_container.png)

-   설정 페이지의 아래쪽에 있는 "적용" 단추와 같이 단일 단추가 있는 페이지에서는 단추를 **왼쪽에 맞춰야** 합니다. 이를 통해 단추가 나머지 페이지 콘텐츠와 맞춰집니다.

![페이지의 단추](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>뒤로 단추

뒤로 단추는 뒤로 스택 또는 사용자의 탐색 기록을 통해 뒤로 탐색할 수 있게 하는 시스템 제공 UI 요소입니다. 뒤로 단추를 직접 만들지 않아도 되지만 좋은 뒤로 탐색 환경을 사용하려면 일부 작업을 수행해야 할 수 있습니다. 자세한 내용은 [검색 기록 및 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)을 참조하세요.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.


## <a name="related-articles"></a>관련 문서
- [단추 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [라디오 단추](radio-button.md)
- [확인란](checkbox.md)
- [토글 스위치](toggles.md)
