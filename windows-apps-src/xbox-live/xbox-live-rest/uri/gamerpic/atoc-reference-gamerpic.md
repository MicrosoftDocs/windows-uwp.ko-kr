---
title: Gamerpic URI
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
author: KevinAsgari
description: " Gamerpic URI"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d612f2fe9cb327b792ed3ab73ad17421f394a030
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4418485"
---
# <a name="gamerpic-uris"></a>Gamerpic URI
 
이 섹션에서는 *gamerpics*에 대 한 Xbox Live 서비스에서 Gamerpic 유니버설 URI (Resource Identifier) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 합니다.
 
이러한 Uri에 대 한 도메인은 `gamerpics.xboxlive.com`.
 
Gamerpic 서비스 제목을 사용자가 자신의 게임 문자의 gamerpic를 생성 하는 기능을 부여 하 여 사용자에 게 더 많은 개인 설정 옵션을 제공 하도록 설계 되었습니다 (이 시나리오에서는 게임 문자 게임에서 protagonist 참조, 사용자 수 자동차, 우주선, 또는 다른 제목에서 컨트롤을 사용자 엔터티).
 
제목 gamerpic 생성의 기본 흐름은 다음과 같습니다.
 
   * 제목 게임에서 문자를 사용 하는 이미지를 만드는 기능을 사용 하 여 사용자를 제공 합니다. 
     * 그렇지 않은 경우에 제목 적절 한 권한을 있지 않습니다 사용자 메시지 다음 수 있습니다.
     * 사용자는 권한이, 사용자는 자신의 문자 gamerpic 만드는 계속 수 있습니다.
  
   * 사용자 이미지를 만들고, 제목 gamerpic 서비스 1080 x 1080.png 파일을 보냅니다.
   * 서비스는 이미지를 저장 하 고 사용자의 새 gamerpic로 이미지를 설정 합니다.
   * 사용자의 게이머 사진에 대 한 호출 모든 환경을 업데이트 된 이미지를 받게 됩니다.
  
제목 gamerpic을 설정할 수는 적용 전용 권한 (211)에 의해 제어 됩니다. 적용 권한이 해지, 사용자는 제목 gamerpic 저장 하지 것입니다 하 고 서비스 403 반환 됩니다. 타이틀 CheckPrivilege 사용자 (개인 211) 콘텐츠를 공유할 수는 확인을 호출 해야 합니다.
 
현재이 서비스를 사용 하기 위해 타이틀 허용 목록 이어야 합니다. 승인을 요청할 메일 `slsgamerpics@microsoft.com`.
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>이 섹션 내용

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;1080 x 1080 gamerpic에 액세스합니다.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   