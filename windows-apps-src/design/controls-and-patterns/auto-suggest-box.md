---
Description: 사용자가 입력할 때 제안을 제공하는 텍스트 입력 상자입니다.
title: 자동 제안 상자에 대한 지침
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 86386c75407cb1132cc71766e4e126b7e0e3c81b
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339479"
---
# <a name="auto-suggest-box"></a>자동 제안 상자

AutoSuggestBox를 사용하여 사용자가 입력과 동시에 선택할 수 있는 제안 사항 목록을 제공합니다.

> **중요 API**: [AutoSuggestBox 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox), [TextChanged 이벤트](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged), [SuggestionChose 이벤트](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen), [QuerySubmitted 이벤트](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted)

![자동 제안 상자](images/controls/auto-suggest-box-open.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

제안 목록을 통한 텍스트 검색을 허용하는 간단하고 사용자 지정 가능한 컨트롤을 원하는 경우에는 자동 제안 상자를 선택합니다.

올바른 텍스트 컨트롤을 선택하는 방법에 대한 자세한 내용은 [텍스트 컨트롤](text-controls.md) 문서를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우에는 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/AutoSuggestBox">앱을 열고 작동 중인 AutoSuggestBox를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Groove 음악 앱의 자동 제안 상자입니다.

![Groove 음악 앱의 자동 제안 상자](images/control-examples/auto-suggest-box-groove.png)

## <a name="anatomy"></a>구조
자동 제안 상자에 대한 진입점은 선택적 힌트 텍스트가 있는 입력란과 선택적 헤더로 구성됩니다.

![자동 제안 컨트롤의 진입점 예](images/controls_autosuggest_entrypoint.png)

사용자가 텍스트 입력을 시작하면 자동 제안 결과 목록이 자동으로 채워집니다. 결과 목록은 텍스트 입력 상자의 위 또는 아래에 나타날 수 있습니다. "모두 지우기" 단추가 표시됩니다.

![확장된 자동 제안 컨트롤 예](images/controls_autosuggest_expanded01.png)

## <a name="create-an-auto-suggest-box"></a>자동 제안 상자 만들기

AutoSuggestBox를 사용하려면 세 가지 사용자 작업에 응답해야 합니다.

- 텍스트가 변경됨 - 사용자가 텍스트를 입력하면 제안 목록을 업데이트합니다.
- 제안 사항이 선택됨 - 사용자가 제안 사항 목록에서 제안 사항을 선택하면 입력란을 업데이트합니다.
- 쿼리가 제출됨 - 사용자가 쿼리를 제출하면 쿼리 결과를 표시합니다.

### <a name="text-changed"></a>텍스트가 변경됨

입력란의 콘텐츠가 업데이트될 때마다 [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged) 이벤트가 발생합니다. 이벤트 인수 [Reason](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason) 속성을 사용하여 사용자 입력으로 인해 변경되었는지 확인합니다. 변경 이유가 **UserInput**이면 입력에 따라 데이터를 필터링합니다. 그런 다음 필터링된 데이터를 AutoSuggestBox의 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)로 설정하여 제안 사항 목록을 업데이트합니다.

[DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) 또는 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)을 사용하여 제안 사항 목록에 항목이 표시되는 방법을 제어할 수 있습니다.

- 데이터 항목의 단일 속성에 대한 텍스트를 표시하려면 제안 사항 목록에 표시할 개체의 속성을 선택하도록 DisplayMemberPath 속성을 설정합니다.
- 목록에서 각 항목의 사용자 지정 모양을 정의하려면 ItemTemplate 속성을 사용합니다.

### <a name="suggestion-chosen"></a>제안 사항이 선택됨

키보드를 사용하여 제안 사항 목록을 탐색할 때 이에 일치하도록 입력란의 텍스트를 업데이트해야 합니다.

입력란에 표시될 데이터 개체의 속성을 선택하도록 [TextMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textmemberpath) 속성을 선택할 수 있습니다. TextMemberPath를 지정하는 경우 입력란이 자동으로 업데이트됩니다. 제안 사항 목록과 입력란의 텍스트가 동일하도록 일반적으로 DisplayMemberPath와 TextMemberPath의 값을 동일하게 지정해야 합니다.

간단한 속성을 두 개 이상 표시해야 하는 경우 선택한 항목에 따라 사용자 지정된 텍스트로 입력란을 채우도록 [SuggestionChosen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen) 이벤트를 처리합니다.

### <a name="query-submitted"></a>쿼리가 제출됨

앱에 적절한 쿼리 작업을 수행하고 사용자에게 결과를 표시하도록 [QuerySubmitted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted) 이벤트를 처리합니다.

QuerySubmitted 이벤트는 사용자가 쿼리 문자열을 커밋할 때 발생합니다. 사용자는 다음과 같은 방법 중 하나로 쿼리를 커밋할 수 있습니다.
- 포커스가 입력란에 있는 동안 Enter 키를 누르거나 쿼리 아이콘을 클릭합니다. 이벤트 인수 [ChosenSuggestion](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion) 속성은 **null**입니다.
- 포커스가 제안 목록에 있는 동안 Enter 키를 누르거나 항목을 클릭하거나 탭합니다. 이벤트 인수 ChosenSuggestion 속성은 목록에서 선택된 항목을 포함합니다.

모든 경우에 이벤트 인수 [QueryText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext) 속성은 입력란의 텍스트를 포함합니다.

다음은 필수 이벤트 처리기가 포함된 간단한 AutoSuggestBox입니다.

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## <a name="use-autosuggestbox-for-search"></a>검색에 AutoSuggestBox 사용

AutoSuggestBox를 사용하여 사용자가 입력과 동시에 선택할 수 있는 제안 사항 목록을 제공합니다.

기본적으로 텍스트 입력 상자에 쿼리 단추가 표시되지 않습니다. 입력란의 오른쪽에 지정된 아이콘과 함께 단추를 추가하려면 [QueryIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.queryicon) 속성을 설정할 수 있습니다. 예를 들어 AutoSuggestBox를 일반적인 검색 상자와 같이 표시하려면 다음과 같이 '찾기' 아이콘을 추가합니다.

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

다음은 '찾기' 아이콘이 있는 AutoSuggestBox입니다.

![자동 제안 컨트롤의 진입점 예](images/controls_autosuggest_entrypoint.png)

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

-   자동 제안 상자를 사용하여 검색을 수행했는데 입력한 텍스트에 대한 검색 결과가 없는 경우, 사용자가 검색 요청이 실행되었음을 알 수 있도록 "결과 없음" 메시지 한 줄을 결과로 표시하는 것이 좋습니다.

    ![검색 결과가 없는 자동 제안 상자의 예](images/controls_autosuggest_noresults.png)

<!--
<div class="microsoft-internal-note">
**Globalization and localization checklist**

<table>
<tr>
<th>Vertical spacing</th><td>Use non-Latin characters for vertical spacing to ensure non-Latin scripts will display properly, including numbers.</td>
</tr>
<tr>
<th>Scrolling</th><td>When auto suggest text is selected, user should be able to scroll to end of string.</td>
</tr>
</table>
</div>
-->

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.
- [AutoSuggestBox 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>관련 문서

- [텍스트 컨트롤](text-controls.md)
- [맞춤법 검사](text-controls.md)
- [검색](search.md)
- [TextBox 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length 속성](https://docs.microsoft.com/dotnet/api/system.string.length)
