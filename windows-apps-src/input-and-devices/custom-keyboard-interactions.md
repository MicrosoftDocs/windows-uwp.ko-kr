---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 010663320b4011d853c53bc4121f86a14bfbfe0c
ms.sourcegitcommit: a2908889b3566882c7494dc81fa9ece7d1d19580
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/31/2017
---
# <a name="custom-keyboard-interactions"></a>사용자 지정 키보드 조작

키보드 고급 사용자와 장애 및 기타 접근성 요구 사항이 있는 사용자를 위해 UWP 앱 및 사용자 지정 컨트롤에 포괄적이고 일관적인 키보드 조작 환경을 제공하세요.

이 항목에서는 PC의 UWP 앱에 대한 사용자 지정 컨트롤을 통해 키보드 입력을 지원하는 방법을 집중적으로 다룹니다. 하지만 Windows 내레이터 같은 접근성 도구를 지원하고 터치 키보드 및 OSK(화상 키보드) 같은 소프트웨어 키보드를 사용하려면 적절하게 설계된 키보드 환경이 중요합니다.

## <a name="providing-2d-directional-inner-navigation-a-namexyfocuskeyboardnavigation"></a>2D 방향 내부 탐색 제공 <a name="xyfocuskeyboardnavigation">

**XYFocusKeyboardNavigation** 속성을 사용하여 키보드(화살표 키), Xbox 게임 패드(D 패드 및 왼쪽 스틱 단추) 및 Xbox 리모컨(D 패드)으로 사용자 지정 컨트롤 및 컨트롤 그룹의 2D 방향 내부 탐색을 지원합니다.

**참고** 컨트롤 또는 컨트롤 그룹의 내부 탐색 영역을 *방향 영역*이라고 부릅니다.

**XYFocusKeyboardNavigation**에는 **XYFocusKeyboardNavigationMode** 유형의 값이 있으며 가능한 값은 **자동**(기본값), **사용** 또는 **사용 안 함**입니다.

이 속성은 탭 탐색에 영향을 주지 않고, 컨트롤 또는 컨트롤 그룹 내부의 자식 요소 내부 탐색에만 영향을 줍니다. 방향 영역의 자녀 요소는 탭 탐색에 포함되면 안 됩니다.

### <a name="default-behavior"></a>기본 동작

방향 탐색 동작은 요소의 상위 구조 또는 상속 계층을 기반으로 합니다. 모든 상위 구조가 기본 모드이거나 **자동**으로 설정되면 키보드에 방향 탐색 동작이 지원되지 않습니다(게임 패드 및 리모컨은 명시적으로 **사용 안 함**으로 설정하지 않는 한 항상 방향 탐색을 지원함).

### <a name="custom-behavior"></a>사용자 지정 동작

이 속성을 **사용**으로 설정하면 컨트롤 내의 모든 UIElement에서 컨트롤이 (키보드 화살표 키를 사용한) 2D 내부 탐색을 지원합니다.

키보드 화살표 키를 사용하는 경우 방향 영역 내에서 탐색이 제한됩니다(탭 키를 누르면 방향 영역 외부에 있는 그 다음 포커스 가능 요소에 포커스가 설정됨).

**참고** 게임 패드 또는 리모컨을 사용하는 경우에는 이 제한이 적용되지 않으며, 방향 영역 외부에서 그 다음 포커스 가능 컨트롤까지 탐색이 계속됩니다.

이 속성은 화살표 키를 사용한 내부 탐색에만 영향을 주며, 탭 키 탐색에는 영향을 주지 않습니다. 모든 컨트롤은 예상되는 탭 순서 계층을 유지합니다.

다음 그림은 방향 영역에 포함된 세 개의 단추(A, B 및 C)와 방향 영역 외부에 있는 네 번째 단추(D)를 보여 줍니다.

![방향 영역](images/keyboard/directional-area.png)

*키보드 화살표 키를 사용하여 단추 A-B-C 사이에서 포커스를 이동할 수 있지만 D로는 이동할 수 없음*

다음 코드 예제는 **XYFocusKeyboardNavigation**을 사용할 때 탐색에 미치는 영향을 보여 줍니다. 이전 이미지를 사용하는 경우 먼저 A에 포커스가 있고 탭 키는 모든 컨트롤을 순환(A -&gt; B -&gt; C -&gt; D 그리고 다시 A로)하는 반면 화살표 키는 방향 영역으로 제한됩니다.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" />
      </StackPanel>
      <Button Content="D" />
