---
title: Direct3D 9에서 Direct3D 11로 중요 한 변경 내용
description: 이 항목에서는 DirectX 9와 DirectX 11의 상위 수준 차이점에 대해 설명 합니다.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, direct3d 9, direct3d 11, 변경 내용
ms.localizationpriority: medium
ms.openlocfilehash: ec70f77bbb124e5bce33f98e9f93b4bfd04e4a35
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168207"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Direct3D 9에서 Direct3D 11로 중요 한 변경 내용



**요약**

-   [DirectX 포트 계획](plan-your-directx-port.md)
-   Direct3D 9에서 Direct3D 11로 중요 한 변경 내용
-   [기능 매핑](feature-mapping.md)


이 항목에서는 DirectX 9와 DirectX 11의 상위 수준 차이점에 대해 설명 합니다.

Direct3D 11은 기본적으로 Direct3D 9와 동일한 유형의 API로, 그래픽 하드웨어에서 낮은 수준의 가상화 된 인터페이스입니다. 이를 통해 다양 한 하드웨어 구현에서 그래픽 그리기 작업을 수행할 수 있습니다. Direct3D 9 이후로 그래픽 API의 레이아웃이 변경되었습니다. 디바이스 컨텍스트의 개념이 확장되고 특히 그래픽 인프라에 대한 API가 추가되었습니다. Direct3D 장치에 저장 된 리소스에는 리소스 뷰 라는 데이터 다형성을 위한 novel 메커니즘이 있습니다.

## <a name="core-api-functions"></a>핵심 API 함수


Direct3D 9에서 사용을 시작 하기 전에 Direct3D API에 대 한 인터페이스를 만들어야 했습니다. UWP (Direct3D 11 유니버설 Windows 플랫폼) 게임에서 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 라는 정적 함수를 호출 하 여 장치 및 장치 컨텍스트를 만듭니다.

## <a name="devices-and-device-context"></a>장치 및 장치 컨텍스트


Direct3D 11 장치는 가상화 된 그래픽 어댑터를 나타냅니다. 비디오 메모리에서 리소스를 만드는 데 사용 됩니다. 예를 들어 GPU에 질감 업로드, 질감 리소스 및 스왑 체인에 대 한 보기 만들기, 텍스처 샘플러 만들기 등이 있습니다. Direct3D 11 장치 인터페이스를 사용 하는 방법에 대 한 전체 목록은 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 및 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)를 참조 하세요.

Direct3D 11. 장치 컨텍스트는 파이프라인 상태를 설정 하 고 렌더링 명령을 생성 하는 데 사용 됩니다. 예를 들어 Direct3D 11 렌더링 체인은 장치 컨텍스트를 사용 하 여 렌더링 체인을 설정 하 고 장면을 그립니다 (아래 참조). 장치 컨텍스트는 Direct3D 장치 리소스에 사용 되는 비디오 메모리에 액세스 (매핑) 하는 데 사용 됩니다. 또한 상수 버퍼 데이터와 같은 하위 리소스 데이터를 업데이트 하는 데 사용 됩니다. Direct3D 11 장치 컨텍스트를 사용 하는 방법에 대 한 전체 목록은 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 및 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)를 참조 하세요. 이 샘플의 대부분은 직접 컨텍스트를 사용 하 여 장치에 직접 렌더링 하지만 Direct3D 11은 주로 다중 스레딩에 사용 되는 지연 된 장치 컨텍스트를 지원 합니다.

Direct3D 11에서 장치 핸들 및 장치 컨텍스트 핸들은 모두 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)를 호출 하 여 가져옵니다. 이 방법은 특정 하드웨어 기능 집합을 요청 하 고 그래픽 어댑터에서 지 원하는 Direct3D 기능 수준에 대 한 정보를 검색 하는 데도 사용할 수 있습니다. 장치, 장치 컨텍스트 및 스레딩 고려 사항에 대 한 자세한 내용은 [Direct3D 11의 장치 소개](/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro) 를 참조 하세요.

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>장치 인프라, 프레임 버퍼 및 렌더링 대상 뷰


