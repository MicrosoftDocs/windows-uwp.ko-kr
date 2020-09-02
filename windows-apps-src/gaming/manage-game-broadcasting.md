---
ms.assetid: ''
description: Windows 시스템 UI 및 UWP 용 Windows 데스크톱 확장을 사용 하 여 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 게임 브로드캐스팅을 관리 하는 방법을 알아봅니다.
title: 게임 브로드캐스팅 관리
ms.date: 09/27/2017
ms.topic: article
keywords: windows 10, 게임, 브로드캐스팅
ms.localizationpriority: medium
ms.openlocfilehash: 87c35bb4612ad970f01853b2ace46b44b1882781
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362496"
---
# <a name="manage-game-broadcasting"></a>게임 브로드캐스팅 관리
이 문서에서는 UWP 앱에 대 한 게임 브로드캐스팅을 관리 하는 방법을 보여 줍니다. 사용자는 Windows에 기본 제공 되는 시스템 UI를 사용 하 여 브로드캐스팅을 시작 해야 하 고, Windows 10 버전 1709부터 응용 프로그램은 시스템 브로드캐스팅 UI를 시작 하 고 브로드캐스팅이 시작 및 중지 될 때 알림을 받을 수 있습니다.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>앱에 UWP 용 Windows 데스크톱 확장 추가
응용 프로그램 브로드캐스팅을 관리 하기 위한 Api는 **[Windows. p. AppBroadcasting](/uwp/api/windows.media.appbroadcasting)** 네임 스페이스에 있습니다. Api에 액세스 하려면 다음 단계를 사용 하 여 UWP에 대 한 Windows 데스크톱 확장에 대 한 참조를 앱에 추가 해야 합니다.

1. Visual Studio의 **솔루션 탐색기**에서 UWP 프로젝트를 확장 하 고 **참조** 를 마우스 오른쪽 단추로 클릭 한 다음 **참조 추가**...를 선택 합니다. 
2. **유니버설 Windows** 노드를 확장 하 고 **확장**을 선택 합니다.
3. 확장 목록에서 프로젝트에 대 한 대상 빌드와 일치 하는 **UWP 항목에 대 한 Windows 데스크톱 확장** 옆의 확인란을 선택 합니다. 앱 브로드캐스트 기능의 경우 버전은 1709 이상 이어야 합니다.
4. **확인**을 클릭합니다.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>사용자가 브로드캐스팅을 시작할 수 있도록 시스템 UI를 시작 합니다.
현재 장치가 브로드캐스팅을 위한 하드웨어 요구 사항을 충족 하지 않거나 다른 앱이 현재 브로드캐스팅 중인 경우를 포함 하 여 앱에서 현재 브로드캐스트할 수 없는 몇 가지 이유가 있습니다. 시스템 UI를 시작 하기 전에 앱이 현재 브로드캐스트할 수 있는지 확인할 수 있습니다. 먼저 현재 장치에서 브로드캐스트 Api를 사용할 수 있는지 확인 합니다. Windows 10, 버전 1709 이전 버전의 OS를 실행 하는 장치에서는 Api를 사용할 수 없습니다. 특정 OS 버전을 확인 하는 대신 **[IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 메서드를 사용 하 여 *AppBroadcastingContract* 버전 1.0을 쿼리 합니다. 이 계약이 있는 경우 장치에서 브로드캐스팅 Api를 사용할 수 있습니다.

그런 다음 한 번에 한 명의 사용자가 로그인 한 경우 PC에서 팩터리 메서드 **[Getdefault](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** 를 호출 하 여 **[AppBroadcastingUI](/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** 클래스의 인스턴스를 가져옵니다. 여러 사용자가 로그인 할 수 있는 Xbox의 경우 대신 **[Getforuser](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)** 를 호출 합니다. 그런 다음 **[GetStatus](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** 를 호출 하 여 앱의 브로드캐스팅 상태를 가져옵니다.

**AppBroadcastingStatus** 클래스의 **[canstartbroadcast](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** 속성은 앱이 현재 브로드캐스팅을 시작할 수 있는지 여부를 알려 줍니다. 그렇지 않으면 **[Details](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** 속성을 확인 하 여 브로드캐스트를 사용할 수 없는 이유를 확인할 수 있습니다. 이유에 따라 사용자에 게 상태를 표시 하거나 브로드캐스팅 사용에 대 한 지침을 표시할 수 있습니다.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetCanStartBroadcast":::

**[ShowBroadcastUI](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)** 를 호출 하 여 시스템에서 앱 브로드캐스트 UI를 표시 하도록 요청 합니다.

> [!NOTE] 
> **ShowBroadcastUI** 메서드는 시스템의 현재 상태에 따라 성공 하지 못할 수 있는 요청을 나타냅니다. 앱에서이 메서드를 호출한 후 브로드캐스팅을 시작 했다고 가정 하지 않아야 합니다. 브로드캐스팅이 시작 되거나 중지 될 때 **[IsCurrentAppBroadcastingChanged](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 이벤트를 사용 하 여 알림 메시지를 표시 합니다.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetLaunchBroadcastUI":::

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>브로드캐스팅이 시작 및 중지 될 때 알림 받기
사용자가 시스템 UI를 사용 하 여 **[AppBroadcastingMonitor](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** 클래스의 인스턴스를 초기화 하 고  **[IsCurrentAppBroadcastingChanged](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** 이벤트에 대 한 처리기를 등록 하 여 응용 프로그램 브로드캐스팅을 시작 하거나 중지할 때 알림을 받도록 등록 합니다. 이전 섹션에서 설명한 것 처럼 특정 시점에 **[IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 를 사용 하 여 브로드캐스트 api가 장치에 있는지 확인 해야 합니다. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetAppBroadcastingRegisterChangedHandler":::

**IsCurrentAppBroadcastingChanged** 이벤트에 대 한 처리기에서 현재 브로드캐스팅 상태를 반영 하도록 앱의 UI를 업데이트 하려고 할 수 있습니다.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetAppBroadcastingChangedHandler":::

## <a name="related-topics"></a>관련 항목

* [게임](index.md)

 

 
