---
title: UWP에서 Windows 시스템 사용자 검색
author: KevinAsgari
description: 유니버설 Windows 플랫폼 (UWP) 게임에서 Windows 시스템 사용자 검색 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 06/07/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 시스템 사용자
ms.localizationpriority: medium
ms.openlocfilehash: 0533e9f63453ee2a7a8a7cb77f6f253e2d9c0e23
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5435769"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>유니버설 Windows 플랫폼 (UWP) 제목에 Windows 시스템 사용자 검색

## <a name="windowssystemuser"></a>Windows.System.User

[Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 개체는 로컬 Windows 사용자를 나타냅니다. Xbox 콘솔에서 여러 windows 사용자를 Windows.System.User 개체는 XboxLiveUser 라이브 서비스에 액세스 하기 위해 생성 해야 할 것 앱이 다중 사용자 응용 프로그램을 단일 대화형 세션에서 동시에 기록 될 수 있습니다. PC 또는 휴대폰 같은 다른 windows 플랫폼에서 것에 허용 하거나 windows 사용자 하나의 장치 또는 하나의 대화형 세션은 XboxLiveUser 만들려고 Windows.System.User 전달 필요 하지 않습니다.

## <a name="ways-to-retrieve-windows-system-user"></a>Windows 시스템 사용자 검색 하는 방법

* **[Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 클래스에서 정적 메서드를 사용 합니다.**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) 클래스 Windows.System.User 개체를 검색 하는 데 정적 메서드 집합을 제공 합니다. 예를 들어 모든 활성 창은 사용자를 가져와야 FindAllAsync를 호출할 수 있습니다.

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker) 클래스는 사용자가 선택기 UI를 시작 하 고 다중 사용자 시나리오에서 windows 시스템 사용자를 선택 하는 메서드를 제공 합니다.

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller) 는 모든 컨트롤러 장치 (게임 패드, 레이싱 휠, 플라이트 스틱 등)에 대 한 핵심 인터페이스입니다. 해당 사용자 속성을 호출 하 여 게임 컨트롤러와 관련 된 Windows.System.User 개체를 얻을 수 있습니다.  

  Windows [ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick) [RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel) [FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick), [게임 패드](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad)의해 구현 하는 몇 가지 게임 컨트롤러는 다음과 같습니다.

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  장치 id 사용 하 여 연결 된 사용자를 찾을 수 FindUserFromDeviceId 정적 메서드를 호출할 수 있습니다. [Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs), [Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs) 같은 windows 입력된 이벤트에서 일반적으로 장치 id를 얻을 수합니다 있습니다.
