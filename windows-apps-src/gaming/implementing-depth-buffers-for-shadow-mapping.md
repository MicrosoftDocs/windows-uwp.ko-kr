---
title: 연습--Direct3D 11 섀도 볼륨 깊이 버퍼
description: 이 연습에서는 모든 Direct3D 기능 수준의 장치에서 Direct3D 11을 사용 하 여 깊이 맵을 사용 하 여 섀도 볼륨을 렌더링 하는 방법을 보여 줍니다.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 섀도 볼륨, 깊이 버퍼, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: e2b54f179d61a9479bdb921ef0a68b4c1772c20c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159387"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>연습: Direct3D 11에서 깊이 버퍼를 사용 하 여 섀도 볼륨 구현



이 연습에서는 모든 Direct3D 기능 수준의 장치에서 Direct3D 11을 사용 하 여 깊이 맵을 사용 하 여 섀도 볼륨을 렌더링 하는 방법을 보여 줍니다.
## 
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
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">깊이 버퍼 장치 리소스 만들기</a></p></td>
<td align="left"><p>섀도 볼륨의 깊이 테스트를 지 원하는 데 필요한 Direct3D 장치 리소스를 만드는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">그림자 맵을 깊이 버퍼로 렌더링 합니다.</a></p></td>
<td align="left"><p>광원의 관점에서 렌더링 하 여 그림자 볼륨을 나타내는 2 차원 깊이 지도를 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">깊이 테스트를 사용 하 여 장면 렌더링</a></p></td>
<td align="left"><p>꼭 짓 점 (또는 기 하 도형) 셰이더 및 픽셀 셰이더에 깊이 테스트를 추가 하 여 그림자 효과를 만듭니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">하드웨어 범위에서 섀도 맵 지원</a></p></td>
<td align="left"><p>더 빠른 장치에서 더 높은 성능의 그림자를 렌더링 하 고, 더 낮은 수준의 장치에서 더 빠른 그림자를 렌더링 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Direct3D 9 desktop 포팅에 응용 프로그램 섀도 매핑


기능 수준 9 \_ 1 및 9 3에 대 한 Windows 8 adde d 깊이 비교 기능 \_ . 이제 섀도 볼륨이 있는 렌더링 코드를 DirectX 11로 마이그레이션할 수 있으며, Direct3D 11 렌더러는 기능 수준 9 장치와 호환 됩니다. 이 연습에서는 심층적 테스트를 사용 하 여 Direct3D 11 앱 또는 게임에서 기존 섀도 볼륨을 구현 하는 방법을 보여 줍니다. 코드는 다음 프로세스를 포함 합니다.

1.  섀도 매핑을 위한 Direct3D 장치 리소스를 만드는 중입니다.
2.  렌더링 패스를 추가 하 여 깊이 맵 만들기
3.  주 렌더링 패스에 깊이 테스트를 추가 합니다.
4.  필요한 셰이더 코드를 구현 합니다.
5.  하위 하드웨어에서 빠른 렌더링을 위한 옵션입니다.

이 연습을 완료 한 후에는 기능 수준 9 1 이상과 호환 되는 Direct3D 11에서 기본적인 호환 가능한 섀도 볼륨 기술을 구현 하는 방법에 대해 잘 알고 있어야 합니다 \_ .

## <a name="prerequisites"></a>필수 구성 요소


[UWP (유니버설 Windows 플랫폼) DirectX 게임 개발을 위한 개발 환경을 준비](prepare-your-dev-environment-for-windows-store-directx-game-development.md)해야 합니다. 아직 템플릿이 필요 하지 않지만이 연습에 대 한 코드 샘플을 빌드하려면 Microsoft Visual Studio 2015이 필요 합니다.

## <a name="related-topics"></a>관련 항목


**Direct3D**

* [Direct3D 9에서 HLSL 셰이더 작성](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [UWP 용 새 DirectX 11 프로젝트 만들기](user-interface.md)

**섀도 매핑 기술 문서**

* [그림자 깊이 맵을 개선 하는 일반적인 기술](/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)
* [계단식 그림자 맵](/windows/desktop/DxTechArts/cascaded-shadow-maps)

 

 