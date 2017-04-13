---
author: mukin
title: "현금 출납기"
description: "이 문서는 현금 출납기 서비스 지점 장치 제품군에 대한 정보를 포함하고 있습니다."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 
ms.openlocfilehash: 376272356cf720ddd9519f0077e771a1016abb1e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="cash-drawer"></a>현금 출납기

응용 프로그램 개발자가 [현금 출납기](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.cashdrawer)와 상호 작용할 수 있도록 합니다.

## <a name="requirements"></a>요구 사항
이 네임스페이스를 필요로 하는 응용 프로그램은 앱 패키지 매니페스트에 “pointOfService”([DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef))를 추가해야 합니다.

## <a name="device-support"></a>장치 지원
현금 출납기 장치의 기능에 따라 네트워크나 Bluetooth를 통해 현금 출납기에 직접 연결할 수 있습니다. 또한 네트워크 또는 Bluetooth 기능이 없는 현금 출납기의 경우 지원되는 POS 프린터나 Star Micronics DK-AirCash 액세서리를 통해 연결할 수 있습니다. 현재 USB나 직렬 포트를 통한 현금 출납기 연결은 지원되지 않습니다.

**참고:** DK-AirCash에 대한 자세한 내용은 Star Micronics에 문의하십시오.

### <a name="networkbluetooth"></a>네트워크/Bluetooth
| 제조업체 |    모델 |
|--------------|-----------|
| APG Cash Drawer |    NetPRO, BluePRO |

## <a name="examples"></a>예제
예제 구현을 위한 현금 출납기 샘플을 참조하세요.
+    [현금 출납기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
