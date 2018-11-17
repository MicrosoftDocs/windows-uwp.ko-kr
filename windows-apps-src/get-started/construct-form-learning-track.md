---
author: QuinnRadich
title: 학습 트랙 - 양식 생성 및 구성
description: 앱에서 강력한 양식을 만들기 위해 필요한 것에 대해 알아봅니다.
ms.author: quradic
ms.date: 05/07/2018
ms.topic: article
keywords: 시작, uwp, windows 10, 학습 트랙, 레이아웃, 양식
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c624a3c666dcc405ee2375738c605ae147b7d9d3
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7151873"
---
# <a name="create-and-customize-a-form"></a>양식 만들기 및 사용자 지정

사용자가 상당한 양의 정보를 입력해야 하는 앱을 만드는 경우 그들이 작성할 양식을 만들려는 가능성이 큽니다. 이 문서는 유용하고 강력한 양식을 만들기 위해 알아야 내용을 보여 줍니다.

이 문서는 자습서가 아닙니다. 자습서를 원하는 경우 단계별 안내식 환경을 제공하는 우리의 [적응 레이아웃 자습서](../design/basics/xaml-basics-adaptive-layout.md)를 참조하세요.

양식에 들어갈 **XAML 컨트롤**, 페이지에서 이를 가장 잘 정렬하는 방법 및 화면 크기를 변경하기 위해 양식을 최적화하는 방법을 설명합니다. 하지만 양식은 시각적 요소의 상대 위치에 대한 것이기 때문에 먼저 XAML이 있는 페이지 레이아웃을 살펴보겠습니다.

## <a name="what-do-you-need-to-know"></a>알아야 할 사항?

UWP는 앱에 추가하고 구성할 수 있는 명시적 양식 컨트롤이 없습니다. 대신, 페이지에서 UI 요소 컬렉션을 정렬하여 양식을 만들어야 합니다.

이렇게 하기 위해 **레이아웃 패널**을 파악해야 합니다. 레이아웃 패널은 앱의 UI 요소를 포함하고 정렬 및 그룹화할 수 있는 컨테이너입니다. 다른 레이아웃 패널 내에 레이아웃 패널을 배치하면 각 항목 사이에 대해 항목을 표시할 위치와 방법에 대한 상당한 제어를 제공합니다. 또한 앱이 훨씬 더 쉽게 화면 크기를 변경할 수 있게 조정할 수 있습니다.

[레이아웃 패널에 대한 설명서](../design/layout/layout-panels.md)를 읽어 보세요. 양식은 일반적으로 하나 이상의 세로 열에서 표시되기 때문에 유사한 항목을 **StackPanel**에 그룹화하고, 필요한 경우 **RelativePanel** 내에 정렬할 수 있습니다. 이제 일부 패널을 결합하기 시작합니다. 참조가 필요한 경우 아래에 2열 양식에 대한 기본 레이아웃 프레임워크가 표시됩니다.

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Customer">
        <!--Save and Cancel buttons-->
    </StackPanel>
</RelativePanel>
```

## <a name="what-goes-in-a-form"></a>양식에 무엇이 들어가나요?

다양한 [XAML 컨트롤](../design/controls-and-patterns/controls-and-events-intro.md)로 양식을 작성해야 합니다. 이러한 컨트롤에 익숙할 수도 있지만 언제든지 필요한 경우 세부 정보를 읽을 수 있습니다. 특히, 사용자가 텍스트를 입력하거나 값 목록에서 선택할 수 있는 컨트롤이 필요할 수 있습니다. 이 기본 목록을 추가할 수 있는 옵션 – 읽을,에 대 한 모든 충분히 어떤 모습와 그 작동 방식을 이해 필요가 없습니다.

* [TextBox](../design/controls-and-patterns/text-box.md) 를 앱에 사용자 입력된 텍스트를 수 있습니다.
* [ToggleSwitch](../design/controls-and-patterns/toggles.md)는 사용자가 두 가지 옵션 중에서 선택할 수 있도록 합니다.
* [DatePicker](../design/controls-and-patterns/date-picker.md)는 날짜 값을 선택할 수 있도록 합니다.
* [TimePicker](../design/controls-and-patterns/time-picker.md)는 시간 값을 선택할 수 있도록 합니다.
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)는 선택할 수 있는 항목의 목록을 표시하도록 확장합니다. [여기](../design/controls-and-patterns/lists.md#drop-down-lists)에서 자세한 내용을 알아볼 수 있습니다.

사용자가 저장하거나 취소할 수 있도록 [단추](../design/controls-and-patterns/buttons.md)를 추가하고자 할 수도 있습니다.

## <a name="format-controls-in-your-layout"></a>레이아웃의 서식 컨트롤

레이아웃 패널을 정렬하는 방법을 알고 추가하려는 항목이 있지만 어떻게 서식을 지정해야 할까요? [양식](../design/controls-and-patterns/forms.md) 페이지에는 몇 가지 특정 디자인 지침이 있습니다. 유용한 정보를 위해 **양식 유형** 및 **레이아웃**의 섹션을 읽어 보세요. 접근성 및 상대적 레이아웃은 곧 더 자세히 설명하겠습니다.

이를 염두에 두고, 레이아웃에 컨트롤 추가를 시작하고 주어진 레이블을 적절히 지정하고 배치하는지 확인해야 합니다. 예를 들어 위의 레이아웃, 컨트롤 및 디자인 지침을 사용하는 단일 페이지 양식에 대한 골자는 다음과 같습니다.

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" HorizontalAlignment="Left" />
            <RelativePanel>
                <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" HorizontalAlignment="Left" />
                <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0" RelativePanel.RightOf="City">
                    <!--List of valid states-->
                </ComboBox>
            </RelativePanel>
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.Below="Customer">
        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Associate">
        <Button Content="Save" Margin="24" />
        <Button Content="Cancel" Margin="24" />
    </StackPanel>
</RelativePanel>
```

시각적 환경 개선을 위해 더 많은 속성이 있는 컨트롤을 사용자 지정할 수 있습니다.

## <a name="make-your-layout-responsive"></a>반응형 레이아웃 만들기

사용자는 서로 다른 화면 폭의 다양한 장치에서 UI를 볼 수 있습니다. 화면에 관계없이 좋은 환경을 제공하기 위해 [반응형 디자인](../design/layout/responsive-design.md)을 사용해야 합니다. 진행하면서 유의해야 한 디자인 철학에 대한 좋은 조언을 해당 페이지를 통해 읽어보세요.

[XAML을 사용한 반응형 레이아웃](../design/layout/layouts-with-xaml.md) 페이지에서는 이를 구현하는 방법을 자세히 설명합니다. 지금은 **유동 레이아웃** 및 **XAML에서 시각적 상태**에 집중합니다.

준비한 기본 양식 개요는 특정 픽셀 크기와 위치를 최소로 사용하는 상대 컨트롤 위치를 따르므로 이미 **유동 레이아웃**입니다. 그렇지만 추후에 만들 수 있는 추가 UI를 위해 이 지침을 염두에 두세요.

반응형 레이아웃에 더 중요한 것은 **시각적 상태**입니다. 시각적 상태는 주어진 조건이 참일 때 주어진 요소에 적용되는 속성 값을 정의합니다. [xaml에서 이 작업을 수행하는 방법을 읽고](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup), 이를 양식에 구현합니다. *매우* 기본적인 양식은 이전 샘플에서 다음과 같이 보일 수 있습니다.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="640" />
            </VisualState.StateTriggers>

            <VisualState.Setters>
                <Setter Target="Associate.(RelativePanel.RightOf)" Value="Customer"/>
                <Setter Target="Associate.(RelativePanel.Below)" Value=""/>
                <Setter Target="Save.(RelativePanel.Below)" Value="Customer"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

<RelativePanel>
    <!--Previous 3 stack panels-->
</RelativePanel>
```

다양한 화면 크기의 시각적 상태를 만드는 것은 실용적이지 않으며 두 개 이상의 화면은 앱의 사용자 환경에 큰 영향을 줄 수 있습니다. 대신 몇 가지 주요 중단점에 맞춰 디자인하는 것이 좋습니다. [여기에서 더 자세히 알아볼 수 있습니다](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="add-accessibility-support"></a>추가 접근성 지원

화면 크기의 변경에 대응하는 잘 생성된 레이아웃이 있으므로 사용자 환경을 개선하기 위해 할 수 있는 마지막 작업은 [앱에 액세스할 수 있도록](../design/accessibility/accessibility-overview.md) 만드는 것입니다. 여러 방법을 사용할 수 있지만 이와 같은 양식에서는 보이는 것보다 쉽습니다. 다음에 중점을 둡니다.

* 키보드 지원 - UI 패널에서 요소의 순서가 화면에 표시되는 방식과 어떻게 일치하는지 확인하여 사용자가 이 사이를 쉽게 이동할 수 있도록 합니다.
* 화면 읽기 프로그램 지원 - 모든 컨트롤을 설명하는 이름이 있는지 확인합니다.

더 많은 시각적 요소의 더 복잡한 레이아웃을 만드는 경우 자세한 내용은 [접근성 검사 목록](../design/accessibility/accessibility-checklist.md)를 참조하세요. 결국 앱에 접근성이 필요 없지만 더 많은 고객을 유치하고 참여하게 만드는 데 도움이 됩니다.

## <a name="going-further"></a>더 나아가기

여기에서 양식을 만들었지만 레이아웃과 컨트롤의 개념은 생성할 수 있는 모든 XAML UI 전반에서 적용됩니다. 자유롭게 새로운 UI 기능을 추가 하 고 사용자 환경을 구체화 있는 양식을 시험해를 연결한 문서를 통해 다시 이동할 수 있습니다. 더 자세한 레이아웃 기능을 통해 단계별 지침을 원한다 면 우리의 [적응형 레이아웃 자습서](../design/basics/xaml-basics-adaptive-layout.md) 를 참조 하십시오.

양식은 비워둘 필요도 없습니다. 한 발자국 나아가 [마스터/세부 정보 패턴](../design/controls-and-patterns/master-details.md) 또는 [피벗 컨트롤](../design/controls-and-patterns/tabs-pivot.md) 내에 자신의 양식을 채울 수 있습니다. 또는 양식의 코드 뒤에서 작업하려는 경우 우리의 [이벤트 개요](../xaml-platform/events-and-routed-events-overview.md)로 시작할 수 있습니다.

## <a name="useful-apis-and-docs"></a>유용한 API 및 문서

여기에 데이터 바인딩을 시작하는 데 도움이 되는 API의 빠른 요약 및 다른 유용한 문서가 제공됩니다.

### <a name="useful-apis"></a>유용한 API

| API | 설명 |
|------|---------------|
| [양식에 유용한 컨트롤](../design/controls-and-patterns/forms.md#input-controls) | 양식을 만드는 데 유용한 입력 컨트롤 목록과 이 컨트롤을 어디에 사용할지에 대한 기본 지침입니다. |
| [그리드](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | 다중 행/열 레이아웃의 요소를 배열하는 패널입니다. |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | 다른 요소 및 패널의 경계와 관련하여 항목을 배열하는 패널입니다. |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | 가로나 세로줄로 요소를 배열하는 패널입니다. |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | 특정 상태일 때 UI 요소의 모양을 설정할 수 있습니다. |

### <a name="useful-docs"></a>유용한 문서

| 항목 | 설명 |
|-------|----------------|
| [접근성 개요](../design/accessibility/accessibility-overview.md) | 앱에서 접근성 옵션에 대한 광범위한 개요입니다. |
| [접근성 검사 목록](../design/accessibility/accessibility-checklist.md) | 앱이 접근성 표준을 준수하는지 확인하기 위한 실용적인 검사 목록입니다. |
| [이벤트 개요](../xaml-platform/events-and-routed-events-overview.md) | UI 작업을 처리하기 위한 이벤트 추가 및 구조화에 대해 자세히 설명합니다. |
| [양식](../design/controls-and-patterns/forms.md) | 양식 작성에 대한 전반적인 지침입니다. |
| [레이아웃 패널](../design/layout/layout-panels.md) | 레이아웃 패널의 유형과 용도에 대한 개요를 제공합니다. |
| [마스터/세부 정보 패턴](../design/controls-and-patterns/master-details.md) | 하나 또는 여러 개의 양식 주변에 구현할 수 있는 디자인 패턴입니다. |
| [피벗 컨트롤](../design/controls-and-patterns/tabs-pivot.md) | 하나 또는 여러 개의 양식을 포함할 수 있는 컨트롤. |
| [반응형 디자인](../design/layout/responsive-design.md) | 대규모 반응형 디자인 원칙에 대한 개요입니다. | 
| [XAML을 사용한 반응형 레이아웃](../design/layout/layouts-with-xaml.md) | 반응형 디자인의 시각적 상태 및 기타 구현에 대한 정보입니다. |
| [반응형 디자인의 화면 크기](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | 반응형 레이아웃의 범위여야 하는 화면 크기에 대한 지침입니다. |

## <a name="useful-code-samples"></a>유용한 코드 샘플

| 코드 샘플 | 설명 |
|-----------------|---------------|
| [적응형 레이아웃 자습서](../design/basics/xaml-basics-adaptive-layout.md) | 적응형 레이아웃 및 반응형 디자인을 단계별로 안내하는 경험입니다. | 
| [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | 여러 페이지 엔터프라이즈 샘플에서 작업 중인 레이아웃 및 양식을 표시합니다. |
| [XAML 컨트롤 갤러리](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | XAML 컨트롤 선택 및 구현 방법을 확인합니다. |
| [추가 코드 샘플](https://developer.microsoft.com//windows/samples) | 관련된 코드를 표시하려면 범주 드롭다운 목록에서 **컨트롤, 레이아웃 및 텍스트**를 선택합니다. |
