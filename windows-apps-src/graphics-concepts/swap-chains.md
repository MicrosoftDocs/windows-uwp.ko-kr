---
title: 스왑 체인
description: 스왑 체인은 사용자에게 프레임을 표시하는 데 사용되는 버퍼의 컬렉션입니다.
ms.assetid: A38E8BB7-1E77-4D93-B321-D3572A80D5DD
keywords:
- 스왑 체인
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b24b50c18716f14c58244e52bbb77668760b042
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1045042"
---
# <a name="swap-chains"></a>스왑 체인


스왑 체인은 사용자에게 프레임을 표시하는 데 사용되는 버퍼의 컬렉션입니다. 응용 프로그램이 표시할 새 프레임을 제공할 때마다, 스왑 체인의 첫 버퍼가 표시되는 버퍼의 자리를 차지합니다. 이 프로세스를 *스와핑* 또는 *플리핑*이라고 부릅니다.

그래픽 어댑터는 모니터에 표시되는 이미지를 나타내는 표면(전면 버퍼)에 포인터를 유지합니다. 모니터를 새로 고칠 때 그래픽 카드가 전면 버퍼의 내용을 표시될 모니터로 보냅니다. 하지만 이로 인해 실시간 그래픽을 렌더링할 때 "화면 잘림(tearing)" 문제가 발생합니다. 문제의 핵심은 모니터 새로 고침 빈도가 나머지 컴퓨터 작업에 비해 매우 느리다는 것입니다. 일반적인 새로 고침 빈도는 60Hz(초당 60회)에서 100Hz 사이입니다.

모니터 새로 고침 도중에 응용 프로그램이 전면 버퍼를 업데이트할 경우 표시되는 이미지가 절반으로 잘려 디스플레이 상반부에는 이전 이미지가, 하반부에는 새 이미지가 표시됩니다. 이 문제를 *화면 잘림(tearing)* 이라고 합니다.

## <a name="span-idavoidingtearingspanspan-idavoidingtearingspanspan-idavoidingtearingspanavoiding-tearing"></a><span id="Avoiding_tearing"></span><span id="avoiding_tearing"></span><span id="AVOIDING_TEARING"></span>화면 잘림 방지


Direct3D는 화면 잘림을 방지하기 위한 두 가지 옵션을 제공합니다.

-   첫 번째 옵션은 수직 리트레이스(수직 동기화) 작업 시에만 모니터 업데이트를 허용합니다. 일반적으로 모니터는 라이트 핀을 모니터 왼쪽 상단에서 오른쪽 하단까지 지그재그 방식으로 수평 이동하여 이미지를 새로 고칩니다. 라이트 핀이 하단에 도달하면 모니터가 라이트 핀을 다시 왼쪽 상단으로 이동하여 재보정합니다. 이렇게 프로세스가 다시 시작됩니다.

    이 로캘이나 수직 동기화를 라고 합니다. 세로 동기화 하는 동안 모니터는 하지 드로잉 아무것도, 되므로 프런트엔드 버퍼에 업데이트를 적용 모니터 시작을 다시 그릴 때까지 표시 되지 않습니다. 수직 동기화는 비교적 느리지만 대기하는 도중 복잡한 장면을 렌더링할 수 있을 정도로 느리지는 않습니다. 화면 잘림을 방지하고 복잡한 장면을 렌더링할 수 있기 위해 필요한 것이 백 버퍼링이라고 하는 프로세스입니다.

-   두 번째 옵션은 백 버퍼링이라는 기법을 사용합니다. 백 버퍼링은 장면은 화면 외부 표면, 즉 백 버퍼에 그리는 프로세스입니다. 전면 버퍼 이외의 모든 표면은 절대로 모니터에 직접 표시되지 않으므로 화면 외부 표면이라고 부릅니다.

    응용 프로그램은 백 버퍼를 사용하여 시스템이 유휴 상태일(즉 대기 중인 Windows 메시지가 없을) 때마다 모니터의 새로 고침 빈도를 고려할 필요 없이 자유롭게 장면을 렌더링할 수 있습니다. 백 버퍼링은 언제, 어떻게 백 버퍼를 전면 버퍼로 이동하는가라는 문제를 추가로 제기합니다.

## <a name="span-idsurfaceflippingspanspan-idsurfaceflippingspanspan-idsurfaceflippingspansurface-flipping"></a><span id="Surface_flipping"></span><span id="surface_flipping"></span><span id="SURFACE_FLIPPING"></span>표면 플리핑


백 버퍼가 전면 버퍼로 이동하는 프로세스를 표면 플리핑이라고 합니다. 그래픽 카드는 단순히 표면에 대한 포인터를 사용하여 전면 버퍼를 나타내므로 간단한 포인터 변경이 백 버퍼를 전면 버퍼로 설정하는 데 필요한 전부입니다. 응용 프로그램이 Direct3D에게 백 버퍼를 전면 버퍼로 전환하도록 요청하면 Direct3D는 두 포인터를 "플립"하기만 합니다. 그러면 이제 백 버퍼가 새로운 전면 버퍼가 되고, 이전의 전면 버퍼는 새로운 백 버퍼가 됩니다.

응용 프로그램이 Direct3D 장치에 백 버퍼를 표시하도록 요청할 때마다 표면 플립이 호출됩니다. 하지만 수직 동기화가 발생할 때까지 요청을 큐에 넣도록 Direct3D를 설정할 수 있습니다. 이 옵션을 Direct3D 장치의 표시 간격이라고 합니다. 응용 프로그램이 어떻게 Direct3D에서 화면 플리핑을 처리하도록 지정했는지에 따라 새 백 버퍼의 데이터를 재사용하지 못할 수 있습니다.

화면 플리핑은 멀티미디어, 애니메이션, 게임 소프트웨어에서 매우 중요합니다. 애니메이션에서 종이 패드를 사용하는 것과 동일합니다. 각 페이지에서 아티스트는 그림을 약간씩 변경합니다. 그러면 각 페이지를 빠르게 넘길 때 그림이 애니메이션화되어 보입니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[장치](devices.md)

 

 



