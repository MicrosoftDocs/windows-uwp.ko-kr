---
title: DirectX 11 포팅 FAQ
description: UWP(유니버설 Windows 플랫폼)로 게임을 포팅하는 방법에 대한 질문과 대답입니다.
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 31c165d47beea8ee0e31a3213bdd0dbf0c2bc3d7
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8895534"
---
# <a name="directx-11-porting-faq"></a>DirectX 11 포팅 FAQ




UWP(유니버설 Windows 플랫폼)로 게임을 포팅하는 방법에 대한 질문과 대답입니다.

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>게임 포팅은 API 메서드에 대한 일련의 검색 및 바꾸기 작업인가요 아니면 보다 철저한 포팅 프로세스를 계속해야 할까요?


Direct3D 11은 Direct3D 9로부터의 중요한 업그레이드입니다. 가상화된 그래픽 어댑터를 위한 별도의 API 및 해당 컨텍스트와 장치 리소스에 대한 새로운 다형성 계층 등 몇 가지 패러다임 변화가 있습니다. 게임은 본질적으로 동일한 방식으로 그래픽 하드웨어를 계속 사용할 수 있지만, 새로운 Direct3D 11 API 아키텍처에 대해 알아보고 올바른 API 구성 요소를 사용하도록 그래픽 코드의 각 부분을 업데이트해야 합니다. [포팅 개념 및 고려 사항](porting-considerations.md)을 참조하세요.

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>새 디바이스 컨텍스트는 무엇을 위한 것인가요? Direct3D 9 장치를 Direct3D 11 장치, 디바이스 컨텍스트 또는 둘 다로 바꾸어야 할까요?


Direct3D 장치는 비디오 메모리에 리소스를 만드는 데 사용하는 반면, 디바이스 컨텍스트는 파이프라인 상태를 설정하고 렌더링 명령을 생성하는 데 사용합니다. 자세한 내용은 [Direct3D 9 이후에 가장 중요한 변경 내용은 무엇인가요?](understand-direct3d-11-1-concepts.md)를 참조하세요.

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>UWP에 대한 게임 타이머를 업데이트해야 할까요?


