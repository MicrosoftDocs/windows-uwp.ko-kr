---
title: OpenGL ES 2.0에서 Direct3D 11로 포팅
description: OpenGL ES 2.0 그래픽 파이프라인을 Direct3D 11 및 Windows 런타임으로 포팅하는 방법에 대한 문서, 개요 및 설명이 포함되어 있습니다.
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, opengl, direct3d 11, 포트, 그래픽
ms.localizationpriority: medium
ms.openlocfilehash: 9af5e42a27e21b8a4300edc4b8171f7abc64bac7
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7827572"
---
# <a name="port-from-opengl-es-20-to-direct3d-11"></a>OpenGL ES 2.0에서 Direct3D 11로 포팅



OpenGL ES 2.0 그래픽 파이프라인을 Direct3D 11 및 Windows 런타임으로 포팅하는 방법에 대한 문서, 개요 및 연습이 포함되어 있습니다.

> **참고**  OpenGL ES 2.0 프로젝트를 포팅하는 중간 단계는 Microsoft Store 용 ANGLE을 사용 합니다. ANGLE을 사용하면 OpenGL ES API 호출을 DirectX 11 API 호출로 변환하여 Windows에서 OpenGL ES 콘텐츠를 실행할 수 있습니다. ANGLE에 대한 자세한 내용은 [Microsoft Store용 ANGLE Wiki](http://go.microsoft.com/fwlink/p/?linkid=618387)를 참조하세요.

 

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
<td align="left"><p><a href="map-concepts-and-infrastructure.md">Direct3D 11.1에 OpenGL ES 2.0 매핑</a></p></td>
<td align="left"><p>처음으로 OpenGL ES 2.0에서 Direct3D로 그래픽 아키텍처를 포팅하는 프로세스를 시작하는 경우에는 API 간의 주요 차이점에 익숙해지도록 하세요. 이 섹션의 항목은 그래픽 처리를 Direct3D로 이동할 때 수행해야 하는 API 변경과 포팅 전략을 계획하는 데 도움이 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md">방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11.1로 포팅</a></p></td>
<td align="left"><p>이 포팅 연습에 대한 기본 사항인 Visual Studio 2015에서 DirectX 11 앱(유니버설 Windows) 템플릿을 일치시키는 것과 같이, 회전하는 꼭짓점 음영 큐브에 대한 간단한 렌더러를 OpenGL ES 2.0에서 Direct3D로 가져오기에 대해 먼저 살펴보겠습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="opengl-es-2-0-to-directx-11-1-reference.md">OpenGL ES 2.0-Direct3D 11.1 참조</a></p></td>
<td align="left"><p>OpenGL ES 2.0에서 Direct3D 11로 포팅할 때 API 매핑 및 간단한 코드 샘플을 보려면 다음 참조 항목을 사용하세요.</p></td>
</tr>
</tbody>
</table>

 

 

 




