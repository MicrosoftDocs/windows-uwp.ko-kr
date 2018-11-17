---
author: mtoepke
title: OpenGL ES 2.0에서 Direct3D로의 포팅 계획
description: iOS 또는 Android 플랫폼에서 게임을 포팅하는 경우 OpenGL ES 2.0에 많은 투자를 했을 것입니다.
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, opengl, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 2308c0b931b58209d1233205c355ac09680803dd
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7154218"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>OpenGL ES 2.0에서 Direct3D로의 포팅 계획




**중요 API**

-   [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)
-   [Visual C++](https://msdn.microsoft.com/library/windows/apps/60k1461a.aspx)

iOS 또는 Android 플랫폼에서 게임을 포팅하는 경우 OpenGL ES 2.0에 많은 투자를 했을 것입니다. 그래픽 파이프라인 코드베이스를 Direct3D 11 및 Windows 런타임으로 이동할 준비를 할 때에는 시작하기 전에 몇 가지 사항을 고려해야 합니다.

대부분의 포팅 작업은 대개 처음에 코드베이스를 살펴보고 두 모델 간에 공통 API 및 패턴을 매핑하는 작업으로 구성됩니다. 이 항목을 읽고 검토하는 데 시간을 투자한다면 이 프로세스가 좀 더 쉽게 느껴질 것입니다.

OpenGL ES 2.0에서 Direct3D 11로 그래픽을 포팅할 때 고려해야 할 몇 가지 사항은 다음과 같습니다.

## <a name="notes-on-specific-opengl-es-20-providers"></a>특정 OpenGL ES 2.0 공급자에 대한 참고 사항


이 섹션의 포팅 항목에서는 Khronos Group에서 만든 OpenGL ES 2.0 사양의 Windows 구현을 참조합니다. 모든 OpenGL ES 2.0 코드 샘플은 Visual Studio 2012 및 기본 Windows C 구문을 사용하여 개발되었습니다. Objective-C(iOS) 또는 Java(Android)에서 포팅하는 경우 제공된 OpenGL ES 2.0 코드 샘플이 유사한 API 호출 구문 또는 매개 변수를 사용하지 않을 수 있음에 유의하세요. 이 지침에서는 가능한 한 플랫폼 독립성을 유지하려고 합니다.

이 설명서에서는 OpenGL ES 코드 및 참조를 위해 2.0 사양 API만 사용합니다. OpenGL ES 1.1 또는 3.0에서 포팅하는 경우 이 콘텐츠가 여전히 유용할 수 있지만, 일부 OpenGL ES 2.0 코드 예제 및 컨텍스트가 익숙하지 않을 수 있습니다.

이러한 항목의 Direct3D 11 샘플에서는 Microsoft Windows C++ with Component Extensions(CX)를 사용합니다. 이 버전의 C++ 구문에 대한 자세한 내용은 [Visual C++](https://msdn.microsoft.com/library/windows/apps/60k1461a.aspx), [런타임 플랫폼용 구성 요소 확장](https://msdn.microsoft.com/library/windows/apps/xey702bw.aspx) 및 [빠른 참조(C++\\CX)](https://msdn.microsoft.com/library/windows/apps/br212455.aspx)를 읽어 보세요.

## <a name="understand-your-hardware-requirements-and-resources"></a>하드웨어 요구 사항 및 리소스 이해


OpenGL ES 2.0에서 지원하는 그래픽 처리 기능 집합은 Direct3D 9.1에서 제공하는 기능에 대략적으로 매핑됩니다. Direct3D 11에서 제공하는 보다 고급 기능을 이용하려면 포팅 계획 시 [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080) 설명서를 검토하거나, 초기 작업을 완료한 경우 [DirectX 9에서 UWP(유니버설 Windows 플랫폼)로 포팅](porting-your-directx-9-game-to-windows-store.md) 항목을 검토하세요.

초기 포팅 작업을 단순화하려면 Visual Studio Direct3D 템플릿으로 시작합니다. 이 템플릿은 이미 구성되어 있는 기본 렌더러를 제공하고, 창 변경 및 Direct3D 기능 수준에 대한 리소스 다시 만들기와 같은 UWP 앱 기능을 지원합니다.

## <a name="understand-direct3d-feature-levels"></a>Direct3D 기능 수준 이해


Direct3D 11은 하드웨어 "기능 수준" 9\_1 (Direct3D 9.1) for 11\_1을 지원합니다. 이러한 기능 수준은 특정 그래픽 기능과 리소스의 가용성을 나타냅니다. 일반적으로 대부분의 OpenGL ES 2.0 플랫폼은 Direct3D 9.1(기능 수준 9\_1) 기능 집합을 지원합니다.

## <a name="review-directx-graphics-features-and-apis"></a>DirectX 그래픽 기능 및 API 검토


| API 제품군                                                | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)                     | DXGI(DirectX Graphics Infrastructure)는 그래픽 하드웨어와 Direct3D 간의 인터페이스를 제공하며, [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) 및 [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) COM 인터페이스를 사용하여 장치 어댑터와 하드웨어 구성을 설정합니다. DXGI를 사용하여 버퍼 및 기타 창 리소스를 만들고 구성할 수 있습니다. 특히, [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) 팩터리 패턴은 스왑 체인(프레임 버퍼 집합)을 비롯한 그래픽 리소스를 획득하는 데 사용됩니다. DXGI가 스왑 체인을 소유하고 있으므로 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) 인터페이스는 화면에 프레임을 표시하는 데 사용됩니다. |
| [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080)       | Direct3D는 그래픽 인터페이스의 가상 표현을 제공하는 API 집합으로서, 이를 사용하여 그래픽을 그릴 수 있습니다. 버전 11은 기능면에서 OpenGL 4.3과 거의 비슷합니다. (반면 OpenGL ES 2.0은 기능면에서 DirectX9 및 OpenGL 2.0과 비슷하지만 OpenGL 3.0의 통합 셰이더 파이프라인과 비슷합니다.) 대부분의 번거로운 작업은 개별 리소스와 하위 리소스 및 렌더링 컨텍스트에 각각 액세스를 제공하는 ID3D11Device1 및 ID3D11DeviceContext1 인터페이스를 사용하여 수행합니다.                                                                                                                                          |
| [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)                      | Direct2D는 GPU 가속 2D 렌더링을 위한 API 집합을 제공하며, 용도면에서 OpenVG와 비슷한 것으로 간주될 수 있습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)            | DirectWrite는 GPU 가속, 고품질 글꼴 렌더링을 위한 API 집합을 제공합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)                  | DirectXMath는 일반적인 선형 대수 및 삼각법 형식, 값 및 함수 처리를 위한 API 및 매크로 집합을 제공합니다. 이러한 형식 및 함수는 Direct3D 및 해당 셰이더 작업에 맞게 디자인되었습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509580) | Direct3D 셰이더에서 사용하는 현재 HLSL 구문이며, Direct3D 셰이더 모델 5.0을 구현합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Windows 런타임 API 및 템플릿 라이브러리 검토


