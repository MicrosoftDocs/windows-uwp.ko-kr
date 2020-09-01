---
title: 암호화 키
description: 이 문서에서는 표준 키 파생 함수를 사용 하 여 키를 파생 하는 방법과 대칭 및 비대칭 키를 사용 하 여 콘텐츠를 암호화 하는 방법을 보여 줍니다.
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: a781ae79b54223916c9de379dcdb4f8cb3b8e4b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167257"
---
# <a name="cryptographic-keys"></a>암호화 키




이 문서에서는 표준 키 파생 함수를 사용 하 여 키를 파생 하는 방법과 대칭 및 비대칭 키를 사용 하 여 콘텐츠를 암호화 하는 방법을 보여 줍니다. 

## <a name="symmetric-keys"></a>대칭 키


비밀 키 암호화 라고도 하는 대칭 키 암호화를 사용 하려면 암호화에 사용 되는 키를 암호 해독에도 사용 해야 합니다. [**SymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.SymmetricKeyAlgorithmProvider) 클래스를 사용 하 여 대칭 알고리즘을 지정 하 고 키를 만들거나 가져올 수 있습니다. [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) 클래스에서 정적 메서드를 사용 하 여 알고리즘과 키를 사용 하 여 데이터를 암호화 하 고 암호 해독할 수 있습니다.

대칭 키 암호화는 일반적으로 블록 암호화 및 블록 암호화 모드를 사용 합니다. 블록 암호화는 고정 크기 블록에서 작동 하는 대칭 암호화 함수입니다. 암호화 하려는 메시지가 블록 길이 보다 길면 블록 암호화 모드를 사용 해야 합니다. 블록 암호화 모드는 블록 암호화를 사용 하 여 작성 된 대칭 암호화 함수입니다. 일련의 고정 된 크기 블록으로 일반 텍스트를 암호화 합니다. 앱에 대해 지원 되는 모드는 다음과 같습니다.

-   ECB (electronic codebook) 모드는 메시지의 각 블록을 개별적으로 암호화 합니다. 이는 보안 암호화 모드로 간주 되지 않습니다.
-   CBC (암호화 블록 체인) 모드는 이전 암호 텍스트 블록을 사용 하 여 현재 블록을 난독 처리 합니다. 첫 번째 블록에 사용할 값을 결정 해야 합니다. 이 값을 IV (initialization vector) 라고 합니다.
-   CCM (CBC-MAC을 사용 하는 카운터) 모드는 CBC 블록 암호화 모드를 MAC (메시지 인증 코드)와 결합 합니다.
-   GCM (Galois 카운터 모드) 모드는 카운터 암호화 모드와 Gis 인증 모드를 결합 합니다.

CBC와 같은 일부 모드에서는 첫 번째 암호 텍스트 블록에 IV (초기화 벡터)를 사용 해야 합니다. 일반적인 초기화 벡터는 다음과 같습니다. [**CryptographicEngine**](/uwp/api/windows.security.cryptography.core.cryptographicengine.encrypt)를 호출할 때 IV를 지정 합니다. 대부분의 경우 IV를 동일한 키로 다시 사용 하지 않는 것이 중요 합니다.

-   Fixed는 암호화 될 모든 메시지에 대해 동일한 IV를 사용 합니다. 이러한 정보는 누출 되며 사용 하지 않는 것이 좋습니다.
-   카운터는 각 블록에 대 한 IV를 증가 시킵니다.
-   임의는 의사 (pseudo) IV를 만듭니다. CryptographicBuffer를 사용 하 여 IV를 만들 수 있습니다 [**.**](/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom)
-   Nonce에서 생성 되는 각 메시지에 대해 고유 번호를 사용 하 여 암호화 합니다. 일반적으로 nonce는 수정 된 메시지 또는 트랜잭션 식별자입니다. Nonce는 비밀로 유지할 필요가 없지만 동일한 키에서 다시 사용 하면 안 됩니다.

대부분의 모드에서는 일반 텍스트의 길이가 블록 크기의 정확한 배수 여야 합니다. 이 경우 일반적으로 적절 한 길이를 얻기 위해 일반 텍스트를 채워야 합니다.

