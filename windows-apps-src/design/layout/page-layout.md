---
title: UWP 앱에 대 한 페이지 레이아웃
description: 응용 프로그램을 디자인할 때 가장 먼저 고려해 야 할 사항은 레이아웃 구조입니다. 이 문서에서는 필요한 UI 요소와 페이지에서 이동 해야 하는 위치를 비롯 하 여 기본 페이지 레이아웃의 공통 구조에 대해 설명 합니다. UWP 앱에서 각 페이지에는 일반적으로 탐색, 명령 및 콘텐츠 요소가 있습니다.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684542"
---
# <a name="page-layout"></a>페이지 레이아웃

UWP 앱에서 각 [**페이지**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 에는 일반적으로 탐색, 명령 및 콘텐츠 요소가 있습니다. 

앱은 여러 페이지를 포함할 수 있습니다. 사용자가 UWP 앱을 시작 하면 응용 프로그램 코드는 응용 프로그램의 [**창**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)내부에 삽입할 [**프레임**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 을 만듭니다. 그러면 프레임은 응용 프로그램의 [**페이지**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 인스턴스 간을 [탐색할](../basics/navigate-between-two-pages.md) 수 있습니다. 

대부분의 페이지는 일반적인 레이아웃 구조를 따르고,이 문서에서는 필요한 UI 요소와 페이지에서 이동 해야 하는 위치에 대해 설명 합니다. 

![페이지 구조](images/page-components.svg)

## <a name="navigation"></a>탐색
앱 레이아웃은 사용자가 앱의 페이지 간에 탐색 하는 방법을 정의 하는 선택한 탐색 모델로 시작 합니다. 이 문서에서는 두 가지 일반적인 탐색 패턴 인 왼쪽 탐색 및 위쪽 탐색을 설명 합니다. 다른 탐색 옵션을 선택 하는 방법에 대 한 지침은 [UWP 앱에 대 한 탐색 디자인 기본 사항](../basics/navigation-basics.md)을 참조 하세요.

![위쪽 및 왼쪽 탐색 패턴](images/top-left-nav.svg)

### <a name="left-nav"></a>왼쪽 탐색
왼쪽 탐색 [창 또는 탐색 창](../controls-and-patterns/navigationview.md) 패턴은 일반적으로 앱 수준 탐색을 위해 예약 되 고 앱 내에서 가장 높은 수준에 존재 하므로 항상 표시 되 고 사용 가능 해야 합니다. 5 개 이상의 탐색 항목이 있거나 앱에 5 개 이상의 페이지가 있는 경우 왼쪽 탐색을 권장 합니다. 탐색 창 패턴은 일반적으로 다음을 포함 합니다.
- 탐색 항목
- 앱 설정에 대 한 진입점
- 계정 설정에 대 한 진입점

[Navigationview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) 컨트롤은 UWP의 왼쪽 탐색 패턴을 구현 합니다.

탐색 항목을 선택 하면 프레임에서 선택한 항목의 페이지로 이동 해야 합니다.

![탐색 창 확장 됨](images/navview-expanded.svg)

사용자는 메뉴 단추를 사용 하 여 탐색 창을 확장 하거나 축소할 수 있습니다. 화면 크기가 640 px 보다 크면 메뉴 단추를 클릭 하면 탐색 창이 막대로 축소 됩니다.

![탐색 창 압축](images/navview-compact.svg)

화면 크기가 640 px 보다 작으면 탐색 창이 완전히 축소 됩니다.

![탐색 창 최소](images/navview-minimal.svg)

### <a name="top-nav"></a>위쪽 탐색

위쪽 탐색은 최상위 탐색 역할을 할 수도 있습니다. 왼쪽 탐색은 축소 가능 하지만 위쪽 탐색은 항상 표시 됩니다. [Navigationview](../controls-and-patterns/navigationview.md) 컨트롤은 UWP의 위쪽 탐색 및 탭 패턴을 구현 합니다.

![위쪽 탐색](images/pivot-large.svg)

## <a name="command-bar"></a>명령 모음

다음으로 사용자에 게 앱의 가장 일반적인 작업에 쉽게 액세스할 수 있도록 제공 하려고 할 수 있습니다. [명령 모음은](../controls-and-patterns/app-bars.md) 앱 수준 또는 페이지 수준 명령에 대 한 액세스를 제공할 수 있으며, 탐색 패턴과 함께 사용할 수 있습니다.

![위쪽에 명령 모음 배치 ](images/app-bar-desktop.svg)

명령 모음은 페이지의 맨 위 또는 맨 아래에 배치 될 수 있으며 앱에 가장 적합 합니다.

![아래쪽에 명령 모음 배치](images/app-bar-mobile.svg)

## <a name="content"></a>콘텐츠

마지막으로 콘텐츠는 앱 마다 크게 달라 지므로 다양 한 방법으로 콘텐츠를 제공할 수 있습니다. 여기서는 앱에서 사용할 수 있는 몇 가지 일반적인 페이지 패턴에 대해 설명 합니다. 많은 앱이 이런 일반적인 페이지 패턴의 일부 또는 전부를 사용하여 여러 유형의 콘텐츠를 표시합니다. 마찬가지로, 앱에 맞게 최적화 하기 위해 이러한 패턴을 자유롭게 조합 하 고 일치 시킬 수 있습니다.

## <a name="landing"></a>방문

![방문 페이지](images/hero-screen.svg)

방문 페이지는 종종 앱 환경의 최상위에 나타나는 히어로 스크린으로도 불립니다. 큰 노출 영역은 앱이 사용자가 찾아서 사용하려는 콘텐츠를 강조하는 무대 역할을 합니다.

## <a name="collections"></a>컬렉션

![갤러리](images/gridview.svg)

컬렉션은 사용자가 콘텐츠 또는 데이터 그룹을 찾을 수 있게 해줍니다. [그리드 보기](../controls-and-patterns/item-templates-gridview.md)는 사진이나 미디어 중심 콘텐츠에, [목록 보기](../controls-and-patterns/item-templates-listview.md)는 텍스트가 많은 콘텐츠나 데이터에 좋은 옵션이 될 수 있습니다.

## <a name="masterdetail"></a>마스터/세부 정보

![마스터 세부 정보](images/master-detail.svg)

[마스터/세부 정보](../controls-and-patterns/master-details.md) 모델은 목록 보기(마스터) 및 콘텐츠 보기(세부 정보)로 구성되어 있습니다. 두 창은 고정되어 있고 세로 스크롤을 갖고 있습니다. 목록 뷰의 항목을 선택 하면 콘텐츠 뷰가 업데이트 됩니다. 

## <a name="forms"></a>양식
![양식](images/form.svg)

[양식](../controls-and-patterns/forms.md)은 사용자의 데이터를 수집하고 제출할 수 있는 컨트롤 그룹입니다. 전부 그런 것은 아니지만 대부분의 앱은 페이지 설정과 포털 로그인, 피드백 허브, 계정 생성 등의 프로세스에 양식을 사용합니다. 

## <a name="sample-apps"></a>방법
이러한 패턴을 구현할 수 있는 방법을 보려면 [UWP 샘플 앱](https://developer.microsoft.com/windows/samples)을 확인 하세요.
- [BuildCast 비디오 플레이어](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database)