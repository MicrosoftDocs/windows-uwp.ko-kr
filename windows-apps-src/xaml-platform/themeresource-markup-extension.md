---
description: 현재 활성 테마에 따라 서로 다른 리소스를 검색하는 추가 시스템 논리를 통해 리소스에 대한 참조를 평가하여 XAML 특성의 값을 제공합니다.
title: ThemeResource 태그 확장
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9466ec598fad090e31768d680b64ffea52688844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661148"
---
# <a name="themeresource-markup-extension"></a>{ThemeResource} 태그 확장

현재 활성 테마에 따라 서로 다른 리소스를 검색하는 추가 시스템 논리를 통해 리소스에 대한 참조를 평가하여 XAML 특성의 값을 제공합니다. [{StaticResource} 태그 확장](staticresource-markup-extension.md)과 비슷하게, 리소스는 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에 정의되어 있으며 **ThemeResource** 사용 시 **ResourceDictionary**의 해당 리소스 키가 참조됩니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{ThemeResource key}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| 키 | 요청된 리소스에 대한 키입니다. 이 키는 처음에 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에 의해 할당됩니다. 리소스 키는 XamlName 문법에 정의된 문자열일 수 있습니다. |

## <a name="remarks"></a>설명

**ThemeResource**는 XAML 리소스 사전에 정의된 XAML 특성 값을 가져오기 위한 기술입니다. 태그 확장은 [{StaticResource} 태그 확장](staticresource-markup-extension.md)과 기본 용도가 같습니다. 동작 면에서 {StaticResource} 태그 확장과 다른 점은, **ThemeResource** 참조는 현재 시스템에서 사용 중인 테마에 따라 다양한 사전을 기본 조회 위치로 동적으로 사용할 수 있다는 것입니다.

앱을 처음 시작할 때 **ThemeResource** 참조에서 수행하는 리소스 참조는 시작 시점에 사용된 테마를 기반으로 평가됩니다. 하지만 이후 런타임에 사용자가 활성 테마를 변경하면 시스템에서 모든 **ThemeResource** 참조를 다시 평가하고 다른 테마별 리소스를 검색하며 새 리소스 값을 사용하여 시각적 트리의 모든 적정 위치에서 앱을 다시 표시합니다. **StaticResource**는 XAML을 로드할 때 및 앱이 시작될 때 결정되며 런타임에 다시 평가되지 않습니다. (예를 들어 시각적 상태와 같이 XAML을 동적으로 다시 로드하는 다른 기술이 있지만, 이 기술은 [{StaticResource} 태그 확장](staticresource-markup-extension.md)에서 사용되는 기본적인 리소스 평가보다 더 높은 수준에서 작동합니다.)

**ThemeResource**는 요청된 리소스의 키를 지정하는 인수 하나를 사용합니다. Windows 런타임 XAML에서 리소스 키는 항상 문자열입니다. 초기에 리소스 키를 지정하는 방법에 대한 자세한 내용은 [x:Key 특성](x-key-attribute.md)을 참조하세요.

리소스 정의 방법과 샘플 코드를 포함한 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 적절한 사용 방법에 대한 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)를 확인하세요.

**중요****StaticResource**와 마찬가지로, **ThemeResource**는 XAML 파일에서 어휘상으로 정의되어 있는 리소스에 대한 전방 참조를 시도해서는 안 됩니다. 이러한 시도는 지원되지 않습니다. 전방 참조가 실패하지 않더라도 전방 참조를 만들려고 하면 성능이 저하됩니다. 최상의 결과를 얻으려면 전방 참조를 피할 수 있도록 리소스 사전의 컴퍼지션을 조정하세요.

확인할 수 없는 키에 **ThemeResource**를 지정하려고 하면 런타임에 XAML 구문 분석 예외가 발생합니다. 디자인 도구에서 경고나 오류를 제공할 수도 있습니다.

Windows 런타임 XAML 프로세서 구현에는 **ThemeResource**을 위한 지원 클래스 표현이 없습니다. 가장 근접하게 코드에 상당하는 방법은 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 컬렉션 API를 사용하는 것입니다(예: [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) 또는 [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139) 호출).

**ThemeResource**는 태그 확장입니다. 태그 확장은 특정 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 구현되며, 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 특성 구문에 "{" 및 "}" 문자를 사용하며, 여기서 특성 구문은 XAML 프로세서가 태그 확장이 특성을 처리해야 함을 인식하는 데 사용하는 규칙입니다.

### <a name="when-and-how-to-use-themeresource-rather-than-staticresource"></a>{StaticResource} 대신 {ThemeResource}를 사용하는 시기 및 방법

