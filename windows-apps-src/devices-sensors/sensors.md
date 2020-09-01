---
ms.assetid: 415F4107-0612-4235-9722-0F5E4E26F957
title: 센서
description: 센서를 사용 하면 앱에서 장치와 실제 세계 간의 관계를 알 수 있습니다. 센서는 장치의 방향, 방향 및 움직임을 응용 프로그램에 알릴 수 있습니다.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c76893dd56c2c3d207444b5c6331c0cd4445ed1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159657"
---
# <a name="sensors"></a>센서



센서를 사용 하면 앱에서 장치와 실제 세계 간의 관계를 알 수 있습니다. 센서는 장치의 방향, 방향 및 움직임을 응용 프로그램에 알릴 수 있습니다. 이러한 센서를 사용 하면 장치의 동작을 사용 하 여 화면에서 문자를 정렬 하거나 환경에서 장치를 조종 하 고 장치를 조종으로 사용 하는 등의 고유한 입력 형식을 제공 하 여 게임, 보강 된 현실 앱 또는 유틸리티 앱을 보다 유용 하 고 대화형으로 만들 수 있습니다.

일반적으로 앱이 센서에 독점적으로 종속 되는지 아니면 센서가 추가 제어 메커니즘도 제공할지를 결정 합니다. 예를 들어 장치를 가상 조종 휠으로 사용 하는 운전 게임은 화상 GUI를 통해 제어 될 수 있습니다. 이러한 방식으로 앱은 시스템에서 사용할 수 있는 센서에 관계 없이 작동 합니다. 반면, 대리석으로 기울이는 미로는 적절 한 센서가 있는 시스템 에서만 작동 하도록 코딩 될 수 있습니다. 센서에 전적으로 의존 하는지 여부를 전략적으로 선택 해야 합니다. 마우스/터치 컨트롤 구성표는 더 많은 제어를 위해 집중 교육를 거래 합니다.

