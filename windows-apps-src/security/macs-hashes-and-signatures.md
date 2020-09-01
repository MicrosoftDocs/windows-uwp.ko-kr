---
title: MAC, 해시, 서명
description: 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에서 Mac (메시지 인증 코드), 해시 및 서명을 사용 하 여 메시지 변조를 검색 하는 방법을 설명 합니다.
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 7932636679a09834b982e2c320309b90c759c365
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167147"
---
# <a name="macs-hashes-and-signatures"></a>MAC, 해시, 서명




이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에서 Mac (메시지 인증 코드), 해시 및 서명을 사용 하 여 메시지 변조를 검색 하는 방법을 설명 합니다.

## <a name="message-authentication-codes-macs"></a>Mac (메시지 인증 코드)


암호화는 권한이 없는 개인이 메시지를 읽지 못하게 하는 데 도움이 되지만, 해당 개인이 메시지를 변조 하는 것을 방지 하지는 않습니다. 변경 된 메시지는 변경 내용이 있지만 아무런 영향도 주지 않는 경우에도 실제 비용을 절감할 수 있습니다. 메시지 인증 코드 (MAC)는 메시지 변조를 방지 하는 데 도움이 됩니다. 예를 들어 다음 시나리오를 고려할 수 있습니다.

-   Bob과 Alice는 비밀 키를 공유 하 고 사용할 MAC 기능에 동의 합니다.
-   Bob은 메시지를 만든 다음 mac 함수에 메시지 및 비밀 키를 입력 하 여 MAC 값을 검색 합니다.
-   Bob은 \[ 네트워크를 통해 암호화 되지 않은 \] 메시지와 MAC 값을 Alice에 게 보냅니다.
-   Alice는 비밀 키와 메시지를 MAC 함수의 입력으로 사용 합니다. 그는 생성 된 MAC 값을 Bob이 보낸 MAC 값과 비교 합니다. 동일한 경우 메시지가 전송 중에 변경 되지 않았습니다.

Bob과 Alice 간의 대화에 대 한 타사 도청은 메시지를 효율적으로 조작할 수 없습니다. 전날은 개인 키에 액세스할 수 없으며 변조 된 메시지가 합법적인 메시지를 Alice에 게 표시 하는 MAC 값을 만들 수 없습니다.

메시지 인증 코드를 만들면 원래 메시지가 변경 되지 않은 것으로 확인 되 고, 공유 암호 키를 사용 하 여 메시지 해시가 해당 개인 키에 대 한 액세스 권한이 있는 사용자에 의해 서명 되었습니다.

[**MacAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.MacAlgorithmProvider) 를 사용 하 여 사용 가능한 MAC 알고리즘을 열거 하 고 대칭 키를 생성할 수 있습니다. [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) 클래스에서 정적 메서드를 사용 하 여 MAC 값을 만드는 데 필요한 암호화를 수행할 수 있습니다.

디지털 서명은 개인 키 Mac (메시지 인증 코드)에 해당 하는 공개 키입니다. Mac에서는 개인 키를 사용 하 여 메시지 수신자가 전송 중에 메시지가 변경 되지 않았음을 확인할 수 있도록 합니다. 서명에는 개인/공개 키 쌍이 사용 됩니다.

이 예제 코드에서는 [**MacAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.MacAlgorithmProvider) 클래스를 사용 하 여 HMAC (해시 된 메시지 인증 코드)를 만드는 방법을 보여 줍니다.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Storage.Streams;

namespace SampleMacAlgorithmProvider
{
    sealed partial class MacAlgProviderApp : Application
    {
        public MacAlgProviderApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Initialize the hashing process.
            String strMsg = "This is a message to be authenticated";
            String strAlgName = MacAlgorithmNames.HmacSha384;
            IBuffer buffMsg;
            CryptographicKey hmacKey;
            IBuffer buffHMAC;

            // Create a hashed message authentication code (HMAC)
            this.CreateHMAC(
                strMsg,
                strAlgName,
                out buffMsg,
                out hmacKey,
                out buffHMAC);

            // Verify the HMAC.
            this.VerifyHMAC(
                buffMsg,
                hmacKey,
                buffHMAC);
        }

        void CreateHMAC(
            String strMsg,
            String strAlgName,
            out IBuffer buffMsg,
            out CryptographicKey hmacKey,
            out IBuffer buffHMAC)
        {
            // Create a MacAlgorithmProvider object for the specified algorithm.
            MacAlgorithmProvider objMacProv = MacAlgorithmProvider.OpenAlgorithm(strAlgName);

            // Demonstrate how to retrieve the name of the algorithm used.
            String strNameUsed = objMacProv.AlgorithmName;

            // Create a buffer that contains the message to be signed.
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
            buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a key to be signed with the message.
            IBuffer buffKeyMaterial = CryptographicBuffer.GenerateRandom(objMacProv.MacLength);
            hmacKey = objMacProv.CreateKey(buffKeyMaterial);

            // Sign the key and message together.
            buffHMAC = CryptographicEngine.Sign(hmacKey, buffMsg);

            // Verify that the HMAC length is correct for the selected algorithm
            if (buffHMAC.Length != objMacProv.MacLength)
            {
                throw new Exception("Error computing digest");
            }
         }

