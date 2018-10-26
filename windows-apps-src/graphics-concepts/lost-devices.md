---
title: 분실 장치
description: Direct3D 장치가 작동 상태이거나 분실된 상태일 수 있습니다.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- 분실 장치
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 92dc9437dc2417b8f3da99df7cd6d6eb0c8edd1b
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5594750"
---
# <a name="lost-devices"></a>분실 장치


Direct3D 디바이스는 작동 상태이거나 손실 상태일 수 있습니다. *작동* 상태는 디바이스가 실행되고 모든 렌더링이 예상대로 표시되는 디바이스의 정상 상태입니다. 전체 화면 응용 프로그램에서 키보드 포커스를 잃어 렌더링이 불가능할 경우, 장치는 *손실* 상태로 전환됩니다. 분실된 상태의 특징은 모든 렌더링 작업의 자동 실패입니다. 즉 렌더링 작업이 실패하더라도 렌더링 메서드가 성공 코드를 반환할 수 있습니다.

설계상 장치 분실을 일으킬 가능성이 있는 전체 시나리오 집합은 지정되지 않았습니다. 사용자가 ALT+TAB을 누르거나 시스템 대화 상자가 초기화된 경우 등 포커스를 잃어버리는 경우가 대표적인 예입니다. 전원 관리 이벤트가 발생하거나 다른 응용 프로그램이 전체 화면 작업을 가정할 때도 장치를 분실할 수 있습니다. 또한 장치 재설정으로 인한 오류 발생 시 장치가 분실 상태가 됩니다.

[**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)에서 파생된 모든 메서드는 장치 분실 후에도 작동합니다. 장치 분실 후 각 기능에는 일반적으로 다음 세 가지 옵션이 있습니다.

-   "장치 분실" 오류로 실패 - 즉 응용 프로그램이 장치 분실을 인식하여 예상 대로 작동하지 않음을 파악해야 한다는 뜻입니다.
-   S\_OK 또는 다른 반환 코드를 반환하는 자동 실패 - 기능이 자동으로 실패하면 응용 프로그램은 일반적으로 "성공"과 "자동 실패"의 결과를 구분할 수 없습니다.
-   반환 코드를 반환합니다.

## <a name="span-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>장치 분실에 응답


분실된 장치가 재설정된 후 리소스(비디오 메모리 리소스 포함)를 다시 만들어야 합니다. 장치가 분실되면 응용 프로그램은 작동 상태로 복원할 수 있는지 확인하기 위해 장치에 쿼리합니다. 그렇지 않으면 장치를 복원할 수 있을 때까지 응용 프로그램이 기다립니다.

장치를 복원할 수 있는 경우 응용 프로그램이 모든 비디오 메모리 리소스와 스왑 체인을 파괴하여 장치를 준비시킵니다. 재설정은 장치 분실 시 효과가 있는 유일한 절차이며, 응용 프로그램이 장치를 분실 상태에서 작동 상태로 변경할 수 있는 유일한 방법입니다. 렌더링 대상과 깊이 스텐실 표면을 포함해 할당된 모든 리소스를 응용 프로그램이 해제하지 않는 한 재설정에 실패합니다.

대부분의 경우, Direct3D의 고주파수 호출은 장치의 분실 여부에 대한 정보를 반환하지 않습니다. 응용 프로그램은 분실 장치 알림을 받지 않고 렌더링 메서드를 계속 호출할 수 있습니다. 내부적으로 이러한 작업은 장치가 작동 상태로 재설정될 때까지 삭제됩니다.

## <a name="span-idlockingoperationsspanspan-idlockingoperationsspanspan-idlockingoperationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>잠금 작업


내부적으로 Direct3D는 장치 분실 후 잠금 작업이 성공하기에 충분한 작업을 수행합니다. 그러나 잠금 작업 중에 비디오 메모리 리소스의 데이터가 정확하다고 보장하지 않습니다. 오류 코드가 반환되지 않도록 보장합니다. 그러면 잠금 작업 중 장치 분실에 대한 우려 없이 응용 프로그램을 작성할 수 있습니다.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>리소스


리소스는 비디오 메모리를 소모할 수 있습니다. 분실 장치가 어댑터 소유의 비디오 메모리에서 분리되었기 때문에 장치 분실 시 비디오 메모리의 할당을 보장할 수 없습니다. 그 결과 모든 리소스 생성 메서드가 성공하도록 구현되지만 실제로는 더미 시스템 메모리만 할당합니다. 장치 크기를 조정하기 전에 비디오 메모리 리소스를 삭제해야 하기 때문에 비디오 메모리의 과도 할당은 발생하지 않습니다. 이 더미 표면을 사용하면 응용 프로그램이 장치 분실을 발견할 때까지 잠금 및 복사 작업이 정상적으로 작동하는 것처럼 보일 수 있습니다.

장치를 분실 상태에서 작동 상태로 재설정하기 전에 모든 비디오 메모리를 해제해야 합니다. 작동 상태로 전환하면서 다른 상태 데이터는 자동으로 삭제됩니다.

장치 분실에 응답하기 위해 단일 코드 경로로 응용 프로그램을 개발하는 것이 좋습니다. 이 코드 경로는 시작 시 장치를 초기화하는 데 사용된 코드 경로와 동일하거나 비슷할 가능성이 높습니다.

## <a name="span-idretrieveddataspanspan-idretrieveddataspanspan-idretrieveddataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>검색된 데이터


Direct3D를 통해 응용 프로그램이 하드웨어의 단일 패스 렌더링을 기준으로 텍스처와 렌더링 상태의 유효성을 검사할 수 있습니다.

Direct3D를 통해 응용 프로그램은 이전에 작성했거나 만든 이미지를 비디오 메모리 리소스에서 비휘발성 시스템 메모리 리소스로 복사할 수도 있습니다. 이러한 전달의 소스 이미지는 언제든 분실될 수 있기 때문에, Direct3D는 장치 분실 시 이러한 복사 작업이 실패하도록 합니다.

장치 분실 시 기본 표면이 없기 때문에 복사 작업에 실패할 수 있습니다. 장치가 분실되면 백 버퍼도 만들 수 없으므로 스왑 체인 만들기 역시 실패할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[장치](devices.md)

 

 




