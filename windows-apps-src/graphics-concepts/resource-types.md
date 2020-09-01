---
title: 리소스 종류
description: 서로 다른 유형의 리소스에는 고유한 레이아웃 (또는 메모리 공간)이 있습니다.
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- 리소스 종류
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b300747027f0c9466e7ca04b4c4b571882f57a6e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156437"
---
# <a name="resource-types"></a>리소스 종류


서로 다른 유형의 리소스에는 고유한 레이아웃 (또는 메모리 공간)이 있습니다. Direct3D 파이프라인에서 사용 하는 모든 리소스는 두 가지 기본 리소스 유형인 [버퍼](#buffer-resources) 및 [질감](#texture-resources)에서 파생 됩니다. 버퍼는 원시 데이터 (요소)의 컬렉션입니다. 질감은 텍셀 (질감 요소)의 컬렉션입니다.

리소스의 레이아웃 (또는 메모리 공간)을 완전히 지정 하는 방법에는 다음 두 가지가 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>직접</p></td>
<td align="left"><p>리소스를 만들 때 형식을 완전히 지정 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>형식 없는</p></td>
<td align="left"><p>리소스가 파이프라인에 바인딩될 때 형식을 완전히 지정 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>버퍼 리소스


버퍼 리소스는 완전히 형식화 된 데이터의 컬렉션입니다. 내부적으로 버퍼에는 요소가 포함 됩니다. 요소는 1 ~ 4 개의 구성 요소로 구성 됩니다. 요소 데이터 형식의 예로는 압축 된 데이터 값 (예: R8G8B8A8), 8 비트 정수 4 32 비트 float 값이 있습니다. 이러한 데이터 형식은 위치 벡터, 일반 벡터, 꼭 짓 점 버퍼의 질감 좌표, 인덱스 버퍼의 인덱스 또는 장치 상태와 같은 데이터를 저장 하는 데 사용 됩니다.

버퍼는 비구조적 리소스로 생성 됩니다. 구조화 되지 않으므로 버퍼는 밉 맵 수준을 포함할 수 없으며 읽을 때 필터링 되지 않으며 다중 샘플 수 없습니다.

### <a name="span-idbuffer_typesspanspan-idbuffer_typesspanspan-idbuffer_typesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>버퍼 유형

-   [꼭 짓 점 버퍼](#vertex-buffer)
-   [인덱스 버퍼](#index-buffer)
-   [상수 버퍼](#shader-constant-buffer)

### <a name="span-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>꼭 짓 점 버퍼

버퍼는 요소의 컬렉션입니다. 꼭 짓 점 버퍼는 꼭 짓 점 별 데이터를 포함 합니다. 가장 간단한 예는 위치 데이터와 같은 하나의 데이터 형식을 포함 하는 꼭 짓 점 버퍼입니다. 다음 그림과 같이 시각화할 수 있습니다.

![위치 데이터를 포함 하는 꼭 짓 점 버퍼의 그림](images/d3d10-resources-single-element-vb2.png)

더 자주, 꼭 짓 점 버퍼에는 3D 꼭 짓 점을 완전히 지정 하는 데 필요한 모든 데이터가 포함 됩니다. 이에 대 한 예는 꼭 짓 점 별 위치, 법선 및 질감 좌표를 포함 하는 꼭 짓 점 버퍼 일 수 있습니다. 이 데이터는 일반적으로 다음 그림에 표시 된 것 처럼 꼭 짓 점 별 요소 집합으로 구성 됩니다.

![position, normal 및 질감 데이터를 포함 하는 꼭 짓 점 버퍼의 그림](images/d3d10-vertex-buffer-element.png)

이 꼭 짓 점 버퍼는 8 개의 꼭 짓 점에 대 한 꼭 짓 점 데이터를 포함 합니다. 각 꼭 짓 점에는 세 개의 요소 (위치, 보통 및 질감 좌표)가 저장 됩니다. Position 및 normal은 일반적으로 3 32 비트 float를 사용 하 여 지정 하 고, 질감 좌표는 2 32 비트 float를 사용 하 여 지정 합니다.

꼭 짓 점 버퍼의 데이터에 액세스 하려면 액세스할 꼭 짓 점 및 기타 버퍼 매개 변수를 알고 있어야 합니다.

-   *Offset* -버퍼의 시작부터 첫 번째 꼭 짓 점에 대 한 데이터 까지의 바이트 수입니다.
-   *BaseVertexLocation* -오프셋에서 적절 한 그리기 호출에 사용 되는 첫 번째 꼭 짓 점 까지의 바이트 수입니다.

꼭 짓 점 버퍼를 만들기 전에 입력 레이아웃 개체를 만들어 해당 레이아웃을 정의 해야 합니다. 입력 레이아웃 개체를 만든 후에는 입력 어셈블러 (IA) 단계에 바인딩합니다.

### <a name="span-idindex_bufferspanspan-idindex_bufferspanspan-idindex_bufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>인덱스 버퍼

인덱스 버퍼에는 16 비트 또는 32 비트 인덱스의 순차 집합이 포함 되어 있습니다. 각 인덱스는 꼭 짓 점 버퍼에서 꼭 짓 점을 식별 하는 데 사용 됩니다. 하나 이상의 꼭 짓 점 버퍼와 함께 인덱스 버퍼를 사용 하 여 IA 단계에 데이터를 제공 하는 것을 인덱싱 이라고 합니다. 다음 그림과 같이 인덱스 버퍼를 시각화할 수 있습니다.

![인덱스 버퍼의 그림](images/d3d10-index-buffer.png)

인덱스 버퍼에 저장 된 순차 인덱스에는 다음 매개 변수가 있습니다.

-   *Offset* -버퍼의 시작부터 첫 번째 인덱스 까지의 바이트 수입니다.
-   *Startindexlocation* -오프셋에서 적절 한 그리기 호출에 사용 되는 첫 번째 꼭 짓 점 까지의 바이트 수입니다.
-   *Indexcount* -렌더링할 인덱스 수입니다.

인덱스 버퍼는 여러 줄 또는 삼각형 스트립 ([기본 토폴로지](primitive-topologies.md))을 스트립 잘림 인덱스와 구분 하 여 연결할 수 있습니다. 줄무늬 잘림 인덱스를 사용 하면 단일 그리기 호출로 여러 줄 또는 삼각형 스트립을 그릴 수 있습니다. 줄무늬 인덱스는 인덱스에 사용할 수 있는 최대 값입니다 (16 비트 인덱스의 경우 0xffff, 32 비트 인덱스의 경우 0xffffffff). 줄무늬-잘라내기 인덱스는 인덱싱된 기본 형식으로 굴곡 순서를 다시 설정 하며, 삼각형 스트립에서 적절 한 굴곡 순서를 유지 하기 위해 필요할 수 있는 중복 제거 삼각형의 필요성을 제거 하는 데 사용할 수 있습니다. 다음 그림에서는 줄무늬 인덱스의 예를 보여 줍니다.

![줄무늬 인덱스 그림](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>상수 버퍼

Direct3D에는 셰이더 상수 버퍼 또는 단순한 상수 버퍼 라는 셰이더 상수를 제공 하기 위한 버퍼가 있습니다. 개념적으로 다음 그림과 같이 단일 요소 꼭 짓 점 버퍼 처럼 보입니다.

![셰이더 상수 버퍼의 그림](images/d3d10-shader-resource-buffer.png)

각 요소는 저장 된 데이터의 형식에 따라 결정 되는 1-4 구성 요소 상수를 저장 합니다.

상수 버퍼는 각 상수를 별도로 커밋하는 개별 호출을 수행 하지 않고 셰이더 상수를 함께 그룹화 하 고 동시에 커밋할 수 있도록 하 여 셰이더 상수를 업데이트 하는 데 필요한 대역폭을 줄입니다.

셰이더는 상수 버퍼의 일부가 아닌 변수를 읽을 때와 동일한 방식으로 상수 버퍼의 변수를 변수 이름으로 직접 계속 읽습니다.

각 셰이더 단계는 최대 15 개의 셰이더 상수 버퍼를 허용 합니다. 각 버퍼는 최대 4096 개의 상수를 보유할 수 있습니다.

상수 버퍼를 사용 하 여 스트림 출력 단계의 결과를 저장 합니다.

셰이더에서 상수 버퍼를 선언 하는 예제는 [셰이더 상수 (DIRECTX HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) 를 참조 하세요.

## <a name="span-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>질감 리소스


질감 리소스는 텍셀을 저장 하도록 디자인 된 데이터의 구조화 된 컬렉션입니다. 버퍼와 달리 텍스처는 셰이더 단위에서 읽을 때 질감 샘플러를 기준으로 필터링 할 수 있습니다. 질감의 형식은 질감이 필터링 되는 방식에 영향을 줍니다. 텍셀은 파이프라인에서 읽거나 쓸 수 있는 질감의 가장 작은 단위를 나타냅니다. 각 텍셀는 DXGI 형식 중 하나로 정렬 된 1 ~ 4 개의 구성 요소를 포함 합니다 ( [**dxgi \_ 형식**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)참조).

질감이 구조화 된 리소스로 만들어지므로 크기를 알 수 있습니다. 그러나 질감이 파이프라인에 바인딩될 때 뷰를 사용 하 여 형식이 완전히 지정 된 경우에는 리소스를 만들 때 각 질감을 입력 하거나 형식이 지정 되지 않을 수 있습니다.

-   [텍스처 형식](#texture-types)
-   [하위 리소스](#subresources)
-   [강력 하 고 약한 형식 비교](#typed)

### <a name="span-idtexture_typesspanspan-idtexture_typesspanspan-idtexture_typesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>질감 형식

여러 종류의 질감 (1D, 2D, 3D)이 있습니다. 각 질감은 mip 맵을를 사용 하거나 사용 하지 않고 만들 수 있습니다. Direct3D는 질감 배열 및 다중 샘플 질감도 지원 합니다.

-   [1D 질감](#texture1d-resource)
-   [1D 질감 배열](#texture1d-array-resource)
-   [2D 질감 및 2D 질감 배열](#texture2d-resource)
-   [3D 질감](#texture3d-resource)

### <a name="span-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D 질감

가장 간단한 형식의 1D 질감에는 단일 질감 좌표로 처리할 수 있는 질감 데이터가 포함 됩니다. 다음 그림에 표시 된 것 처럼 텍셀의 배열로 시각화할 수 있습니다.

![1d 질감 그림](images/d3d10-1d-texture.png)

각 텍셀는 저장 된 데이터의 형식에 따라 다양 한 색 구성 요소를 포함 합니다. 복잡성을 추가 하면 다음 그림과 같이 밉 맵 수준으로 1D 질감을 만들 수 있습니다.

![밉 맵 수준의 1d 질감 그림](images/d3d10-resource-texture1d.png)

밉 맵 수준은 상위 수준 보다 작은 2의 거듭제곱이 되는 질감입니다. 최상위 수준에는 가장 자세한 내용이 포함 되며, 각 후속 수준은 더 작습니다. 1D 밉 맵의 경우 가장 작은 수준은 하나의 텍셀를 포함 합니다. 서로 다른 수준은 LOD 이라는 인덱스 (세부 수준)로 식별 됩니다. 카메라에 가까이 있지 않은 기 하 도형을 렌더링할 때 LOD를 사용 하 여 더 작은 질감에 액세스할 수 있습니다.

### <a name="span-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D 질감 배열

Direct3D 10에는 질감 배열에 대 한 새로운 데이터 구조도 있습니다. 1D 질감의 배열은 개념적으로 다음 그림과 같습니다.

![1d 질감 배열의 그림](images/d3d10-resource-texture1darray.png)

이 질감 배열은 3 개의 질감을 포함 합니다. 세 질감 각각에는 질감 너비 5 (첫 번째 계층의 요소 수)가 있습니다. 각 질감에는 3 계층 밉 맵도 포함 됩니다.

Direct3D의 모든 질감 배열은 유형이 같은 질감 배열입니다. 즉, 질감 배열의 모든 질감이 동일한 데이터 형식 및 크기 (질감 너비 및 밉 맵 수준 수 포함)를 가져야 합니다. 각 배열의 모든 질감이 크기와 일치 하는 한 여러 크기의 질감 배열을 만들 수 있습니다.

### <a name="span-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D 질감 및 2D 질감 배열

Texture2D 리소스는 텍셀의 2D 그리드를 포함 합니다. 각 텍셀는 u, v vector로 주소를 지정할 수 있습니다. 질감 리소스 이므로 밉 맵 수준과 하위 리소스를 포함할 수 있습니다. 완전히 채워진 2D 텍스처 리소스는 다음 그림과 같습니다.

![2d 질감 리소스의 그림](images/d3d10-resource-texture2d.png)

이 질감 리소스에는 세 가지 밉 맵 수준을 포함 하는 단일 3x5 질감이 포함 됩니다.

Texture2DArray 리소스는 2D 질감의 같은 배열입니다. 즉, 각 질감에 동일한 데이터 형식 및 차원 (밉 맵 수준 포함)이 있습니다. 이제 질감이 2D 데이터를 포함 하는 것을 제외 하 고는 1D 텍스처 배열과 비슷한 레이아웃을 가지 므로 다음 그림과 같습니다.

![2d 질감 리소스의 배열 그림](images/d3d10-resource-texture2darray.png)

이 질감 배열에는 세 가지 질감이 있습니다. 각 질감은 두 개의 밉 맵 수준으로 3x5 됩니다.

### <a name="span-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Texture2DArray을 질감 큐브로 사용

질감 큐브는 큐브의 각 면에 하나씩 6 개의 질감이 포함 된 2D 질감 배열입니다. 완전히 채워진 질감 큐브는 다음 그림과 같습니다.

![질감 큐브를 나타내는 2d 질감 리소스의 배열 그림](images/d3d10-resource-texturecube.png)

6 개의 질감이 포함 된 2D 질감 배열은 큐브 맵 내장 함수를 사용 하 여 셰이더 내에서 읽을 수 있습니다 .이는 큐브-질감 뷰를 사용 하 여 파이프라인에 바인딩한 후에 있습니다. 질감 큐브는 질감 큐브의 중심에서 바깥쪽을 가리키는 3D 벡터를 사용 하 여 셰이더에 주소를 지정 합니다.

### <a name="span-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D 질감

Texture3D 리소스 (볼륨 질감이 라고도 함)에는 텍셀의 3D 볼륨이 포함 되어 있습니다. 질감 리소스 이므로 밉 맵 수준을 포함할 수 있습니다. 완전히 채워진 3D 질감은 다음 그림과 같습니다.

![3d 질감 리소스의 그림](images/d3d10-resource-texture3d.png)

3D 질감 밉 맵 조각이 렌더링 대상 출력으로 바인딩되는 경우 (렌더링 대상 뷰를 사용 하 여) 3D 질감이 n 개의 조각이 있는 2D 텍스처 배열과 동일 하 게 동작 합니다. 특정 렌더링 조각이 기 하 도형 셰이더 단계에서 선택 됩니다.

3D 질감 배열의 개념은 없습니다. 따라서 3D 질감 하위 리소스는 단일 밉 맵 수준입니다.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>하위

Direct3D API는 전체 리소스 또는 리소스의 하위 집합을 참조 합니다. 리소스의 일부를 지정 하기 위해 Direct3D는 리소스의 하위 집합을 의미 하는 하위 *리소스*라는 용어를 연결 설명은 합니다.

버퍼는 단일 하위 리소스로 정의 됩니다. 질감은 몇 가지 다른 질감 형식 (1D, 2D 등)이 있으므로 약간 더 복잡 합니다. 일부는 밉 맵 수준 및/또는 질감 배열을 지원 합니다. 가장 간단한 경우부터 다음 그림과 같이 1D 질감이 단일 하위 리소스로 정의 됩니다.

![1d 질감 그림](images/d3d10-1d-texture.png)

즉, 1D 질감을 구성 하는 텍셀의 배열이 단일 하위 리소스에 포함 됩니다.

3 개의 밉 맵 수준으로 1D 질감을 확장 하는 경우 다음과 같이 시각화할 수 있습니다.

![밉 맵 수준의 1d 질감 그림](images/d3d10-resource-texture1d.png)

이는 세 개의 하위 질감으로 구성 된 단일 질감으로 생각 하면 됩니다. 각 subtexture 하위 리소스로 계산 되므로이 1D 질감에는 3 개의 하위 리소스가 포함 되어 있습니다. 단일 질감에 대 한 세부 수준 (LOD)을 사용 하 여 subtexture 나 subtexture를 인덱싱할 수 있습니다. 질감의 배열을 사용 하는 경우 특정 하위 질감에 액세스 하려면 LOD 및 특정 질감이 모두 필요 합니다. 또는이 API는 여기에 표시 된 것 처럼 이러한 두 가지 정보를 단일 0부터 시작 하는 하위 리소스 인덱스로 결합 합니다.

![0부터 시작 하는 하위 리소스 인덱스의 그림](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselecting_subresourcesspanspan-idselecting_subresourcesspanspan-idselecting_subresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>하위 리소스 선택

일부 Api는 전체 리소스에 액세스 하 고, 다른 Api는 리소스의 일부에 액세스 합니다. 리소스의 특정 부분에 액세스 하는 Api는 일반적으로 보기 설명을 사용 하 여 액세스할 하위 리소스를 지정 합니다.

다음 그림에서는 질감 배열에 액세스할 때 보기 설명에서 사용 되는 용어를 보여 줍니다.

### <a name="span-idarray_slicespanspan-idarray_slicespanspan-idarray_slicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>배열 조각

다음 그림에 표시 된 것 처럼 mip 맵을를 사용 하는 각 질감, 배열 조각 (흰색 사각형으로 표시)에는 질감 배열이 지정 되 고, 다음 그림에 표시 된 것 처럼 하나의 질감 및 모든 하위 질감이 포함 됩니다.

![배열 스플라이스 그림](images/d3d10-resource-array-slice.png)

### <a name="span-idmip_slicespanspan-idmip_slicespanspan-idmip_slicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>밉 조각

다음 그림에 표시 된 것 처럼, 흰색 사각형으로 표시 되는 밉 조각에는 배열의 모든 질감에 대 한 하나의 밉 맵 수준이 포함 됩니다.

![밉 스플라이스 그림](images/d3d10-resource-mip-slice.png)

### <a name="span-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>단일 하위 리소스 선택

다음 그림에 표시 된 것 처럼 이러한 두 가지 유형의 조각을 사용 하 여 단일 subresource를 선택할 수 있습니다.

![배열 조각 및 밉 스플라이스를 사용 하 여 하위 리소스를 선택 하는 방법에 대 한 그림](images/d3d10-resource-subresources-1.png)

### <a name="span-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>여러 Subresources 선택

또는 여러 하위 리소스를 선택 하기 위해 밉 맵 수준 및/또는 질감 수와 함께 이러한 두 가지 유형의 조각을 사용할 수 있습니다.

![여러 하위 선택 그림](images/d3d10-resource-subresources-2.png)

텍스처 배열 유무에 관계 없이 사용 하는 텍스처 형식에 관계 없이 mip 맵을를 사용 하거나 사용 하지 않고 특정 하위 리소스의 인덱스를 계산 하기 위해 제공 하는 도우미 함수가 종종 있습니다.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>강력 하 고 약한 형식 비교

완전히 형식화 된 리소스를 만들면 리소스가 생성 된 형식으로 제한 됩니다. 이렇게 하면 런타임이 응용 프로그램에서 매핑할 수 없음을 나타내는 플래그를 사용 하 여 리소스를 만드는 경우에 특히 런타임이 액세스를 최적화할 수 있습니다. 특정 유형을 사용 하 여 만든 리소스는 뷰 메커니즘을 사용 하 여 다시 해석 수 없습니다.

관대 한 리소스에서 리소스를 처음 만들 때 데이터 형식을 알 수 없습니다. 응용 프로그램은 사용 가능한 형식 없는 형식에서 선택 해야 합니다. 할당할 메모리 크기와 런타임이 밉 맵 내에서 하위 질감을 생성 해야 하는지 여부를 지정 해야 합니다.

그러나 데이터가 정수, 부동 소수점 값, 부호 없는 정수 등으로 해석 되는지 여부와 관계 없이 정확한 데이터 형식이 뷰가 포함 된 파이프라인에 바인딩될 때까지 결정 되지 않습니다. 질감이 파이프라인에 바인딩될 때까지 질감 형식이 유연 하 게 유지 되므로 리소스를 약하게 형식화 된 저장소 라고 합니다. 약하게 형식화 된 저장소는 새 형식의 구성 요소 비트가 이전 형식의 비트 수와 일치 하는 한 다시 사용 하거나 다시 해석 수 있다는 장점이 있습니다.

각 위치에서 형식을 완전히 정규화 하는 고유한 뷰가 있는 한 단일 리소스를 여러 파이프라인 단계에 바인딩할 수 있습니다. 예를 들어, 형식이 아닌 형식을 사용 하 여 만든 리소스는 파이프라인의 여러 위치에서 동시에 FLOAT 형식 및 UINT 형식으로 사용할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[리소스](resources.md)