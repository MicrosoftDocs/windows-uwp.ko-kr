---
title: IOS, Android 및 Windows 10 간의 플랫폼 기능을 비교 합니다.
description: IOS, Android 및 Windows 10의 유니버설 Windows 플랫폼 (UWP) 간에 개발 개념 및 플랫폼 기능을 자세히 비교 하 여 확인 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: 0591b32671c7e1e74b47a41448f3b77b915a7dc7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174907"
---
# <a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Android 및 iOS 개발자용 Windows 앱 개념 매핑

Android 또는 iOS 기술 및/또는 코드를 사용 하는 개발자 인 경우 Windows 10 및 유니버설 Windows 플랫폼 (UWP)로 이동 하려는 경우이 리소스에는 세 가지 플랫폼 간에 플랫폼 기능을 매핑하는 데 필요한 모든 정보가 포함 되어 있습니다.

[iOS에서 UWP로 이동](ios-to-uwp-root.md)의 포팅 콘텐츠도 참조하세요. 이 문서는 [다운로드](https://www.microsoft.com/download/details.aspx?id=52041)로도 제공 됩니다.

## <a name="user-interface-ui"></a>UI (사용자 인터페이스)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>디자인 언어.</strong><br><br>플랫폼의 앱의 모양과 동작을 규정 하는 규칙 집합입니다.</td>
<td align="left">Android <strong>재질 디자인</strong> 지침은 android designer 및 개발자가 따라야 하는 시각적 언어를 제공 합니다.</td>
<td align="left"><strong>인간 인터페이스 지침은</strong> iOS 디자이너 및 개발자에 대 한 조언을 제공 합니다.</td>
<td align="left"><a href="https://developer.microsoft.com/windows/apps/design"><strong>UWP Windows 앱 디자인</strong></a> 에서는 모든 Windows 10 장치에서 멋진 응용 프로그램을 만드는 방법을 보여 줍니다. UI (사용자 인터페이스) 디자인 기본 사항, 응답성이 뛰어난 디자인 기술 및 자세한 지침의 전체 목록을 확인할 수 있습니다.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>사용자 인터페이스 태그 언어입니다.</strong> <br><br>UI 및 해당 구성 요소를 렌더링 하 고 설명 하는 태그 언어입니다. 각 플랫폼은 비주얼 및 태그 편집용 편집기를 제공 합니다.<br/></td>
<td align="left"><strong>Android Studio</strong> 또는 <strong>Eclipse</strong>를 사용 하 여 편집 되는 <strong>XML 레이아웃</strong>.</td>
<td align="left">Xcode 내의 <strong>Interface Builder</strong> 를 사용 하 여 편집한 <strong>XIB</strong> 및 <strong>storyboard</strong> .</td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong> 및 <strong><a href="https://docs.microsoft.com/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend for Visual Studio</a></strong>를 사용 하 여 편집 되는 <strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong><br/><br/><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/index">XAML 플랫폼</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui">XAML을 사용 하 여 UI 만들기</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML을 사용 하 여 레이아웃 정의</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>기본 제공 사용자 인터페이스 컨트롤입니다.</strong> <br><br>플랫폼에서 제공 하는 다시 사용할 수 있는 UI 요소 (예: 단추, 목록 컨트롤 및 텍스트 컨트롤)</td>
<td align="left">미리 빌드된 <strong>보기</strong> 및 <strong>보기 그룹</strong> 클래스는 위젯, 레이아웃, 텍스트 필드, 컨테이너, 날짜/시간 컨트롤 및 전문가 컨트롤 이라고 합니다.</td>
<td align="left">Xcode 개체 라이브러리에 있고 UIKit 사용자 인터페이스 카탈로그에 나열 된 <strong>뷰</strong> 및 <strong>컨트롤</strong> 입니다. 보기에는 이미지 뷰, 선택기 뷰 및 스크롤 뷰가 있습니다. 컨트롤에는 단추, 날짜 선택기 및 텍스트 필드가 포함 됩니다.</td>
<td align="left">XAML 플랫폼은 단추, 목록 컨트롤, 패널, 텍스트 컨트롤, 명령 모음, 선택기, 미디어, 잉크 잉크 등의 <strong>기본 제공 컨트롤</strong> 집합을 제공 합니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">컨트롤 추가 및 이벤트 처리</a></td>
</tr>
<tr class="even">
<td align="left"><strong>이벤트 처리를 제어 합니다.</strong> <br><br>UI 컨트롤 내에서 이벤트가 트리거될 때 실행 되는 논리를 정의 합니다.</td>
<td align="left"><strong>이벤트 처리기</strong> 와 <strong>이벤트 수신기</strong> 는 XML에 추가 되거나 프로그래밍 방식으로 추가 됩니다.</td>
<td align="left">컨트롤은 <strong>동작</strong> 메시지를 <strong>대상</strong>으로 보냅니다.</td>
<td align="left">XAML 페이지에 연결 된 <strong>코드 숨김이 있는 파일</strong> 에서 xaml 컨트롤의 이벤트를 처리 하는 메서드를 정의할 수 있습니다. <strong>이벤트 처리기</strong> 는 항상 코드로 작성 됩니다. 그러나 이러한 처리기를 XAML 태그 또는 코드의 이벤트에 연결할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">컨트롤 추가 및 이벤트 처리</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview">이벤트 및 라우트된 이벤트 개요</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>데이터 바인딩.</strong> <br><br>응용 프로그램 UI에서 데이터를 렌더링 하 고 필요에 따라 해당 데이터와 동기화 상태를 유지할 수 있도록 하는 소프트웨어 디자인 패턴입니다.</td>
<td align="left">아직 베타 버전 이지만 제공 되는 <strong>데이터 바인딩 라이브러리가</strong> 있습니다.</td>
<td align="left">IOS에 기본 제공 바인딩 시스템이 없습니다. <strong>키-값 관찰</strong> 은 타사 라이브러리를 사용 하거나 추가 코드를 작성 하는 방법으로 데이터 바인딩을 수행 하기 위해 빌드할 수 있습니다. 컨트롤은 데이터를 가져오는 대리자/콜백 방법을 사용 합니다.</td>
<td align="left">UWP 플랫폼은 <strong>데이터 바인딩을</strong> 처리 합니다. <strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension">{X:bind}</a></strong> 태그 확장을 사용 하 여 고성능 바인딩을 활용 하거나 <strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension">{binding}</a></strong> 을 사용 하 여 더 많은 기능을 이용할 수 있습니다. 즉, 플랫폼에서 단방향 <strong>바인딩을</strong> 사용 하 여 ui에 있는 데이터 소스의 값을 표시 하는지 여부 또는 <strong>양방향 바인딩을</strong>사용 하 여 변경 될 때 ui를 업데이트 하는지 여부를 선택 하도록 바인딩을 구성 하는 경우입니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/data-binding/index">데이터 바인딩</a></td>
</tr>
<tr class="even">
<td align="left"><strong>UI 자동화.</strong> <br><br>UI 요소에 프로그래밍 방식으로 액세스 하 여 앱이 보조 기술 제품에 액세스할 수 있게 하 고 자동화 된 테스트 스크립트를 사용 하 여 UI와 상호 작용할 수 있습니다.</td>
<td align="left"><strong>텍스트 레이블</strong>, <strong>contentdescription</strong> 및 <strong>힌트</strong> 값은 자동화에서 UI 요소를 찾을 수 있도록 도와줍니다. Android Studio를 사용 하면 <strong>Ui Automator</strong> 및 <strong>Espresso</strong> 테스트 프레임 워크를 사용 하 여 ui 테스트를 작성할 수 있습니다.</td>
<td align="left"><strong>자동화 계측</strong> 기능을 사용 하면 요소 <strong>계층</strong>에서 <strong>내게 필요한 옵션</strong> 설정 또는 요소의 위치를 사용 하 여 요소를 식별 하는 자동화 된 UI 테스트 스크립트를 작성할 수 있습니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-uiautomationoverview">Ui 자동화</a></strong>를 사용 하 여 UWP의 기본 제공 ui 요소에 프로그래밍 방식으로 액세스할 수 있습니다.<br/><strong><a href="https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers">사용자 지정 자동화 피어</a></strong> 를 사용 하면 고유한 사용자 지정 UI 클래스에 대 한 자동화 지원을 제공할 수 있습니다. Visual Studio의 <strong><a href="https://docs.microsoft.com/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">코딩 된 Ui 테스트 프로젝트</a></strong> 를 사용 하면 ui를 통해 전체 응용 프로그램을 자동으로 테스트 하거나 격리 된 ui를 테스트할 수 있습니다.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>컨트롤의 모양 변경</strong> <br><br>크기, 색 및 기타 특성 편집</td>
<td align="left">컨트롤에는 디자이너 도구를 사용 하거나 XML 태그에서 프로그래밍 방식으로 편집할 수 있는 <strong>속성이</strong> 있습니다.</td>
<td align="left">컨트롤에는 Interface Builder에서 나 프로그래밍 방식으로 <strong>특성 검사자</strong> 를 사용 하 여 편집할 수 있는 <strong>특성이</strong> 있습니다.</td>
<td align="left">Visual Studio 및 Blend for Visual Studio를 사용 하 여 프로그래밍 방식으로 또는 XAML 태그에서 컨트롤의 <strong>속성</strong> 을 편집할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">컨트롤 추가 및 이벤트 처리</a></td>
</tr>
<tr class="even">
<td align="left"><strong>다시 사용할 수 있는 비주얼 스타일입니다.</strong> <br><br>재사용 가능한 형식으로 다양 한 컨트롤에 시각적 변경을 적용 합니다.</td>
<td align="left"><strong>XML 스타일</strong> 은 하나 이상의 컨트롤에 적용 되는 속성 집합입니다.</td>
<td align="left">iOS에서는 재사용 가능한 비주얼 스타일을 기본적으로 지원 하지 않지만 UIAppearance 프로토콜을 사용 하면 여러 컨트롤에서 공통 특성을 공유할 수 있습니다.</td>
<td align="left">다시 사용 하기 위해 여러 컨트롤에 적용 되 고 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> 에 저장 될 수 있는 재사용 가능한 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style">스타일</a></strong>을 만들 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/previous-versions/windows/apps/hh465381(v=win.10)">빠른 시작: 컨트롤 스타일 지정</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>컨트롤의 시각적 구조 편집</strong> <br><br>확인란의 확인란 텍스트를 이동 하는 것과 같이 속성 또는 특성만 수정 하는 것 외에도 컨트롤의 시각적 구조를 사용자 지정 합니다.</td>
<td align="left">Android에서 컨트롤의 시각적 구조를 편집 하는 간단한 방법은 없습니다.</td>
<td align="left">IOS에서 컨트롤의 시각적 구조를 편집 하는 간단한 방법은 없습니다.</td>
<td align="left">컨트롤의 시각적 구조를 사용자 지정 하기 위해 XAML 태그에서 해당 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">컨트롤 템플릿을</a></strong> 복사 하 고 편집할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)">빠른 시작: 컨트롤 템플릿</a></td>
</tr>
<tr class="even">
<td align="left"><strong>기본 제공 터치 제스처입니다.</strong> <br><br>보기 및 컨트롤에서 탭 하 고 두 번 탭 하는 것과 같은 높은 수준의 추상화 된 제스처 이벤트를 처리 하 여 사용자 지정 터치 지원을 제공 합니다.</td>
<td align="left"><strong>제스처 감지기</strong> 는 스크롤, 길게 누르기, 탭, 두 번 누르기 및 fling을 비롯 한 일반적인 터치 제스처를 검색 합니다.</td>
<td align="left">UIKit 프레임 워크는 터치 제스처를 검색 하는 기본 제공 <strong>제스처 인식자</strong> 를 제공 합니다.</td>
<td align="left"><strong>UI 요소</strong> 를 사용 하 여 탭, 두 번 탭, 오른쪽 탭 및 보유를 비롯 한 <strong>정적 제스처</strong> 이벤트 뿐만 아니라 슬라이드, 살짝 밀기, 턴, 손가락 및 늘이기를 포함 하는 <strong>조작 제스처 이벤트</strong> 를 처리할 수 있습니다. 제스처 이벤트는 <strong>라우트된 이벤트</strong> 이며 자식 UIElement를 포함 하는 부모 개체에 의해 처리 될 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions">터치 조작</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/layout/index">사용자 지정 사용자 상호 작용-제스처, 조작 및 상호 작용</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">탐색 및 응용 프로그램 구조</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>모양의.</strong> <br><br>레이아웃은 사용자 인터페이스의 구조를 정의 합니다.</td>
<td align="left">레이아웃은 다른 보기 그룹 또는 보기를 중첩할 수 있는 <strong>LinearLayout</strong> 및 <strong>RelativeLayout</strong> 와 같은 <strong>보기 그룹</strong> 으로 구성 됩니다.</td>
<td align="left">레이아웃은 중첩할 수 있는 <strong>Uiview</strong>가 포함 된 <strong>uiviewcontroller</strong> 로 구성 됩니다.</td>
<td align="left">정적 및 응답성이 뛰어난 레이아웃을 위해 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas">Canvas</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid">Grid</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel">RelativePanel</a></strong> 및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> 과 같은 <strong>레이아웃 패널 클래스로</strong> 구성 된 유연한 레이아웃 시스템을 제공 하는 XAML입니다. <strong><a href="https://docs.microsoft.com/visualstudio/ide/reference/properties-window?view=vs-2015">속성</a></strong> 은 요소의 크기와 위치를 제어 하는 데 사용 됩니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML을 사용 하 여 레이아웃 정의</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>피어 탐색.</strong> <br><br>동일 하 게 계층적인 중요도의 페이지를 탐색 하는 방법을 사용자에 게 제시 합니다.</td>
<td align="left"><strong>탭</strong>, <strong>살짝 밀기 보기</strong> 및 <strong>탐색 서랍</strong> 은 <strong>측면 탐색</strong>을 제공 합니다.</td>
<td align="left"><strong>탭 모음 컨트롤러</strong>, <strong>분할 뷰 컨트롤러</strong> 및 <strong>페이지 보기 컨트롤러</strong> 를 사용 하면 동일한 계층 구조의 뷰 간을 탐색할 수 있습니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot">탭/피벗</a></strong>을 사용 하 여 콘텐츠 위에 링크/탭의 영구적 목록을 표시할 수 있습니다. <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/split-view">탐색 창/분할 뷰에서</a></strong> 는 콘텐츠와 함께 링크 목록을 표시할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-basics">탐색</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigate-between-two-pages">두 페이지 간 이동</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>계층적 탐색.</strong> <br><br>계층의 부모 및 자식 페이지 간을 이동 합니다.</td>
<td align="left"><strong>목록</strong>및 <strong>표 형태 목록</strong>, <strong>단추</strong> 및 기타 컨트롤은 다른 <strong>활동</strong>을 로드 하는 <strong>의도</strong> 를 사용 하 여 하위 <strong>탐색</strong> 을 제공 합니다.</td>
<td align="left">사용자는 <strong>탐색 컨트롤러</strong> 를 사용 하 여 계층의 수준 간을 탐색할 수 있습니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/hub">허브</a></strong> 를 사용 하면 사용자에 게 하위 페이지로 이동 하도록 선택할 수 있는 콘텐츠의 미리 보기를 표시할 수 있습니다. <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/master-details">마스터/세부 정보</a></strong> 는 사용자가 해당 세부 정보 섹션 옆에 표시 되는 항목 요약 목록에서 선택할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-basics">탐색</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigate-between-two-pages">두 페이지 간 이동</a></td>
</tr>
<tr class="even">
<td align="left"><strong>뒤로 단추 탐색.</strong> <br><br>응용 프로그램을 통해 다시 탐색 합니다.</td>
<td align="left">작업 모음 내의 <strong>뒤로</strong> 및 <strong>위로</strong> 단추를 사용 하 여 <strong>ancestral</strong> 및 <strong>임시</strong> 탐색을 제공 <strong>합니다.</strong></td>
<td align="left"><strong>탐색 컨트롤러</strong> 에는 뒤로 단추가 추가 될 수 있습니다.<br/></td>
<td align="left">사용자가 <strong>탐색 기록을</strong>트래버스할 수 있도록 하는 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack">back stack 속성</a></strong> 을 사용 하 여 소프트웨어 또는 하드웨어 뒤로 단추 누름을 쉽게 처리할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-history-and-backwards-navigation">뒤로 단추 탐색</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>시작 화면.</strong> <br><br>주로 브랜딩 용으로 사용 되는 앱 시작 시 이미지를 표시 합니다.</td>
<td align="left">시작 화면은 기본적으로 제공 되지 않으며, 첫 번째 활동 <strong>테마 배경을</strong>편집 하 여 구현 됩니다.</td>
<td align="left">앱에는 <strong>정적 시작 이미지</strong> 또는 <strong>XIB/storyboard 시작 파일이</strong>있어야 합니다.</td>
<td align="left"><strong>이미지</strong> 및 컬러 배경을 사용 하 여 시작 화면을 만듭니다. <a href="https://docs.microsoft.com/windows/uwp/launch-resume/create-a-customized-splash-screen">시작 화면 시간은 확장할 수 있습니다</a>.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/add-a-splash-screen">시작 화면 추가</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">사용자 지정 입력</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>보냈습니다.</strong> <br><br>음성 인식 및 추가 음성 기능을 제공 합니다.</td>
<td align="left"><strong>Google Speech Search</strong>와 같은 <strong>RecognizerIntent</strong>을 구현 하는 모든 앱에서 음성 입력을 제공할 수 있습니다. <strong>SpeechRecognizer</strong> 클래스를 사용 하면 앱이 Google의 음성 인식 API를 사용할 수 있습니다.</td>
<td align="left">앱은 <strong>SFSpeechRecognizer</strong> 클래스를 사용 하 여 음성 입력 및 음성 인식을 구현할 수 있습니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/speech-recognition">음성 인식</a></strong> API를 사용 하 여 포그라운드에서 앱과 상호 작용할 수 있습니다. 음성 기반 <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/cortana-interactions">Cortana 상호 작용</a></strong> 을 사용 하 여 포그라운드 또는 백그라운드에서 앱을 시작 하 고 백그라운드 앱과 상호 작용할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions">음성 조작</a></td>
</tr>
<tr class="even">
<td align="left"><strong>사용자 지정 사용자 입력.</strong> <br><br>키보드, 마우스, 스타일러스 및 기타 입력 처리</td>
<td align="left">상호 작용에 대 한 지원은 <strong>터치</strong>, <strong>터치 패드</strong>, <strong>스타일러스</strong>, <strong>마우스</strong> 및 <strong>키보드</strong>를 포함 합니다. 이동 및 입력은 터치와 동일한 방식으로 보고 되지만 <strong>입력 장치</strong>에 대 한 자세한 정보를 검색할 수 있습니다.</td>
<td align="left"><strong>터치</strong>지원, <strong>Apple 연필</strong> 및 하드웨어 <strong>키보드가</strong> 제공 됩니다.</td>
<td align="left">디지털 잉크, <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/mouse-interactions">마우스</a></strong> 및 <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions">키보드</a></strong>를 사용 하는 <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions">터치</a></strong>, <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touchpad-interactions">터치 패드</a></strong>, <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions">펜/스타일러스</a></strong> 를 비롯 한 다양 한 상호 작용에 대 한 지원을 찾을 수 있습니다. 앱에서 사용 된 입력 장치를 알 필요 없이 데이터를 처리할 수 있으며, 필요한 경우 원시 입력 장치 데이터에 액세스할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input">포인터 입력 처리</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/layout/index">사용자 지정 사용자 조작</a></td>
</tr>
</tbody>
</table>
<h2 id="data">데이터</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>로컬 응용 프로그램 데이터입니다.</strong> <br><br>앱과 관련 된 설정 및 파일을 로컬로 저장 합니다.</td>
<td align="left"><strong>Openfileoutput</strong> 및 <strong>openfileoutput</strong>을 사용 하 여 로컬 파일을 저장할 수 있습니다. <strong>공유 기본 설정 파일</strong> 의 설정은 <strong>getsharedpreferences</strong>설정을 사용 하 여 액세스할 수 있습니다.</td>
<td align="left">로컬 파일은 <strong>Nsfilemanager</strong> 클래스를 통해 액세스 되는 <strong>응용 프로그램 지원</strong> 디렉터리에 저장할 수 있습니다. <strong>기본</strong> 설정 파일의 설정은 <strong>nsuserdefaults</strong> 클래스에서 액세스할 수 있습니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Storage">Windows 저장소</a></strong> 클래스는 통합 된 방식으로 로컬 데이터 저장소를 처리 합니다. <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer">않지만 windows.storage.applicationdatacontainer</a></strong> 개체는 <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings">LocalSettings</a></strong> 속성을 통해 액세스 되는 설정을 저장 합니다. <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder">Applicationdata. localfolder</a></strong> 속성을 통해 액세스 하는 <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.storagefolder">storagefolder</a></strong> 개체에 파일을 저장 합니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data">설정과 기타 앱 데이터의 저장 및 검색</a></td>
</tr>
<tr class="even">
<td align="left"><strong>로컬 데이터베이스 저장소입니다.</strong> <br><br>응용 프로그램 데이터를 관계형 데이터베이스에 저장 하 고, 해당 하는 경우 ORM (개체 관계형 매퍼)을 사용 합니다.</td>
<td align="left"><strong>SQLite</strong> 데이터베이스가 제공 됩니다. ORM은 기본 제공 되지 않습니다. <strong>SQLiteDatabase</strong> 클래스를 사용 하 여 SQL 쿼리를 실행 합니다.</td>
<td align="left"><strong>SQLite</strong> 데이터베이스가 제공 됩니다. <strong>CoreData</strong> 는 SQLite와 함께 사용할 수 있는 기본 제공 개체 그래프 프레임 워크로, ORM과 비슷한 기능을 제공 합니다.</td>
<td align="left"><strong>SQLite</strong>를 사용 하 여 데이터를 저장할 수 있습니다. <strong><a href="https://docs.microsoft.com/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> 은 많은 데이터 액세스 코드를 작성할 필요가 없도록 하는 기본 제공 ORM 이며 SQL을 작성 하지 않고도 쉽게 데이터베이스를 쿼리할 수 있습니다. <a href="https://docs.microsoft.com/windows/uwp/data-access/sqlite-databases">SQLite 라이브러리</a>를 사용 하 여 SQL 쿼리를 직접 실행할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/data-access/index">데이터 액세스</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>REST 액세스용 HTTP 라이브러리.</strong> <br><br>HTTP (S)를 사용 하 여 웹 서비스 및 웹 서버와 통신할 수 있도록 하는 기본 제공 라이브러리입니다.<br/></td>
<td align="left">HTTP 라이브러리 <strong>HttpURLConnection</strong> 및 <strong>volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> 및 <strong>NSURLDownload</strong>입니다.</td>
<td align="left">기본 제공 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient">Httpclient</a></strong> API를 사용 하 여 GET, DELETE, PUT, POST, common authentication 패턴, SSL, 쿠키 및 진행률 정보를 비롯 한 일반적인 HTTP 기능에 액세스할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left"><strong>클라우드 백업 서비스.</strong> <br><br>플랫폼에서 제공 하는 앱 데이터 백업 서비스입니다.</td>
<td align="left">Android의 <strong>백업 관리자</strong> 는 Google의 <strong>android backup Service</strong>에서 응용 프로그램 데이터를 백업 하는 작업을 처리 합니다.</td>
<td align="left">사용자가 응용 프로그램 데이터를 포함 하 여 백업을 처리 하도록 <strong>ICloud 백업을</strong> 구성할 수 있습니다. Icloud 호환 <strong>핵심 데이터</strong>, <strong>icloud 키-값 저장소</strong> 및 <strong>icloud 문서 저장소</strong>를 사용 하는 앱입니다.</td>
<td align="left">로밍 <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata">Applicationdata api</a></strong> ( <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder">RoamingFolder</a></strong> 및 <a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>RoamingSettings</strong></a>포함)를 사용 하 여 저장 하는 모든 앱 데이터는 클라우드와 사용자의 다른 장치에 자동으로 동기화 됩니다. 동기화는 사용자의 Microsoft 계정를 통해 수행 됩니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data">로밍 앱 데이터에 대 한 지침</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP 파일 다운로드.</strong> <br><br>HTTP를 통해 크고 작은 파일 다운로드</td>
<td align="left"><strong>Urlconnection</strong> 및 <strong>HTTPURLConnection</strong> 는 HTTP 및 FTP를 통해 다운로드 하는 데 사용 되며, 시스템 <strong>다운로드 관리자</strong> 를 사용 하 여 백그라운드에서 다운로드할 수도 있습니다.</td>
<td align="left"><strong>NSURLSession</strong> 및 <strong>NSURLConnection</strong> 는 HTTP 및 FTP를 통해 파일을 다운로드 하는 데 사용할 수 있습니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer">백그라운드 전송 API</a></strong> 를 사용 하면 응용 프로그램 일시 중단, 연결 손실 및 연결 및 배터리 수명에 따라 조정 하는 것을 고려 하 여 HTTP (S) 및 FTP를 통해 파일을 안정적으로 전송할 수 있습니다. 더 작은 파일에 이상적인 <strong><a href="https://docs.microsoft.com/uwp/api/windows.web.http.httpclient">Httpclient</a></strong> 를 사용할 수도 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">네트워킹 기술 선택</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/background-transfers">백그라운드 전송</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Layer.</strong> <br><br>사용자 고유의 프로토콜을 사용 하 여 다른 장치와 통신 하기 위해 낮은 수준의 UDP 데이터 그램 및 TCP 소켓 만들기</td>
<td align="left"><strong>Socket</strong> 클래스는 TCP 소켓을 제공 하며, <strong>DATAGRAMSOCKET</strong> 클래스는 UDP 소켓을 제공 합니다.</td>
<td align="left"><strong>Nsstream</strong> 및 <strong>CFSTREAM</strong> 은 TCP 소켓을 제공 하 고 <strong>cfstream</strong> 은 UDP 소켓을 제공 합니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> 클래스를 사용 하 여 UDP 데이터 그램 소켓과 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket">streamsocket</a></strong> 클래스를 사용 하 여 통신 하 고 TCP 또는 Bluetooth RFCOMM를 통해 통신할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/networking-basics">네트워킹 기본 사항</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">네트워킹 기술 선택</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/sockets">소켓 개요</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Websocket.</strong> <br><br>클라이언트와 서버 간의 양방향 통신을 제공 하 여 실시간 데이터 전송을 가능 하 게 합니다.</td>
<td align="left">Android에는 기본 제공 Websocket 라이브러리가 없습니다.</td>
<td align="left">IOS에는 기본 제공 Websocket 라이브러리가 없습니다.</td>
<td align="left">Websocket을 지 원하는 서버에 대 한 보안 연결은 섹션에서 읽을 수 있는 더 큰 이진 파일 전송에 대 한 수신 알림 및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocket">Streamwebsocket</a></strong> 을 포함 하는 작은 메시지에 대해 <strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.sockets.messagewebsocket">messagewebsocket</a></strong> 클래스를 사용 하 여 만들 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/networking-basics">네트워킹 기본 사항</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">네트워킹 기술 선택</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/websockets">Websocket 개요</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth 라이브러리.</strong> <br><br>OAuth 라이브러리를 사용 하면 타사 OAuth 공급자 및 플랫폼에 기본 제공 되는 모든 계정 관리에 액세스할 수 있습니다.</td>
<td align="left">제네릭 OAuth 라이브러리는 제공 되지 않습니다. <strong>GoogleAuthUtil</strong> 클래스는 Google Play 서비스를 사용 하 여 OAuth 인증을 위해 제공 됩니다.<br/></td>
<td align="left">제네릭 OAuth 라이브러리는 제공 되지 않습니다. <strong>계정 프레임 워크</strong> 는 Facebook 및 Twitter와 같은 장치에 이미 저장 된 사용자 계정에 대 한 액세스를 제공 합니다.</td>
<td align="left">일반 OAuth 라이브러리 <strong><a href="https://docs.microsoft.com/windows/uwp/security/web-authentication-broker">웹 인증 브로커</a></strong> 를 사용 하 여 타사 id 공급자 서비스에 연결할 수 있습니다. <strong><a href="https://docs.microsoft.com/windows/uwp/security/credential-locker">자격 증명</a></strong> 보관을 사용 하면 사용자가 로그인을 저장 하 고 여러 장치에서 사용할 수 있습니다. <strong><a href="https://docs.microsoft.com/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft live</a></strong> 네임 스페이스를 사용 하면 microsoft 서비스에 액세스 하기 위해 Live SDK OAuth에 쉽게 액세스할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/security/authentication-and-user-identity">인증 및 사용자 ID</a><br/><br/><a href="https://docs.microsoft.com/uwp/api/windows.security.authentication.web">Windows. 웹 API 설명서</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker 코드 예제</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">도구</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>앱을 만드는 데 사용 되는 도구 집합입니다.</td>
<td align="left">Google을 사용 하 여 개발자가 Android Studio 사용을 위해 <strong>Android Studio</strong> 및 <strong>Eclipse</strong></td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual Studio</a></strong> 및 <strong><a href="https://docs.microsoft.com/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">BLEND FOR VISUAL STUDIO</a></strong> 에는 UWP 앱을 코딩, 디자인, 연결, 디버그, 분석, 최적화 및 테스트 하는 데 필요한 모든 도구가 포함 되어 있습니다. 또한 Visual Studio는 Windows 10 장치에 대 한 <strong><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/test-with-the-emulator">에뮬레이터</a></strong> 를 제공 하므로 다양 한 에뮬레이트된 장치에서 앱을 테스트할 수 있습니다.<br/><br/><a href="https://developer.microsoft.com/windows/downloads">UWP 용 다운로드 및 도구</a></td>
</tr>
<tr class="even">
<td align="left"><strong>코드 구성.</strong> <br><br>일반적으로 초기 템플릿에서 만든 앱의 기본 폴더 구조입니다.</td>
<td align="left"><strong>Androidmanifest</strong> 파일, 소스 파일을 포함 하는 <strong>java</strong> 폴더, 레이아웃 및 값을 포함 하는 리소스를 포함 하는 <strong>res</strong> 폴더, Android Studio의 빌드 스크립트 및 Eclipse의 <strong>Ant</strong> 빌드 스크립트를 <strong>Gradle</strong> 합니다.</td>
<td align="left">원본 파일 및 <strong>지원 파일</strong>, <strong>Info.plist</strong> 파일, <strong>기본 storyboard</strong> 및 <strong>launchscreen</strong>. 이미지는 <strong>자산 라이브러리</strong>에 저장 됩니다.</td>
<td align="left">UWP 앱에는 Example. .xaml 및 Example.xaml.cs와 같은 앱에 대 한 XAML 및 코드 파일, <strong>자산 폴더</strong>의 다양 한 이미지, <strong>mainpage .xaml</strong> , <strong>MainPage.xaml.cs</strong> 및 manifest와 같은 시작 페이지가 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">Hello 세계 앱 만들기</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">앱 수명 주기</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>앱 수명 주기.</strong> <br><br>앱 시작, 일시 중단, 다시 시작 및 닫기에서 이벤트를 처리 하 여 응용 프로그램 상태를 저장/복원 하 고 다른 작업을 실행할 수 있는 기회를 제공 합니다.</td>
<td align="left">각 활동에는 <strong>다시 시작</strong>됨과 같은 상태의 고유한 <strong>활동 수명 주기가</strong> 있습니다. <strong>Onresume</strong> 과 같은 <strong>수명 주기 콜백은</strong> <strong>활동 클래스</strong>에서 구현 됩니다.</td>
<td align="left"><strong>응용 프로그램 수명 주기에</strong> 는 <strong>일시 중단</strong>됨과 같은 상태가 있습니다. <strong>ApplicationDidEnterBackground:</strong> 와 같은 메서드는 상태 변경 시 코드를 실행 하기 위해 <strong>응용 프로그램 대리자 개체</strong> 에서 구현 됩니다.</td>
<td align="left">응용 프로그램의 <strong>앱 실행 상태</strong> 는 NotRunning, 활성화 됨, 실행 중, 일시 중단 됨, 다시 시작 중입니다.<br/><br/><strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application">응용 프로그램 클래스</a></strong> 메서드를 구현할 수 있습니다. 응용 프로그램에서 상태를 변경 하는 경우 코드를 실행 하기 위해 응용 프로그램에서 Onlaunched, 일시 중단 또는 다시 시작 합니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle">앱 수명 주기</a></td>
</tr>
<tr class="even">
<td align="left"><strong>백그라운드 작업.</strong> <br><br>백그라운드 작업을 수행 하 고 앱이 포그라운드에서 더 이상 없는 경우 계속 실행 되는 작업입니다.</td>
<td align="left">앱은 앱이 더 이상 포그라운드에 없을 때 백그라운드 작업을 수행 하는 <strong>서비스</strong> 를 시작할 수 있습니다. 서비스는 자체 <strong>수명 주기</strong> 를 가지 며 매니페스트에 등록 됩니다.</td>
<td align="left"><strong>백그라운드 실행</strong> 은 특정 작업 유형에 대해서만 허용 됩니다.<br/><br/>앱은 <strong>UIBackgroundModes</strong>를 사용 하 여 info.plist 파일에서 <strong>지원 되는 백그라운드 작업</strong> 을 선언 합니다.<br/><br/>시스템은 백그라운드 작업이 실행 되는 시간 및 기간에 대 한 제어를 제어 합니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask">IBackgroundTask</a></strong> 인터페이스를 구현 하 고 응용 프로그램 매니페스트에 작업을 등록 하 여 백그라운드 작업을 만들 수 있습니다. <a href="https://docs.microsoft.com/windows/uwp/launch-resume/run-a-background-task-on-a-timer-"><strong>타이머</strong></a>, <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>시스템 트리거</strong></a>및 <a href="https://docs.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>유지 관리 트리거</strong></a>를 사용 하 여 작업을 트리거하도록 설정할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks">백그라운드 작업을 사용하여 앱 지원</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task">백그라운드 작업 만들기 및 등록</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/guidelines-for-background-tasks">백그라운드 작업 지침</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">성능</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>성능 모범 사례</strong> <br><br>신속 하 고 응답성이 뛰어난 앱을 빌드하기 위한 지침으로, 빠른 시작 시간을 사용 하 여 배터리 수명을 간에.</td>
<td align="left">Android는 성능 교육 가이드를 <strong>위한 모범 사례</strong> 를 제공 합니다.</td>
<td align="left">iOS는 <strong>성능 개요</strong> 문서를 제공 합니다.</td>
<td align="left">과 같은 항목에 대 한 섹션을 참조 하 여 자세한 <strong><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui">성능 가이드</a></strong> 를 읽을 수 있습니다. 성능 목표, 성능 측정, 메모리 관리, 부드러운 애니메이션, 효율적인 파일 시스템 액세스 및 프로 파일링 및 성능에 사용할 수 있는 도구를 설정 합니다.</td>
</tr>
<tr class="even">
<td align="left"><strong>응답성이 뛰어난 UI에 대 한 최적화를 확인 합니다.</strong> <br><br>뷰를 최적화 하 여 성능을 향상 시킵니다.</td>
<td align="left">계층 구조 뷰어 도구를 사용 하 여 <strong>레이아웃 계층</strong> 최적화, <strong>레이아웃 재사용</strong> 및 <strong>주문형 뷰</strong> 로드는 UI 스레드를 응답성을 유지 하 고 &quot; 응용 프로그램 응답 없음 &quot; 대화 (<strong>ANR</strong>)를 방지 하는 데 도움이 되는 모든 기술입니다.<br/></td>
<td align="left"><strong>오프 스크린 렌더링</strong>을 사용 하 여 ui 문제를 해결 하 고, <strong>혼합 레이어</strong>를 사용 하 고, <strong>코어 애니메이션</strong> 도구를 사용 하 여 <strong>래스터화</strong> 를 통해 ui 스레드를</td>
<td align="left">몇 가지 간단한 단계를 수행 하 여 XAML <strong>태그</strong> 및 <strong>레이아웃</strong> 을 쉽게 <strong>최적화할</strong> 수 있습니다. 기술에는 레이아웃 구조를 줄이고 요소 수를 최소화 하 고 과도 한 그리기를 최소화 하는 작업이 포함 됩니다. <br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">UI 스레드 응답 유지</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-xaml-loading">XAML 태그 최적화</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-your-xaml-layout">XAML 레이아웃 최적화</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>스레딩을.</strong> <br><br>스레딩을 사용 하 여 응답성이 <strong>뛰어난 UI</strong> 를 유지 관리 하 고 여러 <strong>작업을 병렬로</strong>실행 합니다.</td>
<td align="left">실행 가능, <strong>처리기</strong>, <strong>ThreadPoolExecutor</strong>및 상위 수준 <strong>asynctask</strong> <strong>클래스를 사용</strong>하 여 스레딩을 구현할 수 있습니다.</td>
<td align="left">스레딩은 <strong>Nsthread</strong>, <strong>그랜드 Central 디스패치</strong>및 상위 수준 <strong>NSOperation</strong>를 사용 하 여 달성 됩니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync">Runasync</a></strong>를 사용 하 여 <strong>threadpool</strong> 에 <strong>작업 항목</strong> 을 제출 하 여 스레드 작업을 수행할 수 있습니다. 타이머를 사용 하 여 <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer">Createtimer</a></strong> 를 사용 하 여 작업 항목을 제출 하 고 <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">CreatePeriodicTimer</a></strong>를 사용 하 여 반복 작업 항목을 만들 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">스레드 풀에 작업 항목 제출</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">타이머를 사용하여 작업 항목 제출</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/create-a-periodic-work-item">정기 작업 항목 만들기</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">스레드 풀을 사용하기 위한 모범 사례</a></td>
</tr>
<tr class="even">
<td align="left"><strong>비동기 프로그래밍.</strong> <br><br>UI 스레드 응답성을 유지 하기 위해 비동기 프로그래밍 패턴을 활용 하 여 스레딩 복잡성을 방지 합니다.</td>
<td align="left">사용자 고유의 비동기 클래스를 만들려면 <strong>스레딩을 사용 해야</strong> 합니다. 일부 기본 제공 클래스는 비동기입니다.</td>
<td align="left">사용자 고유의 비동기 클래스를 만들려면 <strong>스레딩을</strong> 사용 해야 합니다. 일부 기본 제공 클래스는 비동기입니다.</td>
<td align="left">비동기 패턴을 사용 하 여 사용자 고유의 Api를 만들 때 주 스레드가 차단 되는 것을 방지할 수 있습니다. 예를 들어, 비동기 <strong>를 사용 하</strong> 고 c #에서 <strong>wait</strong> 를 사용 하 고 Visual Basic <strong>Async</strong>라는 단어로 끝나는 비동기 기본 제공 api를 사용할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">비동기 프로그래밍</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">C# 또는Visual Basic에서 비동기식 API 호출</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>목록 뷰 최적화입니다.</strong> <br><br>데이터 목록을 최적화 하는 데 도움이 되는 기본 제공 패턴으로 많은 양의 데이터를 표시 해야 하는 경우 성능이 저하 되는 경우가 많습니다.</td>
<td align="left"><strong>ViewHolder</strong> 디자인 패턴은 재사용 가능한 UI 요소를 사용할 수 있도록 하는 여러 뷰 조회를 방지 하는 데 사용 됩니다.</td>
<td align="left"><strong>Uitableview</strong>의 성능을 향상 시키기 위해 최적화 범위를 만들 수 있으며, 기본 제공 되는 경우는 없습니다.</td>
<td align="left"><strong>UI 가상화</strong> 를 기본적으로 제공 하는 <a href="/uwp/api/windows.ui.xaml.controls.listview">ListView</a> 및 <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview">GridView</a> 컨트롤을 사용 하 여 부드러운 패닝 및 스크롤 환경을 제공 하 고 시작 시간을 단축할 수 있습니다. 데이터 원본에서 <a href="/dotnet/api/system.collections.ilist">IList</a> 및 <a href="https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged">INotifyCollectionChanged</a> 를 구현 하 여 <strong>데이터 가상화</strong> 를 제공 하 고 성능을 향상 시킬 수도 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview">ListView 및 GridView UI 최적화</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">ListView 및 GridView 데이터 가상화</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">수익 창출</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>앱에서 바로 구매.</strong> <br><br>사용자가 앱에서 구매할 수 있게 해 주는 플랫폼 기능.</td>
<td align="left"><strong>앱 내 청구</strong> 는 Google 서비스에서 제공 합니다. 제품이 <strong>Google Play 개발자 콘솔</strong>에 추가됩니다. 앱 내 구매는 <strong>Google Play 청구 라이브러리</strong>를 사용 하 여 구현 됩니다.</td>
<td align="left">제품이 <strong>ITunes Connect</strong>에 추가 됩니다. 앱에서 바로 구매는 기능 <strong>키트</strong> 프레임 워크를 사용 하 여 구현 됩니다.<br/><br/>제품은 <strong>SKMutablePayment</strong> 및 <strong>SKPaymentQueue</strong>를 사용 하 여 구매 합니다.</td>
<td align="left">앱을 앱에 <a href="https://docs.microsoft.com/windows/uwp/publish/iap-submissions">추가 하 고 스토어에 제출</a>하 여 앱에 대 한 앱 내 제품 구매를 만듭니다. <br/><br/><strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp">Currentapp 클래스</a></strong> 를 사용 하 여 앱 내 구매를 정의 합니다. <br/><br/>RequestProductPurchaseAsync를 사용 하 여 고객이 제품을 구매할 수 있도록 하는 UI를 표시 합니다 <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">.</a></strong><br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases">앱에서 바로 구매 제품 사용</a></td>
</tr>
<tr class="even">
<td align="left"><strong>앱에서 바로 구매를 사용할 때</strong> <br><br>구매 하 여 사용 하 고 다시 구매할 수 있는 앱 내 제품입니다.</td>
<td align="left">사용 가능한 구매는 정기적인 구매를 수행한 다음 <strong>consumePurchase</strong>와 함께 사용 하 여 구매 하 고 사용 하 여 다시 구매할 수 있게 합니다.</td>
<td align="left">사용할 수 있는 제품은 iTunes Connect에서 사용할 수 있는 <strong>제품으로 정의</strong> 됩니다.</td>
<td align="left">저장소 <a href="https://docs.microsoft.com/windows/uwp/publish/enter-iap-properties">에 제출할 때 제품 유형을 사용할 수 있는 것으로 정의</a> 하 여 소모품을 지원할 수 있습니다. 그런 다음 고객에 게 액세스를 허용 하도록 ReportConsumableFulfillmentAsync 구매 후 <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">Currentapp.</a></strong> 를 호출 합니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-in-app-product-purchases">앱에서 사용할 수 있는 구매 사용</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>앱에서 바로 구매를 테스트 합니다.</strong> <br><br>앱을 스토어에 배치 하지 않고도 앱 내 구매 코드를 테스트할 수 있습니다.</td>
<td align="left"><strong>앱 내 청구 샌드박스</strong> 는 테스트에 사용 됩니다.</td>
<td align="left"><strong>샌드박스 테스터 계정은</strong> 테스트에 사용 됩니다.</td>
<td align="left">CurrentApp 대신 <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator">Currentappsimulator</a></strong> 클래스를 사용 하 여 앱 내 구매를 테스트할 수 있습니다.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>평가판.</strong> <br><br>앱의 평가판을 기준으로 콘텐츠를 쉽게 제한 하거나 광고를 제거할 수 있습니다.</td>
<td align="left">Google Play는 <strong>앱 평가판을 공식적으로 지원 하지 않습니다</strong>. 구입을 완료 했는지 확인 하는 경우 앱 내 구매를 만들고 적절 한 코드 경로를 만들어 보급을 시험 하거나 제거 합니다.</td>
<td align="left">앱 스토어는 <strong>앱 평가판을 공식적으로 지원 하지 않습니다</strong>. 구입을 완료 했는지 확인 하는 경우 앱 내 구매를 만들고 적절 한 코드 경로를 만들어 보급을 시험 하거나 제거 합니다.</td>
<td align="left">앱을 스토어에 제출할 때 <strong><a href="https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability">' 무료 평가판 ' 옵션</a></strong> 을 사용 하 여 앱의 무료 평가판 버전을 제공할 수 있습니다. 그런 다음 <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">LicenseInformation</a></strong> 를 사용 하 여 앱의 평가판 상태를 확인 하 고 다른 코드 경로를 그에 따라 표시할 수 있습니다. 앱이 실행 되는 동안 사용자가 평가판 상태를 변경 하는 경우 알리도록 <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged 이벤트</a> 를 등록할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">평가판의 기능 제외 또는 제한</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">여러 플랫폼에 적응</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>적응 UI: 유연한 레이아웃.</strong> <br><br>높이가 유연 하 고 너비가 다른 화면 크기를 지원 합니다.</td>
<td align="left">LinearLayout 개체의 <strong>wrap_content</strong> 및 <strong>match_parent</strong> 값을 사용 하거나 맞춤에 RelativeLayout 개체를 사용 하 여 유연한 레이아웃을 구현할 수 있습니다.</td>
<td align="left">HorizontalSizeClass 및 displayScale와 같은 <strong>제약 조건</strong> 및 <strong>특성</strong> 을 사용 하 여 뷰 컨트롤러에 적용 되는 <strong>자동 레이아웃</strong> 을 사용 하 여 유연한 레이아웃을 범용 <strong>storyboard와 함께</strong> 사용 하 여 유연한 레이아웃을 구현할 수 있습니다.</td>
<td align="left">고정 및 동적 크기 조정을 조합한 <strong>레이아웃 속성</strong> 및 <strong>패널</strong> 을 사용 하 여 유체 레이아웃을 만들 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML 레이아웃 속성 및 패널을 사용 하 여 레이아웃 정의</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">응답성이 뛰어난 디자인 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>적응 UI: 맞춤형 레이아웃.</strong> <br><br>별도의 대상 레이아웃을 사용 하 여 다양 한 화면 크기 지원.</td>
<td align="left"><strong>Small</strong>, <strong>large</strong>, <strong>ldpi</strong>및 <strong>hdpi</strong> 와 같은 <strong>구성 한정자</strong> 를 사용 하 여 resources 디렉터리에서 다른 화면 구성에 대 한 대체 레이아웃 파일을 제공 하면 사용자 지정 레이아웃을 다양 한 크기와 밀도의 화면으로 지정할 수 있습니다.</td>
<td align="left">유니버설 앱에서 다양 한 장치 제품군에 맞게 레이아웃을 조정 하는 <strong>별도 iPhone 및 IPad 스토리 보드</strong> 를 정의 합니다.</td>
<td align="left">장치 제품군 마다 <strong>다른 XAML 태그 파일</strong> 을 정의 하 여 맞춤형 레이아웃을 빌드할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML 맞춤 레이아웃을 사용 하 여 레이아웃 정의</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>적응 UI: 반응 형 레이아웃입니다.</strong> <br><br>화면 크기 변경에 대 한 응답 (예: 회전) 또는 창 크기 변경</td>
<td align="left"><strong>LinearLayout</strong> 및 <strong>RelativeLayout</strong>에서 유연한 레이아웃을 사용 하거나 다른 방향에 대 한 대체 레이아웃 파일을 제공 하 여 응답성이 뛰어난 레이아웃을 가능 하 게 합니다.</td>
<td align="left">뷰의 <strong>크기나</strong> <strong>특성이</strong> 변경 되 면 storyboard에 지정 된 <strong>제약 조건이</strong> 적용 됩니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate">Visualstate</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager">r</a></strong> 및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong>를 사용 하 여 창 크기 변경에 대 한 응답으로 런타임에 UI의 섹션을 쉽게 다시 배치 하 고, 위치를 변경 하거나, 크기를 조정 하거나, 표시 하거나, 표시할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML을 사용 하 여 레이아웃 정의-시각적 상태 및 상태 트리거</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">응답성이 뛰어난 디자인 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>다른 장치 기능을 지원 합니다.</strong> <br><br>고급 하드웨어 기능을 활용 하면서도 장치를 지원 하지 않는 장치를 지원 합니다.</td>
<td align="left">런타임에 장치 기능 테스트 <strong>PackageManager 기능</strong> 을 사용 하면 하드웨어 관련 코드를 실행할 수 있는지 여부를 결정할 수 있습니다.</td>
<td align="left">런타임 시 디바이스 기능을 테스트하기 위해 수행할 수 있는 <strong>단일 검사는 없으며</strong> 특정 방법으로 각 기능을 테스트하여 하드웨어 관련 코드를 실행할 수 있는지 확인합니다.</td>
<td align="left"><strong>플랫폼 확장 sdk</strong> 를 패키지에 추가 하 여 휴대폰, 데스크톱 및 IoT를 비롯 한 다양 한 장치 제품군에 있는 추가 기능을 대상으로 지정할 수 있습니다. <strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation">Apiinformation API</a></strong> 를 사용 하 여 런타임에 형식 및 멤버의 존재 여부를 테스트 하 고, 있는 경우에만 해당 형식과 멤버를 호출할 수 있습니다.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>다른 장치 기능을 지원 합니다.</strong> <br><br>고급 하드웨어 기능을 활용 하면서도 장치를 지원 하지 않는 장치를 지원 합니다.</td>
<td align="left"><strong>Android 지원 라이브러리</strong> 를 앱과 함께 패키지 하 여 이전 버전의 android에서 최신 api를 사용할 수 있도록 할 수 있습니다. 런타임에 API 수준에 대 한 테스트는 <strong>SDK_INT</strong>을 사용 하 여 수행할 수 있습니다.</td>
<td align="left">표준 런타임 검사는 클래스의 존재 여부를 확인 하 고 <strong>respondsToSelector:</strong> 클래스의 메서드를 확인 하는 <strong>클래스</strong> 메서드와 같은 api를 사용할 수 있는지 확인 하는 데 사용 됩니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">IsApiContractPresent</a></strong> 를 사용 하 여 지정 된 주 및 부 번호의 API 계약이 있는지를 식별할 수 있습니다. 또한 <strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation">Apiinformation API</a></strong> 를 사용 하 여 런타임에 형식 및 멤버의 존재 여부를 테스트 하 고, 있는 경우에만 해당 형식과 멤버를 호출할 수 있습니다.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">공지</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>타일 및 배지.</strong> <br><br>홈 화면에 사용자에 대 한 업데이트를 제공 합니다.</td>
<td align="left"><strong>앱 위젯은</strong> 홈 화면에 포함 될 수 있는 응용 프로그램의 뷰입니다. 정기적 업데이트를 받을 수 있습니다. Android에 <strong>배지 시스템이 없습니다</strong> . 타일에 동일한 시스템이 없습니다.</td>
  <td align="left">IOS m의 <strong>위젯은</strong> 알림 센터에 표시 되며 <strong>앱 확장</strong>으로 구현 됩니다. 로컬 또는 원격 알림에 대 한 응답으로 변경 될 수 있는 번호를 사용 하 여 아이콘에 <strong>배지</strong> 를 추가할 수도 있습니다. 타일 시스템이 없습니다.</td>
<td align="left">앱에는 시작 화면에 고정할 수 있는 <strong>타일이</strong> 있으며 텍스트, 이미지 및 문자 모양과 숫자가 포함 된 <strong>배지</strong> 를 표시 하는 데 사용 됩니다. 앱에서 타일의 콘텐츠를 업데이트할 수 있습니다. 푸시 알림 또는 미리 정의 된 일정을 통해 타일은 적응이 가능 하며 표시 되는 위치에 따라 변경 될 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">타일 만들기</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">적응형 타일 만들기</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">알림 전달 방법 선택</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">타일 및 배지에 대한 지침</a></td>
</tr>
<tr class="even">
<td align="left"><strong>알림 표시</strong> <br><br>표시할 수 있는 알림 유형입니다.</td>
<td align="left">알림 <strong>영역</strong> 및 <strong>알림 서랍</strong>에 알림이 표시 될 수 있으며, <strong>준비 알림은</strong> 작은 부동 창에 알림을 제공 합니다. 알림은 <strong>Pendingintent</strong>를 정의 하 여 추가 된 작업을 포함할 수 있습니다.</td>
<td align="left">팝업 알림은 <strong>배너나</strong> <strong>경고</strong>로 표시 됩니다. <strong>Uimutableusernotificationaction</strong>으로 정의 된 <strong>조치 가능한 알림에</strong> 사용자 지정 작업 단추를 추가할 수 있습니다.</td>
<td align="left">알림 <strong>알림</strong>이라는 적응 팝업 알림을 만들 수 있습니다. 시각적 콘텐츠, 단추가 될 수 있는 <strong>작업</strong> , 입력 및 오디오를 사용 하 여 XML에서 알림을를 정의할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">적응형 및 대화형 알림 메시지</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">알림 전달 방법 선택</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications">알림 메시지에 대한 지침</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>로컬 알림 예약</strong> <br><br>예약 된 시간에 앱에서 보낸 로컬 알림</td>
<td align="left">알림과 작업은 <strong>Notificationcompat</strong> 를 사용 하 여 정의 되며, <strong>AlarmManager</strong> 및 <strong>BroadcastReceiver</strong>를 사용 하 여 앱 내에서 예약 하 고 처리할 수 있습니다.</td>
<td align="left">로컬 알림은 <strong>UILocalNotification</strong> <b> scheduleLocalNotification:<strong>. |를 사용 하 여 예약 될 수 있습니다. ScheduledToastNotification를 사용 하 여 알림 메시지를 예약할 수 </strong> <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">ScheduledToastNotification</a>있습니다<strong>. TileNotification 클래스를 사용 하 여 앱에서 타일 알림을 보내거나 </strong> <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification">TileNotification class</a> <strong> <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">ScheduledTileNotification</a>로 타일 알림을 예약할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">적응형 및 대화형 알림 메시지</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">로컬 타일 알림 보내기</a> | | </strong>푸시 알림을 보내는 중입니다.</b> 푸시 알림 서버에서 전송 되 고 선택적으로 앱 내에서 처리 되는 알림입니다.</td>
<td align="left"><strong>Google Cloud Messaging</strong> 는 Android에 대 한 푸시 알림 지원을 제공 합니다.</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">미디어 캡처 및 렌더링</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>미디어 캡처.</strong> <br><br>오디오 및 시각적 콘텐츠 기록</td>
<td align="left">MediaStore와 같은 <strong>의도</strong> 를 사용 하면 기존 카메라 앱으로 미디어를 캡처할 수 ACTION_VIDEO_CAPTURE. <strong>Camera2</strong> 또는 <strong>카메라</strong> 라이브러리를 사용 하면 사용자 지정 카메라 인터페이스를 구현할 수 있습니다. <strong>Mediarecorder</strong> Api는 오디오를 캡처하는 데 사용할 수 있습니다.</td>
<td align="left"><strong>Uiimagepickercontroller</strong> 를 사용 하면 시스템 UI로 비디오와 사진을 캡처할 수 있습니다. <strong>AVCaptureSession</strong> 와 같은 <strong>avfoundation</strong> 클래스는 카메라에 직접 액세스할 수 있습니다. <br/><strong>AVAudioRecorder</strong> 클래스는 오디오 녹음을 사용 하도록 설정 합니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI">CameraCaptureUI 클래스</a></strong>를 사용 하 여 기본 제공 카메라 UI를 사용 하는 동안 사진과 비디오를 캡처할 수 있습니다. 낮은 수준에서 카메라와 상호 작용 하 고 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture">MEDIACAPTURE API</a></strong>와 같은 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture">Windows</a></strong> 의 클래스를 사용 하 여 오디오를 캡처할 수 있습니다. <br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">CameraCaptureUI를 사용 하 여 사진 및 비디오 캡처</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">MediaCapture를 사용 하 여 사진 및 비디오 캡처</a></td>
</tr>
<tr class="even">
<td align="left"><strong>미디어 재생.</strong> <br><br>오디오 및 비디오 파일 재생</td>
<td align="left"><strong>MediaPlayer</strong> 및 오디오 <strong>관리자</strong> 클래스는 오디오 및 비디오 파일을 재생 하는 데 사용 됩니다.</td>
<td align="left"><strong>Avkit 프레임 워크</strong>, <strong>avkit Player</strong>및 <strong>Media Player 프레임 워크</strong> 는 오디오 및 비디오 파일을 재생 하는 데 사용 됩니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource">MediaSource 클래스</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong>및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong> 클래스를 사용 하 여 로컬 및 원격 파일과 같은 소스에서 오디오 및 비디오를 재생할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource">MediaSource를 사용 하 여 미디어 재생</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>미디어를 편집 합니다.</strong> <br><br>기존 기록에서 새 미디어 파일을 작성 하 고 특수 효과를 적용 합니다.</td>
<td align="left"><strong>Mediacodec</strong>, <strong>MediaMuxer</strong>및 <strong>android</strong> 와 같은 하위 수준 클래스는 콘텐츠를 편집 하는 데 사용할 수 있습니다.</td>
<td align="left"><strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong>및 <strong>AVMutableAudioMix</strong> 와 같은 <strong>AV 기반</strong> 프레임 워크의 클래스는 콘텐츠 편집에 사용할 수 있습니다.</td>
<td align="left">미디어를 사용 하 여 <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition">Mediacomposition</a></strong> 및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip">Mediaclip</a></strong> 와 같은 <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing">편집</a></strong> api를 사용 하 여 오디오 및 비디오 파일에서 미디어 컴퍼지션을 만들 수 있습니다. 비디오 및 이미지 오버레이를 추가 하 고, 비디오 클립을 결합 하 고, 배경 오디오를 추가 하 고, 오디오 및 비디오 효과를 적용할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-compositions-and-editing">미디어 컴퍼지션 및 편집</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">센서</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>센서.</strong> <br><br>장치 이동, 위치 및 환경 속성을 검색 합니다.</td>
<td align="left"><strong>센서 프레임 워크</strong> 는 <strong>SensorManager</strong> 및 <strong>SensorEvent</strong>와 같은 클래스를 사용 하 여 하드웨어 및 소프트웨어 센서에 액세스 하는 데 사용 됩니다.</td>
<td align="left"><strong>핵심 동작 프레임 워크</strong> 는 원시 및 처리 된 센서 데이터에 액세스 하는 데 사용 됩니다.</td>
<td align="left">센서에서 새로운 데이터 읽기가 수신 될 때 트리거되는 센서 판독값 및 이벤트에 액세스 하려면 <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.sensors">Windows. 센서</a></strong> 의 클래스를 사용할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/devices-sensors/sensors">센서</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">위치 및 매핑</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>위치도.</strong> <br><br>장치의 <strong>현재</strong> 위치를 찾고 <strong>변경 내용을</strong>추적 합니다.</td>
<td align="left">Google Play services 위치 Api는 <strong>getLastLocation</strong> 및 <strong>requestlocationupdates</strong> 메서드를 사용 하 여 <strong>퓨즈 위치 공급자</strong> 를 사용 하 여 <strong>마지막으로 알려진 위치</strong> 에 대 한 높은 수준의 액세스를 제공 합니다. <strong>LocationManager</strong>를 사용하여 Android 라이브러리에 낮은 수준 액세스를 제공합니다.</td>
<td align="left"><strong>핵심 위치</strong> <strong>cllocationmanager</strong> 클래스는 표준 위치 서비스에 대 한 <strong>startUpdatingLocation</strong> 및 <strong>중요 한 변경</strong> 위치 서비스의 <strong>startMonitoringSignificantLocationChanges</strong> 을 사용 하 여 장치의 위치를 모니터링 하는 데 사용 됩니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation">Windows. 장치</a></strong>에서 클래스를 사용 하 여 장치 위치를 추적할 수 있습니다. 한 번의 읽기에 대해 <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">Geolocator를 사용 합니다. Getgeoposit, async</a></strong> 를 사용 합니다. Geolocator를 사용 하 여 타이머를 사용 하 여 정기적으로 위치를 구하거 나 위치가 변경 될 때 알려 주어 야 <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged">합니다.</a></strong><br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/get-location">사용자 위치 가져오기</a></td>
</tr>
<tr class="even">
<td align="left"><strong>지도를 표시 합니다.</strong> <br><br><strong>대화형 기본 제공 맵을</strong> 표시 하 고 <strong>원하는 위치</strong>를 추가 합니다.</td>
<td align="left"><strong>Google Maps ANDROID API</strong> 내의 <strong>GoogleMap</strong>, <strong>mapfragment</strong>및 <strong>mapfragment</strong> 클래스는 응용 프로그램에 맵을 포함할 수 있도록 허용 합니다. <strong>표식을</strong> 사용 하 고 사용자 지정 가능한 <strong>마커</strong> 클래스를 사용 하 여 관심 영역을 표시할 수 있습니다.</td>
<td align="left">Maps는 <strong>Mapkit 프레임 워크</strong>의 <strong>MKMapView</strong> 클래스를 사용 하 여 iOS 앱에 포함 됩니다. <strong>MKPointAnnotation</strong> 와 같은 개체 클래스와 <strong>MKPinAnnotationView</strong>등의 뷰 클래스를 사용 하 여 관심 지점이 표시 되도록 앱에 <strong>주석을</strong> 추가할 수 있습니다.</td>
<td align="left">2D, 3D 및 streetside 뷰를 제공 하는 기본 제공 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">없습니다</a></strong> XAML 컨트롤을 사용 하 여 응용 프로그램에 맵을 포함할 수 있습니다. <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon">Mapicon</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolygon">MapPolygon</a></strong> 및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline">MapPolyline</a></strong>와 같은 클래스를 사용 하 여 압정, 이미지 또는 모양으로 관심 요소를 추가할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps">2D, 3D 및 Streetside 뷰가 있는 지도 표시</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/display-poi">지도에 POI(관심 지점) 표시</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>지 오 펜싱.</strong> <br><br>특정 지리적 지역을 입력 하 고 종료 하는 것을 모니터링 합니다.</td>
<td align="left">지역 구분는 Google Play 서비스 SDK의 <strong>위치 서비스</strong> 를 사용 하 여 모니터링 됩니다.</td>
<td align="left">지역은 <strong>CLCircularRegion</strong> 클래스를 사용 하 여 모니터링 되 고 <strong>cllocationmanager. startMonitoringForRegion:</strong>에 등록 됩니다.</td>
<td align="left">지 <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence">오</a></strong> 클래스를 사용 하 여 지 오를 만들고 영역을 시작 하거나 종료 하는 등의 모니터링 되는 <strong>상태</strong> 를 정의할 수 있습니다. <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">GeofenceMonitor 클래스</a></strong>를 사용 하 여 포그라운드에서 지 오 이벤트를 처리 하 고 <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.locationtrigger">locationtrigger background 클래스</a></strong>를 사용 하 여 백그라운드에서 처리 합니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence">지오펜스 설정</a></td>
</tr>
<tr class="even">
<td align="left"><strong>지 오 코딩 및 reverse 지 오 코딩.</strong> <br><br>주소를 지리적 위치 (지 오 코딩)로 변환 하 고 지리적 위치를 주소로 변환 (역방향 지 오 코딩).<br/></td>
<td align="left"><strong>Geocoder</strong> 클래스는 지 오 코딩 및 역방향 지 오 코딩에 사용 됩니다.</td>
<td align="left"><strong>CLGeocoder</strong> 클래스는 지 오 코딩에 사용 됩니다.</td>
<td align="left">지 오 코딩에서 <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder">MapLocationFinder 클래스</a></strong> 를 사용 하 여이를 수행할 수 <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps">있습니다.</a></strong> 지 오 코딩 및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">FindLocationsAtAsync</a></strong> for reverse 지 오 코딩에 <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong> 를 사용 합니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/geocoding">지오코딩 및 역방향 지오코딩 수행</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>경로 및 방향.</strong> <br><br>두 지리적 위치 사이에 경로, 거리 및 방향 제공</td>
<td align="left">Google은 SDK를 제공 하지 않더라도 Android에서 사용할 수 있는 웹 서비스 <strong>Google 지도 방향 API</strong> 를 제공 합니다.</td>
<td align="left">지도 키트는 경로 및 방향에 대 한 정보를 가져오는 데 사용할 수 있는 <strong>MKDirections</strong> API를 제공 합니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder">MapRouteFinder에서</a></strong> 클래스를 사용 하 여 탐색 또는 구동 경로를 요청할 수 <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps">있습니다.</a></strong> 경로는 없습니다에 쉽게 표시 될 수 있는 <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproute">Maproute</a></strong> 인스턴스로 반환 됩니다. 방향은 <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong> 개체 내에서 반환 됩니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/routes-and-directions">지도에 경로 및 방향 표시</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">앱 간 통신</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>다른 앱 호출</strong> <br><br>다른 앱을 시작 하 고 필요에 따라 링크, 텍스트, 사진, 동영상, 파일 등의 데이터를 공유 합니다.</td>
<td align="left"><strong>암시적 의도</strong> 는 <strong>의도</strong> 한 대로 <strong>작업</strong> 및 선택적 데이터를 정의 하 고 <strong>startactivityforresult</strong>로 호출 하 여 다른 앱을 시작 하는 데 사용 됩니다.<br/></td>
<td align="left">앱 <strong>확장</strong> 은 다른 앱에 대 한 앱 데이터 액세스를 제공 하는 데 사용할 수 있습니다. <strong>Url 스키마</strong> 를 사용 하면 url을 다른 앱에 전달할 수 있습니다.</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync">LaunchUriAsync</a></strong>또는 <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriforresultsasync">LaunchUriForResultsAsync</a></strong> 를 사용 하 여 URI에 대해 등록 된 다른 앱을 시작 하 여 결과를 시작 하 고 시작 된 앱에서 데이터를 다시 가져올 수 있습니다. <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync">LaunchFileAsync</a></strong> 를 사용 하 여 처리할 파일을 다른 앱에 전달할 수 있습니다.<br/><br/><strong>공유 계약</strong> 을 사용 하 여 앱 간에 데이터를 쉽게 공유할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/launch-default-app">URI에 대한 기본 앱 실행</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/how-to-launch-an-app-for-results">결과를 위한 앱 실행</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/launch-the-default-app-for-a-file">파일에 대한 기본 앱 시작</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/share-data">데이터 공유</a></td>
</tr>
<tr class="even">
<td align="left"><strong>앱을 호출할 수 있도록 허용 합니다.</strong> <br><br>앱이 다른 앱의 요청에 응답할 수 있도록 허용 합니다.</td>
<td align="left">앱은 <strong>의도 필터</strong> 를 사용 하 여 <strong>의도 처리 활동</strong> 을 등록 하 여 다른 앱의 암시적 의도에 응답 합니다.</td>
<td align="left"><strong>앱 확장</strong> 을 패키지 하면 다른 앱과 데이터를 공유할 수 있습니다. 앱은 info.plist의 <strong>CFBundleURLTypes</strong> 키를 사용 하 여 <strong>사용자 지정 URL 체계</strong> 를 등록할 수 있습니다.</td>
<td align="left">패키지 매니페스트에 <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">프로토콜</a></strong> 을 등록 하 고 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated">응용 프로그램</a></strong> 을 업데이트 하 여 (필요에 따라 결과 반환) 응용 프로그램을 <strong>URI 스키마 이름</strong> 에 대 한 기본 처리기로 등록할 수 있습니다. 패키지 매니페스트에 선언을 추가 하 고 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated">응용 프로그램. OnFileActivated</a></strong> 된 이벤트를 처리 하 여 응용 프로그램을 특정 파일 형식에 대 한 기본 처리기로 등록 하는 것과 동일한 방식으로 사용할 수 있습니다.<br/><br/><strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsharetargetactivated">응용 프로그램</a></strong> 을 매니페스트에서 공유 대상으로 등록 하 고 OnShareTargetActivated 이벤트를 처리 하 여 공유 계약 요청을 처리할 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/how-to-launch-an-app-for-results">결과를 위한 앱 실행</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/handle-file-activation">파일 활성화 처리</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/receive-data">데이터 수신</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>복사 하 여 붙여 넣습니다.</strong> <br><br>앱 간에 텍스트 및 기타 콘텐츠를 복사 하 여 붙여 넣습니다.</td>
<td align="left"><strong>클립보드 프레임 워크</strong> 를 사용 하 여 <strong>ClipboardManager</strong> 및 <strong>ClipData</strong> 클래스를 사용 하 여 복사 및 붙여넣기를 구현할 수 있습니다.</td>
<td align="left"><strong>Uipasteboard</strong>나 <strong>uimenucontroller</strong>및 <strong>UIResponderStandardEditActions</strong> 를 사용 하 여 복사 및 붙여넣기를 구현할 수 있습니다.</td>
<td align="left">많은 기본 XAML 컨트롤이 이미 복사 및 붙여넣기를 지원 합니다. <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong> 및 DataTransfer의 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">클립보드</a></strong> 클래스를 사용 하 여 복사 및 붙여넣기를 구현할 수 있습니다 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer">.</a></strong><br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/copy-and-paste">복사 및 붙여넣기</a></td>
</tr>
<tr class="even">
<td align="left"><strong>끌어 놓습니다.</strong> <br><br>앱 간에 콘텐츠 끌어서 놓기</td>
<td align="left">끌어서 놓기는 <strong>Android 끌어서 놓기 프레임 워크</strong>를 사용 하 여 단일 응용 프로그램 내에서 구현할 수 있습니다.</td>
<td align="left">IOS에서 제공 하는 상위 수준의 끌어서 놓기 Api는 없습니다.</td>
<td align="left">앱 간, 데스크톱-앱 및 앱 간 끌어서 놓기 기능을 사용 하도록 앱에서 끌어서 놓기를 구현할 수 있습니다. <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop">Allowdrop</a></strong>, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.candrag">Candrag</a></strong> 속성, <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover">system.windows.uielement.dragover></a></strong>및 <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop">drop</a></strong> 이벤트를 사용 하 여 UIElement 클래스에서 끌어서 놓기를 지원 합니다.<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop">끌어서 놓기</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">소프트웨어 디자인</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>소프트웨어 디자인 패턴.</strong> <br><br>플랫폼에 권장 되거나 잘 사용 되는 패턴입니다.</td>
<td align="left">베타 데이터 바인딩 프레임 워크는 <strong>MVVM (모델-뷰-ViewModel)</strong> 패턴을 더욱 광범위 하 게 사용할 수 있지만 Android 개발용으로는 공식적인 패턴을 권장 하거나 제공 하지 않습니다. 많은 타사 문서와 프레임 워크에서 <strong>MVP (모델-뷰-발표자)</strong> 및 <strong>MVVM</strong> 접근 방법을 권장 합니다.</td>
<td align="left"><strong>MVC (모델-뷰-컨트롤러)</strong> 는 iOS에서 사용 되는 일반적인 패턴으로, 플랫폼에 통합 되어 있습니다.</td>
<td align="left">UWP에 대해 빌드할 때 특정 패턴으로 제한 되지 않습니다.<br/><br/>기본 제공 <a href="https://docs.microsoft.com/windows/uwp/data-binding/index">데이터 바인딩</a> 패턴을 사용 하 여 데이터 문제 및 ui 문제를 명확 하 게 분리 하 고, 속성 값을 업데이트 하는 ui 이벤트 처리기를 코딩할 필요가 없도록 할 수 있습니다.<br/><br/><a href="https://archive.codeplex.com/?p=mvvmlight">MVVM Light 도구 키트</a>와 같은 타사 MVVM 라이브러리를 사용 하거나 자체를 롤링 하 고 코드 숨김으로 논리를 유지 하 여 데이터 바인딩을 확장 하 여 <strong>MVVM (모델-뷰-ViewModel)</strong> 패턴을 따를 수 있습니다.<br/><br/><a href="https://docs.microsoft.com/previous-versions/msp-n-p/hh848246(v=pandp.10)">MVVM 패턴</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">템플릿 10 Visual Studio 프로젝트 템플릿</a></td>
</tr>
</tbody>
</table>