Windows 런타임 API는 UWP 앱을 위한 전체 인프라를 제공합니다. [여기](https://msdn.microsoft.com/library/windows/apps/br211377)에서 해당 내용을 검토하세요.

그래픽 파이프라인 포팅에 사용되는 핵심 Windows 런타임 API는 다음과 같습니다.

-   [**Windows::UI::Core::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows::UI::Core::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)

또한 WRL(Windows 런타임 C++ 템플릿 라이브러리)은 Windows 런타임 구성 요소를 만들고 사용하는 낮은 수준의 방법을 제공하는 템플릿 라이브러리입니다. UWP 앱용 Direct3D 11 API는 이 라이브러리의 인터페이스 및 형식(예: 스마트 포인터([ComPtr](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)))과 함께 가장 잘 사용됩니다. WRL에 대한 자세한 내용은 [WRL(Windows 런타임 C++ 템플릿 라이브러리)](https://msdn.microsoft.com/library/windows/apps/hh438466.aspx)을 읽어 보세요.

## <a name="change-your-coordinate-system"></a>좌표계 변경


초기 포팅 작업에서 혼란을 일으키는 한 가지 차이점은 OpenGL의 경우 전통적인 오른손 좌표계를 사용하는 반면, Direct3D의 경우 기본적으로 왼손 좌표계를 사용한다는 점입니다. 이러한 좌표 모델링 변경은 꼭짓점 버퍼의 설정 및 구성에서 많은 행렬 수학 함수에 이르기까지 게임의 많은 부분에 영향을 줍니다. 수행할 가장 중요한 두 가지 변경 사항은 다음과 같습니다.

-   Direct3D가 앞쪽에서부터 시계 방향으로 트래버스하도록 삼각형 꼭짓점의 순서를 대칭 이동합니다. 예를 들어, OpenGL 파이프라인에서 꼭짓점의 색인을 0, 1 및 2로 지정한 경우 대신 꼭짓점을 0, 2, 1로 Direct3D에 전달합니다.
-   월드 공간을 z 방향으로 -1.0f씩 크기 조정하려면 보기 행렬을 사용합니다(효과적으로 z축 좌표를 반전시킴). 이렇게 하려면 보기 행렬에서 위치 M31, M32 및 M33에 있는 값의 부호를 반대로 합니다([**Matrix**](https://msdn.microsoft.com/library/windows/desktop/bb147180) 형식으로 포팅하는 경우). M34가 0이 아닌 경우 해당 부호도 반대로 합니다.

그러나 Direct3D는 오른손 좌표계를 지원할 수 있습니다. DirectXMath는 왼손 좌표계와 오른손 좌표계 둘 다에서 및 둘 다에 대해 작동하는 많은 함수를 제공합니다. 이러한 함수는 원래 메시 데이터 및 행렬 처리 중 일부를 유지하는 데 사용할 수 있습니다. 해당 함수는 다음과 같습니다.

| DirectXMath 행렬 함수                                                   | 설명                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](https://msdn.microsoft.com/library/windows/desktop/ee419969)                               | 카메라 위치, 위쪽 방향 및 초점을 사용하여 왼손 좌표계용 보기 행렬을 작성합니다.       |
| [**XMMatrixLookAtRH**](https://msdn.microsoft.com/library/windows/desktop/ee419970)                               | 카메라 위치, 위쪽 방향 및 초점을 사용하여 오른손 좌표계용 보기 행렬을 작성합니다.      |
| [**XMMatrixLookToLH**](https://msdn.microsoft.com/library/windows/desktop/ee419971)                               | 카메라 위치, 위쪽 방향 및 카메라 방향을 사용하여 왼손 좌표계용 보기 행렬을 작성합니다.  |
| [**XMMatrixLookToRH**](https://msdn.microsoft.com/library/windows/desktop/ee419972)                               | 카메라 위치, 위쪽 방향 및 카메라 방향을 사용하여 오른손 좌표계용 보기 행렬을 작성합니다. |
| [**XMMatrixOrthographicLH**](https://msdn.microsoft.com/library/windows/desktop/ee419975)                   | 왼손 좌표계용 직교 투영 행렬을 작성합니다.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](https://msdn.microsoft.com/library/windows/desktop/ee419976) | 왼손 좌표계용 사용자 지정 직교 투영 행렬을 작성합니다.                                           |
| [**XMMatrixOrthographicOffCenterRH**](https://msdn.microsoft.com/library/windows/desktop/ee419977) | 오른손 좌표계용 사용자 지정 직교 투영 행렬을 작성합니다.                                          |
| [**XMMatrixOrthographicRH**](https://msdn.microsoft.com/library/windows/desktop/ee419978)                   | 오른손 좌표계용 직교 투영 행렬을 작성합니다.                                                |
| [**XMMatrixPerspectiveFovLH**](https://msdn.microsoft.com/library/windows/desktop/ee419979)               | 보기의 필드를 기준으로 왼손 원근 투영 행렬을 작성합니다.                                                |
| [**XMMatrixPerspectiveFovRH**](https://msdn.microsoft.com/library/windows/desktop/ee419980)               | 보기의 필드를 기준으로 오른손 원근 투영 행렬을 작성합니다.                                               |
| [**XMMatrixPerspectiveLH**](https://msdn.microsoft.com/library/windows/desktop/ee419981)                     | 왼손 원근 투영 행렬을 작성합니다.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](https://msdn.microsoft.com/library/windows/desktop/ee419982)   | 왼손 원근 투영 행렬의 사용자 지정 버전을 작성합니다.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](https://msdn.microsoft.com/library/windows/desktop/ee419983)   | 오른손 원근 투영 행렬의 사용자 지정 버전을 작성합니다.                                                    |
| [**XMMatrixPerspectiveRH**](https://msdn.microsoft.com/library/windows/desktop/ee419984)                     | 오른손 원근 투영 행렬을 작성합니다.                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>OpenGL ES2.0-Direct3D 11 포팅 질문과 대답


-   질문: "일반적으로 OpenGL 코드에서 특정 문자열이나 패턴을 검색하여 Direct3D 문자열이나 패턴으로 바꿀 수 있나요?"
-   대답: 아니요. OpenGL ES 2.0과 Direct3D 11은 그래픽 파이프라인 모델링 세대가 서로 다릅니다. 개념과 API 간에 몇 가지 표면적인 유사성(예: 렌더링 컨텍스트 및 셰이더 인스턴스화)이 존재하지만 1:1 매핑을 시도하는 대신 파이프라인을 다시 만들 때 최선의 선택을 할 수 있도록 이 지침과 Direct3D 11 참조를 검토해야 합니다. 그러나 GLSL에서 HLSL로 포팅하는 경우 GLSL 변수, 내부 기능 및 함수에 대한 공통 별칭 집합을 만들면 포팅이 더 쉬워질 뿐만 아니라 단 하나의 셰이더 코드 파일 집합만 유지 관리할 수 있습니다.

 

 




