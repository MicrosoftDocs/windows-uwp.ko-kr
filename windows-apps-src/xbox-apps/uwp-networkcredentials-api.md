---
title: 장치 포털 네트워크 자격 증명 API 참조
description: 네트워크 자격 증명을 프로그래밍 방식으로 추가, 제거 또는 업데이트 하는 방법을 알아봅니다.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 7ba9e25031590da02881276ff9a10a9f952c78ec
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254218"
---
# <a name="network-credentials-api-reference"></a>네트워크 자격 증명 API 참조

이 REST API 사용 하 여 devkit에서 저장 된 네트워크 자격 증명을 추가, 제거 또는 업데이트할 수 있습니다.

## <a name="get-existing-credentials"></a>기존 자격 증명 가져오기

**요청**

해당 네트워크 공유에 대 한 자격 증명이 있는 사용자의 사용자 이름과 함께 저장 된 공유의 목록을 가져올 수 있습니다.

| 방법 | 요청 URI |
|--------|-------------|
| GET | /ext/networkcredential |

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**Response**   

다음 형식의 JSON 배열:

* 자격 증명
  * NetworkPath-네트워크 공유에 대 한 경로입니다.
  * 사용자 이름-저장 된 자격 증명이 있는 사용자 이름입니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

| HTTP 상태 코드 | Description |
|------------------|-------------|
| 200 | Success |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

## <a name="add-or-update-stored-credentials-for-a-user"></a>사용자의 저장 된 자격 증명 추가 또는 업데이트

**요청**

| 방법 | 요청 URI |
|--------|-------------|
| POST | /ext/networkcredential |

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | Description     |
| ------------------ |-----------------|
| NetworkPath        | 액세스할 자격 증명을 추가할 공유의 네트워크 경로입니다. |

**요청 헤더**

- 없음

**요청 본문**

다음 JSON 요소:
* NetworkPath-네트워크 공유에 대 한 경로입니다.
* 사용자 이름-자격 증명을 저장할 사용자 이름입니다.
* 암호-이 사용자에 대 한 새 암호 또는 업데이트 된 암호입니다.

**Response**   

- 없음  

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

| HTTP 상태 코드 | Description |
|------------------|-------------|
| 204 | Success |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

## <a name="remove-stored-credentials-for-a-share"></a>공유에 대 한 저장 된 자격 증명을 제거 합니다.

**요청**

| 방법 | 요청 URI |
|--------|-------------|
| Delete | /ext/networkcredential |

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | Description     |
| ------------------ |-----------------|
| NetworkPath        | 저장 된 자격 증명을 제거 하는 공유의 네트워크 경로입니다. |

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
| 204 | 자격 증명에 대 한 요청이 성공 했습니다. |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

**사용 가능한 장치 패밀리**

* Windows Xbox