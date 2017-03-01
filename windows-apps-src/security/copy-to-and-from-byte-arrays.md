---
title: "바이트 배열에 및 바이트 배열에서 복사"
description: "이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 바이트 배열 간에 복사하는 방법을 보여 줍니다."
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 9183ca237709100e16afd9ce2f8387ff464021ce
ms.lasthandoff: 02/07/2017

---

# <a name="copy-to-and-from-byte-arrays"></a>바이트 배열에 및 바이트 배열에서 복사


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 바이트 배열 간에 복사하는 방법을 보여 줍니다.

```cs
public void ByteArrayCopy()
{
    // Initialize a byte array.
    byte[] bytes = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Create a buffer from the byte array.
    IBuffer buffer = CryptographicBuffer.CreateFromByteArray(bytes);

    // Encode the buffer into a hexadecimal string (for display);
    string hex = CryptographicBuffer.EncodeToHexString(buffer);

    // Copy the buffer back into a new byte array.
    byte[] newByteArray;
    CryptographicBuffer.CopyToByteArray(buffer, out newByteArray);
}
```
