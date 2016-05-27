---
author: Xansky
Description: 고대비 테마가 활성 상태일 때 UWP(유니버설 Windows 플랫폼) 앱을 사용하는 데 필요한 단계에 대해 설명합니다.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: 고대비 테마
label: High-contrast themes
template: detail.hbs
---

# 고대비 테마  



고대비 테마가 활성 상태일 때 UWP(유니버설 Windows 플랫폼 앱을 사용할 수 있도록 하는 데 필요한 단계에 대해 설명합니다.

UWP 앱은 기본적으로 고대비 테마를 지원합니다. 사용자가 시스템에서 시스템 설정 또는 접근성 도구의 고대비 테마를 사용하도록 선택한 경우 이 프레임워크에서는 UI의 컨트롤 및 구성 요소에 대해 고대비 레이아웃 및 렌더링을 생성하는 색상 및 스타일 설정을 자동으로 사용합니다.

이 기본 기원은 기본 테마 및 템플릿 사용을 기반으로 합니다. 이러한 테마 및 템플릿은 시스템 색상을 리소스 정의로 참조하므로 시스템에서 고대비 모드를 사용하는 경우 리소스 원본이 자동으로 변경됩니다. 그러나 사용자 지정 템플릿, 테마 및 스타일을 컨트롤에 사용하는 경우 고대비에 대한 기본 제공 지원을 사용하도록 해야 합니다. 스타일 지정에 Microsoft Visual Studio용 XAML 디자이너를 사용하는 경우 디자이너에서는 기본 템플릿과 현저하게 다른 템플릿을 정의할 때마다 기본 테마와 함께 별도의 고대비 테마를 생성합니다. 별도의 테마 사전은 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 요소의 전용 속성인 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807) 컬렉션으로 이동합니다.

테마 및 컨트롤 템플릿에 대한 자세한 내용은 [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374)을 참조하세요. 특정 컨트롤에 대한 XAML 리소스 사전 및 테마를 확인하고 테마가 구성되는 방법 및 가능한 각 고대비 설정에 대해 유사하지만 다른 리소스를 참조하는 방법을 참조하면 매우 도움이 될 수 있습니다.

<span id="Detecting_when_a_high-contrast_theme_is_enabled"/>
<span id="detecting_when_a_high-contrast_theme_is_enabled"/>
<span id="DETECTING_WHEN_A_HIGH-CONTRAST_THEME_IS_ENABLED"/>
## 고대비 테마가 사용되는 경우 탐색  
UWP 앱은 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 클래스의 멤버를 사용하여 고대비 테마의 현재 설정을 검색할 수 있습니다. [
            **HighContrast**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrast) 속성은 고대비 테마가 현재 선택되어 있는지 여부를 확인합니다. **HighContrast**가 **true**로 설정되어 있으며 다음 단계에서 [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrastscheme) 속성의 값을 확인하여 사용되는 고대비 테마의 이름을 가져옵니다. "고대비 흰색" 및 "고대비 검정"은 일반적으로 코드에서 응답해야 하는 **HighContrastScheme**에 대한 값입니다. XAML 정의 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 키에는 공백이 없으므로 리소스 사전에서 이러한 테마에 해당되는 키는 각각 "HighContrastWhite" 및 "HighContrastBlack"입니다. 값이 몇 가지 다른 문자열인 경우 기본 고대비 테마에 대한 폴백 논리도 있어야 합니다. [XAML 고대비 샘플](http://go.microsoft.com/fwlink/p/?linkid=254993)에서는 이에 대한 논리를 보여 줍니다.

> [!NOTE]앱이 초기화되어 이미 콘텐츠를 표시하고 있는 범위에서 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)생성자를 호출해야 합니다.

앱은 실행하는 동안 고대비 리소스 값을 사용하는 것으로 전환할 수 있습니다. 이 작업은 스타일 또는 템플릿 XAML에서 [{ThemeResource}](https://msdn.microsoft.com/library/windows/apps/Mt185591)을 사용하여 리소스를 요청한 경우에만 작동합니다. 기본 테마(generic.xaml)는 모두 이 {ThemeResource} 태그 확장 기술을 사용하므로, 기본 컨트롤 테마를 사용하는 경우 이 동작을 구현할 수 있습니다. 이는 사용자 지정 컨트롤 또는 사용자 지정 컨트롤 스타일에서 수행할 수 있습니다. 단, 사용자 지정 템플릿 및 스타일에서도 이 {ThemeResource} 태그 확장 리소스 기술을 사용해야 합니다.

<span id="related_topics"/>
## 관련 항목  
* [접근성](accessibility.md)
* [UI 대비 및 설정 샘플](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 접근성 샘플](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 고대비 샘플](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)


<!--HONumber=May16_HO2-->


