---
title: 래스터화 규칙
description: 래스터화 규칙은 벡터 데이터가 래스터 데이터에 매핑되는 방법을 정의합니다.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- 래스터화 규칙
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c622c037f878d1ad34cdadf897dde10683532832
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7704474"
---
# <a name="rasterization-rules"></a>래스터화 규칙


래스터화 규칙은 벡터 데이터가 래스터 데이터에 매핑되는 방법을 정의합니다. 정수 위치로 래스터 데이터를 끌어온 다음, 최소한의 픽셀을 그리기 위해 정수 위치를 고르고 잘라내고, 꼭짓점별 특성에서 픽셀별 특성을 보간한 후 픽셀 셰이더에 전달합니다.

매핑되는 기본 요소의 유형과 데이터가 앨리어싱을 줄이기 위해 다중 샘플링을 사용하는지 여부에 따라 달라지는 몇 가지 규칙 유형이 있습니다. 다음 그림은 코너 케이스가 처리되는 방법을 보여 줍니다.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>삼각형 래스터화 규칙(다중 샘플링 없음)


삼각형 안의 모든 픽셀 중심이 그려집니다. 픽셀이 상단 왼쪽 규칙을 통과하는 경우, 내부에 있는 것으로 간주됩니다. 상단 왼쪽 규칙이란 삼각형의 상단 가장자리 또는 왼쪽 가장자리에 놓여 있는 픽셀 중심은 삼각형 내부에 있는 것으로 정의된다는 규칙입니다.

여기서

-   상단 가장자리는 다른 가장자리들에 정확히 수평이며 그 위에 있는 가장자리입니다.
-   왼쪽 가장자리는 삼각형의 왼쪽에 정확히 수평하지 않으면서 그 위에 있는 가장자리입니다. 삼각형에는 하나 또는 두 개의 왼쪽 가장자리가 있을 수 있습니다.

상단 왼쪽 규칙은 인접 삼각형을 한 번 그릴 수 있도록 합니다.

이 그림은 삼각형 내부에 있거나 상단 왼쪽 규칙을 따르기 때문에 그려지는 픽셀의 예를 보여 줍니다.

![상단 왼쪽 삼각형 래스터화의 예](images/d3d10-rasterrulestriangle.png)

픽셀의 밝은 회색 및 어두운 회색 외피는 픽셀들을 픽셀 그룹으로 표시해 어떤 삼각형 내부에 있는지 나타냅니다.

## <a name="span-idline1spanspan-idline1spanspan-idline1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>선 래스터화 규칙(앨리어싱됨, 다중 샘플링 없음)


선 래스터화 규칙은 다이아몬드 테스트 영역을 사용하여 선이 픽셀을 포함하는지 결정합니다. x-major 선(-1 &lt;= slope &lt;= +1인 선)의 경우, 다이아몬드 테스트 영역에는 아래 왼쪽 가장자리, 아래 오른쪽 가장자리, 하단 코너가 포함되며(실선으로 표시), 위 왼쪽 가장자리, 위 오른쪽 가장자리, 상단 코너, 왼쪽 코너, 오른쪽 코너는 다이아몬드에서 제외됩니다(점선으로 표시). y-major 선은 x-major 선이 아닌 모든 선입니다. 테스트 다이아몬드 영역은 오른쪽 코너도 포함된다는 점만 제외하고 x-major 선의 설명과 동일합니다.

다이아몬드 영역이 주어진 경우, 시작부터 끝을 향해 선을 따라 이동할 때 선이 픽셀의 다이아몬드 테스트 영역을 벗어나면 해당 선은 해당 픽셀을 포함합니다. 선 스트립은 선의 연속으로 그려지므로 동일하게 동작합니다.

다음 그림은 몇 가지 예를 보여 줍니다.

![앨리어싱된 선 래스터화의 예](images/d3d10-rasterrulesline.png)

## <a name="span-idline2spanspan-idline2spanspan-idline2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>선 래스터화 규칙(앤티앨리어싱됨, 다중 샘플링 없음)


앤티앨리어싱된 선은 사각형(너비 = 1)인 것처럼 래스터화됩니다. 이 사각형은 픽셀별 커버리지 값을 생성하는 렌더링 대상과 교차하며, 이 값은 증가하여 픽셀 셰이더 출력 알파 구성 요소가 됩니다. 다중 샘플링된 렌더링 대상에서 선을 그릴 때는 앤티앨리어싱이 수행되지 않습니다.

앤티앨리어싱된 선 렌더링을 수행하는 유일한 "최선의" 방법은 없다고 간주됩니다. Direct3D는 다음 그림에 나온 메서드를 가이드라인으로 채택합니다. 이 메서드는 경험에서 도출된 것으로서 바람직하다고 생각되는 여러 시각적 속성을 보여 줍니다.

