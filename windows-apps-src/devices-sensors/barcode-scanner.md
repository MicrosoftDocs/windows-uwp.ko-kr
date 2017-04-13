---
author: mukin
title: "바코드 스캐너"
description: "이 문서는 바코드 스캐너 서비스 지점 장치 제품군에 대한 정보를 포함하고 있습니다."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8d9ef9bc08fa666c2af1348450f7a5fb0a0c7b65
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="barcode-scanner"></a>바코드 스캐너
응용 프로그램 개발자가 [바코드 스캐너](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodescanner)에 액세스하여, 하드웨어의 지원 여부에 따라 UPC, QR 코드와 같은 바코드 기호의 디코딩된 데이터를 검색할 수 있도록 합니다. 지원되는 기호의 전체 목록은 [BarcodeSymbologies](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodesymbologies) 클래스를 참조하세요.

## <a name="requirements"></a>요구 사항
이 네임스페이스를 필요로 하는 응용 프로그램은 앱 패키지 매니페스트에 “pointOfService”([DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef))를 추가해야 합니다.

## <a name="device-support"></a>장치 지원

### <a name="usb"></a>USB

#### <a name="hidscanner"></a>HID.Scanner
Windows에는 USB.org에서 정의한 HID.Scanner(8C) 페이지를 기반으로 하는 바코드 스캐너 클래스 드라이버가 포함되어 있습니다. 이 클래스 드라이버는 이 표준을 구현하는 모든 바코드 스캐너를 지원하며, 예는 다음과 같습니다. 제조업체    모델 Honeywell    1900GSR-2, 1200g-2 Intermec    SG20

바코드 스캐너가 USB.HID.Scanner로 구성될 수 있는지 알아보려면 바코드 스캐너 설명서를 참조하거나 제조업체에 문의하십시오.

#### <a name="hidvendor-specific"></a>HID.Vendor 특정
Windows는 추가 바코드 스캐너 지원을 위해 특정 벤더 드라이버의 구현을 지원합니다. 장치를 기본 제공 USB.HID.Scanner에서 지원하지 않는지 바코드 스캐너 제조업체에 확인하십시오.

### <a name="bluetooth"></a>Bluetooth
#### <a name="serial-port-protocol-spp--simple-serial-interface-ssi"></a>SPP(직렬 포트 프로토콜) - SSI(단순 직렬 인터페이스)
Windows는 SPP-SSI 기반 Bluetooth 바코드 스캐너를 지원합니다.

| 제조업체 |    모델 |
|--------------|-----------|
| Socket Mobile |    **CHS 7 Series:** <br/> 7 Ci 1D Imager Barcode Scanner <br/> 7Di 1D Durable, Imager Barcode Scanner <br/> 7Mi 1D Laser Barcode Scanner <br/> 7Pi 1D Durable, LaserBarcode Scanner <br/> **DuraScan 700 Series:** <br/> D700 1D Imager Barcode Scanner <br/> D730 1D Laser Barcode Scanner <br/> **SocketScan 800 Series** <br/> S800 1D Imager Barcode Scanner(이전 CHS 8Ci) <br/> S850 2D Imager Barcode Scanner(이전 CHS 8Qi)

## <a name="examples"></a>예제
예제 구현을 위한 바코드 스캐너 샘플을 참조하세요.
+    [바코드 스캐너 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
