---
title: 장치 포털 HTTP 모니터 API 참조
description: /Wext/svmsrvrvrvrvrvrms/sessions Xbox Device Portal REST API를 사용 하 여 Xbox의 집중 앱에서 실시간 HTTP 트래픽에 액세스 하는 방법을 알아봅니다.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 6ea53f6356aa89a83f3b267a65f65b32aad5749d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304535"
---
# <a name="http-monitor-api-reference"></a>HTTP Monitor API 참조   
개발자 홈의 확인란을 선택 하 여 Xbox 콘솔에서 HTTP 모니터를 사용 하도록 설정한 경우이 API를 사용 하 여 집중 된 앱에 대 한 실시간 HTTP 트래픽에 액세스할 수 있습니다.

## <a name="get-if-the-http-monitor-is-enabled"></a>HTTP 모니터가 사용 되도록 설정 된 경우 가져오기

**요청**

개발자 홈에서 HTTP 모니터를 사용 하도록 설정 했는지 여부를 가져올 수 있습니다.

방법      | 요청 URI
:------     | :-----
GET | /ext/httpmonitor/sessions

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답이**   
다음 필드를 포함 하는 JSON 개체입니다.

* 사용-(Bool) 개발자 홈의 확인란을 선택 하 여 Xbox 콘솔에서 HTTP 모니터를 사용 하도록 설정할지 여부를 선택 합니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 요청 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="get-http-traffic-from-the-focused-app"></a>포커스가 있는 앱에서 HTTP 트래픽 가져오기

**요청**

개발자 홈에서 HTTP 모니터가 사용 되도록 설정 된 경우 실시간으로 시스템 앱이 아닌 Xbox의 집중 된 앱에서 HTTP 트래픽을 가져옵니다.

방법      | 요청 URI
:------     | :-----
서버당 | /ext/httpmonitor/sessions

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답이**   
다음 필드를 포함 하는 JSON 개체입니다.

* 세션
    * RequestHeaders-(JSON 개체) HTTP 요청의 요청 헤더입니다.
    * RequestContentHeaders-(JSON 개체) HTTP 요청에서 콘텐츠 헤더를 요청 합니다.
    * RequestURL-(String) 요청 URL입니다.
    * RequestMethod-(String) request 메서드.
    * RequestMessage-(String) 현재 JSON 및 텍스트 콘텐츠만 지 원하는 요청 메시지입니다.
    * ResponseHeaders-(JSON 개체) HTTP 응답의 응답 헤더입니다.
    * ResponseContentHeaders-(JSON 개체) HTTP 응답의 응답 콘텐츠 헤더입니다.
    * StatusCode-(Number) 응답 상태 코드입니다.
    * ReasponsePhrase-(String) 응답 이유 문구입니다.
    * ResponseMessage-(String) 응답 메시지 이며 현재 JSON 및 텍스트 콘텐츠만 지원 합니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 요청 성공
4XX | 오류 코드
403 | HTTP 모니터 사용 안 함, 개발 홈에서 사용 하도록 설정 해야 함
5XX | 오류 코드


**사용 가능한 장치 패밀리**

* Windows Xbox