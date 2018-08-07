---
author: WilliamsJason
title: 장치 포털 SSH 핀 API 참조
description: 신뢰할 수 있는 모든 SSH 핀을 프로그래밍 방식으로 제거하는 방법을 알아보세요.
ms.localizationpriority: medium
ms.openlocfilehash: 88ba9d3e35650c8c581b9ddb76911636fc18c72e
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921271"
---
# <a name="ssh-pins-api-reference"></a>SSH 핀 API 참조
이 REST API를 사용하여 devkit에서 신뢰할 수 있는 모든 SSH 핀을 제거할 수 있습니다.

## <a name="remove-trusted-ssh-pins"></a>신뢰할 수 있는 SSH 핀 제거

**요청**

메서드      | 요청 URI
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**응답**   

- 없음 

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
204 | 핀 지우기에 대한 요청이 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox
