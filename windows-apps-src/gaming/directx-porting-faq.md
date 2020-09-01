---
title: DirectX 11 포팅 FAQ
description: 유니버설 Windows 플랫폼 (UWP)에 게임을 이식 하는 방법에 대 한 자주 묻는 질문에 대 한 답변입니다.
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 81fc8e9cd762c5fc0bb602e32907c40ca5c60e46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163137"
---
# <a name="directx-11-porting-faq"></a>DirectX 11 포팅 FAQ




유니버설 Windows 플랫폼 (UWP)에 게임을 이식 하는 방법에 대 한 자주 묻는 질문에 대 한 답변입니다.

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>게임을 이식 하는 것이 API 메서드에 대 한 검색 및 바꾸기 작업 집합으로 이동 하 고 있거나 보다 자세한 정보 포팅 포팅 프로세스를 계획 해야 하나요?


Direct3D 11은 Direct3D 9에서 중요 한 업그레이드입니다. 가상화 된 그래픽 어댑터에 대 한 별도의 Api와 해당 컨텍스트 뿐만 아니라 장치 리소스에 대 한 새로운 다형성 계층이 포함 된 여러 패러다임 이동이 있습니다. 게임은 기본적으로 동일한 방식으로 그래픽 하드웨어를 사용할 수 있지만 새 Direct3D 11 API 아키텍처에 대해 알아보고 올바른 API 구성 요소를 사용 하도록 그래픽 코드의 각 부분을 업데이트 해야 합니다. [포팅 개념 및 고려 사항을](porting-considerations.md)참조 하세요.

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>의 새 장치 컨텍스트는 무엇 인가요? Direct3d 11 장치, 장치 컨텍스트 또는 둘 다를 사용 하 여 Direct3D 9 장치를 교체 해야 하나요?


이제 Direct3D 장치는 비디오 메모리에서 리소스를 만드는 데 사용 되는 반면 장치 컨텍스트는 파이프라인 상태를 설정 하 고 렌더링 명령을 생성 하는 데 사용 됩니다. 자세한 내용은 [Direct3D 9 이후 가장 중요 한 변경 사항은 무엇 인가요?](understand-direct3d-11-1-concepts.md) 를 참조 하세요.

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>UWP에 대 한 게임 타이머를 업데이트 해야 하나요?


[**QueryPerformanceFrequency**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancefrequency)와 함께 [**queryperformancecounter**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter)는 아직 UWP 앱에 대 한 게임 타이머를 구현 하는 가장 좋은 방법입니다.

타이머 및 UWP 앱 수명 주기를 사용 하는 nuance에 대해 알고 있어야 합니다. 게임이 마지막으로 재생 된 시간부터의 시간에 스냅숏이 다시 시작 되므로 일시 중단/다시 시작은 플레이어와는 다릅니다. 많은 시간이 경과한 경우 (예: 몇 주) 일부 게임 타이머 구현이 정상적으로 동작 하지 않을 수 있습니다. 게임을 다시 시작할 때 앱 수명 주기 이벤트를 사용 하 여 타이머를 다시 설정할 수 있습니다.

여전히 RDTSC 명령을 사용 하는 게임을 업그레이드 해야 합니다. [게임 타이밍 및 다중 코어 프로세서](/windows/desktop/DxTechArts/game-timing-and-multicore-processors)를 참조 하세요.

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>게임 코드는 D3DX 및 DXUT를 기반으로 합니다. 내 코드를 마이그레이션하는 데 도움이 되는 사용 가능한 항목이 있나요?


