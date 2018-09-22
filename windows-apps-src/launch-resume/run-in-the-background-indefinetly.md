---
author: TylerMSFT
title: 백그라운드에서 무기한 실행
description: extendedExecutionUnconstrained 기능을 사용하여 백그라운드 작업 또는 확장된 실행 세션을 백그라운드에서 무기한 실행하세요.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 백그라운드 작업을 실행, 리소스, 제한, 백그라운드 작업 확장
ms.author: twhitney
ms.date: 10/3/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: af0f7670f2b131671ce82708d2b0a826db0fcfb1
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4124634"
---
# <a name="run-in-the-background-indefinitely"></a>백그라운드에서 무기한 실행

사용자에게 최상의 환경을 제공하기 위해 Windows는 UWP(유니버설 Windows 플랫폼) 앱에 대한 리소스 제한을 적용합니다. 포그라운드 앱에는 가장 많은 메모리와 실행 시간이 주어지며, 백그라운드 앱은 더 적습니다. 따라서 사용자는 포그라운드 앱의 성능 저하 및 배터리 사용량의 많은 소모로부터 보호됩니다.

하지만 UWP 앱을 개인용으로 개발하는 개발자(Microsoft Store에 게시되지 않고 테스트용으로 로드된 앱) 또는 엔터프라이즈 UWP 앱을 개발하는 개발자는 백그라운드나 확장 없이 장치에서 사용할 수 있는 모든 리소스를 사용하려고 할 수 있습니다. 기간 업무 및 개인 UWP 응용 프로그램은 Windows 크리에이터스 업데이트(버전 1703)의 API를 사용하여 제한 기능을 해제할 수 있습니다. 이러한 API를 사용하는 경우 앱을 Microsoft Store에 게시할 수 없습니다.

## <a name="run-while-minimized"></a>최소화된 상태로 실행

UWP 앱이 포그라운드에서 실행되지 않을 때 일시 중단된 상태로 이동합니다. 데스크톱에서는 사용자가 앱을 최소화할 때 발생합니다. 앱은 최소화된 상태에서 계속 실행하기 위해 확장된 실행 세션을 사용합니다. Microsoft Store에서 허용하는 확장된 실행 API는 [확장 실행을 사용하여 앱 일시 중단 연기](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution)에 세부적으로 나와 있습니다.

Microsoft Store에 제출하지 않는 앱을 개발하는 경우, 장치의 에너지 상태에 관계없이 앱이 최소화된 상태로 계속 실행될 수 있도록 [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 및 `extendedExecutionUnconstrained` 제한 기능을 사용할 수 있습니다.  

`extendedExecutionUnconstrained` 기능이 앱의 매니페스트에 제한 기능으로 추가됩니다. 제한 기능에 대한 자세한 내용은 [앱 접근 권한 값 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조하세요.

_Package.appxmanifest_
```xml
<Package ...>
...
  <Capabilities>  
    <rescap:Capability Name="extendedExecutionUnconstrained"/>  
  </Capabilities>  
</Package>
```

`extendedExecutionUnconstrained` 기능을 사용하면 [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 및 [ExtendedExecutionForegroundReason](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason)은 [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) 및 [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) 대신 사용됩니다. 세션을 만들고, 구성원을 설정하고, 확장을 비동기적으로 요청하기 위한 동일한 패턴이 여전히 적용됩니다. 

```cs
var newSession = new ExtendedExecutionForegroundSession();  
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;  
newSession.Description = "Long Running Processing";  
newSession.Revoked += SessionRevoked;  
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();  
switch (result)  
{  
    case ExtendedExecutionResult.Allowed:  
        DoLongRunningWork();  
        break;  

    default:  
    case ExtendedExecutionResult.Denied:  
        DoShortRunningWork();  
        break;  
}
```

앱이 포그라운드로 오자마자 이 확장된 실행 세션을 요청할 수 있습니다. 제한되지 않은 확장 실행 세션은 에너지 할당량이나 운영 체제 배터리 절약 모드에 의해 제한되지 않습니다. 세션 개체에 대한 참조가 존재하는 한 앱은 실행 상태를 유지하고 일시 중단된 상태가 되지 않습니다. 앱이 사용자에 의해 닫히면 세션이 취소됩니다.

**Revoked** 이벤트에 등록하면 앱에서 필요한 정리 작업을 수행할 수 있습니다. 일시 중단된 상태에서 [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason)가 있는 확장된 실행 세션을 생성하여 앱이 종료되고 메모리에서 제거되기 전에 사용자 데이터를 저장할 수 있습니다.

## <a name="run-background-tasks-indefinitely"></a>백그라운드 작업 무기한 실행

유니버설 Windows 플랫폼에서 백그라운드 작업은 사용자 인터페이스 없이 백그라운드에서 실행되는 프로세스입니다. 백그라운드 작업은 일반적으로 취소되기 전에 최대 25초 동안 실행될 수 있습니다. 장기 실행 작업 중 일부에는 백그라운드 작업이 유휴 상태이거나 메모리를 사용하고 있지 않은지 확인하는 검사가 있습니다. Windows 크리에이터스 업데이트(버전 1703)에서 [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) 제한된 접근 권한 값은 이러한 제한을 제거하기 위해 도입되었습니다. **extendedBackgroundTaskTime** 기능이 앱의 매니페스트 파일에 제한 기능으로 추가됩니다.

_Package.appxmanifest_
```xml
<Package ...>
   <Capabilities>  
       <rescap:Capability Name="extendedBackgroundTaskTime"/>  
   </Capabilities>  
</Package>
```

이 기능은 실행 시간 제한과 유휴 작업 감시를 제거합니다. 트리거 또는 앱 서비스 호출에 의해 백그라운드 작업이 시작되고 **Run** 메서드에서 제공하는 [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)에 지연이 있으면 무기한 실행될 수 있습니다. 앱이 **Windows에서 관리됨**으로 설정된 경우 여전히 앱에 적용된 에너지 할당량을 가질 수 있으며, 배터리 절약 모드가 활성화되어 있을 때 해당 백그라운드 작업은 활성화되지 않습니다. 이는 OS 설정으로 변경할 수 있습니다. 자세한 내용은 [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)에서 확인할 수 있습니다.

유니버설 Windows 플랫폼은 백그라운드 작업 실행을 모니터링하여 배터리 수명을 늘리고 원활한 포그라운드 앱 환경을 보장합니다. 그러나 개인 앱 및 엔터프라이즈 기간 업무 앱은 확장된 실행과 **extendedBackgroundTaskTime** 기능을 사용하여 장치의 리소스 가용성에 관계없이 필요한 만큼 실행되는 앱을 만들 수 있습니다.

**extendedExecutionUnconstrained** 및 **extendedBackgroundTaskTime** 기능은 UWP 앱의 기본 정책을 재정의할 수 있으며 배터리를 상당히 소모할 수 있습니다. 이러한 기능을 사용하기 전에 먼저 기본 확장 실행 및 백그라운드 작업 시간 정책이 요구 사항을 충족하지 않은지 확인하고, 배터리 제한 조건에서 테스트를 수행하여 앱이 장치에 미칠 영향을 파악하세요.

## <a name="see-also"></a>참고 항목

[백그라운드 작업 리소스 제한 제거](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)
