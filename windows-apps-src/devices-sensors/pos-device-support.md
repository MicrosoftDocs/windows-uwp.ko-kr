---
author: mukin
title: "POS 디바이스 지원"
description: "이 문서에서는 각 POS 장치 제품군에 대한 디바이스 지원에 대한 정보를 담고 있습니다."
ms.author: mukin
ms.date: 05/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 58018385082f7f7e49edba0351d919bc840ade05
ms.sourcegitcommit: 53930c9871461f6106f785ae4fabb2296eb359f1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2017
---
# <a name="pos-device-support"></a>POS 디바이스 지원

## <a name="barcode-scanner"></a>바코드 스캐너
| 연결 | 지원 |
| -------------|-------------|
| USB          | <p>Windows에는 [USB.org](http://www.usb.org/developers/hidpage/)에서 정의된 HID POS 스캐너 사용 테이블(8c) 규격에 기초한 USB와 연결된 바코드 스캐너에 대한 기본 제공 클래스 드라이버가 포함되어 있습니다. 알려진 호환되는 디바이스 목록은 다음 표를 참조하세요.  바코드 스캐너가 USB.HID.POS 스캐너 모드로 구성될 수 있는지 알아보려면 바코드 스캐너 설명서를 참조하거나 제조업체에 문의하십시오. </p><p>Windows는 특정 공급 업체의 구현 또한 지원하여 USB.HID.POS 스캐너 표준을 지원하지 않는 추가 바코드 스캐너를 지원합니다. 특정 공급 업체의 드라이버 사용 가능 여부는 바코드 스캐너 제조업체에 확인하세요.</p>|
| Bluetooth    | <p>Windows는 SPP-SSI 기반 Bluetooth 바코드 스캐너를 지원합니다. 알려진 호환되는 디바이스 목록은 다음 표를 참조하세요.</p> |

### <a name="compatible-hardware"></a>호환 가능한 하드웨어
| 범주 | 연결 | 제조업체/모델 |
|--------------|-----------|-----------|
| **1D Handheld 스캐너** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg(업그레이드 가능)|
| **1D Handheld 스캐너** | **Bluetooth** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800(이전의 CHS 8Ci) <br/>|
|**2D Handheld 스캐너** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg(업그레이드 가능)<br/>Honeywell Voyager 1602g<br/>Intermec SG20|
|**2D Handheld 스캐너** | **Bluetooth** |Socket Mobile SocketScan S850(이전의 CHS 8Qi)|
| **프레젠테이션 스캐너** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **인카운터 스캐너** | **USB** |Honeywell Stratos 2700|
| **검색 엔진** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Windows Mobile 장치**| **기본 제공** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Windows Mobile 장치**| **사용자 지정** | HP Elite X3(바코드 스캐너 덮개 포함) |

## <a name="cash-drawer"></a>현금 출납기
| 연결 | 지원 |
| -------------|-------------|
| 네트워크/Bluetooth | <p> 현금 출납기 장치의 기능에 따라 네트워크나 Bluetooth를 통해 현금 출납기에 직접 연결할 수 있습니다. </p>|
| DK 포트 | <p> 네트워크 또는 Bluetooth 기능이 없는 현금 출납기의 경우 지원되는 POS 프린터나 Star Micronics DK-AirCash 액세서리를 통해 연결할 수 있습니다. </p>
| OPOS    | <p> OPOS 드라이버를 지원하는 모든 현금 출납기 디바이스를 지원하며 OPOS 서비스 개체를 제공합니다. 특정 장치 제조업체의 설치 지침에 따라 OPOS 드라이버를 설치합니다. 이를 통해 제조업체 사양에 기초하여 USB 및 기타 연결을 지원합니다. </p> |

**참고:** DK-AirCash에 대한 자세한 내용은 Star Micronics에 문의하십시오.

### <a name="networkbluetooth"></a>네트워크/Bluetooth
| 제조업체 |    모델 |
|--------------|-----------|
| APG Cash Drawer |    NetPRO, BluePRO |

## <a name="line-display"></a>라인 디스플레이
OPOS 드라이버를 지원하는 모든 라인 디스플레이 디바이스를 지원하며 OPOS 서비스 개체를 제공합니다. 특정 장치 제조업체의 설치 지침에 따라 OPOS 드라이버를 설치합니다.

## <a name="magnetic-stripe-reader"></a>자기 띠 판독기

Windows에는 [USB.org](http://www.usb.org/developers/hidpage/)에서 정의된 HID POS 스캐너 사용 테이블(8c) 규격에 기초한 USB와 연결된 자기 띠 판독기에 대한 기본 제공 클래스 드라이버가 포함되어 있습니다.

### <a name="vendor-specific-support"></a>특정 공급업체 관련 지원
Windows는 공급업체 ID와 제품 ID(VID/PID)를 기반으로 Magtek과 IDTech의 다음과 같은 자기 띠 판독기를 지원합니다.

| 제조업체 |     모델 |    부품 번호 |
|--------------|-----------|--------------|
| IDTech | SecureMag(VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag(VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe(VID:0801 PID:0011) |    210730xx |
| |    Dynamag(VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>특정 사용자 지정 공급업체
Windows는 추가 자기 띠 판독기 지원을 위해 측정 추가 공급업체 드라이버의 구현을 지원합니다. 자기 띠 판독기 제조업체에 사용 가능 여부를 확인하세요.

## <a name="pos-printer"></a>POS 프린터
Windows는 Epson ESC/POS 프린터 제어 언어를 네트워크와 Bluetooth로 연결된 영수증 프린터로 인쇄하는 기능을 지원합니다. ESC/POS에 대한 자세한 내용은 [서식 지정을 사용하는 Epson ESC/POS](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting)를 참조하세요.

API에서 제공하는 클래스, 열거형, 인터페이스는 영수증 프린터, 전표 프린터, 업무 일지 프린터를 지원하지만 드라이버 인터페이스는 영수증 프린터만 지원합니다. 지금 전표 프린터나 업무 일지 프린터를 사용하려고 시도하면 구현되지 않았다는 상태가 반환됩니다.

| 연결 | 지원 |
| -------------|-------------|
| 네트워크/Bluetooth | <p> POS 프린터 장치의 기능에 따라 네트워크나 Bluetooth를 통해 POS 프린터에 직접 연결할 수 있습니다. </p>|
| OPOS    | <p> OPOS 드라이버를 지원하는 모든 POS 프린터 장치를 지원 및/또는 OPOS 서비스 개체를 제공합니다. 특정 장치 제조업체의 설치 지침에 따라 OPOS 드라이버를 설치합니다. 이를 통해 제조업체 사양에 기초하여 USB 및 기타 연결을 지원합니다. </p> |

### <a name="stationary-pos-printers-networkbluetooth"></a>고정 POS 프린터(네트워크/Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>모바일 POS 프린터(Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |