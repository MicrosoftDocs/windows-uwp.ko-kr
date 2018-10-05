---
title: 문자열과 이진 데이터 간 변환
description: 이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 문자열과 이진 데이터 간에 변환하는 방법을 보여 줍니다.
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: b3c3a3f6f831186302fc32b1f510919da40c57cc
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4393923"
---
# <a name="convert-between-strings-and-binary-data"></a>문자열과 이진 데이터 간 변환



이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 문자열과 이진 데이터 간에 변환하는 방법을 보여 줍니다.

```cs
public void ConvertData()
{
    // Create a string to convert.
    String strIn = "Input String";

    // Convert the string to UTF16BE binary data.
    IBuffer buffUTF16BE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16BE);

    // Convert the string to UTF16LE binary data.
    IBuffer buffUTF16LE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16LE);

    // Convert the string to UTF8 binary data.
    IBuffer buffUTF8 = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf8);
}
```