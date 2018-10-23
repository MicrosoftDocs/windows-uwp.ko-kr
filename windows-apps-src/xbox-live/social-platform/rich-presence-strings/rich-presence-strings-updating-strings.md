---
title: 문자열을 업데이트 하는 다양 한 상태
author: KevinAsgari
description: Xbox Live 다양 한 상태 문자열을 업데이트 하는 방법을 알아봅니다.
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 풍부한 존재 여부, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 9e43cacf9a3fcaedd3829a690441dd28be935339
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5405082"
---
# <a name="rich-presence-updating-strings"></a>문자열을 업데이트 하는 다양 한 상태

타이틀에서 다양 한 상태 문자열을 업데이트 하려면 JSON 개체에 적절 한 매개 변수를 사용 하 여 작성 제목 URI를 호출할 수 있습니다. Restful이 호출은 Xbox 서비스 Api도 래핑됩니다. **Microsoft.Xbox.Services.Presence Namespace** 관련된 API에 대 한 정보를 참조 하세요.

URI는 다음과 같습니다.

          POST /users/xuid({xuid})/devices/current/titles/current

다음은 다양 한 상태 문자열을 설정 하기 위한 필드만입니다. 여기에 나열 되지 않은 제목에 대 한 쓰기 현재 상태와 관련 된 선택적 필드가 다른입니다.

## <a name="titlerequest-object"></a>TitleRequest 개체

속성 | 형식 | 필수 | 설명
---|---|---|---
작업|ActivityRequest|N|제목에서 정보 (다양 한 상태 및 미디어 정보를 사용 가능한 경우)를 설명 하는 레코드

## <a name="activityrequest-object"></a>ActivityRequest 개체

속성 | 형식 | 필수 | 설명
---|---|---|---
richPresence|RichPresenceRequest|N|사용 해야 하는 다양 한 상태 문자열의 friendlyName 합니다.

## <a name="richpresencerequest-object"></a>RichPresenceRequest 개체

속성 | 형식 | 필수 | 설명
---|---|---|---
Id|문자열|예|FriendlyName 사용 해야 하는 다양 한 상태 문자열
서비스 안내|문자열|예|다양 한 상태 문자열 정의 된 알려주는 서비스 안내 합니다.

예를 들어 있는 xuid 12345 사용자에 대 한 다양 한 상태를 업데이트 하고자 내 통화 다음과 같은 모습:

          POST /users/xuid(12345)/devices/current/titles/current


다음과 같은 JSON 본문:

```json
          {
            activity:
            {
              richPresence:
              {
                id:"playingMap",
                scid:"0000-0000-0000-0000-01010101"
              }
            }
          }
```

래퍼 API를 사용 하 여 이것이 **PresenceService.SetPresenceAsync 메서드** 를 호출

관리 경우 데이터 플랫폼 최신 때마다 다양 한 상태 문자열을 다시 설정 하지 않아도 됩니다. 그런 다음 빈 변경에 맞게 데이터. 위 예제에서 현재 지도 사용 하 여 원하는 알아야 합니다. 현재 상태에서는 사용자가 현재 값에 맞게 문자열을 읽는 데이터 플랫폼의 데이터를 찾습니다. 따라서 플레이어에서 지도를 전환 하는 경우에 맵을 필요가 데이터 플랫폼에 적절 한 이벤트를 보내는으로 게임에 다양 한 상태 문자열을 다시 설정 합니다. 데이터 플랫폼을 통해 해당 방법을 찾아야 데이터에 대 한 몇 초 걸릴를 염두에 두어야 합니다.

그런 다음 사용자 사용자 12345의 다양 한 상태를 읽고 하려고 하는 경우 서비스는 로캘에 관계를 요청 하는 살펴보고 반환 되기 전에 적절 하 게 문자열 형식을 지정 합니다.

이 경우 사용자가 EN-US 문자열을 가정해 보겠습니다. 다음과 같이 작동 풍부한 존재 읽기 (이 호출에 대 한 자세한 내용은 참조 **가져오기 (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

이 API 래퍼는 **PresenceService.GetPresenceAsync 방법** 입니다.

일어나는 해당 xuid 12345는 사용자의 PresenceRecord 요청 하는 다음과 같습니다. 및 세부 정보 수준을 "all" 되도록 요청 하 고 있습니다. "모든"가 지정 하는 경우 다양 한 상태 하지 반환 됩니다.

하 고 다음 JSON 응답에 반환 합니다.

```json
          {
            xuid:"12345",
            state:"online",
            devices:
            [
              {
                type:"D",
                titles:
                [
                  {
                    id:"12345",
                    name:"Buckets are Awesome",
                    lastModified:"2012-09-17T07:15:23.4930000",
                    placement: "full",
                    state:"active",
                    activity:
                    {
                      richPresence:"Playing on map:Mountains"
                    }
                  }
                ]
              }
            ]
          }
```
