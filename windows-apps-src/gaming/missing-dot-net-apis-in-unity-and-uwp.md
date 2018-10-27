---
author: eliotcowley
title: Unity 및 UWP에서 누락된 .NET API
description: Unity에서 UWP 게임을 빌드할 때 누락된 .NET API와 일반적인 문제에 대한 해결 방법에 대해 알아보세요.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.author: elcowle
ms.date: 2/21/2018
ms.topic: article
keywords: windows 10, uwp, 게임, .net, unity
ms.localizationpriority: medium
ms.openlocfilehash: 4b795ed47249eee1f9dc21b195d46f450997019e
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703719"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>Unity 및 UWP에서 누락된 .NET API

.NET을 사용하여 UWP 게임을 빌드할 때 Unity 편집기나 독립 실행형 PC 게임에서 사용할 수 있는 몇 가지 API가 UWP에 대해 없는 것을 발견할 수 있습니다. UWP 앱용 .NET에 각 네임스페이스에 대한 전체 .NET Framework에서 제공하는 하위 집합 형식이 포함되어 있기 때문입니다.

또한 일부 게임 엔진은 Unity의 Mono와 같은 UWP용 .NET과 완전히 호환되지 않는 다른 형식을 사용합니다. 따라서 사용자가 게임을 작성할 때 편집기에서 모든 것이 제대로 작동하지만 UWP용 빌드로 이동했을 때 **'System.Runtime.Serialization' 네임스페이스에 'Formatters' 형식 또는 네임스페이스가 존재하지 않습니다(어셈블리 참조가 누락되었습니까?)** 와 같은 오류가 발생할 수 있습니다.

다행히 Unity에는 이러한 누락된 일부 API를 확장 메서드 및 대체 형식으로 제공하며, [유니버설 Windows 플랫폼: .NET 스크립팅 백 엔드에서 누락된 .NET 형식](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)에 설명되어 있습니다. 그러나 필요한 기능이 없는 경우 [Windows 8.x 앱용 .NET 개요](https://msdn.microsoft.com/library/windows/apps/br230302)에서 WinRT 또는 UWP용 .NET API를 사용하기 위한 코드를 변환할 수 있는 방법에 대해 설명합니다. (Windows 8을 다루고 있지만 Windows 10 UWP 앱에도 적용할 수 있습니다.)

## <a name="net-standard"></a>.NET Standard

일부 API가 작동하지 않는 이유를 이해하기 위해 여러 .NET 형식 및 UWP가 .NET을 구현하는 방식을 이해하는 것이 중요합니다. [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)는 플랫폼 간 .NET API의 공식 사양이며 다른 .NET 형식을 통합합니다. .NET의 각 구현은 특정 버전의 .NET Standard를 지원합니다. [.NET 구현 지원](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)에서 표준 및 구현 표를 볼 수 있습니다.

UWP SDK의 각 버전은 다른 수준의 .NET Standard를 따릅니다. 예를 들어 16299 SDK(Fall Creators Update)는 .NET Standard 2.0을 지원합니다.

특정 .NET API가 대상으로 하는 UWP 버전에서 지원되는지 여부를 알고자 할 경우 [.NET Standard API 참조](https://docs.microsoft.com/dotnet/api/index?view=netstandard-2.0)를 확인하여 해당 UWP 버전에서 지원되는 .NET Standard의 버전을 선택할 수 있습니다.

## <a name="scripting-backend-configuration"></a>스크립팅 백 엔드 구성

UWP를 빌드하는 데 문제가 있는 경우 먼저 **플레이어 설정**(**파일 > 빌드 설정**에서 **유니버설 Windows 플랫폼**과 **플레이어 설정**을 차례로 선택)을 확인해야 합니다. **기타 설정 > 구성** 아래에 있는 처음 세 개의 드롭다운(**스크립팅 런타임 버전**, **스크립팅 백 엔드**, 및 **Api 호환성 수준**)은 모두 고려할 중요한 설정입니다.

**스크립팅 런타임 버전**은 Unity 스크립팅 백 엔드가 사용하며 선택하는 .NET Framework 지원에 해당하는 (근사한) 버전을 다운로드할 수 있습니다. 그러나 해당 버전의 .NET Framework의 모든 API가 지원되지는 않으며 사용자 UWP가 대상으로 하는 버전의 .NET Standard만 지원됩니다.

종종 새로운 .NET 릴리스를 통해 더 많은 API가 .NET Standard에 추가되어 독립 실행형과 UWP에서 동일한 코드를 사용하게 될 수 있습니다. 예를 들어 [System.Runtime.Serialization.Json](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json) 네임스페이스는 .NET Standard 2.0에서 도입되었습니다. **스크립팅 런타임 버전**을 **.NET 3.5 등가**(이전 버전의 .NET Standard를 대상으로 함)로 설정하는 경우, AP;를 사용하려고 하면 오류가 발생합니다. **.NET 4.6 등가**(.NET Standard 2.0 지원)로 전환하면 API가 작동합니다.

**스크립팅 백 엔드**는 **.NET** 또는 **IL2CPP**일 수 있습니다. 이 항목에서는 여기에서 논의한 문제가 생기는 위치인 **.NET**을 선택한 것으로 가정합니다. 자세한 내용은 [스크립팅 백 엔드](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html)를 참조하세요.

마지막으로 게임이 실행되기 원하는 버전의 .NET으로 **Api 호환성 수준**을 설정해야 합니다. 이는 **스크립팅 런타임 버전**과 일치해야 합니다.

일반적으로 **스크립팅 런타임 버전** 및 **Api 호환성 수준**에 대해 .NET Framework와 더 많은 호환성을 획득하여 .NET API를 사용할 수 있도록 사용 가능한 가장 최신 버전을 선택해야 합니다.

![구성: 스크립팅 런타임 버전, 스크립팅 백 엔드, Api 호환성 수준](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>플랫폼 종속 컴파일

UWP를 포함한 여러 플랫폼에 대해 Unity 게임을 작성하는 경우 플랫폼 종속 컴파일을 사용하여 게임을 UWP로 작성할 때에만 UWP용 코드를 실행하도록 하고자 할 수 있습니다. 이러한 방법으로 빌드 오류가 발생하지 않고 독립 실행형 데스크톱 및 기타 플랫폼, 그리고 UWP용 WinRT API에 대해 전체 .NET Framework를 사용할 수 있습니다.

다음과 같은 지시문을 사용하여 UWP 앱으로 실행될 때 코드만 컴파일할 수 있습니다.

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` 이는 .NET 스크립팅 백 엔드에 대해 C# 코드를 컴파일하는지 확인하기 위함입니다. IL2CPP 등의 다른 스크립팅 백 엔드를 사용하는 경우 대신 `UNITY_WSA_10_0`을 사용합니다.

플랫폼 종속 컴파일 지시어의 전체 목록 [플랫폼 종속 컴파일](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html)을 참조하세요.

## <a name="common-issues-and-workarounds"></a>일반적인 문제 및 해결 방법

다음 시나리오에서는 UWP 하위 집합에서 .NET API가 누락된 경우 발생할 수 있는 일반적인 문제와 이를 해결하는 방법에 대해 설명합니다.

### <a name="data-serialization-using-binaryformatter"></a>BinaryFormatter를 사용하여 데이터 직렬화

일반적으로 게임은 플레이어가 쉽게 조작할 수 없도록 데이터를 직렬화하여 저장합니다. 그러나 개체를 바이너리로 직렬화하는 [BinaryFormatter](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter)는 이전 버전의 .NET Standard(2.0 이전)에서 사용할 수 없습니다. 대신 [XmlSerializer](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) 또는 [DataContractJsonSerializer](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer)를 사용하는 것을 고려하세요.

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>I/O 작업

[FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream)과 같은 [System.IO](https://docs.microsoft.com/dotnet/api/system.io) 네임스페이스에서 일부 형식은 이전 버전의 .NET Standard에서 사용할 수 없습니다. 그러나 Unity는 이를 게임에서 사용할 수 있도록 [Directory](https://docs.microsoft.com/dotnet/api/system.io.directory), [File](https://docs.microsoft.com/dotnet/api/system.io.file) 및 **FileStream** 형식을 제공합니다.

또는 UWP 앱에만 제공되는 [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage) API를 사용할 수도 있습니다. 그러나 이러한 API는 앱에서 특정 저장소를 작성하지 못하도록 하며 전체 파일 시스템에 대한 무료 액세스를 제공하지 않습니다. 자세한 내용은 [파일, 폴더 및 라이브러리](https://docs.microsoft.com/windows/uwp/files/)를 참조하세요.

중요한 정보는 [Close](https://docs.microsoft.com/dotnet/api/system.io.stream.close) 메서드는 .NET Standard 2.0 이상에서만 사용할 수 있다는 것입니다(그러나 Unity에서 확장 메서드를 제공함). 대신 [Dispose](https://docs.microsoft.com/dotnet/api/system.io.stream.dispose)를 사용하십시오.

### <a name="threading"></a>스레딩

[ThreadPool](https://docs.microsoft.com/dotnet/api/system.threading.threadpool)과 같은 [System.Threading](https://docs.microsoft.com/dotnet/api/system.threading) 네임스페이스에서 일부 형식은 이전 버전의 .NET Standard에서 사용할 수 없습니다. 이 경우 [Windows.System.Threading](https://docs.microsoft.com/uwp/api/windows.system.threading) 네임스페이스를 사용할 수 있습니다.

UWP 및 비 UWP 플랫폼 모두에 대해 준비하도록 플랫폼 종속 컴파일을 사용하여 Unity 게임에서 스레딩을 처리하는 방법은 다음과 같습니다.

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>보안

[System.Security.Cryptography.X509Certificates](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0) 같은 일부 **System.Security.*** 네임스페이스는 UWP용 Unity 게임을 빌드할 때 사용할 수 없습니다. 이러한 경우, 동일한 기능의 상당 부분을 포함하는 **Windows.Security.*** API를 사용합니다.

다음 예제에서는 지정된 이름을 가진 인증서 저장소에서 인증서를 가져옵니다.

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

WinRT 보안 API를 사용하는 방법에 대한 자세한 내용은 [보안](https://docs.microsoft.com/windows/uwp/security/)을 참조하세요.

### <a name="networking"></a>네트워킹

[System.Net.Mail](https://docs.microsoft.com/dotnet/api/system.net.mail?view=netstandard-2.0) 같은 일부 **System&period;Net.*** 네임스페이스 역시 UWP용 Unity 게임을 빌드할 때 사용할 수 없습니다. 이러한 API 중 대부분에서는 해당되는 **Windows.Networking.*** 및 **Windows.Web.*** WinRT API를 사용하여 비슷한 기능을 가져옵니다. 자세한 내용은 [네트워킹 및 웹 서비스](https://docs.microsoft.com/windows/uwp/networking/)를 참조하세요.

**System.Net.Mail**의 경우에는 [Windows.ApplicationModel.Email](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email) 네임슾이스를 사용합니다. 자세한 내용은 [이메일 전송](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email)을 참조하세요.

## <a name="see-also"></a>참고 항목

* [유니버설 Windows 플랫폼: .NET 스크립팅 백 엔드에서 누락된 .NET 형식](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [UWP 앱용 .NET 개요](https://msdn.microsoft.com/library/windows/apps/br230302)
* [Unity UWP 포팅 가이드](https://unity3d.com/partners/microsoft/porting-guides)