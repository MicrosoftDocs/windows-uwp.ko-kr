---
description: "앱을 전 세계 사용자들이 사용하고 액세스할 수 있게 하는 방법을 알아봅니다."
keywords: "UWP 앱 접근성, 세계화, 디자인 포괄 앱, 접근성 앱 요구 사항"
title: "UWP 앱의 유용성 - Windows 앱 개발"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 589e3290a47a9245ddb5e43f64d13bae1244ac8b
ms.openlocfilehash: d7246bc898e36155996941875b6c00da7891809d

---
# UWP 앱의 유용성

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

세부 사항에 조금 더 관심을 기울이면 사용자 환경을 전 세계 사용자의 요구를 충족하는 진정으로 포괄적인 사용자 환경으로 변형할 수 있습니다.

이 섹션의 디자인 및 코딩 지침을 통해 접근성 기능을 추가하고, 세계화 및 지역화가 가능하도록 설정하고, 사용자가 환경을 사용자 지정할 수 있도록 설정하고, 사용자가 필요로 할 때 지원을 제공하여 UWP 앱을 더욱 포괄적으로 만들 수 있습니다.


## 접근성

접근성은 신체적 제약으로 인해 기존 사용자 인터페이스를 제대로 사용할 수 없는 사람들이 앱을 사용할 수 있도록 하는 것입니다. 일부 상황의 경우 접근성 요구 사항이 법으로 지정됩니다. 그러나 앱이 최대한 많은 사용자층을 확보할 수 있도록 하려면 법적 요구 사항에 관계없이 접근성 문제를 해결하는 것이 좋습니다.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[접근성 개요](../accessibility/accessibility-overview.md)</b> <br/> 이 문서는 UWP 앱의 접근성 시나리오와 관련된 개념 및 기술에 대한 개요입니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[포괄 소프트웨어 디자인](../accessibility/designing-inclusive-software.md)</b><br/>Windows 10용 UWP(유니버설 Windows 플랫폼) 앱을 사용하여 포괄 디자인으로 진화해 나가는 방법에 대해 알아보세요.  접근성을 염두에 둔 포괄 소프트웨어를 디자인하고 빌드하세요.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[포괄 Windows 앱 개발](../accessibility/developing-inclusive-windows-apps.md)</b><br/> 이 문서는 접근성 있는 UWP 앱을 개발하기 위한 로드맵입니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[접근성 테스트](../accessibility/accessibility-testing.md) </b><br/>UWP 앱에 접근성을 구현하기 위해 따라야 할 몇 가지 테스트 절차입니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[스토어의 접근성](../accessibility/accessibility-in-the-store.md)</b><br/>UWP 앱을 Windows 스토어에 접근 가능한 앱으로 등록하기 위한 요구 사항에 대해 설명합니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[접근성 검사 목록](../accessibility/accessibility-checklist.md)</b><br/>UWP 앱이 접근 가능한지 확인하는 검사 목록을 제공합니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[기본적인 접근성 정보 표시](../accessibility/basic-accessibility-information.md)</b><br/>경우에 따라 기본 접근성 정보는 이름, 역할 및 값으로 분류됩니다. 이 항목에서는 보조 기술이 필요로 하는 기본 정보를 앱에 표시하는 데 도움이 되는 코드에 대해 설명합니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[키보드 접근성](../accessibility/keyboard-accessibility.md)</b><br/>따라서 앱의 키보드 접근성이 좋지 않을 경우 시각 장애나 이동성 문제가 있는 사용자가 앱을 사용하는 데 어려움을 겪거나 아예 사용하지 못할 수 있습니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[고대비 테마](../accessibility/high-contrast-themes.md)</b><br/>고대비 테마가 활성 상태일 때 UWP 앱을 사용할 수 있도록 하는 데 필요한 단계에 대해 설명합니다. </p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[접근성 있는 텍스트 요구 사항](../accessibility/accessible-text-requirements.md)</b><br/>이 항목에서는 색 및 배경이 필요한 명암비를 충족하도록 하여 앱 텍스트의 접근성에 대한 모범 사례를 설명합니다. 또한 이 항목에서는 UWP 앱의 텍스트 요소에 부여될 수 있는 Microsoft UI 자동화 역할 및 그래픽의 텍스트에 대한 모범 사례를 설명합니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[피해야 할 접근성 사례](../accessibility/practices-to-avoid.md)</b><br/>UWP 앱에 접근성을 구현하려는 경우 피해야 할 사례에 대해 설명합니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[사용자 지정 자동화 피어](../accessibility/custom-automation-peers.md)</b><br/>UI 자동화의 자동화 피어 개념과 고유한 사용자 지정 UI 클래스에 대해 자동화 지원을 제공하는 방법을 설명합니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[컨트롤 패턴 및 인터페이스](../accessibility/control-patterns-and-interfaces.md)</b><br/>Microsoft UI 자동화 컨트롤 패턴, 클라이언트가 컨트롤 패턴에 액세스하는 데 사용하는 클래스 및 공급자가 컨트롤 패턴을 구현하는 데 사용하는 인터페이스를 나열합니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b>   
</p>
  </div>
