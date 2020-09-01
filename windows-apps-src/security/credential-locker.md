---
title: 자격 증명 보관
description: 이 문서에서는 UWP (유니버설 Windows 플랫폼) 앱에서 자격 증명 보관을 사용 하 여 사용자 자격 증명을 안전 하 게 저장 및 검색 하 고 사용자의 Microsoft 계정를 사용 하 여 장치 간에 로밍 하는 방법을 설명 합니다.
ms.assetid: 7BCC443D-9E8A-417C-B275-3105F5DED863
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 1a9f47a0f48d00c898bdb6df71f5cbd858b03bc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167267"
---
# <a name="credential-locker"></a>자격 증명 보관




이 문서에서는 UWP (유니버설 Windows 플랫폼) 앱에서 자격 증명 보관을 사용 하 여 사용자 자격 증명을 안전 하 게 저장 및 검색 하 고 사용자의 Microsoft 계정를 사용 하 여 장치 간에 로밍 하는 방법을 설명 합니다.

예를 들어 미디어 파일, 소셜 네트워킹 등의 보호 된 리소스에 액세스 하기 위해 서비스에 연결 하는 앱이 있습니다. 서비스에는 각 사용자에 대 한 로그인 정보가 필요 합니다. 사용자의 사용자 이름과 암호를 가져온 다음 사용자를 서비스에 로그인 하는 데 사용 되는 UI를 앱에 빌드 했습니다. 자격 증명 보관 API를 사용 하 여 사용자에 대 한 사용자 이름 및 암호를 저장 하 고, 쉽게 검색 하 고, 사용자가 다음에 앱을 열 때 해당 사용자에 게 자동으로 로그인 할 수 있습니다.

CredentialLocker에 저장 된 사용자 자격 증명은 만료 *되지* 않으며, [**applicationdata**](/uwp/api/windows.storage.applicationdata.roamingstoragequota) *의 영향을 받지 않으며,* 기존의 로밍 데이터와 같은 비활성으로 인해 제거 *되지* 않습니다. 그러나 CredentialLocker에서 앱 당 최대 20 개의 자격 증명을 저장할 수 있습니다.

자격 증명 보관은 도메인 계정에 대해 약간 다르게 작동 합니다. Microsoft 계정와 함께 저장 된 자격 증명이 있고 해당 계정을 도메인 계정 (예: 직장에서 사용 하는 계정)과 연결 하는 경우 자격 증명이 해당 도메인 계정으로 로밍 됩니다. 그러나 도메인 계정으로 로그온 할 때 추가 된 새 자격 증명은 로밍되지 않습니다. 이렇게 하면 도메인의 개인 자격 증명이 도메인 외부에 노출 되지 않습니다.

## <a name="storing-user-credentials"></a>사용자 자격 증명 저장


1.  PasswordVault 네임 스페이스에서 [**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault) 개체를 사용 하 여 자격 증명 보관에 대 한 참조를 [**가져옵니다.**](/uwp/api/Windows.Security.Credentials)
2.  앱에 대 한 식별자, 사용자 이름 및 암호를 포함 하는 [**passwordcredential**](/uwp/api/Windows.Security.Credentials.PasswordCredential) 개체를 만들고 [**PasswordVault**](/uwp/api/windows.security.credentials.passwordvault.add) 메서드에 전달 하 여 자격 증명을 보관에 추가 합니다.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Add(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="retrieving-user-credentials"></a>사용자 자격 증명 검색


[**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault) 개체에 대 한 참조를 가져온 후 자격 증명 보관에서 사용자 자격 증명을 검색 하는 몇 가지 옵션이 있습니다.

-   [**PasswordVault RetrieveAll**](/uwp/api/windows.security.credentials.passwordvault.retrieveall) 메서드를 사용 하 여 사용자가 보관에서 앱에 대해 제공한 모든 자격 증명을 검색할 수 있습니다.

-   저장 된 자격 증명의 사용자 이름을 알고 있는 경우 [**PasswordVault. FindAllByUserName**](/uwp/api/windows.security.credentials.passwordvault.findallbyusername) 메서드를 사용 하 여 해당 사용자 이름의 자격 증명을 모두 검색할 수 있습니다.

-   저장 된 자격 증명에 대 한 리소스 이름을 알고 있는 경우 [**PasswordVault. FindAllByResource**](/uwp/api/windows.security.credentials.passwordvault.findallbyresource) 메서드를 사용 하 여 해당 리소스 이름에 대 한 모든 자격 증명을 검색할 수 있습니다.

-   마지막으로, 자격 증명에 대 한 사용자 이름과 리소스 이름을 모두 알고 있는 경우 [**PasswordVault**](/uwp/api/windows.security.credentials.passwordvault.retrieve) 메서드를 사용 하 여 해당 자격 증명만 검색할 수 있습니다.

리소스 이름을 앱에 전역적으로 저장 하 고 해당 자격 증명을 찾은 경우 자동으로 사용자를 로그온 하는 예를 살펴보겠습니다. 동일한 사용자에 대해 여러 자격 증명을 찾은 경우 사용자에 게 로그온 할 때 사용할 기본 자격 증명을 선택 하도록 요청 합니다.

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


자격 증명 보관에서 사용자 자격 증명을 삭제 하는 작업은 빠른 2 단계 프로세스 이기도 합니다.

1.  PasswordVault 네임 스페이스에서 [**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault) 개체를 사용 하 여 자격 증명 보관에 대 한 참조를 [**가져옵니다.**](/uwp/api/Windows.Security.Credentials)

2.  삭제 하려는 자격 증명을 [**PasswordVault**](/uwp/api/windows.security.credentials.passwordvault.remove) 메서드에 전달 합니다.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Remove(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="best-practices"></a>모범 사례


암호에는 자격 증명 보관만 사용 하 고, 더 큰 데이터 blob에는 사용 하지 마십시오.

다음 조건을 충족 하는 경우에만 자격 증명 보관에 암호를 저장 합니다.

-   사용자가 성공적으로 로그인 했습니다.
-   사용자가 암호를 저장 하도록 옵트인 했습니다.

앱 데이터 또는 로밍 설정을 사용 하 여 일반 텍스트로 자격 증명을 저장 하지 않습니다.