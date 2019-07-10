---
Description: 사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 터치 키보드나 SIP(Soft Input Panel)를 사용한 데이터 입력을 도울 수 있습니다.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 입력 범위를 사용하여 터치 키보드 변경
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: 키보드, 손쉬운 사용, 탐색, 포커스, 텍스트, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: c522e21c45a3edd08a14b081cc227a83f19a3ea0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365362"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>입력 범위를 사용하여 터치 키보드 변경

사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 터치 키보드나 SIP(Soft Input Panel)를 사용한 데이터 입력을 도울 수 있습니다.

### <a name="important-apis"></a>중요 API
- [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


터치 키보드는 앱이 터치 스크린이 있는 디바이스에서 실행될 때 텍스트 입력에 사용할 수 있습니다. 터치 키보드는 사용자가 **[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 또는 **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** 같이 편집 가능한 입력 필드를 탭할 때 호출됩니다. 사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 *입력 범위*를 설정하여 사용자가 앱에서 데이터를 쉽고 빠르게 입력할 수 있도록 지원할 수 있습니다. 입력 범위는 시스템에서 해당 입력 형식에 맞는 특수한 터치 키보드를 제공할 수 있도록 컨트롤에서 예상되는 텍스트 입력 형식에 대한 힌트를 시스템에 제공합니다.

예를 들어 텍스트 상자가 4자리 숫자의 PIN을 입력하는 목적으로만 사용될 경우 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 속성을 **Number**로 설정합니다. 이렇게 하면 사용자가 PIN을 쉽게 입력할 수 있도록 시스템에서 숫자 키패드 레이아웃이 표시됩니다.

> [!IMPORTANT]
> - 이 정보는 SIP에만 적용됩니다. Windows 접근성 옵션에서 제공되는 화상 키보드 또는 하드웨어 키보드에는 적용되지 않습니다.
> - 입력 범위는 입력 유효성 검사가 수행되지 않으며 사용자가 하드웨어 키보드 또는 다른 입력 디바이스를 사용해서 입력을 제공하지 못하도록 방지하지 않습니다. 따라서 필요에 따라 입력 코드에 대한 유효성을 검사해야 합니다.

## <a name="changing-the-input-scope-of-a-text-control"></a>텍스트 컨트롤의 입력 범위 변경

앱에서 사용할 수 있는 입력 범위는 **[InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** 열거의 멤버입니다. 설정할 수 있습니다 합니다 **InputScope** 의 속성을 **[텍스트 상자](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 또는 **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** 다음 중 하나를 값입니다.

> [!IMPORTANT]
> 합니다 **[InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** 속성을 **[PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** 만 지원 합니다 **암호** 및  **NumericPin** 값입니다. 다른 값이 무시됩니다.

여기서는 여러 텍스트 상자의 예상 데이터와 일치하도록 각 텍스트 상자의 입력 범위를 변경할 수 있습니다.

**XAML의 입력된 범위를 변경 하려면**

1.  페이지의 XAML 파일에서 변경하려는 텍스트 컨트롤에 대한 태그를 찾습니다.
2.  [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 특성을 태그에 추가하고 예상되는 입력과 일치하는 [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 값을 지정합니다.

    다음은 일반적인 고객 연락처 양식에서 볼 수 있는 몇 가지 입력란입니다. [  **InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)가 설정된 상태에서 데이터에 적합한 레이아웃의 터치 키보드가 각 입력란에 대해 표시됩니다.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**코드에서 입력된 범위를 변경 하려면**

1.  페이지의 XAML 파일에서 변경하려는 텍스트 컨트롤에 대한 태그를 찾습니다. 설정되지 않은 경우, 해당 코드에서 컨트롤을 참조할 수 있도록 [x:Name 특성](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute)을 설정합니다.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  새 [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope) 개체를 인스턴스화합니다.

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  새 [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 개체를 인스턴스화합니다.
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  [**InputScopeName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) 개체의 [**NameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 속성을 [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 열거형 값으로 설정합니다.

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 개체를 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope.names) 개체의 [**Names**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope) 컬렉션에 추가합니다.

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope) 개체를 텍스트 컨트롤의 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 속성 값으로 설정합니다.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

코드를 모두 합하면 다음과 같습니다.

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

다음과 같은 단축형 코드를 사용해서 위와 동일한 단계를 압축시킬 수 있습니다.

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>텍스트 자동 완성, 맞춤법 검사 및 자동 고침

[**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 및 [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 컨트롤은 SIP의 동작에 영향을 주는 몇 가지 속성을 포함합니다. 사용자에게 최상의 환경을 제공하기 위해서는 이러한 속성이 터치를 사용한 텍스트 입력에 어떤 영향을 주는지 이해하는 것이 중요합니다.

-   [**IsSpellCheckEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled)-컨트롤을 인식 하지 못하는 단어가 표시 되는 시스템의 맞춤법 검사 엔진과 상호 작용 하는 맞춤법 검사 텍스트 컨트롤에 사용 됩니다. 단어를 탭하면 제안된 수정 단어 목록을 볼 수 있습니다. 기본적으로 맞춤법 검사는 사용하도록 설정되어 있습니다.

    **Default** 입력 범위의 경우 이 속성은 문장의 첫 번째 단어 자동 대문자 표시 및 입력 단어 자동 고침을 활성화합니다. 이러한 자동 수정 기능은 다른 입력 범위에서 비활성화할 수 있습니다. 자세한 내용은 이 항목의 뒷부분에 있는 표를 참조하세요.

-   [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled)-텍스트 예측 텍스트 컨트롤을 사용 하도록 구성 되어 시스템 형식으로 시작 될 수 있습니다 하는 단어의 목록을 표시 합니다. 전체 단어를 입력하는 대신 목록에서 쉽게 선택할 수 있습니다. 텍스트 자동 완성은 기본적으로 사용됩니다.

    입력 범위가 **Default**가 아니면 [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) 속성이 **true**이더라도 텍스트 자동 완성이 비활성화될 수 있습니다. 자세한 내용은 이 항목의 뒷부분에 있는 표를 참조하세요.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus)—이 속성을 **true**, 시스템에서 포커스 텍스트 컨트롤에 프로그래밍 방식으로 설정 된 경우 SIP를 표시 되지 않습니다. 대신 사용자가 컨트롤을 사용할 때만 키보드가 표시됩니다.

## <a name="touch-keyboard-index-for-windows"></a>Windows에 대 한 터치 키보드 인덱스

이러한 테이블에는 일반적인 입력된 범위 값에 대 한 Windows 소프트 입력 패널 (SIP) 레이아웃을 보여 줍니다. **IsSpellCheckEnabled** 및 **IsTextPredictionEnabled** 속성으로 활성화되는 기능에 입력 범위가 미치는 효과가 각 입력 범위에 대해 나열되어 있습니다. 이 목록은 사용 가능한 입력 범위에 대한 전체 목록이 아닙니다.

> [!Tip] 
> 키를 눌러 대부분의 터치 키보드 레이아웃을 알파벳 및 숫자 및 기호 레이아웃 간에 전환할 수 있습니다 합니다 **123 &** 누릅니다 숫자 및 기호 레이아웃을 변경 하는 키를 **abcd** 키 알파벳 레이아웃을 변경 합니다.

### <a name="default"></a>기본값

`<TextBox InputScope="Default"/>`

기본 Windows 터치 키보드.

![기본 Windows 터치 키보드](images/input-scopes/default.png)
- 맞춤법 검사: **IsSpellCheckEnabled** = **true**의 경우 활성화, **IsSpellCheckEnabled** = **false**의 경우 비활성화
- 자동 고침: **IsSpellCheckEnabled** = **true**의 경우 활성화, **IsSpellCheckEnabled** = **false**의 경우 비활성화
- 자동 대문자화: **IsSpellCheckEnabled** = **true**의 경우 활성화, **IsSpellCheckEnabled** = **false**의 경우 비활성화
- 텍스트 자동 완성: **IsTextPredictionEnabled** = **true**의 경우 활성화, **IsTextPredictionEnabled** = **false**의 경우 비활성화

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

기본 숫자 및 기호 키보드 레이아웃입니다.

![통화용 Windows 터치 키보드](images/input-scopes/currencyamountandsymbol.png)

- 자세한 기호를 표시 하려면 페이지 왼쪽/오른쪽 키를 포함 합니다.
- 맞춤법 검사: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 수정: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 대문자화: 항상 비활성화됨
- 텍스트 자동 완성: 기본적으로 설정되어 있으며, 비활성화할 수 있음
 
### <a name="url"></a>url

`<TextBox InputScope="Url"/>`

![URL용 Windows 터치 키보드](images/input-scopes/url.png)

- **.com** 및 ![이동 키](images/input-scopes/kbdgokey.png)를 포함합니다. 키를 누른 합니다 **.com** 추가 옵션을 표시 하는 키 ( **.org**에 **.net**, 및 지역별 접미사)
- 포함 된 **:** 를 **-** , 및 **/** 키
- 맞춤법 검사: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 자동 수정: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 자동 대문자화: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 텍스트 자동 완성: 기본적으로 해제되어 있으며, 활성화할 수 있음


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![메일 주소용 Windows 터치 키보드](images/input-scopes/emailsmtpaddress.png)
- **@**  and **.com** 키를 포함합니다. 키를 누른 합니다 **.com** 추가 옵션을 표시 하는 키 ( **.org**에 **.net**, 및 지역별 접미사)
- 포함 된 **_** 하 고 **-** 키
- 맞춤법 검사: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 자동 수정: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 자동 대문자화: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 텍스트 자동 완성: 기본적으로 해제되어 있으며, 활성화할 수 있음


### <a name="number"></a>Number

`<TextBox InputScope="Number"/>`

![숫자용 Windows 터치 키보드](images/input-scopes/number.png)
- 맞춤법 검사: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 수정: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 대문자화: 항상 비활성화됨
- 텍스트 자동 완성: 기본적으로 설정되어 있으며, 비활성화할 수 있음

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![전화 번호용 Windows 터치 키보드](images/input-scopes/telephonenumber.png)
- 맞춤법 검사: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 수정: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 대문자화: 항상 비활성화됨
- 텍스트 자동 완성: 기본적으로 설정되어 있으며, 비활성화할 수 있음

### <a name="search"></a>검색

`<TextBox InputScope="Search"/>`

![검색용 Windows 터치 키보드](images/input-scopes/search.png)
- 포함 된 **검색** 대신 키를 **Enter** 키
- 맞춤법 검사: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 수정: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 대문자화: 항상 비활성화됨
- 텍스트 자동 완성: 기본적으로 설정되어 있으며, 비활성화할 수 있음

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![증분 검색에 대 한 Windows 터치 키보드](images/input-scopes/searchincremental.png)
- 동일한 레이아웃 **기본**
- 맞춤법 검사: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 자동 수정: 항상 비활성화됨
- 자동 대문자화: 항상 비활성화됨
- 텍스트 자동 완성: 항상 비활성화됨

### <a name="formula"></a>수식

`<TextBox InputScope="Formula"/>`

![수식에 대 한 Windows 터치 키보드](images/input-scopes/formula.png)
- 포함 된 **=** 키
- 또한 합니다 **%** 를 **$** , 및 **+** 키
- 맞춤법 검사: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 수정: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 대문자화: 항상 비활성화됨
- 텍스트 자동 완성: 기본적으로 설정되어 있으며, 비활성화할 수 있음

### <a name="chat"></a>채팅

`<TextBox InputScope="Chat"/>`

![기본 Windows 터치 키보드](images/input-scopes/default.png)
- 동일한 레이아웃 **기본**
- 맞춤법 검사: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 수정: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 자동 대문자화: 기본적으로 설정되어 있으며, 비활성화할 수 있음
- 텍스트 자동 완성: 기본적으로 설정되어 있으며, 비활성화할 수 있음

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![기본 Windows 터치 키보드](images/input-scopes/default.png)
- 동일한 레이아웃 **기본**
- 맞춤법 검사: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 자동 수정: 기본적으로 해제되어 있으며, 활성화할 수 있음
- 자동 대문자 표시: 해제 기본적으로 사용할 수 있습니다 (각 단어의 첫 글자가 대문자로 표시)
- 텍스트 자동 완성: 기본적으로 해제되어 있으며, 활성화할 수 있음
