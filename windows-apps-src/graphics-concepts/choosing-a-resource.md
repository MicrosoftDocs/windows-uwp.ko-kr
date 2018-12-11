---
title: 리소스 선택
description: 리소스는 3D 파이프라인에서 사용하는 데이터의 모음입니다.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- 리소스 선택
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ccc99395dba2f2d1894db81fb48abb59f9a8ba4f
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918842"
---
# <a name="choosing-a-resource"></a>리소스 선택


리소스는 3D 파이프라인이 사용하는 데이터의 모음입니다. 리소스를 만들고 리소스의 동작을 정의하는 것이 응용 프로그램 프로그래밍의 첫 단계입니다. 이 가이드에서는 응용 프로그램에 필요한 리소스 선택에 대한 기본 항목을 다룹니다.

## <a name="span-ididentifybindingspanspan-ididentifybindingspanspan-ididentifybindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>리소스가 필요한 파이프라인 단계 식별


첫 번째 단계는 리소스를 사용할 [그래픽 파이프라인](graphics-pipeline.md) 단계를 하나 이상 선택하는 것입니다. 즉, 리소스에서 데이터를 읽을 각 단계와 리소스에 데이터를 쓸 단계를 식별합니다. 리소스가 사용될 파이프라인 단계를 알면 단계에 리소스를 바인딩하기 위해 호출되는 API가 결정됩니다.

다음 표에는 각 파이프라인 단계에 바인딩할 수 있는 리소스의 유형이 나열되어 있습니다. 여기에는 리소스를 입력 또는 출력으로 바인딩할 수 있는지 여부가 포함됩니다.

| 파이프라인 단계  | 입/출력 | 리소스               | 리소스 유형                           |
|-----------------|--------|------------------------|-----------------------------------------|
| 입력 어셈블러 | 입력     | 꼭짓점 버퍼          | 버퍼                                  |
| 입력 어셈블러 | 입력     | 인덱스 버퍼           | 버퍼                                  |
| 셰이더 단계   | 입력     | 셰이더 리소스 뷰    | 버퍼, Texture1D, Texture2D, Texture3D |
| 셰이더 단계   | 입력     | 셰이더 상수 버퍼 | 버퍼                                  |
| 스트림 출력   | 출력    | 버퍼                 | 버퍼                                  |
| 출력 병합기   | 출력    | 렌더링 대상 뷰     | 버퍼, Texture1D, Texture2D, Texture3D |
| 출력 병합기   | 출력    | 깊이/스텐실 뷰     | Texture1D, Texture2D                    |

 

## <a name="span-ididentifyusagespanspan-ididentifyusagespanspan-ididentifyusagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>각 리소스가 사용되는 방법 식별


응용 프로그램에서 사용할 파이프라인 단계와 각 단계에 필요한 리소스를 선택했으면 다음 단계로 각 리소스의 사용 방법, 즉 CPU에서 리소스에 액세스할지 아니면 GPU에서 리소스에 액세스할지를 결정합니다.

응용 프로그램이 실행되고 있는 하드웨어에는 CPU와 GPU가 각각 하나 이상 있습니다. 사용법 값을 선택하려면 다음 옵션 중 리소스를 읽거나 써야 하는 프로세서 유형을 고려하세요.

| 리소스 사용법 | 다음에 의해 업데이트 가능                    | 업데이트 빈도 |
|----------------|--------------------------------------|---------------------|
| 기본        | GPU                                  | 빈도 낮음        |
| 동적        | CPU                                  | 빈도 높음          |
| 준비        | GPU                                  | 해당 없음                 |
| 변경 불가능      | CPU(리소스 작성 시간) | 해당 없음                 |

 

CPU에서 자주 업데이트하지 않을 것으로 예상(프레임당 1번 미만)되는 리소스에는 기본 사용법이 필요합니다. 성능 저하를 피하기 위해 CPU가 기본 사용법의 리소스에 직접 쓰기 않는 것이 가장 좋습니다.

CPU에서 상대적으로 자주 업데이트(프레임당 1번 이상)하는 리소스에는 동적 사용법이 필요합니다. 동적 리소스의 일반적인 시나리오는 런타임 시 각 프레임에 대한 사용자의 관점에서 보이는 기하 도형 관련 데이터로 채워지는 동적 꼭짓점과 인덱스 버퍼를 만드는 것입니다. 이러한 버퍼는 해당 버퍼에 대해 사용자에게 보이는 기하 도형을 렌더링하는 데만 사용됩니다.

다른 리소스와 데이터를 복사하는 데는 준비 사용법이 필요합니다. 일반적인 시나리오는 기본 사용법의 리소스(CPU에서 액세스할 수 없음) 데이터를 준비 사용법의 리소스(CPU에서 액세스할 수 있음)에 복사하는 것입니다.

리소스의 데이터가 절대 변경되지 않을 경우 변경 불가능 리소스를 사용해야 합니다.

같은 아이디어를 보는 다른 방법은 응용 프로그램에서 리소스로 수행하는 작업을 생각하는 것입니다.

| 응용 프로그램에서 리소스를 사용하는 방법     | 리소스 사용법       |
|---------------------------------------|----------------------|
| 한 번 로드 또는 업데이트 안 함            | 변경 불가능 또는 기본 |
| 응용 프로그램이 반복해서 리소스를 채움 | 동적              |
| 텍스처로 렌더링                     | 기본              |
| GPU 데이터의 CPU 액세스                | 준비              |

 

어떤 사용법을 선택할지 확실하지 않을 경우 가장 일반적인 경우인 기본 사용법으로 시작합니다. 셰이더 상수 버퍼는 항상 기본 사용법을 사용해야 하는 한 리소스 유형입니다.

## <a name="span-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>파이프라인 단계에 리소스 바인딩


리소스 작성 시 지정된 제한 사항을 충족하는 경우 여러 파이프라인에 한 리소스를 동시에 바인딩할 수 있습니다. 이러한 제한 사항은 사용 플래그, 바인딩 플래그 또는 CPU 액세스 플래그로 지정됩니다. 보다 구체적으로 말하면, 리소스의 일부를 동시에 읽고 쓸 수 없는 경우 입력과 출력으로 리소스를 동시에 바인딩할 수 있습니다.

리소스를 바인딩할 때 GPU와 CPU가 리소스를 액세스하는 방법에 대해 생각합니다. 하나의 용도로 설계된 리소스(다중 사용, 바인딩 및 CPU 액세스 플래그 사용 안 함)의 성능이 더 좋을 가능성이 높습니다.

예를 들어, 텍스처로 여러 번 사용된 렌더링 대상의 경우를 고려합니다. 렌더링 대상과 셰이더 리소스로 사용되는 텍스처, 이렇게 2개의 리소스를 사용하는 것이 더 빠를 수 있습니다. 리소스마다 "렌더링 대상" 또는 "셰이더 리소스"를 나타내는 하나의 바인딩 플래그만 있습니다. 렌더링 대상에서 셰이더 텍스처로 데이터가 복사됩니다.

이 예제에서 이 기술은 셰이더 텍스처 읽기에서 렌더링 대상 쓰기를 분리하여 성능을 향상시킬 수 있습니다. 두 방법을 모두 구현하고 특정 응용 프로그램에서의 성능 차이를 측정하는 것만이 확실한 방법입니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[리소스](resources.md)

 

 




