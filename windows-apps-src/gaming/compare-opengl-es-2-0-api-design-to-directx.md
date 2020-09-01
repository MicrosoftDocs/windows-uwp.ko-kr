---
title: OpenGL ES 2.0에서 Direct3D로 포트 계획
description: IOS 또는 Android 플랫폼에서 게임을 이식 하는 경우 OpenGL ES 2.0에 상당한 투자가 발생 했을 것입니다.
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, opengl, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: dddbf3a1009ccbaeb7fd0695d995cd17ba6182d3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165347"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>OpenGL ES 2.0에서 Direct3D로 포트 계획




**중요 API**

-   [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
-   [Visual C++](/previous-versions/60k1461a(v=vs.140))

IOS 또는 Android 플랫폼에서 게임을 이식 하는 경우 OpenGL ES 2.0에 상당한 투자가 발생 했을 것입니다. 그래픽 파이프라인 코드 베이스를 Direct3D 11 및 Windows 런타임로 이동 하기 위해 준비 하는 경우 시작 하기 전에 몇 가지 사항을 고려해 야 합니다.

대부분의 포팅 활동은 일반적으로 코드 베이스를 탐색 하 고 두 모델 간에 공통 Api 및 패턴을 매핑합니다. 이 항목을 읽고 검토 하는 데 시간이 오래 걸리는 경우이 프로세스를 조금 더 쉽게 찾을 수 있습니다.

OpenGL ES 2.0에서 Direct3D 11로 그래픽을 이식할 때 알아야 할 몇 가지 사항은 다음과 같습니다.

## <a name="notes-on-specific-opengl-es-20-providers"></a>특정 OpenGL ES 2.0 공급자에 대 한 참고 사항


이 섹션의 포팅 항목에서는 Khronos 그룹에서 만든 OpenGL ES 2.0 사양의 Windows 구현을 참조 합니다. 모든 OpenGL ES 2.0 코드 샘플은 Visual Studio 2012 및 기본 Windows C 구문을 사용 하 여 개발 되었습니다. 목적-C (iOS) 또는 Java (Android) 코드 베이스에서 제공 되는 경우 제공 된 OpenGL ES 2.0 코드 샘플은 유사한 API 호출 구문 또는 매개 변수를 사용 하지 않을 수 있습니다. 이 지침은 플랫폼을 최대한 독립적으로 유지 하려고 시도 합니다.

이 설명서에서는 OpenGL ES 코드 및 참조에 대해서만 2.0 사양 Api를 사용 합니다. OpenGL ES 1.1 또는 3.0에서 이식 하는 경우 일부 OpenGL ES 2.0 코드 예제 및 컨텍스트가 익숙하지 않을 수 있지만이 콘텐츠는 여전히 유용 합니다.

이러한 항목의 Direct3D 11 샘플은 CX (구성 요소 확장)와 함께 Microsoft Windows c + +를 사용 합니다. 이 버전의 c + + 구문에 대 한 자세한 내용은 [Visual C++](/previous-versions/60k1461a(v=vs.140)), [런타임 플랫폼용 구성 요소 확장](/cpp/windows/component-extensions-for-runtime-platforms)및 [빠른 참조 (c + + \\ CX)를 참조](/cpp/cppcx/quick-reference-c-cx)하세요.

## <a name="understand-your-hardware-requirements-and-resources"></a>하드웨어 요구 사항 및 리소스 이해


OpenGL ES 2.0에서 지원 되는 그래픽 처리 기능 집합은 Direct3D 9.1에서 제공 되는 기능에 약 매핑됩니다. Direct3D 11에서 제공 되는 고급 기능을 활용 하려면 포트를 계획할 때 [direct3d 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) 설명서를 검토 하거나, 초기 노력으로 완료 된 경우 [에는 DirectX 9에서 유니버설 Windows 플랫폼 (UWP)](porting-your-directx-9-game-to-windows-store.md) 항목의 포트를 검토 하세요.

초기 포팅 작업을 간단 하 게 하려면 Visual Studio Direct3D 템플릿부터 시작 합니다. 이 도구는 이미 구성 된 기본 렌더러를 제공 하며, 창 변경 및 Direct3D 기능 수준에서 리소스를 다시 만드는 것과 같은 UWP 앱 기능을 지원 합니다.

## <a name="understand-direct3d-feature-levels"></a>Direct3D 기능 수준 이해


Direct3D 11은 \_ 11 1 (direct3d 9.1)의 하드웨어 "기능 수준"에 대 한 지원을 제공 \_ 합니다. 이러한 기능 수준은 특정 그래픽 기능 및 리소스의 가용성을 표시 합니다. 일반적으로 대부분의 OpenGL ES 2.0 플랫폼은 Direct3D 9.1 (기능 수준 9 \_ 1) 기능 집합을 지원 합니다.

## <a name="review-directx-graphics-features-and-apis"></a>DirectX 그래픽 기능 및 Api 검토


| API 제품군                                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)                     | DXGI (DirectX Graphics Infrastructure)는 그래픽 하드웨어와 Direct3D 간의 인터페이스를 제공 합니다. [**Idxgid 어댑터**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) 및 [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) COM 인터페이스를 사용 하 여 장치 어댑터 및 하드웨어 구성을 설정 합니다. 버퍼 및 기타 창 리소스를 만들고 구성 하는 데 사용 합니다. 특히 [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) factory 패턴은 스왑 체인 (프레임 버퍼 집합)을 포함 하 여 그래픽 리소스를 가져오는 데 사용 됩니다. DXGI는 스왑 체인을 소유 하므로 [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) 인터페이스를 사용 하 여 화면에 프레임을 표시 합니다. |
| [Direct3D](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)       | Direct3D는 그래픽 인터페이스의 가상 표현을 제공 하 고이를 사용 하 여 그래픽을 그릴 수 있도록 하는 Api 집합입니다. 버전 11은 거의 비교할 수 있는 기능, 즉 OpenGL 4.3에 대 한 것입니다. 반면에 OpenGL ES 2.0은 DirectX9, 기능 및 OpenGL 2.0과 비슷하지만 OpenGL 3.0의 통합 셰이더 파이프라인을 사용 합니다. 대부분의 많은 리프트는 개별 리소스 및 하위 리소스에 대 한 액세스를 제공 하는 ID3D11Device1 및 ID3D11DeviceContext1 인터페이스를 사용 하 여 수행 됩니다.                                                                                                                                          |
| [Direct2D](/windows/desktop/Direct2D/direct2d-portal)                      | Direct2D는 GPU 가속 2D 렌더링을 위한 Api 집합을 제공 합니다. OpenVG와 유사한 방식으로 간주할 수 있습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal)            | DirectWrite는 GPU 가속 고품질 글꼴 렌더링을 위한 Api 집합을 제공 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](/windows/desktop/dxmath/directxmath-portal)                  | DirectXMath는 일반적인 선형 대 수 및 삼각 형식, 값 및 함수를 처리 하기 위한 Api 및 매크로 집합을 제공 합니다. 이러한 형식 및 함수는 Direct3D 및 해당 셰이더 작업에서 잘 작동 하도록 디자인 되었습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) | Direct3D 셰이더에서 사용 되는 현재 HLSL 구문입니다. Direct3D 셰이더 모델 5.0을 구현 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Windows 런타임 Api 및 템플릿 라이브러리 검토


