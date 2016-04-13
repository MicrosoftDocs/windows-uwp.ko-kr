---
OpenGL ES 2.0에서 Direct3D 11로 포팅
OpenGL ES 2.0 그래픽 파이프라인을 Direct3D 11 및 Windows 런타임으로 포팅에 대한 문서, 개요 및 설명이 포함되어 있습니다.
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
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
<td align="left"><p>[Map OpenGL ES 2.0 to Direct3D 11.1](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>처음으로 OpenGL ES 2.0에서 Direct3D로 그래픽 아키텍처를 포팅하는 프로세스를 시작하는 경우에는 API 간의 주요 차이점에 익숙해지도록 하세요. 이 섹션의 항목은 그래픽 처리를 Direct3D로 이동할 때 수행해야 하는 API 변경과 포팅 전략을 계획하는 데 도움이 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Walkthrough sample ports from OpenGL ES 2.0](walkthrough-sample-ports-from-opengl-es-2-0.md)</p></td>
<td align="left"><p>이 항목에서는 각각 복잡한 정도가 다른 다양한 OpenGL ES 2.0 그래픽 파이프라인 포팅 시나리오를 살펴봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[OpenGL ES 2.0 to Direct3D 11.1 reference](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>OpenGL ES 2.0에서 Direct3D 11로 포팅할 때 API 매핑 및 간단한 코드 샘플을 보려면 다음 참조 항목을 사용합니다.</p></td>
</tr>
</tbody>
</table>

 

> **참고**  
이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

 

 






<!--HONumber=Mar16_HO1-->


