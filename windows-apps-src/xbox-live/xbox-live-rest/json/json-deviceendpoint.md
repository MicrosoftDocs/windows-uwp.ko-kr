---
title: DeviceEndpoint(JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
description: " DeviceEndpoint(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0eaa21072ebf14b6f6d959ff40af34724a45522f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618878"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint(JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
DeviceEndpoint 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| deviceName| 문자열| 선택 사항. 해당 하는 경우 장치에 대 한 이름입니다. 현재이 값이 사용 되지 않습니다.| 
| endpointUri| 문자열| 필수. 클라이언트 플랫폼 (Windows 또는 Windows Phone)에 푸시 알림 서비스 (WNS 또는 MPNS)에서 가져온 URL입니다.| 
| locale| 문자열| 필수. 원하는 언어가이 끝점에 보낸 알림입니다. 기본 설정 순서 대로 쉼표로 구분 된 값 목록이 될 수 있습니다. 예: "DE-DE, EN-US, en"입니다.| 
| 플랫폼| 문자열| 선택 사항. 현재 지원 되는 값은 "WindowsPhone" 및 "Windows"입니다. 지정 하지 않으면 장치 토큰에서 파생 됩니다.| 
| platformVersion| 문자열| 선택 사항. 이 문자열의 형식은 각 플랫폼에 특정 합니다. 현재이 값이 사용 되지 않습니다.| 
| systemId| GUID| 필수. "앱 인스턴스"에 대 한 고유 식별자 (장치/사용자 조합). 모범 사례 구현 설치/첫 번째-실행 시 임의 GUID를 생성 하는 앱 이며 앱의 후속 실행 시 해당 값을 사용 하 여 계속 합니다.| 
| titleId| 32 비트 부호 없는 정수| 필수. 제목 ID 서비스에 대 한 호출을 실행 하는 게임입니다.| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "https://cloud.notify.windows.com/.../",
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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>참고자료   