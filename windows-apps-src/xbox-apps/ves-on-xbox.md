---
title: Xbox의 VES (Voice Enabled Shell)
description: Xbox의 UWP 앱에 음성 제어 지원을 추가 하는 방법에 대해 알아봅니다.
ms.date: 10/19/2017
ms.topic: article
keywords: windows 10, uwp, xbox, 음성, 음성 사용 셸
ms.openlocfilehash: db846e906917f29781200f3c312f6dbd6e2b2dd1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161667"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Speech를 사용 하 여 UI 요소 호출

VES (Voice Enabled Shell)는 응용 프로그램 내에서 최고 수준의 음성 환경을 가능 하 게 하는 Windows 음성 플랫폼의 확장으로, 사용자가 화면에 있는 컨트롤을 호출 하 고 받아쓰기를 통해 텍스트를 삽입할 수 있도록 합니다. VES는 응용 프로그램 개발자에 게 필요한 최소한의 노력으로 모든 Windows 셸 및 장치에서 일반적인 종단 간 it 환경을 제공 하기 위해 노력 하 고 있습니다.  이를 위해 Microsoft 음성 플랫폼과 UIA (UI 자동화) 프레임 워크를 활용 합니다.

## <a name="user-experience-walkthrough"></a>사용자 환경 연습 ##
다음은 Xbox에서 VES를 사용할 때 사용자가 경험할 수 있는 작업에 대 한 개요 이며, VES의 작동 방식에 대 한 세부 정보를 살펴보기 전에 컨텍스트를 설정 하는 데 도움이 됩니다.

- 사용자는 Xbox 본체를 켜고 앱을 탐색 하 여 관심 있는 항목을 찾습니다.

        User: "Hey Cortana, open My Games and Apps"

- 사용자는 ALM (활성 수신 모드)로 남아 있습니다. 즉, 콘솔에서 사용자가 화면에 표시 되는 컨트롤을 호출할 때까지 수신 대기 하 고 있습니다.  이제 사용자가 앱 보기로 전환 하 고 앱 목록을 스크롤할 수 있습니다.

        User: "applications"

- 보기를 스크롤하려면 사용자는 다음과 같은 작업을 수행 하면 됩니다.

        User: "scroll down"

- 사용자가 관심 있는 앱의 상자 아트를 볼 수 있지만 이름을 잊으셨나요?  사용자가 표시할 음성 팁 레이블을 요청 합니다.

        User: "show labels"

- 무엇 보다 분명 하 게 알 수 있게 되었으므로 앱을 시작할 수 있습니다.

        User: "movies and TV"

- 활성 수신 대기 모드를 종료 하기 위해 사용자는 Xbox에서 수신 대기를 중지 하도록 지시 합니다.

        User: "stop listening"

- 이후에는 다음을 사용 하 여 새 활성 수신 대기 세션을 시작할 수 있습니다.

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>UI 자동화 종속성 ##
VES는 UI 자동화 클라이언트 이며 UI 자동화 공급자를 통해 앱에서 노출 하는 정보를 기반으로 합니다. 이는 Windows 플랫폼의 내레이터 기능에서 이미 사용 하 고 있는 동일한 인프라입니다.  UI 자동화를 사용 하면 컨트롤 이름, 컨트롤의 형식 및 구현 하는 컨트롤 패턴을 비롯 하 여 사용자 인터페이스 요소에 프로그래밍 방식으로 액세스할 수 있습니다.  앱에서 UI가 변경 됨에 따라 VES는 UIA 업데이트 이벤트에 반응 하 고 업데이트 된 UI 자동화 트리를 다시 구문 분석 하 여 모든 실행 가능한 항목을 검색 하 고,이 정보를 사용 하 여 음성 인식 문법을 작성 합니다. 

모든 UWP 앱은 UI 자동화 프레임 워크에 액세스할 수 있으며, 작성 된 그래픽 프레임 워크 (XAML, DirectX/Direct3D, Xamarin 등)에 관계 없이 UI에 대 한 정보를 노출할 수 있습니다.  XAML과 같이 대부분의 많은 리프트는 프레임 워크에 의해 수행 되므로 내레이터 및 VES를 지 원하는 데 필요한 작업을 크게 줄일 수 있습니다.

UI 자동화에 대 한 자세한 내용은 [Ui 자동화 기본 사항](/dotnet/framework/ui-automation/ui-automation-fundamentals "UI 자동화 기본 사항")을 참조 하세요.

