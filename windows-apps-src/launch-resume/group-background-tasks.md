---
title: 백그라운드 작업 등록 그룹화
description: 백그라운드 작업을 그룹의 일부로 등록/등록 취소 하 여 해당 등록을 격리 합니다.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, 백그라운드 작업
ms.openlocfilehash: 61419ac45acd27758e3c874ac4b03510561ccaf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155867"
---
# <a name="group-background-task-registration"></a>백그라운드 작업 등록 그룹화

**중요 API**

[BackgroundTaskRegistrationGroup 클래스](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

이제 백그라운드 작업을 그룹에 등록 하 여 논리적 네임 스페이스로 간주할 수 있습니다. 이러한 격리는 앱의 여러 구성 요소 또는 다른 라이브러리의 백그라운드 작업 등록을 방해 하지 않도록 하는 데 도움이 됩니다.

앱과이 응용 프로그램에서 사용 하는 프레임 워크 (또는 라이브러리)가 동일한 이름의 백그라운드 작업을 등록 하면 앱에서 프레임 워크의 백그라운드 작업 등록을 실수로 제거할 수 있습니다. 앱 작성자는 [BackgroundTaskRegistration 작업](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)을 사용 하 여 등록 된 모든 백그라운드 작업을 등록 취소할 수 있으므로 프레임 워크 및 라이브러리 백그라운드 작업 등록을 실수로 제거할 수도 있습니다.  그룹을 사용 하면 백그라운드 작업 등록을 격리할 수 있으므로이 작업은 발생 하지 않습니다.

## <a name="features-of-groups"></a>그룹의 기능

* GUID로 그룹을 고유 하 게 식별할 수 있습니다. 디버깅 하는 동안 더 쉽게 읽을 수 있는 연결 된 이름 문자열을 포함할 수도 있습니다.
* 그룹에는 여러 백그라운드 작업을 등록할 수 있습니다.
* 그룹에 등록 된 백그라운드 작업은 [BackgroundTaskRegistration](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)에 표시 되지 않습니다. 따라서 현재 **BackgroundTaskRegistration 작업** 을 사용 하 여 해당 작업의 등록을 취소 하는 앱은 그룹에 등록 된 백그라운드 작업을 실수로 등록 취소 하지 않습니다. 그룹의 일부로 등록 된 모든 백그라운드 트리거의 등록을 취소 하는 방법은 아래 [그룹의 백그라운드 작업 등록 취소](#unregister-background-tasks-in-a-group) 를 참조 하세요.
* 각 백그라운드 작업 등록에는 연결 된 그룹을 결정 하기 위한 그룹 속성이 있습니다.
* 그룹을 사용 하 여 In-process 백그라운드 작업을 등록 하면 활성화가 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_)대신 [BackgroundTaskRegistrationGroup BackgroundActivated](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) 이벤트를 거칩니다.

## <a name="register-a-background-task-in-a-group"></a>그룹에 백그라운드 작업 등록

다음은 그룹의 일부로 백그라운드 작업 (이 예제에서는 표준 시간대 변경에 의해 트리거됨)을 등록 하는 방법을 보여 줍니다.

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

## <a name="unregister-background-tasks-in-a-group"></a>그룹에서 백그라운드 작업 등록 취소

다음은 그룹의 일부로 등록 된 백그라운드 작업의 등록을 취소 하는 방법을 보여 줍니다.
그룹에 등록 된 백그라운드 작업은 [BackgroundTaskRegistration](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)에 표시 되지 않으므로 그룹을 반복 하 고 각 그룹에 등록 된 백그라운드 작업을 찾아 등록을 취소 해야 합니다.

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

## <a name="register-persistent-events"></a>영구적 이벤트 등록

In-process 백그라운드 작업에서 백그라운드 작업 등록 그룹을 사용 하는 경우 백그라운드 활성화는 응용 프로그램 또는 CoreApplication 개체에 있는 것이 아니라 그룹의 이벤트로 전달 됩니다. 이렇게 하면 응용 프로그램 개체에 모든 활성화 코드 경로를 저장 하는 대신 응용 프로그램 내의 여러 구성 요소가 활성화를 처리할 수 있습니다. 다음은 그룹의 백그라운드 활성화 이벤트를 등록 하는 방법을 보여 줍니다. 먼저 [BackgroundTaskRegistration. GetTaskGroup](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) 를 확인 하 여 그룹이 이미 등록 되었는지 확인 합니다. 그렇지 않으면 id와 이름을 사용 하 여 새 그룹을 만듭니다. 그런 다음 그룹의 BackgroundActivated 이벤트에 이벤트 처리기를 등록 합니다.

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