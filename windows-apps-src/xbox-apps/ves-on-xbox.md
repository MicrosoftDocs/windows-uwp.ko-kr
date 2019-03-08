---
title: 음성은 Xbox에서 셸 VES ()를 사용 하도록 설정
description: Xbox에서 UWP 앱에 음성 제어 지원을 추가 하는 방법을 알아봅니다.
ms.date: 10/19/2017
ms.topic: article
keywords: windows 10, uwp, xbox, 음성, 음성 지원 셸
ms.openlocfilehash: ea51216c804754e98c3bac459b79fb75dd9369cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596058"
---
# <a name="using-speech-to-invoke-ui-elements"></a>음성을 사용 하 여 UI 요소를 호출 합니다.

음성 사용 셸 (VES)는 Windows 음성 플랫폼에 앱 내에서 최고 수준의 음성 환경을 지는 사용자가 화면의 호출에 대 한 음성 기능을 사용 하도록 허용 확장 컨트롤 및 받아쓰기를 통해 텍스트를 삽입 합니다. VES 앱 개발자 로부터 필요한 최소한의 노력으로 모든 Windows 셸 및 장치에서 일반적인 종단 간 참조 it-say it 경험을 제공 하려고 합니다.  이 위해 Microsoft 음성 플랫폼 및 UIA (UI 자동화) 프레임 워크를 활용 합니다.

## <a name="user-experience-walkthrough"></a>사용자 환경 연습 ##
다음은 Xbox에서 VE를 사용 하는 경우 사용자는 환경 개요 및 VE 작동 하는 방법의 세부 정보를 살펴보기 전에 컨텍스트를 설정 합니다. 도움이 될 것입니다.

- 사용자는 Xbox 콘솔 켜고 관심 항목을 찾으려면 해당 앱을 탐색 하려고 합니다.

        User: "Hey Cortana, open My Games and Apps"

- 사용자는 왼쪽에 활성 수신 모드 (ALM), "Hey Cortana" 콘솔은 이제 들어 필요 없이 화면에 표시 되는 컨트롤을 호출 하는 사용자에 대 한 수신 의미 때마다입니다.  사용자 앱 및 앱 목록 스크롤하여 볼 전환할 수 있습니다.

        User: "applications"

- 뷰 스크롤을 사용자 수 라고 지정 하면 됩니다.

        User: "scroll down"

- 박스 아트에 관심이 있지만 이름을 찾기 앱에 대 한 사용자에 게 표시 합니다.  사용자가 표시할 음성 팁 레이블에 대 한 요청:

        User: "show labels"

- 이제는 명확한 예를 들어 내용을, 앱을 시작할 수 있습니다.

        User: "movies and TV"

- 활성 수신 모드를 종료 하려면 사용자 수신 대기를 중지할 Xbox를 지시 합니다.

        User: "stop listening"

- 나중에 사용 하 여 새 활성 수신 세션을 시작할 수 있습니다.

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>UI automation 종속성 ##
VE는 UI 자동화 클라이언트 및 해당 UI 자동화 공급자를 통해 앱에서 노출 되는 정보에 의존 합니다. 이것이 Windows 플랫폼에서 내레이터 기능에서 이미 사용 중인 동일한 인프라입니다.  UI 자동화에는 컨트롤, 해당 유형 및 컨트롤 패턴 구현의 이름을 포함 한 사용자 인터페이스 요소에 프로그래밍 방식으로 액세스할 수 있습니다.  UI 앱에서 변경 VES UIA 업데이트 이벤트에 대응 되며 다시 구문 분석 가능한 모든 항목을 찾으려면 업데이트 UI Automation 트리 스피치 인식 그래 머를이 정보를 사용 하 여 됩니다. 

모든 UWP 앱 UI 자동화 프레임 워크에 액세스할 수 있으며 빌드 (XAML, DirectX/Direct3D, Xamarin 등)에 그래픽 프레임 워크의 독립적인 UI에 대 한 정보를 노출할 수 있습니다.  XAML와 같은 일부 경우에서 어려운 작업 중 대부분은 내레이터 및 VE를 지 원하는 데 필요한 작업을 대폭 단축 프레임 워크에 의해 수행 됩니다.

UI 자동화에 대 한 자세한 정보를 참조 하세요. [UI 자동화 기본 사항](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "UI 자동화 기본 사항")합니다.

## <a name="control-invocation-name"></a>컨트롤 호출 이름 ##
컨트롤의 이름으로 음성 인식기를 사용 하 여 등록 하려면 구를 무엇을 확인 하기 위한 다음과 같은 추론을 사용 하는 VE (ie. 컨트롤을 호출 하는 데 필요한 사용자).  음성 팁 레이블에 표시 되는 구 이기도 합니다.

