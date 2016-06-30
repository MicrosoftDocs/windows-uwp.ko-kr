---
author: TylerMSFT
Description: "이 항목에서는 가장 일반적인 스트림 및 버퍼 관련 EDP(엔터프라이즈 데이터 보호) 시나리오 중 일부를 달성하기 위해 필요한 코딩 작업의 예를 보여 줍니다."
MS-HAID: dev\_files.use\_edp\_to\_protect\_streams\_and\_buffers
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "EDP(엔터프라이즈 데이터 보호)를 사용하여 스트림 및 버퍼 보호"
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: fdde4f7d2ab46b6349273f7c1c9d91cf27aa341a

---

# EDP(엔터프라이즈 데이터 보호)를 사용하여 스트림 및 버퍼 보호

__참고__ Windows 10 버전 1511(빌드 10586) 이전에서는 EDP(엔터프라이즈 데이터 보호) 정책을 적용할 수 없습니다.

이 항목에서는 가장 일반적인 스트림 및 버퍼 관련 EDP(엔터프라이즈 데이터 보호) 시나리오 중 일부를 달성하기 위해 필요한 코딩 작업의 예를 보여 줍니다. EDP 기능이 파일, 스트림, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보려면 [EDP(엔터프라이즈 데이터 보호)](../enterprise/edp-hub.md)를 참조하세요.

**참고** [EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)에서는 이 항목에 설명된 것과 동일한 여러 시나리오를 다룹니다.

## 필수 조건


-   **EDP 설정**

    [EDP를 위한 컴퓨터 설정](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)을 참조하세요.

