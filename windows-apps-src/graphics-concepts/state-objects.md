---
title: 상태 개체
description: 장치 상태가 상태 개체로 그룹화되어 상태 변경 비용이 크게 줄어듭니다. 상태 개체는 여러 개이며 각 상태 개체는 특정 파이프라인 단계의 상태 집합을 초기화합니다. 상태 개체는 Direct3D 버전에 따라 다릅니다.
ms.assetid: D998745C-2B75-4E59-9923-AD1A17A92E05
keywords:
- 상태 개체
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3437119979073a5cec27948fc90f954e06c2fc93
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707124"
---
# <a name="state-objects"></a>상태 개체


장치 상태가 상태 개체로 그룹화되어 상태 변경 비용이 크게 줄어듭니다. 상태 개체는 여러 개이며 각 상태 개체는 특정 파이프라인 단계의 상태 집합을 초기화합니다. 상태 개체는 Direct3D 버전에 따라 다릅니다.

## <a name="span-idinputlayoutspanspan-idinputlayoutspanspan-idinputlayoutspaninput-layout-state"></a><span id="Input_Layout"></span><span id="input_layout"></span><span id="INPUT_LAYOUT"></span>입력 레이아웃 상태


이 상태 그룹은 [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)가 입력 버퍼에서 데이터를 읽고 꼭짓점 셰이더가 사용하도록 어셈블하는 방식을 지정합니다. 여기에는 입력 버퍼 내 요소 수, 입력 데이터의 서명 등의 상태가 포함됩니다. IA(입력 어셈블러) 단계는 원형을 메모리에서 파이프라인으로 스트리밍합니다.

## <a name="span-idrasterizerspanspan-idrasterizerspanspan-idrasterizerspanrasterizer-state"></a><span id="Rasterizer"></span><span id="rasterizer"></span><span id="RASTERIZER"></span>래스터라이저 상태


이 상태 그룹은 [RS(래스터라이저) 단계](rasterizer-stage--rs-.md)를 초기화합니다. 이 개체는 fill 또는 cull 모드와 같은 상태를 포함하여 클리핑을 위해 가위 직사각형을 활성화하고 다중 샘플 매개 변수를 설정합니다. 이 단계에서는 원형을 픽셀로 래스터화하여 원형을 클리핑하고 뷰포트로 매핑하는 등의 작업을 수행합니다.

## <a name="span-iddepthstencilspanspan-iddepthstencilspanspan-iddepthstencilspandepth-stencil-state"></a><span id="DepthStencil"></span><span id="depthstencil"></span><span id="DEPTHSTENCIL"></span>깊이 스텐실 상태


이 상태 그룹은 [OM(출력 병합기) 단계](output-merger-stage--om-.md)의 깊이 스텐실 부분을 초기화합니다. 보다 구체적으로는, 이 개체는 깊이 및 스텐실 테스트를 초기화합니다.

## <a name="span-idblendspanspan-idblendspanspan-idblendspanblend-state"></a><span id="Blend"></span><span id="blend"></span><span id="BLEND"></span>혼합 상태


이 상태 그룹은 [OM(출력 병합기) 단계](output-merger-stage--om-.md)의 혼합 부분을 초기화합니다.

## <a name="span-idsamplerspanspan-idsamplerspanspan-idsamplerspansampler-state"></a><span id="Sampler"></span><span id="sampler"></span><span id="SAMPLER"></span>샘플러 상태


이 상태 그룹은 샘플러 개체를 초기화합니다. 샘플러 개체는 셰이더 단계가 메모리에서 텍스처를 필터링하는 데 사용합니다.

Direct3D에서는 샘플러 개체가 특정 텍스처에 바인딩되지 않고 연결된 리소스에 대해 필터링하는 방법을 설명만 합니다.

## <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>성능 고려 사항


API가 상태 개체를 사용하도록 설계하면 여러 가지 성능 이점이 생깁니다. 이러한 이점에는 개체 생성 시 상태를 확인, 하드웨어에 상태 개체 캐싱을 사용, 상태 설정 API 호출 시 (상태 대신 핸들을 상태 개체로 전달하여) 상태 수량이 크게 감소 등이 있습니다.

이러한 성능 향상을 실현하려면 응용 프로그램이 시작하는 시점에서 렌더 루프 훨씬 이전에 상태 개체를 생성해야 합니다. 상태 개체는 일단 생성된 후에는 변경할 수 없습니다. 대신, 삭제한 다음 다시 만들어야 합니다.

다양한 샘플러 상태 조합으로 여러 샘플러 개체를 만들 수 있습니다. 그런 다음 (샘플러 상태가 아니라) 개체에 핸들을 전달하는 적절한 "Set" API를 호출하여 샘플러 상태 변경이 이루어집니다. 그러면 호출 횟수와 데이터 양이 크게 감소하므로 상태 변경을 위한 각 렌더 프레임 동안 오버헤드가 크게 줄어듭니다.

또는, 앱에서 자동으로 효율적인 상태 개체 생성 및 삭제를 관리하는 효과 시스템을 사용할 수도 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

[뷰](views.md)

 

 




