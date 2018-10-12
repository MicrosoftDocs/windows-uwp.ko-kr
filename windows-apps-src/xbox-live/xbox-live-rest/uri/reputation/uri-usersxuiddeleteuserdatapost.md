---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
author: KevinAsgari
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7bcb7b1c6c23f39846084ba4e6583553e2ff04a1
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4540376"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
테스트 사용자에 대 한 안전도 데이터를 완전히 다시 설정합니다. 만 테스트 합니다.

  * [설명](#ID4EQ)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [필요한 요청 헤더](#ID4E3B)
  * [HTTP 상태 코드](#ID4EHC)
  * [응답 본문](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>설명

이 API를 호출 하면 모든 피드백 항목과 평판 데이터 사용자에서 제거 됩니다. 파트너 소매를 제외 하 고 모든 샌드박스에 대해이 API를 호출할 수 있습니다. 적용 팀 사용 하 여 모든 샌드박스 id.이 API를 호출할 수 있습니다.

이러한 Uri에 대 한 도메인은 `reputation.xboxlive.com`. 이 URI는 항상 10443 포트에서 호출 됩니다.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 데이터를 삭제 하 고 사용자의 합니다.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>권한 부여

소매 샌드박스를 적용 팀에서 **PartnerClaim** 대 한

모든 다른 샌드박스에, **PartnerClaim** 및 **SandboxIdClaim**합니다.

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

**콘텐츠 유형: 응용 프로그램/j** **Xbl 계약 버전 X** (현재 버전이 101).

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 되는 표준 HTTP 상태 코드의 전체 목록을 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.|
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.|
| 500| 내부 서버 오류| 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 했습니다.|
| 503| 사용할 수 없는 서비스| 요청을 제한, 클라이언트 재시도 값 초 (예: 5 초) 한 후 다시 시도 합니다.|

<a id="ID4EJF"></a>


## <a name="response-body"></a>응답 본문

성공을 나타내고에 없음 그렇지 않으면 [ServiceError (JSON)](../../json/json-serviceerror.md) 문서.

<a id="ID4EWF"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EYF"></a>


##### <a name="parent"></a>부모

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
