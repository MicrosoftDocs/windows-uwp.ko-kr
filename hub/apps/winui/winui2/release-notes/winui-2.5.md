---
title: WinUI 2.5 릴리스 정보
description: 새로운 기능 및 버그 수정이 포함된 WinUI 2.5 릴리스에 대한 정보입니다.
ms.date: 12/01/2020
ms.topic: reference
ms.openlocfilehash: 1fe64ea547b3e8966ea0ec085e77225a3a1895df
ms.sourcegitcommit: e252ccb4a76c4883c7073083d73007957202b84d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657670"
---
# <a name="windows-ui-library-25"></a>Windows UI 라이브러리 2.5

WinUI 2.5는 WinUI(Windows UI) 라이브러리의 최신 공식 릴리스입니다.

WinUI는 GitHub의 [Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 호스팅되는 오픈 소스 프로젝트입니다. 이 리포지토리에서 모든 버그 보고서, 기능 요청 및 커뮤니티 코드 기여를 등록하세요.

WinUI 릴리스: [GitHub 릴리스 페이지](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI 패키지는 NuGet 패키지 관리자를 통해 Visual Studio 프로젝트에 추가할 수 있습니다. 자세한 내용은 [Windows UI 라이브러리 시작](../getting-started.md)을 참조하세요.

NuGet 패키지 다운로드: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>새로운 기능

### <a name="infobar"></a>InfoBar

[InfoBar](/windows/uwp/design/controls-and-patterns/infobar) 컨트롤은 사용자에게 눈에 잘 띄지만 방해가 되지 않도록 앱 전체의 상태 메시지를 표시하는 데 사용됩니다. 이 컨트롤에는 표시된 메시지 유형을 나타내는 Severity 속성과 사용자 고유의 동작 호출 또는 하이퍼링크 단추를 지정하는 옵션이 포함되어 있습니다. InfoBar는 다른 UI 콘텐츠와 인라인되므로 컨트롤이 항상 표시되는지 또는 사용자가 해제할 수 있는지 여부를 지정할 수도 있습니다.

이 예는 닫기 단추와 메시지가 있는 기본 상태의 InfoBar를 보여줍니다.

:::image type="content" source="../images/infobar-default-title-message.png" alt-text="닫기 단추와 메시지가 있는 기본 상태의 InfoBar의 예입니다.":::

이 애니메이션 예에서는 다양한 심각도 상태 및 사용자 지정 메시지가 있는 InfoBar를 보여줍니다.

:::image type="content" source="../images/infobar-severity-animated.gif" alt-text="InfoBar 심각도 상태 및 사용자 지정 메시지의 애니메이션 예입니다.":::

[사용 지침](/windows/uwp/design/controls-and-patterns/infobar)

[API 참조](/windows/winui/api/microsoft.ui.xaml.controls.infobar)

### <a name="determinate-progressring"></a>확정된 ProgressRing

[ProgressRing](/windows/uwp/design/controls-and-patterns/progress-controls)의 확정 상태는 작업의 완료율을 표시합니다. 이는 작업 기간을 알 수 있고 작업 진행률이 앱과의 사용자 상호 작용을 차단하지 않는 작업 중에 사용해야 합니다.

다음 애니메이션 이미지는 확정된 ProgressRing 컨트롤을 보여줍니다.

:::image type="content" source="../images/progressring-determinate-mode-animated.gif" alt-text="확정된 ProgressRing 컨트롤의 애니메이션 예입니다.":::<br>

[사용 지침](/windows/uwp/design/controls-and-patterns/progress-controls#progress-controls-best-practices)

[API 참조](/windows/winui/api/microsoft.ui.xaml.controls.progressring)


### <a name="navigationview-footermenuitems"></a>NavigationView FooterMenuItems

NavigationView 컨트롤의 FooterMenuItems 속성을 사용하여 탐색 창 끝에 탐색 항목을 배치합니다(창 시작 부분에 항목을 배치하는 MenuItems 속성과 비교).

다음 이미지는 바닥글 메뉴에 *계정*, *카트* 및 *도움말* 탐색 항목이 있는 NavigationView를 보여줍니다.

:::image type="content" source="../images/navigationview-footermenuitems.png" alt-text="바닥글 메뉴에 계정, 카트 및 도움말 탐색 항목이 있는 NavigationView의 예입니다.":::

[사용 지침](/windows/uwp/design/controls-and-patterns/navigationview?#footer-menu-items)

[API 참조](/windows/winui/api/microsoft.ui.xaml.controls.navigationview.footermenuitems)

## <a name="samples"></a>샘플

**XAML 컨트롤 갤러리** 샘플 앱에는 이러한 각 WinUI 기능 및 컨트롤의 예가 포함되어 있습니다.

**XAML 컨트롤 갤러리** 앱을 설치하고 최신 버전으로 업데이트한 경우:

- 작동 중인 [InfoBar](xamlcontrolsgallery:/item/InfoBar)를 참조하세요.
- 작동 중인 [ProgressRing](xamlcontrolsgallery:/item/ProgressRing)을 참조하세요.
- 작동 중인 [NavigationView](xamlcontrolsgallery:/item/NavigationView)를 참조하세요.

XAML 컨트롤 갤러리 앱이 설치되지 않은 경우 [Microsoft Store](https://aka.ms/xamlgalleryapp)에서 가져옵니다.

[GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery)에서 XAML 컨트롤 갤러리 소스 코드를 보고, 복제하고, 빌드할 수도 있습니다.

## <a name="other-updates"></a>기타 업데이트

이 릴리스에서 해결된 많은 GitHub 문제는 [주요 변경 사항](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.5.0) 목록을 참조하세요.
