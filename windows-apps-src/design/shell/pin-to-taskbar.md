---
Description: You can programmatically pin your app to the taskbar,  bnd you can check if it's currently pinned.
title: 작업 표시줄에 앱 고정
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 작업 표시줄, 작업 표시줄 관리자, 작업 표시줄에 고정, 기본 타일
ms.localizationpriority: medium
ms.openlocfilehash: 640dc637a1c50718210d87af87cb8b8e706a5ab7
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8937571"
---
# <a name="pin-your-app-to-the-taskbar"></a>작업 표시줄에 앱 고정

[시작 메뉴에 앱 고정](tiles-and-notifications/primary-tile-apis.md)과 마찬가지 방법으로 앱을 프로그래밍 방식으로 작업 표시줄에 고정할 수 있습니다. 그리고 앱이 현재 고정되어 있는지, 작업 표시줄에 고정할 수 있는지 여부를 확인할 수 있습니다. 

![작업 표시줄](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **Fall Creators Update 필요**: 작업 표시줄 API를 사용하려면 SDK 16299를 대상으로 하고 빌드 16299 이상을 실행하고 있어야 합니다.

> **중요 API**: [TaskbarManager 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>언제 사용자에게 앱을 작업 표시줄에 고정하라고 요청해야 할까요? 

[TaskbarManager 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)를 사용하여 사용자에게 앱을 작업 표시줄에 고정하라고 요청하면 사용자는 요청을 승인해야 합니다. 여러분은 멋진 앱을 만들기 위해 많은 노력을 기울였으며, 이제 사용자에게 앱을 작업 표시줄에 고정하라고 요청할 차례입니다. 그러나 코딩을 진행하기 전에 환경 설계 시 유념해야 할 몇 가지 사항이 있습니다.

* 명확한 "작업 표시줄에 고정" 행동 요청을 통해 중단이 없고 쉽게 해제할 수 있는 UX를 앱에 **만드세요**. 이러한 목적에 대화 상자와 플라이아웃은 사용하지 마세요. 
* 사용자에게 앱을 고정하라고 요청하기 전에 앱의 가치에 대해 명확하게 **설명하세요**.
* 타일이 이미 고정되어 있거나 장치에서 고정을 지원하지 않는 경우 앱을 고정하라고 **요청하지 마세요**. (이 문서에서는 고정이 지원되는지 확인하는 방법을 설명합니다.)
* 사용자에게 앱을 고정하라고 **반복적으로 요청하지 마세요**. 사용자가 귀찮아할 수 있습니다.
* 명시적인 사용자 상호 작용이 없거나 앱이 최소화되어 있거나 열려 있지 않은 경우 고정 API를 **호출하지 마세요**.


## <a name="1-check-whether-the-required-apis-exist"></a>1. 필요한 API가 있는지 확인

앱에서 이전 버전의 Windows 10을 지원하는 경우 TaskbarManager 클래스를 사용할 수 있는지 확인해야 합니다. [ApiInformation.IsTypePresent 메서드](https://docs.microsoft.com/en-us/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_)를 사용하여 이 확인 작업을 수행할 수 있습니다. TaskbarManager 클래스를 사용할 수 없는 경우 API 호출을 실행하지 마세요.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. 작업 표시줄이 있고 고정이 가능한지 확인

UWP 앱은 다양한 장치에서 실행 가능하지만 모든 장치가 작업 표시줄을 지원하는 것은 아닙니다. 현재는 데스크톱 장치만 작업 표시줄을 지원합니다. 

작업 표시줄을 사용할 수 있더라도 사용자 컴퓨터의 그룹 정책에 의해 작업 표시줄에 고정을 사용하지 못할 수도 있습니다. 따라서 앱을 고정하려고 시도하기 전에 작업 표시줄에 고정이 지원되는지 여부를 먼저 확인해야 합니다. 작업 표시줄이 있고 앱 고정이 허용되면 [TaskbarManager.IsPinningAllowed 속성](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed)이 true를 반환합니다. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> 앱을 작업 표시줄에 고정하지 않고 작업 표시줄을 사용할 수 있는지만 확인하려면 [TaskbarManager.IsSupported 속성](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported)을 사용합니다.


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. 앱이 현재 작업 표시줄에 고정되어 있는지 확인

당연한 말이지만, 앱이 이미 작업 표시줄에 고정되어 있는데 사용자에게 앱을 작업 표시줄에 고정하라고 요청하는 것은 아무 의미가 없습니다. 사용자에게 앱을 고정하라고 요청하기 전에 [TaskbarManager.IsCurrentAppPinnedAsync 메서드](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync)를 사용하여 앱이 이미 고정되어 있는지 확인할 수 있습니다.

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4. 앱 고정

작업 표시줄이 있고, 앱 고정이 허용되고, 앱이 현재 고정되어 있지 않으면 사용자에게 앱을 고정할 수 있음을 알리는 세련된 팁을 표시할 수 있습니다. 예를 들어 UI 어딘가에 사용자가 클릭할 수 있는 핀 아이콘을 표시할 수 있습니다. 

사용자가 고정 제안 UI를 클릭하면 [TaskbarManager.RequestPinCurrentAppAsync 메서드](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync)를 호출해야 합니다. 이 메서드는 앱을 작업 표시줄에 고정할 것인지 사용자에게 다시 묻는 대화 상자를 표시합니다.

> [!IMPORTANT]
> 이 메서드는 포그라운드 UI 스레드에서 호출되어야 하며, 그렇지 않으면 예외가 발생합니다.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![고정 대화 상자](images/taskbar/pin-dialog.png)

이 메서드는 앱이 현재 작업 표시줄에 고정되어 있는지 여부를 나타내는 부울 값을 반환합니다. 앱이 이미 고정되어 있으면 이 메서드는 사용자에게 대화 상자를 표시하지 않고 즉시 true를 반환됩니다. 사용자가 대화 상자에서 "아니요"를 클릭하거나 작업 표시줄에 앱 고정이 허용되지 않으면 이 메서드는 false를 반환합니다. 그렇지 않고 사용자가 예를 클릭하고 앱이 고정되어 있으면 API가 true를 반환합니다.


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [TaskbarManager 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [시작 메뉴에 앱 고정](tiles-and-notifications/primary-tile-apis.md)