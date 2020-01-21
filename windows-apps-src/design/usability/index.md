---
description: 앱을 전 세계 사용자들이 사용하고 액세스할 수 있게 하는 방법을 알아봅니다.
keywords: UWP 앱 접근성, 세계화, 디자인 포괄 앱, 접근성 앱 요구 사항
title: UWP 앱의 유용성 - Windows 앱 개발
layout: LandingPage
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: a6912932b7ad71fd3d04c038eab7e0aa4dd6cb11
ms.sourcegitcommit: 2fa2d2236870eaabc95941a95fd4e358d3668c0c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70076386"
---
# <a name="usability-for-uwp-apps"></a>UWP 앱의 유용성

세부 사항에 조금 더 관심을 기울이면 사용자 환경을 전 세계 사용자의 요구를 충족하는 진정으로 포괄적인 사용자 환경으로 변형할 수 있습니다.

이 섹션의 디자인 및 코딩 지침을 통해 접근성 기능을 추가하고, 세계화 및 지역화가 가능하도록 설정하고, 사용자가 환경을 사용자 지정할 수 있도록 설정하고, 사용자가 필요로 할 때 지원을 제공하여 UWP 앱을 더욱 포괄적으로 만들 수 있습니다.

## <a name="accessibility"></a>접근성

접근성은 신체적 제약으로 인해 기존 사용자 인터페이스를 제대로 사용할 수 없는 사람들이 앱을 사용할 수 있도록 하는 것입니다. 일부 상황의 경우 접근성 요구 사항이 법으로 지정됩니다. 그러나 앱이 최대한 많은 사용자층을 확보할 수 있도록 하려면 법적 요구 사항에 관계없이 접근성 문제를 해결하는 것이 좋습니다.

[접근성 포털](../accessibility/accessibility.md)

<!--
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">Accessibility overview</a></b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">Designing inclusive software</a></b><br/>Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">Developing inclusive Windows apps</a></b><br/> This article is a roadmap for developing accessible UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">Accessibility testing</a> </b><br/>Testing procedures to follow to ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">Accessibility in the Store</a></b><br/>Describes the requirements for declaring your UWP app as accessible in the Microsoft Store.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">Accessibility checklist</a></b><br/>Provides a checklist to help you ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">Expose basic accessibility information</a></b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">Keyboard accessibility</a></b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">High-contrast themes</a></b><br/>Describes the steps needed to ensure your UWP app is usable when a high-contrast theme is active. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">Accessible text requirements</a></b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a UWP app can have, and best practices for text in graphics.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">Accessibility practices to avoid</a></b><br/>Lists the practices to avoid if you want to create an accessible UWP app.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">Custom automation peers</a></b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">Control patterns and interfaces</a></b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>
-->

## <a name="app-settings"></a>앱 설정

앱 설정을 통해 사용자는 앱을 사용자 지정하여 각자의 요구 사항과 설정에 맞게 최적화할 수 있습니다. 올바르게 설정하고 적절하게 저장하면 훨씬 더 좋은 사용자 환경을 만들 수 있습니다.

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">지침</a></b><br/>앱 설정을 만들고 표시하기 위한 모범 사례입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">앱 데이터 저장 및 검색</a></b><br/>로컬, 로밍 및 임시 앱 데이터를 저장하고 검색하는 방법입니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="globalization-and-localization"></a>세계화 및 지역화

Windows는 전 세계에서 언어, 지역, 문화 측면에서 다양한 사용자가 사용합니다. 사용자는 다양한 국가와 지역에서 다양한 언어를 사용합니다. 일부 사용자는 둘 이상의 언어를 사용합니다. 따라서 앱은 언어, 지역 및 문화 시스템 설정이 다양하게 조합된 구성에서 실행됩니다. *세계화* 및 *지역화*를 통해 앱을 쉽게 조정할 수 있도록 설계하여 앱에 대한 잠재적 시장을 넓힙니다.

<a href="../globalizing/globalizing-portal.md">세계화 및 지역화 포털</a>

## <a name="in-app-help"></a>앱 내 도움말
앱을 아무리 잘 디자인했더라도 일부 사용자는 추가 지원이 필요합니다.

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">앱 도움말에 대한 지침</a></b><br/>응용 프로그램은 복잡할 수 있으며, 사용자에게 효과적인 도움말을 제공하면 환경을 훨씬 개선할 수 있습니다.
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">사용 안내 UI</a></b><br/>경우에 따라 특정 터치 조작 등 사용자에게 명확하지 않을 수 있는 앱의 기능에 대해 설명하는 것이 유용할 수 있습니다. 이러한 경우 찾지 못했을 수 있는 기능을 검색하고 사용할 수 있도록 UI를 통해 사용자에게 지침을 제공해야 합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">앱 내 도움말</a></b><br/>대부분의 경우 도움말은 앱 내에 표시되고 사용자가 보려고 선택할 때 표시되는 것이 좋습니다. 앱 내 도움말을 만들 때 다음과 같은 지침을 고려합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">외부 도움말</a></b><br/>대부분의 경우 도움말은 앱 내에 표시되고 사용자가 보려고 선택할 때 표시되는 것이 좋습니다. 앱 내 도움말을 만들 때 다음과 같은 지침을 고려합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul>

