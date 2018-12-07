---
title: 조건부 XAML
description: 이전 버전과 호환성을 유지하면서 XAML 태그에 새로운 API 사용
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c75a6c487fe4a7f7cb56deff869b36309a4b9c7
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8786827"
---
# <a name="conditional-xaml"></a>조건부 XAML

*조건부 XAML*은 XAML 태그에 [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) 메서드를 사용하는 방법을 제공합니다. 이를 통해 코드 숨김을 사용하지 않고도 API의 존재 여부를 기반으로 태그에서 속성을 설정하고 개체를 인스턴스화할 수 있습니다. 조건부 XML은 요소나 특성을 런타임에 사용할 수 있는지 파악하기 위해 선택적으로 구문 분석합니다. 조건부 명령문은 런타임에 평가되며, **true**로 평가된 경우 조건부 XAML 태그로 자격이 부여된 요소가 구문 분석되고, 그렇지 않으면 무시됩니다.

조건부 XAML은 크리에이터스 업데이트(버전 1703 빌드 15063)부터 사용할 수 있습니다. 조건부 XAML을 사용하려면 Visual Studio 프로젝트의 최소 버전이 15063(크리에이터스 업데이트) 이상으로 설정되어야 하며, 대상 버전은 최소 버전 이상으로 설정되어야 합니다. Visual Studio 프로젝트 구성에 대한 자세한 내용은 [버전 적응 앱](version-adaptive-apps.md)을 참조하세요.

> [!NOTE]
> 빌드 15063 이하의 최소 버전으로 버전 적응 앱을 만들려면 XAML이 아닌 [버전 적응 코드](version-adaptive-code.md)를 사용해야 합니다.

ApiInformation 및 API 계약에 대한 중요한 배경 정보는 [버전 적응 앱](version-adaptive-apps.md)을 참조하세요.

## <a name="conditional-namespaces"></a>조건부 네임스페이스

XAML에서 조건부 메서드를 사용하려면 먼저 페이지 맨 위에 조건부 [XAML 네임스페이스](../xaml-platform/xaml-namespaces-and-namespace-mapping.md)를 선언해야 합니다. 조건부 네임스페이스의 가상 코드 예는 다음과 같습니다.

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

조건부 네임스페이스는 '?' 구분 기호를 사용하여 두 부분으로 나눌 수 있습니다. 

- 구분 기호 앞의 내용은 참조되는 API가 포함된 네임스페이스 또는 스키마를 나타냅니다. 
- '?' 구분 기호 뒤의 내용은 조건부 네임스페이스가 **true** 또는 **false**로 평가될지 결정하는 조건부 메서드를 나타냅니다.

대부분의 경우 스키마가 기본 XAML 네임스페이스가 됩니다.

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

조건부 XAML은 다음 조건부 메서드를 지원합니다.

메서드 | 반대
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

이 문서에서 나중에 나올 섹션에서 이런 메서드에 대해 더 자세히 설명하겠습니다.

> [!NOTE]
> IsApiContractPresent 및 IsApiContractNotPresent를 사용하는 것이 좋습니다. 다른 조건은 Visual Studio 디자인 환경에서 완전하게 지원되지 않습니다.

## <a name="create-a-namespace-and-set-a-property"></a>네임스페이스를 만들고 속성을 설정

이 예에서는 앱이 Fall Creators Update 이상에서 실행될 경우 텍스트 블록의 콘텐츠로 "Hello, Conditional XAML"을 표시하며, 기본값은 이전 버전에 실행이 되는 경우에는 콘텐츠를 표시하지 않도록 지정되어 있습니다.

