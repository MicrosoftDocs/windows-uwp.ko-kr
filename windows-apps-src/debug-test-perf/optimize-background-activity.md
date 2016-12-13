---
author: PatrickFarley
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: "배터리 절약 기능 활용"
description: "시스템과 연동하여 배터리에 효율적인 방식으로 백그라운드 작업을 사용하는 UWP 앱을 만듭니다."
translationtype: Human Translation
ms.sourcegitcommit: 73b19e54b863693aece045e5b653bc0583a676bb
ms.openlocfilehash: 854ec43d075f8adc1f875d3b9e5e2d818434edb9

---

# <a name="optimize-background-activity"></a>백그라운드 작업 최적화

유니버설 Windows 앱은 모든 디바이스 패밀리에서 일관된 성능으로 작동해야 합니다. 배터리 전원을 사용하는 디바이스에서는 전원 소비가 사용자의 전반적인 앱 환경에서 중요한 요소입니다. 배터리 사용 시간이 하루 종일 유지되는 것은 모든 사용자에게 유용한 기능이지만 해당 앱을 포함하여 디바이스에 설치된 모든 소프트웨어의 효율성이 필요합니다. 

백그라운드 작업 동작은 앱의 총 에너지 비용에서 가장 큰 요소입니다. 백그라운드 작업은 앱을 열지 않고 실행되도록 시스템에 등록된 모든 프로그램 활동입니다. 자세한 내용은 [out-of-process 백그라운드 작업 만들기 및 등록](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task)을 참조하세요.

## <a name="background-activity-allowance"></a>백그라운드 작업 사용 가능 시간

Windows 10 버전 1607부터 사용자는 설정 앱의 **배터리** 섹션에서 "앱에 의한 배터리 사용"을 볼 수 있습니다. 여기에 앱 목록과 각 앱이 사용한 배터리 사용 시간의 비율(마지막 충전 이후 사용된 배터리 사용 시간 중)이 표시됩니다. 

![앱에 의한 배터리 사용](images/battery-usage-by-app.png)

이 목록에 있는 UWP 앱에 대해 사용자는 시스템에서 백그라운드 작업을 처리하는 방식을 일부 제어할 수 있습니다. 백그라운드 작업을 "항상 허용됨", "Windows에서 관리됨"(기본 설정) 또는 "허용되지 않음"으로 지정할 수 있습니다(자세한 내용은 아래 참조). [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) 메서드에서 반환된 **BackgroundAccessStatus** 열거형 값을 사용하여 앱의 백그라운드 작업 사용 가능 시간을 확인할 수 있습니다.

![백그라운드 작업 사용 권한](images/background-task-permissions.png)

앱에서 책임 있는 백그라운드 작업 관리를 구현하지 않을 경우 사용자가 앱에 대해 백그라운드 사용 권한을 완전히 거부할 수 있으며, 이는 양쪽에 모두 바람직하지 않습니다.

## <a name="work-with-the-battery-saver-feature"></a>배터리 절약 모드 기능 사용
배터리 절약 모드는 사용자가 설정에서 구성할 수 있는 시스템 수준의 기능입니다. 배터리 잔량이 사용자가 정의한 임계값 아래로 내려가면 "항상 허용됨"으로 설정된 앱의 백그라운드 작업을 *제외하고* 모든 앱의 백그라운드 작업이 모두 중단됩니다.

앱이 "Windows에서 관리됨"으로 표시되는 경우 배터리 절약 모드가 켜져 있는 동안 **BackgroundExecutionManager.RequestAccessAsync()**를 호출하여 백그라운드 작업을 등록하면 **DeniedSubjectToSystemPolicy** 값이 반환됩니다. 앱은 이 문제를 처리하기 위해 배터리 절약 모드가 해제되고 백그라운드 작업이 시스템에 다시 등록될 때까지 지정된 백그라운드 작업이 실행되지 않음을 사용자에게 알려야 합니다. 백그라운드 작업이 실행되도록 이미 등록되었으며 트리거 시 배터리 절약 모드가 켜져 있으면 작업이 실행되지 않고 사용자가 알림을 받지도 않습니다. 이 문제가 발생할 가능성을 줄이려면 앱이 열릴 때마다 해당 백그라운드 작업을 다시 등록하도록 프로그래밍하는 것이 좋습니다.

백그라운드 작업 관리가 배터리 절약 모드의 주요 목적이지만 배터리 절약 모드가 켜져 있는 동안 에너지를 더 절약하기 위해 앱에서 추가로 조정할 수 있습니다. [**PowerManager.PowerSavingMode**](https://msdn.microsoft.com/library/windows/apps/windows.phone.system.power.powermanager.powersavingmode.aspx) 속성을 참조하여 앱 내에서 배터리 절약 모드의 상태를 확인합니다. 상태는 열거형 값인 **PowerSavingMode.Off** 또는 **PowerSavingMode.On**입니다. 배터리 절약 모드가 켜져 있는 경우 앱에서 애니메이션 사용을 줄이거나, 위치 폴링을 중지하거나, 동기화 및 백업을 연기할 수 있습니다. 

## <a name="further-optimize-background-tasks"></a>추가 백그라운드 작업 최적화
다음은 백그라운드 작업을 등록할 때 배터리 인식을 향상하기 위해 수행할 수 있는 추가 단계입니다.

유지 관리 트리거를 사용합니다. [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) 개체 대신 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) 개체를 사용하여 백그라운드 작업이 시작되는 시기를 결정할 수 있습니다. 유지 관리 트리거를 사용하는 작업은 디바이스가 AC 전원에 연결된 경우에만 실행되며, 더 오래 실행할 수 있습니다. 지침은 [유지 관리 트리거 사용](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger)을 참조하세요.

**BackgroundWorkCostNotHigh** 시스템 조건 유형을 사용합니다. 백그라운드 작업을 실행하려면 시스템 조건을 충족해야 합니다(자세한 내용은 [백그라운드 작업 실행 조건 설정](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task) 참조). 백그라운드 작업 비용은 백그라운드 작업 실행이 에너지에 미치는 *상대적인* 영향을 나타내는 측정값입니다. 디바이스가 AC 전원에 연결되어 있을 때 실행 중인 작업은 **낮음**(배터리에 미치는 영향이 거의/전혀 없음)으로 표시됩니다. 화면이 꺼진 상태로 디바이스가 배터리 전원을 사용할 때 실행 중인 작업은 당시에 디바이스에서 실행 중인 프로그램 활동이 거의 없어서 백그라운드 작업의 상대적인 비용이 커지기 때문에 **높음**으로 표시됩니다. 화면이 *켜진* 상태로 디바이스가 배터리 전원을 사용할 때 실행 중인 작업은 이미 실행 중인 일부 프로그램 활동이 있고 백그라운드 작업으로 인해 에너지 비용이 약간 더 추가되므로 **중간**으로 표시됩니다. **BackgroundWorkCostNotHigh** 시스템 조건은 단순히 화면이 켜지거나 디바이스가 AC 전원에 연결될 때까지 작업 실행을 연기합니다.

## <a name="test-battery-efficiency"></a>배터리 효율성 테스트

높은 전원 소비 시나리오에 대해 실제 디바이스에서 앱을 테스트해야 합니다. 여러 디바이스와 다양한 네트워크 강도의 환경에서 배터리 절약 모드를 켜거나 끄고 앱을 테스트하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목

* [out-of-process 백그라운드 작업 만들기 및 등록](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task)  
* [성능 계획](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  




<!--HONumber=Dec16_HO1-->


