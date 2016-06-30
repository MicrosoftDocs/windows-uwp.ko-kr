---
author: mtoepke
title: "OpenGL ES 2.0에서 Direct3D 11로 포팅"
description: "OpenGL ES 2.0 그래픽 파이프라인을 Direct3D 11 및 Windows 런타임으로 포팅에 대한 문서, 개요 및 설명이 포함되어 있습니다."
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.sourcegitcommit: 814f056eaff5419b9c28ba63cf32012bd82cc554
ms.openlocfilehash: 40380582a9210cb705a5e7e591d4a8f37c42f8dd

---

# OpenGL ES 2.0에서 Direct3D 11로 포팅


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

OpenGL ES 2.0 그래픽 파이프라인을 Direct3D 11 및 Windows 런타임으로 포팅에 대한 문서, 개요 및 설명이 포함되어 있습니다.

> **참고** OpenGL ES 2.0 프로젝트를 포팅하는 중간 단계에서 Windows 스토어용 ANGLE을 사용합니다. ANGLE을 사용하면 OpenGL ES API 호출을 DirectX 11 API 호출로 변환하여 Windows에서 OpenGL ES 콘텐츠를 실행할 수 있습니다. ANGLE에 대한 자세한 내용은 [Windows 스토어용 ANGLE Wiki](http://go.microsoft.com/fwlink/p/?linkid=618387)를 참조하세요.

 

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
<td align="left"><p>[Direct3D 11.1에 OpenGL ES 2.0 매핑](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>처음으로 OpenGL ES 2.0에서 Direct3D로 그래픽 아키텍처를 포팅하는 프로세스를 시작하는 경우에는 API 간의 주요 차이점에 익숙해지도록 하세요. 이 섹션의 항목은 그래픽 처리를 Direct3D로 이동할 때 수행해야 하는 API 변경과 포팅 전략을 계획하는 데 도움이 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11.1로 포팅](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)</p></td>
<td align="left"><p>이 포팅 연습에 대한 기본 사항인 Visual Studio 2015에서 DirectX 11 앱(유니버설 Windows) 템플릿을 일치시키는 것과 같이, 회전하는 꼭짓점 음영 큐브에 대한 간단한 렌더러를 OpenGL ES 2.0에서 Direct3D로 가져오기에 대해 먼저 살펴보겠습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[OpenGL ES 2.0-Direct3D 11.1 참조](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>OpenGL ES 2.0에서 Direct3D 11로 포팅할 때 API 매핑 및 간단한 코드 샘플을 보려면 다음 참조 항목을 사용합니다.</p></td>
</tr>
</tbody>
</table>

 

> **참고**  
이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

 

 







<!--HONumber=Jun16_HO4-->


