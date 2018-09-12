---
author: andrewleader
Description: You can programmatically pin your own app's primary tile to Start, just like you can pin secondary tiles. And you can check whether it's currently pinned.
title: 기본 타일 API
label: Primary tile API's
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, StartScreenManager, 기본 타일 고정, 기본 타일 api, 타일이 고정되었는지 확인, live tile
ms.localizationpriority: medium
ms.openlocfilehash: 42b4c014dfd49c42497b8846e37e37af53cc3885
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3929569"
---
# <a name="primary-tile-apis"></a>기본 타일 API
 

기본 타일 API를 사용하여 현재 앱이 시작에 고정되었는지 확인하고 앱을 기본 타일에 고정하라고 요청할 수 있습니다.

> [!IMPORTANT]
> **크리에이터스 업데이트 필요**: 기본 타일 API를 사용하려면 SDK 15063을 대상으로 하고 빌드 15063 이상을 실행하고 있어야 합니다.

> **중요 API**: [**StartScreenManager class**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>기본 타일 API를 사용하는 시기

앱의 기본 타일에 대한 멋진 환경을 설계하기 위해 많은 노력을 기울였으므로 이제 사용자에게 이를 시작에 고정하라고 요청할 차례입니다. 그러나 코딩을 진행하기 전에 환경 설계 시 유념해야 할 몇 가지 사항이 있습니다.

* 작업에 대한 일반 "라이브 타일 고정" 호출로 앱에서 쉽게 해제할 수 있는 비파괴 UX를 **만드세요**.
* 사용자에게 고정을 요청하기 전에 앱의 라이브 타일의 값을 명확하게 **설명하세요**.
* 타일이 이미 고정되어 있거나 장치에서 타일을 지원하지 않는 경우 앱의 타일을 고정하라고 **요청하지 마세요**(자세한 정보는 아래 참조).
* 사용자에게 앱의 타일을 고정하라고 **반복적으로 요청하지 마세요**. 사용자가 귀찮아할 수 있습니다.
* 명시적인 사용자 상호 작용이 없거나 앱이 최소화되어 있거나 열려 있지 않은 경우 고정 API를 **호출하지 마세요**.


## <a name="checking-whether-the-apis-exist"></a>API가 있는지 여부 확인

앱에서 이전 버전의 Windows 10을 지원하는 경우 해당 기본 타일 API를 사용할 수 있는지 확인해야 합니다. 그러려면 ApiInformation을 사용합니다. 기본 타일 API를 사용할 수 없는 경우 API 호출을 실행하지 마세요.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.StartScreen.StartScreenManager"))
{
    // Primary tile API's supported!
}
else
{
    // Older version of Windows, no primary tile API's
}
```


## <a name="check-if-start-supports-your-app"></a>시작에서 앱을 지원하는지 확인

현재 시작 메뉴와 앱 유형에 따라 현재 시작 화면에 앱을 고정하지 못할 수 있습니다. 데스크톱과 모바일만 시작에 기본 타일 고정을 지원합니다. 따라서 고정 UI를 표시하거나 고정 코드를 실행하기 전에 현재 시작 화면에 대해 앱이 지원되는지 확인해야 합니다. 지원되지 않는 경우 사용자에게 타일을 고정할지 묻는 메시지를 표시하지 마세요.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>현재 고정되어 있는지 확인

기본 타일이 시작에 현재 고정되어 있는지 확인하려면 [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) 메서드를 사용합니다.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>기본 타일 고정

기본 타일이 현재 고정되어 있지 않으며 시작에서 타일을 지원하는 경우 기본 타일을 고정할 수 있다는 설명을 사용자에게 표시할 수 있습니다.

> [!NOTE]
> 앱이 포그라운드에 있는 동안 UI 스레드에서 이 API를 호출해야 합니다. 또한 사용자가 타일 고정에 대한 설명에 대해 예라고 답하는 경우와 같이 기본 타일 고정을 확실히 요청한 후에만 이 API를 호출해야 합니다.

사용자가 기본 타일을 고정하는 단추를 클릭하면 [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) 메서드를 호출하여 시작에 타일 고정을 요청합니다. 타일을 시작에 고정할지 다시 묻는 대화 상자가 나타납니다.

타일이 시작에 고정되어 있는지 여부를 나타내는 부울이 반환됩니다. 타일이 이미 고정된 경우 사용자에게 대화 상자가 표시되지 않고 바로 true가 반환됩니다. 사용자가 대화 상자에서 아니요를 클릭하거나 시작에 타일 고정이 지원되지 않는 경우 false가 반환됩니다. 그렇지 않고 사용자가 예를 클릭했고 타일이 고정되어 있으면 API가 true를 반환합니다.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* [작업 표시줄에 고정](../pin-to-taskbar.md)
* [타일, 배지 및 알림](index.md)
* [적응형 타일 설명서](create-adaptive-tiles.md)
