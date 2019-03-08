---
title: PagingInfo(JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
description: " PagingInfo(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0e773d73499e79fe23f736a536027932ca1a07b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651218"
---
# <a name="paginginfo-json"></a>PagingInfo(JSON)
데이터 페이지에 반환 되는 결과 대 한 페이징 정보를 포함 합니다. 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>pagingInfo
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| continuationToken| 문자열| 결과의 다음 페이지에 액세스 하는 데 사용 되는 불투명 연속 토큰입니다. 최대 32 자입니다. 호출자에서이 값을 제공할 수는 <b>continuationToken</b> 다음 컬렉션의 항목 집합을 검색 하려면 쿼리 매개 변수입니다. 이 속성이 <b>null</b>, 다음 컬렉션에 추가 항목이 없습니다. 이 속성은 필수 이며 컬렉션으로 페이지가 매겨지며 되는 경우에 제공 됩니다 <b>skipItems</b>합니다.| 
| totalItems| 32 비트 부호 있는 정수| 컬렉션에서 항목의 총 수입니다. 서비스 컬렉션의 크기에 대 한 실시간 보기를 제공할 수 없는 경우에 제공 되지 않습니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   