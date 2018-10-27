---
author: mijacobs
Description: TODO
title: 게임 패드 및 리모컨 조작
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a3e87fea7dfce66b16ccdf04ef13950c092660c8
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5705785"
---
# <a name="gamepad-and-remote-control-interactions"></a>게임 패드 및 리모컨 조작

![원격 및 D 패드](images/dpad-remote/dpad-remote.png)

유니버설 Windows 플랫폼(UWP) 앱은 이제 Xbox 및 TV 경험에 대한 기본 입력 디바이스인 게임 패드 및 원격 제어 입력을 지원합니다.

UWP 앱은 PC의 키보드 및 마우스 입력과 휴대폰 또는 태블릿의 터치식 입력과 같은 이러한 입력 디바이스 유형에 최적화되어야 합니다.

앱이 이러한 입력 디바이스와 잘 작동하는지 확인하는 작업은 Xbox 및 TV에 최적화할 때 중요한 단계입니다.

> [!NOTE] 
> 이제 작업 유효성을 쉽게 검사하는 PC의 UWP 앱으로 게임 패드를 연결하여 사용할 수 있습니다.

게임 패드 또는 원격 제어를 사용할 때 UWP 앱의 사용자 환경을 성공적이고 재미있게 하려면 다음을 고려해야 합니다.

* [하드웨어 단추](../devices/designing-for-tv.md#hardware-buttons) - 게임 패드와 리모컨의 단추 및 구성은 서로 매우 다릅니다.

* [XY 포커스 탐색 및 조작](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction) - XY 포커스 탐색을 사용하면 사용자가 앱의 UI 주위를 탐색하도록 할 수 있습니다

* [마우스 모드](../devices/designing-for-tv.md#mouse-mode) - XY 포커스 탐색이 충분하지 않은 경우 마우스 모드를 사용하면 앱이 마우스 환경을 에뮬레이트할 수 있습니다.
