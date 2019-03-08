---
title: 마켓플레이스 URI
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
description: " 마켓플레이스 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9fd8112c6e16b3e9d9fb70c34381e88ba5aa6273
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593878"
---
# <a name="marketplace-uris"></a>마켓플레이스 URI

이 섹션에서 Xbox Live 서비스에 대 한 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 *marketplace* 엔터테인먼트 라고도 서비스 검색 서비스 (EDS)입니다.

Xbox 360, Windows Phone 장치 또는 Xbox.com에서를 실행 하는 게임만이 서비스를 사용할 수 있습니다.

이러한 Uri에 대 한 도메인은 eds.xboxlive.com 및 inventory.xboxlive.com입니다.

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/users/me/inventory](uri-inventory.md)

&nbsp;&nbsp;현재 제공 된 사용자와 연결 된 인벤토리 집합에 액세스 합니다.

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;전체 집합을 사용할 수 있는 특정 재고 항목에 대 한 세부 정보에 액세스 합니다.

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;특정 재고 항목에 대 한 세부 정보의 전체 집합에 액세스 합니다.

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;여러 다양 한 미디어 그룹에서 액세스 항목입니다.

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;단일 미디어 그룹 내의 항목에 대 한 검색을 허용 합니다.

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;콘텐츠 등급 토큰에 액세스 합니다.

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;세부 정보 및 메타 데이터 반환 제공에 대 한 하나 이상의 항목.

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;필드 토큰에 액세스합니다.

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;지정된 된 EDS 버전에 대 한 모든 지원 되는 mediaGroups를 나열합니다.

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;미디어 그룹당 사용 가능한 mediaItemTypes EDS의 지정된 된 버전에 대 한 액세스.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;액세스 필드 지정된 mediaitemtype 및 EDS의 지정된 된 버전에 대 한 데이터를 예상할 수 하나입니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;지정 된 미디어 항목 형식에 대 한 쿼리 구체화에 액세스 합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;지정 된 쿼리 구체화 이름 및 지정 된 미디어 항목 유형에 허용 되는 값에 액세스합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;지정 된 쿼리 구체화 값 (예를 들어 "subgenres 지정 장르의")에 대 한 하위 값의 목록에 액세스 합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;지정된 된 EDS 버전에 대 한 모든 지원 되는 mediaItemTypes에 액세스합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;액세스를 사용할 수 있는 정렬 순서 지정된 mediaitem 형식과 EDS의 지정된 된 버전에 대 한 합니다.

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;단일 미디어 그룹 내의 항목에 대 한 검색을 허용합니다.

<a id="ID4EFD"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EHD"></a>


##### <a name="parent"></a>Parent

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>추가 정보

[추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
