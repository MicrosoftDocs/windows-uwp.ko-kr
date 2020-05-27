---
Description: SendRequestAsync 메서드를 사용 하 여 Windows SDK에서 아직 API를 사용할 수 없는 작업에 대 한 Microsoft Store에 요청을 보낼 수 있습니다.
title: Microsoft Store에 요청 보내기
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, StoreRequestHelper, SendRequestAsync
ms.localizationpriority: medium
ms.openlocfilehash: 810c546eb0ee0263dcb50b3ce58e593ad294435c
ms.sourcegitcommit: 577a54d36145f91c8ade8e4509d4edddd8319137
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83867333"
---
# <a name="send-requests-to-the-microsoft-store"></a>Microsoft Store에 요청 보내기

Windows 10 버전 1607부터 Windows SDK은 [windows](https://docs.microsoft.com/uwp/api/windows.services.store) 의 스토어 관련 작업 (예: 앱에서 바로 구매)을 위한 api를 제공 합니다. 그러나 저장소를 지 원하는 서비스는 OS 릴리스 간에 지속적으로 업데이트, 확장 및 향상 되지만, 일반적으로 주요 OS 릴리스 중에만 새 Api가 Windows SDK에 추가 됩니다.

새 버전의 Windows SDK를 릴리스하기 전에 새 저장소 작업을 UWP (유니버설 Windows 플랫폼) 앱에서 사용할 수 있도록 하는 유연한 방법으로 [Sendrequestasync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) 메서드를 제공 합니다. 이 메서드를 사용 하 여 최신 버전의 Windows SDK에서 사용할 수 있는 해당 API가 아직 없는 새 작업의 저장소에 요청을 보낼 수 있습니다.

> [!NOTE]
> **Sendrequestasync** 메서드는 Windows 10, 버전 1607 이상을 대상으로 하는 앱에만 사용할 수 있습니다. 이 메서드에서 지 원하는 일부 요청은 Windows 10 버전 1607 이후의 릴리스에서만 지원 됩니다.

**Sendrequestasync** 는 [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper) 클래스의 정적 메서드입니다. 이 메서드를 호출 하려면 메서드에 다음 정보를 전달 해야 합니다.
* 작업을 수행 하려는 사용자에 대 한 정보를 제공 하는 [Storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 개체입니다. 이 개체에 대 한 자세한 내용은 [StoreContext 클래스 시작](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class)을 참조 하세요.
* 저장소로 보내려는 요청을 식별 하는 정수입니다.
* 요청에서 인수를 지 원하는 경우 요청과 함께 전달할 인수를 포함 하는 JSON 형식 문자열을 전달할 수도 있습니다.

다음 예제에서는 이 메서드의 호출 방법을 보여 줍니다. 이 예제를 사용 하려면 Windows. **Store** 및 **system.object** 네임 스페이스에 대 한 문을 사용 해야 합니다.

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": { \"flightGroupId\": \"your group ID\" } }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

**Sendrequestasync** 메서드를 통해 현재 사용할 수 있는 요청에 대 한 자세한 내용은 다음 섹션을 참조 하세요. 새 요청에 대 한 지원이 추가 되 면이 문서를 업데이트 합니다.

## <a name="request-for-in-app-ratings-and-reviews"></a>앱 내 등급 및 리뷰에 대 한 요청

앱에서 응용 프로그램을 평가 하 고 요청 정수 16을 **Sendrequestasync** 메서드에 전달 하 여 검토를 제출 하도록 요청 하는 대화 상자를 프로그래밍 방식으로 시작할 수 있습니다. 자세한 내용은 [앱에서 등급 및 검토 대화 상자 표시](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)를 참조 하세요.

## <a name="requests-for-flight-group-scenarios"></a>비행 그룹 시나리오에 대 한 요청

> [!IMPORTANT]
> 이 섹션에서 설명 하는 모든 비행 그룹 요청은 현재 대부분의 개발자 계정에서 사용할 수 없습니다. 개발자 계정이 Microsoft에서 특별히 프로 비전 되는 경우를 제외 하 고 이러한 요청은 실패 합니다.

**Sendrequestasync** 메서드는 항공편 그룹에 사용자 또는 장치를 추가 하는 것과 같은 비행 그룹 시나리오에 대 한 요청 집합을 지원 합니다. 이러한 요청을 제출 하려면 값 7 또는 8을 JSON 형식 문자열을 사용 하 여 *parametersAsJson* *매개 변수에 전달* 합니다 .이 매개 변수는 관련 인수와 함께 전송 하려는 요청을 나타냅니다. 이러한 *Requestkind* 값은 다음과 같은 점에서 다릅니다.

|  요청 종류 값  |  Description  |
|----------------------|---------------|
|  7                   |  요청은 현재 장치의 컨텍스트에서 수행 됩니다. 이 값은 Windows 10, 버전 1703 이상 에서만 사용할 수 있습니다.  |
|  8                   |  요청은 현재 저장소에 로그인 한 사용자의 컨텍스트에서 수행 됩니다. 이 값은 Windows 10, 버전 1607 이상에서 사용할 수 있습니다.  |

다음 비행 그룹 요청은 현재 구현 되어 있습니다.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>가장 높은 순위의 비행 그룹의 원격 변수 검색

> [!IMPORTANT]
> 이 요청은 현재 대부분의 개발자 계정에서 사용할 수 없습니다. 개발자 계정이 Microsoft에서 특별히 프로 비전 되지 않은 경우이 요청이 실패 합니다.

이 요청은 현재 사용자 또는 장치에 대해 순위가 가장 높은 비행 그룹의 원격 변수를 검색 합니다. 이 요청을 보내려면 **Sendrequestasync** 메서드의 *Requestkind* 및 *parametersAsJson* 매개 변수에 다음 정보를 전달 합니다.

|  매개 변수  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  장치에 대해 가장 높은 순위의 비행 그룹을 반환 하려면 7을 지정 하 고, 현재 사용자 및 장치에 대해 순위가 가장 높은 비행 그룹을 반환 하려면 8을 지정 합니다. 이 값은 현재 사용자와 장치 모두에 대 한 멤버 자격에서 가장 높은 순위의 비행 그룹을 반환 하므로 *Requestkind* 매개 변수에 값 8을 사용 하는 것이 좋습니다.  |
|  *parametersAsJson*                   |  아래 예제에 표시 된 데이터를 포함 하는 JSON 형식 문자열을 전달 합니다.  |

다음 예제에서는 *parametersAsJson*에 전달할 JSON 데이터의 형식을 보여 줍니다. *GetRemoteVariables*문자열에 *형식* 필드를 할당 해야 합니다. 파트너 센터에서 원격 변수를 정의한 프로젝트의 ID에 *projectId* 필드를 할당 합니다.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

이 요청을 제출한 후 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 반환 값의 [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) 속성은 다음 필드를 포함 하는 JSON 형식 문자열을 포함 합니다.

|  필드  |  설명  |
|----------------------|---------------|
|  *익명*                   |  부울 값입니다. 여기에서 **true** 는 사용자 또는 장치 id가 요청에 없음을 나타내고, **false** 는 사용자 또는 장치 id가 요청에 표시 됨을 나타냅니다.  |
|  *name*                   |  장치나 사용자가 속하는 가장 높은 순위의 비행 그룹의 이름을 포함 하는 문자열입니다.  |
|  *설정*                   |  개발자가 비행 그룹에 대해 구성한 원격 변수의 이름과 값을 포함 하는 키/값 쌍의 사전입니다.  |

다음 예제에서는이 요청에 대 한 반환 값을 보여 줍니다.

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

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>비행 그룹에 현재 장치 또는 사용자 추가

> [!IMPORTANT]
> 이 요청은 현재 대부분의 개발자 계정에서 사용할 수 없습니다. 개발자 계정이 Microsoft에서 특별히 프로 비전 되지 않은 경우이 요청이 실패 합니다.

이 요청을 보내려면 **Sendrequestasync** 메서드의 *Requestkind* 및 *parametersAsJson* 매개 변수에 다음 정보를 전달 합니다.

|  매개 변수  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  6을 지정 하 여 비행기 그룹에 장치를 추가 하거나 8을 지정 하 여 현재 상점에 로그인 한 사용자를 비행 그룹에 추가 합니다.  |
|  *parametersAsJson*                   |  아래 예제에 표시 된 데이터를 포함 하는 JSON 형식 문자열을 전달 합니다.  |

다음 예제에서는 *parametersAsJson*에 전달할 JSON 데이터의 형식을 보여 줍니다. *AddToFlightGroup*문자열에 *형식* 필드를 할당 해야 합니다. *FlightGroupId* 필드를 장치 또는 사용자를 추가 하려는 비행 그룹의 ID에 할당 합니다.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

요청에 오류가 있는 경우 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 반환 값의 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 속성은 응답 코드를 포함 합니다.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>비행 그룹에서 현재 장치 또는 사용자를 제거 합니다.

> [!IMPORTANT]
> 이 요청은 현재 대부분의 개발자 계정에서 사용할 수 없습니다. 개발자 계정이 Microsoft에서 특별히 프로 비전 되지 않은 경우이 요청이 실패 합니다.

이 요청을 보내려면 **Sendrequestasync** 메서드의 *Requestkind* 및 *parametersAsJson* 매개 변수에 다음 정보를 전달 합니다.

|  매개 변수  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  비행 그룹에서 장치를 제거 하려면 7을 지정 하 고, 현재 비행 그룹에서 스토어에 로그인 한 사용자를 제거 하려면 8을 지정 합니다.  |
|  *parametersAsJson*                   |  아래 예제에 표시 된 데이터를 포함 하는 JSON 형식 문자열을 전달 합니다.  |

다음 예제에서는 *parametersAsJson*에 전달할 JSON 데이터의 형식을 보여 줍니다. *RemoveFromFlightGroup*문자열에 *형식* 필드를 할당 해야 합니다. *FlightGroupId* 필드를 장치 또는 사용자를 제거 하려는 비행 그룹의 ID에 할당 합니다.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

요청에 오류가 있는 경우 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 반환 값의 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 속성은 응답 코드를 포함 합니다.

## <a name="related-topics"></a>관련 항목

* [앱에서 등급 및 검토 대화 상자 표시](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)
