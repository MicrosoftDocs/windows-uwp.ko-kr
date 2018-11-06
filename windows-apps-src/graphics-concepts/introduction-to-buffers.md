---
title: 버퍼 소개
description: 버퍼 리소스는 완벽하게 입력된 데이터 컬렉션이며, 요소로 그룹화됩니다.
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- 버퍼 소개
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 14e78aec9afa361b2627d62d92f0ee7d7ab0565b
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6040858"
---
# <a name="introduction-to-buffers"></a>버퍼 소개


버퍼 리소스는 완벽하게 입력된 데이터 컬렉션이며, 요소로 그룹화됩니다. 버퍼는 *꼭짓점 버퍼*의 텍스처 좌표, *인덱스 버퍼*의 인덱스, *상수 버퍼*의 셰이더 상수 데이터, 위치 벡터, 일반 벡터, 디바이스 상태 등의 데이터를 저장합니다.

버퍼 요소는 1 ~ 4 개 구성 요소 구성 됩니다. 버퍼 요소에는 압축된 데이터 값(R8G8B8A8 표면 값), 8비트 정수 하나 또는 32비트 부동 소수점 값 네 개가 포함될 수 있습니다.

버퍼는 구조화되지 않은 리소스로 생성됩니다. 구조화 되지 않았기 때문에 버퍼는 mipmap 수준을 포함할 수 없습니다, 읽었을 때 필터링 얻을 수 없는 및 다중 샘플링 될 수 없습니다.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>버퍼 유형


Direct3D 11에서 지 원하는 버퍼 리소스 종류는 다음과 같습니다.

-   [꼭지점 버퍼](#vertex-buffer)
-   [인덱스 버퍼](#index-buffer)
-   [상수 버퍼](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>꼭 짓 점 버퍼

꼭 짓 점 버퍼는 기 하 도형을 정의 하는 데 꼭 짓 점 데이터를 포함 합니다. 꼭지점 데이터에는 위치 좌표, 색상 데이터, 텍스처 좌표 데이터, 법선 데이터 등이 포함되어 있습니다.

꼭 짓 점 버퍼의 간단한 예는 위치 데이터 포함 합니다. 다음 그림과 같이 표현할 수 있습니다.

![위치 데이터를 포함한 꼭짓점 버퍼의 그림](images/d3d10-resources-single-element-vb2.png)

3D 꼭짓점을 완벽하게 지정하는 데 필요한 모든 데이터를 포함하는 경우가 더 많습니다. 꼭짓점별 위치, 일반 및 텍스처 좌표가 포함된 꼭짓점 버퍼가 한 예입니다. 이 데이터는 일반적으로 다음 그림과 같이 꼭짓점별 요소 집합으로 구성됩니다.

![위치, 일반 및 텍스처 데이터가 포함된 꼭짓점 버퍼의 그림](images/d3d10-vertex-buffer-element.png)

이 꼭 짓 점 버퍼에는 꼭 짓 점별 데이터가 포함 되어 각 꼭 짓 점 세 개의 요소 (위치, 일반 및 텍스처 좌표)를 저장합니다. 위치 및 일반 좌표는 일반적으로 32비트 부동 소수점 3개를 사용하여 지정되고 텍스처 좌표는 32비트 부동 소수점 2개를 사용하여 지정됩니다.

꼭 짓 점 버퍼에서 데이터에 액세스 하는 꼭 짓 점 액세스와 다음 추가 버퍼 매개 변수를 알아야 할:

-   오프셋 - 버퍼 시작부터 첫 번째 꼭짓점의 데이터까지 바이트 수입니다.
-   BaseVertexLocation - 오프셋부터 해당 그리기 호출에 사용된 첫 번째 꼭짓점까지 바이트 수입니다.

꼭 짓 점 버퍼를 만들기 전에 레이아웃을 정의 해야 합니다. 입력 레이아웃 개체를 만든 후 [입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md)에 바인딩합니다.

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>인덱스 버퍼

인덱스 버퍼는 꼭 짓 점 버퍼에 대 한 정수 오프셋을 포함 하며 더 효율적으로 원형 렌더링 사용 됩니다. 인덱스 버퍼는 16비트 또는 32비트 인덱스의 순차 집합을 포함하며, 각 인덱스는 꼭짓점 버퍼에서 꼭짓점을 식별하는 데 사용됩니다. 인덱스 버퍼는 다음 그림과 같이 시각화할 수 있습니다.

![인덱스 버퍼의 그림](images/d3d10-index-buffer.png)

인덱스 버퍼에 저장된 순차 인덱스는 다음 매개 변수로 배치됩니다.

-   오프셋-인덱스 버퍼의 기본 주소 점까지에서 바이트 수입니다.
-   StartIndexLocation-첫 번째 인덱스 버퍼 요소에서 기본 주소 및 오프셋을 지정 합니다. 시작 위치 렌더링 하려면 첫 번째 인덱스를 나타냅니다.
-   IndexCount - 렌더링할 인덱스 수입니다.

인덱스 버퍼의 시작 인덱스 버퍼 기준 주소 오프셋 (바이트) + StartIndexLocation = \ * ElementSize (바이트)입니다.

이 계산 ElementSize 각 인덱스 버퍼 요소에는 2 개 또는 4 바이트의 크기를입니다.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>상수 버퍼

상수 버퍼를 사용 하면 파이프라인에 셰이더 상수 데이터를 효율적으로 제공할 수 있습니다. 스트림 출력 단계의 결과를 저장할 상수 버퍼를 사용할 수 있습니다. 개념적으로 상수 버퍼는 다음 그림과에서 같이 단일 요소 꼭 짓 점 버퍼 처럼 보입니다.

![셰이더 상수 버퍼의 그림](images/d3d10-shader-resource-buffer.png)

각 요소는 저장된 데이터의 형식에 의해 결정되는 1~4개 구성 요소 상수를 저장합니다.

상수 버퍼는 단일 바인딩 플래그를 다른 바인딩 플래그와 함께 사용할 수 없으며만 사용할 수 있습니다.

셰이더 상수 버퍼에서 셰이더를 읽으려면 HLSL 로드 함수를 사용 합니다. 각 셰이더 단계는 최대 15개의 셰이더 상수 버퍼를 허용하며 각 버퍼는 최대 4,096개 상수를 보관할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[꼭짓점 및 인덱스 버퍼](vertex-and-index-buffers.md)

 

 




