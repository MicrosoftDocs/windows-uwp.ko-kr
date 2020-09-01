---
description: 현재 활성 테마에 따라 다른 리소스를 검색 하는 추가 시스템 논리를 사용 하 여 리소스에 대 한 참조를 계산 함으로써 모든 XAML 특성에 대 한 값을 제공 합니다.
title: 태그 확장
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 89ae286fbc29628e8d81899265019ccdaaa038a3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161707"
---
# <a name="themeresource-markup-extension"></a>{ThemeResource} 태그 확장

현재 활성 테마에 따라 다른 리소스를 검색 하는 추가 시스템 논리를 사용 하 여 리소스에 대 한 참조를 계산 함으로써 모든 XAML 특성에 대 한 값을 제공 합니다. [{StaticResource} 태그 확장과](staticresource-markup-extension.md)유사 하 게 리소스는 [**resourcedictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 정의 되 고, 지 수 정의는 **resourcedictionary**에서 해당 리소스의 **키를 참조** 합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{ThemeResource key}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| key | 요청한 리소스의 키입니다. 이 키는 초기에 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 의해 할당 됩니다. 리소스 키는 XamlName 문법에 정의 된 임의의 문자열일 수 있습니다. |

## <a name="remarks"></a>설명

특성 **은 xaml** 리소스 사전의 다른 곳에서 정의 된 xaml 특성에 대 한 값을 가져오는 기술입니다. 태그 확장은 [{StaticResource} 태그 확장과](staticresource-markup-extension.md)동일한 기본 용도로 사용 됩니다. 동작 및 {StaticResource} 태그 확장의 차이점은 기능이 현재 시스템에서 사용 중인 **테마에 따라 다양 한 사전을** 기본 조회 위치로 동적으로 사용할 수 있다는 것입니다.

앱이 처음 시작 될 때 **에는 시작** 시 사용 중인 테마를 기반으로 하는 모든 리소스 참조가 평가 됩니다. 그러나 이후에 사용자가 런타임에 활성 테마를 변경 하는 경우 시스템 **은 모든 기능** 참조를 다시 평가 하 고, 다를 수 있는 테마별 리소스를 검색 하 고, 시각적 트리의 적절 한 모든 위치에서 새 리소스 값으로 앱을 다시 표시 합니다. **StaticResource** 는 XAML 로드 시간/앱 시작 시 결정 되며 런타임에 다시 계산 되지 않습니다. XAML을 동적으로 다시 로드 하는 시각적 상태와 같은 다른 기술이 있지만 이러한 기술은 [{StaticResource} 태그 확장](staticresource-markup-extension.md)에서 기본 리소스 평가를 사용 하도록 설정 하는 상위 수준에서 작동 합니다.

필요한 리소스에 대 한 키를 지정 하는 인수 하나 **를 사용 합니다** . 리소스 키는 항상 Windows 런타임 XAML의 문자열입니다. 리소스 키를 처음 지정 하는 방법에 대 한 자세한 내용은 [x:Key 특성](x-key-attribute.md)을 참조 하세요.

리소스를 정의 하 고 샘플 코드를 비롯 한 [**resourcedictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)를 적절히 사용 하는 방법에 대 한 자세한 내용은 [resourcedictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)를 참조 하세요.

**중요** **StaticResource**와 마찬가지로, 필요한 경우 XAML 파일 내에서 어휘 적으로 정의 된 리소스에 대 한 전방 참조를 **만들려고 해서는 안** 됩니다. 이 작업을 수행 하는 것은 지원 되지 않습니다. 전방 참조가 실패 하지 않는 경우에도 그 중 하나를 수행 하려고 하면 성능이 저하 됩니다. 최상의 결과를 위해 전방 참조를 피할 수 있도록 리소스 사전의 컴퍼지션을 조정 합니다.

확인할 수 없는 키 **에 대 한** 지 수를 지정 하려고 하면 런타임에 XAML 구문 분석 예외가 throw 됩니다. 디자인 도구에서 경고나 오류를 제공할 수도 있습니다.

Windows 런타임 XAML 프로세서 구현에는 지 수 **에 대 한**지원 클래스 표현이 없습니다. 가장 근접하게 코드에 상당하는 방법은 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)의 컬렉션 API를 사용하는 것입니다(예: [**Contains**](/uwp/api/windows.ui.xaml.resourcedictionary.contains) 또는 [**TryGetValue**](/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue) 호출).

