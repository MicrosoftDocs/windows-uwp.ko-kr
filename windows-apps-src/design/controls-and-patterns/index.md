---
description: UWP 앱에 컨트롤 &amp; 패턴을 추가하는 방법에 대한 디자인 지침과 코딩 지침을 가져옵니다. 앱에서 사용할 45가지 이상의 강력한 컨트롤을 찾습니다.
title: UWP 컨트롤 및 패턴 - Windows 앱 개발
author: mijacobs
keywords: UWP 컨트롤, 사용자 인터페이스, 앱 컨트롤
label: Controls & patterns
template: detail.hbs
ms.author: mijacobs
ms.date: 11/16/2017
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: e28d557280ab253a09d5697f369c694e490d3c7d
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6672059"
---
# <a name="controls-and-patterns-for-uwp-apps"></a>UWP 앱의 컨트롤 및 패턴
 

UWP 앱 개발에서 <i>컨트롤</i>은 콘텐츠를 표시하거나 조작을 가능하게 하는 UI 요소입니다. 컨트롤은 사용자 인터페이스의 구성 요소입니다. <i>패턴</i>은 새로운 것을 만들기 위해 여러 컨트롤을 조합하는 방법입니다.

간단한 단추에서 그리드 보기처럼 강력한 데이터 컨트롤까지 45개 이상의 사용할 수 있는 컨트롤을 제공합니다.  이러한 컨트롤은 Fluent 디자인 시스템의 일부이며 모든 장치와 화면 크기에서 멋지게 보이고 확장 가능한 UI를 만드는 데 도움을 줍니다. 

이 섹션의 문서에서는 UWP 앱에 컨트롤 및 패턴을 추가하는 방법에 대한 디자인 지침과 코딩 지침을 제공합니다. 

## <a name="intro"></a>소개

XAML 및 C#으로 컨트롤을 추가하고 스타일을 지정하는 방법에 대한 일반 지침과 코드 예제입니다.

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">컨트롤 추가 및 이벤트 처리</a></b> <br/>
앱에 컨트롤을 추가하는 세 가지 주요 단계는 앱 UI에 컨트롤 추가, 컨트롤의 속성 설정, 특정 작업을 수행하도록 컨트롤의 이벤트 처리기에 코드 추가입니다.</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">컨트롤 스타일 지정</a></b> <br/>
XAML 프레임워크를 사용하여 다양한 방법으로 앱 모양을 사용자 지정할 수 있습니다. 스타일을 사용하면 컨트롤 속성을 설정하고 이 설정을 재사용하여 여러 컨트롤에서 일관된 모양을 얻을 수 있습니다.</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>Windows UI 라이브러리 가져오기
일부 컨트롤은 Windows UI 라이브러리 에서만 사용할 수 있습니다. 를 가져오려면 [Windows UI 라이브러리 개요 및 설치 지침](/uwp/toolkits/winui/)을 참조 하세요.

## <a name="alphabetical-index"></a>사전순 인덱스 

특정 컨트롤 및 패턴에 대한 자세한 정보입니다. 기능별로 정렬된 목록을 보려면 <a href="controls-by-function.md">기능별 컨트롤 인덱스</a>를 참조하세요.

<div style="column-count: 2; column-gap: 40px; margin-top: 40px;" >
<ul style="margin-top: 0px; padding-top: 0px; list-style-type: none;">
<li style="list-style-type: none;"><a href="auto-suggest-box.md">자동 제안 상자</a></li>

<li style="list-style-type: none;"><a href="app-bars.md">막대</a></li>

<li style="list-style-type: none;"><a href="buttons.md">단추</a></li>

<li style="list-style-type: none;"><a href="checkbox.md">확인란 </a></li>

<li style="list-style-type: none;"><a href="color-picker.md">색 선택</a></li>

<li style="list-style-type: none;"><a href="contact-card.md">연락처 카드</a></li>

<li style="list-style-type: none;"><a href="date-and-time.md">날짜 및 시간 컨트롤</a></li>

<li style="list-style-type: none;"><a href="dialogs-and-flyouts/index.md">대화 상자 및 플라이아웃</a></li>

<li style="list-style-type: none;"><a href="flipview.md">대칭 이동 보기</a></li>

<li style="list-style-type: none;"><a href="forms.md">양식</a></li>

