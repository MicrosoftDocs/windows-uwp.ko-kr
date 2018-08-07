---
author: WilliamsJason
title: 장치 포털 컨트롤러 API 참조
description: 연결된 실제 컨트롤러 수를 얻고 프로그래밍 방식으로 이를 끄는 방법을 알아봅니다.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 292c9c29149b56b9be4255215e97e703278d8abf
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921270"
---
# <a name="controller-api-reference"></a>컨트롤러 API 참조   
연결된 실제 컨트롤러 수를 얻고 REST API를 사용하여 이를 끌 수 있습니다.

## <a name="determine-the-number-of-attached-physical-controllers"></a>연결될 실제 컨트롤러 수 결정

**요청**

다음 요청을 사용하여 장치에 연결된 실제 컨트롤러의 수를 확인할 수 있습니다.

메서드      | 요청 URI
:------     | :-----
GET | /ext/remoteinput/controllers
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**응답**   

- 연결된 실제 컨트롤러의 수를 지정하는 JSON 숫자 속성 ConnectedControllerCount.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>devkit에서 모든 실제 컨트롤러 분리

**요청**

다음 요청을 사용하여 디바이스에서 모든 실제 컨트롤러의 연결을 끊을 수 있습니다.

메서드      | 요청 URI
:------     | :-----
DELETE | /ext/remoteinput/controllers
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
204 | 컨트롤러 연결 끊기에 대한 요청이 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox