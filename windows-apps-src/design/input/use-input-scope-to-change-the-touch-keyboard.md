---
Description: 사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 터치 키보드나 SIP(Soft Input Panel)를 사용한 데이터 입력을 도울 수 있습니다.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 입력 범위를 사용하여 터치 키보드 변경
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: 키보드, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 상호 작용
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: e6e140a1967ca3ffe7775f427ccae7a7e07c5ca6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165817"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>입력 범위를 사용하여 터치 키보드 변경

사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 터치 키보드나 SIP(Soft Input Panel)를 사용한 데이터 입력을 도울 수 있습니다.

### <a name="important-apis"></a>중요 API
- [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


터치 키보드는 앱이 터치 스크린이 있는 디바이스에서 실행될 때 텍스트 입력에 사용할 수 있습니다. 터치 키보드는 **[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 또는 **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** 같은 편집 가능한 입력 필드를 탭 할 때 호출 됩니다. 사용자가 입력 해야 하는 데이터의 종류와 일치 하도록 텍스트 컨트롤의 *입력 범위* 를 설정 하 여 사용자가 앱에서 데이터를 훨씬 더 빠르고 쉽게 입력할 수 있습니다. 입력 범위는 시스템에서 해당 입력 형식에 맞는 특수한 터치 키보드를 제공할 수 있도록 컨트롤에서 예상되는 텍스트 입력 형식에 대한 힌트를 시스템에 제공합니다.

예를 들어 텍스트 상자를 4 자리 PIN을 입력 하는 데만 사용 하는 경우 [**Inputscope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 속성을 **Number**로 설정 합니다. 이렇게 하면 사용자가 PIN을 쉽게 입력할 수 있도록 시스템에서 숫자 키패드 레이아웃이 표시됩니다.

> [!IMPORTANT]
> - 이 정보는 SIP에만 적용 됩니다. Windows의 접근성 옵션에서 사용 가능한 하드웨어 키보드나 화상 키보드에는 적용 되지 않습니다.
> - 입력 범위는 입력 유효성 검사를 수행 하지 않으며, 사용자가 하드웨어 키보드나 기타 입력 장치를 통해 입력을 제공 하는 것을 방지 하지 않습니다. 따라서 필요에 따라 입력 코드에 대한 유효성을 검사해야 합니다.

## <a name="changing-the-input-scope-of-a-text-control"></a>텍스트 컨트롤의 입력 범위 변경

앱에 사용할 수 있는 입력 범위는 **[InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** 열거형의 멤버입니다. **[텍스트 상자](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 또는 **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** 의 **inputscope** 속성을 다음 값 중 하나로 설정할 수 있습니다.

> [!IMPORTANT]
> **[Passwordbox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** 의 **[Inputscope](/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** 속성은 **Password** 및 **NumericPin** 값만 지원 합니다. 다른 값이 무시됩니다.

여기서는 각 텍스트 상자에 대해 예상 되는 데이터와 일치 하도록 여러 텍스트 상자의 입력 범위를 변경 합니다.

**XAML에서 입력 범위를 변경 하려면**

1.  페이지의 XAML 파일에서 변경 하려는 텍스트 컨트롤에 대 한 태그를 찾습니다.
2.  [**Inputscope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 특성을 태그에 추가 하 고 예상 입력과 일치 하는 [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 값을 지정 합니다.

    다음은 일반적인 고객 연락처 양식에 나타날 수 있는 몇 가지 텍스트 상자입니다. [**Inputscope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 집합을 사용 하 여 데이터에 적합 한 레이아웃을 포함 하는 터치 키보드는 각 텍스트 상자에 대해 표시 됩니다.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**코드에서 입력 범위를 변경 하려면**

1.  페이지의 XAML 파일에서 변경 하려는 텍스트 컨트롤에 대 한 태그를 찾습니다. 설정 되지 않은 경우 코드에서 컨트롤을 참조할 수 있도록 [x:Name 특성](../../xaml-platform/x-name-attribute.md) 을 설정 합니다.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  새 [**Inputscope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) 개체를 인스턴스화합니다.

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  새 [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 개체를 인스턴스화합니다.
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 개체의 [**NameValue**](/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) 속성을 [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 열거형 값으로 설정 합니다.

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  [**Inputscope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) 개체의 [**Names**](/uwp/api/windows.ui.xaml.input.inputscope.names) 컬렉션에 [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 개체를 추가 합니다.

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  [**Inputscope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) 개체를 text 컨트롤의 [**inputscope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 속성 값으로 설정 합니다.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

코드는 모두 함께 나와 있습니다.

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

동일한 단계를이 약식 코드에 압축할 수 있습니다.

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>텍스트 예측, 맞춤법 검사 및 자동 수정

[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 및 [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 컨트롤에는 SIP의 동작에 영향을 주는 몇 가지 속성이 있습니다. 사용자에 게 최상의 환경을 제공 하려면 터치를 사용 하 여 이러한 속성이 텍스트 입력에 미치는 영향을 이해 하는 것이 중요 합니다.

-   [**IsSpellCheckEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled)-텍스트 컨트롤에 맞춤법 검사를 사용 하는 경우 컨트롤은 시스템의 맞춤법 검사 엔진과 상호 작용 하 여 인식 되지 않는 단어를 표시 합니다. 단어를 누르면 제안 된 수정 목록을 볼 수 있습니다. 맞춤법 검사는 기본적으로 사용 하도록 설정 되어 있습니다.

    **기본** 입력 범위에 대해이 속성을 사용 하면 문장의 첫 단어에 대 한 자동 대문자화를 사용 하 고 입력할 때 단어를 자동으로 수정할 수도 있습니다. 이러한 자동 수정 기능은 다른 입력 범위에서 사용 하지 않도록 설정할 수 있습니다. 자세한 내용은이 항목의 뒷부분에 나오는 표를 참조 하십시오.

-   [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled)-텍스트 컨트롤에 대해 텍스트 예측을 사용 하는 경우 입력 하기 시작할 수 있는 단어 목록이 시스템에 표시 됩니다. 전체 단어를 입력할 필요가 없도록 목록에서 선택할 수 있습니다. 텍스트 예측은 기본적으로 사용 하도록 설정 되어 있습니다.

    [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) 속성이 **true**인 경우에도 입력 범위가 **기본값이**아닌 경우 텍스트 예측을 사용 하지 않도록 설정할 수 있습니다. 자세한 내용은이 항목의 뒷부분에 나오는 표를 참조 하십시오.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus)-이 속성이 **true**이면 포커스가 텍스트 컨트롤에 프로그래밍 방식으로 설정 된 경우 시스템에서 SIP를 표시 하지 않습니다. 대신 사용자가 컨트롤과 상호 작용 하는 경우에만 키보드를 표시 합니다.

## <a name="touch-keyboard-index-for-windows"></a>Windows 용 터치 키보드 인덱스

다음 표에서는 일반적인 입력 범위 값에 대 한 Windows SIP (Soft Input Panel) 레이아웃을 보여 줍니다. **IsSpellCheckEnabled** 및 **IsTextPredictionEnabled** 속성에 의해 설정 된 기능에 대 한 입력 범위의 효과는 각 입력 범위에 대해 나열 됩니다. 이는 사용 가능한 입력 범위의 포괄적인 목록이 아닙니다.

> [!Tip] 
> **&123** 키를 눌러 숫자 및 기호 레이아웃으로 변경 하 고, **abcd** 키를 눌러 사전순 레이아웃으로 변경 하 여 영문자 레이아웃과 숫자 및 기호 레이아웃 사이에서 대부분의 터치 키보드를 설정/해제할 수 있습니다.

### <a name="default"></a>기본값

`<TextBox InputScope="Default"/>`

기본 Windows touch 키보드입니다.

![기본 Windows touch 키보드](images/input-scopes/default.png)
- 맞춤법 검사: **IsSpellCheckEnabled**  =  **true**이면 사용, **IsSpellCheckEnabled**  =  **false** 인 경우 사용 안 함
- 자동 수정: true 이면 **IsSpellCheckEnabled**  =  **true**이 고, 그렇지 않으면 **IsSpellCheckEnabled**  =  **false** 이면 사용 안 함입니다.
- 자동 대문자 표시: **IsSpellCheckEnabled**  =  **true**이면 enabled, **IsSpellCheckEnabled**  =  **false** 인 경우 사용 안 함
- 텍스트 예측: **IsTextPredictionEnabled**true 이면 사용 하도록 설정  =  **true**하 고 **IsTextPredictionEnabled**  =  **false** 인 경우 사용 하지 않도록 설정 합니다.

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

기본 숫자 및 기호 키보드 레이아웃입니다.

![통화에 대 한 Windows touch 키보드](images/input-scopes/currencyamountandsymbol.png)

- 추가 기호를 표시 하는 페이지 왼쪽/오른쪽 키를 포함 합니다.
- 맞춤법 검사: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 대문자화: 항상 사용 안 함
- 텍스트 예측: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
 
### <a name="url"></a>Url

`<TextBox InputScope="Url"/>`

![Url에 대 한 Windows touch 키보드](images/input-scopes/url.png)

- **.Com** 및 ![ go 키 ](images/input-scopes/kbdgokey.png) (go) 키를 포함 합니다. **.Com** 키를 길게 눌러 추가 옵션 (**org**, **.net**및 지역별 접미사)을 표시 합니다.
- 에는 **:**, **-** 및 키가 포함 되어 있습니다. **/**
- 맞춤법 검사: 기본적으로 해제 되어 있으므로 사용 하도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 해제 되어 있으므로 사용 하도록 설정할 수 있습니다.
- 자동 대문자화: 기본적으로 해제 되어 있을 수 있습니다.
- 텍스트 예측: 기본적으로 해제 되어 있을 수 있습니다.


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![전자 메일 주소에 대 한 Windows touch 키보드](images/input-scopes/emailsmtpaddress.png)
- **@** 및 **.com** 키를 포함 합니다. **.Com** 키를 길게 눌러 추가 옵션 (**org**, **.net**및 지역별 접미사)을 표시 합니다.
- **_** 및 키를 포함 합니다. **-**
- 맞춤법 검사: 기본적으로 해제 되어 있으므로 사용 하도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 해제 되어 있으므로 사용 하도록 설정할 수 있습니다.
- 자동 대문자화: 기본적으로 해제 되어 있을 수 있습니다.
- 텍스트 예측: 기본적으로 해제 되어 있을 수 있습니다.


### <a name="number"></a>Number

`<TextBox InputScope="Number"/>`

![숫자에 대 한 Windows touch 키보드](images/input-scopes/number.png)
- 맞춤법 검사: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 대문자화: 항상 사용 안 함
- 텍스트 예측: 기본적으로 사용 하지 않도록 설정할 수 있습니다.

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![전화 번호에 대 한 Windows touch 키보드](images/input-scopes/telephonenumber.png)
- 맞춤법 검사: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 대문자화: 항상 사용 안 함
- 텍스트 예측: 기본적으로 사용 하지 않도록 설정할 수 있습니다.

### <a name="search"></a>검색

`<TextBox InputScope="Search"/>`

![검색을 위한 Windows touch 키보드](images/input-scopes/search.png)
- **Enter** 키 대신 **검색** 키를 포함 합니다.
- 맞춤법 검사: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 대문자화: 항상 사용 안 함
- 텍스트 예측: 기본적으로 사용 하지 않도록 설정할 수 있습니다.

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![증분 검색을 위한 Windows touch 키보드](images/input-scopes/searchincremental.png)
- **기본값과** 동일한 레이아웃
- 맞춤법 검사: 기본적으로 해제 되어 있으므로 사용 하도록 설정할 수 있습니다.
- 자동 수정: 항상 사용 안 함
- 자동 대문자화: 항상 사용 안 함
- 텍스트 예측: 항상 사용 안 함

### <a name="formula"></a>수식

`<TextBox InputScope="Formula"/>`

![수식의 Windows touch 키보드](images/input-scopes/formula.png)
- 키를 포함 합니다. **=**
- **%** 에는, **$** 및 **+** 키도 포함 되어 있습니다.
- 맞춤법 검사: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 대문자화: 항상 사용 안 함
- 텍스트 예측: 기본적으로 사용 하지 않도록 설정할 수 있습니다.

### <a name="chat"></a>채팅

`<TextBox InputScope="Chat"/>`

![기본 Windows touch 키보드](images/input-scopes/default.png)
- **기본값과** 동일한 레이아웃
- 맞춤법 검사: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 자동 대문자화: 기본적으로 사용 하지 않도록 설정할 수 있습니다.
- 텍스트 예측: 기본적으로 사용 하지 않도록 설정할 수 있습니다.

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![기본 Windows touch 키보드](images/input-scopes/default.png)
- **기본값과** 동일한 레이아웃
- 맞춤법 검사: 기본적으로 해제 되어 있으므로 사용 하도록 설정할 수 있습니다.
- 자동 수정: 기본적으로 해제 되어 있으므로 사용 하도록 설정할 수 있습니다.
- 자동 대문자화: 기본적으로 해제 되어 있습니다 (각 단어의 첫 글자는 대문자로 표시 됨).
- 텍스트 예측: 기본적으로 해제 되어 있을 수 있습니다.