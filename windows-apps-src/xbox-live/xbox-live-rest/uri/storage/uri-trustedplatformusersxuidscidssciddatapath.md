---
title: /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: a60a231c-359a-ee6a-6d18-f9e8c6afd0fc
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapath.html
description: " /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ff0d2589eb970ef05f8746bb407857de7ebb3256
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589728"
---
# <a name="trustedplatformusersxuidxuidscidssciddatapath"></a>/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}
지정된 된 경로에서 파일 정보를 나열합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 부호 없는 64 비트 정수| Xbox 사용자 ID (XUID) 플레이어의 요청을 수행 하는 합니다.| 
| scid| guid| 조회 서비스 구성 파일의 ID입니다.| 
| 경로| 문자열| 반환할 데이터 항목에 대 한 경로입니다. 일치 하는 모든 디렉터리와 하위 디렉터리를 반환 합니다. 유효한 문자는 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/)를 포함 합니다. 비어 있을 수 있습니다. 최대 길이는 256 자입니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET](uri-trustedplatformusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;지정된 된 경로에서 파일 정보를 나열합니다.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[제목 저장소 Uri](atoc-reference-storagev2.md)

   