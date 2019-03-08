---
title: /global/scids/{scid}/data/{path}
assetID: d6353cd3-9127-98d4-bb99-5df690e07022
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapath.html
description: " /global/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b8788ae6773c53c2f86b3f51ee9023876416feeb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618868"
---
# <a name="globalscidssciddatapath"></a>/global/scids/{scid}/data/{path}
지정된 된 경로에서 파일 정보를 나열합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| guid| 조회 서비스 구성 파일의 ID입니다.| 
| 경로| 문자열| 반환할 데이터 항목에 대 한 경로입니다. 일치 하는 모든 디렉터리와 하위 디렉터리를 반환 합니다. 유효한 문자는 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/)를 포함 합니다. 비어 있을 수 있습니다. 최대 길이는 256 자입니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET](uri-globalscidssciddatapath-get.md)

&nbsp;&nbsp;지정된 된 경로에서 파일 정보를 나열합니다.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[제목 저장소 Uri](atoc-reference-storagev2.md)

   