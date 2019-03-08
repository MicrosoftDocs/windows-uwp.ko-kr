---
title: EDS 쿼리 구체화
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
description: " EDS 쿼리 구체화"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c00ff971e05003ec88c47d3803e565f6e9406c47
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611468"
---
# <a name="eds-query-refiners"></a>EDS 쿼리 구체화
 
<a id="ID4EO"></a>

  
 
구체적인된 항목 집합이 엔터테인먼트 검색 서비스 (EDS) 쿼리를 구체화 하는 다음 매개 변수를 사용할 수 있습니다. 모든 API에 필요한 매개 변수가 있지만 쿼리 구체화를 허용 하는 모든 API에서 수락한 합니다.
 
매개 변수 이름은 전달할 수의 값으로 "queryRefiners" 매개 변수입니다. 이 다음 쿼리 구체화를 사용 하 여 요청 된 반복 하는 경우 반환 되는 항목 수가 적용 쿼리 구체화의 각 값 별로 반환 합니다.
 
실제로 작동할 수 하는 방법을 다음과 같습니다.
 
   * 찾아보기 api 호출에 매개 변수를 포함 하 여 "queryRefiners 장르 ="입니다.
   * API 8 게임을 반환합니다. 항목 외에도 항목이 포함 된 각 장르 목록을 반환할 해당 장르에 속한 항목 수와 함께 합니다. 이 게임에서 않을 "슈팅: 3, 퍼즐: 5".
   * 두 번째 쿼리는 만들어집니다. 첫 번째 동일 점을 제외 하 고 "장르 슈팅 =" 추가 됩니다.
   * 응답에는 이제 "슈팅" 범주에 속하는 모든 세 개만 게임을 포함 합니다.
  
| 매개 변수| 데이터 형식| 설명| 
| --- | --- | --- | 
| <b>decade</b>| 문자열| 모든 항목 해야 릴리스된 지난 10 년간 합니다.| 
| <b>genre</b>| 문자열의 배열| 목록 항목을 모두 포함 해야 하는 장르입니다.| 
| <b>labelOwner</b>| 문자열| 연결 된 artist, album 또는 추적 음악 레이블.| 
| <b>network</b>| 문자열의 배열| 이 네트워크에 항목을 만든입니다.| 
| <b>studio</b>| 문자열의 배열| 항목을 만든 studio를 지정 합니다.| 
| <b>xboxAppCategories</b>| 문자열의 배열| 모든 Xbox 앱을 포함 해야 하는 범주 목록입니다.| 
| <b>xboxAvatarClothes</b>| 문자열의 배열| Clothing 형식 목록이 모든 Xbox 아바타 항목이 있어야 합니다.| 
| <b>xboxAvatarStores</b>| 문자열의 배열| 모든 Xbox 아바타 항목이 속해야 하는 저장소의 목록입니다.| 
| <b>xboxGamePublisherBits</b>| 문자열의 배열| 모든 GameType 항목 또는 AppType 항목에 설정 해야 하는 게임 게시자 비트의 목록입니다.| 
| <b>xboxIsBrowsable</b>| 부울 값| 하는 경우 <b>true</b>, 실행 가능한 콘텐츠 외에도 직접 실행 가능 하지 않은 전체 게임을 반환 합니다. 기본값으로 <b>false</b>합니다.| 
| <b>xboxHasChildMediaItemTypes</b>| 문자열의 배열| 게임의 미디어 그룹을 사용 하 여 반환 되는 모든 항목에는 자식 미디어 항목 형식이 제공 된 값 중 하나는 있어야 합니다.| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>추가 정보 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   