-   **엔터프라이즈 인식(enterprise-enlightened) 앱 빌드 구현**

    자체적으로 엔터프라이즈 데이터가 엔터프라이즈를 통해 관리되도록 하는 앱을 엔터프라이즈 인식(enterprise-enlightened) 앱이라고 합니다. 인식 앱은 비인식 앱보다 더 강력하고 스마트하고 유연하며 신뢰할 수 있습니다. 앱은 제한된 **enterpriseDataPolicy** 기능을 선언하여 인식되도록 시스템에 알립니다. 그렇지만 기능을 설정하는 것 이상의 효과가 있습니다. 자세한 내용은 [엔터프라이즈 인식(enterprise-enlightened) 앱](../enterprise/edp-hub.md#enterprise-enlightened-apps)을 참조하세요.

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C\# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C\# 또는 Visual Basic에서 비동기 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

## 엔터프라이즈 ID로 데이터 스트림 보호


**참고** 스트림 또는 버퍼를 보호할 때마다 디바이스에서 EDP를 사용할 수 없는 경우 앱이 인식할 수 있도록 [**ProtectionPolicyManager.PolicyChanged**](https://msdn.microsoft.com/library/windows/apps/mt608411) 이벤트를 구독하는 것이 좋습니다. 이 경우 모든 스트림과 버퍼 보호를 해제해야 합니다. 보호된 상태로 두는 스트림이나 버퍼는 사용자가 MDM(모바일 디바이스 관리)에서 디바이스 등록을 해제할 경우 해지될 수 있습니다. 리소스를 만들 때 EDP를 사용하지 않도록 설정한 경우에는 해당 해지가 부적절합니다. EDP를 사용할 수 없을 경우 리소스 보호를 해제하면 이를 방지할 수 있습니다.



이 시나리오에서 사용자 앱은 엔터프라이즈 데이터를 포함하는 보호되지 않은 스트림에 액세스할 수 있습니다. 디바이스 내부 및 외부로 전송할 때 이 스트림을 보호된 상태로 유지하기 위해 앱은 속하는 엔터프라이즈 ID로 데이터를 보호합니다. 이렇게 하면 엔터프라이즈에서 필요에 따라 데이터를 지울 수 있습니다. 나중에 스트림에서 "보호 해제" 메서드를 호출할지 여부를 결정하기 위해 앱은 스트림이 보호되었는지 여부를 나타내는 상태를 유지 관리해야 하며, 이 코드 예제의 함수가 해당 상태를 반환하는 것은 이런 이유 때문입니다. 전달된 ID가 관리되지 않거나 앱이 해당 ID에 대해 허용되지 않는 경우에는 스트림이 보호되지 않으며 [**DataProtectionManager.ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706021) 호출에서 '보호 해제됨' 상태가 반환됩니다.

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async System.Threading.Tasks.Task<bool> ProtectAStream
    (InMemoryRandomAccessStream unprotectedInMemoryRandomAccessStream, string identity,
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    IInputStream unprotectedStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0);
    IOutputStream protectedStream = protectedInMemoryRandomAccessStream.GetOutputStreamAt(0);

    // Protect "inputStream".
    DataProtectionInfo info = 
        await DataProtectionManager.ProtectStreamAsync(unprotectedStream, identity, protectedStream);

    // Indicate to the caller whether the stream was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. Similar to buffers, this status
    // must be stored by the app. UnprotectStreamAsync must only be called if the stream was protected
    // successfully earlier.

    return (info.Status == DataProtectionStatus.Protected);
}
```

위 코드 예제와 같은 메서드의 사용 방법을 보여 주기 위해 다음 도우미 메서드는 이 메서드를 사용하여 문자열을 보호되지 않은 스트림으로 변환한 후 스트림을 보호합니다.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<bool> ProtectStringAsStreamAsync
    (string unprotectedEnterpriseData, string identity, 
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        using (var dataWriter = new DataWriter(unprotectedInMemoryRandomAccessStream))
        {
            dataWriter.WriteString(unprotectedEnterpriseData);
            await dataWriter.StoreAsync();
            await dataWriter.FlushAsync();
            return await this.ProtectAStream(unprotectedInMemoryRandomAccessStream,
                identity, protectedInMemoryRandomAccessStream);
        }
    }
}
```

## 스트림 상태 검색


이 시나리오에서 사용자 앱에는 이전에 보호된 스트림이 있으며, 이 스트림에 대한 무단 액세스를 방지해야 합니다. 필요할 때 다시 스트림의 콘텐츠를 검색하기 위해 앱이 스트림 상태를 확인할 수 있습니다.

스트림 상태도 [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023)에서 반환됩니다. 이 API는 입력 리소스가 보호되도록 요구하므로 '보호되지 않음' 상태를 반환하지 않습니다(리소스 보호가 해제되었는지 안정적으로 확인할 수 없음). 결과를 '보호되지 않음'과 비교하는 코드가 있는 경우 디자인 결함이 있다는 의미입니다. 코드에서 스트림이 보호되었는지 여부를 더 이상 추적할 수 없음을 나타냅니다.

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async void CheckProtectedStreamStatus(IInputStream protectedStream)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetStreamProtectionInfoAsync(protectedStream);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation. Perhaps, show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## 데이터 스트림 보호 해제


이 시나리오에서 사용자 앱은 이전에 보호된 스트림의 보호를 해제하려고 합니다. 이 코드 예제에서는 보호된 스트림을 가져와(이 프로세스가 작동하려면 스트림이 보호되어 있어야 함) [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023) 호출을 통해 보호를 해제합니다. 그런 다음 스트림에서 문자열로 읽고 반환합니다.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<string> UnprotectStreamIntoStringAsync
    (InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        DataProtectionInfo dataProtectionInfo = 
            await DataProtectionManager.UnprotectStreamAsync(protectedInMemoryRandomAccessStream, 
            unprotectedInMemoryRandomAccessStream);

        using (var inputStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0))
        {
            using (var dataReader = new DataReader(inputStream))
            {
                await dataReader.LoadAsync((uint)unprotectedInMemoryRandomAccessStream.Size);
                return dataReader.ReadString((uint)unprotectedInMemoryRandomAccessStream.Size);
            }
        }
    }
}
```

지금까지 제공된 도우미 메서드의 사용 방법을 보여 주기 위해 다음 이벤트 처리기는 텍스트 상자에서 문자열을 가져와 스트림에 쓰고, 스트림을 보호한 후 보호를 해제하고(성공적으로 보호된 경우), 마지막으로 보호되지 않은 스트림에서 문자열을 읽고 UI에 표시합니다.

```CSharp
using Windows.Storage.Streams;

private async void ProtectAndThenUnprotectStream_Click(object sender, RoutedEventArgs e)
{
    var protectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream();
    bool isStreamProtected = await this.ProtectStringAsStreamAsync
        (this.enterpriseDataTextBox.Text, MainPage.IDENTITY, protectedInMemoryRandomAccessStream);

    // Only unprotect the stream if we're sure that the stream actually was protected.
    if (isStreamProtected)
    {
        this.resultDataTextBlock.Text = 
            await this.UnprotectStreamIntoStringAsync(protectedInMemoryRandomAccessStream);
    }
}
```

## 정적 데이터 버퍼의 상태 검색


이 시나리오에서 사용자 앱에는 이전에 보호된 버퍼가 있으며, 이 버퍼에 대한 무단 액세스를 방지해야 합니다. 필요할 때 다시 버퍼의 콘텐츠를 검색하기 위해 앱이 버퍼 상태를 확인할 수 있습니다.

버퍼 상태도 [**DataProtectionManager.UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706022)에서 반환됩니다. 이 API는 입력 리소스가 보호되도록 요구하므로 '보호되지 않음' 상태를 반환하지 않습니다.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

private async void CheckProtectedBufferStatus(IBuffer protectedData)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(protectedData);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation, perhaps show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## 엔터프라이즈 리소스에서 검색된 정적 데이터 보호


이 시나리오는 데이터 버퍼에서 작동한다는 점을 제외하고 스트림 코드 예제와 동일한 경우를 다룹니다. 마찬가지로, 예제와 같이 버퍼가 보호되었는지 여부를 나타내는 상태를 유지 관리해야 합니다. 전달된 ID가 관리되지 않거나 앱이 해당 ID에 대해 허용되지 않는 경우에는 버퍼가 보호되지 않으며 [**DataProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706020) 호출에서 '보호 해제됨' 상태가 반환됩니다.

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private IBuffer data = null;
private bool isProtected = false;

void StoreBuffer(IBuffer data, bool isProtected)
{
    this.data = data;
    this.isProtected = isProtected;
}

IBuffer GetStoredBuffer(out bool isProtected)
{
    isProtected = this.isProtected;
    return this.data;
}

private string identity = "contoso.com";

private async void ProtectAndThenUnprotectBuffer_Click(object sender, RoutedEventArgs e)
{
    BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
    IBuffer inputData = CryptographicBuffer.ConvertStringToBinary
        (this.enterpriseDataTextBox.Text, encoding);
    BufferProtectUnprotectResult result =
        await DataProtectionManager.ProtectAsync(inputData, this.identity);

    // Record whether the buffer was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. This status
    // must be stored by the app. UnprotectAsync must only be called if the buffer was
    // protected successfully earlier.
    bool isBufferProtected = 
        (result.ProtectionInfo.Status == DataProtectionStatus.Protected);
    IBuffer outputData = result.Buffer;

    // Store the data away...
    this.StoreBuffer(outputData, isBufferProtected);

    // ...and then later retrieve it.
    outputData = this.GetStoredBuffer(out isBufferProtected);

    string storedString = string.Empty;

    if (isBufferProtected)
    {
        result = await DataProtectionManager.UnprotectAsync(outputData);

        if (result.ProtectionInfo.Status != DataProtectionStatus.Unprotected)
        {
            // Code goes here to handle the situation where the buffer
            // is no longer accessible.
            return;
        }
        outputData = result.Buffer;
    }
    this.resultDataTextBlock.Text = CryptographicBuffer.ConvertBinaryToString(encoding, outputData);
}
```

## 스트림 또는 버퍼의 보호된 ID에 따라 UI 정책 적용 사용


앱이 보호된 스트림이나 버퍼의 콘텐츠를 UI에 표시하려는 경우 리소스가 보호된 ID에 따라 UI 정책 적용을 사용하도록 설정해야 합니다. 리소스의 보호 정보를 쿼리하고 검색된 ID에서 시스템의 UI 정책 적용을 사용하도록 설정해야 합니다.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private async void EnableUIPolicyFromProtectedBuffer(IBuffer buffer)
{
    DataProtectionInfo protectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(buffer);

    if (protectionInfo.Status != DataProtectionStatus.Protected)
    {
        // In this case, the app has lost access to the buffer
        // (ProtectedToOtherIdentity, Revoked). This must be handled.
        // 'Unprotected' is never returned for GetProtectionInfoAsync().
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(protectionInfo.Identity);
}

```

**참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

## 관련 항목


[EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 






<!--HONumber=Jun16_HO4-->


