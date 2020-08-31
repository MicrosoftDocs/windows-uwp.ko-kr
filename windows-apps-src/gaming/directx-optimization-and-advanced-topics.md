---
title: DirectX 게임에 대 한 최적화 및 고급 항목
description: DirectX 게임 성능 및 기타 고급 항목을 최적화 하는 방법에 대 한 정보를 제공 하는 문서를 참조 하세요.
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 최적화, 다중 샘플링, 스왑 체인
ms.localizationpriority: medium
ms.openlocfilehash: f19081a2618ea683c7790db3cd7517078f19269c
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053653"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>DirectX 게임에 대 한 최적화 및 고급 항목

이 섹션에서는 DirectX 게임 성능 및 기타 고급 항목을 최적화 하는 방법에 대 한 정보를 제공 합니다.

게임의 비동기 프로그래밍 항목에서는 비동기 프로그래밍을 사용 하 여 일부 구성 요소를 병렬화 하 고 다중 스레딩을 사용 하 여 강력한 GPU의 사용을 극대화 하려는 경우 고려해 야 할 여러 가지 사항을 설명 합니다.

Direct3D 11에서 장치 제거 시나리오 처리 항목에서는 연습을 사용 하 여 Direct3D 11을 사용 하 여 개발 된 게임이 그래픽 어댑터를 다시 설정, 제거 또는 변경 하는 상황을 감지 하 고 대응할 수 있는 방식을 설명 합니다.

UWP 앱의 다중 샘플링 항목에서는 Direct3D를 사용 하 여 빌드된 UWP 게임에서 별칭이 지정 된 가장자리의 모양을 줄이는 그래픽 기술인 다중 샘플 앤티 앨리어싱을 사용 하는 방법에 대 한 개요를 제공 합니다.

입력 및 렌더링 루프 최적화 항목에서는 입력 대기 시간을 관리 하 고 렌더링 루프를 최적화 하기 위한 올바른 입력 이벤트 처리 옵션을 선택 하는 방법에 대 한 지침을 제공 합니다.

DXGI 1.3 스왑 체인을 사용 하 여 대기 시간 감소 토픽은 스왑 체인이 새 프레임 렌더링을 시작할 적절 한 시간에 신호를 보낼 때까지 대기 하 여 효과적인 프레임 대기 시간을 줄이는 방법을 설명 합니다.

스왑 체인 크기 조정 및 오버레이 항목에서는 크기 조정 된 스왑 체인을 사용 하 여 디스플레이가 기본적으로 지원 되는 것 보다 낮은 해상도로 실시간 게임 콘텐츠를 렌더링 하 여 렌더링 시간을 향상 시키는 방법에 대해 설명 합니다. 하드웨어 오버레이 기능이 있는 장치에 대해 오버레이 스왑 체인을 만드는 방법에 대해서도 설명 합니다. 이 기술은 스왑 체인 크기 조정을 사용 하 여 발생 하는 축소 된 UI의 문제를 완화 하는 데 사용할 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">게임에 대 한 비동기 프로그래밍</a></p></td>
<td align="left"><p>DirectX를 사용한 비동기 프로그래밍 및 스레딩을 이해 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">Direct3D 11에서 장치 제거 시나리오 처리</a></p></td>
<td align="left"><p>그래픽 어댑터를 제거 하거나 다시 초기화할 때 Direct3D 및 DXGI 장치 인터페이스 체인을 다시 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">UWP 앱의 다중 샘플링</a></p></td>
<td align="left"><p>Direct3D를 사용 하 여 빌드된 UWP 게임에서 다중 샘플링을 사용 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">입력 및 렌더링 루프 최적화</a></p></td>
<td align="left"><p>입력 대기 시간을 줄이고 렌더링 루프를 최적화 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">DXGI 1.3 스왑 체인으로 대기 시간 단축</a></p></td>
<td align="left"><p>유효한 프레임 대기 시간을 줄이려면 DXGI 1.3를 사용 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">스왑 체인 확장 및 오버레이</a></p></td>
<td align="left"><p>크기 조정 된 스왑 체인을 만들어 모바일 장치에서 보다 빠르게 렌더링 하 고 오버레이 스왑 체인을 사용 하 여 시각적 품질을 향상 시킵니다.</p></td>
</tr>
</tbody>
</table>