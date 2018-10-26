---
author: TerryWarwick
title: 서비스 지점 하드웨어 지원
description: 이 문서에는 각각의 서비스 지점 장치 클래스에서 하드웨어 지원에 대한 정보가 포함
ms.author: jken
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df6e2c15260759f164a37b68365e0268633b22d5
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5640175"
---
# <a name="supported-point-of-service-peripherals"></a>지원되는 서비스 지점 주변 장치

## <a name="barcode-scanner"></a>바코드 스캐너
| 연결 | 지원 |
| -------------|-------------|
| USB          | <p>Windows에는 [USB.org](http://www.usb.org/developers/hidpage/)에서 정의된 HID POS 스캐너 사용 테이블(8c) 규격에 기초한 USB 연결 바코드 스캐너용 기본 제공 클래스 드라이버가 포함되어 있습니다. 알려진 호환 장치의 목록은 아래 표를 참조하세요.  바코드 스캐너를 **USB.HID.POS** 스캐너 모드로 구성하는 방법을 알아보려면 바코드 스캐너 설명서를 참조하거나 제조업체에 문의하세요. </p><p>Windows는 특정 공급 업체의 구현 또한 지원하여 USB.HID.POS 스캐너 표준을 지원하지 않는 추가 바코드 스캐너를 지원합니다. 특정 공급 업체의 드라이버 사용 가능 여부는 바코드 스캐너 제조업체에 확인하세요.</p><p>바코드 스캐너 제조업체의 경우 사용자 지정 바코드 스캐너 드라이버 생성에 대한 내용은 [바코드 스캐너 드라이버 디자인 가이드](https://aka.ms/pointofservice-drv)를 참조하세요.</p> |
| Bluetooth    | <p>Windows는 SPP(직렬 포트 프로토콜) - SSI(단순 직렬 인터페이스) 기반 Bluetooth 바코드 스캐너를 지원합니다. 알려진 호환되는 디바이스 목록은 다음 표를 참조하세요. 바코드 스캐너를 **SPP-SSI** 스캐너 모드로 구성하는 방법을 알아보려면 바코드 스캐너 설명서를 참조하거나 제조업체에 문의하세요.</p> |
| 웹캠       | <p>Windows 10, 버전 1803부터는 유니버설 Windows 응용 프로그램에서 기준 카메라 렌즈를 통해 바코드를 읽을 수 있습니다. 자동 초점과 최소 1920 x 1440의 해상도를 지원하는 카메라를 사용하는 것이 좋습니다.  일부 저해상도 카메라에서는 바코드가 충분히 크게 인쇄되는 경우에 표준 바코드를 읽을 수 있습니다.  슬림한 요소를 가진 바코드에서는 고해상도 카메라가 필요할 수 있습니다.</p>| 
|


| 제조업체  | 모델                          | 접근 권한 값 | 연결    | 유형         | 모드                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| Code          | Reader™ 950                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Code          | Reader™ 1021                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Code          | Reader™ 1421                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Code          | Reader™ 5000                   | 2D         | USB          | 프레젠테이션 | HID POS 스캐너           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | 프레젠테이션 | HID POS 스캐너           |
| Honeywell     | Granit 198Xi                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | N5680                          | 2D         | 내부     | 구성 요소    | HID POS 스캐너           |
| Honeywell     | N3680                          | 2D         | 내부     | 구성 요소    | HID POS 스캐너           |
| Honeywell     | 궤도 7190g                    | 2D         | USB          | 프레젠테이션 | HID POS 스캐너           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | 카운터   | HID POS 스캐너           |
| Honeywell     | Voyager 1200g                  | 1D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 1202g                  | 1D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 1202-bf                | 1D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 145Xg                  | 1D/2 D ¹   | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Xenon 1900g                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Xenon 1902g                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Xenon 1902g-bf                 | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Xenon 1900h                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Xenon 1902h                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| HP            | 바코드 스캐너 (HR2150) 값 | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Intermec      | SG20                           | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Socket Mobile | CHS 7Ci                        | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | CHS 7Di                        | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | CHS 7mi                        | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | CHS 7Pi                        | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | CHS 8Ci                        | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | DuraScan D700                  | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | DuraScan D730                  | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | DuraScan D740                  | 2D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | SocketScan S700                | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | SocketScan S730                | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | SocketScan S740                | 2D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | SocketScan S800                | 1D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Socket Mobile | SocketScan S850                | 2D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| Zebra         | DS2278                         | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Zebra         | DS8108²                        | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
|


Honeywell 통해 2D 바코드를 지원 하기 위해 ¹ Upgradable <br/>
² 최소 펌웨어 016 (2018.01.18)가 필요 합니다. 업그레이드 가능한 Zebra [123Scan](http://www.zebra.com/123Scan)를 사용 합니다. 


<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>기본 제공 바코드 스캐너를 사용 하 여 Windows 장치
| 제조업체   | 모델 | 운영 체제 |
|----------------|-------|------------------|
| Innowi         | ChecOut M | Windows10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>기본 제공 바코드 스캐너를 사용 하 여 Windows Mobile 장치
| 제조업체   | 모델 | 운영 체제 |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| Zebra          | TC700j | Windows Mobile   |
| HP             | Elite X3 재킷 | Windows Mobile   |




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
