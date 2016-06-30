---
author: mtoepke
title: "비동기 프로그래밍(DirectX 및 C++)"
description: "이 항목에서는 비동기 프로그래밍과 스레딩을 DirectX와 함께 사용할 때 고려할 여러 사항에 대해 설명합니다."
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 67a75e9d1324e7ac50e0575bfd7bda870a87efb2

---

# 비동기 프로그래밍(DirectX 및 C++)


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 항목에서는 비동기 프로그래밍과 스레딩을 DirectX와 함께 사용할 때 고려할 여러 사항에 대해 설명합니다.

## 비동기 프로그래밍 및 DirectX


DirectX를 처음 배우는 사용자인 경우 또는 숙련된 사용자라도 모든 그래픽 처리 파이프라인을 하나의 스레드에 배치하는 것이 좋습니다. 지정된 모든 게임 장면에는 비트맵, 셰이더 및 배타적 액세스가 필요한 기타 자산과 같은 일반 리소스가 있습니다. 이 동일한 리소스에서 리소스에 대한 모든 액세스를 병렬 스레드에 동기화해야 합니다. 렌더링은 여러 스레드에서 병렬화하기 어려운 프로세스입니다.

하지만 게임이 충분히 복잡하거나 성능을 향상시키려는 경우 비동기 프로그래밍을 사용하여 렌더링 파이프라인과 관련이 없는 일부 구성 요소를 병렬화할 수 있습니다. 최신 하드웨어는 여러 코어 및 하이퍼 스레드 CPU를 제공하며 앱에서 이 기능을 활용할 수 있습니다. 이렇게 하려면 다음 Direct3D 디바이스 컨텍스트에 대한 직접 액세스가 필요 없는 게임의 일부 구성 요소에 비동기 프로그래밍을 사용합니다.

-   파일 I/O
-   물리학
-   AI
-   네트워킹
-   오디오
-   컨트롤
-   XAML 기반 UI 구성 요소

앱은 여러 동시 스레드에서 이러한 구성 요소를 처리할 수 있습니다. 수 메가바이트(또는 수백 메가바이트)의 자산을 로드하거나 스트리밍하는 동안 게임 또는 앱이 대화형 상태일 수 있으므로 파일 I/O, 특히 자산 로드에는 비동기 로드가 큰 도움이 됩니다. 이러한 스레드를 만들고 관리하는 가장 쉬운 방법은 PPLTasks.h에 정의된 **concurrency** 네임스페이스에 포함된 대로 [병렬 패턴 라이브러리](https://msdn.microsoft.com/library/dd492418.aspx)와 **task** 패턴을 사용하는 것입니다. [병렬 패턴 라이브러리](https://msdn.microsoft.com/library/dd492418.aspx)를 사용하면 여러 코어 및 하이퍼 스레드 CPU가 직접 활용되며 인식된 로드 시간에서 CPU 계산이나 네트워크 처리가 많아서 발생하는 지연 및 문제에 이르기까지 모든 기능이 향상될 수 있습니다.

> **참고** UWP(유니버설 Windows 플랫폼) 앱의 사용자 인터페이스는 완전히 STA(단일 스레드 아파트)에서 실행됩니다. [XAML interop](directx-and-xaml-interop.md)를 사용하여 DirectX 게임에 대한 UI를 만드는 경우 STA를 사용하여 컨트롤에만 액세스할 수 있습니다.

 

## Direct3D 장치를 사용한 다중 스레딩


디바이스 컨텍스트에 대한 다중 스레딩은 Direct3D 기능 수준 11\_0 이상을 지원하는 그래픽 디바이스에서만 사용할 수 있습니다. 하지만 전용 게임 플랫폼 등의 많은 플랫폼에서 강력한 GPU 사용을 최대화하는 것이 좋습니다. 가장 간단한 사례에서는 HUD(헤즈업 표시) 오버레이의 렌더링을 3D 장면 렌더링 및 프로젝션에서 분리하고 두 구성 요소가 모두 별개의 병렬 파이프라인을 사용하도록 하는 것이 좋습니다. 두 스레드는 동일한 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)를 사용하여 단일 스레드이고, 안전하게 액세스하기 위해 일종의 동기화 메커니즘(예: 임계 영역)을 구현해야 하는 리소스 개체(텍스처, 메시, 셰이더 및 기타 자산)를 만들고 관리해야 합니다. 또한 지연 렌더링을 위해 각 스레드에 디바이스 컨텍스트에 대한 별도의 명령 목록을 만들 수 있지만 동일한 **ID3D11DeviceContext** 인스턴스에서 이러한 명령 목록을 동시에 재생할 수는 없습니다.

이제 앱에서 다중 스레딩에 안전한 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)를 사용하여 리소스 개체를 만들 수도 있습니다. 따라서 항상 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 대신 **ID3D11Device**를 사용해 보세요. 현재 다중 스레딩에 대한 드라이버 지원은 일부 그래픽 인터페이스에 사용할 수 없습니다. 장치를 쿼리하고 다중 스레딩을 지원하는지 확인할 수도 있지만 가장 광범위한 사용자에 도달하려는 경우 단일 스레드 **ID3D11DeviceContext**를 리소스 개체 관리에 사용하는 것이 좋습니다. 즉, 그래픽 장치 드라이버가 다중 스레딩 또는 명령 목록을 지원하지 않는 경우 Direct3D 11은 디바이스 컨텍스트에 대한 동기화된 액세스를 내부적으로 처리하며, 명령 목록이 지원되지 않는 경우 소프트웨어 구현을 제공합니다. 따라서 다중 스레드 디바이스 컨텍스트 액세스에 대한 드라이버 지원이 없는 그래픽 인터페이스가 있는 플랫폼에서 실행될 다중 스레드 코드를 작성할 수 있습니다.

앱에서 명령 목록을 처리하고 프레임을 표시하는 별개의 스레드를 지원하는 경우 GPU를 활성 상태로 유지하고, 인식할 수 있는 스터터나 지연 없이 프레임을 적시에 표시하는 동시에 명령 목록을 처리하는 것이 좋습니다. 이 경우 각 스레드에 별개의 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)를 사용하고 D3D11\_RESOURCE\_MISC\_SHARED 플래그로 리소스를 만들어 텍스처 등의 리소스를 공유할 수 있습니다. 이 시나리오에서는 리소스 개체의 처리 결과를 표시 스레드에 표시하기 전에 처리 스레드에서 [**ID3D11DeviceContext::Flush**](https://msdn.microsoft.com/library/windows/desktop/ff476425)를 호출하여 명령 목록의 실행을 완료해야 합니다.

## 지연 렌더링


지연 렌더링은 다른 시간에 재생할 수 있도록 명령 목록에 그래픽 명령을 기록하며 한 스레드에서 렌더링을 지원하는 동시에 추가 스레드에서 렌더링할 명령을 기록합니다. 이러한 명령은 완료된 후 최종 표시 개체(프레임 버퍼, 텍스처 또는 다른 그래픽 출력)를 생성하는 스레드에서 실행할 수 있습니다.

즉각적인 컨텍스트를 만드는 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 또는 [**D3D11CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083) 대신 [**ID3D11Device::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/ff476505)를 사용하여 지연 컨텍스트를 만듭니다. 자세한 내용은 [즉시 및 지연 렌더링](https://msdn.microsoft.com/library/windows/desktop/ff476892)을 참조하세요.

## 관련 항목


* [Direct3D 11의 다중 스레딩 소개](https://msdn.microsoft.com/library/windows/desktop/ff476891)

 

 







<!--HONumber=Jun16_HO4-->