태그 **확장은 태그 확장입니다.** 태그 확장은 특성 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 일반적으로 구현되며 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 특성 구문에서 "{" 및 "}" 문자를 사용 합니다 .이는 XAML 프로세서가 태그 확장이 특성을 처리 해야 함을 인식 하는 데 사용 되는 규칙입니다.

### <a name="when-and-how-to-use-themeresource-rather-than-staticresource"></a>{StaticResource} 대신 {ThemeResource}을 사용 하는 경우 및 방법

리소스 사전의 항목 **을 확인 하는 데** 사용할 수 있는 규칙은 일반적으로 **StaticResource**와 동일 합니다. [**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) collection에서 참조 되는 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 파일을 사용할 수 있지만 **StaticResource** 는이 **작업을 수행할** 수 있습니다. 차이점은 지 수는 런타임에 다시 **평가할 수 있고** **StaticResource** 수 없다는 것입니다.

각 테마 사전의 키 집합은 활성화 된 테마에 관계 없이 동일한 키 리소스 집합을 제공 해야 합니다. 지정 된 키가 있는 리소스가 **system.windows.forms.systeminformation.highcontrast** 테마 사전에 있으면 해당 이름을 가진 또 다른 리소스는 **광원** 및 **기본값**에도 있어야 합니다. 그렇지 않으면 사용자가 테마를 전환 하 고 앱이 올바르게 표시 되지 않는 경우 리소스 조회가 실패할 수 있습니다. 테마 사전은 동일한 범위 내 에서만 참조 되는 키가 있는 리소스를 포함 하 여 하위 값을 제공할 수 있습니다. 이러한 항목은 모든 테마에서 동일 하지 않아도 됩니다.

일반적으로 테마 사전에 리소스를 추가 하 고 해당 값이 테마 간에 변경 될 수 있거나 변경 되는 값에 의해 지원 되는 경우 **에만 해당** 리소스에 대 한 참조를 만들어야 합니다. 이러한 종류의 리소스에 적합 합니다.

-   [**System.windows.media.solidcolorbrush>**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)에 대 한 특정 색의 브러시입니다. 이는 기본 XAML 컨트롤 템플릿 (.xaml)에서 **관련 된 사용** 의 80%를 차지 합니다.
-   테두리, 오프셋, 여백 및 안쪽 여백 등의 픽셀 값입니다.
-   **FontFamily** 또는 **FontSize**와 같은 글꼴 속성
-   일반적으로 시스템 스타일을 지정 하 고 동적 프레젠테이션에 사용 되는 제한 된 수의 컨트롤 (예: [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) 및 [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem))에 대 한 전체 템플릿
-   텍스트 표시 스타일 (일반적으로 글꼴 색, 배경 및 크기를 변경할 수 있음)

Windows 런타임은 기능에서 특별히 **참조 하는**리소스 집합을 제공 합니다. 이러한 모두는 XAML 파일 themeresources의 일부로 나열 됩니다 .이 파일은 Windows SDK (소프트웨어 개발 키트)의 일부로 include/winrt/XAML/디자인 폴더에서 사용할 수 있습니다. 테마 브러시 및 themeresources에 정의 된 추가 스타일에 대 한 설명서는 [xaml 테마 리소스](../design/controls-and-patterns/xaml-theme-resources.md)를 참조 하세요. 브러시는 각 브러시가 가능한 세 가지 활성 테마 각각에 대해 표시 되는 색 값을 알려주는 표에 설명 되어 있습니다.

