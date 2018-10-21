---
title: 다양 한 상태 문자열 업데이트
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
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5160153"
---
# <a name="rich-presence-updating-strings"></a>다양 한 상태 문자열 업데이트

타이틀에서 다양 한 상태 문자열을 업데이트 하려면 JSON 개체에 적절 한 매개 변수를 사용 하 여 작성 제목 URI를 호출할 수 있습니다. 이 restful 호출은 Xbox 서비스 Api도 래핑됩니다. **Microsoft.Xbox.Services.Presence Namespace** 관련된 API에 대 한 정보를 참조 하세요.

URI는 다음과 같습니다.

          POST /users/xuid({xuid})/devices/current/titles/current

다음은 다양 한 상태 문자열을 설정 하기 위한 필드만입니다. 여기에 나열 되지 않은 제목에 대 한 쓰기 현재 상태에 관련 된 선택적 필드가 다른입니다.

## <a name="titlerequest-object"></a>TitleRequest 개체

속성 | 형식 | 필수 | 설명
---|---|---|---
작업|ActivityRequest|N|제목에서 정보 (다양 한 상태 및 미디어 정보를 사용할 수 있는 경우)를 설명 하는 레코드

## <a name="activityrequest-object"></a>ActivityRequest 개체

속성 | 형식 | 필수 | 설명
---|---|---|---
richPresence|RichPresenceRequest|N|사용 해야 하는 다양 한 상태 문자열의 friendlyName 합니다.

## <a name="richpresencerequest-object"></a>RichPresenceRequest 개체

속성 | 형식 | 필수 | 설명
---|---|---|---
Id|문자열|예|FriendlyName 사용 해야 하는 다양 한 상태 문자열
서비스 안내|문자열|예|다양 한 상태 문자열 정의 되어 있는 알려주는 서비스 안내 합니다.

예를 들어 있는 xuid 12345는 사용자에 대 한 다양 한 상태를 업데이트 하고자 하는 경우 내 호출 모습으로 다음과 같이:

          POST /users/xuid(12345)/devices/current/titles/current


다음 JSON 본문:

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

래퍼 API를 사용 하 여, 이것이 **PresenceService.SetPresenceAsync 메서드** 호출

관리 경우 데이터 플랫폼 최신 때마다 다양 한 상태 문자열을 다시 설정 필요가 다음 빈 변경에 맞게 데이터. 위 예제에서 현재 지도 사용 하 여 원하는 알아야 합니다. 현재 상태에서는 사용자가 현재 값에 맞게 문자열 하려고 할 때 데이터 플랫폼의 데이터를 찾습니다. 따라서 플레이어에서 지도를 전환 하는 경우에 맵을 필요가 적절 한 이벤트 데이터 플랫폼을 보내는으로 게임에 다양 한 상태 문자열을 다시 설정 합니다. 데이터 플랫폼을 통해 해당 방법을 찾아야 데이터에 대 한 몇 초 걸릴을 염두에 두어야 합니다.

그런 다음 사용자 12345의 다양 한 상태를 읽고 하려고 누군가 서비스는 로캘에 관계를 요청 하는 살펴보고 반환 되기 전에 적절 하 게 문자열 형식을 지정 합니다.

이 경우 사용자가 EN-US 문자열을 가정해 보겠습니다. 다음과 같이 작동 풍부한 존재 읽기 (이 호출에 대 한 자세한 내용은 참조 **가져오기 (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

이 API 래퍼는 **PresenceService.GetPresenceAsync 메서드**

과정과의 사용자, 해당 xuid 12345 PresenceRecord 해줘야는 다음과 같습니다. 및 세부 정보 수준을 "모든" 되도록 요청 합니다. "모든"가 지정 하는 경우 다양 한 상태 하지 반환 됩니다.

및 다음 JSON 응답에 반환 합니다.

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
