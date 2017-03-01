---
title: "깊이 스텐실 기능 구성"
description: "이 섹션에서는 출력 병합기 단계의 깊이 스텐실 버퍼와 깊이 스텐실 상태를 설정하는 단계를 다룹니다."
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- "깊이 스텐실 기능 구성"
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6814e5ee5aa99558830af4da3b43d102048f8bcb
ms.lasthandoff: 02/07/2017

---

# <a name="span-iddirect3dconceptsconfiguringdepth-stencilfunctionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>깊이 스텐실 기능 구성


이 섹션에서는 출력 병합기 단계의 깊이 스텐실 버퍼와 깊이 스텐실 상태를 설정하는 단계를 다룹니다.

깊이 스텐실 버퍼와 해당 깊이 스텐실 상태를 사용하는 방법을 숙지한 후에는 [고급 스텐실 기술](#advanced-stencil-techniques)을 참조하세요.

## <a name="span-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>깊이 스텐실 상태 만들기


깊이 스텐실 상태는 출력 병합기 단계에 [depth-stencil test(깊이 스텐실 테스트)](https://msdn.microsoft.com/library/windows/desktop/bb205120)를 수행하는 방법을 알려줍니다. 깊이 스텐실 테스트를 통해 지정된 픽셀을 그려야 하는지 여부가 결정됩니다.

## <a name="span-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>OM 단계에 깊이 스텐실 데이터 바인딩


깊이 스텐실 상태 바인딩

뷰를 사용하여 깊이 스텐실 리소스를 바인딩합니다.

렌더링 대상은 모두 같은 유형의 리소스여야 합니다. 다중 샘플링 앤티앨리어싱 사용 시 모든 바운드 렌더링 대상과 깊이 버퍼의 샘플 수가 동일해야 합니다.

버퍼를 렌더링 대상으로 사용할 경우 깊이 스텐실 테스트와 다중 렌더링 대상은 지원되지 않습니다.

-   최대 8개의 렌더링 대상을 동시에 바인딩할 수 있습니다.
-   모든 렌더링 대상의 크기가 모든 차원(3D의 경우 너비, 높이, 깊이, \*배열 유형의 경우 배열 크기)에서 동일해야 합니다.
-   렌더링 대상마다 데이터 형식이 다를 수 있습니다.
-   쓰기 마스크는 렌더링 대상에 기록되는 데이터를 제어합니다. 출력 쓰기 마스크는 렌더링 대상과 구성 요소 수준별로 렌더링 대상에 기록되는 데이터를 제어합니다.

## <a name="span-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>고급 스텐실 기술


깊이 스텐실 버퍼의 스텐실 부분은 합치기, 전사, 윤곽선 등의 렌더링 효과를 만드는 데 사용할 수 있습니다.

-   [합치기](#compositing)
-   [전사](#decaling)
-   [윤곽선 및 실루엣](#outlines-and-silhouettes)
-   [양면 스텐실](#two-sided-stencil)
-   [깊이 스텐실 버퍼를 텍스처로 읽기](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>합치기

응용 프로그램에서 스텐실 버퍼를 사용하여 2D 또는 3D 이미지를 3D 장면으로 합칠 수 있습니다. 스텐실 버퍼의 마스크는 렌더링 대상 표면의 영역을 폐색하는 데 사용됩니다. 그런 다음 텍스트와 비트맵 등의 저장된 2D 정보를 폐색된 영역에 쓸 수 있습니다. 또는 응용 프로그램에서 렌더링 대상 표면의 스텐실 마스킹 영역에 추가 3D 원형을 렌더링할 수 있습니다. 전체 장면을 렌더링할 수도 있습니다.

게임에서 여러 3D 장면을 합치는 경우가 많습니다. 예를 들어, 운전 게임은 대개 백미러를 표시합니다. 백미러에는 운전자 두의 3D 장면이 들어 있습니다. 이는 기본적으로 운전자의 전방 뷰와 합쳐진 두 번째 3D 화면입니다.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>전사

Direct3D 응용 프로그램에서는 전사를 사용하여 렌더링 대상 표면에 그릴 특정 원형 이미지의 픽셀을 제어합니다. 공면 다각형이 제대로 렌더링되도록 응용 프로그램에서 원형의 이미지에 전사를 적용합니다.

예를 들어, 도로에 바퀴 자국과 노랑 차선을 적용할 때 표시가 도로 바로 위에 표시되어야 합니다. 그러나 표시와 도로의 z 값이 동일합니다. 따라서 깊이 버퍼는 이 둘을 깨끗하게 나눌 수 없습니다. 뒤쪽 원형의 일부 픽셀을 앞쪽 원형의 위에 렌더링하거나 그 반대로 렌더링할 수 있습니다. 결과 이미지는 프레임 사이에서 희미하게 빛나는 것처럼 나타납니다. 이 효과를 z 플라이팅 또는 미광이라고 합니다.

이 문제를 해결하려면 스텐실을 사용하여 전사가 표시될 뒤쪽 원형의 섹션을 표시합니다. z 버퍼링을 끄고 앞쪽 원형의 이미지를 렌더링 대상 표면의 마스킹 오프 영역으로 렌더링합니다.

다중 텍스처 블렌딩을 사용하여 이 문제를 해결할 수 있습니다.

### <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">윤곽선 및 실루엣

윤곽선과 실루엣 등의 보다 추상적인 효과에 스텐실 버퍼를 사용할 수 있습니다.

응용 프로그램에서 2개의 렌더링 패스를 수행하는 경우(스텐실 마스크를 생성하는 렌더링 패스와 이미지에 스텐실 마스크를 적용하는 렌더링 패스, 단, 두 번째 패스의 원형이 조금 더 작음) 결과 이미지에는 원형의 윤곽선만 있습니다. 그런 다음 응용 프로그램에서 원형이 볼록해 보이게 이미지의 스텐실 마스킹 영역을 단색으로 채웁니다.

스텐실 마스크와 렌더링 중인 원형의 크기와 셰이프가 같은 경우 결과 이미지의 원형 위치에 구멍이 표시됩니다. 그러면 응용 프로그램에서 구멍을 검정으로 채워 원형의 실루엣을 생성합니다.

### <a name="span-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwo-sided-stencilspantwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span><span id="two-sided-stencil"></span>양면 스텐실

섀도 볼륨은 스텐실 버퍼로 섀도를 그리는 데 사용됩니다. 응용 프로그램에서는 기하 도형을 폐색하고 실루엣 가장자리를 계산한 후 조명에서 3D 볼륨 집합으로 밀어내서 섀도 볼륨 캐스트를 계산합니다. 그런 다음 이러한 볼륨은 스텐실 버퍼로 두 번 렌더링됩니다.

첫 번째 렌더링은 정면 다각형을 그리고 스텐실 버퍼 값을 늘립니다. 두 번째 렌더링은 섀도 볼륨의 후면 다각형을 그리고 스텐실 버퍼 값을 줄입니다.

일반적인 경우 늘어난 값과 줄어든 값 모두 서로 취소합니다. 그러나 일반 기하 도형으로 장면이 이미 렌더링되어 섀도 볼륨이 렌더링될 때 일부 픽셀이 z 버퍼 테스트에 실패하게 됩니다. 스텐실에 남은 값은 섀도에 있는 픽셀에 해당합니다. 이러한 나머지 스텐실 버퍼 콘텐츠는 모두 포괄하는 큰 검정 4색을 장면에 알파 혼합하기 위한 표시로 사용됩니다. 스텐실 버퍼가 표시 역할을 하여 결과적으로 섀도에 있는 픽셀이 어두워집니다.

이는 섀도 기하 도형이 광원당 두 번씩 그려져서 GPU의 꼭짓점 처리량에 압력을 줍니다. 양명 스텐실 기능은 이 상황을 완화하도록 설계되었습니다. 이 접근 방식에서는 2개의 스텐실 상태 집합(아래 이름이 지정됨)이 있습니다. 그 중 하나는 정면 삼각형용이고 다른 하나는 후면 삼각형용입니다. 이러한 방식으로 섀도 볼륨당, 조명당 단일 패스만 그려집니다.

### <a name="span-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>깊이 스텐실 버퍼를 텍스처로 읽기

셰이더에서 비활성 깊이 스텐실 버퍼를 텍스처로 읽을 수 있습니다. 깊이 스텐실 버퍼를 텍스처로 읽는 응용 프로그램은 2개의 패스로 렌더링됩니다. 첫 번째 패스는 깊이 스텐실 버퍼에 쓰고, 두 번째 패스는 버퍼에서 읽습니다. 이를 통해 셰이더에서 이전에 버퍼에 기록된 깊이 또는 스텐실 값을 현재 렌더링 중인 픽셀에 대한 값과 비교할 수 있습니다. 비교 결과를 사용하여 섀도 매핑 또는 파티클 시스템의 부드러운 파티클과 같은 효과를 만들 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[OM(출력 병합기) 단계](output-merger-stage--om-.md)

[그래픽 파이프라인](graphics-pipeline.md)

[출력 병합기 단계](https://msdn.microsoft.com/library/windows/desktop/bb205120)

