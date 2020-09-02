---
title: 백그라운드에서 무기한 실행
description: ExtendedExecutionUnconstrained 기능을 사용 하 여 백그라운드에서 백그라운드 작업 또는 확장 된 실행 세션을 무기한으로 실행 합니다.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 백그라운드 작업, 확장 된 실행, 리소스, 제한, 백그라운드 작업
ms.date: 10/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 33b41c432edde42bc31daa1d5631f60fb38d8397
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304505"
---
# <a name="run-in-the-background-indefinitely"></a>백그라운드에서 무기한 실행

사용자에 게 최상의 환경을 제공 하기 위해 Windows는 UWP (유니버설 Windows 플랫폼) 앱에 대 한 리소스 제한을 설정 합니다. 포그라운드 앱에는 메모리 및 실행 시간이 가장 많이 제공 됩니다. 백그라운드 앱이 줄어듭니다. 따라서 사용자는 포그라운드 앱 성능이 저하 되 고 배터리가 많이 소모 되지 않도록 보호 됩니다.

그러나 개인 용 UWP 앱 (즉, Microsoft Store에 게시 되지 않은 테스트용 로드 된 앱)을 작성 하는 개발자 또는 엔터프라이즈 UWP 앱을 작성 하는 개발자는 백그라운드 또는 확장 된 실행 제한 없이 장치에서 사용 가능한 모든 리소스를 사용 하는 것이 좋습니다. Lob (기간 업무) 및 개인 UWP 응용 프로그램은 Windows 크리에이터 업데이트 (버전 1703)에서 Api를 사용 하 여 제한을 해제할 수 있습니다. 이러한 Api를 사용 하는 경우 Microsoft Store에 앱을 배치할 수 없습니다.

## <a name="run-while-minimized"></a>최소화 된 상태에서 실행

UWP 앱은 포그라운드로 실행 되 고 있지 않을 때 일시 중단 됨 상태로 전환 됩니다. 바탕 화면에서 사용자가 앱을 최소화할 때 발생 합니다. 앱은 최소화 된 상태에서 계속 실행 하기 위해 확장 된 실행 세션을 사용 합니다. Microsoft Store에서 허용 하는 확장 된 실행 Api는 [확장 된 실행으로 앱 일시 중단 연기](./run-minimized-with-extended-execution.md)에 자세히 설명 되어 있습니다.

Microsoft Store에 제출 하기에 적합 하지 않은 앱을 개발 하는 경우 [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) `extendedExecutionUnconstrained` 장치의 에너지 상태와 관계 없이 앱이 최소화 된 상태에서 계속 실행 될 수 있도록 제한 된 기능과 함께 ExtendedExecutionForegroundSession를 사용할 수 있습니다.  

`extendedExecutionUnconstrained`기능은 앱의 매니페스트에 제한 된 기능으로 추가 됩니다. 제한 된 기능에 대 한 자세한 내용은 [앱 기능 선언](../packaging/app-capability-declarations.md) 을 참조 하세요.

> [!NOTE]
> *Xmlns: rescap* XML 네임 스페이스 선언을 추가 하 고 *rescap* 접두사를 사용 하 여 기능을 선언 합니다.
>
> 자세한 내용은 [앱 기능 선언의](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)제한 된 기능 섹션을 참조 하세요.
>

_Package.appxmanifest_

```xml
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
  ...
  <Capabilities>
    <rescap:Capability Name="extendedExecutionUnconstrained"/>
  </Capabilities>
</Package>
```

