---
Description: 키보드 고급 사용자와 장애 및 기타 접근성 요구 사항이 있는 사용자에게 가능한 최고의 환경을 제공하도록 UWP 앱을 디자인하고 최적화하는 방법을 알아보세요.
title: 키보드 조작
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: 키보드, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 조작, 게임 패드, 원격
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: eef1c3dd50f06d38023102122cc7fc63a36df25c
ms.sourcegitcommit: 3ec8c1d0ea3798cdb2745e7a941a025cf3cf21c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650790"
---
# <a name="keyboard-interactions"></a>키보드 조작

![키보드 영웅 이미지](images/keyboard/keyboard-hero.jpg)

키보드 고급 사용자와 장애 및 기타 접근성 요구 사항이 있는 사용자에게 가능한 최고의 환경을 제공하도록 UWP 앱을 디자인하고 최적화하는 방법을 알아보세요.

모든 디바이스에서 키보드 입력은 전체적인 UWP(유니버설 Windows 플랫폼) 조작 환경의 중요한 부분입니다. 키보드 환경이 적절하게 설계되면 사용자가 키보드에서 손을 떼지 않고도 앱 UI를 효율적으로 탐색하고 전체 기능을 사용할 수 있습니다.

![키보드 및 게임 패드 이미지](images/keyboard/keyboard-gamepad.jpg)

***일반적인 상호 작용 패턴 키보드와 gamepad 간에 공유 됩니다.***

