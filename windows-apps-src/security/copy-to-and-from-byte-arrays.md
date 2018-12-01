---
title: 바이트 배열에 및 바이트 배열에서 복사
description: 이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 바이트 배열 간에 복사하는 방법을 보여 줍니다.
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: b3ce63ca1780f9ed1ecd32f3ab1c029a1a92e1b5
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8343459"
---
# <a name="copy-to-and-from-byte-arrays"></a>바이트 배열에 및 바이트 배열에서 복사



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