하드웨어가 이 알고리즘에 정확히 일치할 필요는 없습니다. 이 참조에 대한 테스트는 아래에 자세히 나열된 몇 가지 원칙에 따라 "합리적인" 허용 오차를 둠으로써 다양한 하드웨어 구현과 필터 커널 크기를 허용해야 합니다. 하지만 하드웨어 구현에서 허용되는 이 유연성 중 어느 것도 단순한 선 그리기 및 그 모양의 관찰/측정 외에는 Direct3D를 통해 응용 프로그램으로 전달될 수 없습니다.

![앤티앨리어싱된 선 래스터화의 예](images/d3d10-rasterruleslineaa.png)

이 알고리즘은 농도가 균일하고 들쭉날쭉한 가장자리나 브레이딩(braiding)이 최소인 상대적으로 매끄러운 선을 생성합니다. 닫힌 선의 모아레 패턴 형성이 최소화됩니다. 끝에서 끝까지 배치된 선 세그먼트 간 접합에 좋은 커버리지가 있습니다.

필터 커널은 가장자리 블러의 양과 감마 보정으로 인한 농도 변화 사이의 합리적인 절충입니다. 커버리지 값은 [OM(출력 병합기) 단계](output-merger-stage--om-.md)에 의해 다음 수식에 따라 픽셀 셰이더 o0.a(srcAlpha)로 증가됩니다.

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>점 래스터화 규칙(다중 샘플링 없음)


점은 삼각형 래스터화 규칙을 사용하는 Z 패턴의 삼각형 2개로 구성된 것으로 해석됩니다. 좌표는 1픽셀 너비의 정사각형의 중심을 식별합니다. 점에는 컬링이 없습니다.

다음 그림은 몇 가지 예를 보여 줍니다.

