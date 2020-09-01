---
title: Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 포팅
description: 포그라운드 앱 프로세스 내에서 실행 되는 in-process 백그라운드 작업에 대해 out-of-process 백그라운드 작업을 이식 합니다.
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업, app service
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: f500be50c8b57415f5ad2fe62c28cc21f4b57b68
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162657"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 포팅

OOP (out-of-process) 백그라운드 작업을 in-process 작업으로 이식 하는 가장 간단한 방법은 응용 프로그램 내에서 IBackgroundTask 메서드 코드를 [실행](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) 하 고 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)에서 시작 하는 것입니다. 여기에 설명 된 기술에서는 OOP 백그라운드 작업에서 in-process 백그라운드 작업으로의 shim을 만드는 방법에 대해서는 설명 하지 않습니다. OOP 버전을 in-process 버전으로 다시 작성 (또는 포팅) 하는 것이 좋습니다.

앱에 여러 백그라운드 작업이 있는 경우 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) 에서는를 사용 하 여 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 어떤 작업이 시작 되는지 식별 하는 방법을 보여 줍니다.

현재 백그라운드 프로세스와 포그라운드 프로세스 간에 통신 하는 경우 해당 상태 관리 및 통신 코드를 제거할 수 있습니다.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>변환할 수 없는 백그라운드 작업 및 트리거 유형

* In-process 백그라운드 작업은 VoIP 백그라운드 작업을 활성화 하는 것을 지원 하지 않습니다.
* In-process 백그라운드 작업은 [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) 및 **iotstartuptask** 트리거를 지원 하지 않습니다.