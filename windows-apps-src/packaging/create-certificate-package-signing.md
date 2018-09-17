---
author: laurenhughes
title: 패키지 서명용 인증서 만들기
description: PowerShell 도구로 앱 패키지 서명용 인증서를 만들고 내보냅니다.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: db2c360a881071db14a1e65ffe2cd9a5bb16f0fe
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3982775"
---
# <a name="create-a-certificate-for-package-signing"></a>패키지 서명용 인증서 만들기


이 문서에서는 PowerShell 도구를 사용하여 앱 패키지 서명용 인증서를 만들고 내보내는 방법을 설명합니다. [UWP 앱 패키지](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)에 Visual Studio를 사용하는 것도 좋지만, Visual Studio를 사용하여 앱을 개발하지 않은 경우 스토어용 앱을 수동으로 패키지할 수 있습니다.

> [!IMPORTANT] 
> Visual Studio를 사용하여 앱을 개발하는 경우, Visual Studio 마법사를 사용하여 인증서를 가져오고 앱 패키지에 서명하는 것이 좋습니다. 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

- **패키지된 앱 또는 패키지되지 않은 앱**  
AppxManifest.xml 파일이 들어 있는 앱 최종 앱 패키지에 서명하는 데 사용될 인증서를 만드는 동안 매니페스트 파일을 참조해야 합니다. 앱을 수동으로 패키지하는 방법에 대한 자세한 내용은 [MakeAppx.exe 도구를 사용하여 앱 패키지 만들기](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)를 참조하세요.

- **PKI(공개 키 인프라) Cmdlet**  
서명 인증서를 만들고 내보내려면 PKI cmdlet이 필요합니다. 자세한 내용은 [Public Key Infrastructure Cmdlets(공개 키 인프라 Cmdlet)](https://docs.microsoft.com/powershell/module/pkiclient/)를 참조하세요.

## <a name="create-a-self-signed-certificate"></a>자체 서명된 인증서 만들기

자체 서명된 인증서는 스토어에 앱을 게시할 준비가 되기 전 앱을 테스트하는 데 사용됩니다. 이 섹션에 설명된 단계에 따라 자체 서명된 인증서를 만듭니다.

### <a name="determine-the-subject-of-your-packaged-app"></a>패키지된 앱의 제목 결정  

인증서를 사용하여 앱 패키지에 서명하려면 인증서의 "제목"이 앱 매니페스트의 "Publisher" 섹션과 **일치해야** 합니다.

예를 들어, 앱의 AppxManifest.xml 파일의 "Identity" 섹션은 다음과 같습니다.
```
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

이 경우 "Publisher"는 "CN=Contoso Software, O=Contoso Corporation, C=US"이고 이는 인증서를 만드는 데 사용되어야 합니다. 

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>**New-SelfSignedCertificate**를 사용하여 인증서 만들기
**New-SelfSignedCertificate** PowerShell cmdlet을 사용하여 자체 서명된 인증서를 만듭니다. **New-SelfSignedCertificate**에는 사용자 지정할 매개 변수가 여러 개 있지만 이 문서에서는 **SignTool**에서 사용할 단순 인증서를 만드는 데 집중하겠습니다. 이 cmdlet의 예제와 사용법은 [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate)를 참조하세요.

이전 예제의 AppxManifest.xml 파일을 기반으로 다음 구문을 사용하여 인증서를 만들어야 합니다. 관리자 권한 PowerShell 프롬프트에서
```
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName <Your Friendly Name> -CertStoreLocation "Cert:\LocalMachine\My"
```

이 명령을 실행한 후 "-CertStoreLocation" 매개 변수에서 지정한 대로 로컬 인증서 스토어에 인증서가 추가됩니다. 명령의 결과 인증서의 지문도 생성 합니다.  

**참고**  
다음 명령을 사용하여 PowerShell 창에서 인증서를 볼 수 있습니다.
```
Set-Location Cert:\LocalMachine\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```
그러면 로컬 스토어의 모든 인증서가 표시됩니다.

## <a name="export-a-certificate"></a>인증서 내보내기 

로컬 스토어의 인증서를 PFX(Personal Information Exchange) 파일로 내보내려면 **Export-PfxCertificate** cmdlet을 사용합니다.

**Export-PfxCertificate**를 사용할 때 암호를 만들어서 사용하거나"-ProtectTo" 매개 변수를 사용하여 암호 없이 파일에 액세스할 수 있는 사용자나 그룹을 지정해야 합니다. "-Password" 또는 "-ProtectTo" 매개 변수를 사용하지 않는 경우 오류가 표시됩니다.

- **암호 사용**
```
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\LocalMachine\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

- **ProtectTo 사용**
```
Export-PfxCertificate -cert Cert:\LocalMachine\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

인증서를 만들고 내보낸 후에는 **SignTool**로 앱 패키지에 서명할 수 있습니다. 수동 패키지 프로세스의 다음 단계는 [Sign an app package using SignTool(SignTool을 사용하여 앱 패키지에 서명)](https://msdn.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool)을 참조하세요.

## <a name="security-considerations"></a>보안 고려 사항 
[로컬 머신 인증서 스토어](https://msdn.microsoft.com/windows/hardware/drivers/install/local-machine-and-current-user-certificate-stores)에 인증서를 추가하면 컴퓨터에 있는 모든 사용자의 인증서 신뢰에 영향이 있습니다. 인증서가 시스템 신뢰 손상을 방지하는 데 더 이상 필요하지 않을 경우 해당 인증서를 제거하는 것이 좋습니다.
