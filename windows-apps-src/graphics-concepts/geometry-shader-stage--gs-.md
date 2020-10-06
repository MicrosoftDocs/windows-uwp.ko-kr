---
title: GS(기하 도형 셰이더) 단계
description: 기 하 도형 셰이더 (GS) 스테이지는 인접 한 꼭지점과 함께 전체 기본 삼각형, 선 및 점을 처리 합니다.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- GS(기하 도형 셰이더) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e4573547ebffa0a58b4e2347162799035d8f898
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750009"
---
# <a name="geometry-shader-gs-stage"></a>GS(기하 도형 셰이더) 단계


기 하 도형 셰이더 (GS) 스테이지는 인접 한 꼭지점과 함께 삼각형, 선 및 점의 전체 기본 형식을 처리 합니다. 점 스프라이트 확장, 동적 파티클 시스템 및 섀도 볼륨 생성을 포함 하는 알고리즘에 유용 합니다. 기 하 도형 증폭 및 비 증폭을 지원 합니다.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


기 하 도형 셰이더 단계는 전체 기본 형식, 즉 삼각형 (인접 한 꼭 짓 점이 3 개까지 꼭 짓 점), 선 (최대 2 개의 인접 한 꼭 짓 점을 포함 하는 두 개의 꼭 짓 점) 및 점 (1 꼭지점)

![삼각형 및 인접 한 꼭 짓 점을 포함 하는 선 그림](images/d3d10-gs.png)

또한 기 하 도형 셰이더는 제한 된 기 하 도형 증폭 및 비 증폭을 지원 합니다. 입력 기본 형식이 지정 된 경우 기 하 도형 셰이더는 기본 형식을 삭제 하거나 하나 이상의 새 기본 형식을 내보낼 수 있습니다.

기호 셰이더 (GS) 단계는 프로그래밍 가능한 셰이더 단계입니다. [그래픽 파이프라인](graphics-pipeline.md) 다이어그램에서 모퉁이가 둥근 블록으로 표시 됩니다. 이 셰이더 단계는 셰이더 모델을 기반으로 하는 고유한 기능을 제공 합니다 ( [공통 셰이더 코어](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)참조).

Geometry 셰이더 단계는 다음을 비롯 한 알고리즘에 적합 합니다.

-   Point 스프라이트 확장
-   동적 파티클 시스템
-   이유/Fin 생성
-   섀도 볼륨 생성
-   단일 패스 렌더링-큐브 맵
-   프리미티브 재질 스와핑
-   프리미티브 재질 설정-이 기능은 픽셀 셰이더가 사용자 지정 특성 보간을 수행할 수 있도록 기본 데이터로 barycentric 좌표를 생성 하는 것을 포함 합니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


기 하 도형 셰이더 단계는 전체 기본 형식을 입력으로 사용 하 여 응용 프로그램 지정 셰이더 코드를 실행 하 고 출력에서 꼭 짓 점을 생성 합니다. 단일 꼭 짓 점에 대해 작동 하는 꼭 짓 점 셰이더와 달리 기 하 도형 셰이더의 입력은 전체 기본 형식 (삼각형의 경우 3 개의 꼭 짓 점, 선에는 두 개의 꼭 짓 점)에 대 한 꼭 짓 점입니다. 기 하 도형 셰이더는 가장자리 인접 기본 형식에 대 한 꼭 짓 점 데이터를 입력으로 가져올 수도 있습니다 (삼각형의 경우 3 개, 한 줄에 대 한 추가 두 꼭 짓 점).

기 하 도형 셰이더 단계는 [IA (입력 어셈블러) 단계](input-assembler-stage--ia-.md)에서 자동 생성 되는 **SV \_ PrimitiveID** 시스템 생성 값을 사용할 수 있습니다. 이렇게 하면 기본 데이터 별 데이터를 인출 하거나 원하는 경우 계산할 수 있습니다.

Geometry 셰이더가 활성 상태 이면 파이프라인에서 이전에 전달 되거나 생성 된 모든 기본 형식에 대해 한 번 호출 됩니다. 기 하 도형 셰이더를 호출할 때마다 호출 하는 기본 형식에 대 한 데이터 (단일 점, 단일 선 또는 단일 삼각형)를 입력으로 볼 수 있습니다. 파이프라인의 앞부분에 있는 삼각형 스트립은 스트립이 삼각형 목록으로 확장 된 경우 처럼 스트립의 각 개별 삼각형에 대해 geometry 셰이더를 호출 합니다. 개별 기본 형식의 각 꼭 짓 점에 대 한 모든 입력 데이터를 사용할 수 있습니다 (즉, 삼각형의 경우 3 개의 꼭 짓 점), 인접 한 꼭 짓 점 데이터 (해당 하는 경우)를 사용할 수 있습니다.