</StackPanel>
```

#### <a name="override-directional-navigation"></a>방향 탐색 재정의

기본 탐색 동작을 재정의하려면 XYFocusRight/XYFocusLeft/XYFocusTop/XYFocusDown 속성을 사용합니다.

다음은 이전 예제와 똑같은 그림으로 방향 영역에 포함된 세 개의 단추(A, B 및 C)와 방향 영역 외부의 네 번째 단추(D)를 보여 줍니다.

![방향 영역](images/keyboard/directional-area.png)

*키보드 화살표 키를 사용하여 단추 A-B-C 사이에 그리고 외부에 있는 D로 포커스를 이동할 수 있습니다.*

이 코드 샘플은 방향 영역 외부에 있는 컨트롤을 탐색할 수 있도록 허용하여 오른쪽 화살표 키의 기본 탐색 동작을 재정의하는 방법을 보여 줍니다. 왼쪽 화살표 키를 사용하여 방향 영역을 다시 입력할 수 없습니다.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" XYFocusRight="{x:Bind ButtonD}" />
      </StackPanel>
      <Button Content="D" x:Name="ButtonD"/>
</StackPanel>
```

자세한 내용은 이 항목 뒷부분의 [XYFocus 탐색 전략](#set-the-tab-navigation-behavior) 섹션을 참조하세요.

#### <a name="restrict-navigation-with-disabled"></a>사용 안 함으로 탐색 제한

방향 영역 내에서 화살표 키 탐색을 제한하려면 **XYFocusKeyboardNavigation**을 **사용 안 함**으로 설정하세요.

**참고** 이 속성을 설정하면 컨트롤 자체에 대한 키보드 탐색, 컨트롤의 자녀 요소에 영향을 줍니다.

다음 코드 예제에서 부모 StackPanel의 **XYFocusKeyboardNavigation**은 **사용**으로, 자녀 요소 C의 **XYFocusKeyboardNavigation**은 **사용 안 함**으로 설정되었습니다. 자녀 요소 C만 화살표 키 탐색을 사용할 수 없습니다.

```XAML
<StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
        <Button Content="A" />
        <Button Content="B" />
        <Button Content="C" XYFocusKeyboardNavigation="Disabled" >
            ...
        </Button>
</StackPanel>
```

#### <a name="use-nested-directional-areas"></a>중첩된 방향 영역 사용

다양한 수준의 중첩된 방향 영역을 사용할 수 있습니다. 모든 부모 요소에서 **XYFocusKeyboardNavigation**이 **사용**으로 설정되면 화살표 키 탐색에서 지역 경계가 무시됩니다.

다음 그림은 중첩된 방향 영역에 포함된 세 개의 단추(A, B 및 C)와 방향 영역 외부에 있는 네 번째 단추(D)를 보여 줍니다.

![중첩된 방향 영역](images/keyboard/nested-directional-area.png)

*키보드 화살표 키를 사용하여 단추 A-B-C 사이에서 포커스를 이동할 수 있지만 D로는 이동할 수 없음*

이 코드 예제는 지역 경계에서 화살표 키 탐색을 지원하는 중첩된 방향 영역을 지정하는 방법을 보여 줍니다.

```XAML
<StackPanel Orientation="Horizontal">
        <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
            <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
                <Button Content="A" />
                <Button Content="B" />
            </StackPanel>
            <Button Content="C" />
        </StackPanel>
        <Button Content="D" />
 </StackPanel>
```

다음 그림은 A 및 B는 중첩된 방향 영역에 포함되어 있고 C 및 D는 다른 영역에 포함되어 있는 네 개의 단추(A, B, C 및 D)를 보여 줍니다. 부모 요소에서 **XYFocusKeyboardNavigation**이 **사용 안 함**으로 설정되었기 때문에 화살표 키를 사용하여 중첩된 각 영역의 경계를 통과할 수 없습니다.

![비 방향 영역](images/keyboard/non-directional-area.png)

*키보드 화살표 키를 사용하여 단추 A-B 사이와 단추 C-D 사이에서 포커스를 이동할 수 있지만 지역 간에는 이동할 수 없음*

이 코드 예제는 지역 경계에서 화살표 키 탐색을 지원하지 않는 중첩된 방향 영역을 지정하는 방법을 보여 줍니다.

```XAML
<StackPanel Orientation="Horizontal">
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="A" />
    <Button Content="B" />
  </StackPanel>
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="C" />
    <Button Content="D" />
  </StackPanel>
</StackPanel>
```

다음은 좀 더 복잡한 중첩된 방향 영역의 예입니다.

-   화살표 키로 B, C, D에 도달할 수 없는 비 방향 영역 경계가 있기 때문에 A에 포커스가 있는 경우 E만 탐색할 수 있습니다(반대의 경우도 마찬가지).
-   D가 방향 영역 외부에 있고 비 방향 영역 경계가 A 및 E에 대한 액세스를 차단하기 때문에 B에 포커스가 있는 경우 C만 탐색할 수 있습니다(반대의 경우도 마찬가지).
-   D에 포커스가 있는 경우 화살표 키 탐색을 사용할 수 없으므로 Tab 키를 사용하여 컨트롤을 탐색해야 합니다.

![중첩된 비 방향 영역](images/keyboard/nested-non-directional-area.png)

*키보드 화살표 키를 사용하여 단추 A-E 사이와 단추 B-C 사이에서 포커스를 이동할 수 있지만 다른 지역 간에는 이동할 수 없음*

이 코드 예제는 지역 경계에서 복잡한 화살표 키 탐색을 지원하는 중첩된 방향 영역을 지정하는 방법을 보여 줍니다.

```XAML
<StackPanel  Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
  <Button Content="A" />
    <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Disabled">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
        <Button Content="B" />
        <Button Content="C" />
      </StackPanel>
        <Button Content="D" />
    </StackPanel>
  <Button Content="E" />
</StackPanel>
```

## <a name="set-the-tab-navigation-behavior-a-nametab-navigation"></a>탭 탐색 동작 설정 <a name="tab-navigation">

UIElement.[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) 속성은 전체 개체 트리(또는 방향 영역)의 탭 탐색 동작을 지정합니다.

[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation)에는 **TabFocusNavigationMode** 유형의 값이 있으며 가능한 값은 **한 번**, **주기** **또는 로컬**(기본값)입니다.

다음 그림에서는 방향 영역의 탭 탐색에 따라 포커스가 다음과 같은 방식으로 이동합니다.

-   한 번: A, B1, C, A
-   로컬: A, B1, B2, B3, B4, B5, C, A
-   주기: A, B1, B2, B3, B4, B5, B1, B2, B3, B4, B5, (B에서 순환)

![탭 탐색](images/keyboard/tab-navigation.png)

*탭 탐색 모드 기반의 포커스 동작*

다음은 TabFocusNavigation을 한 번 모드로 사용할 때의 코드 예제입니다.

```XAML
<Button Content="X" Click="OnAClick"/>
<StackPanel Orientation="Horizontal" XYFocusNavigation ="Local"
   TabFocusNavigation ="Local">
   <Button Content="A" Click="OnAClick"/>
   <StackPanel Orientation="Horizontal" TabFocusNavigation ="Once">
        <Button Content="B" Click="OnBClick"/>
        <Button Content="C" Click="OnCClick"/>
        <Button Content="D" Click="OnDClick"/>
   </StackPanel>
   <Button Content="E" Click="OnBClick"/>
</StackPanel>
```

*X에 포커스가 있을 때의 Tab 탐색: A,B,E,X*

#### <a name="about-tabfocusnavigation-and-tabindex"></a>TabFocusNavigation 및 TabIndex에 대한 정보

UIElement.TabFocusNavigation 속성은 TabIndex 작업 방식을 포함하여 Control.TabNavigation 속성과 똑같은 동작을 갖고 있습니다.

컨트롤에 TabIndex가 지정되지 않은 경우 프레임워크는 해당 컨트롤에 현재 최고 인덱스 값보다 높은 인덱스 값(그리고 가장 낮은 우선 순위)을 할당합니다. 시각적 트리의 첫 번째 요소를 선택하여 모호성을 해결합니다. 프레임워크는 범위별로 Tab 인덱스를 해결합니다. 컨트롤의 자녀는 범위로 간주되며, 이 자녀 중 하나에 자녀가 있으면 해당 자녀는 다른 범위에 포함된 것입니다.

다음 그림에서는 방향 영역의 탭 탐색 및 요소의 탭 인덱스에 따라 포커스가 다음과 같은 방식으로 이동합니다.

-   한 번: A, B3, C, A.
-   로컬: A, B3, B4, B5, B1, B2, C, A.
-   주기: A, B3, B4, B5, B1, B2, B3, B4, B5, B1, B2, (B에서 순환)

![탭 인덱스](images/keyboard/tab-index.png)

*탭 탐색 모드 및 탭 인덱스 기반의 포커스 동작*

방향 영역이 범위로 간주되는 원리 그리고 포커스 탐색이 우선 순위가 가장 높은 컨트롤인 B3로 가장 먼저 이동하는 원리를 살펴보세요. 사실, 두 가지 범위가 있습니다. 하나는 A, 방향 영역 및 C에 대한 것이고 다른 하나는 방향 영역에 대한 것입니다. 방향 영역은 TabStop이 아니기 때문에 프레임워크가 범위를 전환하여 최적의 후보를 찾은 후 모든 자녀 요소를 반복적으로 살펴봅니다.
