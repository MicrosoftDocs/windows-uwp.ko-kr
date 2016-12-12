---
author: PatrickFarley
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: Utilize battery-saving features
description: Create UWP apps that work with the system to use background tasks in a battery-efficient way.
translationtype: Human Translation
ms.sourcegitcommit: 73b19e54b863693aece045e5b653bc0583a676bb
ms.openlocfilehash: 854ec43d075f8adc1f875d3b9e5e2d818434edb9

---

# <a name="optimize-background-activity"></a>Optimize background activity

Universal Windows apps should perform consistently well across all device families. On battery-powered devices, power consumption is a critical factor in the user's overall experience with your app. All-day battery life is a desirable feature to every user, but it requires efficiency from all of the software installed on the device, including your own. 

Background task behavior is arguably the greatest factor in the total energy cost of an app. A background task is any program activity that has been registered with the system to run without the app being open. See [Create and register an out-of-process background task](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task) for more.

## <a name="background-activity-allowance"></a>Background activity allowance

In Windows 10, version 1607, users can view their "Battery usage by app" in the **Battery** section of the Settings app. Here, they will see a list of apps and the percentage of battery life (out of the amount of battery life that has been used since the last charge) that each app has consumed. 

![battery usage by app](images/battery-usage-by-app.png)

For UWP apps on this list, users have some control over how the system treats their background activity. Background activity can be specified as "Always allowed," "Managed by Windows" (the default setting), or "Never allowed" (more details on these below). Use the **BackgroundAccessStatus** enum value returned from the [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) method to see what background activity allowance your app has.

![background task permissions](images/background-task-permissions.png)

All this is to say that if your app doesn't implement responsible background activity management, the user may deny background permissions to your app altogether, which is not desirable for either party.

## <a name="work-with-the-battery-saver-feature"></a>Work with the Battery Saver feature
Battery Saver is a system-level feature that users can configure in Settings. It cuts off all background activity of all apps when the battery level drops below a user-defined threshold, *except* for the background activity of apps that have been set to "Always allowed."

If your app is marked as "Managed by Windows" and calls **BackgroundExecutionManager.RequestAccessAsync()** to register a background activity while Battery Saver is on, it will return a **DeniedSubjectToSystemPolicy** value. Your app should handle this by notifying the user that the given background task(s) will not run until Battery Saver is off and they are re-registered with the system. If a background task has already been registered to run, and Battery Saver is on at the time of its trigger, the task will not run and the user will not be notified. To reduce the chance of this happening, it is a good practice to program your app to re-register its background tasks each time it is opened.

While background activity management is the primary purpose of the Battery Saver feature, your app can make additional adjustments to further conserve energy when Battery Saver is on. Check the status of Battery Saver mode from within your app by referencing the [**PowerManager.PowerSavingMode**](https://msdn.microsoft.com/library/windows/apps/windows.phone.system.power.powermanager.powersavingmode.aspx) property. It is an enum value: either **PowerSavingMode.Off** or **PowerSavingMode.On**. In the case where Battery Saver is on, your app could reduce its use of animations, stop location polling, or delay syncs and backups. 

## <a name="further-optimize-background-tasks"></a>Further optimize background tasks
The following are additional steps you can take when registering your background tasks to make them more battery-aware.

Use a maintenance trigger. A [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) object can be used instead of a [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) object to determine when a background task starts. Tasks that use maintenance triggers will only run when the device is connected to AC power, and they are allowed to run for longer. See [Use a maintenance trigger](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger) for instructions.

Use the **BackgroundWorkCostNotHigh** system condition type. System conditions must be met in order for background tasks to run (see [Set conditions for running a background task](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task) for more). The background work cost is a measurement that denotes the *relative* energy impact of running the background task. A task running when the device is plugged into AC power would be marked as **low** (little/no impact on battery). A task running when the device is on battery power with the screen off is marked as **high** because there is presumably little program activity running on the device at the time, so the background task would have a greater relative cost. A task running when the device is on battery power with the screen *on* is marked as **medium**, because there is presumably already some program activity running, and the background task would add a bit more to the energy cost. The **BackgroundWorkCostNotHigh** system condition simply delays your task's ability to run until either the screen is on or the device is connected to AC power.

## <a name="test-battery-efficiency"></a>Test battery efficiency

Make sure to test your app on real devices for any high-power-consumption scenarios. It's a good idea to test your app on many different devices, with Battery Saver on and off, and in environments of varying network strength.

## <a name="related-topics"></a>Related topics

* [Create and register an out-of-process background task](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task)  
* [Planning for performance](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  




<!--HONumber=Dec16_HO1-->


