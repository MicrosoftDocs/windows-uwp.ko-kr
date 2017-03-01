---
author: mtoepke
title: "게임 및 DirectX"
description: "UWP(유니버설 Windows 플랫폼)에서는 게임을 만들어 배포하고 수익을 창출할 수 있는 새로운 기회를 제공합니다. 새 게임을 시작하거나 기존 게임을 포팅하는 방법에 대해 알아봅니다."
ms.assetid: 4073b835-c900-4ff2-9fc5-da52f9432a1f
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 게임, directx"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c3a24c15f61204f3ecbc0587cf54e1da69ee712c
ms.lasthandoff: 02/07/2017

---

# <a name="games-and-directx"></a>게임 및 DirectX


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

UWP(유니버설 Windows 플랫폼)에서는 게임을 만들어 배포하고 수익을 창출할 수 있는 새로운 기회를 제공합니다. 새 게임을 시작하거나 기존 게임을 포팅하는 방법에 대해 알아봅니다.

| 항목 | 설명 |
|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Windows 10 게임 개발 가이드](e2e.md) | UWP 게임 개발용 리소스 및 정보에 대한 종단 간 가이드입니다. |
| [유니버설 Windows 플랫폼 앱용 게임 기술](game-development-platform-guide.md) | 이 가이드에서는 UWP 게임을 개발하는 데 사용할 수 있는 기술에 대해 알아봅니다. |
| [게임용 프로젝트 템플릿 및 도구](prepare-your-dev-environment-for-windows-store-directx-game-development.md) | UWP용 DirectX 게임 프로그래밍을 시작하는 데 필요한 사항을 보여 줍니다. |
| [앱 개체 및 DirectX](about-the-metro-style-user-interface-and-directx.md) | DirectX로 작성된 UWP 게임은 Windows UI 사용자 인터페이스 요소 및 개체를 거의 사용하지 않습니다. 더 정확히 말하면 그러한 요소 및 개체는 Windows 런타임 스택의 하위 수준에서 실행되므로 앱 개체에 직접 액세스하여 상호 작용하는 보다 본질적인 방식으로 사용자 인터페이스 프레임워크와 상호 작용해야 합니다. 이러한 상호 작용이 발생하는 경우와, DirectX 개발자로서 UWP 앱 개발에 이 모델을 효율적으로 사용하는 방법에 대해 학습합니다. |
| [앱 시작 및 다시 시작](launching-and-resuming-apps-directx-and-cpp.md) | UWP DirectX 앱을 시작, 일시 중단 및 다시 시작하는 방법을 알아봅니다. |
| [DirectX 게임의 2D 그래픽](working-with-2d-graphics-in-your-directx-game.md) | 2D 비트맵 그래픽 및 효과의 용도와 게임에서 사용하는 방법에 대해 검토합니다.다. |
| [DirectX 게임의 기본 3D 그래픽](an-introduction-to-3d-graphics-with-directx.md) | DirectX 프로그래밍을 사용해 3D 그래픽의 기본 개념을 구현하는 방법을 소개합니다. |
| [DirectX 게임에 리소스 로드](load-a-game-asset.md) | 대부분의 게임은 특정 시점에 로컬 저장소 또는 몇몇 다른 데이터 스트림에서 셰이더, 텍스처, 미리 정의된 메시 또는 기타 그래픽 데이터 등, 리소스와 자산을 로드합니다. 여기서는 UWP 게임에 사용하기 위해 이러한 파일을 로드할 때 고려해야 할 부분에 대해 심도 있게 다룹니다. |
| [DirectX로 간단한 UWP 게임 만들기](tutorial--create-your-first-metro-style-directx-game.md) | 이 자습서 집합에서는 DirectX 및 C++를 사용하여 기본 UWP 게임을 만드는 방법을 알아봅니다. 아트 및 메시 같은 자산 로드, 주 게임 루프 만들기, 간단한 렌더링 파이프라인 구현, 사운드 및 컨트롤 추가 등에 대한 프로세스를 포함하여 게임의 모든 주요 부분에 대해 다루고, |
| [C++ 및 DirectX로 유니버설 Windows 플랫폼 게임 Marble Maze 개발](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md) | 이 설명서 섹션에서는 DirectX 및 Visual C++를 사용하여 3D UWP 게임을 만드는 방법을 설명합니다. 이 설명서에서는 태블릿 등의 새로운 폼 팩터를 수용하고 기존의 데스크톱 및 랩톱 PC에서도 작동하는 3D 게임 Marble Maze를 만드는 방법을 보여 줍니다. |
| [화면 방향 지원](supporting-screen-rotation-directx-and-cpp.md) | 여기서는 Windows 10 디바이스의 그래픽 하드웨어가 효율적이면서 효과적으로 사용되도록 UWP DirectX 앱에서 화면 회전을 처리하는 모범 사례에 대해 설명합니다. |
| [게임의 오디오](working-with-audio-in-your-directx-game.md) | 음악 및 사운드를 개발하여 DirectX 게임에 통합하는 방법 및 오디오 신호를 처리하여 동적 및 위치 사운드를 만드는 방법을 알아봅니다. |
| [게임용 터치 컨트롤](tutorial--adding-touch-controls-to-your-directx-game.md) | DirectX를 사용하는 UWP C++ 게임에 기본 터치 컨트롤을 추가하는 방법을 알아봅니다. 터치 기반 컨트롤을 추가하여 Direct3D 환경에서 고정 평면 카메라를 이동하는 방법을 보여 줍니다. 이러한 Direct3D 환경에서는 손가락이나 스타일러스로 끌면 카메라 관점이 이동합니다. |
| [게임용 이동-보기 컨트롤](tutorial--adding-move-look-controls-to-your-directx-game.md) | 기존 마우스 및 키보드 이동-보기 컨트롤(마우스 보기 컨트롤이라고도 함)을 DirectX 게임에 추가하는 방법을 알아봅니다. |
| [상대 마우스 이동](relative-mouse-movement.md) | 시스템 커서를 사용하지 않고 절대 화면 좌표를 반환하지 않는 상대 마우스 컨트롤을 추가하는 방법에 대해 설명합니다. 대신 이 컨트롤은 마우스 이동 사이의 픽셀 델타를 추적합니다. |
| [입력 및 렌더링 루프 최적화](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) | 입력 대기 시간은 게임 환경에 큰 영향을 줄 수 있으며, 최적화하면 게임에 더 세련된 느낌을 줄 수 있습니다. 또한 입력 이벤트를 적절하게 최적화하면 배터리 수명을 향상시킬 수 있습니다. 올바른 [CoreDispatcher](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) 입력 이벤트 처리 옵션을 선택하여 게임에서 입력을 가능한 한 매끄럽게 처리하도록 하는 방법을 알아봅니다. |
| [스왑 체인 크기 조정 및 오버레이](multisampling--scaling--and-overlay-swap-chains.md) | 모바일 디바이스에서 보다 신속한 렌더링을 위해 크기 조정된 스왑 체인을 만들고 오버레이 스왑 체인(사용 가능한 경우)을 사용하여 시각적 품질을 향상시키는 방법을 알아봅니다. |
| [DXGI 1.3 스왑 체인으로 대기 시간 단축](reduce-latency-with-dxgi-1-3-swap-chains.md) | DXGI 1.3을 사용하여 스왑 체인이 새로운 프레임 렌더링을 시작할 적절한 시간을 신호로 보낼 때까지 기다려 효과적인 프레임 지연을 줄입니다. |
| [UWP 앱의 다중 샘플링](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md) | Direct3D를 사용하는 UWP 앱에서 다중 샘플링을 사용하는 방법에 대해 알아봅니다. |
| [Direct3D 11에서 디바이스 제거 시나리오 처리](handling-device-lost-scenarios.md) | 이 항목에서는 그래픽 어댑터가 제거되거나 다시 초기화될 때 Direct3D 및 DXGI 디바이스 인터페이스 체인을 다시 만드는 방법에 대해 설명합니다. |
| [게임용 비동기 프로그래밍](asynchronous-programming-directx-and-cpp.md) | 이 항목에서는 비동기 프로그래밍과 스레딩을 DirectX와 함께 사용할 때 고려할 여러 사항에 대해 설명합니다. |
| [게임의 네트워킹](work-with-networking-in-your-directx-game.md) | 네트워킹 기능을 개발하고 DirectX 게임에 통합하는 방법을 알아봅니다. |
| [게임의 접근성](accessibility-for-games.md) | 게임의 접근성을 높이는 방법을 알아봅니다. |
| [게임의 클라우드](cloud-for-games.md) | 게임 개발에 클라우드 기술을 사용하는 방법을 알아봅니다. |
| [게임의 수익 창출](monetization-for-games.md) | 게임을 통해 수익을 창출할 수 있는 방법에 대해 알아봅니다. |
| [DirectX 및 XAML interop](directx-and-xaml-interop.md) | UWP 게임에서 XAML(Extensible Application Markup Language)과 Microsoft DirectX를 함께 사용할 수 있습니다. |
| [게임 패키지 작성](package-your-windows-store-directx-game.md) | 대규모 UWP 게임, 특히 지역별 자산이나 선택적 기능 HD 자산을 사용하여 여러 언어를 지원하는 게임은 크기가 쉽게 급증할 수 있습니다. 이 항목에서는 고객이 실제로 필요한 리소스만 받을 수 있도록 앱 패키지와 앱 번들을 사용하여 앱을 사용자 지정하는 방법을 알아봅니다. |
| [개념 승인](concept-approval.md) | 제품이 Xbox에서 실행되거나 Xbox Live를 사용하는 경우 필요한 개념 승인을 위해 제품을 제출하는 방법을 알아봅니다. |
| [게임 포팅 가이드](porting-guides.md) | 기존 게임을 Direct3D 11, UWP 및 Windows 10으로 포팅하는 방법에 대한 가이드를 제공합니다. |
| [게임 프로그래밍 리소스](additional-directx-game-programming-resources.md) | Windows의 게임 프로그래밍에 대한 자세한 내용은 다음 리소스를 확인하세요. |

 

> **참고**  
이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

게임 개발 개요 및 자습서를 가장 효과적으로 사용하려면 다음 주제를 잘 알고 있어야 합니다.

-   Microsoft C++ with Component Extensions(C++/CX). 자동 참조 카운트를 통합하는 Microsoft C++의 업데이트이며 DirectX 11.1 이상 버전을 사용하여 UWP 게임을 개발하기 위한 언어입니다.
-   기본 그래픽 프로그래밍 용어
-   기본 Windows 프로그래밍 개념
-   Direct3D 9 또는 11 API에 대한 기본적인 지식

 

 





