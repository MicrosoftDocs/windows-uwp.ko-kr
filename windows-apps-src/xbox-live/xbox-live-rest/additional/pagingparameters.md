---
title: 페이징 매개 변수
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
author: KevinAsgari
description: " 페이징 매개 변수"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e1ed654e4dc1c0f1233ecdedf5d4af66da868bff
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4753595"
---
# <a name="paging-parameters"></a>페이징 매개 변수
 
일부 Xbox Live 서비스 Uri는 JavaScript 개체 Notation (JSON) 개체의 컬렉션을 반환 합니다. URI에 연결 된 쿼리 문자열의 일부로 페이징 매개 변수를 지정 하 여 이러한 컬렉션을 통해 페이징 될 수 있습니다. 페이징 매개 변수의 전체 목록은 다음과 같습니다. 페이징 매개 변수를 사용할 수 있는 모든 Uri는이 페이지의 아래쪽에 연결 됩니다.
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수 
 
| 매개 변수| 필수| 유형| 설명| 
| --- | --- | --- | --- | 
| continuationToken| 아니요| string| 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. | 
| maxItems| 아니요| 32 비트 부호 있는 정수| 최대 <b>skipItems</b> <b>continuationToken</b> 항목의 범위를 반환할 수와 결합할 수 있는 컬렉션에서 반환할 항목 수입니다. 서비스 수 기본값을 제공 <b>maxItems</b> 있는 이며 <b>maxItems</b>보다 적은 반환 될 수 결과의 마지막 페이지 아직 반환 되지 않은 경우에 합니다. | 
| skipItems| 아니요| 32 비트 부호 있는 정수| 항목 수가 지정 된 후 시작 하는 항목을 반환 합니다. 예를 들어 <b>skipItems = "3"</b> 항목을 검색 하는 시작 네 번째 항목으로 검색 합니다. | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>참조 [가져오기 (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   