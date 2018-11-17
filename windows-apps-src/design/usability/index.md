---
description: 앱을 전 세계 사용자들이 사용하고 액세스할 수 있게 하는 방법을 알아봅니다.
keywords: UWP 앱 접근성, 세계화, 디자인 포괄 앱, 접근성 앱 요구 사항
title: UWP 앱의 유용성 - Windows 앱 개발
author: mijacobs
layout: LandingPage
template: detail.hbs
ms.author: mijacobs
ms.date: 10/18/2017
ms.topic: landing-page
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: 80f57ba29d31da0b4d49b620e916e39d4ef69842
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7169011"
---
# <a name="usability-for-uwp-apps"></a>UWP 앱의 유용성



세부 사항에 조금 더 관심을 기울이면 사용자 환경을 전 세계 사용자의 요구를 충족하는 진정으로 포괄적인 사용자 환경으로 변형할 수 있습니다.

이 섹션의 디자인 및 코딩 지침을 통해 접근성 기능을 추가하고, 세계화 및 지역화가 가능하도록 설정하고, 사용자가 환경을 사용자 지정할 수 있도록 설정하고, 사용자가 필요로 할 때 지원을 제공하여 UWP 앱을 더욱 포괄적으로 만들 수 있습니다.


## <a name="accessiblity"></a>접근성

접근성은 신체적 제약으로 인해 기존 사용자 인터페이스를 제대로 사용할 수 없는 사람들이 앱을 사용할 수 있도록 하는 것입니다. 일부 상황의 경우 접근성 요구 사항이 법으로 지정됩니다. 그러나 앱이 최대한 많은 사용자층을 확보할 수 있도록 하려면 법적 요구 사항에 관계없이 접근성 문제를 해결하는 것이 좋습니다.

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">접근성 개요</a></b> <br/> 이 문서는 UWP 앱의 접근성 시나리오와 관련된 개념 및 기술에 대한 개요입니다.</p>
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
<p><b><a href="../accessibility/designing-inclusive-software.md">포괄 소프트웨어 디자인</a></b><br/>Windows 10용 UWP(유니버설 Windows 플랫폼) 앱을 사용하여 포괄 디자인으로 진화해 나가는 방법에 대해 알아보세요.  접근성을 염두에 둔 포괄 소프트웨어를 디자인하고 빌드하세요.</p>
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
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">포괄 Windows 앱 개발</a></b><br/> 이 문서는 접근성 있는 UWP 앱을 개발하기 위한 로드맵입니다.</p>
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
<p><b><a href="../accessibility/accessibility-testing.md">접근성 테스트</a> </b><br/>UWP 앱에 접근성을 구현하기 위해 따라야 할 몇 가지 테스트 절차입니다.</p>
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
<p><b><a href="../accessibility/accessibility-in-the-store.md">스토어의 접근성</a></b><br/>UWP 앱을 Microsoft Store에서 접근 가능한 앱으로 선언하기 위한 요구 사항에 대해 설명합니다.</p>
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
<p><b><a href="../accessibility/accessibility-checklist.md">접근성 검사 목록</a></b><br/>UWP 앱이 접근 가능한지 확인하는 검사 목록을 제공합니다.</p>
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
<p><b><a href="../accessibility/basic-accessibility-information.md">기본적인 접근성 정보 표시</a></b><br/>경우에 따라 기본 접근성 정보는 이름, 역할 및 값으로 분류됩니다. 이 항목에서는 보조 기술이 필요로 하는 기본 정보를 앱에 표시하는 데 도움이 되는 코드에 대해 설명합니다.</p>
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
<p><b><a href="../accessibility/keyboard-accessibility.md">키보드 접근성</a></b><br/>따라서 앱의 키보드 접근성이 좋지 않을 경우 시각 장애나 이동성 문제가 있는 사용자가 앱을 사용하는 데 어려움을 겪거나 아예 사용하지 못할 수 있습니다.</p>
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
<p><b><a href="../accessibility/high-contrast-themes.md">고대비 테마</a></b><br/>고대비 테마가 활성 상태일 때 UWP 앱을 사용할 수 있도록 하는 데 필요한 단계에 대해 설명합니다. </p>
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
<p><b><a href="../accessibility/accessible-text-requirements.md">접근성 있는 텍스트 요구 사항</a></b><br/>이 항목에서는 색 및 배경이 필요한 명암비를 충족하도록 하여 앱 텍스트의 접근성에 대한 모범 사례를 설명합니다. 또한 이 항목에서는 UWP 앱의 텍스트 요소에 부여될 수 있는 Microsoft UI 자동화 역할 및 그래픽의 텍스트에 대한 모범 사례를 설명합니다.</p>                    
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
<p><b><a href="../accessibility/practices-to-avoid.md">피해야 할 접근성 사례</a></b><br/>UWP 앱에 접근성을 구현하려는 경우 피해야 할 사례에 대해 설명합니다.</p>                    
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
<p><b><a href="../accessibility/custom-automation-peers.md">사용자 지정 자동화 피어</a></b><br/>UI 자동화의 자동화 피어 개념과 고유한 사용자 지정 UI 클래스에 대해 자동화 지원을 제공하는 방법을 설명합니다.</p>                    
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
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">컨트롤 패턴 및 인터페이스</a></b><br/>Microsoft UI 자동화 컨트롤 패턴, 클라이언트가 컨트롤 패턴에 액세스하는 데 사용하는 클래스 및 공급자가 컨트롤 패턴을 구현하는 데 사용하는 인터페이스를 나열합니다.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>


## <a name="globalization-and-localization"></a>세계화 및 지역화

Windows는 전 세계에서 언어, 지역, 문화가 다양한 사용자가 사용합니다. 사용자는 다양한 국가와 지역에서 다양한 언어를 사용합니다. 일부 사용자는 2개국어 이상을 사용합니다. 따라서 앱은 언어, 지역 및 문화 시스템 설정이 다양하게 조합된 구성에서 실행됩니다. *세계화* 및 *지역화*를 사용하여 앱을 쉽게 조정할 수 있도록 디자인하면 앱의 잠재 시장을 늘릴 수 있습니다.

<a href="../globalizing/globalizing-portal.md">세계화 및 지역화 포털</a>

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
<p><b><a href="../in-app-help/instructional-ui.md">UI 사용 안내</a></b><br/>경우에 따라 특정 터치 조작 등 사용자에게 명확하지 않을 수 있는 앱의 기능에 대해 설명하는 것이 유용할 수 있습니다. 이러한 경우 보지 못했을 수 있는 기능을 검색하고 사용할 수 있도록 UI를 통해 사용자에게 지침을 제공해야 합니다.</p>
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

