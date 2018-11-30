---
title: PS(픽셀 셰이더) 단계
description: 'PS(픽셀 셰이더) 단계는 기본 요소에 대한 보간된 데이터를 받아 픽셀별 데이터(예: 색)를 생성합니다.'
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- PS(픽셀 셰이더) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e1f7e787f2ee80a3168d38a9afd9a249dc0e6de0
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8329173"
---
# <a name="pixel-shader-ps-stage"></a>PS(픽셀 셰이더) 단계


PS(픽셀 셰이더) 단계는 기본 요소에 대한 보간된 데이터를 받아 픽셀별 데이터(예: 색)를 생성합니다.

이것은 프로그램 가능 셰이더 단계로서 [그래픽 파이프라인](graphics-pipeline.md) 다이어그램에서 둥근 블록으로 표시됩니다. 이 셰이더 단계는 셰이더 모델 4.0 [공통 셰이더 코어](https://msdn.microsoft.com/library/windows/desktop/bb509580)에 기반하는 고유의 기능을 노출합니다.

PS(픽셀 셰이더) 단계로 픽셀별 조명과 후처리 같은 풍부한 음영 기술을 사용할 수 있습니다. 픽셀 셰이더는 상수, 텍스처 데이터, 보간된 꼭짓점별 값 및 기타 데이터를 조합하여 픽셀별 출력을 산출하는 프로그램입니다. [RS(래스터라이저) 단계](rasterizer-stage--rs-.md)는 기본 요소에 포함된 각 픽셀에 대해 픽셀 셰이더를 한 번 호출하지만 **NULL** 셰이더를 지정하면 셰이더 실행을 방지할 수 있습니다.

텍스처를 다중 샘플링할 때는 포함된 픽셀당 한 번 픽셀 셰이더가 호출되며, 포함된 각각의 다중 샘플에 대해 깊이/스텐실 테스트가 이루어집니다. 깊이/스텐실 테스트를 통과하는 샘플은 픽셀 셰이더 출력 색상으로 업데이트됩니다.

픽셀 셰이더 내장 함수는 화면 공간 x와 y를 기준으로 수량 파생 항목을 만들거나 사용합니다. 파생 항목의 가장 흔한 용도는 텍스처 샘플링을 위한 LOD(level-of-detail) 계산을 하고, 이방성 필터링의 경우에는 이방성 축을 따라 샘플을 선택하는 것입니다. 일반적으로 하드웨어 구현은 여러 픽셀(예: 2x2 그리드)에서 동시에 하나의 픽셀 셰이더를 실행하므로 픽셀 셰이더에서 계산되는 수량 파생 항목은 인접 픽셀의 동일한 실행 지점에서 값의 델타로 합리적으로 어림잡을 수 있습니다.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>입력


파이프라인이 기하 도형 셰이더 없이 구성된 경우, 픽셀 셰이더는 16, 32비트, 4-구성 요소 입력으로 제한됩니다. 그렇지 않은 경우, 최대 32, 32비트, 4-구성 요소 입력을 취할 수 있습니다.

픽셀 셰이더 입력 데이터는 꼭짓점 특성(원근 수정을 포함하거나 원근 수정 없이 보간 가능)을 포함하거나 기본 요소별 상수로 취급할 수 있습니다. 픽셀 셰이더 입력은 선언된 보간 모드에 따라 래스터화되는 기본 요소의 꼭짓점 특성으로부터 보간됩니다. 래스터화 전에 기본 요소가 클리핑되는 경우, 클리핑 프로세스 도중에도 보간 모드가 적용됩니다.

꼭짓점 특성은 픽셀 셰이더 중앙 위치에서 보간(또는 평가)됩니다. 픽셀 셰이더 속성 보간 모드는 [인수](https://msdn.microsoft.com/library/windows/desktop/bb509606) 또는 [입력 구조](https://msdn.microsoft.com/library/windows/desktop/bb509668)로 입력 등록 선언에서 요소별로 선언됩니다. 속성은 선형적으로 또는 중심 샘플링을 사용하여 보간할 수 있습니다. [래스터화 규칙](rasterization-rules.md)의 "다중 샘플 앤티앨리어싱 시 속성의 중심 샘플링" 섹션을 참조하세요. 중심 평가는 기본 요소에 픽셀이 포함되지만 픽셀 중심은 포함되지 않을 수 있는 경우를 포함하기 위해 다중 샘플링 도중에만 관련됩니다. 중심 평가는 (비포함) 픽셀 중심에 최대한 가까이서 이루어집니다.

입력은 다른 파이프라인 단계에서 사용되는 매개 변수를 표시하는 [시스템 값 시맨틱](https://msdn.microsoft.com/library/windows/desktop/bb509647)을 사용하여 선언될 수도 있습니다. 예를 들어 픽셀 위치는 SV\_Position 시맨틱으로 표시되어야 합니다. [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)는 픽셀 셰이더에 하나의 스칼라를 산출할 수 있고(SV\_PrimitiveID 사용), [RS(래스터라이저) 단계](rasterizer-stage--rs-.md)도 픽셀 셰이더에 하나의 스칼라를 산출할 수 있습니다(SV\_IsFrontFace 사용).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>출력


픽셀 셰이더는 최대 8, 32비트, 4-구성 요소 색을 출력할 수 있습니다. 픽셀이 삭제된 경우에는 아무 색도 출력하지 않습니다. 픽셀 셰이더 출력 등록 구성 요소는 사용하기 전에 선언되어야 합니다. 각 등록에는 고유의 출력 쓰기 마스크가 허용됩니다.

깊이-쓰기-허용 상태를 ([OM(출력 병합기) 단계](output-merger-stage--om-.md)에서) 사용하여 깊이 버퍼에 깊이 데이터가 쓰여질지 여부를 제어합니다(또는 삭제 지시를 사용하여 해당 픽셀의 데이터를 삭제). 픽셀 셰이더는 깊이 테스트를 위해 옵션인 32비트, 1-구성 요소, 부동 소수점 깊이 값도 출력할 수 있습니다(SV\_Depth 시맨틱 사용). 깊이 값은 oDepth 등록에서 출력되며, 깊이 테스트의 보간된 깊이 값을 대체합니다(깊이 테스트가 활성화 되어 있다고 가정). 고정 함수 깊이 사용 또는 oDepth 사용 사이에서 동적으로 전환하는 방법은 없습니다.

픽셀 셰이더는 스텐실 값을 출력할 수 없습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




