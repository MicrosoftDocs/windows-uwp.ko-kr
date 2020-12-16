---
title: GPIO, I2C 및 SPI에 대한 사용자 모드 액세스를 사용하도록 설정
description: 이 자습서에서는 Windows 10에서 GPIO, I2C, SPI 및 UART에 대 한 사용자 모드 액세스를 사용 하도록 설정 하는 방법을 설명 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, acpi, gpio, i2c, spi, uefi
ms.assetid: 2fbdfc78-3a43-4828-ae55-fd3789da7b34
ms.localizationpriority: medium
ms.openlocfilehash: 0fc07cf45fc4762b202698c07b7cf2088e260e1b
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598844"
---
# <a name="enable-user-mode-access-to-gpio-i2c-and-spi"></a>GPIO, I2C 및 SPI에 대한 사용자 모드 액세스를 사용하도록 설정

Windows 10에는 일반 용도의 GPIO (입력/출력), I2C (Inter-Integrated 회로), SPI (직렬 주변 장치 인터페이스) 및 UART (universal 비동기 수신기-전송기)의 사용자 모드에서 직접 액세스 하기 위한 새 Api가 포함 되어 있습니다. Raspberry Pi 2와 같은 개발 보드는 이러한 연결의 하위 집합을 노출 하 여 특정 응용 프로그램을 처리 하기 위해 사용자 지정 회로를 사용 하 여 기본 계산 모듈을 확장할 수 있도록 합니다. 이러한 낮은 수준 버스는 일반적으로 헤더에 표시 된 GPIO 핀 및 버스의 하위 집합만 포함 하 여 다른 중요 한 온보드 함수와 공유 됩니다. 시스템 안정성을 유지 하려면 사용자 모드 응용 프로그램에서 수정 하기에 안전 하 게 사용할 핀과 버스를 지정 해야 합니다.

이 문서에서는 ACPI (고급 구성 및 전원 인터페이스)에서이 구성을 지정 하는 방법에 대해 설명 하 고, 구성이 올바르게 지정 되었는지 유효성을 검사 하는 도구를 제공 합니다.

> [!IMPORTANT]
> 이 문서의 대상은 UEFI (UEFI(Unified Extensible Firmware Interface)) 및 ACPI 개발자를 위한 것입니다. ACPI, ASL (ACPI 원본 언어) 제작 및 SpbCx/Gpix/Gpix를 사용 하는 것이 가장 좋습니다.

Windows에서 낮은 수준의 버스에 대 한 사용자 모드 액세스는 기존 `GpioClx` 및 프레임 워크를 통해 배관 됩니다 `SpbCx` . Windows IoT Core 및 Windows Enterprise에서 사용할 수 있는 *RhProxy* 라는 새 드라이버가 `GpioClx` `SpbCx` 사용자 모드에 리소스를 노출 합니다. Api를 사용 하도록 설정 하려면 사용자 모드에 노출 해야 하는 각 GPIO 및 SPB 리소스를 사용 하 여 ACPI 테이블에서 rhproxy에 대 한 장치 노드를 선언 해야 합니다. 이 문서에서는 ASL을 작성 하 고 확인 하는 과정을 안내 합니다.

## <a name="asl-by-example"></a>예제 별 ASL

Raspberry Pi 2에서 rhproxy 장치 노드 선언에 대해 살펴보겠습니다. 먼저 _SB 범위에서 ACPI 장치 선언을 만듭니다 \\ .

```cpp
Device(RHPX)
{
    Name(_HID, "MSFT8000")
    Name(_CID, "MSFT8000")
    Name(_UID, 1)
}
```

- _HID – 하드웨어 Id입니다. 공급 업체별 하드웨어 ID로 설정 합니다.
- _CID 호환 Id입니다. "MSFT8000" 여야 합니다.
- _UID – 고유 Id입니다. 1로 설정 합니다.

그런 다음 사용자 모드로 노출 되어야 하는 각 GPIO 및 SPB 리소스를 선언 합니다. 리소스 인덱스를 사용 하 여 리소스와 속성을 연결 하는 것이 중요 하므로 리소스가 선언 되는 순서는 중요 합니다. 여러 I2C 또는 SPI 버스가 노출 되는 경우 첫 번째 선언 된 버스가 해당 형식에 대 한 ' 기본 ' 버스로 간주 되며 `GetDefaultAsync()` [I2cController](/uwp/api/windows.devices.i2c.i2ccontroller) 및 [SpiController](/uwp/api/windows.devices.spi.spicontroller)의 메서드에서 반환 되는 인스턴스가 됩니다.

### <a name="spi"></a>SPI

Raspberry Pi에는 두 개의 노출 된 SPI 버스가 있습니다. SPI0에는 두 개의 하드웨어 칩 선택 회선이 있으며 SPI1에는 하드웨어 칩 선택 줄이 하나 있습니다. 각 칩의 각 칩 선택 줄에는 하나의 SPISerialBus () 리소스 선언이 필요 합니다. 다음 두 가지 SPISerialBus 리소스 선언은 SPI0에서 두 개의 칩 선택 줄에 대 한 것입니다. DeviceSelection 필드에는 드라이버가 하드웨어 칩 선택 줄 식별자로 해석 하는 고유 값이 포함 되어 있습니다. DeviceSelection 필드에 입력 하는 정확한 값은 드라이버가 ACPI 연결 설명자의이 필드를 해석 하는 방법에 따라 달라 집니다.

> [!NOTE]
> 이 문서에는 Microsoft에서 더 이상 사용 하지 않는 용어 종속 용어에 대 한 참조가 포함 되어 있습니다. 소프트웨어에서 용어를 제거 하는 경우이 문서에서 제거 합니다.

```cpp
// Index 0
SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                           // MOSI - GPIO 10 - Pin 19
                           // MISO - GPIO 9  - Pin 21
                           // CE0  - GPIO 8  - Pin 24
    0,                     // Device selection (CE0)
    PolarityLow,           // Device selection polarity
    FourWireMode,          // wiremode
    0,                     // databit len: placeholder
    ControllerInitiated,   // slave mode
    0,                     // connection speed: placeholder
    ClockPolarityLow,      // clock polarity: placeholder
    ClockPhaseFirst,       // clock phase: placeholder
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
    0,                     // ResourceSourceIndex
                           // Resource usage
    )                      // Vendor Data

// Index 1
SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                           // MOSI - GPIO 10 - Pin 19
                           // MISO - GPIO 9  - Pin 21
                           // CE1  - GPIO 7  - Pin 26
    1,                     // Device selection (CE1)
    PolarityLow,           // Device selection polarity
    FourWireMode,          // wiremode
    0,                     // databit len: placeholder
    ControllerInitiated,   // slave mode
    0,                     // connection speed: placeholder
    ClockPolarityLow,      // clock polarity: placeholder
    ClockPhaseFirst,       // clock phase: placeholder
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
    0,                     // ResourceSourceIndex
                           // Resource usage
    )                      // Vendor Data

```

이러한 두 리소스가 동일한 버스와 연결 되어야 하는 소프트웨어는 어떻게 알 수 있나요? 버스 이름 및 리소스 인덱스 간의 매핑은 DSD에 지정 됩니다.

```cpp
Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }},
```

그러면 두 개의 칩 select 줄 (리소스 인덱스 0 및 1)이 포함 된 "SPI0" 라는 버스가 생성 됩니다. SPI 버스의 기능을 선언 하려면 몇 가지 추가 속성이 필요 합니다.

```cpp
Package(2) { "SPI0-MinClockInHz", 7629 },
Package(2) { "SPI0-MaxClockInHz", 125000000 },
```

**MinClockInHz** 및 **MaxClockInHz** 속성은 컨트롤러에서 지 원하는 최소 및 최대 클록 속도를 지정 합니다. API를 통해 사용자는이 범위를 벗어나는 값을 지정할 수 없습니다. 클럭 속도는 연결 설명자 (ACPI 섹션 6.4.3.8.2.2)의 _SPE 필드에서 SPB 드라이버로 전달 됩니다.

```cpp
Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }},
```

**SupportedDataBitLengths** 속성에는 컨트롤러에서 지 원하는 데이터 비트 길이가 나열 됩니다. 쉼표로 구분 된 목록에 값을 여러 개 지정할 수 있습니다. API를 통해 사용자는이 목록 외부의 값을 지정할 수 없습니다. 데이터 비트 길이는 연결 설명자 (ACPI 섹션 6.4.3.8.2.2)의 _LEN 필드에서 SPB 드라이버로 전달 됩니다.

이러한 리소스 선언은 "템플릿"으로 생각할 수 있습니다. 일부 필드는 시스템 부팅 시 고정 되 고 다른 일부는 런타임에 동적으로 지정 됩니다. SPISerialBus 설명자의 다음 필드는 고정 되어 있습니다.

- DeviceSelection
- DeviceSelectionPolarity
- WireMode
- SlaveMode
- 자원 Ource

다음 필드는 런타임에 사용자가 지정 하는 값에 대 한 자리 표시자입니다.

- DataBitLength
- ConnectionSpeed
- ClockPolarity
- ClockPhase

SPI1에는 단일 칩 select 줄만 포함 되어 있으므로 단일 `SPISerialBus()` 리소스가 선언 됩니다.

```cpp
// Index 2
SPISerialBus(              // SCKL - GPIO 21 - Pin 40
                           // MOSI - GPIO 20 - Pin 38
                           // MISO - GPIO 19 - Pin 35
                           // CE1  - GPIO 17 - Pin 11
    1,                     // Device selection (CE1)
    PolarityLow,           // Device selection polarity
    FourWireMode,          // wiremode
    0,                     // databit len: placeholder
    ControllerInitiated,   // slave mode
    0,                     // connection speed: placeholder
    ClockPolarityLow,      // clock polarity: placeholder
    ClockPhaseFirst,       // clock phase: placeholder
    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
    0,                     // ResourceSourceIndex
                           // Resource usage
    )                      // Vendor Data

```

필요한 이름 선언 – DSD에 지정 되 고이 리소스 선언의 인덱스를 참조 합니다.

```cpp
Package(2) { "bus-SPI-SPI1", Package() { 2 }},
```

그러면 "SPI1" 라는 버스가 만들어지고 리소스 인덱스 2에 연결 됩니다.

#### <a name="spi-driver-requirements"></a>SPI 드라이버 요구 사항

- 사용 `SpbCx` 하거나 SpbCx 호환 가능 해야 합니다.
- [MITT SPI 테스트](/windows-hardware/drivers/spb/spi-tests-in-mitt) 를 통과 해야 합니다.
- 4Mhz 클록 속도를 지원 해야 합니다.
- 8 비트 데이터 길이를 지원 해야 합니다.
- 모든 SPI 모드 (0, 1, 2, 3)를 지원 해야 합니다.

### <a name="i2c"></a>I2C

다음으로는 I2C 리소스를 선언 합니다. Raspberry Pi는 핀 3과 5에 단일 I2C 버스를 제공 합니다.

```cpp
// Index 3
I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1)
    0xFFFF,                // SlaveAddress: placeholder
    ,                      // SlaveMode: default to ControllerInitiated
    0,                     // ConnectionSpeed: placeholder
    ,                      // Addressing Mode: placeholder
    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name
    ,
    ,
    )                      // VendorData

```

함께 제공 되는 이름 선언 (필수)은 DSD에 지정 되어 있습니다.

```cpp
Package(2) { "bus-I2C-I2C1", Package() { 3 }},
```

이는 위에서 선언한 I2CSerialBus () 리소스의 인덱스인 리소스 인덱스 3을 참조 하는 이름이 "I2C1" 인 I2C 버스를 선언 합니다.

I2CSerialBus () 설명자의 다음 필드는 고정 되어 있습니다.

- SlaveMode
- 자원 Ource

다음 필드는 런타임에 사용자가 지정 하는 값에 대 한 자리 표시자입니다.

- SlaveAddress
- ConnectionSpeed
- AddressingMode

#### <a name="i2c-driver-requirements"></a>I2C 드라이버 요구 사항

- SpbCx를 사용 하거나 SpbCx와 호환 되어야 합니다.
- [MITT I2C 테스트](/windows-hardware/drivers/spb/run-mitt-tests-for-an-i2c-controller-) 를 통과 해야 합니다.
- 7 비트 주소 지정을 지원 해야 합니다.
- 100kHz 클럭 속도를 지원 해야 합니다.
- 400kHz 클럭 속도를 지원 해야 합니다.

### <a name="gpio"></a>GPIO

그런 다음 사용자 모드로 노출 되는 모든 GPIO 핀을 선언 합니다. 표시할 핀을 결정할 때 다음 지침을 제공 합니다.

- 노출 된 헤더의 모든 pin을 선언 합니다.
- 단추 및 Led와 같은 유용한 온보드 기능에 연결 된 핀을 선언 합니다.
- 시스템 함수에 대해 예약 되었거나 어떤 것에도 연결 되지 않은 pin은 선언 하지 마십시오.

ASL의 다음 블록은 GPIO4 및 GPIO5 라는 두 개의 핀을 선언 합니다. 다른 pin은 간단 하 게 나타내기 위해 여기에 표시 되지 않습니다. 부록 C에는 GPIO 리소스를 생성 하는 데 사용할 수 있는 샘플 powershell 스크립트가 포함 되어 있습니다.

```cpp
// Index 4 – GPIO 4
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 4 }
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 4 }

// Index 6 – GPIO 5
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 5 }
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 5 }
```

GPIO 핀을 선언할 때 다음 요구 사항을 준수 해야 합니다.

- 메모리 매핑된 GPIO 컨트롤러만 지원 됩니다. I2C/SPI가 되는 GPIO 컨트롤러는 지원 되지 않습니다. 컨트롤러 드라이버는 [CLIENT_QueryControllerBasicInformation](/windows-hardware/drivers/ddi/content/gpioclx/nc-gpioclx-gpio_client_query_controller_basic_information) 콜백에 대 한 응답으로 [CLIENT_CONTROLLER_BASIC_INFORMATION](/windows-hardware/drivers/ddi/content/gpioclx/ns-gpioclx-_client_controller_basic_information) 구조에 [MemoryMappedController](/windows-hardware/drivers/ddi/content/gpioclx/ns-gpioclx-_controller_attribute_flags) 플래그를 설정 하는 경우 메모리 매핑된 컨트롤러입니다.
- 각 pin에는 GpioIO 및 Gpioio 리소스가 모두 필요 합니다. GpioInt 리소스는 Gpioint 리소스 바로 뒤에와 야 하며 동일한 pin 번호를 참조 해야 합니다.
- GPIO 리소스는 pin 번호를 늘려서 정렬 되어야 합니다.
- 각 GpioIO 및 Gpioio 리소스는 pin 목록에서 정확히 하나의 pin 번호를 포함 해야 합니다.
- 두 설명자의 ShareType 필드를 공유 해야 합니다.
- GpioInt 설명자의 EdgeLevel 필드는 Edge 여야 합니다.
- GpioInt 설명자의 ActiveLevel 필드는 모두 활성 이어야 합니다.
- PinConfig 필드
  - GpioIO 및 Gpioio 설명자 모두에서 동일 해야 합니다.
  - PullUp, 풀 다운 또는 PullNone 중 하나 여야 합니다. PullDefault 수 없습니다.
  - 끌어오기 구성은 핀의 전원 켜기 상태와 일치 해야 합니다. 지정 된 풀 모드의 전원 켜기 상태에서 pin을 지정 하면 pin의 상태를 변경 하지 않아야 합니다. 예를 들어, 데이터 시트가 풀을 사용 하 여 pin을 제공 하도록 지정 하는 경우 PullUp로 PinConfig를 지정 합니다.

펌웨어, UEFI 및 드라이버 초기화 코드는 부팅 하는 동안 전원 켜기 상태에서 pin의 상태를 변경 하면 안 됩니다. 사용자만 pin에 연결 된 것을 알고 있으므로 안전 하 게 상태를 전환 합니다. 사용자가 pin을 올바르게 인터페이스 하는 하드웨어를 디자인할 수 있도록 각 pin의 전원 켜기 상태를 문서화 해야 합니다. Pin은 부팅 하는 동안 예기치 않게 상태를 변경 해서는 안 됩니다.

#### <a name="supported-drive-modes"></a>지원 되는 드라이브 모드

GPIO 컨트롤러에서 기본 제공 풀을 지원 하 고 높은 임피던스 입력 및 CMOS 출력 외에도 pull을 가져오는 경우 선택적 Supported Emodes 속성을 사용 하 여이를 지정 해야 합니다.

```cpp
Package (2) { “GPIO-SupportedDriveModes”, 0xf },
```

Supported드라이브 Emodes 속성은 GPIO 컨트롤러에서 지원 되는 드라이브 모드를 나타냅니다. 위의 예에서는 다음 드라이브 모드를 모두 지원 합니다. 속성은 다음 값의 비트 마스크입니다.

| 플래그 값 | 드라이브 모드 | Description |
|------------|------------|-------------|
| 0x1        | InputHighImpedance | Pin은 ACPI의 "PullNone" 값에 해당 하는 high 임피던스 입력을 지원 합니다. |
| 0x2        | InputPullUp | Pin은 기본 제공 풀을 지원 합니다 .이 저항기는 ACPI의 "PullUp" 값에 해당 합니다. |
| 0x4        | InputPullDown 다운 | Pin은 기본 제공 풀 다운 저항기를 지원 합니다 .이는 ACPI의 "풀 풀" 값에 해당 합니다. |
| 0x8        | OutputCmos | Pin은 오픈 드레이닝이 아닌 강력한 최대값 표현 및 강력한 최소값를 모두 생성할 수 있도록 지원 합니다. |

InputHighImpedance 및 OutputCmos는 거의 모든 GPIO 컨트롤러에서 지원 됩니다. Supported드라이브 Emodes 속성이 지정 되지 않은 경우 기본값입니다.

표시 된 헤더에 도달 하기 전에 GPIO 신호가 수준 이동 기를 통과 하는 경우에는 외부 헤더에서 드라이브 모드가 관찰 가능 하지 않아도 SOC에서 지 원하는 드라이브 모드를 선언 합니다. 예를 들어 pin이 resistive pull을 사용 하 여 오픈 드레이닝으로 표시 되는 양방향 수준 전환을 통과 하는 경우 pin이 상위 임피던스 입력으로 구성 된 경우에도 노출 된 헤더에서 높은 임피던스 상태를 관찰 하지 않습니다. Pin이 high 임피던스 입력을 지원함을 여전히 선언 해야 합니다.

#### <a name="pin-numbering"></a>Pin 번호 매기기

Windows에서는 두 개의 pin 번호 매기기 스키마를 지원 합니다.

- 순차 Pin 번호 매기기 – 사용자에 게 0, 1, 2 등의 숫자가 표시 됩니다. 최대 노출 된 핀 수 0은 ASL에서 선언 된 첫 번째 GpioIo 리소스이 고, 1은 ASL에 선언 된 두 번째 GpioIo 리소스입니다.
- 기본 Pin 번호 매기기 – 사용자에 게 GpioIo 설명자에 지정 된 Pin 번호 (예: 4, 5, 12, 13 등)가 표시 됩니다.

```cpp
Package (2) { “GPIO-UseDescriptorPinNumbers”, 1 },
```

**UseDescriptorPinNumbers** 속성은 Windows에서 순차적인 pin 번호 매기기 대신 기본 pin 번호를 사용 하도록 지시 합니다. UseDescriptorPinNumbers 속성을 지정 하지 않거나 값이 0 인 경우 Windows는 기본적으로 순차적 pin 번호 매기기를 지정 합니다.

기본 pin 번호 매기기를 사용 하는 경우에는 **Pincount** 속성도 지정 해야 합니다.

```cpp
Package (2) { “GPIO-PinCount”, 54 },
```

**Pincount** 속성은 드라이버의 [CLIENT_QueryControllerBasicInformation](/windows-hardware/drivers/ddi/content/gpioclx/nc-gpioclx-gpio_client_query_controller_basic_information) 콜백에서 **totalpins** 속성을 통해 반환 되는 값과 일치 해야 합니다 `GpioClx` .

보드의 기존 게시 된 설명서와 가장 호환 되는 번호 매기기 구성표를 선택 합니다. 예를 들어, 많은 기존 핀아웃 다이어그램이 BCM2835 pin 번호를 사용 하기 때문에 Raspberry Pi는 네이티브 pin 번호 매기기를 사용 합니다. MinnowBoardMax는 기존 핀아웃 다이어그램이 거의 없기 때문에 순차적 pin 번호 매기기를 사용 하 고, 순차적인 pin 번호 매기기는 10 개의 pin만 200 핀 이상 노출 되므로 개발자 환경을 단순화 합니다. 개발자 혼동을 줄이기 위해 순차적 또는 기본 pin 번호 매기기를 사용 하는 결정을 내려야 합니다.

#### <a name="gpio-driver-requirements"></a>GPIO 드라이버 요구 사항

- 사용 해야 함 `GpioClx`
- SOC 메모리를 매핑해야 합니다.
- 에뮬레이트된 ActiveBoth 인터럽트 처리를 사용 해야 합니다.

### <a name="uart"></a>UART

UART 드라이버가 `SerCx` 또는를 사용 하 `SerCx2` 는 경우 rhproxy를 사용 하 여 사용자 모드에 드라이버를 노출할 수 있습니다. 유형의 장치 인터페이스를 만드는 UART 드라이버는 `GUID_DEVINTERFACE_COMPORT` rhproxy를 사용할 필요가 없습니다. 수신함 `Serial.sys` 드라이버는 이러한 경우 중 하나입니다.

`SerCx`사용자 모드에 스타일 UART를 노출 하려면 `UARTSerialBus` 다음과 같이 리소스를 선언 합니다.

```cpp
// Index 2
UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2
    115200,                // InitialBaudRate: in bits ber second
    ,                      // BitsPerByte: default to 8 bits
    ,                      // StopBits: Defaults to one bit
    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
    ,                      // IsBigEndian: default to LittleEndian
    ,                      // Parity: Defaults to no parity
    ,                      // FlowControl: Defaults to no flow control
    32,                    // ReceiveBufferSize
    32,                    // TransmitBufferSize
    "\\_SB.URT2",          // ResourceSource: UART bus controller name
    ,
    ,
    ,
    )
```

다른 모든 필드는 런타임에 사용자가 지정 하는 값에 대 한 자리 표시자 인 반면에는 Sourceource 필드만 고정 되어 있습니다.

함께 제공 되는 이름 선언은 다음과 같습니다.

```cpp
Package(2) { "bus-UART-UART2", Package() { 2 }},
```

이렇게 하면 사용자가 사용자 모드에서 bus에 액세스 하는 데 사용 하는 식별자 인 "UART2" 라는 이름이 컨트롤러에 할당 됩니다.

## <a name="runtime-pin-muxing"></a>런타임 Pin Muxing

Pin muxing는 다른 기능에 동일한 실제 pin을 사용 하는 기능입니다. I2C 컨트롤러, SPI 컨트롤러 및 GPIO 컨트롤러와 같은 여러 개의 다양 한 칩 주변 장치가 SOC의 동일한 실제 pin으로 라우팅될 수 있습니다. Mux 블록은 지정 된 시간에 핀에서 활성 상태인 함수를 제어 합니다. 일반적으로 펌웨어는 부팅 시 함수 할당을 설정 하는 일을 담당 하며,이 할당은 부팅 세션을 통해 정적으로 유지 됩니다. 런타임 pin muxing는 런타임에 고정 함수 할당을 다시 구성 하는 기능을 추가 합니다. 사용자가 런타임에 고정 함수를 선택할 수 있도록 하면 사용자가 보드의 pin을 신속 하 게 다시 구성할 수 있도록 하 여 개발 속도를 높이고, 하드웨어는 정적 구성 보다 더 광범위 한 응용 프로그램을 지원할 수 있습니다.

사용자는 추가 코드를 작성 하지 않고 GPIO, I2C, SPI 및 UART에 대해 muxing 지원을 사용 합니다. 사용자가 [Openpin ()](/uwp/api/windows.devices.gpio.gpiocontroller.openpin) 또는 [FromIdAsync ()](/uwp/api/windows.devices.i2c.i2cdevice.fromidasync)를 사용 하 여 GPIO 또는 버스를 열면 기본 실제 pin은 요청 된 함수에 자동으로 muxed 됩니다. 다른 함수에서 pin을 이미 사용 하 고 있으면 OpenPin () 또는 FromIdAsync () 호출이 실패 합니다. 사용자가 [Gpiopin](/uwp/api/windows.devices.gpio.gpiopin), [I2cDevice](/uwp/api/windows.devices.i2c.i2cdevice), [SpiDevice](/uwp/api/windows.devices.spi.spidevice)또는 [SerialDevice](/uwp/api/windows.devices.serialcommunication.serialdevice) 개체를 삭제 하 여 장치를 닫으면 pin이 해제 되므로 나중에 다른 기능에 대해 열 수 있습니다.

Windows에는 [Gpioclx](/windows-hardware/drivers/ddi/content/index), [Spbcx](/windows-hardware/drivers/spb/spb-framework-extension)및 [sercx](/windows-hardware/drivers/ddi/content/index) 프레임 워크의 고정 muxing에 대 한 기본 제공 지원이 포함 되어 있습니다. 이러한 프레임 워크를 함께 사용 하 여 GPIO 핀 또는 버스에 액세스할 때 pin을 올바른 함수로 자동 전환 합니다. Pin에 대 한 액세스는 여러 클라이언트 간의 충돌을 방지 하기 위해 arbitrated 됩니다. 이러한 기본 제공 지원 외에도 pin muxing의 인터페이스와 프로토콜은 일반적인 용도로, 추가 장치 및 시나리오를 지원 하도록 확장할 수 있습니다.

이 문서에서는 먼저 고정 muxing 관련 된 기본 인터페이스와 프로토콜에 대해 설명한 다음 GpioClx, SpbCx 및 SerCx controller 드라이버에 muxing 고정에 대 한 지원을 추가 하는 방법을 설명 합니다.

### <a name="pin-muxing-architecture"></a>Muxing 아키텍처 고정

이 섹션에서는 고정 muxing 관련 된 기본 인터페이스 및 프로토콜에 대해 설명 합니다. GpioClx/SpbCx/SerCx 드라이버를 사용 하 여 pin muxing을 지원 하기 위해 기본 프로토콜에 대 한 지식이 반드시 필요한 것은 아닙니다. GpioCls/SpbCx/SerCx 드라이버에서 pin muxing을 지 원하는 방법에 대 한 자세한 내용은 [GpioClx 클라이언트 드라이버에서 pin muxing 지원 구현](#supporting-muxing-support-in-gpioclx-client-drivers) 및 [spbcx 및 sercx controller 드라이버에서 muxing 지원](#supporting-muxing-in-spbcx-and-sercx-controller-drivers)사용을 참조 하세요.

Pin muxing는 여러 구성 요소가 협력 하 여 수행 됩니다.

- Muxing 서버 고정 – pin muxing 제어 블록을 제어 하는 드라이버입니다. Muxing 서버 고정은 요청을 통해 muxing 리소스를 예약 하는 요청을 통해 클라이언트로부터 muxing 요청을 수신 하 고 ( *IRP_MJ_CREATE*) 요청을 통해 pin의 함수를 전환 하는 요청을 받습니다 (* IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS 요청을 통해). Muxing 블록은 종종 GPIO 블록의 일부 이기 때문에 핀 muxing 서버는 일반적으로 GPIO 드라이버입니다. Muxing 블록이 별도의 주변 장치인 경우에도 GPIO 드라이버는 muxing 기능을 배치 하는 논리적인 위치입니다.
- Pin muxing 클라이언트 – pin muxing를 사용 하는 드라이버입니다. Pin muxing 클라이언트는 ACPI 펌웨어에서 pin muxing 리소스를 수신 합니다. Pin muxing 리소스는 연결 리소스의 형식이 며 리소스 허브에 의해 관리 됩니다. Pin muxing 클라이언트는 리소스에 대 한 핸들을 열어 muxing 리소스를 예약 합니다. 하드웨어 변경을 적용 하려면 클라이언트가 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 요청을 전송 하 여 구성을 커밋해야 합니다. 클라이언트는 핸들을 닫아 muxing 리소스를 해제 합니다. 이때 muxing 구성이 기본 상태로 되돌려집니다.
- ACPI 펌웨어 – 리소스를 사용 하 여 muxing 구성을 지정 합니다 `MsftFunctionConfig()` . MsftFunctionConfig 리소스는 클라이언트에 필요한 muxing 구성의 핀을 표현 합니다. MsftFunctionConfig 리소스에는 함수 번호, 끌어오기 구성 및 pin 번호 목록이 포함 됩니다. MsftFunctionConfig 리소스는 muxing 클라이언트를 하드웨어 리소스로 고정 하기 위해 제공 됩니다 .이 리소스는 해당 PrepareHardware 콜백에서 GPIO 및 SPB 연결 리소스와 유사 하 게 수신 됩니다. 클라이언트는 리소스에 대 한 핸들을 여는 데 사용할 수 있는 리소스 허브 ID를 수신 합니다.

> `/MsftInternal` `asl.exe` `MsftFunctionConfig()` 이러한 설명자는 현재 ACPI 작업 위원회에서 검토 하 고 있으므로 설명자를 포함 하는 asl 파일을 컴파일하려면로 명령줄 스위치를 전달 해야 합니다. `asl.exe /MsftInternal dsdt.asl`

Pin muxing 관련 된 작업의 시퀀스는 다음과 같습니다.

![Muxing 클라이언트 서버 상호 작용 고정](images/usermode-access-diagram-1.png)

1. 클라이언트는 [Evtdevicepreparehardware ()](/windows-hardware/drivers/ddi/content/wdfdevice/nc-wdfdevice-evt_wdf_device_prepare_hardware) 콜백의 ACPI 펌웨어에서 MsftFunctionConfig 리소스를 수신 합니다.
2. 클라이언트는 리소스 허브 도우미 함수를 사용 하 여 `RESOURCE_HUB_CREATE_PATH_FROM_ID()` 리소스 ID에서 경로를 만든 다음 [Zwcreatefile ()](/windows-hardware/drivers/ddi/content/ntifs/nf-ntifs-ntcreatefile), [iogetdeviceobjectpointer ()](/windows-hardware/drivers/ddi/content/wdm/nf-wdm-iogetdeviceobjectpointer)또는 [wdfiotargetopen ()](/windows-hardware/drivers/ddi/content/wdfiotarget/nf-wdfiotarget-wdfiotargetopen)을 사용 하 여 경로에 대 한 핸들을 엽니다.
3. 서버는 리소스 허브 도우미 함수를 사용 하 여 파일 경로에서 리소스 허브 ID를 추출 하 `RESOURCE_HUB_ID_FROM_FILE_NAME()` 고 리소스 허브를 쿼리하여 리소스 설명자를 가져옵니다.
4. 서버는 설명자의 각 핀에 대해 중재 공유를 수행 하 고 IRP_MJ_CREATE 요청을 완료 합니다.
5. 클라이언트에서 수신 된 핸들에 대 한 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 요청을 발급 합니다.
6. *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 에 대 한 응답으로 서버는 각 pin에서 지정 된 기능을 활성화 하 여 하드웨어 muxing 작업을 수행 합니다.
7. 클라이언트는 요청 된 pin muxing 구성에 종속 된 작업을 진행 합니다.
8. 클라이언트에서 더 이상 pin을 muxed 필요가 없으면 핸들을 닫습니다.
9. 종결 중인 핸들에 대 한 응답으로 서버는 pin을 다시 초기 상태로 되돌립니다.

### <a name="protocol-description-for-pin-muxing-clients"></a>Pin muxing 클라이언트에 대 한 프로토콜 설명

이 섹션에서는 클라이언트에서 pin muxing 기능을 사용 하는 방법을 설명 합니다. `SerCx` `SpbCx` 프레임 워크는 컨트롤러 드라이버를 대신 하 여이 프로토콜을 구현 하기 때문에이는 및 컨트롤러 드라이버에는 적용 되지 않습니다.

#### <a name="parsing-resources"></a>리소스 구문 분석

WDF 드라이버는 `MsftFunctionConfig()` [Evtdevicepreparehardware ()](/windows-hardware/drivers/ddi/content/wdfdevice/nc-wdfdevice-evt_wdf_device_prepare_hardware) 루틴에서 리소스를 수신 합니다. MsftFunctionConfig 리소스는 다음 필드를 통해 식별할 수 있습니다.

```cpp
CM_PARTIAL_RESOURCE_DESCRIPTOR::Type = CmResourceTypeConnection
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Class = CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Type = CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG
```

루틴은 다음과 `EvtDevicePrepareHardware()` 같이 MsftFunctionConfig 리소스를 추출할 수 있습니다.

```cpp
EVT_WDF_DEVICE_PREPARE_HARDWARE evtDevicePrepareHardware;

_Use_decl_annotations_
NTSTATUS
evtDevicePrepareHardware (
    WDFDEVICE WdfDevice,
    WDFCMRESLIST ResourcesTranslated
    )
{
    PAGED_CODE();

    LARGE_INTEGER connectionId;
    ULONG functionConfigCount = 0;

    const ULONG resourceCount = WdfCmResourceListGetCount(ResourcesTranslated);
    for (ULONG index = 0; index < resourceCount; ++index) {
        const CM_PARTIAL_RESOURCE_DESCRIPTOR* resDescPtr =
            WdfCmResourceListGetDescriptor(ResourcesTranslated, index);

        switch (resDescPtr->Type) {
        case CmResourceTypeConnection:
            switch (resDescPtr->u.Connection.Class) {
            case CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG:
                switch (resDescPtr->u.Connection.Type) {
                case CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG:
                    switch (functionConfigCount) {
                    case 0:
                        // save the connection ID
                        connectionId.LowPart = resDescPtr->u.Connection.IdLowPart;
                        connectionId.HighPart = resDescPtr->u.Connection.IdHighPart;
                        break;
                    } // switch (functionConfigCount)
                    ++functionConfigCount;
                    break; // CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG

                } // switch (resDescPtr->u.Connection.Type)
                break; // CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
            } // switch (resDescPtr->u.Connection.Class)
            break;
        } // switch
    } // for (resource list)

    if (functionConfigCount < 1) {
        return STATUS_INVALID_DEVICE_CONFIGURATION;
    }
    // TODO: save connectionId in the device context for later use

    return STATUS_SUCCESS;
}
```

#### <a name="reserving-and-committing-resources"></a>리소스 예약 및 커밋

클라이언트는 pin을 mux 하려고 할 때 MsftFunctionConfig 리소스를 예약 하 고 커밋합니다. 다음 예제에서는 클라이언트가 MsftFunctionConfig 리소스를 예약 하 고 커밋하는 방법을 보여 줍니다.

```cpp
_IRQL_requires_max_(PASSIVE_LEVEL)
NTSTATUS AcquireFunctionConfigResource (
    WDFDEVICE WdfDevice,
    LARGE_INTEGER ConnectionId,
    _Out_ WDFIOTARGET* ResourceHandlePtr
    )
{
    PAGED_CODE();

    //
    // Form the resource path from the connection ID
    //
    DECLARE_UNICODE_STRING_SIZE(resourcePath, RESOURCE_HUB_PATH_CHARS);
    NTSTATUS status = RESOURCE_HUB_CREATE_PATH_FROM_ID(
            &resourcePath,
            ConnectionId.LowPart,
            ConnectionId.HighPart);
    if (!NT_SUCCESS(status)) {
        return status;
    }

    //
    // Create a WDFIOTARGET
    //
    WDFIOTARGET resourceHandle;
    status = WdfIoTargetCreate(WdfDevice, WDF_NO_ATTRIBUTES, &resourceHandle);
    if (!NT_SUCCESS(status)) {
        return status;
    }

    //
    // Reserve the resource by opening a WDFIOTARGET to the resource
    //
    WDF_IO_TARGET_OPEN_PARAMS openParams;
    WDF_IO_TARGET_OPEN_PARAMS_INIT_OPEN_BY_NAME(
        &openParams,
        &resourcePath,
        FILE_GENERIC_READ | FILE_GENERIC_WRITE);

    status = WdfIoTargetOpen(resourceHandle, &openParams);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    //
    // Commit the resource
    //
    status = WdfIoTargetSendIoctlSynchronously(
            resourceHandle,
            WDF_NO_HANDLE,      // WdfRequest
            IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS,
            nullptr,            // InputBuffer
            nullptr,            // OutputBuffer
            nullptr,            // RequestOptions
            nullptr);           // BytesReturned

    if (!NT_SUCCESS(status)) {
        WdfIoTargetClose(resourceHandle);
        return status;
    }

    //
    // Pins were successfully muxed, return the handle to the caller
    //
    *ResourceHandlePtr = resourceHandle;
    return STATUS_SUCCESS;
}
```

드라이버는 나중에 닫을 수 있도록 해당 컨텍스트 영역 중 하나에 WDFIOTARGET을 저장 해야 합니다. 드라이버가 muxing 구성을 해제할 준비가 되 면 WDFOBJECTDELETE을 다시 사용 하려는 경우에는 [Wdfobjectdelete ()](/windows-hardware/drivers/ddi/content/wdfobject/nf-wdfobject-wdfobjectdelete)또는 [Wdfiotargetclose ()](/windows-hardware/drivers/ddi/content/wdfiotarget/nf-wdfiotarget-wdfiotargetclose) 를 호출 하 여 리소스 핸들을 닫아야 합니다.

```cpp
    WdfObjectDelete(resourceHandle);
```

클라이언트에서 해당 리소스 핸들을 닫으면 pin은 다시 초기 상태로 muxed 다른 클라이언트에서 가져올 수 있습니다.

### <a name="protocol-description-for-pin-muxing-servers"></a>Pin muxing 서버에 대 한 프로토콜 설명

이 섹션에서는 pin muxing 서버가 해당 기능을 클라이언트에 노출 하는 방법을 설명 합니다. 이는 `GpioClx` 프레임 워크에서 클라이언트 드라이버를 대신 하 여이 프로토콜을 구현 하기 때문에 미니 포트 드라이버에는 적용 되지 않습니다. 클라이언트 드라이버에서 pin muxing을 지 원하는 방법에 대 한 자세한 `GpioClx` 내용은 [GpioClx 클라이언트 드라이버에서 Muxing 지원 구현](#supporting-muxing-support-in-gpioclx-client-drivers)을 참조 하세요.

#### <a name="handling-irp_mj_create-requests"></a>IRP_MJ_CREATE 요청 처리

클라이언트는 pin muxing 리소스를 예약 하려고 할 때 리소스에 대 한 핸들을 엽니다. Pin muxing 서버는 리소스 허브에서 재분석 작업을 통해 *IRP_MJ_CREATE* 요청을 받습니다. *IRP_MJ_CREATE* 요청의 후행 경로 구성 요소에는 64 비트 정수인 16 진수 형식의 리소스 허브 ID가 포함 되어 있습니다. 서버는 reshub. h에서를 사용 하 여 파일 이름에서 리소스 허브 ID를 추출 하 `RESOURCE_HUB_ID_FROM_FILE_NAME()` 고, *IOCTL_RH_QUERY_CONNECTION_PROPERTIES* 리소스 허브로 보내서 설명자를 가져옵니다 `MsftFunctionConfig()` .

서버에서 설명자의 유효성을 검사 하 고 설명자에서 공유 모드 및 pin 목록을 추출 해야 합니다. 그런 다음 pin에 대해 공유 조정을 수행 하 고 성공 하면 요청을 완료 하기 전에 pin을 예약 된 것으로 표시 합니다.

Pin 목록의 각 pin에 대해 공유 조정을 성공적으로 수행 하면 공유가 전체적으로 성공 합니다. 각 pin은 다음과 같이 arbitrated 되어야 합니다.

- Pin이 아직 예약 되지 않은 경우 공유 중재가 성공 합니다.
- Pin이 이미 단독으로 예약 되어 있으면 공유 중재가 실패 합니다.
- Pin이 이미 공유로 예약 된 경우
  - 그리고 들어오는 요청을 공유 하 고 공유 중재를 성공적으로 수행 했습니다.
  - 들어오는 요청은 배타적 이므로 공유 중재가 실패 합니다.

중재 공유가 실패 하면 *STATUS_GPIO_INCOMPATIBLE_CONNECT_MODE* 를 사용 하 여 요청을 완료 해야 합니다. 중재 공유가 성공 하면 *STATUS_SUCCESS* 를 사용 하 여 요청을 완료 해야 합니다.

들어오는 요청의 공유 모드는 [Irpsp >매개 변수가](/windows-hardware/drivers/ifs/irp-mj-create)아닌 MsftFunctionConfig 설명자에서 가져와야 합니다. Shareaccess를 만듭니다.

#### <a name="handling-ioctl_gpio_commit_function_config_pins-requests"></a>IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS 요청 처리

클라이언트는 핸들을 열어 MsftFunctionConfig 리소스를 성공적으로 예약한 후에 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 를 전송 하 여 실제 하드웨어 muxing 작업을 수행 하도록 서버를 요청할 수 있습니다. 서버 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 수신 되 면 pin 목록의 각 핀에 대해

- PNP_FUNCTION_CONFIG_DESCRIPTOR 구조의 PinConfiguration 구성원에 지정 된 끌어오기 모드를 하드웨어로 설정 합니다.
- PNP_FUNCTION_CONFIG_DESCRIPTOR 구조체의 FunctionNumber 멤버로 지정 된 함수에 대 한 pin을 Mux 합니다.

그러면 서버에서 *STATUS_SUCCESS* 를 사용 하 여 요청을 완료 해야 합니다.

FunctionNumber의 의미는 서버에서 정의 되며 서버에서이 필드를 해석 하는 방법에 대 한 정보를 사용 하 여 MsftFunctionConfig 설명자를 작성 했음을 인식 합니다.

핸들이 닫히면 서버는 IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS 수신 되었을 때의 구성에 대 한 pin을 되돌려야 하므로 서버에서 pin 상태를 수정 하기 전에 저장 해야 할 수도 있습니다.

#### <a name="handling-irp_mj_close-requests"></a>IRP_MJ_CLOSE 요청 처리

클라이언트에 muxing 리소스가 더 이상 필요 하지 않은 경우 해당 핸들을 닫습니다. 서버 *IRP_MJ_CLOSE* 요청을 받으면 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 수신 되었을 때의 상태로 pin을 되돌려야 합니다. 클라이언트에서 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 보내지 않은 경우 아무 작업도 필요 하지 않습니다. 그런 다음 서버는 조정 공유와 관련 하 여 pin을 사용 가능으로 표시 하 고 *STATUS_SUCCESS* 를 사용 하 여 요청을 완료 해야 합니다. *IRP_MJ_CREATE* 처리를 사용 하 여 *IRP_MJ_CLOSE* 처리를 적절히 동기화 해야 합니다.

### <a name="authoring-guidelines-for-acpi-tables"></a>ACPI 테이블에 대 한 작성 지침

이 섹션에서는 클라이언트 드라이버에 muxing 리소스를 제공 하는 방법을 설명 합니다. 리소스를 포함 하는 테이블을 컴파일하려면 Microsoft ASL 컴파일러 빌드 14327 이상이 필요 합니다 `MsftFunctionConfig()` . `MsftFunctionConfig()` 리소스는 muxing 클라이언트를 하드웨어 리소스로 고정 하기 위해 제공 됩니다. `MsftFunctionConfig()` 고정 muxing 변경 (일반적으로 SPB 및 직렬 컨트롤러 드라이버)을 요구 하는 드라이버에 리소스를 제공 해야 하지만, 컨트롤러 드라이버가 muxing 구성을 처리 하므로 SPB 및 직렬 주변 장치 드라이버에 제공 하면 안 됩니다.
ACPI 매크로는 다음과 같이 `MsftFunctionConfig()` 정의 됩니다.

```cpp
  MsftFunctionConfig(Shared/Exclusive
                PinPullConfig,
                FunctionNumber,
                ResourceSource,
                ResourceSourceIndex,
                ResourceConsumer/ResourceProducer,
                VendorData) { Pin List }

```

- Shared/Exclusive – exclusive 인 경우 단일 클라이언트에서 한 번에이 pin을 가져올 수 있습니다. 공유 되는 경우 여러 공유 클라이언트에서 리소스를 획득할 수 있습니다. 조정되지 않은 여러 클라이언트가 변경 가능한 리소스에 액세스할 수 있도록 하면 데이터 경합이 발생하여 예측할 수 없는 결과가 발생할 수 있으므로 이 옵션은 항상 전용으로 설정하세요.
- PinPullConfig – 다음 중 하나
  - PullDefault – SOC에서 정의한 전원 켜기 기본 끌어오기 구성 사용
  - PullUp – 풀 저항기 사용
  - 풀 다운 사용 – 풀 다운 저항기 사용
  - PullNone – 모든 풀 저항기를 사용 하지 않도록 설정
- FunctionNumber – mux로 프로그래밍할 함수 번호입니다.
- Sourceource – pin muxing 서버의 ACPI 네임 스페이스 경로입니다.
- ResourceSourceIndex – 0으로 설정 합니다.
- ResourceConsumer/ResourceProducer – ResourceConsumer로 설정 합니다.
- VendorData – pin muxing 서버에 의해 정의 된 의미를 갖는 선택적 이진 데이터입니다. 일반적으로 비워 두어야 합니다.
- Pin 목록 – 구성이 적용 되는 pin 번호의 쉼표로 구분 된 목록입니다. 핀 muxing 서버가 GpioClx driver 인 경우 이러한 드라이버는 GPIO pin 번호 이며 GpioIo 설명자의 pin 번호와 동일한 의미를 갖습니다.

다음 예제에서는 MsftFunctionConfig () 리소스를 I2C 컨트롤러 드라이버에 제공 하는 방법을 보여 줍니다.

```cpp
Device(I2C1)
{
    Name(_HID, "BCM2841")
    Name(_CID, "BCMI2C")
    Name(_UID, 0x1)
    Method(_STA)
    {
        Return(0xf)
    }
    Method(_CRS, 0x0, NotSerialized)
    {
        Name(RBUF, ResourceTemplate()
        {
            Memory32Fixed(ReadWrite, 0x3F804000, 0x20)
            Interrupt(ResourceConsumer, Level, ActiveHigh, Shared) { 0x55 }
            MsftFunctionConfig(Exclusive, PullUp, 4, "\\_SB.GPI0", 0, ResourceConsumer, ) { 2, 3 }
        })
        Return(RBUF)
    }
}
```

일반적으로 컨트롤러 드라이버에 필요한 메모리 및 인터럽트 리소스 외에 `MsftFunctionConfig()` 도 리소스가 지정 됩니다. 이 리소스를 사용 하면 I2C 컨트롤러 드라이버가 _SB에서 장치 노드에 의해 고정 2와 3으로 관리 되도록 설정할 수 있습니다 \\ . GPIO0 – 풀 저항기를 사용 하는 함수 4에서

## <a name="supporting-muxing-support-in-gpioclx-client-drivers"></a>GpioClx 클라이언트 드라이버에서 muxing 지원 지원

`GpioClx` 는 pin muxing를 기본적으로 지원 합니다. GpioClx 미니 포트 드라이버 ("GpioClx 클라이언트 드라이버" 라고도 함), GPIO 컨트롤러 하드웨어 드라이브 Windows 10 빌드 14327부터 GpioClx 미니 포트 드라이버는 다음과 같은 두 개의 새로운 DDIs 구현 하 여 pin muxing에 대 한 지원을 추가할 수 있습니다.

- CLIENT_ConnectFunctionConfigPins- `GpioClx` 지정 된 muxing 구성을 적용 하기 위해 미니 포트 드라이버 명령을 위해에 의해 호출 됩니다.
- CLIENT_DisconnectFunctionConfigPins – `GpioClx` 미니 포트 드라이버를 명령으로 호출 하 여 muxing 구성을 되돌립니다.

이러한 루틴에 대 한 설명은 [Gpioclx 이벤트 콜백 함수](/previous-versions/hh439464(v=vs.85)) 를 참조 하세요.

이러한 두 가지 새로운 DDIs 외에도 기존 DDIs는 pin muxing 호환성에 대해 감사 되어야 합니다.

- CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt – CLIENT_ConnectIoPins은 GpioClx를 명령 하 여 GPIO 입력 또는 출력에 대 한 설정 핀을 구성 하는 미니 포트 드라이버를 호출 합니다. GPIO는 MsftFunctionConfig와 함께 사용할 수 없습니다. 즉, 핀이 GPIO와 MsftFunctionConfig에 대해 동시에 연결 되지 않습니다. Pin의 기본 함수는 GPIO 일 필요가 없기 때문에 ConnectIoPins가 호출 될 때 pin은 muxed로 반드시 매핑되지 않을 수 있습니다. ConnectIoPins는 muxing 작업을 포함 하 여 GPIO IO를 위해 pin을 준비 하는 데 필요한 모든 작업을 수행 하는 데 필요 합니다. *CLIENT_ConnectInterrupt* 은 GPIO 입력의 특수 한 경우로 간주할 수 있으므로 비슷하게 동작 합니다.
- CLIENT_DisconnectIoPins/CLIENT_DisconnectInterrupt – PreserveConfiguration 플래그가 지정 되지 않은 경우 이러한 루틴은 CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt가 호출 되었을 때의 상태로 pin을 반환 해야 합니다. 포트는 고정 방향을 기본 상태로 되돌리는 것 외에도, 각 핀의 muxing 상태를 _Connect 루틴이 호출 되었을 때의 상태로 되돌려야 합니다.

예를 들어, pin의 기본 muxing 구성이 UART 이며 pin을 GPIO로 사용할 수도 있다고 가정 합니다. GPIO에 대 한 핀을 연결 하기 위해 CLIENT_ConnectIoPins를 호출 하는 경우 핀을 GPIO로 mux 하 고 CLIENT_DisconnectIoPins에서 pin을 다시 UART로 mux 해야 합니다. 일반적으로 연결 끊기 루틴은 연결 루틴에서 수행한 작업을 실행 취소 해야 합니다.

## <a name="supporting-muxing-in-spbcx-and-sercx-controller-drivers"></a>SpbCx 및 SerCx controller 드라이버에서 muxing 지원

Windows 10 빌드 14327 `SpbCx` 부터 및 `SerCx` 프레임 워크에는 `SpbCx` `SerCx` 컨트롤러 드라이버 자체에 대 한 코드 변경 없이 및 컨트롤러 드라이버가 muxing 클라이언트를 고정 하는 데 사용할 수 있는 pin muxing에 대 한 기본 제공 지원이 포함 됩니다. 확장을 통해 muxing 사용 SpbCx/SerCx 컨트롤러 드라이버에 연결 하는 SpbCx/SerCx 주변 드라이버는 pin muxing 작업을 트리거합니다.

다음 다이어그램에서는 이러한 각 구성 요소 간의 종속성을 보여 줍니다. 여기에서 볼 수 있듯이 pin muxing는 SerCx 및 SpbCx 컨트롤러 드라이버에서 일반적으로 muxing을 담당 하는 GPIO 드라이버에 대 한 종속성을 소개 합니다.

![Muxing 종속성 고정](images/usermode-access-diagram-2.png)

장치 초기화 시 `SpbCx` 및 `SerCx` 프레임 워크는 `MsftFunctionConfig()` 장치에 하드웨어 리소스로 제공 된 모든 리소스를 구문 분석 합니다. SpbCx/SerCx는 주문형 pin muxing 리소스를 획득 하 고 해제 합니다.

`SpbCx`클라이언트 드라이버의 [Evtspbtargetconnect ()](/windows-hardware/drivers/ddi/content/spbcx/nc-spbcx-evt_spb_target_connect) 콜백을 호출 하기 직전에 *IRP_MJ_CREATE* 처리기에 핀 muxing 구성을 적용 합니다. Muxing 구성을 적용할 수 없는 경우 컨트롤러 드라이버의 `EvtSpbTargetConnect()` 콜백이 호출 되지 않습니다. 따라서 SPB 컨트롤러 드라이버는가 호출 될 때 핀이 SPB 함수에 muxed 것으로 간주할 수 있습니다 `EvtSpbTargetConnect()` .

`SpbCx`컨트롤러 드라이버의 [Evtspbtargetdisconnect ()](/windows-hardware/drivers/ddi/content/spbcx/nc-spbcx-evt_spb_target_disconnect) 콜백을 호출한 후에 *IRP_MJ_CLOSE* 처리기에서 pin muxing 구성을 되돌립니다. 결과적으로, 주변 드라이버가 SPB 컨트롤러 드라이버에 대 한 핸들을 열 때마다 핀이 SPB 함수로 muxed 주변 드라이버가 해당 핸들을 닫으면 muxed 사라집니다.

`SerCx` 비슷하게 동작 합니다. `SerCx`컨트롤러 드라이버의 `MsftFunctionConfig()` [EvtSerCx2FileOpen ()](/windows-hardware/drivers/ddi/content/sercx/nc-sercx-evt_sercx2_fileopen) 콜백을 호출 하기 직전에 해당 *IRP_MJ_CREATE* 처리기의 모든 리소스를 가져오고, 컨트롤러 드라이버의 [EvtSerCx2FileClose](/windows-hardware/drivers/ddi/content/sercx/nc-sercx-evt_sercx2_fileclose) 콜백을 호출한 후에도 해당 IRP_MJ_CLOSE 처리기의 모든 리소스를 해제 합니다.

및 컨트롤러 드라이버에 대 한 dynamic pin muxing의 의미는 `SerCx` `SpbCx` 특정 시간에 spb/UART 함수에서 핀을 muxed 하는 것을 허용할 수 있어야 한다는 것입니다. 컨트롤러 드라이버는 또는가 호출 될 때까지 pin이 muxed 않는 것으로 가정 해야 `EvtSpbTargetConnect()` `EvtSerCx2FileOpen()` 합니다. 다음 콜백 중에 SPB/UART 함수를 muxed 핀은 필요 하지 않습니다. 다음은 전체 목록이 아니라 컨트롤러 드라이버가 구현 하는 가장 일반적인 PNP 루틴을 나타냅니다.

- DriverEntry
- EvtDriverDeviceAdd
- EvtDevicePrepareHardware/EvtDeviceReleaseHardware
- EvtDeviceD0Entry/EvtDeviceD0Exit

## <a name="verification"></a>확인

Rhproxy를 테스트할 준비가 되 면 다음 단계별 절차를 사용 하는 것이 좋습니다.

1. 각 `SpbCx` , `GpioClx` 및 `SerCx` 컨트롤러 드라이버가 로드 되 고 제대로 작동 하는지 확인 합니다.
1. `rhproxy`이 시스템에 있는지 확인 합니다. 일부 버전 및 Windows의 빌드에는 포함 되지 않습니다.
1. 를 사용 하 여 rhproxy 노드 컴파일 및 로드 `ACPITABL.dat`
1. 장치 노드가 있는지 확인 합니다. `rhproxy`
1. 를 `rhproxy` 로드 하 고 시작 하 고 있는지 확인 합니다.
1. 필요한 장치가 사용자 모드로 노출 되는지 확인 합니다.
1. 명령줄에서 각 장치와 상호 작용할 수 있는지 확인 합니다.
1. UWP 앱에서 각 장치와 상호 작용할 수 있는지 확인 합니다.
1. HLK 테스트 실행

### <a name="verify-controller-drivers"></a>컨트롤러 드라이버 확인

Rhproxy는 시스템에서 사용자 모드로 다른 장치를 노출 하므로 해당 장치가 이미 작동 하 고 있는 경우에만 작동 합니다. 첫 번째 단계는 해당 장치 (표시 하려는 I2C, SPI, GPIO 컨트롤러)가 이미 작동 하 고 있는지 확인 하는 것입니다.

명령 프롬프트에서 다음을 실행합니다.

```ps
devcon status *
```

출력을 확인 하 고 관심 있는 모든 장치가 시작 되었는지 확인 합니다. 장치에 문제 코드가 있는 경우 장치가 로드 되지 않는 이유를 해결 해야 합니다. 초기 플랫폼 bringup 중에 모든 장치를 사용 하도록 설정 해야 합니다. `SpbCx`, `GpioClx` 또는 `SerCx` 컨트롤러 드라이버는이 문서의 범위를 벗어난 문제를 해결 합니다.

### <a name="verify-that-rhproxy-is-present-on-the-system"></a>Rhproxy가 시스템에 있는지 확인 합니다.

`rhproxy`서비스가 시스템에 있는지 확인 합니다.

```ps
reg query HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\rhproxy
```

레지스트리 키가 없는 경우 rhproxy가 시스템에 존재 하지 않습니다. Rhproxy은 IoT Core 및 Windows Enterprise build 15063 이상의 모든 빌드에 표시 됩니다.

### <a name="compile-and-load-asl-with-acpitabldat"></a>ACPITABL를 사용 하 여 ASL 컴파일 및 로드

이제 rhproxy ASL 노드를 작성 했으므로 컴파일 및 로드 해야 합니다. 시스템 ACPI 테이블에 추가할 수 있는 독립 실행형 AML 파일로 rhproxy 노드를 컴파일할 수 있습니다. 또는 시스템의 ACPI 원본에 액세스할 수 있는 경우 rhproxy 노드를 플랫폼의 ACPI 테이블에 직접 삽입할 수 있습니다. 그러나 초기에는를 사용 하는 것이 더 쉬울 수 있습니다 `ACPITABL.dat` .

1. RHPX 라는 파일을 만들고 다음과 같이 DefinitionBlock 내에 추가 합니다.

    ```cpp
    DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
    {
        Scope (\_SB)
        {
            Device(RHPX)
            {
            ...
            }
        }
    }
    ```

2. [WDK](/windows-hardware/drivers/download-the-wdk) 를 다운로드 하 고 `asl.exe``C:\Program Files (x86)\Windows Kits\10\Tools\x64\ACPIVerify`
3. 다음 명령을 실행 하 여 ACPITABL를 생성 합니다.

    ```ps
    asl.exe yourboard.asl
    ```

4. 테스트 중인 시스템의 c:\system32에 결과 ACPITABL 파일을 복사 합니다.
5. 테스트 중인 시스템에서 testsigning을 설정 합니다.

    ```ps
    bcdedit /set testsigning on
    ```

6. 테스트 중인 시스템을 다시 부팅 합니다. ACPITABL.dat에 정의된 ACPI 테이블이 시스템 펌웨어 테이블에 추가됩니다.

### <a name="verify-that-the-rhproxy-device-node-exists"></a>Rhproxy 장치 노드가 있는지 확인 합니다.

다음 명령을 실행 하 여 rhproxy device 노드를 열거 합니다.

```ps
devcon status *msft8000
```

Devcon의 출력에는 장치가 존재 하는 것으로 표시 되어야 합니다. 장치 노드가 없는 경우 ACPI 테이블이 시스템에 추가 되지 않았습니다.

### <a name="verify-that-rhproxy-is-loading-and-starting"></a>Rhproxy를 로드 하 고 시작 하 고 있는지 확인 합니다.

Rhproxy의 상태를 확인 합니다.

```ps
devcon status *msft8000
```

출력에 rhproxy가 시작 됨이 표시 되 면 rhproxy가 로드 되 고 성공적으로 시작 된 것입니다. 문제 코드가 표시 되 면 조사 해야 합니다. 몇 가지 일반적인 문제 코드는 다음과 같습니다.

- 문제 51- `CM_PROB_WAITING_ON_DEPENDENCY` -종속성 중 하나를 로드 하지 못했으므로 시스템에서 rhproxy를 시작 하지 않습니다. 즉, rhproxy에 전달 된 리소스가 잘못 된 ACPI 노드를 가리키거나 대상 장치가 시작 되지 않습니다. 먼저 모든 장치가 성공적으로 실행 되 고 있는지 확인 합니다 (위의 ' 컨트롤러 드라이버 확인 ' 참조). 그런 다음 ASL을 다시 확인 하 고 모든 리소스 경로 (예: `\_SB.I2C1` )가 올바른지 확인 하 고 DSDT의 올바른 노드를 가리킵니다.
- 문제 10- `CM_PROB_FAILED_START` -Rhproxy를 시작 하지 못했습니다. 리소스 구문 분석 문제가 발생 한 것 같습니다. ASL로 이동 하 여 DSD에서 리소스 인덱스를 두 번 확인 하 고, GPIO 리소스가 pin 번호의 오름차순으로 지정 되었는지 확인 합니다.

### <a name="verify-that-the-expected-devices-are-exposed-to-user-mode"></a>필요한 장치가 사용자 모드로 노출 되는지 확인 합니다.

이제 rhproxy가 실행 되 고 있으므로 사용자 모드로 액세스할 수 있는 장치 인터페이스를 만들어야 합니다. 여러 명령줄 도구를 사용 하 여 장치를 열거 하 고 있음을 확인 합니다.

리포지토리를 복제 [https://github.com/ms-iot/samples](https://github.com/ms-iot/samples) 하 고 `GpioTestTool` ,, `I2cTestTool` `SpiTestTool` 및 `Mincomm` 샘플을 빌드합니다. 테스트 중인 장치에 도구를 복사 하 고 다음 명령을 사용 하 여 장치를 열거 합니다.

```ps
I2cTestTool.exe -list
SpiTestTool.exe -list
GpioTestTool.exe -list
MinComm.exe -list
```

장치 및 나열 된 이름이 표시 됩니다. 올바른 장치 및 이름이 표시 되지 않으면 ASL을 다시 확인 합니다.

### <a name="verify-each-device-on-the-command-line"></a>명령줄에서 각 장치 확인

다음 단계는 명령줄 도구를 사용 하 여 장치를 열고 상호 작용 하는 것입니다.

I2CTestTool 예:

```ps
I2cTestTool.exe 0x55 I2C1
> write {1 2 3}
> read 3
> writeread {1 2 3} 3
```

SpiTestTool 예:

```ps
SpiTestTool.exe -n SPI1
> write {1 2 3}
> read 3
```

GpioTestTool 예제:

```ps
GpioTestTool.exe 12
> setdrivemode output
> write 0
> write 1
> setdrivemode input
> read
> interrupt on
> interrupt off
```

MinComm (직렬) 예제입니다. 실행 하기 전에 Rx를 Tx에 연결 합니다.

```ps
MinComm "\\?\ACPI#FSCL0007#3#{86e0d1e0-8089-11d0-9ce4-08003e301f73}\0000000000000006"
(type characters and see them echoed back)
```

### <a name="verify-each-device-from-a-uwp-app"></a>UWP 앱에서 각 장치 확인

다음 샘플을 사용 하 여 장치가 UWP에서 작동 하는지 확인 합니다.

- [IoT-GPIO](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-GPIO)
- [IoT-I2C](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-I2C)
- [IoT-SPI](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-SPI)
- [CustomSerialDeviceAccess](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomSerialDeviceAccess)

### <a name="run-the-hlk-tests"></a>HLK 테스트 실행

[하드웨어 랩 키트 (HLK)](/windows-hardware/test/hlk/windows-hardware-lab-kit)를 다운로드 합니다. 사용할 수 테스트는 다음과 같습니다.

- [GPIO WinRT 기능 및 스트레스 테스트](/windows-hardware/test/hlk/testref/f1fc0922-1186-48bd-bfcd-c7385a2f6f96)
- [I2C WinRT 쓰기 테스트 (EEPROM 필수)](/windows-hardware/test/hlk/testref/2ab0df1b-3369-4aaf-a4d5-d157cb7bf578)
- [I2C WinRT 읽기 테스트 (EEPROM 필수)](/windows-hardware/test/hlk/testref/ca91c2d2-4615-4a1b-928e-587ab2b69b04)
- [I2C WinRT 존재 하지 않는 슬레이브 주소 테스트](/windows-hardware/test/hlk/testref/2746ad72-fe5c-4412-8231-f7ed53d95e71)
- [I2C WinRT 고급 기능 테스트 (mbed LPC1768 필요)](/windows-hardware/test/hlk/testref/a60f5a94-12b2-4905-8416-e9774f539f1d)
- [SPI WinRT 클록 빈도 확인 테스트 (mbed LPC1768 필요)](/windows-hardware/test/hlk/testref/50cf9ccc-bbd3-4514-979f-b0499cb18ed8)
- [SPI WinRT IO 전송 테스트 (mbed LPC1768 필요)](/windows-hardware/test/hlk/testref/00c892e8-c226-4c71-9c2a-68349fed7113)
- [SPI WinRT Stride 검증 테스트](/windows-hardware/test/hlk/testref/20c6b079-62f7-4067-953f-e252bd271938)
- [SPI WinRT 전송 Gap 검색 테스트 (mbed LPC1768 필요)](/windows-hardware/test/hlk/testref/6da79d04-940b-4c49-8f00-333bf0cfbb19)

HLK manager에서 rhproxy device 노드를 선택 하면 해당 하는 테스트가 자동으로 선택 됩니다.

HLK manager에서 "리소스 허브 프록시 장치"를 선택 합니다.

![리소스 허브 프록시 장치 옵션이 선택 된 선택 탭을 보여 주는 Windows 하드웨어 랩 키트의 스크린샷](images/usermode-hlk-1.png)

그런 다음 테스트 탭을 클릭 하 고 I2C WinRT, Gpio WinRT 및 Spi WinRT 테스트를 선택 합니다.

![G P i/o Win R T 기능 및 스트레스 테스트 옵션을 선택 하 여 테스트 탭을 보여 주는 Windows 하드웨어 랩 키트의 스크린샷](images/usermode-hlk-2.png)

선택한 실행을 클릭 합니다. 테스트를 마우스 오른쪽 단추로 클릭 하 고 "테스트 설명"을 클릭 하 여 각 테스트에 대 한 추가 설명서를 사용할 수 있습니다.

## <a name="resources"></a>리소스

- [ACPI 5.0 사양](http://acpi.info/spec.htm)
- [Asl.exe (Microsoft ASL 컴파일러)](/windows-hardware/drivers/bringup/microsoft-asl-compiler)
- [Windows. Gpio](/uwp/api/Windows.Devices.Gpio)
- [Windows. Devices](/uwp/api/Windows.Devices.I2c)
- [Windows. Spi](/uwp/api/Windows.Devices.Spi)
- [SerialCommunication](/uwp/api/Windows.Devices.SerialCommunication)
- [테스트 작성 및 실행 프레임 워크 (TAEF)](/windows-hardware/drivers/taef/)
- [SpbCx](https://msdn.microsoft.com/library/windows/hardware/hh450906.aspx)
- [GpioClx](https://msdn.microsoft.com/library/windows/hardware/hh439508.aspx)
- [SerCx](/previous-versions//ff546939(v=vs.85))
- [MITT I2C 테스트](/windows-hardware/drivers/spb/run-mitt-tests-for-an-i2c-controller-)
- [GpioTestTool](https://github.com/microsoft/Windows-iotcore-samples/tree/6e473075bbe616e4d9ce90e67c6412fba661c337/BusTools/GpioTestTool)
- [I2cTestTool](https://github.com/microsoft/Windows-iotcore-samples/tree/6e473075bbe616e4d9ce90e67c6412fba661c337/BusTools/I2cTestTool)
- [SpiTestTool](https://github.com/microsoft/Windows-iotcore-samples/tree/6e473075bbe616e4d9ce90e67c6412fba661c337/BusTools/SpiTestTool)
- [MinComm (직렬)](https://github.com/microsoft/Windows-iotcore-samples/tree/6e473075bbe616e4d9ce90e67c6412fba661c337/BusTools/MinComm)
- [HLK(Hardware Lab Kit)](/windows-hardware/drivers/)

## <a name="appendix"></a>부록

### <a name="appendix-a---raspberry-pi-asl-listing"></a>부록 A-Raspberry Pi ASL 목록

참고 항목 [Raspberry Pi 2 & 3 Pin 매핑](/windows/iot-core/learn-about-hardware/pinmappings/pinmappingsrpi)

```cpp
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{

    Scope (\_SB)
    {
        //
        // RHProxy Device Node to enable WinRT API
        //
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate()
            {
                // Index 0
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE0  - GPIO 8  - Pin 24
                    0,                     // Device selection (CE0)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 1
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE1  - GPIO 7  - Pin 26
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 2
                SPISerialBus(              // SCKL - GPIO 21 - Pin 40
                                           // MOSI - GPIO 20 - Pin 38
                                           // MISO - GPIO 19 - Pin 35
                                           // CE1  - GPIO 17 - Pin 11
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data
                // Index 3
                I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1)
                    0xFFFF,                // SlaveAddress: placeholder
                    ,                      // SlaveMode: default to ControllerInitiated
                    0,                     // ConnectionSpeed: placeholder
                    ,                      // Addressing Mode: placeholder
                    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name
                    ,
                    ,
                    )                      // VendorData

                // Index 4 - GPIO 4 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 4 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 4 }
                // Index 6 - GPIO 5 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 5 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 5 }
                // Index 8 - GPIO 6 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 6 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 6 }
                // Index 10 - GPIO 12 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 12 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 12 }
                // Index 12 - GPIO 13 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 13 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 13 }
                // Index 14 - GPIO 16 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 16 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 16 }
                // Index 16 - GPIO 18 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 18 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 18 }
                // Index 18 - GPIO 22 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 22 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 22 }
                // Index 20 - GPIO 23 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 23 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 23 }
                // Index 22 - GPIO 24 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 24 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 24 }
                // Index 24 - GPIO 25 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 25 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 25 }
                // Index 26 - GPIO 26 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 26 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 26 }
                // Index 28 - GPIO 27 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 27 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 27 }
                // Index 30 - GPIO 35 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 35 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 35 }
                // Index 32 - GPIO 47 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 47 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 47 }
            })

            Name(_DSD, Package()
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package()
                {
                    // Reference http://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md
                    // SPI 0
                    Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }},                       // Index 0 & 1
                    Package(2) { "SPI0-MinClockInHz", 7629 },                               // 7629 Hz
                    Package(2) { "SPI0-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // SPI 1
                    Package(2) { "bus-SPI-SPI1", Package() { 2 }},                          // Index 2
                    Package(2) { "SPI1-MinClockInHz", 30518 },                              // 30518 Hz
                    Package(2) { "SPI1-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI1-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // I2C1
                    Package(2) { "bus-I2C-I2C1", Package() { 3 }},
                    // GPIO Pin Count and supported drive modes
                    Package (2) { "GPIO-PinCount", 54 },
                    Package (2) { "GPIO-UseDescriptorPinNumbers", 1 },
                    Package (2) { "GPIO-SupportedDriveModes", 0xf },                        // InputHighImpedance, InputPullUp, InputPullDown, OutputCmos
                }
            })
        }
    }
}

```

### <a name="appendix-b---minnowboardmax-asl-listing"></a>부록 B-MinnowBoardMax ASL 목록

[MinnowBoard Max Pin 매핑](/windows/iot-core/learn-about-hardware/pinmappings/pinmappingsmbm) 도 참조 하세요.

```cpp
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate()
            {
                // Index 0
                SPISerialBus(            // Pin 5, 7, 9 , 11 of JP1 for SIO_SPI
                    1,                     // Device selection
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    8,                     // databit len
                    ControllerInitiated,   // slave mode
                    8000000,               // Connection speed
                    ClockPolarityLow,      // Clock polarity
                    ClockPhaseSecond,      // clock phase
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                    ResourceConsumer,      // Resource usage
                    JSPI,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      // Vendor Data

                // Index 1
                I2CSerialBus(            // Pin 13, 15 of JP1, for SIO_I2C5 (signal)
                    0xFF,                  // SlaveAddress: bus address
                    ,                      // SlaveMode: default to ControllerInitiated
                    400000,                // ConnectionSpeed: in Hz
                    ,                      // Addressing Mode: default to 7 bit
                    "\\_SB.I2C6",          // ResourceSource: I2C bus controller name (For MinnowBoard Max, hardware I2C5(0-based) is reported as ACPI I2C6(1-based))
                    ,
                    ,
                    JI2C,                  // Descriptor Name: creates name for offset of resource descriptor
                    )                      // VendorData

                // Index 2
                UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    ,                      // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT2",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR2,                  // DescriptorName: creates name for offset of resource descriptor
                    )

                // Index 3
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {0}  // Pin 21 of JP1 (GPIO_S5[00])
                // Index 4
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {0}

                // Index 5
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {1}  // Pin 23 of JP1 (GPIO_S5[01])
                // Index 6
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {1}

                // Index 7
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {2}  // Pin 25 of JP1 (GPIO_S5[02])
                // Index 8
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {2}

                // Index 9
                UARTSerialBus(           // Pin 6, 8, 10, 12 of JP1, for SIO_UART1
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    FlowControlHardware,   // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT1",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR1,              // DescriptorName: creates name for offset of resource descriptor
                    )

                // Index 10
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {62}  // Pin 14 of JP1 (GPIO_SC[62])
                // Index 11
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {62}

                // Index 12
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {63}  // Pin 16 of JP1 (GPIO_SC[63])
                // Index 13
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {63}

                // Index 14
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {65}  // Pin 18 of JP1 (GPIO_SC[65])
                // Index 15
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {65}

                // Index 16
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {64}  // Pin 20 of JP1 (GPIO_SC[64])
                // Index 17
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {64}

                // Index 18
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {94}  // Pin 22 of JP1 (GPIO_SC[94])
                // Index 19
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {94}

                // Index 20
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {95}  // Pin 24 of JP1 (GPIO_SC[95])
                // Index 21
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {95}

                // Index 22
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {54}  // Pin 26 of JP1 (GPIO_SC[54])
                // Index 23
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {54}
            })

            Name(_DSD, Package()
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package()
                {
                    // SPI Mapping
                    Package(2) { "bus-SPI-SPI0", Package() { 0 }},

                    Package(2) { "SPI0-MinClockInHz", 100000 },
                    Package(2) { "SPI0-MaxClockInHz", 15000000 },
                    // SupportedDataBitLengths takes a list of support data bit length
                    // Example : Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8, 7, 16 }},
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32 }},
                     // I2C Mapping
                    Package(2) { "bus-I2C-I2C5", Package() { 1 }},
                    // UART Mapping
                    Package(2) { "bus-UART-UART2", Package() { 2 }},
                    Package(2) { "bus-UART-UART1", Package() { 9 }},
                }
            })
        }
    }
}
```

### <a name="appendix-c---sample-powershell-script-to-generate-gpio-resources"></a>부록 C-GPIO 리소스를 생성 하는 샘플 Powershell 스크립트

다음 스크립트를 사용 하 여 Raspberry Pi에 대 한 GPIO 리소스 선언을 생성할 수 있습니다.

```ps
$pins = @(
    @{PinNumber=4;PullConfig='PullUp'},
    @{PinNumber=5;PullConfig='PullUp'},
    @{PinNumber=6;PullConfig='PullUp'},
    @{PinNumber=12;PullConfig='PullDown'},
    @{PinNumber=13;PullConfig='PullDown'},
    @{PinNumber=16;PullConfig='PullDown'},
    @{PinNumber=18;PullConfig='PullDown'},
    @{PinNumber=22;PullConfig='PullDown'},
    @{PinNumber=23;PullConfig='PullDown'},
    @{PinNumber=24;PullConfig='PullDown'},
    @{PinNumber=25;PullConfig='PullDown'},
    @{PinNumber=26;PullConfig='PullDown'},
    @{PinNumber=27;PullConfig='PullDown'},
    @{PinNumber=35;PullConfig='PullUp'},
    @{PinNumber=47;PullConfig='PullUp'})

# generate the resources
$FIRST_RESOURCE_INDEX = 4
$resourceIndex = $FIRST_RESOURCE_INDEX
$pins | % {
    $a = @"
// Index $resourceIndex - GPIO $($_.PinNumber) - $($_.Name)
GpioIO(Shared, $($_.PullConfig), , , , "\\_SB.GPI0", , , , ) { $($_.PinNumber) }
GpioInt(Edge, ActiveBoth, Shared, $($_.PullConfig), 0, "\\_SB.GPI0",) { $($_.PinNumber) }
"@
    Write-Host $a
    $resourceIndex += 2;
}
```