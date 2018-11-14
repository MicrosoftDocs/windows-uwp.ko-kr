---
title: GET (/media/{marketplaceId}/details)
assetID: 7c222fc7-d70a-84ac-5aaf-f22d186f7a43
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetailsget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/details)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6f0cb0fdd3d50a45f4cbeeedacc38bada5874c86
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6182896"
---
# <a name="get-mediamarketplaceiddetails"></a>GET (/media/{marketplaceId}/details)
반환 제품 세부 정보 및 메타 데이터에 대 한 하나 이상의 항목입니다.
이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4ECB)
  * [쿼리 문자열 매개 변수](#ID4ERB)
  * [응답 본문](#ID4EYF)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

**SandboxId** 는 이제는 XToken 클레임이에서 검색 하 고 적용 합니다. **SandboxId** 없는 경우 엔터테인먼트 검색 서비스 (EDS) 400 잘못 된 요청 오류를 throw 합니다.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.|

<a id="ID4ERB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- |
| id| string]| 필수. 모든 세부 정보는 반환 됩니다 (최대 10) Id. 참고 하는 ID 모든 URL에서 잘못 된 문자가 포함 (Id는 일반적으로 ProviderContentId 형식 자체 Url 전체 및 따라서 잘못 된 문자가 포함) 엔터테인먼트 검색 서비스 (EDS)에 전송할 제대로 URL로 인코딩된 이어야 <b>합니다</b> . 또한이 수 있음을 단일 값 ID 형식이 ProviderContentId note 합니다. 둘 이상의 ProviderContentId 원한다 면 여러 번 호출 EDS 연결할 수 있어야 합니다.|
| IdType| string| 선택 사항입니다. 형식에 'id' 매개 변수에 전달 되는 Id입니다. 유효한 값은 다음과 같습니다. <ul><li><b>정식</b> (Bing/시장) </li><li><b>ZuneCatalog</b></li><li><b>ZuneMediaInstance</b> (예: 132kb WMA 음악 파일) </li><li><b>AMG</b></li><li><b>MediaNet</b> (이전 MusiWave) </li><li><b>XboxHexTitle</b> (앱이 콘솔에서 재생) </li></ul>|
| DesiredMediaItemTypes| string| <b>필요한 경우 MediaGroup 전달 되지 않습니다. 둘 다를 전달 되어야 합니다.</b> 미디어 항목 typs id입니다. 모든 제공 Id 같은 종류를 공유 해야 합니다. 여러 종류, 필요한 경우 위의 IdType에 설명 된 대로 가능한 모든 형식에 전달 합니다. 이 값의 기본값은 "알 수 없음" 존재 없는 경우 모든 ID 형식에 대 한 valied 되지 않습니다. |
| MediaGroup| string| <b>필요한 경우 DesiredMediaItemTypes 전달 되지 않습니다. 둘 다를 전달 되어야 합니다.</b>|
| ConditionSets| string| <b>선택 사항</b>입니다. 클라이언트는 키-값 쌍이 쿼리 문자열을 통해 지정 하는 조건 집합에 따라 <b>가용성</b> 정리를 요청할 수 있습니다. 가용성 조건 집합에 일치에 사용 됩니다. 조건 집합에 맞게 사용할 수 있는 키 목록은 다음과 같습니다. <ul><li><b>플랫폼</b>: 제품의 빌드될 위치 및 재생할 수 있습니다.</li><li><b>구독</b>: 목록 (실버 또는 골드)이이 가용성에 대 한 지원 되는 구독입니다.</li><li><b>EntitlementIds</b>: 사용자가 게임을 구입 후 추적 합니다.</li></ul> | 

<a id="ID4EYF"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4E5F"></a>


### <a name="sample-response"></a>예제 응답

아래 JSON 코드 호출에 대 한 응답에서은 `/media/en-us/details?ids=6c5402e4-3cd5-4b29-a9c4-bec7d2c7514a&mediaGroup=GameType`.


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
        "KValue": "4",
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
            }],
            "LicensePolicyTicket": "eyJhbGciOiJIUzI1NiIsImtpZCI6IlYxIn0.eyJhbGxvd1BlcnNpc3RlbnRSb290IjpmYWxzZSwiYmxvY2tlZENvdW50cmllcyI6bnVsbCwiZW50aXRsZWRDb3VudHJpZXMiOm51bGwsImVzY3Jvd2VkQ29udGVudEtleSI6IlJVdENNY3NCQUFBQkFBSUFBQUFGQUFJQUlBQUFBREpsWlRKaVpERTFORGhsT1RZMk5EZGhOVEEyWW1ZeVkyVmpZek5sTVRGa0F3QUNBQUFBQmdBRUFBRUFBQUF4QlFBQ0FBQUFBUUFIQUlBQkFBQzJxb0Vsbm83eWNaZ05iY2R0dFB0TXBlZW95VEVjVUorSmF1WlhmSU9xRFRiV09ud2x1anZEZzU1YzFJby94Q1llTzYxd0VkYTdyZmQ3bVZWMFdTVmNXQUZ2OHplK2toMG1VZVcyUkU3MDd6S05GYzllanVLWlhoY0dkQ1RBYmdVYzJQUVlFNkZOZDJtaWEzdDgyaXlPam9UeGcxWkNaaUR1LzFZaUJ1ZzE1R3BvWVZZREpIbkt5K3NuaEZMdEFKcEI0cER3VXhkVTcvZlV6Y0dvdEtyUW12VDJRQVk5d1NxM0d4WWJ3a1RnVnRrUHVzNTdsN1FVSmp6clBiYkpzeWk5UjhaKzN5RE1EM1JoN0NmekhtVGx3NnBaclFMbnhTTHM0bldPQS92eGo5WS9aN25EM2NJRWlScUhMUU9LSE9GOXVqL0pOOUxtT1FqVkNaaHpLekhRMk5sVURBWmFzYkZ4ZnZjOVh1bDZqZVNCamJsR2xCMlRpZUlTemdPSDh3V0lRWmFGZTMwcVd0a2d4dC9MOEhwTmRwWkwzbG1mcU0vcmswMEtFWUtLaGI4K0E1TGMzeEd2clg3TW9GcmdTUGFONE4xMUNSRVdFZkFGOGc5WWdROEt4R3B0SzdKc0ZpR0NDUmVOeXVYYTQ0NU9NMFNCSFFHVXVMTjl1R0RVTVNNPSIsImV4cGlyYXRpb24iOiIyMDEzLTA4LTEyVDIyOjE3OjI4Ljg2MTYzNzhaIiwibWlpZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC0wMDAwLTAwMDAwMDAwMDAwMCIsIm9paWQiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAiLCJwb2xpY3lDYXJkSWRzIjpbImdhbWVzOjhmY2UwMjkyLTgyZDUtNDFhNi1hN2IzLTM5NzNkOGM5MWZiNyJdLCJwcm9kdWN0SWQiOiIzMmViZDlmZS02YTIyLTQxMTEtOTU0ZS01NjRlYzY5ZDgwMmEiLCJzZSI6WyJHYW1lOzMyZWJkOWZlLTZhMjItNDExMS05NTRlLTU2NGVjNjlkODAyYSJdLCJ2ZXJzaW9uIjoiMiJ9.T0vaH03VDHr913_PRYVBaOXb3XcHX7AwfXNG2rxcVI8",
            "OfferDisplayData": "{
                \"acceptablePaymentInstrumentTypes\": [\"CreditCard\"],
                \"availabilityDescription\": \"AvailabilityDescription for bc545b24-a8e5-422d-bcca-ed1ed5c1dfbc\",
                \"currencyCode\": \"USD\",
                \"displayPrice\": \"$0.00\",
                \"displayListPrice\": \"$0.00\",
                \"distributionType\": \"Full\",
                \"isPurchasable\": true,
                \"listPrice\": 0.0,
                \"price\": 0.0
            }",
            "SignedOffer": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjIzMUZBNzkzQTRENUY3ODc4NUFGQ0FEMEExNTEwRUMwOEJDQURDNTMifQ.eyIkdHlwZSI6Ik9mZmVycy5Db250cmFjdHMuVjMuT2ZmZXIiLCJhY2NlcHRhYmxlUGF5bWVudEluc3RydW1lbnRUeXBlcyI6WyJDcmVkaXRDYXJkIl0sImF2YWlsYWJpbGl0eURlc2NyaXB0aW9uIjoiQXZhaWxhYmlsaXR5RGVzY3JpcHRpb24gZm9yIGJjNTQ1YjI0LWE4ZTUtNDIyZC1iY2NhLWVkMWVkNWMxZGZiYyIsImF2YWlsYWJpbGl0eUlkIjoiN2Q5YWJmOWUtOGQxYy00MWVjLWIxYjAtN2IyMDQ1MTU3MzM2IiwiY3JlYXRlZCI6IjIwMTMtMDQtMTRUMjI6MTc6MjguODYxNjM3OFoiLCJleHBpcmVzIjoiMjAxMy0wOC0xMlQyMjoxNzoyOC44NjE2Mzc4WiIsImlkIjoiZDIwMzljMDctMzBjZi00MzBiLTgxNTQtYjliN2JkNzFiYjY0IiwicHJvZHVjdERlc2NyaXB0aW9uIjp7IiR0eXBlIjoiT2ZmZXJzLkNvbnRyYWN0cy5WMy5Qcm9kdWN0RGVzY3JpcHRpb24iLCJkaXN0cmlidXRpb25UeXBlIjoiRnVsbCIsImdyYW50ZWRFbnRpdGxlbWVudHMiOlt7IiR0eXBlIjoiT2ZmZXJzLkNvbnRyYWN0cy5WMy5FbnRpdGxlbWVudERlc2NyaXB0aW9uIiwiYmlsbGluZ09mZmVySWQiOm51bGwsImNhdGFsb2dPZmZlcklkIjoiMzJlYmQ5ZmUtNmEyMi00MTExLTk1NGUtNTY0ZWM2OWQ4MDJhIiwiY2F0YWxvZ09mZmVySW5zdGFuY2VJZCI6IjMyZWJkOWZlLTZhMjItNDExMS05NTRlLTU2NGVjNjlkODAyYSIsImNvbnRhaW5lcnMiOm51bGwsImRpc3RyaWJ1dGlvblR5cGUiOiJGdWxsIiwiZHVyYXRpb24iOiIxMDY3NTE5OTowMjo0ODowNS40Nzc1ODA3IiwiZW50aXRsZW1lbnRJZCI6IkdhbWU7MzJlYmQ5ZmUtNmEyMi00MTExLTk1NGUtNTY0ZWM2OWQ4MDJhIiwiZW50aXRsZW1lbnRUeXBlIjoiR2FtZSIsInByb2R1Y3RJZCI6IjMyZWJkOWZlLTZhMjItNDExMS05NTRlLTU2NGVjNjlkODAyYSIsInF1YW50aXR5IjpudWxsLCJzYW5kYm94SWQiOiJQQVJULkRldjEiLCJ0aXRsZUlkIjoiMHhGRkZFMDdEMSIsInVvZGJPZmZlcklkIjpudWxsfV0sInByb2R1Y3RJZCI6IjMyZWJkOWZlLTZhMjItNDExMS05NTRlLTU2NGVjNjlkODAyYSIsInByb2R1Y3RUeXBlIjoiREdhbWUiLCJyZWR1Y2VkVGl0bGUiOiJUYW5rcyIsInN1YnNjcmlwdGlvblBvbGljeUlkIjpudWxsfSwicHJpY2UiOnsiJHR5cGUiOiJPZmZlcnMuQ29udHJhY3RzLlYzLlByaWNlIiwiY2FtcGFpZ25TZWdtZW50ZElkIjpudWxsLCJjdXJyZW5jeUNvZGUiOiJVU0QiLCJpc1RheEluY2x1ZGVkIjpmYWxzZSwicHJpY2VDYW1wYWlnbmRJZCI6bnVsbCwicHJpY2VDYXJkSWQiOiI4OTZjZTNhMy0wMDAwLTAwMDAtYWFhYS0wMDE1NWQyNDFjMTgiLCJwcmljZUNvZGUiOm51bGwsInJldGFpbEFtb3VudCI6MC4wLCJza3VzIjpbeyIkdHlwZSI6Ik9mZmVycy5Db250cmFjdHMuVjMuU2t1IiwiYWNjb3VudCI6Ilplcm9Eb2xsYXJTa3UiLCJhbW91bnQiOjAuMCwiYWNjb3VudFR5cGUiOiJSZXZlbnVlIn1dLCJ3aG9sZXNhbGVBbW91bnQiOjAuMH0sInh1aWQiOm51bGx9.LFX_eBwjspSl_n52ysv5avxcDymWFKH2UyJ517D9I6jbkCS9HyEGmqZxmbDPg88qQnRv1Uq0MEvA0v6jjWCKdxYmmjK4PubgmbTQYdgKfas6AjI2P4jQGQILduhgO9YZ6v8DEmihigfanjRwPTi5WhE_2x1mau51mrZBtDERstL9w-mBSkP3Kgu3FT09MNTbqCdDGH1iSpgvNxaXrs336CtEsTLeuy_yvyRvBR-hjxBTo2JH09v5RYQWA8sJ3zmuYxLVe_Cs55DoyCwMwBXbw1tAZE91uyXwQJUFVmZnVxVx9MLQ8EgePJ6BoUpUasp6Jax-mHiBRswsM1V0STGrxQ"
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
    }],
    "ImpressionGuid": "8e6bddc2-ded7-4921-b766-b3a887381caa"
}

```


<a id="ID4ENG"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EPG"></a>


##### <a name="parent"></a>부모

[/media/{marketplaceId}/details](uri-medialocaledetails.md)


<a id="ID4EZG"></a>


##### <a name="further-information"></a>자세한 정보

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
