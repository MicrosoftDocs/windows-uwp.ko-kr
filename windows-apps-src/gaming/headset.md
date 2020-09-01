---
title: 헤드셋
description: 헤드셋을 검색 하 고, 플레이어 음성을 캡처하고, 오디오를 재생 하는 데 사용할 수 있습니다.
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 헤드셋
ms.localizationpriority: medium
ms.openlocfilehash: 5e5b0894dbab03490cb74bdae8c231b57d397cb4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175297"
---
# <a name="headset"></a>헤드셋

이 페이지에서는 [Windows]를 사용 하는 헤드셋 프로그래밍의 기본 사항에 대해 설명 합니다. 헤드셋[헤드셋] 및 유니버설 Windows 플랫폼 (UWP) 관련 api

이 페이지를 읽으면 다음에 대해 알아봅니다.
* 입력 또는 탐색 장치에 연결 된 헤드셋에 액세스 하는 방법
* 헤드셋이 연결 되었거나 연결이 끊어졌는지 확인 하는 방법


## <a name="headset-overview"></a>헤드셋 개요

헤드셋은 온라인 게임의 다른 플레이어와 통신 하는 데 가장 자주 사용 되는 오디오 캡처 및 재생 장치 이지만 플레이 또는 다른 독창적인 사용에도 사용할 수 있습니다. 헤드셋 [은 windows 10][] 및 Xbox UWP 앱에서 지원 됩니다.


## <a name="detect-and-track-headsets"></a>헤드셋 검색 및 추적

헤드셋은 시스템에서 관리 되므로이를 만들거나 초기화할 필요가 없습니다. 시스템은에 연결 된 입력 장치 및 헤드셋이 연결 되거나 연결이 끊어진 경우 사용자에 게 알리는 이벤트를 통해 헤드셋에 대 한 액세스를 제공 합니다.

### <a name="igamecontrollerheadset"></a>IGameController

IGameController 네임 스페이스의 모든 입력 장치는 현재 장치에 연결 된 헤드셋이 되도록 [헤드셋][igamecontroller.headset] 속성을 정의 하는 [IGameController][] 인터페이스를 구현 [합니다.][]

### <a name="connecting-and-disconnecting-headsets"></a>헤드셋 연결 및 연결 끊기

헤드셋이 연결 되거나 연결이 끊어지면 [헤드 setconnected][igamecontroller.headsetconnected] 및 [헤드 setdisconnected][igamecontroller.headsetdisconnected] 이벤트가 발생 합니다. 이 이벤트에 대 한 처리기를 등록 하 여 입력 장치에 현재 연결 된 헤드셋이 있는지 여부를 추적할 수 있습니다.

다음 예제에서는 이벤트에 대 한 처리기를 등록 하는 방법을 보여 줍니다 `HeadsetConnected` .

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

다음 예제에서는 이벤트에 대 한 처리기를 등록 하는 방법을 보여 줍니다 `HeadsetDisconnected` .

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>헤드셋 사용

[헤드셋][] 클래스는 xaudio 끝점 id를 나타내는 두 개의 문자열로 구성 됩니다. 하나는 오디오 캡처 (헤드셋 마이크에서 기록)이 고 다른 하나는 오디오 렌더링 (헤드셋을 통해 재생)입니다.

XAudio 사용에 대 한 자세한 내용은 여기서 설명 하지 않습니다. 자세한 내용은 [XAudio2 프로그래밍 가이드](/windows/desktop/xaudio2/programming-guide) 및 [XAudio2 API 참조](/windows/desktop/xaudio2/programming-reference)를 참조 하세요.


[Windows. 게임 입력]: /uwp/api/Windows.Gaming.Input
[igamecontroller]: /uwp/api/Windows.Gaming.Input.IGameController
[igamecontroller.headset]: /uwp/api/Windows.Gaming.Input.IGameController
[igamecontroller.headsetconnected]: /uwp/api/Windows.Gaming.Input.IGameController
[igamecontroller.headsetdisconnected]: /uwp/api/Windows.Gaming.Input.IGameController
[헤드셋]: /uwp/api/Windows.Gaming.Input.Headset