<li style="list-style-type: none;"><a href="hub.md">허브</a></li>

<li style="list-style-type: none;"><a href="hyperlinks.md">하이퍼링크</a></li>

<li style="list-style-type: none;"><a href="images-imagebrushes.md">이미지 및 이미지 브러시</a></li>

<li style="list-style-type: none;"><a href="inking-controls.md">잉크 입력 컨트롤</a></li>

<li style="list-style-type: none;"><a href="lists.md">목록</a></li>

<li style="list-style-type: none;"><a href="../../maps-and-location/controls-map.md">지도 컨트롤</a></li>

<li style="list-style-type: none;"><a href="master-details.md">마스터/세부</a></li>

<li style="list-style-type: none;"><a href="media-playback.md">미디어 재생</a></li>

<li style="list-style-type: none;"><a href="menus.md">메뉴 및 상황에 맞는 메뉴</a></li>

<li style="list-style-type: none;"><a href="navigationview.md">탐색 보기</a></li>

<li style="list-style-type: none;"><a href="person-picture.md">인물 사진</a></li>

<li style="list-style-type: none;"><a href="pivot.md">피벗</a></li>

<li style="list-style-type: none;"><a href="progress-controls.md">진행률 컨트롤</a></li>

<li style="list-style-type: none;"><a href="radio-button.md">라디오 단추</a></li>

<li style="list-style-type: none;"><a href="rating.md">평점 컨트롤</a></li>

<li style="list-style-type: none;"><a href="scroll-controls.md">컨트롤 스크롤 및 이동</a></li>

<li style="list-style-type: none;"><a href="search.md">검색</a></li>

<li style="list-style-type: none;"><a href="semantic-zoom.md">시맨틱 줌</a></li>

<li style="list-style-type: none;"><a href="shapes.md">셰이프</a></li>

<li style="list-style-type: none;"><a href="slider.md">슬라이더</a></li>

<li style="list-style-type: none;"><a href="split-view.md">분할 보기</a></li>

<li style="list-style-type: none;"><a href="text-controls.md">텍스트 컨트롤</a></li>


<li style="list-style-type: none;"><a href="toggles.md">토글</a></li>
<li style="list-style-type: none;"><a href="tooltips.md">도구 설명</a></li>

<li style="list-style-type: none;"><a href="tree-view.md">트리 뷰</a></li>

<li style="list-style-type: none;"><a href="web-view.md">웹 보기</a></li>
</ul>
</div>

## <a name="xaml-controls-gallery"></a>XAML 컨트롤 갤러리

이러한 컨트롤과 작동 중인 흐름 디자인 시스템을 확인하려면 Microsoft Store에서 _XAML 컨트롤 갤러리_ 앱을 다운로드합니다. 이 앱은 이 웹사이트에 대한 대화형 도우미입니다. 이 도우미가 설치되어 있으면 개별 컨트롤 페이지에 대한 링크를 사용하여 앱을 시작하고 작동 중인 컨트롤을 확인할 수 있습니다.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a>

<a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>추가 컨트롤

<a href="http://www.telerik.com/">Telerik</a>, <a href="https://www.syncfusion.com/products/uwp">SyncFusion</a>, <a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>, <a href="http://www.infragistics.com/products/universal-windows-platform">Infragistics</a>, <a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> 및 <a href="http://www.actiprosoftware.com/products/controls/universal">ActiPro</a> 등에서 제공하는 UWP 개발용 추가 컨트롤을 사용할 수 있습니다. 이러한 컨트롤은 사용자 지정 컨트롤과 서비스로 표준 시스템 컨트롤을 보강하여 엔터프라이즈 및 .NET 개발자에게 추가 지원을 제공합니다.  

이러한 컨트롤에 대해 자세히 알고 싶은 경우 GitHub의 <a href="https://github.com/Microsoft/Windows-appsample-customers-orders-database">고객 주문 데이터베이스</a> 샘플을 확인하세요. 이 샘플은 UWP 제품군의 UI의 일부로 포함된 Telerik의 데이터 그리드 컨트롤과 데이터 입력 유효성 검사를 사용합니다. UWP 제품군의 UI는 .NET foundation을 통해 오픈 소스 프로젝트로 제공되는 20개 이상의 컨트롤 모음입니다.