이 항목에서는 PC의 키보드 입력에 대한 UWP 앱 디자인을 중점적으로 살펴보겠습니다. 그러나는 잘 설계 된 키보드 환경은 Windows 내레이터와 같은 내게 필요한 옵션 도구를 지원 하기 위해 필요를 사용 하 여 [소프트웨어 키보드](#software-keyboard) 다른 처리, 터치 키보드 및 화상 키보드 (OSK), 등 원격 제어 및 Xbox 게임 패드와 같은 장치 유형을 입력 합니다.

[포커스 화면 효과](#focus-visuals), [선택키](#access-keys), [UI 탐색](#navigation)을 포함하여 여기서 다루는 여러 지침과 권장 사항은 다른 시나리오에도 적용할 수 있습니다.

**참고** 텍스트 입력에 하드웨어 키보드와 소프트웨어 키보드가 모두 사용되지만 이 항목의 초점은 탐색과 조작입니다.

## <a name="built-in-support"></a>기본 지원

키보드는 마우스와 함께 PC에서 가장 광범위하게 사용되는 주변 장치이며, 따라서 PC 환경에서 핵심적인 부분입니다. PC 사용자는 키보드 입력에 대한 응답으로 두 시스템과 개별 앱에서 포괄적이고 일관적인 환경을 기대합니다.

모든 UWP 컨트롤은 풍부한 키보드 환경 및 사용자 조작을 위한 기본 지원을 포함하고 있으며, 플랫폼은 그 자체로 사용자 지정 컨트롤 및 앱에 가장 적합한 키보드 환경을 만들 수 있는 방대한 기반을 제공합니다.

![키보드와 전화 이미지](images/keyboard/keyboard-phone.jpg)

***모든 장치를 사용 하 여 UWP 지원 키보드***

## <a name="basic-experiences"></a>기본 환경
![포커스 기반 디바이스](images/keyboard/focus-based-devices.jpg)

앞서 언급했듯이, Xbox 게임 패드 및 리모컨 같은 입력 디바이스와 내레이터 같은 접근성 도구는 탐색과 명령에 사용되는 키보드 입력 환경의 상당 부분을 공유합니다. 이처럼 여러 입력 유형 및 도구에서 공통 환경을 공유하기 때문에 추가 작업이 최소화되고 유니버설 Windows 플랫폼의 "한 번 빌드하여 어디서나 실행" 목표에도 부합합니다.

필요한 경우 주요 차이점에 주의 고려해 야 하는 모든 완화 설명 식별 됩니다.

다음은 이 항목에서 다룰 디바이스와 도구입니다.

| 디바이스/도구                       | 설명     |
|-----------------------------------|-----------------|
|키보드(하드웨어 및 소프트웨어)   |UWP 응용 프로그램을 표준 하드웨어 키보드를 외에도 두 개의 소프트웨어 키보드 지원: 합니다 [터치 (또는 소프트웨어) 키보드](#software-keyboard) 하며 [화상 키보드](#on-screen-keyboard)합니다.|
|게임 패드 및 리모컨         |Xbox 게임 패드 및 리모컨은 [3m 환경](../devices/designing-for-tv.md)의 핵심 입력 디바이스입니다. UWP의 게임 패드 및 리모컨 지원에 대한 자세한 내용은 [게임 패드 및 리모컨 조작](gamepad-and-remote-interactions.md)을 참조하세요.|
|화면 읽기 프로그램(내레이터)          |내레이터는 고유한 조작 환경과 기능을 제공하는 Windows의 기본 화면 읽기 프로그램이지만 여전히 기본 키보드 탐색 및 입력을 사용합니다. 내레이터에 대한 자세한 내용은 [내레이터 시작](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)을 참조하세요.|

## <a name="custom-experiences-and-efficient-keyboarding"></a>사용자 지정 환경 및 효율적인 키보드 조작
앞서 언급했듯이, 키보드 지원은 기술, 능력, 기대치가 각기 다른 사용자들이 응용 프로그램을 원활하게 조작하기 위한 필수 요소입니다. 다음 사항을 우선적으로 처리할 것을 권장합니다.
- 키보드 탐색 및 조작 지원
    - 실행 가능한 항목은 탭 정지로 식별하고(실행 가능하지 않은 항목은 탭 정지로 식별하지 말고), 탐색 명령은 논리적이고 예측 가능해야 함([탭 정지](#tab-stops) 참조)
    - 가장 논리적인 요소에 초기 포커스 설정([초기 포커스](#initial-focus) 참조)
    - "내부 탐색"에 화살표 키 탐색 제공([탐색](#navigation) 참조)
- 키보드 바로 가기 지원
    - 바로 가기를 위한 바로 가기 키 제공([바로 가기](#accelerators) 참조)
    - 응용 프로그램의 UI를 탐색 하려면 액세스 키를 제공 (참조 [선택키가](access-keys.md))

### <a name="focus-visuals"></a>포커스 시각적 개체

UWP는 모든 입력 유형과 환경에서 원활하게 작동하는 하나의 포커스 화면 효과 디자인을 지원합니다.
![포커스 시각적 개체](images/keyboard/focus-visual.png)

포커스 화면 효과의 특징:

- UI 요소가 키보드 및/또는 게임 패드/리모컨으로부터 포커스를 받으면 포커스 화면 효과가 표시됩니다.
- UI 요소 주변에 강조 표시된 테두리로 렌더링되어 작업을 수행할 수 있음을 나타냅니다.
- 사용자가 헤매지 않고 앱 UI를 쉽게 탐색할 수 있도록 도와줍니다.
- 앱에 대해 사용자 지정할 수 있습니다([높은 가시성 포커스 화면 효과](guidelines-for-visualfeedback.md#high-visibility-focus-visuals) 참조).

**참고** UWP 포커스 화면 효과는 내레이터 포커스 사각형과 다릅니다.

### <a name="tab-stops"></a>탭 정지

키보드에서 컨트롤(탐색 요소 포함)을 사용하려면 컨트롤에 포커스가 있어야 합니다. 키보드 포커스를 받을 컨트롤에 대 한 가지 방법은 탭으로 식별 하 여 탭 탐색을 통해 액세스할 수 있도록 하는 응용 프로그램의 탭 순서에 중지 합니다.

탭 순서에 포함 될 컨트롤을 [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) 속성으로 설정 되어 있어야 **true** 및 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성 설정 해야 합니다 **true**.

컨트롤 탭 순서를에서 특별히 제외를 설정 합니다 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성을 **false**합니다.

기본적으로 탭 순서는 UI 요소가 생성된 순서를 반영합니다. 예를 들어 `StackPanel`에 `Button`, `Checkbox` 및 `TextBox`가 포함된 경우 탭 순서는 `Button`, `Checkbox`, `TextBox`입니다.

기본 탭 순서를 설정 하 여 재정의할 수 있습니다 합니다 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성입니다.

#### <a name="tab-order-should-be-logical-and-predictable"></a>탭 순서는 논리적이고 예측 가능해야 함

논리적이고 예측 가능한 탭 순서를 사용하여 키보드 탐색 모델을 적절하게 디자인하면 앱의 직관성이 향상되고 사용자가 좀 더 효율적으로 기능을 탐색, 검색 및 액세스할 수 있습니다.

모든 대화형 컨트롤에 탭 정지 해야 합니다. (에 있지 않는 한를 [그룹](#control-group)), 레이블 등의 비 대화형 컨트롤 하는 동안 안 됩니다.

포커스가 응용 프로그램 내에서 무질서하게 이동하도록 사용자 지정 탭 순서를 지정하면 안 됩니다. 예를 들어 양식의 컨트롤 목록은 탭 순서가 위에서 아래로, 왼쪽에서 오른쪽으로 흘러야 합니다(로캘에 따라 다름).

참조 [키보드 접근성](../accessibility/keyboard-accessibility.md) 탭 정지를 사용자 지정 하는 방법에 대 한 자세한 내용은 합니다.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>탭 정지 및 시각적 순서를 조정

탭 순서와 시각적 순서 (순서나 표시 순서를 읽고이 라고도 함) 조정 응용 프로그램의 UI를 통해 탐색 하는 동안 사용자의 혼동을 줄일 수 있습니다.

명령, 컨트롤 및 콘텐츠의 순위를 정하고 탭 순서와 시각적 순서에서 가장 중요한 항목을 맨 처음에 제공하도록 노력합니다. 그러나 실제 표시 위치는 부모 레이아웃 컨테이너 및 레이아웃에 영향을 주는 자식 요소의 특정 속성에 따라 달라질 수 있습니다. 특히, 그리드 메타포 또는 테이블 메타포를 사용하는 레이아웃은 시각적 순서가 탭 순서와 상당히 다를 수 있습니다.

**참고** 시각적 순서는 로케일 및 언어에 따라서도 달라집니다.

### <a name="initial-focus"></a>초기 포커스

초기 포커스는 응용 프로그램 또는 페이지가 처음으로 시작되거나 활성화될 때 UI 요소에서 받는 포커스를 지정합니다. 키보드를 사용 하면 사용자가 응용 프로그램의 UI를 사용 하 여 상호 작용을 시작 하는이 요소에서 것입니다.

UWP 앱에 대 한 최고 요소에 초기 포커스를 설정할지 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 포커스를 받을 수는 있습니다. 컨테이너 컨트롤의 자식 요소는 무시됩니다. 타일에서는 시각적 트리의 첫 번째 요소가 포커스를 받습니다.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>가장 논리적인 요소에 초기 포커스 설정

사용자가 앱을 시작하거나 페이지를 탐색할 때 수행할 가능성이 가장 높은 첫 번째 또는 기본 작업에 대한 UI 요소에 초기 포커스를 설정합니다. 예를 들면 다음과 같습니다.
-   갤러리의 첫 번째 항목에 포커스가 설정되는 사진 앱
-   재생 단추에 포커스가 설정되는 음악 앱

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>잠재적으로 음수 이거나 초래할 수도 결과 노출 하는 요소에 초기 포커스를 설정 하지 마세요

이 수준의 기능을 사용자의 선택 해야 합니다. 중요한 결과가 포함된 요소에 초기 포커스를 설정하면 의도치 않은 데이터 손실이나 시스템 액세스로 이어질 수 있습니다. 예를 들어, 설정 하지 않습니다 포커스 삭제 단추에 전자 메일을 이동할 때.

참조 [탐색 집중](focus-navigation.md) 탭 순서를 재정의 하는 방법에 대 한 자세한 내용은 합니다.

### <a name="navigation"></a>탐색

키보드 탐색은 일반적으로 Tab 키와 화살표 키를 통해 지원됩니다.

![Tab 및 화살표 키](images/keyboard/tab-and-arrow.png)

기본적으로 UWP 컨트롤은 다음과 같은 기본 키보드 동작을 따릅니다.
-   **Tab 키** - 탭 순서에 따라 실행 가능/활성 컨트롤을 탐색합니다.
-   **Shift + Tab** - 탭 순서의 역방향으로 컨트롤을 탐색합니다. 사용자가 화살표 키를 사용하여 컨트롤 내부를 탐색한 경우 컨트롤 내부의 마지막으로 알려진 값에 포커스가 설정됩니다.
-   **화살표 키** - 컨트롤 관련 "내부 탐색"을 노출합니다. 사용자가 "내부 탐색"을 입력하면 화살표 키가 컨트롤 외부를 탐색하지 않습니다. 예를 들면 다음과 같습니다.
    -   위쪽/아래쪽 화살표 키 내에서 포커스를 이동 하는 `ListView` 및 `MenuFlyout`
    -   현재 선택 된 값을 수정한 `Slider` 및 `RatingsControl`
    -   내에서 캐럿 이동 `TextBox`
    -   항목 내에서 확대/축소 합니다. `TreeView`

응용 프로그램의 키보드 탐색을 최적화 하기 위해 이러한 기본 동작을 사용 합니다.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>관련 컨트롤 집합에 "내부 탐색" 사용

전체 응용 프로그램의 UI 조직 내에서 관계를 강화 관련된 컨트롤의 집합으로 화살표 키 탐색을 제공 합니다.

예를 들어 여기에 표시된 `ContentDialog` 컨트롤은 기본적으로 가로 행 단추에 대한 내부 탐색을 제공합니다(사용자 지정 컨트롤에 대한 내용은 [컨트롤 그룹](#control-group) 섹션 참조).

![대화 상자 예제](images/keyboard/dialog.png)

***상호 관련 된 단추 컬렉션 화살표 키 탐색을 사용 하 여 쉽게 이루어집니다.***

항목이 한 열에 표시되는 경우 위쪽/아래쪽 화살표 키가 항목을 탐색합니다. 항목이 한 행에 표시되는 경우 왼쪽/오른쪽 화살표 키가 항목을 탐색합니다. 항목이 여러 열인 경우 4개 화살표 키가 항목을 탐색합니다.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>관련된 컨트롤의 컬렉션에 대 한 단일 탭 정지를 정의 합니다.

관련 된 또는 보완 하는 컨트롤의 컬렉션에 대 한 단일 탭 정지를 정의 하 여 앱에서 전체 탭 정지의 수를 최소화할 수 있습니다.

예를 들어 다음 그림은 두 개의 누적된 `ListView`컨트롤을 보여 줍니다. 왼쪽 그림은 탭 정지와 함께 `ListView` 컨트롤을 탐색하는 데 사용된 화살표 키 탐색을 보여 주고, 오른쪽 이미지는 Tab 키로 부모 컨트롤을 트래버스할 필요성을 제거하여 자식 요소 간의 탐색을 좀 더 쉽고 효율적으로 하는 방법을 보여 줍니다.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***두 개의 누적된 ListView 컨트롤과 상호 작용 탭 정지를 제거 하 고 화살표 키만로 이동 하 여 더 쉽고 효율적으로 만들 수 있습니다.***

응용 프로그램 UI에 최적화 예제를 적용하는 방법을 알아보려면 [제어 그룹](#control-group) 섹션을 방문하세요.

### <a name="interaction-and-commanding"></a>조작 및 명령

컨트롤에 포커스가 있으면 사용자는 특정 키보드 입력을 사용하여 해당 컨트롤을 조작하고 관련 기능을 호출할 수 있습니다.

#### <a name="text-entry"></a>텍스트 입력

`TextBox` 및 `RichEditBox`처럼 텍스트 입력을 위해 특별히 설계된 컨트롤의 경우 모든 키보드 입력이 텍스트 입력 또는 탐색에 사용되며, 다른 키보드 명령보다 높은 우선 순위를 갖습니다. 예를 들어 `AutoSuggestBox` 컨트롤의 드롭다운 메뉴는 **Space** 키를 선택 명령으로 인식하지 못합니다.

![텍스트 입력](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Space 키

텍스트 입력 모드에 있지 않은 경우 **Space** 키는 포커스가 설정된 컨트롤과 연결된 작업 또는 명령(예: 터치로 탭 또는 마우스로 클릭)을 호출합니다.

![Space 키](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Enter 키

**Enter** 키는 포커스가 있는 컨트롤에 따라 다양한 공통 사용자 상호 작용을 수행할 수 있습니다.
-   `Button`또는 `Hyperlink` 같은 명령 컨트롤을 활성화합니다. 최종 사용자의 혼란을 방지하기 위해 **Enter** 키는 `ToggleButton` 또는 `AppBarToggleButton` 같은 명령 컨트롤처럼 보이는 컨트롤도 활성화합니다.
-   `ComboBox` 및 `DatePicker` 같은 컨트롤의 선택기 UI를 표시합니다. 또한 **Enter** 키는 선택기 UI를 커밋하고 닫습니다.
-   `ListView`, `GridView` 및 `ComboBox` 같은 목록 컨트롤을 활성화합니다.
    -   목록 및 그리드 항목과 연결된 추가 작업(새 창 열기)이 없으면 **Enter** 키는 목록 및 그리드 항목에 대한 **Space** 키로 선택 작업을 수행합니다.
    -   컨트롤과 연결된 추가 작업이 있으면 **Enter** 키가 추가 작업을 수행하고 **Space** 키가 선택 작업을 수행합니다.

**참고** **Enter** 키와 **Space** 키가 항상 같은 작업을 수행하는 것은 아니지만 같은 작업을 수행하는 경우가 자주 있습니다.

![Enter 키](images/keyboard/enter-key.png)

Esc 키를 통해 사용자는 임시 UI(해당 UI에서 진행 중인 작업과 함께)를 취소할 수 있습니다.

이 환경의 예는 다음과 같습니다.
-   사용자가 값이 선택된 `ComboBox`를 열고 화살표 키를 사용하여 포커스 선택을 새 값으로 이동합니다. Esc 키를 눌러 `ComboBox`를 닫고 선택한 값을 원래 값으로 다시 설정합니다.
-   사용자가 전자 메일에 대한 영구 삭제 작업을 호출하고 `ContentDialog`에 작업을 확인하라는 메시지가 표시됩니다. 사용자는 의도한 작업이 아님을 확인하고 **Esc** 키를 눌러 대화 상자를 닫습니다. **Esc** 키는 **취소** 단추와 연결되어 있기 때문에 대화 상자가 닫히고 작업이 취소됩니다. **Esc** 키는 임시 UI에만 영향을 주며, 앱 UI를 닫거나 뒤로 탐색하지 않습니다.

![Esc 키](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Home 및 End 키

사용자는 **Home** 및 **End** 키로 UI 영역의 시작 또는 끝으로 스크롤할 수 있습니다.

이 환경의 예는 다음과 같습니다.
-   `ListView` 및 `GridView` 컨트롤의 경우 **Home** 키는 포커스를 첫 번째 요소로 이동하여 보기로 스크롤하고, **End** 키는 포커스를 마지막 요소로 이동하여 보기로 스크롤합니다.
-   `ScrollView` 컨트롤의 경우 **Home** 키는 영역 맨 위로 스크롤하고, **End** 키는 영역 맨 아래로 스크롤합니다(포커스는 변경되지 않음).

![Home 및 End 키](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Page up 및 Page down 키

사용자는 **Page** 키를 사용하여 UI 영역을 개별 단위로 스크롤할 수 있습니다.

예를 들어 `ListView` 및 `GridView` 컨트롤의 경우 **Page up** 키는 영역을 한 "페이지"(일반적으로 뷰포트 높이)만큼 위로 스크롤하고 포커스를 영역 맨 위로 이동합니다. 반면 **Page down** 키는 영역을 한 페이지만큼 아래로 스크롤하고 포커스를 영역 맨 아래로 이동합니다.

![Page up 및 Page down 키](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>바로 가기 키

바로 가기 키 앱 둘 다 내게 필요한 옵션에 대 한 지원이 향상 되었습니다 및 키보드 사용자가 효율성 향상을 제공 하 여 쉽게 수 있습니다.

앱에서 키보드 탐색 및 정품 인증을 지원할 뿐 아니라 것도 좋은 방법은 응용 프로그램의 기능에 대 한 바로 가기를 제공 합니다. 탭 탐색 키보드 지원 하지만 바로 가기 키에 대 한 지원을 추가 하려는 더 복잡 한 UI를 사용 하 여 적합 한, 기본 수준을 제공 합니다. 

바로 가기는 사용자가 앱 기능에 효율적으로 액세스할 수 있도록 하여 생산성을 향상하는 키보드 조합입니다. 다음과 같은 두 종류의 바로 가기가 있습니다.
-   [가속기](#accelerators) 은 응용 프로그램 명령입니다. 호출 하는 바로 가기입니다. 앱 수 또는 명령에 해당 하는 특정 UI를 제공할 수 없습니다. 가속기는 Ctrl 키와 문자 키의 일반적으로 구성 됩니다.
-   [액세스 키](#access-keys) 은 응용 프로그램에서 특정 UI에 포커스를 설정 하는 바로 가기입니다. 액세스 키 typicaly Alt 키와 문자 키를 구성 합니다.

응용 프로그램에서 유사한 작업을 지 원하는 일관 된 바로 가기 키를 제공 합니다. 훨씬 더 유용 하 고 강력한 있게 고 사용자가 기억 하기.

#### <a name="accelerators"></a>바로 연결

가속기 훨씬 더 빠르고 효율적으로 응용 프로그램에서 일반 동작을 수행할 수 있도록 합니다. 

바로 가기의 예:
-   키를 누르면 Ctrl + N에서 아무 곳 이나 키를 **메일** 앱이 새 메일 항목을 시작 합니다.
-   Ctrl + E Microsoft Edge에서 아무 곳 이나 키 (및 대부분의 Microsoft Store 응용 프로그램)를 눌러 검색을 시작 합니다.

바로 가기의 특징은 다음과 같습니다.
-   주로 Ctrl와 함수를 사용 (Windows 시스템 바로 가기 키 사용 Alt + 영숫자 키 및 Windows 로고 키) 시퀀스 키입니다.
-   바로 가기 키는 가장 자주 사용하는 명령에만 할당됩니다.
-   바로 가기 키는 기억해야 하며, 메뉴, 도구 설명 및 도움말에만 기록됩니다.
-   지원 되는 경우 전체 응용 프로그램 전체에서 영향을 주지 합니다.
-   이러한 할당 되어야 합니다 일관 되 게 여 암기 되 고 직접 설명 되지 않은.

#### <a name="access-keys"></a>선택키

UWP로 선택키를 지원하는 방법에 대한 자세한 내용은 [선택키](access-keys.md) 페이지를 참조하세요.

선택키는 거동 장애가 있는 사용자가 한 번에 하나의 키를 눌러 UI의 특정 항목을 수행할 수 있게 도와줍니다. 뿐만 아니라 선택키를 추가 바로 가기 키와 통신하는 데 사용하면 고급 사용자가 신속하게 작업을 수행할 수 있습니다.

선택키의 특성은 다음과 같습니다.
-   선택키는 Alt 키와 영숫자 키를 사용합니다.
-   선택키는 주로 접근성을 위한 기능입니다.
-   컨트롤에 인접 한 UI에서 직접 설명 되어 있습니다를 통해 [키 팁](access-keys.md)합니다.
-   선택키는 현재 창에만 영향을 주며 해당 메뉴 항목이나 컨트롤로 이동합니다.
-   자주 사용 되는 명령 (특히: 커밋 단추)에 액세스 키를 일관 되 게 할당 해야 가능 합니다.
-   선택키는 지역화됩니다.

#### <a name="common-keyboard-shortcuts"></a>일반적인 바로 가기 키

다음 표에서 자주 사용 되는 바로 가기 키의 작은 예제. 

| 작업                               | 키 명령                                      |
|--------------------------------------|--------------------------------------------------|
| 모두 선택                           | Ctrl+A                                           |
| 계속해서 선택                  | Shift+화살표 키                                  |
| 저장                                 | Ctrl+S                                           |
| 찾기                                 | Ctrl+F                                           |
| 인쇄                                | Ctrl+P                                           |
| 복사                                 | Ctrl+C                                           |
| 잘라내기                                  | Ctrl+X                                           |
| 붙여넣기                                | Ctrl+V                                           |
| 실행 취소                                 | Ctrl+Z                                           |
| 다음 탭                             | Ctrl+Tab                                         |
| 탭 닫기                            | Ctrl+F4 또는 Ctrl+W                                |
| 시맨틱 줌                        | Ctrl++ 또는 Ctrl+-                                 |

Windows 시스템 바로 가기 키 목록은 포괄적인 참조 [Windows에 대 한 바로 가기 키](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts)합니다. 일반적인 응용 프로그램 바로 가기를 참조 [바로 가기 키 Microsoft 응용 프로그램에 대 한](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps)합니다.

## <a name="advanced-experiences"></a>고급 환경

이 섹션에서는 앱을 다른 디바이스 및 다른 도구와 함께 사용할 때 알아야 하는 일부 동작과 함께, UWP 앱에서 지원되는 좀 더 복잡한 키보드 조작 환경 중 일부를 살펴보겠습니다.

### <a name="control-group"></a>컨트롤 그룹

관련 컨트롤 또는 상호 보완적 컨트롤 집합을 "컨트롤 그룹"(또는 방향 영역)으로 그룹화할 수 있으며, 이렇게 하면 화살표 키를 사용하여 "내부 탐색"이 가능합니다. 컨트롤 그룹은 단일 탭 정지일 수도 있고, 개발자가 컨트롤 그룹 내에서 여러 탭 정지를 지정할 수도 있습니다.

#### <a name="arrow-key-navigation"></a>화살표 키 탐색

UI 영역에 서로 관련이 있는 비슷한 컨트롤 그룹이 있으면 사용자는 화살표 키 탐색이 지원될 것으로 기대합니다.
-   `AppBarButtons` 에 `CommandBar`
-   `ListItems` 또는 `GridItems` 내부 `ListView` 또는 `GridView`
-   `Buttons` 내부 `ContentDialog`

UWP 컨트롤은 기본적으로 화살표 키 탐색을 지원합니다. 사용자 지정 레이아웃 및 컨트롤 그룹의 경우 `XYFocusKeyboardNavigation="Enabled"`를 사용하여 유사한 동작을 제공합니다.

다음 컨트롤을 사용 하는 경우 키 탐색 화살표에 대 한 지원을 추가 하는 것이 좋습니다.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>대화 상자 단추</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems 및 GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>탭 정지

응용 프로그램의 기능 및 레이아웃에 따라 컨트롤 그룹에 대 한 최상의 탐색 옵션에는 자식 요소, 여러 탭 정지 또는 이들의 조합 탐색 화살표를 사용 하 여 단일 탭 정지를 수 있습니다.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>단추에 다중 탭 정지와 화살표 키 사용

접근성 사용자는 잘 정립된 키보드 탐색 규칙을 사용하며, 이 규칙은 일반적으로 단추 컬렉션 탐색에 화살표 키를 사용하지 않습니다. 하지만 시각 장애가 없는 사용자는 이 동작이 자연스럽다고 느낄 수 있습니다.

이 문서에서 기본 UWP 동작의 예는 `ContentDialog`입니다. 화살표 키를 사용하여 단추 사이를 이동할 수 있지만, 각 단추가 탭 정지이기도 합니다.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>친숙한 UI 패턴에 단일 탭 정지 할당

레이아웃이 잘 알려진 컨트롤 그룹의 UI 패턴을 따르는 경우 그룹에 단일 탭 정지를 할당하면 사용자의 탐색 효율성을 향상할 수 있습니다.

예를 들면 다음과 같습니다.
-   `RadioButtons`
-   여러 `ListViews` 유사 하 고 단일 처럼 `ListView`
-   모양과 동작이 타일 그리드(예: 시작 메뉴 타일)와 비슷한 UI

#### <a name="specifying-control-group-behavior"></a>컨트롤 그룹 동작 지정

다음 API를 사용하여 사용자 지정 컨트롤 그룹 동작을 지원합니다(모든 내용은 이 항목의 뒷부분에서 자세히 설명).

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) - 화살표 키로 컨트롤을 탐색할 수 있게 합니다.
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) - 여러 탭 정지가 있는지 아니면 단일 탭 정지가 있는지를 나타냅니다.
-   [FindFirstFocusableElement and FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) - **Home** 키를 사용하여 첫 번째 항목에 포커스를 설정하고 **End** 키를 사용하여 마지막 항목에 포커스를 설정합니다.

다음 그림은 연결된 라디오 단추의 컨트롤 그룹에 대한 직관적인 키보드 탐색 동작을 보여 줍니다. 이 예에서는 컨트롤 그룹에 대한 단일 탭 정지, 화살표 키를 사용하는 라디오 단추 간 내부 탐색, 첫 번째 라디오 단추에 연결된 **Home** 키, 마지막 라디오 단추에 연결된 **End** 키를 권장합니다.

![요약](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>키보드 및 내레이터

내레이터는 키보드 사용자를 위한 UI 접근성 도구입니다(다른 입력 유형도 지원됨). 하지만 내레이터 기능은 UWP 앱에서 지원하는 키보드 조작에서 그치지 않으며 내레이터용 UWP 앱을 디자인할 때는 각별한 주의가 필요합니다. ([내레이터 기본 사항 페이지](https://support.microsoft.com/help/22808/windows-10-narrator-basics)에 내레이터 사용자 환경이 설명되어 있습니다.)

UWP 키보드 동작과 내레이터에서 지원하는 동작의 차이점은 다음과 같습니다.
-   컨트롤 레이블을 읽는 Caps Lock + 화살표 키처럼 표준 키보드 탐색을 통해 노출되지 않는 UI 요소를 탐색하는 추가 키 조합.
-   비활성화된 항목 탐색. 기본적으로 비활성화된 항목은 표준 키보드 탐색을 통해 노출되지 않습니다.
-   UI 세분성에 따라 보다 빠른 탐색을 위한 컨트롤 "보기". 사용자가 항목, 문자, 단어, 줄, 단락, 링크, 머리글, 표, 랜드마크 및 제안을 탐색할 수 있습니다. 표준 키보드 탐색에서 이러한 개체를 단순 목록으로 노출하며, 바로 가기 키를 제공하지 않으면 탐색이 번거로울 수 있습니다.

#### <a name="case-study--autosuggestbox-control"></a>사례 연구 – AutoSuggestBox 컨트롤

`AutoSuggestBox`에 대한 검색 버튼은 Tab 및 화살표 키를 사용하는 표준 키보드 탐색에 액세스할 수 없습니다. 왜냐하면 사용자가 **Enter** 키를 눌러 검색 쿼리를 제출할 수 있기 때문입니다. 하지만 사용자가 Caps Lock + 화살표 키를 누를 때 내레이터를 통해 액세스할 수 있습니다.

![자동 제안 키보드 포커스](images/keyboard/auto-suggest-keyboard.png)

*사용자가 키보드를 사용 하 여 키를 눌러 합니다* ***Enter*** *검색 쿼리를 제출 하는 키*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>사용자가 내레이터를 사용 하 여 키를 눌러 합니다 <strong>Enter</strong> 검색 쿼리를 제출 하는 키</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>내레이터를 사용 하 여 사용자도 사용 하 여 search 단추에 액세스할 수 합니다 <strong>Caps Lock + 오른쪽 화살표 키</strong>를 눌러 <strong>공간</strong> 키</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>키보드와 Xbox 게임 패드 및 리모컨

Xbox 게임 패드 및 리모컨은 여러 UWP 키보드 동작과 환경을 지원합니다. 그러나 키보드에서 사용할 수 있는 다양한 키 옵션이 부족하기 때문에 게임 패드 및 리모컨은 여러 키보드 최적화가 부족합니다(리모컨이 게임 패드보다 훨씬 제한적).

참조 [Gamepad 및 원격 제어 상호 작용](gamepad-and-remote-interactions.md) gamepad 입력과 원격 제어에 대 한 UWP 지원에 대 한 세부 정보에 대 한 합니다.

다음은 키보드, 게임 패드 및 리모컨 사이의 몇 가지 키 매핑을 보여 줍니다.

| **키보드**  | **게임 패드**                         | **원격 제어**  |
|---------------|-------------------------------------|---------------------|
| Space         | A 버튼                            | 선택 단추       |
| 입력         | A 버튼                            | 선택 단추       |
| 이스케이프        | B 버튼                            | 뒤로 단추         |
| Home/End      | 해당 사항 없음                                 | 해당 사항 없음                 |
| Page Up/Down  | 세로 방향 스크롤을 위한 트리거 단추, 가로 방향 스크롤을 위한 범퍼 단추   | 해당 사항 없음                 |

게임 패드 및 리모컨과 함께 사용할 UWP 앱을 디자인할 때에는 다음과 같은 주요 차이점에 대해 알아야 합니다.
-   텍스트를 입력하려면 사용자가 A를 눌러 텍스트 컨트롤을 활성화해야 합니다.
-   포커스 탐색은 컨트롤 그룹을 제한되지 않으며, 사용자는 앱에서 포커스 가능한 UI 요소를 자유롭게 탐색할 수 있습니다.

    **참고** 포커스가 오버레이 UI에 있거나 [포커스 연결](gamepad-and-remote-interactions.md#focus-engagement)이 지정되지 않은 이상, 키 누르기 방향의 포커스 가능 UI 요소로 포커스가 이동할 수 있으며, 따라서 A 단추와 연결/연결 해제될 때까지 포커스가 영역에 들어가거나 영역에서 나오지 못합니다. 자세한 내용은 [방향 탐색](#directional-navigation) 섹션을 참조하세요.
-   D 패드 및 왼쪽 스틱 단추는 컨트롤 사이에서 포커스를 이동하고 내부 탐색하는 데 사용 됩니다.

    **참고** 게임 패드 및 리모컨은 방향 키를 누를 때와 시각적 순서가 동일한 항목만 탐색합니다. 포커스를 받을 수 있는 후속 요소가 없는 경우 해당 방향으로 탐색을 사용할 수 없습니다. 상황에 따라 키보드 사용자에게 항상 이 제한이 적용되는 것은 아닙니다. 자세한 내용은 [기본 키보드 최적화](#built-in-keyboard-optimization) 섹션을 참조하세요.

#### <a name="directional-navigation"></a>방향 탐색

방향 탐색은 눌린 방향 키(화살표 키, D 패드)를 가져와서 해당 시각적 방향으로 포커스를 이동하려고 시도하는 UWP [포커스 관리자](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) 도우미 클래스를 통해 관리됩니다.

앱의 opts 때 키보드와 달리 [마우스 모드](gamepad-and-remote-interactions.md#mouse-mode), 방향 탐색 gamepad 및 원격 제어에 대 한 전체 응용 프로그램에서 적용 됩니다. 참조 [Gamepad 및 원격 제어 상호 작용](gamepad-and-remote-interactions.md) 방향 탐색 최적화에 대 한 세부 정보에 대 한 합니다.

**참고** 키보드 Tab 키를 사용한 탐색은 방향 탐색으로 간주되지 않습니다. 자세한 내용은 [탭 정지](#tab-stops) 섹션을 참조하세요.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>방향 탐색 지원</strong></br>사용자 다른 컨트롤 간을 탐색할 수 (키보드 화살표, 게임 패드 및 원격 제어 패드) 방향 키를 사용 합니다.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>방향 탐색 지원 되지 않습니다</strong> </br>사용자가 방향 키를 사용하여 여러 컨트롤을 탐색할 수 없습니다. 다른 방법 (tab 키) 다음 컨트롤로 이동의 영향을 받지 않습니다.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>키보드 최적화에 기본 제공

사용되는 레이아웃과 컨트롤에 따라 UWP 앱을 특별히 키보드 입력에 맞게 최적화할 수 있습니다.

다음 예에서는 단일 탭 정지에 할당된 목록 항목, 그리드 항목 및 메뉴 항목 그룹을 보여 줍니다([탭 정지](#tab-stops) 섹션 참조). 그룹에 포커스가 있으면 방향 화살표 키를 사용하여 해당 시각적 순서로 내부 탐색이 수행됩니다([탐색](#navigation) 섹션 참조).

![단일 열 화살표 키 탐색](images/keyboard/single-column-arrow.png)

***단일 열 화살표 키 탐색***

![단일 행 화살표 키 탐색](images/keyboard/single-row-arrow.png)

***단일 행 화살표 키 탐색***

![다중 열 및 행 화살표 키 탐색](images/keyboard/multiple-column-and-row-navigation.png)

***여러 열/행 화살표 키 탐색***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>동종 목록 및 그리드 보기 항목 래핑

방향 탐색이 항상 목록 및 GridView 항목의 여러 행과 열을 탐색하는 가장 효율적인 방법인 것은 아닙니다.

**참고** 메뉴 항목은 일반적으로 단일 열 목록이지만, 경우에 따라 특별한 포커스 규칙이 적용될 수 있습니다([팝업 UI](#popup-ui) 참조).

여러 행과 열을 사용하여 목록 및 그리드 개체를 만들 수 있습니다. 이러한 개체는 일반적으로 행 중심(항목이 전체 행을 채운 후 그 다음 행을 채움) 또는 열 중심(항목이 전체 열을 채운 후 그 다음 열을 채움) 배열입니다. 행 또는 열 중심 배열은 스크롤 방향에 따라 결정되며 항목 순서가 이 방향과 충돌하지 않도록 주의해야 합니다.

행 중심 배열(항목이 왼쪽에서 오른쪽으로, 위에서 아래로 채움)에서는 포커스가 행의 마지막 항목에 있는 상태에서 오른쪽 화살표 키를 누르면 포커스가 그 다음 행의 첫 번째 항목으로 이동합니다. 반대로이 동일한 동작이 발생합니다. 포커스가 행의 첫 번째 항목으로 설정 됩니다 왼쪽된 화살표 키를 누를 때 포커스가 이동 마지막 항목 이전 행의 합니다.

열 중심 배열(항목이 위에서 아래로, 왼쪽에서 오른쪽으로 채움)에서는 포커스가 열의 마지막 항목에 있는 상태에서 사용자가 아래쪽 화살표 키를 누르면 포커스가 그 다음 열의 첫 번째 항목으로 이동합니다. 반대로이 동일한 동작이 발생합니다. 열에 첫 번째 항목에 포커스가 설정 됩니다 하 고 위쪽 화살표 키를 누르면, 포커스가 이동 마지막 항목 이전 열에 있습니다.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>행 주요 키보드 탐색</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>열 주요 키보드 탐색</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>UI 팝업

언급 했 듯이 응용 프로그램의 UI에서 컨트롤의 시각적 개체 순서에 해당 하는 방향 탐색을 확인 하려고 해야 합니다.

일부 컨트롤 (예: 상황에 맞는 메뉴, 명령 모음 오버플로 메뉴 및 AutoSuggest 메뉴)의 기본 컨트롤 및 사용 가능한 화면 공간을 기준으로 아래쪽으로 기본적으로 위치 및 방향에 팝업 메뉴를 표시합니다. 참고 런타임에 여 방향을 다양 한 요인에 따라 달라질 수 있습니다.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

이러한 컨트롤의 메뉴를 처음으로 열면 (및 사용자가 선택 된 항목이 없음을) 하는 경우 아래쪽 화살표 키를 항상 포커스를 설정 첫 번째 항목 동안 위쪽 화살표 키를 항상 마지막 항목에 집중 합니다. 

마지막 항목에 포커스가 있는 아래쪽 화살표 키를 누를 경우 포커스가 메뉴의 첫 번째 항목으로 이동 합니다. 마찬가지로, 첫 번째 항목에 포커스가 있는 위쪽 화살표 키를 누를 경우 포커스가 이동 마지막 항목 메뉴에서 합니다. 이 동작은 이라고 *순환* 예측할 수 없는 방향으로 열 수 있는 팝업 메뉴를 탐색 하는 데 유용 합니다.

> [!NOTE]
> 사용자는 무한 루프에서 트랩을 가져올 수 있습니다 위치 하는 팝업이 아닌 ui에서 순환를 피해 야 합니다. 

사용자 지정 컨트롤에서 이러한 동일한 동작을 에뮬레이션 하는 것이 좋습니다. 이 동작을 구현 하는 방법에 대 한 코드 샘플에서 확인할 수 있습니다 [프로그래밍 방식 포커스 탐색](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) 설명서.

## <a name="test-your-app"></a>앱 테스트

지원되는 모든 입력 디바이스로 앱을 테스트하여 UI 요소를 일관적이고 직관적인 방법으로 탐색할 수 있고 예상치 못한 요소가 원하는 탭 순서에 방해가 되지 않는지 확인합니다.

## <a name="related-articles"></a>관련 문서
* [키보드 이벤트](keyboard-events.md)
* [입력 디바이스 식별](identify-input-devices.md)
* [터치 키보드의 현재 상태에 대 한 응답](respond-to-the-presence-of-the-touch-keyboard.md)
* [포커스 화면 효과 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619895)

## <a name="appendix"></a>부록

### <a name="software-keyboard"></a>소프트웨어 키보드

소프트웨어 키보드는 화면에 표시되는 키보드이며 사용자가 실제 키보드 대신 터치, 마우스, 펜/스타일러스 또는 기타 포인팅 장치로 데이터를 입력하는 데 사용할 수 있습니다(터치 스크린이 필요하지 않음). 터치 스크린에서 이러한 키보드를 직접 터치하여 텍스트를 입력할 수도 있습니다. Xbox One 디바이스에서는 게임 패드 또는 리모컨을 사용하여 포커스 화면 효과를 이동하거나 바로 가기 키를 사용하여 개별 키를 선택해야 합니다.

![Windows 10 터치 키보드](images/keyboard/default.png)

***Windows 10 터치 키보드***

![Xbox one 화상 키보드](images/keyboard/xbox-onscreen-keyboard.png)

***Xbox One 화상 키보드***

디바이스에 따라 소프트웨어 키보드는 텍스트 필드나 다른 편집 가능한 텍스트 컨트롤이 포커스를 받을 때 또는 사용자가 **알림 센터**를 통해 수동으로 설정할 때 나타납니다.

![알림 센터의 터치 키보드 아이콘](images/keyboard/touch-keyboard-notificationcenter.png)

앱에서 포커스를 프로그래밍 방식으로 텍스트 입력 컨트롤로 설정하는 경우에는 터치 키보드가 호출되지 않습니다. 이는 사용자가 직접 실시하지 않은 예기치 않은 동작을 제거합니다. 그러나 포커스가 프로그래밍 방식으로 텍스트 입력 컨트롤이 아닌 컨트롤로 이동하는 경우에는 키보드가 자동으로 숨겨집니다.

터치 키보드는 사용자가 양식에서 컨트롤 사이를 이동하는 동안 일반적으로 표시된 상태로 유지됩니다. 이 동작은 양식 내에 있는 다른 컨트롤 형식에 따라 다를 수 있습니다.

다음은 터치 키보드를 사용하는 텍스트 입력 세션 중에 키보드를 해제하지 않고 포커스를 받을 수 있는 비편집 컨트롤 목록입니다. 사용자는 터치 키보드에서 컨트롤과 텍스트 입력 사이를 왔다 갔다 하는 경향이 있으므로 터치 키보드를 뷰에 고정하여 불필요하게 UI를 이리 저리 찾아다니거나 혼란에 빠지지 않게 합니다.

-   확인란
-   콤보 상자
-   라디오 단추
-   스크롤 막대
-   trEE
-   트리 항목
-   메뉴
-   메뉴 모음
-   메뉴 항목
-   도구 모음
-   List
-   목록 항목

다음은 터치 키보드의 다양한 모드의 예입니다. 첫 번째 이미지는 기본 레이아웃이고, 두 번째 이미지는 미리 보기 레이아웃으로 일부 언어에서는 사용할 수 없습니다.

![기본 레이아웃 모드의 터치 키보드](images/keyboard/default.png)

***터치 키보드의 기본 레이아웃 모드***

![확장 레이아웃 모드의 터치 키보드](images/keyboard/extendedview.png)

***확장 된 레이아웃 모드에서 터치 키보드***

성공적인 키보드 조작을 통해 사용자는 키보드만 사용하여 기본 앱 시나리오를 수행할 수 있습니다. 즉, 사용자는 모든 대화형 요소에 액세스하고 기본 기능을 활성화할 수 있습니다. 키보드 탐색, 내게 필요한 옵션에 대한 선택키, 고급 사용자를 위한 바로 가기 등 다양한 요소가 성공의 수준에 영향을 미칠 수 있습니다.

**참고**  터치 키보드 설정/해제 하 고 대부분의 시스템 명령을 지원 하지 않습니다.

#### <a name="on-screen-keyboard"></a>화상 키보드
소프트웨어 키보드와 마찬가지로, 화상 키보드는 실제 키보드 대신 터치, 마우스, 펜/스타일러스 또는 기타 포인팅 장치로 데이터를 입력하는 데 사용할 수 있는 시각적 가상 키보드입니다(터치 스크린이 필요하지 않음). 실제 키보드가 없는 시스템이나 움직일 수 없어서 일반적인 입력 장치를 사용할 수 없는 사용자는 화상 키보드를 사용할 수 있습니다. 화상 키보드는 하드웨어 키보드의 기능을 전부는 아니라도 대부분 에뮬레이트합니다.

화상 키보드는 설정 &gt; 접근성의 키보드 페이지에서 켤 수 있습니다.

**참고** 화상 키보드는 터치 키보드보다 우선하며, 현재 화상 키보드가 사용 중이면 터치 키보드는 나타나지 않습니다.

![화상 키보드](images/keyboard/osk.png)

***화상 키보드***

화상 키보드에 대한 자세한 내용은 [화상 키보드 페이지](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard)를 참조하세요.

## <a name="related-articles"></a>관련 문서

- [키보드 접근성](../accessibility/keyboard-accessibility.md)