        public void VerifyHMAC(
            IBuffer buffMsg,
            CryptographicKey hmacKey,
            IBuffer buffHMAC)
        {
            // The input key must be securely shared between the sender of the HMAC and 
            // the recipient. The recipient uses the CryptographicEngine.VerifySignature() 
            // method as follows to verify that the message has not been altered in transit.
            Boolean IsAuthenticated = CryptographicEngine.VerifySignature(hmacKey, buffMsg, buffHMAC);
            if (!IsAuthenticated)
            {
                throw new Exception("The message cannot be verified.");
            }
        }
    }
}
```

## <a name="hashes"></a>해시


암호화 해시 함수는 임의의 long 데이터 블록을 사용 하 고 고정 크기 비트 문자열을 반환 합니다. 해시 함수는 일반적으로 데이터에 서명할 때 사용 됩니다. 대부분의 공개 키 서명 작업은 계산을 많이 수행 하므로 원본 메시지에 서명 하는 것 보다 일반적으로 메시지 해시를 서명 (암호화) 하는 것이 더 효율적입니다. 다음 절차는 일반적이 고 간소화 된 시나리오를 나타냅니다.

-   Bob과 Alice는 비밀 키를 공유 하 고 사용할 MAC 기능에 동의 합니다.
-   Bob은 메시지를 만든 다음 mac 함수에 메시지 및 비밀 키를 입력 하 여 MAC 값을 검색 합니다.
-   Bob은 \[ 네트워크를 통해 암호화 되지 않은 \] 메시지와 MAC 값을 Alice에 게 보냅니다.
-   Alice는 비밀 키와 메시지를 MAC 함수의 입력으로 사용 합니다. 그는 생성 된 MAC 값을 Bob이 보낸 MAC 값과 비교 합니다. 동일한 경우 메시지가 전송 중에 변경 되지 않았습니다.

Alice는 암호화 되지 않은 메시지를 보냈습니다. 해시만 암호화 되었습니다. 이 절차는 원래 메시지가 변경 되지 않았음을 보장 하 고 alice의 공개 키를 사용 하 여 메시지 해시가 alice의 개인 키에 대 한 액세스 권한이 있는 누군가가 서명 되었음을 보장 합니다.

[**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) 클래스를 사용 하 여 사용 가능한 해시 알고리즘을 열거 하 고 [**CryptographicHash**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash) 값을 만들 수 있습니다.

디지털 서명은 개인 키 Mac (메시지 인증 코드)에 해당 하는 공개 키입니다. Mac에서는 개인 키를 사용 하 여 메시지 수신자가 전송 중에 메시지가 변경 되지 않았는지 확인할 수 있도록 합니다. 서명에는 개인/공개 키 쌍이 사용 됩니다.

[**CryptographicHash**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash) 개체를 사용 하 여 각 사용에 대 한 개체를 다시 만들지 않고도 다른 데이터를 반복적으로 해시할 수 있습니다. [**Append**](/uwp/api/windows.security.cryptography.core.cryptographichash.append) 메서드는 해시 될 버퍼에 새 데이터를 추가 합니다. [**Getvalueandreset**](/uwp/api/windows.security.cryptography.core.cryptographichash.getvalueandreset) 메서드는 데이터를 해시 하 고 다른 사용을 위해 개체를 다시 설정 합니다. 이는 다음 예제에 나와 있습니다.

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the hashing algorithm to use.
    String strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    String strMsg1 = "This is message 1.";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    String strMsg2 = "This is message 2.";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Hash message 3.
    String strMsg3 = "This is message 3.";
    IBuffer buffMsg3 = CryptographicBuffer.ConvertStringToBinary(strMsg3, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg3);
    IBuffer buffHash3 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    String strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    String strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
    String strHash3 = CryptographicBuffer.EncodeToBase64String(buffHash3);
}

```

## <a name="digital-signatures"></a>디지털 서명


디지털 서명은 개인 키 Mac (메시지 인증 코드)에 해당 하는 공개 키입니다. Mac에서는 개인 키를 사용 하 여 메시지 수신자가 전송 중에 메시지가 변경 되지 않았는지 확인할 수 있도록 합니다. 서명에는 개인/공개 키 쌍이 사용 됩니다.

그러나 대부분의 공개 키 서명 작업은 계산 집약적 이므로 일반적으로 원본 메시지에 서명 하는 것 보다 메시지 해시를 서명 (암호화) 하는 것이 더 효율적입니다. 발신자는 메시지 해시를 만들어 서명한 다음 서명과 (암호화 되지 않은) 메시지를 모두 보냅니다. 수신자는 메시지에 대 한 해시를 계산 하 고, 서명을 해독 하 고, 해독 된 서명을 해시 값과 비교 합니다. 이러한 항목이 일치 하는 경우 수신자는 메시지를 실제로 보낸 사람이 보낸 것과 전송 중에 변경 되지 않았음을 확신할 수 있습니다.

서명은 원래 메시지를 변경 하지 않고 보낸 사람의 공개 키를 사용 하 여 메시지 해시가 개인 키에 대 한 액세스 권한이 있는 사용자에 의해 서명 되었는지 확인 합니다.

[**AsymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.AsymmetricKeyAlgorithmProvider) 개체를 사용 하 여 사용 가능한 서명 알고리즘을 열거 하 고 키 쌍을 생성 하거나 가져올 수 있습니다. [**CryptographicHash**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash) 클래스에서 정적 메서드를 사용 하 여 메시지에 서명 하거나 서명을 확인할 수 있습니다.