---
title: 장치 포털 컨트롤러 API 참조
description: 연결 된 실제 컨트롤러의 수를 가져와서 프로그래밍 방식으로 해제 하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 535846d7dbeb2d29328b5c5d01b06d4449a53790
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254188"
---
# <a name="controller-api-reference"></a>컨트롤러 API 참조

이 REST API 사용 하 여 연결 된 실제 컨트롤러의 수를 확보 하 고 해제할 수 있습니다.

## <a name="determine-the-number-of-attached-physical-controllers"></a>연결 된 실제 컨트롤러 수 결정

**요청**

다음 요청을 사용 하 여 장치에서 연결 된 실제 컨트롤러의 수를 확인할 수 있습니다.

방법 | 요청 URI |
-------|-------------|
| GET | /ext/remoteinput/controllers |

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**Response**   

- 연결 된 실제 컨트롤러의 수를 지정 하는 JSON number 속성 ConnectedControllerCount.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

| HTTP 상태 코드 | Description |
|------------------|-------------|
| 200 | Success |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>Devkit의 모든 물리적 컨트롤러 연결 끊기

**요청**

다음 요청을 사용 하 여 장치에서 모든 물리적 컨트롤러의 연결을 끊을 수 있습니다.

| 방법 | 요청 URI |
|--------|-------------|
| Delete | /ext/remoteinput/controllers |

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

| HTTP 상태 코드 | Description |
|------------------|-------------|
| 204 | 컨트롤러의 연결을 끊는 요청이 성공 했습니다. |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

**사용 가능한 장치 패밀리**

* Windows Xbox
