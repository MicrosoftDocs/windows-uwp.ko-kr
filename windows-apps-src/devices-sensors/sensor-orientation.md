---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: 센서 방향
description: 가 속도계, 회전 계, 나침반, 경사 계 및 OrientationSensor 클래스의 센서 데이터는 해당 참조 축에 의해 정의 됩니다. 이러한 축은 장치의 가로 방향으로 정의 되 고 사용자가 장치를 전환할 때 장치를 사용 하 여 회전 합니다.
ms.date: 05/24/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4659aaba330d3b41451e91e450ff601e3fcf5407
ms.sourcegitcommit: 42a2d9e47f682ba42d91fed587f4d5924bde9c9a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85840767"
---
# <a name="sensor-orientation"></a>센서 방향

[**가 속도계**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer), [**회전 계**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer), [**나침반**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Compass), [**경사 계**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer)및 [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) 클래스의 센서 데이터는 해당 참조 축에 의해 정의 됩니다. 이러한 축은 장치의 참조 프레임에 의해 정의 되 고 사용자가 장치를 전환할 때 장치를 사용 하 여 회전 합니다. 사용자가 장치를 회전할 때 앱에서 자동 회전을 지원 하 고 장치를 수용 하기 위해 자동 회전을 지 원하는 경우에는 사용 하기 전에 reorients에 대 한 센서 데이터를 조정 해야 합니다.

### <a name="important-apis"></a>중요 API

