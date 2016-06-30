---
title: "난수 만들기"
description: "이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 암호화에 사용할 난수 또는 버퍼를 만드는 방법을 보여 줍니다."
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 1faa3477acd0cf1a5a2acc7c6c0a62910299254b

---

# 난수 만들기


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 암호화에 사용할 난수 또는 버퍼를 만드는 방법을 보여 줍니다.

```cs
public string GenerateRandomData()
{
    // Define the length, in bytes, of the buffer.
    uint length = 32;

    // Generate random data and copy it to a buffer.
    IBuffer buffer = CryptographicBuffer.GenerateRandom(length);

    // Encode the buffer to a hexadecimal string (for display).
    string randomHex = CryptographicBuffer.EncodeToHexString(buffer);

    return randomHex;
}

public uint GenerateRandomNumber()
{
    // Generate a random number.
    uint random = CryptographicBuffer.GenerateRandomNumber();
    return random;
}
```


<!--HONumber=Jun16_HO4-->


