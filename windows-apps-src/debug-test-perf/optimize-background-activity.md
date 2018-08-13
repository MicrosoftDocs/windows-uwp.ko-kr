---
author: jwmsft
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: 백그라운드 작업 최적화
description: 시스템과 연동하여 배터리에 효율적인 방식으로 백그라운드 작업을 사용하는 UWP 앱을 만듭니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 630f280d54c29aa80bc310bd88ceddd76b07f8bf
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/14/2017
ms.locfileid: "894657"
---
# <a name="optimize-background-activity"></a>백그라운드 작업 최적화

유니버설 Windows 앱은 모든 디바이스 패밀리에서 일관된 성능으로 작동해야 합니다. 배터리 전원을 사용하는 디바이스에서는 전원 소비가 사용자의 전반적인 앱 환경에서 중요한 요소입니다. 배터리 사용 시간이 하루 종일 유지되는 것은 모든 사용자에게 유용한 기능이지만 해당 앱을 포함하여 디바이스에 설치된 모든 소프트웨어의 효율성이 필요합니다. 

백그라운드 작업 동작은 앱의 총 에너지 비용에서 가장 중요한 요소입니다. 백그라운드 작업은 앱을 열지 않고 실행되도록 시스템에 등록된 모든 프로그램 활동입니다. 자세한 내용은 [Out-of-process 백그라운드 작업 만들기 및 등록](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)을 참조하세요.

## <a name="background-activity-permissions"></a>백그라운드 작업 사용 권한

Windows 10 버전 1607 이상을 실행 중인 데스크톱 및 모바일 디바이스 사용자는 설정 앱의 배터리 섹션에서 "앱에 의한 배터리 사용"을 확인할 수 있습니다. 여기에 앱 목록과 각 앱이 소비한 배터리 사용 시간의 비율(마지막 충전 이후 사용된 배터리 사용 시간 중)이 표시됩니다. 이 목록에 있는 UWP 앱의 경우 사용자는 앱을 선택하여 백그라운드 작업과 관련된 컨트롤을 열 수 있습니다.

![앱에 의한 배터리 사용](images/battery-usage-by-app.png)

### <a name="background-permissions-on-mobile"></a>휴대폰에서 백그라운드 사용 권한

모바일 디바이스에서 사용자에게 해당 앱에 대한 백그라운드 작업 권한 설정을 지정하는 라디오 단추 목록이 표시됩니다. 백그라운드 작업은 "항상 허용됨", "허용되지 않음", 또는 "Windows에서 관리됨"으로 설정할 수 있기 때문에 앱의 백그라운드 작업이 다양한 요인에 따라 시스템에 의해 규제됩니다. 

![백그라운드 작업 권한 라디오 단추](images/background-task-permissions.png)

### <a name="background-permissions-on-desktop"></a>데스크톱에서 백그라운드 사용 권한

데스크톱 디바이스에서 "Windows에서 관리됨" 설정이 켜기/끄기 스위치로 제공되며 기본 설정은 **켜기**입니다. 사용자가 **끄기**로 전환하는 경우 수동으로 백그라운드 작업 권한을 정의할 수 있는 확인란으로 표시됩니다. 확인란을 선택하는 경우 앱은 항상 백그라운드 작업을 실행하도록 허용합니다. 확인란이 선택되지 않은 경우 백그라운드 작업은 사용되지 않습니다.

![백그라운드 작업 권한 켜기](images/background-task-permissions-on.png)

![백그라운드 작업 권한 끄기](images/background-task-permissions-off.png)

앱에서 [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) 메서드를 호출하여 반환되는 [**BackgroundAccessStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) 열거 값을 사용하여 현재 백그라운드 작업 권한 설정을 확인할 수 있습니다.

앱에서 책임 있는 백그라운드 작업 관리를 구현하지 않을 경우 사용자가 앱에 대해 백그라운드 사용 권한을 완전히 거부할 수 있으며, 이는 양쪽에 모두 바람직하지 않습니다. 앱에 백그라운드 실행 권한이 거부되었지만 사용자에 대한 작업을 완료하기 위해 백그라운드 작업이 필요한 경우 사용자에게 알려 설정 앱으로 이동하도록 할 수 있습니다. 이는 백그라운드 앱 또는 배터리 사용량 정보 페이지에 대해 [설정 앱을 시작](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/launch-settings-app)하여 수행할 수 있습니다.

