---
title: WinUI 2.4 릴리스 정보
description: 새로운 기능 및 버그 수정이 포함된 WinUI 2.4 릴리스에 대한 정보입니다.
ms.date: 07/15/2020
ms.topic: reference
ms.openlocfilehash: 5e2ff23b3b0ea63002ad54a367e82e81ee1cd542
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762914"
---
# <a name="windows-ui-library-24"></a>Windows UI 라이브러리 2.4

WinUI 2.4는 WinUI(Windows UI) 라이브러리의 최신 공식 릴리스입니다.

WinUI는 GitHub의 [Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 호스팅되는 오픈 소스 프로젝트입니다. 이 리포지토리에서 모든 버그 보고서, 기능 요청 및 커뮤니티 코드 기여를 등록하세요.

WinUI 릴리스: [GitHub 릴리스 페이지](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI 패키지는 NuGet 패키지 관리자를 통해 Visual Studio 프로젝트에 추가할 수 있습니다. 자세한 내용은 [Windows UI 라이브러리 시작](../getting-started.md)을 참조하세요.

NuGet 패키지 다운로드: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>새로운 기능

### <a name="radialgradientbrush"></a>RadialGradientBrush

RadialGradientBrush는 Center, RadiusX 및 RadiusY 속성으로 정의된 타원 안에 그려집니다. 그라데이션의 색은 타원의 중심에서 시작하고 반지름에서 끝납니다.

![방사형 그라데이션 브러시의 동작을 보여주는 짧은 비디오.](../images/radialgradientbrush.gif)<br>
*방사형 그라데이션 브러시*

[사용 지침](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[API 참조](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

ProgressRing 컨트롤은 ProgressRing이 사라질 때까지 사용자가 차단되는 모달 상호 작용에 사용됩니다. 작업이 완료될 때까지 앱과의 대부분의 상호 작용을 일시 중단해야 하는 경우 이 컨트롤을 사용합니다.

![진행률 링 컨트롤의 동작을 보여주는 짧은 비디오.](../images/progressring.gif)<br>
*ProgressRing 컨트롤*

[사용 지침](/windows/uwp/design/controls-and-patterns/progress-controls)

[API 참조](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>TabView 업데이트

TabView 컨트롤 업데이트는 탭을 렌더링하는 방법에 대한 더 많은 제어 기능을 제공합니다.

선택하지 않은 탭의 너비를 설정하고 아이콘만 표시하여 화면 공간을 절약할 수 있습니다.

![TabView 컨트롤 탭 크기](..\images\tabview-sizing.gif)<br>
*TabView 컨트롤 탭 크기*

사용자가 탭 위로 마우스를 가져갈 때까지 선택되지 않은 탭에서 닫기 버튼을 숨길 수도 있습니다(이전 버전에서는 항상 표시됨).

![TabView 컨트롤을 가리켜 닫기](..\images\tabview-closebuttononhover.gif)<br>
*TabView 컨트롤을 가리켜 닫기*

[사용 지침](/windows/uwp/design/controls-and-patterns/tab-view)

[API 참조](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>TextBox 컨트롤 제품군에 대한 어두운 테마 업데이트

어두운 테마를 사용하면 TextBox 컨트롤 제품군의 배경색이 텍스트 삽입에서 기본적으로 어둡게 유지됩니다(이전 버전에서는 텍스트 삽입 중에 배경색이 흰색으로 변경됨).

| 이전 | 이후 |
| - | - |
| ![업데이트 전 TextBox 어두운 테마 동작을 보여주는 짧은 비디오.](..\images\textbox-darkthemeupdates-before1.gif)<br>*TextBox 어두운 테마 업데이트(이전)* | ![업데이트 후 TextBox 어두운 테마 동작을 보여주는 짧은 비디오.](..\images\textbox-darkthemeupdates-after1.gif)<br>*TextBox 어두운 테마 업데이트(이후)* |
| ![업데이트 전 TextBox 어두운 테마 동작을 보여주는 또 다른 짧은 비디오.](..\images\textbox-darkthemeupdates-before2.gif)<br>*TextBox 어두운 테마 업데이트(이전)* | ![업데이트 후 TextBox 어두운 테마 동작을 보여주는 또 다른 짧은 비디오.](..\images\textbox-darkthemeupdates-after2.gif)<br>*TextBox 어두운 테마 업데이트(이후)* |

다음은 TextBox 컨트롤 제품군에 포함된 몇 가지 컨트롤입니다.

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [편집 가능한 ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>계층적 탐색

[NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4) 컨트롤은 이제 계층적 탐색을 지원하고 Left, Top 및 LeftCompact 표시 모드를 포함합니다. 계층적 NavigationView는 페이지 범주를 표시하거나, 관련 하위 페이지가 있는 페이지를 식별하거나, 많은 다른 페이지에 연결된 허브 스타일 페이지가 있는 앱 내에서 사용하는 데 유용합니다.

![계층적 NavigationView 컨트롤](..\images\HierarchicalNavView.gif)<br>*계층적 NavigationView 컨트롤*

[사용 지침](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[API 참조](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>샘플

설명된 각 WinUI 2.4 기능의 예는 **XAML 컨트롤 갤러리**에 있습니다.

XAML 컨트롤 갤러리 앱이 설치되지 않은 경우 [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)에서 가져옵니다.

[GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery)에서 XAML 컨트롤 갤러리 소스 코드를 볼 수도 있습니다.
