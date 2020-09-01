---
title: 장치 포털 Fiddler API 참조
description: REST API Xbox Device Portal을 사용 하 여 devkit에서 Fiddler 네트워크 추적을 사용 하거나 사용 하지 않도록 설정 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: f431adae41021432dfcfca6b4e79df5237fcb283
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163997"
---
# <a name="fiddler-settings-api-reference"></a>Fiddler 설정 API 참조   
이 REST API 사용 하 여 devkit에서 Fiddler 네트워크 추적을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Fiddler 추적이 사용 되는지 확인

**요청**

다음 요청을 사용 하 여 장치에서 Fiddler 추적을 사용 하도록 설정할지 여부를 확인할 수 있습니다.

방법      | 요청 URI
:------     | :-----
GET | /ext/fiddler


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**Response**   

- 프록시가 사용 되는지 여부를 지정 하는 JSON bool 속성 IsProxyEnabled입니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | Success
4XX | 오류 코드
5XX | 오류 코드

## <a name="enable-fiddler-tracing"></a>Fiddler 추적 사용

**요청**

다음 요청을 사용 하 여 devkit에 대 한 Fiddler 추적을 사용 하도록 설정할 수 있습니다.  이를 적용 하려면 장치를 다시 시작 해야 합니다.

방법      | 요청 URI
:------     | :-----
POST | /ext/fiddler

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | Description     | 
| ------------------ |-----------------|
| proxyAddress       | Fiddler를 실행 하는 장치의 IP 주소 또는 호스트 이름 |
| proxyPort          | Fiddler에서 트래픽을 모니터링 하는 데 사용 하는 포트입니다. 기본값은 8888입니다. |
| updateCert (선택 사항)| 루트 Fiddler 인증서가 제공 되는지 여부를 나타내는 부울 값입니다. Fiddler이 devkit에서 구성 되지 않았거나 다른 호스트에 대해 구성 된 경우에는 true 여야 합니다.  |


**요청 헤더**

- 없음

**요청 본문**

- UpdateCert가 false 이거나 지정 되지 않은 경우 None입니다. FiddlerRoot 파일을 포함 하는 여러 부분으로 구성 된 http 본문입니다.

**Response**   

- 없음  

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
204 | Fiddler를 사용 하도록 설정 하는 요청이 수락 되었습니다. Fiddler는 다음에 장치를 다시 부팅할 때 사용 하도록 설정 됩니다.
4XX | 오류 코드
5XX | 오류 코드

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Devkit에서 Fiddler 추적 사용 안 함

**요청**

다음 요청을 사용 하 여 장치에서 Fiddler 추적을 사용 하지 않도록 설정할 수 있습니다. 이를 적용 하려면 장치를 다시 시작 해야 합니다.

방법      | 요청 URI
:------     | :-----
Delete | /ext/fiddler

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**Response**   

- 없음 

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
204 | Fiddler 추적을 사용 하지 않도록 설정 하는 요청에 성공 했습니다. 다음 번에 장치를 다시 부팅할 때 추적을 사용할 수 없습니다.
4XX | 오류 코드
5XX | 오류 코드


**사용 가능한 장치 패밀리**

* Windows Xbox

## <a name="see-also"></a>참고 항목
- [Xbox에서 UWP에 대 한 Fiddler 구성](uwp-fiddler.md)

