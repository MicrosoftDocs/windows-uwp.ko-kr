---
title: 게임 컨트롤러의 레지스트리 데이터
description: 사용자의 컨트롤러를 UWP 게임에서 사용할 수 있도록 PC의 레지스트리에 추가할 수 있는 데이터에 대해 알아봅니다.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 04/08/2019
ms.topic: article
keywords: windows 10, uwp, 게임, 입력, 레지스트리, 사용자 지정
ms.localizationpriority: medium
ms.openlocfilehash: 1f3a49ae2c6fc283d479086759744eb51d8b33ce
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220068"
---
# <a name="registry-data-for-game-controllers"></a>게임 컨트롤러의 레지스트리 데이터

> [!NOTE]
> 이 항목은 Windows 10 호환 게임 컨트롤러 제조업체를 대상으로 하며 대부분의 개발자에 게는 적용 되지 않습니다.

Gamepads [네임 스페이스](/uwp/api/windows.gaming.input) 는 ihv (독립 하드웨어 공급 업체)가 PC의 레지스트리에 데이터를 추가 하 여 해당 장치를 적절 하 게 [Gamepads](/uwp/api/windows.gaming.input.gamepad), [RacingWheels](/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](/uwp/api/windows.gaming.input.flightstick)및 [UINavigationControllers](/uwp/api/windows.gaming.input.uinavigationcontroller) 로 표시할 수 있게 해줍니다. 모든 Ihv는 호환 되는 컨트롤러에 대해이 데이터를 추가 해야 합니다. 이렇게 하면 모든 UWP 게임 (및 WinRT API를 사용 하는 모든 데스크톱 게임)에서 게임 컨트롤러를 지원할 수 있습니다.

## <a name="mapping-scheme"></a>매핑 구성표

공급 업체 ID (VID) **Vvvv**, 제품 ID (PID) **Pppp**, Usage PAGE **UUUU**및 usage id **XXXX**를 사용 하는 장치에 대 한 매핑은 레지스트리의이 위치에서 읽습니다.

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

아래 표에서는 장치 루트 위치의 예상 값을 설명 합니다.

<table>
    <tr>
        <th>이름</th>
        <th>Type</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>사용 안 함</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>
            <p>이 특정 장치를 사용 하지 않도록 설정 해야 함을 나타냅니다.</p>
            <ul>
                <li><b>0</b>: 장치를 사용할 수 없습니다.</li>
                <li><b>1</b>: 장치를 사용할 수 없습니다.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>설명</td>
        <td>REG_SZ <td>아니요</td>
        <td>장치에 대 한 간단한 설명입니다.</td>
    </tr>
</table>

장치 설치 관리자는 설치 또는 [INF 파일](/windows-hardware/drivers/install/inf-files)을 통해이 데이터를 레지스트리에 추가 해야 합니다.

장치 루트 위치 아래의 하위 키는 다음 섹션에 자세히 설명 되어 있습니다.

### <a name="gamepad"></a>게임 패드

다음 표에서는 **게임 패드** 하위 키 아래에 필수 및 선택적 하위 키를 나열 합니다.

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
        <td>b</td>
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
        <td>왼쪽 어깨</td>
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
        <td>왼쪽 트리거</td>
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
> 게임 컨트롤러를 지원 되는 게임 **패드**로 추가 하는 경우 지원 되는 **UINavigationController**추가 하는 것이 좋습니다.

### <a name="racingwheel"></a>RacingWheel

다음 표에서는 **RacingWheel** 하위 키 아래에 필수 및 선택적 하위 키를 나열 합니다.

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
        <td>휠</td>
        <td>예</td>
        <td rowspan="5" style="vertical-align: middle;"><a href="#axis-mapping">축 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>제한</td>
        <td>예</td>
    </tr>
    <tr>
        <td>브레이크</td>
        <td>예</td>
    </tr>
    <tr>
        <td>부품</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>수동 브레이크</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>예</td>
        <td><a href="#properties-mapping">속성 매핑</a> 참조</td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

다음 표에서는 **ArcadeStick** 하위 키 아래에 필수 및 선택적 하위 키를 나열 합니다.

