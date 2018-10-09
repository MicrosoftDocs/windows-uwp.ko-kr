---
title: EDS 쿼리 구체화
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
author: KevinAsgari
description: " EDS 쿼리 구체화"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b049965d619a7c25108e2b6308b18f1e402fecab
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4462303"
---
# <a name="eds-query-refiners"></a>EDS 쿼리 구체화
 
<a id="ID4EO"></a>

  
 
더 대상이 지정 된 일련의 항목에는 엔터테인먼트 검색 서비스 (EDS) 쿼리를 구체화 하는 다음 매개 변수를 사용할 수 있습니다. 모든 API에 매개 변수가 필요 하지만 쿼리 구체화를 허용 하는 모든 API에서 허용 하는 것입니다.
 
매개 변수 이름은로 전달할 수의 값으로 "queryRefiners" 매개 변수입니다. 이 다음 쿼리 구체화를 사용 하 여 요청 된 반복 되 면 반환 되는 항목 수가 적용 쿼리 구체화의 각 값으로 나눌 반환 합니다.
 
실제로 어떻게 작동 수 다음과 같습니다.
 
   * 찾아보기 API 호출, 매개 변수를 포함 하 여 "queryRefiners 장르 =".
   * API는 8 게임을 반환합니다. 항목을 외에도 항목이 된 각 장르 목록이 반환 됩니다 함께 얼마나 많은 항목이 해당 장르에 속합니다. 이 게임에서는 될 "슈팅: 3, 퍼즐: 5".
   * 두 번째 쿼리가 이루어집니다. 첫 번째 동일 점을 제외 하 고 "장르 슈팅 =" 추가 됩니다.
   * 응답에는 이제 세 개의 게임을, "슈팅" 범주에 속하는 모두 포함 되어 있습니다.
  
| 매개 변수| 데이터 형식| 설명| 
| --- | --- | --- | 
| <b>10 년</b>| string| 모든 항목 해야 릴리스된 10 년입니다.| 
| <b>장르</b>| 문자열의 배열| 목록 항목을 모두 있어야 하는 장르입니다.| 
| <b>labelOwner</b>| string| 트랙, 앨범 또는 아티스트 관련 된 음악 레이블.| 
| <b>네트워크</b>| 문자열의 배열| 네트워크 만든 항목입니다.| 
| <b>studio</b>| 문자열의 배열| 만든 항목 studio 합니다.| 
| <b>xboxAppCategories</b>| 문자열의 배열| 모든 Xbox 앱에 있어야 하는 범주 목록입니다.| 
| <b>xboxAvatarClothes</b>| 문자열의 배열| 복 형식 목록 모든 Xbox 아바타 항목이 있어야 합니다.| 
| <b>xboxAvatarStores</b>| 문자열의 배열| 아바타 항목에는 모든 Xbox 속해야 저장소 목록.| 
| <b>xboxGamePublisherBits</b>| 문자열의 배열| 목록에서 모든 게임 또는 AppType 항목을 설정 해야 하는 게임 판매자 비트입니다.| 
| <b>xboxIsBrowsable</b>| 부울 값| <b>True</b>인는 직접 실행 가능한 콘텐츠 외에도 실행 가능한 하지 않은 전체 게임 반환 됩니다. 기본값은 <b>false</b>입니다.| 
| <b>xboxHasChildMediaItemTypes</b>| 문자열의 배열| 게임의 미디어 그룹을 사용 하 여 반환 된 모든 항목에 제공된 된 값 중 하나는 미디어 항목 형식의 자식 있어야 합니다.| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>자세한 정보 

[마켓플레이스 URI](../uri/marketplace/atoc-reference-marketplace.md)

   