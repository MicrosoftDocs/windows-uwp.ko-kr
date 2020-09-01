---
title: DirectX 게임 프로젝트 템플릿
description: UWP (유니버설 Windows 플랫폼) 및 DirectX 게임을 만들기 위한 템플릿에 대해 알아봅니다.
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 템플릿
ms.localizationpriority: medium
ms.openlocfilehash: 99d68be96858249fdd314857e1fe6d3020ee56e6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159037"
---
# <a name="directx-game-project-templates"></a>DirectX 게임 프로젝트 템플릿



UWP (DirectX and 유니버설 Windows 플랫폼) 템플릿을 사용 하면 게임의 시작 점으로 프로젝트를 빠르게 만들 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소


프로젝트를 만들려면 다음을 수행 해야 합니다.

-   [Microsoft Visual Studio 2015를 다운로드](https://visualstudio.microsoft.com/vs/)합니다. Visual Studio 2015에는 디버깅 도구와 같은 그래픽 프로그래밍을 위한 도구가 있습니다. DirectX 그래픽 및 게임 기능 및 도구에 대 한 개요는 [Visual Studio tools For directx game development](set-up-visual-studio-for-game-development.md)를 참조 하세요.

## <a name="choosing-a-template"></a>템플릿 선택


Visual Studio 2015에는 세 가지 DirectX 및 UWP 템플릿이 포함 되어 있습니다.

-   DirectX 11 앱 (유니버설 Windows)-DirectX 11 앱 (유니버설 Windows) 템플릿은 DirectX 11을 사용 하 여 앱 창에 직접 렌더링 하는 UWP 프로젝트를 만듭니다.
-   DirectX 12 앱 (유니버설 Windows)-DirectX 12 앱 (유니버설 Windows) 템플릿은 DirectX 12를 사용 하 여 앱 창에 직접 렌더링 하는 프로젝트 UWP를 만듭니다.
-   DirectX 11 및 XAML 앱 (유니버설 Windows)-DirectX 11 및 XAML 앱 (유니버설 Windows) 템플릿은 DirectX 11을 사용 하 여 XAML 컨트롤 내에서 렌더링 하는 UWP 프로젝트를 만듭니다. 이 템플릿은 [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)를 사용 하므로 XAML UI 컨트롤을 사용할 수 있습니다. 이를 통해 사용자 인터페이스 요소를 더 쉽게 추가할 수 있지만 XAML 템플릿을 사용 하면 성능이 저하 될 수 있습니다.

선택한 템플릿은 성능 및 사용 하려는 기술에 따라 달라 집니다.

## <a name="template-structure"></a>템플릿 구조


DirectX 유니버설 Windows 템플릿에는 다음 파일이 포함 되어 있습니다.

-   pch. h 및 .pch-미리 컴파일된 헤더 지원.
-   Appxmanifest.xml-앱에 대 한 배포 패키지의 속성입니다.
-   \*.pfx-응용 프로그램에 대 한 인증서입니다.
-   외부 종속성-프로젝트에서 사용 하는 외부 파일에 대 한 링크입니다.
-   \*\*응용 프로그램 자산을 관리 하 고, 응용 프로그램 상태를 업데이트 하 고, 프레임을 렌더링 하는 데 사용할 기본 .h 및 기본 .Cpp 메서드입니다.
-   응용 프로그램에 대 한 app-v 및 App.config-주 진입점입니다. 앱을 Windows 셸에 연결 하 고 응용 프로그램 수명 주기 이벤트를 처리 합니다. 이러한 파일은 DirectX 11 앱 (유니버설 Windows) 및 DirectX 12 앱 (유니버설 Windows) 템플릿에만 표시 됩니다.
-   응용 프로그램에 대 한 응용 프로그램의 .xaml, app.xaml 및 app.xaml 진입점입니다. 앱을 Windows 셸에 연결 하 고 응용 프로그램 수명 주기 이벤트를 처리 합니다. 이러한 파일은 DirectX 11 및 XAML 앱 (유니버설 Windows) 템플릿에만 표시 됩니다.
-   Directxpage, DirectXPage 및 Directxpage-DirectX를 호스트 하는 페이지입니다. 이러한 파일은 DirectX 11 및 XAML 앱 (유니버설 Windows) 템플릿에만 표시 됩니다.
-   Content
    -   Sample3DSceneRenderer 및 Sample3DSceneRenderer-기본 렌더링 파이프라인을 인스턴스화하는 샘플 렌더러입니다.
    -   SampleFpsTextRenderer 및 SampleFpsTextRenderer-Direct2D 및 DirectWrite를 사용 하 여 화면의 오른쪽 아래 모서리에 현재 FPS 값을 렌더링 합니다. 이러한 파일은 DirectX 11 앱 (유니버설 Windows) 및 DirectX 11 및 XAML 앱 (유니버설 Windows) 템플릿에만 표시 됩니다.
    -   SamplePixelShader hlsl-간단한 예제 픽셀 셰이더입니다.
    -   SampleVertexShader hlsl-간단한 예제 꼭 짓 점 셰이더입니다.
    -   ShaderStructures-예제 꼭 짓 점 셰이더에 날짜를 보내는 데 사용 되는 구조체입니다.
-   일반
    -   Stststst.h. h-애니메이션 및 시뮬레이션 타이밍에 대 한 도우미 클래스입니다.
    -   DirectXHelper .h-기타 도우미 함수.
    -   DeviceResources 및 장치 리소스 .cpp-DeviceResources를 소유 하는 응용 프로그램에 대 한 인터페이스를 제공 하 여 장치를 분실 하거나 만들 때 알립니다.
    -   d3dx12-D3DX12 유틸리티 라이브러리를 포함 합니다. 이 파일은 DirectX 12 앱 (유니버설 Windows)에만 표시 됩니다.
-   자산-응용 프로그램에서 사용 하는 로고 및 splashscreen 이미지입니다.

## <a name="next-steps"></a>다음 단계


이제 시작 점이 있으므로 여기에 추가 하 여 게임 개발 기술 자료를 구축 하 고 게임 개발 기술을 Microsoft Store 합니다.

기존 게임을 이식 하는 경우에는 다음 항목을 참조 하세요.

-   [OpenGL ES 2.0에서 Direct3D 11.1로의 포트](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [DirectX 9에서 유니버설 Windows 플랫폼 포트로](porting-your-directx-9-game-to-windows-store.md)

새 DirectX 게임을 만드는 경우 다음 항목을 참조 하세요.

-   [DirectX를 사용 하 여 간단한 UWP 게임 만들기](tutorial--create-your-first-uwp-directx-game.md)
-   [C + + 및 DirectX에서 대리석, 유니버설 Windows 플랫폼 게임 개발](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)