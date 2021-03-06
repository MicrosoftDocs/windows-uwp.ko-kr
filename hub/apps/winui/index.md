---
title: Windows UI 라이브러리(WinUI)
description: Windows 앱 개발을 위한 WinUI 라이브러리입니다.
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, 도구 키트 sdk, winui, Windows UI 라이브러리
ms.openlocfilehash: d90101cfd674ddb2d422b200443fe7c8552f8f7a
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629281"
---
# <a name="windows-ui-library-winui"></a>Windows UI 라이브러리(WinUI)

![WinUI 로고](../images/logo-winui.png)

WinUI(Windows UI) 라이브러리는 Windows 데스크톱 및 UWP 애플리케이션 모두에 대한 네이티브 UX(사용자 환경) 프레임워크입니다.

WinUI는 [Fluent Design System](https://www.microsoft.com/design/fluent/#/)을 모든 환경, 컨트롤 및 스타일에 통합함으로써 최신 UI(사용자 인터페이스) 패턴을 사용하여 일관되고 직관적이며 액세스 가능한 환경을 제공합니다.

데스크톱 및 UWP 앱을 모두 지원하므로 [Windows용 React Native](https://microsoft.github.io/react-native-windows/)를 통해 C++, C#, Visual Basic 및 Javascript와 같은 친숙한 언어를 사용하여 WinUI에서 처음부터 빌드하거나 기존 MFC, WinForms 또는 WPF 앱을 점진적으로 마이그레이션할 수 있습니다.

> [!Important]
> WinUI에는 **WinUI 2.x** 및 **WinUI 3** 의 두 가지 버전이 있습니다.

## <a name="windows-ui-2x-library"></a>Windows UI 2.x 라이브러리

WinUI 2.x는 UWP 애플리케이션에서 사용할 수 있으며 [XAML Islands](../desktop/modernize/xaml-islands.md)를 통해 신규 또는 기존 데스크톱 애플리케이션에 통합할 수 있습니다.

> [!NOTE]
> WinUI 2.5는 최신 WinUI 2.x 릴리스입니다. 다음 릴리스에 예정된 작업 목록은 [WinUI 2.6 마일스톤](https://github.com/microsoft/microsoft-ui-xaml/milestone/11)을 참조하세요.

WinUI 2.x 라이브러리는 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/)와 밀접하게 연결되어 있으며, UWP 앱용 공식 네이티브 Windows UI 컨트롤 및 기타 UI 요소를 제공합니다.

Windows 10의 이전 버전과 하위 수준 호환성을 유지하므로, 사용자에게 최신 OS가 없더라도 WinUI 2.x 컨트롤이 작동합니다.

![WinUI 2.x 플랫폼 지원](../images/platforms-winui2.png)

**설치 지침은 [Windows UI 라이브러리 시작](winui2/getting-started.md)을 참조하세요.**

### <a name="related-links-for-winui-2x"></a>WinUI 2.x에 대한 관련 링크

- [WinUI 2.x 라이브러리 개요](winui2/index.md)
- [API 문서](/windows/winui/api/)
- [소스 코드](https://aka.ms/winui)
- [XAML 컨트롤 갤러리 앱](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-project-reunion-05-preview"></a>Windows UI 3 라이브러리(프로젝트 리유니언 0.5 미리 보기)

WinUI 3은 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/)에서 완전히 분리된 네이티브 Windows 10 UI 플랫폼인 WinUI의 다음 버전입니다.

> [!Important]
> WinUI 3 미리 보기의 목적은 개발자 커뮤니티에서 초기 평가를 받고 피드백을 수집하는 것입니다. 프로덕션 앱에는 사용하면 **안 됩니다**.
>
> 첫 번째 공식 지원 릴리스는 2021년 3월 말에 제공할 예정입니다. 프로젝트 리유니언 0.5 패키지에 포함됩니다.
>
> [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)를 사용하여 피드백을 제공하고 제안 사항과 문제를 기록해 주세요.

[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/)에서 XAML, 컴퍼지션 및 입력 API를 완전히 분리하여 WinUI 3의 범위에는 전체 Windows 10 네이티브 UI 플랫폼이 포함됩니다.

[프로젝트 리유니언](../project-reunion/index.md)의 구성 요소인 WinUI 3은 광범위한 대상 Windows 10 OS 버전의 모든 데스크톱 앱에서 일관적인 방식으로 사용할 수 있는 통합 API 및 도구 세트를 제공합니다. 프로젝트 리유니언의 구성 요소인 WinUI 3은 프로젝트 리유니언 패키지의 일부로 제공됩니다. 자세한 내용은 [Windows UI 라이브러리 3 - 프로젝트 리유니언 0.5 미리 보기(2021년 3월 출시 예정)](winui3/index.md)를 참조하세요.

WinUI 3은 모든 Windows 앱에 적용됩니다. 네이티브 UWP 또는 Win32 앱에서 UI 계층으로 사용하거나, [XAML Islands](../desktop/modernize/xaml-islands.md)를 사용하여 데스크톱 앱을 하나씩 점진적으로 현대화할 수 있습니다.

새로운 모든 XAML 기능은 궁극적으로 WinUI의 일부로 제공됩니다. OS의 일부로 제공되는 기존 UWP XAML API는 더 이상 새로운 기능 업데이트를 받지 않습니다. 그러나 Windows 10 지원 주기에 따라 보안 업데이트 및 중요 수정 사항은 계속 받게 됩니다.

유니버설 Windows 플랫폼에는 단순히 XAML 프레임워크 이상의 기능이 포함되어 있습니다. 애플리케이션 및 보안 모델, 미디어 파이프라인, Xbox 및 Windows 10 셸 통합 및 다양한 디바이스와의 호환성과 같은 기능은 계속 개발되고 지원됩니다.

![WinUI 3 플랫폼 지원](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>WinUI 3에 대한 관련 링크

- [Windows UI 라이브러리 3 - 프로젝트 리유니언 0.5 미리 보기(2021년 3월 출시 예정)](winui3/index.md)
- [XAML Controls Gallery(WinUI 3 미리 보기) 앱](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI 리소스

**Github**: WinUI는 Github에 호스팅되는 오픈 소스 프로젝트입니다. [WinUI 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)를 사용하여 기능 요청 또는 버그를 제출하고, WinUI 팀과 상호 작용하며, [로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)에서 WinUI 3 이상에 대한 팀의 계획을 확인합니다.

**웹 사이트**: [WinUI 웹 사이트](https://aka.ms/winui)에서는 제품을 비교하고 WinUI의 다양한 장점을 설명하며, 제품 및 제품 팀과 교류할 수 있는 방법을 제공합니다.