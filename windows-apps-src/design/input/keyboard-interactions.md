---
Description: Windows 앱을 디자인 하 고 최적화 하는 방법에 대해 알아봅니다. 그러면 키보드 전원 사용자와 장애가 있는 사용자 및 기타 접근성 요구 사항이 있는 최상의 환경을 제공할 수 있습니다.
title: 키보드 조작
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: 키보드, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 상호 작용, 게임 패드, 원격
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: f4f2e9e13f492dd9a38d737c0c86dd3b1e632279
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173487"
---
# <a name="keyboard-interactions"></a>키보드 조작

![키보드 주인공 이미지](images/keyboard/keyboard-hero.jpg)

Windows 앱을 디자인 하 고 최적화 하는 방법에 대해 알아봅니다. 그러면 키보드 전원 사용자와 장애가 있는 사용자 및 기타 접근성 요구 사항이 있는 최상의 환경을 제공할 수 있습니다.

장치에서 키보드 입력은 전체 Windows 앱 상호 작용 환경에서 중요 한 부분입니다. 잘 디자인 된 키보드 환경을 사용 하면 사용자가 앱의 UI를 효율적으로 탐색 하 고 키보드에서 손을 떼지 않고도 전체 기능에 액세스할 수 있습니다.

![키보드 및 게임 패드 이미지](images/keyboard/keyboard-gamepad.jpg)

***일반적인 상호 작용 패턴은 키보드와 게임 패드 간에 공유 됩니다.***

