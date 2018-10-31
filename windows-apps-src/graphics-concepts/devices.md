---
title: 디바이스
description: Direct3D 디바이스는 Direct3D의 렌더링 구성 요소입니다. 디바이스는 렌더링 상태를 캡슐화하여 저장하고, 변환 및 조명 작업을 수행하고 이미지를 표면에 래스터화합니다.
ms.assetid: BC903462-A32A-46BA-8411-FB294F5D2CD9
keywords:
- 디바이스
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d1c35af3dd1f8826fbd61268c5c47cef9d77146a
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5865593"
---
# <a name="devices"></a>디바이스


Direct3D 디바이스는 Direct3D의 렌더링 구성 요소입니다. 장치는 렌더링 상태를 캡슐화하여 저장하고, 변환 및 조명 작업을 수행하고 이미지를 표면에 래스터화합니다.

Direct3D 디바이스의 아키텍처에는 다음 다이어그램과 같이 변환 모듈, 조명 모둘, 및 래스터화 모듈이 포함됩니다.

![direct3d 디바이스 아키텍처의 다이어그램](images/d3ddev.png)

Direct3D는 다음과 같은 두 가지 주요 유형의 Direct3D 디바이스를 지원합니다.

-   하드웨어 가속 래스터화와 하드웨어 및 소프트웨어 모두의 꼭짓점 처리로 음영 적용이 가능한 hal 디바이스
-   참조 디바이스

이러한 디바이스는 별도의 두 드라이버입니다. 소프트웨어 및 참조 디바이스는 소프트웨어 드라이버로 표시되며, hal 디바이스는 하드웨어 드라이브로 표시됩니다. 이러한 디바이스의 장점을 활용하는 가장 일반적인 방법은 hal 디바이스를 배송 응용 프로그램에, 참조 디바이스를 기능 테스팅에 사용하는 것입니다. 이러한 기능은 특정 장치를 에뮬레이션하기 위해 타사가 제공합니다. 예를 들어 개발 하드웨어가 아직 릴리스되지 않은 경우입니다.

응용 프로그램이 만드는 Direct3D 디바이스는 응용 프로그램이 실행되는 하드웨어의 기능과 적절히 맞아야 합니다. Direct3D는 컴퓨터에 설치된 3D 하드웨어에 액세스하거나 소프트웨어에서 3D 하드웨어의 기능을 에뮬레이션하는 방식으로 렌더링 기능을 제공합니다. 따라서 Direct3D는 하드웨어 액세스와 소프트웨어 에뮬레이션 모두를 위한 디바이스를 제공합니다.

하드웨어 가속 디바이스는 소프트웨어 디바이스보다 훨씬 뛰어난 성능을 발휘합니다. hal 디바이스 유형은 Direct3D를 지원하는 모든 그래픽 어댑터에서 사용 가능합니다. 대부분의 경우 응용 프로그램의 대상 컴퓨터는 하드웨어 가속 기능이 있으며 저가형 컴퓨터의 경우 이를 보완하기 위해 소프트웨어 소프트웨어 에뮬레이션에 의존합니다.

참조 디바이스를 제외하고, 소프트웨어 디바이스는 하드웨어 디바이스와 항상 동일한 기능을 지원하지는 않습니다. 응용 프로그램은 항상 어떤 기능이 지원되는지 파악하기 위해 디바이스의 기능을 쿼리해야 합니다.

Direct3D 9와 함께 제공된 소프트웨어와 참조 디바이스의 동작은 hal 디바이스와 동일하므로, hal 디바이스에서 작동하도록 작성된 응용 프로그램 코드는 수정하지 않고도 소프트웨어 또는 참조 디바이스에서 작동합니다. 제공된 소프트웨어 또는 참조 디바이스의 동작은 hal 디바이스와 동일하지만, 디바이스 기능은 다를 수 있으며, 특정 소프트웨어 디바이스는 훨씬 적은 기능만 구현할 수 있습니다.

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
<td align="left"><p><a href="device-types.md">디바이스 유형</a></p></td>
<td align="left"><p>Direct3D 디바이스 유형에는 hal(하드웨어 추상화 계층) 디바이스와 기준 래스터라이저가 포함됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="windowed-vs--full-screen-mode.md">창 모드와 전체 화면 모드 비교</a></p></td>
<td align="left"><p>Direct3D 응용 프로그램은 창 모드 또는 전체 화면 모드 중 하나에서 실행될 수 있습니다. <em>창 모드</em>에서 응용 프로그램은 실행되는 모든 응용 프로그램과 사용할 수 있는 바탕 화면 공간을 공유합니다. <em>전체 화면 모드</em>에서 응용 프로그램이 실행되는 창은 전체 바탕 화면을 덮어서 개발 환경을 포함하여 실행되는 모든 응용 프로그램을 가립니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="lost-devices.md">손실 디바이스</a></p></td>
<td align="left"><p>Direct3D 디바이스는 작동 상태이거나 손실 상태일 수 있습니다. <em>작동</em> 상태는 디바이스가 실행되고 모든 렌더링이 예상대로 표시되는 디바이스의 정상 상태입니다. 전체 화면 응용 프로그램에서 키보드 포커스를 잃어 렌더링이 불가능할 경우, 디바이스는 <em>손실</em> 상태로 전환됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="swap-chains.md">스왑 체인</a></p></td>
<td align="left"><p>스왑 체인은 사용자에게 프레임을 표시하는 데 사용되는 버퍼의 컬렉션입니다. 응용 프로그램이 표시할 새 프레임을 제공할 때마다, 스왑 체인의 첫 버퍼가 표시되는 버퍼의 자리를 차지합니다. 이 프로세스를 <em>스와핑</em> 또는 <em>플리핑</em>이라고 부릅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="introduction-to-rasterization-rules.md">래스터화 규칙 소개</a></p></td>
<td align="left"><p>흔히 꼭짓점에 지정된 점은 화면의 픽셀과 정확히 일치하지는 않습니다. 이 경우, Direct3D가 삼각형 래스터화 규칙을 적용하여 지정된 삼각형에 어떤 픽셀을 적용할지 결정합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

 

 




