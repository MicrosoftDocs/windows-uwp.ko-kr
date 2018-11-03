---
author: mithom
title: 게임 입력 장치
description: 이 섹션에서는 UWP(유니버설 Windows 플랫폼) 게임용 게임 패드 및 기타 입력 장치를 사용하는 방법을 보여줍니다.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10 uwp, 게임, 입력
ms.localizationpriority: medium
ms.openlocfilehash: bb7d70c20aeb2b91d8a6db863e165e017810e924
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5985565"
---
# <a name="input-for-games"></a>게임용 입력 시스템

이 섹션에서는 Windows 10과 Xbox One의 UWP(유니버설 Windows 플랫폼) 게임에서 사용할 수 있는 다양한 입력 장치에 대해 설명하고 기본 사용법을 보여 주며 효과적인 게임 입력 프로그래밍을 위한 권장 패턴과 기술을 제안합니다.

> **참고**    장르 관련 또는 게임 관련 사용자 지정 입력 장치의 경우와 같이 UWP 게임에 사용할 수 있는 다른 종류의 입력 장치가 존재하며 사용 가능합니다. 그러나 그러한 장치 및 프로그래밍에 대해서는 이 섹션에서 다루지 않습니다. 사용자 지정 입력 장치의 원활한 사용을 돕는 인터페이스에 대해 자세한 내용은 [Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom) 네임스페이스를 참조하세요.

## <a name="gaming-input-devices"></a>게임 입력 장치

게임 입력 장치는 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) 네임스페이스로 Windows 10과 Xbox One용 UWP 게임 및 앱에서 지원됩니다.

### <a name="gamepads"></a>게임 패드

게임 패드는 Xbox One의 표준 입력 장치로, 키보드와 마우스를 선호하지 않는 Windows 게이머가 사용하는 경우가 많습니다. 게임 패드는 다양한 디지털 및 아날로그 컨트롤을 제공하여 거의 모든 종류의 게임에 적합하게 해 주며, 포함된 진동 모터를 통해 촉각 피드백도 제공합니다.

UWP 게임에서 게임 패드를 사용하는 방법에 대해 자세한 내용은 [게임 패드와 진동](gamepad-and-vibration.md)을 참조하세요.

### <a name="arcade-sticks"></a>아케이드 스틱

아케이드 스틱은 스탠드형 아케이드 게임기의 느낌을 재현한다는 면에서 중요한 디지털 전용 입력 장치이며, 일대일 대전 게임 또는 기타 아케이드식 게임에 매우 적합합니다.

UWP 게임에서 아케이드 스틱을 이용하는 방법에 대해 자세한 내용은 [아케이드 스틱](arcade-stick.md)을 참조하세요.

### <a name="racing-wheels"></a>레이싱 휠

레이싱 휠은 실제 경주용 자동차의 조종석과 비슷한 느낌을 주는 입력 장치로, 자동차 또는 트럭이 등장하는 모든 레이싱 게임에 매우 적합합니다. 많은 레이싱 휠에는 실제 힘 피드백이 장착되어 있습니다. 즉 단순한 진동이 아니라 조종 핸들과 같은 제어 축에 실제로 힘을 가할 수 있습니다.

UWP 게임에서 레이싱 휠을 사용하는 방법에 대해 자세한 내용은 [레이싱 휠과 힘 피드백](racing-wheel-and-force-feedback.md)을 참조하세요.

### <a name="flight-sticks"></a>플라이트 스틱

플라이트 스틱은 평면 또는 우주선의 조종석에서 찾을 수 있는 플라이트 스틱의 느낌을 재현 하는 게임 입력된 장치입니다. 신속하고 정확한 플라이트 제어를 위한 완벽한 입력 디바이스입니다.

UWP 게임에서 플라이트 스틱을 이용 하는 방법에 대 한 자세한 내용은 [플라이트 스틱](flight-stick.md)를 참조 하세요.

### <a name="raw-game-controllers"></a>원시 게임 컨트롤러

원시 게임 컨트롤러는 다양한 유형의 일반적인 게임 컨트롤러에서 발견된 입력으로 나타나는 게임 컨트롤러의 일반적인 표현입니다. 이러한 입력은 이름 없는 단추, 스위치 및 축의 단순한 배열로 표시됩니다. 원시 게임 컨트롤러를 사용하여 고객이 사용자는 컨트롤러 유형에 관계 없이 사용자 지정 입력 매핑을 만들도록 허용할 수 있습니다.

UWP 게임에서 원시 게임 컨트롤러를 사용 하는 방법에 대 한 자세한 내용은 [원시 게임 컨트롤러](raw-game-controller.md)를 참조 하세요.

### <a name="ui-navigation-controllers"></a>UI 탐색 컨트롤러

UI 탐색 컨트롤러는 UI 탐색 명령에 공통적인 어휘를 제공하는 역할의 논리적 입력 장치로, 다양한 게임 및 물리적 입력 장치 전체에서 일관된 사용자 경험을 용이하게 해 줍니다. 게임의 사용자 인터페이스는 장치별 인터페이스가 아니라 UINavigationController 인터페이스를 사용해야 합니다.

UWP 게임에서 UI 탐색 컨트롤러를 사용하는 방법에 대해 자세한 내용은 [UI navigation controller](ui-navigation-controller.md)를 참조하세요.

### <a name="headsets"></a>헤드셋

헤드셋은 입력 장치를 통해 연결될 때 특정 사용자와 연결되는 오디오 캡처 및 재생 장치입니다. 일반적으로 온라인 게임에서 음성 채팅에 사용되지만 몰입감을 강화하거나 온라인 및 오프라인 게임 양쪽 모두의 게임 플레이 기능을 제공하는 데 사용되기도 합니다.

UWP 게임에서 헤드셋을 사용하는 방법에 대해 자세한 내용은 [헤드셋](headset.md)을 참조하세요.

### <a name="users"></a>사용자

각 입력 장치와 여기에 연결된 헤드셋을 특정 사용자와 연결하여 사용자의 ID를 게임 플레이에 연결할 수 있습니다. 사용자 ID는 물리적 입력 장치에서의 입력이 논리적 UI 탐색 컨트롤러에서의 입력과 연결되는 수단이기도 합니다.

사용자와 사용자의 입력 장치를 관리하는 방법에 대해 자세한 내용은 [사용자 및 사용자 장치의 추적](input-practices-for-games.md#tracking-users-and-their-devices)을 참조하세요.

## <a name="see-also"></a>참고 항목

* [게임 입력 시스템](input-practices-for-games.md)
* [Windows.Gaming.Input 네임스페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.Custom 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)