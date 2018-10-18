---
author: Jwmsft
Description: The hub control uses a hierarchical navigation pattern to support apps with a relational information architecture.
title: 허브 컨트롤
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: Hub
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2440b8589b0bef6471bd0db4a71bb249a19c056f
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4754368"
---
# <a name="hub-controlpattern"></a>허브 컨트롤/패턴

 


허브 컨트롤을 통해 고유하지만 관련된 섹션 또는 범주로 앱 콘텐츠를 구성할 수 있습니다. 허브의 섹션은 선호 순서에 따라 트래버스하도록 설계되었으며 보다 세부적인 환경의 시작점 역할을 합니다.

> **중요 API**: [Hub 클래스](https://msdn.microsoft.com/library/windows/apps/dn251843), [HubSection 클래스](https://msdn.microsoft.com/library/windows/apps/dn251845)

![허브의 예](images/hub_example_tablet.png)

허브의 콘텐츠는 사용자가 새로운 기능, 사용 가능한 기능 및 관련된 기능을 한눈에 살펴볼 수 있는 파노라마 보기로 표시됩니다. 일반적으로 허브에는 페이지 머리글이 있고 각 콘텐츠 섹션에는 섹션 머리글이 있습니다.


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

허브 컨트롤은 계층에 정렬된 많은 양의 콘텐츠를 표시하는 데 적합합니다. 허브는 새 콘텐츠를 우선적으로 찾고 검색하므로 스토어 또는 미디어 컬렉션에서 항목을 표시하는 데 유용합니다.

허브 컨트롤에는 콘텐츠 탐색 패턴을 원활하게 구성해 주는 여러 가지 기능이 있습니다.

-   **시각적 탐색**

    허브를 통해 간략하면서 검색하기 쉬운 간단한 여러 배열로 콘텐츠를 표시할 수 있습니다.

-   **분류**

    각 허브 섹션에서는 콘텐츠를 논리적 순서로 정렬할 수 있습니다.

-   **혼합 콘텐츠 형식**

    혼합 콘텐츠 형식에서는 가변 자산 크기 및 비율이 일반적입니다. 허브를 사용하면 각 허브 섹션에 각 콘텐츠 형식을 독특하고 깔끔하게 배치할 수 있습니다.

-   **가변 페이지 및 콘텐츠 너비**

    파노라마 모델인 경우 허브에서 다양한 섹션 너비를 허용합니다. 다양한 깊이 또는 수량의 콘텐츠에 유용합니다.

-   **유연한 아키텍처**

    앱 아키텍처를 단순하게 유지하려는 경우 모든 채널 콘텐츠를 허브 섹션 요약에 맞출 수 있습니다.

허브는 사용할 수 있는 여러 탐색 요소 중 하나일 뿐입니다. 탐색 패턴 및 다른 탐색 요소에 대해 자세히 알아보려면 [UWP(유니버설 Windows 플랫폼) 앱용 탐색 디자인 기본 사항](../basics/navigation-basics.md)을 참조하세요.

## <a name="hub-architecture"></a>허브 아키텍처

허브 컨트롤은 계층적 탐색 패턴을 사용하여 관계형 정보 아키텍처가 있는 앱을 지원합니다. 허브는 다양한 콘텐츠 범주로 구성되며, 각 범주는 앱의 섹션 페이지에 매핑됩니다. 섹션 페이지는 시나리오와 섹션에 포함된 콘텐츠를 가장 잘 나타내는 형식으로 표시할 수 있습니다.

![계층적 Food with Friends 앱의 와이어프레임](images/navigation_diagram_food_with_friends_app_new.png)

## <a name="layouts-and-panningscrolling"></a>레이아웃 및 이동/스크롤

허브에서 콘텐츠를 배치하고 탐색하는 몇 가지 방법이 있습니다. 단, 허브의 콘텐츠 목록이 허브 스크롤 방향과 항상 수직으로 이동해야 합니다.

**가로 이동**

![가로 이동 허브의 예](images/controls_hub_horizontal_pan.png)
**세로 이동**

![세로 이동 허브의 예](images/controls_hub_vertical_pan.png)
**세로 스크롤 목록/그리드로 가로 이동**

![세로 스크롤 목록으로 가로 이동 허브의 예](images/controls_hub_horizontal_vertical_scroll.png)
**가로 스크롤 목록/그리드로 세로 이동**

![가로 이동 허브의 예](images/controls_hub_vertical_horizontal_scroll.png)

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Hub">앱을 열고 작동 중인 허브를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

허브는 상당한 디자인 유연성을 제공합니다. 따라서 매력적이고 시각적으로 풍부한 다양한 환경을 가진 앱을 디자인할 수 있습니다. 첫 번째 그룹에 영웅 이미지 또는 콘텐츠 섹션을 사용할 수 있습니다. 관심의 중심을 잃지 않고 큰 영웅 이미지를 세로 및 가로로 자를 수 있습니다. 다음은 하나의 영웅 이미지와 가로, 세로 및 좁은 너비에서 해당 이미지가 잘릴 수 있는 방법의 예제입니다.

![다양한 창 크기에 맞게 잘린 영웅 이미지](images/hub_hero_cropped2.png)

모바일 장치에서는 한 번에 하나의 허브 섹션이 표시됩니다.

![작은 화면에서의 허브 패턴 예](images/phone_hub_example.png)

## <a name="recommendations"></a>권장 사항

-   허브 섹션에 더 많은 콘텐츠가 있음을 알리기 위해서는 일정량이 미리 보이도록 콘텐츠를 클리핑하는 것이 좋습니다.
-   앱의 요구 사항에 따라 여러 가지 허브 섹션을 허브 컨트롤에 추가할 수 있으며, 각 섹션은 고유한 기능 용도를 제공합니다. 예를 들어 한 섹션에는 일련의 링크 및 컨트롤이 포함되고 다른 섹션은 미리 보기 이미지의 리포지토리일 수 있습니다. 사용자는 허브 컨트롤에 기본 제공되는 제스처 지원을 사용하여 이러한 섹션 간에 이동할 수 있습니다.
-   그룹의 콘텐츠를 동적으로 재배치할 수 있어 다양한 창 크기에 맞게 콘텐츠를 최상으로 표시할 수 있습니다.
-   허브 섹션이 많은 경우 시맨틱 줌을 추가합니다. 이렇게 하면 좁은 너비에 맞게 앱 크기가 조정된 경우 쉽게 섹션을 찾는 데도 도움이 됩니다.
-   허브 섹션의 항목이 다른 허브로 이어지지 않도록 하는 것이 좋으며 대신, 대화형 머리글을 사용하여 다른 허브 섹션 또는 페이지를 탐색할 수 있습니다.
-   허브는 시작 지점이므로 앱의 요구에 맞게 사용자 지정할 수 있습니다. 허브의 다음 측면을 변경할 수 있습니다.
    -   섹션 수
    -   각 섹션의 콘텐츠 형식
    -   섹션의 배치 및 순서
    -   섹션 크기
    -   섹션 간격
    -   섹션과 허브 위쪽 또는 아래쪽 사이의 간격
    -   머리글과 콘텐츠의 텍스트 스타일 및 크기
    -   배경, 섹션, 섹션 머리글 및 섹션 콘텐츠의 색

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [허브 클래스](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [탐색 기본 사항](../basics/navigation-basics.md)
- [허브 사용](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)
- [XAML 허브 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?LinkID=310072)
