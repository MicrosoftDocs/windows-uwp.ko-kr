---
title: 난수 만들기
description: 이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 암호화에 사용할 난수 또는 버퍼를 만드는 방법을 보여 줍니다.
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: a128535617c97e73b5a389db827fcf8c579b7f13
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6993334"
---
# <a name="create-random-numbers"></a>난수 만들기



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