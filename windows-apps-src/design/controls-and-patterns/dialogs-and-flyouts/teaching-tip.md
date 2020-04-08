---
Description: 교육 팁은 상황별 정보를 제공하는 풍부한 콘텐츠의 반영구적 플라이아웃입니다.
title: 교육 팁
template: detail.hbs
ms.date: 04/01/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
ms.custom: 19H1
ms.localizationpriority: medium
ms.openlocfilehash: 06734c854f0097db5fa96e35d4123dde8bda8a95
ms.sourcegitcommit: 8be8ed1ef4e496055193924cd8cea2038d2b1525
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614146"
---
# <a name="teaching-tip"></a>교육 팁

교육 팁은 상황별 정보를 제공하는 풍부한 콘텐츠의 반영구적 플라이아웃입니다. 작업 환경을 개선할 수 있는 중요한 새 기능을 사용자에게 알리고 교육하는 데 주로 사용됩니다.

교육 팁은 빠르게 해제되거나 닫기 위해 명시적 작업이 필요할 수 있습니다. 교육 팁은 꼬리를 사용하여 특정 UI 요소를 대상으로 지정할 수 있으며, 꼬리 또는 대상 없이 사용할 수도 있습니다.

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](../images/winui-logo-64x64.png) | **TeachingTip** 컨트롤은 UWP 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리가 필요합니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

> **Windows UI 라이브러리 API:** [TeachingTip 클래스](/uwp/api/microsoft.ui.xaml.controls.teachingtip)

