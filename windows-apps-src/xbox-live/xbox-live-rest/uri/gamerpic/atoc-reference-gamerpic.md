---
title: Gamerpic URI
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
description: " Gamerpic URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a8ba79784ed73ae62e7fe8d65c626c3ebc6003a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618388"
---
# <a name="gamerpic-uris"></a>Gamerpic URI
 
이 섹션에서 Xbox Live 서비스에 대 한 Gamerpic 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 *gamerpics*합니다.
 
이러한 Uri에 대 한 도메인은 `gamerpics.xboxlive.com`합니다.
 
Gamerpic 서비스는 사용자가 게임 문자의 gamerpic 생성 하도록 허용 하는 기능 제목에 권한을 부여 하 여 사용자에 게 더 많은 개인 설정 옵션을 제공 하도록 설계 되었습니다 (이 시나리오에서는 게임 문자 참조는 게임 내 protagonist; 사용자 수 자동차는 우주선, 제목에 사용자를 제어 하는 다른 엔터티).
 
제목 gamerpic 생성의 기본 흐름 아래와 같습니다.
 
   * 제목 게임 내 문자의 해당 이미지를 만드는 기능을 사용 하 여 사용자를 제공 합니다. 
     * 그렇지 않은 경우 제목에는 적절 한 권한이 없는 사용자 메시지 다음 수 있습니다.
     * 사용자에 게 권한이 없는, 해당 문자 gamerpic 만들려면은 사용자가 계속 됩니다.
  
   * 사용자 이미지를 만들고, 제목 gamerpic 서비스에 1080 x 1080.png 파일을 보냅니다.
   * 서비스는 이미지를 저장 하 고 사용자의 새 gamerpic로 이미지를 설정 합니다.
   * 사용자의 gamerpic에 대 한 호출 하는 모든 환경을 업데이트 된 이미지를 받게 됩니다.
  
제목 gamerpic를 설정 하는 기능 (211)를 적용 전용 권한으로 제어 됩니다. 적용 권한을 취소, 사용자는 제목 gamerpic 저장 하지 것입니다 하 고 서비스는 403을 반환 합니다. 제목에는 사용자 (priv 211) 콘텐츠를 공유 하도록 허용 된 있는지 확인 하려면 CheckPrivilege 호출 해야 합니다.
 
현재이 서비스를 사용 하기 위해을 제목 허용 목록에 있어야 합니다. 승인 요청을 하려면 전자 메일 `slsgamerpics@microsoft.com`합니다.
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;1080 x 1080 gamerpic에 액세스합니다.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)

   