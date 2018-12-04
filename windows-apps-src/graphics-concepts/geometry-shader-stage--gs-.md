---
title: GS(기하 도형 셰이더) 단계
description: GS(기하 도형 셰이더) 단계는 기본 요소인 삼각형, 선, 점 전체를 인접한 꼭짓점과 함께 처리합니다.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- GS(기하 도형 셰이더) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63c678f4b2dde1a5e35c0131b5154493c9703951
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8483123"
---
# <a name="geometry-shader-gs-stage"></a>GS(기하 도형 셰이더) 단계


GS(기하 도형 셰이더) 단계는 기본 요소인 삼각형, 선, 점 전체를 인접한 꼭짓점과 함께 처리합니다. 이는 점 스프라이트 확장, 동적 입자 시스템, 섀도 볼륨 생성을 포함한 알고리즘에 유용합니다. 이 단계는 기하 도형 확장 및 축소를 지원합니다.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


기하 도형 셰이더 단계는 전체 기본 요소인 삼각형(최대 3개의 인접 꼭짓점을 가진 3개의 꼭짓점), 선(최대 2개의 인접 꼭짓점을 가진 2개의 꼭짓점), 점(1개 꼭짓점)을 처리합니다.

![인접한 꼭짓점과 삼각형 및 선의 그림](images/d3d10-gs.png)

기하 도형 셰이더는 제한된 기하 도형 확장 및 축소도 지원합니다. 입력 기본 요소가 지정되면, 기하 도형 셰이더는 기본 요소를 삭제하거나 하나 이상의 새 기본 요소를 추가할 수 있습니다.

