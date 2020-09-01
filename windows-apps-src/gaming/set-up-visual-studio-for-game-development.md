---
title: Visual Studio tools for game 프로그래밍
description: 이미지 편집기, 모델 편집기 및 셰이더 디자이너를 비롯 하 여 Visual Studio에서 사용할 수 있는 DirectX 게임 프로그래밍에 대 한 도구에 대해 알아봅니다.
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, visual studio, 도구, directx
ms.localizationpriority: medium
ms.openlocfilehash: 250450b2174ce249d1ec5afaf4c5188df9266f5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159267"
---
# <a name="visual-studio-tools-for-game-programming"></a>Visual Studio tools for game 프로그래밍



**요약**

-   [템플릿에서 DirectX 게임 프로젝트 만들기](user-interface.md)
-   Visual Studio tools for DirectX 게임 프로그래밍


Visual Studio Ultimate를 사용 하 여 DirectX 앱을 개발 하는 경우 이미지, 모델 및 셰이더 리소스를 생성, 편집, 미리 보기 및 내보낼 수 있는 추가 도구가 있습니다. 빌드 시 리소스를 변환 하 고 DirectX 그래픽 코드를 디버그 하는 데 사용할 수 있는 도구도 있습니다.

이 항목에서는 이러한 그래픽 도구에 대 한 개요를 제공 합니다.

## <a name="image-editor"></a>이미지 편집기


이미지 편집기를 사용 하 여 DirectX에서 사용 하는 다양 한 질감 및 이미지 형식의 종류를 사용할 수 있습니다. 이미지 편집기는 다음과 같은 형식을 지원 합니다.

-   .png
-   .jpg, .jpeg, .jpe, .jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif, .tiff
-   .tga

빌드 시 빌드 [사용자 지정 파일](#build-customizations-for-3d-assets) 을 만들어 dds 파일로 변환 합니다.

자세한 내용은 [질감 및 이미지 작업](/visualstudio/designers/working-with-textures-and-images?view=vs-2015)을 참조 하세요.

> **참고**    이미지 편집기는 전체 기능 이미지 편집 앱을 대체 하기 위한 것이 아니며 많은 간단한 보기 및 편집 시나리오에 적합 합니다.

 

## <a name="model-editor"></a>모델 편집기


모델 편집기를 사용 하 여 기본 3D 모델을 처음부터 새로 만들거나, 완전 한 기능을 갖춘 3D 모델링 도구에서 보다 복잡 한 3D 모델을 보고 수정할 수 있습니다. 모델 편집기는 DirectX 앱 개발에 사용되는 여러 가지 3D 모델 형식을 지원합니다. 빌드 시간에 이러한 파일을 cmo 파일로 변환 하기 위해 [빌드 사용자 지정 파일](#build-customizations-for-3d-assets) 을 만들 수 있습니다.

-   .fbx
-   .dae
-   .obj

다음은 조명이 적용 된 편집기의 모델 스크린샷입니다.

![주전자](images/modeleditor.png)

자세한 내용은 [3 차원 모델 작업](/visualstudio/designers/working-with-3-d-models?view=vs-2015)을 참조 하세요.

> **참고**    모델 편집기는 전체 기능 모델 편집 앱을 대체 하기 위한 것이 아니며 많은 간단한 보기 및 편집 시나리오에 적합 합니다.

 

## <a name="shader-designer"></a>셰이더 디자이너


HLSL 프로그래밍을 모를 경우에도 셰이더 디자이너를 사용 하 여 게임 또는 앱에 대 한 사용자 지정 시각적 효과를 만들 수 있습니다.

셰이더를 그래프로 시각적으로 만듭니다. 각 노드는 해당 작업에 대 한 출력의 미리 보기를 표시 합니다. 다음은 구 미리 보기를 사용 하 여 램버트 조명을 적용 하는 예제입니다.

![시각적 셰이더 그래프](images/shaderdesigner.png)

셰이더 편집기를 사용 하 여 dgsl 형식으로 셰이더를 디자인, 편집 및 저장 합니다. 또한 다음 형식을 내보냅니다.

-   . hlsl (소스 코드)
-   . caa(바이트 코드)
-   .h (HLSL 바이트 코드 배열)

빌드 시간에 이러한 형식을 .ccefiles로 변환 하는 [빌드 사용자 지정 파일](#build-customizations-for-3d-assets) 을 만듭니다.

다음은 셰이더 편집기에서 내보내는 HLSL 코드의 일부입니다. 이는 램버트 조명 노드에 대 한 코드입니다.

```hlsl
//
// Lambert lighting function
//
float3 LambertLighting(
    float3 lightNormal,
    float3 surfaceNormal,
    float3 materialAmbient,
    float3 lightAmbient,
    float3 lightColor,
    float3 pixelColor
    )
{
    // Compute the amount of contribution per light.
    float diffuseAmount = saturate(dot(lightNormal, surfaceNormal));
    float3 diffuse = diffuseAmount * lightColor * pixelColor;

    // Combine ambient with diffuse.
    return saturate((materialAmbient * lightAmbient) + diffuse);
}
```

자세한 내용은 [셰이더 작업](/visualstudio/designers/working-with-shaders?view=vs-2015)을 참조 하세요.

## <a name="build-customizations-for-3d-assets"></a>3D 자산에 대 한 사용자 지정 빌드


Visual Studio에서 리소스를 사용 가능한 형식으로 변환 하도록 프로젝트에 빌드 사용자 지정을 추가할 수 있습니다. 그런 다음 앱으로 자산을 로드한 후 다른 모든 DirectX 앱에서 하는 것처럼 DirectX 리소스를 만든 후 채워 자산을 사용할 수 있습니다.

빌드 사용자 지정을 추가 하려면 **솔루션 탐색기** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **사용자 지정 빌드**...를 선택 합니다. 프로젝트에 다음 형식의 빌드 사용자 지정을 추가할 수 있습니다.

-   이미지 콘텐츠 파이프라인은 이미지 파일을 입력으로 사용 하 고 DirectDraw Surface (dds) 파일을 출력 합니다.
-   메시 콘텐츠 파이프라인은 메시 파일 (예: fbx) 및 cmo 메시 파일을 사용 합니다.
-   셰이더 콘텐츠 파이프라인은 Visual Studio 셰이더 편집기에서 시각적 셰이더 그래프 (. dgsl)를 사용 하 고 컴파일된 셰이더 출력 파일 (.cs)을 출력 합니다.

자세한 내용은 [게임 또는 앱에서 3 차원 자산 사용](/visualstudio/designers/using-3-d-assets-in-your-game-or-app?view=vs-2015)을 참조 하세요.

## <a name="debugging-directx-graphics"></a>DirectX 그래픽 디버깅


Visual Studio는 그래픽 별 디버깅 도구를 제공 합니다. 이러한 도구를 사용 하 여 다음과 같은 작업을 디버그할 수 있습니다.

-   그래픽 파이프라인입니다.
-   이벤트 호출 스택입니다.
-   개체 테이블입니다.
-   장치 상태입니다.
-   셰이더 버그.
-   초기화 되지 않았거나 잘못 된 상수 버퍼 및 매개 변수입니다.
-   DirectX 버전 호환성.
-   제한 된 Direct2D 지원.
-   운영 체제 및 SDK 요구 사항

자세한 내용은 [DirectX 그래픽 디버그](/visualstudio/debugger/visual-studio-graphics-diagnostics?view=vs-2015)를 참조 하세요.


 

 

 