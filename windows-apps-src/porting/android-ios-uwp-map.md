---
author: GrantMeStrength
Description: Compare platform features between iOS, Android, and Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: Android 및 iOS 개발자용 Windows 앱 개념 매핑
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: f7e211ebfa28421340e716c0176cab80a9511671
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6833391"
---
#<a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Android 및 iOS 개발자용 Windows 앱 개념 매핑

Android 또는 iOS 기술이나 코드를 사용하고 Windows 10 및 UWP(유니버설 Windows 플랫폼)로 이동하려는 개발자의 경우 세 가지 플랫폼 간에 플랫폼 기능과 지식을 매핑하는 데 필요한 모든 정보가 들어 있는 이 리소스를 참조하세요.

[iOS에서 UWP로 이동](ios-to-uwp-root.md)의 포팅 콘텐츠도 참조하세요. 이 문서는 [다운로드](https://www.microsoft.com/download/details.aspx?id=52041)를 위해서도 사용할 수 있습니다.

## <a name="user-interface-ui"></a>UI(사용자 인터페이스)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>디자인 언어.</strong><br><br>플랫폼의 앱 모양과 동작을 규정하는 규칙 집합입니다.</td>
<td align="left"><strong>Android 자료 디자인</strong> 지침에서는 Android 디자이너 및 개발자를 위한 시각적 언어를 제공합니다.</td>
<td align="left"><strong>휴먼 인터페이스 지침</strong>에서는 iOS 디자이너 및 개발자를 위한 권장 사항을 제공합니다.</td>
<td align="left"><a href="https://dev.windows.com/design"><strong>UWP Windows 앱 디자인</strong></a>에서는 모든 Windows 10 장치에서 올바르게 표시되는 앱을 만드는 방법을 보여 줍니다. UI(사용자 인터페이스) 디자인 기본 사항, 반응형 디자인 기술 및 자세한 지침의 전체 목록을 찾을 수 있습니다.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>사용자 인터페이스 생성 언어.</strong> <br><br>UI와 해당 구성 요소를 렌더링 및 설명하는 생성 언어입니다. 각 플랫폼은 디자인 및 태그 편집을 위한 편집기를 제공합니다.<br/></td>
<td align="left"><strong>Android Studio</strong> 또는 <strong>Eclipse</strong>를 사용하여 편집된 <strong>XML 레이아웃</strong>.</td>
<td align="left">Xcode 내의 <strong>Interface Builder</strong>를 사용하여 편집된 <strong>XIB</strong> 및 <strong>스토리보드</strong></td>
<td align="left"><strong><a href="https://www.visualstudio.com/">Microsoft Visual Studio</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend for Visual Studio</a></strong>를 사용하여 편집된 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185595.aspx">XAML</a></strong><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228259.aspx">XAML 플랫폼</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228349.aspx">XAML을 사용하여 UI 만들기</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228350.aspx">XAML을 사용하여 레이아웃 정의</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>기본 제공 사용자 인터페이스 컨트롤.</strong> <br><br>단추, 목록 컨트롤, 텍스트 컨트롤 등 플랫폼에서 제공하는 재사용 가능 UI 요소입니다.</td>
<td align="left">클래스 위젯, 레이아웃, 텍스트 필드, 컨테이너, 날짜/시간 컨트롤 및 전문가 컨트롤이라고도 하는 미리 만들어진 <strong>보기</strong> 및 <strong>보기 그룹</strong>입니다.</td>
<td align="left">Xcode 개체 라이브러리에 있고 UIKit 사용자 인터페이스 카탈로그에 나열된 <strong>보기</strong> 및 <strong>컨트롤</strong>입니다. 보기에는 이미지 보기, 선택기 보기 및 스크롤 보기가 있습니다. 컨트롤에는 단추, 날짜 선택 및 텍스트 필드가 포함됩니다.</td>
<td align="left">XAML 플랫폼은 단추, 목록 컨트롤, 패널, 텍스트 컨트롤, 명령 모음, 선택기 및 수동 입력 등 일반적인 <strong>기본 제공 컨트롤</strong> 집합을 제공합니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">컨트롤 추가 및 이벤트 처리</a></td>
</tr>
<tr class="even">
<td align="left"><strong>이벤트 처리 제어.</strong> <br><br>이벤트는 UI 컨트롤 내에서 트리거될 때 실행되는 논리를 정의합니다.</td>
<td align="left"><strong>이벤트 처리기</strong> 및 <strong>이벤트 수신기</strong>는 XML 또는 프로그래밍 방식으로 추가됩니다.</td>
<td align="left">컨트롤은 <strong>작업</strong> 메시지를 <strong>대상</strong>으로 보냅니다.</td>
<td align="left">XAML 페이지에 연결된 <strong>코드 숨김 파일</strong>에서 XAML 컨트롤의 이벤트를 처리하는 메서드를 정의할 수 있습니다. <strong>이벤트 처리기</strong>는 항상 코드로 작성됩니다. 그러나 XAML 태그 또는 코드에서 이러한 처리기를 이벤트에 후크할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">컨트롤 추가 및 이벤트 처리</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185584.aspx">이벤트 및 라우트된 이벤트 개요</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>데이터 바인딩.</strong> <br><br>앱 UI에서 데이터를 렌더링하고 선택적으로 해당 데이터와의 동기화 상태를 유지할 수 있는 소프트웨어 디자인 패턴입니다.</td>
<td align="left">베타 버전이지만 <strong>데이터 바인딩 라이브러리</strong>를 제공합니다.</td>
<td align="left">iOS에는 기본 제공 바인딩 시스템이 없습니다. <strong>KVO(Key-Value Observing)</strong>를 빌드하여 타사 라이브러리를 사용하거나 추가 코드를 작성하여 데이터 바인딩을 수행할 수 있습니다. 컨트롤은 대리자/콜백 접근 방식을 사용하여 데이터를 가져옵니다.</td>
<td align="left">UWP 플랫폼에서 <strong>데이터 바인딩</strong>을 처리합니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204783.aspx">{x:Bind}</a></strong> 태그 확장을 사용하여 고성능 바인딩을 이용하거나 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204782.aspx">{Binding}</a></strong>을 사용하여 더 많은 기능을 활용합니다. 그런 다음 바인딩을 구성하여 플랫폼에서 <strong>단방향 바인딩</strong>을 통해 UI에 데이터 원본의 값을 표시할지, 아니면 <strong>양방향 바인딩</strong>으로 변경하여 해당 값을 관찰하고 UI를 업데이트할지 선택할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">데이터 바인딩</a></td>
</tr>
<tr class="even">
<td align="left"><strong>UI 자동화.</strong> <br><br>UI 요소에 대한 프로그래밍 방식의 액세스, 보조 기술 제품에 액세스할 수 있는 앱 만들기 및 UI를 조작하는 자동화된 테스트 스크립트 설정입니다.</td>
<td align="left"><strong>텍스트 레이블</strong>, <strong>contentDescription</strong> 및 <strong>힌트</strong> 값을 사용하면 자동화에서 UI 요소를 찾을 수 있습니다. Android Studio를 사용하면 <strong>UI Automator</strong> 및 <strong>Espresso</strong> 테스트 프레임워크를 사용하여 UI 테스트를 작성할 수 있습니다.</td>
<td align="left"><strong>자동화 방법</strong>을 사용하면 <strong>접근성</strong> 설정 또는 <strong>요소 계층 구조</strong>의 요소 위치를 사용하여 요소를 식별하는 자동화된 UI 테스트 스크립트를 작성할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/ee684076.aspx">UI 자동화</a></strong>를 사용하여 기본 UWP의 기본 제공 UI 요소에 프로그래밍 방식으로 액세스합니다.<br/><strong><a href="https://msdn.microsoft.com/library/windows/apps/mt297667.aspx">사용자 지정 자동화 피어</a></strong>를 사용하면 고유한 사용자 지정 UI 클래스에 대한 자동화 지원을 제공할 수 있습니다. Visual Studio의 <strong><a href="https://msdn.microsoft.com/library/dd286726.aspx#VerifyingCodeUsingCUITCreate">코딩된 UI 테스트 프로젝트</a></strong>를 사용하면 UI를 통해 전체 응용 프로그램을 자동으로 테스트하거나 격리된 상태에서 UI를 테스트할 수 있습니다.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>컨트롤의 모양 변경</strong> <br><br>크기, 색 및 기타 특성을 편집합니다.</td>
<td align="left">컨트롤에는 디자이너 도구를 사용하여 XML 태그 또는 프로그래밍 방식으로 편집할 수 있는 <strong>속성</strong>이 있습니다.</td>
<td align="left">컨트롤에는 <strong>특성 검사기</strong>를 사용하여 인터페이스 작성기 또는 프로그래밍 방식으로 편집할 수 있는 <strong>특성</strong>이 있습니다.</td>
<td align="left">Visual Studio 및 Blend for Visual Studio를 사용하여 XAML 태그 또는 프로그래밍 방식으로 컨트롤의 <strong>속성</strong>을 편집할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">컨트롤 추가 및 이벤트 처리</a></td>
</tr>
<tr class="even">
<td align="left"><strong>재사용 가능한 시각적 스타일.</strong> <br><br>재사용 가능한 형식으로 여러 컨트롤에 시각적 변경 사항을 적용합니다.</td>
<td align="left"><strong>XML 스타일</strong>은 하나 이상의 컨트롤에 적용되는 속성 집합입니다.</td>
<td align="left">iOS는 기본으로 제공하는 재사용 가능한 시각적 스타일을 지원하지 않지만 UIAppearance 프로토콜을 사용하면 여러 컨트롤에서 공통 특성을 공유할 수 있습니다.</td>
<td align="left">쉽게 재사용할 수 있도록 여러 컨트롤에 적용할 수 있는 재사용 가능한 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx">스타일</a></strong>을 만들어 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx">ResourceDictionary</a></strong>에 저장할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465381.aspx">빠른 시작: 컨트롤 스타일 지정</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>컨트롤의 시각적 구조 편집.</strong> <br><br>속성 또는 특성을 수정하는 것 외에 확인란 아래의 확인란 텍스트를 이동하는 등 컨트롤의 시각적 구조를 사용자 지정합니다.</td>
<td align="left">Android에서 컨트롤의 시각적 구조를 간단히 편집할 수 있는 방법은 없습니다.</td>
<td align="left">iOS에서 컨트롤의 시각적 구조를 간단히 편집할 수 있는 방법은 없습니다.</td>
<td align="left">컨트롤의 시각적 구조를 사용자 지정하려면 XAML 태그에서 해당 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx">컨트롤 템플릿</a></strong>을 복사 및 편집할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465374.aspx">빠른 시작: 컨트롤 템플릿</a></td>
</tr>
<tr class="even">
<td align="left"><strong>기본 제공 터치 제스처.</strong> <br><br>보기 및 컨트롤에서 탭하기 및 두 번 탭하기 등 수준 높은 추상화 제스처 이벤트를 처리하여 사용자 지정된 터치 지원을 제공합니다.</td>
<td align="left"><strong>제스처 탐지기</strong>는 스크롤, 길게 누르기, 탭하기, 두 번 탭하기 및 플링 등 일반적인 터치 제스처를 감지합니다.</td>
<td align="left">UIKit 프레임워크는 탭하기, 손가락 모으기, 이동, 살짝 밀기, 회전 및 길게 누르기 등 기본 제공 <strong>제스처 인식기</strong>를 제공합니다.</td>
<td align="left"><strong>UI 요소</strong>를 사용하면 탭하기, 두 번 탭하기, 오른쪽 탭하기 및 누르고 있기 등의 <strong>정적 제스처 이벤트</strong>뿐 아니라 밀기, 살짝 밀기, 돌리기, 손가락 모으기 및 늘이기 등의 <strong>조작 제스처 이벤트</strong>도 처리할 수 있습니다. 제스처 이벤트는 <strong>라우트된 이벤트</strong>이며 자식 UIElement를 포함하는 부모 개체에서 처리될 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">터치 조작</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx#gestures__manipulations__and_interactions">사용자 지정 사용자 조작 - 제스처, 조작 및 조작 방식</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">탐색 및 앱 구조</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>레이아웃.</strong> <br><br>레이아웃은 사용자 인터페이스의 구조를 정의합니다.</td>
<td align="left">레이아웃은 다른 보기 그룹 또는 보기를 중첩할 수 있는 <strong>LinearLayout</strong> 및 <strong>RelativeLayout</strong> 등의 <strong>보기 그룹</strong>으로 구성됩니다.</td>
<td align="left">레이아웃은 중첩 가능한 <strong>UIView</strong>를 포함하는 <strong>UIViewController</strong>로 구성됩니다.</td>
<td align="left">정적 및 반응형 레이아웃을 위한 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.canvas.aspx">Canvas</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx">Grid</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.relativepanel.aspx">RelativePanel</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx">StackPanel</a></strong> 등의 <strong>레이아웃 패널 클래스</strong>로 구성된 유연한 레이아웃 시스템을 제공하는 XAML입니다. <strong><a href="https://msdn.microsoft.com/library/ms171352.aspx">속성</a></strong>은 요소의 위치와 크기를 제어하는 데 사용됩니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx">XAML을 사용하여 레이아웃 정의</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>피어 탐색.</strong> <br><br>사용자에게 계층적 중요도가 동일한 페이지 간을 이동하는 방법을 제공합니다.</td>
<td align="left"><strong>탭</strong>, <strong>살짝 밀기 보기</strong> 및 <strong>탐색 창</strong>은 <strong>측면 탐색</strong>을 제공합니다.</td>
<td align="left"><strong>탭 바 컨트롤러</strong>, <strong>분할 보기 컨트롤러</strong> 및 <strong>페이지 보기 컨트롤러</strong>를 사용하면 동일한 계층의 보기 간을 탐색할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997788.aspx">탭/피벗</a></strong>을 사용하여 콘텐츠 위에 링크/탭의 영구 목록을 표시할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997787.aspx">탐색 창/분할 보기</a></strong>를 사용하면 콘텐츠와 함께 링크 목록을 표시할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">탐색</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">두 페이지 간 이동</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>계층적 탐색.</strong> <br><br>계층에서 부모와 자식 페이지 간을 탐색합니다.</td>
<td align="left"><strong>목록</strong>, <strong>눈금 목록</strong>, <strong>단추</strong> 및 기타 컨트롤은 <strong>의도</strong>와 함께 사용하여 다른 <strong>활동</strong>을 로드할 때 <strong>하위 탐색</strong>을 제공합니다.</td>
<td align="left"><strong>탐색 컨트롤러</strong>를 사용하면 사용자가 계층 구조 수준 간을 탐색할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/dn449149.aspx">허브</a></strong>를 사용하면 하위 페이지로 이동하는 데 선택할 수 있도록 사용자에게 콘텐츠의 미리 보기를 표시합니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997765.aspx">마스터/세부 정보</a></strong>를 사용하면 사용자가 해당 세부 정보 섹션 옆에 표시되는 항목 요약 목록에서 선택할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">탐색</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">두 페이지 간 이동</a></td>
</tr>
<tr class="even">
<td align="left"><strong>뒤로 단추 탐색.</strong> <br><br>응용 프로그램을 통해 뒤로 탐색합니다.</td>
<td align="left">작업 모음 내의 <strong>뒤로</strong> 및 <strong>위로</strong> 단추는 <strong>백 스택</strong>을 사용하여 <strong>상위</strong> 및 <strong>임시</strong> 탐색을 제공합니다.</td>
<td align="left"><strong>탐색 컨트롤러</strong>에 뒤로 단추를 추가할 수 있습니다.<br/></td>
<td align="left">사용자가 <strong>탐색 기록</strong>을 트래버스할 수 있는 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.backstack.aspx">백 스택 속성</a></strong>을 사용하여 소프트웨어 또는 하드웨어 뒤로 단추 누르기를 쉽게 처리할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt465734.aspx">뒤로 단추 탐색</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>시작 화면.</strong> <br><br>앱 실행 시 이미지를 표시하며 브랜딩에 주로 사용됩니다.</td>
<td align="left">시작 화면은 기본적으로 제공되지 않으며 첫 번째 활동 <strong>테마 배경</strong>을 편집하여 구현됩니다.</td>
<td align="left">앱에는 <strong>정적 시작 이미지</strong> 또는 <strong>XIB/스토리보드 시작 파일</strong>이 있어야 합니다.</td>
<td align="left"><strong>이미지</strong> 및 색상이 있는 배경을 사용하여 시작 화면을 만듭니다. <a href="https://msdn.microsoft.com/library/windows/apps/mt187309.aspx">시작 화면 시간을 확장할 수 있습니다</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187306.aspx">시작 화면 추가</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>음성.</strong> <br><br>음성 인식 및 추가 음성 기능을 위해 음성을 인식합니다.</td>
<td align="left">음성 입력은 <strong>Google 음성 검색</strong> 등의 <strong>RecognizerIntent</strong>를 구현하는 앱에서 제공할 수 있습니다. <strong>SpeechRecognizer</strong> 클래스를 사용하면 앱에서 Google의 음성 인식 API를 사용할 수 있습니다.</td>
<td align="left">앱은 <strong>SFSpeechRecognizer</strong> 클래스를 사용, 음성 입력 및 음성 인식 기능을 구현합니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185615.aspx">음성 인식</a></strong> API를 사용하여 전경에서 앱을 조작할 수 있습니다. 음성 기반 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185598.aspx">Cortana 조작</a></strong>을 사용하여 전경 또는 백그라운드에서 앱을 시작하고 백그라운드 앱을 조작할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185614.aspx">음성 조작</a></td>
</tr>
<tr class="even">
<td align="left"><strong>사용자 지정 사용자 입력.</strong> <br><br>키보드, 마우스, 스타일러스 및 기타 입력을 처리합니다.</td>
<td align="left"><strong>터치</strong>, <strong>터치 패드</strong>, <strong>스타일러스</strong>, <strong>마우스</strong> 및 <strong>키보드</strong> 조작이 지원됩니다. 동작 및 입력은 터치와 동일한 방식으로 보고되지만 <strong>입력 디바이스</strong>에 대한 더 많은 정보를 검색할 수 있습니다.</td>
<td align="left"><strong>터치</strong>, <strong>Apple 연필</strong> 및 하드웨어 <strong>키보드</strong>에 대한 지원이 제공됩니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">터치</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187313.aspx">터치 패드</a></strong>, 디지털 잉크를 사용하는 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187311.aspx">펜/스타일러스</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187308.aspx">마우스</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185607.aspx">키보드</a></strong>를 포함하여 다양한 조작이 지원됩니다. 사용된 입력 디바이스와 필요한 경우 원시 입력 디바이스 데이터에 액세스할 수 있는 여부를 몰라도 앱에서 데이터를 처리할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404610.aspx">포인터 입력 처리</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx">사용자 지정 사용자 조작</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>로컬 앱 데이터.</strong> <br><br>앱 관련 설정 및 파일을 로컬로 앱에 저장합니다.</td>
<td align="left">로컬 파일은 <strong>openFileOutput</strong> 및 <strong>openFileInput</strong>을 사용하여 저장할 수 있습니다. <strong>공유 기본 설정 파일</strong>의 설정은 <strong>getSharedPreferences</strong>를 사용하여 액세스할 수 있습니다.</td>
<td align="left">로컬 파일은 <strong>NSFileManager</strong> 클래스를 통해 액세스되는 <strong>응용 프로그램 지원</strong> 디렉터리에 저장할 수 있습니다. <strong>기본 설정</strong> 파일의 설정은 <strong>NSUserDefaults</strong> 클래스에서 액세스할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/br230562.aspx">Windows.Storage</a></strong> 클래스는 통합된 방식으로 로컬 데이터 저장소를 처리합니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localsettings.aspx">ApplicationData.LocalSettings</a></strong> 속성을 통해 액세스하는 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdatacontainer.aspx">ApplicationDataContainer</a></strong> 개체로 설정을 저장합니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localfolder.aspx">ApplicationData.LocalFolder</a></strong> 속성을 통해 액세스하는 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefolder.aspx">StorageFolder</a></strong> 개체에 파일을 저장합니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt299098.aspx">설정 및 기타 앱 데이터 저장 및 검색</a></td>
</tr>
<tr class="even">
<td align="left"><strong>로컬 데이터베이스 저장소.</strong> <br><br>해당하는 경우 ORM(개체 관계형 매퍼)을 사용하여 관계형 데이터베이스에 앱 데이터를 저장합니다.</td>
<td align="left"><strong>SQLite</strong> 데이터베이스가 제공됩니다. 기본 제공 ORM은 없습니다. SQL 쿼리는 <strong>SQLiteDatabase</strong> 클래스를 사용하여 실행됩니다.</td>
<td align="left"><strong>SQLite</strong> 데이터베이스가 제공됩니다. <strong>CoreData</strong>는 SQLite와 함께 사용할 수 있는 기본 제공 개체 그래프 프레임워크이며 ORM과 비슷한 기능을 제공합니다.</td>
<td align="left"><strong>SQLite</strong>를 사용하여 데이터를 저장할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592863.aspx">Entity Framework</a></strong>는 많은 데이터 액세스 코드를 작성할 필요가 없으며 SQL 작성 없이도 데이터베이스를 쉽게 쿼리할 수 있는 기본 제공 ORM입니다. <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592864.aspx">SQLite 라이브러리</a>를 사용하여 직접 SQL 쿼리를 직접 실행할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592862.aspx">데이터 액세스</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>REST 액세스용 HTTP 라이브러리.</strong> <br><br>HTTP(S)를 사용하여 웹 서비스 및 웹 서버와 통신할 수 있는 기본 제공 라이브러리입니다.<br/></td>
<td align="left">HTTP 라이브러리 <strong>HttpURLConnection</strong> 및 <strong>Volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> 및 <strong>NSURLDownload</strong>.</td>
<td align="left">기본 제공 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpclient">HttpClient</a></strong> API를 사용하여 GET, DELETE, PUT, POST, 일반 인증 패턴, SSL, 쿠키, 진행 정보를 포함한 일반 HTTP 기능에 액세스할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left"><strong>클라우드 백업 서비스.</strong> <br><br>앱 데이터를 위해 플랫폼에서 제공한 백업 서비스입니다.</td>
<td align="left">Android의 <strong>백업 관리자</strong>는 Google의 <strong>Android 백업 서비스</strong>에서 응용 프로그램 데이터 백업을 처리합니다.</td>
<td align="left">사용자가 앱 데이터를 포함하여 해당 백업을 처리하도록 <strong>iCloud 백업</strong>을 구성할 수 있습니다. iCloud 호환 <strong>핵심 데이터</strong>, <strong>iCloud 키-값 저장소</strong> 및 <strong>iCloud 문서 저장소</strong>를 사용하는 앱입니다.</td>
<td align="left">로밍 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.aspx">ApplicationData API</a></strong>(<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingfolder.aspx">RoamingFolder</a></strong> 및 <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingsettings.aspx"><strong>RoamingSettings</strong></a> 포함)를 사용하여 저장한 모든 앱 데이터는 자동으로 클라우드 및 사용자의 기타 디바이스와 동기화됩니다. 동기화는 사용자의 Microsoft 계정을 통해 수행됩니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/hh465094.aspx">로밍 중인 앱 데이터 지침</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP 파일 다운로드.</strong> <br><br>HTTP를 통해 크고 작은 파일을 다운로드합니다.</td>
<td align="left"><strong>URLConnection</strong> 및 <strong>HTTPURLConnection</strong>은 HTTP 및 FTP를 통해 다운로드하는 데 사용되며 시스템 <strong>다운로드 관리자</strong>를 사용하여 백그라운드에서 다운로드할 수도 있습니다.</td>
<td align="left"><strong>NSURLSession</strong> 및 <strong>NSURLConnection</strong>은 HTTP 및 FTP를 통해 파일을 다운로드하는 데 사용할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.backgroundtransfer.aspx">백그라운드 전송 API</a></strong>를 사용하면 HTTP(S) 및 FTP를 통해 파일을 안정적으로 전송하고, 앱 일시 중단, 연결 해제 및 연결성과 배터리 사용 시간에 따른 조정을 고려합니다. 적은 파일에 적합한 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.httpclient.aspx">HttpClient</a></strong>를 사용할 수도 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">네트워킹 기술 선택</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280377.aspx">백그라운드 전송</a></td>
</tr>
<tr class="even">
<td align="left"><strong>소켓.</strong> <br><br>낮은 수준의 UDP 데이터그램 및 TCP 소켓을 만들어 사용자 고유의 프로토콜을 통해 다른 장치와 통신합니다.</td>
<td align="left"><strong>Socket</strong> 클래스는 TCP 소켓을 제공하고 <strong>DatagramSocket</strong> 클래스는 UDP 소켓을 제공합니다.</td>
<td align="left"><strong>NSStream</strong> 및 <strong>CFStream</strong>은 TCP 소켓을 제공하고 <strong>CFSocket</strong>은 UDP 소켓을 제공합니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/br241319">DatagramSocket</a></strong> 클래스를 사용하여 UDP 데이터그램 소켓을 통해 통신하고 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br226882">StreamSocket</a></strong> 클래스를 사용하여 TCP 또는 Bluetooth RFCOMM을 통해 통신할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">네트워킹 기본 사항</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">네트워킹 기술 선택</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280234.aspx">소켓 개요</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>실시간 데이터 전송을 위해 클라이언트와 서버 간에 양방향 통신을 제공합니다.</td>
<td align="left">Android에는 기본 제공 WebSockets 라이브러리가 없습니다.</td>
<td align="left">iOS에는 기본 제공 WebSockets 라이브러리가 없습니다.</td>
<td align="left">수신 알림 기능이 있는 적은 메시지에 대해 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.messagewebsocket.aspx">MessageWebSocket</a></strong> 클래스를 사용하고, 섹션에서 읽을 수 있는 대용량 이진 파일 전송에 대해 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamwebsocket.aspx">StreamWebSocket</a></strong>을 사용하여 WebSockets를 지원하는 서버에 대한 연결을 보호합니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">네트워킹 기본 사항</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">네트워킹 기술 선택</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt186447.aspx">WebSockets 개요</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth 라이브러리.</strong> <br><br>타사 OAuth 공급자에 대한 액세스를 허용하는 OAuth 라이브러리와 플랫폼에 기본 제공되는 계정 관리입니다.</td>
<td align="left">일반 OAuth 라이브러리는 제공되지 않습니다. Google Play Services를 사용한 OAuth 인증을 위해 <strong>GoogleAuthUtil</strong> 클래스가 제공됩니다.<br/></td>
<td align="left">일반 OAuth 라이브러리는 제공되지 않습니다. <strong>계정 프레임워크</strong>는 Facebook 및 Twitter와 같이 디바이스에 이미 저장된 사용자 계정에 대한 액세스를 제공합니다.</td>
<td align="left">일반 OAuth 라이브러리 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270196.aspx">웹 인증 브로커</a></strong>를 사용하여 타사 ID 공급자 서비스에 연결할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270189.aspx">자격 증명 보관</a></strong>을 사용하여 사용자가 로그인을 저장하고 여러 디바이스에서 사용하도록 할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn896755.aspx">Microsoft.Live</a></strong> 네임스페이스를 사용하면 Microsoft 서비스 액세스를 위해 Live SDK OAuth에 쉽게 액세스할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt270184.aspx">인증 및 사용자 ID</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.aspx">Windows.Security.Authentication.Web API 설명서</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker 코드 예제</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>앱을 만드는 데 사용되는 도구 집합입니다.</td>
<td align="left"><strong>Android Studio</strong> 및 <strong>Eclipse</strong>, Google에서는 개발자에게 Android Studio를 사용하도록 권장합니다.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://www.visualstudio.com/features/universal-windows-platform-vs.aspx">Visual Studio</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend for Visual Studio</a></strong>에는 UWP 앱을 코딩, 디자인, 연결, 디버그, 분석, 최적화 및 테스트하는 데 필요한 모든 도구가 있습니다. Visual Studio에서는 Windows 10 디바이스용 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt188754.aspx">에뮬레이터</a></strong>를 제공하므로 다양한 에뮬레이트 디바이스에서 앱을 테스트할 수 있습니다.<br/><br/><a href="https://dev.windows.com/downloads">UWP용 다운로드 및 도구</a></td>
</tr>
<tr class="even">
<td align="left"><strong>코드 구성.</strong> <br><br>앱의 기본 폴더 구조로, 초기 템플릿에서 생성됩니다.</td>
<td align="left"><strong>AndroidManifest</strong> 파일, 소스 파일이 포함된 <strong>java</strong> 폴더, 레이아웃 및 값을 포함한 리소스가 있는 <strong>res</strong> 폴더, Android Studio의 <strong>Gradle</strong> 빌드 스크립트 및 Eclipse의 <strong>Ant</strong> 빌드 스크립트</td>
<td align="left">소스 파일 및 <strong>지원 파일</strong>, <strong>Info.plist</strong> 파일, <strong>Main.storyboard</strong> 및 <strong>LaunchScreen.storyboard</strong>. 이미지는 <strong>자산 라이브러리</strong>에 저장됩니다.</td>
<td align="left">UWP 앱에는 Example.xaml 및 Example.xaml.cs라는 앱에 대한 XAML 및 코드 파일, <strong>Assets 폴더</strong>의 다양한 이미지, <strong>MainPage.xaml</strong> 및 <strong>MainPage.xaml.cs</strong> 등의 시작 페이지 및 매니페스트가 포함됩니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn765018.aspx">hello world 앱 만들기</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>앱 수명 주기.</strong> <br><br>앱 시작, 일시 중단, 다시 시작 및 닫기 시 이벤트 처리는 응용 프로그램 상태를 저장/복원하고 다른 작업을 실행할 수 있는 기회를 제공합니다.</td>
<td align="left">각 작업에는 자체 <strong>작업 수명 주기</strong>와 <strong>다시 시작</strong> 등의 상태가 있습니다. <strong>OnResume</strong> 등의 <strong>수명 주기 콜백</strong> <strong>활동 클래스</strong>에서 구현 됩니다.</td>
<td align="left"><strong>응용 프로그램 수명 주기</strong>에는 <strong>일시 중단</strong> 등의 상태가 있습니다. <strong>applicationDidEnterBackground:</strong>와 같은 메서드가 <strong>응용 프로그램 대리자 개체</strong>에서 구현되어 상태 변경 시 코드를 실행합니다.</td>
<td align="left">응용 프로그램의 <strong>앱 실행 상태</strong>는 NotRunning, Activated, Running, Suspending, Suspended 및 Resuming입니다.<br/><br/>상태가 변경될 때 앱에서 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.aspx">응용 프로그램 클래스</a></strong> 메서드 OnLaunched, OnActivated, Suspending 또는 Resuming을 구현하여 코드를 실행할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt243287.aspx">앱 수명 주기</a></td>
</tr>
<tr class="even">
<td align="left"><strong>백그라운드 작업.</strong> <br><br>앱이 전경에 없을 때 백그라운드 작업을 수행하고 실행을 계속하는 작업입니다.</td>
<td align="left">앱이 전경에 없을 때 앱에서 백그라운드 작업을 수행하는 <strong>서비스</strong>를 시작할 수 있습니다. 서비스에는 자체 <strong>수명 주기</strong>가 있으며 매니페스트에 등록됩니다.</td>
<td align="left"><strong>백그라운드 실행</strong>은 특정 작업 유형에만 사용할 수 있습니다.<br/><br/><strong>UIBackgroundModes</strong>를 사용하여 앱에서 Info.plist 파일에 <strong>지원되는 백그라운드 작업</strong>을 선언합니다.<br/><br/>시스템은 백그라운드 작업을 실행할 시기와 기간을 제어합니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx">IBackgroundTask</a></strong> 인터페이스를 구현하고 응용 프로그램 매니페스트에 작업을 등록하여 백그라운드 작업을 만들 수 있습니다. <a href="https://msdn.microsoft.com/library/windows/apps/mt186458.aspx"><strong>타이머</strong></a>, <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtriggertype.aspx"><strong>시스템 트리거</strong></a>, 및 <a href="https://msdn.microsoft.com/library/windows/apps/mt185632.aspx"><strong>유지 관리 트리거</strong></a>를 사용하여 트리거할 작업을 설정할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299103.aspx">백그라운드 작업을 사용하여 앱 지원</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299100.aspx">백그라운드 작업 만들기 및 등록</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187310.aspx">백그라운드 작업 지침</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>성능 모범 사례.</strong> <br><br>시작 시간을 단축하면서 빠르고, 반응성이 뛰어나고, 배터리 사용 시간을 고려하는 앱을 작성하기 위한 지침입니다.</td>
<td align="left">Android는 <strong>성능 모범 사례</strong> 교육 가이드를 제공합니다.</td>
<td align="left">iOS는 <strong>성능 개요</strong> 문서를 제공합니다.</td>
<td align="left">성능 목표 설정, 성능 측정, 메모리 관리, 매끄러운 애니메이션, 효율적인 파일 시스템 액세스, 프로파일링 및 성능에 사용할 수 있는 도구 등에 대한 항목을 다루는 섹션이 포함된 자세한 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270266.aspx">성능 가이드</a></strong>를 참조할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left"><strong>반응형 UI에 대한 최적화 보기.</strong> <br><br>보기를 최적화하여 성능을 향상시킵니다.</td>
<td align="left">계층적 뷰어 도구를 사용한 <strong>레이아웃 계층</strong> 최적화, <strong>레이아웃 다시 사용</strong> 및 <strong>주문형 보기</strong> 로드는 UI 스레드 응답을 유지하고 <strong>ANR</strong>(&quot;응용 프로그램이 응답하지 않음&quot;) 대화 상자를 방지하는 기술입니다.<br/></td>
<td align="left"><strong>핵심 애니메이션</strong> 도구를 사용하여 <strong>오프스크린 렌더링</strong>, <strong>혼합 계층</strong>, <strong>래스터화</strong>를 통해 UI 문제를 해결하면 UI 스레드 응답을 유지할 수 있습니다.</td>
<td align="left">몇 가지 간단한 단계에 따라 XAML <strong>태그</strong> 및 <strong>레이아웃</strong>을 쉽게 <strong>최적화</strong>할 수 있습니다. 기술에는 레이아웃 구조 줄이기, 요소 수 최소화 및 과도한 그리기 최소화가 포함됩니다. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185403.aspx">UI 스레드 응답 유지</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204779.aspx">XAML 태그 최적화</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404609.aspx">XAML 레이아웃 최적화</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>스레딩.</strong> <br><br>스레딩을 사용하여 <strong>반응형 UI</strong>를 유지하고 여러 <strong>작업을 병렬로</strong> 실행합니다.</td>
<td align="left">스레딩은 <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong> 및 높은 수준 <strong>AsyncTask</strong> 클래스를 통해 사용할 수 있습니다.</td>
<td align="left">스레딩은 <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong> 및 높은 수준 <strong>NSOperation</strong>을 통해 사용할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpool.runasync.aspx">RunAsync</a></strong>를 사용하여 <strong>작업 항목</strong>을 <strong>threadpool</strong>에 제출하여 스레드 작업을 수행할 수 있습니다. 타이머를 사용하여 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230590.aspx">CreateTimer</a></strong>로 작업 항목을 제출하고 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230589.aspx">CreatePeriodicTimer</a></strong>로 반복 작업 항목을 만들 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187339.aspx">스레드 풀에 작업 항목 제출</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187341.aspx">타이머를 사용하여 작업 항목 제출</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187338.aspx">정기 작업 항목 만들기</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187336.aspx">스레드 풀을 사용하기 위한 모범 사례</a></td>
</tr>
<tr class="even">
<td align="left"><strong>비동기 프로그래밍.</strong> <br><br>비동기 프로그래밍 패턴을 통해 UI 스레드 응답을 유지하여 스레딩 복잡성을 방지합니다.</td>
<td align="left">자체 비동기 클래스를 만들려면 <strong>스레딩 필요</strong>를 사용합니다. 일부 기본 제공 클래스는 비동기입니다.</td>
<td align="left">자체 비동기 클래스를 만들려면 <strong>스레딩 필요</strong>를 사용합니다. 일부 기본 제공 클래스는 비동기입니다.</td>
<td align="left">자체 API를 만들 때 비동기 패턴(예: C# 및 Visual Basic의 <strong>async</strong> 및 <strong>await</strong>)을 사용하여 주 스레드 차단을 방지할 수 있습니다. 단어가 <strong>Async</strong>로 끝나는 비동기 기본 제공 API를 사용할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187335.aspx">비동기 프로그래밍</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187337.aspx">C# 또는 Visual Basic에서 비동기식 API 호출</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>목록 보기 최적화.</strong> <br><br>데이터 목록 최적화를 지원하는 기본 제공 패턴이며 많은 양의 데이터를 표시해야 할 경우 성능이 저하될 수 있습니다.</td>
<td align="left"><strong>ViewHolder</strong> 디자인 패턴은 여러 보기 조회를 방지하는 데 사용됩니다. 이를 통해 재사용 가능한 UI 요소를 사용할 수 있습니다.</td>
<td align="left">다양한 최적화를 통해 <strong>UITableView</strong>의 성능을 향상시킬 수 있습니다. 기본 제공되는 항목은 없습니다.</td>
<td align="left">기본 <strong>UI 가상화</strong>를 제공하는 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx">ListView</a> 및 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx">GridView</a> 컨트롤을 사용하여 이동 및 스크롤 환경을 제공하고 시작 시간을 단축할 수 있습니다. 데이터 원본에서 <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.ilist.aspx">IList</a> 및 <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged.aspx">INotifyCollectionChanged</a>를 구현하여 <strong>데이터 가상화</strong> 및 개선된 성능을 제공할 수도 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204776.aspx">ListView 및 GridView UI 최적화</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt574120.aspx">ListView 및 GridView 데이터 가상화</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>앱에서 바로 구매.</strong> <br><br>사용자가 앱에서 구매할 수 있는 플랫폼 기능입니다.</td>
<td align="left"><strong>앱에서 바로 청구</strong>는 Google 서비스에서 제공됩니다. 제품이 <strong>Google Play 개발자 콘솔</strong>에 추가됩니다. 앱에서 바로 구매는 <strong>Google Play Billing Library</strong>를 통해 구현됩니다.</td>
<td align="left">제품이 <strong>iTunes Connect</strong>에 추가됩니다. 앱에서 바로 구매는 <strong>StoreKit</strong> 프레임워크를 사용하여 구현됩니다.<br/><br/>제품은 <strong>SKMutablePayment</strong> 및 <strong>SKPaymentQueue</strong>를 사용하여 구매합니다.</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/mt148551.aspx">앱에 추가하고 Microsoft Store에 제출</a>하여 앱에서 바로 구매 제품 구매를 만듭니다. <br/><br/><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx">CurrentApp 클래스</a></strong>를 사용하여 앱에서 바로 구매를 정의합니다. <br/><br/><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.requestproductpurchaseasync.aspx">CurrentApp.RequestProductPurchaseAsync</a></strong>를 사용하여 고객이 제품을 구매할 수 있는 UI를 표시합니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219684.aspx">앱에서 바로 구매 제품 구매 사용</a></td>
</tr>
<tr class="even">
<td align="left"><strong>앱에서 바로 소모성 제품 구매.</strong> <br><br>앱에서 바로 구매 제품은 구매하고 사용한 다음 다시 구매할 수 있습니다.</td>
<td align="left">소모성 구매는 일반 구매를 만든 다음 <strong>consumePurchase</strong>를 통해 이를 소모할 수 있으며 구매, 사용, 다시 구매할 수 있습니다.</td>
<td align="left">소모성 제품은 <strong>iTunes Connect에서 소모성 제품으로 정의</strong>됩니다.</td>
<td align="left">Microsoft Store에 <a href="https://msdn.microsoft.com/library/windows/apps/mt148534.aspx">제출할 때 제품 형식을 소모성으로 정의</a>하여 소모성 제품을 지원할 수 있습니다. 그런 다음 고객이 소모성 제품에 액세스할 수 있게 되면<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync.aspx"> CurrentApp.ReportConsumableFulfillmentAsync</a></strong>를 호출할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219683.aspx">앱에서 바로 소모성 제품 구매 사용</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>앱에서 바로 구매 테스트.</strong> <br><br>Microsoft Store에 앱을 추가하지 않고 앱에서 바로 구매 코드를 테스트할 수 있습니다.</td>
<td align="left"><strong>앱에서 바로 청구 샌드박스</strong>가 테스트에 사용됩니다.</td>
<td align="left"><strong>샌드박스 테스터 계정</strong>이 테스트에 사용됩니다.</td>
<td align="left">CurrentApp 대신 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx">CurrentAppSimulator</a></strong> 클래스를 사용하여 간단히 앱에서 바로 구매를 테스트할 수 있습니다.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>무료 평가판.</strong> <br><br>앱의 평가판을 기준으로 쉽게 콘텐츠를 제한하거나 광고를 제거할 수 있습니다.</td>
<td align="left">Google Play는 <strong>앱 평가판을 공식적으로 지원하지 않습니다</strong>. 평가판 또는 광고 제거 버전을 얻으려면 앱에서 바로 구매를 만들고 구매를 완료했을 때 받은 적절한 코드 경로를 사용해야 합니다.</td>
<td align="left">앱 Microsoft Store는 <strong>앱 평가판을 공식적으로 지원하지 않습니다</strong>. 평가판 또는 광고 제거 버전을 얻으려면 앱에서 바로 구매를 만들고 구매를 완료했을 때 받은 적절한 코드 경로를 사용해야 합니다.</td>
<td align="left">Microsoft Store에 앱을 제출할 때 <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt148548.aspx">'무료 평가판' 옵션</a></strong>을 사용하여 앱의 무료 평가판 버전을 제공할 수 있습니다. 그런 다음 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.istrial.aspx">LicenseInformation.IsTrial</a></strong>을 사용하여 앱의 평가판 상태를 확인하고 그에 따라 다른 코드 경로를 제공합니다. 앱 실행 중 사용자가 평가판 상태를 변경할 때 알릴 <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged</a> 이벤트를 등록할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219685.aspx">평가판의 기능 제외 또는 제한</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">여러 플랫폼에 맞게 조정</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>적응 UI: 유연한 레이아웃.</strong> <br><br>유연한 높이 및 너비를 사용하여 다양한 화면 크기를 지원합니다.</td>
<td align="left">유연한 레이아웃을 사용하려면 LinearLayout 개체에서 <strong>wrap_conten</strong>t 및 <strong>match_parent</strong> 값을 사용하거나 맞춤을 위해 RelativeLayout을 사용합니다.</td>
<td align="left">유연한 레이아웃을 사용하려면 유니버설 스토리보드와 함께 <strong>적응 모델</strong>을 사용하여 컨트롤러를 볼 때 적용되는 horizontalSizeClass 및 displayScale 등의 <strong>제약 조건</strong> 및 <strong>특성</strong>과 함께 <strong>자동 레이아웃</strong>을 사용합니다.</td>
<td align="left">고정 및 동적 크기 조정의 조합과 함께 <strong>레이아웃 속성</strong> 및 <strong>패널</strong>을 사용하여 유동 레이아웃을 만들 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#layout_overview">XAML을 사용하여 레이아웃 정의 - 레이아웃 속성 및 패널</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">반응형 디자인 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>적응 UI: 맞춤형 레이아웃.</strong> <br><br>개별 대상의 레이아웃을 사용하여 다양한 화면 크기를 지원합니다.</td>
<td align="left"><strong>small</strong>, <strong>large</strong>, <strong>ldpi</strong> 및 <strong>hdpi</strong>와 같은 <strong>구성 한정자</strong>를 사용하여 리소스 디렉터리의 다양한 화면 구성에 대해 대체 레이아웃 파일을 제공합니다.</td>
<td align="left"><strong>개별 iPhone 및 iPad 스토리보드</strong>를 정의하여 유니버설 앱의 다른 장치 패밀리에 맞게 레이아웃을 조정합니다.</td>
<td align="left">장치 패밀리마다 <strong>다른 XAML 태그 파일</strong>을 정의하여 맞춤형 레이아웃을 빌드할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#tailored_layouts">XAML을 사용하여 레이아웃 정의 - 맞춤형 레이아웃</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>적응 UI: 반응형 레이아웃.</strong> <br><br>회전 등의 화면 크기 변경 또는 창 크기 변경에 응답합니다.</td>
<td align="left"><strong>LinearLayout</strong> 및 <strong>RelativeLayout</strong>을 통해 유연한 레이아웃을 사용하거나 다양한 방향을 위한 대체 레이아웃 파일을 제공하여 반응형 레이아웃을 설정합니다.</td>
<td align="left">뷰의 <strong>크기</strong> 또는 <strong>특성</strong>을 변경할 때 스토리보드에 지정된 <strong>제약 조건</strong>이 적용됩니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.aspx">VisualState</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager.aspx">VisualStateManager</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.aspx">AdaptiveTrigger</a></strong>를 사용한 창 크기 변경에 대한 응답으로 런타임 시 UI에 대해 쉽게 재배치, 위치 변경, 크기 변경, 노출 또는 섹션 바꾸기 등을 수행할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#visual_states_and_state_triggers">XAML을 사용하여 레이아웃 정의 - 시각적 상태 및 상태 트리거</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">반응형 디자인 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>다른 디바이스 기능 지원.</strong> <br><br>고급 하드웨어 기능을 활용하고 이러한 기능이 없는 장치를 지원합니다.</td>
<td align="left">런타임 시 <strong>PackageManager.hasSystemFeature</strong>를 사용하여 디바이스 기능을 테스트하면 하드웨어 관련 코드 실행 가능 여부를 확인할 수 있습니다.</td>
<td align="left">런타임 시 디바이스 기능을 테스트하기 위해 수행할 수 있는 <strong>단일 검사는 없으며</strong> 특정 방법으로 각 기능을 테스트하여 하드웨어 관련 코드를 실행할 수 있는지 확인합니다.</td>
<td align="left"><strong>플랫폼 확장 SDK</strong>를 패키지에 추가하여 휴대폰, 데스크톱, IoT 등 다른 장치 패밀리의 추가 기능을 대상으로 할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">ApiInformation API</a></strong>를 사용하여 런타임 시 형식과 멤버의 존재 여부를 테스트하고 있는 경우에만 이러한 형식 및 멤버를 호출할 수 있습니다.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>다른 장치 기능 지원.</strong> <br><br>고급 하드웨어 기능을 활용하고 이러한 기능이 없는 장치를 지원합니다.</td>
<td align="left"><strong>Android 지원 라이브러리</strong>를 앱과 패키징하면 이전 버전의 Android 앱에서 일부 최신 API를 사용할 수 있습니다. 런타임 시 API 수준에 대한 테스트는 <strong>Build.Version.SDK_INT</strong>를 사용하여 수행할 수 있습니다.</td>
<td align="left">표준 런타임 검사는 API 사용 가능 여부를 확인하는 데 사용됩니다. 예를 들어 <strong>class</strong> 메서드는 클래스 존재 여부를 확인하고 <strong>respondsToSelector:</strong>는 클래스의 메서드를 확인합니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/dn949005.aspx">ApiInformation.IsApiContractPresent</a></strong>를 사용하여 지정된 주 및 부 번호의 API 계약이 있는지 확인할 수 있습니다. 또한 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">ApiInformation API</a></strong>를 사용하여 런타임 시 형식과 멤버의 존재 여부를 테스트하고 있는 경우에만 이러한 형식 및 멤버를 호출할 수 있습니다.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">알림</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>타일 및 배지.</strong> <br><br>홈 화면에서 사용자에게 업데이트를 제공합니다.</td>
<td align="left"><strong>앱 위젯</strong>은 홈 화면에 포함하여 정기 업데이트를 받을 수 있는 응용 프로그램의 보기입니다. Android에는 <strong>배지 시스템이 없습니다</strong>. 타일에 동일한 시스템이 없습니다.</td>
  <td align="left">iOS의 알림 센터에 <strong>위젯</strong>이 표시되며 <strong>앱 확장</strong>으로 구현됩니다. 아이콘에 숫자와 함께 <strong>배지</strong>를 추가할 수 있으며 이는 로컬 또는 원격 알림에 대한 응답으로 변경할 수 있습니다. 타일 시스템이 없습니다.</td>
<td align="left">앱에는 시작 화면에 고정하여 선택한 텍스트를 표시할 수 있는 <strong>타일</strong>과, 숫자 및 문자 모양이 있는 <strong>배지</strong>가 있습니다. 푸시 알림 또는 미리 정의된 일정에 따라 앱에서 타일 콘텐츠를 업데이트할 수 있습니다. 타일은 적응형이 될 수 있으며 표시 중인 위치에 따라 변경될 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185605.aspx">타일 만들기</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt590880.aspx">적응형 타일 만들기</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">알림 전달 방법 선택</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465403.aspx">타일 및 배지에 대한 지침</a></td>
</tr>
<tr class="even">
<td align="left"><strong>알림 표시.</strong> <br><br>표시될 수 있는 알림 형식입니다.</td>
<td align="left">알림은 <strong>알림 영역</strong> 및 <strong>알림 창</strong>에 표시할 수 있으며 <strong>경고 알림</strong>은 작은 부동 창에 알림을 제공합니다. <strong>PendingIntent</strong>를 정의하여 알림에 작업을 추가할 수 있습니다.</td>
<td align="left">팝업 알림은 <strong>배너</strong> 또는 <strong>경고</strong>로 나타납니다. <strong>UIMutableUserNotificationAction</strong>을 사용하여 정의된 사용자 지정 작업 단추를 <strong>실행 가능한 알림</strong>에 추가할 수 있습니다.</td>
<td align="left"><strong>알림 메시지</strong>라는 적응형 팝업 알림을 만들 수 있습니다. XML에서 시각적 콘텐츠인 <strong>작업</strong>(단추 또는 입력 및 오디오일 수 있음)이 포함된 알림 메시지를 정의할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">적응형 및 대화형 알림 메시지</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">알림 전달 방법 선택</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465391.aspx">알림 메시지에 대한 지침</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>로컬 알림 예약.</strong> <br><br>예약된 시간에 앱에서 보낸 로컬 알림입니다.</td>
<td align="left">알림 및 작업은 <strong>NotificationCompat.Builder</strong>를 사용하여 정의되며 앱에서 <strong>AlarmManager</strong> 및 <strong>BroadcastReceiver</strong>를 사용하여 예약 및 처리할 수 있습니다.</td>
<td align="left">로컬 알림은 <strong>UILocalNotification</strong>을 사용하여 만들어지며 <b>UILocalNotification.scheduleLocalNotification:<strong>으로 일정을 조정합니다. | </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtoastnotification.aspx">ScheduledToastNotification</a><strong>을 사용하여 알림 메시지의 일정을 조정할 수 있습니다. 타일 알림은 앱에서 </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.tilenotification.aspx">TileNotification 클래스</a><strong>를 사용하여 보낼 수 있으며, 일정은 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtilenotification.aspx">ScheduledTileNotification</a>으로 조정할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">적응형 및 대화형 알림 메시지</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt593299.aspx">로컬 타일 알림 보내기</a> | | </strong>푸시 알림 보내기.</b> 푸시 알림 서버에서 전송하고 필요에 따라 앱 내에서 처리되는 알림.</td>
<td align="left"><strong>Google Cloud Messaging</strong>에서는 Android용 푸시 알림 지원을 제공합니다.</td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>미디어 캡처.</strong> <br><br>오디오 및 동영상 콘텐츠를 기록합니다.</td>
<td align="left">MediaStore.ACTION_VIDEO_CAPTURE 등의 <strong>의도</strong>를 사용하여 기존 카메라 앱으로 미디어를 캡처할 수 있습니다. <strong>android.hardware.camera2</strong> 또는 <strong>카메라</strong> 라이브러리를 사용하여 사용자 지정 카메라 인터페이스를 구현할 수 있습니다. <strong>MediaRecorder</strong> API를 사용하여 오디오를 캡처할 수 있습니다.</td>
<td align="left"><strong>UIImagePickerController</strong>를 사용하면 시스템 UI로 동영상 및 사진을 캡처할 수 있습니다. <strong>AVCaptureSession</strong> 등의 <strong>AVFoundation</strong> 클래스를 사용하면 카메라에 직접 액세스할 수 있습니다. <br/><strong>AVAudioRecorder</strong> 클래스를 사용하면 오디오를 녹음할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.cameracaptureui.aspx">CameraCaptureUI 클래스</a></strong>를 통해 기본 제공 카메라 UI를 사용하여 사진 및 동영상을 캡처할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.mediacapture.aspx">MediaCapture API</a></strong> 등 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.aspx">Windows.Media.Capture</a></strong>의 클래스를 사용하여 낮은 수준 카메라를 조작하고 오디오를 캡처할 수 있습니다. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt282142.aspx">CameraCaptureUI를 사용하여 사진 및 비디오 캡처</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243896.aspx">MediaCapture를 사용하여 사진 및 비디오 캡처</a></td>
</tr>
<tr class="even">
<td align="left"><strong>미디어 재생.</strong> <br><br>오디오 및 동영상 파일을 재생합니다.</td>
<td align="left"><strong>MediaPlayer</strong> 및 <strong>AudioManager</strong> 클래스는 오디오 및 동영상 파일을 재생하는 데 사용됩니다.</td>
<td align="left"><strong>AVKit 프레임워크</strong>, <strong>AVAudioPlayer</strong> 및 <strong>미디어 플레이어 프레임워크</strong>는 오디오 및 동영상 파일을 재생하는 데 사용됩니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.core.mediasource.aspx">MediaSource 클래스</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx">MediaElement</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx">MediaPlayer</a></strong> 클래스를 사용하여 로컬 및 원격 파일과 같은 원본에서 오디오 및 동영상을 재생할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592657.aspx">MediaSource를 사용하여 미디어 재생</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>미디어 편집.</strong> <br><br>기존 녹음에서 새 미디어 파일을 작성하고 특수 효과를 적용합니다.</td>
<td align="left"><strong>MediaCodec</strong>, <strong>MediaMuxer</strong> 및 <strong>android.media.effect</strong>와 같은 낮은 수준 클래스를 콘텐츠 편집을 위해 사용할 수 있습니다.</td>
<td align="left">콘텐츠 편집을 위해 <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> 및 <strong>AVMutableAudioMix</strong>와 같은 <strong>AV Foundation</strong> 프레임워크의 클래스를 사용할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediacomposition.aspx">MediaComposition</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediaclip.aspx">MediaClip</a></strong>과 같은 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.aspx">Windows.Media.Editing</a></strong> API를 사용하여 오디오 및 동영상 파일에서 미디어 컴퍼지션을 만들 수 있습니다. 동영상 및 이미지 오버레이 추가, 비디오 클립 결합, 백그라운드 오디오 추가, 오디오 및 동영상 효과를 적용할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204792.aspx">미디어 컴퍼지션 및 편집</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>센서.</strong> <br><br>장치 이동, 위치 및 환경 속성을 검색합니다.</td>
<td align="left"><strong>센서 프레임워크</strong>는 <strong>SensorManager</strong> 및 <strong>SensorEvent</strong> 등의 클래스를 사용하여 하드웨어 및 소프트웨어 센서에 액세스할 수 있습니다.</td>
<td align="left"><strong>Core Motion 프레임워크</strong>는 원시 및 처리된 센서 데이터에 액세스하는 데 사용됩니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.aspx">Windows.Devices.Sensors</a></strong>의 클래스를 사용하여 센서 읽기 및 센서에서 새 읽기 데이터가 수신될 때 트리거되는 이벤트에 액세스할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187358.aspx">센서</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>위치.</strong> <br><br>디바이스의 <strong>현재</strong> 위치를 찾고 <strong>변경 내용</strong>을 추적합니다.</td>
<td align="left">Google Play Services 위치 API는 <strong>getLastLocation</strong> 및 <strong>requestLocationUpdates</strong> 메서드를 사용하여 <strong>퓨즈 위치 공급자</strong>에게 <strong>마지막으로 알려진 위치</strong>에 대한 높은 수준 액세스를 제공합니다. <strong>LocationManager</strong>를 사용하여 Android 라이브러리에 하위 수준 액세스를 제공합니다.</td>
<td align="left"><strong>핵심 위치</strong> <strong>CLLocationManager</strong> 클래스는 표준 위치 서비스에 대한 <strong>startUpdatingLocation</strong> 및 표준 위치 서비스 및 <strong>중요한 변경</strong> 위치 서비스에 대한 <strong>startMonitoringSignificantLocationChanges</strong>를 사용하여 디바이스의 위치를 모니터링하는 데 사용됩니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.aspx">Windows.Devices.Geolocation</a></strong>의 클래스를 사용하여 디바이스 위치를 추적할 수 있습니다. 한 번 읽으려면 <strong><a href="https://msdn.microsoft.com/library/windows/apps/br225537.aspx">Geolocator.GetGeopositionAsync</a></strong>를 사용합니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.positionchanged.aspx">Geolocator.PositionChanged</a></strong>를 사용하여 타이머를 통해 위치를 정기적으로 가져오거나 위치가 변경될 때 알림을 받을 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219698.aspx">사용자 위치 가져오기</a></td>
</tr>
<tr class="even">
<td align="left"><strong>지도 표시.</strong> <br><br><strong>대화형 기본 제공 지도</strong>를 표시하고 <strong>관심 지점</strong>을 추가합니다.</td>
<td align="left"><strong>Google 지도 Android API</strong>의 <strong>GoogleMap</strong>, <strong>MapFragment</strong> 및 <strong>MapView</strong> 클래스를 사용하여 지도를 앱에 포함할 수 있습니다. 관심 지점은 <strong>표식</strong> 및 사용자 지정 가능한 <strong>Marker</strong> 클래스를 사용하여 표시할 수 있습니다.</td>
<td align="left">지도는 <strong>MapKit 프레임워크</strong>의 <strong>MKMapView</strong> 클래스를 사용하여 iOS 앱에 포함됩니다. 앱에 <strong>주석</strong>을 추가하여 <strong>MKPointAnnotation</strong> 등의 개체 클래스와 <strong>MKPinAnnotationView</strong> 등의 보기 클래스를 통해 관심 지점을 표시할 수 있습니다.</td>
<td align="left">2D, 3D 및 Streetside 뷰를 제공하는 기본 제공 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx">MapControl</a></strong> XAML 컨트롤을 사용하여 앱에 지도를 포함할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapicon.aspx">MapIcon</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolygon.aspx">MapPolygon</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolyline.aspx">MapPolyline</a></strong> 등의 클래스를 사용하여 고정핀, 이미지 또는 셰이프로 관심 지점을 추가할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219695.aspx">2D, 3D 및 Streetside 뷰가 있는 지도 표시</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219696.aspx">지도에 POI(관심 지점) 표시</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>지오펜스.</strong> <br><br>특정 지리적 지역에 들어가고 나가는 것을 모니터링합니다.</td>
<td align="left">지오펜스는 Google Play Services SDK의 <strong>위치 서비스</strong>를 사용하여 모니터링됩니다.</td>
<td align="left">지역은 <strong>CLCircularRegion</strong> 클래스를 사용하여 모니터링되고 <strong>CLLocationManager.startMonitoringForRegion:</strong>을 사용하여 등록됩니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofence.aspx">Geofence</a></strong> 클래스를 사용하여 지오펜스를 만들고 지역에 들어가기와 나가기 등 <strong>모니터링 상태</strong>를 정의합니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofencemonitor.aspx">GeofenceMonitor 클래스</a></strong>를 사용하여 전경에서, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.locationtrigger.aspx">LocationTrigger 백그라운드 클래스</a></strong>를 사용하여 백그라운드에서 지오펜스 이벤트를 처리합니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219702.aspx">지오펜스 설정</a></td>
</tr>
<tr class="even">
<td align="left"><strong>지오코딩 및 리버스 지오코딩.</strong> <br><br>주소를 지리적 위치로 변환하고(지오코딩) 지리적 위치를 주소로 변환합니다(리버스 지오코딩).<br/></td>
<td align="left"><strong>Geocoder</strong> 클래스는 지오코딩 및 리버스 지오코딩에 사용됩니다.</td>
<td align="left"><strong>CLGeocoder</strong> 클래스는 지오코딩에 사용됩니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong>의 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx">MapLocationFinder 클래스</a></strong>를 사용하여 지오코딩을 수행할 수 있습니다. 지오코딩에 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsasync.aspx">FindLocationsAsync</a></strong>, 리버스 지오코딩에 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsatasync.aspx">FindLocationsAtAsync</a></strong>를 사용합니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219697.aspx">지오코딩 및 리버스 지오코딩 수행</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>경로 및 길 찾기.</strong> <br><br>경로, 거리 및 두 지리적 위치 간의 길 찾기를 제공합니다.</td>
<td align="left">Google에서는 SDK가 제공되지 않지만 Android에서 사용할 수 있는 웹 서비스인 <strong>Google 지도 방향 API</strong>를 제공합니다.</td>
<td align="left">지도 키트는 경로 및 길 찾기에 대한 정보를 가져올 수 있는 <strong>MKDirections</strong> API를 제공합니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong>의 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutefinder.aspx">MapRouteFinder</a></strong> 클래스를 사용하여 도보 또는 운전 경로를 요청할 수 있습니다. 경로는 MapControl에 쉽게 표시할 수 있는 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproute.aspx">MapRoute</a></strong> 인스턴스로 반환됩니다. 길 찾기는 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutemaneuver.aspx">MapRouteManeuver</a></strong> 개체 내에 반환됩니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219701.aspx">지도에 경로 및 길 찾기 표시</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>다른 앱 호출.</strong> <br><br>다른 앱을 시작하고 필요에 따라 링크, 텍스트, 사진, 동영상 및 파일을 공유합니다.</td>
<td align="left"><strong>암시적 의도</strong>는 <strong>의도</strong>에서 <strong>작업</strong> 및 선택적 데이터를 정의하고 <strong>startActivityForResult</strong>를 통해 호출하여 다른 앱을 실행하는 데 사용됩니다.<br/></td>
<td align="left"><strong>앱 확장</strong>은 앱 데이터에 대한 액세스를 다른 앱에 제공하는 데 사용할 수 있습니다. <strong>URL 체계</strong>를 사용하면 URL을 다른 앱에 전달할 수 있습니다.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriasync.aspx">Launcher.LaunchUriAsync</a></strong> 또는 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriforresultsasync.aspx">Launcher.LaunchUriForResultsAsync</a></strong>를 사용하여 URI에 등록된 다른 앱을 실행하여 결과를 실행하고 실행된 앱에서 데이터를 다시 가져올 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/hh701471.aspx">Launcher.LaunchFileAsync</a></strong>를 사용하여 다른 앱에서 처리하도록 파일을 전달할 수 있습니다.<br/><br/><strong>공유 계약</strong>을 사용하여 앱 간에 데이터를 쉽게 공유할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228340.aspx">URI에 대한 기본 앱 실행</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">결과에 대한 앱 실행</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299102.aspx">파일에 대한 기본 앱 시작</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243293.aspx">데이터 공유</a></td>
</tr>
<tr class="even">
<td align="left"><strong>앱에서 호출 허용.</strong> <br><br>앱에서 다른 앱의 요청에 응답할 수 있습니다.</td>
<td align="left">앱은 <strong>의도 필터</strong>로 <strong>의도 처리 활동</strong>을 등록하여 다른 앱의 암시적 의도에 응답합니다.</td>
<td align="left"><strong>앱 확장</strong>을 패키징하면 다른 앱과 데이터를 공유할 수 있습니다. Info.plist의 <strong>CFBundleURLTypes</strong> 키를 사용하여 앱에서 <strong>사용자 지정 URL 체계</strong>를 등록할 수 있습니다.</td>
<td align="left">패키지 매니페스트에 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.activationkind.aspx#Protocol">프로토콜</a></strong>을 등록하고, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx">Application.OnActivated</a></strong> 이벤트 처리기를 업데이트하고, 선택적으로 결과를 반환하여 앱이 <strong>URI 체계 이름</strong>에 대한 기본 처리기가 되도록 등록할 수 있습니다. 마찬가지로 패키지 매니페스트에 선언을 추가하고, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onfileactivated.aspx">Application.OnFileActivated</a></strong> 이벤트를 처리하여 앱을 특정 파일 형식에 대한 기본 처리기가 되도록 등록할 수 있습니다.<br/><br/>매니페스트에 공유 대상으로 앱을 등록하고, <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onsharetargetactivated.aspx">Application.OnShareTargetActivated</a></strong> 이벤트를 처리하여 공유 계약 요청을 처리할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">결과에 대한 앱 실행</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269385.aspx">파일 활성화 처리</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243292.aspx">데이터 수신</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>복사 및 붙여넣기.</strong> <br><br>앱 간에 텍스트 및 기타 콘텐츠를 복사하고 붙여넣습니다.</td>
<td align="left"><strong>클립보드 프레임워크</strong>를 사용하여 <strong>ClipboardManager</strong> 및 <strong>ClipData</strong> 클래스로 복사 및 붙여넣기를 구현할 수 있습니다.</td>
<td align="left"><strong>UIPasteboard</strong>, <strong>UIMenuController</strong> 및 <strong>UIResponderStandardEditActions</strong>를 사용하여 복사 및 붙여넣기를 구현할 수 있습니다.</td>
<td align="left">많은 기본 XAML 컨트롤은 이미 복사 및 붙여넣기를 지원합니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/br205967">Windows.ApplicationModel.DataTransfer</a></strong>의 <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx">DataPackage</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.aspx">Clipboard</a></strong> 클래스를 사용하여 복사 및 붙여넣기를 구현할 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243291.aspx">복사 및 붙여넣기</a></td>
</tr>
<tr class="even">
<td align="left"><strong>끌어서 놓기.</strong> <br><br>앱 간에 콘텐츠를 끌어서 놓습니다.</td>
<td align="left">단일 응용 프로그램 내에서 <strong>Android 끌기/놓기 프레임워크</strong>를 사용하여 끌어서 놓기를 구현할 수 있습니다.</td>
<td align="left">iOS에서는 높은 수준의 끌어서 놓기 API가 제공되지 않습니다.</td>
<td align="left">앱에서 끌어서 놓기를 구현하여 앱-앱, 데스크톱-앱, 앱-데스크톱 간 끌어서 놓기 기능을 구현할 수 있습니다. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx">AllowDrop</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx">CanDrag</a></strong> 속성, <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx">DragOver</a></strong> 및 <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx">Drop</a></strong> 이벤트를 사용하여 UIElement 클래스에서 끌어서 놓기 지원을 구현합니다.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt227651.aspx">끌어서 놓기</a></td>
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
<th align="left"><strong>일반적인 개념</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>소프트웨어 디자인 패턴.</strong> <br><br>플랫폼에 대해 권장되거나 사용에 적합한 패턴입니다.</td>
<td align="left">베타 데이터 바인딩 프레임워크는 <strong>MVVM(Model-View-ViewModel)</strong> 패턴을 더 광범위하게 사용할 수 있지만 Android 개발에 대해 권장되거나 제공되는 공식 패턴은 없습니다. 많은 타사 문서 및 프레임워크에서 <strong>MVP(Model-View-Presenter)</strong> 및 <strong>MVVM</strong> 접근 방식을 권장합니다.</td>
<td align="left"><strong>MVC(Model-View-Controller)</strong>는 iOS와 함께 사용되고 플랫폼에 통합된 일반 패턴입니다.</td>
<td align="left">UWP를 빌드할 때 특정 패턴으로 제한되지는 않습니다.<br/><br/>기본 제공 <a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">데이터 바인딩</a> 패턴을 사용하여 데이터 관심사와 UI 관심사를 분리할 수 있으며 속성 값을 업데이트하는 UI 이벤트 처리기를 코딩할 필요가 없습니다.<br/><br/><a href="https://mvvmlight.codeplex.com/">MVVM Light Toolkit</a>와 같은 타사 MVVM 라이브러리를 사용하거나 고유 라이브러리를 롤링하거나 논리를 숨겨진 코드 밖에 유지함으로써 데이터 바인딩을 확장하여 <strong>MVVM(Model-View-ViewModel)</strong> 패턴을 따를 수 있습니다.<br/><br/><a href="https://msdn.microsoft.com/library/hh848246.aspx">MVVM 패턴</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">템플릿 10 Visual Studio 프로젝트 템플릿</a></td>
</tr>
</tbody>
</table>
