---
ms.openlocfilehash: 15379e51f8c272d0cc1e888684322104186bb200
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249505"
---
# <a name="teaching-tip"></a>교육 팁

교육 팁 반 영구 및 풍부한 콘텐츠가 포함 플라이 아웃을 컨텍스트 정보를 제공 하는 경우 알리는, 상기 시켜, 및 경험을 향상 시킬 수 있는 중요 하 고 새 기능에 대 한 사용자 교육에 대 한 자주 사용 됩니다.

**중요 한 Api:** [TeachingTip 클래스](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

교육 팁 않을 light 해제 하거나 닫으려면 명시적 작업이 필요 합니다. 교육 팁 꼬리를 사용 하 여 특정 UI 요소를 대상으로 지정할 수 및 비상 또는 대상 없이 사용할 수도 있습니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요? 

사용 하 여는 **TeachingTip** 경험을 개선 나 작업을 완료 해야 하는 방법을 사용자에 설명 불필요 한 옵션을 사용자에 게 알림 새롭거나 중요 한 업데이트 및 기능에 사용자의 주의 집중 하는 컨트롤입니다. 

교육 팁 일시적 이기 때문에 오류 또는 중요 한 상태 변경에 대 한 사용자에 게 요청 하는 것에 대 한 권장 되는 제어 되지 않습니다.


## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 <a href="xamlcontrolsgallery:/item/TeachingTip">앱을 열고 참조 중인 TeachingTip</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

교육 팁을 포함 하 여 이러한 주목할 만한 몇 가지 구성을 가질 수 있습니다.

교육 팁 표시 정보의 컨텍스트 명확성을 향상 시키기 위해 해당 꼬리가 있는 특정 UI 요소를 대상으로 지정할 수 있습니다. 

![저장 대상 교육 팁을 사용 하 여 샘플 앱 단추입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-targeted.png)

제공 된 정보를 특정 UI 요소에 적용 되지 않습니다, 경우 nontargeted 교육 팁 비상 로그를 제거 하 여 만들 수 있습니다.

![오른쪽 아래 모서리에 교육 팁을 사용 하 여 샘플 앱입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-non-targeted.png)

교육 팁 위쪽 모서리에 있는 "X" 단추를 또는 맨 아래에 "닫기" 단추를 통해 해제 하려면 사용자를 요구할 수 있습니다. 교육 팁 수 또한 수 light-해제 있는 경우 사용할 수 없는 해제 단추가 고 사용자가 스크롤할 또는 응용 프로그램의 다른 요소와 상호 작용 하는 경우에 대신 교육 팁 해제 됩니다. 이 동작으로 인해 light-해제 팁 팁 스크롤 가능한 영역에 배치 해야 하는 경우 가장 적합 한 솔루션을 합니다. 

![사용 하는 샘플 앱을 오른쪽 아래 모서리에서 교육 팁 light-해제 합니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다."](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>교육 팁 만들기

다음 대상된 교육에 대 한 XAML은 설명 제목 및 부제목 TeachingTip의 기본 모양을 보여 주는 컨트롤입니다. 교육 팁 요소 트리 또는 코드 숨김에 어디서 나 나타날 수 있는 참고 합니다. 이 예에서 아래 ResourceDictionary에 있습니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
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

페이지 단추가 들어 있는 및 팁을 가르치는 표시 되 면 결과 다음과 같습니다.

![저장 대상 교육 팁을 사용 하 여 샘플 앱 단추입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>비 타기 팅 팁

일부 팁 화면 요소와 관련이 있습니다. 이러한 시나리오에 대 한 대상 속성을 설정 하지 않으면 및 교육 팁 대신 xaml 루트의 가장자리를 기준으로 표시 됩니다. 그러나 교육 팁 "축소 된" TailVisibility 속성을 설정 하 여 UI 요소를 기준으로 배치를 유지 하는 동안 제거 비상이 있을 수 있습니다. 다음 예제에서는 교육 아닌 팁입니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

이 예제는 TeachingTip 요소 트리의 보다는 ResourceDictionary에 또는 코드 숨김 참고 합니다. 이 동작에 영향을 주지 TeachingTip는만 열리면를 표시 하 고 없는 레이아웃 공간을 차지 합니다.

![오른쪽 아래 모서리에 교육 팁을 사용 하 여 샘플 앱입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>기본 배치

교육 팁 플라이 아웃의 복제 [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) TeachingTipPlacementMode 속성을 사용 하 여 배치 동작 합니다. 기본 배치 모드는 해당 대상 초과 대상된 교육 팁 및 교육 아닌 팁 xaml 루트의 아래쪽 가운데에 배치 하려고 합니다. 으로 플라이 아웃을 사용 하 여 기본 배치 모드를 표시 하려면 교육 팁 확보는 경우 다른 배치 모드로 자동으로 선택 됩니다. 

Gamepad 입력을 예측 하는 응용 프로그램을 참조 하세요 [gamepad 및 원격 제어 상호 작용]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)합니다. 앱의 UI의 모든 가능한 구성을 사용 하 여 각 교육 팁의 gamepad 액세스 가능성을 테스트 하는 것이 좋습니다.

아래쪽 가운데에 해당 대상의 왼쪽으로 이동 하는 교육 팁의 본문을 사용 하 여 비상 로그를 사용 하 여 해당 PreferredPlacement "왼쪽 맨 아래"로 설정 된 대상된 교육 팁 표시 됩니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![왼쪽 아래에 있는 교육 팁 대상이 되는 "저장" 단추를 사용 하 여 샘플 앱입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-targeted-preferred-placement.png)


해당 PreferredPlacement "왼쪽 맨 아래"로 설정 된 교육 아닌 팁 xaml 루트의 왼쪽된 아래 모서리에 표시 됩니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![왼쪽된 아래 모서리에 교육 팁을 사용 하 여 샘플 앱입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-non-targeted-preferred-placement.png)

아래 다이어그램에서는 팁을 가르치는 대상에 대해 설정할 수 있습니다 하는 모든 13 PreferredPlacement 모드의 결과를 보여 줍니다.
![세 가지 개체도 레이블이 지정 된 "대상" 강의 교육 팁의 대상으로 지정 된 기본 배치 모드를 표시 하기 위해 이들을 사용 하는 팁을 대상으로 합니다. 꼬리를 사용 하 여 대상에 있는 아래쪽을 가리키는 "Center" 레이블이 지정 된 대상된 교육 팁은 첫 번째 대상 가운데로 정렬 합니다. "Top" 아래쪽 꼬리를 사용 하 여 대상에 레이블이 지정 된 대상된 교육 팁은 첫 번째 대상을 초과 가운데 맞춤 합니다. 꼬리를 사용 하 여 대상에 있는 왼쪽을 가리키는 "오른쪽" 레이블이 지정 된 대상된 교육 팁은 첫 번째 대상의 오른쪽 가운데 맞춤 합니다. 꼬리를 사용 하 여 대상에 있는 위쪽을 가리키는 "아래쪽" 레이블이 지정 된 대상된 교육 팁은 첫 번째 대상입니다. 첫 번째 대상의 왼쪽은 "왼쪽"를 가리키는 바로 꼬리를 사용 하 여 대상 레이블이 지정 된 대상된 교육 팁 가운데 맞춤 합니다. 두 번째 대상의 왼쪽에는 교육 팁 레이블이 지정 된 대상에 있는 오른쪽을 가리키는 "LeftTop" 및에 몬 위로 이동 합니다. 두 번째 대상을 초과 교육 팁 레이블이 지정 "왼쪽 맨 위" 대상에 있는 아래쪽을 가리키는 되며 해당 본문으로 leftward 합니다. 두 번째의 오른쪽에 대상 지점 대상에 있는 왼쪽을 본문으로 아래로 "RightBottom" 레이블이 지정 된 교육 팁입니다. 두 번째 목표치 교육 팁 레이블이 지정 "오른쪽 맨 아래" 대상에 있는 위쪽을 가리키는 되며 해당 본문으로 패턴입니다. 세 번째 대상 위에서 교육 팁 레이블이 지정 대상에 있는 아래쪽을 가리키는 "개체" 되며 해당 본문으로 패턴입니다. 세 번째의 오른쪽에 대상에는 대상에 있는 지점 였다가 해당 본문 위로 이동 "RightTop" 레이블이 지정 된 교육 팁입니다. 세 번째 목표치 교육 팁 이라고 "왼쪽 맨 아래" 대상에 있는 위쪽을 가리키는 및 본문으로 leftward 합니다. 세 번째 대상의 왼쪽에는 교육 팁 레이블이 지정 된 대상에 있는 오른쪽을 가리키는 "LeftBottom" 및 본문으로 하향.](../images/teaching-tip-targeted-preferred-placement-modes.png)

아래 다이어그램에서는 아닌 교육 팁에 대해 설정할 수 있는 모든 13 PreferredPlacement 모드의 결과를 보여 줍니다.
![교육 팁 아닌 기본 배치 모드를 보여 주기 위해 9 아닌 교육 팁을 표시 하는 앱 창입니다. 앱의 왼쪽된 위 모퉁이에 교육 팁 "왼쪽 맨 위 또는 LeftTop." 라고 합니다. 앱의 위쪽 가장자리에서 가운데 교육 팁 "Top." 라고 합니다. 앱의 오른쪽 위 모서리에 교육 팁 "오른쪽 맨 위 또는 RightTop." 라고 합니다. 앱의 왼쪽된 가장자리에서 가운데 교육 팁 "Left." 라고 합니다. 앱을 중간 가운데 교육 팁 "가운데" 이라고 합니다.
앱의 오른쪽 가장자리에서 가운데 교육 팁 "권한입니다." 라고 합니다. 앱의 왼쪽된 아래 모서리에 교육 팁 "왼쪽 맨 아래 또는 LeftBottom." 라고 합니다. 앱의 아래쪽 가장자리에서 가운데 교육 팁 "아래쪽." 라고 합니다. 앱의 오른쪽 아래 모서리에 교육 팁 "오른쪽 맨 아래 또는 RightBottom." 라고 합니다.](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>배치 여백을 추가합니다  

해당 대상 외에도 대상된 교육 팁 설정 되는 정도 고 xaml 루트의 가장자리와는 별도로 PlacementMargin 속성을 사용 하 여 아닌 교육 팁은 설정 하는 정도 제어할 수 있습니다. 와 같은 [여백](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin), PlacementMargin의 4 가지 값-오른쪽, top, left, 및 아래쪽 –만 관련 값이 사용 됩니다. 예를 들어 PlacementMargin.Left xaml 루트의 왼쪽된 가장자리 또는 대상의 팁을 그대로 사용 하는 경우 적용 됩니다.

다음 예제에서는 PlacementMargin의 왼쪽/오른쪽/상한/를 사용 하 여 아닌 팁 80 모든 집합을 보여 줍니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</controls:TeachingTip>
```

![배치를 포함 하지만 오른쪽 아래 모퉁이 대 한 완전히 교육 팁을 사용 하 여 샘플 앱입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>콘텐츠 추가

콘텐츠 속성을 사용 하 여 교육 팁에 콘텐츠를 추가할 수 있습니다. 더 많은 콘텐츠를 표시 하면 교육 팁의 크기 보다 있으면 콘텐츠 영역을 스크롤할 수 있도록 스크롤 막대를 자동으로 활성화 됩니다. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 대상 교육 팁을 사용 하 여 샘플 앱 단추입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 콘텐츠 영역에서 확인란을 레이블이 지정 "시작 시 팁을 표시 안 함" 되며이 아래에 "변경할 수 있습니다 설정에서 팁 기본 설정을 변경 하려는 경우" 라는 텍스트가 "설정" 앱의 설정 페이지에 대 한 링크입니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>단추 추가

기본적으로 "X" 닫기 단추가 표준 교육 설명의 제목 옆에 표시 됩니다. 닫기 단추는 사례 단추 교육 팁의 아래쪽으로 이동 하는 CloseButtonContent 속성을 사용 하 여 사용자 지정할 수 있습니다.

**참고: 닫기 단추가 없습니다 light 해제 사용된 팁에 표시 됩니다**

사용자 지정 작업 단추가 ActionButtonContent 속성 (및 필요에 따라는 ActionButtonCommand 속성과 ActionButtonCommandParameter)를 설정 하 여 추가할 수 있습니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 대상 교육 팁을 사용 하 여 샘플 앱 단추입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 콘텐츠 영역에서 확인란을 레이블이 지정 "시작 시 팁을 표시 안 함" 되며이 아래에 "변경할 수 있습니다 설정에서 팁 기본 설정을 변경 하려는 경우" 라는 텍스트가 "설정" 앱의 설정 페이지에 대 한 링크입니다. 아래쪽의 교육에는 두 개의 단추, "사용 안 함"을 읽고 있는 왼쪽 회색 하나 및 읽는 오른쪽의 파란색 단일 "하셨습니다."](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Hero 콘텐츠

Edge 지 콘텐츠를 HeroContent 속성을 설정 하 여 교육 팁을 추가할 수 있습니다. HeroContentPlacement 속성을 설정 하 여 위쪽 이나 아래쪽 교육 팁에 hero 콘텐츠의 위치를 설정할 수 있습니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <controls:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </controls:TeachingTip.HeroContent>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 대상 교육 팁을 사용 하 여 샘플 앱 단추입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 맨 아래에서 클라우드에 파일을 배치 하는 만화 남자가 나오는 테두리-테두리 이미지가입니다. 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>아이콘 추가

제목 및 부제목 IconSource 속성을 사용 하 여 옆에 있는 아이콘을 추가할 수 있습니다. 권장 되는 아이콘 크기 16px, 24px, 및 32px 포함 됩니다. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <controls:TeachingTip.IconSource>
                <controls:SymbolIconSource Symbol="Save" />
            </controls:TeachingTip.IconSource>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![저장 대상 교육 팁을 사용 하 여 샘플 앱 단추입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 제목 및 부제목의 왼쪽에 있는 플로피 디스크 아이콘이 경우 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Enable light 해제

Light 해제 기능이 기본적으로 비활성화 되어 있지만 교육 팁 해제 됩니다, 예를 들어 사용자가 스크롤할 또는 응용 프로그램의 다른 요소와 상호 작용 하는 경우를 사용할 수 있습니다. 이 동작으로 인해 light-해제 팁 팁 스크롤 가능한 영역에 배치 해야 하는 경우 가장 적합 한 솔루션을 합니다. 

닫기 단추 데 light-dismiss 설정 된 교육 팁에서 자동으로 제거 됩니다 해당 light 해제 사용자에 게 동작 합니다. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![사용 하는 샘플 앱을 오른쪽 아래 모서리에서 교육 팁 light-해제 합니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다."](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Xaml 루트 범위를 이스케이프합니다.

Windows에서 버전 19 H 1 이상에 교육 팁 ShouldConstrainToRootBounds 속성을 설정 하 여 xaml 루트 및 화면의 범위를 이스케이프할 수 있습니다. 이 속성을 사용할 때 교육 팁 xaml 루트 문자도 화면 경계 내에 유지 하 고 항상 집합 위치를 시도 하지 것입니다 PreferredPlacement 모드입니다. IsLightDismissEnabled 속성을 사용 하 여 사용자에 대 한 최상의 환경을 위해 xaml 루트의 중심에 가장 가까운 PreferredPlacement 모드를 설정 하는 것이 좋습니다.

Windows의 이전 버전에서는이 속성이 무시 되 고 교육 팁 xaml 루트 범위 내에서 항상 유지 됩니다.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</controls:TeachingTip>
```

![앱의 오른쪽 아래 모서리 외부 교육 팁을 사용 하 여 샘플 앱입니다. 팁 제목을 읽고 "자동으로 저장" 읽고 부제목 "에서는 있도록 변경 내용을 저장 량-필요가 없습니다." 교육 팁의 오른쪽 위 모서리에서 닫기 단추가 있습니다.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>취소 및 닫기 지연

취소 및/또는 교육 팁의 종료를 연기 Closing 이벤트를 사용할 수 있습니다. 교육 팁을 열어 작업 또는 사용자 지정 애니메이션을 수행 하는 시간을 허용 하거나 데 사용할 수 있습니다. 그러나 교육 팁 닫기를 취소 되 면 IsOpen 돌아갑니다 true, 계속 유지 됩니다 false를 지연 하는 동안. 프로그래밍 방식으로 닫기를 취소할 수도 있습니다. 

**참고: 배치 옵션 없이 완벽 하 게 표시할 교육 팁을 허용 하는 교육 팁 없이 액세스할 수 있는 닫기 단추를 표시 하지 않고 강제 닫기 이벤트 수명 주기가 반복 됩니다. 앱에서 닫는 이벤트를 취소 하는 경우 교육 팁 없이 액세스할 수 있는 닫기 단추를 열기 남아 있을 수 있습니다.**

XAML
```XAML
<controls:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</controls:TeachingTip>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="remarks"></a>설명

### <a name="related-articles"></a>관련 문서 

* [대화 상자 및 플라이아웃](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>권장 사항
* 팁은 영구적이 지 및 정보 또는 응용 프로그램의 환경에 중요 한 옵션을 포함 하지 않아야 합니다. 
* 너무 자주 교육 팁을 표시 하지 않도록 하려고 합니다. 교육 팁 가능성이 가장 긴 세션 전체에서 또는 여러 세션에서 시차를 적용 될 때 각 개별 주의 수신 합니다.    
* 간결한 유지 팁 및 해당 항목 선택을 취소합니다. 3 ~ 5 단어 읽고만 2-3 단어 팁 상호 작용할 수를 결정 하기 전에 이해할만 연구 평균적으로 사용자를 보여 줍니다.
* 교육 팁의 내게 필요한 옵션 Gamepad 보장 되지 않습니다. Gamepad 입력을 예측 하는 응용 프로그램을 참조 하세요 [gamepad 및 원격 제어 상호 작용]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)합니다. 앱의 UI의 모든 가능한 구성을 사용 하 여 각 교육 팁의 gamepad 액세스 가능성을 테스트 하는 것이 좋습니다.
* Xaml 루트를 이스케이프할 교육 팁을 사용 하도록 설정 하는 경우 또한 IsLightDismissEnabled 속성을 사용 하도록 설정 하 고 xaml 루트의 중심에 가장 가까운 PreferredPlacement 모드를 설정 하려면 것이 좋습니다. 

### <a name="reconfiguring-an-open-teaching-tip"></a>열고 교육 팁을 다시 구성

교육 팁 열려 있고은 즉시 적용 하는 동안 다시 일부 콘텐츠 및 속성을 구성할 수 있습니다. 다른 콘텐츠 및 속성을 아이콘 속성, 작업 및 닫기 단추 사이 다시 구성 light 해제 및 명시적 해제와 같은 모든 해야 교육 팁을 닫고 변경 내용을 적용 하려면 이러한 속성에 대 한 다시 열어야 합니다. 사건의 동작을 변경 합니다. 수동-해제 light 해제 교육 팁 열려 있는 동안에 해당 닫기 단추를 제거 하기 전에 교육 팁 하면는 light 해제 동작을 사용 하 고 팁 화면의 중단 유지 될 수 있습니다.
