---
title: 스텐실 버퍼
description: 스텐실 버퍼는 특수 효과를 생성하기 위해 이미지의 마스크 픽셀에 사용됩니다.
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- 스텐실 버퍼
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf4cd6ecb325bf0a3ce4a884361c0d098f9e1f05
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044882"
---
# <a name="stencil-buffers"></a>스텐실 버퍼


*스텐실 버퍼*는 특수 효과를 생성하기 위해 이미지의 마스크 픽셀에 사용됩니다. 마스크 픽셀은 픽셀을 그릴지 여부를 제어합니다. 이러한 특수 효과에는 합성, 데칼, 디졸브, 페이드, 스와이프, 아웃라인과 실루엣, 양면 스텐실이 포함됩니다. 몇 가지 보다 일반적인 효과는 다음과 같습니다.

스텐실 버퍼는 픽셀 단위로 렌더링 대상 표면에 그리기를 활성화하거나 비활성화합니다. 가장 기본적인 수준에서, 응용 프로그램이 렌더링된 이미지의 섹션을 표시되지 않도록 마스킹할 수 있습니다. 응용 프로그램은 디졸브, 데칼, 아웃라인과 같은 특수 효과에 스텐실 버퍼를 자주 사용합니다.

스텐실 버퍼 정보는 z 버퍼 데이터에 포함됩니다.

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>스텐실 버퍼 작동 방식


Direct3D는 픽셀 단위로 스텐실 버퍼의 내용을 테스트합니다. 대상 표면의 각 픽셀에 대해, 스텐실 버퍼 내 해당 값, 스텐실 참조 값, 스텐실 마스크 값을 사용하여 테스트를 수행합니다. 테스트가 통과하면 Direct3D가 작업을 수행합니다. 테스트는 다음 단계를 사용하여 수행됩니다.

1.  스텐실 참조 값과 스텐실 마스크에 대해 비트 AND 연산을 수행합니다.
2.  현재 픽셀의 스텐실 버퍼 값과 스텐실 마스크에 대해 비트 AND 연산을 수행합니다.
3.  비교 함수를 사용하여 1단계 결과와 2단계 결과를 비교합니다.

위 단계는 다음 코드 줄에 나와 있습니다.