이 항목에서는 Pc의 키보드 입력을 위한 Windows 앱 디자인에 대해 집중적으로 설명 합니다. 그러나 Windows 내레이터와 같은 내게 필요한 옵션 도구를 지원 하 고, 터치 키보드 및 화상 키보드 (OSK)와 같은 [소프트웨어 키보드](#software-keyboard) 를 사용 하 고, Xbox 게임 및 원격 제어와 같은 기타 입력 장치 유형을 처리 하는 데에는 잘 디자인 된 키보드 환경이 중요 합니다.

[포커스 시각적 개체](#focus-visuals), [액세스 키](#access-keys)및 [UI 탐색](#navigation)을 비롯 하 여 여기에서 설명 하는 대부분의 지침과 권장 사항은 이러한 다른 시나리오에도 적용 됩니다.

**참고**  텍스트 입력에 하드웨어 및 소프트웨어 키보드가 모두 사용 되지만이 항목의 초점은 탐색 및 상호 작용입니다.

## <a name="built-in-support"></a>기본 제공 지원

마우스와 함께 키보드는 pc에서 가장 널리 사용 되는 주변 장치 이며, PC 환경의 기본적인 부분입니다. PC 사용자는 키보드 입력에 대 한 응답으로 시스템 및 개별 앱에서 포괄적이 고 일관 된 환경을 필요로 합니다.

모든 UWP 컨트롤에는 풍부한 키보드 환경 및 사용자 상호 작용에 대 한 기본 제공 지원이 포함 되어 있으며, 플랫폼 자체는 사용자 지정 컨트롤과 앱 모두에 가장 적합 한 키보드 환경을 만들기 위한 광범위 한 기반을 제공 합니다.

![휴대폰 이미지를 사용 하는 키보드](images/keyboard/keyboard-phone.jpg)

***UWP는 모든 장치에서 키보드 지원***

## <a name="basic-experiences"></a>기본 환경
![포커스 기반 장치](images/keyboard/focus-based-devices.jpg)

앞에서 설명한 것 처럼 Xbox 게임 및 원격 제어와 같은 입력 장치와 내레이터와 같은 내게 필요한 옵션 도구는 탐색 및 명령에 많은 키보드 입력 환경을 공유 합니다. 입력 형식 및 도구에서 발생 하는 이러한 일반적인 환경은 사용자의 추가 작업을 최소화 하 고 유니버설 Windows 플랫폼의 "한 번 빌드, 어디서 나 실행" 목표에 기여 합니다.

필요한 경우 알아두어야 하는 주요 차이점을 확인 하 고 고려해 야 할 모든 완화 방법을 설명 합니다.

이 항목에서 설명 하는 장치 및 도구는 다음과 같습니다.

| 장치/도구                       | Description     |
|-----------------------------------|-----------------|
|키보드 (하드웨어 및 소프트웨어)   |표준 하드웨어 키보드 외에도 Windows 응용 프로그램은 [터치 (또는 소프트웨어) 키보드](#software-keyboard) 와 [화상 키보드](#on-screen-keyboard)라는 두 가지 소프트웨어 키보드를 지원 합니다.|
|게임 패드 및 리모컨         |Xbox 게임 패드 및 원격 제어는 [10 피트 환경](../devices/designing-for-tv.md)에서 기본적인 입력 장치입니다. 게임 패드 및 원격 제어에 대 한 Windows 지원에 대 한 자세한 내용은 [게임 패드 및 원격 제어 상호 작용](gamepad-and-remote-interactions.md)을 참조 하세요.|
|화면 판독기 (내레이터)          |내레이터는 고유한 상호 작용 환경과 기능을 제공 하지만 기본적인 키보드 탐색 및 입력을 사용 하는 Windows 용 기본 제공 화면 판독기입니다. 내레이터 세부 정보는 [내레이터 시작](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)을 참조 하세요.|

## <a name="custom-experiences-and-efficient-keyboarding"></a>사용자 지정 환경 및 효율적인 키보드 사용
앞에서 설명한 것 처럼 키보드 지원은 다양 한 기술, 기능 및 기대치를 가진 사용자에 게 적합 한 응용 프로그램을 보장 하기 위한 필수 사항입니다. 다음의 우선 순위를 지정 하는 것이 좋습니다.
- 키보드 탐색 및 상호 작용 지원
    - 실행 가능한 항목이 탭 정지 (및 실행 불가능 한 항목은 아님)로 식별 되 고 탐색 순서가 논리적이 고 예측 가능 하도록 합니다 ( [탭 정지](#tab-stops)참조).
    - 가장 논리적 요소에 초기 포커스를 설정 합니다 ( [초기 포커스](#initial-focus)참조).
    - "내부 탐색"에 대 한 화살표 키 탐색 제공 ( [탐색](#navigation)참조)
- 바로 가기 키 지원
    - 빠른 작업에 액셀러레이터 키 제공 ( [액셀러레이터](#accelerators)키 참조)
    - 응용 프로그램의 UI를 탐색 하기 위한 액세스 키 제공 ( [액세스 키](access-keys.md)참조)

### <a name="focus-visuals"></a>시각적 개체 포커스

UWP는 모든 입력 유형과 환경에서 잘 작동 하는 단일 포커스 비주얼 디자인을 지원 합니다.
![포커스 시각적 개체](images/keyboard/focus-visual.png)

포커스 시각적 개체:

- UI 요소가 키보드 및/또는 게임 패드/원격 제어에서 포커스를 받을 때 표시 됩니다.
- 작업을 수행할 수 있음을 나타내기 위해 UI 요소 주위에 강조 표시 된 테두리로 렌더링 됩니다.
- 사용자가 손실 없이 앱 UI를 탐색할 수 있도록 지원
- 앱에 대해 사용자 지정할 수 있습니다 ( [시각적 시각적 표시 시각적 개체](guidelines-for-visualfeedback.md#high-visibility-focus-visuals)참조).

**참고** UWP 포커스 시각적 개체는 내레이터 포커스 사각형과 동일 하지 않습니다.

### <a name="tab-stops"></a>탭 정지

키보드를 사용 하 여 컨트롤 (탐색 요소 포함)을 사용 하려면 컨트롤에 포커스가 있어야 합니다. 컨트롤에서 키보드 포커스를 수신 하는 한 가지 방법은 응용 프로그램의 탭 순서에서 탭 정지로 식별 하 여 탭 탐색을 통해 액세스할 수 있도록 하는 것입니다.

탭 순서에 컨트롤을 포함 하려면 [IsEnabled](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) 속성을 **true** 로 설정 하 고 [istabstop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성을 **true**로 설정 해야 합니다.

탭 순서에서 컨트롤을 구체적으로 제외 하려면 [Istabstop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성을 **false**로 설정 합니다.

기본적으로 탭 순서는 UI 요소가 생성 되는 순서를 반영 합니다. 예를 들어에, 및가 포함 된 경우 `StackPanel` `Button` `Checkbox` `TextBox` 탭 순서는 `Button` , `Checkbox` 및 `TextBox` 입니다.

[TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성을 설정 하 여 기본 탭 순서를 재정의할 수 있습니다.

#### <a name="tab-order-should-be-logical-and-predictable"></a>탭 순서는 논리적이 고 예측 가능 해야 합니다.

논리적이 고 예측 가능한 탭 순서를 사용 하는 잘 디자인 된 키보드 탐색 모델을 사용 하면 앱을 보다 직관적으로 만들고 사용자가 보다 효율적이 고 효과적으로 기능을 탐색, 검색 및 액세스할 수 있습니다.

모든 대화형 컨트롤은 [그룹](#control-group)에 있는 경우를 제외 하 고 탭 정지를 가져야 하지만 레이블과 같은 비 대화형 컨트롤은 그렇지 않아야 합니다.

응용 프로그램에서 포커스를 이동할 수 있도록 하는 사용자 지정 탭 순서를 사용 하지 않습니다. 예를 들어 양식에 있는 컨트롤의 목록에는 로캘에 따라 위쪽에서 아래쪽으로 이동 하 고 왼쪽에서 오른쪽으로 흐르는 탭 순서가 있어야 합니다.

탭 정지 사용자 지정에 대 한 자세한 내용은 [키보드 접근성](../accessibility/keyboard-accessibility.md) 을 참조 하세요.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>탭 순서 및 시각적 순서를 조정 하려고 합니다.

탭 순서 및 시각적 순서 (읽기 순서 또는 표시 순서 라고도 함)를 조정 하면 사용자가 응용 프로그램의 UI를 탐색할 때 혼동을 줄일 수 있습니다.

가장 중요 한 명령, 컨트롤 및 콘텐츠를 탭 순서와 시각적 순서에서 먼저 순위와 표시를 시도 합니다. 그러나 실제 표시 위치는 부모 레이아웃 컨테이너와 레이아웃에 영향을 주는 자식 요소의 특정 속성에 따라 달라질 수 있습니다. 특히 그리드 메타포 나 테이블 비유를 사용 하는 레이아웃은 탭 순서와 매우 다를 수 있습니다.

**참고** 시각적 순서만 로캘 및 언어에 따라 달라 집니다.

### <a name="initial-focus"></a>초기 포커스

초기 포커스는 응용 프로그램 또는 페이지가 처음 시작 되거나 활성화 될 때 포커스를 받는 UI 요소를 지정 합니다. 키보드를 사용 하는 경우이 요소에서 사용자가 응용 프로그램의 UI와 상호 작용 하기 시작 합니다.

UWP 앱의 경우 초기 포커스는 포커스를 받을 수 있는 가장 높은 [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 가 있는 요소로 설정 됩니다. 컨테이너 컨트롤의 자식 요소는 무시 됩니다. 타이의 시각적 트리에서 첫 번째 요소가 포커스를 받습니다.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>가장 논리적 요소에 초기 포커스 설정

사용자가 앱을 시작 하거나 페이지를 탐색할 때 가장 많이 사용 하는 첫 번째 또는 기본 작업에 대 한 UI 요소에 초기 포커스를 설정 합니다. 일부 사례:
-   포커스가 갤러리의 첫 번째 항목으로 설정 된 사진 앱
-   포커스가 재생 단추로 설정 된 음악 앱

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>잠재적으로 부정적인 결과 또는 치명적인 결과를 노출 하는 요소에 초기 포커스를 설정 하지 마세요.

이 기능 수준은 사용자가 선택 해야 합니다. 중요 한 결과를 가진 요소로 초기 포커스를 설정 하면 의도 하지 않은 데이터 손실이 나 시스템 액세스가 발생할 수 있습니다. 예를 들어 전자 메일로 이동할 때 포커스를 삭제 단추로 설정 하지 마세요.

탭 순서를 재정의 하는 방법에 대 한 자세한 내용은 [포커스 탐색](focus-navigation.md) 을 참조 하세요.

### <a name="navigation"></a>탐색

키보드 탐색은 일반적으로 Tab 키와 화살표 키를 통해 지원 됩니다.

![탭 및 화살표 키](images/keyboard/tab-and-arrow.png)

기본적으로 UWP 컨트롤은 다음과 같은 기본 키보드 동작을 따릅니다.
-   탭 **키** 탭 순서에서 실행 가능한/활성 컨트롤 간을 이동 합니다.
-   **Shift + tab** 을 누르면 역방향 탭 순서로 컨트롤이 이동 합니다. 사용자가 화살표 키를 사용 하 여 컨트롤 내부를 탐색 하는 경우 포커스는 컨트롤 내에서 마지막으로 알려진 값으로 설정 됩니다.
-   **화살표 키는** 사용자가 "내부 탐색"을 입력 하면 컨트롤 관련 "내부 탐색"을 노출 하 고, 화살표 키는 컨트롤에서 이동 하지 않습니다. 일부 사례:
    -   위쪽/아래쪽 화살표 키를 누르면 포커스가로 이동 합니다. `ListView``MenuFlyout`
    -   현재 선택 되어 있는 값 `Slider` 을 수정 합니다. `RatingsControl`
    -   캐럿 이동 `TextBox`
    -   항목 확장/축소 `TreeView`

이러한 기본 동작을 사용 하 여 응용 프로그램의 키보드 탐색을 최적화 합니다.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>관련 컨트롤 집합과 함께 "내부 탐색" 사용

응용 프로그램 UI의 전체 구성 내에서 관계를 강화 하는 일련의 관련 컨트롤에 화살표 키 탐색을 제공 합니다.

예를 들어 `ContentDialog` 여기에 표시 된 컨트롤은 기본적으로 단추의 가로 행에 대해 내부 탐색을 제공 합니다 (사용자 지정 컨트롤의 경우 [컨트롤 그룹](#control-group) 섹션 참조).

![대화 상자 예제](images/keyboard/dialog.png)

***화살표 키 탐색을 사용 하 여 관련 단추 컬렉션과의 상호 작용을 쉽게 수행할 수 있습니다.***

항목이 단일 열에 표시 되는 경우 위쪽/아래쪽 화살표 키가 항목을 탐색 합니다. 항목이 단일 행에 표시 되는 경우 오른쪽/왼쪽 화살표 키에서 항목을 탐색 합니다. 항목이 여러 열인 경우 4 개의 화살표 키로 이동 합니다.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>관련 컨트롤의 컬렉션에 대 한 단일 탭 정지를 정의 합니다.

관련 컨트롤 또는 보완 컨트롤의 컬렉션에 대해 단일 탭 정지를 정의 하면 앱에서 전체 탭 정지의 수를 최소화할 수 있습니다.

예를 들어 다음 이미지에서는 두 개의 누적 된 컨트롤을 보여 줍니다 `ListView` . 왼쪽의 이미지는 탭 정지에 사용 되는 화살표 키 탐색을 사용 하 여 컨트롤 간을 탐색 하 `ListView` 는 반면, 오른쪽의 이미지는 tab 키를 사용 하 여 부모 컨트롤을 트래버스할 필요가 없기 때문에 자식 요소 간의 탐색을 더 쉽고 효율적으로 만들 수 있는 방법을 보여 줍니다.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***탭 정지를 제거 하 고 화살표 키를 사용 하 여 탐색 하 여 두 개의 누적 된 ListView 컨트롤과 상호 작용할 수 있습니다.***

응용 프로그램 UI에 최적화 예제를 적용 하는 방법을 알아보려면 [컨트롤 그룹](#control-group) 섹션을 방문 하세요.

### <a name="interaction-and-commanding"></a>상호 작용 및 명령

컨트롤에 포커스가 있으면 사용자가 컨트롤과 상호 작용 하 고 특정 키보드 입력을 사용 하 여 연결 된 모든 기능을 호출할 수 있습니다.

#### <a name="text-entry"></a>텍스트 항목

및와 같은 텍스트 입력을 위해 특별히 디자인 된 컨트롤의 경우 `TextBox` `RichEditBox` 모든 키보드 입력은 텍스트를 입력 하거나 탐색 하는 데 사용 되며,이는 다른 키보드 명령 보다 우선적으로 적용 됩니다. 예를 들어 컨트롤의 드롭다운 메뉴는 `AutoSuggestBox` **Space** 키를 선택 명령으로 인식 하지 않습니다.

![텍스트 입력](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Space 키

텍스트 입력 모드에 있지 않은 경우에는 터치를 사용 하 여 탭 하거나 마우스를 클릭 하는 것 처럼 **공간** 키가 포커스가 있는 컨트롤과 연결 된 작업 또는 명령을 호출 합니다.

![space 키](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Enter 키

**Enter** 키를 누르면 포커스가 있는 컨트롤에 따라 다양 한 일반적인 사용자 상호 작용을 수행할 수 있습니다.
-   또는와 같은 명령 컨트롤을 `Button` 활성화 `Hyperlink` 합니다. 최종 사용자 혼란을 방지 하기 위해 **Enter** 키를 사용 하면 또는와 같은 명령 컨트롤과 같은 컨트롤을 활성화할 수도 있습니다 `ToggleButton` `AppBarToggleButton` .
-   및와 같은 컨트롤에 대 한 선택 UI를 표시 합니다 `ComboBox` `DatePicker` . 또한 **Enter** 키는 선택 UI를 커밋하고 닫습니다.
-   `ListView`,, 등의 목록 컨트롤을 활성화 `GridView` `ComboBox` 합니다.
    -   **Enter** 키를 선택 하면 해당 항목과 연결 된 추가 작업 (새 창 열기)이 없는 한 선택 작업이 목록 및 그리드 항목의 **공간** 키로 수행 됩니다.
    -   추가 동작이 컨트롤과 연결 된 경우에는 **Enter** 키를 사용 하 여 추가 작업을 수행 하 고 **Space** key는 선택 작업을 수행 합니다.

**참고** **Enter** 키와 **Space** 키는 항상 동일한 작업을 수행 하는 것은 아니지만 일반적으로 동일한 작업을 수행 합니다.

![키 입력](images/keyboard/enter-key.png)

Esc 키를 사용 하면 사용자가 임시 UI (해당 UI의 모든 진행 중인 작업과 함께)를 취소할 수 있습니다.

이러한 환경의 예는 다음과 같습니다.
-   사용자가 `ComboBox` 선택한 값을 가진을 열고 화살표 키를 사용 하 여 포커스 선택을 새 값으로 이동 합니다. Esc 키를 누르면가 닫히고 `ComboBox` 선택한 값이 원래 값으로 다시 설정 됩니다.
-   사용자가 전자 메일에 대 한 영구 삭제 작업을 호출 하 고 작업을 확인 하 라는 메시지가 표시 됩니다 `ContentDialog` . 사용자가 의도 한 작업이 아니라 **Esc** 키를 눌러 대화 상자를 닫습니다. **Esc** 키가 **취소** 단추와 연결 되 면 대화 상자가 닫히고 작업이 취소 됩니다. **Esc** 키는 일시적 UI에만 영향을 주며 앱 UI를 닫거나 뒤로 이동 하지는 않습니다.

![Esc 키](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Home 및 End 키

**Home** 및 **end** 키를 사용 하면 사용자가 UI 영역의 시작 이나 끝으로 스크롤할 수 있습니다.

이러한 환경의 예는 다음과 같습니다.
-   `ListView`및 컨트롤의 경우 `GridView` **Home** 키를 누르면 포커스가 첫 번째 요소로 이동 하 여 뷰로 스크롤됩니다. 반면, **최종** 키는 포커스를 마지막 요소로 이동 하 여 뷰로 스크롤합니다.
-   컨트롤의 경우에는 `ScrollView` **Home** 키가 영역의 맨 위로 스크롤되며 **최종** 키는 영역의 아래쪽으로 스크롤됩니다. 포커스는 변경 되지 않습니다.

![home 및 end 키](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Page up 및 Page down 키

**페이지** 키를 사용 하면 사용자가 개별 증가값으로 UI 영역을 스크롤할 수 있습니다.

예를 들어 `ListView` 및 컨트롤의 경우 `GridView` **page up** 키는 영역을 "page" (일반적으로 뷰포트 높이) 만큼 위로 스크롤하고 영역 위쪽으로 포커스를 이동 합니다. 또는 **page down** 키를 눌러 영역을 페이지 아래로 스크롤하고 영역 아래쪽으로 포커스를 이동 합니다.

![page up/down 키](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>바로 가기 키

바로 가기 키를 사용 하면 내게 필요한 옵션에 대 한 향상 된 지원과 키보드 사용자의 효율성 향상을 모두 제공 하 여 앱을 더 쉽게 사용할 수 있습니다.

앱에서 키보드 탐색 및 활성화를 지 원하는 것 외에도 응용 프로그램의 기능에 대 한 바로 가기를 제공 하는 것이 좋습니다. 탭 탐색 기능을 사용 하면 기본 제공 되는 기본 수준의 키보드 지원이 제공 되지만 보다 복잡 한 UI를 사용 하면 바로 가기 키에 대 한 지원도 추가할 수 있습니다. 

바로 가기는 사용자가 앱 기능에 액세스 하는 효율적인 방법을 제공 하 여 생산성을 향상 시키는 키보드 조합입니다. 다음과 같은 두 가지 종류의 바로 가기가 있습니다.
-   [액셀러레이터](#accelerators) 는 응용 프로그램 명령을 호출 하는 바로 가기입니다. 앱은 명령에 해당 하는 특정 UI를 제공 하거나 제공 하지 않을 수 있습니다. 액셀러레이터 키는 일반적으로 Ctrl 키와 문자 키로 구성 됩니다.
-   [액세스 키는](#access-keys) 응용 프로그램에서 특정 UI에 포커스를 설정 하는 바로 가기입니다. 액세스 키 typicaly는 Alt 키와 문자 키로 구성 됩니다.

응용 프로그램 간에 유사한 작업을 지 원하는 일관 된 바로 가기 키를 제공 하면 훨씬 더 유용 하 고 강력 하며 사용자를 쉽게 기억할 수 있습니다.

#### <a name="accelerators"></a>액셀러레이터

가속기를 통해 사용자는 응용 프로그램에서 일반적인 작업을 보다 빠르고 효율적으로 수행할 수 있습니다. 

액셀러레이터 키의 예:
-   **메일** 앱의 아무 곳에서 나 Ctrl + N 키를 누르면 새 메일 항목이 시작 됩니다.
-   Microsoft Edge의 어디에서 나 Ctrl + E 키를 누르면 (그리고 많은 Microsoft Store 응용 프로그램) 검색을 시작 합니다.

액셀러레이터에는 다음과 같은 특징이 있습니다.
-   주로 Ctrl 및 함수 키 시퀀스를 사용 합니다. Windows 시스템 바로 가기 키는 Alt + 영숫자가 아닌 키와 Windows 로고 키도 사용 합니다.
-   가장 일반적으로 사용 되는 명령에만 할당 됩니다.
-   이러한 문서는 memorized 수 있도록 설계 되었으며 메뉴, 도구 설명 및 도움말에만 설명 되어 있습니다.
-   이는 지원 될 때 전체 응용 프로그램 전체에 적용 됩니다.
-   Memorized는 일관 되 게 문서화 되지 않고 일관 되 게 할당 되어야 합니다.

#### <a name="access-keys"></a>액세스 키

UWP에서 액세스 키 지원에 대 한 자세한 내용은 [액세스 키](access-keys.md) 페이지를 참조 하세요.

액세스 키를 사용 하면 모터 기능 장애가 있는 사용자가 한 번에 한 키를 눌러 UI의 특정 항목에 대 한 작업을 수행할 수 있습니다. 또한 액세스 키를 사용 하 여 고급 사용자가 신속 하 게 작업을 수행할 수 있도록 추가 바로 가기 키를 전달할 수 있습니다.

액세스 키에는 다음과 같은 특징이 있습니다.
-   Alt 키와 영숫자 키를 사용 합니다.
-   주로 내게 필요한 옵션입니다.
-   [핵심 팁](access-keys.md)을 통해 컨트롤에 인접 하 여 UI에 직접 설명 되어 있습니다.
-   현재 창에만 적용 되 고 해당 메뉴 항목이 나 컨트롤로 이동 합니다.
-   가능 하면 일반적으로 사용 되는 명령 (특히 커밋 단추)에 액세스 키를 일관 되 게 할당 해야 합니다.
-   지역화 됩니다.

#### <a name="common-keyboard-shortcuts"></a>일반 바로 가기 키

다음 표는 자주 사용 되는 바로 가기 키의 작은 샘플입니다. 

| 작업                               | 키 명령                                      |
|--------------------------------------|--------------------------------------------------|
| 모두 선택                           | Ctrl+A                                           |
| 연속 선택                  | Shift + 화살표 키                                  |
| 저장                                 | Ctrl+S                                           |
| 찾기                                 | Ctrl+F                                           |
| 인쇄                                | Ctrl+P                                           |
| 복사                                 | Ctrl+C                                           |
| 잘라내기                                  | Ctrl+X                                           |
| 붙여넣기                                | Ctrl+V                                           |
| 실행 취소                                 | Ctrl+Z                                           |
| 다음 탭                             | Ctrl+Tab                                         |
| 탭 닫기                            | Ctrl + F4 또는 Ctrl + W                                |
| 시맨틱 줌                        | Ctrl + + 또는 Ctrl +-                                 |

Windows 시스템 바로 가기의 포괄적인 목록은 [windows 용 바로 가기 키](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts)를 참조 하세요. 일반적인 응용 프로그램 바로 가기에 대 한 자세한 내용은 [Microsoft 응용 프로그램의 바로 가기 키](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps)를 참조 하세요.

## <a name="advanced-experiences"></a>고급 환경

이 섹션에서는 다양 한 도구를 사용 하 여 다양 한 장치에서 앱을 사용 하는 경우 알고 있어야 하는 몇 가지 동작을 비롯 하 여 UWP 앱에서 지원 되는 보다 복잡 한 키보드 상호 작용 환경에 대해 설명 합니다.

### <a name="control-group"></a>컨트롤 그룹

화살표 키를 사용 하 여 "내부 탐색"을 가능 하 게 하는 "컨트롤 그룹" 또는 방향 영역에서 관련 컨트롤 또는 보완 컨트롤의 집합을 그룹화 할 수 있습니다. 컨트롤 그룹은 단일 탭 정지 일 수도 있고 컨트롤 그룹 내에서 여러 탭 정지를 지정할 수도 있습니다.

#### <a name="arrow-key-navigation"></a>화살표 키 탐색

UI 영역에 비슷한 관련 컨트롤 그룹이 있는 경우 사용자는 화살표 키 탐색을 지원 합니다.
-   `AppBarButtons` 에서 `CommandBar`
-   `ListItems`또는 `GridItems` 내부 `ListView``GridView`
-   `Buttons` 내부의 `ContentDialog`

UWP 컨트롤은 기본적으로 화살표 키 탐색을 지원 합니다. 사용자 지정 레이아웃 및 컨트롤 그룹의 경우 `XYFocusKeyboardNavigation="Enabled"` 를 사용 하 여 유사한 동작을 제공 합니다.

다음 컨트롤을 사용할 때 화살표 키 탐색 지원을 추가 하는 것이 좋습니다.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>대화 상자 단추</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>라디오</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>App바 단추</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems 및 GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>탭 정지

응용 프로그램의 기능 및 레이아웃에 따라 컨트롤 그룹에 대 한 가장 적합 한 탐색 옵션은 자식 요소, 여러 탭 정지 또는 일부 조합에 대 한 화살표 탐색이 있는 단일 탭 정지 일 수 있습니다.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>단추에 대해 여러 탭 정지 및 화살표 키 사용

접근성 사용자는 일반적으로 화살표 키를 사용 하 여 단추 컬렉션을 탐색 하지 않는 잘 설정 된 키보드 탐색 규칙을 사용 합니다. 그러나 시각 장애가 없는 사용자는 동작이 자연스럽 게 느껴질 수 있습니다.

이 경우 기본 UWP 동작의 예는 `ContentDialog` 입니다. 화살표 키를 사용 하 여 단추 간을 탐색할 수 있지만 각 단추는 탭 정지 이기도 합니다.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>친숙 한 UI 패턴에 단일 탭 정지 할당

레이아웃이 컨트롤 그룹에 대해 잘 알려진 UI 패턴을 따르는 경우 그룹에 단일 탭 정지를 할당 하면 사용자의 탐색 효율성이 향상 될 수 있습니다.

다음은 이러한 템플릿의 예입니다.
-   `RadioButtons`
-   `ListViews`처럼 보이지만 단일 처럼 동작 합니다.`ListView`
-   타일의 그리드와 같이 모양과 동작 하는 UI (예: 시작 메뉴 타일)

#### <a name="specifying-control-group-behavior"></a>컨트롤 그룹 동작 지정

다음 Api를 사용 하 여 사용자 지정 컨트롤 그룹 동작을 지원 합니다. 모두이 항목의 뒷부분에서 자세히 설명 합니다.

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) 컨트롤 간의 화살표 키 탐색을 사용 합니다.
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) 는 여러 탭 정지 또는 단일 탭 정지가 있는지 여부를 나타냅니다.
-   [FindFirstFocusableElement 및 FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) 는 **Home** 키가 있는 첫 번째 항목 및 **끝** 키가 있는 마지막 항목에 포커스를 설정 합니다.

다음 이미지는 연결 된 라디오 단추의 컨트롤 그룹에 대 한 직관적인 키보드 탐색 동작을 보여 줍니다. 이 경우 컨트롤 그룹에 대해 단일 탭 정지를 사용 하 고 화살표 키를 사용 하는 라디오 단추, 첫 번째 라디오 단추에 바인딩된 **홈** 키, 마지막 라디오 단추에 바인딩된 **끝** 키 사이에서 내부 탐색을 사용 하는 것이 좋습니다.

![모두 함께 배치](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>키보드 및 내레이터

내레이터는 키보드 사용자를 중심으로 하는 UI 접근성 도구입니다 (다른 입력 유형도 지원 됨). 그러나 내레이터 기능은 UWP 앱에서 지원 되는 키보드 상호 작용을 벗어나,이를 위해 UWP 앱을 설계할 때 추가 주의가 필요 합니다. [내레이터 기본 사항 페이지](https://support.microsoft.com/help/22808/windows-10-narrator-basics) 에서 내레이터 사용자 환경을 안내 합니다.

UWP 키보드 동작 및 내레이터가 지 원하는 몇 가지 차이점은 다음과 같습니다.
-   표준 키보드 탐색을 통해 노출 되지 않는 UI 요소에 대 한 탐색을 위한 추가 키 조합 (예: 컨트롤 레이블을 읽도록 Caps lock + 화살표 키)
-   비활성화 된 항목으로 이동 합니다. 기본적으로 비활성화 된 항목은 표준 키보드 탐색을 통해 노출 되지 않습니다.
-   UI 세분성을 기반으로 더 빠른 탐색을 위해 "뷰"를 제어 합니다. 사용자는 항목, 문자, 단어, 줄, 단락, 링크, 머리글, 표, 랜드마크 및 제안으로 이동할 수 있습니다. 표준 키보드 탐색은 이러한 개체를 기본 목록으로 표시 합니다. 바로 가기 키를 제공 하지 않으면 탐색 하기가 번거로울 수 있습니다.

#### <a name="case-study--autosuggestbox-control"></a>사례 연구 – AutoSuggestBox 컨트롤

`AutoSuggestBox`사용자가 **enter** 키를 눌러 검색 쿼리를 전송할 수 있으므로의 검색 단추는 tab 키와 화살표 키를 사용 하 여 표준 키보드 탐색에 액세스할 수 없습니다. 그러나 사용자가 Caps Lock + 화살표 키를 누를 때 내레이터를 통해 액세스할 수 있습니다.

![키보드 포커스 autosuggest](images/keyboard/auto-suggest-keyboard.png)

*키보드를 사용 하 여 사용자* 는 ***enter*** 키를 눌러 *검색 쿼리를 제출* 합니다.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>내레이터를 사용 하 여 사용자는 <strong>enter</strong> 키를 눌러 검색 쿼리를 제출 합니다.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>내레이터를 사용 하면 사용자는 <strong>Caps lock + Right 화살표 키</strong>를 사용 하 여 검색 단추에 액세스 한 다음 <strong>Space</strong> 키를 누를 수도 있습니다.</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>키보드 및 Xbox 게임 및 원격 제어

Xbox gamepads 및 원격 제어는 많은 UWP 키보드 동작 및 환경을 지원 합니다. 그러나 키보드에서 사용할 수 있는 다양 한 키 옵션의 부족으로 인해 게임 패드 및 원격 제어에 많은 키보드 최적화가 없습니다 (원격 제어는 게임 패드 보다 훨씬 제한적 임).

게임 패드 및 원격 제어 입력에 대 한 UWP 지원에 대 한 자세한 내용은 [게임 패드 및 원격 제어 상호 작용](gamepad-and-remote-interactions.md) 을 참조 하세요.

다음은 키보드, 게임 패드 및 원격 제어 간의 몇 가지 주요 매핑을 보여 줍니다.

| **키보드**  | **게임 패드**                         | **원격 제어**  |
|---------------|-------------------------------------|---------------------|
| Space         | 단추                            | 선택 단추       |
| Enter         | 단추                            | 선택 단추       |
| 이스케이프        | B 단추                            | 뒤로 단추         |
| 홈/끝      | 해당 없음                                 | 해당 없음                 |
| Page Up/Down  | 세로 스크롤에 대 한 트리거 단추, 가로 스크롤에 대 한 범퍼 단추   | 해당 없음                 |

게임 패드 및 원격 제어 사용에 사용할 UWP 앱을 디자인할 때 알아야 할 몇 가지 주요 차이점은 다음과 같습니다.
-   텍스트 항목을 사용 하려면 사용자가를 눌러 텍스트 컨트롤을 활성화 해야 합니다.
-   포커스 탐색은 컨트롤 그룹으로 제한 되지 않으며, 사용자는 앱에서 포커스를 받을 수 있는 UI 요소로 자유롭게 이동할 수 있습니다.

    **참고** 오버레이 UI에 없거나 포커스 [참여가](gamepad-and-remote-interactions.md#focus-engagement) 지정 된 경우를 제외 하 고 포커스는 키 누름 방향의 포커스 가능 UI 요소로 이동할 수 있습니다 .이로 인해 포커스가 단추를 사용 하 여 사용/해제할 때까지 영역을 시작 하거나 종료할 수 없습니다. 자세한 내용은 [방향 탐색](#directional-navigation) 섹션을 참조 하세요.
-   D-패드 및 왼쪽 스틱 단추는 컨트롤과 내부 탐색을 위해 포커스를 이동 하는 데 사용 됩니다.

    **참고** 게임 패드 및 원격 제어는 방향 키를 누른 경우와 동일한 시각적 순서로 있는 항목만 탐색 합니다. 포커스를 받을 수 있는 후속 요소가 없는 경우에는 해당 방향으로 탐색을 사용할 수 없습니다. 상황에 따라 키보드 사용자에 게는 항상 해당 제약 조건이 없습니다. 자세한 내용은 [기본 제공 키보드 최적화](#built-in-keyboard-optimization) 섹션을 참조 하세요.

#### <a name="directional-navigation"></a>방향 탐색

방향 탐색은 (화살표 키, D-패드) 방향 키를 누른 상태에서 해당 시각적 방향으로 포커스를 이동 하려고 하는 UWP [Focus 관리자](/uwp/api/Windows.UI.Xaml.Input.FocusManager) 도우미 클래스를 통해 관리 됩니다.

키보드와는 달리, 앱이 [마우스 모드](gamepad-and-remote-interactions.md#mouse-mode)에서 opts 면 전체 응용 프로그램에 대 한 방향성 탐색이 게임 패드 및 원격 제어에 적용 됩니다. 방향성 탐색 최적화에 대 한 자세한 내용은 [게임 패드 및 원격 제어 상호 작용](gamepad-and-remote-interactions.md) 을 참조 하세요.

**참고** 키보드 Tab 키를 사용 하는 탐색은 방향 탐색으로 간주 되지 않습니다. 자세한 내용은 [탭 정지](#tab-stops) 섹션을 참조 하세요.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>방향성 탐색 지원</strong></br>사용자는 방향 키 (키보드 화살표, 게임 패드 및 원격 제어)를 사용 하 여 여러 컨트롤 간에 이동할 수 있습니다.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>방향 탐색이 지원 되지 않음</strong> </br>사용자는 방향 키를 사용 하는 여러 컨트롤 간에 이동할 수 없습니다. 컨트롤 (tab 키) 사이를 탐색 하는 다른 방법은 영향을 받지 않습니다.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>기본 제공 키보드 최적화

사용 되는 레이아웃 및 컨트롤에 따라 UWP 앱은 키보드 입력 전용으로 최적화 될 수 있습니다.

다음 예제에서는 단일 탭 정지에 할당 된 목록 항목, 그리드 항목 및 메뉴 항목 그룹을 보여 줍니다 ( [탭 정지](#tab-stops) 섹션 참조). 그룹에 포커스가 있는 경우 해당 시각적 순서 ( [탐색](#navigation) 섹션 참조)에서 방향 화살표 키를 사용 하 여 내부 탐색을 수행 합니다.

![단일 열 화살표 키 탐색](images/keyboard/single-column-arrow.png)

***단일 열 화살표 키 탐색***

![단일 행 화살표 키 탐색](images/keyboard/single-row-arrow.png)

***단일 행 화살표 키 탐색***

![여러 열 및 행 화살표 키 탐색](images/keyboard/multiple-column-and-row-navigation.png)

***여러 열/행 화살표 키 탐색***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>같은 항목 목록 및 그리드 보기 항목 래핑

방향성 탐색은 항상 목록과 GridView 항목의 여러 행과 열을 탐색 하는 가장 효율적인 방법이 아닙니다.

**참고** 메뉴 항목은 일반적으로 단일 열 목록 이지만 특별 한 포커스 규칙이 적용 될 수도 있습니다 ( [팝업 UI](#popup-ui)참조).

여러 행과 열을 사용 하 여 목록 및 표 개체를 만들 수 있습니다. 이러한 항목은 일반적으로 행이 중요 합니다. 여기서 항목은 다음 행을 채우기 전에 전체 행을 채우도록 하는 경우이 고, 다음 열에 채우기 전에 먼저 전체 열을 채우는 항목입니다. 행 또는 열 주 순서는 스크롤 방향에 따라 다르며, 항목 순서가이 방향과 충돌 하지 않는지 확인 해야 합니다.

행의 마지막 항목에 포커스가 있고 오른쪽 화살표 키를 누르는 경우 행 우선 순위 (왼쪽에서 오른쪽으로, 위쪽에서 아래쪽으로 채우기)에서 포커스가 다음 행의 첫 번째 항목으로 이동 합니다. 이와 동일한 동작이 역순으로 발생 합니다. 즉, 포커스가 행의 첫 번째 항목으로 설정 되 고 왼쪽 화살표 키를 누르면 포커스가 이전 행의 마지막 항목으로 이동 합니다.

열이 중요 한 순서 (위에서 아래로, 왼쪽에서 오른쪽으로 입력 하는 경우)에서 포커스가 열의 마지막 항목에 있고 사용자가 아래쪽 화살표 키를 누르면 포커스가 다음 열의 첫 번째 항목으로 이동 합니다. 이와 동일한 동작이 역방향으로 수행 됩니다. 포커스를 열의 첫 번째 항목으로 설정 하 고 위쪽 화살표 키를 누르면 포커스가 이전 열의 마지막 항목으로 이동 합니다.

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

#### <a name="popup-ui"></a>Popup UI

앞서 언급 했 듯이 방향 탐색을 응용 프로그램 UI에 있는 컨트롤의 시각적 순서와 일치 하는지 확인 해야 합니다.

상황에 맞는 메뉴, 명령 모음 오버플로 메뉴, AutoSuggest 메뉴 등의 일부 컨트롤에는 기본 컨트롤 및 사용 가능한 화면 공간에 상대적인 위치 및 방향 (기본적으로 아래쪽)에 메뉴 팝업이 표시 됩니다. 열기 방향은 런타임에 다양 한 요인에 의해 영향을 받을 수 있습니다.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

이러한 컨트롤의 경우 메뉴가 처음 열릴 때 (사용자가 항목을 선택 하지 않은 경우) 아래쪽 화살표 키를 누르면 항상 첫 번째 항목으로 포커스를 설정 하는 반면 위쪽 화살표 키는 항상 메뉴의 마지막 항목으로 포커스를 설정 합니다. 

마지막 항목에 포커스가 있고 아래쪽 화살표 키를 누르면 포커스가 메뉴의 첫 번째 항목으로 이동 합니다. 마찬가지로, 첫 번째 항목에 포커스가 있는 상태에서 위쪽 화살표 키를 누르면 포커스가 메뉴의 마지막 항목으로 이동 합니다. 이 동작을 *순환* 이라고 하며 예측할 수 없는 방향으로 열 수 있는 팝업 메뉴를 탐색 하는 데 유용 합니다.

> [!NOTE]
> 사용자가 무한 루프에서 트랩 된 느낌을 받을 수 있는 비 팝업 Ui에서는 순환을 피해 야 합니다. 

사용자 지정 컨트롤에서 이와 동일한 동작을 에뮬레이트하는 것이 좋습니다. 이 동작을 구현 하는 방법에 대 한 코드 샘플은 [프로그래밍 방식 포커스 탐색](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) 설명서에서 찾을 수 있습니다.


## <a name="test-your-app"></a>앱 테스트

지원 되는 모든 입력 장치를 사용 하 여 앱을 테스트 하 여 UI 요소를 일관 되 고 직관적인 방식으로 탐색 하 고 예기치 않은 요소가 원하는 탭 순서를 방해 하지 않도록 합니다.

## <a name="related-articles"></a>관련된 문서
* [키보드 이벤트](keyboard-events.md)
* [입력 디바이스 식별](identify-input-devices.md)
* [터치 키보드의 현재 상태에 응답](respond-to-the-presence-of-the-touch-keyboard.md)
* [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
* [NavigationView 컨트롤 키보드 사용 특성](../controls-and-patterns/navigationview.md#hierarchical-navigation) 

## <a name="appendix"></a>부록

### <a name="software-keyboard"></a>소프트웨어 키보드

소프트웨어 키보드는 터치, 마우스, 펜/스타일러스 또는 다른 포인팅 장치를 사용 하 여 데이터를 입력 하 고 입력 하는 데 사용자가 실제 키보드 대신 사용할 수 있는 화면에 표시 되는 키보드입니다 (터치 화면은 필요 하지 않음). 터치 스크린에서 이러한 키보드는 텍스트를 직접 입력할 수 있습니다. Xbox One 장치에서 포커스 시각적 개체를 이동 하거나 게임 패드 또는 원격 제어를 사용 하 여 바로 가기 키를 사용 하 여 개별 키를 선택 해야 합니다.

![Windows 10 touch 키보드](images/keyboard/default.png)

***Windows 10 Touch 키보드***

![Xbox one 온스크린 키보드](images/keyboard/xbox-onscreen-keyboard.png)

***Xbox One 온스크린 키보드***

장치에 따라 텍스트 필드나 기타 편집 가능한 텍스트 컨트롤이 포커스를 가져오거나 사용자가 **알림 센터**를 통해 수동으로 사용 하도록 설정 하면 소프트웨어 키보드가 나타납니다.

![알림 센터의 터치 키보드 아이콘](images/keyboard/touch-keyboard-notificationcenter.png)

앱에서 프로그래밍 방식으로 텍스트 입력 컨트롤로 포커스를 설정 하는 경우 터치 키보드는 호출 되지 않습니다. 이렇게 하면 사용자가 직접 간섭 하지 않는 예기치 않은 동작이 제거 됩니다. 그러나 프로그래밍 방식으로 텍스트가 아닌 입력 컨트롤로 포커스를 이동할 때 키보드는 자동으로 숨겨집니다.

터치 키보드는 일반적으로 사용자가 양식에서 컨트롤 간을 탐색 하는 동안 표시 됩니다. 이 동작은 폼 내의 다른 컨트롤 형식에 따라 달라질 수 있습니다.

다음은 키보드를 해제 하지 않고 터치 키보드를 사용 하 여 텍스트 입력 세션 중에 포커스를 받을 수 있는 편집 되지 않는 컨트롤의 목록입니다. 사용자가 터치 키보드를 사용 하 여 이러한 컨트롤과 텍스트 항목 사이에서 앞뒤로 이동할 가능성이 있으므로 UI를 불필요 하 게 이탈 하 고 사용자의 상태를 벗어날 때 터치 키보드는 계속 표시 됩니다.

-   확인란
-   콤보 상자
-   라디오 단추
-   스크롤 막대
-   트리
-   트리 항목
-   메뉴
-   메뉴 모음
-   Menu item
-   도구 모음
-   목록
-   목록 항목

터치 키보드에 대 한 다양 한 모드의 예는 다음과 같습니다. 첫 번째 이미지는 기본 레이아웃 이며, 두 번째 이미지는 thumb 레이아웃 (일부 언어에서는 사용할 수 없음)입니다.

![기본 레이아웃 모드의 터치 키보드](images/keyboard/default.png)

***기본 레이아웃 모드의 터치 키보드***

![확장 된 레이아웃 모드의 터치 키보드](images/keyboard/extendedview.png)

***확장 된 레이아웃 모드의 터치 키보드***

키보드 상호 작용 성공으로 사용자는 키보드만 사용 하 여 기본 앱 시나리오를 수행할 수 있습니다. 즉, 사용자가 모든 대화형 요소에 연결 하 고 기본 기능을 활성화할 수 있습니다. 키보드 탐색, 내게 필요한 옵션에 대 한 액세스 키, 고급 사용자를 위한 액셀러레이터 키 (또는 바로 가기 키)를 비롯 한 다양 한 요소가 성공 수준에 영향을 줄 수 있습니다.

**참고**    터치 키보드는 토글 및 대부분의 시스템 명령을 지원 하지 않습니다.

#### <a name="on-screen-keyboard"></a>화상 키보드
소프트웨어 키보드와 마찬가지로 화상 키보드는 터치, 마우스, 펜/스타일러스 또는 다른 포인팅 장치를 사용 하 여 데이터를 입력 하 고 입력 하는 실제 키보드 대신 사용할 수 있는 시각적 소프트웨어 키보드입니다 (터치 화면은 필요 하지 않음). 화상 키보드는 물리적 키보드를 사용 하지 않는 시스템이 나 이동성 장애가 있는 사용자가 기존의 물리적 입력 장치를 사용 하지 못하도록 하는 데 제공 됩니다. 화상 키보드는 하드웨어 키보드의 기능을 대부분 에뮬레이션 합니다.

화상 키보드는 설정 쉬운 액세스의 키보드 페이지에서 설정할 수 있습니다 &gt; .

**참고** 화상 키보드가 터치 키보드 보다 우선 하므로 화상 키보드가 있는 경우 표시 되지 않습니다.

![화상 키보드](images/keyboard/osk.png)

***화상 키보드***

화상 키보드에 대 한 자세한 내용은 화상 [키보드 페이지](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard) 를 참조 하세요.

## <a name="related-articles"></a>관련된 문서

- [키보드 접근성](../accessibility/keyboard-accessibility.md)