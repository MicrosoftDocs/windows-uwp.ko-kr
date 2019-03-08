---
title: 백그라운드 작업 등록 그룹화
description: 그룹의 일부로 백그라운드 작업을 등록/등록 취소하여 이러한 등록을 격리합니다.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, 백그라운드 작업
ms.openlocfilehash: a70c814e5e35359746076c5418d1f1d973e61773
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623858"
---
# <a name="group-background-task-registration"></a>백그라운드 작업 등록 그룹화

**중요 한 Api**

[BackgroundTaskRegistrationGroup 클래스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

이제 논리적 네임스페이스라고 생각할 수 있는 그룹에 백그라운드 작업을 등록할 수 있습니다. 이렇게 격리하면 하나의 앱이나 서로 다른 라이브러리의 여러 구성 요소가 서로의 백그라운드 작업 등록을 방해하지 않습니다.

앱과 앱에서 사용하는 프레임워크나 라이브러리가 백그라운드 작업을 동일한 이름으로 등록할 경우 앱에서 실수로 프레임워크의 백그라운드 작업 등록을 제거할 수 있습니다. 앱 작성자가 [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)를 사용하여 등록된 백그라운드 작업을 모두 등록 취소할 수 있기 때문에 실수로 프레임워크와 라이브러리 백그라운드 작업 등록을 제거할 수도 있습니다.  그룹으로 백그라운드 작업 등록을 격리하여 이를 방지할 수 있습니다.

## <a name="features-of-groups"></a>그룹의 기능

* GUID로 그룹을 고유하게 식별할 수 있습니다. 또한 디버그 중 더 쉽게 읽을 수 있게 그룹에 이름 문자열을 연결할 수도 있습니다.
* 여러 백그라운드 작업은 한 그룹에 등록할 수 있습니다.
* 그룹에 등록된 백그라운드 작업은 [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)에 나타나지 않습니다. 따라서 현재 **BackgroundTaskRegistration.AllTasks**를 사용하여 작업을 등록 취소하는 앱은 그룹에 등록된 백그라운드 작업을 실수로 등록 취소하지 않습니다. 참조 [백그라운드 작업 그룹의 등록을 취소](#unregister-background-tasks-in-a-group) 그룹의 일부로 등록 된 모든 백그라운드 트리거 등록을 취소 하는 방법을 보려면 아래.
* 백그라운드 작업 등록마다 연결되는 그룹을 결정하는 Group 속성이 있습니다.
* 그룹을 사용 하 여 백그라운드 작업 등록 프로세스를 활성화 하면 [BackgroundTaskRegistrationGroup.BackgroundActivated](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) 이벤트 대신 [Application.OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

## <a name="register-a-background-task-in-a-group"></a>그룹에 백그라운드 작업 등록

다음은 그룹의 일부로 백그라운드 작업을 등록하는 방법을 보여 줍니다. 이 예에서는 시간대 변경에 의해 백그라운드 작업이 트리거됩니다.

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>그룹에 백그라운드 작업 등록 취소

다음은 그룹의 일부로 등록된 백그라운드 작업을 등록 취소하는 방법을 보여 줍니다.
그룹에 등록된 백그라운드 작업은 [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)에 나타나지 않으므로 그룹을 반복하고 각 그룹에 등록된 백그라운드 작업을 찾아 등록 취소해야 합니다.

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>영구 이벤트 등록

In-process 백그라운드 작업과 함께 백그라운드 작업 등록 그룹을 사용하면 백그라운드 활성화가 Application 또는 CoreApplication 개체의 이벤트 대신 그룹의 이벤트로 리디렉션됩니다. 이를 통해 앱 내의 여러 구성 요소가 Application 개체에 모든 활성화 코드 경로를 넣지 않고 활성화를 처리할 수 있습니다. 다음은 그룹의 백그라운드 활성 이벤트에 등록하는 방법을 보여 줍니다. 먼저 [BackgroundTaskRegistration.GetTaskGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup)을 점검하여 그룹이 이미 등록되었는지 확인합니다. 등록되지 않았으면 자신의 ID와 이름으로 새 그룹을 만듭니다. 그런 다음 그룹의 BackgroundActivated 이벤트에 이벤트 처리기를 등록합니다.

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```
