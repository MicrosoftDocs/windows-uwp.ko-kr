---
title: WinUI 2.3 릴리스 정보
description: 새로운 기능 및 버그 수정이 포함된 WinUI 2.3 릴리스에 대한 정보입니다.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 63091f927f63a708c5a5e4d41e9d81fd9f528cb1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154787"
---
# <a name="windows-ui-library-23"></a>Windows UI 라이브러리 2.3

WinUI 2.3은 WinUI(Windows UI) 라이브러리의 최신 공식 릴리스입니다.

WinUI는 GitHub의 [Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 호스팅되는 오픈 소스 프로젝트입니다. 이 리포지토리에서 모든 버그 보고서, 기능 요청 및 커뮤니티 코드 기여를 등록하세요.

WinUI 릴리스: [GitHub 릴리스 페이지](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI 패키지는 NuGet 패키지 관리자를 통해 Visual Studio 프로젝트에 추가할 수 있습니다. 자세한 내용은 [Windows UI 라이브러리 시작](../getting-started.md)을 참조하세요.

NuGet 패키지 다운로드: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>새로운 기능

### <a name="progress-bar-visual-refresh"></a>진행률 표시줄 시각적 새로 고침

**ProgressBar**에는 두 가지 시각적 표현이 있습니다.

#### <a name="indeterminate-progress-bar"></a>비활성화 상태 진행률 표시줄

작업이 진행 중임을 보여 주지만 사용자 상호 작용을 차단하지는 않습니다.

![비활성화 상태 진행률 표시줄](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>활성화 상태 진행률 표시줄

알려진 작업량에서 수행된 진행률을 보여 줍니다. 

![활성화 상태 진행률 표시줄](../images/DeterminateProgressBar.gif)

[문서 링크](/windows/uwp/design/controls-and-patterns/progress-controls)

[샘플 링크](/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

**NumberBox**는 숫자를 표시하고 편집하는 데 사용할 수 있는 컨트롤을 나타냅니다. 이는 곱하기, 나누기, 더하기 및 빼기와 같은 기본 수식의 유효성 검사, 증가 단계별 실행 및 컴퓨팅 인라인 계산을 지원합니다.

![NumberBox](../images/NumberBoxGif.gif)

[문서 및 샘플 링크](/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**RadioButtons**는 관련된 RadioButton 요소 그룹을 쉽게 만들 수 있도록 하면서 키보드 및 내레이터/스크린 판독기 기능을 올바르게 지원할 수 있는 새 컨테이너 컨트롤입니다.

![RadioButtons](../images/RadioButtons.png)

[문서 및 샘플 링크](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>예

Xaml 컨트롤 갤러리 샘플 앱에는 WinUI 컨트롤을 사용하기 위한 대화형 데모와 샘플 코드가 포함되어 있습니다.

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)에서 XAML 컨트롤 갤러리 앱 설치

* [GitHub에서 오픈 소스](
https://github.com/Microsoft/Xaml-Controls-Gallery)로도 제공되는 Xaml 컨트롤 갤러리

## <a name="documentation"></a>문서

Windows UI 라이브러리 컨트롤에 대한 방법 문서가 [유니버설 Windows 플랫폼 컨트롤 설명서](/windows/uwp/design/controls-and-patterns/)에 포함되어 있습니다.

API 참조 문서는 [Windows UI 라이브러리 API](/uwp/api/overview/winui/)에 있습니다.