---
description: 작업 표시줄에 보조 타일을 고정 하 여 사용자에 게 앱 내의 콘텐츠에 대 한 빠른 액세스를 제공 하는 방법을 알아봅니다.
title: 작업 표시줄에 보조 타일 고정
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 작업 표시줄에 고정, 보조 타일, 보조 타일을 작업 표시줄에 고정, 바로 가기
ms.localizationpriority: medium
ms.openlocfilehash: 22f49fba21e4d3f997efee1a59123ab453e555ea
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220226"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>작업 표시줄에 보조 타일 고정

보조 타일을 시작으로 고정 하는 것 처럼 보조 타일을 작업 표시줄에 고정 하 여 사용자에 게 앱 내의 콘텐츠에 빠르게 액세스할 수 있습니다.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **제한 된 액세스 api**:이 api는 제한 된 액세스 기능입니다. 이 API를 사용 하려면에 문의 하세요 [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar) .

> **10 월 2018 업데이트 필요**: 작업 표시줄에 고정 하려면 SDK 17763를 대상으로 하 고 빌드 17763 이상을 실행 해야 합니다.


## <a name="guidance"></a>지침

보조 타일은 사용자가 앱 내의 특정 영역에 직접 액세스할 수 있는 일관 되 고 효율적인 방법을 제공 합니다. 사용자가 작업 표시줄에 보조 타일을 "고정" 할지 여부를 선택 하더라도 응용 프로그램의 pinnable 영역은 개발자에 의해 결정 됩니다. 자세한 지침은 [보조 타일 지침](secondary-tiles-guidance.md)을 참조 하세요.


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. API가 있는지 확인 하 고 제한 된 액세스의 잠금을 해제 합니다.

이전 장치에는 Api 고정 (이전 버전의 Windows 10을 대상으로 하는 경우)이 없습니다. 따라서 이러한 장치에 고정할 수 없는 고정 단추를 표시 해서는 안 됩니다.

