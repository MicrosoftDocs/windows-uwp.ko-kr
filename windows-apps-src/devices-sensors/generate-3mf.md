---
Description: 3D 제조 형식 파일 형식의 구조와 Printing3D API를 사용 하 여이를 만들고 조작 하는 방법을 설명 합니다.
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 3MF 패키지 생성
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c24e3bcc6ce10fb85f9e87ac213172e147d9e6b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172277"
---
# <a name="generate-a-3mf-package"></a>3MF 패키지 생성

**중요 API**

-   [**Printing3D**](/uwp/api/windows.graphics.printing3d)

이 가이드에서는 3D 제조 형식 문서의 구조와 [**Printing3D**](/uwp/api/windows.graphics.printing3d) API를 사용 하 여이를 만들고 조작 하는 방법을 설명 합니다.

## <a name="what-is-3mf"></a>3MF 란?

3D 제조 형식은 제조 목적의 3D 모델의 모양과 구조를 설명 하기 위해 XML을 사용 하는 규칙 집합입니다 (3D 인쇄). 3D 제조 장치에 필요한 모든 정보를 제공 하는 것을 목표로 하는 파트 집합 (필수 및 일부 옵션) 및 해당 관계를 정의 합니다. 3D 제조 형식을 따르는 데이터 집합은 확장명이 3mf 인 파일로 저장할 수 있습니다.

Windows 10에서 **Printing3D** 네임 스페이스의 [**Printing3D3MFPackage**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage) 클래스는 단일. 3mf 파일과 비슷하며 다른 클래스는 파일의 특정 XML 요소에 매핑됩니다. 이 가이드는 3MF 문서의 각 주요 부분을 프로그래밍 방식으로 만들고 설정 하는 방법, 3MF 자료 확장을 활용 하는 방법, **Printing3D3MFPackage** 개체를. 3mf 파일로 변환 및 저장 하는 방법을 설명 합니다. 3MF 또는 3MF 재질 확장의 표준에 대 한 자세한 내용은 [3Mf 사양을](https://3mf.io/what-is-3mf/3mf-specification/)참조 하십시오.

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>3MF 구조의 핵심 클래스

**Printing3D3MFPackage** 클래스는 전체 3mf 문서를 나타내고 3mf 문서의 핵심은 [**Printing3DModel**](/uwp/api/windows.graphics.printing3d.printing3dmodel) 클래스에 의해 표현 되는 모델 부분입니다. 3D 모델에 대해 지정 하려는 대부분의 정보는 **Printing3DModel** 클래스의 속성과 기본 클래스의 속성을 설정 하 여 저장 됩니다.

[!code-cs[InitClasses](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetInitClasses)]

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>메타데이터

3MF 문서의 모델 **부분은 메타 데이터 속성에** 저장 된 문자열의 키/값 쌍 형식으로 메타 데이터를 보유할 수 있습니다. 메타 데이터의 미리 정의 된 이름은 여러 가지가 있지만 확장의 일부로 다른 쌍을 추가할 수 있습니다 ( [3Mf 사양](https://3mf.io/what-is-3mf/3mf-specification/)에 자세히 설명 되어 있음). 패키지 (3D 제조 장치)의 수신자가 메타 데이터를 처리할지 여부를 결정 하는 것은 아니지만 3MF 패키지에서 가능한 한 많은 기본 정보를 포함 하는 것이 좋습니다.

[!code-cs[Metadata](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMetadata)]

## <a name="mesh-data"></a>메시 데이터

이 가이드의 컨텍스트에서 메시는 단일 실선으로 나타날 필요가 없지만 단일 꼭 짓 점 집합에서 생성 된 3 차원 기 하 도형의 본문입니다. 메시 부분은 [**Printing3DMesh**](/uwp/api/windows.graphics.printing3d.printing3dmesh) 클래스로 나타냅니다. 유효한 메시 개체는 특정 꼭 짓 점 집합 사이에 존재 하는 모든 삼각형 면 뿐만 아니라 모든 꼭 짓 점의 위치에 대 한 정보를 포함 해야 합니다.

다음 메서드는 망상에 꼭 짓 점을 추가 하 고 3D 공간에서 해당 위치를 제공 합니다.

[!code-cs[Vertices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetVertices)]

다음 메서드는 이러한 꼭 짓 점에 걸쳐 그릴 모든 삼각형을 정의 합니다.

[!code-cs[TriangleIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTriangleIndices)]

> [!NOTE]
> 모든 삼각형의 인덱스는 시계 반대 방향으로 정의 되어야 합니다 (메시 개체 외부에서 삼각형을 볼 때).

Printing3DMesh 개체에 유효한 꼭지점과 삼각형 집합이 있으면 모델의 **메시** 속성에 추가 해야 합니다. 패키지의 모든 **Printing3DMesh** 개체는 **Printing3DModel** 클래스의 **메시** 속성 아래에 저장 되어야 합니다.

[!code-cs[MeshAdd](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMeshAdd)]


## <a name="create-materials"></a>자료 만들기


3D 모델은 여러 자료에 대 한 데이터를 보유할 수 있습니다. 이 규칙은 단일 인쇄 작업에서 여러 자료를 사용할 수 있는 3D 제조 장치를 활용 하기 위한 것입니다. 여러 가지 *유형의* 자재 그룹이 반환 있습니다. 각각은 다양 한 개별 materals를 지원할 수 있습니다. 각 재질 그룹에는 고유한 참조 id 번호가 있어야 하 고 해당 그룹 내의 각 자료에는 고유한 id가 있어야 합니다.

그러면 모델 내의 다른 메시 개체가 이러한 재료를 참조할 수 있습니다. 또한 각 망상의 개별 삼각형은 다른 자료를 지정할 수 있습니다. 또한 다른 자료를 단일 삼각형 내에 표시할 수도 있습니다. 각 삼각형 꼭 짓 점이 다른 재질을 할당 하 고 얼굴 재질을 그라데이션으로 계산 합니다.

이 가이드는 먼저 해당 재질 그룹 내에서 다양 한 종류의 자료를 만들고 모델 개체에 리소스로 저장 하는 방법을 보여 줍니다. 그런 다음 개별 메시와 개별 삼각형에 다른 자료를 할당 하는 방법을 알아봅니다.

### <a name="base-materials"></a>기본 자료

기본 재질 형식은 **색 재질** 값 (아래 설명)과 사용할 재질 *유형을* 지정 하기 위한 이름 특성을 모두 포함 하는 **기본 재질**입니다.

[!code-cs[BaseMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetBaseMaterialGroup)]

> [!NOTE]
> 3D 제조 장치는 3MF에 저장 된 가상 자재 요소에 대 한 사용 가능한 물리적 재질 지도를 결정 합니다. 재질 매핑은 1:1 일 필요가 없습니다. 3D 프린터가 하나의 재질만 사용 하는 경우 다른 자료에 할당 된 개체나 얼굴에 관계 없이 전체 모델을 해당 재질에 인쇄 합니다.

### <a name="color-materials"></a>색 재질

**색 자료** 는 **기본 자료**와 비슷하지만 이름을 포함 하지 않습니다. 따라서 컴퓨터에서 사용 해야 하는 자료의 유형에 대 한 지침을 제공 하지 않습니다. 색 데이터만 보유 하 고, 컴퓨터에서 재질 유형을 선택 하 게 하 고, 컴퓨터에서 사용자에 게 선택 하 라는 메시지를 표시할 수 있도록 합니다. 아래 코드에서는 `colrMat` 이전 메서드의 개체를 자체적으로 사용 합니다.

[!code-cs[ColorMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetColorMaterialGroup)]

### <a name="composite-materials"></a>복합 재질

**복합 자료** 는 다른 **기본 자료**를 균일 하 게 혼합 하 여 사용 하도록 제조 장치에 지시 합니다. 각 **복합 재질 그룹** 은 원료를 그릴 **기본 재질 그룹** 을 정확히 하나 참조 해야 합니다. 또한이 그룹 내에서 사용할 수 있는 **기본 자료** 는 **자료 인덱스** 목록에 나열 되어야 합니다 .이 목록에서 각 **복합 재질** 은 비율을 지정할 때 참조 됩니다. 모든 **복합 재질** 은 단순히 **기본 자료**의 비율입니다.

[!code-cs[CompositeMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetCompositeMaterialGroup)]

### <a name="texture-coordinate-materials"></a>질감 좌표 재질

3MF는 2D 이미지를 사용 하 여 3D 모델의 표면을 색으로 표시 하도록 지원 합니다. 이러한 방식으로 모델은 삼각형 면에서 색 값을 하나만 포함 하는 것이 아니라 삼각형 면 마다 훨씬 더 많은 색 데이터를 전달할 수 있습니다. **색 재질**처럼 질감 좌표 재질은 색 데이터만 효과적입니다. 2D 질감을 사용 하려면 먼저 질감 리소스를 선언 해야 합니다.

[!code-cs[TextureResource](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTextureResource)]

> [!NOTE]
> 텍스처 데이터는 패키지 내의 모델 부분이 아니라 3MF 패키지 자체에 속합니다.

다음으로 **Texture3Coord 자료**를 작성 해야 합니다. 이러한 각는 질감 리소스를 참조 하 고 이미지의 특정 점 (UV 좌표)을 지정 합니다.

[!code-cs[Texture2CoordMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTexture2CoordMaterialGroup)]

## <a name="map-materials-to-faces"></a>면에 재질 매핑

각 삼각형에서 꼭 짓 점에 매핑되는 재질을 지정 하기 위해 모델의 메시 개체에서 더 많은 작업을 수행 해야 합니다. 모델에 여러 메쉬가 포함 되어 있으면 각각의 자료를 개별적으로 할당 해야 합니다. 위에서 언급 했 듯이 자료는 꼭 짓 점 별, 삼각형 단위로 할당 됩니다. 이 정보를 입력 하 고 해석 하는 방법을 보려면 아래 코드를 참조 하세요.

[!code-cs[MaterialIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMaterialIndices)]

## <a name="components-and-build"></a>구성 요소 및 빌드

사용자는 구성 요소 구조를 사용 하 여 두 개 이상의 메시 개체를 인쇄 가능한 3D 모델에 둘 수 있습니다. [**Printing3DComponent**](/uwp/api/windows.graphics.printing3d.printing3dcomponent) 개체에는 단일 메시와 다른 구성 요소에 대 한 참조 목록이 포함 되어 있습니다. 이는 실제로 [**Printing3DComponentWithMatrix**](/uwp/api/windows.graphics.printing3d.printing3dcomponentwithmatrix) 개체의 목록입니다. **Printing3DComponentWithMatrix** 개체에는 각각 **Printing3DComponent** 및 **Printing3DComponent**의 포함 된 구성 요소에 적용 되는 변형 매트릭스가 포함 됩니다.

예를 들어 자동차의 모델은 자동차 본문의 메시를 보유 하는 "Body" **Printing3DComponent** 로 구성 될 수 있습니다. 그러면 "Body" 구성 요소에는 모두 동일한 Printing3DComponent ( **Printing3DComponentWithMatrix** )를 참조 하는 4 개의 다른 개체에 대 한 참조가 포함 될 수 있습니다 .이 개체는 모두 "휠" 메시를 사용 하 여 동일한 **Printing3DComponent** 를 참조 하 고 4 개의 서로 다른 변환 매트릭스를 포함 합니다 이 시나리오에서 최종 제품이 총 5 개의 메시를 기능 하는 경우에도 "Body" 메시 및 "휠" 메시는 각각 한 번만 저장 해야 합니다.

모든 **Printing3DComponent** 개체는 모델의 **구성 요소** 속성에서 직접 참조 해야 합니다. 인쇄 작업에 사용할 특정 구성 요소 하나는 **빌드** 속성에 저장 됩니다.

[!code-cs[Components](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetComponents)]

## <a name="save-package"></a>패키지 저장
이제 정의 된 자료 및 구성 요소가 포함 된 모델이 있으므로 패키지에 저장할 수 있습니다.

[!code-cs[SavePackage](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSavePackage)]

이 함수를 사용 하면 질감이 올바르게 지정 됩니다.

[!code-cs[FixTexture](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetFixTexture)]

여기에서 앱 내에서 인쇄 작업을 시작 하거나 ( [앱에서 3d 인쇄](./3d-print-from-app.md)참조)이 **Printing3D3MFPackage** 를. 3mf 파일로 저장할 수 있습니다.

다음 메서드는 완성 된 **Printing3D3MFPackage** 를 사용 하 여 해당 데이터를. 3mf 파일에 저장 합니다.

[!code-cs[SaveTo3mf](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSaveTo3mf)]

## <a name="related-topics"></a>관련 항목

[앱에서 3D 인쇄](./3d-print-from-app.md)  
[3D 인쇄 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 