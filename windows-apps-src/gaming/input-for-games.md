---
title: 게임용 입력 시스템
description: 이 섹션에서는 gamepads 및 기타 UWP (입력 유니버설 Windows 플랫폼 장치) 게임을 사용 하는 방법을 보여 줍니다.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 입력
ms.localizationpriority: medium
ms.openlocfilehash: 0c9565f02356b776738bb325eb9a29e84582f4ad
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163097"
---
# <a name="input-for-games"></a>게임용 입력 시스템

이 섹션에서는 Windows 10 및 Xbox One에서 UWP (유니버설 Windows 플랫폼) 게임에서 사용할 수 있는 다양 한 종류의 입력 장치에 대해 설명 하 고, 기본적인 사용 방법을 보여 주고, 게임의 효과적인 입력 프로그래밍에 대 한 패턴과 기술을 제안 합니다.

> **참고**    다른 종류의 입력 장치는 다른 종류의 입력 장치를 사용 하 여 사용자 지정 입력 장치 (예: 장르 관련 또는 게임 관련) 일 수 있습니다. 이러한 장치 및 해당 프로그래밍은이 섹션에서 다루지 않습니다. 사용자 지정 입력 장치를 용이 하 게 하는 데 사용 되는 인터페이스에 대 한 자세한 내용은 [Windows. 사용자 지정](/uwp/api/windows.gaming.input.custom) 네임 스페이스를 참조 하세요.

## <a name="gaming-input-devices"></a>게임 입력 장치

게임 입력 장치 [는 windows 10](/uwp/api/windows.gaming.input) 및 XBOX 용 UWP 게임 및 앱에서 지원 됩니다.

### <a name="gamepads"></a>Gamepads

Gamepads는 Xbox One의 표준 입력 장치 이며 키보드와 마우스를 선호 하지 않는 경우 Windows 게이머를 위한 일반적인 선택입니다. 이러한 컨트롤은 거의 모든 종류의 게임에 적합 하 게 하는 다양 한 디지털 및 아날로그 컨트롤을 제공 하 고 포함 된 진동 모터를 통해 tactile 피드백을 제공 합니다.

UWP 게임에서 gamepads를 사용 하는 방법에 대 한 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md)(영문)을 참조 하세요.

### <a name="arcade-sticks"></a>아케이드 스틱

아케이드 스틱은 모든 디지털 입력 장치 이며,이는 대량 아케이드 컴퓨터의 느낌을 재현 하는 데 사용할 수 있는 것이 고, 헤드를 위한 완벽 한 형식의 게임을 위한 완벽 한 입력 장치입니다.

UWP 게임에서 아케이드 스틱을 사용 하는 방법에 대 한 자세한 내용은 [아케이드 스틱](arcade-stick.md)을 참조 하세요.

### <a name="racing-wheels"></a>레이스 휠

경주 바퀴는 진정한 racecar 면에서와 유사한 입력 장치 이며 자동차 또는 트럭을 지 원하는 레이스 게임을 위한 완벽 한 입력 장치입니다. 많은 레이스 휠에는 진정한 강제 피드백이 포함 되어 있습니다. 즉, 간단한 진동 뿐만 아니라 조종 휠 등의 제어 축에 실제 힘을 적용할 수 있습니다.

UWP 게임에서 경주 바퀴를 사용 하는 방법에 대 한 자세한 내용은 [레이스 휠 및 피드백 적용](racing-wheel-and-force-feedback.md)을 참조 하세요.

### <a name="flight-sticks"></a>비행 스틱

비행 스틱은 비행기 또는 spaceship의 환경에서 발견 될 비행 스틱 느낌을 재현 하는 게임 입력 장치입니다. 이러한 장치는 항공편을 빠르고 정확 하 게 제어 하기 위한 완벽 한 입력 장치입니다.

UWP 게임에서 비행기를 사용 하는 방법에 대 한 자세한 내용은 [비행 스틱](flight-stick.md)을 참조 하세요.

### <a name="raw-game-controllers"></a>원시 게임 컨트롤러

원시 게임 컨트롤러는 다양 한 종류의 일반적인 게임 컨트롤러에서 제공 되는 입력을 사용 하는 게임 컨트롤러를 일반적으로 표현한 것입니다. 이러한 입력은 명명 되지 않은 단추, 스위치 및 축의 간단한 배열로 표시 됩니다. 원시 게임 컨트롤러를 사용 하 여 고객이 사용 중인 컨트롤러의 유형에 관계 없이 사용자 지정 입력 매핑을 만들 수 있습니다.

UWP 게임에서 raw game controller를 사용 하는 방법에 대 한 자세한 내용은 [raw game controller](raw-game-controller.md)를 참조 하세요.

### <a name="ui-navigation-controllers"></a>UI 탐색 컨트롤러

UI 탐색 컨트롤러는 다양 한 게임 및 물리적 입력 장치에서 일관 된 사용자 환경을 촉진 하는 UI 탐색 명령에 대 한 공통 용어를 제공 하기 위해 존재 하는 논리적 입력 장치입니다. 게임의 사용자 인터페이스는 장치 특정 인터페이스 대신 UINavigationController 인터페이스를 사용 해야 합니다.

UWP 게임에서 UI 탐색 컨트롤러를 사용 하는 방법에 대 한 자세한 내용은 [ui 탐색 컨트롤러](ui-navigation-controller.md)를 참조 하세요.

### <a name="headsets"></a>헤드세트

헤드셋은 입력 장치를 통해 연결 된 경우 특정 사용자와 연결 된 오디오 캡처 및 재생 장치입니다. 일반적으로 음성 채팅을 위한 온라인 게임에서 사용 되지만 집중 교육를 개선 하거나 온라인 및 오프 라인 게임 모두에서 게임 기능을 제공 하는 데 사용할 수도 있습니다.

UWP 게임에서 헤드셋을 사용 하는 방법에 대 한 자세한 내용은 [헤드셋](headset.md) 을 참조 하세요.

### <a name="users"></a>사용자

각 입력 장치 및 연결 된 헤드셋을 특정 사용자와 연결 하 여 해당 id를 게임에 연결할 수 있습니다. 사용자 id는 또한 물리적 입력 장치의 입력이 논리적 UI 탐색 컨트롤러의 입력과 상관 관계를 지정 하는 수단입니다.

사용자 및 해당 입력 장치를 관리 하는 방법에 대 한 자세한 내용은 [사용자 및 장치 추적](input-practices-for-games.md#tracking-users-and-their-devices)을 참조 하세요.

## <a name="see-also"></a>관련 항목

* [게임용 입력 시스템](input-practices-for-games.md)
* [Windows 게이밍. 입력 네임 스페이스](/uwp/api/windows.gaming.input)
* [Windows. 사용자 지정 네임 스페이스](/uwp/api/windows.gaming.input.custom)