컨트롤 템플릿에서 시각적 상태의 XAML 정의는 테마 변경으로 인해 변경 될 수 있는 기본 리소스가 있을 때마다 **기능 참조를** 사용 해야 합니다. 시스템 테마를 변경 하면 일반적으로 시각적 상태 변경도 발생 하지 않습니다. 이 경우 리소스는 여전히 활성 시각적 상태에 대 한 값을 다시 평가할 수 있도록이 경우에는 **해당 참조를** 사용 해야 합니다. 예를 들어 특정 UI 부분의 브러시 색과 해당 속성 중 하나를 변경 하는 시각적 상태가 있고 브러시 색이 테마별 인 경우에는 기본 템플릿에 해당 속성의 값을 제공 하 고 기본 템플릿에 시각적 상태를 수정 **하는 데** 필요 합니다.

**ThemeResource** 종속 된 값은 일련의 종속 값에서 볼 수 있습니다. 예를 들어 키가 지정 된 리소스 이기도 한 [**system.windows.media.solidcolorbrush>**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 에서 사용 하는 [**색**](/uwp/api/Windows.UI.Color) **값은 필요한 참조를** 사용할 수 있습니다. 그러나 키가 지정 된 **system.windows.media.solidcolorbrush>** 리소스를 사용 하는 UI 속성은 기능을 사용 하는 경우에도 테마를 변경할 때 동적 값이 변경 될 수 있도록 하는 각각의 [**브러시**](/uwp/api/Windows.UI.Xaml.Media.Brush)형식 **속성을 사용** 합니다.

**참고**   `{ThemeResource}` 테마 전환에 대 한 및 런타임 리소스 평가는 Windows 8.1 XAML에서 지원 되지만 Windows 8을 대상으로 하는 앱의 경우 XAML에서 지원 되지 않습니다.

### <a name="system-resources"></a>시스템 리소스

일부 테마 리소스는 시스템 리소스 값을 내부 하위 값으로 참조 합니다. 시스템 리소스는 XAML 리소스 사전에 없는 특수 한 리소스 값입니다. 이러한 값은 Windows 런타임 XAML 지원의 동작에 의존 하 여 시스템 자체에서 값을 전달 하 고 XAML 리소스가 참조할 수 있는 형태로이를 나타냅니다. 예를 들어 RGB 색을 나타내는 "SystemColorButtonFaceColor" 라는 시스템 리소스가 있습니다. 이 색은 Windows 런타임 및 Windows 런타임 앱에만 국한 되지 않는 시스템 색 및 테마의 측면에서 제공 됩니다.

시스템 리소스는 대개 고대비 테마의 기본 값입니다. 사용자는 고대비 테마에 대 한 색 선택 사항을 제어 하 고 있으며 사용자는 Windows 런타임 앱에 국한 되지 않는 시스템 기능을 사용 하 여 이러한 옵션을 선택할 수 있습니다. Windows 런타임 앱에 대 한 고대비 **ThemeResource** 테마의 기본 동작은 사용자가 제어 하 고 시스템에서 노출 하는 이러한 테마 관련 값을 사용할 수 있습니다. 또한 시스템에서 런타임 테마 변경을 감지한 경우 참조가 재평가 되도록 표시 됩니다.

### <a name="an-example-themeresource-usage"></a>예 {Anerg} 사용법

다음은 기본 xaml 및 themeresources 파일에서 가져온 몇 가지 예제 XAML을 사용 하 여 사용 하는 방법을 보여 **줍니다.** 템플릿 하나 (기본 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button))와 두 속성 ([**배경**](/uwp/api/windows.ui.xaml.controls.control.background) 및 [**전경**](/uwp/api/windows.ui.xaml.controls.control.foreground))이 테마 변경에 반응 하도록 선언 되는 방법을 살펴보겠습니다.

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

