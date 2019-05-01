---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 사용자 지정 SSL 인증서로 Device Portal 프로비전
description: Windows Device Portal HTTPS 통신에 사용할 사용자 지정 인증서를 사용 하 여 프로 비전 하는 방법에 알아봅니다.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp, 장치 포털
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ce4e45bc23f4efb636618bb4891b9d6e9d207490
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63774148"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>사용자 지정 SSL 인증서로 Device Portal 프로비전

Windows 10 크리에이터스 업데이트에서 Windows Device Portal은 장치 관리자가 HTTPS 통신에 사용할 사용자 지정 인증서를 설치하는 방법을 추가했습니다.

자체 PC에서 이 작업을 수행할 수도 있지만, 이 기능은 주로 기존의 인증서 인프라가 있는 기업을 대상으로 합니다.  

예를 들어 회사가 HTTPS를 통해 제공되는 인트라넷 웹 사이트의 인증서에 서명하는 데 사용하는 CA(인증 기관)가 있을 수 있습니다. 이 기능은 해당 인프라의 최상위에 있습니다.

## <a name="overview"></a>개요

기본적으로 장치 포털 자체 서명 된 루트 CA를 생성 하 고에서 수신 대기 중인 모든 끝점에 대 한 SSL 인증서를 서명 하는. 여기에는 포함 `localhost`, `127.0.0.1` 및 `::1`(IPv6 로컬 호스트)이 포함됩니다.

또한 장치의 호스트 이름(예: `https://LivingRoomPC`)과 장치에 할당된 각 링크 로컬 IP 주소(네트워크 어댑터당 최대 두 개[IPv4, IPv6])가 포함됩니다.
Device Portal의 네트워킹 도구를 살펴보면 장치의 링크 로컬 IP 주소를 확인할 수 있습니다. 이는 IPv4에 대해 `10.` 또는 `192.`, IPv6에 대해 `fe80:`으로 시작합니다.

기본 설정에서는 신뢰할 수 없는 루트 CA로 인해 브라우저에 인증서 경고가 표시될 수 있습니다. 특히 Device Portal에서 제공하는 SSL 인증서는 브라우저 또는 PC가 신뢰하지 않는 루트 CA에 의해 서명됩니다. 새 신뢰할 수 있는 루트 CA를 만들어서 이 문제를 해결할 수 있습니다.

## <a name="create-a-root-ca"></a>루트 CA 만들기

회사(또는 집)에 인증서 인프라가 설정되어 있지 않고 한 번만 수행해야 하는 경우에만 이 작업을 수행해야 합니다. 다음 PowerShell 스크립트는 _WdpTestCA.cer_라는 루트 CA를 만듭니다. 이 파일을 로컬 컴퓨터의 신뢰할 수 있는 루트 인증 기관에 설치하면 장치가 이 루트 CA에서 서명한 SSL 인증서를 신뢰하게 됩니다. 이 .cer 파일은 Windows Device Portal에 연결하려는 각 PC에 설치할 수 있습니다.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

이 파일이 생성되면 _WdpTestCA.cer_ 파일을 사용하여 SSL 인증서에 서명할 수 있습니다.

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>루트 CA로 SSL 인증서 만들기

SSL 인증서에는 두 가지의 중요한 기능이 있습니다. 하나는 암호화를 통한 연결 보안이며 다른 하나는 브라우저 바(Bing.com, 192.168.1.37 등)에 표시된 주소와 실제로 통신하고 있으며 악의적인 제3자가 아닌지 확인하는 기능입니다.

다음 PowerShell 스크립트는 `localhost` 끝점에 대한 SSL 인증서를 만듭니다. Device Portal에서 수신하는 각 끝점에는 자체 인증서가 필요합니다. 스크립트의 `$IssuedTo` 인수를 장치에 대한 다른 끝점인 호스트 이름, 로컬 호스트 및 IP 주소로 바꿀 수 있습니다.

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

여러 장치가 있는 경우 localhost .pfx 파일을 다시 사용할 수 있지만 각 장치에 대한 IP 주소 및 호스트 이름 인증서를 별도로 만들어야 합니다.

.Pfx 파일의 번들 생성 되 면 Windows Device Portal 로드 해야 합니다.

## <a name="provision-device-portal-with-the-certifications"></a>인증서로 Device Portal 프로비전

장치에 대해 만든 각 .pfx 파일의 경우 관리자 권한 명령 프롬프트에서 다음 명령을 실행해야 합니다.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

사용 예는 아래를 참조하세요.

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

인증서를 설치한 후, 변경 내용이 적용 되도록 서비스를 다시 시작 하면 됩니다.

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 주소는 시간이 지나면 변경될 수 있습니다.
많은 네트워크가 DHCP를 사용하여 IP 주소를 제공하므로 장치가 항상 이전에 사용했던 것과 동일한 IP 주소를 얻는 것은 아닙니다. 장치에서 IP 주소에 대한 인증서를 생성했고 해당 장치의 주소가 변경된 경우, Windows Device Portal에서 기존의 자체 서명된 인증서를 사용하여 새 인증서를 생성하고 사용자가 만든 인증서의 사용을 중지합니다. 이렇게 하면 브라우저에 인증서 경고 페이지가 다시 표시됩니다. 이러한 이유로 Device Portal에서 설정할 수 있는 호스트 이름을 통해 장치에 연결하는 것이 좋습니다. 이는 IP 주소와 상관없이 동일하게 유지됩니다.
