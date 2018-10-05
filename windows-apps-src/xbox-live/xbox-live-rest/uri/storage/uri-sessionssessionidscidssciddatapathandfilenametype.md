---
title: /sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}
assetID: f5e91763-0704-8526-77d4-c55dfec3b5a5
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f0ce084fed46cce407c712ee42b782565c1174d4
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4360430"
---
# <a name="sessionssessionidscidssciddatapathandfilenametype"></a>/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}
파일을 다운로드 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| sessionId| string| 조회 세션의 ID입니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 사용할 수 있는 문자를 포함 하 여 최종 슬래시 경로 부분에 대 한 (A Z) 대문자, 소문자 (a-z), 숫자 (0-9) 밑줄 (_)를 포함 하 고 슬래시 (/). 경로 부분 비어 있을 수 있습니다. 유효한 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 후 모든) 파일 이름 부분 포함 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 비워 둘 수, 마침표 없거나 두 개의 연속 된 기간을 포함 합니다.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 또는 json 합니다.| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})](uri-sessionssessionidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;파일을 삭제 합니다. 

[GET (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})](uri-sessionssessionidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;파일을 다운로드 합니다.

[PUT (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})](uri-sessionssessionidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;파일을 업로드 합니다. 데이터는 데이터 및 메타 데이터 전송 되는 단일 메시지 또는 일련의 작은 블록에 데이터 및 메타 데이터를 전송 하는 다중 블록 업로드로 전체 업로드를 업로드할 수 있습니다. 4 개의 메가바이트 보다 작은 파일만 단일 메시지로 보낼 수 있습니다. 다중 블록 업로드의 json 형식 데이터에 대 한 지원 되지 않습니다. 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   