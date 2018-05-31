---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 대화 상자 및 플라이아웃
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9ceb698bfbe95693ff9d5785b4bea94f1ec3070c
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675380"
---
# <a name="dialogs-and-flyouts"></a>대화 상자 및 플라이아웃



대화 상자 및 플라이아웃은 사용자로부터 알림, 승인 또는 추가 정보를 요청하는 문제가 발생할 경우 나타나는 임시 UI 요소입니다.

> **중요 API**: [ContentDialog 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [Flyout 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::행::: :::열::: **대화 상자**
        
        ![Example of a dialog](images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::행 끝:::


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

* 대화 상자를 사용하여 사용자에게 중요한 정보를 알리거나 확인 또는 추가 정보를 요청한 후에 작업을 완료할 수 있습니다.
* [도구 설명](tooltips.md) 또는 [상황에 맞는 메뉴](menus.md) 대신에 플라이아웃을 사용하지 마세요. 도구 설명을 사용하여 지정된 시간 후에 숨겨지는 간단한 설명을 표시합니다. 복사 및 붙여넣기 등 UI 요소에 관련된 상황별 작업을 위한 상황에 맞는 메뉴를 사용합니다.  


대화 상자 및 플라이아웃을 통해 사용자가 중요한 정보를 알 수 있지만 사용자 환경을 방해할 수도 있습니다. 대화 상자는 모달(차단)이므로 대화 상자를 조작할 때까지 아무 작업도 수행하지 못하도록 하여 사용자를 중단시킵니다. 플라이아웃은 덜 방해되는 환경을 제공하지만 너무 많은 플라이아웃이 표시되면 주의가 분산될 수 있습니다.

공유하려는 정보가 사용자를 중단시킬 만큼 중요한 정보인지 고려해야 합니다. 또한 정보를 표시해야 하는 빈도도 고려해야 합니다. 대화 상자나 알림을 몇 분 마다 표시할 경우에는 기본 UI에 이 정보에 대한 공간을 대신에 할당할 수 있습니다. 예를 들어 채팅 클라이언트에서 친구가 로그인 할 때마다 플라이아웃을 표시하지 않고 온라인 상태인 친구의 목록을 표시하고 친구가 로그온할 때 강조 표시할 수 있습니다.

대화 상자는 흔히 파일 삭제 등과 같이 작업을 실행하기 전에 확인하는 데 사용됩니다. 사용자가 특정 작업을 자주 수행할 경우에는 매번 작업을 확인하지 않고 실수하면 작업을 취소할 수 있는 방법을 제공하는 것이 좋습니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 작동 중인 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 또는 <a href="xamlcontrolsgallery:/item/Flyout">플라이아웃</a>을 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="dialogs-vs-flyouts"></a>대화 상자와 플라이아웃 비교

대화 상자 또는 플라이아웃을 사용하기로 결정했으면 무엇을 사용할 것인지 선택해야 합니다.

대화 상자는 상호 작용을 차단하고 플라이아웃은 그러지 않으므로 질문에 대답하거나 소량의 특정 정보에 중점을 두는 상황에서는 대화 상자를 예약해야 합니다. 반면 무언가에 주의를 집중시키지만 사용자가 무시해도 되는 경우 플라이아웃을 사용할 수 있습니다.

:::행::: :::열:::
   <p><b>대화 상자는 다음과 같은 작업에 사용됩니다.</b> <br/>
<ul>
<li>사용자가 계속하기 전에 읽고 <b>반드시</b> 승인해야 하는 중요한 정보 표시 예를 들면 다음과 같습니다.
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
:::열 끝::: :::행::: <p><b>플라이아웃은 다음과 같은 작업에 사용됩니다.</b> <br/>
<ul>
<li>작업을 완료할 수 있기 전에 필요한 추가 정보 수집</li>
<li>특정 시간에만 관련되는 정보 표시 예를 들어 사진 갤러리 앱에서 이미지 미리 보기를 클릭할 때 플라이아웃을 사용하여 큰 버전의 이미지를 표시할 수 있습니다.</li>
<li>페이지에 항목에 대한 세부 정보 또는 긴 설명과 같은 추가 정보 표시</li>
</ul></p>
:::열 끝::: :::행 끝:::



## <a name="dialogs"></a>대화 상자
### <a name="general-guidelines"></a>일반 지침

-   대화 상자의 텍스트 첫 줄에서 문제점이나 사용자의 목적을 명확히 식별해야 합니다.
-   대화 상자 제목은 기본 지침이며 선택 사항입니다.
    -   간단한 제목을 사용하여 대화 상자에서 수행해야 하는 작업을 설명할 수 있습니다.
    -   대화 상자를 사용하여 단순 메시지, 오류 또는 질문을 제공하는 경우 선택적으로 제목을 생략할 수 있습니다. 내용 텍스트를 사용하여 핵심 정보를 전달합니다.
    -   제목은 단추 선택 항목과 직접 관련된 것이어야 합니다.
-   대화 상자 내용에는 설명 텍스트가 포함되며 필수 사항입니다.
    -   메시지, 오류 또는 차단 질문은 최대한 간결하게 제공합니다.
    -   대화 상자 제목을 사용하는 경우, 콘텐츠 영역을 사용하여 세부 정보를 제공하거나 용어를 정의할 수 있습니다. 제목과 똑같은 내용을 표현만 조금 바꿔 제공하지 마세요.
-   하나 이상의 대화 상자 단추가 표시되어야 합니다.
    -   "확인", "닫기" 또는 "취소"와 같은 안전한 비파괴 작업에 해당하는 단추가 대화 상자에 하나 이상 있는지 확인합니다. CloseButton API를 사용하여 이 단추를 추가합니다.
    -   기본 지시 사항이나 내용에 대한 특정 응답을 단추 텍스트로 사용합니다. 예를 들어, "AppName에서 해당 위치에 액세스하도록 하시겠어요?" 뒤에 "허용" 및 "차단" 단추를 제공합니다. 특정 응답은 보다 신속하게 이해할 수 있어서 효율적인 결정을 유도할 수 있습니다.
    - 작업 단추의 텍스트가 간결한지 확인합니다. 짧은 문자열을 사용하면 사용자가 빠르고 자신 있게 선택할 수 있습니다.
    - 안전한 비파괴 작업 외에도 필요에 따라 기본 지시 사항과 관련된 1-2개의 단추를 사용자에게 제공할 수 있습니다. 이러한 "수행" 작업 단추는 대화 상자의 기본 요소를 확인합니다. PrimaryButton 및 SecondaryButton API를 사용하여 이러한 "수행" 작업을 추가합니다.
    - "수행" 작업 단추는 맨 왼쪽 단추로 나타나야 합니다. 안전한 비파괴 작업은 맨 오른쪽 단추로 나타나야 합니다.
    - 필요에 따라 대화 상자의 기본 단추로 세 가지 단추 중 하나를 식별하도록 선택할 수 있습니다. DefaultButton API를 사용하여 단추 중 하나를 식별합니다.  
-   유효성 검사 오류(예: 암호 필드의 오류)와 같이 페이지의 특정 위치에 해당하는 오류의 경우에는 대화 상자를 사용하지 마세요. 대신 앱의 캔버스 자체를 사용하여 인라인 오류를 표시합니다.
- [ContentDialog 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)를 사용하여 대화 상자 환경을 구축합니다. 사용되지 않는 MessageDialog API를 사용하지 않습니다.

### <a name="dialog-scenarios"></a>대화 상자 시나리오
대화 상자는 사용자 상호 작용을 차단하고 단추는 사용자가 대화 상자를 해제하는 기본 메커니즘이기 때문에 대화 상자에 "닫기"나 "확인" 등의 "안전한" 비파괴 단추가 하나 이상 있는지 확인하세요. **모든 대화 상자에 대화 상자를 닫기 위한 안전한 작업 단추가 하나 이상 있어야 합니다.** 이를 통해 사용자가 작업을 수행하지 않고 대화 상자를 닫을 수 있습니다.<br>![단추가 1개인 대화 상자](images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

대화 상자가 차단 질문을 표시하는 데 사용될 때 대화 상자에서 사용자에게 질문과 관련된 작업 단추를 표시해야 합니다. "안전한" 비파괴 단추에 1-2개의 "수행" 작업 단추가 수반될 수 있습니다. 사용자에게 여러 옵션을 표시할 때 단추가 제안된 질문과 관련된 "권장 사항" 및 안전/"금지 사항" 작업을 명확하게 설명하는지 확인하세요.

![단추가 2개인 대화 상자](images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

사용자에게 "권장 사항" 작업과 "금지 사항" 작업을 제공할 때 단추가 3개인 대화 상자가 사용됩니다. 보조 작업과 안전/닫기 작업의 명확한 구분과 함께 단추가 3개인 대화 상자는 제한적으로 사용해야 합니다.

![단추가 3개인 대화 상자](images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
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

### <a name="the-three-dialog-buttons"></a>3개의 대화 상자 단추
ContentDialog에는 대화 상자 환경을 구현하는 데 사용할 수 있는 3가지 단추가 있습니다.

- **CloseButton** - 필수 - 사용자가 대화 상자를 종료할 수 있게 하는 안전한 비파괴 작업을 나타냅니다. 맨 오른쪽 단추로 나타납니다.
- **PrimaryButton** - 선택 사항 - 첫 번째 "권장 사항" 작업을 나타냅니다. 맨 왼쪽 단추로 나타납니다.
- **SecondaryButton** - 선택 사항 - 두 번째 "권장 사항" 작업을 나타냅니다. 가운데 단추로 나타납니다.

기본 제공 단추를 사용하면 단추가 적절히 배치되고, 단추가 키보드 이벤트에 제대로 응답하고, 화상 키보드가 실행 중일 때도 명령 영역이 계속 표시되며, 대화 상자가 다른 대화 상자와 일관성 있게 표시됩니다.

#### <a name="closebutton"></a>CloseButton
모든 대화 상자에 사용자가 대화 상자를 종료하는 데 사용할 수 있는 안전한 비파괴 작업 단추가 있어야 합니다.

ContentDialog.CloseButton API를 사용하여 이 단추를 만듭니다. 이를 통해 마우스, 키보드, 터치 및 게임 패드를 포함한 모든 입력에 대한 올바른 사용자 환경을 구축할 수 있습니다. 다음과 같은 입력이 여기에 해당합니다.
<ol>
    <li>사용자가 CloseButton을 클릭하거나 탭함 </li>
    <li>사용자가 시스템 뒤로 단추를 누름 </li>
    <li>사용자가 키보드의 ESC 단추를 누름 </li>
    <li>사용자가 게임 패드 B를 누름 </li>
</ol>

사용자가 대화 단추를 클릭하면 [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드는 사용자가 클릭한 단추를 알 수 있도록 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)를 반환합니다. CloseButton을 누르면 ContentDialogResult.None이 반환됩니다.

#### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton 및 SecondaryButton
CloseButton 외에도 필요에 따라 기본 지시 사항과 관련된 1-2개의 단추를 사용자에게 제공할 수 있습니다.
첫 번째 "권장 사항" 작업에 PrimaryButton을 활용하고 두 번째 "권장 사항" 작업에 SecondaryButton을 활용합니다. 단추가 3개인 대화 상자에서 PrimaryButton은 대개 긍정 "권장 사항" 작업을 나타내지만 SecondaryButton은 대개 중립 또는 보조 "권장 사항" 작업을 나타냅니다.
예를 들어, 앱에서 사용자에게 서비스에 가입하라는 메시지를 표시할 수 있습니다. 긍정 "권장 사항" 작업으로서 PrimaryButton은 가입 텍스트를 호스트하지만 중립 "권장 사항" 작업으로서 SecondaryButton은 체험해 보기 텍스트를 호스트합니다. CloseButton을 사용하면 사용자가 어떤 작업도 수행하지 않고 취소할 수 있습니다.

사용자가 PrimaryButton을 클릭하면 [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드가 ContentDialogResult.Primary를 반환해야 합니다.
사용자가 SecondaryButton을 클릭하면 [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드가 ContentDialogResult.Secondary를 반환해야 합니다.

![단추가 3개인 대화 상자](images/dialogs/dialog_RS2_three_button.png)

#### <a name="defaultbutton"></a>DefaultButton
필요에 따라 기본 단추로 세 가지 단추 중 하나를 식별하도록 선택할 수 있습니다. 기본 단추를 지정하면 다음과 같은 일이 일어납니다.
- 단추가 강조색 단추 시각적 처리를 수신합니다.
- 단추가 Enter 키에 자동으로 응답합니다.
    - 사용자가 키보드에서 Enter 키를 누르면 기본 단추와 연결된 클릭 처리기가 시작되고 ContentDialogResult가 기본값 단추와 연결된 값을 반환합니다.
    - 사용자가 Enter를 처리하는 컨트롤에 키보드 포커스를 둔 경우 기본 단추가 Enter 누름에 응답하지 않습니다.
- 대화 상자의 콘텐츠에 포커스 가능한 UI가 없는 경우 대화 상자가 열릴 때 단추가 자동으로 포커스를 받습니다.

ContentDialog.DefaultButton 속성을 사용하여 기본 단추를 나타냅니다. 기본적으로 기본 단추가 설정되지 않습니다.

![기본 단추를 포함한 3개의 단추가 있는 대화 상자](images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="confirmation-dialogs-okcancel"></a>확인 대화 상자(확인/취소)
확인 대화 상자에서는 사용자가 작업을 수행할지 확인할 수 있습니다. 작업을 확정하거나 취소하도록 선택할 수 있습니다.  
일반적인 확인 대화 상자에는 확정(“확인") 단추와 취소 단추 두 개가 있습니다.  

<ul>
    <li>
        <p>일반적으로 확정 단추는 왼쪽에(기본 단추), 취소 단추(보조 단추)는 오른쪽에 있어야 합니다.</p>
         ![확인/취소 대화 상자](images/dialogs/dialog_RS2_delete_file.png)

    </li>
    <li>일반 권장 사항 섹션에서 설명한 대로 기본 지시 사항이나 내용에 대한 특정 응답을 식별하는 텍스트가 있는 단추를 사용합니다.
    </li>
</ul>

> 일부 플랫폼에서는 확정 단추가 왼쪽 대신 오른쪽에 배치됩니다. 왼쪽에 배치하는 것을 권장하는 이유는 무엇일까요?  대부분의 사용자가 오른손잡이고 오른손으로 전화를 든다고 가정하면 확정 단추가 왼쪽에 있을 때 실제로 좀 더 편안하다고 느낍니다. 단추가 사용자의 엄지 손가락을 뻗어 닿기 편한 곳에 있기 때문입니다. 화면의 오른쪽에 있는 단추는 사용자가 엄지 손가락을 약간 불편한 위치로 안쪽으로 당겨야 합니다.

### <a name="create-a-dialog"></a>대화 상자 만들기
대화 상자를 만들려면 [ContentDialog 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)를 사용합니다. 코드나 태그에서 대화 상자를 만들 수 있습니다. 일반적으로 XAML에서 UI 요소를 정의하는 것이 더 쉽지만 간단한 대화 상자의 경우 코드를 사용하는 것이 실제로 더 쉽습니다. 이 예제에서는 WiFi 연결이 없음을 사용자에게 알리는 대화 상자를 만든 다음 [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드를 사용하여 대화 상자를 표시합니다.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

사용자가 대화 단추를 클릭하면 [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드는 사용자가 클릭한 단추를 알 수 있도록 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)를 반환합니다.

이 예제의 대화 상자는 질문한 다음 반환된 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)를 사용하여 사용자의 응답을 확인합니다.

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="flyouts"></a>플라이아웃
###  <a name="create-a-flyout"></a>플라이아웃 만들기

플라이아웃은 빠른 해제 컨테이너로 임의의 UI를 내용으로 표시할 수 있습니다. 플라이아웃에 중첩된 환경을 만들기 위한 다른 플라이아웃이나 상황에 맞는 메뉴가 포함될 수 있습니다.

![플라이아웃 내에 중첩된 상황에 맞는 메뉴](images/flyout-nested.png)

플라이아웃은 특정 컨트롤에 연결됩니다. [Placement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) 속성을 사용하여 플라이아웃이 표시되는 위치(위쪽, 왼쪽, 아래쪽, 오른쪽 또는 전체)를 지정할 수 있습니다. [전체 배치 모드](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode)를 선택하면 앱은 플라이아웃을 확대하고 앱 창 내에서 가운데로 지정합니다. [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)과 같은 일부 컨트롤은 플라이아웃이나 [상황에 맞는 메뉴](menus.md)를 연결하는 데 사용할 수 있는 [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) 속성을 제공합니다.

이 예에서는 단추를 누를 때 일부 텍스트를 표시하는 간단한 플라이아웃을 만듭니다.
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

컨트롤에 플라이아웃 속성이 없으면 대신 [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) 관련 속성을 사용할 수 있습니다. 이 속성을 사용할 경우 [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) 메서드도 호출해야 플라이아웃을 표시할 수 있습니다.

이 예제에서는 이미지에 간단한 플라이아웃을 추가합니다. 사용자가 이미지를 탭하면 앱이 플라이아웃을 표시합니다.

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
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

### <a name="style-a-flyout"></a>플라이아웃 스타일 지정
플라이아웃의 스타일을 지정하려면 해당 [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle)을 수정합니다. 이 예에서는 줄 바꿈 단락을 보여 주고, 화면 읽기 프로그램에서 텍스트 블록에 액세스하도록 설정합니다.

![텍스트 줄 바꿈을 포함한 액세스 가능한 플라이아웃](images/flyout-wrapping-text.png)

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

#### <a name="styling-flyouts-for-10-foot-experience"></a>3m 환경을 위한 플라이아웃 스타일 지정

플라이아웃과 같은 빠른 해제 컨트롤은 해제될 때까지 키보드 및 게임 패드 포커스를 임시 UI 내에 트래핑합니다. 이 동작에 대한 시각 신호를 제공하기 위해 Xbox의 빠른 해제 컨트롤은 범위를 벗어난 UI의 대비와 가시성을 줄이는 오버레이를 그립니다. 이 동작은 [`LightDismissOverlayMode`](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode) 속성으로 수정할 수 있습니다. 기본적으로 플라이아웃은 Xbox에 빠른 해제 오버레이를 그리지만 다른 장치 패밀리에는 그리지 않습니다. 그러나 앱에서 오버레이가 항상 **설정** 또는 항상 **해제** 상태가 되도록 선택할 수 있습니다.

![흐리게 표시 오버레이를 포함한 플라이아웃](images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

### <a name="light-dismiss-behavior"></a>빠른 해제 동작
다음을 포함한 빠른 해제 동작으로 플라이아웃을 닫을 수 있습니다.
-   플라이아웃 밖 탭
-   Esc 키보드 키 누르기
-   하드웨어 또는 소프트웨어 시스템의 뒤로 단추 누르기
-   게임 패드 B 단추 누르기

탭으로 해제 시 이 제스처는 대개 포함되고 UI 아래에 전달되지 않습니다. 예를 들어, 열려 있는 플라이아웃 뒤에 단추가 표시된 경우 사용자의 첫 번째 탭으로 플라이아웃이 해제되지만 이 단추가 활성화되지는 않습니다. 단추를 누르려면 두 번째 탭이 필요합니다.

플라이아웃에 대한 입력 통과 요소로 단추를 지정하여 이 동작을 변경할 수 있습니다. 플라이아웃이 위에 설명된 빠른 해제 작업의 결과로 닫히고 탭 이벤트를 지정된 `OverlayInputPassThroughElement`에 전달합니다. 이러한 동작을 채택하여 기능적으로 비슷한 항목에 대한 사용자 상호 작용 속도를 높일 수 있습니다. 앱에 즐겨찾기 컬렉션이 있고 컬렉션의 각 항목에 연결된 플라이아웃이 포함된 경우 사용자가 빠르게 여러 플라이아웃과 연달아 상호 작용할 것으로 예상할 수 있습니다.

[!NOTE] 파괴적 작업을 유발하는 오버레이 입력 통과 요소를 지정하지 않도록 주의하세요. 사용자가는 기본 UI를 활성화하지 않는 빠른 해제 작업에 익숙해졌습니다. 예기치 않은 파괴적 동작을 피하기 위해 닫기, 삭제 또는 비슷하게 파괴적인 단추가 빠른 해제 시 활성화되면 안 됩니다.

다음 예에서는 FavoritesBar 내의 3가지 단추 모두 첫 번째 탭에서 활성화됩니다.

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서
- [도구 설명](tooltips.md)
- [메뉴 및 상황에 맞는 메뉴](menus.md)
- [Flyout 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
