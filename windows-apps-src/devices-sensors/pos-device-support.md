---
author: TerryWarwick
title: 서비스 지점 하드웨어 지원
description: 이 문서에는 각각의 서비스 지점 장치 클래스에서 하드웨어 지원에 대한 정보가 포함
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb2468497115c9595f6fd17ab61b30caed507ab
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832097"
---
# <a name="supported-point-of-service-peripherals"></a>지원되는 서비스 지점 주변 장치

## <a name="barcode-scanner"></a>바코드 스캐너
| 연결 | 지원 |
| -------------|-------------|
| USB          | <p>Windows에는 [USB.org](http://www.usb.org/developers/hidpage/)에서 정의된 HID POS 스캐너 사용 테이블(8c) 규격에 기초한 USB 연결 바코드 스캐너용 기본 제공 클래스 드라이버가 포함되어 있습니다. 알려진 호환 장치의 목록은 아래 표를 참조하세요.  바코드 스캐너를 **USB.HID.POS** 스캐너 모드로 구성하는 방법을 알아보려면 바코드 스캐너 설명서를 참조하거나 제조업체에 문의하세요. </p><p>Windows는 특정 공급 업체의 구현 또한 지원하여 USB.HID.POS 스캐너 표준을 지원하지 않는 추가 바코드 스캐너를 지원합니다. 특정 공급 업체의 드라이버 사용 가능 여부는 바코드 스캐너 제조업체에 확인하세요.</p><p>바코드 스캐너 제조업체의 경우 사용자 지정 바코드 스캐너 드라이버 생성에 대한 내용은 [바코드 스캐너 드라이버 디자인 가이드](https://aka.ms/pointofservice-drv)를 참조하세요.</p> |
| Bluetooth    | <p>Windows는 SPP(직렬 포트 프로토콜) - SSI(단순 직렬 인터페이스) 기반 Bluetooth 바코드 스캐너를 지원합니다. 알려진 호환되는 디바이스 목록은 다음 표를 참조하세요. 바코드 스캐너를 **SPP-SSI** 스캐너 모드로 구성하는 방법을 알아보려면 바코드 스캐너 설명서를 참조하거나 제조업체에 문의하세요.</p> |
| 웹캠       | <p>Windows 10, 버전 1803부터는 유니버설 Windows 응용 프로그램에서 기준 카메라 렌즈를 통해 바코드를 읽을 수 있습니다. 자동 초점과 최소 1920 x 1440의 해상도를 지원하는 카메라를 사용하는 것이 좋습니다.  일부 저해상도 카메라에서는 바코드가 충분히 크게 인쇄되는 경우에 표준 바코드를 읽을 수 있습니다.  슬림한 요소를 가진 바코드에서는 고해상도 카메라가 필요할 수 있습니다.</p>| 
|


### <a name="compatible-barcode-scanners"></a>호환되는 바코드 스캐너
| 범주 | 연결 | 제조업체/모델 |
|--------------|-----------|-----------|
| **1D Handheld 스캐너** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg(업그레이드 가능)|
| **1D Handheld 스캐너** | **Bluetooth** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800(이전의 CHS 8Ci) <br/>|
|**2D Handheld 스캐너** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg(업그레이드 가능)<br/>Honeywell Voyager 1602g<br/>Intermec SG20<br/>Zebra DS2278<br/>Zebra DS8108 ¹<hr><small>¹ 최소 펌웨어 016(2018년 1월 18일 출시)이 필요합니다 [123Scan](http://www.zebra.com/123Scan)을 사용하여 업그레이드 가능</small>|
|**2D 핸드헬드 스캐너** | **Bluetooth** |Socket Mobile SocketScan S850(이전의 CHS 8Qi)|
| **프레젠테이션 스캐너** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **인카운터 스캐너** | **USB** |Honeywell Stratos 2700|
| **검색 엔진** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Windows Mobile 장치**| **기본 제공** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Windows Mobile 장치**| **사용자 지정** | HP Elite X3(바코드 스캐너 덮개 포함) |


## <a name="cash-drawer"></a>현금 출납기
| 연결 | 지원 |
| -------------|-------------|
| 네트워크/Bluetooth | <p> 현금 출납기 장치의 기능에 따라 네트워크나 Bluetooth를 통해 현금 출납기에 직접 연결할 수 있습니다. </p><p>APG Cash Drawer: NetPRO BluePRO</p> |
| DK 포트 | <p> 네트워크 또는 Bluetooth 기능이 없는 현금 출납기의 경우 지원되는 영수증 프린터나 Star Micronics DK-AirCash 액세서리를 통해 연결할 수 있습니다. </p>
| OPOS    | <p> 제조업체에서 제공하는 OPOS 서비스 개체를 통해 OPOS 호환 금전 출납기를 지원합니다. 장치 제조업체의 설치 지침에 따라 OPOS 드라이버를 설치합니다. </p> |


## <a name="customer-display-linedisplay"></a>고객 디스플레이(LineDisplay)
제조업체에서 제공하는 OPOS 서비스 개체를 통해 OPOS 호환 라인 디스플레이를 지원합니다. 장치 제조업체의 설치 지침에 따라 OPOS 드라이버를 설치합니다.

## <a name="magnetic-stripe-reader"></a>자기 띠 판독기
Windows는 공급업체 ID와 제품 ID(VID/PID)를 기반으로 Magtek과 IDTech의 다음과 같은 자기 띠 판독기를 지원합니다.

| 제조업체 |    모델 |  부품 번호 |
|--------------|-----------|--------------|
| IDTech | SecureMag(VID:0ACD PID:2010) | IDRE-3x5xxxx |
| | MiniMag(VID:0ACD PID:0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe(VID:0801 PID:0011) |  210730xx |
| | Dynamag(VID:0801 PID:0002) |   210401xx |

 Windows는 추가 자기 띠 판독기 지원을 위해 측정 추가 공급업체 드라이버의 구현을 지원합니다. 자기 띠 판독기 제조업체에 사용 가능 여부를 확인하세요. 자기 띠 판독기 제조업체들의 경우 사용자 지정 자기 띠 판독기 드라이버를 생성하는 방법은 [자기 띠 판독기 드라이버 디자인 가이드](https://aka.ms/pointofservice-drv)를 참조하세요.

## <a name="receipt-printer-posprinter"></a>영수증 프린터(POSPrinter)
| 연결 | 지원 |
| -------------|-------------|
| 네트워크 및 Bluetooth | <p>Windows는 Epson ESC/POS 프린터 제어 언어를 사용하여 네트워크와 Bluetooth로 연결된 영수증 프린터를 지원합니다.  아래 나열된 프린터는 POSPrinter API를 사용하여 자동으로 검색됩니다. ESC/POS 에뮬레이션을 제공하는 추가 영수증 프린터도 작동할 수 있지만, [대역 외 페어링](https://aka.ms/pointofservice-oobpairing) 프로세스를 사용하여 연결해야 합니다.</p><p>참고: 전표 스테이션 및 저널 스테이션은 이 방법으로 지원되지 않습니다.</p> |
| OPOS    | <p> OPOS 서비스 개체를 통해 OPOS 호환 영수증 프린터를 지원합니다. 장치 제조업체의 설치 지침에 따라 OPOS 드라이버를 설치합니다. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>고정 영수증 프린터(네트워크/Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>모바일 영수증 프린터(Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |
