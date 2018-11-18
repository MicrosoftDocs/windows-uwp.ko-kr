---
title: 마켓플레이스 URI
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
author: KevinAsgari
description: " 마켓플레이스 URI"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 96055bd7a2d4169c15e37b55aa70a94fab128a59
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7169946"
---
# <a name="marketplace-uris"></a>마켓플레이스 URI

이 섹션에서는 *마켓플레이스* 서비스, 라고도 엔터테인먼트 검색 서비스 (EDS)에 대 한 Xbox Live 서비스에서 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 합니다.

Xbox 360, Windows Phone 디바이스 또는 Xbox.com 실행 된 게임에만이 서비스를 사용할 수 있습니다.

이러한 Uri에 대 한 도메인은 eds.xboxlive.com 및 inventory.xboxlive.com입니다.

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/users/me/inventory](uri-inventory.md)

&nbsp;&nbsp;현재 제공 되는 사용자와 관련 된 인벤토리 집합에 액세스 합니다.

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;특정 소모 성 인벤토리 항목에 대 한 세부 정보의 전체 집합을 액세스 합니다.

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;특정 인벤토리 항목에 대 한 세부 정보의 전체 집합을 액세스 합니다.

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;여러 다른 미디어 그룹에서 항목을 액세스 합니다.

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;단일 미디어 그룹 내에서 항목을 검색할 수 있습니다.

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;콘텐츠 등급 토큰에 액세스 합니다.

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;반환 제품 세부 정보 및 메타 데이터에 대 한 하나 이상의 항목입니다.

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;필드 토큰에 액세스합니다.

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;지정 된 EDS 버전에 대 한 지원 되는 모든 mediaGroups 나열 되어 있습니다.

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;EDS의 지정 된 버전에 대 한 미디어 그룹당 사용할 수 있는 mediaItemTypes에 액세스합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;지정 된 mediaitemtype 및 EDS의 특정된 버전에 대 한 데이터를 예상할 수 있는 하나 액세스 필드입니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;지정 된 미디어 항목 형식에 대 한 쿼리 구체화에 액세스 합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;지정 된 쿼리 구체화 이름에 대 한 및 미디어 항목 유형 지정 된 허용 되는 값에 액세스합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;지정 된 쿼리 구체화 값 (예: "subgenres 주어진 장르의")에 대 한 하위 값의 목록에 액세스 합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;지정 된 EDS 버전에 대 한 지원 되는 모든 mediaItemTypes에 액세스합니다.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;사용 가능한 액세스 주문 주어진된 mediaitem 형식과 EDS의 특정된 버전에 대 한 정렬합니다.

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;단일 미디어 그룹 내의 항목에 대 한 검색을 허용 합니다.

<a id="ID4EFD"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EHD"></a>


##### <a name="parent"></a>부모

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>자세한 정보

[추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
