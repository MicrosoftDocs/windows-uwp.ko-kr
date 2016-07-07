---
author: TylerMSFT
Description: "이 항목에서는 가장 일반적인 파일 관련 EDP(엔터프라이즈 데이터 보호) 시나리오 중 일부를 달성하기 위해 필요한 코딩 작업의 예를 보여 줍니다."
MS-HAID: dev\_files.protect\_your\_enterprise\_data\_with\_edp
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "EDP(엔터프라이즈 데이터 보호)를 사용하여 파일 보호"
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 2d9b1ec4e39e5c8a100030184ee9287a0d97ea24

---

# EDP(엔터프라이즈 데이터 보호)를 사용하여 파일 보호

__참고__ Windows 10 버전 1511(빌드 10586) 이전에서는 EDP(엔터프라이즈 데이터 보호) 정책을 적용할 수 없습니다.

이 항목에서는 가장 일반적인 파일 관련 EDP(엔터프라이즈 데이터 보호) 시나리오 중 일부를 달성하기 위해 필요한 코딩 작업의 예를 보여 줍니다. EDP 기능이 파일, 스트림, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보려면 [EDP(엔터프라이즈 데이터 보호)](../enterprise/edp-hub.md)를 참조하세요.

**참고** [EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)에서는 이 항목에 설명된 것과 동일한 여러 시나리오를 다룹니다.

## 필수 조건

-   **EDP 설정**

    [EDP를 위한 컴퓨터 설정](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)을 참조하세요.

