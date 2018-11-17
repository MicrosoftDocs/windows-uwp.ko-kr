---
author: M-Stahl
title: 디바이스 포털 Xbox 정보 API 참조
description: Xbox 장치 정보에 액세스 하는 방법을 알아봅니다.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
keywords: windows 10, uwp, xbox 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: 4b0e2bab0ce7d5525e8032809954ff656a74a61c
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7153494"
---
# <a name="xbox-info-api-reference"></a>Xbox 정보 API 참조   
이 API를 사용 하 여 Xbox One 장치 정보에 액세스할 수 있습니다.

## <a name="get-xbox-one-device-information"></a>Xbox One 장치 정보 가져오기

**요청**

Xbox One에 대 한 장치 정보를 얻을 수 있습니다.

메서드      | 요청 URI
:------     | :-----
GET | /ext/xbox/info
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**   
다음 필드가 있는 JSON 개체입니다.

* OsVersion-(문자열) 운영 체제의 버전입니다.
* OsEdition-(문자열)는 OS 버전 등 "2017 년 3 월" 또는 "1" QFE 2017 년 3 월 합니다.
* ConsoleId-(문자열) 콘솔의 id입니다.
* DeviceId-(문자열) 콘솔의 Xbox Live 장치 id입니다.
* SerialNumber-(문자열) 콘솔의 일련번호 합니다.
* DevMode-(문자열) 콘솔의 현재 개발자와 같은 모드를 "None" 또는 "소매"입니다.
* ConsoleType-예: "Xbox One" 또는 "Xbox One S" (문자열) 콘솔의 유형입니다.
* DevkitCertificateExpirationTime-(숫자)는 UTC 시간 (초) 때 콘솔의 개발자 키트 인증서가 만료 됩니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청에 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox