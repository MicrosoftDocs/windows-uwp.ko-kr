---
author: DelfCo
Description: "이 항목에서는 EDP(엔터프라이즈 데이터 보호) 시나리오에서 네트워크 연결을 만들기 전에 보호된 스레드 컨텍스트를 만드는 방법을 보여 줍니다."
MS-HAID: dev\_networking.tagging\_network\_connections\_with\_edp\_identity
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "EDP ID를 사용하여 네트워크 연결 태그 지정"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 2b960bbb5cf58991778e5c20bb915a202ecf6e04

---

# EDP ID를 사용하여 네트워크 연결 태그 지정

__참고__ Windows 10 버전 1511(빌드 10586) 이전에서는 EDP(엔터프라이즈 데이터 보호) 정책을 적용할 수 없습니다.

이 항목에서는 EDP(엔터프라이즈 데이터 보호) 시나리오에서 네트워크 연결을 만들기 전에 보호된 스레드 컨텍스트를 만드는 방법을 보여 줍니다. EDP 기능이 파일, 스트림, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보려면 [EDP(엔터프라이즈 데이터 보호)](../enterprise/edp-hub.md)를 참조하세요.

**참고** [EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)은 여러 EDP 시나리오에 대해 다룹니다.



## 필수 조건


-   **EDP 설정**

    [EDP를 위한 컴퓨터 설정](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)을 참조하세요.

-   **엔터프라이즈 인식(enterprise-enlightened) 앱 빌드 구현**

    자체적으로 엔터프라이즈 데이터가 엔터프라이즈를 통해 관리되도록 하는 앱을 엔터프라이즈 인식(enterprise-enlightened) 앱이라고 합니다. 인식 앱은 비인식 앱보다 더 강력하고 스마트하고 유연하며 신뢰할 수 있습니다. 앱은 제한된 **enterpriseDataPolicy** 기능을 선언하여 인식되도록 시스템에 알립니다. 그렇지만 기능을 설정하는 것 이상의 효과가 있습니다. 자세한 내용은 [엔터프라이즈 인식(enterprise-enlightened) 앱](../enterprise/edp-hub.md#enterprise-enlightened-apps)을 참조하세요.

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C\# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C\# 또는 Visual Basic에서 비동기 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

## 네트워크를 통해 엔터프라이즈 리소스에 액세스


이 시나리오에서는 인식 메일 앱이 엔터프라이즈 사서함과 개인 사서함을 모두 혼합한 사서함 집합을 동기화합니다. 앱은 [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025)에 대한 호출에 사용자의 ID를 전달하여 보호된 스레드 컨텍스트를 만듭니다. 그러면 해당 ID를 사용하여 동일한 스레드에서 나중에 만든 모든 네트워크 연결에 태그가 지정되며 엔터프라이즈의 정책에 의해 액세스가 제어되는 엔터프라이즈 네트워크 리소스에 액세스할 수 있습니다.

여기에서 "엔터프라이즈"는 사용자 ID가 속한 엔터프라이즈를 나타냅니다. [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025)는 정책 적용과 관계없이 [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706029) 개체를 반환합니다. 일반적으로 앱에서 혼합된 리소스를 처리하려는 경우 모든 ID에 대해 **CreateCurrentThreadNetworkContext**를 호출하도록 선택할 수 있습니다. 네트워크 리소스를 검색한 후 앱은 **ThreadNetworkContext**에서 **Dispose**를 호출하여 현재 스레드에서 모든 ID 태그를 지웁니다. 컨텍스트 개체를 처리하는 데 사용하는 패턴은 프로그래밍 언어에 따라 달라집니다.

ID를 알 수 없는 경우 앱은 [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) API를 사용하여 리소스의 네트워크 주소에서 엔터프라이즈 정책 관리 ID를 쿼리할 수 있습니다.

**참고** 코드 예제에서 알 수 있듯이 [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025)에 대한 올바른 사용 패턴은 적용 범위를 최소로 유지하는 것입니다. 엔터프라이즈 네트워크 컨텍스트를 설정하고, 네트워크 연결을 만든 후 컨텍스트를 되돌리고, 연결을 사용한 후 닫아야 합니다. 아래 코드 예제에서는 세부 정보를 보여 줍니다. 스레드에 엔터프라이즈 네트워크 컨텍스트가 설정된 동안에는 해당 스레드에서 파일을 만들지 않는 것이 중요합니다. 이렇게 하면 개인 파일로 설정 여부에 관계없이 파일이 자동으로 암호화됩니다. 이는 가능한 한 빨리 컨텍스트를 되돌리도록 권장되는 이유 중 하나입니다.



```CSharp
using Windows.Data.Xml.Dom;
using Windows.Networking;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;
using Windows.Web.Http;

...

public static async void SyncMailbox(string identity)
{
    HttpClient client = null;
    // Create a protected network context for "identity" on the current
    // thread. Network connections made after this call will be tagged
    // to "identity", and will have policy enforced. This is required
    // to access enterprise network resources for "identity".
    using (ThreadNetworkContext threadNetworkContext = 
        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        // Retrieve new mail.
        client = new HttpClient();
    }

    string xmlResponse = await client.GetStringAsync
        (new Uri("https://contosomail/getnewmail?user=" + identity));

    // Now, process mail data received.
    var xmlDocument = new XmlDocument();
    xmlDocument.LoadXml(xmlResponse);
    foreach (IXmlNode mailNode in xmlDocument.GetElementsByTagName("mail"))
    {
        // Code to process text data in mail (body, recipients etc.)
        // would go here ...

        // Processes resource links in mail body.
        foreach (IXmlNode childNode in mailNode.ChildNodes)
        {
            if (childNode.NodeName == "resource")
            {
                var resourceUri = new Uri(childNode.InnerText);

                // Check if the resource is on an enterprise-managed endpoint.
                string resourceIdentity =
                    await ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync
                        (new HostName(resourceUri.Host));
                if (!string.IsNullOrEmpty(resourceIdentity))
                {
                    using (ThreadNetworkContext threadNetworkContext =
                        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(resourceIdentity))
                    {
                        client = new HttpClient();
                    }
                    IBuffer data = await client.GetBufferAsync(resourceUri);
                    // Code to process retrieved resource data would go here...
                }
            }
        }
    }
}
```

**참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.



## 관련 항목


[EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 






<!--HONumber=Jun16_HO4-->


