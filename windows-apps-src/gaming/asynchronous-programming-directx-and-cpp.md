---
title: 비동기 프로그래밍 (DirectX 및 c + +)
description: 이 항목에서는 DirectX를 사용 하 여 비동기 프로그래밍 및 스레딩을 사용 하는 경우 고려해 야 할 다양 한 사항을 설명 합니다.
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 비동기 프로그래밍, directx
ms.localizationpriority: medium
ms.openlocfilehash: e5a8eb1ee9cd6a7b5a00eaf13bf04d8956df42c5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172067"
---
# <a name="asynchronous-programming-directx-and-c"></a>비동기 프로그래밍 (DirectX 및 c + +)



이 항목에서는 DirectX를 사용 하 여 비동기 프로그래밍 및 스레딩을 사용 하는 경우 고려해 야 할 다양 한 사항을 설명 합니다.

## <a name="async-programming-and-directx"></a>비동기 프로그래밍 및 DirectX


DirectX에 대 한 학습 또는 경험이 있는 경우에도 모든 그래픽 처리 파이프라인을 하나의 스레드에 배치 하는 것이 좋습니다. 게임의 지정 된 장면에는 비트맵, 셰이더 및 독점적인 액세스를 필요로 하는 기타 자산과 같은 일반적인 리소스가 있습니다. 이러한 동일한 리소스를 사용 하려면 병렬 스레드 전체에서 이러한 리소스에 대 한 액세스를 동기화 해야 합니다. 렌더링은 여러 스레드 간에 병렬화 하는 어려운 프로세스입니다.

그러나 게임이 충분히 복잡 하거나 향상 된 성능을 제공 하려는 경우에는 비동기 프로그래밍을 사용 하 여 렌더링 파이프라인에 한정 되지 않은 일부 구성 요소를 병렬화 할 수 있습니다. 최신 하드웨어는 여러 코어 및 하이퍼 스레드 Cpu를 사용 하 고 앱은이를 활용 해야 합니다. 다음과 같이 Direct3D 장치 컨텍스트에 직접 액세스할 필요가 없는 게임의 일부 구성 요소에 대해 비동기 프로그래밍을 사용 하 여이를 확인할 수 있습니다.

-   파일 I/O
-   물리
-   AI
-   네트워킹
-   오디오
-   controls
-   XAML 기반 UI 구성 요소

앱은 여러 동시 스레드에서 이러한 구성 요소를 처리할 수 있습니다. 게임 또는 앱이 여러 개의 (또는 수백) 메가바이트의 자산이 로드 되거나 스트리밍되는 동안 게임 또는 앱이 대화형 상태가 될 수 있으므로 파일 i/o, 특히 자산 로드는 비동기 로드에서 크게 향상 됩니다. 이러한 스레드를 만들고 관리 하는 가장 쉬운 방법은 Ppltasks.h에 정의 된 **concurrency** 네임 스페이스에 포함 된 것 처럼 [병렬 패턴 라이브러리](/cpp/parallel/concrt/parallel-patterns-library-ppl) 및 **작업** 패턴을 사용 하는 것입니다. [병렬 패턴 라이브러리](/cpp/parallel/concrt/parallel-patterns-library-ppl) 를 사용 하면 여러 코어 및 하이퍼 스레드 cpu를 직접 활용 하 고 cpu 계산 또는 네트워크 처리가 많이 발생 하는 hitches 및 지연에 이르기까지 인식 되는 로드 시간까지 모든 것을 향상할 수 있습니다.

> **참고**    UWP (유니버설 Windows 플랫폼) 앱에서 사용자 인터페이스는 완전히 STA (단일 스레드 아파트)로 실행 됩니다. [XAML interop](directx-and-xaml-interop.md)를 사용 하 여 DirectX 게임에 대 한 UI를 만드는 경우에는 STA를 사용 해야만 컨트롤에 액세스할 수 있습니다.

 

## <a name="multithreading-with-direct3d-devices"></a>Direct3D 장치를 사용 하는 다중 스레딩


