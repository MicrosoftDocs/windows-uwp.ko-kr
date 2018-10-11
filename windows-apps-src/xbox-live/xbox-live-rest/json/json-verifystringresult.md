---
title: VerifyStringResult(JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
author: KevinAsgari
description: " VerifyStringResult(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f774f603d89e29f5233fb0866303bab4577ca3d2
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4508178"
---
# <a name="verifystringresult-json"></a>VerifyStringResult(JSON)
[/System/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md)에 제출 된 각 문자열에 해당 되는 결과 코드.
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

VerifyStringResult 개체에는 다음 사양을 있습니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| resultCode| 32 비트 부호 없는 정수| 필수. HResult 코드에 해당 하 문자열을 전송 합니다.|
| offendingString| string| 필수. 문자열을 거부 하는 문자열 값입니다.|

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
| 1| 불쾌 문자열|
| 2| 너무 긴 문자열|
| 3| 알 수 없는 오류|

<a id="ID4ELD"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4END"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>참조

[POST (/system/strings/validate)](../uri/stringserver/uri-systemstringsvalidatepost.md)
