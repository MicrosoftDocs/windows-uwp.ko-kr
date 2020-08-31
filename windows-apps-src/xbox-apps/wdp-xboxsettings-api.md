---
title: 장치 포털 Xbox 개발자 설정 API 참조
description: Xbox 장치 포털 REST API를 사용 하 여 개발에 유용한 Xbox One 설정에 액세스 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 0aceb7afdce9cc76eab3ee330f0018fdc7ccd1bb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168987"
---
# <a name="developer-settings-api-reference"></a>개발자 설정 API 참조

이 API를 사용 하 여 개발 하는 데 유용한 Xbox One 설정에 액세스할 수 있습니다.

## <a name="get-all-developer-settings-at-once"></a>한 번에 모든 개발자 설정 가져오기

**요청**

다음 요청을 사용 하 여 단일 요청으로 모든 개발자 설정을 가져올 수 있습니다.

방법      | 요청 URI
:------     | :-----
GET | /ext/settings

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답이**   
응답은 모든 설정을 포함 하는 설정 JSON 배열입니다. 각 설정 개체에는 다음 필드가 포함 됩니다.

* 이름-(String) 설정의 이름입니다.
* 값-(String) 설정의 값입니다.
* RequiresReboot-("Yes" | "No")이 필드는 설정을 적용 하려면 다시 부팅 해야 하는지 여부를 나타냅니다.
* 사용 안 함-("예" | "No")이 필드는 설정을 사용 하지 않도록 설정 하 고 편집할 수 없는지 여부를 나타냅니다.
* 범주-(문자열) 설정의 범주입니다.
* 유형-("Text" | "Number" | "Bool" | "Select")이 필드는 설정의 유형을 나타냅니다 .이 필드는 텍스트 입력, 부울 값 ("true" 또는 "false"), min 및 max를 포함 하는 숫자, 특정 값 목록을 사용 하 여 선택 합니다.

설정이 숫자 인 경우:

* Min-(Number)이 필드는 설정의 최소 숫자 값을 나타냅니다.
* Max-(Number)이 필드는 설정의 최대 숫자 값을 나타냅니다.

다음 설정을 선택 합니다.

* 기타 변수-("Yes" | "No")이 필드는 다시 부팅 하지 않고 유효한 옵션을 변경할 수 있는 경우 설정 옵션이 변수 인지 여부를 나타냅니다.
* Options-유효한 select 옵션을 문자열로 포함 하는 JSON 배열입니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 요청 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="get-settings-one-at-a-time"></a>한 번에 하나씩 설정 가져오기

설정은 개별적으로 검색할 수도 있습니다.

**요청**

다음 요청을 사용 하 여 개별 설정에 대 한 정보를 가져올 수 있습니다.

방법      | 요청 URI
:------     | :-----
GET | />\<setting name\>

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답이**   
응답은 다음 필드를 포함 하는 JSON 개체입니다.

* 이름-(String) 설정의 이름입니다.
* 값-(String) 설정의 값입니다.
* RequiresReboot-("Yes" | "No")이 필드는 설정을 적용 하려면 다시 부팅 해야 하는지 여부를 나타냅니다.
* 사용 안 함-("예" | "No")이 필드는 설정을 사용 하지 않도록 설정 하 고 편집할 수 없는지 여부를 나타냅니다.
* 범주-(문자열) 설정의 범주입니다.
* 유형-("Text" | "Number" | "Bool" | "Select")이 필드는 설정의 유형을 나타냅니다 .이 필드는 텍스트 입력, 부울 값 ("true" 또는 "false"), min 및 max를 포함 하는 숫자, 특정 값 목록을 사용 하 여 선택 합니다.

설정이 숫자 인 경우:

* Min-(Number)이 필드는 설정의 최소 숫자 값을 나타냅니다.
* Max-(Number)이 필드는 설정의 최대 숫자 값을 나타냅니다.

다음 설정을 선택 합니다.

* 기타 변수-("Yes" | "No")이 필드는 다시 부팅 하지 않고 유효한 옵션을 변경할 수 있는 경우 설정 옵션이 변수 인지 여부를 나타냅니다.
* Options-유효한 select 옵션을 문자열로 포함 하는 JSON 배열입니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 요청 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="set-the-value-of-a-setting"></a>설정 값 설정

설정 값을 설정할 수 있습니다.

**요청**

다음 요청을 사용 하 여 설정에 대 한 값을 설정할 수 있습니다.

방법      | 요청 URI
:------     | :-----
PUT | />\<setting name\>

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   
요청 본문은 다음 필드를 포함 하는 JSON 개체입니다.   
값-(String) 설정에 대 한 새 값입니다.

**Response**   

- 없음

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 요청 성공
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 장치 패밀리**

* Windows Xbox