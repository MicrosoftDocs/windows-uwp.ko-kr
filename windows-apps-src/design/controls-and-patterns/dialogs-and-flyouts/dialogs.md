---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 대화 상자 컨트롤
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9ba4bfcd38acba2bcd7c8399b8b17184edacc15a
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5638773"
---
## <a name="dialog-controls"></a>대화 상자 컨트롤

대화 상자 컨트롤은 상황에 맞는 앱 정보를 제공 하는 모달 UI 오버레이입니다. 명시적으로 닫을 때까지 앱 창의 조작을 차단 있습니다. 종종 사용자의 작업을 요청하기도 합니다.

![대화 상자 예제](../images/dialogs/dialog_RS2_delete_file.png)


> **중요 Api**: [ContentDialog 클래스](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

대화 상자를 사용하여 사용자에게 중요한 정보를 알리거나 확인 또는 추가 정보를 요청한 후에 작업을 완료할 수 있습니다.

권장 사항 사용 하는 경우에 대화 상자 및 플라이 아웃 (유사한 컨트롤)를 사용 하는 경우 [대화 상자 및 플라이 아웃](index.md)참조 하세요. 

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 작동 중인 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 또는 <a href="xamlcontrolsgallery:/item/Flyout">플라이아웃</a>을 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="general-guidelines"></a>일반 지침

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
- [ContentDialog 클래스](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)를 사용하여 대화 상자 환경을 구축합니다. 사용되지 않는 MessageDialog API를 사용하지 않습니다.

## <a name="how-to-create-a-dialog"></a>대화 상자를 만드는 방법
대화 상자를 만들려면 [ContentDialog 클래스](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)를 사용합니다. 코드나 태그에서 대화 상자를 만들 수 있습니다. 일반적으로 XAML에서 UI 요소를 정의하는 것이 더 쉽지만 간단한 대화 상자의 경우 코드를 사용하는 것이 실제로 더 쉽습니다. 이 예제에서는 WiFi 연결이 없음을 사용자에게 알리는 대화 상자를 만든 다음 [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드를 사용하여 대화 상자를 표시합니다.

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

사용자가 대화 단추를 클릭하면 [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드는 사용자가 클릭한 단추를 알 수 있도록 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)를 반환합니다.

이 예제의 대화 상자는 질문한 다음 반환된 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)를 사용하여 사용자의 응답을 확인합니다.

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

## <a name="provide-a-safe-action"></a>안전한 작업을 제공 합니다.
대화 상자는 사용자 상호 작용을 차단하고 단추는 사용자가 대화 상자를 해제하는 기본 메커니즘이기 때문에 대화 상자에 "닫기"나 "확인" 등의 "안전한" 비파괴 단추가 하나 이상 있는지 확인하세요. **모든 대화 상자에 대화 상자를 닫기 위한 안전한 작업 단추가 하나 이상 있어야 합니다.** 이를 통해 사용자가 작업을 수행하지 않고 대화 상자를 닫을 수 있습니다.<br>![단추가 1개인 대화 상자](../images/dialogs/dialog_RS2_one_button.png)

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

![단추가 2개인 대화 상자](../images/dialogs/dialog_RS2_two_button.png)

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

![단추가 3개인 대화 상자](../images/dialogs/dialog_RS2_three_button.png)

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

## <a name="the-three-dialog-buttons"></a>3개의 대화 상자 단추
ContentDialog에는 대화 상자 환경을 구현하는 데 사용할 수 있는 3가지 단추가 있습니다.

- **CloseButton** - 필수 - 사용자가 대화 상자를 종료할 수 있게 하는 안전한 비파괴 작업을 나타냅니다. 맨 오른쪽 단추로 나타납니다.
- **PrimaryButton** - 선택 사항 - 첫 번째 "권장 사항" 작업을 나타냅니다. 맨 왼쪽 단추로 나타납니다.
- **SecondaryButton** - 선택 사항 - 두 번째 "권장 사항" 작업을 나타냅니다. 가운데 단추로 나타납니다.

기본 제공 단추를 사용하면 단추가 적절히 배치되고, 단추가 키보드 이벤트에 제대로 응답하고, 화상 키보드가 실행 중일 때도 명령 영역이 계속 표시되며, 대화 상자가 다른 대화 상자와 일관성 있게 표시됩니다.

### <a name="closebutton"></a>CloseButton
모든 대화 상자에 사용자가 대화 상자를 종료하는 데 사용할 수 있는 안전한 비파괴 작업 단추가 있어야 합니다.

ContentDialog.CloseButton API를 사용하여 이 단추를 만듭니다. 이를 통해 마우스, 키보드, 터치 및 게임 패드를 포함한 모든 입력에 대한 올바른 사용자 환경을 구축할 수 있습니다. 다음과 같은 입력이 여기에 해당합니다.
<ol>
    <li>사용자가 CloseButton을 클릭하거나 탭함 </li>
    <li>사용자가 시스템 뒤로 단추를 누름 </li>
    <li>사용자가 키보드의 ESC 단추를 누름 </li>
    <li>사용자가 게임 패드 B를 누름 </li>
</ol>

사용자가 대화 단추를 클릭하면 [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드는 사용자가 클릭한 단추를 알 수 있도록 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)를 반환합니다. CloseButton을 누르면 ContentDialogResult.None이 반환됩니다.

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton 및 SecondaryButton
CloseButton 외에도 필요에 따라 기본 지시 사항과 관련된 1-2개의 단추를 사용자에게 제공할 수 있습니다.
첫 번째 "권장 사항" 작업에 PrimaryButton을 활용하고 두 번째 "권장 사항" 작업에 SecondaryButton을 활용합니다. 단추가 3개인 대화 상자에서 PrimaryButton은 대개 긍정 "권장 사항" 작업을 나타내지만 SecondaryButton은 대개 중립 또는 보조 "권장 사항" 작업을 나타냅니다.
예를 들어, 앱에서 사용자에게 서비스에 가입하라는 메시지를 표시할 수 있습니다. 긍정 "권장 사항" 작업으로서 PrimaryButton은 가입 텍스트를 호스트하지만 중립 "권장 사항" 작업으로서 SecondaryButton은 체험해 보기 텍스트를 호스트합니다. CloseButton을 사용하면 사용자가 어떤 작업도 수행하지 않고 취소할 수 있습니다.

사용자가 PrimaryButton을 클릭하면 [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드가 ContentDialogResult.Primary를 반환해야 합니다.
사용자가 SecondaryButton을 클릭하면 [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 메서드가 ContentDialogResult.Secondary를 반환해야 합니다.

![단추가 3개인 대화 상자](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
필요에 따라 기본 단추로 세 가지 단추 중 하나를 식별하도록 선택할 수 있습니다. 기본 단추를 지정하면 다음과 같은 일이 일어납니다.
- 단추가 강조색 단추 시각적 처리를 수신합니다.
- 단추가 Enter 키에 자동으로 응답합니다.
    - 사용자가 키보드에서 Enter 키를 누르면 기본 단추와 연결된 클릭 처리기가 시작되고 ContentDialogResult가 기본값 단추와 연결된 값을 반환합니다.
    - 사용자가 Enter를 처리하는 컨트롤에 키보드 포커스를 둔 경우 기본 단추가 Enter 누름에 응답하지 않습니다.
- 대화 상자의 콘텐츠에 포커스 가능한 UI가 없는 경우 대화 상자가 열릴 때 단추가 자동으로 포커스를 받습니다.

ContentDialog.DefaultButton 속성을 사용하여 기본 단추를 나타냅니다. 기본적으로 기본 단추가 설정되지 않습니다.

![기본 단추를 포함한 3개의 단추가 있는 대화 상자](../images/dialogs/dialog_RS2_three_button_default.png)

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

## <a name="confirmation-dialogs-okcancel"></a>확인 대화 상자(확인/취소)
확인 대화 상자에서는 사용자가 작업을 수행할지 확인할 수 있습니다. 작업을 확정하거나 취소하도록 선택할 수 있습니다.  
일반적인 확인 대화 상자에는 확정(“확인") 단추와 취소 단추 두 개가 있습니다.  

<ul>
    <li>
        <p>일반적으로 확정 단추는 왼쪽에(기본 단추), 취소 단추(보조 단추)는 오른쪽에 있어야 합니다.</p>
        <img alt="An OK/cancel dialog" src="../images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>일반 권장 사항 섹션에서 설명한 대로 기본 지시 사항이나 내용에 대한 특정 응답을 식별하는 텍스트가 있는 단추를 사용합니다.
    </li>
</ul>

> 일부 플랫폼에서는 확정 단추가 왼쪽 대신 오른쪽에 배치됩니다. 왼쪽에 배치하는 것을 권장하는 이유는 무엇일까요?  대부분의 사용자가 오른손잡이고 오른손으로 전화를 든다고 가정하면 확정 단추가 왼쪽에 있을 때 실제로 좀 더 편안하다고 느낍니다. 단추가 사용자의 엄지 손가락을 뻗어 닿기 편한 곳에 있기 때문입니다. 화면의 오른쪽에 있는 단추는 사용자가 엄지 손가락을 약간 불편한 위치로 안쪽으로 당겨야 합니다.





## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서
- [도구 설명](../tooltips.md)
- [메뉴 및 상황에 맞는 메뉴](../menus.md)
- [Flyout 클래스](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 클래스](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
