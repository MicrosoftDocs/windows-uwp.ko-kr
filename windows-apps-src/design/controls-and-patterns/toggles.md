---
Description: The toggle switch represents a physical switch that allows users to turn things on or off.
title: 토글 스위치 컨트롤에 대한 지침
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d0436f360facceb4a7ee0d02defd5fac2e33eaae
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8702328"
---
# <a name="toggle-switches"></a>토글 스위치

토글 스위치는 전등 스위치처럼 사용자가 켜거나 끌 수 있는 물리적 스위치를 나타냅니다. 토글 스위치 컨트롤을 사용하여 사용자에게 서로 배타적인 두 옵션(예: 켜짐/꺼짐)을 제공합니다. 이러한 옵션은 선택하는 즉시 결과가 제공됩니다.

토글 스위치 컨트롤을 [ToggleSwitch 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)를 사용합니다.

> **중요 API**: [ToggleSwitch 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch), [IsOn 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison), [Toggled 이벤트](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

사용자가 토글 스위치를 전환하면 바로 적용되는 이진 파일 작업용 토글 스위치를 사용합니다.

![WiFi 토글 스위치, 켜짐 및 꺼짐](images/toggleswitches01.png)

토글 스위치를 장치의 물리적 전원 스위치에 비유할 수 있습니다. 장치에서 수행하는 작업을 설정하거나 해제하려고 할 때 토글 스위치를 켜고 끕니다.

토글 스위치를 쉽게 이해할 수 있게 스위치가 제어하는 기능을 설명하는 한두 단어(대개 명사)로 레이블을 붙입니다. "WiFi"나 "주방 등"을 예로 들 수 있습니다. 

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 작동 중인 <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> 또는 <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a>을 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>전환 스위치와 확인란 중 선택

일부 작업에 대해 토글 스위치나 확인란을 사용할 수 있습니다. 어떤 컨트롤이 더 잘 작동하는지 결정하려면 다음 팁을 따르세요.

- 사용자가 변경하는 즉시 적용되는 이진 설정에는 토글 스위치를 사용합니다.

    ![토글 스위치 및 확인란](images/toggleswitches02.png)

    이 예에서는 토글 스위치를 사용하여 주방 등을 “켜짐"으로 설정합니다. 그러나 확인란을 사용하면 주방 등이 현재 켜져 있는지 또는 주방 등을 켜기 위해 확인란을 선택해야 하는지 여부를 판단해야 합니다.

- "있으면 좋은" 선택적 항목에는 확인란을 사용합니다.
- 변경 사항을 적용하려면 추가 단계를 수행해야 하는 경우에는 확인란을 사용합니다. 예를 들어, 사용자가 '전송' 또는 '다음' 단추를 클릭하여 변경 사항을 적용해야 하는 경우에는 확인란을 사용합니다.
- 사용자가 단일 설정이나 기능과 관련된 여러 항목을 선택할 수 있을 때 확인란을 사용합니다.

## <a name="toggle-switches-in-the-windows-ui"></a>Windows UI의 토글 스위치

다음 이미지는 Windows UI에서 토글 스위치를 사용하는 방법을 보여 줍니다. 다음은 Smart Storage 설정 화면에서 토글 스위치를 사용하는 방법입니다.

![Smart Storage의 토글 스위치](images/SmartStorageToggle.png)

다음은 야간 모드 설정 페이지의 예입니다.

![Windows의 시작 메뉴 설정에 있는 토글 스위치](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>토글 스위치 만들기

간단한 토글 스위치를 만드는 방법은 다음과 같습니다. 이 XAML은 앞서 보여준 토글 스위치를 만듭니다.

```xaml
<ToggleSwitch x:Name="lightToggle" Header="Kitchen Lights"/>
```

코드에서 동일한 토글 스위치를 만드는 방법은 다음과 같습니다.

```csharp
ToggleSwitch lightToggle = new ToggleSwitch();
lightToggle.Header = "Kitchen Lights";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(lightToggle);
```

### <a name="ison"></a>IsOn

스위치는 켜짐 또는 꺼짐일 수 있습니다. [IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison) 속성을 사용하여 스위치의 상태를 확인합니다. 스위치를 사용하여 다른 이진 속성의 상태를 제어하는 경우 다음과 같이 바인딩을 사용할 수 있습니다.

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

다른 경우에서는 [Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled) 이벤트를 처리하여 상태 변경에 응답할 수 있습니다.

이 예제에서는 XAML 및 코드에서 Toggled 이벤트 처리기를 추가하는 방법을 보여 줍니다. Toggled 이벤트를 처리하여 진행률 링을 켜거나 끄고 표시 여부를 변경합니다.

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True"
              Toggled="ToggleSwitch_Toggled"/>
```

코드에서 동일한 토글 스위치를 만드는 방법은 다음과 같습니다.

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

Toggled 이벤트 처리기는 다음과 같습니다.

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### <a name="onoff-labels"></a>켜짐/꺼짐 레이블

기본적으로 토글 스위치에는 자동으로 지역화되는 리터럴 켜짐 및 꺼짐 레이블이 포함됩니다. [OnContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) 및 [OffContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent) 속성을 설정하여 이러한 레이블을 바꿀 수 있습니다.

이 예제에서는 켜짐/꺼짐 레이블을 표시/숨기기 레이블로 바꿉니다.

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

[OnContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) 및 [OffContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate) 속성을 설정하여 더 복잡한 콘텐츠를 사용할 수도 있습니다.

## <a name="recommendations"></a>권장 사항

- 가능하면 기본값인 켜짐 및 꺼짐 라벨을 사용합니다. 토글 스위치의 용도를 설명하는 데 필요할 경우에만 다른 라벨을 사용하세요. 라벨을 바꿀 경우 토글을 더 정확하게 설명하는 한 단어를 사용하세요. 일반적으로 "켜짐" 및 "꺼짐"이 토글 스위치에 연결된 작업을 제대로 설명하지 못할 경우 다른 컨트롤이 필요할 수 있습니다.
- 꼭 필요한 경우 외에는 켜짐과 꺼짐 레이블을 바꾸지 마세요. 사용자 지정 레이블이 필요한 경우 외에는 기본 레이블을 고수하세요.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [ToggleSwitch 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [라디오 단추](radio-button.md)
- [토글 스위치](toggles.md)
- [확인란](checkbox.md)
