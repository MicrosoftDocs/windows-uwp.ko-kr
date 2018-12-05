---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: 센서 방향
description: Accelerometer, Gyrometer, Compass, Inclinometer 및 OrientationSensor 클래스의 센서 데이터는 참조 축에 의해 정의됩니다. 이러한 축은 장치의 가로 방향에서 정의되고 사용자가 돌릴 때 장치와 함께 회전합니다.
ms.date: 05/24/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a4c7f1ad75e1e0544486049f9bd721d8a82edf03
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740224"
---
# <a name="sensor-orientation"></a>센서 방향


**중요 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

[**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687), [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718), [**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705), [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) 및 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 클래스의 센서 데이터는 참조 축에 의해 정의됩니다. 이러한 축은 디바이스의 참조 프레임에서 정의되고 사용자가 돌릴 때 디바이스와 함께 회전합니다. 앱이 자동 회전을 지원하고 사용자가 장치를 회전할 때 장치에 맞게 자동으로 방향이 조정되는 경우사용하기 전에 센서 데이터를 회전에 대해 조정해야 합니다.

## <a name="display-orientation-vs-device-orientation"></a>디스플레이 방향 및 장치 방향

센서의 참조 축을 이해하려면 디스플레이 방향과 장치 방향을 구분해야 합니다. 디스플레이 방향은 텍스트 및 이미지가 화면에 표시되는 방향이고, 디바이스 방향은 디바이스의 실제 위치입니다. 다음 그림에서는 디바이스 및 디스플레이 방향이 모두 **가로**입니다(표시된 센서 축이 가로 방향 우선 디바이스에만 적용됨).

![디스플레이 및 디바이스 방향(Landscape)](images/sensor-orientation-a.PNG)

다음 그림에서는 디스플레이 및 디바이스 방향이 둘 다 **LandscapeFlipped**입니다.

![디스플레이 및 디바이스 방향(LandscapeFlipped)](images/sensor-orientation-b.PNG)

다음 그림에서 디스플레이 방향은 Landscape이고 장치 방향은 LandscapeFlipped입니다.

![디스플레이 방향은 Landscape이고 장치 방향은 LandscapeFlipped임](images/sensor-orientation-c.PNG)

[**CurrentOrientation**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.currentorientation.aspx) 속성과 함께 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.getforcurrentview.aspx) 메서드를 사용하여 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Dn264258) 클래스를 통해 방향 값을 쿼리할 수 있습니다. 그런 다음 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/BR226142) 열거형과 비교하여 논리를 만들 수 있습니다. 지원하는 각 방향에 대해 참조 축을 해당 방향으로 변환할 수 있도록 지원해야 합니다.

## <a name="landscape-first-vs-portrait-first-devices"></a>가로 방향 우선 및 세로 방향 우선 장치

제조업체에서는 가로 방향 우선 디바이스와 세로 방향 우선 디바이스를 둘 다 생산하고 있습니다. 참조 프레임은 가로 방향 우선 디바이스(예: 데스크톱 및 랩톱)와 세로 방향 우선 디바이스(예: 휴대폰 및 일부 태블릿) 간에 다릅니다. 다음 표에서는 가로 방향 우선 디바이스와 세로 방향 우선 디바이스의 센서 축을 보여 줍니다.

| 방향 | 가로 방향 우선 | 세로 방향 우선 |
|-------------|-----------------|----------------|
| **가로** | ![Landscape 방향의 가로 방향 우선 디바이스](images/sensor-orientation-0.PNG) | ![Landscape 방향의 세로 방향 우선 디바이스](images/sensor-orientation-1.PNG) |
| **세로** | ![Portrait 방향의 가로 방향 우선 디바이스](images/sensor-orientation-2.PNG) | ![Portrait 방향의 세로 방향 우선 디바이스](images/sensor-orientation-3.PNG) |
| **LandscapeFlipped** | ![LandscapeFlipped 방향의 가로 방향 우선 디바이스](images/sensor-orientation-4.PNG) | ![LandscapeFlipped 방향의 세로 방향 우선 디바이스](images/sensor-orientation-5.PNG) | 
| **PortraitFlipped** | ![PortraitFlipped 방향의 가로 방향 우선 디바이스](images/sensor-orientation-6.PNG)| ![PortraitFlipped 방향의 세로 방향 우선 장치](images/sensor-orientation-7.PNG) |

## <a name="devices-broadcasting-display-and-headless-devices"></a>디스플레이 및 헤드리스 장치를 브로드캐스트하는 장치

일부 장치는 디스플레이를 다른 장치에 브로드캐스트할 수 있습니다. 예를 들어 태블릿을 가져와 가로 방향의 프로젝터에 디스플레이를 브로드캐스트할 수 있습니다. 이 시나리오에서는 장치 방향이 디스플레이를 프레젠테이션하는 장치가 아니라 원래 장치를 기반으로 합니다. 따라서 가속도계가 태블릿에 대한 데이터를 보고합니다.

또한 일부 장치에는 디스플레이가 없습니다. 이러한 장치를 사용할 경우 기본 방향은 세로입니다.

## <a name="display-orientation-and-compass-heading"></a>디스플레이 방향 및 나침반 제목


나침반 제목은 참조 축에 따라 달라지므로 장치 방향과 함께 변경됩니다. 다음 표에 따라 보완합니다(사용자가 북쪽을 향하고 있다고 가정).

| 디스플레이 방향 | 나침반 제목에 대한 참조 축 | 북쪽을 향하는 경우 API 나침반 제목(가로 방향 우선) | 북쪽을 향하는 경우 API 나침반 제목(세로 방향 우선) |나침반 제목 보완(가로 방향 우선) | 나침반 제목 보완(세로 방향 우선) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| 가로           | -Z | 0   | 270 | 제목               | (제목 + 90) % 360  |
| 세로            |  예 | 90  | 0   | (제목 + 270) % 360 |  제목              |
| LandscapeFlipped    |  Z | 180 | 90  | (제목 + 180) % 360 | (제목 + 270) % 360 |
| PortraitFlipped     |  예 | 270 | 180 | (제목 + 90) % 360  | (제목 + 180) % 360 |

제목을 제대로 표시하기 위해 표와 같이 나침반 제목을 수정합니다. 다음 코드 조각은 이 작업을 수행하는 방법을 보여 줍니다.

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

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>가속도계 및 회전계를 사용한 디스플레이 방향

다음 표에서는 디스플레이 방향에 대한 가속도계 및 회전계 데이터를 변환합니다.

| 참조 축        |  X |  예 | Z |
|-----------------------|----|----|---|
| **가로**         |  X |  예 | Z |
| **세로**          |  예 | -X | Z |
| **LandscapeFlipped**  | -X | -Y | Z |
| **PortraitFlipped**   | -Y |  X | Z |

다음은 이러한 변환을 회전계에 적용하는 코드 예제입니다.

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

## <a name="display-orientation-and-device-orientation"></a>디스플레이 방향 및 장치 방향

[**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 데이터는 다른 방법으로 변경해야 합니다. 다른 방향을 Z축에 대한 시계 반대 방향 회전으로 간주하므로 사용자 방향을 다시 가져오기 위해 회전을 반대로 해야 합니다. 사원수 데이터의 경우 오일러의 공식을 사용하여 참조 사원수로 회전을 정의할 수 있으며, 참조 회전 행렬을 사용할 수도 있습니다.

![오일러의 공식](images/eulers-formula.png)

원하는 상대 방향을 가져오려면 절대 개체에 참조 개체를 곱합니다. 이 수학은 가환성이 아닙니다.

![절대 개체에 참조 개체 곱하기](images/orientation-formula.png)

앞의 식에서 절대 개체는 센서 데이터에서 반환됩니다.


| 디스플레이 방향  | Z축을 중심으로 시계 반대 방향 회전 | 참조 사원수(역회전) | 참조 회전 행렬(역회전) | 
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **가로**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **세로**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1 0 0<br/> 0 0 1]             |

