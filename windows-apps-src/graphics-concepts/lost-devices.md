---
title: 분실한 디바이스
description: Direct3D 장치는 작동 상태 또는 분실 상태일 수 있습니다.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- 분실한 디바이스
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d7f2c06564e0477fcec67418cae739e2fae123d7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158857"
---
# <a name="lost-devices"></a>분실한 디바이스


Direct3D 장치는 작동 상태 또는 분실 상태일 수 있습니다. *작동* 상태는 장치가 실행 되는 장치의 일반적인 상태 이며 예상 대로 모든 렌더링을 제공 합니다. 전체 화면 응용 프로그램에서 키보드 포커스가 손실 되는 등의 이벤트가 발생 하 여 렌더링을 수행할 수 없게 되 면 장치는 *분실* 상태로 전환 됩니다. 손실 상태는 모든 렌더링 작업의 자동 오류로 구분 됩니다. 즉, 렌더링 작업이 실패 하더라도 렌더링 메서드가 성공 코드를 반환할 수 있습니다.

의도적으로 장치를 분실 시킬 수 있는 전체 시나리오 집합이 지정 되지 않았습니다. 사용자가 ALT + TAB을 누르거나 시스템 대화 상자를 초기화 하는 등의 몇 가지 일반적인 예를 들면 포커스가 손실 됩니다. 전원 관리 이벤트로 인해 또는 다른 응용 프로그램에서 전체 화면 작업을 가정 하는 경우에도 장치를 잃을 수 있습니다. 또한 장치 다시 설정 하의 모든 오류는 장치를 분실 상태로 전환 합니다.

[**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) 에서 파생 되는 모든 메서드는 장치가 손실 된 후에도 작동 하도록 보장 됩니다. 장치 손실 후 각 함수에는 일반적으로 다음과 같은 세 가지 옵션이 있습니다.

-   "장치가 손실 되었습니다." 오류가 발생 하면 실패 합니다. 즉, 응용 프로그램이 예상 대로 작동 하지 않는 것을 식별할 수 있도록 장치가 손실 되었음을 인식 해야 합니다.
-   자동으로 실패 하 고 S \_ 확인 또는 기타 반환 코드를 반환 합니다. 함수가 자동으로 실패 하면 응용 프로그램은 일반적으로 "성공" 결과와 "자동 오류"의 결과를 구분할 수 없습니다.
-   반환 코드를 반환 합니다.

## <a name="span-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>분실 한 장치에 응답


분실 한 장치는 다시 설정 된 후 리소스 (비디오 메모리 리소스 포함)를 다시 만들어야 합니다. 장치를 분실 한 경우 응용 프로그램은 장치를 쿼리하여 작동 상태로 복원할 수 있는지 확인 합니다. 그렇지 않으면 장치를 복원할 수 있을 때까지 응용 프로그램이 대기 합니다.

장치를 복원할 수 있는 경우 응용 프로그램은 모든 비디오 메모리 리소스 및 스왑 체인을 제거 하 여 장치를 준비 합니다. 다시 설정 하는 것은 장치가 손실 될 때 영향을 주는 유일한 절차 이며 응용 프로그램이 장치를 분실 상태에서 작동 상태로 변경할 수 있는 유일한 방법입니다. 응용 프로그램에서 렌더링 대상 및 깊이 스텐실 화면을 비롯 하 여 할당 된 모든 리소스를 해제 하지 않는 한 Reset이 실패 합니다.

대부분의 경우 Direct3D의 빈도가 높은 호출은 장치가 손실 되었는지 여부에 대 한 정보를 반환 하지 않습니다. 응용 프로그램은 분실 한 장치에 대 한 알림을 받지 않고 렌더링 메서드를 계속 호출할 수 있습니다. 내부적으로 이러한 작업은 장치가 작동 상태로 다시 설정 될 때까지 삭제 됩니다.

## <a name="span-idlocking_operationsspanspan-idlocking_operationsspanspan-idlocking_operationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>잠금 작업


내부적으로 Direct3D는 장치가 손실 된 후 잠금 작업이 성공 하도록 충분 한 작업을 수행 합니다. 그러나 잠금 작업 중에는 비디오 메모리 리소스의 데이터가 정확 하지 않을 수 있습니다. 오류 코드가 반환 되지 않습니다. 이렇게 하면 잠금 작업 중 장치 손실에 대해 걱정 하지 않고 응용 프로그램을 작성할 수 있습니다.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>인사


리소스는 비디오 메모리를 사용할 수 있습니다. 분실 한 장치는 어댑터가 소유 하는 비디오 메모리와 연결 되지 않으므로 장치가 손실 될 때 비디오 메모리 할당을 보장할 수 없습니다. 결과적으로 모든 리소스 생성 메서드가 성공 하도록 구현 되지만 실제로는 더미 시스템 메모리만 할당 합니다. 장치의 크기를 조정 하기 전에 비디오 메모리 리소스를 모두 제거 해야 하므로 비디오 메모리를 과도 하 게 할당 하는 문제는 발생 하지 않습니다. 이러한 더미 표면에서는 응용 프로그램이 장치를 분실 한 것을 검색할 때까지 잠금 및 복사 작업이 정상적으로 작동 하는 것 처럼 보일 수 있습니다.

장치를 분실 상태에서 작동 상태로 다시 설정 하려면 먼저 모든 비디오 메모리를 해제 해야 합니다. 다른 상태 데이터는 작동 상태로 전환 되어 자동으로 삭제 됩니다.

장치 손실에 응답 하려면 단일 코드 경로로 응용 프로그램을 개발 하는 것이 좋습니다. 이 코드 경로는 시작 시 장치를 초기화 하는 데 사용 된 코드 경로와 유사할 수 있습니다.

## <a name="span-idretrieved_dataspanspan-idretrieved_dataspanspan-idretrieved_dataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>검색 한 데이터


Direct3D를 사용 하면 응용 프로그램에서 하드웨어의 단일 패스 렌더링에 대해 텍스처 및 렌더링 상태의 유효성을 검사할 수 있습니다.

Direct3D를 사용 하면 응용 프로그램에서 생성 되거나 이전에 작성 한 이미지를 비디오 메모리 리소스에서 비휘발성 시스템 메모리 리소스로 복사할 수도 있습니다. 이러한 전송의 원본 이미지는 언제 든 지 손실 될 수 있으므로, Direct3D를 사용 하면 장치가 손실 될 때 이러한 복사 작업이 실패할 수 있습니다.

장치를 분실 한 경우 기본 화면이 없으므로 복사 작업이 실패할 수 있습니다. 장치를 분실 한 경우 백 버퍼를 만들 수 없기 때문에 스왑 체인 만들기가 실패할 수도 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[디바이스](devices.md)

 

 