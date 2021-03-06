---
title: 분할 보기
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: 분할 보기 컨트롤에는 확장/축소 가능한 창 및 콘텐츠 영역이 있습니다.
label: Split view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c942c5748ef1d62dc86f6fa3d041bae651167f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173907"
---
# <a name="split-view-control"></a>분할 보기 컨트롤

분할 보기 컨트롤에는 확장/축소 가능한 창 및 콘텐츠 영역이 있습니다.

> **중요 API**: [SplitView 클래스](/uwp/api/Windows.UI.Xaml.Controls.SplitView)

다음은 SplitView를 사용하여 허브를 표시하는 Microsoft Edge 앱 예제입니다.

![Microsoft Edge 분할 보기 예제](images/split_view_Edge.png)


 분할 보기의 콘텐츠 영역은 항상 표시됩니다. 창은 확장하거나 축소할 수 있으며, 앱 창의 왼쪽 또는 오른쪽에 표시할 수 있습니다. 창에는 다음과 같은 네 가지 모드가 있습니다.

-   **Overlay**

    창이 열릴 때까지 표시되지 않습니다. 열린 창은 콘텐츠 영역을 오버레이합니다.

-   **Inline**

    창이 항상 표시되며 콘텐츠 영역을 오버레이하지 않습니다. 창 및 콘텐츠 영역이 사용 가능한 화면 공간을 나눕니다.

-   **CompactOverlay**

    창의 좁은 부분이 항상 이 모드로 표시되며 아이콘을 표시할 정도의 공간만 사용합니다. 닫힌 창의 기본 너비는 48px이며, `CompactPaneLength`에서 수정할 수 있습니다. 창이 열리면 콘텐츠 영역을 오버레이합니다.

-   **CompactInline**

    창의 좁은 부분이 항상 이 모드로 표시되며 아이콘을 표시할 정도의 공간만 사용합니다. 닫힌 창의 기본 너비는 48px이며, `CompactPaneLength`에서 수정할 수 있습니다. 창이 열릴 경우 콘텐츠에서 사용 가능한 공간이 줄어 들어 콘텐츠를 가리게 됩니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

분할 보기 컨트롤을 사용하여 사용자가 보조 창을 열고 닫을 수 있는 “서랍” 환경을 만들 수 있습니다. 예를 들어 SplitView를 사용하여 [마스터/세부](master-details.md) 패턴을 빌드할 수 있습니다.

확장/축소 단추 및 탐색 항목 목록을 통해 탐색 메뉴를 빌드하려는 경우 [NavigationView](navigationview.md) 컨트롤을 사용합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/SplitView">앱을 열고 SplitView의 기능을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-split-view"></a>분할 보기 만들기

다음은 콘텐츠 옆에 인라인으로 나타나는 열린 창을 동반하는 SplitView 컨트롤입니다.
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-topics"></a>관련 항목
- [탐색 창 패턴](navigationview.md)
- [목록 보기](lists.md)
- [마스터/세부](master-details.md)