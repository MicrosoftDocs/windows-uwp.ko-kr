---
author: Jwmsft
Description: A color picker lets a user browse through and select colors.
title: 색 선택
label: Color Picker
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f2b6270fcd7bc3dcf45d6e80dd547ee783d8e9ae
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6858756"
---
# <a name="color-picker"></a>색 선택

색 선택은 색을 찾아보고 선택하는 데 사용됩니다. 색 선택을 사용하면 사용자가 기본적으로 색 스펙트럼에서 색을 탐색하거나 RGB(Red-Green-Blue), HSV(색상 채도 값) 또는 16진수 텍스트 상자에서 색을 지정할 수 있습니다.

> **중요 API**: [ColorPicker 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker), [Color 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color), [ColorChanged 이벤트](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

![기본 색 선택](images/color-picker-default.png)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

색 선택을 사용하여 사용자가 앱에서 색을 선택할 수 있게 하세요. 예를 들어, 색 선택을 사용하여 글꼴 색, 배경 또는 앱 테마 색 등의 색 설정을 변경하세요.

앱이 펜을 사용한 그리기 등의 작업용인 경우 색 선택과 함께 [잉크 컨트롤](inking-controls.md) 사용을 고려하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/ColorPicker">앱을 열고 작동 중인 ColorPicker를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>색 선택 만들기

이 예에서는 XAML에서 기본 색 선택을 만드는 방법을 보여 줍니다.

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

기본적으로 색 선택은 색 스펙트럼 옆의 사각형 표시줄에 선택한 색을 미리 보여 줍니다. [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) 이벤트나 [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color) 속성을 사용하여 선택한 색을 액세스하고 앱에 사용할 수 있습니다. 자세한 코드는 다음 예를 참조하세요.

### <a name="bind-to-the-chosen-color"></a>선택한 색에 바인딩

색 선택이 즉시 적용되어야 하는 경우 데이터 바인딩을 사용하여 Color 속성에 바인딩하거나 ColorChanged 이벤트를 처리하여 코드에서 선택한 색에 액세스할 수 있습니다.

이 예에서는 Rectangle에 대한 Fill로 사용되는 SolidColorBrush의 Color 속성을 색 선택의 선택된 색에 직접 바인딩합니다. 색 선택을 변경하면 바인딩된 속성도 바로 변경됩니다.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape=”Ring”
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

이 예에서는 "일반"적인 색 선택 환경인 원과 슬라이더가 있는 간단한 색 선택을 사용합니다. 영향을 받는 개체에 색 변경이 실시간으로 적용되면 색 미리 보기 표시줄을 표시할 필요가 없습니다. 자세한 내용은 *색 선택 사용자 지정* 섹션을 참조하세요.

### <a name="save-the-chosen-color"></a>선택한 색 저장

색 변경을 즉시 적용하지 않으려는 경우가 있습니다. 예를 들어, 플라이아웃에서 색 선택을 호스트하는 경우 사용자가 선택 사항을 확인하고 플라이아웃을 닫은 후에만 선택한 색을 적용하는 것이 좋습니다. 나중에 사용할 수 있게 선택한 색 값을 저장할 수도 있습니다.

이 예에서는 확인 및 취소 단추로 플라이아웃에 색 선택을 호스트합니다. 사용자가 색 선택을 확인하면 앱에서 나중에 사용할 수 있게 선택한 색을 저장합니다.

```xaml
<Page.Resources>
    <Flyout x:Key="myColorPickerFlyout">
        <RelativePanel>
            <ColorPicker x:Name="myColorPicker"
                         IsColorChannelTextInputVisible="False"
                         IsHexInputVisible="False"/>

            <Grid RelativePanel.Below="myColorPicker"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Content="OK" Click="confirmColor_Click"
                        Margin="0,12,2,0" HorizontalAlignment="Stretch"/>
                <Button Content="Cancel" Click="cancelColor_Click"
                        Margin="2,12,0,0" HorizontalAlignment="Stretch"
                        Grid.Column="1"/>
            </Grid>
        </RelativePanel>
    </Flyout>
</Page.Resources>

<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Button x:Name="colorPickerButton"
            Content="Pick a color"
            Flyout="{StaticResource myColorPickerFlyout}"/>
</Grid>
```

```csharp
private Color mycolor;

private void confirmColor_Click(object sender, RoutedEventArgs e)
{
    // Assign the selected color to a variable to use outside the popup.
    myColor = myColorPicker.Color;

    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}

private void cancelColor_Click(object sender, RoutedEventArgs e)
{
    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}
```

### <a name="configure-the-color-picker"></a>색 선택 구성

사용자가 색을 선택할 수 있기 위해 모든 필드가 필요하지는 않으므로 색 선택은 유연합니다. 색 선택은 필요에 맞게 컨트롤을 구성할 수 있는 다양한 옵션을 제공합니다.

예를 들어, 노트 기록 앱에서 형광펜 색을 선택하는 경우와 같이 사용자가 정밀한 컨트롤을 필요로 하지 않을 때 경우 간단한 UI를 사용할 수 있습니다. 텍스트 입력 필드를 숨기고 색 스펙트럼을 원으로 변경할 수 있습니다.

그래픽 디자인 앱에서와 같이 사용자가 정밀한 제어를 필요로 할 경우 색의 각 양상에 대해 슬라이더와 텍스트 입력 필드를 모두 표시할 수 있습니다.

#### <a name="show-the-circle-spectrum"></a>원 스펙트럼 표시

이 예에서는 [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) 속성을 사용하여 기본 사각형 대신 원 스펙트럼을 사용하도록 색 선택을 구성하는 방법을 보여 줍니다.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![원 스펙트럼이 포함하는 색 선택](images/color-picker-ring.png)

사각형과 원 색 스펙트럼 중 하나를 선택해야 하는 경우 첫 번째로 고려해야 할 사항은 정밀도입니다. 사각형을 사용하면 더 많은 색 영역이 표시되기 때문에 더 정밀하게 특정 색을 선택할 수 있습니다. 좀 더 "일반"적인 색 선택을 원할 경우 원 스펙트럼이 적합합니다.

#### <a name="show-the-alpha-channel"></a>알파 채널 표시

이 예에서는 색 선택에서 불투명도 슬라이더와 텍스트 상자를 활성화합니다.

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![IsAlphaEnabled가 true로 설정된 색 선택](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>단순 선택 표시

이 예에서는 "일반" 용도의 단순한 UI로 색 선택을 구성하는 방법을 보여 줍니다. 원형 스펙트럼을 표시하고 기본 텍스트 입력 상자를 숨깁니다. 영향을 받는 개체에 색 변경이 실시간으로 적용되면 색 미리 보기 표시줄을 표시할 필요가 없습니다. 그렇지 않으면 색 미리 보기를 표시된 상태로 두어야 합니다.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![단순 색 선택](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>추가 기능 표시 또는 숨기기

이 표에는 ColorPicker 구성에 사용할 수 있는 모든 옵션이 나와 있습니다.

기능 | 속성
--------|-----------
색 스펙트럼 | IsColorSpectrumVisible, ColorSpectrumShape, ColorSpectrumComponents
색 미리 보기 | IsColorPreviewVisible
색 값| IsColorSliderVisible, IsColorChannelTextInputVisible
투명도 값 | IsAlphaEnabled, IsAlphaSliderVisible, IsAlphaTextInputVisible
16진수 값 | IsHexInputVisible

> [!NOTE]
> 불투명도 텍스트 상자와 슬라이더를 표시하려면 IsAlphaEnabled가 **true**여야 합니다. 그러면 IsAlphaTextInputVisible 및 IsAlphaSliderVisible 속성을 사용하여 입력 컨트롤의 가시성을 수정할 수 있습니다. 자세한 내용은 API 설명서를 참조하세요.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 어떤 종류의 색 선택 환경이 앱에 적합한지 생각하세요. 일부 시나리오는 세밀한 색 선택이 필요 없고 간단한 선택이 유리할 수 있습니다.
- 가장 정밀한 색 선택 환경이 필요한 경우 256x256px 이상의 사각형 스펙트럼을 사용하거나 텍스트 입력 필드를 포함하여 사용자가 선택한 값을 미세 조정할 수 있게 하세요.
- 플라이아웃에 사용할 경우 스펙트럼을 탭하거나 슬라이더만 조정하여 색 선택을 커밋하면 안 됩니다. 선택을 커밋하려면
  - 선택 적용 또는 취소를 위한 커밋 및 취소 단추를 제공합니다. 뒤로 단추를 누르거나 플라이아웃 바깥쪽을 탭하면 선택이 해제되고 사용자의 선택 내용이 저장되지 않습니다.
  - 또는 플라이아웃 바깥쪽을 탭하거나 뒤로 단추를 눌러 플라이아웃을 해제할 때 선택을 커밋합니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [UWP 앱에서 펜 및 스타일러스 조작](../input/pen-and-stylus-interactions.md)
- [수동 입력](inking-controls.md)

<!--
<div class=”microsoft-internal-note”>
<p>
<p>
Note: For more info, see the [color picker redlines](http://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->