---
Description: You can use the SendRequestAsync method to send requests to the Microsoft Store for operations that do not yet have an API available in the Windows SDK.
title: Microsoft Store에 요청 보내기
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, StoreRequestHelper, SendRequestAsync
ms.localizationpriority: medium
ms.openlocfilehash: d492bc7dde990404552689516731850974c31a7c
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8710743"
---
# <a name="send-requests-to-the-microsoft-store"></a>Microsoft Store에 요청 보내기

Windows 10 버전 1607부터 Windows SDK는 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 Microsoft Store 관련 작업(예: 앱에서 바로 구매)을 위한 API를 제공합니다. 그러나 OS 릴리스 간에 스토어를 지원하는 서비스가 지속적으로 업데이트, 확장 및 개선되고 있지만 일반적으로 새 API는 주요 OS 릴리스 동안에만 Windows SDK에 제공됩니다.

새 버전의 Windows SDK가 릴리스되기 전까지 UWP(유니버설 Windows 플랫폼) 앱에 새 스토어 작업을 유연하게 제공할 수 있도록 [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) 메서드가 제공됩니다. 이 메서드를 사용하여 아직 Windows SDK 최신 릴리스에 해당 API가 제공되지 않는 새 작업에 대한 요청을 스토어로 보낼 수 있습니다.

> [!NOTE]
> **SendRequestAsync** 메서드는 Windows 10 버전 1607 이상을 대상으로 하는 앱에만 사용할 수 있습니다. 이 메서드에서 지원하는 요청 중 일부는 Windows 10 버전 1607 이후 릴리스에서만 지원됩니다.

**SendRequestAsync**는 [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper) 클래스의 정적 메서드입니다. 이 메서드를 호출하려면 이 메서드에 다음 정보를 전달해야 합니다.
* 작업을 수행하려는 사용자에 대한 정보를 제공하는 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 개체. 이 개체에 대한 자세한 내용은 [StoreContext 클래스 시작](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class)을 참조하세요.
* 스토어로 보낼 요청을 식별하는 정수.
* 요청에서 모든 인수를 지원하는 경우 요청과 함께 전달할 인수가 포함된 JSON 형식의 문자열도 전달할 수 있습니다.

다음 예에서는 이 메서드를 호출하는 방법을 보여 줍니다. 이 예에는 **Windows.Services.Store** 및 **System.Threading.Tasks** 네임스페이스에 대한 using 문이 필요합니다.

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

현재 **SendRequestAsync** 메서드를 통해 제공되는 요청에 대한 내용은 다음 섹션을 참조하세요. 새 요청에 대한 지원이 추가되면 이 문서가 업데이트될 것입니다.

## <a name="request-for-in-app-ratings-and-reviews"></a>앱 내 평점 및 리뷰 요청

**SendRequestAsync** 메서드에 요청 정수 16을 전달하여 고객에게 앱에 대한 평점을 지정하고 리뷰를 제출하도록 요청하는 대화 상자를 앱에서 프로그래밍 방식으로 실행할 수 있습니다. 자세한 내용은 [앱에서 평점 및 리뷰 대화 상자 표시](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)를 참조하세요.

## <a name="requests-for-flight-group-scenarios"></a>플라이트 그룹 시나리오에 대한 요청

> [!IMPORTANT]
> 이 섹션에서 설명하는 모든 플라이트 그룹 요청은 현재 대부분의 개발자 계정에서 사용할 수 있습니다. Microsoft에서 개발자 계정을 특별히 프로비전하지 않는 한 이러한 요청은 실패합니다.

**SendRequestAsync** 메서드는 플라이트 그룹에 사용자 또는 디바이스 추가와 같은 플라이트 그룹 시나리오에 대한 일련의 요청을 지원합니다. 이러한 요청을 제출하려면 관련 인수와 함께 제출할 요청을 나타내는 *requestKind* 매개 변수에 값 7 또는 8을 전달하고 JSON 형식 문자열을 *parametersAsJson* 매개 변수에 전달합니다. 이러한 *requestKind* 값은 다음과 같은 차이점이 있습니다.

|  요청 종류 값  |  설명  |
|----------------------|---------------|
|  7                   |  현재 디바이스와 관련하여 요청이 수행됩니다. 이 값은 Windows 10 버전 1703 이상에만 사용할 수 있습니다.  |
|  8                   |  현재 스토어에 로그인되어 있는 사용자와 관련하여 요청이 수행됩니다. 이 값은 Windows 10 버전 1607 이상에서 사용할 수 있습니다.  |

다음 플라이트 그룹 요청은 현재 구현되었습니다.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>최상위 플라이트 그룹에 대한 원격 변수 검색

> [!IMPORTANT]
> 이 요청은 현재 대부분 개발자 계정에서 사용할 수 없습니다. Microsoft에서 개발자 계정을 특별히 프로비전하지 않는 한 이 요청은 실패합니다.

