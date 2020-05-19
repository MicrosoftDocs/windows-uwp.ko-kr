---
title: WinUI 2.1 릴리스 정보
description: 새로운 기능 및 버그 수정이 포함된 WinUI 2.1 릴리스에 대한 정보입니다.
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: f62dd912d76f8045a46d467f49167cdc426b75c2
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580620"
---
# <a name="windows-ui-library-21"></a>Windows UI 라이브러리 2.1

Windows UI 라이브러리의 최신 공식 버전인 WinUI 2.1은 2019년 4월 8일에 릴리스되었습니다. 

WinUI는 최신 Fluent 컨트롤 및 스타일을 포함하여 이전 버전의 Windows 10 1주년 업데이트(14393)와 호환되는 방법으로 즉시 사용할 수 있는 다양한 최신 Windows UX 플랫폼 기능을 제공합니다. [XAML Controls Gallery](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery)는 라이브러리에 추가된 모든 새롭고 멋진 기능을 검색할 수 있는 샘플을 제공합니다.

[WinUI 2.1 NuGet 패키지](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)를 다운로드합니다.

NuGet 패키지 관리자를 사용하여 앱에서 WinUI 패키지를 사용하도록 선택할 수 있습니다. 자세한 내용은 [Windows UI 라이브러리 시작](https://docs.microsoft.com/uwp/toolkits/winui/getting-started)을 참조하세요.

WinUI는 GitHub에 호스팅되는 오픈 소스 프로젝트입니다. [Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 버그 보고서를 제출하고, 기능을 요청하고, 커뮤니티 코드를 제공할 수 있습니다.

## <a name="whats-new-in-this-release"></a>이 릴리스의 새로운 기능

### <a name="itemsrepeater"></a>ItemsRepeater

유연한 레이아웃 시스템, 사용자 지정 보기 및 가상화를 사용하는 사용자 지정 컬렉션 환경을 만들려면 ItemsRepeater를 사용합니다.
ListView와는 달리 ItemsRepeater는 포괄적인 최종 사용자 환경을 제공하지 않습니다. 즉, 기본 UI가 없으며 포커스, 선택 또는 사용자 상호 작용과 관련된 정책을 제공하지 않습니다. 대신 이는 고유한 컬렉션 기반 환경 및 사용자 지정 컨트롤을 만드는 데 사용할 수 있는 문서 블록입니다. 더 풍부하고 성능이 뛰어난 환경을 빌드할 수 있도록 지원합니다.

![예제](../images/ItemsRepeater%20-%20MSN%20News.gif)

[문서](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

AnimatedVisualPlayer는 애니메이션 시각적 개체의 재생을 호스팅하고 제어하여 고성능 사용자 지정 동작 그래픽을 앱에 추가할 수 있도록 합니다. 예를 들어 AnimatedVisualPlayer는 Lottie 애니메이션을 표시하고 제어하는 데 사용됩니다.

![예제](../images/AnimatedVisualPlayerUpdated.gif)

[문서](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

TeachingTip은 애플리케이션에서 사용자에게 콘텐츠가 풍부한 비침입 팁을 안내하고 알릴 수 있는 매력적인 Fluent 방법을 제공합니다. TeachingTip은 새롭거나 중요한 기능에 집중하고, 사용자에게 작업을 수행하는 방법을 설명하고, 작업에 대한 상황에 맞는 관련 정보를 제공하여 워크플로를 향상시킬 수 있습니다.

![예제](../images/TeachingTipUpdated.gif)

[문서](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

MenuBar에서 '라디오 단추' 스타일 옵션을 사용할 수 있는 기능이 포함되어 있습니다. 이렇게 하면 라디오 단추 그룹과 같이 결합된 글머리 기호가 있는 옵션 그룹을 사용할 수 있습니다. 개발자를 위한 논리가 처리됩니다.

![예제](../images/RadioMenuFlyoutItem1.png)

[문서](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

컴팩트 모드를 사용하면 개발자가 다양한 시나리오에 맞게 편안한 환경을 만들 수 있습니다. 리소스 사전을 추가하기만 하면 애플리케이션에서 평균 33% 이상 더 많은 UI에 맞출 수 있습니다.

![컴팩트 밀도 예](../images/CompactDensityUpdated.png)

[문서](https://docs.microsoft.com/windows/uwp/design/style/spacing )

### <a name="shadows"></a>그림자

![예제](../images/shadow.gif)

UI 요소에 대한 시각적 계층 구조를 만들면 UI를 쉽게 검사하고 집중해야 하는 항목을 전달할 수 있습니다. 선택된 UI 요소를 앞으로 가져오는 작업인 승격은 소프트웨어에서 이러한 계층 구조를 구현하는 데 사용되는 경우가 많습니다. 

Windows 10 2019년 5월 업데이트를 사용하는 경우 대부분의 일반 컨트롤에서 기본적으로 z 깊이 및 그림자를 사용하여 승격을 추가합니다. 또한 Windows 10 2019년 5월 업데이트가 설치된 OS에서 실행하는 경우 WinUI 2.1의 NavigationView 및 TeachingTip 컨트롤에는 기본 그림자가 있습니다. Windows 10 2019년 5월 업데이트가 릴리스되고 링크가 여기에 게시되면 기본 그림자가 있는 컨트롤의 전체 목록 및 추가 API를 사용하는 방법을 사용할 수 있습니다.

## <a name="examples"></a>예

Xaml 컨트롤 갤러리 샘플 앱에는 WinUI 컨트롤을 사용하기 위한 대화형 데모와 샘플 코드가 포함되어 있습니다.

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)에서 XAML 컨트롤 갤러리 앱 설치

* [GitHub에서 오픈 소스](
https://github.com/Microsoft/Xaml-Controls-Gallery)로도 제공되는 Xaml 컨트롤 갤러리

## <a name="documentation"></a>문서

Windows UI 라이브러리 컨트롤에 대한 방법 문서가 [유니버설 Windows 플랫폼 컨트롤 설명서](/windows/uwp/design/controls-and-patterns/)에 포함되어 있습니다.

API 참조 문서는 [Windows UI 라이브러리 API](/uwp/api/overview/winui/)에 있습니다.

## <a name="microsoftuixaml-21-version-history"></a>Microsoft.UI.Xaml 2.1 버전 기록

### <a name="microsoftuixaml-21-official-release"></a>Microsoft.UI.Xaml 2.1 공식 릴리스

2019년 4월

[GitHub 릴리스 페이지](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[NuGet 패키지 다운로드](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>새 기능(이전 시험판에는 포함되지 않음)

* [CompactDensity](https://docs.microsoft.com/windows/uwp/design/style/spacing): 컴팩트 모드를 사용하면 개발자가 다양한 시나리오에 맞게 편안한 환경을 만들 수 있습니다. 리소스 사전을 추가하기만 하면 애플리케이션에서 평균 33% 이상 더 많은 UI에 맞출 수 있습니다.

* 그림자: UI 요소에 대한 시각적 계층 구조를 만들면 UI를 쉽게 검사하고 집중해야 하는 항목을 전달할 수 있습니다. 선택된 UI 요소를 앞으로 가져오는 작업인 승격은 소프트웨어에서 이러한 계층 구조를 구현하는 데 사용되는 경우가 많습니다. 대부분의 일반 컨트롤에서 기본적으로 z 깊이 및 그림자를 사용하여 승격을 추가합니다.  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-prerelease

2019년 2월

[GitHub 릴리스 페이지](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[NuGet 패키지 다운로드](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

새 실험적 기능:

* [TeachingTip 컨트롤](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  이 새 컨트롤은 앱에서 애플리케이션의 사용자에게 콘텐츠가 풍부한 비침입 팁을 안내하고 알릴 수 있는 방법을 제공합니다. TeachingTip은 새롭거나 중요한 기능에 집중하고, 사용자에게 작업을 수행하는 방법을 설명하고, 작업에 대한 상황에 맞는 관련 정보를 즉시 제공하여 사용자 워크플로를 향상시키는 데 사용할 수 있습니다.

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-prerelease

2019년 2월

[GitHub 릴리스 페이지](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[NuGet 패키지 다운로드](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

새 실험적 기능:

* [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  이 새 컨트롤을 사용하면 [Lottie-Windows](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)를 사용하여 만든 [Lottie](https://github.com/airbnb/lottie) 애니메이션을 포함하여 복잡한 고성능 벡터 애니메이션을 재생할 수 있습니다.

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-prerelease

2018년 12월

[GitHub 릴리스 페이지](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[NuGet 패키지 다운로드](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

새 실험적 기능:

* [ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [RadioButtons](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)