기능을 사용 하는 경우 `extendedExecutionUnconstrained` [ExtendedExecutionSession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) 및 [ExtendedExecutionReason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason)대신 [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 및 [ExtendedExecutionForegroundReason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) 가 사용 됩니다. 세션을 만들고, 멤버를 설정 하 고, 확장을 요청 하는 동일한 패턴이 여전히 적용 됩니다. 

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

앱이 포그라운드로 들어오는 즉시이 확장 된 실행 세션을 요청할 수 있습니다. 제한 없는 확장 실행 세션은 에너지 할당량 또는 운영 체제 배터리 보호기로 제한 되지 않습니다. 세션 개체에 대 한 참조가 있으면 앱이 실행 중 상태가 되 고 일시 중단 됨 상태로 전환 됩니다. 사용자가 앱을 닫으면 세션이 해지 됩니다.

**해지** 된 이벤트를 등록 하면 앱에서 필요한 정리 작업을 수행할 수 있습니다. 일시 중단 상태에서는 앱이 종료 되 고 메모리에서 제거 되기 전에   [ExtendedExecutionReason. SavingData](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) 를 사용 하 여 확장 된 실행 세션을 만들어 사용자 데이터를 저장할 수 있습니다.

## <a name="run-background-tasks-indefinitely"></a>무기한으로 백그라운드 작업 실행

유니버설 Windows 플랫폼에서 백그라운드 작업은 사용자 인터페이스의 폼 없이 백그라운드에서 실행 되는 프로세스입니다. 백그라운드 작업은 일반적으로 최대 25 초 동안 실행 될 수 있으며 취소 됩니다. 또한 오래 실행 되는 작업 중 일부에는 백그라운드 작업이 유휴 상태가 아니거나 메모리를 사용 하 고 있지 않은지 확인 해야 합니다. Windows 크리에이터 업데이트 (버전 1703)에서는 이러한 한도를 제거 하기 위해 [extendedBackgroundTaskTime](../packaging/app-capability-declarations.md) 제한 된 기능이 도입 되었습니다. **ExtendedBackgroundTaskTime** 기능은 앱의 매니페스트 파일에 제한 된 기능으로 추가 됩니다.

> [!NOTE]
> *Xmlns: rescap* XML 네임 스페이스 선언을 추가 하 고 *rescap* 접두사를 사용 하 여 기능을 선언 합니다.
>
> 자세한 내용은 [앱 기능 선언의](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)제한 된 기능 섹션을 참조 하세요.
>

_Package.appxmanifest_

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
...
  <Capabilities>
    <rescap:Capability Name="extendedBackgroundTaskTime"/>
  </Capabilities>
</Package>
```

이 기능은 실행 시간 제한 및 유휴 작업 watchdog을 제거 합니다. 트리거 또는 app service 호출로 인해 백그라운드 작업이 시작 된 후에는 **실행** 메서드에서 제공 하는 [BackgroundTaskInstance](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 에서 지연이 발생 하면 무기한 실행 될 수 있습니다. 앱이 **Windows에서 관리**하도록 설정 된 경우에는 여전히 에너지 할당량이 적용 될 수 있으며 배터리 절약이 활성화 되 면 백그라운드 작업이 활성화 되지 않습니다.OS 설정으로 변경할 수 있습니다. [백그라운드 작업 최적화](../debug-test-perf/optimize-background-activity.md)에서 추가 정보를 사용할 수 있습니다.

유니버설 Windows 플랫폼은 백그라운드 작업 실행을 모니터링 하 여 배터리 수명 및 부드러운 포그라운드 앱 환경을 보장 합니다. 그러나 개인 앱 및 엔터프라이즈 기간 업무 앱은 확장 된 실행 및 **extendedBackgroundTaskTime** 기능을 사용 하 여 장치의 리소스 가용성에 관계 없이 필요한 경우에만 실행 되는 앱을 만들 수 있습니다.

**ExtendedExecutionUnconstrained** 및 **extendedBackgroundTaskTime** 기능은 UWP 앱에 대 한 기본 정책을 재정의할 수 있으며 상당한 배터리가 소모 될 수 있습니다. 이러한 기능을 사용 하기 전에 먼저 기본 확장 실행 및 백그라운드 작업 시간 정책이 사용자의 요구를 충족 하지 않는지 확인 하 고 배터리 제한 조건에서 테스트를 수행 하 여 앱이 장치에 주는 영향을 파악 합니다.

## <a name="see-also"></a>참고 항목

[백그라운드 작업 리소스 제한 제거](/windows/application-management/enterprise-background-activity-controls)