이 요청은 현재 사용자 또는 디바이스의 최상위 플라이트 그룹에 대한 원격 변수를 검색합니다. 이 요청을 보내려면 **SendRequestAsync** 메서드의 *requestKind* 및 *parametersAsJson* 매개 변수에 다음 정보를 전달합니다.

|  매개 변수  |  설명  |
|----------------------|---------------|
|  *requestKind*                   |  디바이스의 최상위 플라이트 그룹을 반환하려면 7을 지정하고, 현재 사용자 및 디바이스의 최상위 플라이트 그룹을 반환하려면 8을 지정합니다. *requestKind* 매개 변수에는 8을 사용하는 것이 좋습니다. 왜냐하면 이 값은 현재 사용자와 디바이스의 구성원 전체 중에서 최상위 플라이트 그룹을 반환하기 때문입니다.  |
|  *parametersAsJson*                   |  아래 예제에 표시된 데이터를 포함하는 JSON 형식 문자열을 전달합니다.  |

다음 예제는 JSON 형식 데이터를 *parametersAsJson* 매개 변수로 전달하는 것을 보여 줍니다. *유형* 필드를 *GetRemoteVariables* 문자열에 할당해야 합니다. 파트너 센터에서 원격 변수를 정의한 프로젝트 id *projectId* 필드를 할당 합니다.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

이 요청을 제출하면 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 반환 값의 [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) 속성에 다음 필드가 있는 JSON 형식 문자열이 포함됩니다.

|  필드  |  설명  |
|----------------------|---------------|
|  *익명*                   |  부울 값이며, **true**는 사용자 또는 디바이스 ID가 요청에 없음을 나타내고 **false**는 사용자 또는 디바이스 ID가 요청에 있음을 나타냅니다.  |
|  *이름*                   |  디바이스 또는 사용자가 속한 최상위 플라이트 그룹의 이름을 포함하고 있는 문자열입니다.  |
|  *설정*                   |  개발자가 플라이트 그룹에 대해 구성한 원격 변수의 이름과 값을 포함하고 있는 키/값 쌍 사전입니다.  |

다음 예에서는 이 요청에 대한 반환 값을 보여 줍니다.

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>플라이트 그룹에 현재 디바이스 또는 사용자 추가

> [!IMPORTANT]
> 이 요청은 현재 대부분 개발자 계정에서 사용할 수 없습니다. Microsoft에서 개발자 계정을 특별히 프로비전하지 않는 한 이 요청은 실패합니다.

이 요청을 보내려면 **SendRequestAsync** 메서드의 *requestKind* 및 *parametersAsJson* 매개 변수에 다음 정보를 전달합니다.

|  매개 변수  |  설명  |
|----------------------|---------------|
|  *requestKind*                   |  플라이트 그룹에 디바이스를 추가하려면 7을 지정하고, 현재 스토어에 로그인된 사용자를 플라이트 그룹에 추가하려면 8을 지정합니다.  |
|  *parametersAsJson*                   |  아래 예제에 표시된 데이터를 포함하는 JSON 형식 문자열을 전달합니다.  |

다음 예제는 JSON 형식 데이터를 *parametersAsJson* 매개 변수로 전달하는 것을 보여 줍니다. *유형* 필드를 *AddToFlightGroup* 문자열에 할당해야 합니다. 디바이스 또는 사용자를 추가할 플라이트 그룹 ID에 *flightGroupId* 필드를 할당합니다.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

이 요청에 오류가 있으면 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 반환 값의 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 속성에 응답 코드가 포함됩니다.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>플라이트 그룹에서 현재 디바이스 또는 사용자 제거

> [!IMPORTANT]
> 이 요청은 현재 대부분 개발자 계정에서 사용할 수 없습니다. Microsoft에서 개발자 계정을 특별히 프로비전하지 않는 한 이 요청은 실패합니다.

이 요청을 보내려면 **SendRequestAsync** 메서드의 *requestKind* 및 *parametersAsJson* 매개 변수에 다음 정보를 전달합니다.

|  매개 변수  |  설명  |
|----------------------|---------------|
|  *requestKind*                   |  플라이트 그룹에서 디바이스를 제거하려면 7을 지정하고, 현재 스토어에 로그인된 사용자를 플라이트 그룹에서 제거하려면 8을 지정합니다.  |
|  *parametersAsJson*                   |  아래 예제에 표시된 데이터를 포함하는 JSON 형식 문자열을 전달합니다.  |

다음 예제는 JSON 형식 데이터를 *parametersAsJson* 매개 변수로 전달하는 것을 보여 줍니다. *유형* 필드를 *RemoveFromFlightGroup* 문자열에 할당해야 합니다. 디바이스 또는 사용자를 제거할 플라이트 그룹 ID에 *flightGroupId* 필드를 할당합니다.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

이 요청에 오류가 있으면 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 반환 값의 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 속성에 응답 코드가 포함됩니다.

## <a name="related-topics"></a>관련 항목

* [앱에서 평점 및 리뷰 대화 상자 표시](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)
