---
author: PatrickFarley
Description: "3D 제조 형식 파일 유형의 구조 및 Windows.Graphics.Printing3D API를 사용하여 이 구조를 만들고 조작하는 방법을 설명합니다."
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "3MF 패키지 생성"
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2b1f15534e1388bfd61ba09faeb590464e44d8fd
ms.lasthandoff: 02/07/2017

---

# <a name="generate-a-3mf-package"></a>3MF 패키지 생성


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx)

\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

이 가이드에서는 3D 제조 형식 문서의 구조와 [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx) API를 사용하여 이 구조를 만들고 조작하는 방법을 설명합니다.

## <a name="what-is-3mf"></a>3MF란?

3D 제조 형식은 제조(3D 인쇄)를 위한 용도로 XML을 사용하여 3D 모델의 모양과 구조를 설명하는 규칙 집합입니다. 3D 제조 디바이스에 필요한 모든 정보를 제공하려는 목표로 부분 집합(일부는 필수이고 일부는 선택임)과 해당하는 관계를 정의합니다. 3D 제조 형식을 준수하는 데이터 집합은 .3mf 확장명의 파일로 저장할 수 있습니다.

Windows 10에서 **Windows.Graphics.Printing3D** 네임스페이스의 [**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.aspx) 클래스는 단일 .3mf 파일과 유사하며 다른 클래스를 해당 파일의 특정 XML 요소에 매핑합니다. 이 가이드에서는 3MF 문서의 주요 부분을 각각 만들고 프로그래밍 방식으로 설정하는 방법과 3MF 재료 확장을 활용하는 방법 그리고 **Printing3D3MFPackage** 개체를 변환하고 .3mf 파일로 저장할 수 있는 방법을 설명합니다. 3MF 또는 3MF 재료 확장의 표준에 대한 자세한 내용은 [3MF 사양](http://3mf.io/what-is-3mf/3mf-specification/)을 참조하세요.

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>3MF 구조의 핵심 클래스

**Printing3D3MFPackage** 클래스는 전체 3MF 문서를 나타내며 3MF 문서의 핵심에 [**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmodel.aspx) 클래스로 표시되는 모델 부분이 있습니다. 3D 모델에 대해 지정하려는 대부분의 정보는 **Printing3DModel** 클래스의 속성과 해당하는 기본 클래스의 속성을 설정하여 저장합니다.

[!code-cs[InitClasses](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetInitClasses)]

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>메타데이터

3MF 문서의 모델 부분은 **메타데이터** 속성에 저장된 문자열의 키/값 쌍의 형식으로 메타데이터를 저장할 수 있습니다. 미리 정의된 메타데이터의 이름이 많이 있지만 다른 쌍은 일부 확장으로 추가할 수 있습니다([3MF 사양](http://3mf.io/what-is-3mf/3mf-specification/)에 자세히 설명되어 있음). 패키지 수신기(3D 제조 디바이스)가 메타데이터를 처리할지 여부 및 처리하는 방법을 결정하지만 3MF 패키지에 가능한 한 많은 기본 정보를 포함하는 것이 좋습니다.

[!code-cs[메타데이터](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMetadata)]

## <a name="mesh-data"></a>메시 데이터

이 가이드의 컨텍스트에서 메시는 단일 꼭짓점 집합에서 생성된 3D 기하 도형의 본체입니다(단일 입체로 표시할 필요 없음). 메시 부분은 [**Printing3DMesh**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmesh.aspx) 클래스에 의해 표시됩니다. 유효한 메시 개체에는 모든 꼭짓점과 특정 꼭짓점의 집합 사이에 있는 삼각형의 모든 면의 위치에 대한 정보가 포함되어야 합니다.

다음 메서드는 메시에 꼭짓점을 추가한 다음 3D 공간에서 위치를 지정합니다.

[!code-cs[Vertices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetVertices)]

다음 메서드는 이러한 꼭짓점에서 그려질 모든 삼각형을 정의합니다.

[!code-cs[TriangleIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTriangleIndices)]

> [!NOTE]
> 모든 삼각형은 면-법선 벡터가 바깥쪽을 가리키도록 인덱스가 시계 반대 방향의 순서(삼각형을 메시 개체의 외부에서 볼 때)로 정의되어야 합니다.

Printing3DMesh 개체가 유효한 꼭짓점 및 삼각형의 집합을 포함하는 경우 모델의 **메시** 속성에 추가되어야 합니다. 패키지의 모든 **Printing3DMesh** 개체는 **Printing3DModel** 클래스의 **메시** 속성 아래에 저장되어야 합니다.

[!code-cs[MeshAdd](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMeshAdd)]


## <a name="create-materials"></a>재료 만들기


3D 모델은 여러 재료에 대한 데이터를 저장할 수 있습니다. 이 규칙은 단일 인쇄 작업에서 여러 재료를 사용할 수 있는 3D 제조 디바이스를 활용하기 위한 것입니다. 또한 여러 *유형*의 재료 그룹이 있으며 각 유형은 다양한 다른 개별 재료를 지원할 수 있습니다. 각 재료 그룹은 고유한 참조 ID 번호가 있어야 하고 해당 그룹 내의 각 재료 또한 고유 ID가 있어야 합니다.

그러면 모델 내의 다른 메시 개체가 이러한 재료를 참조할 수 있습니다. 또한 각 메시의 개별 삼각형이 다른 재료를 지정할 수 있습니다. 더 나아가 삼각형의 각 꼭짓점에 다른 재료가 할당되고 면 재료는 이들 사이의 그라데이션으로 계산되어 단일 삼각형 내에서 다른 재료가 나타날 수도 있습니다.

이 가이드는 먼저 해당 각 재료 그룹 내에서 다양한 종류의 재료를 만들고 모델 개체에서 리소스로 저장하는 방법을 보여 줍니다. 그런 다음 개별 메시 및 개별 삼각형에 다른 재료를 할당하기 시작합니다.

### <a name="base-materials"></a>기본 재료

기본 재료 유형은 **기본 재료**이며, **색 재료** 값(아래에서 설명)과 사용할 재료의 *유형*을 지정하기 위한 이름 특성이 모두 있습니다.

[!code-cs[BaseMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetBaseMaterialGroup)]

> [!NOTE]
> 3D 제조 디바이스에서 어떤 사용 가능한 실제 재료를 3MF에 저장되어 있는 어떤 가상 재료 요소에 매핑할지를 결정합니다. 재료 매핑은 1:1일 필요는 없습니다. 3D 프린터가 하나의 재료만 사용하는 경우 해당 프린터는 다른 재료가 할당된 개체 또는 면에 상관없이 해당 재료의 전체 모델을 인쇄합니다.

### <a name="color-materials"></a>색 재료

**색 재료**는 **기본 재료**와 비슷하지만 이름이 포함되어 있지 않습니다. 따라서 컴퓨터에서 사용해야 하는 재료의 유형에 대해 지침을 제공하지 않습니다. 색 데이터만 저장하고 컴퓨터가 재료 유형을 선택하도록 합니다. 그러면 컴퓨터는 사용자에게 선택하라는 메시지를 표시할 수 있습니다. 아래 코드에서는 이전 메서드의 `colrMat` 개체가 단독으로 사용됩니다.

[!code-cs[ColorMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetColorMaterialGroup)]

### <a name="composite-materials"></a>복합 재료

**복합 재료**는 제조 디바이스가 일관되게 혼합된 다양한 **기본 재료**를 사용하도록 간단히 지시합니다. 각 **복합 재료 그룹**은 재료를 끌어올 하나의 **기본 재료 그룹**을 정확히 참조해야 합니다. 또한 사용할 수 있게 될 이 그룹 내의 **기본 재료**는 **재료 인덱스** 목록에 나열되어야 하며 각 **복합 재료**가 비율을 지정할 때 이를 참조하게 됩니다(모든 **복합 재료**는 단순히 **기본 재료**의 비율임).

[!code-cs[CompositeMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetCompositeMaterialGroup)]

### <a name="texture-coordinate-materials"></a>텍스처 좌표 재료

3MF는 3D 모델의 표면의 색을 지정하기 위해 2D 이미지 사용을 지원합니다. 이렇게 하면 모델이 삼각형 면당 더 많은 색 데이터를 전달할 수 있습니다(삼각형 꼭짓점당 하나의 색 값만 있는 것과 반대임). **색 재료**와 마찬가지로 텍스처 좌표 재료는 색 데이터를 전달하기만 합니다. 2D 텍스처를 사용하려면 먼저 텍스처 리소스를 선언해야 합니다.

[!code-cs[TextureResource](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTextureResource)]

> [!NOTE]
> 텍스처 데이터는 패키지 내에서 모델 부분이 아닌 3MF 패키지 자체에 속합니다.

다음으로 **Texture3Coord 재료**를 채워야 합니다. 이러한 재료의 각각은 텍스처 리소스를 참조하고 UV 좌표의 이미지에서 특정 지점을 지정합니다.

[!code-cs[Texture2CoordMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTexture2CoordMaterialGroup)]

## <a name="map-materials-to-faces"></a>재료를 면에 매핑

각 삼각형에서 어떤 재료를 어떤 꼭짓점에 매핑할지 받아쓰기 위해서 모델의 메시 개체에서 일부 더 많은 작업을 수행해야 합니다(모델에 여러 메시가 포함되어 있으면 각각 재료가 별도로 할당되어야 함). 위에서 설명한 대로 재료는 꼭짓점당, 삼각형당 할당됩니다. 이 정보를 입력하고 해석하는 방법을 확인하려면 다음 코드를 참조하세요.

[!code-cs[MaterialIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMaterialIndices)]

## <a name="components-and-build"></a>구성 요소 및 빌드

구성 요소 구조를 통해 인쇄 가능한 3D 모델에서 둘 이상의 메시 개체를 배치할 수 있습니다. [**Printing3DComponent**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponent.aspx) 개체에는 단일 메시 및 다른 구성 요소에 대한 참조 목록이 포함되어 있습니다. 이는 실제로 [**Printing3DComponentWithMatrix**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponentwithmatrix.aspx) 개체의 목록입니다. 각 **Printing3DComponentWithMatrix** 개체에는 **Printing3DComponent**가 포함되며 중요하게는 메시 및 언급한 **Printing3DComponent**의 포함된 구성 요소에 적용되는 변형 매트릭스가 포함됩니다.

예를 들어 자동차의 모델은 해당 자동차의 본체용 메시를 보유하고 있는 "본체" **Printing3DComponent**로 구성될 수 있습니다. "본체" 구성 요소는 4개의 서로 다른 **Printing3DComponentWithMatrix** 개체에 대한 참조를 포함할 수 있으며 모두 "바퀴" 메시와 동일한 **Printing3DComponent**를 참조하고 4개의 다양한 변형 매트릭스를 포함합니다(자동차 본체에서 바퀴를 4개의 서로 다른 위치에 매핑함). 이 시나리오에서 최종 제품은 총 5개의 메시를 특징으로 하지만 "본체" 메시와 "바퀴" 메시는 각각 한 번만 저장하면 됩니다.

모든 **Printing3DComponent** 개체는 모델의 **Components** 속성에서 직접 참조해야 합니다. 인쇄 작업에서 사용할 한 가지 특별한 구성 요소는 **Build** 속성에 저장됩니다.

[!code-cs[구성 요소](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetComponents)]

## <a name="save-package"></a>패키지 저장
이제 정의된 재료 및 구성 요소와 함께 보유한 모델을 패키지에 저장할 수 있습니다.

[!code-cs[SavePackage](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSavePackage)]

여기에서 인쇄 작업을 앱 내에서 시작하거나([앱에서 3D 인쇄](https://msdn.microsoft.com/library/windows/apps/mt204541.aspx) 참조) 이 **Printing3D3MFPackage**를 .3mf 파일로 저장할 수 있습니다.

다음 메서드는 완료된 **Printing3D3MFPackage**를 사용하고 .3mf 파일에 데이터를 저장합니다.

[!code-cs[SaveTo3mf](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSaveTo3mf)]

## <a name="related-topics"></a>관련 항목

[앱에서 3D 인쇄](https://msdn.microsoft.com/windows/uwp/devices-sensors/3d-print-from-app)  
[3D 인쇄 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 

