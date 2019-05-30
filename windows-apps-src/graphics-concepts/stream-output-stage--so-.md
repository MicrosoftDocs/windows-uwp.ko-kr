---
title: SO(스트림 출력) 단계
description: SO(스트림 출력) 단계에서는 이전의 활성 단계에서 하나 이상의 메모리 내 버퍼로 꼭짓점 데이터를 연속적으로 출력(또는 스트리밍)합니다. 메모리로 스트리밍된 데이터는 입력 데이터 또는 CPU에서 다시 읽기로 파이프라인으로 다시 재순환될 수 있습니다.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- SO(스트림 출력) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e3614b7bde3a87c8f5fa6fdc0eada560fd7bbcdc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370960"
---
# <a name="stream-output-so-stage"></a>SO(스트림 출력) 단계


SO(스트림 출력) 단계에서는 이전의 활성 단계에서 하나 이상의 메모리 내 버퍼로 꼭짓점 데이터를 연속적으로 출력(또는 스트리밍)합니다. 메모리로 스트리밍된 데이터는 입력 데이터 또는 CPU에서 다시 읽기로 파이프라인으로 다시 재순환될 수 있습니다.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>용도 사용


![파이프라인 내 스트림 출력 단계의 위치 다이어그램](images/d3d10-pipeline-stages-so.png)

스트림 출력 단계에서는 파이프라인에서 메모리를 거쳐 래스터라이저로 원형 데이터를 스트리밍합니다. 이전 단계의 데이터는 메모리로 스트리밍되거나 래스터라이저로 전달될 수 있습니다. 메모리로 스트리밍된 데이터는 입력 데이터 또는 CPU에서 다시 읽기로 파이프라인으로 다시 재순환될 수 있습니다.

메모리로 스트리밍된 데이터는 이후의 렌더링 패스에서 파이프라인으로 다시 입력되거나 (CPU에서 읽을 수 있도록) 준비 리소스로 복사될 수 있습니다. 스트리밍 방식으로 출력 데이터의 양이 달라질 수 있습니다. Direct3D는 기록 된 데이터 양에 대 한 쿼리 (GPU) 필요 없이 데이터를 처리 하도록 설계 되었습니다.-&gt;

파이프라인에 스트림 출력 데이터를 공급하는 방법은 두 가지가 있습니다.

-   스트림 출력 데이터를 IA(입력 어셈블러) 단계로 피드백할 수 있습니다.
-   스트림 출력 데이터를 프로그래밍 가능 셰이더가 [Load](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load) 함수를 사용하여 읽을 수 있습니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


이전 셰이더 단계의 꼭짓점 데이터.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Output


SO(스트림 출력) 단계에서는 이전 활성 단계(예: GS(기하 도형 셰이더) 단계)의 꼭짓점 데이터를 하나 이상의 메모리 내 버퍼로 연속적으로 출력(또는 스트리밍)합니다. 기 하 도형 셰이더 (GS) 스테이지 활성 상태인 경우 Stream 출력 (등) 스테이지 꼭 짓 점 데이터 (DS) 도메인 셰이더 단계에서 메모리 (또는 경우 DS 활성화 되어 있지도 꼭 짓 점 셰이더 (VS) 단계에서)의 버퍼를 지속적으로 출력 합니다.

삼각형 또는 선 스트립이 IA(입력 어셈블러) 단계에 바인딩된 경우 각 스트립은 목록으로 변환된 후 스트림 출력됩니다. 꼭짓점은 항상 완전한 원형으로 쓰여지며(예: 삼각형의 경우 한 번에 세 꼭짓점) 불완전한 원형은 스트림 출력되지 않습니다. 인접성을 포함한 원형 유형은 데이터를 스트림 출력하기 전에 인접성 데이터를 삭제기합니다.

스트림 출력 단계는 최대 4개의 버퍼를 동시에 지원합니다.

-   데이터를 여러 버퍼로 스트리밍하는 경우 각 버퍼는 암시된 데이터 진행 속도가 각 버퍼 내 요소 너비와 동일한(단일 요소 버퍼가 입력에 대해 셰이더 단계로 바인딩되는 방식과 호환됨), 꼭짓점당 데이터의 단일 요소(최대 4개 구성 요소)만 캡처할 수 있습니다. 또한 버퍼 크기가 서로 다를 경우 버퍼 중 하나가 가득 차는 즉시 쓰기가 중단됩니다.
-   데이터를 단일 버퍼로 스트리밍하는 경우 버퍼는 꼭짓점당 데이터(256바이트 이하)의 스칼라 구성 요소를 최대 64개 캡처할 수 있거나 꼭짓점 진행 속도가 최대 2048바이트일 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




