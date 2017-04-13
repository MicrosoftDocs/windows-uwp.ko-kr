---
author: mukin
title: "POS 프린터"
description: "이 문서는 서비스 지점 프린터 장치 제품군에 대한 정보를 포함하고 있습니다."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d8340af651157bb6fae82785812f259c16d0a6c0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pos-printer"></a>POS 프린터

응용 프로그램 개발자가 Epson ESC/POS 프린터 제어 언어를 사용하여 네트워크와 Bluetooth로 연결된 [영수증 프린터](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.posprinter)로 인쇄할 수 있도록 합니다.

## <a name="requirements"></a>요구 사항
이 네임스페이스를 필요로 하는 응용 프로그램은 앱 패키지 매니페스트에 “pointOfService”([DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef))를 추가해야 합니다.

## <a name="device-support"></a>장치 지원
Windows는 Epson ESC/POS 프린터 제어 언어를 네트워크와 Bluetooth로 연결된 영수증 프린터로 인쇄하는 기능을 지원합니다. ESC/POS에 대한 자세한 내용은 [서식 지정을 사용하는 Epson ESC/POS](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting)를 참조하세요.

API에서 제공하는 클래스, 열거형, 인터페이스는 영수증 프린터, 전표 프린터, 업무 일지 프린터를 지원하지만 드라이버 인터페이스는 영수증 프린터만 지원합니다. 지금 전표 프린터나 업무 일지 프린터를 사용하려고 시도하면 구현되지 않았다는 상태가 반환됩니다.

현재 지원은 아래 표에 나열된 네트워크 및 Bluetooth 장치 모델로 제한됩니다. USB 연결 프린터는 현재 지원되지 않습니다. 나중에 다시 확인하여 추가 지원이 추가되었는지 살펴보세요.

### <a name="stationary-pos-printers-network-bluetooth"></a>고정 POS 프린터(네트워크, Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>모바일 POS 프린터(Bluetooth)
| 제조업체 |    모델 |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |

## <a name="examples"></a>예제
예제 구현을 위한 POS 프린터 샘플을 참조하세요.
+ [POS 프린터 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