```
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef는 스텐실 참조 값을 나타냅니다.
-   StencilMask는 스텐실 마스크 값을 나타냅니다.
-   CompFunc는 비교 함수입니다.
-   StencilBufferValue는 현재 픽셀의 스텐실 버퍼 내용입니다.
-   앰퍼샌드(&) 기호는 비트 AND 연산을 나타냅니다.

현재 픽셀은 스텐실 테스트가 통과하면 대상 표면에 기록되고, 실패하면 무시됩니다. 기본 비교 동작은 각 연산을 설정 하는 방법에 관계 없이 픽셀을 작성 하는 것입니다. 원하는 비교 함수를 식별 하는 열거 형식의 값을 변경 하 여이 동작을 변경할 수 있습니다.

응용 프로그램이 스텐실 버퍼의 작업을 사용자 지정할 수 있습니다. 비교 함수, 스텐실 마스크 및 스텐실 참조 값을 설정할 수 있습니다. 또한 스텐실 테스트가 통과 또는 실패할 때 Direct3D가 수행하는 작업을 제어할 수도 있습니다.

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>합성


응용 프로그램에서 스텐실 버퍼를 사용하여 2D 또는 3D 이미지를 3D 장면으로 합칠 수 있습니다. 스텐실 버퍼 안의 마스크는 렌더링 대상 표면의 특정 영역을 폐색하는 데 사용합니다. 그러면 텍스트 또는 비트맵과 같은 저장된 2D 정보가 폐색된 영역에 기록될 수 있습니다. 또는, 응용 프로그램이 렌더링 대상 표면의 스텐실 마스크된 영역에 추가 3D 원형을 렌더링할 수 있습니다. 심지어 전체 장면을 렌더링할 수도 있습니다.

게임은 흔히 여러 3D 장면을 합성합니다. 예를 들어 드라이빙 게임은 일반적으로 백미러를 표시합니다. 미러에는 드라이버 후방의 3D 장면에 대한 뷰가 포함됩니다. 이 뷰는 기본적으로 드라이버의 전방 뷰와 합성된 두 번째 3D 장면입니다.

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>데칼링


Direct3D 응용 프로그램은 데칼링을 사용하여 특정 원형 이미지에서 어느 픽셀을 렌더링 대상 표면에 그릴지 제어할 수 있습니다. 응용 프로그램은 공면 다각형이 올바로 렌더링될 수 있도록 원형의 이미지에 데칼을 적용합니다.

예를 들어 도로에 타이어 마크와 노란색 선을 적용할 때 표시가 도로 바로 위에 나타나야 합니다. 그러나 표시와 도로의 z 값은 동일합니다. 따라서 깊이 버퍼가 두 표시를 명료하게 분리하지 못할 수 있습니다. 후면 원형의 일부 픽셀이 전면 원형 위에, 또는 그 반대로 렌더링될 수 있습니다. 결과 이미지가 프레임마다 어른거려 보입니다. 이 효과를 *z 충돌* 또는 *플리머링*이라고 합니다.

이 문제를 해결하기 위해 스텐실을 사용하여 데칼이 표시될 후면 원형의 섹션을 마스킹합니다. Z 버퍼링을 끄고 전면 원형의 이미지를 렌더링 대상 표면의 마스킹된 영역에 렌더링합니다.

다중 텍스처 혼합을 사용하여 이 문제를 해결할 수 있지만 그러면 응용 프로그램이 생성할 수 있는 다른 특수 효과의 수가 제한됩니다. 스텐실 버퍼를 사용하여 데칼을 적용하면 다른 효과를 위한 텍스처 혼합 단계를 확보할 수 있습니다.

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>디졸브, 페이드 및 스와이프


응용 프로그램이 디졸브, 스와이프, 페이드와 같이 영화와 비디오에서 흔히 사용되는 특수 효과를 이용하는 경우가 점점 늘고 있습니다.

디졸브에서는 한 이미지가 매끄러운 프레임 시퀀스에서 점진적으로 다른 이미지로 대체됩니다. Direct3D가 다중 텍스처 혼합을 사용하여 동일한 효과를 구현하는 방법을 제공하지만, 디졸브를 위해 스텐실 버퍼를 사용하는 응용 프로그램은 디졸브를 구현하면서 텍스처 혼합 기능을 다른 효과에 사용할 수 있습니다.

응용 프로그램은 디졸브를 수행할 때 두 개의 이미지를 렌더링해야 합니다. 응용 프로그램은 스텐실 버퍼를 사용하여 각 이미지에서 어느 픽셀을 렌더링 대상 표면에 그릴지 제어합니다. 일련의 스텐실 마스크를 정의하여 연속 프레임에서 스텐실 버퍼로 복사할 수 있습니다. 또는, 첫 번째 프레임에 대해 기본 스텐실 마스크를 정의하고 점증하도록 변경할 수 있습니다.

디졸브 시작 시, 응용 프로그램은 스텐실 함수 및 스텐실 기능을 시작 이미지의 픽셀이 대부분 스텐실 테스트를 통과하도록 설정합니다. 종료 이미지의 픽셀은 대부분 스텐실 테스트에서 실패해야 합니다. 연속 프레임에서, 시작 이미지의 픽셀이 테스트를 통과하는 수가 점감하도록 스텐실 마스크가 업데이트됩니다. 프레임이 진행되면서 종료 이미지의 픽셀이 테스트에 실패하는 수가 점감합니다. 이러한 방식으로 응용 프로그램이 임의의 디졸브 패널을 사용하여 디졸브를 수행할 수 있습니다.

페이드 인 또는 페이드 아웃은 디졸브의 특별한 경우입니다. 페이드 인의 경우, 스텐실 버퍼가 검은색 또는 흰색 이미지에서 3D 장면의 렌더링으로 디졸브하는 데 사용됩니다. 페이드 아웃에서는 이와 반대로 응용 프로그램이 3D 장면 렌더링에서 시작하여 검은색 또는 흰색으로 디졸브합니다. 페이드는 원하는 임의의 패턴을 사용하여 수행할 수 있습니다.

Direct3D 응용 프로그램은 스와이프에 대해 비슷한 기법을 사용합니다. 예를 들어, 응용 프로그램이 왼쪽에서 오른쪽 스와이프를 수행하는 경우 종료 이미지가 시작 이미지 위를 왼쪽에서 오른쪽으로 점점 미끄러지는 것처럼 보입니다. 디졸브에서와 마찬가지로, 연속 프레임에서 스텐실 버퍼에 로드되는 일련의 스텐실 마스크를 정의하거나 시작 스텐실 마스크를 연속적으로 수정해야 합니다. 시작 이미지의 픽셀이 기록되지 않고 종료 이미지의 픽셀이 기록되도록 스텐실 마스크가 사용됩니다.

스와이프는 응용 프로그램이 스와이프의 역순으로 종료 이미지의 픽셀을 읽어야 한다는 점에서 디졸브보다 약간 복잡합니다. 즉, 스와이프가 왼쪽에서 오른쪽으로 이동한다면 응용 프로그램은 종료 이미지의 픽셀을 오른쪽에서 왼쪽으로 읽어야 합니다.

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>아웃라인과 실루엣


아웃라인과 실루엣과 같은 보다 추상적인 효과에 스텐실 버퍼를 사용할 수 있습니다.

응용 프로그램이 동일한 형태지만 약간 더 작은 원형의 이미지에 스텐실 마스크를 적용하는 경우 결과 이미지는 원형의 아웃라인만 포함합니다. 그러면 응용 프로그램이 이미지의 스텐실 마스크된 영역을 단색으로 채워 원형이 볼록하게 보이도록 할 수 있습니다.

스텐실 마스크가 렌더링하는 원형과 동일한 크기 및 형태인 경우 결과 이미지는 원형이 위치해야 할 구멍을 포함합니다. 그러면 응용 프로그램이 구멍을 검은색으로 채워 원형의 실루엣을 생성할 수 있습니다.

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>양면 스텐실


스텐실 버퍼를 사용하여 그림자를 그리는 데 섀도 볼륨이 사용됩니다. 응용 프로그램은 실루엣 가장자리를 계산하고 이들을 조명에서 3D 볼륨 집합으로 밀어내는 방식으로 기하 도형을 폐색하여 형성된 섀도 볼륨을 계산합니다. 이러한 볼륨이 스텐실 버퍼로 두 번 렌더링됩니다.

첫 번째 렌더는 전면 다각형을 그린 다음 스텐실 버퍼 값을 점증시킵니다. 두 번째 렌더는 섀도 볼륨의 후면 다각형을 그린 다음 스텐실 버퍼 값을 점감시킵니다. 일반적으로 모든 증가 및 감소 값 취소 서로 합니다. 그러나 장면 이미 일부 픽셀 그림자 볼륨 렌더링 되는 대로 z 버퍼 테스트 실패를 일으키는 일반 기 하 도형 렌더링 합니다. 스텐실 버퍼에 남은 값이 그림자의 픽셀에 해당합니다. 이 나머지 스텐실 버퍼 내용은 마스크로 사용되어 전부를 포함하는 큰 검은색 사각형을 장면에 알파 혼합합니다. 스텐실 버퍼가 마스크로 작용하므로 결과는 그림자 안의 픽셀이 어두워지는 것입니다.

즉, 그림자 기하 도형을 광원당 두 번 그립니다. 그러므로 GPU의 꼭짓점 처리량에 부담을 줍니다. 양면 스텐실 기능은 이 상황을 완화하기 위해 고안되었습니다. 이 접근 방법에는 전면 삼각형과 후면 삼각형에 대해 각각 하나씩, 두 개의 스텐실 상태 집합(이름은 아래에서 설명)이 있습니다. 이러한 방식으로 각 섀도 볼륨에 대해 광원당 한 번의 패스만 그립니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[깊이 및 스텐실 버퍼](depth-and-stencil-buffers.md)

 

 




