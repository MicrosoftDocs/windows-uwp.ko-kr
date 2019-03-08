---
title: /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
assetID: be789e70-517d-383e-ea35-b0c39e846081
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype.html
description: " /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 035f30279321b7b386f59e014fa6d185e6b71b57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603718"
---
# <a name="trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
다운로드, 업로드, 또는 파일을 삭제 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 부호 없는 64 비트 정수| Xbox 사용자 ID (XUID) 플레이어의 요청을 수행 하는 합니다.| 
| scid| guid| 조회 서비스 구성 파일의 ID입니다.| 
| pathAndFileName| 문자열| 액세스 항목에 대 한 경로 및 파일 이름입니다. 유효한 문자 (최대 및 마지막 슬래시 포함) 경로 부분 포함 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_), 슬래시 (/)를 전달 합니다. 경로 부분이 비어 있을 수 있습니다. 유효한 문자 (마지막 슬래시 다음의 모든) 파일 이름 부분을 포함 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름을 하지 비워둘 수, 마침표로 끝날 수도 연속 마침표 두 개가 포함 합니다.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 또는 json입니다.| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[DELETE](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;파일을 삭제 합니다. 

[GET](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;파일을 다운로드 합니다.

[PUT](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;파일을 업로드합니다. 데이터 및 메타 데이터를 전송 되는 단일 메시지 또는 일련의 작은 블록에서 데이터 및 메타 데이터는 전송 다중 블록 업로드를 전체 업로드에서 데이터를 업로드할 수 있습니다. 4 (메가바이트 단위) 보다 작은 파일만 단일 메시지로 보낼 수 있습니다. 다중 블록 업로드 형식 json 데이터에 대 한 지원 되지 않습니다. 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 

[제목 저장소 Uri](atoc-reference-storagev2.md)

   