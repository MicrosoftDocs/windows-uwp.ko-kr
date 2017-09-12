---
author: mukin
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: "디바이스, 센서 및 전원"
description: "사용자에게 풍부한 환경을 제공하기 위해 외부 디바이스나 센서를 앱에 통합해야 하는 경우가 있습니다."
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a15ed83ccb7571b772ce77669646e58894e71842
ms.sourcegitcommit: a2908889b3566882c7494dc81fa9ece7d1d19580
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/31/2017
---
# <a name="devices-sensors-and-power"></a>장치, 센서 및 전원

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

사용자에게 풍부한 환경을 제공하기 위해 외부 디바이스나 센서를 앱에 통합해야 하는 경우가 있습니다. 이 섹션에 설명된 기술을 사용하여 앱에 추가할 수 있는 기능의 몇 가지 예는 다음과 같습니다.

-   향상된 인쇄 환경 제공
-   게임에 동작 및 방향 센서 통합
-   직접 또는 네트워크 프로토콜을 통해 디바이스에 연결

| 항목 | 설명 |
|-------|-------------|
| [디바이스 기능 사용](enable-device-capabilities.md) | 이 자습서에서는 Microsoft Visual Studio에서 디바이스 접근 권한 값을 선언하는 방법을 설명합니다. 이를 통해 앱에서 카메라, 마이크, 위치 센서 및 기타 디바이스를 사용할 수 있습니다. | 
| [Windows IoT용 사용자 모드 액세스를 사용하도록 설정](enable-usermode-access.md) | 이 자습서에서는 Windows 10 IoT Core에서 GPIO, I2C, SPI 및 UART에 대한 사용자 모드 액세스를 사용하도록 설정하는 방법에 대해 설명합니다. |
| [디바이스 열거](enumerate-devices.md) | 열거형 네임스페이스를 사용하면 시스템에 내부에서 연결되거나, 외부에서 연결되거나, 무선 또는 네트워킹 프로토콜을 통해 검색 가능한 디바이스를 찾을 수 있습니다. |
| [디바이스 페어링](pair-devices.md) | 일부 디바이스는 페어링해야 사용할 수 있습니다. [<strong>Windows.Devices.Enumeration</strong>](https://msdn.microsoft.com/library/windows/apps/BR225459) 네임스페이스는 세 가지 방법으로 디바이스를 페어링하도록 지원합니다. |
| [대역 외 페어링](out-of-band-pairing.md) | 이 섹션에서는 대역 외 페어링을 사용하여 앱이 검색 없이 특정 디바이스에 연결할 수 있는 방법을 설명합니다. | 
| [센서](sensors.md) | 센서를 사용하면 앱에서 디바이스와 디바이스를 둘러싼 실제 주변 환경 간의 관계를 알 수 있습니다. 센서는 디바이스의 방향과 움직임을 앱에 알려줄 수 있습니다. |
| [Bluetooth](bluetooth.md) | 이 섹션에는 RFCOMM, GATT 및 LE(저에너지) 광고를 사용하는 방법을 비롯해 UWP(유니버설 Windows 플랫폼) 앱에 Bluetooth를 통합하는 방법에 대한 문서가 포함되어있습니다. | 
| [인쇄 및 스캔](printing-and-scanning.md) | 이 섹션에서는 유니버설 Windows 앱에서 인쇄 및 스캔하는 방법을 설명합니다. | 
| [3D 인쇄](3d-printing.md) | 이 섹션에서는 유니버설 Windows 앱에서 3D 인쇄 기능을 활용하는 방법을 설명합니다. |
| [NFC 스마트 카드 앱 만들기](host-card-emulation.md) | Windows Phone 8.1에서는 SIM 기반 보안 요소를 사용하여 NFC 카드 에뮬레이션 앱을 지원했지만, 해당 모델에서는 보안 결제 앱이 MNO(모바일 네트워크 운영자)와 밀접하게 결합되어야 합니다. 이는 MNO와 결합되어 있지 않은 다른 판매자 또는 개발자에 의해 가능한 결제 솔루션의 다양성을 제한합니다. Windows 10 Mobile에서는 HCE(호스트 카드 에뮬레이션)라는 새로운 카드 에뮬레이션 기술이 도입되었습니다. HCE 기술을 통해 앱이 NFC 카드 판독기와 직접 통신할 수 있습니다. 이 항목에서는 HCE(호스트 카드 에뮬레이션)가 Windows 10 Mobile 장치에서 작동하는 방식을 설명하며 MNO와 공동 작업을 하지 않고도 고객이 실제 카드 대신 휴대폰을 통해 서비스에 액세스할 수 있도록 HCE 앱을 개발하는 방법을 보여 줍니다. |
| [배터리 정보 가져오기](get-battery-info.md) | [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/Dn895017) 네임스페이스에서 API를 사용하여 자세한 배터리 정보를 가져오는 방법을 알아봅니다. |