Direct3D 11에서 장치 어댑터 및 하드웨어 구성은 [**Idxgid 어댑터**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) 및 [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) COM 인터페이스를 사용 하 여 DXGI (DIRECTX Graphics Infrastructure) API를 사용 하 여 설정 됩니다. 버퍼 및 기타 창 리소스 (표시 또는 오프 스크린)는 특정 DXGI 인터페이스에 의해 생성 및 구성 됩니다. [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) factory 패턴 구현은 프레임 버퍼와 같은 DXGI 리소스를 가져옵니다. DXGI는 스왑 체인을 소유 하므로 DXGI 인터페이스를 사용 하 여 화면에 프레임을 표시 합니다. [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)를 참조 하세요.

[**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) 를 사용 하 여 게임과 호환 되는 스왑 체인을 만듭니다. HWND에 대 한 스왑 체인을 만드는 대신 핵심 창 또는 컴퍼지션 (XAML interop)의 스왑 체인을 만들어야 합니다.

## <a name="device-resources-and-resource-views"></a>장치 리소스 및 리소스 뷰


Direct3D 11은 보기 라고 하는 비디오 메모리 리소스에서 추가 수준의 다형성을 지원 합니다. 기본적으로 질감에 대 한 단일 Direct3D 9 개체를 사용 하는 경우 데이터를 보관 하는 질감 리소스와 뷰가 렌더링에 사용 되는 방식을 나타내는 리소스 뷰에 두 개체가 있습니다. 리소스를 기반으로 하는 뷰를 사용 하면 특정 용도에 해당 리소스를 사용할 수 있습니다. 예를 들어 2D 질감 리소스가 [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)으로 생성 된 다음 셰이더에[**ID3D11ShaderResourceView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview)(셰이더 리소스 뷰)를 만들어 셰이더에 질감으로 사용할 수 있습니다. 렌더링 대상 뷰 ([**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview))는 그리기 화면으로 사용할 수 있도록 동일한 2d 질감 리소스에도 만들 수 있습니다. 또 다른 예로 동일한 픽셀 데이터는 단일 질감 리소스에 대해 2 개의 개별 뷰를 사용 하 여 2 개의 다른 픽셀 형식으로 표현 됩니다.

기본 리소스는 생성 되는 뷰의 형식과 호환 되는 속성을 사용 하 여 만들어야 합니다. 예를 들어 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 이 표면에 적용 되는 경우 [**D3D11 \_ BIND \_ 렌더링 \_ 대상**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_bind_flag) 플래그를 사용 하 여 해당 화면을 만듭니다. 또한 표면에는 렌더링 ( [**dxgi \_ 형식**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)참조)과 호환 되는 DXGI 표면 형식이 있어야 합니다.

렌더링에 사용 하는 대부분의 리소스는 [**ID3D11DeviceChild**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild)에서 상속 되는 [**ID3D11Resource**](/windows/desktop/api/d3d11/nn-d3d11-id3d11resource) 인터페이스에서 상속 됩니다. 꼭 짓 점 버퍼, 인덱스 버퍼, 상수 버퍼 및 셰이더는 모두 Direct3D 11 리소스입니다. 입력 레이아웃 및 샘플러 상태는 [**ID3D11DeviceChild**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild)에서 직접 상속 됩니다.

리소스 뷰는 DXGI \_ 형식 열거형 값을 사용 하 여 픽셀 형식을 표시 합니다. 모든 D3DFMT가 DXGI 형식으로 지원 되는 것은 아닙니다 \_ . 예를 들어 DXGI에는 D3DFMT R8G8B8와 동일한 24bpp RGB 형식이 없습니다 \_ . 모든 RGB 형식에 해당 하는 BGR로 없습니다 \_ . DXGI format \_ R10G10B10A2 \_ UNORM은 D3DFMT A2B10G10R10와 동일 \_ 하지만 D3DFMT A2R10G10B10와는 직접적인 관계가 없습니다 \_ . 이러한 레거시 형식의 콘텐츠를 빌드 시 지원 되는 형식으로 변환 하는 계획을 세워야 합니다. DXGI 형식의 전체 목록은 [**dxgi \_ 형식**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) 열거를 참조 하세요.

Direct3D 장치 리소스 (및 리소스 뷰)는 장면을 렌더링 하기 전에 생성 됩니다. 장치 컨텍스트는 아래에 설명 된 대로 렌더링 체인을 설정 하는 데 사용 됩니다.

## <a name="device-context-and-the-rendering-chain"></a>장치 컨텍스트 및 렌더링 체인


