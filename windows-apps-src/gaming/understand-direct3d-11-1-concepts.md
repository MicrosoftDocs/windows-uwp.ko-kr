---
title: Direct3D 9에서 Direct3D 11로의 중요 변경 사항
description: 이 항목에서는 DirectX 9과 DirectX 11의 전반적인 차이점을 설명합니다.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, direct3d 9, direct3d 11, 변경 사항
ms.localizationpriority: medium
ms.openlocfilehash: ecdd8591efb3920d2cfe333aa8ec02c65c1a1465
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7700431"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Direct3D 9부터 Direct3D 11의 주요 변경 사항



**요약**

-   [DirectX 포트 계획](plan-your-directx-port.md)
-   Direct3D 9에서 Direct3D 11로의 중요 변경 사항
-   [기능 매핑](feature-mapping.md)


이 항목에서는 DirectX 9과 DirectX 11의 전반적인 차이점을 설명합니다.

Direct3D 11은 기본적으로 그래픽 하드웨어로 가상화된 하위 수준의 인터페이스인 Direct3D 9과 같은 유형의 API입니다. Direct3D 11에서는 여전히 다양한 하드웨어 구현에서 그래픽 그리기 작업을 수행할 수 있습니다. Direct3D 9 이후로 그래픽 API의 레이아웃이 변경되었습니다. 디바이스 컨텍스트의 개념이 확장되고 특히 그래픽 인프라에 대한 API가 추가되었습니다. Direct3D 디바이스에 저장된 리소스는 리소스 뷰라고 하는 고유한 데이터 다형성 메커니즘을 갖습니다.

## <a name="core-api-functions"></a>주요 API 기능


Direct3D 9에서 Direct3D API를 사용하려면 먼저 이에 대한 인터페이스를 만들어야 했습니다. Direct3D 11 UWP(유니버설 Windows 플랫폼) 게임에서는 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)라는 정적 함수를 호출하여 장치와 디바이스 컨텍스트를 만듭니다.

## <a name="devices-and-device-context"></a>장치 및 디바이스 컨텍스트


Direct3D 11 장치는 가상화된 그래픽 어댑터를 나타냅니다. 이 장치는 비디오 메모리에 리소스를 만드는 데 사용됩니다(예: GPU에 텍스처 업로드, 텍스처 리소스 및 스왑 체인에서 뷰 만들기, 텍스처 샘플러 만들기). Direct3D 11 장치 인터페이스가 어떤 용도로 사용되는지 전체 목록을 보려면 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 및 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)을 참조하세요.

Direct3D 11 디바이스 컨텍스트는 파이프라인 상태를 설정하고 렌더링 명령을 생성하는 데 사용됩니다. 예를 들어 Direct3D 11 렌더링 체인은 디바이스 컨텍스트를 사용하여 렌더링 체인을 설정하고 장면을 그립니다(아래 참조). 디바이스 컨텍스트는 Direct3D 장치 리소스에서 사용하는 비디오 메모리에 액세스(매핑)하는 데 사용되며, 또한 하위 리소스 데이터(예: 상수 버퍼 데이터)를 업데이트하는 데도 사용됩니다. Direct3D 11 디바이스 컨텍스트가 어떤 용도로 사용되는지 전체 목록을 보려면 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 및 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)을 참조하세요. 샘플에서는 대부분 장치에 바로 렌더링할 직접 컨텍스트를 사용하지만, Direct3D 11에서는 주로 다중 스레딩에 사용되는 지연된 디바이스 컨텍스트도 지원합니다.