| 항목                                                       | Description  |
|-------------------------------------------------------------|--------------|
| [센서 보정](calibrate-sensors.md)                   | 나침반, 경사계 방향 센서 같이 자력계를 기반으로 한 디바이스의 센서는 환경 요인으로 인해 보정이 필요해질 수 있습니다. [<strong>MagnetometerAccuracy</strong>](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 열거는 장치가 보정을 필요로 할 때 작업 과정을 결정 하는 데 도움이 될 수 있습니다. |
| [센서 방향](sensor-orientation.md)                 | [<strong>OrientationSensor</strong>](/uwp/api/Windows.Devices.Sensors.OrientationSensor) 클래스의 센서 데이터는 해당 참조 축에 의해 정의 됩니다. 이러한 축은 장치의 가로 방향으로 정의 되 고 사용자가 장치를 전환할 때 장치를 사용 하 여 회전 합니다. |
| [가속도계 사용](use-the-accelerometer.md)           | 가 속도계를 사용 하 여 사용자 이동에 응답 하는 방법을 알아봅니다. |
| [나침반 사용](use-the-compass.md)                       | 나침반을 사용 하 여 현재 머리글을 확인 하는 방법을 알아봅니다. |
| [회전계 사용](use-the-gyrometer.md)                   | 회전 계를 사용 하 여 사용자 이동의 변경 내용을 검색 하는 방법을 알아봅니다. | 
| [경사계 사용](use-the-inclinometer.md)             | 경사 계를 사용 하 여 피치, 롤 및 요를 확인 하는 방법을 알아봅니다. |
| [광원 센서 사용](use-the-light-sensor.md)             | 주변 광원 센서를 사용 하 여 조명 변화를 검색 하는 방법을 알아봅니다. |
| [방향 센서 사용](use-the-orientation-sensor.md) | 방향 센서를 사용 하 여 장치 방향을 결정 하는 방법에 대해 알아봅니다.|

## <a name="sensor-batching"></a>센서 일괄 처리

일부 센서는 일괄 처리 개념을 지원 합니다. 이는 사용 가능한 개별 센서에 따라 달라 집니다. 센서가 일괄 처리를 구현 하는 경우 지정 된 시간 간격 동안 여러 데이터 요소를 수집 하 고 해당 데이터를 모두 한 번에 전송 합니다. 이는 센서에서 읽기를 수행 하는 즉시 해당 결과를 보고 하는 일반적인 동작과는 다릅니다. 데이터를 수집 하 고 배달 하는 방법을 먼저 정상적인 배달 및 일괄 처리로 배달 하는 방법을 보여 주는 다음 다이어그램을 살펴보십시오.

![센서 일괄 처리 컬렉션](images/batchsample.png)

센서 일괄 처리에 대 한 주요 이점은 배터리 수명 prolonging. 데이터를 즉시 전송 하지 않을 경우이는 프로세서 전원을 절약 하 고 데이터를 즉시 처리할 필요가 없도록 합니다. 시스템의 일부는 필요할 때까지 절전 모드에서 절전 모드를 만들 수 있습니다.

대기 시간을 조정 하 여 센서가 일괄 처리를 전송 하는 빈도에 영향을 줄 수 있습니다. 예를 들어 [**가 속도계**](/uwp/api/Windows.Devices.Sensors.Accelerometer) 센서에는 [**reportlatency**](/uwp/api/windows.devices.sensors.accelerometer.reportlatency) 속성이 있습니다. 응용 프로그램에 대해이 속성이 설정 되 면 센서는 지정 된 시간 후에 데이터를 전송 합니다. [**Reportinterval**](/uwp/api/windows.devices.sensors.accelerometer.reportinterval) 속성을 설정 하 여 지정 된 대기 시간 동안 누적 되는 데이터의 양을 제어할 수 있습니다.

대기 시간 설정과 관련 하 여 주의 해야 할 몇 가지 주의 사항이 있습니다. 첫 번째 주의 사항은 각 센서가 센서 자체를 기반으로 지원할 수 있는 [**Maxbatchsize**](/uwp/api/windows.devices.sensors.accelerometer.maxbatchsize) 를 포함 한다는 것입니다. 센서가 강제로 보내기 전에 캐시할 수 있는 이벤트의 수입니다. **Maxbatchsize** 를 [**reportinterval**](/uwp/api/windows.devices.sensors.accelerometer.reportinterval)과 곱하면 최대 [**reportinterval**](/uwp/api/windows.devices.sensors.accelerometer.reportlatency) 값이 결정 됩니다. 이보다 큰 값을 지정 하면 데이터가 손실 되지 않도록 최대 대기 시간이 사용 됩니다. 또한 여러 응용 프로그램이 각각 원하는 대기 시간을 설정할 수 있습니다. 모든 응용 프로그램의 요구 사항을 충족 하기 위해 가장 짧은 대기 시간이 사용 됩니다. 이러한 팩트 때문에 응용 프로그램에서 설정한 대기 시간이 관찰 된 대기 시간과 일치 하지 않을 수 있습니다.

센서가 일괄 처리 보고를 사용 하는 경우 [**GetCurrentReading**](/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) 를 호출 하면 현재 데이터 일괄 처리를 지우고 새 대기 시간을 시작 합니다.

## <a name="accelerometer"></a>가속도계

[**가 속도계**](/uwp/api/Windows.Devices.Sensors.Accelerometer) 센서는 장치의 X, Y 및 Z 축을 따라 G-force 값을 측정 하 고 간단한 동작 기반 응용 프로그램에 적합 합니다. G-force 값은 중력으로 인 한 가속을 포함 합니다. 장치에 테이블에 **FaceUp** 의 [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) 가 있는 경우가 속도계는 Z 축에서-1 G를 읽습니다. 따라서 accelerometers는 속도 변경 속도와 같이 좌표 가속을 측정 하지 않을 수 있습니다. 가 속도계를 사용 하는 경우 중력의 gravitational 벡터와 이동의 선형 가속 벡터를 구분 해야 합니다. 고정 된 장치에 대해 gravitational 벡터가 1로 정규화 되어야 합니다.

다음 다이어그램은 다음을 보여 줍니다.

-   V1 = Vector 1 = 중력으로 인 한 강제 적용
-   V2 = 벡터 2 =-장치 섀시의 Z 축 (화면 뒤쪽에서 가리키기)
-   Θi = 기울기 각도 (기울기) = 장치 섀시의 Z 축과 무게 벡터의 각도

![가속도계](images/accelerometer1.png)![가 속도계 측정](images/accelerometer2.png)

가 속도계 센서를 사용할 수 있는 앱에는 화면의 대리석이 장치를 기울이는 방향으로 롤업되는 게임 (gravitational vector)이 포함 됩니다. 이 유형의 기능은 [**경사 계**](/uwp/api/Windows.Devices.Sensors.Inclinometer) 를 긴밀 하 게 미러링 하며 피치와 롤의 조합을 사용 하 여 해당 센서에서 수행할 수도 있습니다. 가 속도계의 중력 벡터를 사용 하면 장치 기울기를 위해 쉽게 수학적으로 조작 된 벡터를 제공 하 여 이러한 정도를 간소화할 있습니다. 또 다른 예로, 사용자가 공기 (선형 가속 벡터)를 통해 장치를 긋기 하는 경우에는 뚝딱의 카드를 사용 하는 앱이 있습니다.

구현에 대 한 예제는 [가 속도계 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer)을 참조 하세요.

## <a name="activity-sensor"></a>활동 센서

[**활동**](/uwp/api/Windows.Devices.Sensors.ActivitySensor) 센서는 센서에 연결 된 장치의 현재 상태를 확인 합니다. 이 센서는 장치를 운반 하는 사용자가 실행 중이거나 탐색 되는 경우를 추적 하기 위해 적합성 응용 프로그램에서 자주 사용 됩니다. 이 센서 API에서 검색할 수 있는 가능한 작업 목록은 [**ActivityType**](/uwp/api/Windows.Devices.Sensors.ActivityType) 을 참조 하세요.

구현 예제는 [활동 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ActivitySensor)을 참조 하세요.

## <a name="altimeter"></a>Altimeter

[**Altimeter**](/uwp/api/Windows.Devices.Sensors.Altimeter) 센서는 센서의 고도를 나타내는 값을 반환합니다. 이를 통해 해상 수준에서의 미터를 기준으로 하는 변화를 추적할 수 있습니다. 이를 사용 하는 앱의 한 가지 예는 실행 중인 calories를 계산 하기 위해 실행 중에 권한 상승 변경을 추적 하는 실행 중인 앱입니다. 이 경우이 센서 데이터를 [**활동**](/uwp/api/Windows.Devices.Sensors.ActivitySensor) 센서와 결합 하 여 보다 정확한 추적 정보를 제공할 수 있습니다.

구현에 대 한 예제는 [altimeter 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Altimeter)을 참조 하세요.

## <a name="barometer"></a>지표

[**Barometer**](/uwp/api/Windows.Devices.Sensors.Barometer) 센서를 사용 하면 응용 프로그램에서 바 ometric 판독값을 얻을 수 있습니다. 날씨 응용 프로그램은이 정보를 사용 하 여 현재 대기 압력을 제공할 수 있습니다. 이를 사용 하 여 자세한 정보를 제공 하 고 잠재적인 날씨 변경을 예측할 수 있습니다.

구현에 대 한 예제는 [barometer 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Barometer)을 참조 하세요.

## <a name="compass"></a>나침반

[**나침반**](/uwp/api/Windows.Devices.Sensors.Compass) 센서는 지구의 가로 평면을 기반으로 하는 자기 북쪽에 대해 2d 제목을 반환 합니다. 나침반 센서는 특정 장치 방향을 결정 하거나 3D 공간에서 모든 항목을 표시 하는 데 사용 하면 안 됩니다. 지리적 기능으로 인해 제목에서 자연 스러운 declination 발생할 수 있으므로 일부 시스템은 [**HeadingMagneticNorth**](/uwp/api/windows.devices.sensors.compassreading.headingmagneticnorth) 와 [**HeadingTrueNorth**](/uwp/api/windows.devices.sensors.compassreading.headingtruenorth)를 모두 지원 합니다. 앱에서 선호 하는 것을 고려 하지만 모든 시스템이 진정한 북쪽 값을 보고 하는 것은 아닙니다. 회전 계 및 지자기 센터 (자기 강도 크기를 측정 하는 장치) 센서는 데이터를 결합 하 여 데이터를 안정화 하는 데 큰 영향을 주는 나침반 머리글을 생성 합니다. 즉, 전기 시스템 구성 요소로 인해 자기 필드 강도가 매우 불안정 합니다.

![자기 북부와 관련 하 여 나침반 판독값](images/compass.png)

나침반 로즈를 표시 하거나 지도를 탐색 하려는 앱은 일반적으로 나침반 센서를 사용 합니다.

구현 예제는 [나침반 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass)을 참조 하세요.

## <a name="gyrometer"></a>Gyrometer

[**회전 계**](/uwp/api/Windows.Devices.Sensors.Gyrometer) 센서는 X, Y, Z 축을 따라 각도를 측정 합니다. 이러한 기능은 장치 방향에는 영향을 주지 않지만 다양 한 속도로 장치를 회전 하는 것을 고려 하는 간단한 동작 기반 앱에서 매우 유용 합니다. Gyrometers는 하나 이상의 축을 따라 데이터 또는 상수 바이어스가 떨어질 수 있습니다. 가 속도계를 쿼리하여 회전 계가 바이어스에서 인지 확인 하 고 앱에서 적절 하 게 보정 하는지 확인 하기 위해 장치가 이동 하는지 확인 해야 합니다.

![피치, 롤 및 요 있는 회전 계](images/gyrometer.png)

회전 계 센서를 사용할 수 있는 앱의 예로는 장치의 빠른 회전 jerk을 기반으로 roulette 휠을 회전 하는 게임이 있습니다.

구현에 대 한 예제는 [회전 계 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Gyrometer)을 참조 하세요.

## <a name="inclinometer"></a>Inclinometer

[**경사 계**](/uwp/api/Windows.Devices.Sensors.Inclinometer) 센서는 장치의 요, 피치 및 롤 값을 지정 하 고 장치가 공간에 배치 되는 방식을 고려 하는 앱에서 가장 잘 작동 합니다. 피치와 롤백은가 속도계의 중력 벡터를 가져오고 회전 계의 데이터를 통합 하 여 파생 됩니다. 요는 지자기 센터 및 회전 계 (나침반 제목) 데이터와 유사 하 게 설정 됩니다. Inclinometers는 쉬운 좋게 및 이해 하기 쉬운 방식으로 고급 지향 데이터를 제공 합니다. 장치 방향이 필요 하지만 센서 데이터를 조작 하지 않아도 되는 경우 inclinometers를 사용 합니다.

![피치, 롤 및 요 데이터를 사용한 경사 계](images/inclinometer.png)

장치 방향에 맞게 보기를 변경 하는 앱은 경사 계 센서를 사용할 수 있습니다. 또한 장치의 요, 피치 및 롤러와 일치 하는 비행기를 표시 하는 앱은 경사 계 판독값도 사용 합니다.

구현에 대 한 예제는 경사 계 샘플을 참조 [https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer) 하세요.

## <a name="light-sensor"></a>빛 센서

[**빛**](/uwp/api/Windows.Devices.Sensors.LightSensor) 센서는 센서 주변 주변 광원을 확인할 수 있습니다. 이를 통해 앱은 장치 주변의 광원 설정이 변경 된 시기를 확인할 수 있습니다. 예를 들어 슬레이트 장치를 사용 하는 사용자는 sunny 날에 실내에서 실외로 이동할 수 있습니다. 스마트 응용 프로그램은이 값을 사용 하 여 배경색과 렌더링 되는 글꼴 간의 대비를 높일 수 있습니다. 그러면 더 밝은 실외 설정에서 콘텐츠를 계속 읽을 수 있습니다.

구현 예제는 [light 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor)을 참조 하세요.

## <a name="orientation-sensor"></a>방향 센서

장치 방향은 4 원수와 회전 매트릭스를 모두 통해 표현 됩니다. [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) 는 절대 제목과 관련 하 여 장치에 있는 방법을 결정 하는 데 높은 수준의 정밀도를 제공 합니다. **OrientationSensor** 데이터는가 속도계, 회전 계 및 지자기 센터에서 파생 됩니다. 따라서 경사 계 및 나침반 센서는 모두 4 원수 값에서 파생 될 수 있습니다. 4 원수 및 회전 행렬은 고급 수학적 조작에 적합 하며 그래픽 프로그래밍에 자주 사용 됩니다. 복잡 한 조작을 사용 하는 앱은 4 원수 및 회전 매트릭스를 기반으로 하는 많은 변환의 방향 센서를 선호 해야 합니다.

![방향 센서 데이터](images/orientation-sensor.png)

방향 센서는 장치 뒷면의 방향에 따라 사용자의 환경에 오버레이를 그리는 고급 확대 현실 앱에서 자주 사용 됩니다.

구현 예제는 [방향 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)을 참조 하세요.

## <a name="pedometer"></a>Pedometer

[**Pedometer**](/uwp/api/Windows.Devices.Sensors.Pedometer) 센서는 연결 된 장치를 운반 하는 사용자가 수행 하는 단계 수를 추적 합니다. 센서는 지정 된 기간 동안의 단계 수를 추적 하도록 구성 되어 있습니다. 사용자를 설정 하 고 다양 한 목표를 달성 하기 위해 수행 하는 단계 수를 추적 하는 등의 다양 한 적합성 응용 프로그램입니다. 그런 다음이 정보를 수집 하 고 저장 하 여 시간에 따른 진행률을 표시할 수 있습니다.

예제 구현 [pedometer 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Pedometer)을 참조 하세요.

## <a name="proximity-sensor"></a>근접 센서

[**근접**](/uwp/api/Windows.Devices.Sensors.ProximitySensor) 센서를 사용 하 여 센서가 개체가 검색 되는지 여부를 나타낼 수 있습니다. 개체가 장치 범위 내에 있는지 여부를 확인 하는 것 외에도 근접 센서는 검색 된 개체에 대 한 거리를 확인할 수 있습니다. 이를 사용 하는 한 가지 예는 사용자가 지정 된 범위 내에서 들어올 때 절전 상태에서 발생 하려는 응용 프로그램을 사용 하는 것입니다. 근접 센서가 개체를 감지 하 고 더 많은 활성 상태를 시작할 수 있을 때까지 장치는 저전원 절전 상태일 수 있습니다.

구현 예제는 [근접 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ProximitySensor)을 참조 하세요.

## <a name="simple-orientation"></a>간단한 방향

[**SimpleOrientationSensor**](/uwp/api/windows.devices.sensors.simpleorientationsensor) 는 지정 된 장치에 대 한 현재 사분면 방향을 검색 합니다. 6 가지 가능한 [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) 상태 (**notrotated**, **Rotated90**, **Rotated180**, **Rotated270**, **FaceUp**, **FaceDown**)가 있습니다.

병렬 또는 접지를 중심으로 하는 장치에 따라 디스플레이를 변경 하는 판독기 앱은 SimpleOrientationSensor의 값을 사용 하 여 장치를 보유 하는 방법을 결정 합니다.

구현 예제는 [간단한 방향 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)을 참조 하세요.