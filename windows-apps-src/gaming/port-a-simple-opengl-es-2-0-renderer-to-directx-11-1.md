---
title: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 이식
description: 첫 번째 포팅 실습의 경우, Visual Studio 2015의 DirectX 11 앱 (유니버설 Windows) 템플릿과 일치 하도록 OpenGL ES 2.0에서 Direct3D로 회전 하는 꼭 짓 점 음영 큐브에 대 한 간단한 렌더러를 가져오는 기본 사항부터 시작 합니다.
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, opengl, direct3d 11, 포트
ms.localizationpriority: medium
ms.openlocfilehash: cdd5bc20d9cceff992cc23ae4863f952ea719877
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175217"
---
# <a name="port-a-simple-opengl-es-20-renderer-to-direct3d-11"></a>간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 이식



이 포팅 연습에서는 Visual Studio 2015의 DirectX 11 앱 (유니버설 Windows) 템플릿과 일치 하도록 OpenGL ES 2.0에서 Direct3D로 회전 하는 꼭 짓 점 음영 큐브에 대 한 간단한 렌더러를 가져오는 기본 사항부터 시작 합니다. 이 포트 프로세스를 진행할 때 다음에 대해 알아봅니다.

-   간단한 버텍스 버퍼 집합을 Direct3D 입력 버퍼로 이식 하는 방법
-   상수 버퍼에 대 한 각 형식 및 특성을 이식 하는 방법
-   Direct3D 셰이더 개체를 구성 하는 방법
-   Direct3D shader development에서 기본 HLSL 의미 체계를 사용 하는 방법
-   HLSL에 매우 단순한 인 글을 이식 하는 방법

이 항목은 새 DirectX 11 프로젝트를 만든 후에 시작 됩니다. 새 DirectX 11 프로젝트를 만드는 방법에 대 한 자세한 내용은 [유니버설 Windows 플랫폼에 대 한 새 directx 11 프로젝트 만들기 (UWP)](user-interface.md)를 참조 하세요.

이러한 링크 중 하나에서 만든 프로젝트에는 [direct3d](/windows/desktop/direct3d11/dx-graphics-overviews) infrastructure에 대 한 모든 코드가 준비 되어 있으며, 열린 GL ES 2.0에서 Direct3D 11로 렌더러를 이식 하는 프로세스를 즉시 시작할 수 있습니다.

이 항목에서는 동일한 기본 그래픽 태스크를 수행 하는 두 가지 코드 경로를 보여 줍니다. 창에 회전 하는 꼭 짓 점 음영 큐브를 표시 합니다. 두 경우 모두 코드는 다음 프로세스를 포함 합니다.

1.  하드 코드 된 데이터에서 큐브 메시 만들기 이 메시는 각 꼭 짓 점이 위치, 법선 벡터 및 색 벡터 처리 개체로 하는 꼭 짓 점 목록으로 표시 됩니다. 이 메시는 음영 파이프라인에서 처리 하기 위해 꼭 짓 점 버퍼에 배치 됩니다.
2.  큐브 메시를 처리 하는 셰이더 개체 만들기 두 가지 셰이더가 있습니다. 래스터화의 꼭 짓 점을 처리 하는 꼭 짓 점 셰이더 및 래스터화 후 큐브의 개별 픽셀을 색으로 하는 조각 (픽셀) 셰이더를 사용할 수 있습니다. 이러한 픽셀은 표시를 위해 렌더링 대상에 기록 됩니다.
3.  꼭 짓 점 및 조각 셰이더에서 꼭 짓 점 및 픽셀 처리에 사용 되는 음영 언어를 형성 합니다.
4.  화면에 렌더링된 큐브 표시

![단순 opengl 큐브](images/simple-opengl-cube.png)

이 연습을 완료 하면 Open GL ES 2.0 및 Direct3D 11의 기본 차이점에 대해 잘 알고 있어야 합니다.

-   꼭 짓 점 버퍼와 꼭 짓 점 데이터의 표현입니다.
-   셰이더를 만들고 구성 하는 프로세스입니다.
-   음영 언어, 셰이더 개체에 대 한 입력 및 출력입니다.
-   화면 그리기 동작입니다.

이 연습에서는 다음과 같이 정의 된 단순 하 고 일반적인 OpenGL 렌더러 구조를 참조 합니다.

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

이 구조에는 하나의 인스턴스가 있으며 매우 간단한 꼭 짓 점 음영 메시를 렌더링 하기 위한 모든 필수 구성 요소가 포함 되어 있습니다.

> **참고**    이 항목의 모든 OpenGL ES 2.0 코드는 Khronos 그룹에서 제공 하는 Windows API 구현을 기반으로 하며 Windows C 프로그래밍 구문을 사용 합니다.

 

## <a name="what-you-need-to-know"></a>기억해야 하는 사항


### <a name="technologies"></a>기술

-   [Microsoft Visual C++](/previous-versions/60k1461a(v=vs.140))
-   OpenGL ES 2.0

### <a name="prerequisites"></a>필수 조건

-   선택 사항입니다. [포트 코드 코드를 DXGI 및 Direct3D로](moving-from-egl-to-dxgi.md)검토 합니다. DirectX에서 제공 하는 그래픽 인터페이스를 더 잘 이해 하려면이 항목을 참조 하세요.

## <a name="span-idkeylinks_steps_headingspansteps"></a><span id="keylinks_steps_heading"></span>단계


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
<td align="left"><p><a href="port-the-shader-config.md">셰이더 개체 이식</a></p></td>
<td align="left"><p>OpenGL ES 2.0에서 단순 렌더러를 이식할 때 첫 번째 단계는 Direct3D 11에서 해당 하는 꼭 짓 점 및 조각 셰이더 개체를 설정 하 고, 컴파일된 후 주 프로그램이 셰이더 개체와 통신할 수 있는지 확인 하는 것입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-the-vertex-buffers-and-data-config.md">꼭 짓 점 버퍼 및 데이터를 이식 합니다.</a></p></td>
<td align="left"><p>이 단계에서는 셰이더를 포함 하는 꼭 짓 점 버퍼와 셰이더가 지정 된 순서로 꼭 짓 점을 트래버스할 수 있도록 하는 인덱스 버퍼를 정의 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="port-the-glsl.md">인 포트</a></p></td>
<td align="left"><p>버퍼 및 셰이더 개체를 만들고 구성 하는 코드를 이동한 후에는 OpenGL ES 2.0의 HLSL (High level Shader language)에서 해당 셰이더 안에 있는 코드를 해당 셰이더로 이식 해야 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="draw-to-the-screen.md">화면에 그리기</a></p></td>
<td align="left"><p>마지막으로 회전 하는 큐브를 그리는 코드를 화면에 이식 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idadditional_resourcesspanadditional-resources"></a><span id="additional_resources"></span>추가 자료


-   [UWP DirectX 게임 개발을 위한 개발 환경 준비](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [UWP 용 새 DirectX 11 프로젝트 만들기](user-interface.md)
-   [OpenGL ES 2.0 개념 및 인프라를 Direct3D 11에 매핑](map-concepts-and-infrastructure.md)

 

 