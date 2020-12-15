---
title: WinUI 2.0 릴리스 정보
description: WinUI 2.0 릴리스에 대한 정보입니다.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 0e6c1bed21619d9ab12f5699c6a446754576cc29
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603820"
---
# <a name="windows-ui-library-20"></a>Windows UI 라이브러리 2.0

WinUI 2.0은 Windows UI 라이브러리의 최초 공개 릴리스(2018년 10월 릴리스)입니다.

WinUI는 훌륭한 Windows용 Fluent Design 환경을 구축하는 가장 쉬운 방법입니다.

여기에 포함되는 두 가지 NuGet 패키지는 다음과 같습니다.

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml): 컨트롤 및 UWP 앱용 Fluent Design입니다. 주요 WinUI 패키지입니다.

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct): 미들웨어 구성 요소에 사용되는 하위 수준 API입니다.

앱에서 NuGet 패키지 관리자를 사용하여 WinUI 패키지를 다운로드하고 사용할 수 있습니다. 자세한 내용은 [Windows UI 라이브러리 시작](/uwp/toolkits/winui/getting-started)을 참조하세요.

WinUI는 GitHub에 호스팅되는 오픈 소스 프로젝트입니다. [Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 버그 보고서를 제출하고, 기능을 요청하고, 커뮤니티 코드를 제공할 수 있습니다.

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

2018년 10월

[Microsoft.UI.Xaml NuGet 패키지](https://www.nuget.org/packages/Microsoft.UI.Xaml)의 첫 번째 릴리스입니다. 여기에는 Windows UWP 앱용 공식 네이티브 Fluent 컨트롤 및 기능이 포함되어 있습니다.

### <a name="new-features"></a>새로운 기능

이 릴리스의 컨트롤 및 패턴은 다음과 같습니다.

| 기능 | 설명 |
| --- | --- |
|[AcrylicBrush]( /uwp/api/microsoft.ui.xaml.media.acrylicbrush)| 흐림 효과와 노이즈 질감을 포함하여 여러 효과를 사용하는 반투명 재질을 사용하여 영역을 그립니다.|
|[BitmapIconSource]( /uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| 비트맵을 콘텐츠로 사용하는 아이콘 원본을 나타냅니다.|
|[ColorPicker]( /uwp/api/microsoft.ui.xaml.controls.colorpicker)| 사용자가 색 스펙트럼, 슬라이더 및 텍스트 입력을 사용하여 색을 선택할 수 있는 컨트롤을 나타냅니다.|
|[CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|AppBarButton 및 관련 명령 요소에 대한 레이아웃을 제공하는 특수 플라이아웃을 나타냅니다.|
|[DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|메뉴를 열기 위한 갈매기형 수장이 있는 단추를 나타냅니다.|
|[FontIconSource ](/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|지정된 글꼴의 문자 모양을 사용하는 아이콘 원본을 나타냅니다.|
|[MenuBar](/uwp/api/microsoft.ui.xaml.controls.menubar)|가로 행(일반적으로 앱 창의 위쪽에 있음)의 메뉴 세트를 표시하는 특수 컨테이너를 나타냅니다.|
|[MenuBarItem](/uwp/api/microsoft.ui.xaml.controls.menubaritem)|MenuBar 컨트롤의 최상위 메뉴를 나타냅니다.|
|[NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)|앱 콘텐츠를 탐색할 수 있도록 하는 컨테이너를 나타냅니다. 여기에는 헤더, 기본 콘텐츠 보기 및 탐색 명령 메뉴 창이 있습니다.|
|[ParallaxView](/uwp/api/microsoft.ui.xaml.controls.parallaxview)|전경 요소(예: 목록)의 스크롤 위치를 배경 요소(예: 이미지)에 연결하는 컨테이너를 나타냅니다. 전경 요소를 스크롤하면 애니메이션 효과를 배경 요소에 적용하여 시차 효과를 만듭니다.|
|[PersonPicture](/uwp/api/microsoft.ui.xaml.controls.personpicture)|사람에 대한 아바타 이미지가 있는 경우 이를 표시하는 컨트롤을 나타냅니다. 그렇지 않으면 사람의 이니셜 또는 일반 문자 모양을 표시합니다.|
|[RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|사용자가 별 등급을 입력할 수 있는 컨트롤을 나타냅니다.|
|[RefreshContainer](/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|스크롤할 수 있는 콘텐츠에 대한 RefreshVisualizer 및 새로 고치려면 당김 기능을 제공하는 컨테이너 컨트롤을 나타냅니다.|
|[RefreshVisualizer](/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|콘텐츠 새로 고침을 위한 애니메이션 상태 표시기를 제공하는 컨트롤을 나타냅니다.|
|[RevealBackgroundBrush](/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|컴포지션 브러시 및 조명 효과를 사용하는 표시 효과를 사용하여 컨트롤 배경을 그립니다.|
|[RevealBorderBrush](/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|컴포지션 브러시 및 조명 효과를 사용하는 표시 효과를 사용하여 컨트롤 테두리를 그립니다.|
|[RevealBrush](/uwp/api/microsoft.ui.xaml.media.revealbrush)|컴포지션 효과와 조명을 사용하여 시각적 표시 디자인 처리를 구현하는 브러시의 기본 클래스입니다.|
|[SplitButton](/uwp/api/microsoft.ui.xaml.controls.splitbutton)|개별적으로 호출할 수 있는 두 부분으로 구성된 단추를 나타냅니다. 한 부분은 표준 단추처럼 작동하고, 다른 부분은 플라이아웃을 호출합니다.|
|[SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|터치 상호 작용을 통해 상황별 명령에 액세스할 수 있는 컨테이너를 나타냅니다.|
|[SymbolIconSource](/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|Segoe MDL2 자산 글꼴의 문자 모양을 콘텐츠로 사용하는 아이콘 원본을 나타냅니다.|
|[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|텍스트 편집 명령이 포함된 특수 명령 모음 플라이아웃을 나타냅니다.|
|[ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|개별적으로 호출할 수 있는 두 부분으로 구성된 단추를 나타냅니다. 한 부분은 토글 단추처럼 작동하고, 다른 부분은 플라이아웃을 호출합니다.|
|[TreeView](/uwp/api/microsoft.ui.xaml.controls.treeview)|중첩된 항목이 포함된 노드를 확장하거나 축소하는 계층 구조 목록을 나타냅니다.|

## <a name="examples"></a>예

Xaml 컨트롤 갤러리 샘플 앱에는 WinUI 컨트롤을 사용하기 위한 대화형 데모와 샘플 코드가 포함되어 있습니다.

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)에서 XAML 컨트롤 갤러리 앱 설치

* [GitHub에서 오픈 소스](
https://github.com/Microsoft/Xaml-Controls-Gallery)로도 제공되는 Xaml 컨트롤 갤러리

## <a name="documentation"></a>문서

Windows UI 라이브러리 컨트롤에 대한 방법 문서가 [유니버설 Windows 플랫폼 컨트롤 설명서](/windows/uwp/design/controls-and-patterns/)에 포함되어 있습니다.

API 참조 문서는 [Windows UI 라이브러리 API](/windows/winui/api/)에 있습니다.