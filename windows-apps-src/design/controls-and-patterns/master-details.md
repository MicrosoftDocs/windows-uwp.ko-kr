---
author: Jwmsft
Description: The master/detail pattern displays a master list and the details for the currently selected item. This pattern is frequently used for email and contact lists/address books.
title: 마스터/세부
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1236c2ade0423f6e092024e786741f3f3bf6d11
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4681704"
---
# <a name="masterdetails-pattern"></a>마스터/세부 정보 패턴

 

마스터/세부 정보 패턴에는 마스터 창(일반적으로 [목록 보기](lists.md)와 함께) 및 콘텐츠의 세부 정보 창이 있습니다. 마스터 목록의 항목을 선택하면 세부 정보 창이 업데이트됩니다. 이 패턴은 메일 및 주소록에 자주 사용됩니다.

> **중요 API**: [ListView 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView), [SplitView 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)

![마스터/세부 정보 패턴의 예](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>올바른 패턴인가요?

다음을 하려는 경우에 마스터/세부 정보 패턴이 제대로 작동합니다.

-   메일 앱, 주소록 또는 목록 세부 정보 레이아웃을 기반으로 하는 앱을 빌드합니다.
-   큰 콘텐츠 컬렉션을 찾아 우선 순위를 지정합니다.
-   컨텍스트 간 앞과 뒤에서 작업하는 동안 목록에 항목을 빠르게 추가하고 목록에서 항목을 제거하도록 허용합니다.

## <a name="choose-the-right-style"></a>올바른 스타일 선택

마스터/세부 정보 패턴을 구현할 때 사용 가능한 화면 공간의 크기에 따라 누적 스타일 또는 가로 정렬 스타일을 사용하는 것이 좋습니다.

| 사용 가능한 창 너비 | 권장 스타일 |
|------------------------|-------------------|
| 320 epx-640 epx        | 누적           |
| 641 epx 이상       | 가로 정렬      |

 
## <a name="stacked-style"></a>누적 스타일

누적 스타일에서는 마스터 또는 세부 정보 창이 한 번에 하나만 표시됩니다.

![누적 모드의 마스터 세부 정보](images/patterns-md-stacked.png)

사용자는 마스터 창에서 시작하여 마스터 목록에서 항목을 선택함으로써 세부 정보 창으로 "드릴다운"합니다. 사용자에게는 마스터 및 세부 정보 보기가 별도의 두 페이지에 있는 것처럼 나타납니다.

### <a name="create-a-stacked-masterdetails-pattern"></a>누적 마스터/세부 정보 패턴 만들기

누적 마스터/세부 정보 패턴을 만드는 한 가지 방법은 마스터 창 및 세부 정보 창에 별도의 페이지를 사용하는 것입니다. 한 페이지에 마스터 보기를 배치하고 세부 정보 창은 별도의 페이지에 배치합니다.

![누적 스타일 마스터 세부 정보에 대한 부분](images/patterns-md-stacked-parts.png)

마스터 보기 페이지의 경우 이미지와 텍스트를 포함할 수 있는 목록을 표시하는 데 [목록 보기](lists.md) 컨트롤이 적합합니다. 

세부 정보 보기 페이지의 경우 가장 적합한 [콘텐츠 요소](../layout/layout-panels.md)를 사용합니다. 별도의 필드가 많은 경우 **그리드** 레이아웃을 사용하여 요소를 양식에 정렬하는 것이 좋습니다.

페이지 간의 탐색에 대한 내용은 [UWP 앱의 탐색 기록 및 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)을 참조하세요.

## <a name="side-by-side-style"></a>가로 정렬 스타일

가로 정렬 스타일에서는 마스터 창과 세부 정보 창이 동시에 표시될 수 있습니다.

![마스터/세부 정보 패턴](images/patterns-masterdetail-400x227.png)

마스터 창의 목록에는 현재 선택한 항목을 나타내는 선택 시각 효과가 있습니다. 마스터 목록에서 새 항목을 선택하면 세부 정보 창이 업데이트됩니다.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>가로 정렬 마스터/세부 정보 패턴 만들기

가로 정렬 마스터/세부 정보 패턴을 만드는 한 가지 방법은 [분할 보기](split-view.md) 컨트롤을 사용하는 것입니다. 마스터 보기를 분할 보기 창에 배치하고 세부 정보 보기는 분할 보기 콘텐츠에 배치합니다.

![마스터 세부 정보 분할 보기 부분](images/patterns_md_splitview_parts.png)

마스터 창의 경우 이미지와 텍스트를 포함할 수 있는 목록을 표시하는 데 [목록 보기](lists.md) 컨트롤이 적합합니다.

세부 정보 콘텐츠의 경우 가장 적합한 [콘텐츠 요소](../layout/layout-panels.md)를 사용합니다. 별도의 필드가 많은 경우 **그리드** 레이아웃을 사용하여 요소를 양식에 정렬하는 것이 좋습니다.

## <a name="adaptive-layout"></a>적응형 레이아웃

모든 화면 크기에서 마스터/세부 정보 패턴을 구현하려면 [적응형 레이아웃](../layout/layouts-with-xaml.md)에서 반응형 UI를 만듭니다.

![적응형 마스터 세부 정보 레이아웃](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>적응형 마스터/세부 정보 패턴 만들기
적응형 레이아웃을 만들려면 UI에서 서로 다른 [**VisualStates**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.visualstate)를 정의하고 [**AdaptiveTriggers**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)를 통해 서로 다른 상태에 대한 중단점을 선언합니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

다음 샘플은 적응형 레이아웃을 통해 마스터/세부 정보 패턴을 구현하고 정적, 데이터베이스 및 온라인 리소스에 대한 데이터 바인딩을 보여줍니다. 
- [마스터/세부 정보 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [마스터/세부 정보 및 선택 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Windows Template Studio 마스터/세부 정보 샘플](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS 수집기 샘플](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>관련 문서

- [목록](lists.md)
- [검색](search.md)
- [앱 및 명령 모음](app-bars.md)
- [ListView 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)
