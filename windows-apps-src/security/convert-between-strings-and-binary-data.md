---
title: "문자열과 이진 데이터 간 변환"
description: "이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 문자열과 이진 데이터 간에 변환하는 방법을 보여 줍니다."
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 667338ed3808f79b75230bc0c3ea00cc7e274d5a

---

# 문자열과 이진 데이터 간 변환


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

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


<!--HONumber=Jun16_HO4-->


