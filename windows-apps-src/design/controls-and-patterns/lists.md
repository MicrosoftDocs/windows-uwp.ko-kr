---
author: muhsinking
Description: Lists display and enable interaction with collection-based content.
title: 목록
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Lists
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f54985d446c0c95f4ee0b917a1d91e546e359483
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5757061"
---
# <a name="lists"></a>목록

목록은 컬렉션 기반 콘텐츠를 표시하고 조작할 수 있게 합니다. 이 문서에서 다루는 네 가지 목록 패턴은 다음과 같습니다.

-   목록 보기 - 주로 텍스트가 많은 콘텐츠 모음을 표시하는 데 사용됨
-   그리드 보기 - 주로 이미지가 많은 콘텐츠 모음을 표시하는 데 사용됨
-   드롭다운 목록 - 사용자가 확장 목록에서 하나의 항목을 선택할 수 있음
-   목록 상자 - 사용자가 스크롤할 수 있는 상자에서 하나 이상의 항목을 선택할 수 있음

각 목록 패턴에 대한 디자인 지침, 기능 및 예제가 제공됩니다.

> **중요 API**: [ListView 클래스](https://msdn.microsoft.com/library/windows/apps/br242878), [GridView 클래스](https://msdn.microsoft.com/library/windows/apps/br242705), [ComboBox 클래스](https://msdn.microsoft.com/library/windows/apps/br209348)


> <div id="main">
> <strong>Windows 10 Fall Creators Update - 동작 변경 사항</strong>
> </div>
> 이제는 기본적으로 선택을 수행하는 대신 터치, 터치 패드, 패시브 펜 등의 활성 펜이 UWP 앱의 목록을 스크롤/이동합니다.
> 앱이 이전 동작을 사용하는 경우 펜 스크롤을 무시하고 이전 동작으로 되돌릴 수 있습니다. 자세한 내용은 [ScrollViewer 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) API 참조 항목을 참조하십시오.

## <a name="list-views"></a>목록 보기

목록 보기를 사용하면 항목을 분류하고, 그룹 헤더를 할당하며, 항목을 끌어서 놓고, 콘텐츠를 구성하며, 항목의 순서를 다시 매길 수 있습니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

목록 보기를 사용하여 다음을 수행할 수 있습니다.

-   주로 텍스트로 구성된 콘텐츠 모음을 표시합니다.
-   단일 또는 분류된 콘텐츠 모음을 탐색합니다.
-   [마스터/세부 정보 패턴](master-details.md)의 마스터 창을 만듭니다. 마스터/세부 정보 패턴은 메일 앱에서 흔히 사용되는 패턴으로, 하나의 창(마스터)에는 선택 가능한 항목이 있으며 다른 창(세부 정보)에는 선택한 항목의 자세히 보기가 있습니다.

### <a name="examples"></a>예제

다음은 휴대폰의 그룹화된 데이터를 보여 주는 간단한 목록 보기입니다.

![그룹화된 데이터를 사용한 목록 보기](images/simple-list-view-phone.png)

### <a name="recommendations"></a>권장 사항

-   목록 내의 항목은 동작이 동일해야 합니다.
-   목록이 그룹으로 나뉜 경우 [시맨틱 줌](semantic-zoom.md)을 사용하면 사용자가 그룹화된 콘텐츠를 쉽게 탐색할 수 있습니다.

### <a name="list-view-articles"></a>목록 보기 문서
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">목록 보기 및 그리드 보기</a></p></td>
<td align="left"><p>앱에서 필수인 목록 보기 또는 그리드 보기 사용에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">항목 컨테이너 및 템플릿</a></p></td>
<td align="left"><p>목록이나 그리드에 표시하는 항목은 앱의 전체 모양에서 중요한 역할을 담당할 수 있습니다. 컨트롤 템플릿과 데이터 템플릿을 수정하여 항목 모양을 정의하고 앱을 멋지게 만들 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">목록 보기의 항목 템플릿</a></p></td>
<td align="left"><p>ListView에서 이러한 항목 템플릿 예제를 사용하여 일반적인 앱 유형의 형태를 확인하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">반전된 목록</a></p></td>
<td align="left"><p>반전된 목록에는 채팅 앱에서처럼 아래쪽에 추가된 새 항목이 있습니다. 앱에서 반전된 목록을 사용하려면 이 지침을 따릅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">당겨서 새로 고침</a></p></td>
<td align="left"><p>당겨서 새로 고침 패턴을 사용하면 데이터 목록을 터치하고 아래로 당겨서 더 많은 데이터를 검색할 수 있습니다. 목록 보기에서 당겨서 새로 고침을 구현하려면 이 지침을 사용합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">중첩된 UI</a></p></td>
<td align="left"><p>중첩된 UI는 실행 가능한 컨트롤을 사용자가 실행할 수도 있는 컨테이너 내에 묶어 표시하는 UI(사용자 인터페이스)입니다. 예를 들어 단추가 있는 목록 보기 항목이 있을 수 있으며 사용자가 목록 항목을 선택하거나 목록 항목 내에 중첩된 단추를 누를 수 있습니다. 사용자에게 최상의 중첩된 UI 환경을 제공하려면 다음 모범 사례를 따르세요.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>그리드 보기

그리드 보기는 이미지 기반 콘텐츠 모음의 정렬 및 탐색에 적합합니다. 그리드 보기 레이아웃은 세로로 스크롤되고 가로로 이동합니다. 항목은 왼쪽에서 오른쪽으로, 위에서 아래로 읽기 순서에 따라 배치됩니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

목록 보기를 사용하여 다음을 수행할 수 있습니다.

-   주로 이미지로 구성된 콘텐츠 모음을 표시합니다.
-   콘텐츠 라이브러리를 표시합니다.
-   [시맨틱 줌](semantic-zoom.md)과 연결된 두 콘텐츠 보기의 서식을 지정합니다.

### <a name="examples"></a>예제

이 예제에서는 일반적인 그리드 보기 레이아웃(앱 찾아보기)을 보여 줍니다. 그리드 보기 항목에 대한 메타데이터는 일반적으로 몇 줄의 텍스트 및 항목 등급으로 제한됩니다.

![그리드 보기 레이아웃의 예](images/controls_gridview_example02.png)

그리드 보기는 사진, 동영상 등의 미디어를 표시하는 데 자주 사용되는 콘텐츠 라이브러리에 이상적인 솔루션입니다. 콘텐츠 라이브러리에서 사용자는 항목을 탭하여 동작을 호출할 수 있을 것으로 기대합니다.

![콘텐츠 라이브러리의 예](images/controls_list_contentlibrary.png)

### <a name="recommendations"></a>권장 사항

-   목록 내의 항목은 동작이 동일해야 합니다.
-   목록이 그룹으로 나뉜 경우 [시맨틱 줌](semantic-zoom.md)을 사용하면 사용자가 그룹화된 콘텐츠를 쉽게 탐색할 수 있습니다.

### <a name="grid-view-articles"></a>그리드 보기 문서
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">목록 보기 및 그리드 보기</a></p></td>
<td align="left"><p>앱에서 필수인 목록 보기 또는 그리드 보기 사용에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">항목 컨테이너 및 템플릿</a></p></td>
<td align="left"><p>목록이나 그리드에 표시하는 항목은 앱의 전체 모양에서 중요한 역할을 담당할 수 있습니다. 컨트롤 템플릿과 데이터 템플릿을 수정하여 항목 모양을 정의하고 앱을 멋지게 만들 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">그리드 보기의 항목 템플릿</a></p></td>
<td align="left"><p>GridView에서 이러한 항목 템플릿 예제를 사용하여 일반적인 앱 유형의 형태를 확인하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">중첩된 UI</a></p></td>
<td align="left"><p>중첩된 UI는 실행 가능한 컨트롤을 사용자가 실행할 수도 있는 컨테이너 내에 묶어 표시하는 UI(사용자 인터페이스)입니다. 예를 들어 단추가 있는 목록 보기 항목이 있을 수 있으며 사용자가 목록 항목을 선택하거나 목록 항목 내에 중첩된 단추를 누를 수 있습니다. 사용자에게 최상의 중첩된 UI 환경을 제공하려면 다음 모범 사례를 따르세요.</p></td>
</tr>
</tbody>
</table>

## <a name="drop-down-lists"></a>드롭다운 목록

콤보 상자라고도 하는 드롭다운 목록은 압축 상태로 시작되어 선택 가능한 항목의 목록을 표시하도록 확장됩니다. 선택한 항목은 항상 표시되며, 표시되지 않은 항목은 사용자가 콤보 상자를 탭하여 확장하면 보기로 가져올 수 있습니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

-   드롭다운 목록을 사용하여 사용자가 한 줄의 텍스트로 적절하게 나타낼 수 있는 일련의 항목에서 단일 값을 선택할 수 있도록 합니다.
-   여러 줄의 텍스트나 이미지가 포함된 항목을 표시하려면 콤보 상자 대신 목록 또는 그리드 보기를 사용합니다.
-   항목이 4개 이하이면 [라디오 단추](radio-button.md)(한 항목만 선택할 수 있는 경우) 또는 [확인란](checkbox.md)(여러 항목을 선택할 수 있는 경우)을 사용합니다.
-   선택 항목이 앱 흐름에서 두 번째로 중요한 경우 콤보 상자를 사용합니다. 대부분의 상황에서 대부분의 사용자에게 기본 옵션이 권장되는 경우 목록 보기를 사용하여 모든 항목을 표시하면 필요 이상으로 옵션에 주의를 집중시킬 수 있습니다. 콤보 상자를 사용하여 공간을 절약하고 주의 분산을 최소화할 수 있습니다.

### <a name="examples"></a>예

압축 상태의 콤보 상자는 헤더를 표시할 수 있습니다.

![압축 상태의 드롭다운 목록의 예](images/combo_box_collapsed.png)

콤보 상자는 더 긴 문자열 길이를 지원하도록 확장되지만 읽기 어려운 너무 긴 문자열은 피해야 합니다.

![텍스트 문자열이 긴 드롭다운 목록의 예](images/combo_box_listitemstate.png)

콤보 상자에 있는 컬렉션이 긴 경우 이를 수용할 수 있는 스크롤 막대가 표시됩니다. 목록의 항목을 논리적으로 그룹화합니다.

![드롭다운 목록에 있는 스크롤 막대의 예](images/combo_box_scroll.png)

### <a name="recommendations"></a>권장 사항

-   콤보 상자 항목의 텍스트 콘텐츠를 한 줄로 제한합니다.
-   가장 논리적인 순서로 콤보 상자의 항목을 정렬합니다. 관련 옵션을 그룹화하고 가장 일반적인 옵션을 맨 위에 배치합니다. 이름은 사전순으로 정렬하고, 숫자는 숫자순으로 정렬하고, 날짜는 시간순으로 정렬합니다.
-   사용자가 글꼴 선택 드롭다운과 같은 화살표 키를 사용하고 있는 동안 라이브로 업데이트되는 콤보 상자를 만들려면 SelectionChangedTrigger를 “항상”으로 설정합니다.  

### <a name="text-search"></a>텍스트 검색

콤보 상자는 자동으로 컬렉션 내의 검색을 지원합니다. 사용자가 열리거나 닫힌 콤보 상자에 포커스가 있는 상태에서 실제 키보드를 통해 문자를 입력하면 사용자 문자열과 일치하는 항목이 표시됩니다. 이 기능은 긴 목록을 탐색할 때 특히 유용합니다. 예를 들어 상태 목록이 포함된 드롭다운을 조작하는 경우 사용자는 빠른 선택을 위해 "w" 키를 눌러 "워싱턴"을 표시할 수 있습니다.


## <a name="list-boxes"></a>목록 상자

목록 상자를 사용하면 사용자가 모음에서 단일 항목 또는 여러 항목을 선택할 수 있습니다. 목록 상자는 항상 열려 있다는 점을 제외하고는 드롭다운 목록과 유사합니다. 목록 상자에는 압축(확장되지 않은) 상태가 없습니다. 모두 표시할 공간이 없는 경우 목록 상자의 항목을 스크롤할 수 있습니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

-   목록 상자는 목록의 항목이 중요하여 강조 표시해야 하거나 전체 목록을 표시하기에 충분한 화면 공간이 있는 경우에 유용할 수 있습니다.
-   목록 상자는 중요한 선택 시 전체 대안 집합에 대한 사용자의 주의를 끌어야 합니다. 반면, 드롭다운 목록은 처음에 선택한 항목에 대한 사용자의 주의를 끕니다.
-   다음에 해당하는 경우 목록 상자를 사용하지 마세요.
    -   목록의 항목 수가 매우 적은 경우. 항상 동일한 2개 옵션이 포함된 단일 선택 목록 상자는 [라디오 단추](radio-button.md)로 제공하는 것이 좋습니다. 3개 또는 4개의 정적 항목이 목록에 있는 경우에도 라디오 단추를 사용하는 것이 좋습니다.
    -   목록 상자에서는 한 개만 선택할 수 있으므로 목록 상자는 "켜짐" 및 "꺼짐"과 같이 하나는 곧 나머지 하나가 '아님'을 의미할 수 있는 동일한 2개 옵션을 항상 포함합니다. 단일 확인란 또는 토글 스위치를 사용합니다.
    -   매우 많은 항목이 있습니다. 긴 목록의 경우 그리드 보기 및 목록 보기를 사용하는 것이 좋습니다. 그룹화된 매우 긴 데이터 목록의 경우 시맨틱 줌을 사용하는 것이 좋습니다.
    -   항목이 연속적인 숫자 값입니다. 이 경우 [슬라이더](slider.md)를 사용하는 것이 좋습니다.
    -   선택 항목이 앱 흐름에서 두 번째로 중요하거나, 대부분의 상황에서 대부분 사용자에게 기본 옵션이 권장됩니다. 드롭다운 목록을 사용합니다.

### <a name="recommendations"></a>권장 사항

-   목록 상자의 항목에 대한 이상적인 범위는 3~9개입니다.
-   목록 상자는 항목이 동적으로 달라질 수 있는 경우에도 제대로 작동합니다.
-   가능한 경우 항목 목록을 이동하거나 스크롤할 필요가 없도록 목록 상자 크기를 설정합니다.
-   목록 상자의 목적과 현재 선택되어 있는 항목이 명확한지 확인합니다.
-   터치 피드백 및 선택한 항목 상태에 대한 시각적 효과와 애니메이션을 유지합니다.
-   목록 상자 항목의 텍스트 콘텐츠를 한 줄로 제한합니다. 항목이 화면 효과인 경우 크기를 사용자 지정할 수 있습니다. 항목에 텍스트 또는 이미지가 여러 줄로 포함된 경우 그리드 보기 또는 목록 보기를 사용합니다.
-   브랜드 지침에 다른 글꼴을 사용하도록 규정되어 있지 않은 경우 기본 글꼴을 사용하세요.
-   명령을 수행하거나 다른 컨트롤을 동적으로 표시하거나 숨기는 데 목록 상자를 사용하지 마세요.

## <a name="selection-mode"></a>선택 모드

선택 모드를 통해 사용자는 단일 항목 또는 여러 항목에 대한 작업을 수행할 수 있습니다. 이 모드는 Ctrl 키 또는 Shift 키를 누른 채 항목을 클릭하거나 갤러리 보기에서 항목의 대상을 롤오버하여 상황에 맞는 메뉴를 통해 호출할 수 있습니다. 선택 모드가 활성화되면 각 목록 항목 옆에 확인란이 표시되고 화면의 위쪽이나 아래쪽에 작업이 표시될 수 있습니다.

다음과 같은 세 가지 선택 모드가 있습니다.

-   단일 - 사용자가 한 번에 하나의 항목만 선택할 수 있습니다.
-   다중 - 사용자가 보조 키를 사용하지 않고 여러 항목을 선택할 수 있습니다.
-   확장 - 사용자가 보조 키를 사용하여 여러 항목을 선택할 수 있습니다. 예를 들어 Shift 키를 누른 상태로 선택할 수 있습니다.

항목의 아무 곳이나 탭하면 선택됩니다. 명령 모음 작업을 탭하면 선택한 모든 항목에 영향을 줍니다. 항목을 선택하지 않으면 “모두 선택"을 제외한 명령 모음 작업이 비활성화되어야 합니다.

선택 모드에는 빠른 해제 모델이 없습니다. 즉, 선택 모드가 활성화된 프레임의 외부를 탭해도 모드가 취소되지 않습니다. 따라서 실수로 모드를 비활성화하는 것을 방지할 수 있습니다. 뒤로 단추를 클릭하면 다중 선택 모드가 해제됩니다.

작업이 선택된 경우 시각적 확인을 표시합니다. 특정 작업, 특히 삭제와 같은 파괴적인 작업에 대해서는 확인 대화 상자를 표시하는 것이 좋습니다.

선택 모드는 해당 모드가 활성화된 페이지로 제한되며, 해당 페이지 외부의 항목에는 영향을 줄 수 없습니다.

선택 모드의 진입점은 이 모드의 영향을 받는 콘텐츠와 나란히 배치되어야 합니다.

명령 모음 권장 사항에 대해서는 [명령 모음에 대한 지침](app-bars.md)을 참조하세요.

## <a name="globalization-and-localization-checklist"></a>세계화 및 지역화 검사 목록

<table>
<tr>
<th>줄 바꿈</th><td>목록 레이블에 두 줄을 허용합니다.</td>
</tr>
<tr>
<th>가로 확장</th><td>필드가 텍스트 확장을 수용할 수 있고 스크롤 가능하도록 합니다.</td>
</tr>
<tr>
<th>세로 간격</th><td>세로 간격에 라틴어가 아닌 문자를 사용하여 라틴어가 아닌 스크립트가 제대로 표시되게 합니다.</td>
</tr>
</table>


## <a name="related-articles"></a>관련 문서

- [허브](hub.md)
- [마스터/세부](master-details.md)
- [탐색 창](navigationview.md)
- [시맨틱 줌](semantic-zoom.md)
- [끌어서 놓기](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [미리 보기 이미지](../../files/thumbnails.md)

**개발자용**
- [ListView 클래스](https://msdn.microsoft.com/library/windows/apps/br242878)
- [GridView 클래스](https://msdn.microsoft.com/library/windows/apps/br242705)
- [ComboBox 클래스](https://msdn.microsoft.com/library/windows/apps/br209348)
- [ListBox 클래스](https://msdn.microsoft.com/library/windows/apps/br242868)