여기서 속성은 [**브러시**](/uwp/api/Windows.UI.Xaml.Media.Brush) 값을 사용 하 고 및 라는 [**system.windows.media.solidcolorbrush>**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 리소스에 대 한 참조를 `ButtonBackgroundThemeBrush` `ButtonForegroundThemeBrush` 사용 하 **ThemeResource**여 생성 됩니다.

이러한 동일한 속성도 [**단추의**](/uwp/api/Windows.UI.Xaml.Controls.Button)일부 시각적 상태에 의해 조정 됩니다. 특히 단추를 클릭할 때 배경색이 변합니다. 시각적 상태 storyboard의 [**배경**](/uwp/api/windows.ui.xaml.controls.control.background) 및 [**전경**](/uwp/api/windows.ui.xaml.controls.control.foreground) 애니메이션은 [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) 개체를 사용 하 고 키 프레임 **값으로 사용 되는** 브러시에 대 한 참조를 사용 합니다.

```xml
<VisualState x:Name="Pressed">
  <Storyboard>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Border"
        Storyboard.TargetProperty="Background">
      <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedBackgroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter"
         Storyboard.TargetProperty="Foreground">
       <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedForegroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
  </Storyboard>
</VisualState>
```

이러한 각 브러시는 앞에서 일반적으로 사용 되는 것으로 정의 되어 있습니다. xaml 전방 참조를 방지 하기 위해 템플릿 이전에 정의 해야 합니다. "Default" 테마 사전에 대 한 정의는 다음과 같습니다.

```xml
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="Transparent" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="#FFFFFFFF" />
...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="#FFFFFFFF" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="#FF000000" />
...
```

그런 다음, 다른 각 테마 사전은 이러한 브러시를 정의 합니다. 예를 들면 다음과 같습니다.

```xml
        <ResourceDictionary x:Key="HighContrast">
            <!-- High Contrast theme resources -->
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />

...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
```

여기서 [**색**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 값은 시스템 리소스 **에 대 한 다른 리소스** 참조입니다. 시스템 리소스를 참조 하 고 테마 변경에 대 한 응답으로 변경 하려는 경우에는 필요에 따라 참조를 **사용 해야** 합니다.

## <a name="windows8-behavior"></a>Windows 8 동작

Windows **8은 기능을 갖춘 태그 확장** 을 지원 하지 않으며 Windows 8.1부터 사용할 수 있습니다. 또한 Windows 8은 Windows 런타임 앱의 테마 관련 리소스를 동적으로 전환 하는 기능을 지원 하지 않습니다. XAML 템플릿 및 스타일에 대 한 테마 변경을 선택 하기 위해 앱을 다시 시작 해야 합니다. 이는 좋은 사용자 환경이 아니므로 앱이 컴파일 및 대상 Windows 8.1를 사용 하는 것이 좋습니다. 이렇게 하면 사용자가 **기능을 사용 하 여 스타일** 을 사용 하 고 사용자가 수행할 때 테마를 동적으로 전환할 수 있습니다. Windows 8 용으로 컴파일되어 Windows 8.1에서 실행 되는 앱은 Windows 8 동작을 계속 사용 합니다.

## <a name="design-time-tools-support-for-the-themeresource-markup-extension"></a>**{Anerg}** 태그 확장에 대 한 디자인 타임 도구 지원

Microsoft Visual Studio 2013에는 XAML 페이지에서 **{Gerg}** 태그 확장을 사용할 때 Microsoft IntelliSense 드롭다운에 가능한 키 값이 포함 될 수 있습니다. 예를 들어 "{ThemeResource를 입력 하는 즉시 [XAML 테마 리소스](../design/controls-and-patterns/xaml-theme-resources.md) 의 모든 리소스 키가 표시 됩니다.

리소스 키가 {사용 중 **}** 사용의 일부로 존재 하는 경우 **에는 정의로 이동** (F12) 기능을 사용 하 여 해당 리소스를 확인 하 고 디자인 타임 (테마 리소스가 정의 됨)에 대 한 일반 .xaml을 표시할 수 있습니다. 테마 리소스는 두 번 이상 정의 되므로 (테마 당) **정의로 이동** 하면 **기본**에 대 한 정의 인 파일에 있는 첫 번째 정의로 이동 합니다. 다른 정의를 원하는 경우 파일 내에서 키 이름을 검색 하 고 다른 테마의 정의를 찾을 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [XAML 테마 리소스](../design/controls-and-patterns/xaml-theme-resources.md)
* [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [x:Key 특성](x-key-attribute.md)
 