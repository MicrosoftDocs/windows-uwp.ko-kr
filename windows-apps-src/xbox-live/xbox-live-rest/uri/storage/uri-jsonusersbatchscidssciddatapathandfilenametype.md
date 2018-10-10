---
title: /json/users/batch/scids/{scid}/data/{pathAndFileName},json
assetID: 06ae159f-7739-1330-df15-871d260e6ba1
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /json/users/batch/scids/{scid}/data/{pathAndFileName},json"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8b620144bbeee783835f5bf9181381a4c9b38a66
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4497797"
---
# <a name="jsonusersbatchscidssciddatapathandfilenamejson"></a>/json/users/batch/scids/{scid}/data/{pathAndFileName},json
여러 사용자가 동일한 파일 이름으로에서 여러 파일을 다운로드합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 경로 부분까지 포함 하 여 최종 슬래시를 사용할 수 있는 문자는 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/). 경로 부분 비어 있을 수 있습니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 다음의 모든) 파일 이름 부분을 포함 한 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 없습니다 비워 둘 수, 마침표 개 연속 합니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[POST](uri-jsonusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;여러 사용자가 동일한 파일 이름으로에서 여러 파일을 다운로드합니다. 파일 다운로드 요청 URI에 의해 결정 됩니다. 요청 본문 파일을 다운로드 하려면 사용자의 XUIDs 목록이 포함 되어 있습니다. 응답 본문에는 고유한 헤더를 사용 하 여 특정 사용자에 대 한 파일을 나타내는 각 부분을 사용 하 여 다중 파트 MIME 메시지 됩니다. 성공 및 실패에 대 한 응답의 부분에 대 한 것이 가능 합니다.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   