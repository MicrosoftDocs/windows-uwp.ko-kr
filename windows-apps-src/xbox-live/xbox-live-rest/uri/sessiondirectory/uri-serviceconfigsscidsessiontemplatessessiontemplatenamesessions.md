---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
assetID: 8d55818f-99fd-146a-896b-0f100e78799f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0eff47ed041502838b5101cd5700dbba7a03f383
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4571901"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
지정 된 템플릿 이름 사용 하 여 세션 템플릿 집합을 검색 하는 가져오기 작업을 지원 합니다. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션의 id.| 
| 키워드| string| 해당 문자열을 사용 하 여 식별 하는 단지 세션에 결과 필터링 하는 데 키워드입니다.| 
| xuid| GUID| 세션을 검색 하 고 사용자에 대 한 Xbox 사용자 Id입니다. 사용자 세션에서 활성 상태 여야 합니다. | 
| 예약| string| 세션 목록에 사용자가 수락 하지 않는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다. 이 설정은 호출자가 세션에 대 한 서버 수준 액세스 이상의 호출자의 XUID Xbox 사용자 ID 필터와 일치 하도록 요청 합니다. | 
| 비활성| string| 세션 목록에 사용자가 수락 하지만 적극적으로 재생 되지 않는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다. | 
| 개인| string| 세션의 목록을 개인 세션을 포함 하는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다. 서버 간 쿼리 하는 경우 또는 고유한 세션을 쿼리 하는 경우에 유효 합니다. 호출자가 세션에 대 한 서버 수준 액세스 하려면이 매개 변수를 true로 설정 하거나 호출자의 XUID Xbox 사용자 ID 필터와 일치 하도록 요청 합니다. | 
| visibility| 문자열| 결과 필터링에 사용 되는 표시 상태를 나타내는 열거형 값. 현재이 매개 변수 시키면 열기 열려 있는 세션을 포함 하도록 합니다. <b>MultiplayerSessionVisibility</b>를 참조 하세요. | 
| 버전| 문자열| 양의 정수 주요 세션 버전 또는 하위 세션을 나타내는 포함 하도록 합니다. 값은 100 나머지 요청의 계약 버전 이어야 합니다. | 
| 시험| string| 양의 정수 세션의 최대 수를 나타내는를 검색 합니다.| 
  
<a id="ID4EZD"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.md)

&nbsp;&nbsp;세션 템플릿 문서를 검색합니다.
 
<a id="ID4EDE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFE"></a>

 
##### <a name="parent"></a>부모 

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)

   