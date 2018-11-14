---
title: 리소스 종류
description: 다양한 리소스 종류는 고유한 레이아웃(또는 메모리 공간)을 가지고 있습니다.
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- 리소스 종류
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c23cc07c84e9a77b36c812c6f273f518e98838e
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6445532"
---
# <a name="resource-types"></a>리소스 종류


다양한 리소스 종류는 고유한 레이아웃(또는 메모리 공간)을 가지고 있습니다. Direct3D 파이프라인이 사용하는 모든 리소스는 [버퍼](#buffer-resources)와 [텍스처](#texture-resources)라는 두 가지 기본 리소스 종류에서 파생됩니다. 버퍼는 원시 데이터(요소)의 모음이며, 텍스처는 텍셀(텍스처 요소)의 모음입니다.

리소스의 레이아웃(또는 메모리 공간)을 완전히 지정할 수 있는 방법은 다음 두 가지입니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>형식 있음</p></td>
<td align="left"><p>리소스를 만들 때 형식을 완전히 지정합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>형식 없음</p></td>
<td align="left"><p>리소스가 파이프라인에 바인딩될 때 형식을 완전히 지정합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbufferresourcesspanspan-idbufferresourcesspanspan-idbufferresourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>버퍼 리소스


버퍼 리소스는 완전하게 형식이 지정된 데이터의 모음입니다. 내부적으로 버퍼에는 요소가 포함됩니다. 요소는 1-4개의 구성 요소로 이루어집니다. 요소 데이터 형식의 예: 압축된 데이터 값(예: R8G8B8A8), 단일 8비트 정수, 4개의 32비트 부동 값. 이러한 데이터 형식은 위치 벡터, 일반 벡터, 꼭짓점 버퍼 내 텍스처 좌표, 인덱스 버퍼 내 인덱스 또는 장치 상태 같은 데이터를 저장하는 데 사용됩니다.

버퍼는 구조화되지 않은 리소스로 생성됩니다. 구조화되지 않았기 때문에 버퍼는 Mipmap 수준을 포함할 수 없고, 읽었을 때 필터링되지 않으며, 다중 샘플링될 수 없습니다.

### <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>버퍼 유형

-   [꼭짓점 버퍼](#vertex-buffer)
-   [인덱스 버퍼](#index-buffer)
-   [상수 버퍼](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>꼭짓점 버퍼

버퍼는 요소의 모음입니다. 꼭짓점 버퍼에는 꼭짓점별 데이터가 포함됩니다. 가장 단순한 예는 위치 데이터 등 한 가지 형식의 데이터를 포함하는 꼭짓점 버퍼입니다. 다음 그림과 같이 표현할 수 있습니다.

![위치 데이터를 포함한 꼭짓점 버퍼의 그림](images/d3d10-resources-single-element-vb2.png)

3D 꼭짓점을 완벽하게 지정하는 데 필요한 모든 데이터를 포함하는 경우가 더 많습니다. 꼭짓점별 위치, 일반 및 텍스처 좌표가 포함된 꼭짓점 버퍼가 한 예입니다. 이 데이터는 일반적으로 다음 그림과 같이 꼭짓점별 요소 집합으로 구성됩니다.

![위치, 일반 및 텍스처 데이터가 포함된 꼭짓점 버퍼의 그림](images/d3d10-vertex-buffer-element.png)

이 꼭짓점 버퍼에는 8개 꼭짓점의 꼭짓점별 데이터가 포함되며, 각각의 꼭짓점은 3개의 요소(위치, 일반 및 텍스처 좌표)를 저장합니다. 위치 및 일반 좌표는 일반적으로 32비트 부동 소수점 3개를 사용하여 지정되고 텍스처 좌표는 32비트 부동 소수점 2개를 사용하여 지정됩니다.

꼭짓점 버퍼에서 데이터에 액세스하려면 액세스할 꼭짓점과 다음과 같은 그 밖의 버퍼 매개 변수를 알아야 합니다.

-   *오프셋* - 버퍼 시작부터 첫 번째 꼭짓점의 데이터까지 바이트 수입니다.
-   *BaseVertexLocation* - 오프셋부터 해당 그리기 호출에 사용된 첫 번째 꼭짓점까지 바이트 수입니다.

꼭짓점 버퍼를 만들기 전에 입력 레이아웃 개체를 만들어 레이아웃을 정의해야 합니다. 입력 레이아웃 개체를 만든 다음 IA(입력 어셈블러) 단계에 바인딩합니다.

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>인덱스 버퍼

인덱스 버퍼는 16비트 또는 32비트 인덱스의 순차 집합을 포함하며, 각 인덱스는 꼭짓점 버퍼에서 꼭짓점을 식별하는 데 사용됩니다. 인덱스 버퍼와 하나 이상의 꼭짓점 버퍼를 사용하여 IA 단계에 데이터를 제공하는 것을 인덱싱이라고 합니다. 인덱스 버퍼는 다음 그림과 같이 시각화할 수 있습니다.

![인덱스 버퍼의 그림](images/d3d10-index-buffer.png)

인덱스 버퍼에 저장된 순차 인덱스는 다음 매개 변수로 배치됩니다.

-   *오프셋* - 버퍼 시작부터 첫 번째 인덱스까지 바이트 수입니다.
-   *StartIndexLocation* - 오프셋부터 해당 그리기 호출에 사용된 첫 번째 꼭짓점까지 바이트 수입니다.
-   *IndexCount* - 렌더링할 인덱스 수입니다.

인덱스 버퍼는 스트립 잘라내기 인덱스로 각각을 분할하여 여러 개의 선 또는 삼각형 스트립([기본 토폴로지](primitive-topologies.md))을 이어붙일 수 있습니다. 스트립 잘라내기 인덱스를 사용하여 여러 개의 선 또는 삼각형 스트립을 단일 그리기 호출로 그릴 수 있습니다. 스트립 잘라내기 인덱스는 인덱스에 가능한 최대 값입니다(16비트 인덱스는 0xffff, 32비트 인덱스는 0xffffffff). 스트립 잘라내기 인덱스는 인덱싱된 기본 요소에서 두르기 순서를 재설정하며, 재설정하지 않을 경우 삼각형 스트립에서 올바른 두르기 순서를 유지하는 데 필요한 중복 제거 삼각형의 필요성을 제거하는 데 사용될 수 있습니다. 다음 그림은 스트립 잘라내기 인덱스의 예를 보여 줍니다.

![스트립 잘라내기 인덱스 그림](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>상수 버퍼

Direct3D에는 셰이더 상수 버퍼 또는 간단히 상수 버퍼라고 하는 셰이더 상수를 제공하기 위한 버퍼가 있습니다. 이론상 이 버퍼는 다음 그림에서 보듯 단일 요소 꼭짓점 버퍼처럼 보입니다.

![셰이더 상수 버퍼의 그림](images/d3d10-shader-resource-buffer.png)

각 요소는 저장된 데이터의 형식에 의해 결정되는 1~4개 구성 요소 상수를 저장합니다.

상수 버퍼는 각각의 셰이더 상수를 개별적으로 호출해 개별적으로 커밋하지 않고 그룹화하여 동시에 커밋할 수 있도록 함으로써 셰이더 상수 업데이트에 필요한 대역폭을 줄입니다.

셰이더는 상수 버퍼의 일부가 아닌 변수를 읽는 것과 동일한 방식으로 변수 이름을 기준으로 직접 상수 버퍼 내의 변수를 계속 읽습니다.

각 셰이더 단계는 최대 15개의 셰이더 상수 버퍼를 허용하며 각 버퍼는 최대 4,096개 상수를 보관할 수 있습니다.

상수 버퍼를 사용해 스트림 출력 단계의 결과를 저장합니다.

셰이더에서 상수 버퍼를 선언하는 예는 [셰이더 상수(DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)를 참조하세요.

## <a name="span-idtextureresourcesspanspan-idtextureresourcesspanspan-idtextureresourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>텍스처 리소스


텍스처 리소스는 텍셀을 저장하도록 구조화된 데이터 모음입니다. 버퍼와 달리 텍스처는 셰이더 단위에서 읽으므로 텍스처 샘플러에서 필터링할 수 있습니다. 텍스처의 유형은 텍스처의 필터링 방식에 영향을 미칩니다. 텍셀은 파이프라인에서 읽거나 쓸 수 있는 텍스처의 최소 단위를 나타냅니다. 각 텍셀에는 DXGI 형식 중 하나로 배열된 1~4개의 구성 요소가 포함됩니다([**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059) 참조).

텍스처는 크기를 알 수 있도록 구조화된 리소스로 생성됩니다. 그러나 텍스처가 파이프라인에 바인딩될 때 보기를 사용해 형식이 완전히 지정되면 리소스 생성 시간에 각 텍스처에 형식이 있거나 없을 수 있습니다.

-   [텍스처 형식](#texture-types)
-   [하위 리소스](#subresources)
-   [강력한 형식 지정 대 약한 형식 지정](#typed)

### <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>텍스처 형식

1D, 2D, 3D 등 여러 유형의 텍스처가 있으며, 각각 Mipmap을 사용하거나 사용하지 않고 만들 수 있습니다. Direct3D는 텍스처 배열과 다중 샘플링된 텍스처도 지원합니다.

-   [1D 텍스처](#texture1d-resource)
-   [1D 텍스처 배열](#texture1d-array-resource)
-   [2D 텍스처 및 2D 텍스처 배열](#texture2d-resource)
-   [3D 텍스처](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D 텍스처

가장 단순한 형태의 1D 텍스처에는 단일 텍스처 좌표로 주소를 지정할 수 있는 텍스처 데이터가 포함됩니다. 이는 다음 그림과 같이 텍셀 배열로 시각화할 수 있습니다.

![1D 텍스처 그림](images/d3d10-1d-texture.png)

저장되는 데이터 형식에 따라 각각의 텍셀에 수많은 색상 구성 요소가 포함됩니다. 다음 그림과 같이 Mipmap 수준이 포함된 1D 텍스처를 생성할 수 있다는 점에서 더욱 복잡합니다.

![Mipmap 수준이 포함된 1D 텍스처 그림](images/d3d10-resource-texture1d.png)

Mipmap 수준은 바로 위 수준보다 2의 거듭 제곱만큼 작은 텍스처입니다. 최상위 수준에는 가장 많은 세부가 포함되며, 이후의 각 수준은 그보다 작습니다. 1D Mipmap의 경우, 가장 작은 수준은 하나의 텍셀을 포함합니다. 서로 다른 수준은 LOD(level-of-detail)라고 하는 인덱스로 식별됩니다. 카메라에 가깝지 않은 기하 도형을 렌더링할 때 LOD를 사용하여 보다 작은 텍스처에 액세스할 수 있습니다.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D 텍스처 배열

Direct3D 10에도 텍스처 배열을 위한 새로운 데이터 구조가 있습니다. 이론적으로 1D 텍스처의 배열은 다음 그림과 같이 보입니다.

![1D 텍스처 배열 그림](images/d3d10-resource-texture1darray.png)

이 텍스처 배열에는 3개의 텍스처가 포함되어 있습니다. 이 3개 텍스처의 폭은 각각 5(첫 번째 계층의 요소 개수)입니다. 각 텍스처에는 3 계층 Mipmap도 포함되어 있습니다.

Direct3D의 모든 텍스처 배열은 같은 유형의 텍스처 배열입니다. 즉 텍스처 배열의 모든 텍스처는 동일한 데이터 형식과 크기(텍스처 폭과 Mipmap 수준 개수 포함)로 구성되어야 합니다. 각 배열에 속한 모든 텍스처의 크기가 일치하면 서로 다른 크기의 텍스처 배열을 만들 수 있습니다.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D 텍스처 및 2D 텍스처 배열

Texture2D 리소스에는 텍셀의 2D 그리드가 포함되어 있습니다. 각 텍셀은 a u, v 벡터로 주소를 지정할 수 있습니다. 이는 텍스처 리소스이므로 Mipmap 수준과 하위 리소스를 포함할 수 있습니다. 완전히 채워진 2D 텍스처 리소스는 다음 그림처럼 보입니다.

![2D 텍스처 리소스 그림](images/d3d10-resource-texture2d.png)

이 텍스처 리소스에는 3 Mipmap 수준의 3x5 텍스처가 하나 포함되어 있습니다.

Texture2DArray 리소스는 동일한 유형의 2D 텍스처 배열입니다. 즉, 각 텍스처의 데이터 형식과 차원(Mipmap 수준 포함)이 동일합니다. 이제 텍스처에 2D 데이터가 포함된다는 점만 제외하면 1D 텍스처 배열과 레이아웃이 비슷하므로 다음 그림처럼 보입니다.

![2D 텍스처 리소스 배열 그림](images/d3d10-resource-texture2darray.png)

이 텍스처 배열에는 세 개의 텍스처가 포함되며 각 텍스처는 2 Mipmap 수준의 3x5입니다.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Texture2DArray를 텍스처 큐브로 사용

텍스처 큐브는 각각 큐브의 한 면에 해당하는 6개 텍스처를 포함한 2D 텍스처 배열입니다. 완전히 채워진 텍스처 큐브는 다음 그림처럼 보입니다.

![텍스처 큐브를 나타내는 2D 텍스처 리소스 배열의 그림](images/d3d10-resource-texturecube.png)

6개 텍스처를 포함하는 2D 텍스처 배열은 큐브-텍스처 보기로 파이프라인에 바인딩된 후 큐브 맵 기본 기능이 있는 셰이더 내에서 읽을 수 있습니다. 셰이더에서 텍스처 큐브는 텍스처 큐브의 중앙에서부터 시작되는 3D 벡터로 주소가 지정됩니다.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D 텍스처

볼륨 텍스처라고도 하는 Texture3D 리소스에는 텍셀의 3D 볼륨이 포함되어 있습니다. 텍스처 리소스이기 때문에 Mipmap 수준이 포함될 수 있습니다. 완전히 채워진 3D 텍스처는 다음 그림처럼 보입니다.

![3D 텍스처 리소스 그림](images/d3d10-resource-texture3d.png)

3D 텍스처 Mipmap 슬라이스가 렌더링 대상 출력으로 바인딩되면(렌더링 대상 뷰 이용) 3D 텍스처가 n개 슬라이스가 있는 2D 텍스처 배열과 동일하게 동작합니다. 특정한 렌더링 슬라이스가 기하 도형 셰이더 단계에서 선택됩니다.

3D 텍스처 배열의 개념이 없기 때문에 3D 텍스처 하위 리소스는 단일 Mipmap 수준입니다.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>하위 리소스

Direct3D API는 전체 리소스 또는 리소스의 하위 집합을 참조합니다. 리소스 일부를 지정하기 위해 Direct3D는 리소스의 하위 집합을 뜻하는 *하위 리소스*라는 용어를 만들었습니다.

버퍼는 단일 하위 리소스로 정의됩니다. 텍스처는 조금 더 복잡한데, 몇 가지 텍스처 유형(1D, 2D 등)이 있고 그중 일부는 Mipmap 수준 및/또는 텍스처 배열을 지원하기 때문입니다. 가장 간단한 경우부터 시작하면 1D 텍스처는 다음 그림에서 보듯 단일 하위 리소스로 정의됩니다.

![1D 텍스처 그림](images/d3d10-1d-texture.png)

이것은 1D 텍스처를 구성하는 텍셀의 배열이 단일 하위 리소스에 포함되어 있음을 뜻합니다.

3개의 Mipmap 수준이 포함된 1D 텍스처를 확장하면 다음과 같이 시각화할 수 있습니다.

![Mipmap 수준이 포함된 1D 텍스처 그림](images/d3d10-resource-texture1d.png)

이것을 3개의 하위 텍스처로 구성된 단일 텍스처로 생각해 보세요. 각 하위 텍스처는 하나의 하위 리소스로 계산되므로 이 1D 텍스처에는 3개의 하위 리소스가 포함됩니다. 하위 텍스처(또는 하위 리소스)는 단일 텍스처에 대한 LOD(level-of-detail)를 사용하여 인덱싱할 수 있습니다. 텍스처 배열을 사용할 때 특정 하위 텍스처에 액세스하려면 LOD와 특정 텍스처가 모두 필요합니다. 또는 API는 여기서 보듯 이 2개의 정보 조각을 0부터 시작하는 단일한 하위 리소스로 결합합니다.

![0부터 시작하는 하위 리소스 인덱스 그림](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselectingsubresourcesspanspan-idselectingsubresourcesspanspan-idselectingsubresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>하위 리소스 선택

전체 리소스에 액세스하는 API도 있고 리소스 일부에 액세스하는 API도 있습니다. 리소스 일부에 액세스하는 API는 일반적으로 보기 설명을 사용하여 액세스할 하위 리소스를 지정합니다.

이 그림은 텍스처 배열에 액세스할 때 보기 설명이 사용하는 용어를 보여 줍니다.

### <a name="span-idarrayslicespanspan-idarrayslicespanspan-idarrayslicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>배열 슬라이스

다음 그림에서 보듯 주어진 텍스처 배열에서 Mipmap이 포함된 각 텍스처, 배열 슬라이스(흰색 사각형으로 표시)에는 하나의 텍스처와 그 모든 하위 텍스처가 포함됩니다.

![배열 슬라이스 그림](images/d3d10-resource-array-slice.png)

### <a name="span-idmipslicespanspan-idmipslicespanspan-idmipslicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Mip 슬라이스

다음 그림에서 보듯 Mip 슬라이스(흰색 사각형으로 표시)에는 배열 내 모든 텍스처마다 하나의 Mipmap 수준이 포함됩니다.

![Mip 스플라이스 그림](images/d3d10-resource-mip-slice.png)

### <a name="span-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>단일 하위 리소스 선택

다음 그림에서 보듯 이 두 가지 유형의 슬라이스를 사용하여 단일 하위 리소스를 선택할 수 있습니다.

![배열 슬라이스와 Mip 슬라이스를 사용한 하위 리소스 선택 그림](images/d3d10-resource-subresources-1.png)

### <a name="span-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>여러 하위 리소스 선택

또는 이 두 가지 유형의 슬라이스와 Mipmap 수준의 수 및/또는 텍스처 수를 사용하여 여러 개의 하위 리소스를 선택할 수 있습니다.

![여러 하위 리소스 선택 그림](images/d3d10-resource-subresources-2.png)

사용 중인 텍스처 형식에 상관없이(Mipmap 유무, 텍스처 배열 유무 불문) 특정 하위 리소스의 인덱스를 계산하도록 제공된 도우미 기능이 있는 경우가 많습니다.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>강력한 형식 지정 대 약한 형식 지정

완전히 형식이 지정된 리소스를 만들면 리소스는 생성된 형식으로 제한됩니다. 이로써 특히 응용 프로그램에 매핑될 수 없음을 나타내는 플래그를 사용하여 리소스가 생성된 경우, 런타임이 액세스를 최적화할 수 있습니다. 특정 형식을 사용하여 생성된 리소스는 보기 메커니즘을 사용하여 다시 해석할 수 없습니다.

형식이 없는 리소스에서는 리소스가 처음 만들어질 때 데이터 형식을 알 수 없습니다. 응용 프로그램은 사용 가능한 형식 없는 형식 중에서 선택해야 합니다. 할당할 메모리 크기 및 런타임이 Mipmap에서 하위 텍스처를 생성해야 하는지 여부를 지정해야 합니다.

하지만 정확한 데이터 형식(메모리가 정수, 부동 소수값, 부호 없는 정수 등으로 해석되는지 여부)은 보기를 사용하여 리소스가 파이프라인에 바인딩될 때까지 결정되지 않습니다. 텍스쳐가 파이프라인에 바인딩될 때까지 텍스처 형식이 유동적이므로 해당 리소스를 약한 형식의 저장소라고 합니다. 약한 형식의 저장소는 새 형식의 구성 요소 비트가 예전 형식의 비트 개수와 일치하는 한 다시 사용하거나 다시 해석할 수 있다는(다른 형식으로) 장점이 있습니다.

각 리소스에 각 위치에서 형식을 완전히 한정하는 고유의 보기가 있으면 단일 리소스를 여러 파이프라인 단계에 바인딩할 수 있습니다. 예를 들어 형식 없는 형식으로 만들어진 리소스를 파이프라인의 서로 다른 위치에서 동시에 FLOAT 형식과 UNIT 형식으로 사용할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[리소스](resources.md)
