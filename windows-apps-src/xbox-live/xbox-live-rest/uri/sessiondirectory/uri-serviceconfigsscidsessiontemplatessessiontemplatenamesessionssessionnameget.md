---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: 6a4c4a13-c968-3271-cbc3-b742a8de98b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.html
author: KevinAsgari
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f1b1c9d15cc1bc06c14a44d395b478cdc536fd74
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4419211"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
세션 개체를 가져옵니다.

> [!IMPORTANT]
> 이 URI 메서드에 필요 X Xbl-계약 버전의 헤더 요소: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EMB)
  * [HTTP 상태 코드](#ID4EZB)
  * [요청 본문](#ID4E6B)
  * [응답 본문](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드는 지정 된 이름을 세션 문서 읽고 세션을 검색 합니다. 성공 시 서버에서 가져온 모든 속성을 사용 하 여 세션 개체를 반환 합니다. 이 메서드는 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionAsync**하 여 줄 바꿈할 수 있습니다. GET 메서드 매개 변수 **GetCurrentSessionAsync** *sessionReference* 매개 변수에 전달 된 세션에 대 한 **MultiplayerSessionReference** 개체에 지정 된에 직접 비슷합니다.

통신 형식은 GET 메서드는 다음과 같습니다.

```cpp
GET /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
      Content-Type: application/json

```



<a id="ID4EMB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션 식별자입니다.|
| sessionTemplateName| string| 현재 인스턴스의 세션 템플릿 이름입니다. 2 부 세션 식별자입니다.|
| 세션 이름| GUID| 세션의 고유 ID입니다. 3 부 세션 식별자입니다.|

<a id="ID4EZB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4E6B"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4EKC"></a>


## <a name="response-body"></a>응답 본문
[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)에서 응답 구조를 참조 하세요.  
<a id="ID4ETC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EVC"></a>


##### <a name="parent"></a>부모

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)