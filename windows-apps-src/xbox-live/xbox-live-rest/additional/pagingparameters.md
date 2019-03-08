---
title: 페이징 매개 변수
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
description: " 페이징 매개 변수"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe80e1666f9eab8caf7e0bbdb2b537bd7661c9a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627298"
---
# <a name="paging-parameters"></a>페이징 매개 변수
 
일부 Xbox Live 서비스 Uri 개체 JSON (JavaScript Notation) 개체의 컬렉션을 반환 합니다. 이러한 컬렉션 URI에 연결 된 쿼리 문자열의 일부로 페이징 매개 변수를 지정 하 여 통해 페이징할 수 있습니다. 페이징 매개 변수의 전체 목록은 다음과 같습니다. 페이징 매개 변수를 허용 하는 모든 Uri는이 페이지의 아래쪽에 연결 됩니다.
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수 
 
| 매개 변수| 필수| 형식| 설명| 
| --- | --- | --- | --- | 
| continuationToken| 아니오| 문자열| 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. | 
| maxItems| 아니오| 32 비트 부호 있는 정수| 과 결합 될 수 있는 컬렉션에서 반환할 항목의 최대 수 <b>skipItems</b> 하 고 <b>continuationToken</b> 항목의 범위를 반환할 수 있습니다. 서비스 하는 경우 기본 값을 제공할 수 있습니다 <b>maxItems</b> 없 및 보다 더 적은 수를 반환할 수 있습니다 <b>maxItems</b>결과의 마지막 페이지 아직 반환 되지 않은 경우에 합니다. | 
| skipItems| 아니오| 32 비트 부호 있는 정수| 항목 수가 지정 된 후 시작 하는 항목을 반환 합니다. 예를 들어 <b>skipItems = "3"</b> 항목을 검색 하는 네 번째 항목 부터는 검색 합니다. | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>참조 [GET (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   