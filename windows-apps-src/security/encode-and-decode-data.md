---
title: 데이터 인코드 및 디코드
description: 이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 base64 및 16진수 데이터를 인코딩 및 디코딩하는 방법을 보여 줍니다.
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
author: awkoren
---

# 데이터 인코드 및 디코드


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 base64 및 16진수 데이터를 인코딩 및 디코딩하는 방법을 보여 줍니다.

```cs
public void EncodeDecodeBase64()
{
    // Define a Base64 encoded string.
    String strBase64 = "uiwyeroiugfyqcajkds897945234==";

    // Decoded the string from Base64 to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromBase64String(strBase64);

    // Encode the buffer back into a Base64 string.
    String strBase64New = CryptographicBuffer.EncodeToBase64String(buffer);
}

public void EncodeDecodeHex()
{
    // Define a hexadecimal string.
    String strHex = "30310AFF";

    // Decode a hexadecimal string to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromHexString(strHex);

    // Encode the buffer back into a hexadecimal string.
    String strHexNew = CryptographicBuffer.EncodeToHexString(buffer);
}
```


<!--HONumber=May16_HO2-->


