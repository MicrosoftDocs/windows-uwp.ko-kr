---
author: WilliamsJason
title: 장치 포털 네트워크 자격 증명 API 참조 (영문)
description: 추가, 제거 또는 네트워크 자격 증명을 프로그래밍 방식으로 업데이트 하는 방법에 알아봅니다.
ms.localizationpriority: medium
ms.openlocfilehash: 7e00169f92ee6f0aa48df64ec4a1186f9682b358
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "410144"
---
# <a name="network-credentials-api-reference"></a>네트워크 자격 증명 API 참조 (영문)
추가, 제거 또는이 REST API를 사용 하 여 devkit에 저장 된 네트워크 자격 증명을 업데이트 수 있습니다.

## <a name="get-existing-credentials"></a>기존 자격 증명 얻기

**요청**

해당 네트워크 공유에 대 한 자격 증명을 가진 사용자의 사용자와 함께 저장 된 공유 폴더의 목록을 가져올 수 있습니다.

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

- 다음 형식의 JSON 배열 합니다.
* 자격 증명
  * NetworkPath-네트워크 공유 경로를 합니다.
  * 사용자 이름-사용자가 자격 증명을 저장 하는 이름입니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="add-or-update-stored-credentials-for-a-user"></a>추가 하거나 사용자에 대 한 저장 된 자격 증명 업데이트

**요청**

메서드      | 요청 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| NetworkPath        | 네트워크 공유 경로를 추가 하는 자격 증명에 액세스 합니다. |
<br>

**요청 헤더**

- 없음

**요청 본문**

- 다음 JSON 요소가 포함 됩니다.
* NetworkPath-네트워크 공유 경로를 합니다.
* 사용자 이름-아래에서 자격 증명을 저장할 사용자 이름입니다.
* 암호-이 사용자에 대 한 신규 또는 업데이트 된 암호입니다.

**응답**   

- 없음  

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
204 | 성공
4XX | 오류 코드
5XX | 오류 코드

## <a name="remove-stored-credentials-for-a-share"></a>프로그램 공유에 대 한 저장 된 자격 증명을 제거 합니다.

**요청**

메서드      | 요청 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| NetworkPath        | 저장 된 자격 증명을 제거 하려는 하는 공유 네트워크 경로입니다. |
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
204 | 자격 증명에 요청을 성공적으로 완료 합니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox


