---
title: 난수 만들기
description: 이 예제 코드는 UWP(유니버설 Windows 플랫폼) 앱에서 암호화에 사용할 난수 또는 버퍼를 만드는 방법을 보여 줍니다.
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da60dd982ab378bc71ecbc1f485ca3127cefc9dd
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689229"
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