먼저 'contract5Present'라는 접두사를 가진 사용자 지정 네임스페이스를 정의하고, 기본 XAML 네임스페이스(http://schemas.microsoft.com/winfx/2006/xaml/presentation) 를 [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.Text) 속성이 포함된 스키마로 사용합니다. 이 조건부 네임스페이스를 만들려면 스키마 뒤에 '?' 구분 기호를 추가합니다.

그런 다음 Fall Creators Update 이상을 실행하는 장치에서 **true**를 반환하는 조건부를 정의합니다. ApiInformation 메서드 **IsApiContractPresent**를 사용하여 5번째 버전의 UniversalApiContract를 확인할 수 있습니다. UniversalApiContract 버전 5는 Fall Creators Update(SDK 16299)와 함께 출시되었습니다.

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

네임스페이스가 정의된 후, 네임스페이스 접두사를 TextBox의 Text 속성 앞에 추가하여 런타임에 조건부로 설정되는 속성이 되도록 합니다.

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

완성된 XAML은 다음과 같습니다.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

Fall Creators Update에서 이 예를 실행하면 “Hello, Conditional XAML”이라는 텍스트가 표시되며, 크리에이터스 업데이트에서 실행하면 텍스트가 표시되지 않습니다.

조건부 XAML을 사용하여 코드에서 하던 API 검사를 태그에서 대신할 수 있습니다. 이 검사와 동일한 기능을 하는 코드는 다음과 같습니다.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

IsApiContractPresent 메서드는 *contractName* 매개 변수에 대한 문자열을 받지만, XAML 네임스페이스 선언에는 따옴표(" ")를 사용할 수 없음을 유의하십시오.

## <a name="use-ifelse-conditions"></a>if/else 조건 사용

이전 예에서 Text 속성은 앱이 Fall Creators Update에서 실행되는 경우에만 설정됩니다. 하지만 Creators Update에서 실행될 때 다른 텍스트를 표시하려면 어떻게 할까요? 다음과 같이 조건부 한정자 없이 Text 속성을 설정할 수 있습니다.

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

이는 크리에이터스 업데이트에서 실행될 때 작동하지만, Fall Creators Update에서 실행될 때는 Text 속성이 두 번 이상 설정되어 있다는 오류가 발생합니다.

앱이 Windows 10의 다른 버전에서 실행될 때 다른 텍스트가 표시되도록 설정하려면, 다른 조건이 필요합니다. 조건부 XAML은 다음과 같이 if/else 조건을 만들 수 있는 지원되는 각 ApiInformation 메서드의 반대 메서드를 제공합니다.

현재 장치가 지정된 계약 및 버전 번호를 포함할 경우 IsApiContractPresent 메서드는 **true**를 반환합니다. 예를 들어, 앱이 크리에이터스 업데이트에서 실행된다고 가정하면, 유니버설 API 계약의 4번째 버전을 가집니다.

IsApiContractPresent에 대한 다양한 호출의 결과는 다음과 같습니다.

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true.

IsApiContractNotPresent는 IsApiContractPresent의 반대를 반환합니다. IsApiContractNotPresent에 대한 호출의 결과는 다음과 같습니다.

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

반대 조건을 사용하려면 **IsApiContractNotPresent** 조건을 사용하는 두 번째 조건부 XAML 네임스페이스를 만들어야 합니다. 여기를 보면 접두사 'contract5NotPresent'가 있습니다.

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

두 네임스페이스가 정의되면, 다음과 같이 런타임에 한 속성만 사용된다는 전제 하에 Text 속성을 두 번 설정할 수 있습니다.

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

단추의 배경을 설정하는 다른 예는 다음과 같습니다. [아크릴 재질](../design/style/acrylic.md) 기능은 Fall Creators Update에서 사용할 수 있으므로, 앱이 Fall Creators Update에서 실행되는 경우 배경으로 아크릴을 사용할 수 있습니다. 이전 버전에서는 사용할 수 없으므로 이 경우, 배경을 빨간색으로 설정합니다.

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>컨트롤 생성 및 속성 바인딩

지금까지 조건부 XAML를 사용하여 속성을 설정하는 방법을 살펴 보았습니다. 하지만 런타임에 API 계약을 따라 조건부로 컨트롤을 인스턴스할 수도 있습니다.

다음 코드에서는 컨트롤을 사용할 수 있는 Fall Creators Update에서 앱이 실행될 때 [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker)가 인스턴스화됩니다. Fall Creators Update 이전 버전에서는 ColorPicker를 사용할 수 없으므로, 앱이 이전 버전에서 실행될 때는 [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox)를 사용하여 사용자에게 간단한 색상 선택권을 제공할 수 있습니다.

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

다른 형식의 [XAML 속성 구문](../xaml-platform/xaml-syntax-guide.md)을 통해 조건부 한정자를 사용할 수 있습니다. 다음 코드에서는 사각형의 Fill 속성이 Fall Creators Update에 대해서는 속성 요소 구문을 사용하여 설정되며, 이전 버전에 대해서는 특성 구문을 사용하여 설정됩니다.

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

조건부 네임스페이스에 의존하는 다른 속성에 속성을 바인딩할 경우, 두 속성에 같은 조건을 사용해야 합니다. 여기에서 `colorPicker.Color`는 'contract5Present' 조건부 네임스페이스에 의존하므로, 'contract5Present' 접두사를 SolidColorBrush.Color 속성에도 붙여야 합니다. (또는 Color 속성 대신 SolidColorBrush에 'contract5Present' 접두사를 붙어야 합니다.) 그렇지 않으면 컴파일 시간 오류가 발생합니다.

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

다음은 이러한 시나리오를 보여주는 완전한 XAML입니다. 이 예에는 사각형과 이의 색상을 설정할 수 있는 UI가 포함되어 있습니다.

앱이 Fall Creators Update에서 실행될 경우, ColorPicker를 사용하여 사용자가 색상을 설정하도록 할 수 있습니다. Fall Creators Update 이전 버전에서는 ColorPicker를 사용할 수 없으므로, 앱이 이전 버전에서 실행될 때는 콤바 상자를 사용하여 사용자에게 간단한 색상 선택권을 제공할 수 있습니다.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>관련 문서

- [UWP 앱 가이드](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [API 계약을 사용하여 동적으로 기능 검색](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API 계약](https://channel9.msdn.com/Events/Build/2015/3-733)(빌드 2015 비디오)