장치 컨텍스트에 대 한 다중 스레딩은 11 0 이상의 Direct3D 기능 수준을 지 원하는 그래픽 장치 에서만 사용할 수 있습니다 \_ . 그러나 전용 게임 플랫폼과 같은 많은 플랫폼에서 강력한 GPU를 최대한 활용 하는 것이 좋습니다. 가장 간단한 경우는 3D 장면 렌더링 및 프로젝션에서 HUD (헤드 표시) 오버레이의 렌더링을 분리 하 고 두 구성 요소 모두 별도의 병렬 파이프라인을 사용 하도록 할 수 있습니다. 두 스레드는 모두 동일한 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 를 사용 하 여 리소스 개체 (질감, 메시, 셰이더 및 기타 자산)를 만들고 관리 해야 합니다. 단, 단일 스레드 이며, 안전 하 게 액세스 하기 위해 일종의 동기화 메커니즘 (예: 임계 영역)을 구현 해야 하는 경우입니다. 다른 스레드에서 장치 컨텍스트에 대 한 별도의 명령 목록을 만들 수 있지만 (지연 된 렌더링의 경우) 동일한 **ID3D11DeviceContext** 인스턴스에서 동시에 해당 명령 목록을 재생할 수 없습니다.

이제 앱에서 다중 스레딩을 사용 하는 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)를 사용 하 여 리소스 개체를 만들 수도 있습니다. 따라서 항상 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)대신 **ID3D11Device** 를 사용 하지 않는 이유는 무엇 인가요? 현재 다중 스레딩에 대 한 드라이버 지원은 일부 그래픽 인터페이스에 사용 하지 못할 수 있습니다. 장치를 쿼리하여 다중 스레딩을 지원 하는지 확인할 수 있지만, 가장 광범위 한 대상에 도달 하려는 경우 리소스 개체 관리를 위해 단일 스레드 **ID3D11DeviceContext** 를 사용할 수 있습니다. 즉, 그래픽 장치 드라이버가 다중 스레딩 또는 명령 목록을 지원 하지 않는 경우 Direct3D 11은 내부적으로 장치 컨텍스트에 대 한 동기화 된 액세스를 처리 하려고 합니다. 그리고 명령 목록이 지원 되지 않는 경우 소프트웨어 구현을 제공 합니다. 따라서 다중 스레드 장치 컨텍스트 액세스에 대 한 드라이버 지원이 없는 그래픽 인터페이스를 사용 하 여 플랫폼에서 실행 되는 다중 스레드 코드를 작성할 수 있습니다.

앱에서 명령 목록을 처리 하는 별도의 스레드를 지원 하 고 프레임을 표시 하는 경우, 크게 끊길 또는 lag 없이 적절 한 시간 내에 프레임을 표시 하는 동시에 GPU를 활성 상태로 유지 하 고 명령 목록을 처리 하는 것이 좋습니다. 이 경우 각 스레드에 대해 별도의 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 를 사용 하 고 D3D11 \_ 리소스 \_ 기타 공유 플래그를 사용 하 여 리소스 (예: 질감)를 공유할 수 있습니다 \_ . 이 시나리오에서 표시 스레드의 리소스 개체 처리 결과를 표시 하기 전에 처리 스레드에서 [**ID3D11DeviceContext:: Flush**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-flush) 를 호출 하 여 명령 목록 실행을 완료 해야 합니다.

## <a name="deferred-rendering"></a>지연 된 렌더링


지연 된 렌더링은 명령 목록에 그래픽 명령을 기록 하 여 다른 시간에 재생할 수 있도록 하 고, 추가 스레드에 렌더링 하기 위한 명령을 기록 하는 동안 한 스레드에서 렌더링을 지원 하도록 디자인 되었습니다. 이러한 명령이 완료 된 후 최종 표시 개체 (프레임 버퍼, 질감 또는 기타 그래픽 출력)를 생성 하는 스레드에서 실행 될 수 있습니다.

직접 컨텍스트를 만드는 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 또는 [**D3D11CreateDeviceAndSwapChain**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain)대신 [**ID3D11Device:: CreateDeferredContext**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createdeferredcontext) 를 사용 하 여 지연 된 컨텍스트를 만듭니다. 자세한 내용은 [직접 및 지연 렌더링](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-render)을 참조 하세요.

## <a name="related-topics"></a>관련 항목


* [Direct3D 11의 다중 스레딩 소개](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)

 

 