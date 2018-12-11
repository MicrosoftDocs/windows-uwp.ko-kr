---
title: /serviceconfigs/{scid}/sessions
assetID: d1932817-dcce-5a5c-d7e4-9fd97e1d287c
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessions.html
description: " /serviceconfigs/{scid}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 700575ee9e9afa7bc7a1d34388dc071873d3b9ed
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918269"
---
# <a name="serviceconfigsscidsessions"></a>/serviceconfigs/{scid}/sessions
세션 문서 집합을 검색 하는 가져오기 작업을 지원 합니다. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션의 id.| 
| 키워드| string| 해당 문자열을 사용 하 여 식별 하는 단지 세션에 결과 필터링 하는 데 키워드입니다.| 
| xuid| GUID| 세션을 검색 하 고 사용자에 대 한 Xbox 사용자 Id입니다. 사용자 세션에서 활성 상태 여야 합니다.| 
| 예약| string| 세션 목록에 사용자가 수락 하지 않는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다. 이 설정은 호출자가 세션에 대 한 서버 수준 액세스 이상의 호출자의 XUID Xbox 사용자 ID 필터와 일치 하도록 요청 합니다. | 
| 비활성| string| 세션 목록에 사용자가 수락 하지만 적극적으로 재생 되지 않는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다.| 
| 개인| string| 세션 목록을 개인 세션을 포함 하는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다. 서버를 쿼리 하는 경우 또는 고유한 세션을 쿼리 하는 경우에 유효 합니다. 호출자가 세션에 대 한 서버 수준 액세스 하려면이 매개 변수를 true로 설정 하거나 호출자의 XUID Xbox 사용자 ID 필터와 일치 하도록 요청 합니다. | 
| visibility| 문자열| 결과 필터링에 사용 되는 표시 상태를 나타내는 열거형 값입니다. 현재이 매개 변수 시키면 열기 열려 있는 세션을 포함 하도록 합니다. <b>MultiplayerSessionVisibility</b>를 참조 하세요.| 
| 버전| 문자열| 양의 정수 주요 세션 버전 또는 하위 세션을 나타내는 포함 하도록 합니다. 값은 100 나머지 요청의 계약 버전 보다 작거나 이어야 합니다.| 
| 시험| string| 양의 정수 세션의 최대 수를 나타내는를 검색 합니다.| 
  
<a id="ID4E1D"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/serviceconfigs/{scid}/sessions)](uri-serviceconfigsscidsessionsget.md)

&nbsp;&nbsp;검색 세션 정보를 지정 합니다.
 
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>부모 

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)

   