---
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: 서식 지정을 사용하는 Epson ESC/POS
description: ESC/POS 명령 언어를 사용 하 여 서비스 지점 프린터에 대해 굵게 및 이중 크기 문자와 같은 텍스트의 서식을 지정 하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9fa9a0746e65fe3cbb42ce140a62a3129023d011
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165457"
---
# <a name="epson-escpos-with-formatting"></a>서식 지정을 사용하는 Epson ESC/POS


**중요 API**

-   [**PointofService 프린터**](/uwp/api/Windows.Devices.PointOfService)
-   [**Windows.Devices.PointOfService**](/uwp/api/Windows.Devices.PointOfService)

ESC/POS 명령 언어를 사용 하 여 서비스 지점 프린터에 대해 굵게 및 이중 크기 문자와 같은 텍스트의 서식을 지정 하는 방법을 알아봅니다.

## <a name="escpos-usage"></a>ESC/POS 사용

Windows 서비스 지점은 여러 Epson TM 시리즈 프린터를 비롯 한 다양 한 프린터를 사용 합니다. 지원 되는 프린터의 전체 목록은 [Pointofservice 프린터](/uwp/api/Windows.Devices.PointOfService) 페이지를 참조 하세요. Windows에서는 프린터와 통신 하기 위한 효율적이 고 기능적인 명령을 제공 하는 ESC/POS 프린터 컨트롤 언어를 통한 인쇄를 지원 합니다.

ESC/POS는 범용 적용 가능성을 제공 하 여 호환 되지 않는 명령 집합을 방지 하기 위해 광범위 한 POS 프린터 시스템에서 사용 되는 Epson에서 만든 명령 시스템입니다. 대부분의 최신 프린터는 ESC/POS를 지원 합니다.

모든 명령은 ESC 문자 (ASCII 27, 16 진수 1B) 또는 GS (ASCII 29, 16 진수 1D)로 시작 하 여 명령을 지정 하는 다른 문자를 사용 합니다. 일반 텍스트는 줄 바꿈으로 구분 된 프린터로만 전송 됩니다.

[**Windows PointOfService API**](/uwp/api/Windows.Devices.PointOfService) 는 **Print ()** 또는 **printline ()** 메서드를 통해 이러한 기능을 대부분 제공 합니다. 그러나 특정 형식을 가져오거나 특정 명령을 보내려면 문자열로 작성 되 고 프린터로 전송 된 ESC/POS 명령을 사용 해야 합니다.

## <a name="example-using-bold-and-double-size-characters"></a>굵은 글꼴 및 이중 크기 문자 사용 예

아래 예제에서는 ESC/POS 명령을 사용 하 여 굵게 및 이중 크기의 문자로 인쇄 하는 방법을 보여 줍니다. 각 명령은 문자열로 작성 된 다음 printJob 호출에 삽입 됩니다.

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

사용할 수 있는 명령을 포함 하 여 ESC/POS에 대 한 자세한 내용은 [EPSON ESC/POS FAQ](https://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf)를 확인 하세요. [**Windows. pointofservice**](/uwp/api/Windows.Devices.PointOfService) 및 모든 사용 가능한 기능에 대 한 자세한 내용은 MSDN의 [Pointofservice 프린터](/uwp/api/Windows.Devices.PointOfService) 를 참조 하십시오.