---
title: /global/scids/{scid}/data/{path}
assetID: d6353cd3-9127-98d4-bb99-5df690e07022
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapath.html
author: KevinAsgari
description: " /global/scids/{scid}/data/{path}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70ad4c3238dca10150293af60aedb14a4d1ca756
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5994829"
---
# <a name="globalscidssciddatapath"></a>/global/scids/{scid}/data/{path}
지정된 된 경로에 파일 정보를 나열합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| path| string| 반환할 데이터 항목의 경로입니다. 일치 하는 모든 디렉터리와 하위 가져오기 반환 됩니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/)를 포함 합니다. 비어 있을 수 있습니다. 256의 최대 길이입니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET](uri-globalscidssciddatapath-get.md)

&nbsp;&nbsp;지정된 된 경로에 파일 정보를 나열합니다.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   