- [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
- [**Windows. Devices. 사용자 지정**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>방향 및 장치 방향 표시

센서에 대 한 참조 축을 이해 하려면 장치 방향에서 표시 방향을 구분 해야 합니다. 표시 방향은 방향 텍스트와 이미지가 화면에 표시 되는 반면 장치 방향은 장치를 물리적으로 배치 하는 것입니다.

다음 다이어그램에서 장치 및 표시 방향은 [가로](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) (표시 된 센서 축이 가로 방향에 따라 지정 됨) 이며, z 축이 장치에서 확장 됩니다.

이 다이어그램은 표시 및 장치 방향을 [가로로](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)표시 합니다.

:::image type="content" source="images/sensor-orientation-a-small.jpg" alt-text="표시 및 장치 방향 (가로)":::

다음 다이어그램에서는 [LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)의 표시 및 장치 방향을 모두 보여 줍니다.

![LandscapeFlipped의 디스플레이 및 장치 방향](images/sensor-orientation-b-small.jpg)

이 최종 다이어그램은 장치 방향이 [LandscapeFlipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)는 동안 표시 방향을 가로로 보여 줍니다.

![장치 방향이 LandscapeFlipped 된 상태에서 가로 방향 표시](images/sensor-orientation-c-small.jpg)

[**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) 메서드를 [**currentorientation**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.currentorientation) 속성과 함께 사용 하 여 [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) 클래스를 통해 방향 값을 쿼리할 수 있습니다. 그런 다음 [**Displayorientations**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) 열거와 비교 하 여 논리를 만들 수 있습니다. 지원 되는 모든 방향에 대해 참조 축에서 해당 방향으로의 변환을 지원 해야 합니다.

## <a name="landscape-first-vs-portrait-first-devices"></a>가로 및 세로-첫 번째 장치

제조업체는 가로 및 세로-첫 번째 장치를 모두 생성 합니다. 참조 프레임은 가로-첫 번째 장치 (예: 데스크톱 및 랩톱)와 세로 중심 장치 (예: 휴대폰 및 일부 태블릿) 사이에서 다릅니다. 다음 표에서는 가로 및 세로-첫 번째 장치에 대 한 센서 축을 보여 줍니다.

| 방향 | 가로-우선 | 세로-우선 |
|-------------|-----------------|----------------|
| **가로** | ![가로 방향-첫 번째 장치](images/sensor-orientation-0-small.jpg) | ![세로 방향으로 가로-첫 번째 장치](images/sensor-orientation-1-small.jpg) |
| **세로** | ![가로-세로 방향의 첫 번째 장치](images/sensor-orientation-2-small.jpg) | ![세로 방향 세로 방향 기본 장치](images/sensor-orientation-3-small.jpg) |
| **LandscapeFlipped** | ![LandscapeFlipped 방향의 가로-첫 번째 장치](images/sensor-orientation-4-small.jpg) | ![LandscapeFlipped 방향 세로-첫 번째 장치](images/sensor-orientation-5-small.jpg) | 
| **PortraitFlipped** | ![PortraitFlipped 방향의 가로-첫 번째 장치](images/sensor-orientation-6-small.jpg)| ![PortraitFlipped 방향 세로-첫 번째 장치](images/sensor-orientation-7-small.jpg) |

## <a name="devices-broadcasting-display-and-headless-devices"></a>디스플레이 및 헤드리스 장치 브로드캐스트 장치

일부 장치에서는 다른 장치에 디스플레이를 브로드캐스트할 수 있습니다. 예를 들어 태블릿을 사용 하 여 가로 방향으로 프로젝터를 브로드캐스트할 수 있습니다. 이 시나리오에서는 장치 방향이 표시를 제시 하는 장치가 아니라 원래 장치를 기반으로 한다는 점을 명심 해야 합니다. 따라서가 속도계는 태블릿에서 데이터를 보고 합니다.

또한 일부 장치에는 표시 되지 않습니다. 이러한 장치의 기본 방향은 세로입니다.

## <a name="display-orientation-and-compass-heading"></a>방향 및 나침반 제목 표시

나침반 제목은 참조 축에 따라 달라 지 며 장치 방향에 따라 변경 됩니다. 이 테이블을 기반으로 보정 합니다 (사용자가 북부를 향한 것으로 가정).

| 사용 됩니다. | 나침반 제목에 대 한 참조 축 | 북쪽 (가로-위쪽)을 향한 경우의 API 나침반 제목 | 북쪽 북부 (세로-우선)를 향한 경우의 API 나침반 제목 |나침반 제목 보정 (가로-세로) | 나침반 제목 보정 (세로 우선) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| 가로           | -Z | 0   | 270 | 제목               | (제목 + 90) %360  |
| 세로            |  지원 | 90  | 0   | (제목 + 270) %360 |  제목              |
| LandscapeFlipped    |  Z | 180 | 90  | (제목 + 180) %360 | (제목 + 270) %360 |
| PortraitFlipped     |  지원 | 270 | 180 | (제목 + 90) %360  | (제목 + 180) %360 |

머리글을 올바르게 표시 하기 위해 표에 표시 된 것 처럼 나침반 제목을 수정 합니다. 다음 코드 조각에서는이 작업을 수행 하는 방법을 보여 줍니다.

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>가 속도계 및 회전 계를 사용 하 여 방향 표시

이 테이블은가 속도계 및 회전 계 데이터를 표시 방향으로 변환 합니다.

| 참조 축        |  X |  Y | Z |
|-----------------------|----|----|---|
| **가로**         |  X |  Y | Z |
| **세로**          |  지원 | -X | Z |
| **LandscapeFlipped**  | -X | -y | Z |
| **PortraitFlipped**   | -y |  X | Z |

다음 코드 예제에서는 이러한 변환을 회전 계에 적용 합니다.

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>방향 및 장치 방향 표시

[**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) 데이터는 다른 방식으로 변경 해야 합니다. 이러한 여러 방향을 Z 축에서 시계 반대 방향으로 회전 하는 것으로 간주 하므로 사용자의 방향을 다시 가져오려면 회전을 반대로 해야 합니다. 4 원수 데이터의 경우 오일러의 수식을 사용 하 여 참조 4 원수로 회전을 정의할 수 있으며 참조 회전 행렬을 사용할 수도 있습니다.

![오일러의 수식](images/eulers-formula.png)

원하는 상대적인 방향을 얻으려면 참조 개체와 절대 개체를 곱합니다. 이 수치는 비가 환이 지 않습니다.

![절대 개체와 참조 개체를 곱합니다.](images/orientation-formula.png)

위의 식에서 절대 개체는 센서 데이터에서 반환 됩니다.

| 사용 됩니다.  | Z 중심으로 시계 반대 방향 회전 | 참조 4 원수 (역방향 회전) | 참조 회전 행렬 (역방향 회전) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **가로**        | 0                                  | 1 + 0i + 0i + 성공적으로 처리                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **세로**         | 90                                 | cos (-45 ⁰) + (i + j + k) * sin (-45 ⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0-i-j-k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos (-135 ⁰) + (i + j + k) * sin (-135 ⁰)     | \[0 -1 0<br/> 1 0 0<br/> 0 0 1]             |

## <a name="see-also"></a>참조

[동작 및 방향 센서 통합](https://docs.microsoft.com/windows-hardware/design/whitepapers/integrating-motion-and-orientation-sensors)
