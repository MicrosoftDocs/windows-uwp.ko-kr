---
author: mtoepke
title: DirectX 게임의 기본 3D 그래픽
description: DirectX 프로그래밍을 사용해 3D 그래픽의 기본 개념을 구현하는 방법을 소개합니다.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 그래픽
ms.localizationpriority: medium
ms.openlocfilehash: e9834a83620343f26acaabd0e05b30cc2c1dcfab
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7173666"
---
# <a name="basic-3d-graphics-for-directx-games"></a>DirectX 게임용 기본 3D 그래픽



DirectX 프로그래밍을 사용해 3D 그래픽의 기본 개념을 구현하는 방법을 소개합니다.

**목표:** 3D 그래픽 앱의 프로그래밍 방법을 알아봅니다.

## <a name="prerequisites"></a>필수 조건


사용자가 C++에 익숙하다고 가정합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

**총 완료 시간:** 30분입니다.

## <a name="where-to-go-from-here"></a>여기에서 이동할 위치


여기서는 DirectX 및 C++\\Cx를 사용하여 3D 그래픽을 개발하는 방법에 대해 설명합니다. 이 5개의 부분으로 구성된 자습서에서는 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) API와 많은 다른 DirectX 샘플에서도 사용된 개념 및 코드에 대해 소개합니다. 각 부분은 UWP C++ 앱용 DirectX 구성에서 기본 형식의 텍스처 설정 및 효과 추가에 이르기까지 서로를 기반으로 구성됩니다.

> **참고**열 벡터를 사용 하 여이 자습서에서는 오른손 좌표계를 사용 합니다. 대다수의 DirectX 샘플 및 앱에서는 왼손용 행 벡터 좌표계를 사용합니다. 왼손용 열 벡터 좌표계를 지원하는 보다 완벽한 그래픽 수학 솔루션에는 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)를 사용하는 것이 좋습니다. 자세한 내용은 [DirectXMath와 Direct3D 사용](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D)을 참조하세요.

 

다음 작업 방법을 보여 줍니다.

-   Windows 런타임을 사용해 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) 인터페이스 초기화
-   꼭짓점별 셰이더 작업 적용 방법
-   기하 도형 설정 방법
-   장면 래스터화(3D 장면을 2D 투영으로 평면화) 방법
-   숨겨진 표면 선별 방법

> **참고**  

 

이제 Direct3D 장치, 스왑 체인 및 렌더링 대상 보기를 만들고, 렌더링된 이미지를 디스플레이에 표시합니다.

[빠른 시작: DirectX 리소스 설정 및 이미지 표시](setting-up-directx-resources.md)

## <a name="related-topics"></a>관련 항목


* [Direct3D 11 그래픽](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 




