---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
author: KevinAsgari
description: " GET (/qosservers)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 71a4787bf6b139d1a638ec783c0293d70a8ee239
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4747532"
---
# <a name="get-qosservers"></a>GET (/qosservers)
URI를 사용 하 여 Xbox Live 계산 사용 하기 위해 사용할 수 있는 QoS 서버의 목록을 가져올 수는 클라이언트에 의해 호출 합니다. 이러한 Uri에 대 한 도메인은 `gameserverds.xboxlive.com` 및 `gameserverms.xboxlive.com`.
 
  * [필요한 요청 헤더](#ID4EBB)
  * [필수 응답 헤더](#ID4EUC)
  * [응답 본문](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>호스트 이름

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
다음 표에 표시 된 헤더는 요청을 만들 때 필요 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | 
| 콘텐츠 유형| application/json| 전송 되는 데이터의 형식입니다.| 
| 호스트| gameserverds.xboxlive.com|  | 
| Content-Length|  | 요청 개체의 길이입니다.| 
| x xbl-계약 버전| 1| API 계약 버전입니다.| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
응답에는 다음 표에 표시 된 헤더 항상 포함 됩니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 콘텐츠 유형| application/json| 응답 본문에는 데이터의 형식입니다.| 
| Content-Length|  | 응답 본문의 길이입니다.| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>응답 본문
 
호출 되 면 서비스는 다음 멤버가 포함 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| 서버 정보의 배열입니다.| 
| serverFqdn| 정규화 된 도메인 이름 서버입니다.| 
| serverSecureDeviceAddress| 보안 장치 주소 서버입니다.| 
| targetLocation| 지리적 위치 서버입니다.| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{ 
  "qosServers" : 
  [ 
    { "serverFqdn" : "xblqosncus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "North Central US" },
    { "serverFqdn" : "xblqoswus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "West US" },
  ]
}

      
```

   
<a id="ID4EBF"></a>

 
## <a name="see-also"></a>참고 항목
 [/qosservers](uri-qosservers.md)

  