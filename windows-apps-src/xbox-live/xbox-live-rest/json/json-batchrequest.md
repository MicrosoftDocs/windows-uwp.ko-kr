---
title: BatchRequest(JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
description: " BatchRequest(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51073c71700613edcd7c22e18cc0c00a9222d7e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654158"
---
# <a name="batchrequest-json"></a>BatchRequest(JSON)
사용자, 장치, 제목 등의 현재 상태 정보를 필터링 할 속성의 배열입니다.
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

BatchRequest 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| 사용자| 문자열의 배열| 목록 XUIDs 사용자 최대 한 번에 1100 XUIDs 학습 하려면 현재 상태입니다.|
| deviceTypes| 문자열의 배열| 목록에 알아야 할 사용자를 사용 하는 장치 종류입니다. 배열은 비워 두면 기본적으로 모든 가능한 장치 유형 (즉, none은 필터링 되어 제외).|
| 제목| 32 비트 부호 없는 정수 배열| 장치 목록에 대 한 확인 하려는 사용자 형식입니다. 배열은 비워 두면 기본적으로 모든 가능한 제목 (즉, none은 필터링 되어 제외).|
| level| 문자열| 가능한 값: <ul><li>사용자-get 사용자 노드</li><li>장치-사용자 및 장치 노드</li><li>제목-기본 제목 수준 정보 가져오기</li><li>다양 한 상태 정보, 미디어 정보 또는 둘 다 모든-가져오기</li></ul>기본값은 "title".| 
| onlineOnly| 부울 값| 이 속성이 true 인 경우 일괄 처리 작업 오프 라인 사용자 (감추어진된 항목 포함)에 대 한 레코드를 필터링 합니다. 제공 되지 않으면 온라인 및 오프 라인 사용자가 반환 됩니다.|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


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


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>참고자료   