[**QueryPerformanceCounter**](https://msdn.microsoft.com/library/windows/desktop/ms644904)는 [**QueryPerformanceFrequency**](https://msdn.microsoft.com/library/windows/desktop/ms644905)와 함께 여전히 UWP 앱에 대한 게임 타이머를 구현하기 가장 좋은 방법이며,

타이머와 UWP 앱 수명 주기의 미묘한 차이를 알고 있어야 합니다. 일시 중단/다시 시작은 플레이어가 데스크톱 게임을 다시 실행하는 것과는 다른데, 이는 게임이 마지막으로 플레이된 때로부터 시간이 지나 스냅숏을 다시 시작하기 때문입니다. 많은 시간이 경과한 경우(예: 몇 주) 일부 게임 타이머 구현은 정상적으로 동작하지 않을 수 있습니다. 앱 수명 주기 이벤트를 사용하여 게임이 다시 시작될 때 타이머를 초기화할 수 있습니다.

여전히 RDTSC 명령을 사용하는 게임은 업그레이드 해야 합니다. [게임 타이밍 및 멀티 코어 프로세서](https://msdn.microsoft.com/library/windows/desktop/ee417693)를 참조하세요.

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>게임이 D3DX 및 DXUT를 기반으로 합니다. 코드를 마이그레이션하는 데 사용할 수 있는 것이 있나요?


[DirectXTK(DirectX 도구 키트)](http://go.microsoft.com/fwlink/p/?LinkID=248929) 커뮤니티 프로젝트는 Direct3D 11에 사용할 수 있는 도우미 클래스를 제공합니다.

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>데스크톱 및 Microsoft Store에 대 한 코드 경로 유지 관리 하는 있나요?


Chuck Walbourn의 문서 시리즈 [게임용 이중 용도 코딩 기술](http://go.microsoft.com/fwlink/p/?LinkID=286210) 이라는 데스크톱 및 Microsoft 스토어 코드 경로 간의 코드 공유에 대 한 지침을 제공 합니다.

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>DirectX UWP 앱에 이미지 리소스를 로드하는 방법은 무엇인가요?


이미지를 로드하기 위한 API 경로에는 다음과 같은 두 가지가 있습니다.

-   콘텐츠 파이프라인에서, Direct3D 텍스처 리소스로 사용되는 DDS 파일로 이미지를 변환합니다. [게임 또는 앱에 3-D 자산 사용](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)을 참조하세요.
-   [Windows Imaging Component](https://msdn.microsoft.com/library/windows/desktop/ee719902)는 다양한 형식의 이미지를 로드하는 데 사용할 수 있으며, Direct3D 텍스처 리소스 및 Direct2D 비트맵에 사용할 수 있습니다.

또한 [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) 또는 [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926)의 DDSTextureLoader 및 WICTextureLoader를 사용할 수 있습니다.

## <a name="where-is-the-directx-sdk"></a>DirectX SDK 위치


DirectX SDK는 Windows SDK의 일부로 포함되어 있습니다. Windows SDK에서 분리된 최신 DirectX SDK는 2010년 6월에 출시되었습니다. Direct3D 샘플은 나머지 Windows 앱 샘플과 함께 코드 갤러리에 있습니다.

## <a name="what-about-directx-redistributables"></a>DirectX 재배포 가능 구성 요소는 무엇인가요?


Windows SDK의 대부분의 구성 요소는 지원되는 OS 버전에 이미 포함되어 있거나, DLL 구성 요소(예: DirectXMath)를 포함하고 있지 않습니다. UWP 앱에서 사용할 수 있는 모든 Direct3D API 구성 요소는 이미 게임에 사용할 수 있습니다. 해당 구성 요소를 재배포할 필요가 없습니다.

Win32 데스크톱 응용 프로그램에서는 여전히 DirectSetup을 사용하므로, 게임의 데스크톱 버전도 업그레이드하려는 경우 [게임 개발자용 Direct3D 11 배포](https://msdn.microsoft.com/library/windows/desktop/ee416644)를 참조하세요.

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>데스크톱 코드를 효과에서 이전시키기 전에 DirectX 11로 업데이트할 수 있는 방법이 있나요?


[Direct3D 11 업데이트에 대한 효과](http://go.microsoft.com/fwlink/p/?LinkId=271568)를 참조하세요. 효과 11은 레거시 DirectX SDK 헤더에 대한 종속성을 없애 줍니다. 효과 11은 포팅 보조 기능으로 사용하도록 고안되었으며, 데스크톱 앱에만 사용할 수 있습니다.

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>DirectX 8 게임을 UWP로 포팅하는 경로가 있나요?


예:

-   [Direct3D 9로 변환](https://msdn.microsoft.com/library/windows/desktop/bb204851)을 읽어 봅니다.
-   게임에 고정된 파이프라인의 나머지가 있는지 확인합니다([사용되지 않는 기능](https://msdn.microsoft.com/library/windows/desktop/cc308047) 참조).
-   그런 다음 DirectX 9 포팅 경로를 사용합니다([D3D 9에서 UWP로 포팅](walkthrough--simple-port-from-direct3d-9-to-11-1.md)).

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>DirectX 10 또는 11 게임을 UWP로 포팅할 수 있나요?


DirectX 10.x 및 11 데스크톱 게임은 쉽게 UWP로 포팅됩니다. [Direct3D 11로 마이그레이션](https://msdn.microsoft.com/library/windows/desktop/ff476190)을 참조하세요.

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>다중 모니터 시스템에서 적합한 디스플레이 장치를 선택하는 방법은 무엇인가요?


사용자가 앱이 표시되는 모니터를 선택합니다. 첫 번째 매개 변수를 **nullptr**로 설정하여 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)를 호출함으로써 Windows가 올바른 어댑터를 제공하도록 합니다. 그런 다음 장치의 [**IDXGIDevice interface**](https://msdn.microsoft.com/library/windows/desktop/bb174527)를 가져오고 [**GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531)를 호출한 다음 DXGI 어댑터를 사용하여 스왑 체인을 만듭니다.

## <a name="how-do-i-turn-on-antialiasing"></a>앤티앨리어싱을 켜는 방법은 무엇인가요?


Direct3D 장치를 만들 때 앤티앨리어싱(다중 샘플링)을 사용하도록 설정할 수 있습니다. [**CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499)를 호출하여 다중 샘플링 지원을 열거한 다음, [**CreateSurface**](https://msdn.microsoft.com/library/windows/desktop/bb174530)를 호출할 때 [**DXGI\_SAMPLE\_DESC structure**](https://msdn.microsoft.com/library/windows/desktop/bb173072)에서 다중 샘플링 옵션을 설정합니다.

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>다중 스레딩 및/또는 지연 렌더링을 사용하여 게임을 렌더링합니다. Direct3D 11에 대해 알아야 할 사항은 무엇인가요?


시작하려면 [Direct3D 11에서의 다중 스레딩 소개](https://msdn.microsoft.com/library/windows/desktop/ff476891)를 방문하세요. 주요 차이점 목록은 [Direct3D 버전 간 스레딩 차이점](https://msdn.microsoft.com/library/windows/desktop/ff476890)을 참조하세요. 지연 렌더링에서는 *즉각적인 컨텍스트* 대신 장치 *지연된 컨텍스트*를 사용합니다.

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>Direct3D 9 이후 프로그램 가능 파이프라인에 대한 자세한 내용은 어디에서 살펴볼 수 있나요?


다음 항목을 참조하세요.

-   [HLSL에 대한 프로그래밍 지침](https://msdn.microsoft.com/library/windows/desktop/bb509635)
-   [Direct3D 10 질문과 대답](https://msdn.microsoft.com/library/windows/desktop/ee416643)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>.x 파일 형식 대신 무엇을 모델에 사용해야 할까요?


.x 파일 형식에 대한 공식적인 대체 형식은 없지만 많은 샘플에서는 SDKMesh 형식을 사용합니다. 또한 Visual Studio에는 많이 사용되는 몇 가지 형식을 CMO 파일로 컴파일하는 [콘텐츠 파이프라인](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)이 포함되어 있으며, 이러한 CMO 파일은 Visual Studio 3D 시작 키트의 코드를 사용하여 로드하거나 [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929)를 사용하여 로드할 수 있습니다.

## <a name="how-do-i-debug-my-shaders"></a>셰이더를 디버그하는 방법은 무엇인가요?


Microsoft Visual Studio2015 DirectX 그래픽용 진단 도구가 포함 되어 있습니다. [DirectX 그래픽 디버그](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx)를 참조하세요.

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>*x* 함수에 해당하는 Direct3D 11의 함수는 무엇인가요?


DirectX 11 API에 DirectX 9 기능 매핑에 제공되어 있는 [함수 매핑](feature-mapping.md#function-mapping)을 참조하세요.

##  <a name="what-is-the-dxgiformat-equivalent-of-y-surface-format"></a>*y* 화면 형식에 해당하는 DXGI\_FORMAT의 형식은 무엇인가요?


DirectX 11 API에 DirectX 9 기능 매핑에 제공되어 있는 [화면 형식 매핑](feature-mapping.md#surface-format-mapping)을 참조하세요.

 

 




