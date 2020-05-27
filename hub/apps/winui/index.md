---
title: Windows UI 라이브러리(WinUI)
description: Windows 앱 개발을 위한 WinUI 라이브러리입니다.
ms.topic: article
ms.date: 05/11/2020
keywords: windows 10, uwp, 도구 키트 sdk, winui, Windows UI 라이브러리
ms.openlocfilehash: 01eed4a82bfe14b70c86ade1b82c487e33e6f6f6
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775887"
---
# <a name="windows-ui-library-winui"></a>Windows UI 라이브러리(WinUI)

![도구 키트 영웅 이미지](../images/logo-winui.png)

Windows UI 라이브러리(WinUI)는 Win32와 UWP 모두에서 Windows 앱을 위한 최신 기본 UI(사용자 인터페이스) 플랫폼입니다.

WinUI는 [Fluent Design System](https://www.microsoft.com/design/fluent/#/)을 모든 컨트롤 및 스타일에 통합함으로써 최신 UI 패턴을 사용하여 일관되고 직관적이며 액세스할 수 있는 환경을 제공합니다.

그리고 Win32 및 UWP 앱 모두를 지원하기 때문에 [Windows용 React Native](https://microsoft.github.io/react-native-windows/)를 통해 C++, C#, Visual Basic, 심지어 Javascript와 같은 친숙한 언어를 사용하여 WinUI를 처음부터 빌드하거나 기존 MFC, WinForms 또는 WPF 앱을 원하는 속도로 마이그레이션할 수 있습니다.

WinUI에는 **WinUI 2.x** 및 **WinUI 3.0** 두 가지 버전이 있습니다.

## <a name="windows-ui-2x-library"></a>Windows UI 2.x 라이브러리

WinUI 2.x는 현재 UWP 애플리케이션에서 사용할 수 있으며 [XAML Islands](/windows/apps/desktop/modernize/xaml-islands)를 통해 기존 데스크톱 애플리케이션에 통합할 수 있습니다.

WinUI 2.x 라이브러리는 UWP SDK와 밀접하게 연결되어 있으며 UWP 앱용 네이티브 Windows UI 제어 및 기타 UI 요소를 제공합니다.

Windows 10의 이전 버전과 하위 수준 호환성을 유지하므로, 사용자에게 최신 OS가 없더라도 WinUI 2.x 컨트롤이 작동합니다.

![WinUI 2.x 플랫폼 지원](../images/platforms-winui2.png)

> [!NOTE]
> WinUI 2.4는 최신 WinUI 2.x 릴리스입니다. 다음 릴리스에 예정된 작업 목록은 [WinUI 2.5 마일스톤](https://github.com/microsoft/microsoft-ui-xaml/milestone/10)을 참조하세요.

설치 지침은 [Windows UI 라이브러리 시작](winui2/getting-started.md)을 참조하세요.

### <a name="related-links-for-winui-2x"></a>WinUI 2.x에 대한 관련 링크

- [WinUI 2.x 라이브러리 개요](winui2/index.md)
- [API 문서](https://docs.microsoft.com/uwp/api/overview/winui/)
- [소스 코드](https://aka.ms/winui)
- [XAML 컨트롤 갤러리 앱](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Windows UI 3.0 라이브러리(Preview 1)

WinUI 3은 UWP SDK에서 완전히 분리된 네이티브 Windows 10 UI 플랫폼인 WinUI의 다음 버전입니다.

Windows UI 3.0 라이브러리는 UWP SDK에서 Xaml, 컴퍼지션 및 입력 API를 완전히 분리하여 전체 Windows 10 네이티브 UI 플랫폼을 포함할 수 있도록 WinUI의 범위를 크게 확장할 수 있습니다.

WinUI는 모든 Windows 앱의 전달 경로 역할을 합니다. 네이티브 UWP 또는 Win32 앱에서 UI 계층으로 사용하거나, [Xaml Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands)를 통해 데스크톱 앱을 하나씩 현대화할 수 있습니다.

> [!NOTE]
> WinUI 3.0 Preview 1은 WinUI 3.0의 시험판 빌드입니다. [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)에 대한 피드백을 환영합니다.

모든 새 Xaml 기능은 WinUI의 일부로 제공됩니다. OS의 일부로 제공되는 기존 UWP Xaml API는 더 이상 새 기능 업데이트를 받지 않습니다. 그러나 Windows 10 지원 주기에 따라 보안 업데이트 및 중요 수정 사항은 계속 받게 됩니다.

유니버설 Windows 플랫폼에는 Xaml 프레임워크(애플리케이션 및 보안 모델, 미디어 파이프라인, Xbox 및 Windows 10 셀 통합, 광범위한 디바이스 지원 등) 이상이 포함되며 계속 지원됩니다.

![WinUI 3.0 플랫폼 지원](../images/platforms-winui3.png)

> [!Important]
> WinUI 3.0 Preview 1의 목적은 개발자 커뮤니티에서 초기 평가를 받고 피드백을 수집하는 것입니다. 프로덕션 앱에는 사용하면 **안 됩니다**.

### <a name="related-links-for-winui-30"></a>WinUI 3.0에 대한 관련 링크

- [WinUI 3.0 Preview 1(2020년 5월)](winui3/index.md)
- [XAML 컨트롤 갤러리(WinUI 3.0 Preview 1) 앱](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI 리소스

**Github**: WinUI는 Github에 호스팅되는 오픈 소스 프로젝트입니다. [WinUI 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)에서 기능 요청 또는 버그를 제출하고 WinUI 팀과 상호 작용하며 [로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)에서 WinUI 3 이상의 팀 계획을 볼 수 있습니다.

**웹 사이트**: [WinUI 웹 사이트](https://aka.ms/winui)에는 제품 비교, WinUI의 장점, 제품 및 제품 팀과의 관계를 유지하는 방법에 대한 유용한 정보가 포함되어 있습니다.