> [!TIP]
> 이 문서 전체에서 XAML의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이를 [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 요소(`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`)에 추가했습니다.
>
>코드 숨김에서는 C#의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이 **using** 문(`using muxc = Microsoft.UI.Xaml.Controls;`)을 파일 맨 위에 추가했습니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

**TeachingTip** 컨트롤을 사용하여 새롭거나 중요한 업데이트 및 기능에 사용자의 관심을 집중시키거나, 작업 환경을 향상시킬 수 있는 비필수 옵션을 사용자에게 미리 알리거나, 작업 완료 방법을 사용자에게 알려줍니다.

교육 팁은 일시적이므로 사용자에게 오류 또는 중요한 상태 변경을 알릴 때 권장되는 컨트롤은 아닙니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/TeachingTip">앱을 열고 작동 중인 TeachingTip을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

교육 팁은 다음과 같은 주목할 만한 구성을 비롯한 몇 가지 구성으로 표시할 수 있습니다.

교육 팁은 꼬리로 특정 UI 요소를 대상으로 지정하여 나타내는 정보의 컨텍스트 명확성을 높일 수 있습니다.

![저장 단추를 대상으로 지정하는 교육 팁이 있는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-targeted.png)

제공된 정보가 특정 UI 요소에 적용되지 않는 경우 해당 꼬리를 제거하여 대상이 없는 교육 팁을 만들 수 있습니다.

![오른쪽 아래 모서리에 교육 팁이 표시되는 샘플 앱입니다. 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-non-targeted.png)

교육 팁은 사용자가 위쪽 모서리에 있는 "X" 단추 또는 아래쪽의 "닫기" 단추를 통해 해제해야 할 수 있습니다. 교육 팁은 해제 단추가 없는 경우에 빠른 해제를 설정할 수 있으며, 사용자가 스크롤하거나 애플리케이션의 다른 요소와 상호 작용할 때 해제됩니다. 이 동작으로 인해 팀을 스크롤 가능 영역에 배치해야 하는 경우 빠른 해제 팁이 가장 적합한 해결 방법입니다.

![오른쪽 아래 모서리에 빠른 해제 교육 팁이 표시되는 샘플 앱입니다. 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다.](../images/teaching-tip-light-dismiss.png)

### <a name="create-a-teaching-tip"></a>교육 팁 만들기

다음은 제목 및 부제가 있는 TeachingTip의 기본 모양을 보여 주는 대상 있는 교육 팁 컨트롤의 XAML입니다.
교육 팁은 요소 트리 또는 코드 숨김 어디에서든지 나타날 수 있습니다. 아래의 예제에서는 ResourceDictionary에 있습니다.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

다음은 단추 및 교육 팁을 포함하는 페이지가 표시될 때의 결과입니다.

![저장 단추를 대상으로 지정하는 교육 팁이 있는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>대상 없는 팁

모든 팁이 요소 화면과 관련된 것은 아닙니다. 이러한 시나리오에서는 Target 속성을 설정하지 않도록 합니다. 대신, 교육 팁이 xaml 루트의 가장자리를 기준으로 표시됩니다. 그러나 TailVisibility 속성을 "Collapsed"로 설정하여 해당 위치를 UI 요소에 상대적으로 유지하면서 교육 팁의 꼬리를 제거할 수 있습니다. 다음 예제는 대상 없는 교육 팁입니다.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</muxc:TeachingTip>
```

이 예제에서 TeachingTip은 ResourceDictionary 또는 코드 숨김이 아닌 요소 트리에 있습니다. 이러한 사실은 동작에는 영향을 주지 않습니다. TeachingTip은 열려 있을 때만 표시되며, 레이아웃 공간을 차지하지 않습니다.

![오른쪽 아래 모서리에 교육 팁이 표시되는 샘플 앱입니다. 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>기본 설정 배치

교육 팁은 TeachingTipPlacementMode 속성을 사용하여 플라이아웃의 [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) 배치 동작을 복제합니다. 기본 배치 모드는 대상 있는 교육 팁을 대상 위에 배치하고, 대상 없는 교육 팁을 xaml 루트의 아래쪽에 배치하려고 합니다. 플라이아웃을 사용할 때와 마찬가지로, 기본 설정 배치 모드에서 교육 팁을 표시할 공간이 없으면 다른 배치 모드가 자동으로 선택됩니다.

게임 패드 입력을 예측하는 애플리케이션의 경우 [게임 패드 및 리모컨 조작]( https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)을 참조하세요. 앱 UI의 가능한 모든 구성을 사용하여 각 교육 팁의 게임 패드 액세스 가능성을 테스트하는 것이 좋습니다.

PreferredPlacement가 "BottomLeft"로 설정된 대상 있는 교육 팁은 교육 팁 본문이 왼쪽으로 이동된 상태로 꼬리가 대상 맨 아래의 중앙에 표시됩니다.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![교육 팁의 대상으로 지정된 "저장" 단추가 왼쪽 모서리 아래에 표시되는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-targeted-preferred-placement.png)

해당 PreferredPlacement가 "BottomLeft"로 설정된 대상 없는 교육 팁은 xaml 루트의 왼쪽 아래 모서리에 표시됩니다.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</muxc:TeachingTip>
```

![왼쪽 아래 모서리에 교육 팁이 표시되는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-non-targeted-preferred-placement.png)

아래 다이어그램에서는 대상 있는 교육 팁에 대해 설정할 수 있는 13개 PreferredPlacement 모드 전체의 결과를 보여 줍니다.
![각각이 다른 대상 지정 배치 모드를 보여 주는 13개 교육 팁을 포함하는 그림 각 교육 팁이 나타내는 모드가 레이블로 지정됩니다. 배치 모드의 첫 번째 단어는 교육 팁이 중앙에 표시되는 대상의 측면을 나타냅니다. 교육 팁의 꼬리는 항상 해당 대상 측면의 가운데에 위치하며 대상을 향합니다. 배치 모드에 두 번째 단어가 있으면 교육 팁의 본문은 가운데에 오지 않고, 대신 지정한 방향으로 이동합니다. 예를 들어, 배치 모드 "TopRight"는 교육 팁이 대상 위에 표시된 후 오른쪽으로 이동되어 꼬리가 대상 위쪽 가장자리의 가운데를 가리키게 합니다. 본문이 오른쪽으로 이동하므로 꼬리는 거의 교육 팁 본문의 맨 왼쪽 가장자리에 있으며 교육 팁은 대상의 오른쪽 가장자리를 넘어 확장됩니다. 배치 모드 "Center"에서 독특하게 교육 팁의 꼬리가 대상의 가운데를 가리키며, 교육 팁은 대상의 위쪽 절반에서 가운데에 표시됩니다.](../images/teaching-tip-targeted-preferred-placement-modes.png)

아래 다이어그램에서는 대상 없는 교육 팁에 대해 설정할 수 있는 13개 PreferredPlacement 모드 전체의 결과를 보여 줍니다.
![각각이 다른 대상 비지정 배치 모드를 보여 주는 9개 교육 팁을 포함하는 그림 각 교육 팁이 나타내는 모드가 레이블로 지정됩니다. 배치 모드의 첫 번째 단어는 교육 팁이 중앙에 표시되는 xaml 루트의 측면을 나타냅니다. 배치 모드에 두 번째 단어가 있으면 교육 팁은 xaml 루트의 지정된 모서리 쪽으로 배치됩니다. 예를 들어, 배치 모드 "TopRight"는 교육 팁을 xaml 루트의 오른쪽 위 모서리에 표시합니다. 대상 없는 배치 모드의 경우 두 단어의 순서가 배치에 영향을 주지 않습니다. TopRight는 RightTop과 같습니다. 배치 모드 "Center"에서는 독특하게 교육 팁이 xaml 루트의 세로 및 가로 가운데에 표시됩니다.](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>배치 여백 추가

PlacementMargin 속성을 사용하여 대상 있는 교육 팁이 해당 대상에서 얼마나 멀리 떨어져 있는지와 대상 없는 교육 팁이 xaml 루트의 가장자리에서 얼마나 멀리 떨어져 있는지를 제어할 수 있습니다. [Margin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)과 같이, PlacementMargin에는 4가지 값인 left, right, top, bottom이 있으며 해당하는 값만 사용됩니다. 예를 들어, PlacementMargin.Left는 팁이 대상의 왼쪽 또는 xaml 루트의 왼쪽 가장자리에 있을 때 적용됩니다.

다음 예제에서는 PlacementMargin의 Left/Top/Right/Bottom이 모두 80으로 설정된 대상 없는 팁을 보여 줍니다.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</muxc:TeachingTip>
```

![교육 팁이 위쪽으로 배치되지만 오른쪽 아래 모서리에서 완전히 반대되게 배치되지는 않은 샘플 앱입니다. 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>콘텐츠 추가

콘텐츠 속성을 사용하여 교육 팁에 콘텐츠를 추가할 수 있습니다. 교육 팁의 크기가 허용하는 것보다 표시할 콘텐츠가 더 많으면 사용자가 콘텐츠 영역을 스크롤할 수 있게 스크롤 막대가 자동으로 사용하도록 설정됩니다.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 단추를 대상으로 지정하는 교육 팁이 있는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 콘텐츠 영역에는 "시작할 때 팁 표시 안 함" 레이블의 CheckBox가 있고, 아래쪽에 앱 설정 페이지에 대한 “설정" 링크가 표시되는 위치에서 “원할 경우 설정에서 팁 기본 설정을 변경할 수 있습니다.” 텍스트가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>단추 추가

기본적으로 "X" 닫기 단추는 교육 팁의 제목 옆에 표시됩니다. CloseButtonContent 속성을 사용하여 닫기 단추를 사용자 지정할 수 있습니다. 이 경우 단추가 교육 팁의 아래쪽으로 이동합니다.

**참고: 빠른 해제가 사용하도록 설정된 팁에는 닫기 단추가 없습니다.**

ActionButtonContent 속성(및 필요에 따라 ActionButtonCommand 및 ActionButtonCommandParameter 속성)을 설정하여 사용자 지정 작업 단추를 추가할 수 있습니다.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="{x:Bind DisableAutoSaveCommand}"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 단추를 대상으로 지정하는 교육 팁이 있는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 콘텐츠 영역에는 "시작할 때 팁 표시 안 함" 레이블의 CheckBox가 있고, 아래쪽에 앱 설정 페이지에 대한 “설정" 링크가 표시되는 위치에서 “원할 경우 설정에서 팁 기본 설정을 변경할 수 있습니다.” 텍스트가 표시됩니다. 교육 팁 아래쪽에서 왼쪽에는 회색 "사용 안 함" 단추가 표시되고, 오른쪽에는 파란색 “확인" 단추가 표시됩니다.](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Hero 콘텐츠

HeroContent 속성을 설정하여 교육 팁을 가장자리에서 가장자리로 추가할 수 있습니다. HeroContentPlacement 속성을 설정하여 Hero 콘텐츠의 위치를 교육 팁의 위쪽 이나 아래쪽으로 설정할 수 있습니다.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <muxc:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </muxc:TeachingTip.HeroContent>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 단추를 대상으로 지정하는 교육 팁이 있는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 아래쪽에는 구름에 파일을 올려 놓는 만화 속 남성의 전체 이미지가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>아이콘 추가

IconSource 속성을 사용하여 제목 및 부제 옆에 아이콘을 추가할 수 있습니다. 권장되는 아이콘 크기에는 16px, 24px 및 32px이 포함됩니다.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <muxc:TeachingTip.IconSource>
                <muxc:SymbolIconSource Symbol="Save" />
            </muxc:TeachingTip.IconSource>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 단추를 대상으로 지정하는 교육 팁이 있는 샘플 앱 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 제목 및 부제의 왼쪽에는 플로피 디스크 아이콘이 있습니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>빠른 해제 사용

빠른 해제 기능은 기본적으로 사용하지 않도록 설정되어 있지만 사용자가 스크롤하거나 애플리케이션의 다른 요소와 상호 작용할 때 교육 팁이 해제되도록 이 기능을 사용하도록 설정할 수 있습니다. 이 동작으로 인해 팀을 스크롤 가능 영역에 배치해야 하는 경우 빠른 해제 팁이 가장 적합한 해결 방법입니다.

빠른 해제 동작을 사용자에게 알리기 위해 빠른 해제 기능이 설정된 교육 팁에서 닫기 단추가 자동으로 제거됩니다.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</muxc:TeachingTip>
```

![오른쪽 아래 모서리에 빠른 해제 교육 팁이 표시되는 샘플 앱입니다. 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다.](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>XAML 루트 범위 이스케이프

Windows 10, 버전 1903(빌드 18362)부터 학습 팁은 `ShouldConstrainToRootBounds` 속성을 설정하여 XAML 루트 및 화면의 경계를 이스케이프할 수 있습니다. 이 속성을 사용하도록 설정하면 교육 팁은 XAML 루트 및 화면 범위 안을 유지하려고 하지 않으며, 항상 `PreferredPlacement` 설정 모드에 있습니다. `IsLightDismissEnabled` 속성을 사용하고 `PreferredPlacement` 모드를 XAML 루트의 중심에 가장 가깝게 설정하여 사용자에게 최상의 환경을 제공하는 것이 좋습니다.

이전 Windows 버전에서는 이 속성이 무시되고 교육 팁이 항상 XAML 루트 범위를 벗어나지 않습니다.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</muxc:TeachingTip>
```

![앱의 오른쪽 아래 모서리 밖에 교육 팁이 표시되는 샘플 앱입니다. 팁 제목으로 "자동으로 저장"이 표시되고 부제로 "변경 내용을 자동으로 저장하므로 직접 저장할 필요가 없습니다."가 표시됩니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>닫기 취소 및 지연

Closing 이벤트를 사용하여 교육 팁의 닫기를 취소 및/또는 지연할 수 있습니다. 이 이벤트를 사용하여 교육 팁을 열어 두거나 작업 또는 사용자 지정 애니메이션이 수행될 수 있는 시간을 허용할 수 있습니다. 그러나 교육 팁 닫기를 취소하면 IsOpen이 true로 복귀되지만, 지연 동안에는 false 상태를 유지합니다. 프로그래밍 방식 닫기도 취소할 수 있습니다.

> [!NOTE]
> 배치 옵션에 따르면 교육 팁을 완전히 표시하지 못할 경우 교육 팁은 액세스 가능한 닫기 단추가 없는 상태로 표시되지 않고, 이벤트 수명 주기를 반복하여 닫기를 강제로 수행합니다. 앱에서 Closing 이벤트를 취소하는 경우 교육 팁은 액세스 가능한 닫기 단추 없이 열린 상태를 유지할 수 있습니다.

```xaml
<muxc:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</muxc:TeachingTip>
```

```csharp
private void OnTipClosing(muxc.TeachingTip sender, muxc.TeachingTipClosingEventArgs args)
{
    if (args.Reason == muxc.TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                // We were not able to update the settings!
                // Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="recommendations"></a>권장 사항

* 팁은 영구적이지 않으며 애플리케이션 환경에 중요한 정보 또는 옵션을 포함하지 않아야 합니다.
* 교육 팁을 너무 자주 표시하지 않도록 합니다. 교육 팁의 긴 세션 전체에서 또는 여러 세션 간에 시차를 적용하면 각 팁이 따로 집중될 수 있습니다.
* 팁을 간결하게 유지하고 주제를 명확히 드러냅니다. 연구에 따르면 사용자는 팁과 상호 작용할지 결정하기 전에 평균적으로 3 ~ 5개 단어만 읽으며 2-3개 단어만 이해한다고 합니다.
* 교육 팁의 게임 패드 액세스 가능성은 보장되지 않습니다. 게임 패드 입력을 예측하는 애플리케이션의 경우 [게임 패드 및 리모컨 조작]( https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)을 참조하세요. 앱 UI의 가능한 모든 구성을 사용하여 각 교육 팁의 게임 패드 액세스 가능성을 테스트하는 것이 좋습니다.
* 교육 팁이 xaml 루트를 이스케이프하도록 설정하는 경우 IsLightDismissEnabled 속성도 사용하도록 설정하고 PreferredPlacement 모드를 xaml 루트의 중심에 가장 가깝게 설정하는 것이 좋습니다.

## <a name="reconfiguring-an-open-teaching-tip"></a>열린 교육 팁 다시 구성

교육 팁이 열려 있으며 즉시 적용될 경우 일부 콘텐츠 및 속성을 다시 구성할 수 있습니다. 다른 콘텐츠 및 속성(예: 아이콘 속성), 작업 및 닫기 단추, 빠른 해제와 명시적 해제 간의 재구성 작업은 모두 해당 속성의 변경 내용을 적용하기 위해 교육 팁을 닫았다가 다시 얼어야 합니다. 교육 팁을 열어 놓은 상태에서 해제 동작을 수동 해제에서 빠른 해제로 변경하면 빠른 해제 동작이 사용되기 전에 교육 팁의 닫기 단추가 제거되므로 팁이 화면에 표시된 상태를 계속 유지할 수 있습니다.

## <a name="related-articles"></a>관련된 문서

* [대화 상자 및 플라이아웃](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)