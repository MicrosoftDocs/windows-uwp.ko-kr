---
title: 장치 포털 HTTP 모니터 API 참조
description: Xbox의 중심 앱에서 HTTP 트래픽에 액세스하는 방법을 알아보세요.
ms.localizationpriority: medium
ms.openlocfilehash: 81de2a2a3194384e9c5de1c5c45a827e4d965c91
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8751932"
---
# <a name="http-monitor-api-reference"></a>HTTP 모니터 API 참조   
개발자 홈에 있는 상자를 체크하여 HTTP 모니터를 Xbox 콘솔에서 사용할 수 있는 경우 이 API를 사용하여 중심 앱에 대한 실시간 HTTP 트래픽에 액세스할 수 있습니다.

## <a name="get-if-the-http-monitor-is-enabled"></a>HTTP 모니터가 활성화되었는지 여부 가져오기

**요청**

HTTP 모니터가 개발자 홈에서 활성화되어 있는지 여부를 가져올 수 있습니다.

메서드      | 요청 URI
:------     | :-----
GET | /ext/httpmonitor/sessions
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**   
다음 필드가 있는 JSON 개체입니다.

* 활성화됨 - (부울) 개발자 홈에 있는 확인란을 체크하여 HTTP 모니터가 Xbox 콘솔에서 활성화되었는지 여부.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청에 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

## <a name="get-http-traffic-from-the-focused-app"></a>중심 앱에서 HTTP 트래픽 가져오기
**요청**

HTTP 모니터가 개발자 홈에서 활성화되어 있는 경우 실시간으로 Xbox의 중심 앱(시스템 앱이 아닌 경우)에서 HTTP 트래픽을 가져옵니다.

메서드      | 요청 URI
:------     | :-----
Websocket | /ext/httpmonitor/sessions
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**   
다음 필드가 있는 JSON 개체입니다.

* 세션
    * RequestHeaders - (JSON 개체) HTTP 요청의 요청 헤더.
    * RequestContentHeaders - (JSON 개체) HTTP 요청의 요청 콘텐츠 헤더.
    * RequestURL - (문자열) 요청 URL.
    * RequestMethod - (문자열) 요청 메서드.
    * RequestMessage - (문자열) 요청 메시지, 현재 JSON 및 텍스트만 지원.
    * ResponseHeaders - (JSON 개체) HTTP 응답의 응답 헤더.
    * ResponseContentHeaders - (JSON 개체) HTTP 응답의 응답 콘텐츠 헤더.
    * StatusCode - (번호) 응답 상태 코드.
    * ReasponsePhrase - (문자열) 응답 이유 구문.
    * ResponseMessage - (문자열) 응답 메시지, 현재 JSON 및 텍스트만 지원.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청에 성공했습니다.
4XX | 오류 코드
403 | HTTP 모니터가 비활성화됨, 개발자 홈에서 활성화해야 함
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox