---
title: 서비스 지점 하드웨어 지원
description: 이 문서에는 각 서비스 장치 클래스 지점의 하드웨어 지원에 대 한 정보가 포함 되어 있습니다.
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf5d6a2413ba6aeb2e3fd86122e865e34b8729fa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172207"
---
# <a name="supported-point-of-service-peripherals"></a>지원 되는 서비스 주변 장치 지점

## <a name="barcode-scanner"></a>바코드 스캐너
| 연결 | 지원 |
| -------------|-------------|
| USB          | <p>Windows에는 [USB.org](https://www.usb.org/hid)에서 정의한 HID POS 스캐너 사용 테이블 (8c) 사양을 기반으로 하는 USB 연결 바코드 스캐너에 대 한 기본 클래스 드라이버가 포함 되어 있습니다. 알려진 호환 장치 목록은 아래 표를 참조 하세요.  바코드 스캐너에 대 한 설명서를 참조 하거나 제조업체에 문의 하 여 USB에서 스캐너를 구성 하는 방법을 확인 하세요 **. 숨겼습니다. POS 스캐너** 모드입니다. </p><p>Windows는 또한 USB를 지원 하지 않는 추가 바코드 스캐너를 지원 하기 위해 공급 업체의 특정 드라이버 구현을 지원 합니다. 숨겼습니다. POS 스캐너 표준입니다. 공급 업체별 드라이버 가용성은 바코드 스캐너 제조업체에서 확인 하세요.</p><p>바코드 스캐너 제조업체 사용자 지정 바코드 스캐너 드라이버를 만드는 방법에 대 한 자세한 내용은 [바코드 스캐너 드라이버 디자인 가이드](/windows-hardware/drivers/ddi/_pos/index) 를 참조 하세요.</p> |
| Bluetooth    | <p>Windows에서는 SPP (직렬 포트 프로토콜-단순 직렬 인터페이스) 기반 Bluetooth 바코드 스캐너를 지원 합니다. 알려진 호환 장치 목록은 아래 표를 참조 하세요. 사용자의 바코드 스캐너에 대 한 설명서를 참조 하거나 제조업체에 문의 하 여 **SPP-SSI** 모드에서 스캐너를 구성 하는 방법을 확인 하세요.</p> |
| 웹캠       | <p>Windows 10 버전 1803부터 유니버설 Windows 응용 프로그램에서 표준 카메라 렌즈를 통해 바코드를 읽을 수 있습니다. 자동 포커스를 지 원하는 카메라와 최소 해상도 1920 x 1440을 사용 하는 것이 좋습니다.  일부 낮은 해상도 카메라는 바코드를 충분히 크게 인쇄 하는 경우 표준 바코드를 읽을 수 있습니다.  더 가늘게 요소가 있는 바코드는 고해상도 카메라를 요구 합니다.</p>| 
|


| 제조업체  | 모델                          | 기능 | 연결    | 유형         | Mode                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| 코드          | 판독기™ 950                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| 코드          | 판독기™ 1021                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| 코드          | 판독기™ 1421                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| 코드          | 판독기™ 5000                   | 2D         | USB          | 프레젠테이션 | HID POS 스캐너           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | 프레젠테이션 | HID POS 스캐너           |
| Honeywell     | Granit 198Xi 크                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | N5680                          | 2D         | 내부     | 구성 요소    | HID POS 스캐너           |
| Honeywell     | N3680                          | 2D         | 내부     | 구성 요소    | HID POS 스캐너           |
| Honeywell     | 궤도 7190g                    | 2D         | USB          | 프레젠테이션 | HID POS 스캐너           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | In 카운터   | HID POS 스캐너           |
| Honeywell     | Voyager 1200g                  | 차원         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 1202g                  | 차원         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 1202-bf                | 차원         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 145Xg                  | 1D/2D<sup>1</sup>   | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | 크 크 1900g                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | 크 크 1902g                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | 크 크-1902g-bf                 | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | 크 크로 1900h                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Honeywell     | 크 세 1902h                    | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| HP            | 값 바코드 스캐너 (HR2150) | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| Intermec      | SG20                           | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| 소켓 모바일 | CHS 7Ci                        | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | CHS 7Di                        | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | CHS 7Mi                        | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | CHS 7Pi                        | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | CHS 8Ci                        | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | DuraScan D700                  | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | DuraScan D730                  | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | DuraScan D740                  | 2D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | 소켓 스캔 S700                | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | 소켓 스캔 S730                | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | 소켓 스캔 S740                | 2D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | 소켓 스캔 S800                | 차원         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 소켓 모바일 | 소켓 스캔 S850                | 2D         | Bluetooth    | 핸드헬드     | 직렬 포트 프로필 (SPP) |
| 얼룩말         | DS2208<sup>2</sup>                        | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| 얼룩말         | DS2278                         | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| 얼룩말         | DS8108<sup>3</sup>                        | 2D         | USB          | 핸드헬드     | HID POS 스캐너           |
| 얼룩말         | DS8178<sup>4</sup>                         | 2D         | USB          | 핸드헬드     | HID POS 스캐너           | 


<sup>1</sup> Honeywell를 통해 2d 바코드를 지원할 수 있도록 업그레이드 <br/>
<sup>2</sup> 최소 펌웨어 009 (2018.07.09) 필요 합니다. 얼룩말 [123Scan](http://www.zebra.com/123scan)을 사용 하 여 업그레이드할 때<br/>
<sup>3</sup> 2018.01.18 (최소 펌웨어 016)가 필요 합니다. 얼룩말 [123Scan](http://www.zebra.com/123scan)을 사용 하 여 업그레이드할 때<br/> 
<sup>4</sup> 최소 펌웨어 023 (2019.03.11) 필요 합니다. 얼룩말 [123Scan](http://www.zebra.com/123scan)을 사용 하 여 업그레이드할 때<br/>

<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>기본 제공 바코드 스캐너를 사용 하는 Windows 장치
| 제조업체   | 모델 | 운영 체제 |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>기본 제공 바코드 스캐너를 사용 하는 Windows Mobile 장치
| 제조업체   | 모델 | 운영 체제 |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ-E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| 얼룩말          | TC700j | Windows Mobile   |
| HP             | 정예 X3 재킷 | Windows Mobile   |




## <a name="cash-drawer"></a>현금 서랍
| 연결 | 지원 |
| -------------|-------------|
| 네트워크/Bluetooth | <p> 현금 서랍에 직접 연결 하는 것은 현금 서랍 단위의 기능에 따라 네트워크 또는 Bluetooth를 통해 만들 수 있습니다. </p><p>APG 현금 서랍: NetPRO, BluePRO</p> |
| 진한 포트 | <p> 네트워크 또는 Bluetooth 기능을 포함 하지 않는 현금 서랍은 지원 되는 수신 프린터의 진한 포트 또는 별모양 Micronics 진한 방송 현금 액세서리를 통해 연결할 수 있습니다. </p>
| OPOS    | <p> 는 제조업체에서 제공 하는 OPOS 서비스 개체를 통해 OPOS와 호환 되는 현금 서랍을 지원 합니다. 장치 제조업체 설치 지침에 따라 OPOS 드라이버를 설치 합니다. </p> |


## <a name="customer-display-linedisplay"></a>고객 표시 (LineDisplay)
는 제조업체에서 제공 하는 OPOS 서비스 개체를 통해 모든 OPOS 호환 선 표시를 지원 합니다. 장치 제조업체 설치 지침에 따라 OPOS 드라이버를 설치 합니다.

## <a name="magnetic-stripe-reader"></a>자기 줄무늬 판독기
Windows는 해당 공급 업체 ID 및 제품 ID (VID/PID)를 기반으로 하는 Magtek 및 IDTech의 다음 자기 줄무늬 판독기를 지원 합니다.

| 제조업체 |    모델 |  부품 번호 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID: 0ACD PID: 2010) | IDRE 3x5xxxx |
| | MiniMag (VID: 0ACD PID: 0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID: 0801 PID: 0011) |  210730xx |
| | Dynamag (VID: 0801 PID: 0002) |   210401xx |

 Windows에서는 추가 공급 업체 관련 드라이버를 구현 하 여 추가 자기 줄무늬 판독기를 지원할 수 있습니다. 사용 가능 여부는 자기 줄무늬 판독기 제조업체에 문의 하세요. 자기 줄무늬 판독기 제조업체 사용자 지정 자기 줄무늬 판독기 드라이버를 만드는 방법에 대 한 자세한 내용은 [자기 줄무늬 판독기 드라이버 디자인 가이드](/windows-hardware/drivers/ddi/_pos/index) 를 참조 하세요.

## <a name="receipt-printer-posprinter"></a>수신 프린터 (POSPrinter)
| 연결 | 지원 |
| -------------|-------------|
| 네트워크 및 Bluetooth | <p>Windows는 Epson ESC/POS 프린터 컨트롤 언어를 사용 하 여 네트워크 및 Bluetooth 연결 수신 프린터를 지원 합니다.  아래에 나열 된 프린터는 POSPrinter Api를 사용 하 여 자동으로 검색 됩니다. ESC/POS 에뮬레이션을 제공 하는 추가 수신 프린터도 작동할 수 있지만 [대역 외 페어링](./point-of-service.md#out-of-band-pairing) 프로세스를 사용 하 여 연결 해야 합니다.</p><p>참고: slip 스테이션 및 저널 스테이션은이 방법을 통해 지원 되지 않습니다.</p> |
| OPOS    | <p> OPOS 서비스 개체를 통해 OPOS 호환 수신 프린터를 지원 합니다. 장치 제조업체 설치 지침에 따라 OPOS 드라이버를 설치 합니다. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>고정 수신 프린터 (네트워크/Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>모바일 수신 프린터 (Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |