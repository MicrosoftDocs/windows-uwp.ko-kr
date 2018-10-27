---
author: mtoepke
title: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 포팅
description: 첫 번째 포팅 연습에 대한 기본 사항인 Visual Studio 2015에서 DirectX 11 앱(유니버설 Windows) 템플릿을 일치시키는 것과 같이, 회전하는 꼭짓점 음영 큐브에 대한 간단한 렌더러를 OpenGL ES 2.0에서 Direct3D로 가져오기에 먼저 살펴보겠습니다.
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, opengl, direct3d 11, 포트
ms.localizationpriority: medium
ms.openlocfilehash: e7541a8f54f64197c17acea5f1737e36b0e6f670
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5705556"
---
# <a name="port-a-simple-opengl-es-20-renderer-to-direct3d-11"></a>간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 포팅



이 포팅 연습에 대한 기본 사항인 Visual Studio 2015에서 DirectX 11 앱(유니버설 Windows) 템플릿을 일치시키는 것과 같이, 회전하는 꼭짓점 음영 큐브에 대한 간단한 렌더러를 OpenGL ES 2.0에서 Direct3D로 가져오기에 대해 먼저 살펴보겠습니다. 이 포팅 프로세스를 진행하면서 다음을 배우게 됩니다.

-   Direct3D 입력 버퍼로 꼭짓점 버퍼의 간단한 집합을 포팅하는 방법
-   유니폼 및 특성을 상수 버퍼로 포팅하는 방법
-   Direct3D 셰이더 개체를 구성하는 방법
-   Direct3D 셰이더 개발에 기본 HLSL 의미 체계를 사용하는 방법
-   매우 간단한 GLSL를 HLSL로 포팅하는 방법

새 DirectX 11 프로젝트를 만든 후 이 항목을 시작합니다. 새 DirectX 11 프로젝트를 만드는 방법을 알아보려면 [UWP(유니버설 Windows 플랫폼)용 새 DirectX 11 프로젝트 만들기](user-interface.md)를 참조하세요.

이러한 링크 중 하나에서 만든 프로젝트에는 준비된 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476345) 인프라에 대한 모든 코드가 있으며 Open GL ES 2.0에서 Direct3D 11로 렌더러를 포팅하는 프로세스를 즉시 시작할 수 있습니다.

이 항목에서는 동일한 기본 그래픽 작업(창에 회전하는 꼭짓점 음영 큐브 표시)을 수행하는 두 가지 코드 경로를 살펴봅니다. 두 경우 모두 코드에 다음 프로세스가 포함됩니다.

1.  하드 코드된 데이터에서 큐브 메시 만들기. 이 메시는 각 정점이 위치, 법선 벡터 및 색상 벡터를 처리하는 정점 목록으로 표시됩니다. 이 메시는 처리할 음영 파이프라인의 정점 버퍼에 추가됩니다.
2.  큐브 메시를 처리할 셰이더 개체 만들기. 셰이더에는 래스터화용으로 정점을 처리하는 정점 셰이더와, 래스터화 후에 큐브의 개별 픽셀을 착색하는 조각(픽셀) 셰이더가 있습니다. 이러한 픽셀은 디스플레이용 렌더러 대상에 작성됩니다.
3.  각각의 정점 셰이더와 조각 셰이더에서 정점 및 픽셀 처리에 사용되는 음영 언어 형성
4.  화면에 렌더링된 큐브 표시

![간단한 OpenGL 큐브](images/simple-opengl-cube.png)

이 연습 완료 시, 다음과 같은 Open GL ES 2.0 및 Direct3D 11 사이의 기본적인 차이를 잘 알게 될 것입니다.

-   꼭짓점 버퍼 및 꼭짓점 데이터의 표현.
-   셰이더 만들기 및 구성의 프로세스.
-   음영 언어 및 셰이더 개체에 대한 입력과 출력.
-   화면의 그리기 동작.

이 연습에서는 다음과 같이 정의된 간단하고 일반적인 OpenGL 렌더러 구조를 참조합니다.

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

이 구조에는 하나의 인스턴스가 있으며 매우 간단한 꼭짓점 음영 메시 렌더링에 대한 모든 필요한 구성 요소가 포함되어 있습니다.

> **참고**이 항목의 모든 OpenGL ES 2.0 코드는 Khronos Group에서 제공 하는 Windows API 구현을 기반으로 하며 Windows C 프로그래밍 구문을 사용 합니다.

 

## <a name="what-you-need-to-know"></a>알아야 할 사항


### <a name="technologies"></a>기술

-   [Microsoft Visual C++](http://msdn.microsoft.com/library/vstudio/60k1461a.aspx)
-   OpenGL ES 2.0

### <a name="prerequisites"></a>필수 조건

-   옵션. [DXGI 및 Direct3D로 EGL 코드 포팅](moving-from-egl-to-dxgi.md)을 검토하세요. DirectX에서 제공하는 그래픽 인터페이스를 더 잘 이해할 수 있도록 이 항목을 확인하세요.

## <a name="span-idkeylinksstepsheadingspansteps"></a><span id="keylinks_steps_heading"></span>단계


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
<td align="left"><p><a href="port-the-shader-config.md">셰이더 개체 포팅</a></p></td>
<td align="left"><p>OpenGL ES 2.0에서 간단한 렌더러를 포팅하는 경우 첫 번째 단계는 Direct3D 11에서 해당하는 꼭짓점 및 조각 셰이더 개체를 설정하고 주 프로그램이 셰이더 개체가 컴파일된 후 이 셰이더 개체와 통신할 수 있는지 확인하는 것입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-the-vertex-buffers-and-data-config.md">꼭짓점 버퍼 및 데이터 포팅</a></p></td>
<td align="left"><p>이 단계에서는 메시를 포함하는 꼭짓점 버퍼 및 셰이더가 꼭짓점을 지정된 순서로 트래버스할 수 있게 하는 인덱스 버퍼를 정의합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="port-the-glsl.md">GLSL 포팅</a></p></td>
<td align="left"><p>버퍼 및 셰이더 개체를 만들고 구성하는 코드를 이동한 후에는 해당 셰이더 내에서 코드를 OpenGL ES 2.0 GLSL(GL Shader Language)에서 Direct3D 11 HLSL(High Level Shader Language)로 포팅할 차례입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="draw-to-the-screen.md">화면에 그리기</a></p></td>
<td align="left"><p>마지막으로, 화면에 회전하는 큐브를 그리는 코드를 포팅합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idadditionalresourcesspanadditional-resources"></a><span id="additional_resources"></span>추가 리소스


-   [UWP DirectX 게임 개발을 위한 개발 환경 준비](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [UWP용 새 DirectX 11 프로젝트 만들기](user-interface.md)
-   [Direct3D 11에 OpenGL ES 2.0 개념 및 인프라 매핑](map-concepts-and-infrastructure.md)

 

 




