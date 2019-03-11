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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612578"
---
# <a name="serviceconfigsscidsessions"></a>/serviceconfigs/{scid}/sessions
세션 문서 집합을 검색 하는 GET 작업을 지원 합니다. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| GUID| (서비스 안내) 식별자를 구성 하는 서비스입니다. 1 부 세션의 id입니다.| 
| keyword| 문자열| 해당 문자열을 사용 하 여 식별만 세션에 대 한 결과 필터링 하는 데 사용 하는 키워드입니다.| 
| xuid| GUID| Xbox 사용자 세션을 검색할 사용자에 대 한 사용자 Id입니다. 사용자 세션에서 활성 상태 여야 합니다.| 
| 예약| 문자열| 사용자가 수락 하지 않은 세션 목록을 포함 하는 경우를 나타내는 값입니다. 이 매개 변수는 설정만 가능 true로 합니다. 호출자의 XUID Xbox 사용자 ID 필터를 일치 하도록 클레임 또는이 설정은 호출자가 세션에 대 한 서버 수준 권한이 있어야 합니다. | 
| 비활성| 문자열| 세션의 목록에 포함 된 경우 사용자가 수락 하지만 오디오가 재생 하지는 것을 나타내는 값입니다. 이 매개 변수는 설정만 가능 true로 합니다.| 
| 개인| 문자열| 개인 세션 세션의 목록에 포함 하는 경우를 나타내는 값입니다. 이 매개 변수는 설정만 가능 true로 합니다. 서버-투-서버를 쿼리 하는 경우 또는 사용자 자신의 세션을 쿼리 하는 경우에 유효 합니다. 호출자의 XUID Xbox 사용자 ID 필터를 일치 하도록 클레임 또는 호출자가 세션에 대 한 서버 수준 권한이 있어야이 매개 변수를 true로 설정 합니다. | 
| visibility| 문자열| 결과 필터링에 사용 되는 표시 상태를 나타내는 열거형 값입니다. 현재이 매개 변수 에서만 설정할 수 있습니다 열 열려 있는 세션을 포함 합니다. 참조 <b>MultiplayerSessionVisibility</b>합니다.| 
| version| 문자열| 양의 정수 버전 주요 세션 또는 세션 중 더 작은 값을 나타내는를 포함 합니다. 값 보다 작거나 100 모듈로 요청의 계약 버전 이어야 합니다.| 
| Take| 문자열| 양의 정수 세션의 최대 수를 나타내는를 검색 합니다.| 
  
<a id="ID4E1D"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET (/serviceconfigs/{scid}/sessions)](uri-serviceconfigsscidsessionsget.md)

&nbsp;&nbsp;지정 된 세션 정보를 검색 합니다.
 
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent 

[세션 디렉터리 Uri](atoc-reference-sessiondirectory.md)

   