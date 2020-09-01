---
title: 데이터 보호
description: 이 문서에서는 Windows.Security.Cryptography.DataProtection 네임스페이스의 DataProtectionProvider 클래스를 사용하여 UWP 앱에서 디지털 데이터를 암호화 및 해독하는 방법을 설명합니다.
ms.assetid: 9EE3CC45-5C44-4196-BD8B-1D64EFC5C509
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 67419be613c3bd3da4aa9b2b2cb3d265ae5b7c4f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167227"
---
# <a name="data-protection"></a>데이터 보호



이 문서에서는 DataProtectionProvider 네임 스페이스에서 [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) 클래스를 사용 하 여 UWP 앱에서 디지털 데이터를 암호화 하 고 해독 하는 방법을 설명 [**합니다.**](/uwp/api/Windows.Security.Cryptography.DataProtection)

데이터 보호 Api는 다음과 같은 여러 가지 방법으로 사용할 수 있습니다.

-   AD 그룹 등의 AD (Active Directory) 보안 주체로 데이터를 보호 하기 위한 것입니다. 그룹의 모든 멤버는 데이터의 암호를 해독할 수 있습니다.
-   X.509 인증서에 포함 된 공개 키에 대 한 데이터를 보호 하기 위한 것입니다. 개인 키의 소유자는 데이터의 암호를 해독할 수 있습니다.
-   대칭 키를 사용 하 여 데이터를 보호 합니다. 예를 들어 Live ID와 같은 비 AD 보안 주체로 데이터를 보호 하는 데 사용할 수 있습니다.
-   웹 사이트에 로그온 하는 동안 사용 되는 자격 증명 (암호)에 대 한 데이터를 보호 합니다.

데이터를 보호 하려면 [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) 개체를 만들 때 [**ProtectAsync**](/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectasync) 또는 [**ProtectStreamAsync**](/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectstreamasync)를 호출 하기 전에 보호 설명자를 지정 해야 합니다. 다음 예제에서는 가능한 샘플 보호 설명자를 보여 줍니다.

## <a name="protecting-static-data"></a>정적 데이터 보호


다음 예제에서는 [**ProtectAsync**](/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectasync) 및 [**UnprotectAsync**](/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.unprotectasync) 메서드를 사용 하 여 정적 데이터를 현재 사용자의 SID로 비동기적으로 보호 하는 방법을 보여 줍니다.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.DataProtection;
using Windows.Storage.Streams;
using System.Threading.Tasks;

namespace SampleProtectAsync
{
    sealed partial class StaticDataProtectionApp : Application
    {
        public StaticDataProtectionApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Protect data asynchronously.
            this.Protect();
        }

        public async void Protect()
        {
            // Initialize function arguments.
            String strMsg = "This is a message to be protected.";
            String strDescriptor = "LOCAL=user";
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;

            // Protect a message to the local user.
            IBuffer buffProtected = await this.SampleProtectAsync(
                strMsg,
                strDescriptor,
                encoding);

            // Decrypt the previously protected message.
            String strDecrypted = await this.SampleUnprotectData(
                buffProtected,
                encoding);
        }

        public async Task<IBuffer> SampleProtectAsync(
            String strMsg,
            String strDescriptor,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object for the specified descriptor.
            DataProtectionProvider Provider = new DataProtectionProvider(strDescriptor);

            // Encode the plaintext input message to a buffer.
            encoding = BinaryStringEncoding.Utf8;
            IBuffer buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Encrypt the message.
            IBuffer buffProtected = await Provider.ProtectAsync(buffMsg);

            // Execution of the SampleProtectAsync function resumes here
            // after the awaited task (Provider.ProtectAsync) completes.
            return buffProtected;
        }

        public async Task<String> SampleUnprotectData(
            IBuffer buffProtected,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object.
            DataProtectionProvider Provider = new DataProtectionProvider();

            // Decrypt the protected message specified on input.
            IBuffer buffUnprotected = await Provider.UnprotectAsync(buffProtected);

            // Execution of the SampleUnprotectData method resumes here
            // after the awaited task (Provider.UnprotectAsync) completes
            // Convert the unprotected message from an IBuffer object to a string.
            String strClearText = CryptographicBuffer.ConvertBinaryToString(encoding, buffUnprotected);

            // Return the plaintext string.
            return strClearText;
        }
    }
}
```

## <a name="protecting-stream-data"></a>스트림 데이터 보호


다음 예제에서는 [**ProtectStreamAsync**](/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectstreamasync) 및 [**UnprotectStreamAsync**](/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.unprotectstreamasync) 메서드를 사용 하 여 스트림 데이터를 현재 사용자의 SID로 비동기적으로 보호 하는 방법을 보여 줍니다.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.DataProtection;
using Windows.Storage.Streams;
using System.Threading.Tasks;

namespace SampleProtectStreamAsync
{

    sealed partial class StreamDataProtectionApp : Application
    {
        public StreamDataProtectionApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Protect a stream synchronously
            this.ProtectData();
         }

        public async void ProtectData()
        {
            // Initialize function arguments.
            String strDescriptor = "LOCAL=user";
            String strLoremIpsum = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse elementum "
                + "ullamcorper eros, vitae gravida nunc consequat sollicitudin. Vivamus lacinia, "
                + "diam a molestie porttitor, sapien neque volutpat est, non suscipit leo dolor "
                + "sit amet nisl. Praesent tincidunt tincidunt quam ut pharetra. Sed tincidunt "
                + "sit amet nisl. Praesent tincidunt tincidunt quam ut pharetra. Sed tincidunt "
                + "porttitor massa, at convallis dolor dictum suscipit. Nullam vitae lectus in "
                + "lorem scelerisque convallis sed scelerisque orci. Praesent sed ligula vel erat "
                + "eleifend tempus. Nullam dignissim aliquet mauris a aliquet. Nulla augue justo, "
                + "posuere a consectetur ut, suscipit et sem. Proin eu libero ut felis tincidunt "
                + "interdum. Curabitur vulputate eros nec sapien elementum ut dapibus eros "
                + "dapibus. Suspendisse quis dui dolor, non imperdiet leo. In consequat, odio nec "
                + "aliquam tincidunt, magna enim ultrices massa, ac pharetra est urna at arcu. "
                + "Nunc suscipit, velit non interdum suscipit, lectus lectus auctor tortor, quis "
                + "ultrices orci felis in dolor. Etiam congue pretium libero eu vestibulum. "
                + "Mauris bibendum erat eleifend nibh consequat eu pharetra metus convallis. "
                + "Morbi sem eros, venenatis vel vestibulum consequat, hendrerit rhoncus purus.";
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf16BE;

            // Encrypt the data as a stream.
            IBuffer buffProtected = await this.SampleDataProtectionStream(
                strDescriptor,
                strLoremIpsum,
                encoding);

            // Decrypt a data stream.
            String strUnprotected = await this.SampleDataUnprotectStream(
                buffProtected,
                encoding);
        }

        public async Task<IBuffer> SampleDataProtectionStream(
            String descriptor,
            String strMsg,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object for the specified descriptor.
            DataProtectionProvider Provider = new DataProtectionProvider(descriptor);

            // Convert the input string to a buffer.
            IBuffer buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a random access stream to contain the plaintext message.
            InMemoryRandomAccessStream inputData = new InMemoryRandomAccessStream();

            // Create a random access stream to contain the encrypted message.
            InMemoryRandomAccessStream protectedData = new InMemoryRandomAccessStream();

            // Retrieve an IOutputStream object and fill it with the input (plaintext) data.
            IOutputStream outputStream = inputData.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBuffer(buffMsg);
            await writer.StoreAsync();
            await outputStream.FlushAsync();

            // Retrieve an IInputStream object from which you can read the input data.
            IInputStream source = inputData.GetInputStreamAt(0);

            // Retrieve an IOutputStream object and fill it with encrypted data.
            IOutputStream dest = protectedData.GetOutputStreamAt(0);
            await Provider.ProtectStreamAsync(source, dest);
            await dest.FlushAsync();

            //Verify that the protected data does not match the original
            DataReader reader1 = new DataReader(inputData.GetInputStreamAt(0));
            DataReader reader2 = new DataReader(protectedData.GetInputStreamAt(0));
            await reader1.LoadAsync((uint)inputData.Size);
            await reader2.LoadAsync((uint)protectedData.Size);
            IBuffer buffOriginalData = reader1.ReadBuffer((uint)inputData.Size);
            IBuffer buffProtectedData = reader2.ReadBuffer((uint)protectedData.Size);

            if (CryptographicBuffer.Compare(buffOriginalData, buffProtectedData))
            {
                throw new Exception("ProtectStreamAsync returned unprotected data");
            }

            // Return the encrypted data.
            return buffProtectedData;
        }

        public async Task<String> SampleDataUnprotectStream(
            IBuffer buffProtected,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object.
            DataProtectionProvider Provider = new DataProtectionProvider();

            // Create a random access stream to contain the encrypted message.
            InMemoryRandomAccessStream inputData = new InMemoryRandomAccessStream();

            // Create a random access stream to contain the decrypted data.
            InMemoryRandomAccessStream unprotectedData = new InMemoryRandomAccessStream();

            // Retrieve an IOutputStream object and fill it with the input (encrypted) data.
            IOutputStream outputStream = inputData.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBuffer(buffProtected);
            await writer.StoreAsync();
            await outputStream.FlushAsync();

            // Retrieve an IInputStream object from which you can read the input (encrypted) data.
            IInputStream source = inputData.GetInputStreamAt(0);

            // Retrieve an IOutputStream object and fill it with decrypted data.
            IOutputStream dest = unprotectedData.GetOutputStreamAt(0);
            await Provider.UnprotectStreamAsync(source, dest);
            await dest.FlushAsync();

            // Write the decrypted data to an IBuffer object.
            DataReader reader2 = new DataReader(unprotectedData.GetInputStreamAt(0));
            await reader2.LoadAsync((uint)unprotectedData.Size);
            IBuffer buffUnprotectedData = reader2.ReadBuffer((uint)unprotectedData.Size);

            // Convert the IBuffer object to a string using the same encoding that was
            // used previously to conver the plaintext string (before encryption) to an
            // IBuffer object.
            String strUnprotected = CryptographicBuffer.ConvertBinaryToString(encoding, buffUnprotectedData);

            // Return the decrypted data.
            return strUnprotected;
        }
    }
}
```