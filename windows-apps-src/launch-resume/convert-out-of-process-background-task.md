---
author: TylerMSFT
title: "Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환"
description: "Out-of-process 백그라운드 작업을 포그라운드 앱 프로세스 내에서 실행되는 In-process 백그라운드 작업으로 변환합니다."
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: b361a558ecef2030370590eedbef69bd04cf68bd

---

# Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환

Out-of-process 백그라운드 작업을 In-process 작업으로 변환하는 가장 간단한 방법은 응용 프로그램 내에서 [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 메서드 코드를 가져와 [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)에서 시작하는 것입니다.

앱에 여러 백그라운드 작업이 있는 경우 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)에서 `BackgroundActivatedEventArgs.TaskInstance.Task.Name`을 사용하여 시작되는 작업을 식별하는 방법을 보여 줍니다.

현재 백그라운드 및 포그라운드 프로세스 간에 통신 중인 경우 해당 상태 관리 및 통신 코드를 제거할 수 있습니다.

## 변환할 수 없는 백그라운드 작업 및 트리거 유형

* In-process 백그라운드 작업은 VoIP 백그라운드 작업 활성화를 지원하지 않습니다.
* In-process 백그라운드 작업은 [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx), **IoTStartupTask** 등의 트리거를 지원하지 않습니다.



<!--HONumber=Nov16_HO1-->