우선 순위에 따라에서 이름 원본:

1. 요소에는 `LabeledBy` VE 연결된 속성을 사용할지를 `AutomationProperties.Name` 이 텍스트 레이블의.
2. `AutomationProperties.Name` 요소입니다.  XAML을 컨트롤의 텍스트 내용에 대 한 기본 값으로 사용할 수는 `AutomationProperties.Name`합니다.
3. 유효한를 사용 하 여 첫 번째 자식 요소에 대 한 VE 보입니다 ListItem 또는 단추 컨트롤이 있으면 `AutomationProperties.Name`합니다.

## <a name="actionable-controls"></a>실행 가능한 컨트롤 ##
VES 다음 자동화 컨트롤 패턴 중 하나를 구현 하는 경우에 실행 가능한 컨트롤을 간주 합니다.

- **InvokePattern** (예: 단추)-시작 하거나 명확한 단일 작업을 수행 하 고 활성화 되었을 때 상태를 유지 하지 않는 하는 컨트롤을 나타냅니다.

- **TogglePattern** (예: 상자를 선택)-일련의 상태를 순환 하 고 설정 된 상태를 유지할 수 있는 컨트롤을 나타냅니다.

- **SelectionItemPattern** (예: 콤보 상자)-선택 가능한 자식 항목의 컬렉션에 대 한 컨테이너 역할을 하는 컨트롤을 나타냅니다.

- **ExpandCollapsePattern** (예: 콤보 상자)-시각적으로 콘텐츠를 표시 하도록 확장 하 고 콘텐츠를 숨기려고 축소 하는 컨트롤을 나타냅니다.

- **ScrollPattern** (예: 목록)-자식 요소의 컬렉션에 대해 스크롤 가능한 컨테이너 역할을 하는 컨트롤을 나타냅니다.

## <a name="scrollable-containers"></a>스크롤 가능한 컨테이너 ##
스크롤 가능한 컨테이너에 대 한 음성 ScrollPattern, VES 수신할 지 등 "스크롤 왼쪽", "오른쪽으로 스크롤" 명령 및 이러한 명령 중 하나를 트리거할 때 스크롤 적절 한 매개 변수를 사용 하 여 호출 합니다.  스크롤 명령의 값을 기반으로 삽입 되는 `HorizontalScrollPercent` 및 `VerticalScrollPercent` 속성입니다.  예를 들어 경우 `HorizontalScrollPercent` 0, "스크롤 왼쪽" 100 "오른쪽으로 스크롤" 추가 되는 보다 적은 경우 추가할 수 있는 보다 크고 등입니다.

## <a name="narrator-overlap"></a>내레이터 겹침 ##
내레이터 응용 프로그램 UI 자동화 클라이언트가 이기도 하며 사용 하는 `AutomationProperties.Name` 속성을 읽고 현재 선택한 UI 요소에 대 한 텍스트에 대 한 원본 중 하나입니다.  많은 앱을 보다 편리 하 게 내게 필요한 옵션을 제공 하려면 개발자가에 맞도록 오버 로드는 `Name` 자세한 내용 및 내레이터에서 읽을 때 컨텍스트를 제공 하는 목표를 사용 하 여 긴 설명 텍스트를 사용 하 여 속성입니다.  그러나 두 기능 간에 충돌이 발생 합니다. VES 짧은 구 일치 하거나 밀접 하 게 내레이터 이점 더, 설명이 포함 된 구 더 나은 컨텍스트를 제공 하는 동안 컨트롤의 표시 텍스트와 일치 해야 합니다.

이 해결 하려면, Windows 10 크리에이터 스 업데이트부터 내레이터를 살펴볼 수도 하도록 업데이트 되었습니다는 `AutomationProperties.HelpText` 속성입니다.  내레이터 외에 해당 콘텐츠를 말하고이 속성을 비어 있지 않으면 `AutomationProperties.Name`합니다.  경우 `HelpText` 는 비어 있는 경우 내레이터 데이터만 이름의 내용을 읽습니다.  더 이상 설명이 포함 된 문자열을, 필요한 경우 사용 하도록 하면이 있지만에서 짧은, 음성 인식 친숙 한 구문을 유지 관리는 `Name` 속성입니다.

![](images/ves_narrator.jpg)

자세한 내용은 참조 하세요. [UI에서 내게 필요한 옵션 지원에 대 한 자동화 속성](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "UI에서 내게 필요한 옵션 지원에 대 한 자동화 속성")합니다.

## <a name="active-listening-mode-alm"></a>활성 수신 모드 (ALM) ##
### <a name="entering-alm"></a>ALM를 입력합니다. ###
Xbox, VES 지속적으로 수신 하지 음성 입력 합니다.  사용자는 하면 활성 수신 모드를 명시적으로 입력 해야 합니다.

- "Hey Cortana, 선택", 또는
- "Hey Cortana, 선택 항목으로 설정"

또한 "Hey Cortana에 로그인" 또는 "Hey Cortana, 홈으로 이동" 예를 들어 완료 되 면 활성 수신에서 사용자를 유지 하는 다른 여러 Cortana 명령이 있습니다. 

ALM을 입력 해야 같은 결과가 나타납니다.

- Cortana 오버레이에를 알리는 표시 되는 내용 말할 수는 오른쪽 위 모서리에 표시 됩니다.  사용자는 말하자면 동안 음성 인식기에서 인식 되는 구 조각은이 위치에 표시 됩니다.
- VE는 UIA 트리를 구문 분석, 실행 가능한 모든 컨트롤을 찾습니다, 그리고 음성 인식 문법에서 해당 텍스트를 등록 및 연속 수신 세션을 시작

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>기존 ALM ###
사용자가 음성을 사용 하 여 UI 상호 작용 하는 동안 시스템 ALM 유지 됩니다.  ALM을 종료 하려면 두 가지가 있습니다.

- 사용자 명시적으로, "stop" 이라고 표시 수신 대기, 또는
- 제한 시간을 양의 인식 ALM 입력 또는 마지막 양의 인식 이후 17 초 안에 없는 경우 발생 합니다.

## <a name="invoking-controls"></a>컨트롤 호출 ##
음성을 사용 하 여 UI 사용 하 여 사용자가 조작할 수는 ALM의 경우.  UI로 구성 된 경우 올바르게 (표시 되는 텍스트와 일치 하는 이름 속성), 음성을 사용 하 여 작업을 수행 하는 원활한, 자연 스러운 환경을 이어야 합니다.  사용자는 단순히 다음 화면에 표시 되는 내용에 있어야 합니다.

## <a name="overlay-ui-on-xbox"></a>Xbox에서 UI를 오버레이 합니다. ##
이름을 VE 컨트롤 UI에 표시 되는 실제 텍스트 보다 다를 수에 대 한 파생 됩니다.  이로 인해 수는 `Name` 또는 연결 된 컨트롤의 속성 `LabeledBy` 다른 문자열에 명시적으로 설정 되 고 요소가 있습니다.  또는 GUI 텍스트 있지만 아이콘 또는 이미지 요소를 컨트롤에 없습니다.

이러한 경우 사용자는 이러한 컨트롤을 호출 하기 위해 언급 되어야 하는 데 필요한 확인 하는 방법을 해야 합니다.  따라서 한 번 활성 수신 중, 음성 팁 표시 될 수 있습니다 "레이블을 표시 합니다." 라는 가정에서.  이렇게 하면 음성 설명 레이블을 실행 가능한 모든 컨트롤 맨 위에 표시 합니다.

제한은 100 레이블의 될 수 있도록 앱의 UI가 100 보다 조치 가능한 컨트롤에 있으면 일부는 음성 설명 레이블을 표시 합니다.  어떤 레이블이 선택이 경우 결정적이 지 않습니다, 구조 및 현재 UI 조합에 의존 하므로 UIA 트리에 처음으로 열거 합니다.

음성 설명 레이블을 숨기 거 명령이 표시 되 면 남지만 표시는 다음 이벤트 중 하나가 발생할 때까지:

- 사용자 컨트롤 호출
- 사용자가 현재 장면에서
- 사용자가 말하는 "수신 대기를 중지"
- 활성 수신 모드 시간 초과

