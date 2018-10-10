---
title: /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: a60a231c-359a-ee6a-6d18-f9e8c6afd0fc
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapath.html
author: KevinAsgari
description: " /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a33e6c2615eaeea5883cb91b880f4c145d018318
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4505385"
---
# <a name="trustedplatformusersxuidxuidscidssciddatapath"></a>/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}
지정된 된 경로에 파일 정보를 나열합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트의 부호 없는 정수| Xbox 사용자 ID (XUID)의 플레이어를 요청 하 게 합니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| path| string| 반환할 데이터 항목의 경로입니다. 일치 하는 모든 디렉터리와 하위 가져오기 반환 됩니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/)를 포함 합니다. 비어 있을 수 있습니다. 256의 최대 길이입니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET](uri-trustedplatformusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;지정된 된 경로에 파일 정보를 나열합니다.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>부모 

[타이틀 저장소 URI](atoc-reference-storagev2.md)

   