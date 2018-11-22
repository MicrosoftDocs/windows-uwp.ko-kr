---
author: mtoepke
title: Direct3D 11에 OpenGL ES 2.0 매핑
description: 처음으로 OpenGL ES 2.0에서 Direct3D로 그래픽 아키텍처를 포팅하는 프로세스를 시작하는 경우에는 API 간의 주요 차이점에 익숙해지도록 하세요.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, opengl, direct3d, 포팅
ms.localizationpriority: medium
ms.openlocfilehash: 532c2a0a9779ae3eaedb2217175dc0805514f792
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7581633"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Direct3D 11에 OpenGL ES 2.0 매핑



처음으로 OpenGL ES 2.0에서 Direct3D로 그래픽 아키텍처를 포팅하는 프로세스를 시작하는 경우에는 API 간의 주요 차이점에 익숙해지도록 하세요. 이 섹션의 항목은 그래픽 처리를 Direct3D로 이동할 때 수행해야 하는 API 변경과 포팅 전략을 계획하는 데 도움이 됩니다.
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
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">OpenGL ES 2.0에서 Direct3D로의 포팅 계획</a></p></td>
<td align="left"><p>iOS 또는 Android 플랫폼에서 게임을 포팅하는 경우 OpenGL ES 2.0에 많은 투자를 했을 것입니다. 그래픽 파이프라인 코드베이스를 Direct3D 11 및 Windows 런타임으로 이동할 준비를 할 때에는 시작하기 전에 몇 가지 사항을 고려해야 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">EGL 코드와 DXGI 및 Direct3D 비교</a></p></td>
<td align="left"><p>DXGI(DirectX Graphics Interface) 및 여러 Direct3D API는 EGL과 동일한 역할을 합니다. 이 항목은 EGL의 관점에서 DXGI 및 Direct3D 11을 이해하는 데 도움을 줍니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">OpenGL ES 2.0 버퍼, 유니폼 및 꼭짓점 특성과 Direct3D 비교</a></p></td>
<td align="left"><p>OpenGL ES 2.0에서 Direct3D 11로 포팅하는 프로세스 중에 앱과 셰이더 프로그램 사이에 데이터를 전달하기 위한 구문 및 API 동작을 변경해야 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">OpenGL ES 2.0 셰이더 파이프라인과 Direct3D 비교</a></p></td>
<td align="left"><p>개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0의 셰이더 파이프라인과 매우 유사합니다. 그러나 API 디자인의 관점에서 셰이더 단계를 만들고 관리하기 위한 주요 구성 요소는 두 가지 주 인터페이스 <a href="https://msdn.microsoft.com/library/windows/desktop/hh404575"><strong>ID3D11Device1</strong></a> 및 <a href="https://msdn.microsoft.com/library/windows/desktop/hh404598"><strong>ID3D11DeviceContext1</strong></a>의 일부입니다. 이 항목에서는 일반적인 OpenGL ES 2.0 셰이더 파이프라인 API 패턴을 이러한 인터페이스의 Direct3D 11 셰이더 파이프라인 API 패턴에 매핑하려고 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>특정 OpenGL ES 2.0 공급자에 대한 참고 사항


이러한 항목에서는 플랫폼 독립적 C와 Khronos OpenGL ES 2.0 사양을 사용합니다. iOS와 Android는 동일한 사양을 사용하며, 이러한 플랫폼용으로 개발된 OpenGL ES 2.0 코드는 앞으로 살펴볼 코드 조각과 매우 유사합니다(일반적으로 개체 지향 API로 표시되기는 함). 또한 각 플랫폼의 내부 기능 및 언어 차이 때문에 사소한 차이점은 있을 수 있으며, 특히 메서드 매개 변수 형식 또는 일반적인 언어 구문에서 사소한 차이점이 있을 수 있습니다. 예를 들어 iOS에서는 Objective-C를 사용합니다. Android에 C++를 사용할 수 있는 기능이 있기는 하지만, 일부 개발자는 순수 Java 구현에 의존해 왔을 수 있습니다. 이러한 사실을 염두에 둔다면, OpenGL ES API의 전반적인 개념, 구조 및 사용이 다르지 않으므로 이러한 항목이 여전히 도움이 될 것입니다.

 

 




