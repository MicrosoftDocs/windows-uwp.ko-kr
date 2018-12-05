---
title: 바코드 스캐너 구성
description: 원하는 응용 프로그램에 대 한 바코드 스캐너를 구성 하는 방법을 알아봅니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8f88b376dca80043f88260700bb7ef4168b3a445
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8709775"
---
# <a name="configure-a-barcode-scanner"></a>바코드 스캐너 구성

바코드 스캐너는 몇 가지 모드로 구성이 가능합니다.  바코드 스캐너를 원하는 응용 프로그램에 맞게 올바르게 구성하는 것이 중요합니다.

바코드 스캐너가 Windows에 키보드로 나타나도록 해주는 **키보드 웨지** 모드에서 다양한 바코드 스캐너를 구성할 수 있습니다.  따라서 메모장 같이 바코드 스캐너를 인식하지 못하는 응용 프로그램에 대해서도 바코드를 스캔할 수 있습니다.  이 모드에서 바코드를 스캔할 때 키보드를 사용하여 데이터를 입력하는 것처럼 바코드 스캐너에서 디코딩된 데이터가 삽입 지점에 삽입됩니다.  UWP 응용 프로그램에서 바코드 스캐너를 더 많이 제어하고 싶은 경우에는 키보드가 아닌 웨지 모드로 이를 구성해야 합니다.

## <a name="usb-barcode-scanner"></a>USB 바코드 스캐너
USB에 연결된 바코드 스캐너는 Windows에 포함된 바코드 스캐너 드라이버에서 작동하도록 **HID POS 스캐너** 모드로 구성해야 합니다. 이 드라이버는 [USB HID](http://www.usb.org/developers/hidpage/)에 게시 **HID Point of Sale 사용 표** 사양 구현 합니다.  바코드 스캐너 설명서를 참조하거나 바코드 스캐너 제조업체에게 **HID POS 스캐너** 모드를 활성화하기 위한 지침을 문의하세요.  **HID POS 스캐너**로 구성이 된 바코드 스캐너는 **POS 바코드 스캐너** 노드 아래의 장치 관리자에 **POS HID 바코드 스캐너**로 표시됩니다.

바코드 스캐너 제조업체는 **HID POS 스캐너** 이외의 모드를 사용하여 UWP 바코드 스캐너 API를 지원하는 공급업체별 드라이버도 가지고 있을 수 있습니다.  UWP 바코드 스캐너 Api와 호환 되는 제조업체에서 제공한 드라이버를 이미 설치한 경우에 장치 관리자에서 **POS 바코드 스캐너** 아래에 나열 된 공급 업체별 장치를 볼 수 있습니다.

## <a name="bluetooth-barcode-scanner"></a>Bluetooth 바코드 스캐너
Bluetooth에 연결된 바코드 스캐너는 UWP 바코드 스캐너 API에서 작동할 수 있도록 **직렬 포트 프로토콜 - 간단한 직렬 인터페이스(SPP-SSI)** 모드로 구성해야 합니다.o work with the UWP Barcode Scanner APIs.  바코드 스매너 설명서를 참조하거나 바코드 스캐너 제조업체에게 **SPP-SSI 모드**를 활성화하기 위한 지침을 문의하세요.

사용 하 여 페어링을 해야 Bluetooth 바코드 스캐너를 사용 하려면 먼저 **설정 > 장치 > Bluetooth 및 다른 디바이스 > Bluetooth 또는 기타 디바이스 추가**합니다.

시작 하 고 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) 네임 스페이스를 사용 하 여 페어링 프로세스를 제어할 수 있습니다.  자세한 내용은 [장치 페어링](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)을 참조하세요.

[!INCLUDE [feedback](./includes/pos-feedback.md)]