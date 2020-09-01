---
title: HS(헐 셰이더) 단계
description: 선체 셰이더 (HS) 단계는 공간 분할 (tessellation) 단계 중 하나로, 모델의 단일 표면을 여러 삼각형으로 효율적으로 분할 합니다.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- HS(헐 셰이더) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 11c9f0a52c4a320306cf5dfd5ce90fd5bbe2c407
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165017"
---
# <a name="hull-shader-hs-stage"></a>HS(헐 셰이더) 단계

선체 셰이더 (HS) 단계는 공간 분할 (tessellation) 단계 중 하나로, 모델의 단일 표면을 여러 삼각형으로 효율적으로 분할 합니다. 선체 셰이더 (HS) 단계에서는 각 입력 패치 (쿼드, 삼각형 또는 선)에 해당 하는 기 하 도형 패치 (및 패치 상수)를 생성 합니다. 선체 셰이더는 patch 당 한 번 호출 되며, 낮은 순서 화면을 정의 하는 입력 제어 점을 패치를 구성 하는 제어 점으로 변환 합니다. 또한 [Tessellator (TS) 단계](tessellator-stage--ts-.md) 및 [도메인 셰이더 (DS) 단계](domain-shader-stage--ds-.md)에 대 한 데이터를 제공 하기 위해 몇 가지 패치를 계산 합니다.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


![선체-셰이더 단계의 다이어그램](images/d3d11-hull-shader.png)

세 개의 공간 분할 단계는 함께 작동 하 여 그래픽 파이프라인 내에서 자세한 렌더링을 위해 모델을 간단 하 고 효율적으로 유지 하는 높은 순서 서피스를 많은 삼각형으로 변환 합니다. 공간 분할 단계에는 선체 셰이더 (HS) 단계, [Tessellator (TS)](tessellator-stage--ts-.md)단계 및 [도메인 셰이더 (DS) 단계가](domain-shader-stage--ds-.md)포함 됩니다.

선체 셰이더 (HS) 단계는 프로그래밍 가능한 셰이더 단계입니다. HLSL 함수를 사용 하 여 선체 셰이더를 구현 합니다.

선체 셰이더는 하드웨어에서 병렬로 실행 되는 제어 지점 단계와 패치 상수 단계의 두 단계로 작동 합니다. HLSL 컴파일러는 선체 셰이더에 병렬 처리를 추출 하 고 하드웨어를 구동 하는 바이트 코드를 인코딩합니다.

-   제어 지점 단계는 각 제어 지점에 대해 한 번씩 작동 하 고, 패치의 제어 지점을 읽고, 하나의 출력 제어 지점 ( **ControlPointID**으로 식별 됨)을 생성 합니다.
-   패치 상수 단계는 패치 마다 한 번씩 작동 하 여 가장자리 공간 분할 요소 및 기타 패치 별 상수를 생성 합니다. 내부적으로 여러 패치 상수 단계가 동시에 실행 될 수 있습니다. 패치 상수 단계에는 모든 입력 및 출력 제어 점에 대 한 읽기 전용 액세스 권한이 있습니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


1에서 32 사이의 입력 제어 지점과 함께 낮은 순서 표면을 정의 합니다.

-   선체 셰이더는 [TS (Tessellator) 단계](tessellator-stage--ts-.md)에 필요한 상태를 선언 합니다. 여기에는 제어 지점의 수, 패치 표면의 유형 및 개체당 때 사용할 분할 유형과 같은 정보가 포함 됩니다. 이 정보는 일반적으로 셰이더 코드의 앞에 선언으로 표시 됩니다.
-   공간 분할 계수는 각 패치의 세분화 수준을 결정 합니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


1-32 출력 제어 지점과 함께 패치를 구성 합니다.

-   셰이더 출력은 공간 분할 (tessellation) 요소 수에 관계 없이 1에서 32 사이의 제어 요소 사이에 있습니다. 선체 셰이더의 컨트롤 요소 출력은 도메인 셰이더 단계에서 사용 될 수 있습니다. 패치 상수 데이터는 도메인 셰이더에 사용 될 수 있습니다. 공간 분할 계수는 [TS (Tessellator) 단계](tessellator-stage--ts-.md) 와 [DS (도메인 셰이더) 단계](domain-shader-stage--ds-.md)에서 사용 될 수 있습니다.
-   선체 셰이더가 edge 공간 분할 계수를 0 또는 NaN으로 설정 하면 패치가 추출 (생략) 됩니다. 결과적으로 tessellator 단계는 실행 되지 않을 수도 있고, 도메인 셰이더가 실행 되지 않으며, 해당 패치에 대해 표시 되는 출력이 생성 되지 않을 수도 있습니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예 들어


```hlsl
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

[방법: 선체 셰이더 만들기](/windows/desktop/direct3d11/direct3d-11-advanced-stages-hull-shader-create)를 참조 하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 