일반적인 꼭 짓 점 약어:

| 약어 | 용어 |
| ------------ | ---- |
| TV  | 삼각형 꼭 짓 점 |
| LV  | 선 꼭 짓 점     |
| AV  | 인접 한 꼭 짓 점 |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


기 하 도형 셰이더 (GS) 단계는 단일 선택 토폴로지를 형성 하는 여러 꼭 짓 점을 출력할 수 있습니다. 사용 가능한 기 하 도형 셰이더 출력 토폴로지는 **tristrip**, **linestrip**및 **pointlist**입니다. 내보낼 수 있는 최대 꼭 짓 점 수는 정적으로 선언 되어야 하지만 내보내는 기본 형식의 수는 기 하 도형 셰이더 호출 내에서 자유롭게 달라질 수 있습니다. 기 하 도형 셰이더 호출에서 내보낸 스트립 길이는 임의로 지정할 수 있으며 [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL 함수를 통해 새 스트립을 만들 수 있습니다.

Geometry 셰이더 인스턴스의 실행은 스트림에 추가 된 데이터가 직렬 임을 제외 하 고 다른 호출에서 원자 단위입니다. Geometry 셰이더에 대 한 지정 된 호출의 출력은 순서가 적용 되는 다른 호출과는 독립적입니다. 기 하 도형 셰이더 생성 삼각형 스트립은 모든 호출에서 새 스트립을 시작 합니다.

기 하 도형 셰이더 출력은 스트림 출력 단계를 통해 메모리에서 래스터 라이저 및/또는 버텍스 버퍼로 공급할 수 있습니다. 메모리에 제공 된 출력은 개별 포인트/선/삼각형 목록 (래스터 라이저에 전달 되는 것과 정확히 동일 함)으로 확장 됩니다.

기 하 도형 셰이더는 출력 스트림 개체에 꼭 짓 점을 추가 하 여 한 번에 하나씩 데이터를 출력 합니다. 스트림의 토폴로지는 고정 선언에 의해 결정 되며, GS 단계에 대 한 출력으로 **TriangleStream**, **LineStream** 및 **pointstream** 을 선택 합니다.

사용할 수 있는 스트림 개체에는 **TriangleStream**, **LineStream** 및 **pointstream**의 세 가지 유형이 있습니다 .이는 모든 템플릿 개체입니다. 출력의 토폴로지는 해당 개체 형식에 따라 결정 되며, 스트림에 추가 된 꼭 짓 점 형식은 템플릿 형식에 의해 결정 됩니다.

기 하 도형 셰이더 출력이 시스템 해석 된 값 (예: **sv \_ RenderTargetArrayIndex** 또는 **sv \_ Position**)으로 식별 되는 경우 하드웨어는이 데이터를 확인 하 고 해당 값에 따라 다른 동작을 수행 하 여 입력을 위한 다음 셰이더 단계로 데이터를 전달할 수 있습니다. 기 하 도형 셰이더의 이러한 데이터 출력에 대 한 하드웨어 (예: sv ** \_ RenderTargetArrayIndex** 또는 **sv \_ ViewportArrayIndex**)에 대 한 의미가 있는 경우 (예: sv ** \_ ClipDistance \[ n \] ** 또는 **sv \_ 위치**) 기본이 아닌 데이터는 기본 형식에 대해 내보낸 선행 꼭 짓 점에서 가져옵니다.

기 하 도형 셰이더가 종료 되 고 기본 형식이 완전 하지 않은 경우에는 부분적으로 완료 된 기본 형식을 기 하 도형 셰이더에 생성할 수 있습니다. 불완전 한 기본 형식은 자동으로 삭제 됩니다. 이는 IA에서 부분적으로 완료 된 기본 형식을 처리 하는 방식과 비슷합니다.

기 하 도형 셰이더는 화면 공간 파생물 (**samplelevel**, **samplecmplevelzero**, **samplegrad**)이 필요 하지 않은 로드 및 질감 샘플링 작업을 수행할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 