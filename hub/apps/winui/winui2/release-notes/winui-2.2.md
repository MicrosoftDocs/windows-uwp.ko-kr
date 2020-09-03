---
title: WinUI 2.2 릴리스 정보
description: 새로운 기능 및 버그 수정이 포함된 WinUI 2.2 릴리스에 대한 정보입니다.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 4c200701e0d845d6c9b9f8797899d88cc72d8c1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154907"
---
# <a name="windows-ui-library-22"></a>Windows UI 라이브러리 2.2

WinUI 2.2는 Windows UI 라이브러리의 최신 공식 릴리스입니다.

NuGet 패키지 관리자를 사용하여 WinUI 패키지를 앱에 추가할 수 있습니다. 자세한 내용은 [Windows UI 라이브러리 시작](../getting-started.md)을 참조하세요.

WinUI는 GitHub에 호스팅되는 오픈 소스 프로젝트입니다. [Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 버그 보고서를 제출하고, 기능을 요청하고, 커뮤니티 코드를 제공할 수 있습니다.

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 버전 기록

### <a name="windows-ui-library-22-official-release"></a>Windows UI 라이브러리 2.2 공식 릴리스

2019년 8월

[GitHub 릴리스 페이지](https://github.com/microsoft/microsoft-ui-xaml/releases)

[NuGet 패키지 다운로드](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>새로운 기능

#### <a name="tabview"></a>TabView

![예제](../images/tabview-gif.gif)

#### <a name="description"></a>설명

TabView 컨트롤은 각각 앱의 새 페이지 또는 문서를 나타내는 탭의 컬렉션입니다. TabView는 여러 페이지의 콘텐츠가 앱에 있고 사용자가 탭을 추가하고, 닫고, 다시 정렬해야 하는 경우에 유용합니다. 새 [Windows 터미널](https://github.com/Microsoft/Terminal)에서는 TabView를 사용하여 여러 명령줄 인터페이스를 표시합니다.

#### <a name="documentation"></a>문서

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>NavigationView 업데이트

##### <a name="a-navigationviews-back-button-update"></a>a) NavigationView의 뒤로 단추 업데이트

![예제](../images/navigationview-back-button.gif)

##### <a name="description"></a>설명

NavigationView의 최소 모드에서는 뒤로 단추가 더 이상 사라지지 않습니다. 창을 열고 닫을 때 사용자는 더 이상 커서를 이동하여 햄버거 단추를 클릭할 필요가 없습니다. 이 기능은 기본적으로 작동합니다. 이 작업을 수행하도록 코드를 변경할 필요가 없습니다.

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView - 자동 안쪽 여백 없음

![예제](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>설명

앱 개발자는 이제 NavigationView 컨트롤을 사용하고 제목 표시줄 영역으로 확장하는 경우 앱 창의 모든 픽셀을 회수할 수 있습니다.

##### <a name="documentation"></a>문서

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>시각적 스타일 업데이트

##### <a name="a-corner-radius-update"></a>a) 모퉁이 반경 업데이트

![예제](../images/corner-radius.png)

##### <a name="description"></a>설명

CornerRadius 특성이 추가되었습니다. 약간 둥근 모퉁이를 사용하도록 기본 컨트롤이 업데이트되었습니다. 개발자는 원하는 경우 앱에 고유한 모양을 제공하도록 모퉁이 반경을 쉽게 사용자 지정할 수 있습니다.

##### <a name="github-spec-link"></a>GitHub 사양 링크

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) 테두리 두께 업데이트

![예제](../images/border-thickness.png)

##### <a name="description"></a>설명

BorderThickness 속성을 더 쉽게 사용자 지정할 수 있습니다. 더 깔끔하고 친숙한 모양을 위해 윤곽선을 더 얇게 줄이도록 기본 컨트롤이 업데이트되었습니다.

##### <a name="github-spec-link"></a>GitHub 사양 링크

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) 단추 시각적 업데이트

![예제](../images/button-hover-visual-update.png)

##### <a name="description"></a>설명: 
더 깔끔한 모양을 제공하기 위해 마우스로 가리키는 동안 표시되는 윤곽선을 제거하도록 기본 단추의 시각적 개체가 업데이트되었습니다.

##### <a name="github-spec-link"></a>GitHub 사양 링크:  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>d) SplitButton 시각적 업데이트

![예제](../images/splitbutton-visual-update.png)

##### <a name="description"></a>설명: 
DropDownButton과 더 명확하게 구별되도록 기본 SplitButton의 시각적 개체가 업데이트되었습니다.

##### <a name="github-spec-link"></a>GitHub 사양 링크: 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) ToggleSwitch 시각적 업데이트

![예제](../images/toggleswitch-update.png)

##### <a name="description"></a>설명: 
유용성을 유지하면서 시각적으로 균형을 맞추기 위해 기본 ToggleSwitch의 너비를 44px에서 40px로 줄였습니다.

##### <a name="github-spec-link"></a>GitHub 사양 링크: 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) CheckBox 및 RadioButton 시각적 업데이트

![예제](../images/checkbox-radiobutton.png)

##### <a name="description"></a>설명: 
시각적 스타일 변경의 나머지 부분과 일치하도록 CheckBox 및 RadioButton 시각적 개체가 업데이트되었습니다.

##### <a name="github-spec-link"></a>GitHub 사양 링크: 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>예

Xaml 컨트롤 갤러리 샘플 앱에는 WinUI 컨트롤을 사용하기 위한 대화형 데모와 샘플 코드가 포함되어 있습니다.

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)에서 XAML 컨트롤 갤러리 앱 설치

* [GitHub에서 오픈 소스](
https://github.com/Microsoft/Xaml-Controls-Gallery)로도 제공되는 Xaml 컨트롤 갤러리

## <a name="documentation"></a>문서

Windows UI 라이브러리 컨트롤에 대한 방법 문서가 [유니버설 Windows 플랫폼 컨트롤 설명서](/windows/uwp/design/controls-and-patterns/)에 포함되어 있습니다.

API 참조 문서는 [Windows UI 라이브러리 API](/uwp/api/overview/winui/)에 있습니다.

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 버전 기록

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-prerelease

2019년 7월

[GitHub 릴리스 페이지](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[NuGet 패키지 다운로드](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>실험적 기능

* [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-prerelease

2019년 4월

[GitHub 릴리스 페이지](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[NuGet 패키지 다운로드](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>실험적 기능

* [FlowLayout](/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](/uwp/api/microsoft.ui.xaml.controls.scrollviewer)