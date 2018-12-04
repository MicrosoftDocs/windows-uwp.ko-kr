---
Description: Learn how to pin secondary tiles to taskbar.
title: 작업 표시줄에 보조 타일 고정
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10, uwp, 보조 타일, 작업 표시줄에 고정 작업 표시줄에 보조 타일을 고정할 바로 가기
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8474925"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>작업 표시줄에 보조 타일 고정

시작 화면에 보조 타일을 고정할 때와 마찬가지로 사용자가 빠른에 액세스할 수 있도록 콘텐츠 앱 내에서 보조 타일, 작업 표시줄에 고정할 수 있습니다.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **제한 된 액세스 API**:이 API는 제한 된 액세스 기능입니다. 이 API를 사용 하려면 문의 [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **필요 2018 년 10 월 업데이트**: SDK 17763을 대상으로 하 고 작업 표시줄에 고정 하려면 빌드 17763 이상을 실행 합니다.


## <a name="guidance"></a>지침

보조 타일에는 사용자가 앱 내의 특정 영역에 직접 액세스할 수 있는 일관적이 고 효율적인 방법을 제공 합니다. 사용자가 보조 타일을 작업 표시줄에 "고정" 것인지 여부를 선택 하지만 앱에서 고정 가능한 영역은 개발자가 결정 합니다. 자세한 지침은 [보조 타일 지침](secondary-tiles-guidance.md)을 참조 하세요.


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. API의 존재 하는지 확인 하 고 제한 된 액세스 잠금 해제

오래 된 장치 (사용 하려는 경우 이전 버전의 Windows 10) Api를 고정 작업 표시줄이 필요가 없습니다. 따라서 고정 수 없는 이러한 디바이스에서 pin 단추를 표시 하지 않아야 합니다.

또한이 기능은 제한 된 액세스에서 잠겨 있습니다. 권한을 얻으려면 Microsoft에 문의 합니다. API 호출을 **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**, **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 및 **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 액세스가 거부 예외와 함께 실패 합니다. 앱 권한이 없는 경우이 API를 사용 하 여 허용 되지 않으며 API 정의 언제 든 지 변경 될 수 있습니다.

Api의 존재 하는 경우 확인 하기 위해 [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) 메서드를 사용 합니다. 및 다음 **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API 사용 하 여 API를 잠금 해제를 시도 합니다.

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

UWP 앱은 다양한 장치에서 실행 가능하지만 모든 장치가 작업 표시줄을 지원하는 것은 아닙니다. 현재는 데스크톱 장치만 작업 표시줄을 지원합니다. 또한 작업 표시줄의 존재 여부와 하며 이동 될 수 있습니다. 작업 표시줄 현재 있는지 확인 **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 메서드를 호출 하 고 반환 된 인스턴스를 확인 하는 null 되지 않습니다. 작업 표시줄 있으면 핀 단추를 표시 하지 마십시오.

인스턴스를 잡고를 기간을 고정 하 고 다음 새 인스턴스를 다른 작업을 수행 해야 하는 다음 번 끌어와 같은 단일 작업을 사용 하는 것이 좋습니다.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. 타일 현재 작업 표시줄에 고정 되어 있는지 확인

타일이 이미 고정 하는 경우 대신 고정 해제 단추를 표시 해야 합니다. **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 메서드를 사용 하 여 타일이 현재 고정 되어 있는지 여부를 확인할 수 있습니다 (사용자 수 제거 언제 든 지). 이 메서드를 확인 하려면 타일의 **TileId** 전달 고정 되어 있습니다.

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


## <a name="4-check-whether-pinning-is-allowed"></a>4. 고정의 허용 여부 확인

그룹 정책에 따라 작업 표시줄에 고정 해제할 수 있습니다. [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) 속성 고정의 허용 여부를 확인할 수 있습니다.

사용자가 pin 단추를 클릭 하면이 속성을 확인 해야 하 고 false 인 경우 고정이 컴퓨터에 허용 되지 않은 사용자에 게 알려주는 메시지 대화 상자를 표시 해야 합니다.

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


## <a name="5-construct-and-pin-your-tile"></a>5. 생성 및 타일 고정

사용자가 pin 단추를 클릭 하 고 Api를 사용할 수 있는지, 작업 표시줄을 사용할 수 있는지를... 허용 되는 고정 결정 했으면 pin 시간부터!

시작에 고정할 때 것에 먼저 보조 타일을 구성 합니다. [시작 화면에 보조 타일 고정을](secondary-tiles-pinning.md)참조 하 여 보조 타일 속성에 대해 자세히 알아볼 수 있습니다. 그러나 이전에 필요한 속성 외에도 작업 표시줄에 고정할 때 Square44x44Logo (이것이 작업 표시줄에서 사용 되는 로고) 역시 필요 합니다. 그렇지 않으면 예외가 throw 됩니다.

그런 다음 타일 **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 메서드에 전달 합니다. 제한 된 액세스에서 이것이 확인 대화 상자가 표시 되지 않습니다이 나타내고 UI 스레드는 필요 하지 않습니다. 하지만 나중에이 열릴 때 넘어 액세스가 제한 된 호출자는 대화 상자가 표시 되며 UI 스레드를 사용 하는 데 필요한 액세스가 제한 된을 활용 하지 합니다.

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

이 메서드는 타일 작업 표시줄에 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 이미 고정 된 경우 메서드는 기존 타일을 업데이트 하 고 true를 반환 합니다. 고정이 허용 되지 않거나 지원 되지 않는 작업 표시줄, 메서드는 false를 반환 합니다.


## <a name="enumerate-tiles"></a>타일 열거

만들고 여전히 아무 곳 이나 고정 된 모든 타일을 확인 하려면 (시작, 작업 표시줄, 또는 둘 다)는 **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)** 를 사용 합니다. 이후에이 타일은 작업 표시줄 및/또는 시작에 고정 되어 있는지 확인할 수 있습니다. 표면 지원 되지 않는 경우 이러한 메서드는 false를 반환 합니다.

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

이미 고정 된 타일을 업데이트 하려면 [보조 타일 업데이트](secondary-tiles-pinning.md#updating-a-secondary-tile)에 설명 된 대로 [**SecondaryTile.UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) 메서드를 사용할 수 있습니다.


## <a name="unpin-a-tile"></a>타일을 고정 하거나 고정 해제

앱은 타일이 현재 고정 되어 있으면 고정 해제 단추를 제공 해야 합니다. 타일을 고정 하거나 고정 해제, **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 를 만들려고 원하는 보조 타일의 **TileId** 전달 하 여 호출 하면 됩니다.

이 메서드는 타일은 더 이상 작업 표시줄에 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 처음에 고정 되지 않은 경우이 true를 반환 합니다. 고정 해제 허용 되지 않은 경우이 false를 반환 합니다.

타일에만 작업 표시줄에 고정 된 경우 더 이상 고정 어디서 나 이후 타일을 삭제 됩니다.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>타일을 삭제 합니다.

모든 곳에서 (시작, 작업 표시줄)에서 타일을 고정 하거나 고정 해제 하려는 경우 **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 메서드를 사용 합니다.

콘텐츠 고정 되어 사용자가 해당 더 이상 경우 적합 합니다. 예를 들어 앱 시작 및 작업 표시줄에 노트북을 고정할 수 있습니다. 노트를 삭제 한 다음을 노트와 관련 된 타일을 삭제 간단 하 게 해야 합니다.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>시작에서 제거

작업 표시줄에 유지 하는 동안 시작 화면에서 보조 타일을 고정 해제 하려는 경우 **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 메서드를 호출할 수 있습니다. 마찬가지로 다른 화면에 고정 되어 더 이상 경우 타일을 삭제 됩니다.

이 메서드는 타일은 더 이상 시작에 고정 되어 있는지 여부를 나타내는 부울 값을 반환 합니다. 타일이 처음에 고정 되지 않은 경우이 true를 반환 합니다. 허용 되지 않았다면 고정 해제 또는 시작 지원 되지 않는이 false를 반환 합니다.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>리소스

* [TaskbarManager 클래스](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [시작 화면에 보조 타일 고정](secondary-tiles-pinning.md)