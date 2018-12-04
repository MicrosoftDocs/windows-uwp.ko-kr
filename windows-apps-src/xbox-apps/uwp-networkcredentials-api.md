---
title: 장치 포털 네트워크 자격 증명 API 참조
description: 추가, 제거 또는 네트워크 자격 증명을 프로그래밍 방식으로 업데이트 하는 방법을 알아봅니다.
ms.localizationpriority: medium
ms.openlocfilehash: 2da8dae554a0dcbb84d3d3fc3873e2fb035175dc
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469069"
---
# <a name="network-credentials-api-reference"></a>네트워크 자격 증명 API 참조
추가, 제거 또는이 REST API를 사용 하 여 devkit에서 저장 된 네트워크 자격 증명을 업데이트할 수 있습니다.

## <a name="get-existing-credentials"></a>기존 자격 증명을 가져오기

**요청**

사용자에 게 해당 네트워크 공유에 대 한 자격 증명의 사용자와 함께 저장 된 공유 목록을 가져올 수 있습니다.

메서드      | 요청 URI
:------     | :-----
GET | /ext/networkcredential
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**응답**   

- 다음과 같은 형식의 JSON 배열.
* 자격 증명
  * NetworkPath-네트워크 공유에 대 한 경로입니다.
  * 사용자 이름-사용자가 자격 증명을 저장 하는 이름입니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="add-or-update-stored-credentials-for-a-user"></a>추가 하거나 업데이트 저장 된 사용자 자격 증명

**요청**

메서드      | 요청 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| NetworkPath        | 공유 네트워크 경로 추가 하는 자격 증명에 액세스할 수 있습니다. |
<br>

**요청 헤더**

- 없음

**요청 본문**

- 다음과 같은 JSON 요소가 있습니다.
* NetworkPath-네트워크 공유에 대 한 경로입니다.
* 사용자 이름-에서 자격 증명을 저장 하는 사용자 이름입니다.
* 암호-이 사용자에 대 한 새 드라이버나 업데이트 된 암호입니다.

**응답**   

- 없음  

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
204 | 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="remove-stored-credentials-for-a-share"></a>공유에 저장 된 자격 증명을 제거 합니다.

**요청**

메서드      | 요청 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| NetworkPath        | 저장 된 자격 증명을 제거 하 여 공유의 네트워크 경로입니다. |
<br>

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
204 | 자격 증명에 대 한 요청이 성공 했습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox


