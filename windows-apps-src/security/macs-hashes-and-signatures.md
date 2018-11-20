---
title: MAC, 해시, 서명
description: 이 문서에서는 UWP(유니버설 Windows 플랫폼)에서 MAC(메시지 인증 코드), 해시 및 서명을 사용하여 메시지 변조를 감지하는 방법에 대해 설명합니다.
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: b472a7209ddb9e67f7bb7c00e967295892568a29
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7282627"
---
# <a name="macs-hashes-and-signatures"></a>MAC, 해시, 서명




이 문서에서는 UWP(유니버설 Windows 플랫폼)에서 MAC(메시지 인증 코드), 해시 및 서명을 사용하여 메시지 변조를 감지하는 방법에 대해 설명합니다.

## <a name="message-authentication-codes-macs"></a>MAC(메시지 인증 코드)


암호화는 권한이 없는 개인이 메시지를 읽지 못하도록 할 수는 있지만 메시지를 변조하는 것을 막을 수는 없습니다. 변조한 결과가 무의미한 내용에 불과하더라도 변조된 메시지로 인해 손실이 발생할 수 있습니다. MAC(메시지 인증 코드)를 사용하면 메시지 변조를 막는 데 도움이 됩니다. 예를 들어, 다음 시나리오를 가정해 봅시다.

-   Bob과 Alice는 비밀 키를 공유하고 MAC 기능을 사용하는 데 동의합니다.
-   Bob은 메시지를 작성하고 메시지와 비밀 키를 MAC 기능에 입력하여 MAC 값을 검색합니다.
-   Bob은 \[암호화되지 않은\] 메시지와 MAC 값을 네트워크를 통해 Alice에게 보냅니다.
-   Alice는 비밀 키와 메시지를 MAC 기능의 입력으로 사용합니다. Alice는 생성된 MAC 값을 Bob이 보낸 MAC 값과 비교합니다. 두 MAC 값이 같으면 메시지는 전송 중에 변경되지 않은 것입니다.

Bob과 Alice의 대화를 엿들은 제3자 Eve는 사실상 메시지를 조작할 수 없습니다. Eve는 개인 키에 액세스할 수 없으므로 Alice에게 변조된 메시지가 그럴 듯해 보이게 하는 MAC 값을 만들 수 없습니다.

MAC 코드 만들기는 원래 메시지가 변경되지 않았고, 공유 비밀 키가 사용되었고, 해당 개인 키에 액세스할 수 있는 누군가가 메시지 해시에 서명했다는 점만 보장합니다.

[**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530)를 사용하여 사용 가능한 MAC 알고리즘을 열거하고 대칭 키를 생성할 수 있습니다. [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 클래스에서 정적 메서드를 사용하여 MAC 값을 만드는 필요한 암호화를 수행할 수 있습니다.

디지털 서명은 개인 키 MAC(메시지 인증 코드)와 동일한 공개 키입니다. MAC는 메시지 수신자가 해당 메시지가 전송 중에 변경되지 않았음을 확인하는 데 개인 키를 사용하는 반면, 서명은 개인/공개 키 쌍을 사용합니다.

다음 예제 코드에서는 [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) 클래스를 사용하여 HMAC(해시된 메시지 인증 코드)를 만드는 방법을 보여 줍니다.

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


암호화 해시 함수는 데이터의 임의 크기 블록을 가져가서 고정 크기 비트 문자열로 반환합니다. 해시 함수는 일반적으로 데이터에 서명할 때 사용됩니다. 대부분의 공개 키 서명 작업은 집중적인 계산이 필요하므로 주로 원본 메시지에 서명하는 것보다 메시지 해시에 서명(암호화)하는 것이 보다 효과적입니다. 다음 과정은 간소화되기는 했지만 일반적인 시나리오를 나타냅니다.

-   Bob과 Alice는 비밀 키를 공유하고 MAC 기능을 사용하는 데 동의합니다.
-   Bob은 메시지를 작성하고 메시지와 비밀 키를 MAC 기능에 입력하여 MAC 값을 검색합니다.
-   Bob은 \[암호화되지 않은\] 메시지와 MAC 값을 네트워크를 통해 Alice에게 보냅니다.
-   Alice는 비밀 키와 메시지를 MAC 기능의 입력으로 사용합니다. Alice는 생성된 MAC 값을 Bob이 보낸 MAC 값과 비교합니다. 두 MAC 값이 같으면 메시지는 전송 중에 변경되지 않은 것입니다.

Alice는 암호화되지 않은 메시지를 보냈고, 해시만 암호화되었다는 점에 유의하세요. 이 절차는 원래 메시지가 변경되지 않았고, Alice의 공개 키가 사용되었고, Alice의 개인 키에 액세스할 수 있는 누군가(아마도 Alice)가 메시지 해시에 서명했다는 점만 보장합니다.

[**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 클래스를 사용하여 사용 가능한 해시 알고리즘을 열거하고 [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 값을 만들 수 있습니다.

디지털 서명은 개인 키 MAC(메시지 인증 코드)와 동일한 공개 키입니다. MAC는 메시지 수신자가 해당 메시지가 전송 중에 변경되지 않았음을 확인하는 데 개인 키를 사용하는 반면, 서명은 개인/공개 키 쌍을 사용합니다.

[**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 개체를 사용하면 사용할 때마다 개체를 다시 만들지 않고도 여러 데이터를 반복적으로 해시할 수 있습니다. [**Append**](https://msdn.microsoft.com/library/windows/apps/br241499) 메서드는 해시할 새 데이터를 버퍼에 추가합니다. [**GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) 메서드는 데이터를 해시하고 개체를 또 사용하기 위해 초기화합니다. 이 메서드는 다음 예제에 나와 있습니다.

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


디지털 서명은 개인 키 MAC(메시지 인증 코드)와 동일한 공개 키입니다. MAC는 메시지 수신자가 해당 메시지가 전송 중에 변경되지 않았음을 확인하는 데 개인 키를 사용하는 반면, 서명은 개인/공개 키 쌍을 사용합니다.

대부분의 공개 키 서명 작업은 집중적인 계산이 필요하므로 주로 원본 메시지에 서명하는 것보다 메시지 해시에 서명(암호화)하는 것이 보다 효과적입니다. 발신자는 메시지 해시를 만들고, 해시에 서명하고, 서명과 (암호화되지 않은) 메시지 둘 다를 보냅니다. 수신자는 메시지의 해시를 계산하고, 서명을 암호 해독하고, 암호 해독한 서명을 해시 값과 비교합니다. 그 둘이 일치하면 수신자는 발신자가 보낸 메시지가 전송 중에 변경되지 않았음을 확신할 수 있습니다.

서명은 원래 메시지가 변경되지 않았고, 발신자의 공개 키가 사용되었고, 개인 키에 액세스할 수 있는 누군가가 메시지 해시에 서명했다는 점만 보장합니다.

[**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 개체를 사용하면 사용 가능한 서명 알고리즘을 열거하고 키 쌍을 생성하거나 가져올 수 있습니다. [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 클래스에서 정적 메서드를 사용하면 메시지에 서명하거나 서명을 확인할 수 있습니다.