GS(기하 도형 셰이더) 단계는 프로그래밍 가능한 셰이더 단계로, [그래픽 파이프라인](graphics-pipeline.md) 다이어그램에서 모서리가 둥근 블록으로 표시되어 있습니다. 이 셰이더 단계는 셰이더 모델([공통 셰이더 코어](https://msdn.microsoft.com/library/windows/desktop/bb509580) 참조)을 기반으로 구축된 고유의 자체 기능을 제공합니다.

기하 도형 셰이더 단계는 다음을 포함한 알고리즘에 적합합니다.

-   점 스프라이트 확장
-   동적 입자 시스템
-   털/핀 생성
-   섀도 볼륨 생성
-   단일 패스 큐브맵으로 렌더링
-   기본 요소별 재질 바꾸기
-   기본 형식별 재질 설정 - 이 기능에는 무게 중심 좌표를 기본 요소 데이터로 생성하는 기능이 포함되어 있어 픽셀 셰이더가 사용자 지정 특성 보간을 수행할 수 있습니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


기하 도형 셰이더 단계는 전체 기본 요소를 입력으로 받는 응용 프로그램 지정 셰이더 코드를 실행하며 출력 꼭짓점을 생성할 수 있습니다. 한 꼭짓점에서만 작동하는 꼭짓점 셰이더와 달리, 기하 도형 셰이더의 입력은 전체 기본 요소에 대한 꼭짓점(삼각형의 세 꼭짓점, 선의 두 꼭짓점, 점의 한 꼭짓점)입니다. 기하 도형 셰이더는 가장자리가 인접한 기본 요소의 꼭짓점 데이터(삼각형의 경우 추가 3개, 선의 경우 추가 2개 꼭짓점)도 입력으로 가져올 수 있습니다.

기하 도형 셰이더 단계는 [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)에서 자동 생성하는 **SV\_PrimitiveID** 시스템 생성 값을 사용할 수 있습니다. 이를 통해 원할 경우 기본 요소별 데이터를 가져오거나 계산할 수 있습니다.

기하 도형 셰이더가 활성화되면 파이프라인에 전달되었거나 이전에 생성된 모든 기본 요소에 대해 호출됩니다. 기하 도형 셰이더는 호출될 때마다 호출 기본 요소가 한 점, 한 선, 한 삼각형이든 상관없이 이에 대한 데이터를 입력으로 봅니다. 파이프라인에 전에 삼각형 스트립이 있으면 스트립의 각 개별 삼각형에 대해 기하 도형 셰이더가 호출됩니다. 스트립이 삼각형 목록으로 확장된 것과 같습니다. 개별 기본 요소의 각 꼭짓점에 대한 모든 입력 데이터(삼각형의 경우 3개 꼭짓점)를 사용할 수 있으며, 해당하는 경우 인접 꼭짓점 데이터도 사용할 수 있습니다.

일반적인 꼭짓점 약어:

|     |                 |
|-----|-----------------|
| TV  | 삼각형 꼭짓점 |
| LV  | 선 꼭짓점     |
| AV  | 인접 꼭짓점 |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


GS(기하 도형 셰이더) 단계는 선택한 단일 토폴로지를 형성하는 여러 꼭짓점을 출력할 수 있습니다. 사용 가능한 기하 도형 셰이더 출력 토폴로지는 **tristrip**, **linestrip** 및 **pointlist**입니다. 내보내는 기본 요소의 숫자는 기하 도형 셰이더 호출마다 자유롭게 변할 수 있으나, 내보낼 수 있는 최대 꼭짓점의 수는 고정적으로 선언될 수 있습니다. 기하 도형 셰이더 호출에서 내보내는 스트립 길이는 임의의 길이가 될 수 있으며, 새 스트립은 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 함수를 통해 만들 수 있습니다.

기하 도형 셰이더 인스턴스의 실행은 스트림에 추가된 데이터가 직렬인 경우를 제외하고 다른 호출과 별개입니다. 기하 도형 셰이더의 지정된 출력은 다른 호출에 독립적(순서는 존중됨)입니다. 삼각형 스트립을 생성하는 기하 도형 셰이더는 모든 호출 시 새 스트립을 시작합니다.

기하 도형 셰이더 출력은 스트림 출력 단계를 통해 래스터라이저 단계 및/또는 메모리의 꼭짓점 버퍼로 입력될 수 있습니다. 메모리에 공급되는 출력은 개별 점/선/삼각형 목록(래스터라이저에 전달되는 그대로)으로 확장됩니다.

기하 도형 셰이더는 꼭짓점을 출력 스트림 개체에 추가하는 방식으로 한 번에 하나의 꼭짓점을 출력합니다. 스트림의 토폴로지는 고정 선언에 의해 결정됩니다. GS 단계의 출력으로 **TriangleStream**, **LineStream** 및 **PointStream**을 선택하십시오.

사용할 수 있는 스트림 개체의 유형은 **TriangleStream**, **LineStream**, **PointStream**의 세 가지로, 모두 템플릿 기반 개체입니다. 출력의 토폴로지는 해당하는 개체 유형에 따라 결정되며, 스트림에 추가되는 꼭짓점의 형식은 템플릿 유형에 따라 결정됩니다.

기하 도형 셰이더 출력이 시스템 해석 값(예를 들어 **SV\_RenderTargetArrayIndex** 또는 **SV\_Position**)으로 해석되면, 하드웨어는 데이터 자체를 다음 셰이더 단계에 입력으로 전달할 뿐만 아니라 이 데이터를 보고 값에 따라 일부 동작을 수행할 수 있습니다. 기하 도형 셰이더의 특정 데이터 출력이 하드웨어에 꼭짓점별 기반(**SV\_ClipDistance\[n\]** 또는 **SV\_Position** 등)이 아닌 기본 요소별 기반(**SV\_RenderTargetArrayIndex** 또는 **SV\_ViewportArrayIndex**)의 의미를 가지는 경우, 기본 요소별 데이터는 기본 요소에서 내보내는 선행 꼭짓점에서 가져옵니다.

기본 요소 셰이더가 종료되고 기본 요소가 완성되지 않은 경우, 기본 요소 셰이더에서 부분적으로 완료된 기본 요소를 생성할 수 있습니다. 완전하지 않은 기본 요소는 자동으로 삭제됩니다. 이는 IA가 부분적으로 완료된 기본 요소를 처리하는 방법과 비슷합니다.

기하 도형 셰이더는 화면 공간 파생 항목이 필요하지 않은 경우 로드 및 텍스처 샘플링 작업(**samplelevel**, **samplecmplevelzero**, **samplegrad**)을 수행할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