## <a name="work-with-the-battery-saver-feature"></a>배터리 절약 모드 기능 사용
배터리 절약 모드는 사용자가 설정에서 구성할 수 있는 시스템 수준의 기능입니다. 배터리 잔량이 사용자가 정의한 임계값 아래로 내려가면 "항상 허용됨"으로 설정된 앱의 백그라운드 작업을 *제외하고* 모든 앱의 백그라운드 작업이 모두 중단됩니다.

[**PowerManager.EnergySaverStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.system.power.energysaverstatus) 속성을 참조하여 앱 내에서 배터리 절약 모드의 상태를 확인합니다. 열거 값은 **EnergySaverStatus.Disabled**, **EnergySaverStatus.Off** 또는 **EnergySaverStatus.On**입니다. 앱에서 백그라운드 작업이 필요하며 "항상 허용됨"으로 설정되지 않은 경우 배터리 절약 모드가 꺼질 때까지 지정된 백그라운드 작업이 실행되지 않음을 사용자에게 알려 **EnergySaverStatus.On**을 처리해야 합니다. 백그라운드 작업 관리가 배터리 절약 모드의 주요 목적이지만 배터리 절약 모드가 켜져 있는 동안 에너지를 더 절약하기 위해 앱에서 추가로 조정할 수 있습니다.  배터리 절약 모드가 켜져 있는 경우 앱에서 애니메이션 사용을 줄이거나, 위치 폴링을 중지하거나, 동기화 및 백업을 연기할 수 있습니다. 

## <a name="further-optimize-background-tasks"></a>추가 백그라운드 작업 최적화
다음은 백그라운드 작업을 등록할 때 배터리 인식을 향상하기 위해 수행할 수 있는 추가 단계입니다.

### <a name="use-a-maintenance-trigger"></a>유지 관리 트리거 사용 
[**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) 개체 대신 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) 개체를 사용하여 백그라운드 작업이 시작되는 시기를 결정할 수 있습니다. 유지 관리 트리거를 사용하는 작업은 디바이스가 AC 전원에 연결된 경우에만 실행되며, 더 오래 실행할 수 있습니다. 지침은 [유지 관리 트리거 사용](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger)을 참조하세요.

### <a name="use-the-backgroundworkcostnothigh-system-condition-type"></a>**BackgroundWorkCostNotHigh** 시스템 조건 유형 사용
백그라운드 작업을 실행하려면 시스템 조건을 충족해야 합니다(자세한 내용은 [백그라운드 작업 실행 조건 설정](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task) 참조). 백그라운드 작업 비용은 백그라운드 작업 실행이 에너지에 미치는 *상대적인* 영향을 나타내는 측정값입니다. 디바이스가 AC 전원에 연결되어 있을 때 실행 중인 작업은 **낮음**(배터리에 미치는 영향이 거의/전혀 없음)으로 표시됩니다. 화면이 꺼진 상태로 디바이스가 배터리 전원을 사용할 때 실행 중인 작업은 당시에 디바이스에서 실행 중인 프로그램 활동이 거의 없어서 백그라운드 작업의 상대적인 비용이 커지기 때문에 **높음**으로 표시됩니다. 화면이 *켜진* 상태로 디바이스가 배터리 전원을 사용할 때 실행 중인 작업은 이미 실행 중인 일부 프로그램 활동이 있고 백그라운드 작업으로 인해 에너지 비용이 약간 더 추가되므로 **중간**으로 표시됩니다. **BackgroundWorkCostNotHigh** 시스템 조건은 단순히 화면이 켜지거나 디바이스가 AC 전원에 연결될 때까지 작업 실행을 연기합니다.

## <a name="test-battery-efficiency"></a>배터리 효율성 테스트

높은 전원 소비 시나리오에 대해 실제 디바이스에서 앱을 테스트해야 합니다. 여러 디바이스와 다양한 네트워크 강도의 환경에서 배터리 절약 모드를 켜거나 끄고 앱을 테스트하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목

* [out-of-process 백그라운드 작업 만들기 및 등록](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [성능 계획](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  