## <a name="control-invocation-name"></a>컨트롤 호출 이름 ##
VES는 다음 추론을 사용 하 여 음성 인식기에 컨트롤 이름으로 등록할 문구를 결정 합니다 (사용자가 컨트롤을 호출 하는 데 사용 해야 하는 항목).  이는 음성 팁 레이블에 표시 되는 문구 이기도 합니다.

우선 순위에 따라 이름 원본:

1. 요소에 연결 된 속성이 있는 경우 `LabeledBy` VES는 `AutomationProperties.Name` 이 텍스트 레이블의를 사용 합니다.
2. `AutomationProperties.Name` 요소의입니다.  XAML에서 컨트롤의 텍스트 콘텐츠가의 기본값으로 사용 됩니다 `AutomationProperties.Name` .
3. 컨트롤이 ListItem 또는 단추인 경우 VES는 유효한를 가진 첫 번째 자식 요소를 찾습니다 `AutomationProperties.Name` .

## <a name="actionable-controls"></a>실행 가능한 컨트롤 ##
VES는 다음과 같은 자동화 컨트롤 패턴 중 하나를 구현 하는 경우 컨트롤을 실행 가능한 것으로 간주 합니다.

- **InvokePattern** (예: Button)-명확한 단일 작업을 시작 하거나 수행 하 고 활성화 될 때 상태를 유지 하지 않는 컨트롤을 나타냅니다.

- **TogglePattern** (예: 확인란)-상태 집합을 순환 하 고 설정 된 상태를 유지할 수 있는 컨트롤을 나타냅니다.

- **은 selectionitempattern** (예: 콤보 상자)-선택 가능한 자식 항목의 컬렉션에 대 한 컨테이너 역할을 하는 컨트롤을 나타냅니다.

- **ExpandCollapsePattern** (예: 콤보 상자)-콘텐츠를 표시 하 고 축소 하 여 콘텐츠를 숨기도록 시각적으로 확장 되는 컨트롤을 나타냅니다.

- **ScrollPattern** (예: List)-자식 요소 컬렉션에 대해 스크롤 가능한 컨테이너 역할을 하는 컨트롤을 나타냅니다.

## <a name="scrollable-containers"></a>스크롤 가능한 컨테이너 ##
ScrollPattern를 지 원하는 스크롤 가능 컨테이너의 경우, VES는 "왼쪽 스크롤", "오른쪽으로 스크롤" 등과 같은 음성 명령을 수신 대기 하 고 사용자가 이러한 명령 중 하나를 트리거할 때 적절 한 매개 변수를 사용 하 여 Scroll을 호출 합니다.  스크롤 명령은 및 속성의 값에 따라 삽입 됩니다 `HorizontalScrollPercent` `VerticalScrollPercent` .  예를 들어 `HorizontalScrollPercent` 가 0 보다 큰 경우 "왼쪽으로 스크롤"이 추가 되 고, 100 보다 작은 경우 "오른쪽으로 스크롤"이 추가 됩니다.

## <a name="narrator-overlap"></a>내레이터가 겹칩니다. ##
또한 내레이터 응용 프로그램은 UI 자동화 클라이언트 이며, `AutomationProperties.Name` 현재 선택 된 UI 요소에 대해 읽는 텍스트의 원본 중 하나로 속성을 사용 합니다.  더 나은 액세스 가능성 경험을 제공 하기 위해 많은 앱 개발자 들이 `Name` 긴 설명 텍스트를 사용 하 여 속성을 오버 로드 하 고 내레이터에서 읽을 때 추가 정보 및 컨텍스트를 제공 하는 목표를 달성 했습니다.  그러나이로 인해 두 가지 기능이 충돌 합니다. 즉, VES는 컨트롤의 표시 되는 텍스트와 일치 하거나 거의 일치 하는 짧은 구를 필요로 하는 반면 내레이터는 더 긴 컨텍스트를 제공 하는 더 긴 설명 문구를 활용 합니다.

이 문제를 해결 하기 위해 Windows 10 크리에이터 업데이트부터 내레이터가 속성을 볼 수 있도록 업데이트 되었습니다 `AutomationProperties.HelpText` .  이 속성이 비어 있지 않으면 내레이터는 외에도 해당 내용을 말합니다 `AutomationProperties.Name` .  `HelpText`가 비어 있으면 내레이터는 이름 내용만 읽습니다.  이렇게 하면 필요한 경우 더 긴 설명 문자열을 사용할 수 있지만 속성에 짧은 음성 인식 친숙 한 구가 유지 됩니다 `Name` .

![](images/ves_narrator.jpg)

자세한 내용은 [UI의 내게 필요한 옵션 지원에 대 한 자동화 속성](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "UI의 내게 필요한 옵션 지원에 대 한 자동화 속성")을 참조 하세요.