Windows 런타임 Api는 UWP 앱에 대 한 전체 인프라를 제공 합니다. [여기](/uwp/api/)에서 검토 하세요.

그래픽 파이프라인 포팅에 사용 되는 키 Windows 런타임 Api는 다음과 같습니다.

-   [**Windows:: UI:: Core:: CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows:: UI:: Core:: CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)
-   [**Windows:: ApplicationModel:: Core:: IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)
-   [**Windows:: ApplicationModel:: Core:: CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)

또한 Windows 런타임 c + + 템플릿 라이브러리 (WRL)는 Windows 런타임 구성 요소를 작성 하 고 사용 하는 하위 수준 방법을 제공 하는 템플릿 라이브러리입니다. UWP 앱 용 Direct3D 11 Api는 접속사에서 스마트 포인터 ([ComPtr](/cpp/windows/comptr-class))와 같은이 라이브러리의 인터페이스 및 형식과 함께 사용 하는 것이 가장 좋습니다. WRL에 대 한 자세한 내용은 [c + + 템플릿 라이브러리 Windows 런타임 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)를 참조 하세요.

## <a name="change-your-coordinate-system"></a>좌표계 변경


초기 포팅 작업에서 혼란을 일으키는 한 가지 차이점은 OpenGL의 경우 전통적인 오른손 좌표계를 사용하는 반면, Direct3D의 경우 기본적으로 왼손 좌표계를 사용한다는 점입니다. 이와 같이 좌표 모델링의 이러한 변화는 꼭 짓 점 버퍼의 설정 및 구성에서 많은 행렬 수학 함수에 이르기까지 게임의 많은 부분에 영향을 줍니다. 가장 중요 한 두 가지 변경 내용은 다음과 같습니다.