-   **엔터프라이즈 인식(enterprise-enlightened) 앱 빌드 구현**

    자체적으로 엔터프라이즈 데이터가 엔터프라이즈를 통해 관리되도록 하는 앱을 엔터프라이즈 인식(enterprise-enlightened) 앱이라고 합니다. 인식 앱은 비인식 앱보다 더 강력하고 스마트하고 유연하며 신뢰할 수 있습니다. 앱은 제한된 **enterpriseDataPolicy** 기능을 선언하여 인식되도록 시스템에 알립니다. 그렇지만 기능을 설정하는 것 이상의 효과가 있습니다. 자세한 내용은 [엔터프라이즈 인식(enterprise-enlightened) 앱](../enterprise/edp-hub.md#enterprise-enlightened-apps)을 참조하세요.

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C\# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C\# 또는 Visual Basic에서 비동기 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

## 로컬 폴더 경로 및 파일 탐색기에서 보호된 파일 보기


이 항목에서 코드 예제를 호스트하는 앱을 만들어 이러한 작업을 수행해볼 수 있습니다. 이 코드 예제에서는 앱의 로컬 폴더에 파일을 쓰며, 디스크에서 해당 폴더의 정확한 위치는 앱에 고유하게 유지됩니다. 앱의 로컬 폴더 위치를 확인하려면 다음 코드를 추가할 수 있습니다.

```CSharp
// Put a breakpoint on the line after the line of code below. Use the value of localFolderPath
// in File Explorer to find the output file that the later code examples create.
string localFolderPath = ApplicationData.Current.LocalFolder.Path;
```

경로를 만든 후 파일 탐색기를 사용하여 앱에서 만들어진 파일을 쉽게 찾을 수 있습니다. 이러한 방식으로 파일이 보호되도록 할 수 있으며 파일은 올바른 ID로 보호됩니다.

파일 탐색기의 **폴더 및 검색 옵션 변경** 및 **보기** 탭에서 **암호화된 파일을 컬러로 표시**를 선택합니다. 또한 파일 탐색기의 **보기**&gt;**열 추가** 명령을 사용하여 **Encrypted to(암호화)** 열을 추가합니다. 이렇게 하면 파일을 보호할 엔터프라이즈 ID를 확인할 수 있습니다.

## 엔터프라이즈 데이터를 새 파일에서 보호(대화형 앱)


특정 네트워크 끝점, 파일, 클립보드, 또는 공유 계약을 비롯한 다양한 방법을 사용해서 엔터프라이즈 데이터를 앱에 입력할 수 있습니다. 앱에서 새 엔터프라이즈 데이터를 만들 수도 있습니다. 인식 앱이 어떤 방식을 통해 엔터프라이즈 데이터에 얻게 되든, 앱은 데이터를 새 파일에 유지할 때, 관리되는 엔터프라이즈 ID로 파일을 보호하도록 주의해야 합니다.

기본 단계는 일반 저장소 API를 사용하여 파일을 만들고, EDP API를 사용하여 엔터프라이즈 ID로 파일을 보호한 다음(다시 일반 저장소 API 사용) 파일에 쓰는 것입니다. 아래 예제와 같이 파일에 쓰기 전에 파일이 보호되도록 해야 합니다. [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) 메서드를 사용하여 파일을 보호합니다. 또한 일반적인 경우처럼 ID가 관리되는 경우에만 ID로 보호하는 것이 가능합니다. 그 이유와 앱이 실행 중인 엔터프라이즈의 ID를 확인하는 방법에 대한 자세한 내용은 [ID가 관리되는지 확인](../enterprise/edp-hub.md#confirming_an_identity_is_managed)을 참조하세요.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFile storageFile = storageFolder.CreateFileAsync("sample.txt",
        CreationCollisionOption.ReplaceExisting);

    // It's important to protect a file *before* writing enterprise data to it.
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(storageFile, identity);

    // If the file is now protected, to the intended identity, then we can write to it.
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
        await Windows.Storage.FileIO.WriteTextAsync(storageFile, enterpriseData);
}
```

## 엔터프라이즈 데이터를 새 파일에서 보호(백그라운드 작업)


이전 섹션에서 사용했던 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) API는 대화형 앱에만 적합합니다. 백그라운드 작업의 경우에는 잠금 화면 상태에서만 코드를 실행할 수 있습니다. 조직이 안전한 DPL("잠금 상태에서 데이터 보호") 정책을 관리하고 있을 수 있으며, 이 경우 디바이스가 잠기면 보호되는 리소스에 액세스하는 데 필요한 암호화 키가 일시적으로 디바이스 메모리에서 제거됩니다. 따라서 디바이스 분실 시에도 데이터 누출이 방지됩니다. 이 기능은 또한 핸들이 닫혀 있을 때 보호되는 파일과 연결된 키를 제거합니다. 하지만 잠금 기간(디바이스가 잠긴 때부터 잠금 해제되는 때까지의 시간) 동안 보호되는 파일을 새로 만들어 파일 핸들을 열어 둔 상태에서 이 파일에 액세스하는 것은 가능합니다. **StorageFolder.CreateFileAsync**는 파일 생성 시 핸들을 닫기 때문에, 이 알고리즘을 사용할 수가 없습니다.

1.  **StorageFolder.CreateFileAsync**를 사용하여 새 파일을 만듭니다.
2.  **FileProtectionManager.ProtectAsync**를 사용하여 암호화합니다.
3.  파일을 열고 이 파일에 씁니다.

1단계는 파일 핸들 닫기와 관련이 있으므로(그리고 1단계에서 핸들을 닫지 않았더라도 2단계에서 닫음), 해당 파일에 대한 암호화 키를 더 이상 사용할 수 없기 때문에 3단계는 불가능합니다.

이 시나리오를 해결하려면 [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153)를 사용하여 보호되는 파일을 만든 후 **IRandomAccessStream**을 이 파일에 반환하면 됩니다. 그러면 파일 핸들을 열어 둔 채로 앱에서 파일에 여러 번 쓸 수 있습니다.

예제 코드에서는 새 메일이 수신되면 새 파일을 작성하는 간단한 메일 앱을 보여 줍니다. 메일 앱은 디바이스가 잠겨 있을 때 메일을 동기화해야 합니다. 이 앱이 새 메일을 수신하면 [**CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153)를 사용하여 새 파일을 만들고 출력 스트림을 검색하고 이 파일에 메일 본문을 씁니다.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

    ProtectedFileCreateResult protectedFileCreateResult =
        await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
            "sample.txt", identity, CreationCollisionOption.ReplaceExisting);

    // It's important to successfully protect a file *before* writing enterprise data to it.
    if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
        protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        using (IOutputStream outputStream =
            protectedFileCreateResult.Stream.GetOutputStreamAt(0))
        {
            using (DataWriter writer = new DataWriter(outputStream))
            {
                writer.WriteString(enterpriseData);
                await writer.StoreAsync();
                await writer.FlushAsync();
            }
        }
    }
    else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
    {
        // Perform any special processing for the access suspended case.
    }
}
```

## 엔터프라이즈 데이터를 포함하는 데 사용하는 폴더 보호


폴더 내의 모든 항목을 보호하는 것도 가능합니다. [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157)를 사용하여 빈 폴더를 보호할 수 있습니다. 그런 이후에 폴더 내에서 만든 파일이나 폴더도 보호됩니다. 기존 폴더를 보호하거나, 보호할 새 폴더를 만들 수 있습니다(아래 예제에서는 새 폴더를 만듦). 그러나 두 경우 모두 보호를 성공적으로 적용하려면 항상 폴더가 비어 있어야 합니다. 그렇지 않으면 [**FileProtectionInfo.Status**](https://msdn.microsoft.com/library/windows/apps/dn705150)는 [**FileProtectionStatus.NotProtectable**](https://msdn.microsoft.com/library/windows/apps/dn279147) 값을 갖게 됩니다.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
    }
}
```

## 다른 파일로 보호 복사


이 시나리오에서는 해당 엔터프라이즈 ID로 보호되고 있는 파일이 이미 있습니다. 이러한 보호를 다른 파일로 매우 손쉽게 복제할 수 있습니다. ID를 알 필요는 없으며 올바른 ID인지만 알면 됩니다.

보호된 파일의 단순한 복사본을 만들려면 [**StorageFile.CopyAsync**](https://msdn.microsoft.com/library/windows/apps/br227190)를 호출할 수 있습니다. 파일의 결과 복사본은 원본과 동일한 보호 기능을 갖게 됩니다.

보호되지 않은 기존 파일에 엔터프라이즈 데이터를 쓰기 전에 파일을 보호하려는 경우 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157)를 호출하는 대신(이전 시나리오에 설명됨, 관리 ID를 전달해야 함), 코드 예제와 같이 [**FileProtectionManager.CopyProtectionAsync**](https://msdn.microsoft.com/library/windows/apps/dn705152)를 호출할 수 있습니다.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

## 보호된 파일에 대한 액세스를 거부하는 처리


이 시나리오에서 앱은 이전에 앱이 보호한 파일에 액세스하려고 하지만 거부됩니다. 파일 상태를 확인하여 어떤 부분에 문제가 있는지 검토해야 합니다. 이 코드 예제에서 앱은 [**FileProtectionManager.GetProtectionInfoAsync**](https://msdn.microsoft.com/library/windows/apps/dn705154) API를 호출하여 상태를 쿼리하고, 원격 관리의 결과로 파일에 대한 액세스 권한이 취소된 것이 이유인지 확인합니다.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async System.Threading.Tasks.Task<bool> IsFileProtectionStatusRevoked
    (StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status == FileProtectionStatus.Revoked)
    {
        // Inform the user that their data has been revoked.
    }
    return fileProtectionInfo.Status == FileProtectionStatus.Revoked;
}
```

## 파일의 보호되는 ID를 기반으로 UI 정책 적용 사용


보호되는 파일(예: PDF)의 내용을 UI에 표시하려는 경우 앱에서는 파일이 보호되는 ID에 따라 UI 정책 적용을 사용하도록 설정해야 합니다. 파일의 보호 정보를 쿼리하고 검색된 ID에서 시스템의 UI 정책 적용을 사용하도록 설정해야 합니다.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void EnableUIPolicyFromFile(StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo = 
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // No policy enforcement required, unless the file is inaccessible
        // (Revoked, ProtectedToOtherIdentity) in which case that should
        // be handled in an app-specific way.
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(fileProtectionInfo.Identity);
}
```

**참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

## 관련 항목


[EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 






<!--HONumber=Jun16_HO4-->


