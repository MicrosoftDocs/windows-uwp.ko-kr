---
title: 데이터 인코드 및 디코드
description: 이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 base64 및 16진수 데이터를 인코딩 및 디코딩하는 방법을 보여 줍니다.
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 3c4e694dca3c84c7e94e513d8bb10a3f405bbc86
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658268"
---
# <a name="encode-and-decode-data"></a>데이터 인코드 및 디코드



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
