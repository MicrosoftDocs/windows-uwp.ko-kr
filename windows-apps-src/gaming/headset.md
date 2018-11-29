---
title: 헤드셋
description: Windows.Gaming.Input 헤드셋 API를 이용하여 헤드셋을 감지하고 플레이어 음성을 캡처하며 오디오를 재생합니다.
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 헤드셋
ms.localizationpriority: medium
ms.openlocfilehash: b3de68cc59c9928a52eba5caeb840e9e825eecf0
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7989446"
---
# <a name="headset"></a>헤드셋

이 페이지는 [Windows.Gaming.Input.Headset][헤드셋] 및 UWP(유니버설 Windows 플랫폼)에 대해 관련 API를 사용하는 헤드셋 프로그래밍의 기본 사항에 대해 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.
* 입력 장치 또는 탐색 장치에 연결된 헤드셋에 액세스하는 방법
* 헤드셋의 연결 또는 연결 끊김을 감지하는 방법


## <a name="headset-overview"></a>헤드셋 개요

헤드셋은 온라인 게임에서 다른 플레이어와의 통신에 가장 빈번하게 사용되는 오디오 캡처 및 재생 장치지만 게임 플레이나 그 밖의 독창적인 용도로 사용될 수 있습니다. 헤드셋은 Windows 10 및 Xbox UWP 앱에서 [Windows.Gaming.Input][] 네임스페이스로 지원됩니다.


## <a name="detect-and-track-headsets"></a>헤드셋 감지 및 추적

헤드셋은 시스템에서 관리하므로 생성하거나 초기화할 필요가 없습니다. 헤드셋이 연결되거나 연결이 끊겼을 경우 시스템에서 알려주는 이벤트를 제공하며 시스템에 연결된 입력 장치를 통해 헤드셋에 액세스할 수 있습니다.

### <a name="igamecontrollerheadset"></a>IGameController.Headset

[Windows.Gaming.Input][] 네임스페이스의 모든 입력 장치는 [헤드셋][igamecontroller.headset] 속성을 현재 장치에 연결되어 있는 헤드셋으로 정의하는 [IGameController][] 인터페이스를 구현합니다.

### <a name="connecting-and-disconnecting-headsets"></a>헤드셋 연결 및 연결 끊기

헤드셋이 연결되거나 연결이 끊기면 [HeadsetConnected][igamecontroller.headsetconnected] 및 [HeadsetDisconnected][igamecontroller.headsetdisconnected] 이벤트가 발생합니다. 이러한 이벤트의 처리기를 등록하면 현재 입력 장치에 헤드셋이 연결되어 있는지 추적할 수 있습니다.

다음 예제에서는 `HeadsetConnected` 이벤트 처리기를 등록하는 방법을 보여 줍니다.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

다음 예제에서는 `HeadsetDisconnected` 이벤트 처리기를 등록하는 방법을 보여 줍니다.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>헤드셋 사용

[헤드셋][] 클래스는 XAudio 끝점 ID를 나타내는 2개의 문자열로 구성됩니다. 하나는 오디오 캡처(헤드셋 마이크에서 녹음) 용도이고 다른 하나는 오디오 렌더링(헤드셋 이어폰을 통해 재생) 용도입니다.

XAudio 사용에 대한 세부 내용은 여기에서 다루지 않습니다. 자세한 정보는 [XAudio2 프로그래밍 가이드](https://msdn.microsoft.com/library/windows/desktop/ee415737.aspx) 및 [XAudio2 API 참조](https://msdn.microsoft.com/library/windows/desktop/ee415899.aspx)에서 확인할 수 있습니다.


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headset.aspx
[igamecontroller.headsetconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetconnected.aspx
[igamecontroller.headsetdisconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetdisconnected.aspx
[헤드셋]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.headset.aspx
