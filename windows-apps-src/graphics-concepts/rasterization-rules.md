---
title: 래스터화 규칙
description: 래스터화 규칙은 벡터 데이터가 래스터 데이터에 매핑되는 방식을 정의 합니다.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- 래스터화 규칙
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7cc139fe9b3ad5324ea6e325f2806d9cf9c5ee1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168047"
---
# <a name="rasterization-rules"></a>래스터화 규칙


래스터화 규칙은 벡터 데이터가 래스터 데이터에 매핑되는 방식을 정의 합니다. 래스터 데이터는 추출 및 잘린 (최소 픽셀 수를 그리기 위해) 정수 위치로 스냅 된 다음 픽셀 단위 특성은 픽셀 셰이더에 전달 되기 전에 보간 됩니다 (꼭 짓 점 별 특성에서).

매핑되는 기본 형식에 종속 되는 여러 가지 유형의 규칙이 있으며, 데이터에서 다중 샘플링을 사용 하 여 별칭을 줄이도록 할지 여부를 결정 합니다. 다음 그림에서는 모퉁이 사례를 처리 하는 방법을 보여 줍니다.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>삼각형 래스터화 규칙 (다중 샘플링 없음)


삼각형 안에 있는 모든 픽셀 센터가 그려집니다. 픽셀은 왼쪽 위에 있는 규칙을 통과 하는 경우 내부에 있는 것으로 간주 됩니다. 왼쪽 위 규칙은 픽셀 센터가 삼각형의 위쪽 가장자리 또는 삼각형의 왼쪽 가장자리에 있는 경우 삼각형의 안쪽에 있도록 정의 된 것입니다.

여기서

-   위쪽 가장자리는 정확히 가로 이며 다른 가장자리를 넘는 가장자리입니다.
-   왼쪽 가장자리는 정확히 가로는 아니고 삼각형의 왼쪽에 있는 가장자리입니다. 삼각형에는 하나 또는 두 개의 왼쪽 가장자리가 있을 수 있습니다.

왼쪽 위 규칙을 사용 하면 인접 한 삼각형이 한 번 그려집니다.

이 그림은 삼각형 안에 있거나 왼쪽 위에 있는 규칙을 따르기 때문에 그려지는 픽셀의 예를 보여 줍니다.

![왼쪽 위 삼각형 래스터화의 예](images/d3d10-rasterrulestriangle.png)

픽셀을 포함 하는 밝은 회색 및 어두운 회색은 이러한 픽셀을 포함 하는 삼각형을 나타내는 픽셀의 그룹으로 표시 합니다.

## <a name="span-idline_1spanspan-idline_1spanspan-idline_1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>선 래스터화 규칙 (다중 샘플링 없이 별칭 지정)


선 래스터화 규칙은 다이아몬드 테스트 영역을 사용 하 여 줄이 픽셀을 포함 하는지 확인 합니다. X 주 줄 (-1 &lt; = 슬로프 = + 1 인 경우 선)의 경우 &lt; 다이아몬드 테스트 영역에는 왼쪽 아래 가장자리, 오른쪽 아래 가장자리, 아래쪽 모퉁이가 포함 되 고, 다이아몬드는 왼쪽 위 가장자리, 오른쪽 위 가장자리, 위쪽 corder, 왼쪽 모퉁이 및 오른쪽 모퉁이를 제외 합니다. Y 주 선은 x 주 선이 아닌 선입니다. 테스트 다이아몬드 영역은 오른쪽 모퉁이가 포함 된 경우를 제외 하 고는 x 주 선에 대해 설명한 것과 같습니다.

다이아몬드 영역을 지정 하는 경우 줄이 시작 부분에서 끝 방향으로 이동할 때 줄이 픽셀의 다이아몬드 테스트 영역을 벗어나면 픽셀을 포함 합니다. 줄 스트립은 선 시퀀스로 그리기 때문에 동일 하 게 동작 합니다.

다음 그림에서는 몇 가지 예를 보여 줍니다.

![앨리어싱된 선 래스터화의 예](images/d3d10-rasterrulesline.png)

## <a name="span-idline_2spanspan-idline_2spanspan-idline_2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>선 래스터화 규칙 (다중 샘플링 없이 앤티 앨리어싱)


앤티 앨리어싱 선은 사각형 (너비 = 1) 인 것 처럼 래스터화됩니다. 사각형은 픽셀 셰이더 출력 알파 구성 요소에 곱하여 픽셀 단위 검사 값을 생성 하는 렌더링 대상과 교차 합니다. 다중 샘플 렌더링 대상에서 선을 그릴 때 앤티 앨리어싱 수행 없습니다.

앤티 앨리어싱 선 렌더링을 수행 하는 단일 "최상" 방법이 없는 것으로 간주 됩니다. Direct3D는 다음 그림에 표시 된 방법에 대 한 지침으로 채택 합니다. 이 메서드는 다양 한 시각적 속성을 알려지고 실험적으로 하 여 파생 된 것입니다.

하드웨어는이 알고리즘과 정확히 일치 하지 않아도 됩니다. 이 참조에 대 한 테스트에는 아래에 나열 된 몇 가지 원칙에 따라 "적절 한" 공차가 제공 되어 다양 한 하드웨어 구현 및 필터 커널 크기를 허용 해야 합니다. 그러나 하드웨어 구현에 허용 되는 이러한 유연성은 간단 하 게 선을 그리고 모양을 관찰/측정 하는 것 외에도 Direct3D를 통해 응용 프로그램에 전달할 수 있습니다.

![앤티 앨리어싱 선 래스터화의 예](images/d3d10-rasterruleslineaa.png)

이 알고리즘은 최소 들쭉날쭉한 가장자리 또는 braiding를 사용 하 여 균일 한 강도로 균일 한 선을 생성 합니다. 닫힌 줄의 moire 패턴화는 최소화 됩니다. 끝에서 끝까지 배치 된 선 세그먼트 간의 교차점에 대 한 적절 한 검사를 제공 합니다.

필터 커널은 가장자리 흐린 크기와 감마 수정으로 인해 발생 하는 강도의 변화에 대 한 적절 한 균형을 유지 합니다. 검사 값은 [OM (출력 병합기) 단계](output-merger-stage--om-.md)에서 다음 수식에 따라 픽셀 셰이더 O0 (srcAlpha)를 곱합니다.

srcColor \* srcAlpha + destcolor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>점 래스터화 규칙 (다중 샘플링 없음)


점은 Z 패턴에서 삼각형 래스터화 규칙을 사용 하는 두 개의 삼각형으로 구성 된 것 처럼 해석 됩니다. 좌표는 한 픽셀 와이드 사각형의 중심을 식별 합니다. 점에 대 한 고르기 없습니다.

다음 그림에서는 몇 가지 예를 보여 줍니다.

