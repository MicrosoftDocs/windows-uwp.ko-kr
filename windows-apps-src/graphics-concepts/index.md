---
title: Direct3D 그래픽 용어
description: Microsoft Direct3D가 사용 되는 그래픽 용어를 정의 합니다.
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Direct3D 그래픽 용어
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e325494cefeab4cf3ed40939f5b24de5e921b68
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6186414"
---
# <a name="direct3d-graphics-glossary"></a>Direct3D 그래픽 용어


Microsoft Direct3D 그래픽 용어를 정의 합니다. Direct3D 게임 및 앱 개발에 사용 되는 높은 수준의 일반적인 3D 컴퓨터 그래픽 용어에,이 용어를 정의 합니다.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>이 섹션의 내용


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
<td align="left"><p><a href="coordinate-systems-and-geometry.md">좌표계 및 기하 도형</a></p></td>
<td align="left"><p>Direct3D 응용 프로그램을 프로그래밍하려면 3D 기하 도형 원칙 작업에 익숙해야 합니다. 이 섹션에서는 3D 장면 만들기에 가장 중요한 기하 개념을 소개합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">꼭짓점 및 인덱스 버퍼</a></p></td>
<td align="left"><p><em>꼭짓점 버퍼</em>는 꼭짓점 데이터를 포함한 메모리 버퍼입니다. 꼭짓점 버퍼의 꼭짓점을 처리하여 변환, 조명 및 클리핑을 수행합니다. <em>인덱스 버퍼</em>는 인덱스 데이터를 포함하는 메모리 버퍼로써 기본 요소를 렌더링하는 데 사용되는 꼭짓점 버퍼에 대한 정수 오프셋입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">장치</a></p></td>
<td align="left"><p>Direct3D 디바이스는 Direct3D의 렌더링 구성 요소입니다. 장치는 렌더링 상태를 캡슐화하여 저장하고, 변환 및 조명 작업을 수행하고 이미지를 표면에 래스터화합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">조명</a></p></td>
<td align="left"><p>조명은 장면에서 물체를 비추는 데 사용됩니다. 각 개체 꼭짓점의 색상은 현재 텍스처 맵, 꼭짓점 색상, 광원을 기반으로 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">깊이 및 스텐실 버퍼</a></p></td>
<td align="left"><p><em>깊이 버퍼</em>는 깊이 정보를 저장하여 다각형의 어떤 부분이 시야에서 숨겨지지 않고 렌더링될지 제어합니다. <em>스텐실 버퍼</em>는 이미지에서 픽셀을 마스킹하여 합성, 디스케일링, 디졸브, 페이드, 스와이프, 아웃라인과 실루엣, 양면 스텐실 등 특수 효과를 내는 데 사용됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">텍스처</a></p></td>
<td align="left"><p>텍스처는 컴퓨터에서 생성되는 3D 이미지에 현실감을 불어넣을 수 있는 강력한 도구입니다. Direct3D는 광범위한 텍스처 기능 세트를 지원하여 개발자가 고급 텍스처 기법에 쉽게 접근할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">그래픽 파이프라인</a></p></td>
<td align="left"><p>Direct3D 그래픽 파이프라인은 실시간 게임 응용 프로그램의 그래픽을 생성하도록 설계되었습니다. 데이터는 각각의 구성 가능하거나 프로그래밍 가능한 단계를 통해 입력에서 출력으로 흐릅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">보기</a></p></td>
<td align="left"><p>&quot;보기&quot;라는 용어는 &quot;필요한 형식의 데이터&quot;를 뜻하는 데 사용됩니다. 예를 들어, CBV(상수 버퍼 보기)는 올바른 형식의 일정한 버퍼 데이터일 것입니다. 이 섹션은 가장 공통적이고 유용한 보기를 설명합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">계산 파이프라인</a></p></td>
<td align="left"><p>Direct3D 컴퓨팅 파이프라인은 대체로 그래픽 파이프라인과 동시에 수행할 수 있는 계산을 처리하도록 만들어졌습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">리소스</a></p></td>
<td align="left"><p>리소스는 Direct3D 파이프라인에서 액세스할 수 있는 메모리 내 영역입니다. 파이프라인에서 메모리에 효율적으로 액세스하려면 파이프라인에 제공된 데이터(입력 기하 도형, 셰이더 리소스, 텍스처 등)를 리소스에 저장해야 합니다. 모든 Direct3D 리소스가 파생되는 리소스 종류는 버퍼와 텍스처, 두 가지입니다. 각 파이프라인 단계에 최대 128개 리소스를 활성화할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">스트리밍 리소스</a></p></td>
<td align="left"><p><em>스트리밍 리소스</em>는 소량의 실제 메모리를 사용하는 대용량 논리 리소스입니다. 전체 대용량 리소스를 전달하는 대신, 리소스의 일부가 필요에 따라 스트리밍됩니다. 이전에는 스트리밍 리소스를 <em>타일식 리소스</em>라고 했습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">부록</a></p></td>
<td align="left"><p>이 섹션은 자세한 기술 세부 정보를 제공합니다.</p></td>
</tr>
</tbody>
</table>

 

 

 
