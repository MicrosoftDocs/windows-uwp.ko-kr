---
author: mtoepke
title: DirectX 게임 프로젝트 템플릿
description: UWP(유니버설 Windows 플랫폼) 및 DirectX 게임을 만드는 템플릿에 대해 알아봅니다.
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 템플릿
ms.localizationpriority: medium
ms.openlocfilehash: d27e4bb0d643b859958f887133a128572ca31225
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6844582"
---
# <a name="directx-game-project-templates"></a>DirectX 게임 프로젝트 템플릿



DirectX 및 UWP(유니버설 Windows 플랫폼) 템플릿을 사용하여 게임의 시작 지점으로 프로젝트를 신속하게 만들 수 있습니다.

## <a name="prerequisites"></a>필수 조건


프로젝트를 만들려면 다음을 수행해야 합니다.

-   [Microsoft Visual Studio2015를 다운로드](https://www.visualstudio.com/vs-2015-product-editions)합니다. 시각적 Studio2015에 그래픽 디버깅 도구 등의 프로그래밍에 대 한 도구가 있습니다. DirectX 그래픽 및 게임 기능과 도구에 대한 개요를 보려면 [DirectX 게임 개발을 위한 Visual Studio 도구](set-up-visual-studio-for-game-development.md)를 참조하세요.

## <a name="choosing-a-template"></a>템플릿 선택


시각적 Studio2015 세 가지 DirectX 및 UWP 템플릿이 포함 됩니다.

-   DirectX 11 앱(유니버설 Windows) - DirectX 11 앱(유니버설 Windows) 템플릿은 DirectX 11을 사용하여 앱 창에 직접 렌더링되는 UWP 프로젝트를 만듭니다.
-   DirectX 12 앱(유니버설 Windows) - DirectX 12 앱(유니버설 Windows) 템플릿은 DirectX 12를 사용하여 앱 창에 직접 렌더링되는 UWP 프로젝트를 만듭니다.
-   DirectX 11 및 XAML 앱(유니버설 Windows) - DirectX 11 및 XAML 앱(유니버설 Windows) 템플릿은 DirectX 11을 사용하여 XAML 컨트롤 내에 렌더링되는 UWP 프로젝트를 만듭니다. 이 템플릿에서는 XAML UI 컨트롤을 사용할 수 있도록 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)을 사용합니다. 따라서 사용자 인터페이스 요소를 더 쉽게 추가할 수 있지만, XAML 템플릿을 사용하면 성능이 저하될 수 있습니다.

어떤 템플릿을 선택할지는 성능과 사용하려는 기술에 따라 다릅니다.

## <a name="template-structure"></a>템플릿 구조


DirectX 유니버설 Windows 템플릿은 다음 파일을 포함합니다.

-   pch.h 및 pch.cpp - 사전 컴파일된 헤더 지원
-   Package.appxmanifest - 앱 배포 패키지 속성
-   \*.pfx - 응용 프로그램 인증서
-   외부 종속성 - 프로젝트에서 사용하는 외부 파일에 대한 링크
-   \*Main.h 및 \*Main.cpp - 응용 프로그램 자산을 관리하고, 응용 프로그램 상태를 업데이트하며, 프레임을 렌더링하는 메서드
-   App.h 및 App.cpp - 응용 프로그램의 기본 진입점 앱을 Windows 셸과 연결하고 응용 프로그램 수명 주기 이벤트를 처리합니다. 이러한 파일은 DirectX 11 앱(유니버설 Windows) 및 DirectX 12 앱(유니버설 Windows) 템플릿에만 표시됩니다.
-   App.xaml, App.xaml.cpp 및 App.xaml.h - 응용 프로그램의 기본 진입점. 앱을 Windows 셸과 연결하고 응용 프로그램 수명 주기 이벤트를 처리합니다. 이러한 파일은 DirectX 11 및 XAML 앱(유니버설 Windows) 템플릿에만 표시됩니다.
-   DirectXPage.xaml, DirectXPage.xaml.cpp 및 DirectXPage.xaml.h - DirectX SwapChainPanel을 호스팅하는 페이지. 이러한 파일은 DirectX 11 및 XAML 앱(유니버설 Windows) 템플릿에만 표시됩니다.
-   콘텐츠
    -   Sample3DSceneRenderer.h 및 Sample3DSceneRenderer.cpp - 기본 렌더링 파이프라인을 인스턴스화하는 샘플 렌더러
    -   SampleFpsTextRenderer.h 및 SampleFpsTextRenderer.cpp - Direct2D 및 DirectWrite를 사용하여 화면 오른쪽 아래에 있는 현재 FPS 값을 렌더링합니다. 이러한 파일은 DirectX 11 앱(유니버설 Windows)과 DirectX 11 및 XAML 앱(유니버설 Windows) 템플릿에만 표시됩니다.
    -   SamplePixelShader.hlsl - 간단한 예제 픽셀 셰이더
    -   SampleVertexShader.hlsl - 간단한 예제 꼭짓점 셰이더
    -   ShaderStructures.h - 예제 꼭짓점 셰이더로 날짜를 보내는 데 사용되는 구조
-   일반
    -   StepTimer.h - 애니메이션 및 시뮬레이션 타이밍에 대한 도우미 클래스
    -   DirectXHelper.h - 기타 도우미 함수
    -   DeviceResources.h 및 Device Resources.cpp - DeviceResources를 소유한 응용 프로그램에 장치 손실 또는 생성에 대한 알림을 받을 수 있는 인터페이스를 제공합니다.
    -   d3dx12.h - D3DX12 유틸리티 라이브러리를 포함합니다. 이 파일은 DirectX 12 앱(유니버설 Windows)에만 표시됩니다.
-   자산 - 응용 프로그램에서 사용되는 로고 및 시작 화면 이미지

## <a name="next-steps"></a>다음 단계


시작점이 있으므로 여기에 추가하여 게임 개발 지식과 Microsoft Store 게임 개발 기술을 쌓으세요.

기존 게임을 포팅하려면 다음 항목을 참조하세요.

-   [OpenGL ES 2.0에서 Direct3D 11.1로 포팅](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [DirectX 9에서 유니버설 Windows 플랫폼으로 포팅](porting-your-directx-9-game-to-windows-store.md)

DirectX 게임을 새로 만드는 경우 다음 항목을 참조하세요.

-   [DirectX로 간단한 UWP 게임 만들기](tutorial--create-your-first-uwp-directx-game.md)
-   [C++ 및 DirectX로 유니버설 Windows 플랫폼 게임 Marble Maze 개발](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)