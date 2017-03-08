---
title: "버퍼 소개"
description: "버퍼 리소스는 완벽하게 입력된 데이터 컬렉션이며, 요소로 그룹화됩니다."
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- "버퍼 소개"
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ec2d181daca8f9ece8e1d364cd58234feb85977a
ms.lasthandoff: 02/07/2017

---

# <a name="introduction-to-buffers"></a>버퍼 소개


버퍼 리소스는 완벽하게 입력된 데이터 컬렉션이며, 요소로 그룹화됩니다. 버퍼는 *꼭짓점 버퍼*의 텍스처 좌표, *인덱스 버퍼*의 인덱스, *상수 버퍼*의 셰이더 상수 데이터, 위치 벡터, 일반 벡터, 장치 상태 등의 데이터를 저장합니다.

버퍼 요소는 1 ~ 4개 구성 요소로 이루어집니다. 버퍼 요소에는 압축된 데이터 값(R8G8B8A8 표면 값), 8비트 정수 하나 또는 32비트 부동 소수점 값 네 개가 포함될 수 있습니다.

버퍼는 구조화되지 않은 리소스로 생성됩니다. 버퍼가 구조화되지 않았기 때문에 Mipmap 수준을 포함할 수 없고, 읽었을 때 필터링될 수 없으며 다중 샘플링될 수 없습니다.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>버퍼 유형


Direct3D 11이 지원하는 버퍼 리소스 유형은 다음과 같습니다.

-   [꼭짓점 버퍼](#vertex-buffer)
-   [인덱스 버퍼](#index-buffer)
-   [상수 버퍼](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>꼭짓점 버퍼

꼭짓점 버퍼에 기하 도형을 정의하는 데 사용되는 꼭짓점 데이터가 포함됩니다. 꼭짓점 데이터에 위치 좌표, 색상 데이터, 텍스처 좌표 데이터, 일반 데이터 등이 포함됩니다.

위치 데이터만 포함하는 경우가 가장 단순한 꼭짓점 버퍼의 예입니다. 다음 그림과 같이 표현할 수 있습니다.

![위치 데이터를 포함한 꼭짓점 버퍼의 그림](images/d3d10-resources-single-element-vb2.png)

3D 꼭짓점을 완벽하게 지정하는 데 필요한 모든 데이터를 포함하는 경우가 더 많습니다. 꼭짓점별 위치, 일반 및 텍스처 좌표가 포함된 꼭짓점 버퍼가 한 예입니다. 이 데이터는 일반적으로 다음 그림과 같이 꼭짓점별 요소 집합으로 구성됩니다.

![위치, 일반 및 텍스처 데이터가 포함된 꼭짓점 버퍼의 그림](images/d3d10-vertex-buffer-element.png)

이 꼭짓점 버퍼는 꼭짓점별 데이터를 포함하고 각 꼭짓점은 세 가지 요소(위치, 일반 및 텍스처 좌표)를 저장합니다. 위치 및 일반 좌표는 일반적으로 32비트 부동 소수점 3개로 지정되고 텍스처 좌표는 32비트 부동 소수점 두 개로 지정됩니다.

꼭짓점 버퍼에서 데이터에 액세스하려면 액세스할 꼭짓점뿐 아니라 다음과 같은 추가 버퍼 매개 변수를 알아야 합니다.

-   오프셋 - 버퍼 시작부터 첫 번째 꼭짓점의 데이터까지 바이트 수입니다.
-   BaseVertexLocation - 오프셋부터 해당 그리기 호출에 사용된 첫 번째 꼭짓점까지 바이트 수입니다.

꼭짓점 버퍼를 만들기 전에 해당 레이아웃을 정의해야 합니다. 레이아웃 개체를 만든 후 [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)에 바인딩합니다.

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>인덱스 버퍼

인덱스 버퍼는 꼭짓점 버퍼에 대한 정수 오프셋을 포함하며, 원형을 효율적으로 렌더링하는 데 사용됩니다. 인덱스 버퍼는 16비트 또는 32비트 인덱스의 순차 집합을 포함하며, 각 인덱스는 꼭짓점 버퍼에서 꼭짓점을 식별하는 데 사용됩니다. 인덱스 버퍼는 다음 그림과 같이 시각화할 수 있습니다.

![인덱스 버퍼의 그림](images/d3d10-index-buffer.png)

인덱스 버퍼에 저장된 순차 인덱스는 다음 매개 변수로 배치됩니다.

-   오프셋 - 인덱스 버퍼 기본 주소의 바이트 수입니다.
-   StartIndexLocation - 오프셋과 기본 주소에서 첫 번째 인덱스 버퍼 요소를 지정합니다. 시작 위치는 렌더링할 첫 번째 인덱스를 나타냅니다.
-   IndexCount - 렌더링할 인덱스 수입니다.

인덱스 버퍼의 시작 = 인덱스 버퍼 기본 주소 + 오프셋(바이트) + StartIndexLocation \* ElementSize(바이트),

이 계산에서 ElementSize는 각 인덱스 버퍼 요소의 크기로, 2바이트 또는 4바이트입니다.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>상수 버퍼

상수 버퍼를 통해 파이프라인에 셰이더 상수 데이터를 효율적으로 제공할 수 있습니다. 상수 버퍼를 사용해 스트림 출력 단계의 결과를 저장할 수 있습니다. 개념적으로 상수 버퍼는 다음 그림과 같이 단일 요소 꼭짓점 버퍼처럼 보입니다.

![셰이더 상수 버퍼의 그림](images/d3d10-shader-resource-buffer.png)

각 요소는 저장된 데이터의 형식에 의해 결정되는 1 ~4개 구성 요소 상수를 저장합니다.

상수 버퍼는 다른 바인딩 플래그와 결합할 수 없는 단일 바인딩 플래그만 사용할 수 있습니다.

셰이더에서 셰이더 상수 버퍼를 읽으려면 HLSL 로드 기능을 사용합니다. 각 셰이더 단계는 최대 15개의 셰이더 상수 버퍼를 허용하며 각 버퍼는 최대 4,096개 상수를 보관할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[꼭짓점 및 인덱스 버퍼](vertex-and-index-buffers.md)

 

 





