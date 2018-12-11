---
title: 암호화 키
description: 이 문서에서는 표준 키 파생 함수를 사용하여 키를 파생하는 방법과 대칭 및 비대칭 키를 사용하여 콘텐츠를 암호화하는 방법을 보여 줍니다.
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 2b74eccd5f6138e5a9d670aa3a0a93239813cf4d
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918616"
---
# <a name="cryptographic-keys"></a>암호화 키




이 문서에서는 표준 키 파생 함수를 사용하여 키를 파생하는 방법과 대칭 및 비대칭 키를 사용하여 콘텐츠를 암호화하는 방법을 보여 줍니다. 

## <a name="symmetric-keys"></a>대칭 키


대칭 키 암호화는 비밀 키 암호화하고도 하며, 암호화에 사용된 키가 암호 해독에도 사용되어야 합니다. [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 클래스를 사용하면 대칭 알고리즘을 지정하고 키를 만들거나 가져올 수 있습니다. [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 클래스에 정적 메서드를 사용하면 알고리즘과 키를 통해 데이터를 암호화하고 암호 해독할 수 있습니다.

대칭 키 암호화는 주로 블록 암호화 및 블록 암호화 모드를 사용합니다. 블록 암호화는 고정된 크기의 블록에서 작동하는 대칭형 암호화 기능입니다. 암호화할 메시지가 블록 길이보다 길면 블록 암호화 모드를 사용해야 합니다. 블록 암호화 모드는 블록 암호화를 사용하여 만든 대칭 암호화 기능이며 일반 텍스트를 일련의 고정 크기 블록으로 암호화합니다. 앱에서는 다음 모드가 지원됩니다.

-   ECB(electronic codebook) 모드는 각 메시지 블록을 별도로 암호화합니다. 보안 암호화 모드로 간주되지 않습니다.
-   CBC(cipher block chaining) 모드는 이전 암호 텍스트 블록을 사용하여 현재 블록을 난독 처리합니다. 첫 번째 블록에 사용할 값을 결정해야 합니다. 이 값을 IV(Initialization Vector)라고 합니다.
-   CCM (counter with CBC-MAC) 모드는 CBC 블록 암호화 모드와 MAC(메시지 인증 코드)를 결합합니다.
-   GCM (Galois counter mode) 모드는 카운터 인증 모드를 Galois 인증 모드와 결합합니다.

CBC와 같은 일부 모드에서는 첫 번째 암호 텍스트 블록에 IV(Initialization Vector)를 사용해야 합니다. 다음은 일반적인 IV(Initialization Vector)입니다. [**CryptographicEngine.Encrypt**](https://msdn.microsoft.com/library/windows/apps/br241494)를 호출할 때 IV를 지정합니다. 대부분의 경우 IV를 동일한 키와 함께 다시 사용하지 않아야 합니다.

-   Fixed는 암호화할 모든 메시지에 동일한 IV를 사용합니다. 이 경우 정보가 누설되므로 사용하지 않는 것이 좋습니다.
-   Counter는 각 블록에 대해 IV를 늘립니다.
-   Random은 의사 난수 IV를 생성합니다. [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392)을 사용하여 IV를 만들 수 있습니다.
-   Nonce-Generated는 암호화할 각 메시지에 대한 고유한 번호를 사용합니다. 일반적으로 nonce는 수정된 메시지 또는 트랜잭션 식별자입니다. Nonce는 비밀로 유지되지 않으므로 동일한 키로 다시 사용하지 않아야 합니다.

대부분의 모드에서는 일반 텍스트의 길이가 블록 크기의 배수가 되어야 합니다. 이로 인해 대개는 개발자가 일반 텍스트를 패딩하여 적절한 길이로 만들어야 합니다.

블록 암호가 데이터의 고정 크기 블록을 암호화하는 데 반해, 스트림 암호는 일반 텍스트 비트를 의사 난수 비트 스트림과 결합하여 암호 텍스트를 생성하는 대칭형 암호화 기능입니다. OTF(출력 피드백 모드) 및 CTR(카운터 모드) 등, 일부 블록 암호 모드는 효과적으로 블록 암호를 스트림 암호로 변환합니다. 그러나 일반적으로 RC4 같은 실제 스트림 암호는 블록 암호 모드의 수행 능력보다 고속으로 작동합니다.

다음 예제에서는 [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 클래스를 사용하여 대칭 키를 만들고 이 키를 사용하여 데이터를 암호화 및 암호 해독하는 방법을 보여 줍니다.

## <a name="asymmetric-keys"></a>비대칭 키


비대칭 키 암호화는 공개 키 암호화라고도 하며, 공개 키와 개인 키를 사용하여 암호화와 암호 해독을 수행합니다. 두 키는 서로 다르지만 수학적으로 관련이 있습니다. 일반적으로 개인 키는 비밀 상태로 유지되고 데이터의 암호를 해독하는 데 사용되는 반면, 공개 키는 관련 당사자에게 배포되고 데이터를 암호화하는 데 사용됩니다. 또한 비대칭형 암호화는 데이터에 서명하는 데도 유용합니다.

비대칭형 암호화는 대칭형 암호화보다 느리므로 많은 양의 데이터를 직접 암호화하는 데는 거의 사용되지 않습니다. 대신 일반적으로 다음과 같은 방식으로 키를 암호화하는 데 사용됩니다.

-   Alice는 Bob에게 암호화된 메시지만 보내도록 요청합니다.
-   Alice는 개인/공개 키 쌍을 만들어 개인 키 암호는 보관하고 공개 키를 게시합니다.
-   Bob이 Alice에게 보낼 메시지가 있습니다.
-   Bob은 대칭 키를 만듭니다.
-   Bob은 새 대칭 키를 사용하여 Alice에게 보낼 메시지를 암호화합니다.
-   Bob은 Alice의 공개 키를 사용하여 대칭 키를 암호화합니다.
-   Bob은 암호화된 메시지와 암호화된 대칭 키를 Alice에게 보냅니다(봉함).
-   Alice는 개인/공개 쌍에 있는 자신의 개인 키를 사용하여 Bob의 대칭 키 암호를 해독합니다.
-   Alice는 Bob의 대칭 키를 사용하여 메시지 암호를 해독합니다.

[**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 개체를 사용하면 비대칭 알고리즘 또는 서명 알고리즘을 지정하고, 사용 후 삭제되는 키 쌍을 만들거나 가져오고, 또는 키 쌍의 공개 키 부분을 가져올 수 있습니다.

## <a name="deriving-keys"></a>키 파생


공유 암호에서 추가 키를 파생해야 하는 경우가 많습니다. [**KeyDerivationAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241518) 클래스와 [**KeyDerivationParameters**](https://msdn.microsoft.com/library/windows/apps/br241524) 클래스의 다음 특수 메서드 중 하나를 사용하여 키를 파생할 수 있습니다.

| 개체                                                                            | 설명                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](https://msdn.microsoft.com/library/windows/apps/br241525)    | PBKDF2(암호 기반 키 파생 함수 2)에서 사용할 KeyDerivationParameters 개체를 만듭니다.                                 |
| [**BuildForSP800108**](https://msdn.microsoft.com/library/windows/apps/br241526)  | 카운터 모드 HMAC(해시 기반 메시지 인증 코드) 키 파생 함수에서 사용할 KeyDerivationParameters 개체를 만듭니다. |
| [**BuildForSP80056a**](https://msdn.microsoft.com/library/windows/apps/br241527)  | SP800-56A 키 파생 함수에서 사용할 KeyDerivationParameters 개체를 만듭니다.                                                 |

 
