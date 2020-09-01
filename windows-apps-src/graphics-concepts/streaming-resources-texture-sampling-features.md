---
title: 스트리밍 리소스 텍스처 샘플링 기능
description: 스트리밍 리소스 질감 샘플링 기능에는 매핑된 영역에 대 한 셰이더 상태 피드백 가져오기, 액세스 하는 모든 데이터가 리소스에 매핑되는지 여부 확인, 셰이더가 매핑되지 않은 mipmapped 스트리밍 리소스의 영역을 사용 하지 않도록 하는 데 도움이 되는 고정, 전체 질감 필터 공간에 대해 완전히 매핑되는 최소 LOD를 검색 하는 작업이 포함 됩니다.
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- 스트리밍 리소스 텍스처 샘플링 기능
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 823b7b6ba7835f62277e15fa41fb968c6f4a167f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173047"
---
# <a name="streaming-resources-texture-sampling-features"></a>스트리밍 리소스 텍스처 샘플링 기능


스트리밍 리소스 질감 샘플링 기능에는 매핑된 영역에 대 한 셰이더 상태 피드백 가져오기, 액세스 하는 모든 데이터가 리소스에 매핑되는지 여부 확인, 셰이더가 매핑되지 않은 mipmapped 스트리밍 리소스의 영역을 사용 하지 않도록 하는 데 도움이 되는 고정, 전체 질감 필터 공간에 대해 완전히 매핑되는 최소 LOD를 검색 하는 작업이 포함 됩니다.

## <a name="span-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>스트리밍 리소스 요구 사항 질감 샘플링 기능


여기에 설명 된 질감 샘플링 기능에는 [계층 2](tier-2.md) 수준의 스트리밍 리소스 지원이 필요 합니다.

## <a name="span-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>매핑 영역에 대 한 셰이더 상태 피드백


스트리밍 리소스를 읽거나 쓰는 모든 셰이더 명령은 상태 정보를 기록 합니다. 이 상태는 32 비트 임시 레지스터로 이동 하는 모든 리소스 액세스 명령에 대 한 선택적 추가 반환 값으로 노출 됩니다. 반환 값의 내용은 불투명 합니다. 즉, 셰이더 프로그램에의 한 직접 읽기는 허용 되지 않습니다. 그러나 [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) 함수를 사용 하 여 상태 정보를 추출할 수 있습니다.

## <a name="span-idfully_mapped_checkspanspan-idfully_mapped_checkspanspan-idfully_mapped_checkspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>완전히 매핑된 검사


[**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) 함수는 메모리 액세스에서 반환 된 상태를 해석 하 고 액세스 하는 모든 데이터가 리소스에서 매핑되는지 여부를 나타냅니다. **CheckAccessFullyMapped** 는 데이터가 매핑 되었으면 True (0xffffffff)를, 데이터가 매핑되지 않은 경우 false (0x00000000)를 반환 합니다.

필터 작업 중에 지정 된 텍셀의 가중치가 0.0이 됩니다. 예는 텍셀 센터에 직접 속하는 질감 좌표를 포함 하는 선형 샘플입니다. 세 가지 다른 텍셀 (하드웨어에 따라 다를 수 있음)는 필터에 기여 하지만 0 가중치를 사용 합니다. 이러한 0 가중치 텍셀는 필터 결과에 영향을 주지 않으므로 **NULL** 타일을 사용 하는 경우에는 매핑되지 않은 액세스로 계산 되지 않습니다. 참고 여러 밉 수준을 포함 하는 질감 필터에도 동일한 보증이 적용 됩니다. mip 맵을 중 하나의 텍셀 매핑되지 않았지만 해당 텍셀의 가중치가 0 인 경우 해당 텍셀는 매핑되지 않은 액세스로 계산 되지 않습니다.

구성 요소가 4 개 미만인 형식 (예: DXGI \_ 형식 R8 unorm)에서 샘플링 하는 경우 \_ \_ 셰이더가 실제로 결과에 표시 되는 구성 요소에 관계 없이 **null** 타일에 있는 모든 텍셀는 **null** 로 매핑된 액세스가 보고 됩니다. 예를 들어, R8 unorm에서 읽고 yzw를 사용 하 여 \_ 셰이더에 읽기 결과를 마스킹 하는 것은 질감을 읽을 필요가 없는 것 처럼 보일 수 있습니다. 그러나 텍셀 주소가 **null** 매핑된 타일이 면 작업은 여전히 **null** 맵 액세스로 계산 됩니다.

셰이더는 상태를 확인 하 고 실패 시 원하는 작업 과정을 진행할 수 있습니다. 예를 들어, 작업 과정은 ' 누락 ' (UAV write를 통해)을 기록 하거나 매핑될 수 있는 LOD으로 다른 read 고정를 발급 하는 등의 작업을 수행할 수 있습니다. 응용 프로그램은 매핑된 타일 집합의 어느 부분에 액세스할 수 있다는 의미를 얻기 위해 성공적인 액세스를 추적 하고자 할 수 있습니다.

로깅에 대 한 한 가지 복잡 한 것은 액세스 된 정확한 타일 집합을 보고 하는 메커니즘이 없다는 것입니다. 응용 프로그램은 액세스에 사용 되는 좌표를 알고 LOD 명령을 사용 하는 방법에 따라 보수적인 추측을 수행할 수 있습니다. 예를 들어 [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod))는 하드웨어 LOD 계산을 반환 합니다.

또 다른 복잡 한 액세스는 동일한 타일에 대 한 많은 액세스를 가지 므로 많은 중복 로깅이 발생 하 고 메모리에 경합이 발생할 수 있습니다. 이전에 다른 곳에서 보고 된 경우 타일 액세스를 보고 하지 않을 수 있는 옵션을 하드웨어에 제공 하는 경우 편리 합니다. 아마도 이러한 추적의 상태를 API에서 다시 설정할 수 있습니다 (프레임 경계에 있을 수 있음).

## <a name="span-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>샘플 당 MinLOD 클램프


Mipmapped 스트리밍 리소스에서 매핑되지 않는 영역을 방지 하기 위해 샘플러 (필터링)를 사용 하는 대부분의 셰이더 명령에는 셰이더가 추가 float32 MinLOD 클램프 매개 변수를 질감 샘플에 전달할 수 있는 모드가 있습니다. 이 값은 기본 리소스가 아니라 뷰의 밉 맵 번호 공간에 있습니다.

하드웨어는 ` max(fShaderMinLODClamp,fComputedLOD) ` LOD 계산에서 동일한 위치에서 수행 됩니다. 여기서 리소스 MinLOD 클램프는 [**최대**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-max)() 이기도 합니다.

견본에 정의 된 sample LOD 클램프 및 기타 LOD 범위로 제한를 적용 한 결과가 빈 집합인 경우, 결과는 리소스 minLOD 클램프: 0의 구성 요소에 대 한 리소스 클램프: 0과 누락 된 구성 요소에 대 한 기본값입니다.

여기에 설명 된 샘플 별 minLOD 클램프를 이전의 하는 LOD 명령 (예: [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod))은 고정 및 unclamped LOD를 모두 반환 합니다. 이 LOD 명령에서 반환 된 고정 LOD에는 리소스 당 클램프를 비롯 한 모든 고정 반영 되지만 샘플 당 클램프는 반영 되지 않습니다. 샘플 별 클램프는 셰이더에 의해 제어 되 고 알려집니다. 따라서 셰이더 작성자는 원하는 경우 LOD 명령의 반환 값에 해당 클램프를 수동으로 적용할 수 있습니다.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>최소/최대 감소 필터링


응용 프로그램은 스트리밍 리소스에 대 한 매핑이 어떻게 표시 되는지 알려주는 자체 데이터 구조를 관리 하도록 선택할 수 있습니다. 예는 스트리밍 리소스의 모든 타일에 대 한 정보를 저장 하는 텍셀가 포함 된 표면입니다. 지정 된 타일 위치에서 매핑되는 첫 번째 LOD를 저장할 수 있습니다. 스트리밍 리소스를 샘플링 하는 것과 비슷한 방식으로이 데이터 구조를 샘플링 하 여 전체 질감 필터 공간에 대해 완전히 매핑되는 최소 LOD를 검색할 수 있습니다. 이 프로세스를 보다 쉽게 수행할 수 있도록 Direct3D 11.2에는 새로운 범용 샘플러 모드, min/max 필터링이 도입 되었습니다.

LOD 추적에 대 한 min/max 필터링 유틸리티는 깊이 표면의 필터링과 같은 다른 용도로 유용할 수 있습니다.

최소/최대 감소 필터링은 일반 텍스처 필터가 페치할 동일한 텍셀 집합을 페치하는 샘플러의 모드입니다. 그러나 값을 혼합 하 여 대답을 생성 하는 대신 구성 요소 단위로 인출 된 텍셀의 min () 또는 max ()를 반환 합니다 (예: 모든 R 값의 최소값은 모든 G 값의 최소 값과 별도로).

최소/최대 작업은 Direct3D 산술 정밀도 규칙을 따릅니다. 비교 순서는 중요 하지 않습니다.

Min/max가 아닌 필터 작업 중에는 지정 된 텍셀의 가중치가 0.0으로 종료 될 수도 있습니다. 예는 텍셀 center에 직접 속하는 질감 좌표를 포함 하는 선형 샘플입니다. 이 경우 3 개의 다른 텍셀 (하드웨어에 따라 다를 수 있음)는 필터에 기여 하지만 0 가중치를 사용 합니다. Min/max 필터에 0 가중치가 적용 되는 이러한 텍셀의 경우, 필터가 min/max 인 경우에도 이러한 텍셀는 결과에 영향을 주지 않습니다. 가중치는 min/max 필터 작업에 영향을 주지 않습니다.

이 기능에 대 한 지원은 스트리밍 리소스에 대 한 [계층 2](tier-2.md) 지원에 따라 달라 집니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스에 대한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 