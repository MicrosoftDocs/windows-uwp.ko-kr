---
author: M-Stahl
title: 장치 포털 Xbox 정보 API 참조 (영문)
description: Xbox 장치 정보에 액세스 하는 방법에 알아봅니다.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, xbox, 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: db1df2418a2bb60de4a72f51ad01a0bfd547ec20
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "406253"
---
# <a name="xbox-info-api-reference"></a>Xbox Info API 참조 (영문)   
이 API를 사용 하 여 Xbox 하나의 장치 정보에 액세스할 수 있습니다.

## <a name="get-xbox-one-device-information"></a>Xbox 하나의 장치 정보 가져오기

**요청**

프로그램 Xbox 하나에 대 한 장치 정보를 얻을 수 있습니다.

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

* OsVersion-(String) 운영 체제의 버전입니다.
* OsEdition-(String) 버전의 운영 체제의 같은 "2017 년 3 월" 또는 "년 3 월 2017 QFE 1" 입니다.
* ConsoleId-(문자열) 콘솔의 id입니다.
* DeviceId-(String) 콘솔의 Xbox Live 장치 id입니다.
* SerialNumber-(String) 콘솔의 일련 번호가 있습니다.
* DevMode-(String) 콘솔의 현재 개발자 모드인 "None" 또는 "소매" 합니다.
* ConsoleType-예: "Xbox 일" 또는 "Xbox 하나 S" (String) 콘솔의 유형입니다.
* DevkitCertificateExpirationTime-(수)는 UTC 시간 (초) 때 콘솔의 개발자 키트 (영문) 인증서가 만료 됩니다.

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