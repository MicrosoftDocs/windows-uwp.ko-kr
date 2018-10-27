---
title: /json/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: c2745955-5e52-ea2b-7389-cb85202e01c3
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapath.html
author: KevinAsgari
description: " /json/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aed950c0b313d8e646529de42ed39ef41edfa3c8
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5683856"
---
# <a name="jsonusersxuidxuidscidssciddatapath"></a>/json/users/xuid({xuid})/scids/{scid}/data/{path}
지정된 된 경로에 파일 정보를 나열합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트의 부호 없는 정수| Xbox 사용자 ID (XUID) 플레이어의 요청을 하 게 합니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| path| string| 반환할 데이터 항목의 경로입니다. 일치 하는 모든 디렉터리와 하위 가져오기 반환 됩니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/)를 포함합니다. 비어 있을 수 있습니다. 최대 길이는 256 자입니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET](uri-jsonusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;지정된 된 경로에 파일 정보를 나열합니다.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   