## <a name="active-listening-mode-alm"></a>ALM (활성 수신 모드) ##
### <a name="entering-alm"></a>ALM 입력 ###
Xbox에서 VES는 음성 입력을 지속적으로 수신 대기 하지 않습니다.  사용자는 다음과 같이 명시적으로 활성 수신 대기 모드를 입력 해야 합니다.

- "안녕하세요. Cortana, select" 또는
- "Cortana, 선택 항목 만들기"

사용자가 완료 시 활성으로 수신 대기 중인 상태로 유지 되는 몇 가지 다른 Cortana 명령이 있습니다 (예: "안녕하세요 Cortana, 로그인" 또는 "안녕하세요 Cortana, go home"). 

ALM을 입력 하면 다음과 같은 결과가 나타납니다.

- Cortana 오버레이가 오른쪽 위에 표시 되 고 사용자에 게 표시 되는 내용을 표시 하는 메시지가 표시 됩니다.  사용자가 말하는 동안 음성 인식기에서 인식 하는 구 조각이이 위치에도 표시 됩니다.
- VES는 UIA 트리를 구문 분석 하 고, 모든 실행 가능한 컨트롤을 찾고, 음성 인식 문법에 텍스트를 등록 하 고, 연속 수신 대기 세션을 시작 합니다.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>ALM 종료 ###
사용자가 음성을 사용 하 여 UI와 상호 작용 하는 동안 시스템은 ALM에 유지 됩니다.  ALM을 종료 하는 방법에는 다음 두 가지가 있습니다.

- 사용자가 명시적으로 "수신 대기" 또는
- ALM 입력 또는 마지막 긍정 인식 이후 17 초 이내에 긍정 인식이 없는 경우 시간 초과가 발생 합니다.

## <a name="invoking-controls"></a>컨트롤 호출 ##
ALM에서 사용자는 음성을 사용 하 여 UI와 상호 작용할 수 있습니다.  UI가 표시 되는 텍스트와 일치 하는 이름 속성을 사용 하 여 올바르게 구성 된 경우 음성을 사용 하 여 작업을 수행 해야 합니다.  사용자는 화면에 표시 되는 내용만 표시 하면 됩니다.

## <a name="overlay-ui-on-xbox"></a>Xbox의 오버레이 UI ##
컨트롤에 대해 VES 파생 이름은 UI의 실제 표시 되는 텍스트와 다를 수 있습니다.  이는 `Name` 컨트롤의 속성 또는 `LabeledBy` 명시적으로 다른 문자열로 설정 되는 연결 된 요소 때문일 수 있습니다.  또는 컨트롤에 GUI 텍스트가 없지만 아이콘 또는 이미지 요소만 포함 됩니다.

이러한 경우 사용자는 이러한 컨트롤을 호출 하는 데 필요한 항목을 확인 하는 방법이 필요 합니다.  따라서 활성 수신 중에는 "레이블 표시"를 통해 음성 팁을 표시할 수 있습니다.  그러면 음성 팁 레이블이 실행 가능한 모든 컨트롤의 맨 위에 표시 됩니다.

100 개의 레이블이 있습니다. 따라서 앱의 UI가 100 보다 더 많은 실행 가능한 컨트롤을 포함 하는 경우에는 음성 팁 레이블이 표시 되지 않습니다.  이 경우에 선택 되는 레이블은 UIA 트리에서 첫 번째 열거 된 현재 UI의 구조 및 컴퍼지션에 따라 달라 지기 때문에 명확 하지 않습니다.

음성 팁 레이블이 표시 되 면 다음 이벤트 중 하나가 발생할 때까지 표시 되는 명령이 없습니다.

- 사용자가 컨트롤 호출
- 사용자가 현재 장면에서 다른 곳으로 이동
- 사용자가 "수신 중지" 라고 표시 함
- 활성 수신 대기 모드 제한 시간

## <a name="location-of-voice-tip-labels"></a>음성 팁 레이블 위치 ##
음성 팁 레이블은 컨트롤의 BoundingRectangle 내에서 가로 및 세로 방향으로 가운데 맞춤 됩니다.  컨트롤이 작고 긴밀 하 게 그룹화 되 면 다른 사용자가 레이블을 겹치거나 숨길 수 있으며, VES는 이러한 레이블을 분리 하 여 표시 하 고 표시 되도록 합니다.  그러나이는 100%의 시간 동안 작동 하지 않을 수 있습니다.  매우 복잡 한 UI가 있는 경우 일부 레이블이 다른 레이블로 가려져 있을 수 있습니다. "레이블 표시"를 사용 하 여 UI를 검토 하 여 음성 팁 표시 유형을 위한 충분 한 공간이 있는지 확인 하세요.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>콤보 상자 ##
콤보 상자를 확장 하면 콤보 상자의 각 개별 항목에 고유한 음성 팁 레이블이 표시 되 고 일반적으로 드롭다운 목록 뒤에 있는 기존 컨트롤의 위쪽에 표시 됩니다.  콤보 상자 항목이 확장 될 때 콤보 상자 항목 레이블이 콤보 상자 뒤의 컨트롤 레이블과 결합 되는 muddle 복잡 하 고 복잡 한 레이블 표시를 방지 하기 위해 자식 항목의 레이블만 표시 됩니다.  다른 모든 음성 팁 레이블은 숨겨집니다.  그런 다음 사용자는 드롭다운 항목 중 하나를 선택 하거나 콤보 상자에서 "close"를 선택할 수 있습니다.

- 축소 된 콤보 상자의 레이블:

    ![](images/ves_combo_closed.png)

- 확장 된 콤보 상자의 레이블:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>스크롤 가능 컨트롤 ##
스크롤할 수 있는 컨트롤의 경우 스크롤 명령에 대 한 음성 팁이 컨트롤의 각 가장자리를 중심으로 합니다.  음성 팁은 실행 가능한 스크롤 방향에 대해서만 표시 됩니다. 예를 들어 세로 스크롤을 사용할 수 없는 경우 "위로 스크롤" 및 "아래로 스크롤"은 표시 되지 않습니다.  스크롤할 수 있는 여러 영역이 있는 경우 VES는 서 수를 구분 하는 데 서 수를 사용 합니다 (예: "오른쪽으로 스크롤", "오른쪽으로 스크롤" 등이 있습니다.

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>명확성 ##
여러 UI 요소의 이름이 같거나 음성 인식기가 여러 후보와 일치 하는 경우 VES는 명확성 모드로 전환 됩니다.  이 모드에서 사용자가 올바른 항목을 선택할 수 있도록 관련 된 요소에 대해 음성 팁 레이블이 표시 됩니다. 사용자는 "취소"를 말하여 명확성 모드를 취소할 수 있습니다.

예:

- 활성 수신 대기 모드에서의 명확성 사용자에 게 "는 모호 합니다." 라고 표시 됩니다.

    ![](images/ves_disambig1.png) 

- 두 단추가 일치 합니다. 명확성 시작:

    ![](images/ves_disambig2.png) 

- "Select 2"가 선택 된 경우 클릭 동작 표시:

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>샘플 UI ##
다음은 다양 한 방법으로 AutomationProperties.Name를 설정 하는 XAML 기반 UI의 예제입니다.

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


위의 샘플을 사용 하면 UI가 음성 팁 레이블 없이 표시 됩니다.
 
- 활성 수신 대기 모드에서 레이블이 표시 되지 않습니다.

    ![](images/ves_alm_nolabels.png) 

- 활성 수신 대기 모드에서 사용자는 "레이블 표시"를 표시 합니다.

    ![](images/ves_alm_labels.png) 

의 경우 XAML은 `button1` `AutomationProperties.Name` 컨트롤의 표시 되는 텍스트 콘텐츠에서 텍스트를 사용 하 여 속성을 자동으로 채웁니다.  명시적인 집합이 없더라도 음성 팁 레이블이 있는 이유입니다 `AutomationProperties.Name` .

를 사용 하면를 `button2` `AutomationProperties.Name` 컨트롤의 텍스트 이외의 항목으로 명시적으로 설정 합니다.

에서는 `comboBox` 속성을 사용 하 여를 `LabeledBy` `label1` 자동화의 원본으로 참조 하 `Name` 고,에서는 `label1` `AutomationProperties.Name` 화면에 렌더링 되는 것 보다 더 자연 스러운 구로를 설정 합니다 ("요일 선택"이 아닌 "요일").

마지막으로,를 사용 하 여 `button3` VES는 `Name` 첫 번째 자식 요소에서를 가져와 합니다 .이는 자체에 집합이 없기 때문 `button3` `AutomationProperties.Name` 입니다.

## <a name="see-also"></a>참고 항목
- [UI 자동화 기본 사항](/dotnet/framework/ui-automation/ui-automation-fundamentals "UI 자동화 기본 사항")
- [UI의 내게 필요한 옵션 지원에 대 한 자동화 속성](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "UI의 내게 필요한 옵션 지원에 대 한 자동화 속성")
- [자주 묻는 질문](frequently-asked-questions.md)
- [Xbox One의 UWP](index.md)