-   Direct3D가 앞에서 시계 방향으로 이동 하도록 삼각형 꼭 짓 점의 순서를 대칭 이동 합니다. 예를 들어 꼭 짓 점이 OpenGL 파이프라인에서 0, 1, 2로 인덱싱되는 경우 대신 0, 2, 1로 Direct3D에 전달 합니다.
-   뷰 매트릭스를 사용 하 여 z 방향으로 세계 공간을 x 1.0 f로 확장 하 여 z 축 좌표를 효과적으로 반대로 합니다. 이렇게 하려면 뷰 매트릭스에서 M31, M32-16ms 및 M33 위치에 있는 값의 부호를 대칭 이동 합니다 ( [**행렬**](/windows/desktop/direct3d9/matrix4x4) 형식으로 이식 하는 경우). M34가 0이 아닌 경우에는 해당 부호도 전환 합니다.

그러나 Direct3D는 오른손 좌표계를 지원할 수 있습니다. DirectXMath는 왼손 및 오른손 좌표계에서 작동 하는 다양 한 함수를 제공 합니다. 이러한 작업을 사용 하 여 원래 메시 데이터 및 matrix 처리를 유지할 수 있습니다. 다음과 같은 변경 내용이 해당됩니다.

| DirectXMath 행렬 함수                                                   | Description                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatlh)                               | 카메라 위치, 위쪽 방향 및 초점을 사용 하 여 왼손 좌표계의 뷰 행렬을 작성 합니다.       |
| [**XMMatrixLookAtRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatrh)                               | 카메라 위치, 위쪽 방향 및 초점을 사용 하 여 오른손 좌표계의 뷰 행렬을 작성 합니다.      |
| [**XMMatrixLookToLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktolh)                               | 카메라 위치, 위쪽 방향 및 카메라 방향을 사용 하 여 왼손 좌표 시스템에 대 한 뷰 행렬을 작성 합니다.  |
| [**XMMatrixLookToRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktorh)                               | 카메라 위치, 위쪽 방향 및 카메라 방향을 사용 하 여 오른손 좌표계의 뷰 행렬을 작성 합니다. |
| [**XMMatrixOrthographicLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographiclh)                   | 왼쪽 좌표 시스템에 대 한 직교 프로젝션 행렬을 만듭니다.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterlh) | 왼쪽 좌표 시스템에 대 한 사용자 지정 직교 투영 행렬을 만듭니다.                                           |
| [**XMMatrixOrthographicOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterrh) | 오른손 좌표계에 대 한 사용자 지정 직교 투영 행렬을 만듭니다.                                          |
| [**XMMatrixOrthographicRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicrh)                   | 오른손 좌표계에 대 한 직교 프로젝션 행렬을 만듭니다.                                                |
| [**XMMatrixPerspectiveFovLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovlh)               | 뷰의 필드를 기반으로 왼쪽 원근 투영 행렬을 만듭니다.                                                |
| [**XMMatrixPerspectiveFovRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovrh)               | 뷰의 필드를 기반으로 하는 오른쪽 원근 투영 행렬을 만듭니다.                                               |
| [**XMMatrixPerspectiveLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivelh)                     | 왼쪽 원근 투영 행렬을 만듭니다.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterlh)   | 왼쪽 원근 투영 행렬의 사용자 지정 버전을 빌드합니다.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterrh)   | 오른쪽 원근 투영 행렬의 사용자 지정 버전을 빌드합니다.                                                    |
| [**XMMatrixPerspectiveRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiverh)                     | 오른쪽 원근 투영 행렬을 만듭니다.                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>OpenGL ES 2.0-Direct3D 11 포팅 faq (질문과 대답)


-   질문: "일반적으로 내 OpenGL 코드에서 특정 문자열이 나 패턴을 검색 하 고이를 해당 하는 Direct3D로 바꿀 수 있나요?"
-   답변: 아니요. OpenGL ES 2.0 및 Direct3D 11은 다양 한 그래픽 파이프라인 모델 세대에서 제공 됩니다. 렌더링 컨텍스트 및 셰이더의 인스턴스화와 같이 개념 및 Api 사이에는 몇 가지 표면 유사점이 있지만,이 지침과 Direct3D 11 참조를 검토 하 여 일대일 매핑을 시도 하는 대신 파이프라인을 다시 만들 때 최상의 선택을 할 수 있습니다. 그러나가 중 SL에서 HLSL로 이식 하는 경우, intrinsincs 및 함수에 대 한 일반적인 별칭 집합을 만들면 더 쉽게 이식할 수 있으며,이를 통해 셰이더 코드 파일의 집합을 하나만 유지할 수 있습니다.

 

 