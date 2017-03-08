---
author: payzer
title: "디바이스 포털 Xbox 개발자 설정 API 참조"
description: "Xbox 개발자 설정에 액세스하는 방법을 알아봅니다."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: a17a489944fdc2d78831549c1afdc2bd87deabdf
ms.lasthandoff: 02/08/2017

---

# <a name="developer-settings-api-reference"></a>개발자 설정 API 참조   
이 API를 사용하여 개발에 유용한 Xbox One 설정에 액세스할 수 있습니다.

## <a name="get-all-developer-settings-at-once"></a>한 번에 모든 개발자 설정 가져오기

**요청**

다음 요청을 사용하여 단일 요청으로 모든 개발자 설정을 가져올 수 있습니다.

메서드      | 요청 URI
:------     | :-----
GET | /ext/settings
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**   
응답은 모든 설정이 포함된 설정 JSON 배열입니다. 각 설정 개체에는 다음 필드가 포함되어 있습니다.   

이름 - (문자열) 설정의 이름입니다.   
값 - (문자열) 설정의 값입니다.   
RequiresReboot - ("Yes" | "No") 이 필드는 설정을 적용하기 위해 다시 부팅해야 하는지 여부를 나타냅니다.
범주 - (문자열) 설정의 범주입니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청에 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

## <a name="get-settings-one-at-a-time"></a>한 번에 하나씩 설정 가져오기
설정을 개별적으로 검색할 수도 있습니다.

**요청**

다음 요청을 사용하여 개별 설정에 대한 정보를 가져올 수 있습니다.

메서드      | 요청 URI
:------     | :-----
GET | /ext/settings/\&lt;설정 이름\&gt;
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**   
응답은 다음 필드가 있는 JSON 개체입니다.   

이름 - (문자열) 설정의 이름입니다.   
값 - (문자열) 설정의 값입니다.   
RequiresReboot - ("Yes" | "No") 이 필드는 설정을 적용하기 위해 다시 부팅해야 하는지 여부를 나타냅니다.
범주 - (문자열) 설정의 범주입니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청에 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

## <a name="set-the-value-of-a-setting"></a>설정값 설정
설정값을 설정할 수 있습니다.

**요청**

다음 요청을 사용하여 설정값을 설정할 수 있습니다.

메서드      | 요청 URI
:------     | :-----
PUT | /ext/settings/\&lt;설정 이름\&gt;
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   
요청 본문은 다음 필드를 포함하는 JSON 개체입니다.   
값 - (문자열) 설정의 새 값입니다.

**응답**   

- 없음

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


