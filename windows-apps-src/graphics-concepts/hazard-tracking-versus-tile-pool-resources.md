---
title: 위험 추적 및 타일 풀 리소스
description: 비 스트리밍 리소스의 경우, Direct3D는 렌더링하는 동안 특정 위험 조건을 예방할 수 있지만, 위험 추적이 스트리밍 리소스에 대해 타일 수준에 있기 때문에 스트리밍 리소스 렌더링 중 위험 조건 추적에 너무 많은 리소스가 필요할 수 있습니다.
ms.assetid: 8B0C73D3-3F77-41E8-B17D-C595DEE39E49
keywords:
- 위험 추적 및 타일 풀 리소스
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e2ff4e2ff079e15c0af41e3232c4b70c582a6a25
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5553698"
---
# <a name="hazard-tracking-versus-tile-pool-resources"></a>위험 추적 및 타일 풀 리소스


비 스트리밍 리소스의 경우, Direct3D는 렌더링하는 동안 특정 위험 조건을 예방할 수 있지만, 위험 추적이 스트리밍 리소스에 대해 타일 수준에 있기 때문에 스트리밍 리소스 렌더링 중 위험 조건 추적에 너무 많은 리소스가 필요할 수 있습니다.

예를 들어, 비 스트리밍 리소스에 대해 런타임은 입력(예: 셰이더 리소스 보기)과 출력(예: 렌더링 대상 보기)에 어떤 SubResource도 동시에 바인딩하는 것을 허용하지 않습니다. 이러한 상황이 발생하면 런타임이 입력을 바인딩 해제합니다. 런타임의 이 추적은 오버헤드가 적고 하위 리소스 수준에서 수행됩니다. 이 추적 오버헤드의 장점 중 하나는 하드웨어 셰이더 실행 순서에 따라 실수로 응용 프로그램이 위험에 빠지는 경우를 최소화한다는 것입니다. 하드웨어 셰이더 실행 순서는 GPU(그래픽 처리 장치)에 지정되어 있지 않을 경우 다를 수 있으므로, GPU마다 다를 수 있습니다.

스트리밍 리소스에 대해 리소스가 어떻게 바인딩되었는지 추적하는 것은 추적이 타일 수준이기 때문에 리소스가 너무 많이 필요할 수 있습니다. 하나의 타일을 표면의 여러 영역에 동시에 매핑하여 렌더링 대상 보기로 렌더링하려는 시도를 유효성 검사해야 하는 것과 같은 새로운 문제도 발생합니다. 타일별 위험 추적이 런타임에 너무 많은 리소스가 소모된다면, 적어도 디버그 계층에서의 옵션으로는 사용하는 것이 좋습니다.

응용 프로그램은 타일 풀 메모리를 참조하는 스트리밍 리소스에 쓰기 또는 읽기 작업을 발행할 경우, 향후 읽기 또는 쓰기 작업의 별도의 스트리밍 리소스가 같은 타일 풀 메모리를 참조하고 첫 번째 작업이 완료되기 전에 이어지는 작업이 시작될 것으로 예상한다면 디스플레이 드라이버에 이를 알려야 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[타일 풀에 대한 매핑](mappings-are-into-a-tile-pool.md)

 

 




