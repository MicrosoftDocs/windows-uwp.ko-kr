---
title: 자격 증명 보관
description: 이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 자격 증명 보관을 사용하여 사용자 자격 증명을 안전하게 저장 및 검색하고 사용자의 Microsoft 계정을 사용하여 장치 간에 로밍하는 방법을 설명합니다.
ms.assetid: 7BCC443D-9E8A-417C-B275-3105F5DED863
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: b7ac2a625b3769377ed6c8dddce3ca25177dee5f
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338785"
---
# <a name="credential-locker"></a>자격 증명 보관




이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 자격 증명 보관을 사용하여 사용자 자격 증명을 안전하게 저장 및 검색하고 사용자의 Microsoft 계정을 사용하여 장치 간에 로밍하는 방법을 설명합니다.

예를 들어 미디어 파일 또는 소셜 네트워킹 같은 보호된 리소스에 액세스하기 위해 서비스에 연결하는 앱이 있습니다. 서비스에 각 사용자에 대한 로그인 정보가 필요합니다. 사용자의 사용자 이름 및 암호를 가져오는 UI를 앱에 빌드했습니다. 이 정보는 사용자를 서비스에 로그인시키는 데 사용됩니다. 자격 증명 보관 API를 사용하면 사용자의 사용자 이름 및 암호를 저장하고, 다음에 사용자가 앱을 열 때 쉽게 검색하여 장치에 상관없이 사용자를 자동으로 로그인시킬 수 있습니다.

CredentialLocker에 저장된 사용자 자격 증명은 만료되지 *않고* [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)의 영향을 받지 *않으며* 기존 로밍 데이터와 같은 비활성 상태로 인해 삭제되지 *않습니다*. 그러나 CredentialLocker에서 앱당 최대 20개의 자격 증명을 저장할 수 있습니다.

도메인 계정의 경우 자격 증명 보관이 약간 다른 방식으로 작동합니다. Microsoft 계정과 함께 저장된 자격 증명이 있으며 해당 계정을 도메인 계정(예: 회사에서 사용하는 계정)과 연결하는 경우 자격 증명이 도메인 계정에 로밍됩니다. 그러나 도메인 계정으로 로그온한 상태에서 추가된 새 자격 증명은 로밍되지 않습니다. 따라서 도메인의 개인 자격 증명은 도메인 외부에 노출되지 않습니다.

## <a name="storing-user-credentials"></a>사용자 자격 증명 저장


1.  [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 네임스페이스의 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 개체를 사용하여 자격 증명 보관 참조를 가져옵니다.
2.  앱의 식별자, 사용자 이름 및 암호가 포함된 [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) 개체를 만들고 [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) 메서드에 전달하여 자격 증명을 보관에 추가합니다.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Add(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="retrieving-user-credentials"></a>사용자 자격 증명 검색


[**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 개체 참조를 가져온 후 자격 증명 보관에서 사용자 자격 증명을 검색하는 여러 가지 옵션이 있습니다.

-   [**PasswordVault.RetrieveAll**](https://msdn.microsoft.com/library/windows/apps/br227088) 메서드를 사용하면 보관에서 앱에 대해 제공된 모든 자격 증명을 검색할 수 있습니다.

-   저장된 자격 증명의 사용자 이름을 알고 있는 경우 [**PasswordVault.FindAllByUserName**](https://msdn.microsoft.com/library/windows/apps/br227084) 메서드를 사용하여 해당 사용자 이름에 대한 모든 자격 증명을 검색할 수 있습니다.

-   저장된 자격 증명의 리소스 이름을 알고 있는 경우 [**PasswordVault.FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083) 메서드를 사용하여 해당 리소스 이름에 대한 모든 자격 증명을 검색할 수 있습니다.

-   마지막으로, 자격 증명의 사용자 이름과 리소스 이름을 둘 다 알고 있는 경우 [**PasswordVault.Retrieve**](https://msdn.microsoft.com/library/windows/apps/br227087) 메서드를 사용하여 해당 자격 증명만 검색할 수 있습니다.

앱에 전체적으로 리소스 이름을 저장했으며 해당 자격 증명이 있을 경우 사용자를 자동으로 로그인하는 예제를 살펴보겠습니다. 동일한 사용자에 대한 자격 증명이 여러 개 발견되면 로그온할 때 사용할 기본 자격 증명을 선택하라는 메시지가 표시됩니다.

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    var loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }

    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}


private Windows.Security.Credentials.PasswordCredential GetCredentialFromLocker()
{
    Windows.Security.Credentials.PasswordCredential credential = null;

    var vault = new Windows.Security.Credentials.PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);
    if (credentialList.Count > 0)
    {
        if (credentialList.Count == 1)
        {
            credential = credentialList[0];
        }
        else
        {
            // When there are multiple usernames,
            // retrieve the default username. If one doesn't
            // exist, then display UI to have the user select
            // a default username.

            defaultUserName = GetDefaultUserNameUI();

            credential = vault.Retrieve(resourceName, defaultUserName);
        }
    }

    return credential;
}
```

## <a name="deleting-user-credentials"></a>사용자 자격 증명 삭제


두 단계로 이루어진 빠른 프로세스를 통해 자격 증명 보관에서 사용자 자격 증명을 삭제할 수도 있습니다.

1.  [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 네임스페이스의 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 개체를 사용하여 자격 증명 보관 참조를 가져옵니다.

2.  삭제하려는 자격 증명을 [**PasswordVault.Remove**](https://msdn.microsoft.com/library/windows/apps/hh701242) 메서드에 전달합니다.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Remove(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="best-practices"></a>모범 사례


자격 증명 보관은 암호에만 사용하고 큰 데이터 blob에는 사용하지 않습니다.

다음 조건이 충족되는 경우에만 암호를 자격 증명 보관에 저장합니다.

-   사용자가 로그인했습니다.
-   사용자가 암호를 저장하도록 선택했습니다.

앱 데이터 또는 로밍 설정을 사용하여 자격 증명을 일반 텍스트로 저장하지 마세요.
