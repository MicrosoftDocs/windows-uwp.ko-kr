---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 사용자 지정 SSL 인증서로 Device Portal 프로비전
description: TBD
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 10, uwp, windows 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3373675"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>사용자 지정 SSL 인증서로 Device Portal 프로비전
10 작성자 Windows Update Windows 장치 포털 HTTPS 통신에 사용 하기 위해 사용자 지정 인증서를 설치 하려면 장치 관리자를 추가 합니다. 

이렇게 하려면 사용자의 PC에, 한 동안이 기능은 기존 인증서 인프라 구축 되어 있는 기업에 대 한 대부분 것입니다.  

예를 들어, 회사 인트라넷 웹 사이트를 HTTPS를 통해 서비스에 대 한 인증서에 서명 하기 위해 인증 기관 (CA) 있을 수 있습니다. 이 기능은 구조를 나타냅니다. 

## <a name="overview"></a>개요
기본적으로 장치 포털 자체 서명 된 루트 CA를 생성 하 고 모든 끝점에서 수신 대기에 대 한 SSL 인증서를 서명 하는 사용 하. 이때 `localhost`, `127.0.0.1`, 및 `::1` (IPv6 로컬 호스트).

장치의 호스트 이름에는 또한 포함 되어 (예를 들어, `https://LivingRoomPC`)와 장치에 할당 된 각 링크 로컬 IP 주소 (최대 2 개의 [IPv4, IPv6] 네트워크 어댑터 당). 네트워킹 도구 장치 포털에서 확인 하 여 장치에 대해 로컬 링크 IP 주소를 확인할 수 있습니다. 처음부터 `10.` 또는 `192.` i p v 4를 또는 `fe80:` i p v 6입니다. 

기본 설정에서는 신뢰할 수 없는 루트 CA로 인해 인증서 경고가 브라우저에 나타날 수 있습니다. 특히 장치 포털에서 제공 되는 SSL 인증서가 루트 CA 신뢰 하지 브라우저 또는 PC 서명한. 새 신뢰할 수 있는 루트 CA를 만들어이 해결할 수 있습니다.

## <a name="create-a-root-ca"></a>루트 CA를 만들어야

회사 (또는 가정)를 설정 하는 인증서 인프라가 없는 경우에 수행 해야 하 고 한 번만 수행 해야 합니다. 다음 PowerShell 스크립트를 사용 하는 _WdpTestCA.cer_라는 CA 루트를 만듭니다. 이 파일을 로컬 컴퓨터의 신뢰할 수 있는 루트 인증 기관에 설치 하면이 루트 CA에 의해 서명 된 SSL 인증서를 신뢰 하도록 장치. (하 야)를 설치한 Windows 장치 포털에 연결 하려는 각 PC에서이.cer 파일.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

이 생성 되 면 SSL 인증서를 서명 하는 _WdpTestCA.cer_ 파일을 사용할 수 있습니다. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>루트 CA 사용 하 여 SSL 인증서를 만들

SSL 인증서는 두 가지 중요 한 기능: 연결 암호화를 통한 보안 및 통신 하 고 있음을 실제로 브라우저 표시줄에 표시 된 주소를 사용 하 여 확인 (반응을, 192.168.1.37, 등) 및 악의적인 제 3 자가 아닙니다.

다음 PowerShell 스크립트 작성에 대 한 SSL 인증서에서 `localhost` 끝점입니다. 각 포털 장치에서 수신 하는 끝점 필요 자체 인증서입니다. 바꿀 수는 `$IssuedTo` 장치에 각각 서로 다른 끝점을 사용 하 여 스크립트의 인수: 호스트 이름, 로컬 호스트와 IP 주소.

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

여러 장치를 사용 하는 경우 localhost.pfx 파일을 다시 사용할 수 있지만 각 장치에 대해 IP 주소 및 호스트 이름 인증서를 별도로 만들 필요 합니다.

.Pfx 파일의 번들 생성 될 때 Windows 장치 포털에 로드 해야 합니다. 

## <a name="provision-device-portal-with-the-certifications"></a>Certification(s)는 장치 포털 구축

각.pfx 파일 만든 장치에서 상승된 된 명령 프롬프트에서 다음 명령을 실행 해야 합니다.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

사용 예를 들어 아래를 참조 하십시오.
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

인증서를 설치 하면 서비스를 다시 시작 변경 내용을 적용:

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 주소는 시간이 지나면서 변경 됩니다.
대부분의 네트워크는 DHCP를 사용 하 여 IP 주소를 부여할 장치 항상 이전에 사용 했던 같은 IP 주소를 얻을 하지 않습니다. 장치의 IP 주소에 대 한 인증서를 작성 한 및 장치의 주소 변경 Windows 장치 포털은 기존의 자체 서명 된 인증서를 사용 하 여 새 인증서를 생성 합니다 고 만든 것을 사용 하 여 중지 됩니다. 이 인해 인증서 경고 페이지를 다시 브라우저에 표시 됩니다. 이러한 이유로 장치 포털에서 설정할 수 있는 해당 호스트 이름을 통해 해당 장치에 연결 하는 것이 좋습니다. 이 IP 주소에 상관 없이 동일 하 게 유지 됩니다.
