---
author: Jwmsft
Description: A password box is a text input box that conceals the characters typed into it for the purpose of privacy.
title: 암호 상자에 대한 지침
ms.assetid: 332B04D6-4FFE-42A4-8B3D-ABE8266C7C18
dev.assetid: 4BFDECC6-9BC5-4FF5-8C63-BB36F6DDF2EF
label: Password box
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c9e283c8f5c116e300b98d7e4078d91e4dac207e
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6981192"
---
# <a name="password-box"></a>암호 상자

 

암호 상자는 개인 정보 보호를 위해 입력된 문자를 숨기는 텍스트 입력 상자입니다. 암호 상자는 입력란처럼 보이지만 입력된 텍스트 대신 자리 표시자 문자를 렌더링합니다. 자리 표시자 문자는 구성할 수 있습니다.

> **중요 API**: [PasswordBox 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx), [Password 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.password.aspx), [PasswordChar 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchar.aspx), [PasswordRevealMode 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordrevealmode.aspx), [PasswordChanged 이벤트](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchanged.aspx)

기본적으로 암호 상자는 사용자가 표시 단추를 누른 채로 자신의 암호를 볼 수 있는 방법을 제공합니다. 표시 단추를 사용하지 않도록 설정하거나 확인란과 같은 암호를 표시하는 대체 메커니즘을 제공할 수 있습니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

**PasswordBox** 컨트롤을 사용하여 암호 또는 주민 등록 번호 등의 기타 개인 데이터를 수집합니다.

올바른 텍스트 컨트롤을 선택하는 방법에 대한 자세한 내용은 [텍스트 컨트롤](text-controls.md) 문서를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/PasswordBox">앱을 열고 작동 중인 PasswordBox를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

암호 상자는 다음을 비롯하여 여러 상태가 있습니다.

정지된 암호 상자는 사용자가 해당 용도를 알 수 있도록 힌트 텍스트를 표시할 수 있습니다.

![힌트 텍스트가 포함된 정지 상태의 암호 상자](images/passwordbox-rest-hinttext.png)

사용자가 암호 상자에 입력할 때 기본 동작은 입력되는 텍스트를 숨기는 글머리 기호를 표시하는 것입니다.

![텍스트 입력에 포커스를 둔 상태의 암호 상자](images/passwordbox-focus-typing.png)

오른쪽에서 "나타내기" 단추를 누르면 입력 중인 암호 텍스트를 미리 볼 수 있습니다.

![텍스트가 표시된 암호 상자](images/passwordbox-text-reveal.png)

## <a name="create-a-password-box"></a>암호 상자 만들기

