---
title: /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
assetID: be789e70-517d-383e-ea35-b0c39e846081
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0c776c3aae1978edb501d41fffccafcc76f799e
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483519"
---
# <a name="trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
다운로드, 업로드, 또는 파일을 삭제 합니다. 이러한 Uri의 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 부호 없는 64 비트 정수| Xbox 사용자 ID (XUID) 선수의 요청을 하 게 합니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 해당 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 사용할 수 있는 문자를 포함 하 여 마지막 슬래시 경로 부분에 대 한 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_)를 포함 하 고 슬래시 (/) 합니다. 경로 부분을 비어 있을 수 있습니다. 사용할 수 있는 문자 대문자 (A-z), 소문자 (a-z), 숫자 (0-9)를 포함 하는 (마지막 슬래시 다음의 모든) 파일 이름 부분에 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 없습니다 비어, 마침표 개 연속.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 또는 json입니다.| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>올바른 방법

[DELETE](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;파일을 삭제 합니다. 

[GET](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;파일을 다운로드합니다.

[PUT](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;파일을 업로드 합니다. 에 데이터와 메타 데이터는 보내는 단일 메시지 또는 일련의 작은 블록 데이터와 메타 데이터를 전송 하는 멀티 블록 업로드 한 모든 업로드의 데이터를 업로드할 수 있습니다. 4 메가바이트 미만의 파일만 단일 메시지로 보낼 수 있습니다. 다중 블록 업로드 형식 json의 데이터에 지원 되지 않습니다. 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   