## <a name="location-of-voice-tip-labels"></a>음성 팁 레이블의 위치 ##
음성 설명 레이블은 가로 및 세로로 가운데에 맞춰지고 컨트롤의 BoundingRectangle 합니다.  레이블을 수 컨트롤 작고 긴밀 하 게 그룹화 된 경우 중복/가려지고 사람이 및 VE 구분 하 고 표시 되는지를 확인 간격에 이러한 레이블을 푸시 하려고 합니다.  그러나 100%의 시간 동안 작동 하려면이 보장 하는 것은 없습니다.  것은 매우 복잡된 한 UI의 경우 다른 가려져 되 고 일부 레이블에 발생 합니다. "레이블 표시"를 사용 하 여 UI를 검토 하십시오 음성 팁 표시를 위한 적정 한 공간이 있는지 확인 합니다.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>콤보 상자 ##
콤보 상자의 콤보 상자에서 각 개별 항목에 확장 될 때 고유한 음성 설명 레이블을 가져오고 종종이 값은 드롭다운 목록 뒤의 기존 컨트롤을 기반으로 합니다.  (여기서 콤보 상자 항목 레이블은 혼합 된 콤보 상자 뒤의 컨트롤의 레이블을)으로 복잡 하 고 혼란 스러운 명확한 표시 레이블 표시 하지 않으려면 해당 자식 항목으로 표시 되는 대 한 콤보 상자 레이블만 확장 되 면  다른 모든 음성 설명 레이블은 숨겨집니다.  사용자 한 다음 드롭다운 목록 항목 중 하나를 선택 하거나 "닫기" 콤보 상자입니다.

- 축소 된 콤보 상자에 레이블:

    ![](images/ves_combo_closed.png)

- 확장 된 콤보 상자에 레이블:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>스크롤 가능한 컨트롤 ##
스크롤할 수 있는 컨트롤에 대 한 스크롤 명령에 대 한 음성 팁을 각 컨트롤의 가장자리에서 가운데 맞춤 될 됩니다.  음성 팁이 가능 하므로 예를 들어 "위로 스크롤하고" 및 "아래로 스크롤하여" 세로 스크롤을 사용할 수 없는 경우 표시 되지 것입니다 하는 스크롤 방향만 표시 됩니다.  VES 하 여 구분 (예: 서 수를 사용할지 스크롤할 수 있는 영역이 여러 개 있는 경우 “Scroll right 1”, “Scroll right 2”, etc.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>명확성 ##
여러 UI 요소 이름이 같은, 또는 음성 인식기에는 여러 후보 일치 하는 경우 VE 명확성 모드로 전환 됩니다.  이 모드 음성 팁 레이블 표시 됩니다 요소에 대 한 관련 된 사용자에 적합 한 선택할 수 있도록 합니다. 사용자를 "취소" 라는 가정에서 명확성 모드 취소할 수 있습니다.

예를 들어 다음과 같은 가치를 제공해야 합니다.

- 명확성; 하기 전에 수신 대기 하는 활성 모드에서는 사용자가 말하는 "저는 I 모호한":

    ![](images/ves_disambig1.png) 

- 두 단추 모두 일치 합니다. 명확성을 시작 합니다.

    ![](images/ves_disambig2.png) 

- 표시 된 "2 선택"를 선택한 경우 작업을 클릭 합니다.

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>샘플 UI ##
같습니다. 예가 XAML 기반 UI를 다양 한 방법으로 AutomationProperties.Name을 설정 합니다.

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


여기 위의 샘플을 사용 하 여 UI 모양을 음성 설명 레이블이 없는 됩니다.
 
- 활성 수신 모드에서 표시 하는 레이블 없이:

    ![](images/ves_alm_nolabels.png) 

- 활성 수신 모드에서는 사용자가 "레이블 표시"를 말하는 후:

    ![](images/ves_alm_labels.png) 

경우 `button1`, XAML 자동 채웁니다는 `AutomationProperties.Name` 텍스트 컨트롤의 표시 되는 텍스트 콘텐츠를 사용 하 여 속성입니다.  이 경우에 음성 설명 레이블을 명시적 없는 경우에 `AutomationProperties.Name` 설정 합니다.

사용 하 여 `button2`를 명시적으로 설정 합니다 `AutomationProperties.Name` 컨트롤의 텍스트 이외의 값으로.

사용 하 여 `comboBox`를 사용 했습니다를 `LabeledBy` 참조할 속성 `label1` 자동화의 소스로 `Name`, 및 `label1` 설정는 `AutomationProperties.Name` 화면에 렌더링할 대상을 보다 더 자연 스러운 구 하 ("Day of Week" 대신 보다 "Select Day of Week").

마지막으로, `button3`, VES grabs 합니다 `Name` 이후 첫 번째 자식 요소에서 `button3` 없기는 `AutomationProperties.Name` 설정 합니다.

## <a name="see-also"></a>참고 항목
- [UI 자동화 기본 사항](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "UI 자동화 기본 사항")
- [UI에서 내게 필요한 옵션 지원에 대 한 자동화 속성](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "UI에서 내게 필요한 옵션 지원에 대 한 자동화 속성")
- [질문과 대답](frequently-asked-questions.md)
- [Xbox One에서 UWP](index.md)
