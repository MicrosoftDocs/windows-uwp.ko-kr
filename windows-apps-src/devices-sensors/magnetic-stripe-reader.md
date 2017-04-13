---
author: mukin
title: "자기 띠 판독기"
description: "이 문서에는 자기 띠 판독기 서비스 지점 장치 제품군에 대한 정보가 포함되어 있습니다."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a11fe7a63c0444ac986e7bfe0d50472249e5196e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="magnetic-stripe-reader"></a>자기 띠 판독기

응용 프로그램 개발자가 [자기 띠 판독기](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.magneticstripereader)에 액세스하여 신용/직불 카드, 고객 카드, 액세스 카드와 같이 자기 띠를 사용하는 카드에서 데이터를 검색할 수 있도록 합니다.

## <a name="requirements"></a>요구 사항
이 네임스페이스를 필요로 하는 응용 프로그램은 앱 패키지 매니페스트에 “pointOfService”([DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef))를 추가해야 합니다.

## <a name="device-support"></a>장치 지원
### <a name="usb"></a>USB
### <a name="supported-vendor-specific"></a>지원되는 특정 공급업체
Windows는 공급업체 ID와 제품 ID(VID/PID)를 기반으로 Magtek과 IDTech의 다음과 같은 자기 띠 판독기를 지원합니다.

| 제조업체 |     모델 |    부품 번호 |
|--------------|-----------|--------------|
| IDTech | SecureMag(VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag(VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe(VID:0801 PID:0011) |    210730xx |
| |    Dynamag(VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>특정 사용자 지정 공급업체
Windows는 추가 자기 띠 판독기 지원을 위해 측정 추가 공급업체 드라이버의 구현을 지원합니다. 자기 띠 판독기 제조업체에 사용 가능 여부를 확인하세요.

## <a name="examples"></a>예제
예제 구현을 위한 자기 띠 판독기 샘플을 참조하세요.
+    [자석 띠 판독기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