<table>
    <tr>
        <th>하위 키</th>
        <th>필수 여부</th>
        <th>정보</th>
    </tr>
    <tr>
        <td>작업 1</td>
        <td>예</td>
        <td rowspan="12" style="vertical-align: middle;"><a href="#button-mapping">단추 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>작업 2</td>
        <td>예</td>
    </tr>
    <tr>
        <td>작업 3</td>
        <td>예</td>
    </tr>
    <tr>
        <td>작업 4</td>
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

다음 표에서는 **FlightStick** 하위 키 아래에 필수 및 선택적 하위 키를 나열 합니다.

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
        <td>되돌릴</td>
        <td>예</td>
        <td rowspan="4" style="vertical-align: middle;"><a href="#axis-mapping">축 매핑</a> 참조</td>
    </tr>
    <tr>
        <td>피치</td>
        <td>예</td>
    </tr>
    <tr>
        <td>요</td>
        <td>예</td>
    </tr>
    <tr>
        <td>제한</td>
        <td>예</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>예</td>
        <td><a href="#switch-mapping">스위치 매핑</a> 참조</td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

다음 표에서는 **UINavigation** 하위 키 아래에 필수 및 선택적 하위 키를 나열 합니다.

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
        <td>동의함</td>
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
        <td>컨텍스트 1</td>
        <td>아니요</td>
    </tr>
    <tr>
        <td>컨텍스트 2</td>
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

UI 탐색 컨트롤러와 위의 명령에 대 한 자세한 내용은 [ui 탐색 컨트롤러](./ui-navigation-controller.md)를 참조 하세요.

## <a name="keys"></a>구성

다음 섹션에서는 **게임 패드**, **RacingWheel**, **ArcadeStick**, **FlightStick**및 **UINavigation** 키 아래의 각 하위 키 내용에 대해 설명 합니다.

### <a name="button-mapping"></a>단추 매핑

다음 표에서는 단추를 매핑하는 데 필요한 값을 나열 합니다. 예를 들어 게임 컨트롤러에서 **DPadUp** 를 누르는 경우 **DPadUp** 에 대 한 매핑에는 **buttonindex** 값 (**원본** : **단추**)이 포함 되어야 합니다. 스위치 위치에서 **DPadUp** 를 매핑해야 하는 경우 **DPadUp** 매핑은 **switchindex** 및 **switchindex** (**Source** is **switch**) 값을 포함 해야 합니다.

<table>
    <tr>
        <th>원본</th>
        <th>값 이름</th>
        <th>값 형식</th>
        <th>필수 여부</th>
        <th>값 정보</th>
    </tr>
    <tr>
        <td>단추</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> button 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">축</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> axis 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td>[</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td><b>임계값 백분율</b> 및 <b>DebouncePercent</b> 요인이 적용 되기 전에 축 값이 반전 되어야 함을 나타냅니다.</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>예</td>
        <td>눌린 상태와 릴리즈됨 상태 사이에서 매핑된 단추 값이 전환 되는 축 위치를 나타냅니다. 값의 유효한 범위는 0에서 100입니다. 축 값이이 값 보다 크거나 같으면 단추가 눌러져 있는 것으로 간주 됩니다.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>예</td>
        <td>
            <p>보고 된 단추 상태를 해제 하는 데 사용 되는 <b>ThresholdPercent</b> 값을 둘러싼 창의 크기를 정의 합니다. 값의 유효한 범위는 0에서 100입니다. 단추 상태 전환은 축 값이 debounce 창의 상한 또는 하 한 경계에 교차할 때만 발생할 수 있습니다. 예를 들어 <b>ThresholdPercent</b> 가 50이 고 <b>DebouncePercent</b> 가 10 이면 전체 범위 축 값의 45% 및 55%에는 debounce 경계가 생성 됩니다. 축 값이 55% 이상에 도달할 때까지 단추를 눌린 상태로 전환할 수 없으며, 축 값이 45% 또는 아래에 도달할 때까지 다시 릴리스된 상태로 전환할 수 없습니다.</p>
            <p>계산 된 debounce 창 경계는 0%에서 100% 사이에 고정. 예를 들어 임계값이 5%이 고 debounce 창이 20% 이면 debounce 창 경계가 0% 및 15%에 발생 합니다. 0% 및 100%의 축 값에 대 한 단추 상태는 임계값 및 debounce 값에 관계 없이 항상 각각 해제 된 것으로 보고 됩니다.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">스위치</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> switch 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>예</td>
        <td>
            <p>매핑된 단추가 눌린 상태임을 보고 하는 스위치 위치를 나타냅니다. 위치 값은 다음 문자열 중 하나일 수 있습니다.</p>
            <ul>
                <li>위로</li>
                <li>UpRight</li>
                <li>오른쪽</li>
                <li>DownRight</li>
                <li>아래로</li>
                <li>DownLeft</li>
                <li>왼쪽</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>인접 한 스위치 위치도 매핑된 단추가 눌러져 있음을 보고 함을 나타냅니다.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>축 매핑

다음 표에서는 축을 매핑하는 데 필요한 값을 나열 합니다.

<table>
    <tr>
        <th>원본</th>
        <th>값 이름</th>
        <th>값 형식</th>
        <th>필수 여부</th>
        <th>값 정보</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">단추</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td>
            <p>매핑된 단방향 축 값으로 변환 되는 <b>RawGameController</b> button 배열의 인덱스입니다.</p>
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
            <p>매핑된 축이 양방향 임을 나타냅니다. <b>Maxbutton</b> 및 <b>minbutton</b> 의 값은 아래와 같이 단일 양방향 축으로 결합 됩니다.</p>
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
        <td><b>RawGameController</b> axis 배열의 인덱스입니다.</td>
    </tr>
    <tr>
        <td>[</td>
        <td>DWORD</td>
        <td>아니요</td>
        <td>매핑된 축 값이 반환 되기 전에 반전 되어야 함을 나타냅니다.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">스위치</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>예</td>
        <td><b>RawGameController</b> switch 배열의 인덱스입니다.
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>예</td>
        <td>
            <p>Key, Input, Predict, PredictOnly, None</p>
            <ul>
                <li>위로</li>
                <li>UpRight</li>
                <li>오른쪽</li>
                <li>DownRight</li>
                <li>아래로</li>
                <li>DownLeft</li>
                <li>왼쪽</li>
                <li>UpLeft</li>
            </ul>
            <p>매핑된 축 값이 1.0로 보고 되도록 하는 스위치의 위치를 나타냅니다. <b>MaxValueSwitchPosition</b> 의 반대 방향이 0.0으로 처리 됩니다. 예를 들어 <b>MaxValueSwitchPosition</b> 가 <b>Up</b>인 경우 축 값 번역이 다음과 같이 표시 됩니다.</p>
            <table>
                <tr>
                    <th>스위치 위치</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>위로</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>아래로</td>
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
            <p>인접 한 스위치 위치도 매핑된 축이 1.0을 보고 하도록 함을 나타냅니다. 위의 예제에서 <b>Includeadjacent</b> 가 설정 된 경우 축 변환은 다음과 같이 수행 됩니다.</p>
            <table>
                <tr>
                    <th>스위치 위치</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>위로</td>
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
                    <td>아래로</td>
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

### <a name="switch-mapping"></a>매핑 전환

스위치 위치는 **RawGameController** 의 단추 배열에 있는 단추 집합 또는 스위치 배열의 인덱스에서 매핑할 수 있습니다. 스위치 위치는 축에서 매핑할 수 없습니다.

<table>
    <tr>
        <th>원본</th>
        <th>값 이름</th>
        <th>값 형식</th>
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
        <td><b>TwoWay</b>, <b>FourWay</b>또는 <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;"><a href="#buttonindex-values">* Buttonindex values</a> 를 참조 하세요.</td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>왼쪽 Buttonindex</td>
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
        <td>Down왼쪽 Buttonindex</td>
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
        <td><b>TwoWay</b>, <b>FourWay</b>또는 <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> 는 항상 제공 됩니다. <b>XAxisIndex</b> 은 <b>Switchkind</b> 가 <b>FourWay</b> 또는 <b>EightWay</b>경우에만 존재 합니다.</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">축의 가운데 위치 주위의 데드 영역 크기를 표시 합니다.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">보고 된 스위치 상태를 해제 하는 데 사용 되는 낮은 비활성 영역 제한 주위의 창 크기를 정의 합니다.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">데드 영역 및 debounce 창 계산이 적용 되기 전에 해당 축 값이 반전 되도록 지정 합니다.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">스위치</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td><b>RawGameController</b> switch 배열의 인덱스입니다.
    </tr>
    <tr>
        <td>[</td>
        <td>DWORD</td>
        <td>스위치가 기본 시계 방향 대신 시계 반대 방향으로 해당 위치를 보고 함을 나타냅니다.</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>지정 된 양만큼 위치를 보고 하는 시작점을 이동 합니다. <b>Positionbias</b> 는 항상 원래 시작 지점에서 시계 방향으로 계산 되며 값의 순서가 반전 되기 전에 적용 됩니다.</p>
            <p>예를 들어 <b>귀찮고</b> 로 시작 하는 위치를 보고 하는 스위치는 <b>반전</b> 플래그를 설정 하 고 <b>positionbias</b> 를 5로 지정 하 여 정규화 할 수 있습니다.</p>
            <table>
                <tr>
                    <th>위치</th>
                    <th>보고 된 값</th>
                    <th>PositionBias 및 반전 플래그 후</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>오른쪽</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>위로</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>왼쪽</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>아래로</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>* ButtonIndex 값

\*ButtonIndex values **RawGameController**의 단추 배열로 인덱싱합니다.

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
                <li>왼쪽 Buttonindex</li>
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
                <li>왼쪽 Buttonindex</li>
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
                <li>왼쪽 Buttonindex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>Down왼쪽 Buttonindex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>속성 매핑

이러한 값은 서로 다른 매핑 형식에 대 한 정적 매핑 값입니다.

<table>
    <tr>
        <th>매핑</th>
        <th>값 이름</th>
        <th>값 형식</th>
        <th>값 정보</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>휠이 단일 방향으로 지 원하는 최대 실제 휠 각도를 나타냅니다. 예를 들어-90도에서 90 각도로 회전 가능한 바퀴는 90를 지정 합니다.</td>
    </tr>
</table>

## <a name="labels"></a>레이블

레이블은 장치 루트 아래의 **레이블** 키 아래에 있어야 합니다. **레이블에** 는 세 개의 하위 키 ( **단추**, **축**및 **스위치**)가 있을 수 있습니다.

### <a name="button-labels"></a>단추 레이블

**단추** 키는 **RawGameController**의 buttons 배열의 각 단추 위치를 문자열에 매핑합니다. 각 문자열은 내부적으로 해당 [GameControllerButtonLabel](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) 열거형 값에 매핑됩니다. 예를 들어, 게임 패드에 단추가 10 개 있는 경우 **RawGameController** 가 단추를 구문 분석 하 고 단추 보고서에 표시 하는 순서는 다음과 같습니다.

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

레이블은 **단추** 키 아래에 다음 순서로 표시 되어야 합니다.

<table>
    <tr>
        <th>이름</th>
        <th>값 (형식: REG_SZ)</th>
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
        <td>왼쪽 범퍼</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>축 레이블

**축** 키는 **RawGameController**의 축 배열에 있는 각 축 위치를 단추 레이블과 마찬가지로 [GameControllerButtonLabel 열거형](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) 에 나열 된 레이블 중 하나에 매핑합니다. [단추 레이블](#button-labels)의 예제를 참조 하세요.

### <a name="switch-labels"></a>레이블 전환

스위치 키 맵은 스위치 위치를 레이블로 **바꿉니다** . 값은이 명명 규칙을 따릅니다. 즉, 인덱스가 **RawGameController**의 스위치 배열에서 *x* 인 스위치의 위치에 레이블을 지정 하려면 **스위치** 하위 키 아래에 다음 값을 추가 합니다.

* SwitchxUp
* SwitchxUpRight
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

다음 표에서는 **RawGameController**의 인덱스 0에 표시 되는 4 방향 스위치의 스위치 위치에 대 한 예제 레이블 집합을 보여 줍니다.

<table>
    <tr>
        <th>이름</th>
        <th>값 (형식: REG_SZ)</th>
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

## <a name="example-registry-file"></a>예제 레지스트리 파일

이러한 매핑과 값이 함께 표시 되는 방법을 보여 주기 위해 제네릭 **RacingWheel**에 대 한 예제 레지스트리 파일은 다음과 같습니다.

```text
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

* [Windows 게이밍. 입력 네임 스페이스](/uwp/api/windows.gaming.input)
* [Windows. 사용자 지정 네임 스페이스](/uwp/api/windows.gaming.input.custom)
* [INF 파일](/windows-hardware/drivers/install/inf-files)