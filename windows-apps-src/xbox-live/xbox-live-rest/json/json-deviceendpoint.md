---
title: DeviceEndpoint(JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
author: KevinAsgari
description: " DeviceEndpoint(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7dafe60c0d26846d10113a641986842cc52e0334
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5918475"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint(JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
DeviceEndpoint 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| deviceName| string| 선택 사항입니다. 장치에 해당 하는 경우에 대 한 이름입니다. 현재이 값은 사용 되지 않습니다.| 
| endpointUri| string| 필수. (Windows 또는 Windows Phone) 클라이언트 플랫폼에 푸시 알림 서비스 (WNS 또는 MPNS)에서 가져온의 URL입니다.| 
| locale| string| 필수. 이 끝점에 전송 되는 알림의 원하는 언어입니다. 쉼표로 구분 된 값 우선 순위에 목록이 수 있습니다. 예: "DE-DE, EN-US, en".| 
| 플랫폼| string| 선택 사항입니다. 현재 지원 되는 값은 "WindowsPhone" 및 "Windows"입니다. 지정 하지 않으면 장치 토큰에서 파생 됩니다.| 
| platformVersion| string| 선택 사항입니다. 이 문자열의 형식을 각 플랫폼에 한정 됩니다. 현재이 값은 사용 되지 않습니다.| 
| systemId| GUID| 필수. "앱 인스턴스"에 대 한 고유 식별자 (디바이스/사용자 조합). 모범 사례 구현 시 설치/첫 실행, 임의의 GUID를 생성 하려면 앱에 대 한 이며 앱의 후속 실행에서 해당 값을 사용 하 여 계속 합니다.| 
| titleId| 32 비트 부호 없는 정수| 필수. 서비스에 대 한 호출을 실행 하는 게임의 제목 ID입니다.| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "http://cloud.notify.windows.com/.../",
  "platform": "Windows",
  "platformVersion": "1.0",
  "deviceName": "Test Endpoint",
  "locale": "en-US",
  "titleId": 1297290219
}
    
```

  
<a id="ID4EPD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERD"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>참조   