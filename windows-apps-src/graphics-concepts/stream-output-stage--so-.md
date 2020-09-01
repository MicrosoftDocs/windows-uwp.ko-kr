---
title: SO(스트림 출력) 단계
description: 스트림 출력 ()은 이전 활성 단계에서 메모리의 하나 이상의 버퍼로 버텍스 데이터를 지속적으로 출력 (또는 스트림) 합니다. 메모리로 스트리밍되는 데이터는 입력 데이터로 다시 파이프라인으로 다시 또는 CPU에서 다시 읽을 수 있습니다.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- SO(스트림 출력) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f56036ecc083d72f552954860d04750c1c83b8b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156207"
---
# <a name="stream-output-so-stage"></a>SO(스트림 출력) 단계


스트림 출력 ()은 이전 활성 단계에서 메모리의 하나 이상의 버퍼로 버텍스 데이터를 지속적으로 출력 (또는 스트림) 합니다. 메모리로 스트리밍되는 데이터는 입력 데이터로 다시 파이프라인으로 다시 또는 CPU에서 다시 읽을 수 있습니다.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


![파이프라인의 스트림 출력 단계 위치 다이어그램](images/d3d10-pipeline-stages-so.png)

스트림 출력 단계는 파이프라인의 기본 데이터를 래스터 라이저에 대해 메모리에 스트리밍합니다. 이전 단계에서 데이터를 메모리에 스트리밍 하거나 래스터 라이저에 전달할 수 있습니다. 메모리로 스트리밍되는 데이터는 입력 데이터로 다시 파이프라인으로 다시 또는 CPU에서 다시 읽을 수 있습니다.

메모리에 스트리밍된 데이터는 후속 렌더링 패스에서 파이프라인으로 다시 읽거나, 준비 리소스로 복사 하 여 CPU에서 읽을 수 있습니다. 스트리밍된 데이터의 양은 다를 수 있습니다. Direct3D는 작성 된 데이터 양에 대 한 쿼리 (GPU) 없이 데이터를 처리 하도록 설계 되었습니다.--&gt;

파이프라인에 스트림 출력 데이터를 제공 하는 방법에는 두 가지가 있습니다.

-   스트림 출력 데이터를 IA (입력 어셈블러) 단계로 다시 공급할 수 있습니다.
-   스트림 출력 데이터는 [로드](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load) 함수를 사용 하 여 프로그래밍 가능한 셰이더에서 읽을 수 있습니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


이전 셰이더 단계의 꼭 짓 점 데이터입니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


스트림 출력 ()은 이전 활성 단계 (예: 기 하 도형 셰이더 (GS) 단계)에서 메모리의 하나 이상의 버퍼로 연속 된 데이터 (또는 스트림)를 출력 합니다. 기 하 도형 셰이더 (GS) 단계가 비활성 상태 이면 스트림 출력 () 단계에서 계속 해 서 도메인 셰이더 (DS) 단계의 버텍스 데이터를 메모리에 버퍼링 하거나 (또한, DS가 비활성 상태인 경우에는 Vertex 셰이더 (VS) 단계에서).

입력 어셈블러 (IA) 단계에 삼각형 또는 줄 스트립이 바인딩되면 각 스트립은 스트리밍 되기 전에 목록으로 변환 됩니다. 꼭 짓 점은 항상 전체 기본 형식 (예: 삼각형의 경우 3 개의 꼭 짓 점)으로 작성 됩니다. 불완전 한 기본 형식은 스트리밍할 수 없습니다. 인접 하는 기본 형식은 데이터를 스트리밍하기 전에 인접 한 데이터를 삭제 합니다.

스트림 출력 단계는 동시에 최대 4 개의 버퍼를 지원 합니다.

-   데이터를 여러 버퍼로 스트리밍하는 경우 각 버퍼는 꼭 짓 점 데이터의 단일 요소 (최대 4 개의 구성 요소)만 캡처할 수 있습니다. 암시적 데이터는 각 버퍼의 요소 너비와 동일 합니다 (단일 요소 버퍼를 셰이더 단계로 입력 하기 위해 바인딩할 수 있는 방법과 호환 됨). 또한 버퍼의 크기가 다른 경우 버퍼 중 하나가 가득 차면 쓰기가 중지 됩니다.
-   단일 버퍼에 데이터를 스트리밍하는 경우 버퍼는 꼭 짓 점 당 데이터의 최대 64 스칼라 구성 요소 (256 바이트이 하)를 캡처할 수 있습니다. 또는 vertex stride는 최대 2048 바이트입니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 