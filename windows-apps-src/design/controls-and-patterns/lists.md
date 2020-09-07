---
description: 모두 함께 표시되는 여러 관련 데이터 항목의 표현인 컬렉션과 목록에 대해 알아봅니다. 
title: 컬렉션 및 목록
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ede68414d86f333b516be81cbae83ea58dc83ba0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169917"
---
# <a name="collections-and-lists"></a>컬렉션 및 목록

컬렉션과 목록은 모두 함께 표시되는 여러 관련 데이터 항목의 표현을 나타냅니다. 컬렉션은 다양한 컬렉션 컨트롤(컬렉션 보기라고도 함)에서 여러 가지 방법으로 표현할 수 있습니다. 컬렉션 컨트롤은 연락처 목록, 날짜 목록, 이미지 컬렉션 등과 같은 컬렉션 기반 콘텐츠와의 상호 작용을 표시하고 사용하도록 설정합니다.

> **중요 API**: [ListView 클래스](/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView 클래스](/uwp/api/Windows.UI.Xaml.Controls.GridView), [FlipView 클래스](/uwp/api/windows.ui.xaml.controls.flipview), [TreeView 클래스](/uwp/api/windows.ui.xaml.controls.treeview), [ItemsRepeater 클래스](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

이 문서에서 설명하는 컨트롤은 다음과 같습니다.

- 목록 보기 - 주로 텍스트가 많은 콘텐츠 모음을 표시하는 데 사용됨
- 그리드 보기 - 주로 이미지가 많은 콘텐츠 모음을 표시하는 데 사용됨
- 대칭 이동 보기 - 포커스를 하나의 항목에 정확히 한 번에 맞추어야 하는 이미지가 많은 콘텐츠 컬렉션을 표시하는 데 주로 사용됩니다.
- 트리 보기 - 특정 계층 구조에서 텍스트가 많은 콘텐츠 컬렉션을 표시하는 데 주로 사용됩니다.
- ItemsRepeater - 사용자 지정 컬렉션 컨트롤을 만들 수 있는 사용자 지정 가능한 구성 요소입니다.

디자인 지침 - 각 컨트롤에 대한 디자인 지침, 기능 및 예제가 아래에 나와 있습니다.

이러한 각 컨트롤(ItemsRepeater 제외)은 기본 제공 스타일 지정 및 상호 작용을 제공합니다. 그러나 컬렉션 보기의 시각적 모양과 내부의 항목을 추가로 사용자 지정하려면 [DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate)을 사용합니다. 데이터 템플릿에 대한 자세한 내용과 컬렉션 보기의 모양을 사용자 지정하는 방법은 [항목 컨테이너 및 템플릿](./item-containers-templates.md) 페이지에서 찾을 수 있습니다.

이러한 각 컨트롤(ItemsRepeater 제외)에는 하나 또는 여러 항목을 선택할 수 있는 기본 제공 동작도 있습니다. 자세히 알아보려면 [선택 모드 개요](selection-modes.md)를 참조하세요.

이 문서에서 다루지 않는 시나리오 중 하나는 테이블 또는 여러 열에 컬렉션을 표시하는 것입니다. 컬렉션을 이 형식으로 표시하려는 경우 [Windows 커뮤니티 도구 키트](/windows/communitytoolkit/)의 [DataGrid 컨트롤](/windows/communitytoolkit/controls/datagrid)을 사용하는 것이 좋습니다. 

> **Windows 10 Fall Creators Update - 동작 변경 사항** 이제 기본적으로 선택을 수행하는 대신 터치, 터치패드, 패시브 펜 등의 활성 펜이 Windows 앱의 목록을 스크롤/이동합니다.
> 앱이 이전 동작을 사용하는 경우 펜 스크롤을 재정의하고 이전 동작으로 되돌릴 수 있습니다. 자세한 내용은 [Scroll Viewer 클래스](/uwp/api/windows.ui.xaml.controls.scrollviewer)에 대한 API 참조 항목을 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML Controls Gallery</strong> 앱이 설치되어 있으면 <a href="xamlcontrolsgallery:/item/ListView">ListView</a>, <a href="xamlcontrolsgallery:/item/GridView">GridView</a>, <a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>, <a href="xamlcontrolsgallery:/item/TreeView">TreeView</a> 및 <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a>가 작동하는지 확인하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>목록 보기

목록 보기는 텍스트가 많은 항목을 나타내며, 일반적으로 세로로 쌓은 단일 열 레이아웃으로 표시됩니다. 항목을 분류하고, 그룹 헤더를 할당하고, 항목을 끌어서 놓고, 콘텐츠를 큐레이팅하고, 항목을 다시 정렬할 수 있습니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

목록 보기를 사용하여 다음을 수행할 수 있습니다.

- 기본적으로 텍스트 기반 항목으로 구성된 컬렉션을 표시합니다. 이 경우 모든 항목에는 동일한 시각적 및 상호 작용 동작이 있어야 합니다.
- 단일 또는 분류된 콘텐츠 컬렉션을 나타냅니다.
- 다음과 같은 일반적인 사용 사례를 포함하여 다양한 사용 사례를 수용합니다.
    - 메시지 또는 메시지 로그의 목록을 만듭니다.
    - 연락처 목록을 만듭니다.
    - [마스터/세부 정보 패턴](master-details.md)의 마스터 창을 만듭니다. 마스터/세부 정보 패턴은 메일 앱에서 흔히 사용되는 패턴으로, 하나의 창(마스터)에는 선택 가능한 항목이 있으며 다른 창(세부 정보)에는 선택한 항목의 자세히 보기가 있습니다.
    

### <a name="examples"></a>예

연락처 목록을 표시하고 데이터 항목을 사전순으로 그룹화하는 간단한 목록 보기는 다음과 같습니다. 그룹 헤더(이 예의 경우 각 알파벳 문자)는 스크롤하는 동안 항상 ListView의 위쪽에 표시되므로 "고정" 상태를 유지하도록 사용자 지정할 수도 있습니다.

![그룹화된 데이터를 사용한 목록 보기](images/listview-grouped-example-resized-final.png)

이는 최신 메시지가 아래쪽에 표시되는 메시지 로그를 표시하기 위해 반전된 ListView입니다. 반전된 ListView를 사용하면 항목이 기본 제공 애니메이션이 포함된 화면의 아래쪽에 표시됩니다.

![반전된 목록 보기](images/listview-inverted-2.png)

### <a name="related-articles"></a>관련된 문서
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
<td align="left"><p>목록 또는 그리드 보기에 표시되는 항목은 앱의 전체 모양에서 중요한 역할을 수행할 수 있습니다. 컨트롤 템플릿 및 데이터 템플릿을 수정하면서 컬렉션 항목의 모양을 사용자 지정하여 앱을 멋지게 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">목록 보기의 항목 템플릿</a></p></td>
<td align="left"><p>이러한 ListView용 예제 항목 템플릿을 사용하여 일반적인 앱 유형의 모양을 확인합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">반전된 목록</a></p></td>
<td align="left"><p>반전된 목록에는 채팅 앱에서처럼 아래쪽에 추가된 새 항목이 있습니다. 이 문서의 지침에 따라 앱에서 반전된 목록을 사용합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">당겨서 새로 고침</a></p></td>
<td align="left"><p>당겨서 새로 고침 메커니즘을 사용하면 사용자가 더 많은 데이터를 검색할 수 있도록 터치를 사용하여 데이터 목록을 아래로 당길 수 있습니다. 이 문서를 사용하여 목록 보기에서 당겨서 새로 고침을 구현합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">중첩된 UI</a></p></td>
<td align="left"><p>중첩된 UI는 실행 가능한 컨트롤을 사용자가 실행할 수도 있는 컨테이너 내에 묶어 표시하는 UI(사용자 인터페이스)입니다. 예를 들어 단추가 있는 목록 보기 항목이 있을 수 있으며 사용자가 목록 항목을 선택하거나 목록 항목 내에 중첩된 단추를 누를 수 있습니다. 사용자에게 최상의 중첩된 UI 환경을 제공하려면 다음 모범 사례를 따르세요.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>그리드 보기

그리드 보기는 이미지 기반 콘텐츠 모음의 정렬 및 탐색에 적합합니다. 그리드 보기 레이아웃은 세로로 스크롤되고 가로로 이동합니다. 항목은 왼쪽에서 오른쪽으로, 위에서 아래로의 읽기 순서로 표시되므로 래핑된 레이아웃으로 있습니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

그리드 보기를 사용하여 다음을 수행합니다.

- 각 항목의 초점이 이미지이고 각 항목의 시각적 및 상호 작용 동작이 동일해야 하는 콘텐츠 컬렉션을 표시합니다.
- 콘텐츠 라이브러리를 표시합니다.
- [시맨틱 줌](semantic-zoom.md)과 연결된 두 콘텐츠 보기의 서식을 지정합니다.
- 다음과 같은 일반적인 사용 사례를 포함하여 다양한 사용 사례를 수용합니다.
    - 상점형 사용자 인터페이스(예: 앱, 노래, 제품 검색)
    - 대화형 사진 라이브러리

### <a name="examples"></a>예

이 예제에서는 일반적인 그리드 보기 레이아웃(앱 찾아보기)을 보여 줍니다. 그리드 보기 항목에 대한 메타데이터는 일반적으로 몇 줄의 텍스트 및 항목 등급으로 제한됩니다.

![그리드 보기 레이아웃의 예](images/controls_gridview_example02.png)

그리드 보기는 사진, 동영상 등의 미디어를 표시하는 데 자주 사용되는 콘텐츠 라이브러리에 이상적인 솔루션입니다. 콘텐츠 라이브러리에서 사용자는 항목을 탭하여 동작을 호출할 수 있을 것으로 기대합니다.

![콘텐츠 라이브러리의 예](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>관련된 문서
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
<td align="left"><p>목록 또는 그리드 보기에 표시되는 항목은 앱의 전체 모양에서 중요한 역할을 수행할 수 있습니다. 컨트롤 템플릿 및 데이터 템플릿을 수정하면서 컬렉션 항목의 모양을 사용자 지정하여 앱을 멋지게 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">그리드 보기의 항목 템플릿</a></p></td>
<td align="left"><p>이러한 GridView용 예제 항목 템플릿을 사용하여 일반적인 앱 유형의 모양을 확인합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">중첩된 UI</a></p></td>
<td align="left"><p>중첩된 UI는 실행 가능한 컨트롤을 사용자가 실행할 수도 있는 컨테이너 내에 묶어 표시하는 UI(사용자 인터페이스)입니다. 예를 들어 단추가 포함된 그리드 보기 항목이 있을 수 있으며, 사용자는 그리드 항목을 선택하거나 해당 항목 내에 중첩된 단추를 누를 수 있습니다. 사용자에게 최상의 중첩된 UI 환경을 제공하려면 다음 모범 사례를 따르세요.</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>대칭 이동 보기

대칭 이동 보기는 이미지 기반 콘텐츠 컬렉션을 검색하는 데 적합합니다. 특히 한 번에 하나의 이미지만 표시되는 환경이 필요한 경우입니다. 대칭 이동 보기를 사용하면 사용자가 상호 작용한 후에 각 항목이 한 번에 하나씩 나타나도록 컬렉션 항목을 이동시키거나 "대칭 이동"시킬 수 있습니다(세로 또는 가로로).

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

대칭 이동 보기를 사용하여 다음을 수행합니다.

- 메타데이터가 거의 없거나 전혀 없는 이미지로 구성된 중소 규모의 컬렉션(25개 미만의 항목)을 표시합니다.
- 항목을 한 번에 하나씩 표시하고, 최종 사용자가 자신의 속도에 따라 항목을 대칭 이동할 수 있도록 합니다.
- 다음과 같은 일반적인 사용 사례를 포함하여 다양한 사용 사례를 수용합니다.
    - 사진 갤러리
    - 제품 갤러리 또는 쇼케이스

### <a name="examples"></a>예

다음 두 예에서는 각각 가로 및 세로로 대칭 이동하는 FlipView를 보여 줍니다.

![가로 대칭 이동 보기](images/controls_flipview_horizonal.jpg)

![세로 대칭 이동 보기](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>관련된 문서
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
<td align="left"><p><a href="flipview.md">대칭 이동 보기</a></p></td>
<td align="left"><p>대칭 이동 보기 내에서 항목의 모양을 사용자 지정하는 방법과 함께 앱에서 대칭 이동 보기를 사용하는 데 필요한 기본 사항에 대해 알아봅니다.</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>트리 보기

트리 보기는 전시해야 하는 중요한 계층 구조가 포함된 텍스트 기반 컬렉션을 표시하는 데 적합합니다. 트리 보기 항목은 펼치거나 접을 수 있고, 시각적 계층 구조에 표시되고, 아이콘으로 보완할 수 있으며, 트리 보기 간에 끌어서 놓을 수 있습니다. 트리 보기에서는 N-수준 중첩을 사용할 수 있습니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

트리 보기를 사용하여 다음을 수행합니다.

- 컨텍스트와 의미가 계층 구조 또는 특정 조직 체인에 따라 달라지는 중첩된 항목 컬렉션을 표시합니다.
- 다음과 같은 일반적인 사용 사례를 포함하여 다양한 사용 사례를 수용합니다.
    - 파일 브라우저
    - 회사 조직도

### <a name="examples"></a>예

파일 탐색기를 나타내는 트리 보기의 예는 다음과 같으며, 아이콘으로 보완된 여러 개의 중첩된 항목을 보여 줍니다.

![아이콘을 사용하는 트리 보기](images/treeview-icons.png)

### <a name="related-articles"></a>관련된 문서
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
<td align="left"><p><a href="tree-view.md">트리 보기</a></p></td>
<td align="left"><p>트리 보기 내에서 항목의 모양과 상호 작용 동작을 사용자 지정하는 방법과 함께 앱에서 트리 보기를 사용하는 데 필요한 기본 사항에 대해 알아봅니다.</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

예를 들어 속성을 정의하지 않고 단순히 페이지에 배치하기만 하는 경우 스타일 지정 또는 상호 작용을 제공하지 않으므로 ItemsRepeater는 이 페이지에 표시된 나머지 컬렉션 컨트롤과 다릅니다. ItemsRepeater는 개발자가 고유한 사용자 지정 컬렉션 컨트롤, 특히 이 문서의 다른 컨트롤을 사용하여 얻을 수 없는 컨트롤을 만드는 데 사용할 수 있는 구성 요소입니다. ItemsRepeater는 정확한 요구 사항에 맞게 조정할 수 있는 데이터 기반 고성능 패널입니다.

### <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

다음과 같은 경우 ItemsRepeater를 사용합니다.

- 기존 컬렉션 컨트롤을 사용하여 만들 수 없는 특정 사용자 인터페이스와 사용자 환경을 고려하고 있습니다.
- 항목에 대한 기존 데이터 원본(예: 인터넷에서 가져온 데이터, 데이터베이스 또는 코드 숨김에 있는 기존 컬렉션)이 있습니다.

### <a name="examples"></a>예

동일한 데이터 원본(숫자 컬렉션)에 바인딩된 모든 ItemsRepeater 컨트롤의 세 가지 예는 다음과 같습니다. 숫자 컬렉션은 세 가지 방법으로 표현되며, 아래의 각 ItemRepeater는 서로 다른 사용자 지정 [Layout](/uwp/api/microsoft.ui.xaml.controls.layout)과 [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2)을 사용합니다.

![가로 막대를 사용하는 ItemsRepeater](images/itemsrepeater-1.png)
![세로 막대를 사용하는 ItemsRepeater](images/itemsrepeater-2.png)
![원형 표현을 사용하는 ItemsRepeater](images/itemsrepeater-3.png)

### <a name="related-articles"></a>관련된 문서
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
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>컬렉션 보기에 필요한 모든 상호 작용 및 시각적 구성 요소를 구현하는 방법과 함께 앱에서 ItemsRepeater를 사용하는 데 필요한 기본 사항에 대해 알아봅니다.</p></td>
</tr>
</tbody>
</table>


## <a name="globalization-and-localization-checklist"></a>세계화 및 지역화 검사 목록

<table>
<tr>
<th>줄 바꿈</th><td>목록 레이블에 두 줄을 허용합니다.</td>
</tr>
<tr>
<th>가로 확장</th><td>필드에서 텍스트 확장을 수용할 수 있고 스크롤할 수 있도록 합니다.</td>
</tr>
<tr>
<th>세로 간격</th><td>라틴 문자가 아닌 문자를 세로 간격으로 사용하여 라틴어가 아닌 스크립트가 제대로 표시되도록 합니다.</td>
</tr>
</table>

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련된 문서

**디자인 및 UX 지침**
- [마스터/세부](master-details.md)
- [탐색 창](navigationview.md)
- [시맨틱 줌](semantic-zoom.md)
- [끌어서 놓기](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [미리 보기 이미지](../../files/thumbnails.md)

**API 참조**
- [ListView 클래스](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView 클래스](/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [ComboBox 클래스](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)
- [ListBox 클래스](/uwp/api/Windows.UI.Xaml.Controls.ListBox)