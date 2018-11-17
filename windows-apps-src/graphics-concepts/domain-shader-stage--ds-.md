---
title: DS(도메인 셰이더) 단계
description: DS(도메인 셰이더) 단계는 출력 패치에서 세분화된 지점의 꼭짓점 위치를 계산합니다. 이 단계에서 각 도메인 샘플에 해당하는 꼭짓점 위치도 계산합니다.
ms.assetid: 673CC04A-A74F-495F-AFB7-49157538749C
keywords:
- DS(도메인 셰이더) 단계
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3bcad4a5e22249d4d7faed08fe9cc9af4c3fb338
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7150587"
---
# <a name="domain-shader-ds-stage"></a>DS(도메인 셰이더) 단계


DS(도메인 셰이더) 단계는 출력 패치에서 세분화된 지점의 꼭짓점 위치를 계산합니다. 이 단계에서 각 도메인 샘플에 해당하는 꼭짓점 위치도 계산합니다. 도메인 셰이더는 분할기 단계 출력 지점당 한 번씩 실행되며, 헐 셰이더 출력 패치 및 출력 패치 상수, 분할기 단계 출력 UV 좌표에 대한 읽기 전용 액세스 권한을 가집니다.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


DS(도메인 셰이더) 단계 [HS(헐 셰이더) 단계](hull-shader-stage--hs-.md) 및 [TS(분할기) 단계](tessellator-stage--ts-.md)의 입력을 기반으로 출력 패치의 세분화된 지점의 꼭짓점 위치를 출력합니다.

![도메인 셰이더 단계의 다이어그램](images/d3d11-domain-shader.png)

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


-   도메인 셰이더는 [HS(헐 셰이더) 단계](hull-shader-stage--hs-.md)의 출력 제어점을 사용합니다. 헐 셰이더 출력에는 다음이 포함됩니다.
    -   제어점.
    -   패치 상수 데이터.
    -   공간 분할 요소. 공간 분할 요소는 고정 함수 분할기에서 사용하는 값뿐 아니라 기하 모핑 등에서 사용되는 원시 값(예를 들어 정수 공간 분할로 반올림하기 전)도 포함됩니다.
-   도메인 셰이더는 [TS(분할기) 단계](tessellator-stage--ts-.md)의 출력 좌표당 한 번씩 호출됩니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


-   DS(도메인 셰이더) 단계는 출력 패치의 세분화된 지점의 꼭짓점 위치를 출력합니다.

도메인 셰이더가 완료되면, 공간 분할이 끝나고 파이프라인 데이터는 [GS(기하 도형 셰이더) 단계](geometry-shader-stage--gs-.md) 및 [PS(픽셀 셰이더) 단계](pixel-shader-stage--ps-.md)와 같은 다음 파이프라인 단계로 계속됩니다. 인접한 기본 요소(예를 들어, 삼각형당 6개 꼭짓점)을 기대하는 기하 도형 셰이더는 공간 분할이 활성화된 경우 유효하지 않습니다. 이로 인해 디버그 계층에서 문제를 삼을 정의되지 않은 동작이 발생합니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


```
void main( out    MyDSOutput result, 
           float2 myInputUV : SV_DomainPoint, 
           MyDSInput DSInputs,
           OutputPatch<MyOutPoint, 12> ControlPts, 
           MyTessFactors tessFactors)
{
     ...

     result.Position = EvaluateSurfaceUV(ControlPoints, myInputUV);
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