![점 래스터화의 예](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>다중 샘플 앤티앨리어싱 래스터화 규칙


다중 샘플 앤티앨리어싱(MSAA)은 여러 하위 샘플 위치에서 픽셀 커버리지 및 깊이-스텐실 테스트를 사용하여 기하 도형 앨리어싱을 줄입니다. 성능 향상을 위해 포함된 하위 픽셀에서 셰이더 출력을 공유함으로써 포함된 픽셀마다 픽셀별 계산이 한 번 수행됩니다. 다중 샘플 앤티앨리어싱은 표면 앨리어싱을 줄이지 않습니다. 샘플 위치 및 재구성 함수는 하드웨어 구현에 따라 달라집니다.

다음 그림은 몇 가지 예를 보여 줍니다.

![다중 샘플 앤티앨리어싱 래스터화의 예](images/d3d10-rasterrulesmsaa.png)

샘플 위치의 수는 다중 샘플 모드에 따라 달라집니다. 꼭짓점 특성은 픽셀 중심에서 보간되는데, 여기서 픽셀 셰이더가 호출되기 때문입니다(중심이 포함되지 않으면 이것이 외삽이 됩니다). 특성은 중심 샘플링될 특성으로 픽셀 셰이더에서 플래그 지정할 수 있는데, 이로 인해 비포함 픽셀은 픽셀 영역과 기본 요소의 교차에서 특성을 보간합니다.

각각의 2x2 픽셀 영역마다 하나의 픽셀 셰이더가 실행되어 도함수 계산(x 델타와 y 델타 사용)을 지원합니다. 이는 최소 2x2 퀀텀을 채우는 것으로 표시되는 것보다(다중 샘플링과 독립적) 더 많이 셰이더 호출이 이루어짐을 의미합니다. 샘플별 깊이-스텐실 테스트를 통과하는 각각의 포함된 샘플에 대해 셰이더 결과가 기록됩니다.

일반적으로 기본 요소의 래스터화 규칙은 다음 경우를 제외하고 다중 샘플링 앤티앨리어싱에 의해 변경되지 않습니다.

-   삼각형의 경우, 각 샘플 위치(픽셀 중심이 아님)에 대해 커버리지 테스트가 수행됩니다. 포함된 샘플 위치가 둘 이상인 경우, 특성이 픽셀 중심에서 보간된 상태로 픽셀 셰이더가 한 번 실행됩니다. 포함된 각 샘플 위치에 대해 깊이/스텐실 테스트를 통과하는 픽셀에 결과가 저장(복제)됩니다.

    선은 선 너비가 1.4인 2개의 삼각형으로 이루어진 사각형으로 취급됩니다.

-   점의 경우, 각 샘플 위치(픽셀 중심이 아님)에 대해 커버리지 테스트가 수행됩니다.

[load](https://msdn.microsoft.com/library/windows/desktop/bb509694)를 사용하여 셰이더로 다시 읽어올 수 있는 렌더링 대상에 다중 샘플링 형식을 사용할 수 있습니다. 셰이더가 액세스하는 개별 샘플에는 resolve가 필요하지 않기 때문입니다. 다중 샘플 리소스에는 깊이 형식이 지원되지 않으므로 깊이 형식은 렌더링 대상으로만 제한됩니다.

유형 없는 형식은 다중 샘플링을 지원하여 리소스 뷰가 데이터를 다른 방식으로 해석할 수 있습니다. 예를 들어 R8G8B8A8\_TYPELESS를 사용하여 다중 샘플 리소스를 만들고 R8G8B8A8\_UINT 형식이 포함된 렌더링 대상 뷰 리소스를 사용하여 렌더링한 다음 R8G8B8A8\_UNORM 데이터 형식이 포함된 다른 리소스로 콘텐츠를 해결할 수 있습니다.

### <a name="span-idhardwaresupportspanspan-idhardwaresupportspanspan-idhardwaresupportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>하드웨어 지원

API는 품질 수준 수를 통해 다중 샘플링에 대한 하드웨어 지원을 보고합니다. 예를 들어 0 품질 수준은 하드웨어가 다중 샘플링을 지원하지 않음(특정 형식 및 품질 수준에서)을 뜻합니다. 품질 수준 3은 하드웨어가 세 가지 샘플 레이아웃 및/또는 resolve 알고리즘을 지원한다는 뜻입니다. 다음을 가정할 수도 있습니다.

-   다중 샘플링을 지원하는 모든 형식은 해당 패밀리의 모든 형식에 대해 동일한 품질 수준 수를 지원합니다.
-   다중 샘플링을 지원하고 \_UNORM, \_SRGB, \_SNORM 또는 \_FLOAT 형식을 가진 모든 형식도 해결을 지원합니다.

### <a name="span-idcentroidsamplingspanspan-idcentroidsamplingspanspan-idcentroidsamplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>다중 샘플 앤티앨리어싱 시 특성의 중심 샘플링

기본적으로 꼭짓점 특성은 다중 샘플 앤티앨리어싱 도중 픽셀 중심에 보간됩니다. 픽셀 중심이 포함되지 않는 경우, 특성은 픽셀 중심으로 외삽됩니다. 중심 시맨틱을 포함하는(픽셀이 완전히 포함되지 않는다고 가정) 픽셀 셰이더 입력이 픽셀의 포함된 영역 내 어딘가(가능하다면 포함된 샘플 위치 중 하나)에서 샘플링될 경우. 샘플 마스크(래스터라이저 상태에 의해 지정)는 중심 계산 전에 적용됩니다. 따라서 마스크 아웃된 샘플은 중심 위치로 사용되지 않습니다.

참조 래스터라이저는 다음과 비슷한 중심 샘플링의 샘플 위치를 선택합니다.

-   샘플 마스크는 모든 샘플을 허용합니다. 픽셀이 포함되거나 샘플 중 포함된 샘플이 없는 경우, 픽셀 중심을 사용합니다. 그렇지 않은 경우, 첫 번째 포함된 샘플이 픽셀 중심부터 시작해 바깥쪽 순서로 선택됩니다.
-   샘플 마스크는 하나를 제외한 모든 샘플을 끕니다(일반적 시나리오). 응용 프로그램은 싱글 비트 샘플 마스크 값을 순환하고 중심 샘플링을 사용해 각 샘플의 장면을 다시 렌더링함으로써 멀티패스 슈퍼샘플링을 구현할 수 있습니다. 이렇게 하려면 응용 프로그램이 도함수를 조정하여 보다 높은 텍스처 샘플링 밀도를 위해 더욱 상세한 텍스처 mips를 적절히 선택해야 합니다.

### <a name="span-idderivativecalculationsspanspan-idderivativecalculationsspanspan-idderivativecalculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>다중 샘플링 시 도함수 계산

픽셀 셰이더는 항상 최소 2x2 픽셀 영역을 사용하여 실행돼 도함수 계산을 지원합니다. 도함수는 인접 픽셀의 데이터 사이의 델타를 취해 계산됩니다(각 픽셀의 데이터가 수평 또는 수직 단위 간격으로 샘플링되었다고 가정). 이것은 다중 샘플링의 영향을 받지 않습니다.

중심 샘플링된 특성에서 도함수가 요청되면 하드웨어 계산이 조정되지 않아 부정확한 도함수가 초래될 수 있습니다. 셰이더는 렌더링 대상 공간에서 단위 벡터가 필요하지만 다른 일부 벡터 공간에 대해 비단위 벡터를 얻을 수 있습니다. 따라서 중심 샘플링되는 특성으로부터 도함수를 요청할 때 주의를 표시하는 것은 응용 프로그램의 책임입니다.

사실 도함수와 중심 샘플링을 결합하지 않는 것이 좋습니다. 중심 샘플링은 기본 요소의 보간된 특성이 외삽되지 않는 것이 중요한 상황에서 유용할 수 있지만 이 경우, 기본 요소 가장자리가 픽셀을 가로지르는 곳에서 (연속적으로 변하는 것이 아니라) 점프하는 것처럼 보이는 특성이나 LOD를 도출하는 텍스처 샘플링 연산이 사용할 수 없는 도함수와 같은 상충 관계가 따릅니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[부록](appendix.md)

[RS(래스터라이저) 단계](rasterizer-stage--rs-.md)

 

 




