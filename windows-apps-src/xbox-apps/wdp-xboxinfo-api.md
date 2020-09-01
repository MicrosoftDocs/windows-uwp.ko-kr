---
title: 장치 포털 Xbox 정보 API 참조
description: Xbox 장치 포털 REST API의 GET 메서드를 사용 하 여 Xbox One 장치 정보에 액세스 하는 방법에 대해 알아봅니다.
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, xbox, 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: c2a4cecaf3340818b3679dfd3f64b9759ea46752
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174657"
---
# <a name="xbox-info-api-reference"></a>Xbox 정보 API 참조   
이 API를 사용 하 여 Xbox One 장치 정보에 액세스할 수 있습니다.

## <a name="get-xbox-one-device-information"></a>Xbox One 장치 정보 가져오기

## <a name="request"></a>요청

Xbox One에 대 한 장치 정보를 얻을 수 있습니다.

방법      | 요청 URI
:------     | :-----
GET | /ext/xbox/info

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

## <a name="response"></a>응답
다음 필드를 포함 하는 JSON 개체입니다.

* OsVersion-(String) OS 버전입니다.
* OsEdition-(String) OS 버전입니다 (예: "3 월 2017" 또는 "3 월 2017 QFE 1").
* ConsoleId-(String) 콘솔의 ID입니다.
* DeviceId-(문자열) 콘솔의 Xbox Live 장치 Id입니다.
* 일련 번호 (문자열) 콘솔의 일련 번호입니다.
* DevMode-(String) 콘솔의 현재 개발자 모드 (예: "None" 또는 "Retail")입니다.
* ConsoleType-(문자열) 콘솔의 유형 (예: "Xbox one" 또는 "Xbox One S").
* DevkitCertificateExpirationTime-(Number) 콘솔의 개발자 키트 인증서가 만료 되는 UTC 시간 (초)입니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 요청 성공
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 장치 패밀리**

* Windows Xbox
