---
author: drewbatgit
ms.assetid: ''
description: UWP 앱에 대한 게임 브로드캐스팅을 관리하는 방법을 보여 줍니다.
title: 게임 브로드캐스팅 관리
ms.author: drewbat
ms.date: 09/27/2017
ms.topic: article
keywords: Windows 10, 게임, 브로드캐스팅
ms.localizationpriority: medium
ms.openlocfilehash: ae70c29927925abcf948435ed768871ba2427fd9
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7435613"
---
# <a name="manage-game-broadcasting"></a>게임 브로드캐스팅 관리
이 문서는 UWP 앱에 대한 게임 브로드캐스팅을 관리하는 방법을 보여 줍니다. 사용자는 Windows에 기본 제공되는 시스템 UI를 사용하여 브로드캐스트를 시작해야 합니다. 하지만 Windows 10 버전 1709부터 앱에서 시스템 브로드캐스팅 UI를 시작하고 브로드캐스팅이 시작 및 중지될 때 알림을 받을 수 있습니다.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>앱에 UWP용 Windows 데스크톱 확장 추가
**[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** 네임스페이스에서 찾을 수 있는 앱 브로드캐스팅을 관리하기 위한 API는 유니버설 API 계약에 포함되지 않습니다. API에 액세스하려면 다음 단계를 따라 앱에 UWP용 Windows 데스크톱 확장에 대한 참조를 앱 추가해야 합니다.

1. Visual Studio에서 **솔루션 탐색기**의 UWP 프로젝트를 확장하고 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가...** 를 선택합니다. 
2. **유니버설 Windows** 노드를 확장하고 **확장**을 선택합니다.
3. 확장 목록에서 프로젝트의 대상 빌드와 일치하는 **UWP용 Windows 데스크톱 확장** 항목 옆에 있는 확인란을 선택합니다. 앱 브로드캐스트 기능의 경우 버전이 1709 이상이어야 합니다.
4. **확인**을 클릭합니다.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>사용자가 브로드캐스트를 시작할 수 있도록 시스템 UI 시작
현재 기기가 브로드캐스트의 하드웨어 요구 사항을 충족하지 않거나 다른 앱이 현재 브로드캐스팅 중인 경우를 포함하여 현재 사용자의 앱이 브로드캐스트하지 못하는 이유가 여러 가지 있을 수 있습니다. 시스템 UI를 시작하기 전에 현재 앱이 브로드캐스트를 지원하는지 확인할 수 있습니다. 먼저 브로드캐스트 API를 현재 디바이스에서 사용할 수 있는지 확인합니다. API는 Windows 10 버전 1709 이전의 운영 체제 버전을 실행하는 디바이스에서는 사용할 수 없습니다. 특정 OS 버전을 확인하는 것보다 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 메서드를 사용하여 *Windows.Media.AppBroadcasting.AppBroadcastingContract* 버전 1.0을 쿼리하는 것이 좋습니다. 이 계약이 있는 경우 브로드캐스팅 API는 디바이스에서 사용할 수 있습니다.

다음으로 한 번에 로그인한 사용자가 한 명인 PC에서 **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** 팩터리 메서드를 호출하여 **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** 클래스의 인스턴스를 가져옵니다. 여러 사용자가 로그인할 수 있는 XBox에서 **[GetForUser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)** 를 대신 호출합니다. 그런 다음 **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** 를 호출하여 앱의 브로드캐스트 상태를 가져올 수 있습니다.

**AppBroadcastingStatus** 클래스의 **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** 속성은 현재 앱이 브로드캐스트를 시작할 수 있는지 여부를 알려줍니다. 그렇지 않은 경우 **[Details](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** 속성을 통해 브로드캐스트를 사용할 수 없는 이유를 확인할 수 있습니다. 이유에 따라 사용자에게 상태를 표시하거나 브로드캐스트 사용에 대한 지침을 표시할 수 있습니다.

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

**[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)** 를 호출하여 시스템에서 앱 브로드캐스트 UI를 표시하도록 요청합니다.

> [!NOTE] 
> **ShowBroadcastUI** 메서드는 시스템의 현재 상태에 따라 성공하지 않을 수 있는 요청을 나타냅니다. 앱은 이 메서드를 호출한 뒤에 브로드캐스트가 시작된 것으로 가정하지 않아야 합니다. **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 이벤트를 사용하여 브로드캐스트가 시작하거나 중지할 때 알림을 받을 수 있습니다.

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>브로드캐스트 시작 및 중지 시 알림 수신
사용자가 시스템 UI를 사용하여 **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** 클래스의 인스턴스를 초기화하고 **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 이벤트에 대한 처리기를 등록하여 앱의 브로드캐스팅을 시작 또는 중지할 때 알림을 받도록 등록합니다. 이전 섹션에서 설명했듯이 동일한 시점에서 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 를 사용하여 브로드캐스팅 API를 사용하기 전에 해당 API가 디바이스에 있는지 확인합니다. 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

**IsCurrentAppBroadcastingChanged** 이벤트에 대한 처리기에서 현재 브로드캐스트 상태를 반영하기 위해 앱의 UI를 업데이트하고자 할 수 있습니다.

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>관련 항목

* [게임](index.md)

 

 




