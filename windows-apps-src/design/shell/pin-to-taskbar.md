---
Description: 작업 표시줄에 프로그래밍 방식으로 앱을 고정 하 여 현재 고정 되어 있는지 확인할 bnd 수 있습니다.
title: 작업 표시줄에 앱 고정
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 작업 표시줄, 작업 표시줄 관리자, 작업 표시줄에 고정, 기본 타일
ms.localizationpriority: medium
ms.openlocfilehash: 44ef6430398960e13fe5eebb40a52d022df6f0d2
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970658"
---
# <a name="pin-your-app-to-the-taskbar"></a>작업 표시줄에 앱 고정

[앱을 시작 메뉴에 고정](tiles-and-notifications/primary-tile-apis.md)하는 것 처럼 작업 표시줄에 프로그래밍 방식으로 자신의 앱을 고정할 수 있습니다. 앱이 현재 고정 되어 있는지 여부와 작업 표시줄에서 고정을 허용 하는지 여부를 확인할 수 있습니다. 

![작업 표시줄](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **낙하 작성자 업데이트 필요**: 작업 표시줄 api를 사용 하려면 SDK 16299을 대상으로 하 고 빌드 16299 이상을 실행 해야 합니다.

> **중요 한 api**: [task바 관리자 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>사용자에 게 작업 표시줄에 앱을 고정 하도록 요청 해야 하는 경우 

작업 표시줄 [관리자 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 를 사용 하면 사용자에 게 앱을 작업 표시줄에 고정 하도록 요청할 수 있습니다. 사용자가 요청을 승인 해야 합니다. 뛰어난 앱을 빌드하는 데 많은 노력을 기울여야 하므로 이제 사용자에 게 작업 표시줄에 고정 하도록 요청할 수 있습니다. 하지만 코드를 살펴보기 전에 경험을 디자인할 때 고려해 야 할 몇 가지 사항은 다음과 같습니다.

* 작업에 대 한 명확한 "작업 표시줄에 고정" 호출을 사용 하 여 앱에서 비 중단 및 쉽게 해제 가능한 UX **를 만들 수** 있습니다. 이러한 용도로는 대화 및 flyouts를 사용 하지 마십시오. 
* 사용자에 게 pin을 요청 하기 전에 앱의 가치 **를 명확 하** 게 설명 합니다.
* 타일이 이미 고정 되어 있거나 장치에서 지원 하지 않는 경우 사용자에 게 앱을 고정 하도록 요청 **하지 마세요** . 이 문서에서는 고정이 지원 되는지 여부를 확인 하는 방법을 설명 합니다.
* 앱을 고정 하도록 사용자에 게 질문 **하지 않습니다** (표정이 될 수 있음).
* 명시적인 사용자 개입 없이 또는 앱이 최소화 되거나 열리지 않는 경우 pin API를 호출 하지 **마세요** .


## <a name="1-check-whether-the-required-apis-exist"></a>1. 필요한 Api가 있는지 확인 합니다.

앱에서 이전 버전의 Windows 10을 지 원하는 경우 Task 관리자 클래스를 사용할 수 있는지 확인 해야 합니다. [IsTypePresent 메서드](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) 를 사용 하 여이 검사를 수행할 수 있습니다. TaskbarManager 클래스를 사용할 수 없는 경우 Api에 대 한 호출을 실행 하지 마십시오.

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


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. 작업 표시줄이 있고 고정이 허용 되는지 확인 합니다.

Windows 앱은 다양 한 장치에서 실행할 수 있습니다. 일부는 작업 표시줄을 지원 하지 않습니다. 현재는 데스크톱 장치만 작업 표시줄을 지원 합니다. 

작업 표시줄을 사용할 수 있는 경우에도 사용자 컴퓨터의 그룹 정책에서 작업 표시줄 고정을 사용 하지 않도록 설정할 수 있습니다. 따라서 앱을 고정 하기 전에 작업 표시줄에 대 한 고정이 지원 되는지 여부를 확인 해야 합니다. 작업 표시줄이 있고 고정이 허용 되는 경우 [TaskIsPinningAllowed 속성](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) 은 true를 반환 합니다. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> 작업 표시줄에 앱을 고정 하지 않고 작업 표시줄을 사용할 수 있는지 여부를 확인 하려는 경우에는 [Task바 관리자. IsSupported 속성](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported)을 사용 합니다.


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. 앱이 현재 작업 표시줄에 고정 되어 있는지 확인 합니다.

사용자에 게 앱을 이미 고정 된 경우 작업 표시줄에 고정할 수 있도록 사용자에 게 요청 하는 점이 없습니다. [IsCurrentAppPinnedAsync 메서드](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync) 를 사용 하 여 앱이 이미 고정 되어 있는지 여부를 확인 한 후 사용자에 게 요청할 수 있습니다.

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

작업 표시줄이 있고 고정이 허용 되 고 앱이 현재 고정 되어 있지 않은 경우 사용자가 앱을 고정할 수 있음을 알려주는 미묘한 팁을 표시 하는 것이 좋습니다. 예를 들어 사용자가 클릭할 수 있는 UI의 어딘가에 고정 아이콘을 표시할 수 있습니다. 

사용자가 pin 제안 UI를 클릭 하면 [Taskbarmanager. RequestPinCurrentAppAsync 메서드](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync)를 호출 합니다. 이 메서드는 사용자에 게 앱을 작업 표시줄에 고정 하려는 것인지 확인 하는 대화 상자를 표시 합니다.

> [!IMPORTANT]
> 이는 전경 UI 스레드에서 호출 해야 합니다. 그렇지 않으면 예외가 throw 됩니다.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![고정 대화 상자](images/taskbar/pin-dialog.png)

이 메서드는 앱이 이제 작업 표시줄에 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 앱이 이미 고정 된 경우 메서드는 사용자에 게 대화 상자를 표시 하지 않고 즉시 true를 반환 합니다. 사용자가 대화 상자에서 "아니요"를 클릭 하거나 작업 표시줄에 앱을 고정 하는 것이 허용 되지 않는 경우이 메서드는 false를 반환 합니다. 그렇지 않으면 사용자가 예를 클릭 하면 앱이 고정 되 고 API는 true를 반환 합니다.


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [Task바 관리자 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [시작 메뉴에 앱 고정](tiles-and-notifications/primary-tile-apis.md)
