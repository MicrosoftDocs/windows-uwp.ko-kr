---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: Bluetooth
description: 이 섹션에는 RFCOMM, GATT 및 저 에너지 (LE) 광고를 사용 하는 방법을 포함 하 여 Bluetooth를 유니버설 Windows 플랫폼 (UWP) 앱에 통합 하는 방법에 대 한 문서가 포함 되어 있습니다.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469568"
---
# <a name="bluetooth"></a>Bluetooth
이 섹션에는 유니버설 Windows 플랫폼 (UWP) 앱에 Bluetooth를 통합 하는 방법에 대 한 문서가 포함 되어 있습니다. 앱에서 구현 하도록 선택할 수 있는 두 가지 bluetooth 기술이 있습니다.

> [!Important]
> *Appxmanifest.xml*에서 "bluetooth" 기능을 선언 해야 합니다.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>클래식 Bluetooth (RFCOMM)
Bluetooth LE 이전에서 장치는 일반적으로이 프로토콜을 사용 하 여 Bluetooth를 사용 하 여 통신 합니다. 이 프로토콜은 간단 하 고 에너지를 절감 하지 않아도 장치-장치 통신에 유용 합니다. 코드 샘플을 포함 하 여이 프로토콜에 대 한 자세한 내용은 [BLUETOOTH RFCOMM](send-or-receive-files-with-rfcomm.md) 항목을 참조 하세요.

## <a name="bluetooth-low-energy-le"></a>Bluetooth 저 에너지 (LE)
Bluetooth 저 에너지 (LE)는 효율적인 에너지 사용 요구 사항이 있는 장치 간 검색 및 통신을 위한 프로토콜을 정의 하는 사양입니다. 코드 샘플을 포함 한 자세한 내용은 [Bluetooth 저 에너지](bluetooth-low-energy-overview.md) 항목을 참조 하세요.

## <a name="see-also"></a>참고 항목
- [Bluetooth 개발자 FAQ](bluetooth-dev-faq.md)