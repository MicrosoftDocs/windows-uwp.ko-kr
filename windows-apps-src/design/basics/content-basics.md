---
description: Windows 앱에 콘텐츠를 표시하는 일반적인 페이지 패턴과 UI 요소의 개요를 설명합니다.
title: Windows 앱용 콘텐츠 디자인 기본 사항
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3933910d77249476e76c4d87dfd96af66bd418a8
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031577"
---
# <a name="content-design-basics-for-windows-apps"></a>Windows 앱용 콘텐츠 디자인 기본 사항

모든 앱의 주요 용도는 콘텐츠에 대한 액세스 권한을 제공하는 것입니다. 앱의 목적이 아주 다양하기 때문에 콘텐츠의 유형도 아주 많습니다. 사진 편집 앱은 사진이 콘텐츠이고, 여행 앱은 여행 목적지에 대한 정보와 지도가 콘텐츠인 식입니다. 

이 문서에서는 앱에 콘텐츠를 제공할 수 있는 방법에 대한 개요를 제공합니다. 유형이 무엇이든 콘텐츠를 표시하기 위해 사용할 수 있는 일반적인 페이지 패턴 및 UI 요소를 설명합니다.

## <a name="common-page-patterns"></a>일반적인 페이지 패턴

많은 앱이 이런 일반적인 페이지 패턴의 일부 또는 전부를 사용하여 여러 유형의 콘텐츠를 표시합니다. 마찬가지로, 앱의 콘텐츠를 최적화하기 위해 이러한 패턴을 혼합할 수 있습니다.

### <a name="landing"></a>방문

![방문 페이지](images/content-basics/hero-screen.png)

방문 페이지는 종종 앱 환경의 최상위에 나타나는 히어로 스크린으로도 불립니다. 큰 노출 영역은 앱이 사용자가 찾아서 사용하려는 콘텐츠를 강조하는 무대 역할을 합니다.

### <a name="collections"></a>컬렉션

![갤러리](images/content-basics/gridview.png)

컬렉션은 사용자가 콘텐츠 또는 데이터 그룹을 찾을 수 있게 해줍니다. [그리드 보기](../controls-and-patterns/item-templates-gridview.md)는 사진이나 미디어 중심 콘텐츠에, [목록 보기](../controls-and-patterns/item-templates-listview.md)는 텍스트가 많은 콘텐츠나 데이터에 좋은 옵션이 될 수 있습니다.


### <a name="masterdetail"></a>마스터/세부 정보

![마스터 세부 정보](images/content-basics/master-detail.png)

[마스터/세부 정보](../controls-and-patterns/master-details.md) 모델은 목록 보기(마스터) 및 콘텐츠 보기(세부 정보)로 구성되어 있습니다. 두 창은 고정되어 있고 세로 스크롤을 갖고 있습니다. 목록 보기와 콘텐츠 보기는 관계가 명확합니다. 마스터 보기의 항목을 선택하면 세부 정보 목록이 여기에 맞게 업데이트됩니다. 세부 정보 보기 탐색을 제공할 뿐 아니라, 마스터 보기의 항목을 추가 또는 제거할 수 있습니다.

### <a name="details"></a>세부 정보

![여러 보기](images/multi-view.png)

사용자가 원하는 콘텐츠를 찾을 때 주의 분산 없이 페이지를 볼 수 있는 전용 콘텐츠 보기 페이지를 만드는 것을 고려하세요. 가능하다면 콘텐츠가 확장되어 전체 화면을 채우고 다른 UI 요소를 감추는 [전체 화면 보기 옵션](../layout/show-multiple-views.md)을 만듭니다. 

화면 크기 변경에 맞게 조정되도록 만들려면 적절하게 UI 요소를 감추거나 표시하는 [반응형 디자인](design-and-ui-intro.md)을 만드는 것 또한 고려하세요.

### <a name="forms"></a>양식
![양식](images/content-basics/forms.png)

[양식](../controls-and-patterns/forms.md)은 사용자의 데이터를 수집하고 제출할 수 있는 컨트롤 그룹입니다. 전부 그런 것은 아니지만 대부분의 앱은 페이지 설정과 포털 로그인, 피드백 허브, 계정 생성 등의 프로세스에 양식을 사용합니다. 

## <a name="common-content-elements"></a>일반적인 콘텐츠 요소

이러한 페이지 패턴을 만들려면 개별 콘텐츠 요소를 조합해 사용해야 합니다. 다음은 콘텐츠 표시에 일반적으로 사용되는 UI 요소입니다. 전체 UI 요소 목록은 [컨트롤 및 패턴](../controls-and-patterns/index.md)을 참조하세요.

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">범주</th>
<th align="left">요소</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">오디오 및 비디오<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">미디어 재생 및 전송 컨트롤</a></td>
<td align="left">오디오 및 동영상을 재생합니다.</td>
</tr>
<tr class="even">
<td align="left">이미지 뷰어<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">대칭 이동 보기</a>, <a href="../controls-and-patterns/images-imagebrushes.md">이미지</a></td>
<td align="left">이미지를 표시합니다. 대칭 이동 보기는 앨범의 사진이나 제품 세부 정보 페이지의 품목과 같이 한 번에 하나씩 컬렉션의 이미지를 표시합니다.</td>
</tr>
<tr class="odd">
<td align="left">컬렉션 <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">목록 보기 및 그리드 보기</a></td>
<td align="left">항목을 대화형 목록이나 그리드로 표시합니다. 사용자가 새 개봉작 목록에서 영화를 선택하거나 인벤토리를 관리할 수 있도록 하려면 이러한 요소를 선택합니다.</td>
</tr>
<tr class="even">
<td align="left">텍스트 및 텍스트 입력 <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">텍스트 블록</a>, <a href="../controls-and-patterns/text-box.md">텍스트 상자</a>, <a href="../controls-and-patterns/rich-edit-box.md">서식 있는 편집 상자</a></p>
</td>
<td align="left">텍스트를 표시합니다. 일부 요소를 통해 사용자는 텍스트를 편집할 수 있습니다. 자세한 내용은 <a href="../controls-and-patterns/text-controls.md">텍스트 컨트롤</a>을 참조하세요.
<p>텍스트 표시 방법에 대한 지침은 <a href="../style/typography.md">입력 체계</a>를 참조하세요.</p>
</td>
</tr>
<tr class="odd">
<td align="left">지도<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">지구의 기호 또는 사진 효과 지도를 표시합니다.</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">HTML 콘텐츠를 렌더링합니다.</td>
</tr>
</tbody>
</table>
</div>
