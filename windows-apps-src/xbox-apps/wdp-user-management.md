---
title: Xbox Live 테스트 사용자 관리 API 참조
description: Xbox 장치 포털 REST API를 사용 하 여 콘솔에서 사용자 목록을 가져오거나 업데이트 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 0f05bc84469585fc10bfff6a7f0d0f0976a0080d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174677"
---
# <a name="xbox-live-user-management"></a>Xbox Live 사용자 관리

## <a name="request"></a>요청

콘솔에서 사용자 목록을 가져오거나, 기존 사용자를 추가, 제거, 로그인, 로그 아웃 또는 수정할 수 있습니다.

| 방법        | 요청 URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |


**URI 매개 변수**

* 없음

**요청 헤더**

* 없음

**요청 본문**

PUT에 대 한 호출은 다음 구조의 JSON 배열을 포함 해야 합니다.

* 사용자
  * AutoSignIn (선택 사항): EmailAddress 또는 UserId로 지정 된 계정에 대해 자동 로그인을 사용 하지 않거나 사용 하도록 설정 합니다.
  * EmailAddress (선택 사항-사용자에 게 로그인 하지 않는 경우 UserId가 제공 되지 않으면 제공 되어야 함): 수정/추가/삭제할 사용자를 지정 하는 전자 메일 주소입니다.
  * 암호 (선택 사항)-콘솔에 새 사용자를 추가 하는 데 사용 되는 암호 (선택 사항): 암호를 입력 해야 합니다.
  * (선택 사항): 제공 된 계정에 로그인 또는 로그 아웃 해야 하는지 여부를 지정 하는 부울입니다.
  * UserId (선택 사항-후원 된 사용자에 게 로그인 하지 않는 경우 EmailAddress를 제공 하지 않으면 제공 해야 함): 수정/추가/삭제할 사용자를 지정 하는 UserId입니다.
  * SponsoredUser (선택 사항): 후원 사용자를 추가할지 여부를 지정 하는 부울입니다.
  * Delete (선택 사항): 콘솔에서이 사용자를 삭제 하도록 지정 하는 부울

## <a name="response"></a>응답

**응답 본문**

GET을 호출 하면 다음 속성을 사용 하 여 JSON 배열을 반환 합니다.

* 사용자
  * AutoSignIn (선택 사항)
  * EmailAddress (선택 사항)
  * 게이머 태그
  * 이상
  * UserId
  * XboxUserId
  * SponsoredUser (선택 사항)
  
**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

| HTTP 상태 코드   | Description     | 
| ------------------ |-----------------|
| 200                | 성공적으로 GET을 호출 하 고 응답 본문에서 반환 된 사용자의 JSON 배열을 반환 합니다. |
| 204                | PUT을 호출 하 고 콘솔의 사용자를 업데이트 했습니다. |
| 4XX                | 잘못 된 요청 데이터 또는 형식에 대 한 다양 한 오류 |
| 5XX                | 예기치 않은 오류에 대 한 오류 코드 |
