---
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: 서식 지정을 사용하는 Epson ESC/POS
description: 'ESC/POS 명령 언어를 사용하여 서비스 지점 프린터에 대한 텍스트 서식(예: 굵게, 2배 크기)을 지정하는 방법을 알아봅니다.'
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 11467a45021da7898c2b617e3b1b01312c795c4c
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8710643"
---
# <a name="epson-escpos-with-formatting"></a>서식 지정을 사용하는 Epson ESC/POS


**중요 API**

-   [**PointofService 프린터**](https://msdn.microsoft.com/library/windows/apps/Mt426652)
-   [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

ESC/POS 명령 언어를 사용하여 서비스 지점 프린터에 대한 텍스트 서식(예: 굵게, 2배 크기)을 지정하는 방법을 알아봅니다.

## <a name="escpos-usage"></a>ESC/POS 사용

Windows 서비스 지점에서는 몇 가지 Epson TM 시리즈 프린터를 비롯하여 다양한 프린터를 사용할 수 있습니다. 지원되는 프린터의 전체 목록에 대해서는 [서비스 지점 프린터](https://msdn.microsoft.com/library/windows/apps/Mt426652) 페이지를 참조하세요. Windows에서는 프린터와 통신하기 위해 효율적이며 기능적인 명령을 제공하는 ESC/POS Printer Control Language를 통해 인쇄를 지원합니다.

ESC/POS는 광범위한 POS 프린터 시스템 전체에서 사용되는 Epson에서 만든 명령 시스템으로, 범용 적용 가능성을 제공하여 호환되지 않는 명령 집합을 방지하는 것을 목표로 합니다. 대부분의 최신 프린터에서는 ESC/POS를 지원합니다.

모든 명령은 ESC 문자(ASCII 27, 16진수 1B) 또는 GS(ASCII 29, 16진수 1D)로 시작한 후 이미지를 지정하는 다른 문자를 사용합니다. 일반 텍스트는 프린터로 전송되며, 줄 바꿈으로 구분됩니다.

[**Windows PointOfService API**](https://msdn.microsoft.com/library/windows/apps/Dn298071)는 **Print()** 또는 **PrintLine()** 메서드를 통해 해당 기능의 상당 부분을 제공합니다. 그러나 특정 서식 지정을 가져오거나 특정 명령을 보내려면 문자열로 구성되며 프린터로 전송되는 ESC/POS 명령을 사용해야 합니다.

## <a name="example-using-bold-and-double-size-characters"></a>굵게 및 2배 크기 문자를 사용하는 예

아래 예제에서는 ESC/POS 명령을 사용하여 굵게 및 2배 크기의 문자를 인쇄하는 방법을 보여 줍니다. 각 명령은 문자열로 구성된 다음 printJob 호출에 삽입됩니다.

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

사용 가능한 명령을 포함하여 ESC/POS에 대한 자세한 내용은 [Epson ESC/POS FAQ](http://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf)를 확인하세요. [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) 및 사용 가능한 모든 기능에 대해서는 MSDN의 [서비스 지점 프린터](https://msdn.microsoft.com/library/windows/apps/Mt426652)를 참조하세요.
