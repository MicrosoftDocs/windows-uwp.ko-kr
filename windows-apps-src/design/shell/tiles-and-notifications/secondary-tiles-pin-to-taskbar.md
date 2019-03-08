---
Description: 작업 표시줄에 보조 타일을 고정 하는 방법에 알아봅니다.
title: 작업 표시줄에 핀 보조 타일
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: 작업 표시줄에 보조 타일을 고정 하는 windows 10, uwp, 작업 표시줄, 보조 타일에 고정 바로 가기
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591978"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>작업 표시줄에 핀 보조 타일

시작 하려면 보조 타일을 고정 마찬가지로 보조 타일의 작업 표시줄에 액세스할 수 있도록 사용자에 게 빠른 콘텐츠를 앱 내에서 고정할 수 있습니다.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API 액세스를 제한**: 이 API에는 제한 된 액세스 기능입니다. 이 API를 사용 하려면 문의 하세요 [ taskbarsecondarytile@microsoft.com ](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)합니다.

> **2018 년 10 월 업데이트 필요**: SDK 17763 대상으로 지정 해야 하 고 작업 표시줄에 고정 하려면 빌드 17763 이상을 실행 합니다.


## <a name="guidance"></a>지침

보조 타일이 앱 내에서 특정 영역에 직접 액세스 하는 사용자를 일관 되 고 효율적인 방법을 제공 합니다. 사용자가 "고정" 작업 표시줄에 보조 타일 것인지 여부를 선택 하지만 앱에서 개발 영역 개발자가 결정 됩니다. 자세한 지침은 [보조 타일 지침](secondary-tiles-guidance.md)합니다.


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. API가 있는지 확인 하 고 제한 된 액세스를 잠금 해제

이전 장치 (Windows 10의 이전 버전을 대상으로 하는) 하는 경우 Api를 고정 작업 표시줄이 필요가 없습니다. 따라서 고정 수 없는 이러한 장치에서 pin 단추를 표시 하지 않아야 합니다.

또한이 기능은 제한 된 액세스에서 잠겨 있습니다. 액세스 권한을 얻으려면 Microsoft에 문의 합니다. API 호출  **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)** 합니다  **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**, 및 **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 액세스 거부 예외와 함께 실패 합니다. 앱 권한이 없으면이 API를 사용 하도록 허용 되지 않으며 API 정의 언제 든 지 변경 될 수 있습니다.

사용 된 [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) Api 있는지 확인 하는 방법입니다. 사용 하 여 합니다 **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API는 API를 잠금 해제 시도를 합니다.

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


## <a name="2-get-the-taskbarmanager-instance"></a>2. TaskbarManager 인스턴스 얻기

UWP 앱은 다양한 장치에서 실행 가능하지만 모든 장치가 작업 표시줄을 지원하는 것은 아닙니다. 현재는 데스크톱 장치만 작업 표시줄을 지원합니다. 또한 작업 표시줄의 현재 상태 들어오고 나갈 수 있습니다. 작업 표시줄에 현재 있는 인지를 확인 하려면 호출을 **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 메서드와 확인 반환 되는 인스턴스는 null이 아닙니다. 작업 표시줄에 없는 경우에 고정 단추를 표시 하지 마십시오.

고정 한 다음를 클릭 한 다음 새 인스턴스를 다른 작업을 수행 해야 할 다음 시간 등의 단일 작업 기간에 대 한 보관을 인스턴스에 사용 하는 것이 좋습니다.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. 타일이 작업 표시줄에 현재 고정 되어 있는지 여부를 확인 합니다.

이미 고정 된 타일을 고정 해제 단추를 대신 표시 되 있습니다. 사용할 수는 **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 타일이 현재 고정 되어 있는지 여부를 확인 하는 방법 (사용자가 제거할 수 언제 든 지). 전달이 메서드에서 **TileId** 알고 싶은 타일의 고정 됩니다.

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


## <a name="4-check-whether-pinning-is-allowed"></a>4. 고정 허용 되는지 여부를 확인 합니다.

작업 표시줄에 고정 하는 그룹 정책을 통해 해제할 수 있습니다. 합니다 [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) 속성을 통해 고정 허용 되는지 여부를 확인할 수 있습니다.

이 속성을 확인 해야 사용자가 pin 단추를 클릭 하 고 고정이 컴퓨터에서 허용 되지 않는 사용자에 게 알리는 메시지 대화 상자를 표시 해야 false 인 경우.

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


## <a name="5-construct-and-pin-your-tile"></a>5. 생성 하 고 타일 고정

사용자가 pin 단추를 클릭 하 고는 Api를 사용할 수 있는, 작업 표시줄을 사용할 수 있는지 및 허용 되는 고정으로 판단 하는 중... 고정 시간!

먼저 시작에 고정 하는 경우 마찬가지로 보조 타일을 생성 합니다. 참조 하 여 보조 타일 속성에 대해 자세히 알아보십시오 [핀 보조 타일 시작](secondary-tiles-pinning.md)합니다. 그러나 이전에 필요한 속성 외에도 작업 표시줄에 고정 하는 경우 (이 작업 표시줄에서 사용 하는 로고) Square44x44Logo도 필요 합니다. 그렇지 않으면 예외가 throw 됩니다.

그런 다음 타일을 전달 합니다 **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 메서드. 이므로이 제한 된 액세스에서이 확인 대화 상자가 표시 되지 않습니다 하 고 UI 스레드가 필요 하지 않습니다. 하지만 나중에이 열릴 때 액세스가 제한 된 초과 호출자가 제한 된 액세스를 활용 하지 않고 대화 상자를 받고 UI 스레드를 사용 해야 합니다.

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

이 메서드는 타일이 이제 작업 표시줄에 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 이미 고정 된 경우 메서드는 기존 타일을 업데이트 하 고 true를 반환 합니다. 허용 되지 않은 고정 또는 작업 표시줄에서 지원 되지 않습니다 하는 경우 메서드에서 false를 반환 합니다.


## <a name="enumerate-tiles"></a>타일을 열거 합니다.

모든 사용자가 만든 타일 및는 여전히 고정 된 위치 (시작, 작업 표시줄 또는 둘 다)를 참조 하세요.를 사용 하 여  **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)** 합니다. 이후에 시작 및/또는 작업 표시줄에는 이러한 타일이 고정 되어 있는지 여부를 확인할 수 있습니다. 화면에 지원 되지 않는 경우 이러한 메서드는 false를 반환 합니다.

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

이미 고정 된 타일을 업데이트 하려면 사용할 수 있습니다 합니다 [ **SecondaryTile.UpdateAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) 에 설명 된 대로 메서드 [보조 타일 업데이트](secondary-tiles-pinning.md#updating-a-secondary-tile)합니다.


## <a name="unpin-a-tile"></a>타일을 고정 해제

앱 타일은 현재 고정 된 경우 고정 해제 단추를 제공 해야 합니다. 타일을 고정 해제, 호출  **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 전달 합니다 **TileId** 고정 해제 하려는 보조 타일의 합니다.

이 메서드는 작업 표시줄에 타일이 더 이상 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 처음에 고정 되지 않은 경우이 true를 반환 합니다. 필터가 허용 되지 않은 경우이 false를 반환 합니다.

타일을 작업 표시줄에 고정만 하는 경우 더 이상 고정 아무 곳 이나 있으므로 타일을 삭제 됩니다.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>타일 삭제

모든 범위 (시작, 작업 표시줄)에서 타일을 고정 해제 하려는 경우 사용 합니다 **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 메서드.

고정 된 사용자 콘텐츠를 더 하지 않는 경우 해당 하는 경우에 적합 합니다. 예를 들어, 앱 시작 및 작업 표시줄에 notebook을 고정할 수 있습니다. 사용자 전자 필기장을 삭제 하는 다음을 하는 경우 notebook과 사용 하 여 연결 된 타일을 삭제 하기만 하면 됩니다 해야 합니다.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>시작에서 고정 해제

작업 표시줄에서 유지 하면서부터 보조 타일을 고정 해제 하려는 경우 호출할 수 있습니다 합니다 **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 메서드. 마찬가지로 더 이상 다른 화면에 고정 되어 있는 경우 타일을 삭제 합니다.

이 메서드는 타일이 시작에 더 이상 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 처음에 고정 되지 않은 경우이 true를 반환 합니다. 필터가 허용 되지 않은 경우 시작이 지원 되지 않습니다이 false를 반환 합니다.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>리소스

* [TaskbarManager 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [시작 하려면 핀 보조 타일](secondary-tiles-pinning.md)