또한이 기능은 제한 된 액세스 상태에서 잠깁니다. 액세스 권한을 얻으려면 Microsoft에 문의 하세요. **[TaskRequestPinSecondaryTileAsync](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**, **[TaskTryUnpinSecondaryTileAsync. IsSecondaryTilePinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 및 **[TASK 관리자](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 에 대 한 API 호출이 실패 하 고 액세스 거부 예외가 발생 합니다. 앱은 권한 없이이 API를 사용할 수 없으며 API 정의는 언제 든 지 변경 될 수 있습니다.

[Apiinformation. IsMethodPresent](/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) 메서드를 사용 하 여 api가 있는지 확인 합니다. 그런 다음 **[LimitedAccessFeatures](/uwp/api/windows.applicationmodel.limitedaccessfeatures)** api를 사용 하 여 api의 잠금을 해제 합니다.

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. TaskbarManager 인스턴스 가져오기

Windows 앱은 다양 한 장치에서 실행할 수 있습니다. 일부는 작업 표시줄을 지원 하지 않습니다. 현재는 데스크톱 장치만 작업 표시줄을 지원 합니다. 또한 작업 표시줄이 있을 수 있습니다. 작업 표시줄이 현재 존재 하는지 여부를 확인 하려면 **[task ](/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 메서드를 호출 하 고 반환 된 인스턴스가 null이 아닌 지 확인 합니다. 작업 표시줄이 없으면 고정 단추를 표시 하지 않습니다.

단일 작업 (예: 고정)의 기간 동안 인스턴스를 유지 한 다음, 다음에 다른 작업을 수행 해야 할 때 새 인스턴스를 선택 하는 것이 좋습니다.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. 타일이 현재 작업 표시줄에 고정 되어 있는지 확인 합니다.

타일이 이미 고정 되어 있는 경우 대신 고정 해제 단추를 표시 해야 합니다. **[IsSecondaryTilePinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 메서드를 사용 하 여 타일이 현재 고정 되어 있는지 여부를 확인할 수 있습니다 (사용자가 언제 든 지 고정 해제할 수 있음). 이 방법에서는 알고 싶은 타일의 **TileId** 가 고정 되어 전달 됩니다.

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. 고정이 허용 되는지 확인 합니다.

그룹 정책 하 여 작업 표시줄에 고정을 사용 하지 않도록 설정할 수 있습니다. [TaskIsPinningAllowed](/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) 속성을 사용 하 여 고정이 허용 되는지 여부를 확인할 수 있습니다.

사용자가 pin 단추를 클릭 하면이 속성을 확인 하 고, false 이면이 컴퓨터에서 고정이 허용 되지 않는다는 내용의 메시지 대화 상자를 표시 해야 합니다.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. 타일 구성 및 고정

사용자가 pin 단추를 클릭 하 여 Api가 있고, 작업 표시줄이 있으며, 고정이 허용 되는 것으로 확인 되었습니다. 고정 시간

먼저를 시작에 고정 하는 것과 마찬가지로 보조 타일을 구성 합니다. 보조 타일 속성에 대 한 자세한 내용은 [시작 하려면 Pin 보조 타일을](secondary-tiles-pinning.md)참조 하세요. 그러나 작업 표시줄에 고정 하는 경우 이전에 필요한 속성 외에 Square44x44Logo (작업 표시줄에 사용 되는 로고)도 필요 합니다. 그렇지 않으면 예외가 throw됩니다.

그런 다음 **[RequestPinSecondaryTileAsync](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 메서드에 타일을 전달 합니다. 이는 액세스가 제한 된 상태 이므로 확인 대화 상자를 표시 하지 않으며 UI 스레드를 요구 하지 않습니다. 그러나이 기능이 제한 된 액세스를 초과 하 여 열리면 호출자는 제한 된 액세스를 활용 하지 않는 호출자가 대화를 받고 UI 스레드를 사용 해야 합니다.

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

이 메서드는 현재 타일이 작업 표시줄에 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 이미 고정 된 경우 메서드는 기존 타일을 업데이트 하 고 true를 반환 합니다. 고정이 허용 되지 않거나 작업 표시줄이 지원 되지 않는 경우이 메서드는 false를 반환 합니다.


## <a name="enumerate-tiles"></a>타일 열거

만든 모든 타일을 표시 하 고 (시작, 작업 표시줄 또는 둘 다) 어딘가에 계속 고정 하려면 **[FindAllAsync](/uwp/api/windows.ui.startscreen.secondarytile.findallasync)** 를 사용 합니다. 이후에 이러한 타일이 작업 표시줄에 고정 되어 있는지 또는 시작에 고정 되어 있는지 확인할 수 있습니다. 서피스가 지원 되지 않는 경우 이러한 메서드는 false를 반환 합니다.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>타일 업데이트

이미 고정 된 타일을 업데이트 하려면 [보조 타일 업데이트](secondary-tiles-pinning.md#updating-a-secondary-tile)에 설명 된 대로 [**UpdateAsync**](/uwp/api/windows.ui.startscreen.secondarytile.updateasync) 메서드를 사용할 수 있습니다.


## <a name="unpin-a-tile"></a>타일 고정 해제

타일이 현재 고정 되어 있는 경우 앱은 고정 해제 단추를 제공 해야 합니다. 타일의 고정을 해제 하려면 **[TryUnpinSecondaryTileAsync](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 를 호출 하 고 고정 해제 하려는 보조 타일의 **TileId** 을 전달 합니다.

이 메서드는 타일이 더 이상 작업 표시줄에 고정 되지 않는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 처음에 고정 되지 않은 경우에도 true가 반환 됩니다. 고정이 허용 되지 않는 경우 false를 반환 합니다.

타일이 작업 표시줄에 고정 되어 있는 경우 더 이상 아무 곳에도 고정 되지 않으므로 타일이 삭제 됩니다.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>타일 삭제

어디에서 나 (시작, 작업 표시줄) 타일의 고정을 해제 하려면 **[RequestDeleteAsync](/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 메서드를 사용 합니다.

이는 사용자가 고정 한 콘텐츠가 더 이상 적용 되지 않는 경우에 적합 합니다. 예를 들어 앱이 시작 및 작업 표시줄에 노트북을 고정 한 다음 사용자가 노트북을 삭제 하면 노트북에 연결 된 타일을 삭제 하기만 하면 됩니다.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>시작에서만 고정 해제

작업 표시줄을 종료 하는 동안에만 보조 타일의 고정을 해제 하려는 경우 **[Startscreenmanager](/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 메서드를 호출할 수 있습니다. 이렇게 하면 타일이 더 이상 다른 표면에 고정 되지 않은 경우에도 삭제 됩니다.

이 메서드는 타일이 더 이상 시작에 고정 되지 않는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 처음에 고정 되지 않은 경우에도 true가 반환 됩니다. 허용 되지 않음 또는 시작이 지원 되지 않는 경우 false를 반환 합니다.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>리소스

* [Task바 관리자 클래스](/uwp/api/windows.ui.shell.taskbarmanager)
* [시작에 보조 타일 고정](secondary-tiles-pinning.md)
