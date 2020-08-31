---
title: 리소스 선택
description: 응용 프로그램에서 3D 그래픽 파이프라인의 여러 단계로 바인딩할 최상의 리소스를 식별 하 고 선택 하는 방법을 알아봅니다.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- 리소스 선택
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69527915988136854e201b82332c17f3e0134290
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094471"
---
# <a name="choosing-a-resource"></a>리소스 선택


리소스는 3D 파이프라인에서 사용 하는 데이터의 컬렉션입니다. 응용 프로그램 프로그래밍을 위한 첫 번째 단계는 리소스를 만들고 해당 동작을 정의 하는 것입니다. 이 가이드에서는 응용 프로그램에 필요한 리소스를 선택 하기 위한 기본 항목을 설명 합니다.

## <a name="span-ididentify_bindingspanspan-ididentify_bindingspanspan-ididentify_bindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>리소스를 필요로 하는 파이프라인 단계 식별


첫 번째 단계는 리소스를 사용 하는 [그래픽 파이프라인](graphics-pipeline.md) 단계를 선택 하는 것입니다. 즉, 리소스에서 데이터를 읽을 각 단계와 리소스에 데이터를 기록 하는 단계를 식별 합니다. 리소스를 사용 하는 파이프라인 단계를 알면 리소스를 단계에 바인딩하기 위해 호출 되는 Api를 결정 합니다.

다음 표에서는 각 파이프라인 단계에 바인딩할 수 있는 리소스의 유형을 보여 줍니다. 리소스를 입력 또는 출력으로 바인딩할 수 있는지 여부를 포함 합니다.

| 파이프라인 단계  | 입/출력 | 리소스               | 리소스 종류                           |
|-----------------|--------|------------------------|-----------------------------------------|
| 입력 어셈블러 | In(다음 안에)     | 꼭 짓 점 버퍼          | Buffer                                  |
| 입력 어셈블러 | In(다음 안에)     | 인덱스 버퍼           | Buffer                                  |
| 셰이더 단계   | In(다음 안에)     | 셰이더-ResourceView    | Buffer, Texture1D, Texture2D, Texture3D |
| 셰이더 단계   | In(다음 안에)     | 셰이더-상수 버퍼 | Buffer                                  |
| 스트림 출력   | 아웃    | Buffer                 | Buffer                                  |
| 출력 병합기   | 아웃    | 렌더링 대상 뷰     | Buffer, Texture1D, Texture2D, Texture3D |
| 출력 병합기   | 아웃    | 깊이/스텐실 뷰     | Texture1D, Texture2D                    |

 

## <a name="span-ididentify_usagespanspan-ididentify_usagespanspan-ididentify_usagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>각 리소스를 사용 하는 방법 식별


응용 프로그램에서 사용할 파이프라인 단계를 선택 하 고 (따라서 각 단계에 필요한 리소스), 다음 단계는 각 리소스가 사용 되는 방법, 즉 CPU 또는 GPU에서 리소스에 액세스할 수 있는지 여부를 결정 하는 것입니다.

응용 프로그램이 실행 되는 하드웨어에는 최소 하나 이상의 CPU와 하나의 GPU가 포함 됩니다. 사용 값을 선택 하려면 다음 옵션을 사용 하 여 리소스에 대 한 읽기 또는 쓰기를 수행 해야 하는 프로세서 유형을 고려해 야 합니다.

| Resource Usage | 업데이트 가능                    | 업데이트 빈도 |
|----------------|--------------------------------------|---------------------|
| 기본값        | GPU                                  | 자주        |
| 동적        | CPU                                  | 자주          |
| 준비        | GPU                                  | 해당 없음                 |
| 변경할 수 없음      | CPU (리소스를 만들 때만) | 해당 없음                 |

 

CPU에서 자주 업데이트 될 것으로 예상 되는 리소스 (프레임당 한 번이 하)에는 기본 사용을 사용 해야 합니다. 이상적으로 CPU는 잠재적 성능 저하를 방지 하기 위해 기본 사용으로 리소스에 직접 쓰지 않습니다.

동적 사용은 CPU가 비교적 자주 업데이트 하는 리소스 (프레임당 한 번 이상)에 사용 해야 합니다. 동적 리소스에 대 한 일반적인 시나리오는 런타임에 각 프레임에 대 한 사용자의 관점에서 표시 되는 기 하 도형에 대 한 데이터를 사용 하 여 런타임에 채워지는 동적 꼭 짓 점 및 인덱스 버퍼를 만드는 것입니다. 이러한 버퍼는 해당 프레임의 사용자에 게 표시 되는 기 하 도형만 렌더링 하는 데 사용 됩니다.

스테이징 사용은 다른 리소스 간에 데이터를 복사 하는 데 사용 해야 합니다. 일반적인 시나리오는 기본 사용 (CPU가 액세스할 수 없음)을 사용 하 여 리소스의 데이터를 스테이징 사용 (CPU에서 액세스할 수 있음)이 있는 리소스에 복사 하는 것입니다.

리소스의 데이터가 변경 되지 않는 경우에는 변경할 수 없는 리소스를 사용 해야 합니다.

동일한 개념을 확인 하는 또 다른 방법은 응용 프로그램에서 리소스에 대해 수행 하는 작업을 고려 하는 것입니다.

| 응용 프로그램에서 리소스를 사용 하는 방법     | Resource Usage       |
|---------------------------------------|----------------------|
| 한 번 로드 및 업데이트 안 함            | 변경할 수 없음 또는 기본값 |
| 응용 프로그램이 리소스를 반복 해 서 채우기 | 동적              |
| 질감으로 렌더링                     | 기본값              |
| GPU 데이터의 CPU 액세스                | 준비              |

 

어떤 용도를 선택 하는 것이 확실 하지 않은 경우 가장 일반적인 경우로 예상 되는 기본 사용으로 시작 합니다. 셰이더 상수 버퍼는 항상 기본 사용을 가져야 하는 하나의 리소스 형식입니다.

## <a name="span-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>파이프라인 단계에 리소스 바인딩


리소스를 만들 때 지정 된 제한 사항이 충족 되는 한 한 번에 둘 이상의 파이프라인 단계에 리소스를 바인딩할 수 있습니다. 이러한 제한 사항은 사용 플래그, 바인드 플래그 또는 cpu 액세스 플래그로 지정 됩니다. 구체적으로 말해서 리소스의 읽기 및 쓰기를 동시에 수행할 수 없으면 리소스를 입력 및 출력으로 동시에 바인딩할 수 있습니다.

리소스를 바인딩할 때 GPU 및 CPU가 리소스에 액세스 하는 방법을 고려 합니다. 단일 용도로 설계 된 리소스 (여러 사용, 바인딩 및 cpu 액세스 플래그를 사용 하지 않음)가 더 나은 성능을 얻을 수 있습니다.

예를 들어 렌더링 대상이 질감으로 여러 번 사용 되는 경우를 고려해 보세요. 두 리소스를 사용 하는 것이 더 빠를 수 있습니다. 렌더링 대상과 셰이더 리소스로 사용 되는 질감입니다. 각 리소스는 "렌더링 대상" 또는 "셰이더 리소스"를 나타내는 바인드 플래그를 하나만 사용 합니다. 데이터는 렌더링 대상 질감에서 셰이더 질감으로 복사 됩니다.

이 예제의이 기법은 셰이더-질감 읽기에서 렌더링 대상 쓰기를 격리 하 여 성능을 향상 시킬 수 있습니다. 두 가지 방법을 모두 구현 하 고 특정 응용 프로그램의 성능 차이를 측정 하는 방법만을 확인할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[리소스](resources.md)

 

 