Direct3D 11에서 장치 핸들과 디바이스 컨텍스트 핸들은 모두 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)를 호출하여 가져옵니다. 이 메서드는 또한 특정 하드웨어 기능 집합을 요청하고 그래픽 어댑터에서 지원하는 Direct3D 기능 수준에 대한 정보를 검색하는 데도 사용됩니다. 장치, 디바이스 컨텍스트 및 스레딩 고려 사항에 대한 자세한 내용은 [Direct3D 11의 장치 소개](https://msdn.microsoft.com/library/windows/desktop/ff476880)를 참조하세요.

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>디바이스 인프라, 프레임 버퍼 및 렌더링 대상 뷰


Direct3D 11에서 디바이스 어댑터 및 하드웨어 구성은 [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) 및 [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) COM 인터페이스를 사용하여 DXGI(DirectX Graphics Infrastructure) API와 함께 설정됩니다. 버퍼 및 기타 창 리소스(표시 또는 오프스크린)는 특정 DXGI 인터페이스를 통해 만들고 구성합니다. [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) 팩터리 패턴 구현에서는 프레임 버퍼 등의 DXGI 리소스를 가져옵니다. DXGI가 스왑 체인을 소유하므로 화면에 프레임을 제공하는 데 DXGI 인터페이스가 사용됩니다([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) 참조).

[**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)를 사용하여 게임과 호환되는 스왑 체인을 만들 수 있습니다. HWND를 위해 스왑 체인을 만드는 대신 주요 창 또는 컴퍼지션(XAML interop)을 위해 스왑 체인을 만들어야 합니다.

## <a name="device-resources-and-resource-views"></a>장치 리소스 및 리소스 뷰


Direct3D 11은 비디오 메모리 리소스에서 뷰라고 하는 추가 수준의 다형성을 지원합니다. 기본적으로, 이전에는 텍스처에 대한 Direct3D 9 개체가 하나였지만 이제는 데이터를 보관하는 텍스처 리소스와 뷰가 렌더링에 사용되는 방식을 나타내는 리소스 뷰 등 두 개의 개체가 제공됩니다. 리소스를 기반으로 하는 뷰를 사용하면 해당 리소스를 특정 용도로 사용할 수 있습니다. 예를 들어 2D 텍스처 리소스를 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)로 만든 다음 여기에 셰이더 리소스 뷰([**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628))를 만들면 이 리소스를 셰이더에서 텍스처로 사용할 수 있습니다. 또한 동일한 2D 텍스처 리소스에 렌더링 대상 뷰([**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582))를 만들면 이 리소스를 그리기 화면으로 사용할 수 있습니다. 또 다른 예로, 단일 텍스처 리소스에서 2개의 개별 뷰를 사용하여 동일한 픽셀 데이터를 2개의 다른 픽셀 형식으로 나타낼 수 있습니다.

리소스에서 만들 뷰 유형과 호환되는 속성을 사용하여 기본 리소스를 만들어야 합니다. 예를 들어 화면에 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)가 적용될 경우 이 화면은 [**D3D11\_BIND\_RENDER\_TARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476085) 플래그를 사용하여 만들어집니다. 화면은 또한 렌더링과 호환되는 DXGI 화면 형식을 사용해야 합니다([**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059) 참조).

렌더링에 사용하는 대부분의 리소스는 [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380)에서 상속된 [**ID3D11Resource**](https://msdn.microsoft.com/library/windows/desktop/ff476584) 인터페이스에서 상속됩니다. 꼭짓점 버퍼, 인덱스 버퍼, 상수 버퍼 및 셰이더는 모두 Direct3D 11 리소스입니다. 입력 레이아웃 및 샘플러 상태는 [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380)에서 직접 상속됩니다.

리소스 뷰는 DXGI\_FORMAT 열거형 값을 사용하여 픽셀 형식을 나타냅니다. 일부 D3DFMT는 DXGI\_FORMAT으로 지원되지 않습니다. 예를 들어 DXGI에는 D3DFMT\_R8G8B8에 해당하는 24bpp RGB 형식이 없습니다. 또한 일부 RGB 형식에 해당하는 BGR이 없습니다. 예를 들어 DXGI\_FORMAT\_R10G10B10A2\_UNORM은 D3DFMT\_A2B10G10R10과 동일하지만, D3DFMT\_A2R10G10B10에 직접 해당하는 형식은 없습니다. 이러한 레거시 형식의 콘텐츠는 빌드할 때 지원되는 형식으로 변환하도록 계획해야 합니다. 전체 DXGI 형식 목록을 보려면 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059) 열거형을 참조하세요.

장면을 렌더링하기 전에 먼저 Direct3D 디바이스 리소스와 리소스 뷰를 만듭니다. 디바이스 컨텍스트는 렌더링 체인을 설정하는 데 사용됩니다(아래 설명 참조).

