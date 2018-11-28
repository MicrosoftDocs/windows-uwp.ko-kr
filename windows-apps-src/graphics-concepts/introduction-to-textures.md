---
title: 텍스처 소개
description: 텍스처 리소스는 텍셀을 저장할 수 있는 데이터 구조이며, 여기에서 텍셀이란 읽거나 쓸 수 있는 가장 작은 단위의 텍스처를 의미합니다. 텍스처가 셰이더에서 사용할 준비가 되면 텍스처 샘플러에서 필터링할 수 있습니다.
ms.assetid: 6F3C76A8-F762-4296-AE02-BFBD6476A5A8
keywords:
- 텍스처 소개
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3cd5ca66635b57b79c2fca3e6ff10b8debb43fd0
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7842474"
---
# <a name="introduction-to-textures"></a>텍스처 소개


텍스처 리소스는 텍셀을 저장할 수 있는 데이터 구조이며, 여기에서 텍셀이란 읽거나 쓸 수 있는 가장 작은 단위의 텍스처를 의미합니다. 텍스처가 셰이더에서 사용할 준비가 되면 텍스처 샘플러에서 필터링할 수 있습니다.

텍스처 리소스는 텍셀을 저장하도록 구조화된 데이터 모음입니다. 텍셀은 파이프라인에서 읽거나 쓸 수 있는 텍스처의 최소 단위를 나타냅니다. 버퍼와 달리 텍스처는 셰이더 단위에서 읽으므로 텍스처 샘플러에서 필터링할 수 있습니다. 텍스처의 유형은 텍스처의 필터링 방식에 영향을 미칩니다. DXGI\_FORMAT 열거형에 정의된 DXGI 형식 중 하나로 정렬된 구성 요소가 텍셀 하나당 1 ~ 4개 포함됩니다.

텍스처는 알려진 크기의 구조화된 리소스로 생성됩니다. 그러나 텍스처가 파이프라인에 바인딩될 때 보기를 사용해 유형이 완전히 지정되면 리소스가 만들어질 때 각 텍스처에 유형이 있거나 없을 수 있습니다.

## <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span>텍스처 유형


Direct3D는 여러 부동 소수점 표현을 지원합니다. 모든 부동 소수점 계산은 IEEE 754 32비트 단정밀도 부동 소수점 규칙의 정의된 하위 집합에서 작동합니다.

1D, 2D, 3D 등 여러 유형의 텍스처가 있으며, 각각 Mipmap을 사용하거나 사용하지 않고 만들 수 있습니다. Direct3D는 텍스처 배열과 다중 샘플링된 텍스처도 지원합니다.

-   [1D 텍스처](#texture1d-resource)
-   [1D 텍스처 배열](#texture1d-array-resource)
-   [2D 텍스처와 2D 텍스처 배열](#texture2d-resource)
-   [3D 텍스처](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-textures"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D 텍스처

가장 단순한 형태의 1D 텍스처에는 단일 텍스처 좌표로 주소를 지정할 수 있는 텍스처 데이터가 포함됩니다. 이는 다음 그림과 같이 텍셀 배열로 시각화할 수 있습니다.

다음 그림은 1D 텍스처를 보여줍니다.

![1D 텍스처](images/d3d10-1d-texture.png)

저장되는 데이터 형식에 따라 텍셀 하나당 수많은 색상 구성 요소가 포함됩니다. 다음 그림과 같이 Mipmap 수준으로 1D 텍스처를 생성할 수 있다는 점에서 더욱 복잡합니다.

![Mipmap 수준 1D 텍스처](images/d3d10-resource-texture1d.png)

Mipmap 수준은 바로 위 수준보다 2의 거듭 제곱만큼 작은 텍스처입니다. 맨 위 수준에 가장 많은 정보가 담겨 있고 한 수준 낮아질 때마다 정보 양이 줄어듭니다. 1D Mipmap에서 최소 수준에는 1텍셀이 포함됩니다. 게다가 MIP 수준은 항상 1:1로 줄어듭니다.

홀수 크기의 텍스처에 Mipmap이 생성되면 바로 아래 수준은 항상 짝수 크기(최저 수준이 1에 도달한 경우는 예외)입니다. 예를 들어 다음 그림의 5x1 텍스처를 보면, 바로 아래 수준은 2x1 텍스처이고 그 다음(마지막) mip 수준은 1x1 크기의 텍스처입니다. 이 수준은 카메라에서 가깝지 않은 기하 도형을 렌더링할 때 더 작은 텍스처에 액세스하는 데 사용되는 LOD(level-of-detail)라는 인덱스로 식별됩니다.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-arrays"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D 텍스처 배열

Direct3D는 텍스처 배열도 지원합니다. 이론적으로 1D 텍스처의 배열은 다음 그림과 같이 보입니다.

![1D 텍스처의 배열](images/d3d10-resource-texture1darray.png)

이 텍스처 배열에는 세 개의 텍스처가 포함되어 있습니다. 이 세 개 텍스처의 폭은 각각 5(첫 번째 계층의 요소 개수)입니다. 각 텍스처에는 3 계층 Mipmap도 포함되어 있습니다.

Direct3D의 모든 텍스처 배열은 같은 유형의 텍스처 배열입니다. 즉 텍스처 배열의 모든 텍스처는 동일한 데이터 형식과 크기(텍스처 폭과 Mipmap 수준 개수 포함)로 구성되어야 합니다. 각 배열에 속한 모든 텍스처의 크기가 일치하면 서로 다른 크기의 텍스처 배열을 만들 수 있습니다.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-textures-and-2d-texture-arrays"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D 텍스처와 2D 텍스처 배열

Texture2D 리소스에는 텍셀의 2D 그리드가 포함되어 있습니다. 각 텍셀은 a u, v 벡터로 주소를 지정할 수 있습니다. 이는 텍스처 리소스이므로 Mipmap 수준과 하위 리소스를 포함할 수 있습니다. 완전히 채워진 2D 텍스처 리소스는 다음 그림처럼 보입니다.

![2D 텍스처 리소스](images/d3d10-resource-texture2d.png)

이 텍스처 리소스에는 3 Mipmap 수준의 3x5 텍스처가 하나 포함되어 있습니다.

2D 텍스처 배열 리소스는 동일한 유형의 2D 텍스처 배열입니다. 즉 각 텍스처의 데이터 형식과 차원(Mipmap 수준 포함)이 동일합니다. 다음 그림과 같이 이제 텍스처에 2D 데이터가 포함된다는 점을 제외하면 1D 텍스처 배열의 레이아웃과 비슷합니다.

![2D 텍스처의 배열](images/d3d10-resource-texture2darray.png)

이 텍스처 배열에는 세 개의 텍스처가 포함되며 각 텍스처는 2 Mipmap 수준의 3x5입니다.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-2d-texture-array-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>2D 텍스처 배열을 텍스처 큐브로 사용

텍스처 큐브는 각각 큐브의 한 면에 해당하는 6개 텍스처를 포함한 2D 텍스처 배열입니다. 완전히 채워진 텍스처 큐브는 다음 그림처럼 보입니다.

![텍스처 큐브를 나타내는 2D 텍스처 배열](images/d3d10-resource-texturecube.png)

6개 텍스처를 포함하는 2D 텍스처 배열은 큐브-텍스처 보기로 파이프라인에 바인딩된 후 큐브 맵 기본 기능이 있는 셰이더 내에서 읽을 수 있습니다. 셰이더에서 텍스처 큐브는 텍스처 큐브의 중앙에서부터 시작되는 3D 벡터로 주소가 지정됩니다.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-textures"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D 텍스처

볼륨 텍스처라고도 하는 3D 텍스처 리소스에는 텍셀의 3D 볼륨이 포함되어 있습니다. 텍스처 리소스이기 때문에 Mipmap 수준이 포함될 수 있습니다. 완전히 채워진 3D 텍스처는 다음 그림처럼 보입니다.

![3d 텍스처 리소스](images/d3d10-resource-texture3d.png)

3D 텍스처 Mipmap 슬라이스가 렌더링 대상 출력으로 바인딩되면(렌더링 대상 보기 이용) 3D 텍스처가 n개 슬라이스가 있는 2D 텍스처 배열과 동일하게 동작합니다. 특정한 렌더링 슬라이스가 기하 도형 셰이더 단계에서 선택됩니다.

3D 텍스처 배열의 개념이 없기 때문에 3D 텍스처 하위 리소스는 단일 Mipmap 수준입니다.

Direct3D의 좌표계는 픽셀과 텍셀에 대해 정의됩니다.

## <a name="span-idpixelspanspan-idpixelspanspan-idpixelspanpixel-coordinate-system"></a><span id="Pixel"></span><span id="pixel"></span><span id="PIXEL"></span>픽셀 좌표계


Direct3D의 픽셀 좌표계는 다음 그림과 같이 왼쪽 위 모서리에 렌더링 대상의 원점을 정의합니다. 픽셀 중앙은 정수 위치에서 (0.5f,0.5f)만큼 오프셋됩니다.

![direct3d 10의 픽셀 좌표계 그림](images/d3d10-coordspix10.png)

## <a name="span-idtexelspanspan-idtexelspanspan-idtexelspantexel-coordinate-system"></a><span id="Texel"></span><span id="texel"></span><span id="TEXEL"></span>텍셀 좌표계


텍셀 좌표계의 경우, 다음 그림과 같이 텍스처의 왼쪽 위 모서리에 원점이 있습니다. 따라서 픽셀 좌표계가 텍셀 좌표계에 맞춰 정렬되기 때문에 화면 배열 텍스처의 렌더링이 아주 쉬워집니다.

![텍셀 좌표계의 그림](images/d3d10-coordstex10.png)

텍스처 좌표는 정규화되거나 배율이 적용된 숫자로 표현됩니다. 각 텍스처 좌표는 다음과 같이 특정 텍셀에 매핑됩니다.

정규화된 좌표의 경우:

-   점 샘플링: 텍셀 \# = floor(U \* 폭)
-   선형 샘플링: 왼쪽 텍셀 \# = floor(U \* 폭), 오른쪽 텍셀 \# = 왼쪽 텍셀 \# + 1

배율이 적용된 좌표:

-   점 샘플링: 텍셀 \# = floor(U)
-   선형 샘플링: 왼쪽 텍셀 \# = floor(U - 0.5), 오른쪽 텍셀 \# = 왼쪽 텍셀 \# + 1

여기서 폭은 텍스처의 폭(텍셀 단위)입니다.

텍스처 주소 래핑은 텍셀 위치가 계산된 후 이루어집니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처](textures.md)
