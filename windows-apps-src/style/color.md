---
Description: 색은 앱의 다양한 수준의 정보를 통해 탐색하는 직관적인 방식을 제공하며 상호 작용 모델을 강화하는 데 중요한 도구 역할을 합니다.
title: 색
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
brief: Color provides intuitive wayfinding through an app's various levels of information and serves as a crucial tool for reinforcing the interaction model.<br /><br />In Windows, color is also personal. Users can choose a color and a light or dark theme to be reflected throughout their experience.
---

# UWP 앱의 색
색은 앱의 다양한 수준의 정보를 통해 탐색하는 직관적인 방식을 제공하며 상호 작용 모델을 강화하는 데 중요한 도구 역할을 합니다.

## 테마 컬러

사용자는 테마라는 단일 색을 선택할 수 있습니다. 사용자는 48색 견본의 큐레이팅된 집합에서 테마를 선택할 수 있습니다.


<!-- Alternate version for the dev center. Need to add hex values. -->
<figure>
![Accent colors](images/accentcolorswatch.png)
<figcaption>대체로 원래의 테마 컬러를 배경으로 사용하면 항상 흰색 텍스트가 위에 옵니다.</figcaption>
</figure>

사용자가 테마 컬러를 선택하면 시스템 테마의 일부로 나타납니다. 영향을 받는 영역은 시작, 작업 표시줄, 창 크롬, [공용 컨트롤](https://dev.windows.com/design/controls-patterns) 내의 선택한 상호 작용 상태 및 하이퍼링크입니다. 각 앱은 해당 앱의 입력 체계, 배경 및 상호 작용에 테마 컬러를 추가로 통합하거나 재정의하여 특정 브랜딩을 유지합니다.

## 색 선택

테마 컬러를 선택하면 색 광도의 HCL 값에 따라 테마 컬러의 밝고 어두운 음영이 만들어집니다. 앱은 음영 변형을 사용하여 시각적 계층 구조를 만들고 상호 작용 지침을 제공할 수 있습니다.

![6개의 음영을 사용한 단일 테마 컬러](images/shades.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In XAML, the accent color is exposed as a [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx) named `SystemAccentColor`. It's also available programmatically from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx). You can programmatically access the different shades from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx), see the [UIColorType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) enum.
    </div>
</aside>

## 색 테마

사용자는 또한 시스템에 대해 밝거나 어두운 테마를 선택할 수 있습니다(해당 옵션은 휴대폰에는 있으나 태블릿 및 데스크톱에는 아직 없습니다. 앱에서 바로 구매 설정을 제공할 수 있습니다). 일부 앱은 사용자의 기본 설정에 따라 해당 테마를 변경하도록 선택하지만 옵트아웃(opt out)하는 앱도 있습니다.

밝은 테마를 사용하는 앱은 생산성 앱과 관련된 시나리오에 적합합니다. 예제는 Microsoft Office와 함께 사용할 수 있는 앱의 제품군입니다. 밝은 테마는 장기간의 작업과 함께 긴 길이의 텍스트를 읽기 쉽게 합니다.

어두운 테마는 미디어 중심인 앱의 콘텐츠 또는 풍부한 동영상 및 이미지로 사용자가 나타내려는 시나리오의 대비를 더 잘 보이도록 해 줍니다. 이러한 시나리오에서는 영화 시청 환경이 기본 작업일 수 있지만 읽기는 반드시 기본 작업이지는 않으며 주변 조명이 어두운 상태에서 표시됩니다.

앱이 이러한 설명과 맞지 않는 경우 다음 시스템 테마를 고려하면 사용자에게 적합한 테마를 결정할 수 있습니다.

테마를 더 쉽게 디자인하기 위해 Windows에서는 자동으로 테마에 맞게 조정되는 추가 색상표를 제공합니다.


<!-- OP version -->
### 밝은 테마
#### 기본
![기본 밝은 테마](images/themes-light-base.png)
#### 대체
![대체 밝은 테마](images/themes-light-alt.png)
#### 목록
![목록 밝은 테마](images/themes-light-list.png)
#### 크롬
![크롬 밝은 테마](images/themes-light-chrome.png)
### 어두운 테마
#### 기본
![기본 어두운 테마](images/themes-dark-base.png)
#### 대체
![대체 어두운 테마](images/themes-dark-alt.png)
#### 목록
![목록 어두운 테마](images/themes-dark-list.png)
#### 크롬
![크롬 어두운 테마](images/themes-dark-chrome.png)


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Each color is available as a XAML [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) that follows the `System*Color` naming convention (ex: `SystemChromeHighColor`). You can control your app's theme through either [Application.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) or [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## 접근성

색상표는 화면 사용에 최적화됩니다. 최적화된 읽기 환경을 위해 텍스트 대비를 최소 4.5:1로 유지하는 것이 좋습니다.


<!--HONumber=Mar16_HO5-->


