---
title: /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
assetID: d004d715-d73c-693e-2a0b-ce64f482642b
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fa553cce9ee3179f32573e00c1e215ab46e89b21
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4423974"
---
# <a name="jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
다운로드, 업로드, 또는 파일을 삭제 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트의 부호 없는 정수| Xbox 사용자 ID (XUID)의 플레이어를 요청 하 게 합니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 사용할 수 있는 문자를 포함 하 여 최종 슬래시 경로 부분에 대 한 (A Z) 대문자, 소문자 (a-z), 숫자 (0-9) 밑줄 (_)를 포함 하 고 슬래시 (/). 경로 부분 비어 있을 수 있습니다. 유효한 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 후 모든) 파일 이름 부분 포함 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 비워 둘 수, 마침표 없거나 두 개의 연속 된 기간을 포함 합니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[DELETE](uri-jsonusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;파일을 삭제 합니다. 

[GET](uri-jsonusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;파일을 다운로드 합니다.

[PUT](uri-jsonusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;파일을 업로드 합니다. 다중 블록 업로드의 json 형식 데이터에 대 한 지원 되지 않습니다. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   