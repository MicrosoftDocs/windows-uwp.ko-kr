---
author: TylerMSFT
title: out of process 백그라운드 작업을 In-process 백그라운드 작업으로 포팅
description: Out of process 백그라운드 작업을 포그라운드 앱 프로세스 내에서 실행 되는 in-process 백그라운드 작업에 포팅 합니다.
ms.author: twhitney
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업, 앱 서비스
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: b9010f82b0460bd46757bc1e0d58c01dec459104
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4621186"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>out of process 백그라운드 작업을 In-process 백그라운드 작업으로 포팅

Out of process (OOP) 백그라운드 작업 in-process 활동에 포트 하는 가장 간단한 방법은 응용 프로그램을 내 [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 메서드 코드를 가져와서 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)에서 시작 하는 것입니다. 여기서 설명 하 고 기술을 OOP 백그라운드 작업에서을 in-process 백그라운드 작업; shim를 만드는 방법에 대 않습니다. 그에 대 한 다시 (또는 포팅) OOP 버전일 프로세스의 버전입니다.

앱에 여러 백그라운드 작업이 있는 경우 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)에서 `BackgroundActivatedEventArgs.TaskInstance.Task.Name`을 사용하여 시작되는 작업을 식별하는 방법을 보여 줍니다.

현재 백그라운드 및 포그라운드 프로세스 간에 통신 중인 경우 해당 상태 관리 및 통신 코드를 제거할 수 있습니다.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>변환할 수 없는 백그라운드 작업 및 트리거 유형

* In-process 백그라운드 작업은 VoIP 백그라운드 작업 활성화를 지원하지 않습니다.
* In-process 백그라운드 작업은 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx), **IoTStartupTask** 등의 트리거를 지원하지 않습니다.
