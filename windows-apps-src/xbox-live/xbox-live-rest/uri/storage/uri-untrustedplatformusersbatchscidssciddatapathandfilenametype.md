---
title: /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
assetID: c2909e75-e108-3555-c157-f2d552f10e9f
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 905bc248ee330221695c1dc107745bed45ba813c
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5927332"
---
# <a name="untrustedplatformusersbatchscidssciddatapathandfilenametype"></a>/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
동일한 파일 이름으로 다 수의 사용자에서 여러 파일을 다운로드합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 액세스할 수 있도록 항목에 대 한 경로 파일 이름입니다. 경로 부분 (최대 및 최종 슬래시 포함)에 대해 사용할 수 있는 문자는 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/). 경로 부분 비어 있을 수 있습니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), (최종 슬래시 후 모든 것)는 파일 이름 부분 포함 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 비워 둘 수, 마침표 없거나 두 개의 연속 된 기간을 포함 합니다.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 json입니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[POST](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;동일한 파일 이름으로 다 수의 사용자에서 여러 파일을 다운로드합니다.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   