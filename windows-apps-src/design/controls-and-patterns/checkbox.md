---
Description: 작업 항목을 선택하거나 선택 취소하는 데 사용합니다. 단일 목록 항목이나 여러 목록 항목에 대해 사용할 수 있습니다.
title: 확인란
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7add5ca356f5d1c41ddeb1fa19ea54c69ef9e583
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750569"
---
# <a name="check-boxes"></a>확인란

확인란은 작업 항목을 선택하거나 선택 취소하는 데 사용됩니다. 사용자가 선택할 수 있는 단일 항목이나 여러 항목의 목록에도 사용할 수 있습니다. 컨트롤에는 3개의 선택 상태, 즉 선택되지 않음, 선택됨 및 확정되지 않음이 있습니다. 하위 항목 컬렉션에 선택되지 않음과 선택됨 상태가 둘 다 있는 경우에 확정되지 않은 상태를 사용합니다.

![확인란 상태의 예](images/templates-checkbox-states-default.png)

**Windows UI 라이브러리 가져오기**

:::row:::
   :::column:::
      ![WinUI 로고](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI 라이브러리 2.2 이상에는 둥근 모서리를 사용하는 이 컨트롤의 새 템플릿이 포함되어 있습니다. 자세한 내용은 [모서리 반경](../style/rounded-corner.md)을 참조하세요. WinUI는 Windows 앱에 대한 새 컨트롤 및 UI 기능이 포함된 NuGet 패키지입니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요. 
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **플랫폼 API:** [CheckBox 클래스](/uwp/api/Windows.UI.Xaml.Controls.CheckBox), [Checked 이벤트](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked), [IsChecked 속성](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

이진 예/아니요 선택 항목에 **단일 확인란**을 사용합니다. 예를 들어 "기억" 로그인 시나리오 또는 서비스 계약 조건의 경우입니다.

![개별 선택에 사용되는 단일 확인란](images/checkbox1.png)

이진 선택의 경우 **확인란**과 [토글 스위치](toggles.md)의 주요 차이점은, 확인란은 상태에 대한 것이고 토글 스위치는 동작에 대한 것이라는 점입니다. 확인란 상호 작용(예: 양식 전송)의 커밋은 연기할 수 있지만 전환 스위치 상호 작용은 즉시 커밋해야 합니다. 또한 다중 선택에는 확인란만 사용할 수 있습니다.

사용자가 함께 사용할 수 있는 선택 항목 그룹에서 항목을 하나 이상 선택하는 다중 선택 시나리오에 **여러 확인란**을 사용합니다.

사용자가 옵션 조합을 선택할 수 있는 경우 확인란 그룹을 만듭니다.

![확인란으로 여러 개의 옵션 선택](images/checkbox2.png)

옵션을 그룹화하는 경우 확정되지 않은 상태 확인란을 사용하여 전체 그룹을 나타낼 수 있습니다. 사용자가 그룹의 하위 항목 전부가 아닌 일부 항목을 선택하는 경우 확인란의 확정되지 않은 상태를 사용합니다.

![혼합 선택을 표시하는 데 사용되는 확인란](images/checkbox3.png)

**확인란** 및 **라디오 단추** 컨트롤을 통해 사용자는 옵션 목록에서 선택할 수 있습니다. 확인란을 사용하여 사용자는 옵션 조합을 선택할 수 있습니다. 반면, 라디오 단추를 사용하면 함께 사용할 수 없는 옵션에서 한 가지 옵션을 선택할 수 있습니다. 둘 이상의 옵션이 있지만 하나만 선택할 수 있는 경우 대신 라디오 단추를 사용합니다.

## <a name="examples"></a>예

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/CheckBox">앱을 열고 작동 중인 CheckBox를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-checkbox"></a>확인란 만들기

확인란에 레이블을 할당하려면 [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) 속성을 사용합니다. 이 레이블은 확인란 옆에 표시됩니다.

이 XAML은 양식이 제출되기 전 서비스 약관에 동의하는 데 사용되는 단일 확인란을 만듭니다. 

```xaml
<CheckBox x:Name="termsOfServiceCheckBox" 
          Content="I agree to the terms of service."/>
```

다음은 코드에서 만들어진 동일한 확인란입니다.

```csharp
CheckBox checkBox1 = new CheckBox();
checkBox1.Content = "I agree to the terms of service.";
```

### <a name="bind-to-ischecked"></a>IsChecked에 바인딩

[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked) 속성을 사용하여 확인란을 선택하거나 선택 취소 여부를 결정할 수 있습니다. IsChecked 속성의 값을 다른 이진 값에 바인딩할 수 있습니다.
그러나 IsChecked가 [nullable](/dotnet/api/system.nullable-1) 부울 값이므로 캐스트 또는 값 변환기를 사용하여 부울 속성으로 바인딩해야 합니다. 이는 사용 중인 실제 바인딩 유형에 따라 달라지며 가능한 각 유형에 대한 예제는 아래에서 확인할 수 있습니다. 

이 예제에서는 서비스 약관에 동의하는 확인란의 **IsChecked** 속성을 제출 단추의 [IsEnabled](/uwp/api/windows.ui.xaml.controls.control.isenabled) 속성으로 바인딩합니다. 제출 단추는 서비스 약관에 동의하는 경우에만 사용할 수 있습니다.

#### <a name="using-xbind"></a>x:Bind 사용

> 참고&nbsp;&nbsp;여기서는 관련 코드만 표시합니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 개요](../../data-binding/data-binding-quickstart.md)를 참조하세요. 특정 {x:Bind} 정보(예: 캐스팅)는 [여기](../../xaml-platform/x-bind-markup-extension.md)에서 자세히 설명합니다.

```xaml
<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind (x:Boolean)termsOfServiceCheckBox.IsChecked, Mode=OneWay}"/>
</StackPanel>
```

확인란이 **확정되지 않은** 상태일 수도 있는 경우 바인딩의 [FallbackValue](/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) 속성을 사용하여 이 상태를 나타내는 부울 값을 지정합니다. 이 경우 제출 단추도 사용하도록 설정하고 싶지 않습니다.

```xaml
<Button Content="Submit" 
        IsEnabled="{x:Bind (x:Boolean)termsOfServiceCheckBox.IsChecked, Mode=OneWay, FallbackValue=False}"/>
```

#### <a name="using-xbind-or-binding"></a>x:Bind 또는 Binding 사용

> 참고&nbsp;&nbsp;여기서는 {x:Bind} 사용 관련 코드만 표시합니다. {Binding} 예제에서는 {x:Bind}를 {Binding}으로 바꿉니다. 데이터 바인딩, 값 변환기, {x:Bind} 및 {Binding} 태그 확장 간의 차이점에 대한 자세한 내용은 [데이터 바인딩 개요](../../data-binding/data-binding-quickstart.md)를 참조하세요.

```xaml
...
<Page.Resources>
    <local:NullableBooleanToBooleanConverter x:Key="NullableBooleanToBooleanConverter"/>
</Page.Resources>

...

<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind termsOfServiceCheckBox.IsChecked, 
                        Converter={StaticResource NullableBooleanToBooleanConverter}, Mode=OneWay}"/>
</StackPanel>
```


```csharp
public class NullableBooleanToBooleanConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, string language)
    {
        if (value is bool?)
        {
            return (bool)value;
        }
        return false;
    }

    public object ConvertBack(object value, Type targetType, object parameter, string language)
    {
        if (value is bool)
            return (bool)value;
        return false;
    }
}
```

### <a name="handle-click-and-checked-events"></a>Click 및 Checked 이벤트 처리

확인란 상태가 변경될 때 작업을 수행하려면 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트 또는 [Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) 및 [Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked) 이벤트를 처리할 수 있습니다. 

선택된 상태가 변경될 때마다 **Click** 이벤트가 발생합니다. Click 이벤트를 처리하는 경우 **IsChecked** 속성을 사용하여 확인란의 상태를 결정합니다.

**Checked** 및 **Unchecked** 이벤트는 별개로 발생합니다. 이러한 이벤트를 처리하는 경우 둘 다 확인란의 상태 변경에 대해 응답하도록 처리해야 합니다.

다음 예제에서는 Click 이벤트와 Checked 및 Unchecked 이벤트의 처리를 보여 줍니다.

여러 확인란은 동일한 이벤트 처리기를 공유할 수 있습니다. 이 예제에서는 피자 토핑 선택에 대한 네 개의 확인란을 만듭니다. 네 개의 확인란은 동일한 **Click** 이벤트 처리기를 공유하여 선택한 토핑의 목록을 업데이트합니다.

```XAML
<StackPanel Margin="40">
    <TextBlock Text="Pizza Toppings"/>
    <CheckBox Content="Pepperoni" x:Name="pepperoniCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Beef" x:Name="beefCheckbox" 
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Mushrooms" x:Name="mushroomsCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Onions" x:Name="onionsCheckbox"
              Click="toppingsCheckbox_Click"/>

    <!-- Display the selected toppings. -->
    <TextBlock Text="Toppings selected:"/>
    <TextBlock x:Name="toppingsList"/>
</StackPanel>
```

다음은 Click 이벤트에 대한 이벤트 처리기입니다. 확인란을 클릭할 때마다 어떤 확인란이 선택되었고 선택한 토핑의 업데이트 목록을 확인하기 위해 확인란을 검사합니다.

```csharp
private void toppingsCheckbox_Click(object sender, RoutedEventArgs e)
{
    string selectedToppingsText = string.Empty;
    CheckBox[] checkboxes = new CheckBox[] { pepperoniCheckbox, beefCheckbox,
                                             mushroomsCheckbox, onionsCheckbox };
    foreach (CheckBox c in checkboxes)
    {
        if (c.IsChecked == true)
        {
            if (selectedToppingsText.Length > 1)
            {
                selectedToppingsText += ", ";
            }
            selectedToppingsText += c.Content;
        }
    }
    toppingsList.Text = selectedToppingsText;
}
```

### <a name="use-the-indeterminate-state"></a>확정되지 않은 상태 사용

CheckBox 컨트롤은 [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton)에서 상속되며 세 가지 상태를 가질 수 있습니다. 

State | 속성 | Value
------|----------|------
선택됨 | IsChecked | **true** 
선택되지 않음 | IsChecked | **false** 
확정되지 않음 | IsChecked | **null** 

확인란이 확정되지 않은 상태를 보고하도록 하려면 [IsThreeState](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.isthreestate) 속성을 **true**로 설정해야 합니다. 

옵션을 그룹화하는 경우 확정되지 않은 상태 확인란을 사용하여 전체 그룹을 나타낼 수 있습니다. 사용자가 그룹의 하위 항목 전부가 아닌 일부 항목을 선택하는 경우 확인란의 확정되지 않은 상태를 사용합니다.

다음 예제에서는 "모두 선택" 확인란의 IsThreeState 속성이 **true**로 설정되어 있습니다. "모두 선택" 확인란은 모든 자식 요소가 선택되는 경우 선택되고 모든 자식 요소가 선택되지 않고 확정되지 않은 상태이면 선택되지 않습니다.

```xaml
<StackPanel>
    <CheckBox x:Name="OptionsAllCheckBox" Content="Select all" IsThreeState="True" 
              Checked="SelectAll_Checked" Unchecked="SelectAll_Unchecked" 
              Indeterminate="SelectAll_Indeterminate"/>
    <CheckBox x:Name="Option1CheckBox" Content="Option 1" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
    <CheckBox x:Name="Option2CheckBox" Content="Option 2" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" IsChecked="True"/>
    <CheckBox x:Name="Option3CheckBox" Content="Option 3" Margin="24,0,0,0"
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
</StackPanel>
```

```csharp
private void Option_Checked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void Option_Unchecked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void SelectAll_Checked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = true;
}

private void SelectAll_Unchecked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = false;
}

private void SelectAll_Indeterminate(object sender, RoutedEventArgs e)
{
    // If the SelectAll box is checked (all options are selected), 
    // clicking the box will change it to its indeterminate state.
    // Instead, we want to uncheck all the boxes,
    // so we do this programatically. The indeterminate state should
    // only be set programatically, not by the user.

    if (Option1CheckBox.IsChecked == true &&
        Option2CheckBox.IsChecked == true &&
        Option3CheckBox.IsChecked == true)
    {
        // This will cause SelectAll_Unchecked to be executed, so
        // we don't need to uncheck the other boxes here.
        OptionsAllCheckBox.IsChecked = false;
    }
}

private void SetCheckedState()
{
    // Controls are null the first time this is called, so we just 
    // need to perform a null check on any one of the controls.
    if (Option1CheckBox != null)
    {
        if (Option1CheckBox.IsChecked == true &&
            Option2CheckBox.IsChecked == true &&
            Option3CheckBox.IsChecked == true)
        {
            OptionsAllCheckBox.IsChecked = true;
        }
        else if (Option1CheckBox.IsChecked == false &&
            Option2CheckBox.IsChecked == false &&
            Option3CheckBox.IsChecked == false)
        {
            OptionsAllCheckBox.IsChecked = false;
        }
        else
        {
            // Set third state (indeterminate) by setting IsChecked to null.
            OptionsAllCheckBox.IsChecked = null;
        }
    }
}
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

-   확인란의 목적과 현재 상태가 명확한지 확인합니다.
-   확인란의 텍스트 콘텐츠를 두 줄 이내로 제한합니다.
-   확인 표시가 있으면 true를 반환하고 확인 표시가 없으면 false를 반환하는 문으로 확인란 레이블을 작성합니다.
-   브랜드 지침에 다른 글꼴을 사용하도록 규정되지 않은 경우 기본 글꼴을 사용합니다.
-   텍스트 콘텐츠가 동적인 경우 컨트롤 크기를 조정하는 방법과 주위의 시각적 모양을 고려합니다.
-   선택할 수 있는 상호 배타적 옵션이 둘 이상인 경우 [라디오 단추](radio-button.md)를 사용해 봅니다.
-   두 확인란 그룹을 나란히 배치하지 마세요. 그룹 레이블을 사용하여 그룹을 분리합니다.
-   확인란을 설정/해제 컨트롤로 사용하거나 명령을 수행하는 데 사용하면 안 됩니다. 대신 토글 스위치를 사용합니다.
-   확인란을 대화 상자와 같은 다른 컨트롤을 표시하는 데 사용하면 안 됩니다.
-   확정되지 않은 상태를 사용하여 옵션이 하위 항목 전부가 아닌 일부에 대해서만 설정되었음을 나타냅니다.
-   불확실한 상태를 사용할 때는 하위 확인란을 사용하여 선택된 옵션과 선택되지 않은 옵션을 나타냅니다. 하위 항목을 표시할 수 있도록 UI를 디자인합니다.
-   확정되지 않은 상태를 사용하여 세 번째 상태를 나타내지 마세요. 확정되지 않은 상태는 옵션이 하위 항목 전부가 아닌 일부에 대해서만 설정되었음을 나타내는 데 사용됩니다. 따라서 사용자가 확정되지 않은 상태를 직접 설정할 수 없도록 합니다. 예를 들어 이 확인란을 사용하여 중간 양념을 나타내기 위해 불확실한 상태를 사용하면 안 됩니다.

    ![확정되지 않은 상태 확인란](images/checkbox4_spicy.png)

    대신 세 가지 옵션 안 매운 맛, 매운 맛, 아주 매운 맛이 있는 라디오 단추 그룹을 사용하세요.

    ![세 가지 옵션 안 매운 맛, 매운 맛, 아주 매운 맛이 있는 라디오 단추 그룹](images/spicyoptions.png)

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련된 문서

- [CheckBox 클래스](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 
- [라디오 단추](radio-button.md)
- [토글 스위치](toggles.md)
