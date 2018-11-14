---
author: eliotcowley
title: 게임 컨트롤러의 레지스트리 데이터
description: 컨트롤러를 UWP 게임에 사용할 수 있도록 PC의 레지스트리에 추가할 수 있는 데이터에 대해 알아보세요.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.author: wdg-dev-content
ms.date: 06/25/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 입력, 레지스트리, 사용자 지정
ms.localizationpriority: medium
ms.openlocfilehash: 4bbd4074c52514b9cb66fd6f2dd189421f61d5ee
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6192273"
---
# <a name="registry-data-for-game-controllers"></a>게임 컨트롤러의 레지스트리 데이터

> [!NOTE]
> 이 항목은 Windows 10 호환 게임 컨트롤러 제조업체를 위해 준비된 것으로 대부분의 개발자에게는 적용되지 않습니다.

[Windows.Gaming.Input namespace](https://docs.microsoft.com/uwp/api/windows.gaming.input)를 사용하면 IHV(독립 하드웨어 공급업체)는 PC 레지스트리에 데이터를 추가하여 장치가 [Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad), [RacingWheels](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick) 및 [UINavigationControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)에 적절하게 표시되도록 할 수 있습니다. 모든 IHV는 호환 컨트롤러에 대한 이 데이터를 추가해야 합니다. 이렇게 하면 모든 UWP 게임(및 WinRT API를 사용하는 모든 데스크톱 게임)이 게임 컨트롤러를 지원할 수 있습니다.

## <a name="mapping-scheme"></a>매핑 구성표

공급자 ID(VID)가 **VVVV**이고 제품 ID(PID)가 **PPPP**이고 사용 페이지가 **UUUU**이고 사용 ID가 **XXXX**인 장치의 매핑은 레지스트리의 이 위치에서 읽습니다.

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

아래 표에서는 장치 루트 위치 아래에 예상되는 값을 설명합니다.

<table>
    <tr>
        <th>이름</th>
        <th>유형</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>사용 안 함</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>
            <p>이 특정 장치를 사용할 수 없도록 나타냅니다.</p>
            <ul>
                <li><b>0</b>: 장치가 사용하지 않도록 설정되지 않았습니다.</li>
                <li><b>1</b>: 장치가 사용하지 않도록 설정되었습니다.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>설명</td>
        <td>REG_SZ <td>아니요</td>
        <td>장치에 대한 간단한 설명입니다.</td>
    </tr>
</table>

장치 설치 관리자가 이 데이터를 레지스트리에 추가해야 합니다(설치 또는 [INF 파일](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)을 통해).

장치 루트 위치의 하위 키에 대한 내용은 다음 섹션에 자세히 설명되어 있습니다.

### <a name="gamepad"></a>게임 패드

아래 표에는 **Gamepads** 하위 키의 필수 하위 키와 선택적 하위 키가 나열되어 있습니다.

<table>
    <tr>
        <th>하위 키</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>메뉴</td>
        <td>예</td>
        <td rowspan="18" style="vertical-align: middle;"><a href="#button-mapping">단추 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>보기</td>
        <td>예</td>
    </tr>
    <tr>
        <td>A</td>
        <td>예</td>
    </tr>
    <tr>
        <td>B</td>
        <td>예</td>
    </tr>
    <tr>
        <td>X</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Y</td>
        <td>예</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>예</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>예</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>예</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>예</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>예</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>예</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>예</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>LeftTrigger</td>
        <td>예</td>
        <td rowspan="6" style="vertical-align: middle;"><a href="#axis-mapping">축 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>예</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>예</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>예</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>예</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>예</td>
    </tr>
</table>

> [!NOTE]
> 지원되는 **게임 패드**로 게임 컨트롤러를 추가하는 경우 게임 컨트롤러를 **UINavigationController**로도 추가할 것을 강력하게 권장합니다.

### <a name="racingwheel"></a>RacingWheel

아래 표에는 **RacingWheel** 하위 키의 필수 하위 키와 선택적 하위 키가 나열되어 있습니다.

<table>
    <tr>
        <th>하위 키</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>예</td>
        <td rowspan="30" style="vertical-align: middle;"><a href="#button-mapping">단추 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>예</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>FirstGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Wheel</td>
        <td>예</td>
        <td rowspan="5" style="vertical-align: middle;"><a href="#axis-mapping">축 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>Throttle</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Brake</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Clutch</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Handbrake</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>예</td>
        <td><a href="#properties-mapping">속성 매핑</a> 참조</td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

아래 표에는 **ArcadeStick** 하위 키의 필수 하위 키와 선택적 하위 키가 나열되어 있습니다.

<table>
    <tr>
        <th>하위 키</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>Action1</td>
        <td>예</td>
        <td rowspan="12" style="vertical-align: middle;"><a href="#button-mapping">단추 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>예</td>
    </tr>
    <tr>
        <td>StickUp</td>
        <td>예</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>예</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>예</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>예</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

아래 표에는 **FlightStick** 하위 키의 필수 하위 키와 선택적 하위 키가 나열되어 있습니다.

<table>
    <tr>
        <th>하위 키</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>예</td>
        <td rowspan="2" style="vertical-align: middle;"><a href="#button-mapping">단추 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Roll</td>
        <td>예</td>
        <td rowspan="4" style="vertical-align: middle;"><a href="#axis-mapping">축 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>Pitch</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Yaw</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Throttle</td>
        <td>예</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>예</td>
        <td><a href="#switch-mapping">스위치 매핑</a> 참조</td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

아래 표에는 **UINavigation** 하위 키의 필수 하위 키와 선택적 하위 키가 나열되어 있습니다.

<table>
    <tr>
        <th>하위 키</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>메뉴</td>
        <td>예</td>
        <td rowspan="24" style="vertical-align: middle;"><a href="#button-mapping">단추 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>보기</td>
        <td>예</td>
    </tr>
    <tr>
        <td>수락</td>
        <td>예</td>
    </tr>
    <tr>
        <td>취소</td>
        <td>예</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>예</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>예</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>예</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
        <td>예</td>
    </tr>
    <tr>
        <td>Context1</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Context2</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Context3</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>Context4</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>PageUp</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>PageDown</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>ScrollUp</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>ScrollDown</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>ScrollLeft</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>ScrollRight</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>SecondaryUp</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>아니요</td>
    </tr>
</table>

UI 탐색 컨트롤러 및 위의 명령에 대한 자세한 내용은 [UI 탐색 컨트롤러](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller)를 참조하세요.

## <a name="keys"></a>키

다음 섹션에서는 **Gamepad**, **RacingWheel**, **ArcadeStick**, **FlightStick** 및 **UINavigation** 키의 각 하위 키 콘텐츠에 대해 설명합니다.

### <a name="button-mapping"></a>단추 매핑

아래 표에는 단추 매핑에 필요한 값이 나열되어 있습니다. 예를 들어 게임 컨트롤러에서 **DPadUp**을 누르면 **DPadUp**에 대한 매핑에 **ButtonIndex** 값이 포함되어야 합니다(**원본**은 **단추**). **DPadUp**를 스위치 위치에서 매핑해야 하는 경우 **DPadUp** 매핑에 **SwitchIndex** 및 **SwitchPosition** 값이 포함되어야 합니다(**원본**은 **스위치**).

<table>
    <tr>
        <th>원본</th>
        <th>값 이름</th>
        <th>값 유형</th>
        <th>필수 여부</th>
        <th>값 정보</th>
    </tr>
    <tr>
        <td>단추</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> 단추 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">축</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> 축 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td>반전</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td><b>ThresholdPercent</b> 및 <b>DebouncePercent</b> 인수를 적용하기 전에 축 값을 반전해야 함을 나타냅니다.</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>예</td>
        <td>매핑된 단추 값이 손가락으로 누른 상태와 손가락을 뗀 상태 사이에서 전환되는 축 위치를 나타냅니다. 값의 유효한 범위는 0-100입니다. 축 값이 이 값보다 크거나 같으면 단추가 눌린 것으로 간주됩니다.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>예</td>
        <td>
            <p><b>ThresholdPercent</b> 값 근처의 창 크기를 정의하며, 이 값은 보고된 단추 상태를 디바운스하는 데 사용됩니다. 값의 유효한 범위는 0-100입니다. 축 값이 디바운스 창의 상한 경계 또는 하한 경계를 넘을 때만 단추 상태 전환이 발생할 수 있습니다. 예를 들어 <b>ThresholdPercent</b>가 50이고 <b>DebouncePercent</b>가 10이면 디바운스 경계는 45%, 전체 범위 축 값은 55%입니다. 축 값이 55% 또는 그 이상으로 높아지지 않으면 단추가 눌린 상태로 전환될 수 없고, 축 값이 45% 또는 그 아래로 떨어지지 않으면 단추가 다시 손가락을 뗀 상태로 전환될 수 없습니다.</p>
            <p>계산된 디바운스 창 경계는 0%에서 100% 사이로 고정됩니다. 예를 들어 임계값이 5%이고 디바운스 창이 20%이면 디바운스 창 경계는 0%와 15%에서 생성됩니다. 축 값이 0%일 때와 100%일 때의 단추 상태는 임계값 및 디바운스 값에 관계없이 항상 각각 손가락을 뗀 상태와 손가락으로 누른 상태로 보고됩니다.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">스위치</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> 스위치 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>예</td>
        <td>
            <p>매핑된 단추 상태가 손가락으로 누른 상태로 보고되는 스위치 위치를 나타냅니다. 위치 값은 다음 문자열 중 하나일 수 있습니다.</p>
            <ul>
                <li>Up</li> 
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>마찬가지로 매핑된 단추 상태가 손가락으로 누른 상태로 보고되는 인접 스위치 위치를 나타냅니다.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>축 매핑

아래 표에는 축 매핑에 필요한 값이 나열되어 있습니다.

<table>
    <tr>
        <th>원본</th>
        <th>값 이름</th>
        <th>값 유형</th>
        <th>필수 여부</th>
        <th>값 정보</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">단추</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td>
            <p>매핑된 단방향 축 값으로 변환되는 <b>RawGameController</b> 단추 배열의 인덱스입니다.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>
            <p>매핑된 축이 양방향임을 나타냅니다. 아래와 같이 <b>MaxButton</b> 및 <b>MinButton</b> 값이 단일 양방향 축에 결합됩니다.</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>FALSE</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0.5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">축</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> 축 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td>반전</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>매핑된 축 값을 반환하기 전에 먼저 반전해야 함을 나타냅니다.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">스위치</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> 스위치 배열의 인덱스입니다.
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>예</td>
        <td>
            <p>다음 문자열 중 하나입니다.</p>
            <ul>
                <li>Up</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>매핑된 축 값이 1.0으로 보고되는 스위치 위치를 나타냅니다. <b>MaxValueSwitchPosition</b>의 반대 방향은 0.0으로 처리됩니다. 예를 들어 <b>MaxValueSwitchPosition</b>이 <b>Up</b>이면 축 값 변환은 아래와 같습니다.</p>
            <table>
                <tr>
                    <th>스위치 위치</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>
            <p>마찬가지로 매핑된 축 값이 1.0으로 보고되는 인접 스위치 위치를 나타냅니다. 위의 예에서 <b>IncludeAdjacent</b>가 설정되면 축 변환은 다음과 같습니다.</p>
            <table>
                <tr>
                    <th>스위치 위치</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>스위치 매핑

**RawGameController**의 단추 배열에 있는 단추 집합에서 또는 스위치 배열의 인덱스에서 스위치 위치를 매핑할 수 있습니다. 축에서는 스위치 위치를 매핑할 수 없습니다.

<table>
    <tr>
        <th>원본</th>
        <th>값 이름</th>
        <th>값 유형</th>
        <th>값 정보</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">단추</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2, 4 또는 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> 또는 <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;"><a href="#buttonindex-values">*ButtonIndex 값</a> 참조</td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>RightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">축</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> 또는 <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b>는 항상 있습니다. <b>XAxisIndex</b>는 <b>SwitchKind</b>가 <b>FourWay</b> 또는 <b>EightWay</b>인 경우에만 있습니다.</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">축 중앙 위치 주변의 데드존 크기를 나타냅니다.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">데드존 상한 및 하한 근처의 창 크기를 정의하며, 보고된 스위치 상태를 디바운스하는 데 사용됩니다.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">데드존 및 디바운스 창 계산을 적용하기 전에 해당하는 축 값을 반전해야 함을 나타냅니다.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">스위치</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td><b>RawGameController</b> 스위치 배열의 인덱스입니다.
    </tr>
    <tr>
        <td>반전</td>
        <td>DWORD</td>
        <td>스위치가 자신의 위치를 기본 시계 방향 순서가 아닌 시계 반대 방향 순서로 보고함을 나타냅니다.</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>위치 보고 방식의 시작점을 지정된 양만큼 이동합니다. <b>PositionBias</b>는 항상 원래 시작점에서 시계 방향으로 계산되며 값 순서를 반전하기 전에 적용됩니다.</p>
            <p>예를 들어 <b>DownRight</b>부터 시작하여 시계 반대 방향 순서로 위치를 보고하는 스위치는 <b>반전</b> 플래그를 설정하고 <b>PositionBias</b>를 5로 지정하여 정규화할 수 있습니다.</p>
            <table>
                <tr>
                    <th>위치</th>
                    <th>보고된 값</th>
                    <th>PositionBias 및 반전 플래그 후</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Left</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>*ButtonIndex 값

\*ButtonIndex 값 인덱스는 **RawGameController**의 단추 배열로 인덱싱:

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>속성 매핑

다음은 여러 매핑 유형에 대한 정적 매핑 값입니다.

<table>
    <tr>
        <th>매핑</th>
        <th>값 이름</th>
        <th>값 유형</th>
        <th>값 정보</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>휠에서 한 방향으로 지원되는 최대 물리적 휠 각도를 나타냅니다. 예를 들어 -90도부터 90도까지 회전할 수 있는 휠은 90을 지정합니다.</td>
    </tr>
</table>

## <a name="labels"></a>레이블

레이블은 장치 루트의 **레이블** 아래에 있어야 합니다. **레이블**은 **단추**, **축**, **스위치**의 3개 하위 키를 가질 수 있습니다.

### <a name="button-labels"></a>단추 레이블

**단추** 키는 **RawGameController** 단추 배열의 각 단추 위치를 문자열에 매핑합니다. 각 문자열은 해당하는 [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) 열거형 값에 내부적으로 매핑됩니다. 예를 들어 게임 패드에 단추가 10개 있을 때 **RawGameController**가 단추를 구문 분석하고 단추 보고서에 단추를 표시하는 순서는 다음과 같습니다.

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

레이블이 **단추** 키 아래에 다음과 같은 순서로 표시됩니다.

<table>
    <tr>
        <th>이름</th>
        <th>값(유형: REG_SZ)</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>메뉴</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>보기</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>LetterA</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>LetterY</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>축 레이블

**축** 키는 단추 레이블과 마찬가지로 **RawGameController** 축 배열의 각 축 위치를 [GameControllerButtonLabel 열거형](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)에 나열된 레이블 중 하나로 매핑합니다. [단추 레이블](#button-labels)의 예를 참조하세요.

### <a name="switch-labels"></a>스위치 레이블

**스위치** 키는 스위치 위치를 레이블로 매핑합니다. 값은 다음과 같은 명명 규칙을 따릅니다. 인덱스가 **RawGameController** 스위치 배열의 *x*인 스위치 위치에 레이블을 지정하려면 **스위치** 하위 키 아래에 다음 값을 추가합니다. 

* SwitchxUp 
* SwitchxUpRight 
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

다음 표에서는 **RawGameController**에서 인덱스 0에 표시되는 4방향 스위치의 스위치 위치에 대한 레이블 집합의 예를 보여 줍니다. 

<table>
    <tr>
        <th>이름</th>
        <th>값(유형: REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>레지스트리 파일 예제

이러한 매핑과 값이 결합되는 방식을 보여드리기 위해 일반 **RacingWheel**의 레지스트리 파일 예제를 준비했습니다.

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input 네임스페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.Custom 네임스페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)
* [INF 파일](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)