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
keywords: windows 10, uwp, 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4566091"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>사용자 지정 SSL 인증서로 Device Portal 프로비전
Windows 10 크리에이터 스 업데이트 Windows Device Portal HTTPS 통신에 사용 하기 위해 사용자 지정 인증서를 설치 하는 장치 관리자를 추가 합니다. 

동안 자신의 PC에서 이렇게 하려면,이 기능은 주로 기존 인증서 인프라가 엔터프라이즈를 위한 것입니다.  

예를 들어 회사 인트라넷 웹 사이트 HTTPS를 통해 제공 되는 인증서에 서명 하는 데 사용 하는 인증 기관 (CA) 있을 수 있습니다. 이 기능은 인프라를 나타냅니다. 

## <a name="overview"></a>개요
기본적으로 디바이스 포털 자체 서명 된 루트 CA를 생성 하 고에서 수신 대기 중인 모든 끝점에 대 한 SSL 인증서에 서명 하는 사용 합니다. 이때 `localhost`, `127.0.0.1`, 및 `::1` (IPv6 localhost).

디바이스의 호스트 이름도 포함 됩니다 (예를 들어 `https://LivingRoomPC`) 및 장치에 할당 된 각 링크 로컬 IP 주소 (최대 2 개의 [IPv4, IPv6] 네트워크 어댑터 당). 장치 포털에서 네트워킹 도구를 통해 장치에 대 한 링크 로컬 IP 주소를 확인할 수 있습니다. 사용 하 여 시작 합니다 `10.` 또는 `192.` ipv4, 또는 `fe80:` ipv6 합니다. 

기본 설치에서 신뢰할 수 없는 루트 CA 인해 인증서 경고가 브라우저에 나타날 수 있습니다. 특히, 장치 포털에서 제공 하는 SSL 인증서 루트 브라우저 또는 PC를 신뢰할 수 없는 CA에서 서명 됩니다. 새로운 신뢰할 수 있는 루트 CA를 만들어이 해결할 수 있습니다.

## <a name="create-a-root-ca"></a>루트 CA 만들기

회사 (또는 홈) 설정, 인증서 인프라가 없는 경우에 수행 해야 하 고 한 번만 수행 해야 합니다. 다음 PowerShell 스크립트 루트 CA _WdpTestCA.cer_라는 만듭니다. 로컬 컴퓨터의 신뢰할 수 있는 루트 인증 기관에이 파일을 설치 하면 디바이스가이 루트 CA에서 서명 된 SSL 인증서를 신뢰 하도록 합니다. 수 있습니다 (및 해야) 설치한 Windows 장치 포털에 연결 하려는 각 PC에서이.cer 파일.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

이 생성 되 면 SSL 인증서를 서명 하려면 _WdpTestCA.cer_ 파일을 사용할 수 있습니다. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>루트 CA 사용 하 여 SSL 인증서 만들기

SSL 인증서가 두 가지 중요 한 기능: 암호화를 통해 연결을 보호 하 고 브라우저 표시줄에 표시 된 주소와 통신 실제로 확인 (192.168.1.37, Bing.com 등) 및 악성 제 3 자 되지 않습니다.

다음 PowerShell 스크립트에 대 한 SSL 인증서를 만듭니다는 `localhost` 끝점입니다. 장치 포털에서 수신 대기 하는 각 끝점에는 자체 인증서 필요 바꿀 수는 `$IssuedTo` 장치에 대 한 다른 끝점의 각 스크립트에서 인수: 호스트 이름, 로컬 호스트 및 IP 주소입니다.

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

여러 장치가 있는 경우 localhost.pfx 파일을 다시 사용할 수 있지만 각 장치에 대 한 IP 주소와 호스트 이름의 인증서를 별도로 만들기 여전히 필요 합니다.

.Pfx 파일의 번들을 생성할 때 Windows Device Portal에 로드 해야 합니다. 

## <a name="provision-device-portal-with-the-certifications"></a>인증을 사용 하 여 Device Portal 프로 비전

각.pfx 파일을 만든 장치를 관리자 권한 명령 프롬프트에서 다음 명령을 실행 해야 합니다.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

사용 예를 들어 아래를 참조 하세요.
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

인증서를 설치한 후 간단 하 게 서비스를 다시 시작 변경 내용을 적용 되도록 합니다.

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 주소는 시간이 지남에 따라 변경 됩니다.
많은 네트워크에서 DHCP를 사용 하 여 장치 하지 않는 이전에 사용 했던 동일한 IP 주소를 얻기 위해 항상 IP 주소를 제공 합니다. 장치에서 IP 주소에 대 한 인증서를 만들고 및 디바이스의 주소 변경 된 경우 Windows Device Portal에서 기존 자체 서명 된 인증서를 사용 하 여 새 인증서를 생성 하 고 만든 사용 중지 됩니다. 이렇게 하면 인증서 경고 페이지를 다시 브라우저에 표시 합니다. 이러한 이유로 장치 포털에서 설정할 수 있는 해당 호스트 이름을 통해 장치에 연결 하는 것이 좋습니다. 이 IP 주소에 관계 없이 동일 하 게 유지 됩니다.
