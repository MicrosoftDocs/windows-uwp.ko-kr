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
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ccdef5c9462f18c1a7044fcc3a683c749177349
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1070930"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>사용자 지정 SSL 인증서로 Device Portal 프로비전
Windows 10 작성자 업데이트 Windows 장치 포털 HTTPS 통신에 사용 하기 위해 사용자 지정 인증서를 설치 하려면 장치 관리자를 추가 합니다. 

이렇게 하려면 자신의 pc를 하는 동안이 기능은 기업에는 기존 인증서 인프라에 대 한 대부분 제공 됩니다.  

예, 회사 HTTPS를 통해 처리 하는 인트라넷 웹사이트에 대 한 인증서에 서명 하는데 사용 하 여 인증 기관 (CA)에 있을 수 있습니다. 이 기능은 인프라를 나타냅니다. 

## <a name="overview"></a>개요
기본적으로 장치 포털 자체 서명 된 루트 CA를 생성 하 고를 사용 하 여 하는 모든 끝점에서 수신 대기에 대 한 SSL 인증서에 서명 합니다. 여기에 포함 됩니다 `localhost`, `127.0.0.1`, 및 `::1` (i p v 6 localhost).

또한는 포함 되어 장치의 호스트 이름 (예 `https://LivingRoomPC`) 및 장치에 할당 된 각 링크 로컬 IP 주소 (최대 2 개의 [i p v 4, i p v 6] 네트워크 어댑터 당) 합니다. 장치 포털의 네트워킹 도구를 검토 하 여 장치에 대 한 링크 로컬 IP 주소를 확인할 수 있습니다. 부터 설명 하겠습니다 `10.` 또는 `192.` i p v 4, 또는 `fe80:` i p v 6에 대 한 합니다. 

기본 설치 프로그램에서 신뢰할 수 없는 루트 CA로 인해 인증서 경고는 브라우저에 나타날 수 있습니다. 특히, 장치 포털에서 제공 하는 SSL 인증서의 루트 브라우저 또는 PC를 신뢰 하지 CA에 의해 서명 됩니다. 새 신뢰할 수 있는 루트 CA를 만들어서이 해결할 수 있습니다.

## <a name="create-a-root-ca"></a>루트 CA를 만들기

회사 (또는 집)를 설정 하는 인증서 인프라가 없는 경우에 수행 해야 하 고 한 번만 수행 해야 합니다. 다음 PowerShell 스크립트 _WdpTestCA.cer_를 호출 하는 CA 루트를 만듭니다. 이 파일을 설치 하는 로컬 컴퓨터의 신뢰할 수 있는 루트 인증 기관에이 루트 CA에 의해 서명 된 SSL 인증서를 신뢰 하도록 장치로 될 수 있습니다. 수 (및 해야) 설치한 Windows 장치 포털에 연결 하려면 각 PC에서이.cer 파일입니다.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

이 만든 다음에 SSL 인증서에 서명 하 _WdpTestCA.cer_ 파일을 사용할 수 있습니다. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>루트 CA 사용 하 여 SSL 인증서 만들기

SSL 인증서가 두 중요 한 기능: 암호화를 통해 대 한 연결을 보호 하 고 브라우저 표시줄에 표시 되는 주소와 통신 실제로 확인 (영문) (반응을, 192.168.1.37, 등) 및 악의적인 제 3 자 하지 않습니다.

다음 PowerShell 스크립트에 대 한 SSL 인증서를 만들고는 `localhost` 끝점입니다. 장치 포털에서 수신 대기 하는 각 끝점에서 자체 인증서; 필요 바꿀 수는 `$IssuedTo` 장치에 대 한 서로 다른 끝점의 각를 사용 하 여 스크립트에서 인수: 호스트 이름, localhost, 및 IP 주소입니다.

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

장치가 여러 개인 경우 localhost.pfx 파일을 다시 사용할 수 있지만 각 장치에 대 한 IP 주소와 호스트 이름을 인증서를 개별적으로 만들기를 계속 해야 합니다.

.Pfx 파일의 번들 생성 될 때 Windows 장치 포털에이 로드 해야 합니다. 

## <a name="provision-device-portal-with-the-certifications"></a>certification(s) 사용 하는 프로 비전 장치 포털

각.pfx 파일에 사용자가 만든 장치에 대 한 관리자 권한 명령 프롬프트에서 다음 명령을 실행 해야 합니다.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

예: 사용 현황 아래를 참조 합니다.
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

인증서를 설치한 후 단순히 서비스를 다시 시작 변경 내용이 적용 되도록 합니다.

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 주소는 시간이 지남에 따라 변경 수 있습니다.
대부분의 네트워크 DHCP를 사용 하 여 장치는 이전에 사용 했던 동일한 IP 주소를 확인할 항상 하지 IP 주소를 제공 합니다. 장치에서 IP 주소에 대 한 인증서를 만들었고 및 장치의 주소 변경 하는 경우 Windows 장치 포털은 자체 서명 된 인증서의 기존를 사용 하 여 새 인증서를 생성 하 고 만든 하나를 사용 하 여 중지 됩니다. 인증서 경고 페이지를 다시 브라우저에 표시 될 수 있습니다. 이러한 이유로 장치 포털에서 설정할 수 있는 자신의 호스트 이름을 통해 장치에 연결 하는 것이 좋습니다. 이러한 머무르는 IP 주소에 관계 없이 동일 합니다.
