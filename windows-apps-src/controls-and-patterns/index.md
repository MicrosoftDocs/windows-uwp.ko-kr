---
description: "UWP 앱에 컨트롤 &amp; 패턴을 추가하는 방법에 대한 디자인 지침과 코딩 지침을 가져옵니다. 앱에서 사용할 45가지 이상의 강력한 컨트롤을 찾습니다."
title: "UWP 컨트롤 및 패턴 - Windows 앱 개발"
author: mijacobs
keywords: uwp controls, user interface, app controls
translationtype: Human Translation
ms.sourcegitcommit: 7c45a464be250e210df48d3fdef35f00d77e16d5
ms.openlocfilehash: 23fdd3513d11a0fa0171e50fe71d49c8e3db7376

---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


# UWP 앱의 컨트롤 및 패턴

UWP 앱 개발에서 <i>컨트롤</i>은 콘텐츠를 표시하거나 조작을 가능하게 하는 UI 요소입니다. 컨트롤은 사용자 인터페이스의 구성 요소입니다. 간단한 단추에서 그리드 보기처럼 강력한 데이터 컨트롤까지 45개 이상의 사용할 수 있는 컨트롤을 제공합니다. <i>패턴</i>은 새로운 것을 만들기 위해 여러 컨트롤을 조합하는 방법입니다.

이 섹션의 문서에서는 UWP 앱에 컨트롤 및 패턴을 추가하는 방법에 대한 디자인 지침과 코딩 지침을 제공합니다. 

## 소개

XAML 및 C#으로 컨트롤을 추가하고 스타일을 지정하는 방법에 대한 일반 지침과 코드 예제입니다.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[컨트롤 추가 및 이벤트 처리](controls-and-events-intro.md)</b> <br/>
앱에 컨트롤을 추가하는 세 가지 주요 단계는 앱 UI에 컨트롤 추가, 컨트롤의 속성 설정, 특정 작업을 수행하도록 컨트롤의 이벤트 처리기에 코드 추가입니다.</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>[컨트롤 스타일 지정](styling-controls.md)</b> <br/>
XAML 프레임워크를 사용하여 다양한 방법으로 앱 모양을 사용자 지정할 수 있습니다. 스타일을 사용하면 컨트롤 속성을 설정하고 이 설정을 재사용하여 여러 컨트롤에서 일관된 모양을 얻을 수 있습니다.</p>
  </div>
</div>
</div>

## 사전순 인덱스 

특정 컨트롤 및 패턴에 대한 자세한 정보입니다.

기능별로 정렬된 목록을 보려면 [기능별 컨트롤 인덱스](controls-by-function.md)를 참조하세요.

<div class="uwpd-list-of-links">
<ul>

<li>[자동 제안 상자](auto-suggest-box.md)</li>

<li>[막대](app-bars.md)</li>

<li>[단추](buttons.md)</li>

<li>[확인란 ](checkbox.md)</li>

<li>[날짜 및 시간 컨트롤](date-and-time.md)
<ul>

<li>[달력 날짜 선택](calendar-date-picker.md)</li>

<li>[달력 보기](calendar-view.md)</li>

<li>[날짜 선택기](date-picker.md)</li>

<li>[시간 선택기](time-picker.md)</li>
</ul>
</li>


<li>[대화 상자, 팝업 및 메뉴](dialogs-popups-menus.md)</li>

<li>[대칭 이동 보기](flipview.md)</li>

<li>[허브](hub.md)</li>

<li>[하이퍼링크](hyperlinks.md)</li>

<li>[이미지 및 이미지 브러시](images-imagebrushes.md)</li>

<li>[목록](lists.md)</li>

<li>[지도 컨트롤](../maps-and-location/controls-map.md)</li>

<li>[마스터/세부](master-details.md)</li>

<li>[미디어 재생](media-playback.md)
<ul>
<li>[사용자 지정 전송 컨트롤](custom-transport-controls.md)</li>
</ul>
</li>

<li>[탐색 창](nav-pane.md)</li>

<li>[진행률 컨트롤](progress-controls.md)</li>

<li>[라디오 단추](radio-button.md)</li>

<li>[컨트롤 스크롤 및 이동](scroll-controls.md)</li>

<li>[검색](search.md)</li>

<li>[시맨틱 줌](semantic-zoom.md)</li>

<li>[슬라이더](slider.md)</li>

<li>[분할 보기](split-view.md)</li>

<li>[탭 및 피벗](tabs-pivot.md)</li>

<li>[텍스트 컨트롤](text-controls.md)
<ul>

<li>[Labels(레이블)](labels.md)</li>

<li>[암호 상자](password-box.md)</li>

<li>[Rich-edit 상자](rich-edit-box.md)</li>

<li>[서식 있는 텍스트 블록](rich-text-block.md)</li>

<li>[맞춤법 검사 및 자동 완성](spell-checking-and-prediction.md)</li>

<li>[텍스트 블록](text-block.md)</li>

<li>[입력란](text-box.md)</li>
</ul>
</li>



<li>[타일, 배지 및 알림](tiles-badges-notifications.md)
<ul>

<li>[타일](tiles-and-notifications-creating-tiles.md)</li>

<li>[적응형 타일](tiles-and-notifications-create-adaptive-tiles.md)</li>

<li>[적응형 타일 스키마](tiles-and-notifications-adaptive-tiles-schema.md)</li>

<li>[자산 지침](tiles-and-notifications-app-assets.md)</li>

<li>[특수 타일 템플릿](tiles-and-notifications-special-tile-templates-catalog.md)</li>

<li>[적응형 및 대화형 알림 메시지](tiles-and-notifications-adaptive-interactive-toasts.md)</li>

<li>[알림 시각화 도우미](tiles-and-notifications-notifications-visualizer.md)</li>

<li>[알림 전달 방법](tiles-and-notifications-choosing-a-notification-delivery-method.md)</li>

<li>[로컬 타일 알림](tiles-and-notifications-sending-a-local-tile-notification.md)</li>

<li>[정기 알림](tiles-and-notifications-periodic-notification-overview.md)</li>

<li>[WNS](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</li>

<li>[원시 알림](tiles-and-notifications-raw-notification-overview.md)</li>
</ul>
</li>


<li>[토글](toggles.md)</li>
<li>[도구 설명](tooltips.md)</li>

<li>[웹 보기](web-view.md)</li>
</ul>
</div>



<!--HONumber=Jul16_HO2-->