## <a name="device-context-and-the-rendering-chain"></a>디바이스 컨텍스트 및 렌더링 체인


Direct3D 9과 Direct3D 10.x에서는 단일 Direct3D 장치 개체가 리소스 생성, 상태 및 그리기를 관리했습니다. Direct3D 11에서도 Direct3D 장치 인터페이스가 리소스 생성을 관리하지만 모든 상태 및 그리기 작업은 Direct3D 디바이스 컨텍스트를 사용하여 처리합니다. 다음은 디바이스 컨텍스트([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 인터페이스)를 사용하여 렌더링 체인을 설정하는 방법에 대한 예입니다.

-   렌더링 대상 뷰(및 깊이 스텐실 뷰) 설정 및 지우기
-   꼭짓점 버퍼, 인덱스 버퍼 및 IA 단계(입력 어셈블러 단계)에 대한 입력 레이아웃 설정
-   파이프라인에 꼭짓점 및 픽셀 셰이더 바인딩
-   셰이더에 상수 버퍼 바인딩
-   픽셀 셰이더에 텍스처 뷰 및 샘플러 바인딩
-   장면 그리기

[**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) 메서드 중 하나가 호출되면 렌더링 대상 뷰에 장면이 그려집니다. 그리기를 모두 마치면 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)을 호출하여 완성된 프레임을 표시하는 데 DXGI 어댑터가 사용됩니다.

## <a name="state-management"></a>상태 관리


Direct3D 9에서는 SetRenderState, SetSamplerState 및 SetTextureStageState 메서드로 설정한 다양한 개별 토글 집합을 사용하여 상태 설정을 관리했습니다. Direct3D 11에서는 레거시 고정 함수 파이프라인을 지원하지 않으므로 SetTextureStageState 대신 PS(픽셀 셰이더)를 작성합니다. Direct3D 9 상태 블록에 해당하는 기능도 없습니다. Direct3D 11에서는 대신 렌더링 상태를 그룹화하는 더 간소화된 방법을 제공하는 4종류의 상태 개체를 사용하여 상태를 관리합니다.

예를 들어 D3DRS\_ZENABLE의 SetRenderState를 사용하는 대신 이 상태 설정과 다른 관련 상태 설정에 대해 DepthStencilState 개체를 만든 다음 렌더링하는 동안 이 개체를 사용하여 상태를 변경합니다.

Direct3D 9 응용 프로그램을 상태 개체로 포팅할 경우 다양한 상태 조합이 변경이 불가능한 상태 개체로 표시됩니다. 이러한 상태 개체를 만들면 유효한 경우 계속 다시 사용해야 합니다.

## <a name="direct3d-feature-levels"></a>Direct3D 기능 수준


Direct3D는 하드웨어 지원을 결정할 수 있는, 기능 수준이라는 새로운 메커니즘을 사용합니다. 기능 수준은 잘 정의된 GPU 기능 집합을 요청할 수 있도록 하여 그래픽 어댑터의 기능을 알아내는 작업을 간소화합니다. 예를 들어 9\_1 기능 수준은 셰이더 모델 2.x를 포함하여 Direct3D 9 그래픽 어댑터가 제공하는 기능을 구현합니다. 9\_1은 가장 낮은 기능 수준이므로 모든 디바이스에서 꼭짓점 셰이더와 픽셀 셰이더를 지원하는 것(Direct3D 9 프로그램 가능 셰이더 모델에서 지원하는 것과 동일한 단계)을 기대할 수 있습니다.

게임은 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)를 사용하여 Direct3D 디바이스 및 디바이스 컨텍스트를 만듭니다. 이 함수를 호출할 때 게임에서 지원할 수 있는 기능 수준 목록을 제공합니다. 이 함수는 해당 목록에서 지원되는 가장 높은 기능 수준을 반환합니다. 예를 들어 게임에서 BC4/BC5 텍스처(DirectX 10 하드웨어의 기능)를 사용할 수 있는 경우 지원되는 기능 수준 목록에 최소한 9\_1과 10\_0을 포함합니다. 게임이 DirectX 9 하드웨어에서 실행되어 BC4/BC5 텍스처를 사용할 수 없으면 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)는 9\_1을 반환합니다. 그러면 게임이 다른 텍스처 형식(및 더 작은 텍스처)으로 변경될 수 있습니다.

더 높은 Direct3D 기능 수준을 지원하도록 Direct3D 9 게임을 확장하려는 경우에는 먼저 기존 Direct3D 9 그래픽 코드의 포팅을 마치는 것이 좋습니다. 게임이 Direct3D 11에서 작동하도록 하면 추가 렌더링 경로를 추가하는 것이 더 쉬워져서 그래픽이 향상될 수 있습니다.

기능 수준 지원에 대한 자세한 설명을 보려면 [Direct3D 기능 수준](https://msdn.microsoft.com/library/windows/desktop/ff476876)을 참조하세요. 전체 Direct3D 11 기능 목록을 보려면 [Direct3D 11 기능](https://msdn.microsoft.com/library/windows/desktop/ff476342) 및 [Direct3D 11.1 기능](https://msdn.microsoft.com/library/windows/desktop/hh404562)을 참조하세요.

## <a name="feature-levels-and-the-programmable-pipeline"></a>기능 수준 및 프로그램 가능 파이프라인


하드웨어는 Direct3D 9 이후로 계속 진화하여, 프로그램 가능 그래픽 파이프라인에 여러 가지 선택적 단계가 새로 추가되었습니다. 그래픽 파이프라인에 사용할 수 있는 옵션 집합은 Direct3D 기능 수준에 따라 다릅니다. 기능 수준 10.0에는 GPU의 멀티패스 렌더링을 위한 선택적 스트림 아웃과 함께 기하 도형 셰이더 단계가 포함되어 있습니다. 기능 수준 11\_0에는 하드웨어 공간 분할(tessellation)에 사용할 수 있는 헐 셰이더 및 도메인 셰이더가 포함되어 있습니다. 기능 수준 11\_0에서는 또한 DirectCompute 셰이더를 완전히 지원합니다. 반면 기능 수준 10.x에서는 제한된 형태의 DirectCompute만 지원합니다.

모든 셰이더는 Direct3D 기능 수준에 해당하는 셰이더 프로필을 사용하여 HLSL로 작성됩니다. 셰이더 프로필은 상위 버전과 호환되므로 vs\_4\_0\_level\_9\_1 또는 ps\_4\_0\_level\_9\_1을 사용하여 컴파일되는 HLSL 셰이더는 모든 디바이스에서 작동합니다. 셰이더 프로필은 하위 버전과는 호환되지 않으므로 vs\_4\_1을 사용하여 컴파일된 셰이더는 기능 수준 10\_1, 11\_0 또는 11\_1 디바이스에서만 작동합니다.

Direct3D 9에서는 SetVertexShaderConstant 및 SetPixelShaderConstant를 통해 공유 배열을 사용하여 셰이더의 상수를 관리했습니다. Direct3D 11에서는 꼭짓점 버퍼 또는 인덱스 버퍼와 같은 리소스인 상수 버퍼를 사용합니다. 상수 버퍼는 효율적으로 업데이트되도록 디자인되었습니다. 모든 셰이더 상수를 단일 전역 배열로 구성하는 대신 상수를 논리적 그룹으로 구성한 후 하나 이상의 상수 버퍼를 통해 관리합니다. Direct3D 9 게임을 Direct3D 11로 포팅할 경우 상수 버퍼를 적절히 업데이트할 수 있도록 구성해야 합니다. 예를 들어, 매 프레임마다 업데이트되지 않는 셰이더 상수를 별도의 상수 버퍼로 그룹화하면 더 동적인 셰이더 상수와 함께 해당 데이터를 계속해서 그래픽 어댑터에 업로드할 필요가 없습니다.

> **참고**  대부분의 Direct3D 9 응용 프로그램 셰이더를 광범위 하 게 사용 했지만 경우에 따라 레거시 고정 함수 동작 사용 하기도 합니다. Direct3D 11은 프로그램 가능 음영 모델만 사용합니다. Direct3D 9의 레거시 고정 함수 기능은 더 이상 사용되지 않습니다.

 

 

 




