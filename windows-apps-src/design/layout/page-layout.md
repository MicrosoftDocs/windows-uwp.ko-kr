---
title: UWP 앱의 페이지 레이아웃
description: 앱을 디자인할 때 가장 먼저 고려해야 할 사항은 레이아웃 구조입니다. 이 문서에서는 필요한 UI 요소와 페이지에서 이러한 요소를 배치할 위치를 비롯하여 기본 페이지 레이아웃의 공통 구조를 다룹니다. UWP 앱에서 각 페이지에는 일반적으로 탐색, 명령 및 콘텐츠 요소가 있습니다.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684542"
---
# <a name="page-layout"></a>페이지 레이아웃

UWP 앱에서 각 [**페이지**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)에는 일반적으로 탐색, 명령 및 콘텐츠 요소가 있습니다. 

앱에 여러 페이지를 포함할 수 있습니다. 사용자가 UWP 앱을 시작하면 애플리케이션에서는 애플리케이션의 [**창**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) 내부에 배치할 [**프레임**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)을 만듭니다. 그러면 이 프레임은 애플리케이션의 [**페이지**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 인스턴스 간에 [탐색](../basics/navigate-between-two-pages.md)할 수 있습니다. 

대부분의 페이지는 공통 레이아웃 구조를 따르며, 이 문서에서는 필요한 UI 요소와 페이지에서 이러한 요소를 배치할 위치를 다룹니다. 

![페이지 구조](images/page-components.svg)

## <a name="navigation"></a>탐색
앱 레이아웃의 첫 번째 단계는 사용자가 앱의 페이지 간에 이동하는 방법을 정의하는 탐색 모델을 선택하는 것입니다. 이 문서에서는 두 가지 일반적인 탐색 패턴인 왼쪽 탐색과 위쪽 탐색에 대해 설명합니다. 다른 탐색 옵션 선택에 대한 지침은 [UWP 앱의 탐색 디자인 기본 사항](../basics/navigation-basics.md)을 참조하세요.

![위쪽 및 왼쪽 탐색 패턴](images/top-left-nav.svg)

### <a name="left-nav"></a>왼쪽 탐색
왼쪽 탐색 또는 [탐색 창](../controls-and-patterns/navigationview.md) 패턴은 일반적으로 앱 수준 탐색을 위해 예약되고 앱 내에서 가장 높은 수준에 위치하고 있으므로, 항상 표시되고 사용 가능해야 합니다. 탐색 항목이 5개를 초과하거나 앱의 페이지 수가 5페이지를 초과하는 경우 왼쪽 탐색을 사용하는 것이 좋습니다. 탐색 창 패턴은 일반적으로 다음 항목을 포함합니다.
- 탐색 항목
- 앱 설정의 진입점
- 계정 설정의 진입점

[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) 컨트롤은 UWP의 왼쪽 탐색 패턴을 구현합니다.

탐색 항목을 선택하면 프레임이 선택된 항목의 페이지로 이동해야 합니다.

![확장된 탐색 창](images/navview-expanded.svg)

사용자는 메뉴 단추를 사용하여 탐색 창을 확장하거나 축소할 수 있습니다. 화면 크기가 640픽셀보다 큰 경우 메뉴 단추를 클릭하면 탐색 창이 막대로 축소됩니다.

![탐색 창 압축](images/navview-compact.svg)

화면 크기가 640픽셀보다 작은 경우 탐색 창이 완전히 축소됩니다.

![탐색 창 최소화](images/navview-minimal.svg)

### <a name="top-nav"></a>위쪽 탐색

위쪽 탐색은 최상위 탐색 역할을 할 수도 있습니다. 왼쪽 탐색은 축소 가능하지만, 위쪽 탐색은 항상 표시됩니다. [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤은 UWP의 위쪽 탐색 및 탭 패턴을 구현합니다.

![위쪽 탐색](images/pivot-large.svg)

## <a name="command-bar"></a>명령 모음

다음으로, 사용자가 앱에서 가장 많이 사용되는 작업에 쉽게 액세스할 수 있도록 만들어 줍니다. [명령 모음](../controls-and-patterns/app-bars.md)은 앱 수준 또는 페이지 수준 명령에 대한 액세스를 제공하며, 모든 탐색 패턴에 사용할 수 있습니다.

![위쪽에 명령 모음 배치 ](images/app-bar-desktop.svg)

명령 모음은 앱에 따라 페이지의 위쪽 또는 아래쪽에 적절하게 배치할 수 있습니다.

![아래쪽에 명령 모음 배치](images/app-bar-mobile.svg)

## <a name="content"></a>콘텐츠

마지막으로, 콘텐츠는 앱마다 크게 다르므로 다양한 방법으로 콘텐츠를 제공할 수 있습니다. 여기서는 앱에서 사용할 수 있는 몇 가지 일반적인 페이지 패턴에 대해 설명합니다. 많은 앱이 이런 일반적인 페이지 패턴의 일부 또는 전부를 사용하여 여러 유형의 콘텐츠를 표시합니다. 마찬가지로, 이러한 패턴을 자유롭게 혼합하여 앱을 최적화할 수 있습니다.

## <a name="landing"></a>방문

![방문 페이지](images/hero-screen.svg)

방문 페이지는 종종 앱 환경의 최상위에 나타나는 히어로 스크린으로도 불립니다. 큰 노출 영역은 앱이 사용자가 찾아서 사용하려는 콘텐츠를 강조하는 무대 역할을 합니다.

## <a name="collections"></a>컬렉션

![갤러리](images/gridview.svg)

컬렉션은 사용자가 콘텐츠 또는 데이터 그룹을 찾을 수 있게 해줍니다. [그리드 보기](../controls-and-patterns/item-templates-gridview.md)는 사진이나 미디어 중심 콘텐츠에, [목록 보기](../controls-and-patterns/item-templates-listview.md)는 텍스트가 많은 콘텐츠나 데이터에 좋은 옵션이 될 수 있습니다.

## <a name="masterdetail"></a>마스터/세부 정보

![마스터 세부 정보](images/master-detail.svg)

[마스터/세부 정보](../controls-and-patterns/master-details.md) 모델은 목록 보기(마스터) 및 콘텐츠 보기(세부 정보)로 구성되어 있습니다. 두 창은 고정되어 있고 세로 스크롤을 갖고 있습니다. 목록 보기의 항목을 선택하면 그에 맞게 콘텐츠 보기가 업데이트됩니다. 

## <a name="forms"></a>양식
![양식](images/form.svg)

[양식](../controls-and-patterns/forms.md)은 사용자의 데이터를 수집하고 제출할 수 있는 컨트롤 그룹입니다. 전부 그런 것은 아니지만 대부분의 앱은 페이지 설정과 포털 로그인, 피드백 허브, 계정 생성 등의 프로세스에 양식을 사용합니다. 

## <a name="sample-apps"></a>방법
이러한 패턴을 구현하는 방법을 보려면 [UWP 샘플 앱](https://developer.microsoft.com/windows/samples)을 확인하세요.
- [BuildCast 비디오 플레이어](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database)