[Password](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.password.aspx) 속성을 사용하여 PasswordBox의 콘텐츠를 가져오거나 설정합니다. 사용자가 암호를 입력하는 동안 [PasswordChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchanged.aspx) 이벤트의 처리기에서 이 작업을 수행하여 유효성을 검사합니다. 또는 [Click](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 단추와 같은 기타 이벤트를 사용하여 사용자가 텍스트 입력을 완료한 후에 유효성 검사를 수행합니다.

다음은 PasswordBox의 기본 보기를 보여주는 암호 상자 컨트롤에 대한 XAML입니다. 사용자가 암호를 입력하면 암호가 리터럴 값 "Password"인지 확인합니다. 맞으면 사용자에게 메시지를 표시합니다.

```xaml
<StackPanel>  
  <PasswordBox x:Name="passwordBox" Width="200" MaxLength="16"
             PasswordChanged="passwordBox_PasswordChanged"/>

  <TextBlock x:Name="statusText" Margin="10" HorizontalAlignment="Center" />
</StackPanel>   
```

```csharp
private void passwordBox_PasswordChanged(object sender, RoutedEventArgs e)
{
    if (passwordBox.Password == "Password")
    {
        statusText.Text = "'Password' is not allowed as a password.";
    }
    else
    {
        statusText.Text = string.Empty;
    }
}
```
다음은 이 코드가 실행되고 사용자가 "Password"를 입력할 때의 결과입니다.

![유효성 검사 메시지가 있는 암호 상자](images/passwordbox-revealed-validation.png)

### <a name="password-character"></a>암호 문자

[PasswordChar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchar.aspx) 속성을 설정하여 암호를 가리는 데 사용되는 문자를 변경할 수 있습니다. 여기서는 기본 글머리 기호가 별표로 바뀝니다.

```xaml
<PasswordBox x:Name="passwordBox" Width="200" PasswordChar="*"/>
```

결과는 다음과 같습니다.

![사용자 지정 문자가 있는 암호 상자](images/passwordbox-custom-char.png)

### <a name="headers-and-placeholder-text"></a>헤더 및 개체 틀 텍스트

[Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.header.aspx) 및 [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.placeholdertext.aspx) 속성을 사용하여 PasswordBox에 대한 컨텍스트를 제공할 수 있습니다. 암호를 변경하기 위한 양식과 같이 상자가 여러 개 있을 때 특히 유용합니다.

```xaml
<PasswordBox x:Name="passwordBox" Width="200" Header="Password" PlaceholderText="Enter your password"/>
```

![힌트 텍스트가 포함된 정지 상태의 암호 상자](images/passwordbox-rest-hinttext.png)

### <a name="maximum-length"></a>최대 길이

사용자가 [MaxLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.maxlength.aspx) 속성을 설정하여 입력할 수 있는 문자의 최대 수를 지정합니다. 최소 길이를 지정하는 속성은 없지만 암호 길이를 확인하고 앱 코드에서 다른 유효성 검사를 수행할 수 있습니다.

## <a name="password-reveal-mode"></a>암호 표시 모드

PasswordBox에는 사용자가 암호 텍스트를 표시하기 위해 터치하거나 클릭할 수 있는 기본 제공 단추가 있습니다. 다음은 사용자 작업의 결과입니다. 사용자가 이 단추를 해제하면 암호가 자동으로 다시 숨겨집니다.

![텍스트가 표시된 암호 상자](images/passwordbox-text-reveal.png)

### <a name="peek-mode"></a>미리 보기 모드

기본적으로 암호 표시 단추(또는 "미리 보기" 단추)가 표시됩니다. 사용자는 암호를 보기 위해 단추를 계속 눌러야 하므로 높은 수준의 보안이 유지됩니다.

[PasswordRevealMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordrevealmode.aspx) 속성의 값이 암호 표시 단추가 사용자에게 표시되는지 여부를 결정하는 유일한 요소는 아닙니다. 기타 요소로는 컨트롤이 최소 너비보다 큰지 여부, PasswordBox에 포커스가 있는지 여부, 텍스트 입력 필드에서 하나 이상의 문자가 포함되어 있는지 여부 등이 있습니다. 암호 표시 단추는 PasswordBox에 처음 포커스가 배치되고 문자를 입력하는 경우에만 표시됩니다. PasswordBox가 포커스를 잃었다가 다시 얻으면 암호를 지우고 문자 입력을 다시 시작해야만 표시 단추가 다시 표시됩니다.

> **주의**&nbsp;&nbsp;Windows 10 이전에는 기본적으로 암호 표시 단추가 표시되지 않았습니다. 앱 보안 정책에 따라 암호를 항상 가려야 하는 경우 PasswordRevealMode를 Hidden으로 설정하세요.

### <a name="hidden-and-visible-modes"></a>숨김 및 표시 모드

다른 [PasswordRevealMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordrevealmode.aspx) 열거형 값 **Hidden** 및 **Visible**는 암호 표시 단추를 숨기고 암호가 가려지는지 여부를 프로그래밍 방식으로 관리할 수 있도록 합니다.

암호를 항상 가리려면 PasswordRevealMode를 Hidden으로 설정합니다. 암호를 항상 가릴 필요가 없으면 사용자 지정 UI를 제공하여 사용자가 Hidden 및 Visible 간에 PasswordRevealMode를 전환할 수 있도록 합니다.

이전 버전의 Windows Phone에서 PasswordBox는 확인란을 사용하여 암호를 가릴지 여부를 전환합니다. 다음 예제에 표시된 대로 앱에 대한 유사한 UI를 만들 수 있습니다. [ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx)과 같은 기타 컨트롤을 사용하여 사용자가 모드 간을 전환하도록 할 수도 있습니다.

이 예제에서는 [CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx)를 사용하여 사용자가 PasswordBox의 표시 모드를 전환하는 방법을 보여 줍니다.

```xaml
<StackPanel Width="200">
    <PasswordBox Name="passwordBox1"
                 PasswordRevealMode="Hidden"/>
    <CheckBox Name="revealModeCheckBox" Content="Show password"
              IsChecked="False"
              Checked="CheckBox_Changed" Unchecked="CheckBox_Changed"/>
</StackPanel>
```

```csharp
private void CheckBox_Changed(object sender, RoutedEventArgs e)
{
    if (revealModeCheckBox.IsChecked == true)
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Visible;
    }
    else
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Hidden;
    }
}
```

이 PasswordBox는 다음과 같이 표시됩니다.

![사용자 지정 표시 단추가 있는 암호 상자](images/passwordbox-custom-reveal.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>텍스트 컨트롤에 맞는 키보드를 선택합니다.

사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 터치 키보드나 SIP(Soft Input Panel)를 사용한 데이터 입력을 도울 수 있습니다. PasswordBox는 **Password** 및 **NumericPin** 입력 범위 값만 지원합니다. 다른 값이 무시됩니다.

입력 범위를 사용하는 방법에 대한 자세한 내용은 [입력 범위를 사용하여 터치 키보드를 변경](https://msdn.microsoft.com/library/windows/apps/mt280229)을 참조하세요.

## <a name="recommendations"></a>권장 사항

-   암호 상자의 용도가 명확하지 않은 경우 레이블 또는 개체 틀 텍스트를 사용하세요. 레이블은 텍스트 입력란에 값이 있는지 여부에 상관없이 표시됩니다. 개체 틀 텍스트는 텍스트 입력란 내에 표시되었다가 값을 입력하면 사라집니다.
-   입력할 수 있는 값의 범위에 적절한 너비로 암호 상자를 설정하세요. 단어 길이는 언어에 따라 달라지므로 앱을 세계화하려는 경우 지역화를 고려해야 합니다.
-   다른 컨트롤을 암호 입력란의 바로 옆에 배치하지 마세요. 암호 상자에는 사용자가 자신이 입력한 암호를 확인할 수 있도록 암호 표시 단추가 있습니다. 그 바로 옆에 또 다른 컨트롤을 두면 사용자가 다른 컨트롤을 조작하려고 할 때 실수로 자신의 암호를 노출할 수 있습니다. 이런 일을 방지하려면 암호 입력란과 다른 컨트롤 사이에 간격을 두거나 다른 컨트롤을 다음 행에 배치합니다.
-   계정을 만들 때 새 암호와 새 암호 확인에 하나씩 두 개의 암호 상자를 제공하는 것이 좋습니다.
-   로그온하고 있을 경우 단일 암호만 표시합니다.
-   암호 상자를 사용하여 PIN을 입력하는 경우 확인 단추를 누를 필요 없이 마지막 문자가 입력되자마자 반응을 보이는 것이 좋습니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

[텍스트 컨트롤](text-controls.md)

- [맞춤법 검사에 대한 지침](text-controls.md)
- [검색 추가](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [텍스트 입력에 대한 지침](text-controls.md)
- [TextBox 클래스](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 클래스](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 속성](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
