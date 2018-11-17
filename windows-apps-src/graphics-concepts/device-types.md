---
title: 디바이스 유형
description: Direct3D 디바이스 유형에는 hal(하드웨어 추상화 계층) 디바이스와 기준 래스터라이저가 포함됩니다.
ms.assetid: 64084B23-10C0-4541-8E93-FB323385D2F0
keywords:
- 디바이스 유형
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cbf7d984226984391da340c74791dad4a6c0d8fb
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7156148"
---
# <a name="device-types"></a>디바이스 유형


Direct3D 디바이스 유형에는 hal(하드웨어 추상화 계층) 디바이스와 기준 래스터라이저가 포함됩니다.

## <a name="span-idhaldevicespanspan-idhaldevicespanspan-idhaldevicespanhal-device"></a><span id="HAL_Device"></span><span id="hal_device"></span><span id="HAL_DEVICE"></span>HAL 디바이스


기본 디바이스 유형은 hal 디바이스로, 하드웨어 가속 래스터화와 하드웨어 및 소프트웨어 꼭짓점 처리를 모두 지원합니다. 응용 프로그램이 실행 중인 컴퓨터에 Direct3D를 지원하는 디스플레이 어댑터가 장착되어 있는 경우, 응용 프로그램에서 Direct3D 작업에 이를 사용해야 합니다. Direct3D hal 디바이스는 하드웨어에서 변환, 조명 및 래스터화 모듈의 전부 또는 일부를 구현합니다.

응용 프로그램은 그래픽 어댑터에 직접 액세스하지 않습니다. 이는 Direct3D 함수 및 메서드를 호출합니다. Direct3D는 hal을 통해 하드웨어에 액세스합니다. 응용 프로그램을 실행 중인 컴퓨터가 hal을 지원하는 경우 hal 디바이스를 사용하여 최상의 성능을 얻을 것입니다.

## <a name="span-idreferencedevicespanspan-idreferencedevicespanspan-idreferencedevicespanreference-device"></a><span id="Reference_Device"></span><span id="reference_device"></span><span id="REFERENCE_DEVICE"></span>참조 디바이스


Direct3D 참조 디바이스 또는 기준 래스터라이저라고 불리는 추가 디바이스 유형을 지원합니다. 소프트웨어 디바이스와 달리, 기준 래스터라이저는 모든 Direct3D 기능을 지원합니다. 이 디바이스는 디버깅 목적으로 사용하기 위한 것이므로 SDK DirectX가 설치된 컴퓨터에서만 사용할 수 있습니다. 이러한 기능은 속도가 아닌 정확도를 위해 구현되고 소프트웨어에서 구현되기 때문에, 결과가 매우 빠르지는 않습니다. 기준 래스터라이저는 가능할 때마다 특별한 CPU 명령을 사용하지만, 이는 소매용 응용 프로그램을 위해 의도된 것은 아닙니다. 기능 테스트 또는 데모 목적으로만 기준 래스터라이저를 사용하십시오.

## <a name="span-idhalvsrefspanspan-idhalvsrefspanspan-idhalvsrefspanhal-vs-ref-devices"></a><span id="HAL_vs_REF"></span><span id="hal_vs_ref"></span><span id="HAL_VS_REF"></span>HAL과 REF 디바이스 비교


HAL(하드웨어 추상화 계층) 디바이스와 REF(기준 래스터라이저) 디바이스는 Direct3D의 두 가지 주요 디바이스 유형입니다. 첫 번째는 하드웨어 지원을 기반으로 하며 매우 빠르지만 모든 것을 지원하지는 않습니다. 두 번째는 하드웨어 가속화를 사용하지 않으므로 아주 느리지만, 모든 Direct3D 기능을 올바른 방식으로 지원하는 것이 보장됩니다. 일반적으로는 HAL 디바이스만 필요할 것입니다. 하지만 그래픽 카드가 지원하지 않는 일부 고급 기능을 사용하는 경우 REF로 대체해야 할 수 있습니다.

REF 사용이 필요한 다른 경우는 HAL 디바이스가 이상한 결과를 생성하는 경우입니다. 즉 소스 코드가 정확하다고 확신하지만 예상한 결과가 나오지 않는 경우입니다. REF 디바이스는 올바르게 동작하도록 보장되므로, REF 디바이스에서 애플리케이션을 테스트하고 이상한 동작이 계속되는지 확인할 수 있습니다. REF로 대체하여 정상적으로 작동한다면 (a) 응용 프로그램이 그래픽 카드가 지원하지 않는 기능을 사용하거나 (b) 드라이버 버그입니다. REF 디바이스로도 계속 작동하지 않는다면 응용 프로그램 버그입니다.

## <a name="span-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanhardware-vs-software-vertex-processing"></a><span id="Hardware_vs_Software"></span><span id="hardware_vs_software"></span><span id="HARDWARE_VS_SOFTWARE"></span>하드웨어와 소프트웨어 꼭짓점 처리 비교


하드웨어 대 소프트웨어 꼭짓점 처리는 HAL 디바이스에만 적용됩니다. 파이프라인을 통해 꼭짓점을 보내면, 변환(월드, 보기 및 프로젝션 매트릭스에 의해 차례로)되고, 조명 처리(D3D의 기본 제공 조명 사용)됩니다. 이 처리 단계를 T&L(변환 및 조명 처리)이라고 합니다. 하드웨어 꼭짓점 처리는 하드웨어에서 지원하는 경우 하드웨어에서 진행되는 것을 의미하며, 소프트웨어 꼭짓점 처리는 소프트웨어에서 진행됩니다. 일반적인 관행은 하드웨어 T&L 디바이스 생성을 시도해 보고, 실패할 경우 소프트웨어 방식을 시도하는 것입니다. (소프트웨어도 실패하면 포기하고 오류와 함께 종료합니다.)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[장치](devices.md)

 

 




