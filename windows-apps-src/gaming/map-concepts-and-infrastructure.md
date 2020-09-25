---
title: Direct3D 11에 OpenGL ES 2.0 매핑
description: 처음으로 OpenGL ES 2.0에서 Direct3D로 그래픽 아키텍처를 이식 하는 프로세스를 시작할 때 Api 간의 주요 차이점을 숙지 합니다.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, opengl, direct3d, 포팅
ms.localizationpriority: medium
ms.openlocfilehash: d2f889e72215659c21990d84539670bfccddd340
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220056"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Direct3D 11에 OpenGL ES 2.0 매핑



처음으로 OpenGL ES 2.0에서 Direct3D로 그래픽 아키텍처를 이식 하는 프로세스를 시작할 때 Api 간의 주요 차이점을 숙지 합니다. 이 섹션의 항목은 그래픽 처리를 Direct3D로 이동할 때 수행 해야 하는 API 변경 및 포트 전략을 계획 하는 데 도움이 됩니다.
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
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">OpenGL ES 2.0에서 Direct3D로 포트 계획</a></p></td>
<td align="left"><p>IOS 또는 Android 플랫폼에서 게임을 이식 하는 경우 OpenGL ES 2.0에 상당한 투자가 발생 했을 것입니다. 그래픽 파이프라인 코드 베이스를 Direct3D 11 및 Windows 런타임로 이동 하기 위해 준비 하는 경우 시작 하기 전에 몇 가지 사항을 고려해 야 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">. C a l 코드와 DXGI 및 Direct3D 비교</a></p></td>
<td align="left"><p>DXGI (DirectX Graphics Interface)와 여러 Direct3D Api는 동일한 역할을 합니다. 이 항목은 다양 한 측면에서 DXGI 및 Direct3D 11을 이해 하는 데 도움이 됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">OpenGL ES 2.0 버퍼, 모든 형식 및 꼭 짓 점 특성을 Direct3D와 비교</a></p></td>
<td align="left"><p>OpenGL ES 2.0에서 Direct3D 11로 이식 하는 프로세스 중에 앱과 셰이더 프로그램 간에 데이터를 전달 하기 위한 구문 및 API 동작을 변경 해야 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">OpenGL ES 2.0 셰이더 파이프라인을 Direct3D와 비교</a></p></td>
<td align="left"><p>개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0에 있는 것과 매우 비슷합니다. 그러나 API 디자인 측면에서 셰이더 단계를 만들고 관리 하는 주요 구성 요소는 <a href="/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1"><strong>ID3D11Device1</strong></a> 및 <a href="/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1"><strong>ID3D11DeviceContext1</strong></a>의 두 가지 기본 인터페이스의 일부입니다. 이 항목에서는 일반적인 OpenGL ES 2.0 셰이더 파이프라인 API 패턴을 이러한 인터페이스에서 해당 하는 Direct3D 11에 매핑하려고 시도 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>특정 OpenGL ES 2.0 공급자에 대 한 참고 사항


이러한 항목에서는 플랫폼에 구애 받지 않는 C를 사용 하 여 Khronos OpenGL ES 2.0 사양을 사용 합니다. IOS와 Android는 모두 동일한 사양을 사용 하 고 이러한 플랫폼용으로 개발 된 OpenGL ES 2.0 코드는 일반적으로 개체 지향 Api로 노출 되기는 하지만 살펴볼 코드 조각과 매우 비슷합니다. 또한 각 플랫폼의 복잡성과 언어 차이로 인해 몇 가지 사소한 차이점이 있을 수 있습니다. 특히 메서드 매개 변수 형식이 나 일반 언어 구문에서 차이가 있을 수 있습니다. 예를 들어 iOS는 목표-C를 사용 합니다. Android에는 c + +를 사용 하는 기능이 있습니다. 그러나 일부 개발자는 순수 Java 구현에 의존 했을 수 있습니다. 이러한 항목은 전체 개념, 구조 및 OpenGL ES Api의 사용이 다르므로 여전히 유용 합니다.

 

 