Direct3D 9 및 Direct3D 10. x에는 리소스 생성, 상태 및 그리기를 관리 하는 단일 Direct3D 장치 개체가 있습니다. Direct3D 11에서 Direct3D 장치 인터페이스는 여전히 리소스 만들기를 관리 하지만 모든 상태 및 그리기 작업은 Direct3D 장치 컨텍스트를 사용 하 여 처리 됩니다. 다음은 장치 컨텍스트 ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 인터페이스)를 사용 하 여 렌더링 체인을 설정 하는 방법의 예입니다.

-   렌더링 대상 뷰 설정 및 해제 (및 깊이 스텐실 뷰)
-   입력 어셈블러 단계 (IA 단계)에 대 한 꼭 짓 점 버퍼, 인덱스 버퍼 및 입력 레이아웃을 설정 합니다.
-   꼭 짓 점 및 픽셀 셰이더를 파이프라인에 바인딩
-   셰이더에 상수 버퍼 바인딩
-   질감 뷰 및 샘플러를 픽셀 셰이더에 바인딩
-   장면 그리기

[**ID3D11DeviceContext::D raw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) 메서드 중 하나가 호출 되 면 렌더링 대상 보기에 장면이 그려집니다. 완료 되 면 [**IDXGISwapChain1::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)를 호출 하 여 완료 된 프레임을 표시 하는 데 DXGI 어댑터의 모든 그리기가 사용 됩니다.

## <a name="state-management"></a>상태 관리


개별 설정/해제 집합을 포함 하는 Direct3D 9 관리 되는 상태 설정은 SetRenderState, SetSamplerState 및 SetTextureStageState 메서드를 사용 하 여 설정 됩니다. Direct3D 11은 레거시 고정 함수 파이프라인을 지원 하지 않으므로 SetTextureStageState는 픽셀 셰이더 (PS)를 작성 하 여 대체 됩니다. Direct3D 9 state 블록에 해당 하는 항목이 없습니다. Direct3D 11은 렌더링 상태를 그룹화 하는 보다 간소화 된 방법을 제공 하는 4 가지 종류의 상태 개체를 사용 하 여 상태를 관리 합니다.

예를 들어 D3DRS zenable과 함께 SetRenderState를 사용 하는 대신 \_ 이 및 기타 관련 상태 설정을 사용 하 여 DepthStencilState 개체를 만들고 렌더링 하는 동안 상태를 변경 하는 데 사용 합니다.

Direct3D 9 응용 프로그램을 상태 개체로 포팅 하는 경우 다양 한 상태 조합이 변경할 수 없는 상태 개체로 표시 된다는 점에 유의 하십시오. 한 번 만든 후 유효한 경우에는 다시 사용 해야 합니다.

## <a name="direct3d-feature-levels"></a>Direct3D 기능 수준


Direct3D에는 기능 수준 이라는 하드웨어 지원을 결정 하는 새로운 메커니즘이 있습니다. 기능 수준은 잘 정의 된 GPU 기능 집합을 요청할 수 있도록 하 여 그래픽 어댑터에서 수행할 수 있는 작업을 간소화 합니다. 예를 들어 9 \_ 1 기능 수준은 Direct3D 9 그래픽 어댑터에서 제공 하는 기능을 구현 합니다 (셰이더 모델 2.x 포함). 9 \_ 1은 가장 낮은 기능 수준 이므로 모든 장치에서 꼭 짓 점 셰이더 및 픽셀 셰이더를 지원 하도록 할 수 있습니다 .이는 Direct3D 9의 프로그래밍 가능한 셰이더 모델에서 지 원하는 것과 동일한 단계 였습니다.

게임에서 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 를 사용 하 여 Direct3D 장치 및 장치 컨텍스트를 만듭니다. 이 함수를 호출 하면 게임에서 지원할 수 있는 기능 수준 목록을 제공 합니다. 이는 해당 목록에서 지원 되는 가장 높은 기능 수준을 반환 합니다. 예를 들어 게임에서 BC4/BC5 질감 (DirectX 10 하드웨어의 기능)을 사용할 수 있는 경우 \_ \_ 지원 되는 기능 수준 목록에서 최소 9 1 및 10 0을 포함 합니다. 게임이 DirectX 9 하드웨어에서 실행 중이 고 BC4/BC5 텍스처를 사용할 수 없는 경우 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 는 9 1을 반환 \_ 합니다. 그러면 게임이 다른 질감 형식 (그리고 더 작은 질감)으로 대체 될 수 있습니다.