[DirectX Tool Kit (DirectXTK)](https://github.com/Microsoft/DirectXTK) community 프로젝트는 Direct3D 11에서 사용할 도우미 클래스를 제공 합니다.

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>데스크톱 및 Microsoft Store에 대 한 코드 경로를 유지 어떻게 할까요?


Walbourn의 문서 시리즈는 [게임에 대 한 이중 사용 코딩 기술](https://blogs.msdn.com/b/chuckw/archive/2012/09/17/dual-use-coding-techniques-for-games.aspx) 에서 데스크톱과 Microsoft Store 코드 경로 간의 코드 공유에 대 한 지침을 제공 합니다.

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>내 DirectX UWP 앱에서 이미지 리소스를 로드할 어떻게 할까요? 있나요?


이미지를 로드 하는 데는 두 가지 API 경로가 있습니다.

-   콘텐츠 파이프라인은 이미지를 Direct3D 텍스처 리소스로 사용 되는 DDS 파일로 변환 합니다. [게임 또는 앱에서 3 차원 자산 사용](/visualstudio/designers/using-3-d-assets-in-your-game-or-app?view=vs-2015)을 참조 하세요.
-   [Windows 이미징 구성 요소](/windows/desktop/wic/-wic-lh) 는 다양 한 형식에서 이미지를 로드 하는 데 사용할 수 있으며, Direct2D 비트맵 및 Direct3D 텍스처 리소스에도 사용할 수 있습니다.

[DirectXTK](https://github.com/Microsoft/DirectXTK) 또는 [DirectXTex](https://github.com/Microsoft/DirectXTex)에서 DDSTextureLoader 및 WICTextureLoader를 사용할 수도 있습니다.

## <a name="where-is-the-directx-sdk"></a>DirectX SDK 위치


DirectX SDK는 Windows SDK의 일부로 포함되어 있습니다. Windows SDK와 분리 된 최신 DirectX SDK는 6 월 2010입니다. Direct3D 샘플은 나머지 Windows 앱 샘플과 함께 코드 갤러리에 있습니다.

## <a name="what-about-directx-redistributables"></a>DirectX 재배포 가능 패키지는 어떻게 되나요?


Windows SDK의 대부분 구성 요소가 지원 되는 OS 버전에 이미 포함 되어 있거나 DLL 구성 요소 (예: DirectXMath)가 없습니다. UWP 앱에서 사용할 수 있는 모든 Direct3D API 구성 요소는 게임에 이미 사용할 수 있습니다. 재배포할 필요가 없습니다.

Win32 데스크톱 응용 프로그램은 여전히 DirectSetup을 사용 하므로, 게임의 데스크톱 버전도 업그레이드 하는 경우 [게임 개발자를 위한 Direct3D 11 배포](/windows/desktop/direct3darticles/direct3d11-deployment)를 참조 하세요.

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>효과를 벗어나기 전에 바탕 화면 코드를 DirectX 11로 업데이트할 수 있는 방법이 있나요?


[Direct3D 11 업데이트에 대 한 영향](https://github.com/Microsoft/FX11)을 참조 하세요. 효과 11은 레거시 DirectX SDK 헤더에 대 한 종속성을 제거 하는 데 도움이 됩니다. 포팅 지원으로 사용 하기 위한 것 이며 데스크톱 응용 프로그램 에서만 사용할 수 있습니다.

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>DirectX 8 게임을 UWP에 이식할 수 있는 경로가 있나요?


예:

-   [Direct3D 9로 변환을](/windows/desktop/direct3d9/converting-to-directx-9)참조 하세요.
-   게임에 고정 파이프라인의 남아 없는지 확인 합니다. [사용 되지 않는 기능](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)을 참조 하세요.
-   그런 다음 DirectX 9 포팅 경로: [포트를 D3D 9에서 UWP로](walkthrough--simple-port-from-direct3d-9-to-11-1.md)이동 합니다.

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>DirectX 10 또는 11 게임을 UWP로 이식할 수 있나요?


DirectX 10. x 및 11 데스크톱 게임은 UWP로 쉽게 이식할 수 있습니다. [Direct3D 11로 마이그레이션을](/windows/desktop/direct3d11/d3d11-programming-guide-migrating)참조 하세요.

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>다중 모니터 시스템에서 올바른 디스플레이 장치를 선택 어떻게 할까요??


사용자가 앱이 표시 되는 모니터를 선택 합니다. Windows에서 첫 번째 매개 변수가 **nullptr**로 설정 된 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 를 호출 하 여 올바른 어댑터를 제공 하도록 합니다. 그런 다음 장치의 [**Idxgid 장치 인터페이스**](/windows/desktop/api/dxgi/nn-dxgi-idxgidevice)를 가져오고, [**getadapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) 를 호출 하 고, DXGI 어댑터를 사용 하 여 스왑 체인을 만듭니다.

## <a name="how-do-i-turn-on-antialiasing"></a>앤티엘리어싱을 설정 어떻게 할까요?


앤티 앨리어싱 (다중 샘플링)은 Direct3D 장치를 만들 때 사용할 수 있습니다. [**CheckMultisampleQualityLevels**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkmultisamplequalitylevels)를 호출 하 여 다중 샘플링 지원을 열거 한 다음 [**CreateSurface**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-createsurface)를 호출할 때 [**DXGI \_ SAMPLE \_ DESC 구조**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) 에서 다중 샘플 옵션을 설정 합니다.

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>내 게임은 다중 스레딩 및/또는 지연 렌더링을 사용 하 여 렌더링 됩니다. Direct3D 11에 대해 알아야 할 사항은 무엇 인가요?


시작 하려면 [Direct3D 11의 다중 스레딩 소개](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro) 를 참조 하세요. 주요 차이점 목록은 [Direct3D 버전 간의 스레딩 차이점](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-differences)을 참조 하세요. 지연 된 렌더링은 *즉각적인 컨텍스트*대신 장치 *지연 컨텍스트* 를 사용 합니다.

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>Direct3D 9부터 프로그래밍 가능 파이프라인에 대 한 자세한 내용은 어디서 확인할 수 있나요?


다음 항목을 참조 하세요.

-   [HLSL 프로그래밍 가이드](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-pguide)
-   [Direct3D 10 질문과 대답](/windows/desktop/DxTechArts/direct3d10-frequently-asked-questions)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>모델에 대 한 .x 파일 형식 대신 어떤 항목을 사용 해야 하나요?


.X 파일 형식에 대 한 공식적인 대체가 없지만 대부분의 샘플에서는 SDKMesh 형식을 활용 합니다. Visual Studio에는 Visual Studio 3D 시작 키트의 코드를 사용 하 여 로드 하거나 [DirectXTK](https://github.com/Microsoft/DirectXTK)를 사용 하 여 로드할 수 있는 여러 가지 인기 형식을 CMO 파일로 컴파일하는 [콘텐츠 파이프라인](/visualstudio/designers/using-3-d-assets-in-your-game-or-app?view=vs-2015) 도 있습니다.

## <a name="how-do-i-debug-my-shaders"></a>셰이더를 디버깅할 어떻게 할까요? 있나요?


Microsoft Visual Studio 2015에는 DirectX 그래픽용 진단 도구가 포함 되어 있습니다. [DirectX 그래픽 디버그](/visualstudio/debugger/visual-studio-graphics-diagnostics?view=vs-2015)를 참조 하세요.

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>*X* 함수에 해당 하는 Direct3D 11은 무엇 인가요?


Directx 11 기능 지도에 제공 된 [함수 매핑을](feature-mapping.md#function-mapping) Directx 11 api에 참조 하세요.

##  <a name="what-is-the-dxgi_format-equivalent-of-y-surface-format"></a>\_ *Y* 표면 형식에 해당 하는 DXGI 형식은 무엇입니까?


Directx 11의 기능 지도에서 DirectX 11 Api에 제공 된 [surface 형식 매핑을](feature-mapping.md#surface-format-mapping) 참조 하세요.

 

 