---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
description: " GET (/qosservers)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 02d24dbf1d189b759784dbbfa7052e2c218ec27e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632068"
---
# <a name="get-qosservers"></a>GET (/qosservers)
Xbox Live Compute 사용에 대 한 QoS 서버의 목록을 가져오려면 클라이언트에서 호출 하는 URI입니다. 이러한 Uri에 대 한 도메인이 `gameserverds.xboxlive.com` 고 `gameserverms.xboxlive.com`입니다.
 
  * [필요한 요청 헤더](#ID4EBB)
  * [필요한 응답 헤더](#ID4EUC)
  * [응답 본문](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>호스트 이름

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
에 요청을 수행할 때 다음 표에 나와 있는 헤더는 필요 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | 
| Content-Type| application/json| 전송 되는 데이터의 형식입니다.| 
| 호스트| gameserverds.xboxlive.com|  | 
| Content-Length|  | 요청 개체의 길이입니다.| 
| x-xbl-contract-version| 1| API 계약 버전입니다.| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
응답에는 항상 다음 표에 나와 있는 헤더 포함 됩니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/json| 응답 본문에는 데이터 형식입니다.| 
| Content-Length|  | 응답 본문의 길이입니다.| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>응답 본문
 
호출이 성공 하면 서비스는 다음 멤버로 구성 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| 배열 서버 정보입니다.| 
| serverFqdn| 서버의 정규화 된 도메인 이름입니다.| 
| serverSecureDeviceAddress| 서버의 안전한 장치 주소입니다.| 
| targetLocation| 서버의 지리적 위치입니다.| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>샘플 응답
 

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

  