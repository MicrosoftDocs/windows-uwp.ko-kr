---
title: 카메라 바코드 스캐너 Symbologies
description: Windows 10과 함께 제공 되는 소프트웨어 바코드 디코더에 의해 지원 되는 각 symbologies에 대 한 샘플 바코드를 봅니다.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: a9618402a6ee76a20ff5f95418ee7280b39db4a2
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943133"
---
# <a name="symbologies"></a>Symbologies

이 항목은 Windows 10과 함께 제공 되는 소프트웨어 바코드 디코더가 지 원하는 각 symbologies에 대 한 샘플 바코드를 제공 합니다. 여기서는 UPC/e, 코드 39, 코드 128, 인터리브 2/5, 세로 막대형 전방향, 세로 막대형 스택형, QR 코드 및 GS1DWCode을 포함 합니다.

Windows 10은 소프트웨어 디코더와 결합 된 표준 렌즈 카메라를 사용 하 여 바코드 스캐너를 생성 합니다. 이 문서는 소프트웨어 디코더에 의해 지원 되는 symbologies을 나타냅니다. 기본 제공 하드웨어 디코더가 있는 전용 바코드 스캐너 장치에서 추가 symbologies 지원 될 수 있습니다. 자세한 내용은 바코드 스캐너 제조업체에 문의 하세요. 별도로 지정 하지 않는 한, 나열 된 symbologies는 모든 버전의 Windows 10 빌드 17134 이상에서 지원 됩니다.

[GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync) 를 사용 하 여 바코드 스캐너에서 지 원하는 특정 symbologies를 확인 합니다.

> [!NOTE]
> Windows 10에 기본 제공 되는 소프트웨어 디코더는 [*Digimarc Corporation*](https://www.digimarc.com/)에서 제공 합니다.

## <a name="1d-symbologies"></a>1D Symbologies

### <a name="code-39"></a>코드 39
![샘플 바코드-코드 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>코드 128
![샘플 바코드-코드 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>세로 막대형 전방향
![샘플 바코드-세로 막대형 전방향](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>누적 세로 막대형
![샘플 바코드-세로 막대형 스택형](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![샘플 바코드-e-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![샘플 바코드-e-13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>인터리브 2/5
![샘플 바코드-인터리브 2/5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![샘플 바코드-UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![샘플 바코드-UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>2D Symbologies
### <a name="qr-code"></a>QR 코드
![샘플 바코드-QR 코드](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>디지털 워터 마크
### <a name="gs1-dwcode"></a>GS1-DWCode

카메라 바코드 스캐너 응용 프로그램을 사용 하 여 아래 패키지 이미지를 스캔 하 여 GS1DWCode 작동 하는지 확인 합니다.  이미지는 UPCA 856107006854로 인코딩됩니다.  http://www.digimarc.comGS1DWCode 기능에 대 한 자세한 내용은을 참조 하세요.

![샘플 바코드-GS1DWCode](images/pos/Rice-Box-V7.jpg)

## <a name="see-also"></a>참조

### <a name="samples"></a>샘플

- [바코드 스캐너 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
