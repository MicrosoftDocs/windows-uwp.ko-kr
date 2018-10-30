---
author: joannaleecy
title: DirectX 게임에 대한 최적화 및 고급 항목
description: DirectX 게임 프로그래밍에 대한 최적화 및 고급 항목입니다.
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 최적화, 다중 샘플링, 스왑 체인
ms.localizationpriority: medium
ms.openlocfilehash: e1a9b16dcf8c40c2b1db4af172d97009563e677a
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5764404"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>DirectX 게임에 대한 최적화 및 고급 항목

이 섹션에서는 DirectX 게임 성능 최적화 및 기타 고급 항목에 대한 정보를 제공합니다.

게임용 비동기 프로그래밍 항목에서는 비동기 프로그래밍을 사용하여 일부 구성 요소를 병렬화하고 다중 스레드를 사용하여 강력한 GPU 사용을 극대화하려는 경우에 고려해야 할 다양한 사항을 설명합니다.

Direct3D 11에서 디바이스 제거 시나리오 처리 항목에서는 Direct3D 11을 사용하여 개발된 게임이 그래픽 어댑터의 재설정, 제거 또는 변경 상황을 검색하고 이에 대응하는 방법에 대해 설명하는 연습을 사용합니다.

UWP 앱의 다중 샘플링 항목에서는 다중 샘플 앤티엘리어싱이라는 그래픽 기법을 사용하여 Direct3D로 빌드된 UWP 게임의 울퉁불퉁한 가장자리를 다듬는 방법에 대해 간략하게 설명합니다.

입력 및 렌더링 루프 최적화 항목에서는 적절한 입력 이벤트 처리 옵션을 선택하여 입력 대기 시간을 관리하고 렌더링 루프를 최적화하는 방법에 지침을 제공합니다.

DXGI 1.3 스왑 체인으로 대기 시간 단축 항목에서는 스왑 체인이 새 프레임 렌더링을 시작할 적절한 시간을 신호로 보낼 때까지 기다려 유효 프레임 대기 시간을 단축하는 방법을 설명합니다.

스왑 체인 크기 조정 및 오버레이 항목에서는 크기 조정된 스왑 체인을 사용하여 디스플레이에서 기본적으로 지원하는 것보다 낮은 해상도로 실시간 게임 콘텐츠를 렌더링하여 렌더링 시간을 개선하는 방법을 설명합니다. 또한 하드웨어 오버레이 기능이 있는 디바이스를 위한 오버레이 스왑 체인을 만드는 방법도 설명합니다. 이 기법은 스왑 체인 크기 조정 사용으로 인한 UI 크기 축소 문제를 완화하는 데 사용할 수 있습니다.

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
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">게임용 비동기 프로그래밍</a></p></td>
<td align="left"><p>DirectX를 사용한 비동기 프로그래밍과 스레드를 이해합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">Direct3D 11에서 디바이스 제거 시나리오 처리</a></p></td>
<td align="left"><p>그래픽 어댑터가 제거되거나 다시 초기화될 때 Direct3D 및 DXGI 디바이스 인터페이스 체인을 다시 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">UWP 앱의 다중 샘플링</a></p></td>
<td align="left"><p>Direct3D를 사용하여 빌드된 UWP 게임에서 다중 샘플링을 사용합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">입력 및 렌더링 루프 최적화</a></p></td>
<td align="left"><p>입력 대기 시간을 단축하고 렌더링 루프를 최적화합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">DXGI 1.3 스왑 체인으로 대기 시간 단축</a></p></td>
<td align="left"><p>DXGI 1.3을 사용하여 유효 프레임 대기 시간을 단축합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">스왑 체인 크기 조정 및 오버레이</a></p></td>
<td align="left"><p>모바일 디바이스에서 더 빠른 렌더링을 위해 크기 조정된 스왑 체인을 만들고 오버레이 스왑 체인을 사용하여 시각적 품질을 개선합니다.</p></td>
</tr>
</tbody>
</table>