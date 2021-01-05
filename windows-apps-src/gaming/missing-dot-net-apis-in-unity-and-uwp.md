---
title: Unity 및 UWP에서 누락된 .NET API
description: Unity에서 UWP 게임을 빌드할 때 누락 된 .NET Api 및 일반적인 문제에 대 한 해결 방법을 알아봅니다.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 02/21/2018
ms.topic: article
keywords: windows 10, uwp, 게임, .net, unity
ms.localizationpriority: medium
ms.openlocfilehash: b687f3ec09a99ae6ccb81e5c205eb454e0af0e04
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860111"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>Unity 및 UWP에서 누락된 .NET API

.NET을 사용 하 여 UWP 게임을 빌드하는 경우에는 Unity 편집기 또는 독립 실행형 PC 게임에서 사용할 수 있는 일부 Api가 UWP에 대해 존재 하지 않는 것을 알 수 있습니다. UWP 앱 용 .NET에는 각 네임 스페이스에 대해 전체 .NET Framework에서 제공 되는 형식의 하위 집합이 포함 되기 때문입니다.

또한 일부 게임 엔진은 Unity의 Mono와 같이 UWP 용 .NET과 완전히 호환 되지 않는 .NET의 다양 한 기능을 사용 합니다. 따라서 게임을 작성 하는 경우 모든 것이 편집기에서 제대로 작동할 수 있지만 UWP에 대 한 빌드로 이동 하면 ' system.xml ' **네임 스페이스에 ' 포맷터 ' 형식 또는 네임 스페이스가 없습니다. 어셈블리 참조가 있는지 확인** 하세요.

다행히 Unity는 이러한 누락 된 Api 중 일부를 확장 메서드 및 대체 형식으로 제공 합니다. 이러한 Api는 [유니버설 Windows 플랫폼: .Net Scripting 백 엔드에서 누락 된 .Net 형식](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)에 설명 되어 있습니다. 그러나 필요한 기능을 여기에서 사용 하지 않는 경우 [Windows 8.x 앱 용 .net 개요](/previous-versions/windows/apps/br230302(v=vs.140)) 에서는 Windows 런타임 api에 대해 WinRT 또는 .net을 사용 하도록 코드를 변환 하는 방법을 설명 합니다. 이는 Windows 8에 대해 설명 하지만 Windows 10 UWP 앱에도 적용 됩니다.

## <a name="net-standard"></a>.NET Standard

일부 Api가 작동 하지 않는 이유를 이해 하려면 다양 한 .NET 버전 및 UWP에서 .NET을 구현 하는 방법을 이해 하는 것이 중요 합니다. [.NET Standard](/dotnet/standard/net-standard) 는 플랫폼 간 이며 다양 한 .net 기능을 통합 하는 .net api의 공식 사양입니다. .NET의 각 구현에서는 특정 버전의 .NET Standard을 지원 합니다. [.Net 구현 지원](/dotnet/standard/net-standard#net-implementation-support)에서 표준 및 구현 표를 볼 수 있습니다.

각 버전의 UWP SDK는 다른 수준의 .NET Standard을 준수 합니다. 예를 들어 16299 SDK (에 지 크리에이터 업데이트)는 .NET Standard 2.0을 지원 합니다.

대상으로 하는 UWP 버전에서 특정 .NET API가 지원 되는지 확인 하려는 경우 [.NET STANDARD API 참조](/dotnet/api/index?view=netstandard-2.0&preserve-view=true) 를 확인 하 고 해당 버전의 uwp에서 지원 되는 .NET Standard 버전을 선택할 수 있습니다.

## <a name="scripting-backend-configuration"></a>Scripting 백엔드 구성

UWP를 빌드하는 데 문제가 있는 경우 먼저 **플레이어 설정** (**파일 > 빌드 설정**, **유니버설 Windows 플랫폼** 선택, **플레이어 설정**)을 확인 합니다. **기타 설정 > 구성** 에서 처음 세 개의 드롭다운 (**scripting Runtime Version**, **Scripting 백 엔드** 및 **Api 호환성 수준**)은 고려해 야 할 모든 중요 한 설정입니다.

**스크립팅 런타임 버전** 은 사용자가 선택 하는 것과 동일한 .NET Framework 지원 버전을 가져올 수 있도록 하는 Unity scripting 백 엔드가 사용 하는 기능입니다. 그러나 해당 버전의 .NET Framework에 있는 모든 Api가 지원 되는 것은 아닙니다. UWP에서 대상으로 하는 .NET Standard 버전의 Api만 지원 됩니다.

새 .NET 릴리스를 사용 하는 경우 독립 실행형 및 UWP에서 동일한 코드를 사용할 수 있는 .NET Standard에 더 많은 Api가 추가 됩니다. 예를 들어 네임 스페이스 [ 의System.Runtime.Serialization.Js](/dotnet/api/system.runtime.serialization.json) .NET Standard 2.0에서 도입 되었습니다. 이전 버전의 .NET Standard를 대상으로 하는 **스크립팅 런타임 버전** 을 **.net 3.5** 로 설정 하면 API를 사용 하려고 할 때 오류가 발생 합니다. .NET Standard 2.0을 지 원하는 **.net 4.6에 해당** 하는 것으로 전환 하면 API가 작동 합니다.

**Scripting 백엔드** 는 **.net** 또는 **IL2CPP** 일 수 있습니다. 이 항목에서는 여기서 설명 하는 문제가 발생 하는 경우 **.net** 을 선택 했다고 가정 합니다. 자세한 내용은 [스크립팅 백 엔드](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html) 를 참조 하세요.

마지막으로, 게임을 실행할 .NET 버전으로 **Api 호환성 수준을** 설정 해야 합니다. 이는 **스크립팅 런타임 버전과** 일치 해야 합니다.

일반적으로 **런타임 버전** 및 **Api 호환성 수준을** 스크립팅 하기 위해 사용 가능한 최신 버전을 선택 하 여 .NET Framework와의 호환성을 향상 시킬 수 있으므로 더 많은 .net api를 사용할 수 있습니다.

![구성: Scripting Runtime Version; 백 엔드 스크립팅 Api 호환성 수준](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>플랫폼 종속 컴파일

UWP를 비롯 한 여러 플랫폼에 대 한 Unity 게임을 빌드하는 경우에는 플랫폼 종속 컴파일을 사용 하 여 uwp로 작성 된 코드가 UWP로 작성 된 경우에만 실행 되도록 할 수 있습니다. 이러한 방식으로, 빌드 오류 없이 독립 실행형 데스크톱 및 기타 플랫폼에 대 한 전체 .NET Framework 및 UWP 용 WinRT Api를 사용할 수 있습니다.

다음 지시문을 사용 하 여 UWP 앱으로 실행 되는 경우에만 코드를 컴파일합니다.

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` 는 .NET scripting 백 엔드에 대해 c # 코드를 컴파일하는 경우에만 확인 하면 됩니다. IL2CPP와 같은 다른 스크립팅 백 엔드를 사용 하는 경우 대신을 사용 [`ENABLE_WINMD_SUPPORT`](https://docs.unity3d.com/Manual/windowsstore-code-snippets.html) 합니다.

플랫폼 종속 컴파일 지시문의 전체 목록은 [플랫폼 종속 컴파일](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html)을 참조 하세요.

## <a name="common-issues-and-workarounds"></a>일반적인 이슈 및 해결 방법

다음 시나리오에서는 .NET Api가 UWP 하위 집합에 없는 경우 발생할 수 있는 일반적인 문제 및 이러한 문제를 해결 하는 방법을 설명 합니다.

### <a name="data-serialization-using-binaryformatter"></a>BinaryFormatter를 사용 하 여 데이터 직렬화

플레이어에서 쉽게 조작할 수 없도록 데이터를 직렬화 하는 게임이 일반적입니다. 그러나 개체를 이진으로 serialize 하는 [Binaryformatter](/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter)는 이전 버전의 .NET Standard (2.0 이전 버전)에서 사용할 수 없습니다. 대신 [XmlSerializer](/dotnet/api/system.xml.serialization.xmlserializer) 또는 [DataContractJsonSerializer](/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer) 를 사용 하십시오.

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

[System.IO](/dotnet/api/system.io) 네임 스페이스의 일부 형식 (예: [FileStream](/dotnet/api/system.io.filestream))은 이전 버전의 .NET Standard에서 사용할 수 없습니다. 그러나 Unity는 게임에서 사용할 수 있도록 [디렉터리](/dotnet/api/system.io.directory), [파일](/dotnet/api/system.io.file)및 **FileStream** 유형을 제공 합니다.

또는 UWP 앱 에서만 사용할 수 있는 [Windows 저장소](/uwp/api/Windows.Storage) api를 사용할 수 있습니다. 그러나 이러한 Api는 앱이 특정 저장소에 쓰도록 제한 하 고 전체 파일 시스템에 대 한 무료 액세스를 제공 하지 않습니다. 자세한 내용은 [파일, 폴더 및 라이브러리](../files/index.md) 를 참조 하십시오.

한 가지 중요 한 점은 [Close](/dotnet/api/system.io.stream.close) 메서드는 .NET Standard 2.0 이상 에서만 사용할 수 있다는 것입니다. 단, Unity는 확장 메서드를 제공 합니다. 대신 [Dispose](/dotnet/api/system.io.stream.dispose) 를 사용 하십시오.

### <a name="threading"></a>스레딩

스레드 이름 (예: [ThreadPool](/dotnet/api/system.threading.threadpool))의 일부 형식은 이전 버전의 .NET Standard에서 사용할 수 [없습니다.](/dotnet/api/system.threading) 이 경우Windows.System를 사용할 수 있습니다 [ . 대신 스레드](/uwp/api/windows.system.threading) 네임 스페이스를 합니다.

UWP 및 비 UWP 플랫폼을 준비 하기 위해 플랫폼 종속 컴파일을 사용 하 여 Unity 게임에서 스레딩을 처리할 수 있는 방법은 다음과 같습니다.

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

일부 **시스템 보안입니다** . [system.security.cryptography.x509certificates.x509certificate2](/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0&preserve-view=true)과 같은 네임 스페이스는 UWP 용 Unity 게임을 빌드할 때 사용할 수 없습니다. 이러한 경우에는 _*Windows 보안* *_ 을 사용 합니다. Api는 동일한 기능을 대부분 포함 합니다.

다음 예제에서는 지정 된 이름의 인증서 저장소에서 인증서를 가져옵니다.

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

WinRT 보안 Api를 사용 하는 방법에 대 한 자세한 내용은 [보안](../security/index.md) 을 참조 하세요.

### <a name="networking"></a>네트워킹

UWP 용 Unity 게임을 빌드할 때 [시스템 .net. 메일](/dotnet/api/system.net.mail?view=netstandard-2.0&preserve-view=true)등의 일부 _*시스템 &period;* * net-library_ 도 사용할 수 없습니다. 이러한 api의 대부분은 해당 하는 _*windows. 네트워킹과.* *_ 및 _*windows 웹* *_ 을 사용 합니다. WinRT Api를 통해 비슷한 기능을 얻을 수 있습니다. 자세한 내용은 [네트워킹 및 웹 서비스](../networking/index.md) 를 참조 하세요.

_ * 시스템 .Net. 메일 * *의 경우에는 [Windows-ApplicationModel. Email](/uwp/api/windows.applicationmodel.email) 네임 스페이스를 사용 합니다. 자세한 내용은 [전자 메일 보내기](../contacts-and-calendar/sending-email.md) 를 참조 하세요.

## <a name="see-also"></a>추가 정보

* [유니버설 Windows 플랫폼: .NET Scripting 백 엔드에서 .NET 형식이 누락 되었습니다.](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [UWP 앱 용 .NET 개요](/previous-versions/windows/apps/br230302(v=vs.140))
* [Unity UWP 포팅 가이드](https://unity3d.com/partners/microsoft/porting-guides)
