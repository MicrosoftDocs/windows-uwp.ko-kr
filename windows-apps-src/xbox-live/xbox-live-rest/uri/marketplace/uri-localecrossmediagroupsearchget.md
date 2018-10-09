---
title: GET (/media/{marketplaceId}/crossMediaGroupSearch)
assetID: 7c509af1-8dce-f419-c4de-2fad54fd1edb
permalink: en-us/docs/xboxlive/rest/uri-localecrossmediagroupsearchget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/crossMediaGroupSearch)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 430dbce8b3ac2a79709c5f0761124aa7a78cab5e
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4424759"
---
# <a name="get-mediamarketplaceidcrossmediagroupsearch"></a>GET (/media/{marketplaceId}/crossMediaGroupSearch)
여러 다른 미디어 그룹에서 항목을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EPB)
  * [응답 본문](#ID4ETC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
그룹 간 API에는 클라이언트가 여러 다른 미디어 그룹에서 항목을 검색할 수 있습니다. 이 API에 결과 통해 페이징을 정방향 연속 토큰을 사용에 필요합니다. 이 API는 쿼리 구체화를 수락합니다.
 
이제 **SandboxId** 클레임에는 XToken에서 검색 이며 적용 합니다. **SandboxId** 없으면 엔터테인먼트 검색 서비스 (EDS) 400 잘못 된 요청 오류를 throw 합니다.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
  
<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| continuationToken| string| 선택 사항입니다. ContinuationToken 매개 변수를 참조 하세요.| 
| q| string| 필수. 검색에 사용 된 용어를 쿼리 합니다.| 
  
<a id="ID4ETC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EZC"></a>

 
### <a name="sample-response"></a>예제 응답
 
아래 JSON 코드 호출에 대 한 응답에서은 `/media/en-us/crossMediaGroupSearch?q=vector&maxItems=25&fields=all`.
 

```cpp
{
    "Items": [{
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "fd16e2fb-eca4-4182-8f69-a98fdd6e57a1",
        "Name": "Vector based massively multiplayer Tanks game.",
        "ReducedName": "Tanks",
        "ReleaseDate": "2013-03-15T00: 00: 00Z",
        "TitleId": "69A60C76",
        "VuiDisplayName": "Tanks",
        "DeveloperName": "Microsoft Studios",
        "Images": [{
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 300,
            "Width": 219,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 120,
            "Width": 85,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Image"],
            "Purpose": "Image",
            "Height": 720,
            "Width": 1280,
            "Order": 1
        }],
        "PublisherName": "Microsoft Studios",
        "RatingId": "10",
        "ParentalRating": "E",
        "ParentalRatingSystem": "ESRB",
        "SortName": "\n            Vector based massively multiplayer Tanks game.\n          ",
        "Capabilities": [{
            "NonLocalizedName": "onlineMultiplayerMin",
            "Value": "3"
        },
        {
            "NonLocalizedName": "onelineMultiplayerMax",
            "Value": "5"
        }],
        "KValue": "30",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "69A60C76"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            }],
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "PackageType": "XVC",
            "LicenseType": "Xbox Live Games Machine and User"
        }],
        "SandboxId": "PART.Dev1"
    },
    {
        "MediaGroup": "MusicArtistType",
        "MediaItemType": "MusicArtist",
        "ID": "61cdf888-7b2b-5111-82f4-87e9c7a86504",
        "Name": "Vector",
        "Genres": [{
            "Name": "Hip Hop"
        }],
        "SortName": "Vector [2]",
        "KValue": "28",
        "KValueNamespace": "bingbox",
        "AmgId": "P   362877",
        "ZuneId": "89C90600-0200-11DB-89CA-0019B92A3933",
        "SubGenres": [{
            "Name": "Contemporary Hip Hop"
        }],
        "AllTimePlayCount": 995.0,
        "SevenDaysPlayCount": 2.0,
        "ThirtyDaysPlayCount": 57.0,
        "LegacyAmgId": "7D890500-0600-11DB-89CA-0019B92A3933",
        "LegacyIds": [{
            "IdType": "AmgId",
            "Value": "P   362877"
        },
        {
            "IdType": "ZuneId",
            "Value": "89C90600-0200-11DB-89CA-0019B92A3933"
        },
        {
            "IdType": "LegacyAmgId",
            "Value": "7D890500-0600-11DB-89CA-0019B92A3933"
        },
        {
            "IdType": "ArtistId",
            "Value": "444809"
        }]
    }],
    "ContinuationToken": "1-----1",
    "ImpressionGuid": "9e41e6e4-1b2e-4710-abe4-b01ffbaa7605"
}
         
```

   
<a id="ID4EID"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>부모 

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

  
<a id="ID4EUD"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   