---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: 센서 보정
description: 나침반, 경사계 방향 센서 같이 자력계를 기반으로 한 디바이스의 센서는 환경 요인으로 인해 보정이 필요해질 수 있습니다.
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0ef2c1cfbe949e5f850a494e4f1ed9c79faf6050
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168577"
---
# <a name="calibrate-sensors"></a>센서 보정


**중요 API**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**Windows. Devices. 사용자 지정**](/uwp/api/Windows.Devices.Sensors.Custom)

나침반, 경사계 방향 센서 같이 자력계를 기반으로 한 디바이스의 센서는 환경 요인으로 인해 보정이 필요해질 수 있습니다. [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 열거는 장치가 보정을 필요로 할 때 작업 과정을 결정 하는 데 도움이 될 수 있습니다.

## <a name="when-to-calibrate-the-magnetometer"></a>지자기 센터를 보정 하는 경우

[**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 열거형에는 앱이 실행 되는 장치를 보정할 필요가 있는지 여부를 결정 하는 데 도움이 되는 4 가지 값이 있습니다. 장치를 보정 해야 하는 경우 사용자에 게 보정 필요를 알려 주는 것이 좋습니다. 그러나 사용자에 게 너무 자주 보정할 메시지를 표시 해서는 안 됩니다. 10 분 마다 한 번만 권장 됩니다.

| 값           | Description    |
| ----------------- | ------------------- |
| **알 수 없음**     | 센서 드라이버가 현재 정확도를 보고 하지 못했습니다. 이는 반드시 장치가 보정 되지 않았음을 의미 하는 것은 아닙니다. **알 수 없음** 이 반환 되는 경우 가장 적합 한 작업을 결정 하는 것은 앱의 작업입니다. 앱이 정확한 센서 읽기에 종속 되는 경우 사용자에 게 장치를 보정할 지 묻는 메시지를 표시 하는 것이 좋습니다. |
| **대개**  | 현재 지자기 센터에는 높은 수준의 부정확성이 있습니다. 앱은이 값이 처음 반환 될 때 사용자의 보정을 항상 요청 해야 합니다. |
| **근사치** | 데이터는 일부 응용 프로그램에 대해 정확 하 게 정확 합니다. 사용자가 장치를 위로/아래로 이동 하거나 왼쪽/오른쪽으로 이동 하는 경우에만 알아야 하는 가상 현실 앱은 보정 없이 계속 진행할 수 있습니다. 사용자에 게 지침을 제공 하기 위해 주행 방향을 알아야 하는 탐색 앱 처럼 절대 제목이 필요한 앱은 보정을 요청 해야 합니다. |
| **높음**        | 데이터는 정확 합니다. 확대 된 현실 또는 탐색 앱과 같은 절대 제목을 알고 있어야 하는 앱의 경우에도 보정이 필요 하지 않습니다. |