**ThemeResource**가 리소스 사전의 항목으로 확인하는 데 적용되는 규칙은 일반적으로 **StaticResource**의 경우와 같습니다. **ThemeResource** 조회가 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807) 컬렉션에서 참조되는 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 파일로 확장할 수 있으며, **StaticResource**도 이러한 확장이 가능합니다. 차이점은 **ThemeResource**는 런타임에 다시 평가할 수 있지만 **StaticResource**는 그럴 수 없다는 것입니다.

어느 테마가 활성 상태이든 상관없이 각 테마 사전의 키 집합이 동일한 키 지정 리소스 집합을 제공해야 합니다. 주어진 키 지정 리소스가 **HighContrast** 테마 사전에 있다면 이 이름을 가진 다른 리소스도 **Light** 및 **Default**에 있어야 합니다. 그렇지 않으면 사용자가 테마를 전환할 때 리소스가 조회되지 않아 앱의 모양이 제대로 표시되지 않을 수 있습니다. 하지만 테마 사전에 같은 범위 내에서만 참조되어 하위 값을 제공하는 키 지정 리소스가 포함될 수 있으며, 이 리소스는 모든 테마에서 같을 필요가 없습니다.

일반적으로 리소스를 테마 사전에 배치하고 이 값이 테마 간에 변경될 수 있거나 변경되는 값에 의해 지원될 때만 **ThemeResource**를 사용하여 이 리소스를 참조해야 합니다. 이는 다음과 같은 리소스 종류에 적용됩니다.

-   브러시. 특히, [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)에 대한 색. 이 리소스는 기본 XAML 컨트롤 템플릿(generic.xaml)에서 **ThemeResource** 사용의 80% 정도를 구성합니다.
-   테두리, 오프셋, 여백, 안쪽 여백 등의 픽셀 값.
-   글꼴 속성(예: **FontFamily** 또는 **FontSize**).
-   주로 시스템에서 스타일이 설정되고 동적 프레젠테이션에 사용되는 제한된 수의 컨트롤(예: [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 및 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919))에 대한 템플릿을 완료합니다.
-   텍스트 표시 스타일(주로 글꼴 색, 배경 및 크기 변경용).

Windows 런타임은 특히 **ThemeResource**에서 참조하는 용도의 리소스 집합을 제공합니다. 이러한 모든 리소스는 Windows SDK(소프트웨어 개발 키트)의 일부로 include/winrt/xaml/design 폴더에 제공되는 XAML 파일 themeresources.xaml의 일부로 나열됩니다. themeresources.xaml에서 정의하는 테마 브러시 및 추가 스타일에 대한 설명서는 [XAML 테마 리소스](https://msdn.microsoft.com/library/windows/apps/mt187274)를 참조하세요. 세 가지의 가능한 활성 테마 각각에 대해 각 브러시가 가지는 색 값을 보여 주는 표에서 브러시를 설명합니다.

테마 변경으로 인해 변경될 수 있는 기본 리소스가 있을 때마다 컨트롤 템플릿에서 시각적 상태에 대한 XAML 정의에 **ThemeResource** 참조를 사용해야 합니다. 시스템 테마를 변경하는 경우 일반적으로 시각적 상태도 변경되지는 않습니다. 이 경우, 여전히 활성 상태인 시각적 상태에 대해 값을 다시 평가할 수 있도록 리소스에서 **ThemeResource** 참조를 사용해야 합니다. 예를 들어 특정 UI 부분의 브러시 색과 해당 속성 중 하나를 변경하는 시각적 상태가 있으며 테마별로 브러시 색이 다른 시각적 상태가 있는 경우, 기본 템플릿에서 속성의 값을 제공하고 이 템플릿의 시각적 상태를 수정하기 위한 **ThemeResource** 참조를 사용해야 합니다.

**ThemeResource** 사용은 일련의 종속 값에서 확인할 수 있습니다. 예를 들어 키가 지정된 리소스이기도 한 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)에서 사용되는 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 값이 **ThemeResource** 참조를 사용할 수 있습니다. 하지만 키가 지정된 **SolidColorBrush** 리소스를 사용하는 UI 속성도 **ThemeResource** 참조를 사용하므로, 특히 각 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 유형 속성이 테마 변경 시 동적 값 변경을 사용합니다.

**참고**   `{ThemeResource}` 런타임 리소스 평가 테마 전환에 Windows 8.1 XAML에서 지원 되지만 Windows 8을 대상으로 하는 앱에 대 한 XAML에서 지원 되지 않습니다.

### <a name="system-resources"></a>시스템 리소스

일부 테마 리소스는 기본 하위 값으로 시스템 리소스 값을 참조합니다. 시스템 리소스는 XAML 리소스 사전에서 찾을 수 없는 특수 리소스 값입니다. 이 값은 Windows 런타임 XAML 지원에서 시스템 자체의 값을 전달하는 동작에 따라 결정되며 XAML 리소스가 참조할 수 있는 형태로 값이 표현됩니다. 예를 들어 RGB 색을 표현하는 "SystemColorButtonFaceColor"라는 시스템 리소스가 있습니다. 이 색은 Windows 런타임과 Windows 런타임 앱에 특정하지 않은 시스템 색 및 테마의 여러 측면에서 가져옵니다.

