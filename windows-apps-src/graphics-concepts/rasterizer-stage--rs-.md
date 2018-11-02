---
title: RS(래스터라이저) 단계
description: 래스터라이저는 뷰에 없는 기본 요소를 잘라내고, PS(픽셀 셰이더) 단계에 대해 기본 요소를 준비하고, 픽셀 셰이더를 호출하는 방법을 결정합니다.
ms.assetid: 7E80724B-5696-4A99-91AF-49744B5CD3A9
keywords:
- RS(래스터라이저) 단계
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 17d58a05a57fa833003b7c229db91473f6cde3d4
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5972109"
---
# <a name="rasterizer-rs-stage"></a>RS(래스터라이저) 단계


래스터라이저는 뷰에 없는 기본 요소를 잘라내고, [PS(픽셀 셰이더) 단계](pixel-shader-stage--ps-.md)에 대해 기본 요소를 준비하고, 픽셀 셰이더를 호출하는 방법을 결정합니다. 래스터화 단계는 실시간 3D 그래픽을 표시하기 위해 벡터 정보(형태 또는 기본 요소로 구성)를 래스터 이미지(픽셀로 구성)로 변환합니다.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


래스터화 동안 각 기본 요소는 픽셀로 변환되며 각 기본 요소에 걸쳐 꼭짓점별 값을 보간합니다. 래스터화에는 시야 절두체로 꼭짓점 클리핑, z로 나누기 수행을 통한 원근 제공, 기본 요소를 2D 뷰포트로 매핑, 픽셀 셰이더 호출 방법 결정이 포함됩니다. 픽셀 셰이더 사용은 선택적이지만 래스터라이저 단계는 항상 클리핑, 원근 분할을 수행하여 점을 같은 공간으로 변환하고, 꼭짓점을 뷰포트에 매핑합니다.

파이프라인에게 픽셀 셰이더가 없음을 알려 래스터화를 사용하지 않도록 설정할 수 있습니다([PS(픽셀 셰이더) 단계](pixel-shader-stage--ps-.md)를 **NULL**로 설정하고 깊이 및 스텐실 테스트를 사용하지 않도록 설정합니다). 사용하지 않도록 설정되어 있는 동안 래스터화 관련 파이프라인 카운터는 업데이트되지 않습니다.

계층적 Z 버퍼 최적화를 구현하는 하드웨어에서는 PS(픽셀 셰이더) 단계를 **NULL**로 설정하고 깊이 및 스텐실 테스트를 사용하도록 설정하여 z 버퍼 미리 로드를 사용할 수 있습니다.

[래스터화 규칙](rasterization-rules.md)을 참조하세요.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


래스터화 단계에 들어오는 꼭짓점 (x,y,z,w)는 같은 클립 공간에 있는 것으로 간주됩니다. 이 좌표 공간에서 X축은 오른쪽, Y축은 위쪽, Z축은 카메라에서 먼 쪽을 가리킵니다.

고정 함수 RS(래스터라이저) 단계에는 SO(스트림 출력) 단계 및/또는 [기하 도형 셰이더(GS) 단계](geometry-shader-stage--gs-.md) 같은 이전 파이프라인 단계가 제공됩니다. GS가 사용되지 않으면 RS에 [DS(도메인 셰이더) 단계](domain-shader-stage--ds-.md)가 제공됩니다. DS도 사용되지 않으면 RS에 [VS(꼭짓점 셰이더) 단계](vertex-shader-stage--vs-.md)가 제공됩니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


PS(픽셀 셰이더) 단계 사용은 선택적입니다. 래스터라이저 단계는 대신 [OS(출력 병합기) 단계](output-merger-stage--om-.md)로 직접 출력할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[래스터화 규칙](rasterization-rules.md)

[그래픽 파이프라인](graphics-pipeline.md)

 

 