</div>
</div>



## 세계화 및 지역화

Windows는 전 세계에서 문화, 지역 및 언어가 다양한 사용자가 사용합니다. 사용자는 어느 언어든 말할 수 있고 여러 언어를 사용할 수도 있습니다. 지역도 세계 어디든 있을 수 있어서 위치와 언어가 다양할 수 있습니다. 세계화 및 지역화를 사용하여 앱을 쉽게 조정할 수 있도록 디자인하면 앱의 잠재 시장을 늘릴 수 있습니다.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[권장 사항 및 금지 사항](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)</b><br/>앱을 광범위한 대상에 대해 세계화하거나 특정 시장을 위해 지역화하는 모범 사례를 따르세요.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[세계화를 대비한 형식 사용](../globalizing/use-global-ready-formats.md)</b><br/>날짜, 시간, 숫자 및 통화의 형식을 적절하게 지정하여 세계화를 대비한 앱을 개발합니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[언어 및 지역 관리](../globalizing/manage-language-and-region.md)</b><br/>Windows에서 제공되는 다양한 언어 및 지역 설정을 사용하여 Windows에서 UI 리소스를 선택하고 앱의 UI 요소 형식을 지정하는 방법을 제어합니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[날짜 및 시간 형식 지정 패턴 사용](../globalizing/use-patterns-to-format-dates-and-times.md)</b><br/>날짜 및 시간을 원하는 형식으로 정확히 표시하려면 [<strong>DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) API와 사용자 지정 패턴을 사용하세요.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[레이아웃 및 글꼴 조정, RTL 지원](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)</b><br/>RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 개발합니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[앱 지역화 준비](../globalizing/prepare-your-app-for-localization.md)</b><br/>다른 시장, 언어 또는 지역으로의 앱 지역화를 준비합니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[리소스에 UI 문자열 배치](../globalizing/put-ui-strings-into-resources.md)</b><br/>UI용 문자열 리소스를 리소스 파일에 넣습니다. 그러면 코드 또는 태그로부터 해당 문자열을 참조할 수 있습니다.</p>
  </div>
  <div class="side-by-side-content-right">
<b></b>   
<p></p>
  </div>
</div>
</div>


## 앱 설정

앱 설정을 통해 사용자는 앱을 사용자 지정하여 각자의 요구 사항과 설정에 맞게 최적화할 수 있습니다. 올바르게 설정하고 적절하게 저장하면 훨씬 더 좋은 사용자 환경을 만들 수 있습니다.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[지침](../app-settings/guidelines-for-app-settings.md)</b><br/>앱 설정을 만들고 표시하기 위한 모범 사례입니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[앱 데이터 저장 및 검색](../app-settings/store-and-retrieve-app-data.md)</b><br/>로컬, 로밍 및 임시 앱 데이터를 저장하고 검색하는 방법입니다.</p>
  </div>
</div>
</div>

## 앱 내 도움말
앱을 아무리 잘 디자인했더라도 일부 사용자는 추가 지원이 필요합니다.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[앱 도움말에 대한 지침](../in-app-help/guidelines-for-app-help.md)</b><br/>응용 프로그램은 복잡할 수 있으며, 사용자에게 효과적인 도움말을 제공하면 환경을 훨씬 개선할 수 있습니다.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[UI 사용 안내](../in-app-help/instructional-ui.md)</b><br/>경우에 따라 특정 터치 조작 등 사용자에게 명확하지 않을 수 있는 앱의 기능에 대해 설명하는 것이 유용할 수 있습니다. 이러한 경우 보지 못했을 수 있는 기능을 검색하고 사용할 수 있도록 UI를 통해 사용자에게 지침을 제공해야 합니다.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[앱 내 도움말](../in-app-help/in-app-help.md)</b><br/>대부분의 경우 도움말은 앱 내에 표시되고 사용자가 보려고 선택할 때 표시되는 것이 좋습니다. 앱 내 도움말을 만들 때 다음과 같은 지침을 고려합니다.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[외부 도움말](../in-app-help/external-help.md)</b><br/>대부분의 경우 도움말은 앱 내에 표시되고 사용자가 보려고 선택할 때 표시되는 것이 좋습니다. 앱 내 도움말을 만들 때 다음과 같은 지침을 고려합니다.</p>
  </div>
</div>
</div>



<!--HONumber=Aug16_HO5-->


