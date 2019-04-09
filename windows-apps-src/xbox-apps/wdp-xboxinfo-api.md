---
title: 디바이스 포털 Xbox 정보 API 참조
description: Xbox 디바이스 정보에 액세스하는 방법을 알아봅니다.
ms.date: 11/072017
ms.topic: article
keywords: 장치 포털, xbox, uwp, windows 10
ms.localizationpriority: medium
ms.openlocfilehash: 7aa8b11bc439266d36fbb27a7eaa7b07e924a17c
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244039"
---
# <a name="xbox-info-api-reference"></a>Xbox 정보 API 참조   
이 API를 사용하여 Xbox One 디바이스 정보에 액세스할 수 있습니다.

## <a name="get-xbox-one-device-information"></a>Xbox One 디바이스 정보 가져오기

## <a name="request"></a>요청

Xbox One에 대한 디바이스 정보를 가져올 수 있습니다.

메서드      | 요청 URI
:------     | :-----
가져오기 | /ext/xbox/info

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

## <a name="response"></a>응답
다음 필드가 있는 JSON 개체입니다.

* OsVersion - (문자열) OS 버전입니다.
* OsEdition - (문자열) OS 에디션입니다(예: "March 2017" 또는 "March 2017 QFE 1").
* ConsoleId - (문자열) 콘솔의 ID입니다.
* DeviceId - (문자열) 콘솔의 Xbox Live 디바이스 Id입니다.
* SerialNumber - (문자열) 콘솔의 일련 번호입니다.
* DevMode - (문자열) 콘솔의 현재 개발자 모드입니다(예: "없음" 또는 "정품").
* ConsoleType - (문자열) 콘솔의 유형입니다(예: "Xbox One" 또는 "Xbox One S").
* DevkitCertificateExpirationTime - (숫자) 콘솔의 개발자 키트 인증서가 만료되는 UTC 시간입니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청에 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 디바이스 패밀리**

* Windows Xbox