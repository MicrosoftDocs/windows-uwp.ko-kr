---
title: BatchRequest(JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
author: KevinAsgari
description: " BatchRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 405d5668368747fab8133cb9f2ce02117f637b98
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5555247"
---
# <a name="batchrequest-json"></a>BatchRequest(JSON)
사용자, 장치 및 제목 등의 현재 상태 정보를 필터링 할 속성의 배열입니다.
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

BatchRequest 개체에는 다음 사양을 있습니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| 사용자| 문자열의 배열| 목록 XUIDs 사용자 1100 XUIDs 한 번에 최대 자세한 현재 상태입니다.|
| deviceTypes| 문자열의 배열| 목록에 대해 알아야 할 사용자가 사용 하는 장치 유형입니다. 모든 가능한 장치 유형에 기본값으로 배열을 비워 (즉, none 필터링 됩니다).|
| 제목| 32 비트 부호 없는 정수의 배열| 장치 목록을 입력 사용자에 대해 알아야 합니다. 모든 가능한 타이틀 기본값으로 배열을 비워 (즉, none 필터링 됩니다).|
| level| 문자열| 가능한 값: <ul><li>사용자-get 사용자 노드</li><li>장치-get 사용자 및 장치 노드</li><li>제목-기본 제목 수준 정보 가져오기</li><li>다양 한 상태 정보, 미디어 정보 또는 둘 다 모두-가져오기</li></ul>기본값은 "제목".| 
| onlineOnly| 부울 값| 이 속성이 true 이면 일괄 작업을 오프 라인 사용자 (숨겨진된 스타일 포함)에 대 한 레코드 필터링 합니다. 제공 되지 않은 경우 온라인 및 오프 라인 사용자가 반환 됩니다.|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>샘플 JSON 구문


```json
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}


```


<a id="ID4EJD"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ELD"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>참조   
