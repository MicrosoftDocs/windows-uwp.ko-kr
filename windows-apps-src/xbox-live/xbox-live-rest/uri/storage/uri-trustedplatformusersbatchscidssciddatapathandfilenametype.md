---
title: /trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
assetID: c92d247a-5ea1-7a06-36db-7c67a1dc3151
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersbatchscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0278b9a090f648dd2092641efcaa2c7a346d96b1
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5468267"
---
# <a name="trustedplatformusersbatchscidssciddatapathandfilenametype"></a>/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
동일한 파일 이름으로 다 수의 사용자에서 여러 파일을 다운로드합니다. 이러한 Uri의 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 해당 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 사용할 수 있는 문자를 포함 하 여 마지막 슬래시 경로 부분에 대 한 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_)를 포함 하 고 슬래시 (/) 합니다. 경로 부분을 비어 있을 수 있습니다. 사용할 수 있는 문자 대문자 (A-z), 소문자 (a-z), 숫자 (0-9)를 포함 하는 (마지막 슬래시 다음의 모든) 파일 이름 부분에 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 없습니다 비어, 마침표 개 연속.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 또는 json입니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>올바른 방법

[POST](uri-trustedplatformusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;동일한 파일 이름으로 다 수의 사용자에서 여러 파일을 다운로드합니다.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   