---
title: Direct3D 그래픽 용어
description: Microsoft Direct3D에서 사용하는 그래픽 용어를 정의합니다.
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Direct3D 그래픽 용어
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3cb6a2466ea201c9b5047f7eb159477a0d584429
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57583289"
---
# <a name="direct3d-graphics-glossary"></a>Direct3D 그래픽 용어


Microsoft Direct3D 그래픽 용어를 정의합니다. 여기서는 Direct3D 게임 및 앱 개발에 사용되는 일반적인 3D 컴퓨터 그래픽 용어를 개괄적으로 정의하고 있습니다.

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
<td align="left"><p>Direct3D 애플리케이션을 프로그래밍하려면 3D 기하 도형 원칙에 익숙해야 합니다. 이 섹션에서는 3D 장면을 만드는 가장 중요한 기하 도형 개념을 소개합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">꼭짓점 및 인덱스 버퍼</a></p></td>
<td align="left"><p><em>꼭짓점 버퍼</em>는 꼭짓점 데이터를 포함한 메모리 버퍼입니다. 꼭짓점 버퍼의 꼭짓점을 처리하여 변환, 조명 및 클리핑을 수행합니다. <em>인덱스 버퍼</em>는 인덱스 데이터를 포함한 메모리 버퍼이며, 기본 요소를 렌더링하는 데 사용되는 꼭짓점 버퍼에 대한 정수 오프셋입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">디바이스</a></p></td>
<td align="left"><p>Direct3D 디바이스는 Direct3D의 렌더링 구성 요소입니다. 디바이스는 렌더링 상태를 캡슐화하여 저장하고, 변환 및 조명 작업을 수행하며, 이미지를 표면에 래스터화합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">조명</a></p></td>
<td align="left"><p>조명은 장면의 개체를 비추는 데 사용됩니다. 각 개체 꼭짓점의 색은 현재 질감 맵, 꼭짓점 색, 광원을 기반으로 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">깊이 및 스텐실 버퍼</a></p></td>
<td align="left"><p><em>깊이 버퍼</em>는 보기에서 숨기는 대신 렌더링하는 다각형의 영역을 제어하기 위한 깊이 정보를 저장합니다. <em>스텐실 버퍼</em>는 합치기, 데칼코마니, 흩어뿌리기, 페이드, 살짝 밀기, 윤곽선과 그림자 및 양면 스텐실을 포함한 특수 효과를 생성하기 위해 이미지에서 픽셀을 마스킹하는 데 사용합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">질감</a></p></td>
<td align="left"><p>질감은 컴퓨터에서 생성되는 3D 이미지에 현실감을 불어넣을 수 있는 강력한 도구입니다. Direct3D는 광범위한 질감 기능 세트를 지원하여 개발자가 고급 질감 기술에 쉽게 액세스할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">그래픽 파이프라인</a></p></td>
<td align="left"><p>Direct3D 그래픽 파이프라인은 실시간 게임 애플리케이션용 그래픽을 생성하도록 설계되었습니다. 데이터는 각각의 구성 가능하거나 프로그래밍 가능한 각 단계를 통해 입력에서 출력으로 흐릅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">보기</a></p></td>
<td align="left"><p>&quot;보기&quot;라는 용어는 &quot;필요한 형식의 데이터&quot;를 의미합니다. 예를 들어 CBV(상수 버퍼 보기)는 올바른 형식의 상수 버퍼 데이터입니다. 이 섹션에서는 가장 일반적이고 유용한 보기에 대해 설명합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">컴퓨팅 파이프라인</a></p></td>
<td align="left"><p>Direct3D 컴퓨팅 파이프라인은 그래픽 파이프라인과 거의 동시에 수행할 수 있는 계산을 처리하도록 설계되었습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">리소스</a></p></td>
<td align="left"><p>리소스는 Direct3D 파이프라인에서 액세스할 수 있는 메모리 내 영역입니다. 파이프라인에서 메모리에 효율적으로 액세스하려면 파이프라인에 제공되는 데이터(입력 기하 도형, 셰이더 리소스, 질감 등)를 리소스에 저장해야 합니다. 모든 Direct3D 리소스가 파생되는 두 가지 리소스 종류는 버퍼와 질감입니다. 각 파이프라인 단계마다 최대 128개의 리소스를 활성화할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">스트리밍 리소스</a></p></td>
<td align="left"><p><em>스트리밍 리소스</em>는 적은 양의 실제 메모리를 사용하는 논리적 큰 리소스입니다. 전체 큰 리소스를 전달하는 대신 리소스의 작은 부분이 필요에 따라 스트리밍됩니다. 스트리밍 리소스는 이전에 <em>타일식 리소스</em>라고 했습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">부록</a></p></td>
<td align="left"><p>이 섹션에서는 자세한 기술 정보를 제공합니다.</p></td>
</tr>
</tbody>
</table>

 

 

 
