---
title: 바코드 스캐너 구성
description: 원하는 응용 프로그램에 대해 바코드 스캐너를 구성 하는 방법을 알아봅니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96deeb82f0aa04929ac33001b1aee0603e6474c7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168507"
---
# <a name="configure-a-barcode-scanner"></a>바코드 스캐너 구성

바코드 스캐너는 여러 가지 모드로 구성할 수 있습니다.  원하는 응용 프로그램에 대해 바코드 스캐너를 올바르게 구성 하는 것이 중요 합니다.

여러 바코드 스캐너를 **키보드 쐐기형** 모드로 구성할 수 있습니다. 그러면 바코드 스캐너가 Windows에 키보드로 표시 됩니다.  이 기능을 사용 하면 메모장과 같이 바코드 스캐너가 인식 하지 않는 응용 프로그램으로 바코드를 스캔할 수 있습니다.  이 모드에서 바코드를 검색 하면 키보드를 사용 하 여 데이터를 입력 한 것 처럼 바코드 스캐너에서 디코딩된 데이터가 삽입 지점에 삽입 됩니다.  UWP 응용 프로그램에서 바코드 스캐너를 더 효율적으로 제어 하려면 비 키보드 쐐기형 모드에서 구성 해야 합니다.

## <a name="usb-barcode-scanner"></a>USB 바코드 스캐너
Windows에 포함 된 바코드 스캐너 드라이버를 사용 하려면 **HID POS 스캐너** 모드에서 USB 연결 바코드 스캐너를 구성 해야 합니다. 이 드라이버는 [USB hid](https://www.usb.org/hid)에 게시 된 **HID Point Of Sale 사용 테이블** 사양을 구현한 것입니다.  **HID POS 스캐너** 모드를 사용 하도록 설정 하는 방법에 대 한 지침은 바코드 스캐너 설명서를 참조 하거나 바코드 스캐너 제조업체에 문의 하세요.  **HID POS 스캐너** 로 구성 된 후에는 바코드 스캐너가 pos **바코드** 스캐너 노드 아래에 있는 Device Manager에 **pos HID 바코드 스캐너**로 표시 됩니다.

바코드 스캐너 제조업체에는 **HID POS 스캐너**이외의 모드를 사용 하 여 UWP 바코드 스캐너 api를 지 원하는 공급 업체별 드라이버만 있을 수 있습니다.  UWP 바코드 스캐너 Api와 호환 되는 제조업체에서 제공 하는 드라이버를 이미 설치한 경우 Device Manager의 **POS 바코드 스캐너** 아래에 공급 업체별 장치가 표시 될 수 있습니다.

## <a name="bluetooth-barcode-scanner"></a>Bluetooth 바코드 스캐너
UWP 바코드 스캐너 Api에서 작동 하려면 Bluetooth 연결 된 스캐너를 **직렬 포트 프로토콜-SPP (Simple Serial Interface)** 모드에서 구성 해야 합니다.  **SPP-SSI 모드**를 사용 하도록 설정 하는 지침은 바코드 스캐너 설명서를 참조 하거나 바코드 스캐너 제조업체에 문의 하세요.

Bluetooth 바코드 스캐너를 사용 하려면 먼저 **설정 > 장치 > bluetooth & 기타 장치를 사용 하 여 bluetooth 또는 다른 장치를 추가 >** 합니다.

[Windows. Devices](/uwp/api/windows.devices.enumeration) 네임 스페이스를 사용 하 여 페어링 프로세스를 시작 하 고 제어할 수 있습니다.  자세한 내용은 [장치 페어링](./pair-devices.md) 을 참조 하세요.

[!INCLUDE [feedback](./includes/pos-feedback.md)]