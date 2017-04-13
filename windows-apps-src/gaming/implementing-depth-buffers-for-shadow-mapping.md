---
author: mtoepke
title: "연습 - Direct3D 11 섀도 볼륨 깊이 버퍼"
description: "이 연습에서는 모든 Direct3D 기능 수준의 디바이스에서 Direct3D 11을 사용하여 깊이 맵을 사용하는 섀도 볼륨을 렌더링하는 방법을 보여 줍니다."
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 게임, directx, 섀도 볼륨, 깊이 버퍼, directx 11"
ms.openlocfilehash: e68f0d559e6dbd7791104310a985ca99e0131c99
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>연습: Direct3D 11에서 깊이 버퍼를 사용하여 섀도 볼륨 구현


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

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
<td align="left"><p>[깊이 버퍼 디바이스 리소스 만들기](create-depth-buffer-resource--view--and-sampler-state.md)</p></td>
<td align="left"><p>섀도 볼륨에 대한 깊이 테스트를 지원하는 데 필요한 Direct3D 디바이스 리소스를 만드는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[그림자 맵을 깊이 버퍼로 렌더링](render-the-shadow-map-to-the-depth-buffer.md)</p></td>
<td align="left"><p>광원의 관점에서 렌더링하여 그림자 볼륨을 나타내는 2차원 깊이 맵을 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[깊이 테스트로 장면 렌더링](render-the-scene-with-depth-testing.md)</p></td>
<td align="left"><p>꼭짓점(또는 기하 도형) 셰이더와 픽셀 셰이더에 깊이 테스트를 추가하여 그림자 효과를 만듭니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[다양한 하드웨어에서 그림자 맵 지원](target-a-range-of-hardware.md)</p></td>
<td align="left"><p>더 빠른 디바이스에서 충실도가 더 높은 그림자를 렌더링하고 덜 강력한 디바이스에서 보다 빠른 그림자를 렌더링합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Direct3D 9 데스크톱 포팅에 그림자 매핑 적용


Windows 8에서는 기능 수준 9\_1과 9\_3에 깊이 비교 기능이 추가되었습니다. 이제 섀도 볼륨이 포함된 렌더링 코드를 DirectX 11로 마이그레이션할 수 있으므로 Direct3D 11 렌더러가 하위 수준인 기능 수준 9 디바이스와 호환됩니다. 이 연습에서는 깊이 테스트를 사용하여 Direct3D 11 앱이나 게임에서 기존의 섀도 볼륨을 구현할 수 있는 방법을 보여 줍니다. 코드에는 다음 프로세스가 포함됩니다.

1.  섀도 매핑을 위한 Direct3D 장치 리소스 만들기
2.  깊이 맵을 만드는 렌더링 단계 추가
3.  기본 렌더링 단계에 깊이 테스트 추가
4.  필요한 셰이더 코드 구현
5.  하위 수준 하드웨어의 빠른 렌더링 옵션

이 연습을 완료하면 기능 수준 9\_1 이상과 호환되는 Direct3D 11에서 호환되는 기본 섀도 볼륨 기술을 구현하는 방법에 익숙해지게 됩니다.

## <a name="prerequisites"></a>필수 조건


[UWP(유니버설 Windows 플랫폼) DirectX 게임 개발을 위한 개발 환경을 준비](prepare-your-dev-environment-for-windows-store-directx-game-development.md)해야 합니다. 아직 템플릿은 필요하지 않지만 이 연습을 위한 코드 샘플을 작성하려면 Microsoft Visual Studio 2015가 필요합니다.

## <a name="related-topics"></a>관련 항목


**Direct3D**

* [Direct3D 9에서 HLSL 셰이더 작성](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [UWP용 새 DirectX 11 프로젝트 만들기](user-interface.md)

**그림자 매핑 기술 문서**

* [그림자 깊이 맵을 향상시키기 위한 일반적인 기술](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [중첩된 그림자 맵](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 




