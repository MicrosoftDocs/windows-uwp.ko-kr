---
title: UWP 앱에 대 한 페이지 레이아웃
description: 앱을 디자인할 때 고려해 야 할 가장 먼저 레이아웃 구조입니다. 이 문서는 UI 요소를 포함 하 여 필요한 및 페이지에 방문 하는 기본 페이지 레이아웃의 일반적인 구조를 설명 합니다. UWP 앱에서 각 페이지 탐색, 명령 및 콘텐츠 요소에 일반적으로 있습니다.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633718"
---
# <a name="page-layout"></a>페이지 레이아웃

UWP 앱에서 각 [ **페이지** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 일반적으로 탐색, 명령 및 콘텐츠 요소를 포함 합니다. 

앱에서 여러 페이지를 가질 수 있습니다: 사용자는 UWP 앱이 시작 될 때 응용 프로그램 코드를 만듭니다는 [ **프레임** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 응용 프로그램의 내부 배치할 [ **창** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). 그런 다음 프레임 [이동](../basics/navigate-between-two-pages.md) 응용 프로그램 간의 [ **페이지** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 인스턴스. 

대부분의 페이지 레이아웃 공통 구조를 따르고이 문서에서는 UI 요소가 필요한 및 페이지에서 이동 해야 합니다. 

![페이지 구조](images/page-components.svg)

## <a name="navigation"></a>탐색
앱 레이아웃을 선택 하면 사용자가 앱에서 페이지 간에 탐색 하는 방법을 정의 하는 탐색 모델을 사용 하 여 시작 합니다. 이 문서에서는 두 가지 일반적인 탐색 패턴 이야기해 보겠습니다: 왼쪽 탐색 창 및 최상위 탐색 다른 탐색 옵션 선택에 대 한 지침을 참조 하세요 [UWP 앱에 대 한 탐색 디자인 기본 사항](../basics/navigation-basics.md)합니다.

![위쪽 및 왼쪽 탐색 패턴](images/top-left-nav.svg)

### <a name="left-nav"></a>왼쪽된 탐색 창
왼쪽 탐색에서 또는 [탐색 창](../controls-and-patterns/navigationview.md) 패턴, 일반적으로 응용 프로그램 수준 탐색을 위해 예약 되어 및 항상 되어야 하도록 표시 되 고 사용할 수 있는 즉, 앱 내에서 가장 높은 수준에 있습니다. 5 개 탐색 항목 또는 앱에서 5 개 페이지 경우 왼쪽된 탐색 창을 좋습니다. 탐색 창 패턴에는 일반적으로 다음이 포함 됩니다.
- 탐색 항목
- 앱 설정 진입점
- 계정 설정 진입점

합니다 [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) 컨트롤 UWP에 대 한 탐색 패턴을 구현 합니다.

탐색 항목을 선택 하면 선택한 항목의 페이지 프레임 이동 해야 합니다.

![탐색 창 확장](images/navview-expanded.svg)

메뉴 단추를 사용 하면 확장 하 고 탐색 창 축소 수 있습니다. 면 화면 크기 보다 크면 640 픽셀을 메뉴 단추를 클릭 하면 막대 탐색 창을 축소 합니다.

![compact 탐색 창](images/navview-compact.svg)

화면 크기 640 보다 작은 경우 580 탐색 창을 축소 완벽 하 게 됩니다.

![탐색 창 최소화](images/navview-minimal.svg)

### <a name="top-nav"></a>최상위 탐색

위쪽 탐색 표시줄 최상위 탐색 역할도 할 수 있습니다. 왼쪽된 탐색 창 축소 가능한 상태인 위쪽 탐색 창 항상 표시 됩니다. 합니다 [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤 UWP 용 최상위 탐색 및 탭 패턴을 구현 합니다.

![위쪽 탐색](images/pivot-large.svg)

## <a name="command-bar"></a>명령 모음

다음으로, 다음 앱의 가장 일반적인 작업에 쉽게 액세스할 수 있는 사용자를 제공 하는 것이 좋습니다. A [명령 모음](../controls-and-patterns/app-bars.md) 앱 수준 또는 페이지 수준 명령에 대 한 액세스를 제공할 수 있으며 모든 탐색 패턴을 사용 하 여 사용할 수 있습니다.

![맨 위에 있는 명령 모음 배치 ](images/app-bar-desktop.svg)

위쪽 또는 페이지의 맨 아래 명령 모음을 배치할 수 있습니다 더 앱에 가장 적합 합니다.

![맨 아래에 있는 명령 모음 배치](images/app-bar-mobile.svg)

## <a name="content"></a>콘텐츠

마지막으로, 콘텐츠 다르므로 널리에서 앱을 앱에 다양 한 방법으로 콘텐츠를 제공할 수 있습니다. 여기에 앱에서 사용 하려는 몇 가지 일반적인 페이지 패턴을 설명 합니다. 많은 앱이 이런 일반적인 페이지 패턴의 일부나 전부를 사용해 여러 유형의 콘텐츠를 표시합니다. 마찬가지로, 자유롭게 앱에 대 한 최적화 하기 위해 이러한 패턴에 맞게 조정할 수 있습니다.

## <a name="landing"></a>방문

![방문 페이지](images/hero-screen.svg)

방문 페이지는 종종 앱 환경의 최상위에 나타나는 '히어로 스크린'으로도 불립니다. 큰 표면이 앱이 사용자가 찾아보고 사용하기 원하는 콘텐츠를 강조하는 무대 역할을 합니다.

## <a name="collections"></a>컬렉션

![갤러리](images/gridview.svg)

컬렉션은 사용자가 콘텐츠나 데이터 그룹을 찾아볼 수 있도록 해줍니다. [그리드 보기](../controls-and-patterns/item-templates-gridview.md)는 사진이나 미디어 중심 콘텐츠에, [목록 보기](../controls-and-patterns/item-templates-listview.md)는 텍스트가 많은 콘텐츠나 데이터에 좋은 옵션이 될 수 있습니다.

## <a name="masterdetail"></a>마스터/세부 정보

![마스터 세부 정보](images/master-detail.svg)

[마스터/세부 정보](../controls-and-patterns/master-details.md) 모델은 목록 보기(마스터) 및 콘텐츠 보기(세부 정보)로 구성되어 있습니다. 두 창은 고정되어 있고 세로 스크롤을 갖고 있습니다. 목록 뷰에서 항목을 선택 하면 콘텐츠 뷰에 마찬가지로 업데이트 됩니다. 

## <a name="forms"></a>양식
![양식](images/form.svg)

[양식](../controls-and-patterns/forms.md)은 사용자가 데이터를 제출할 수 있고 이를 수집할 수 있는 컨트롤 그룹입니다. 대부분의 앱이 페이지 설정과 포털 로그인, 피드백 허브, 계정 생성 등의 프로세스를 위해 양식을 사용합니다. 

## <a name="sample-apps"></a>방법
이러한 패턴을 구현할 수 있는 방법을 참조 하십시오, 우리의 [UWP 샘플 앱](https://developer.microsoft.com/en-us/windows/samples):
- [BuildCast 비디오 플레이어](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database)