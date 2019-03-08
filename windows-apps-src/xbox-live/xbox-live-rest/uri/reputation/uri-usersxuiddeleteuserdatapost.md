---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab43079dbba3729ff39f3a2116c377c3b73142a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604068"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
테스트 사용자에 대 한 평판 데이터를 완전히 다시 설정합니다. 테스트 전용입니다.

  * [설명](#ID4EQ)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [필요한 요청 헤더](#ID4E3B)
  * [HTTP 상태 코드](#ID4EHC)
  * [응답 본문](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>설명

이 API를 호출 하면 모든 피드백 항목 및 평판 데이터로 사용자 로부터 제거 됩니다. 파트너 소매를 제외한 모든 샌드박스에 대해이 API를 호출할 수 있습니다. 적용 팀 id입니다. 모든 샌드박스를 사용 하 여이 API를 호출할 수 있습니다.

이러한 Uri에 대 한 도메인은 `reputation.xboxlive.com`합니다. 이 URI는 항상 포트 10443에서 호출 됩니다.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 사용자 데이터를 삭제 하는 중입니다.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>권한 부여

소매 샌드박스에 **PartnerClaim** 적용 팀에서.

다른 모든 샌드박스에 대 한 **PartnerClaim** 하 고 **SandboxIdClaim**합니다.

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

**콘텐츠-유형: application/json** 하 고 **Xbl 계약 버전 X** (현재 버전이 101).

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.|
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.|
| 500| 내부 서버 오류| 서버에는 요청을 처리 하지 못하게 하는 예기치 않은 상황이 발생 합니다.|
| 503| 서비스 이용 불가| 요청이 제한 되었습니다, 그리고 요청 클라이언트 다시 시도 값 (초) (예: 5 초) 후에 다시 시도 하십시오.|

<a id="ID4EJF"></a>


## <a name="response-body"></a>응답 본문

메서드가 성공 없음 그렇지 않은 경우는 [ServiceError (JSON)](../../json/json-serviceerror.md) 문서.

<a id="ID4EWF"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EYF"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
