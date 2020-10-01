---
Description: 숫자 상자는 숫자를 표시하고 편집하는 데 사용할 수 있는 컨트롤입니다.
title: 숫자 상자
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9e0cd1979ca1929adc35537dfd3efccc97466391
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217976"
---
# <a name="number-box"></a>숫자 상자

숫자를 표시하고 편집하는 데 사용할 수 있는 컨트롤을 나타냅니다. 이는 곱하기, 나누기, 더하기 및 빼기와 같은 기본 수식의 유효성 검사, 증가 단계별 실행 및 컴퓨팅 인라인 계산을 지원합니다.

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | **NumberBox** 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리가 필요합니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리 개요](/uwp/toolkits/winui/)를 참조하세요. |

**Windows UI 라이브러리 API:** [NumberBox 클래스](/uwp/api/microsoft.ui.xaml.controls.NumberBox)

> [!TIP]
> 이 문서 전체에서 XAML의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이를 [Page](/uwp/api/windows.ui.xaml.controls.page) 요소(`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`)에 추가했습니다.
>
>코드 숨김에서는 C#의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이 **using** 문(`using muxc = Microsoft.UI.Xaml.Controls;`)을 파일 맨 위에 추가했습니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

숫자 상자 컨트롤을 사용하여 수학 입력을 캡처하고 표시할 수 있습니다. 숫자 이외의 값을 허용하는 편집 가능한 입력란이 필요한 경우 [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 컨트롤을 사용합니다. 암호나 다른 중요한 입력을 허용하는 편집 가능한 텍스트 상자가 필요한 경우 [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)를 참조하세요. 검색어를 입력하는 입력란이 필요한 경우 [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)를 참조하세요. 서식 있는 텍스트를 입력하거나 편집해야 하는 경우 [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox)를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치되어 있는 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/TextBox">앱을 열고 작동 중인 NumberBox를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>간단한 NumberBox 만들기

다음은 기본 NumberBox에 대한 기본 모양을 보여 주는 XAML입니다. 사용자에게 표시되는 데이터가 앱에 저장된 데이터와 동기화 상태를 유지하도록 하려면 [x:Bind](../../xaml-platform/x-bind-markup-extension.md#property-path)를 사용합니다.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![0을 표시하는 포커스 내 입력 필드입니다.](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>NumberBox 레이블 지정

NumberBox의 목적이 명확하지 않은 경우 `Header` 또는 `PlaceholderText`를 사용합니다. `Header`는 NumberBox에 값이 있는지 여부에 상관없이 표시됩니다.

```xaml
<muxc:NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![NumberBox 위의 “Enter expression:”을 읽는 헤더입니다.](images/numberbox-header.png)

`PlaceholderText`는 NumberBox 안에 표시되며 `Value`를 NaN으로 설정하거나 사용자가 입력을 지울 때만 표시됩니다.

```xaml
<muxc:NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![“A + B”를 읽는 자리 표시자 텍스트를 포함하는 NumberBox입니다.](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>계산 지원 사용

`AcceptsExpression` 속성을 true로 설정하면 NumberBox에서 표준 연산 순서를 사용하여 곱하기, 나누기, 더하기 및 빼기와 같은 기본 인라인 식을 평가할 수 있습니다. 포커스가 손실되거나 사용자가 “Enter” 키를 누를 때 계산이 트리거됩니다. 식이 계산되면 식의 원래 형태는 유지되지 않습니다.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>증가 및 감소 단계별 실행

`SmallChange` 속성을 사용하여 NumberBox에 포커스가 있고 사용자가 다음을 수행할 때 NumberBox 내의 값을 얼마나 변경할 수 있는지를 구성합니다.

- 스크롤
- 위쪽 화살표 키 누르기
- 아래쪽 화살표 키 누르기

`LargeChange` 속성을 사용하여 NumberBox가 포커스에 있고 사용자가 PageUp 또는 PageDown 키를 누를 때 NumberBox 내의 값을 얼마나 변경할 수 있는지를 구성합니다.

클릭하여 NumberBox의 값을 `SmallChange` 속성으로 지정한 양만큼 증가 또는 감소시킬 수 있는 단추를 활성화하려면 `SpinButtonPlacementMode` 속성을 사용합니다. 다른 단계에서 최댓값 또는 최솟값이 초과되는 경우에는 이러한 단추를 사용할 수 없습니다.

단추를 컨트롤 옆에 표시하려면 `SpinButtonPlacementMode`를 `Inline`으로 설정합니다.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![옆에 아래쪽 화살표 단추와 위쪽 화살표 단추가 있는 NumberBox입니다.](images/numberbox-spinbutton-inline.png)

NumberBox에 포커스가 있는 경우에만 단추를 플라이아웃으로 표시하려면 `SpinButtonPlacementMode`를 `Compact`로 설정합니다.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![위쪽을 가리키는 화살표와 아래쪽을 가리키는 화살표를 표시하는 작은 아이콘이 있는 NumberBox입니다.](images/numberbox-spinbutton-compact-non-visible.png)

![상승된 계층의 옆에 아래쪽 화살표 단추와 위쪽 화살표 단추가 떠 있는 NumberBox입니다.](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>입력 유효성 검사 사용

`ValidationMode`를 `InvalidInputOverwritten`으로 설정하면 NumberBox에서 포커스를 잃거나 “Enter” 키를 누를 때 계산이 트리거되는 경우 숫자도 아니고 정식으로 공식도 아닌 잘못된 입력을 마지막으로 유효한 값으로 덮어쓸 수 있습니다.

```xaml
<muxc:NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

`ValidationMode`를 `Disabled`로 설정하면 사용자 지정 입력 유효성 검사를 구성할 수 있습니다.

소수점 및 쉼표와 관련하여 사용자가 사용하는 서식은 NumberBox에 대해 구성된 서식으로 대체됩니다. 입력 유효성 검사 오류가 트리거되지 않습니다.

### <a name="formatting-input"></a>입력 서식 지정

[숫자 서식 지정](/uwp/api/windows.globalization.numberformatting)은 서식 지정 클래스의 인스턴스를 구성하고 `NumberFormatter` 속성에 할당하여 Numberbox의 값의 형식을 지정하는 데 사용할 수 있습니다. 사용할 수 있는 숫자 서식 지정 클래스로는 십진수, 통화, 백분율 및 유의미한 수치가 있습니다. 반올림도 숫자 서식 지정 속성에 의해 정의된다는 점에 유의하세요.

다음 예제는 DecimalFormatter를 사용하여 NumberBox의 값을 정수 한 자리, 분수 두 자리 및 최대 0.25까지 반올림하도록 서식을 지정합니다.

```xaml
<muxc:NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

```csharp
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![0\.00 값을 표시하는 NumberBox입니다.](images/numberbox-formatted.png)

소수점 및 쉼표와 관련하여 사용자가 사용하는 서식은 NumberBox에 대해 구성된 서식으로 대체됩니다. 입력 유효성 검사 오류가 트리거되지 않습니다.

## <a name="remarks"></a>설명

### <a name="input-scope"></a>입력 범위

`Number`는 [입력 범위](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)에 사용됩니다. 이 입력 범위는 0-9자리로 작업하기 위한 것입니다. 이를 덮어쓸 수 있지만 대체 InputScope 형식은 명시적으로 지원되지 않습니다.

### <a name="not-a-number"></a>숫자가 아님

NumberBox가 입력에서 지워지면 `Value`가 `NaN`으로 설정되어 숫자가 아닌 값이 있음을 나타냅니다.

### <a name="expression-evaluation"></a>식 계산

NumberBox는 중위 표기법을 사용하여 식을 계산합니다. 우선 순위에 따라 허용되는 연산자는 다음과 같습니다.

* ^
* */
* +-

괄호는 선행 규칙을 재정의하는 데 사용할 수 있습니다.

## <a name="recommendations"></a>권장 사항

* `Text` 및 `Value`를 사용하면 값을 형식 간에 변환할 필요 없이 NumberBox 값을 String 또는 Double로 쉽게 캡처할 수 있습니다. NumberBox 값을 프로그래밍 방식으로 변경하는 경우 `Value` 속성을 통해 이 작업을 수행하는 것이 좋습니다. `Value`는 초기 설정의 `Text`를 덮어씁니다. 초기 설정 후 변경 내용이 다른 변경 사항으로 전파되지만, `Value`를 통해 프로그래밍 방식으로 일관되게 변경되면 NumberBox가 `Text`를 통해 숫자가 아닌 문자를 수신한다는 개념적 오해를 피할 수 있습니다.
* `Header` 또는 `PlaceholderText`를 사용하여 NumberBox에서는 입력으로 숫자 문자만 허용한다는 것을 사용자에게 알립니다. “one”과 같은 숫자의 철자 표현은 허용되는 값으로 확인되지 않습니다.
