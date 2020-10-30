---
description: 보조 타일을 고정 하는 것 처럼 프로그래밍 방식으로 자신의 앱 기본 타일을 고정 하 여 시작할 수 있습니다. 현재 고정 되어 있는지 여부를 확인할 수 있습니다.
title: 기본 타일 API
label: Primary tile API's
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, StartScreenManager, 고정 기본 타일, 기본 타일 api, 타일 고정 여부 확인, 라이브 타일
ms.localizationpriority: medium
ms.openlocfilehash: 83cf11d80ffcd03148cbe5e784aaad5836357796
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029696"
---
# <a name="primary-tile-apis"></a>기본 타일 API
 

기본 타일 Api를 사용 하면 앱이 현재 시작에 고정 되어 있는지 여부를 확인 하 고 앱의 기본 타일을 고정 하도록 요청할 수 있습니다.

> [!IMPORTANT]
> **작성자 업데이트 필요** : 기본 타일 api를 사용 하려면 SDK 15063을 대상으로 하 고 빌드 15063 이상을 실행 해야 합니다.

> **중요 한 api** : [**startscreenmanager 클래스**](/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [requestadd사과 istentryasync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>기본 타일 Api를 사용 하는 경우

앱의 기본 타일에 대 한 훌륭한 환경을 디자인 하는 데 많은 노력을 기울여야 하며 이제 사용자에 게 시작에 고정 하도록 요청할 수 있습니다. 그러나 코드를 살펴보기 전에 환경을 디자인 하는 경우 다음 사항을 염두에 두어야 합니다.

* 작업에 대 한 명확한 "라이브 타일 고정" 호출을 사용 하 여 앱에서 비 중단 및 쉽게 해제 가능한 UX **를 만들 수** 있습니다.
* 사용자에 게 pin을 요청 하기 전에 앱의 라이브 타일 값 **을 명확 하** 게 설명 합니다.
* 타일이 이미 고정 되어 있거나 장치에서 지원 하지 않는 경우 사용자에 게 앱 타일을 고정 하도록 요청 **하지 마세요** (자세한 정보).
* 응용 프로그램의 타일을 고정 하도록 사용자에 게 질문 **하지 않습니다** (표정이 될 수 있음).
* 명시적인 사용자 개입 없이 또는 앱이 최소화 되거나 열리지 않는 경우 pin API를 호출 하지 **마세요** .


## <a name="checking-whether-the-apis-exist"></a>API의 존재 여부 확인

앱에서 이전 버전의 Windows 10을 지 원하는 경우 이러한 기본 타일 Api를 사용할 수 있는지 여부를 확인 해야 합니다. ApiInformation을 사용 하 여이 작업을 수행 합니다. 기본 타일 Api를 사용할 수 없는 경우 Api에 대 한 호출을 실행 하지 마십시오.

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


## <a name="check-if-start-supports-your-app"></a>시작이 앱을 지원 하는지 확인

현재 시작 메뉴와 앱 유형에 따라 현재 시작 화면에 앱을 고정 하는 것은 지원 되지 않을 수 있습니다. 바탕 화면 및 모바일 지원만 기본 타일을 고정 하 여 시작 합니다. 따라서 pin UI를 표시 하거나 pin 코드를 실행 하기 전에 먼저 현재 시작 화면에 대해 앱이 지원 되는지 확인 해야 합니다. 지원 되지 않는 경우 사용자에 게 타일을 고정 하 라는 메시지를 표시 하지 않습니다.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>현재 고정 되어 있는지 여부를 확인 합니다.

주 타일이 현재 시작에 고정 되어 있는지 확인 하려면 [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) 메서드를 사용 합니다.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>기본 타일 고정

기본 타일이 현재 고정 되어 있지 않고 타일이 Start에서 지원 되는 경우 사용자에 게 기본 타일을 고정할 수 있는 팁을 표시 하는 것이 좋습니다.

> [!NOTE]
> 응용 프로그램이 전경에 있는 동안에는 UI 스레드에서이 API를 호출 해야 하며, 사용자가 의도적으로 기본 타일을 고정 하도록 요청한 후에만이 API를 호출 해야 합니다. 예를 들어 사용자가 타일을 고정 하는 방법에 대 한 팁으로 사용자를 클릭 한 후에이 api를 호출 해야 합니다.

사용자가 단추를 클릭 하 여 기본 타일을 고정 한 다음 [Requestadd사과 Istentryasync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) 메서드를 호출 하 여 타일이 시작 부분에 고정 되도록 요청 합니다. 그러면 사용자가 타일을 시작 하도록 고정 하 시겠습니까를 확인 하는 대화 상자가 표시 됩니다.

이제 타일이 시작에 고정 되어 있는지 여부를 나타내는 부울이 반환 됩니다. 타일이 이미 고정 된 경우 사용자에 게 대화 상자를 표시 하지 않고 즉시 true가 반환 됩니다. 사용자가 대화 상자에서 아니요를 클릭 하거나 시작 화면에 고정이 지원 되지 않는 경우 false가 반환 됩니다. 그렇지 않으면 사용자가 예를 클릭 하 고 타일이 고정 되었으며 API는 true를 반환 합니다.

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
* [적응 타일 설명서](create-adaptive-tiles.md)
