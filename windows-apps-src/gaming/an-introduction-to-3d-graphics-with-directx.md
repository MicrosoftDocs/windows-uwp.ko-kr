---
title: DirectX 게임의 기본 3D 그래픽
description: DirectX 프로그래밍을 사용 하 여 3D 그래픽의 기본 개념을 구현 하는 방법을 보여 줍니다.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 그래픽
ms.localizationpriority: medium
ms.openlocfilehash: 68ee694c036d4bc92f76ea75f7c5e5e530e26334
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173197"
---
# <a name="basic-3d-graphics-for-directx-games"></a>DirectX 게임의 기본 3D 그래픽



DirectX 프로그래밍을 사용 하 여 3D 그래픽의 기본 개념을 구현 하는 방법을 보여 줍니다.

**목표:** 3D 그래픽 앱을 프로그래밍 하는 방법에 대해 알아봅니다.

## <a name="prerequisites"></a>필수 조건


C + +에 대해 잘 알고 있다고 가정 합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

**총 완료 시간:** 30 분

## <a name="where-to-go-from-here"></a>여기에서 이동할 위치


여기서는 DirectX 및 c + + Cx를 사용 하 여 3D 그래픽을 개발 하는 방법에 대해 설명 \\ 합니다. 다섯 부분으로 구성 된이 자습서에서는 [Direct3D](/windows/desktop/direct3d) API와 기타 많은 DirectX 샘플에서 사용 되는 개념 및 코드도 소개 합니다. 이러한 부분은 UWP c + + 앱에 대 한 DirectX를 구성 하 여 기본 형식을 질감 하 고 효과를 추가 하는 것부터 서로 빌드 됩니다.

> **참고**    이 자습서에서는 열 벡터가 있는 오른손 좌표계를 사용 합니다. 많은 DirectX 샘플과 앱에서 행 벡터가 있는 왼쪽 좌표 시스템을 사용 합니다. 보다 완전 한 그래픽 수학 솔루션과 행 벡터가 있는 왼손 좌표계를 지 원하는 경우에는 [Directxmath](/windows/desktop/dxmath/directxmath-portal)를 사용 하는 것이 좋습니다. 자세한 내용은 [Direct3D에서 DirectXMath 사용](/windows/desktop/dxmath/pg-xnamath-migration-d3dx)을 참조 하세요.

 

다음 작업을 수행 하는 방법을 보여 줍니다.

-   Windows 런타임를 사용 하 여 [Direct3D](/windows/desktop/direct3d) 인터페이스 초기화
-   꼭 짓 점 셰이더 작업 적용
-   기 하 도형 설정
-   장면 래스터화 (3D 장면을 2D 프로젝션으로 평면화)
-   숨겨진 표면 추려내

> **참고**  

 

다음으로, Direct3D 장치, 스왑 체인 및 렌더링 대상 뷰를 만들고 렌더링 된 이미지를 표시에 표시 합니다.

[빠른 시작: DirectX 리소스 설정 및 이미지 표시](setting-up-directx-resources.md)

## <a name="related-topics"></a>관련 항목


* [Direct3D 11 그래픽](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 