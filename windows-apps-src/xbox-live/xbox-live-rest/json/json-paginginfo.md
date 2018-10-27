---
title: PagingInfo(JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
author: KevinAsgari
description: " PagingInfo(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e86fbe2ee840a33e3e4cb21cb9584381d389394a
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5690913"
---
# <a name="paginginfo-json"></a>PagingInfo(JSON)
데이터 페이지에서 반환 하는 결과 위해 페이징 정보가 들어 있습니다. 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| continuationToken| 문자열| 결과의 다음 페이지에 액세스 하는 데는 불투명 연속 토큰입니다. 최대 32 자입니다. 호출자는 다음 집합 컬렉션의 항목을 검색 하기 위해 <b>continuationToken</b> 쿼리 매개 변수에서이 값을 제공할 수 있습니다. 이 속성이 <b>null</b>이면 다음 항목이 없는 추가 컬렉션 에서입니다. 이 속성은 필수 이며 컬렉션 <b>skipItems</b>를 사용 하 여 페이징 될 경우에 제공 됩니다.| 
| totalItems| 32 비트 부호 있는 정수| 컬렉션의 항목의 총 수입니다. 서비스 컬렉션의 크기에 대 한 실시간 뷰 제공할 수 없는 경우에 제공 되지 않습니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   