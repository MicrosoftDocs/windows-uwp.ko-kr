---
title: UWP의 Windows 시스템 사용자를 검색합니다.
description: 유니버설 Windows 플랫폼 (UWP) 게임에서 Windows 시스템의 사용자를 검색 하는 방법에 알아봅니다.
ms.date: 06/07/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 시스템 사용자
ms.localizationpriority: medium
ms.openlocfilehash: c46f7e98c2dea3b23beb2cec80816067d4c4e341
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632758"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>유니버설 Windows 플랫폼 (UWP) 제목에서 Windows 시스템의 사용자를 검색합니다.

## <a name="windowssystemuser"></a>Windows.System.User

A [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 개체는 로컬 Windows 사용자를 나타냅니다. Xbox 콘솔에서 여러 windows 사용자를 앱이 다중 사용자 응용 프로그램을 Windows.System.User 개체는 XboxLiveUser live 서비스에 액세스 하기 위해 생성 하는 데 필요한 경우 단일 대화형 세션에서 동시에 기록 될 수 있습니다. PC 또는 phone과 같은 다른 windows 플랫폼에서 것에 허용 하거나 하나의 windows 사용자 한 장치에서 또는 하나의 대화형 세션만 XboxLiveUser 데 Windows.System.User 전달 필요 하지 않습니다. 합니다.

## <a name="ways-to-retrieve-windows-system-user"></a>Windows 시스템 사용자를 검색 하는 방법

* **정적 메서드를 사용 하 여 [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 클래스입니다.**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 클래스 Windows.System.User 개체를 검색 하는 데 정적 메서드의 집합을 제공 합니다. 예를 들어 모든 활성 windows 사용자를 가져오려는 FindAllAsync를 호출할 수 있습니다.

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker) 클래스는 사용자 선택 UI를 시작 하 고 다중 사용자 시나리오에서 windows 시스템 사용자를 선택 하는 메서드를 제공 합니다.

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller) 모든 컨트롤러 장치 (gamepad, racing 휠, 비행 스틱 등)에 대 한 핵심 인터페이스입니다. 해당 사용자 속성을 호출 하 여 게임 컨트롤러와 연결 된 Windows.System.User 개체를 얻을 수 있습니다.  

  Windows에서 구현 하는 두 게임 컨트롤러는 다음과 같습니다 [ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick)를 [FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick)를 [Gamepad](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad)를 [RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel)합니다.

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  장치 id 사용 하 여 연결 된 사용자를 찾으려면 FindUserFromDeviceId 정적 메서드를 호출할 수 있습니다. 와 같은 windows 입력된 이벤트에서 장치 id를 일반적으로 가져올 수 있습니다 [Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs), [Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs)