Direct3d 9 게임을 확장 하 여 더 높은 Direct3D 기능 수준을 지원 하도록 결정 한 경우 기존 Direct3D 9 그래픽 코드를 먼저 이식 하는 것이 좋습니다. Direct3D 11에서 게임을 진행 하 고 나면 향상 된 그래픽으로 렌더링 경로를 더 쉽게 추가할 수 있습니다.

기능 수준 지원에 대 한 자세한 설명은 [Direct3D 기능 수준](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro) 을 참조 하세요. Direct3D 11 기능에 대 한 전체 목록은 direct3d [11 기능](/windows/desktop/direct3d11/direct3d-11-features) 및 [direct3d 11.1 기능](/windows/desktop/direct3d11/direct3d-11-1-features) 을 참조 하세요.

## <a name="feature-levels-and-the-programmable-pipeline"></a>기능 수준 및 프로그래밍 가능 파이프라인


하드웨어는 Direct3D 9 이후 계속 진화 하 고 몇 가지 새로운 선택적 단계가 프로그래밍 가능한 그래픽 파이프라인에 추가 되었습니다. 그래픽 파이프라인에 대 한 옵션 집합은 Direct3D 기능 수준에 따라 달라 집니다. 기능 수준 10.0에는 GPU에서 multipass 렌더링을 위한 선택적 스트림 출력을 포함 하는 기 하 도형 셰이더 단계가 포함 되어 있습니다. 기능 수준 11 \_ 0에는 하드웨어 공간 분할에 사용할 선체 셰이더 및 도메인 셰이더가 포함 됩니다. 기능 수준 11 \_ 0에는 directcompute 셰이더에 대 한 완전 한 지원도 포함 되지만 기능 수준 10. x는 제한 된 형식의 DirectCompute에 대 한 지원도 포함 합니다.

모든 셰이더는 Direct3D 기능 수준에 해당 하는 셰이더 프로필을 사용 하 여 HLSL로 작성 됩니다. 셰이더 프로필은 위쪽에서 호환 되므로 vs \_ 4 \_ 0 \_ 수준 \_ 9 \_ 1 또는 ps 4 0 수준 9 1을 사용 하 여 컴파일하는 HLSL 셰이더는 \_ \_ \_ \_ \_ 모든 장치에서 작동 합니다. 셰이더 프로필은 하위 호환 되지 않으므로 vs 4 1을 사용 하 여 컴파일된 셰이더는 \_ \_ 기능 수준 10 \_ 1, 11 \_ 0 또는 11 \_ 개 장치 에서만 작동 합니다.

SetVertexShaderConstant 및 SetPixelShaderConstant가 포함 된 공유 배열을 사용 하는 셰이더 용 Direct3D 9 관리 되는 상수 Direct3D 11은 꼭 짓 점 버퍼 또는 인덱스 버퍼와 같은 리소스에 해당 하는 상수 버퍼를 사용 합니다. 상수 버퍼는 효율적으로 업데이트 되도록 설계 되었습니다. 모든 셰이더가 단일 전역 배열로 구성 되는 대신 상수를 논리 그룹으로 구성 하 고 하나 이상의 상수 버퍼를 통해 관리 합니다. Direct3D 9 게임을 Direct3D 11로 이식 하는 경우 적절 하 게 업데이트할 수 있도록 상수 버퍼를 구성 하는 계획을 세워야 합니다. 예를 들어 모든 프레임을 별도의 상수 버퍼로 업데이트 하지 않는 셰이더 상수를 그룹화 하 여 더 많은 동적 셰이더 상수와 함께 그래픽 어댑터에 해당 데이터를 지속적으로 업로드 하지 않아도 되도록 할 수 있습니다.

> **참고**    대부분의 Direct3D 9 응용 프로그램에서는 셰이더를 광범위 하 게 사용 하지만 때때로 레거시 고정 함수 동작을 사용 하는 경우를 혼합 했습니다. Direct3D 11은 프로그래밍 가능 음영 모델만 사용 합니다. Direct3D 9의 레거시 고정 함수 기능은 더 이상 사용 되지 않습니다.

 

 

 