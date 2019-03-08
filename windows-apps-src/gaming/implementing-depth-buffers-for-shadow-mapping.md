---
title: 연습 - Direct3D 11 섀도 볼륨 깊이 버퍼
description: 이 연습에서는 모든 Direct3D 기능 수준의 디바이스에서 Direct3D 11을 사용하여 깊이 맵을 사용하는 섀도 볼륨을 렌더링하는 방법을 보여 줍니다.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 섀도 볼륨, 깊이 버퍼, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 2feecb3080efefb2f9625fd8b66c5b722ad02a45
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622278"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>연습: Direct3D 11에서 깊이 버퍼를 사용 하 여 구현 섀도 볼륨



이 연습에서는 모든 Direct3D 기능 수준의 디바이스에서 Direct3D 11을 사용하여 깊이 맵을 사용하는 섀도 볼륨을 렌더링하는 방법을 보여 줍니다.
## 
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
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">깊이 버퍼 장치 리소스 만들기</a></p></td>
<td align="left"><p>섀도 볼륨에 대한 깊이 테스트를 지원하는 데 필요한 Direct3D 디바이스 리소스를 만드는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">섀도 지도 깊이 버퍼를 렌더링 합니다.</a></p></td>
<td align="left"><p>광원의 관점에서 렌더링하여 그림자 볼륨을 나타내는 2차원 깊이 맵을 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">깊이 테스트 장면 렌더링</a></p></td>
<td align="left"><p>꼭짓점(또는 기하 도형) 셰이더와 픽셀 셰이더에 깊이 테스트를 추가하여 그림자 효과를 만듭니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">다양 한 하드웨어에서 섀도 maps 지원</a></p></td>
<td align="left"><p>더 빠른 디바이스에서 충실도가 더 높은 그림자를 렌더링하고 덜 강력한 디바이스에서 보다 빠른 그림자를 렌더링합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Direct3D 9 데스크톱 포팅에 그림자 매핑 적용


Windows 8 adde d 깊이 비교 기능을 기능 수준 9\_1에서 9\_3입니다. 이제 섀도 볼륨이 포함된 렌더링 코드를 DirectX 11로 마이그레이션할 수 있으므로 Direct3D 11 렌더러가 하위 수준인 기능 수준 9 디바이스와 호환됩니다. 이 연습에서는 깊이 테스트를 사용하여 Direct3D 11 앱이나 게임에서 기존의 섀도 볼륨을 구현할 수 있는 방법을 보여 줍니다. 코드에는 다음 프로세스가 포함됩니다.

1.  섀도 매핑을 위한 Direct3D 장치 리소스 만들기
2.  깊이 맵을 만드는 렌더링 단계 추가
3.  기본 렌더링 단계에 깊이 테스트 추가
4.  필요한 셰이더 코드 구현
5.  하위 수준 하드웨어의 빠른 렌더링 옵션

이 연습을 완료 하면 Direct3D 11 기능 수준 9와 호환 되는 기본 호환 섀도 볼륨 기술을 구현 하는 방법을 잘 알고 있어야\_1 이상입니다.

## <a name="prerequisites"></a>필수 구성 요소


[UWP(유니버설 Windows 플랫폼) DirectX 게임 개발을 위한 개발 환경을 준비](prepare-your-dev-environment-for-windows-store-directx-game-development.md)해야 합니다. 템플릿을 아직 필요는 없지만이 연습에 대 한 코드 샘플을 빌드하려면 Microsoft Visual Studio 2015를 해야 합니다.

## <a name="related-topics"></a>관련 항목


**Direct3D**

* [Direct3D에서 HLSL 셰이더 작성 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [UWP 용 새 DirectX 11 프로젝트를 만들려면](user-interface.md)

**기술 문서 매핑 섀도**

* [섀도 깊이 지도 개선 하는 일반적인 기술](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [연계 섀도 맵](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 




