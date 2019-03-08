---
title: 디바이스 포털 네트워크 자격 증명 API 참조
description: 네트워크 자격 증명을 프로그래밍 방식으로 추가, 제거 또는 업데이트하는 방법을 알아봅니다.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: ac30d8db830c51ee40653feb49b443ed44502617
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659218"
---
# <a name="network-credentials-api-reference"></a>네트워크 자격 증명 API 참조
이 REST API를 사용하여 devkit에 저장된 네트워크 자격 증명을 추가, 제거 또는 업데이트할 수 있습니다.

## <a name="get-existing-credentials"></a>기존 자격 증명 가져오기

**요청**

해당 네트워크 공유의 자격 증명을 갖고 있는 사용자의 사용자 이름과 함께 저장된 공유 목록을 가져올 수 있습니다.

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

- 다음과 같은 형식의 JSON 배열입니다.
* 자격 증명
  * NetworkPath - 네트워크 공유의 경로입니다.
  * 사용자 이름 - 자격 증명을 저장한 사용자 이름입니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="add-or-update-stored-credentials-for-a-user"></a>사용자의 저장된 자격 증명을 추가 또는 업데이트

**요청**

메서드      | 요청 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| NetworkPath        | 액세스할 자격 증명을 추가할 공유의 네트워크 경로입니다. |
<br>

**요청 헤더**

- 없음

**요청 본문**

- 다음 JSON 요소입니다.
* NetworkPath - 네트워크 공유의 경로입니다.
* 사용자 이름 - 자격 증명을 저장할 사용자 이름입니다.
* 암호-이 사용자에 대 한 새롭거나 업데이트 된 암호입니다.

**응답**   

- 없음  

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
204 | 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="remove-stored-credentials-for-a-share"></a>공유에 대해 저장된 자격 증명을 제거합니다.

**요청**

메서드      | 요청 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| NetworkPath        | 저장된 자격 증명을 제거할 공유의 네트워크 경로입니다. |
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
204 | 자격 증명에 대한 요청이 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 장치 패밀리**

* Windows Xbox


