---
description: "다음 문서는 다양한 디바이스 및 화면 크기에서 멋지게 보이고 탐색하기 쉬운 UWP 앱을 디자인하고 코딩하는 데 도움이 됩니다."
title: "레이아웃 디자인 – Windows 앱 개발"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 9f75c39d26bd0c8858f404ab4fcd3d23562ea033
ms.openlocfilehash: 35a8f78420256cb2da02d7fd4720939a16676600

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div class="uwpd-banner">
<h1 class="uwpd-ruledheader">UWP 앱의 레이아웃</h1>
</div>

앱 구조, 페이지 레이아웃 및 탐색은 앱 사용자 환경의 기본입니다. 이 섹션의 문서는 다양한 디바이스 및 화면 크기에서 멋지게 보이고 탐색하기 쉬운 앱을 만드는 데 도움이 됩니다.

## 소개

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p><b>[앱 UI 디자인 소개](design-and-ui-intro.md)</b><br />
UWP 앱을 디자인할 때 다양한 디스플레이 크기를 가진 여러 디바이스에 맞는 사용자 인터페이스를 만듭니다. 이 문서에서는 UI 관련 기능 개요, UWP 앱의 이점 및 반응형 UI 개발에 대한 몇 가지 팁과 유용한 정보를 제공합니다. </p>
  </div>
  <div class="side-by-side-content-right">
    ![An app running on multiple devices](images/rspd-reposition-type1-sm.png)
  </div>
</div>
</div>

## 앱 레이아웃 및 구조
앱 구조 및 세 가지 유형의 UI 요소(탐색, 명령, 콘텐츠) 사용에 대한 아래 권장 사항을 확인하세요.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p>
<b>[탐색 기본 사항](navigation-basics.md)</b><br/>
UWP 앱의 탐색은 탐색 구조, 탐색 요소 및 시스템 수준 기능의 유연한 모델을 기반으로 합니다. 이 문서에서는 이러한 구성 요소를 소개하고 적합한 탐색 환경을 만드는 데 함께 사용하는 방법을 보여 줍니다.
</p>
<p>
<b>[콘텐츠 기본 사항](content-basics.md)</b><br/>
모든 앱의 기본 목적은 콘텐츠에 액세스할 수 있도록 하는 것입니다. 사진 편집 앱에서는 사진이 콘텐츠이고, 여행 앱에서는 여행 목적지에 대한 지도와 정보가 콘텐츠인 식입니다. 이 문서에서는 사용, 만들기, 조작의 세 가지 콘텐츠 시나리오에 대한 콘텐츠 디자인 권장 사항을 제공합니다.
</p> 
  </div>
  <div class="side-by-side-content-right">
<p><b>[명령 기본 사항](commanding-basics.md)</b> <br />
명령 요소는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다. 이 문서에서는 단추 및 확인란과 같은 명령 요소, 이러한 명령 요소에서 지원하는 조작 및 명령 요소를 호스트하기 위한 명령 화면(예: 명령 모음 및 상황에 맞는 메뉴)에 대해 설명합니다.</p>
  </div>
</div>
</div>

## 페이지 레이아웃 
아래 문서는 다양한 화면 크기, 창 크기, 해상도 및 방향에서 멋지게 보이는 유연한 UI를 만드는 데 도움이 됩니다. 


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[화면 크기 및 중단점](screen-sizes-and-breakpoints-for-responsive-design.md)</b><br/>
Windows 10 에코시스템에서는 디바이스 대상 및 화면 크기가 너무 다양해서 각각에 맞게 UI를 최적화하는 것에 대해 걱정할 수조차 없습니다. 대신 360, 640, 1024, 1366 epx 등의 몇 가지 주요 너비("중단점"이라고도 함)에 대해 디자인하는 것이 좋습니다.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[XAML을 사용하여 레이아웃 정의](layouts-with-xaml.md)</b> <br/>
XAML 속성 및 레이아웃 패널을 사용하여 반응성이 뛰어난 적응형 앱을 만드는 방법입니다.</p>
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[레이아웃 패널](layout-panels.md)</b> <br />
각 패널의 레이아웃 유형을 설명하고 패널을 사용하여 XAML UI 요소를 배치하는 방법을 보여 줍니다.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[맞춤, 여백 및 안쪽 여백](alignment-margin-padding.md)</b> <br />
차원 속성(너비, 높이 및 제약 조건) 외에도 요소에는 요소가 레이아웃 단계를 통과하고 UI에 렌더링될 때 레이아웃 동작에 영향을 주는 맞춤, 여백 및 안쪽 여백 속성이 있을 수도 있습니다.</p> 
  </div>
</div>
</div>





<!--HONumber=Jun16_HO4-->


