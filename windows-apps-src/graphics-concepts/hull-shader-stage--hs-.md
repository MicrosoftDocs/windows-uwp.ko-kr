---
title: HS(헐 셰이더) 단계
description: HS(헐 셰이더) 단계는 공간 분할 단계 중 하나로, 모델의 한 표면을 효율적으로 많은 삼각형으로 나눕니다.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- HS(헐 셰이더) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9137f7ef46da1b861976dbac680327febf315dac
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8946276"
---
# <a name="hull-shader-hs-stage"></a>HS(헐 셰이더) 단계


HS(헐 셰이더) 단계는 공간 분할 단계 중 하나로, 모델의 한 표면을 효율적으로 많은 삼각형으로 나눕니다. HS(헐 셰이더) 단계는 각 입력 패치(사각형, 삼각형 또는 선)에 해당하는 기하 도형 패치(및 패치 상수)를 생성합니다. 헐 셰이더는 패치당 한 번 호출되고, 하위 표면을 정의하는 입력 제어점을 패치를 만드는 제어점으로 변환합니다. 또한 [TS(분할기) 단계](tessellator-stage--ts-.md) 및 [DS(도메인 셰이더) 단계](domain-shader-stage--ds-.md)에 데이터를 제공하기 위한 일부 패치별 계산도 수행합니다.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


![헐 셰이더 단계의 다이어그램](images/d3d11-hull-shader.png)

세 개의 공간 분할 단계는 함께 작동하여 그래픽 파이프라인에서 세부적으로 렌더링하기 위해 상위 표면(모델을 간단하고 효율적으로 유지)을 많은 삼각형으로 변환합니다. 공간 분할 단계에는 HS(헐 셰이더) 단계, [TS(분할기) 단계](tessellator-stage--ts-.md), [DS(도메인 셰이더) 단계](domain-shader-stage--ds-.md)가 포함됩니다.

HS(헐 셰이더) 단계는 프로그래밍 가능한 셰이더 단계입니다. 헐 셰이더는 HLSL 함수로 구현됩니다.

헐 셰이더는 제어점 단계와 패치 상수 단계의 두 단계로 작동하며, 이는 하드웨어에 의해 동시에 실행됩니다. HLSL 컴파일러는 헐 셰이더에서 병렬 처리 내용을 추출하고 하드웨어를 구동하는 바이트코드로 인코딩합니다.

-   제어점 단계는 각 제어점에 대해 한 번 작동하며, 패치를 위한 제어점을 읽고, 하나의 출력 제어점(**ControlPointID**로 식별됨)을 생성합니다.
-   패치 상수 단계는 패치당 한 번씩 작동하며, 가장자리 공간 분할 요인 및 기타 패치별 상수를 생성합니다. 내부적으로 많은 패치 상수 단계가 동시에 실행될 수 있습니다. 패치 상수 단계는 모든 입출력 제어점에 대한 읽기 전용 액세스 권한을 가집니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


하위 표면을 함께 정의하는 1에서 32까지의 입력 제어점.

-   헐 셰이더는 [TS(분할기) 단계](tessellator-stage--ts-.md)에서 요구하는 상태를 선언합니다. 여기에는 제어점의 수, 패치 면의 유형, 공간 분할 시 사용하는 분할의 유형 등이 포함됩니다. 이 정보는 일반적으로 셰이더 코드 앞에 선언으로 표시됩니다.
-   공간 분할 요인은 각 패치를 얼마나 세분화할지 결정합니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


패치를 함께 구성하는 1에서 32까지의 출력 제어점.

-   셰이더 출력은 공간 분할 요인의 수에 관계없이 1에서 32까지의 제어점입니다. 헐 셰이더의 제어점 출력은 도메인 셰이더 단계에서 사용할 수 있습니다. 패치 상수 데이터는 도메인 셰이더에서 사용할 수 있습니다. 공간 분할 요인은 [TS(분할기) 단계](tessellator-stage--ts-.md) 및 [DS(도메인 셰이더) 단계](domain-shader-stage--ds-.md)에서 사용할 수 있습니다.
-   헐 셰이더가 어떤 가장자리 공간 분할 요인이라도 0이하나 NaN으로 설정하면 패치는 컬링(생략)됩니다. 그 결과로, 분할기 단계가 실행 또는 실행되지 않을 수도 있고, 도메인 셰이더가 실행되지 않을 수도 있으며, 해당 패치에 대한 출력이 표시되지 않을 수도 있습니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


```
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

[방법: 헐 셰이더 만들기](https://msdn.microsoft.com/library/windows/desktop/ff476338)를 참조하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