![점 래스터화의 예](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>다중 샘플 앤티 앨리어싱 래스터화 규칙


MSAA (다중 샘플 앤티엘리어싱)는 여러 하위 샘플 위치에서 픽셀 범위 및 깊이 스텐실 테스트를 사용 하 여 기 하 도형 앨리어싱을 감소 시킵니다. 성능 향상을 위해 대상 하위 픽셀 간에 셰이더 출력을 공유 하 여 각 대상 픽셀에 대해 픽셀 단위 계산이 한 번씩 수행 됩니다. 다중 샘플 앤티엘리어싱은 표면 앨리어싱을 줄이지 않습니다. 샘플 위치 및 재구성 함수는 하드웨어 구현에 따라 달라 집니다.

다음 그림에서는 몇 가지 예를 보여 줍니다.

![다중 샘플 앤티앨리어싱 래스터화의 예](images/d3d10-rasterrulesmsaa.png)

샘플 위치 수는 다중 샘플 모드에 따라 달라 집니다. 꼭 짓 점 특성은 픽셀 셰이더가 호출 되는 위치 이기 때문에 픽셀 센터에서 보간됩니다 .이는 센터가 포함 되지 않은 경우에는이를 해결할 수 없습니다. 특성은 중심 샘플링 되도록 픽셀 셰이더에 플래그가 지정 될 수 있으며,이로 인해 해당 픽셀의 영역과 기본이 교차 하는 위치에 특성을 보간 하지 않습니다.

X 및 y 델타를 사용 하는 파생 계산을 지원 하기 위해 각 2x2 픽셀 영역에 대해 픽셀 셰이더를 실행 합니다. 즉, 다중 샘플링에 독립적인 최소 2x2 퀀텀을 채우기 위해 셰이더 호출이 표시 된 것 보다 더 많이 발생 합니다. 셰이더 결과는 샘플 별 깊이 스텐실 테스트를 통과 하는 각 대상 샘플에 대해 기록 됩니다.

기본 형식에 대 한 래스터화 규칙은 일반적으로 다음을 제외 하 고 다중 샘플 앤티엘리어싱에 의해 변경 되지 않습니다.

-   삼각형의 경우 각 샘플 위치 (픽셀 센터가 아님)에 대해 검사 테스트가 수행 됩니다. 둘 이상의 샘플 위치가 포함 된 경우 픽셀 셰이더는 픽셀 중심에서 보간된 특성을 사용 하 여 한 번 실행 됩니다. 결과는 깊이/스텐실 테스트를 통과 하는 픽셀의 각 대상 샘플 위치에 대해 저장 (복제) 됩니다.

    선은 줄 너비가 1.4 인 두 개의 삼각형으로 구성 된 사각형으로 처리 됩니다.

-   지점의 경우 각 샘플 위치 (픽셀 센터가 아님)에 대해 검사 테스트가 수행 됩니다.

셰이더에 의해 액세스 되는 개별 샘플에는 확인이 필요 하지 않으므로 [로드](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load)를 사용 하 여 셰이더를 다시 읽을 수 있는 렌더링 대상에서 다중 샘플링 형식을 사용할 수 있습니다. 다중 샘플 리소스에는 깊이 형식이 지원 되지 않으므로 깊이 형식은 렌더링 대상 으로만 제한 됩니다.

관대 한 형식 형식은 다중 샘플링을 지원 하므로 리소스 뷰에서 데이터를 다른 방식으로 해석할 수 있습니다. 예를 들어 R8G8B8A8를 사용 하 여 다중 샘플 리소스를 만들고 \_ R8G8B8A8 UINT 형식의 렌더링 대상 뷰 리소스를 사용 하 여 렌더링 \_ 한 다음 R8G8B8A8 \_ unorm 데이터 형식을 사용 하 여 다른 리소스에 대 한 내용을 확인할 수 있습니다.

### <a name="span-idhardware_supportspanspan-idhardware_supportspanspan-idhardware_supportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>하드웨어 지원

API는 품질 수준 수를 통해 다중 샘플링에 대 한 하드웨어 지원을 보고 합니다. 예를 들어 0 품질 수준은 하드웨어에서 다중 샘플링 (특정 형식 및 품질 수준)을 지원 하지 않음을 의미 합니다. 품질 수준 3은 하드웨어가 세 가지 샘플 레이아웃 및/또는 확인 알고리즘을 지원함을 의미 합니다. 다음을 가정할 수도 있습니다.

-   다중 샘플링을 지 원하는 모든 형식은 해당 패밀리의 모든 형식에 대해 동일한 품질 수준을 지원 합니다.
-   다중 샘플링을 지원 하 고 \_ orm, \_ SRGB, \_ snorm 또는 FLOAT 형식을 포함 하는 모든 형식 \_ 에서을 (를) 확인도 지원 합니다.

### <a name="span-idcentroid_samplingspanspan-idcentroid_samplingspanspan-idcentroid_samplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>다중 샘플 앤티엘리어싱 인 경우 특성의 중심 샘플링

기본적으로 꼭 짓 점 특성은 다중 샘플 앤티엘리어싱 중에 픽셀 중심으로 보간됩니다. 픽셀 센터가 적용 되지 않는 경우 특성은 픽셀 추정 됩니다. 중심 의미 체계가 포함 된 픽셀 셰이더 입력 (픽셀이 완전히 검사 되지 않는다고 가정)은 대상 샘플 위치 중 하나에서 해당 픽셀의 대상 영역 내에서 샘플링 됩니다. 중심 계산 전에 래스터 라이저 상태로 지정 된 샘플 마스크를 적용 합니다. 따라서 마스킹된 샘플은 중심 위치로 사용 되지 않습니다.

참조 래스터 라이저는 다음과 유사한 중심 샘플링에 대 한 샘플 위치를 선택 합니다.

-   샘플 마스크는 모든 샘플을 허용 합니다. 픽셀이 적용 되는 경우 픽셀 중심을 사용 하 고, 포함 된 샘플이 없으면 픽셀 중심을 사용 합니다. 그렇지 않으면 첫 번째 검사 된 샘플이 픽셀 중심에서 시작 하 여 바깥쪽으로 이동 하 여 선택 됩니다.
-   샘플 마스크는 모든 샘플을 해제 하나 (일반적인 시나리오)를 해제 합니다. 응용 프로그램은 단일 비트 샘플 마스크 값을 순환 하 고 중심 샘플링을 사용 하 여 각 샘플에 대 한 장면을 다시 렌더링 하 여 multipass supersampling를 구현할 수 있습니다. 이렇게 하려면 응용 프로그램이 파생물을 조정 하 여 더 높은 질감 샘플링 밀도에 대해 적절 하 게 더 자세한 질감 mips를 선택 해야 합니다.

### <a name="span-idderivative_calculationsspanspan-idderivative_calculationsspanspan-idderivative_calculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>다중 샘플링 시 파생 계산

픽셀 셰이더는 항상 최소 2x2 픽셀 영역을 사용 하 여 파생 계산을 지원 하기 위해 항상 최소 2x2 픽셀 영역을 사용 하 여 계산 됩니다 .이는 인접 픽셀의 데이터 간에 델타를 사용 하 여 계산 됩니다. 각 픽셀의 데이터는 가로 또는 세로 단위로 샘플링 된 것으로 가정 합니다. 이는 다중 샘플링의 영향을 받지 않습니다.

중심 샘플링 된 특성에서 파생물을 요청 하면 하드웨어 계산이 조정 되지 않으므로 부정확 한 파생이 발생할 수 있습니다. 셰이더는 렌더링 대상 공간에서 단위 벡터를 필요로 하지만 다른 벡터 공간에 대해 unit이 아닌 벡터를 가져올 수 있습니다. 따라서 중심 샘플링 된 특성에서 파생물을 요청할 때에는 응용 프로그램의 책임입니다.

실제로 파생 된 샘플과 중심 샘플링을 결합 하지 않는 것이 좋습니다. 중심 샘플링은 기본의 보간된 특성을 추정 하지 않는 상황에 유용할 수 있지만,이는 기본 가장자리가 LOD 파생 된 질감 샘플링 작업에서 사용할 수 없는 픽셀 (지속적으로 변경 되는 것이 아님) 또는 파생물을 교차 하는 위치를 이동 하는 것과 같은 장단점을 제공 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[부록](appendix.md)

[RS(래스터라이저) 단계](rasterizer-stage--rs-.md)

 

 