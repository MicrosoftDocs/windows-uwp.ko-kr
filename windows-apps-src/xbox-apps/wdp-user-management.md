---  
author: WilliamsJason
title: "Xbox Live 테스트 사용자 관리 API 참조"
description: "프로그래밍 방식으로 사용자 관리 API에 액세스하는 방법을 알아봅니다."
ms.sourcegitcommit: 67f158b1d3d5ece14c36483a2513a2db2f478660
ms.openlocfilehash: ad01d4daf089c61fc50c7927cfbf123d7d7ee4df

---  

#Xbox Live 사용자 관리#

**요청**

콘솔에서 사용자 목록을 가져오거나 기존 사용자 추가, 제거, 로그인, 로그아웃, 수정 등 목록을 업데이트할 수 있습니다.

| 메서드        | 요청 URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**URI 매개 변수**

* 없음

**요청 헤더**

* 없음

**요청 본문**

PUT 호출에는 다음 구조를 사용하는 JSON 배열이 포함되어야 합니다.

* 사용자
  * AutoSignIn(선택 사항): EmailAddress 또는 UserId로 지정된 계정에 대해 자동 로그인을 사용하거나 사용하지 않도록 설정하는 부울입니다.
  * EmailAddress(선택 사항 - 보증된 사용자를 로그인하지 않는 한 UserId가 제공되지 않은 경우 제공해야 함): 수정/추가/삭제할 사용자를 지정하는 메일 주소입니다.
  * 암호(선택 사항 - 사용자가 현재 콘솔에 없을 경우 제공해야 함): 콘솔에 새 사용자를 추가하는 데 사용되는 암호입니다.
  * SignedIn(선택 사항): 제공된 계정을 로그인 또는 로그아웃해야 하는지를 지정하는 부울입니다.
  * UserId(선택 사항 - 보증된 사용자를 로그인하지 않는 한 EmailAddress가 제공되지 않은 경우 제공해야 함): 수정/추가/삭제할 사용자를 지정하는 UserId입니다.
  * SponsoredUser(선택 사항): 보증된 사용자를 추가할지 여부를 지정하는 부울입니다.
  * Delete(선택 사항): 콘솔에서 이 사용자를 삭제하도록 지정하는 부울입니다.

###응답###

**응답 본문**

GET 호출은 다음 속성이 있는 JSON 배열을 반환합니다.

* 사용자
  * AutoSignIn(선택 사항)
  * EmailAddress(선택 사항)
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser(선택 사항)
  
**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

| HTTP 상태 코드   | 설명     | 
| ------------------ |-----------------|
| 200                | GET 호출 성공 및 응답 본문에 사용자 JSON 배열이 반환됨 |
| 204                | PUT 호출 성공 및 콘솔의 사용자가 업데이트됨 |
| 4XX                | 잘못된 요청 데이터 또는 형식에 대한 다양한 오류 |
| 5XX                | 예기치 않은 오류에 대한 오류 코드 |
<br>





<!--HONumber=Jun16_HO4-->