블록 암호화는 고정 크기의 데이터 블록을 암호화 하지만, 스트림 암호화는 일반 텍스트 비트를 키 스트림 이라고 하는 의사 (pseudo) 비트 스트림과 결합 하 여 암호 텍스트를 생성 하는 대칭 암호화 함수입니다. 출력 피드백 모드 (이상) 및 카운터 모드 (CTR)와 같은 일부 블록 암호화 모드는 블록 암호화를 스트림 암호화로 효과적으로 전환 합니다. 그러나 RC4와 같은 실제 스트림 암호화는 일반적으로 블록 암호화 모드에서 달성할 수 있는 것 보다 더 높은 속도로 작동 합니다.

다음 예제에서는 [**SymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.SymmetricKeyAlgorithmProvider) 클래스를 사용 하 여 대칭 키를 만들고이 키를 사용 하 여 데이터를 암호화 및 암호 해독 하는 방법을 보여 줍니다.

## <a name="asymmetric-keys"></a>비대칭 키


공개 키 암호화 라고도 하는 비대칭 키 암호화는 공개 키와 개인 키를 사용 하 여 암호화 및 암호 해독을 수행 합니다. 키는 다르지만 수학적으로 관련 되어 있습니다. 일반적으로 개인 키는 비밀로 유지 되 고 공개 키는 관심이 있는 파티에 배포 되 고 데이터를 암호화 하는 데 사용 되는 동안 데이터 암호를 해독 하는 데 사용 됩니다. 비대칭 암호화는 데이터에 서명 하는 데도 유용 합니다.

비대칭 암호화는 대칭 암호화 보다 훨씬 느리지만 많은 양의 데이터를 직접 암호화 하는 데는 거의 사용 되지 않습니다. 대신 일반적으로 키를 암호화 하는 다음과 같은 방식으로 사용 됩니다.

-   Alice는 Bob이 암호화 된 메시지만 전송 하도록 요구 합니다.
-   Alice는 개인/공개 키 쌍을 만들고 개인 키 암호를 유지 하 고 자신의 공개 키를 게시 합니다.
-   Bob은 Alice에 게 보내려는 메시지를 포함 합니다.
-   Bob은 대칭 키를 만듭니다.
-   Bob은 새 대칭 키를 사용 하 여 Alice에 게 메시지를 암호화 합니다.
-   Bob은 Alice의 공개 키를 사용 하 여 대칭 키를 암호화 합니다.
-   Bob은 암호화 된 메시지와 암호화 된 대칭 키를 Alice (엔벌로프된)로 보냅니다.
-   Alice는 개인/공개 쌍의 개인 키를 사용 하 여 Bob의 대칭 키를 해독 합니다.
-   Alice는 Bob의 대칭 키를 사용 하 여 메시지를 해독 합니다.

[**AsymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.AsymmetricKeyAlgorithmProvider) 개체를 사용 하 여 비대칭 알고리즘 또는 서명 알고리즘을 지정 하 고, 임시 키 쌍을 만들거나 가져오거나, 키 쌍의 공개 키 부분을 가져올 수 있습니다.

## <a name="deriving-keys"></a>키 파생


공유 암호에서 추가 키를 파생 해야 하는 경우가 종종 있습니다. [**KeyDerivationAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.KeyDerivationAlgorithmProvider) 클래스 및 [**KeyDerivationParameters**](/uwp/api/Windows.Security.Cryptography.Core.KeyDerivationParameters) 클래스의 다음 특수 메서드 중 하나를 사용 하 여 키를 파생 시킬 수 있습니다.

| 개체                                                                            | Description                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforpbkdf2)    | 암호 기반 키 파생 함수 2 (PBKDF2)에 사용할 KeyDerivationParameters 개체를 만듭니다.                                 |
| [**BuildForSP800108**](/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforsp800108)  | 카운터 모드, HMAC (해시 기반 메시지 인증 코드) 키 파생 함수에서 사용할 KeyDerivationParameters 개체를 만듭니다. |
| [**BuildForSP80056a**](/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforsp80056a)  | SP800-56A 키 파생 함수에서 사용할 KeyDerivationParameters 개체를 만듭니다.                                                 |

 