시스템 리소스는 고대비 테마에 대한 기본값인 경우가 많습니다. 사용자는 고대비 테마의 색상 선택을 제어하며, Windows 런타임 앱에 특정하지 않은 시스템 기능을 사용하여 색상을 선택합니다. 시스템 리소스를 **ThemeResource** 참조로 참조하면 Windows 런타임 앱용 고대비 테마의 기본 동작에서 사용자가 제어하고 시스템이 공개하는 이 테마별 값이 사용될 수 있습니다. 또한 시스템에서 런타임 테마 변경이 감지되면 참조가 재평가로 표시됩니다.

### <a name="an-example-themeresource-usage"></a>{ThemeResource} 사용 예제

다음은 **ThemeResource** 사용 방법을 설명하기 위해 기본 generic.xaml 및 themeresources.xaml 파일에서 가져온 몇 가지 예제 XAML입니다. 템플릿 한 개만(기본 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)) 살펴보고 테마 변경에 대해 두 개의 속성([**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 및 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414))이 반응하도록 선언하는 방법에 대해 알아봅니다.

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

여기서 속성에는 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 값이 사용되며, **ThemeResource**를 사용하여 `ButtonBackgroundThemeBrush` 및 `ButtonForegroundThemeBrush`라는 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 리소스를 참조합니다.

이 속성은 또한 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)에 대한 일부 시각적 상태에 의해 조정됩니다. 특히, 단추를 클릭할 때 배경색이 변경됩니다. 여기서도 시각적 상태 스토리보드의 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 및 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) 애니메이션은 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) 개체와 **ThemeResource** 포함 브러시에 대한 참조를 키 프레임 값으로 사용합니다.

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

이러한 각 브러시는 이전에 generic.xaml에서 정의되어 있습니다. 즉, XAML 전방 참조를 방지하기 위해 브러시를 사용하는 템플릿 이전에 정의해야 했습니다. 다음은 "기본값" 테마 사전에 대한 정의입니다.

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

또한, 다른 테마 사전 각각에도 이 브러시가 정의되어 있습니다. 예를 들어 다음과 같습니다.

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

여기서 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 값은 시스템 리소스에 대한 다른 **ThemeResource** 참조입니다. 시스템 리소스를 참조하는 경우 테마 변경에 따라 시스템 리소스를 변경하려면 **ThemeResource**를 사용하여 참조해야 합니다.

## <a name="windows8-behavior"></a>Windows 8 동작

Windows 8을 지원 하지 않았습니다 합니다 **ThemeResource** 태그 확장 것은 Windows 8.1 사용할 수 있습니다. 또한 Windows 8은 Windows 런타임 앱에 대 한 테마와 관련 된 리소스를 동적으로 전환을 지원 하지 않았습니다. XAML 템플릿 및 스타일에 대한 테마 변경을 반영하려면 앱을 다시 시작해야 합니다. 앱을 다시 컴파일하고 스타일을 사용할 수 있도록 Windows 8.1 대상 것을 적극 되므로 뛰어난 사용자 환경을 이것이 **ThemeResource** 사용 및 사용자가 수행 하는 경우에 동적으로 테마를 전환할 수 있습니다. Windows 8 있지만 Windows 8.1 실행 되 고 계속 Windows 8 동작을 사용 하 게 컴파일된 앱입니다.

## <a name="design-time-tools-support-for-the-themeresource-markup-extension"></a>**{ThemeResource}** 태그 확장을 위한 디자인 타임 도구 지원

사용 하는 경우 Microsoft Visual Studio 2013 Microsoft IntelliSense 드롭다운에 사용할 수 있는 키 값 포함할 수는 **{ThemeResource}** XAML 페이지의 태그 확장 합니다. 예를 들어 "{ThemeResource"를 입력하기 시작하면 즉시 [XAML 테마 리소스](https://msdn.microsoft.com/library/windows/apps/mt187274)의 리소스 키가 표시됩니다.

리소스 키가 **{ThemeResource}** 에서 일부로 사용되어 존재하는 경우 **정의로 이동**(F12) 기능이 해당 리소스를 확인하고 테마 리소스가 정의되어 있는 디자인 타임용 generic.xaml을 표시할 수 있습니다. 테마 리소스는 테마당 한 번 이상 정의되기 때문에 **정의로 이동**은 파일에서 발견되는 첫 번째 정의(**Default**에 대한 정의)로 안내합니다. 다른 정의를 원하는 경우에는 파일 내에서 키 이름을 검색하고 다른 테마의 정의를 찾으면 됩니다.

## <a name="related-topics"></a>관련 항목

* [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [XAML 테마 리소스](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [X:key 특성](x-key-attribute.md)
 

