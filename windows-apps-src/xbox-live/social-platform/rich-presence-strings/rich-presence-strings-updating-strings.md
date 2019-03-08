---
title: 문자열을 업데이트 하는 다양 한 현재 상태
description: Xbox Live 상대의 문자열을 업데이트 하는 방법에 알아봅니다.
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 다기능 현재 상태, xbox
ms.localizationpriority: medium
ms.openlocfilehash: ac4549301c60eafb935dab0ac9c5b5028452edfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610398"
---
# <a name="rich-presence-updating-strings"></a>문자열을 업데이트 하는 다양 한 현재 상태

제목에 다양 한 현재 상태 문자열을 업데이트 하려면 JSON 개체에 적절 한 매개 변수를 사용 하 여 작성 제목 URI를 호출할 수 있습니다. 이 restful 호출도 Xbox 서비스 Api에서 래핑됩니다. 참조 **Microsoft.Xbox.Services.Presence Namespace** 관련된 API에 대 한 정보에 대 한 합니다.

URI는 다음과 같습니다.

          POST /users/xuid({xuid})/devices/current/titles/current

다음은 다양 한 현재 상태 문자열을 설정 하기 위한 필드만입니다. 여기에 나열 되지 제목 사용자의 쓰기 상태와 관련 된 다른 선택적 필드가 있습니다.

## <a name="titlerequest-object"></a>TitleRequest 개체

속성 | 형식 | Req'd | 설명
---|---|---|---
활동|ActivityRequest|N|제목에 정보 (서식 있는 현재 상태 및 미디어 정보, 사용 가능한 경우)를 설명 하는 레코드

## <a name="activityrequest-object"></a>ActivityRequest 개체

속성 | 형식 | Req'd | 설명
---|---|---|---
richPresence|RichPresenceRequest|N|사용 해야 하는 다양 한 현재 상태 문자열의 friendlyName 합니다.

## <a name="richpresencerequest-object"></a>RichPresenceRequest 개체

속성 | 형식 | Req'd | 설명
---|---|---|---
Id|문자열|Y|사용 해야 하는 다양 한 현재 상태 문자열의 friendlyName
서비스 안내|문자열|Y|대화 상대의 문자열은 정의 되어 있는 알려 주는 서비스 안내 합니다.

예를 들어, 해당 xuid이 12345 인 사용자에 대 한 다양 한 현재 상태를 업데이트 하고자 하는 경우 내 호출 다음과 같이 표시 됩니다.

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

API 래퍼를 사용 하 여,이 것에 대 한 호출 **PresenceService.SetPresenceAsync 메서드**

경우 유지 하 고 데이터 플랫폼 최신 상태로 될 때마다 서식 있는 현재 상태 문자열을 다시 설정 하지 않아도 됩니다. 그런 다음 빈 변경 내용에 맞게 데이터입니다. 위의 예제에서 현재 map를 사용 하려는 알고 있습니다. 현재 상태에서는 사용자가 현재 값을 입력 하는 문자열을 읽을 하려고 할 때 데이터 플랫폼에서 데이터를 찾습니다. 따라서 플레이어는 매핑할에서 전환 하는 경우에 맵을 필요가 데이터 플랫폼에 적절 한 이벤트를 보내는으로 게임에서 다양 한 현재 상태 문자열을 다시 설정 합니다. 데이터 플랫폼을 통해 해당 방법을 찾는 데이터에 대 한 몇 초 걸립니다 염두에 두어야 합니다.

그런 다음 누군가가 사용자 12345의 다양 한 현재 상태를 읽을 때 서비스는 어떤 로캘을 요청 하는 살펴보고 반환 하기 전에 문자열을 적절 하 게 형식을 지정 합니다.

이 예제의 경우 EN-US 문자열을 읽으려면 사용자 한다는 가정해 보겠습니다. 다음과 같이 작동 하는 다기능 현재 상태를 읽는 (이 호출에 대 한 자세한 내용은 참조 하세요. **GET (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

이 API 래퍼는 **PresenceService.GetPresenceAsync 메서드**

일어나 PresenceRecord 인 xuid이 12345 인 사용자의 요청 하는 다음과 같습니다. 및 세부 정보 수준의 "모두" 되도록 요청 합니다. "All"를 지정 하지 않으면 대화 상대의 반환 되지 않습니다.

다음 JSON 응답에서 반환 하는 고:

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
