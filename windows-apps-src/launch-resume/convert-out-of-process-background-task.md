---
title: out of process 백그라운드 작업을 In-process 백그라운드 작업으로 포팅
description: 포그라운드 응용 프로그램 프로세스 내에서 실행 되는 in process 백그라운드 작업이 out-of-process-백그라운드 태스크를 포트입니다.
ms.date: 09/19/2018
ms.topic: article
keywords: 앱 서비스, 백그라운드 작업, uwp, windows 10
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 97dd249165877591743892a136d51e0969dd902a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601208"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>out of process 백그라운드 작업을 In-process 백그라운드 작업으로 포팅

In-process 작업에 out of process (OOP) 백그라운드 작업을 이식 하는 가장 간단한 방법은 제공 하는 것에 [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 메서드는 응용 프로그램 내에서 코드 및 시작에서 [OnBackgroundActivated ](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). 여기에 설명 된 기술을; in process 백그라운드 태스크로 OOP 백그라운드 태스크에서 shim을 만드는 방법에 대 되지 에 대 한 재작성 (또는 이식) in process 버전으로는 OOP 버전입니다.

앱에 여러 백그라운드 작업이 있는 경우 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)에서 `BackgroundActivatedEventArgs.TaskInstance.Task.Name`을 사용하여 시작되는 작업을 식별하는 방법을 보여 줍니다.

현재 백그라운드 및 포그라운드 프로세스 간에 통신 중인 경우 해당 상태 관리 및 통신 코드를 제거할 수 있습니다.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>변환할 수 없는 백그라운드 작업 및 트리거 유형

* In-process 백그라운드 작업은 VoIP 백그라운드 작업 활성화를 지원하지 않습니다.
* 프로세스에서 백그라운드 작업 트리거를 지원 하지 않습니다.  [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)하십시오 [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 고 **IoTStartupTask**
