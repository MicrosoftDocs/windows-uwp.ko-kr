---
title: PS(픽셀 셰이더) 단계
description: PS (픽셀 셰이더) 단계는 기본 형식에 대 한 보간된 데이터를 받고 색과 같은 픽셀 별 데이터를 생성 합니다.
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- PS(픽셀 셰이더) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d35fa06ac415b1d4197c1b1bcbf101f1d4cf9b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156287"
---
# <a name="pixel-shader-ps-stage"></a>PS(픽셀 셰이더) 단계


PS (픽셀 셰이더) 단계는 기본 형식에 대 한 보간된 데이터를 받고 색과 같은 픽셀 별 데이터를 생성 합니다.

이는 프로그래밍 가능한 셰이더 단계입니다. [그래픽 파이프라인](graphics-pipeline.md) 다이어그램에서 모퉁이가 둥근 블록으로 표시 됩니다. 이 셰이더 단계는 셰이더 모델 4.0 [공통 셰이더 core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)를 기반으로 하는 고유한 기능을 제공 합니다.

픽셀 셰이더 (PS) 단계에서는 픽셀 별 조명 및 사후 처리와 같은 풍부한 음영 기술을 사용할 수 있습니다. 픽셀 셰이더는 상수 변수, 질감 데이터, 보간된 꼭지점 별 값 및 기타 데이터를 결합 하 여 픽셀 당 출력을 생성 하는 프로그램입니다. [래스터 라이저 (RS) 단계](rasterizer-stage--rs-.md) 는 기본 형식에 포함 된 각 픽셀에 대해 픽셀 셰이더를 한 번씩 호출 하지만 셰이더 실행을 방지 하기 위해 **NULL** 셰이더를 지정할 수 있습니다.

질감을 다중 샘플링 할 때 픽셀 셰이더는 각 대상 다중 샘플에 대해 깊이/스텐실 테스트를 수행 하는 동안 해당 픽셀 당 한 번 호출 됩니다. 깊이/스텐실 테스트를 통과 하는 샘플이 픽셀 셰이더 출력 색으로 업데이트 됩니다.

픽셀 셰이더 내장 함수는 화면 공간 x 및 y에 대해 파생 된 수량을 생성 하거나 사용 합니다. 가장 일반적으로 사용 되는 항목은 질감 샘플링에 대 한 세부 수준 계산을 계산 하 고, 이방성 필터링의 경우 이방성 축을 따라 샘플을 선택 하는 것입니다. 일반적으로 하드웨어 구현은 여러 픽셀 (예: 2x2 그리드)에서 픽셀 셰이더를 동시에 실행 하므로 픽셀 셰이더에 계산 되는 수량의 파생 된 수량을 인접 픽셀의 동일한 실행 지점에 있는 값의 델타로 합리적으로 지정할 수 있습니다.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>내용


기 하 도형 셰이더 없이 파이프라인이 구성 된 경우 픽셀 셰이더는 16 개, 32 비트, 4 개의 구성 요소 입력으로 제한 됩니다. 그렇지 않으면 픽셀 셰이더는 최대 32, 32 비트, 4 개의 구성 요소 입력을 사용할 수 있습니다.

픽셀 셰이더 입력 데이터에는 꼭 짓 점 수정을 사용 하거나 사용 하지 않고 보간 될 수 있는 꼭 짓 점 특성이 포함 되거나 기본 상수로 처리 될 수 있습니다. 픽셀 셰이더 입력은 선언 된 보간 모드에 따라 래스터화된 기본 형식의 꼭 짓 점 특성에서 보간됩니다. 기본 형식이 래스터화 전에 잘린 경우에는 클리핑 프로세스 중에도 보간 모드가 적용 됩니다.

꼭 짓 점 특성은 픽셀 셰이더 센터 위치에서 보간된 (또는 계산) 됩니다. 픽셀 셰이더 특성 보간 모드는 [인수](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-function-parameters) 또는 [입력 구조](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-struct)에서 요소 별로 입력 레지스터 선언에 선언 됩니다. 특성은 선형으로 또는 중심 샘플링을 사용 하 여 보간합니다. [래스터화 규칙](rasterization-rules.md)에서 "특성의 중심 샘플링 When 다중 샘플 앤티엘리어싱" 섹션을 참조 하세요. 중심 평가는 한 픽셀이 기본 형식으로 적용 되지만 픽셀 센터는 그렇지 않을 수 있는 경우에만 다중 샘플링 중에 관련이 있습니다. 중심 평가는 (적용 되지 않은) 픽셀 중심에 최대한 가깝게 발생 합니다.

다른 파이프라인 단계에서 사용 되는 매개 변수를 표시 하는 [시스템 값 의미 체계](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)를 사용 하 여 입력을 선언할 수도 있습니다. 예를 들어, 픽셀 위치는 SV 위치 의미 체계를 사용 하 여 표시 되어야 합니다 \_ . [입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md) 에서 스칼라 (PrimitiveID)를 사용 하 여 픽셀 셰이더에 대 한 스칼라 하나를 생성할 수 있습니다 \_ . [래스터 라이저 (RS) 단계](rasterizer-stage--rs-.md) 는 픽셀 셰이더 (sv IsFrontFace 사용)에 대해 하나의 스칼라를 생성할 수도 있습니다 \_ .

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>출력


픽셀 셰이더는 픽셀을 삭제 하는 경우 최대 8, 32 비트, 4 개의 구성 요소 색 또는 색을 출력할 수 있습니다. 픽셀 셰이더 출력 레지스터 구성 요소를 사용 하려면 먼저 선언 해야 합니다. 각 레지스터에는 고유한 출력 쓰기 마스크가 허용 됩니다.

깊이 있는 쓰기 가능 상태 ( [OM (출력 병합기) 단계](output-merger-stage--om-.md))를 사용 하 여 깊이 데이터를 깊이 버퍼에 쓸지 여부를 제어 하거나, 취소 명령을 사용 하 여 해당 픽셀에 대 한 데이터를 삭제할 수 있습니다. 픽셀 셰이더는 선택적인 32 비트, 1-구성 요소, 부동 소수점, 깊이 테스트를 위한 깊이 값 (SV 깊이 의미 체계 사용)을 출력할 수도 있습니다 \_ . 깊이 값은 oDepth 레지스터에서 출력 되며 깊이 테스트를 위해 보간된 깊이 값이 대체 됩니다 (깊이 테스트를 사용 하도록 설정 했다고 가정). 고정 함수 깊이 또는 셰이더 oDepth를 사용 하는 사이를 동적으로 변경할 수 있는 방법은 없습니다.

픽셀 셰이더는 스텐실 값을 출력할 수 없습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 