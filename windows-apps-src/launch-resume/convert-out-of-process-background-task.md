---
author: TylerMSFT
title: 포트 처리 중인 백그라운드 작업에서 작업 중이 아닌 백그라운드 작업
description: 포트는 작업 중이 아닌 백그라운드 작업을 전경 응용 프로그램 프로세스 내에서 실행 되는 프로그램에서 백그라운드 작업을 확인 하십시오.
ms.author: twhitney
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업, 응용 프로그램 서비스
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: b9010f82b0460bd46757bc1e0d58c01dec459104
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4114419"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>포트 처리 중인 백그라운드 작업에서 작업 중이 아닌 백그라운드 작업

독립 프로세스 (OOP) 백그라운드 작업 프로세스 활동에 이식 하는 가장 간단한 방법은 [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 메서드 코드 내 응용 프로그램을 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)에서 시작 하는 것입니다. 여기에서 설명 하는 기술을 처리 중인 백그라운드 작업; OOP 백그라운드 작업에서 shim을 만드는 방법에 대해 않습니다. 그에 대 한 다시 작성 (또는 이식) 프로세스 버전으로는 OOP 버전.

앱에 여러 백그라운드 작업이 있는 경우 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)에서 `BackgroundActivatedEventArgs.TaskInstance.Task.Name`을 사용하여 시작되는 작업을 식별하는 방법을 보여 줍니다.

현재 백그라운드 및 포그라운드 프로세스 간에 통신 중인 경우 해당 상태 관리 및 통신 코드를 제거할 수 있습니다.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>변환할 수 없는 백그라운드 작업 및 트리거 유형

* In-process 백그라운드 작업은 VoIP 백그라운드 작업 활성화를 지원하지 않습니다.
* In-process 백그라운드 작업은 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx), **IoTStartupTask** 등의 트리거를 지원하지 않습니다.
