---
title: 스트리밍 리소스 텍스처 샘플링 기능
description: 스트리밍 리소스 텍스처 샘플링 기능에는 매핑된 영역에 대한 셰이더 상태 피드백 받기, 액세스되는 모든 데이터가 리소스에서 매핑되었는지 확인, 비매핑으로 알려진 mipmap 스트리밍 리소스의 영역을 셰이더가 피하도록 돕는 고정, 전체 텍스처 필터 공간에 대해 완전히 매핑된 최소 LOD 발견이 포함됩니다.
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- 스트리밍 리소스 텍스처 샘플링 기능
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb0e870aa467641f82d24f03278a199ab56d0c8d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370970"
---
# <a name="streaming-resources-texture-sampling-features"></a>스트리밍 리소스 텍스처 샘플링 기능


스트리밍 리소스 텍스처 샘플링 기능에는 매핑된 영역에 대한 셰이더 상태 피드백 받기, 액세스되는 모든 데이터가 리소스에서 매핑되었는지 확인, 비매핑으로 알려진 mipmap 스트리밍 리소스의 영역을 셰이더가 피하도록 돕는 고정, 전체 텍스처 필터 공간에 대해 완전히 매핑된 최소 LOD 발견이 포함됩니다.

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>리소스를 스트리밍의 요구 사항을 질감 샘플링 기능


여기서 설명하는 텍스처 샘플링 기능은 스트리밍 리소스에 대한 [계층 2](tier-2.md) 수준의 지원이 필요합니다.

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>매핑된 영역에 대 한 셰이더 상태 피드백


스트리밍 리소스에 대한 읽기 및/또는 쓰기를 위한 셰이더 명령으로 인해 상태 정보가 기록됩니다. 이 상태는 32비트 임시 레지스터로 가는 모든 리소스 액세스 명령에서 선택적 추가 반환 값으로 노출됩니다. 반환 값의 내용은 불투명합니다. 즉, 셰이더 프로그램의 직접 읽기가 허용되지 않습니다. 하지만 [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) 함수를 사용하여 상태 정보를 추출할 수 있습니다.

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>매핑된 완벽 하 게 확인


[  **CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) 함수는 메모리 액세스에서 반환된 상태를 해석하고 액세스되는 모든 데이터가 리소스에서 매핑되었는지 여부를 나타냅니다. **CheckAccessFullyMapped**는 데이터가 매핑된 경우 true(0xFFFFFFFF), 매핑되지 않은 경우 false(0x00000000)를 반환합니다.

필터 작업 도중, 지정된 텍셀의 가중치가 0.0이 되는 경우가 가끔 있습니다. 예로 텍셀 센터에서 직접 해당 하는 질감 좌표를 사용 하 여 선형 샘플: (이들은 깨달음은 하드웨어에 따라 달라질 수 있습니다.) 다른 3 텍셀 필터에 있지만 0 가중치를 사용 하 여 영향을 줍니다. 이러한 가중치 0 텍셀은 필터 결과에 전혀 기여하지 않습니다. 따라서 이들 텍셀이 **NULL** 타일 위에 오는 경우 매핑되지 않은 액세스로 계산되지 않습니다. 여러 밉 수준을 포함하는 텍스처 필터에도 동일한 규칙이 적용됩니다. mipmap 중 하나에서 텍셀이 매핑되지 않았지만 이러한 텍셀의 가중치가 0일 경우 해당 텍셀은 매핑되지 않은 액세스로 계산되지 않습니다.

4 개 구성 요소가 포함 된 형식에서 샘플링 하는 경우 (DXGI 등\_형식\_R8\_UNORM)에 속하는 모든 텍셀 **NULL** 타일에서 결과를 **NULL** 액세스 중인을 셰이더 조사 결과에 구성 요소에 관계 없이 보고 매핑됩니다. 예를 들어, R8에서 읽는\_UNORM 및 읽기 결과.gba/.yzw를 사용 하 여 셰이더를 마스킹 질감을 전혀 읽이 필요가 표시 하지 않습니다. 하지만 텍셀 주소가 **NULL** 매핑된 타일이라면 작업은 여전히 **NULL** 맵 액세스로 계산됩니다.

셰이더는 상태를 확인하고 실패 시 원하는 조치를 사용할 수 있습니다. 예를 들어, 조치는 로깅 '누락'(예를 들어 UAV 쓰기를 통해) 및/또는 매핑된 것으로 알려진 정교하지 않은 LOD로 고정된 다른 읽기 발급이 될 수 있습니다. 응용 프로그램은 매핑된 타일 중 얼마나 액세스되었는지 파악하기 위해 성공한 액세스도 추적하기를 원할 수 있습니다.

로깅에서 한 가지 문제는 액세스되었을 타일의 집합을 정확하게 보고하는 메커니즘이 존재하지 않는다는 것입니다. 응용 프로그램은 액세스 시 사용한 좌표를 기반으로 또한 LOD 명령을 사용하여 보수적 추측을 할 수 있습니다. 예를 들어 [**tex2Dlod**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod))가 하드웨어 LOD 계산을 반환합니다.

또 하나의 문제는 많은 액세스가 동일한 타일로 이루어진다는 것이고, 따라서 상당한 중복 로깅이 불가피하여 메모리 경합이 발생할 가능성이 있다는 점입니다 이미 다른 곳에 보고된 타일 액세스는 보고하지 않도록 하는 옵션이 하드웨어에 제공된다면 편리할 것입니다. 아마도 이러한 추적의 상태는 API에서 재설정할 수 있을 것입니다(예를 들어 프레임 경계에서).

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>샘플 당 MinLOD 제한


셰이더가 밉매핑된 스트리밍 리소스에서 매핑되지 않은 것이 확인된 영역을 회피할 수 있도록 샘플러(필터링)을 사용하는 대부분의 셰이더 명령에는 셰이더가 추가 float32 MinLOD 클램프 매개 변수를 텍스처 샘플로 전달하도록 허용하는 모드가 있습니다. 이 값은 기본 리소스와는 달리 뷰의 mipmap 번호 공간에 위치합니다.

하드웨어는 LOD 계산에서 리소스당 MinLOD 클램프가 이루어지는 동일한 위치에서` max(fShaderMinLODClamp,fComputedLOD) `를 수행합니다. 이는 [**max**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-max)()이기도 합니다.

샘플 당 세밀도 제한 하 고 샘플러에 정의 된 다른 LOD clamps 적용 한 결과 빈 집합을 이면 결과가 같습니다 범위 액세스 결과에서 리소스별 minLOD 제한: 화면 형식 및 누락 된 구성 요소에 대 한 기본값의 구성 요소에 대 한 0입니다.

여기서 설명하는 샘플당 minLOD 클램프보다 앞선 LOD 명령(예: [**tex2Dlod**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod))은 고정된 LOD와 고정되지 않은 LOD를 모두 반환합니다. 이 LOD 명령에서 반환된 고정된 LOD는 리소스당 클램프를 포함한 모든 클램프를 반영하지만 샘플당 클램프는 반영하지 않습니다. 샘플당 클램프는 항상 셰이더에 의해 제어 및 인지되므로 셰이더 작성자는 필요한 경우 수동으로 해당 클램프를 LOD 명령의 반환 값에 적용할 수 있습니다.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>최소/최대 감소 필터링


응용 프로그램은 스트리밍 리소스의 매핑 모양을 알려주는 자체 데이터 구조를 관리하도록 선택할 수 있습니다. 스트리밍 리소스의 모든 타일에 대한 정보를 저장하는 텍셀을 포함하는 표면이 한 예입니다. 지정된 타일 위치에서 매핑된 첫 번째 LOD를 저장할 수 있을 것입니다. 이 데이터 구조를 스트리밍 리소스를 샘플링하려는 것과 유사한 방식으로 신중하게 샘플링한다면 전체 텍스처 필터 공간에 대해 완전히 매핑된 최소 LOD를 발견할 수도 있습니다. 이 프로세스를 용이하게 만들기 위해 Direct3D 11.2는 새로운 범용 샘플러 모드인 최소/최대 필터링을 도입했습니다.

LOD 추적에 대한 최소/최대 필터링 유틸리티는 아마도 깊이 표면의 필터링과 같은 다른 용도에 유용할 것입니다.

최소/최대 감소 필터링은 일반 텍스처 필터가 가져오는 것과 동일한 텍셀 집합을 가져오는 하나의 샘플러 모드입니다. 하지만 값을 혼합하여 답변을 생성하는 대신, 구성 요소별로 가져온 텍셀의 min() 또는 max()를 반환합니다(예를 들어 모든 G 값의 최소와 별도로 모든 R 값의 최소 등).

최소/최대 연산은 Direct3D 연산 정밀도 규칙을 따릅니다. 비교 순서는 중요하지 않습니다.

최소/최대가 아닌 필터 작업 도중, 지정된 텍셀의 가중치가 0.0이 되는 경우가 가끔 있습니다. 텍스처 좌표가 텍셀 중앙 바로 위에 오는 선형 샘플이 한 예입니다. 이 경우, 다른 세 텍셀(어느 텍셀인지는 하드웨어에 따라 달라질 수 있음)이 필터에 기여하지만 가중치는 0입니다. 비 최소/최대 필터에서 가중치가 0이 되는 이러한 텍셀은 필터가 최소/최대라도 결과에 기여하지 않습니다(또한 가중치가 최소/최대 필터 작업에 달리 영향을 미치지 않음).

이 기능의 지원은 스트리밍 리소스에 대한 [계층 2](tier-2.md) 지원 여부에 따라 결정됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[리소스를 스트리밍에 대 한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 




