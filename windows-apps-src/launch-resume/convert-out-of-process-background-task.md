---
author: TylerMSFT
title: Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환
description: Out-of-process 백그라운드 작업을 포그라운드 앱 프로세스 내에서 실행되는 In-process 백그라운드 작업으로 변환합니다.
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업, 응용 프로그램 서비스
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 1144443f943f134991d050dea1457f252eaaf36d
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2882767"
---
# <a name="convert-an-out-of-process-background-task-to-an-in-process-background-task"></a>Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환

Out-of-process 백그라운드 작업을 In-process 작업으로 변환하는 가장 간단한 방법은 응용 프로그램 내에서 [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 메서드 코드를 가져와 [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)에서 시작하는 것입니다.

앱에 여러 백그라운드 작업이 있는 경우 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)에서 `BackgroundActivatedEventArgs.TaskInstance.Task.Name`을 사용하여 시작되는 작업을 식별하는 방법을 보여 줍니다.

현재 백그라운드 및 포그라운드 프로세스 간에 통신 중인 경우 해당 상태 관리 및 통신 코드를 제거할 수 있습니다.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>변환할 수 없는 백그라운드 작업 및 트리거 유형

* In-process 백그라운드 작업은 VoIP 백그라운드 작업 활성화를 지원하지 않습니다.
* In-process 백그라운드 작업은 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx), **IoTStartupTask** 등의 트리거를 지원하지 않습니다.
