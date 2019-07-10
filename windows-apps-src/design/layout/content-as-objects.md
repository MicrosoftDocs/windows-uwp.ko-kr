---
description: ''
title: 개체로서의 콘텐츠
template: detail.hbs
ms.localizationpriority: medium
ms.date: 04/18/2019
ms.openlocfilehash: b73e595454121fa34b536f3672c75a5c554ee0f3
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714024"
---
# <a name="content-as-objects"></a>개체로서의 콘텐츠

 

요소의 깊이, 즉 z축 순서를 조작하여 앱 사용이 더욱 용이한 시각적 계층을 만들 수 있습니다.  

> 참고: 이 문서는 Windows 10 RS2의 새로운 기능에 대 한 초기 초안을입니다. 특징 이름, 용어 및 기능은 변경될 수 있습니다. 

## <a name="why-visual-hierarchy-is-important"></a>시각적 계층이 중요한 이유

사용자는 관심을 원하는 수많은 요청을 끊임없이 받게 됩니다. 사용자의 눈에 띄기를 바라는 화면의 요소들, 또는 사용자가 읽거나 클릭해주길 바라는 문자열과 버튼들이 그렇습니다. 시각적 환경이 이러한 요소들로 뒤섞이면서 혼란을 가중할수록 장면을 분석하여 어떤 상황인지 이해하려는 노력이 더욱 필요합니다.  

이는 사용자 인터페이스의 요소를 신중하게 선택하는 것이 왜 중요하고, 그리고 UI 요소 간 시각적 계층을 쉽게 알아볼 수 있는 레이아웃 구성이 왜 중요한지 알 수 있는 이유입니다. <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

명확한 시각적 계층은 어떤 요소가 가장 중요한지 사용자에게 알려줄 뿐만 아니라 요소 간 관계도 나타냅니다. 시각적 계층이 효과적이라면 사용자가 페이지 레이아웃을 한 번에 이해하는 동시에 완수하려는 작업에 집중할 수 있습니다. 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>그렇다면, 명확한 시각적 계층을 만들려면 어떻게 해야 할까요? Windows 10의 초기 버전에서는 공백, 위치 및 입력 체계를 사용하여 시각적 계층을 정의할 수 있었습니다. </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/flat-layout.png">플랫 레이아웃</a>
    
  </div>
</div>
</div>

Windows 10 RS2에서는 또 다른 차원인 깊이를 추가하였습니다. 

<a href="images/content-as-objects/depth-in-layout2.png">레이아웃에서 깊이</a>


## <a name="use-depth-to-establish-a-hierarchy"></a>깊이를 사용한 계층 구성 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>다른 디자인 도구(공백, 위치, 입력 체계)와 함께 깊이(z축 순서)를 사용하여 계층을 구성할 수 있습니다. 가장 중요한 요소를 최상단 계층으로 높이고, 하부 계층은 중요성이 비교적 떨어지는 UI를 표시하는 데 사용합니다. 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">레이아웃에서 깊이</a> 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>어떤 방식으로 작동합니까?
> TODO: 요소의 z 순서를 제어 하는 방법의 간략 한 설명입니다. z축 순서를 명시적으로 하드 코딩하거나, 혹은 의미 체계 등급 시스템이 있습니까? 항목은 계층 사이를 어떻게 이동합니까? 자동으로 이루어지는 시스템 작업은 무엇이고, 디자이너와 개발자들이 주의해야 할 것은 무엇입니까? 

## <a name="the-four-layers-of-a-typical-app-layers"></a>일반적인 앱의 4계층

<p>일반적으로 앱은 4계층으로 구성됩니다.</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>백그라운드 초과</b> 이 계층 앱 뒤 상주 합니다.  요소가 이 계층으로 이동할 경우에는 비대화형 요소로 만드는 것이 좋습니다. 이 계층에 속하는 요소들은 시차가 가장 느리며 앱 창의 크기에 맞게 잘립니다. TODO: 이 계층 확장은? 

<p>예제에서는 백그라운드 요소 콘텐츠를 TODO 뒤에 이미지 포함 됩니다. 예, 할 일: 예입니다.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">기능 이외에 계층 앱의 배경</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>수동 계층</b> UI 요소 어디에 기본적으로 앱의 기본 계층입니다.  이 계층의 요소들은 실시간으로(시차 없음) 이동하고, 앱 창의 크기에 맞춰 잘리고, 100% 배율로 렌더링됩니다. 

<p>예제에서는 요소: 앱 배경, 텍스트, 앱 탐색 UI와 같은 보조 UI입니다.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">앱의 수동 계층</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>작업에 대 한 호출</b> 이 계층은 수동 레이어 요소 위에 우선 순위를 지정 하는 대화형 항목에 대 한 합니다. 이 계층에 속하는 요소들은 시차가 중간 정도이고, 앱 창의 크기에 맞게 잘립니다. TODO: 이 계층 규모 요소 않거나 그림자가?

<p>예제에서는 요소: 목록, 표, 기본 명령 (할 일: Such as...).</p> 
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">앱의 작업에 호출 계층</a>
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Hero 계층</b> 이 계층 시 화면에서 가장 높은 우선 순위 요소입니다.  이 계층의 요소는 앱 창의 경계를 무시하여 크기를 조정할 수 있으며, 자동으로 그림자를 가져옵니다.

<p>요소 예: 사진 요소, 현재 선택된 항목</p>  
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">앱의 대표 계층</a>
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>예: TBD
> TODO: Z 순서를 사용 하는 일반적인 UI 패턴을 적용 하는 방법을 보여 줍니다. 일러스터레이션 및 코드를 표시해야 합니다. 

## <a name="download-the-code-samples"></a>코드 샘플 다운로드
>TODO: 이 기능을 보여 주는 샘플에 연결 합니다. 


## <a name="related-articles"></a>관련 문서
* [콘텐츠 기본 사항](../basics/content-basics.md)
