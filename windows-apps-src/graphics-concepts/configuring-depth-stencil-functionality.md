---
title: 깊이 스텐실 기능 구성
description: 이 섹션에서는 출력-병합기 단계에 대 한 깊이 스텐실 버퍼 및 깊이 스텐실 상태를 설정 하는 단계에 대해 설명 합니다.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- 깊이 스텐실 기능 구성
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2d9c33e9625f36bbf183df46d5cd590f9e66d3c4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162807"
---
# <a name="span-iddirect3dconceptsconfiguring_depth-stencil_functionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>깊이 스텐실 기능 구성


이 섹션에서는 출력-병합기 단계에 대 한 깊이 스텐실 버퍼 및 깊이 스텐실 상태를 설정 하는 단계에 대해 설명 합니다.

깊이 스텐실 버퍼 및 해당 깊이 스텐실 상태를 사용 하는 방법을 알고 있는 경우 [고급 스텐실 기술](#advanced-stencil-techniques)을 참조 하세요.

## <a name="span-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>깊이 스텐실 상태 만들기


깊이 스텐실 상태는 출력 병합기 단계에 [깊이 스텐실 테스트](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)를 수행 하는 방법을 알려줍니다. 깊이 스텐실 테스트는 지정 된 픽셀을 그릴지 여부를 결정 합니다.

## <a name="span-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>깊이 스텐실 데이터를 OM 스테이지에 바인딩


깊이 스텐실 상태를 바인딩합니다.

뷰를 사용 하 여 깊이 스텐실 리소스를 바인딩합니다.

렌더링 대상은 모두 동일한 리소스 유형 이어야 합니다. 다중 샘플 앤티엘리어싱을 사용 하는 경우 모든 바인딩된 렌더링 대상 및 깊이 버퍼의 샘플 수가 같아야 합니다.

버퍼를 렌더링 대상으로 사용 하는 경우 깊이 스텐실 테스트와 여러 렌더링 대상이 지원 되지 않습니다.

-   최대 8 개의 렌더링 대상을 동시에 바인딩할 수 있습니다.
-   모든 렌더링 대상의 크기는 모든 차원 (너비 및 높이, 배열 형식의 경우 3D 또는 배열 크기의 경우 깊이)에서 동일 해야 합니다 \* .
-   각 렌더링 대상의 데이터 형식이 다를 수 있습니다.
-   쓰기 마스크는 렌더링 대상에 기록 되는 데이터를 제어 합니다. 렌더링 대상에서 렌더링 대상에 기록 되는 데이터는 렌더링 대상 당 렌더링 대상, 구성 요소별 수준에서 제어 합니다.

## <a name="span-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>고급 스텐실 기술


깊이 스텐실 버퍼의 스텐실 부분을 사용 하 여 합성, decaling 및 개요와 같은 렌더링 효과를 만들 수 있습니다.

-   [합성](#compositing)
-   [Decaling](#decaling)
-   [개요 및 Silhouettes](#outlines-and-silhouettes)
-   [양면 스텐실](#two-sided-stencil)
-   [깊이 스텐실 버퍼를 질감으로 읽기](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>합성

응용 프로그램은 스텐실 버퍼를 사용 하 여 3D 장면에 2D 또는 3D 이미지를 합성 할 수 있습니다. 스텐실 버퍼의 마스크는 렌더링 대상 표면의 영역을 려 하는 데 사용 됩니다. 텍스트 또는 비트맵과 같은 저장 된 2D 정보를 폐색 영역에 쓸 수 있습니다. 또는 응용 프로그램에서 렌더링 대상 표면의 스텐실에 마스킹된 영역에 추가 3D 기본 형식을 렌더링할 수 있습니다. 전체 장면을 렌더링할 수도 있습니다.

게임은 종종 여러 3D 장면을 함께 복합 합니다. 예를 들어, 게임 구동은 일반적으로 후방 뷰 미러를 표시 합니다. 미러는 드라이버 뒤에 나오는 3D 장면의 뷰를 포함 합니다. 기본적으로 드라이버의 앞으로는 두 번째 3D 장면을 합성 합니다.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Decaling

Direct3D 응용 프로그램은 decaling를 사용 하 여 렌더링 대상 화면에 그려지는 특정 기본 이미지의 픽셀을 제어 합니다. 응용 프로그램은 decals를 기본 이미지에 적용 하 여 동일 평면상의 다각형이 올바르게 렌더링 되도록 합니다.

예를 들어, roadway에 타이어 표시와 노란색 선을 적용 하는 경우 표시는 도로의 바로 위에 표시 되어야 합니다. 그러나 표시 및 이동의 z 값은 동일 합니다. 따라서 깊이 버퍼는 둘 사이를 명확 하 게 분리 하지 못할 수 있습니다. 백 기본 형식의 일부 픽셀은 전면 기본 형식 위에 렌더링 될 수 있으며 그 반대의 경우도 마찬가지입니다. 결과 이미지가 프레임에서 프레임으로 shimmer 표시 됩니다. 이 효과를 z-싸 flimmering 또는 라고 합니다.

이 문제를 해결 하려면 스텐실을 사용 하 여 decal이 표시 되는 백 기본 형식의 섹션을 마스크 합니다. Z 버퍼링을 해제 하 고 front 기본 이미지 이미지를 렌더링 대상 표면의 마스킹된 영역으로 렌더링 합니다.

여러 질감 혼합을 사용 하 여이 문제를 해결할 수 있습니다.

### <a name="span-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">개요 및 Silhouettes

개요 및 silhouetting와 같은 좀 더 추상적인 효과를 위해 스텐실 버퍼를 사용할 수 있습니다.

응용 프로그램에서 두 개의 렌더링 패스를 사용 하 여 스텐실 마스크를 생성 하 고 두 번째 패스를 사용 하 여 스텐실 마스크를 이미지에 적용 하 고 두 번째 패스에서 기본 형식을 약간 작게 설정 하면 결과 이미지는 기본의 개요만 포함 합니다. 그러면 응용 프로그램에서 이미지의 스텐실에 마스킹된 영역을 단색으로 채워서 기본 모양을 볼록 하 게 만들 수 있습니다.

스텐실 마스크의 크기와 모양이 렌더링 하는 기본 형식과 같은 경우 결과 이미지에는 기본 형식이 되어야 하는 구멍이 포함 됩니다. 그런 다음 응용 프로그램은 기본의 실루엣을 생성 하는 검은색으로 구멍을 채울 수 있습니다.

### <a name="span-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo-sided-stenciltwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span><span id="two-sided-stencil">양면 스텐실

섀도 볼륨은 스텐실 버퍼를 사용 하 여 그림자를 그리는 데 사용 됩니다. 응용 프로그램은 실루엣 가장자리를 계산 하 고 광원에서 3D 볼륨 집합으로 extruding 하 여 occluding geometry로 캐스팅 된 섀도 볼륨을 계산 합니다. 이러한 볼륨은 스텐실 버퍼에 두 번 렌더링 됩니다.

첫 번째 렌더링은 전방 다각형을 그리고 스텐실 버퍼 값을 증가 시킵니다. 두 번째 렌더링은 그림자 볼륨의 후방 다각형을 그리고 스텐실 버퍼 값을 감소 시킵니다.

일반적으로 증가 하 고 감소 하는 모든 값은 서로 취소 됩니다. 그러나 섀도 볼륨이 렌더링 될 때 일부 픽셀이 z 버퍼 테스트에 실패 하도록 하는 일반적인 기 하 도형을 사용 하 여 장면을 이미 렌더링 했습니다. 스텐실 버퍼에 남아 있는 값은 그림자에 있는 픽셀에 해당 합니다. 이러한 나머지 스텐실 버퍼 내용은 마스크로 사용 되며, 크게 검정 4 개를 포괄 하는 알파를 장면에 알파 혼합 합니다. 마스크 역할을 하는 스텐실 버퍼의 결과로, 그림자에 있는 픽셀을 어둡게 만듭니다.

즉, 광원 마다 그림자 기 하 도형을 두 번 그려 GPU의 꼭 짓 점 처리량을 부담 합니다. 양면 스텐실 기능은 이러한 상황을 완화 하도록 설계 되었습니다. 이 방법에는 두 가지 스텐실 상태 집합 (아래 이름)이 있으며, 각각은 전면 삼각형에 대해 설정 되 고 다른 하나는 후면 삼각형에 사용 됩니다. 이러한 방식으로 한 번만 패스가 섀도 볼륨 마다 그려집니다.

### <a name="span-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>깊이 스텐실 버퍼를 질감으로 읽기

비활성 깊이 스텐실 버퍼는 셰이더에 질감으로 읽을 수 있습니다. 두 패스에서 질감이 렌더링 되는 깊이 스텐실 버퍼를 읽는 응용 프로그램은 첫 번째 패스는 깊이 스텐실 버퍼에 쓰고 두 번째 패스는 버퍼에서 읽습니다. 이를 통해 셰이더는 렌더링 되는 픽셀 현재 값에 대해 이전에 버퍼에 쓴 깊이 또는 스텐실 값을 비교할 수 있습니다. 비교 결과를 사용 하 여 파티클 시스템에서 그림자 매핑 또는 소프트 파티클과 같은 효과를 만들 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[OM(출력 병합기) 단계](output-merger-stage--om-.md)

[그래픽 파이프라인](graphics-pipeline.md)

[출력-병합기 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)