---
title: VerifyStringResult(JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
description: " VerifyStringResult(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b01793222be80efccdca1f24f5226a2e9ff78064
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599488"
---
# <a name="verifystringresult-json"></a>VerifyStringResult(JSON)
결과에 제출 된 각 문자열에 해당 하는 코드 [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md)합니다.
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

VerifyStringResult 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| resultCode| 32 비트 부호 없는 정수| 필수. 문자열을 제출 HResult 코드에 해당 합니다.|
| offendingString| 문자열| 필수. 거부 문자열을 발생 시킨 문자열 값입니다.|

<a id="ID4EXB"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


```json
{
    "verifyStringResult":
    [
        {"resultCode": "1", "offendingString": "badword"},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
    ]
}

```


#### <a name="common-hresult-values"></a>일반적인 HResult 값

| 값| 오류 이름|
| --- | --- | --- | --- | --- |
| 0| 성공|
| 1| 불쾌감을 주는 문자열|
| 2| 문자열이 너무 깁니다|
| 3| 알 수 없는 오류|

<a id="ID4ELD"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4END"></a>


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>참고자료

[POST (/ 시스템/문자열/유효성 검사)](../uri/stringserver/uri-systemstringsvalidatepost.md)
