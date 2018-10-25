---
author: Xansky
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: 앱에 대한 평점 및 리뷰 요청
ms.author: mhopkins
ms.date: 06/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 평점, 리뷰
ms.localizationpriority: medium
ms.openlocfilehash: cc3dce673b434673f0e8a72158c2d3a593f02c52
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483693"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>앱에 대한 평점 및 리뷰 요청

유니버설 Windows 플랫폼(UWP) 앱에 코드를 추가하여 앱에 대한 등급과 리뷰를 지정하도록 프로그래밍 방식으로 고객에게 메시지를 표시할 수 있습니다. 이 작업을 수행할 수 있는 여러 가지 방법이 있습니다.
* 앱의 컨텍스트에서 평점 및 리뷰 대화 상자를 직접 표시할 수 있습니다.
* 프로그래밍 방식으로 Microsoft Store에서 앱의 평점 및 리뷰 페이지를 열 수 있습니다.

평점 및 리뷰 데이터를 분석할 준비가 되면 Windows 개발자 센터 대시보드에서 데이터를 볼 수 있거나 Microsoft Store 분석 API를 사용하여 프로그래밍 방식으로 이 데이터를 검색할 수 있습니다.

> [!IMPORTANT]
> 앱 내 평점 함수를 추가할 때 모든 리뷰 별 평점 선택한 상관 없이 저장소의 등급 메커니즘에 사용자를 보내야 합니다. 사용자 로부터 피드백 또는 의견을 수집 하는 경우에 관련이 없는 앱 평점 또는 리뷰를 스토어에서 앱 개발자에 게 직접 전송 되는 명확한 이어야 합니다. 개발자 준수 [Fraudulent 또는 악의적인 활동에](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)관한 자세한 내용은 참조 하세요.

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>앱에서 평점 및 리뷰 대화 상자 표시

고객에게 앱에 대한 평점을 지정하고 리뷰를 제출하도록 요청하는 대화 상자를 앱에서 프로그래밍 방식으로 표시하려면 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) 메서드를 호출합니다. 이 코드 예제에 표시된 대로 *requestKind* 매개 변수에 정수 16을 전달하고 *parametersAsJson* 매개 변수에 빈 문자열을 전달합니다. 이 예제에서는 Newtonsoft의 [Json.NET](http://www.newtonsoft.com/json) 라이브러리가 필요하며 **Windows.Services.Store**, **System.Threading.Tasks** 및 **Newtonsoft.Json.Linq** 네임스페이스에 대한 명령문을 사용해야 합니다.

> [!IMPORTANT]
> 평점 및 리뷰 대화 상자를 표시하라는 요청은 앱의 UI 스레드에서 호출되어야 합니다.

```csharp
public async Task<bool> ShowRatingReviewDialog()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 16, String.Empty);

    if (result.ExtendedError == null)
    {
        JObject jsonObject = JObject.Parse(result.Response);
        if (jsonObject.SelectToken("status").ToString() == "success")
        {
            // The customer rated or reviewed the app.
            return true;
        }
    }

    // There was an error with the request, or the customer chose not to
    // rate or review the app.
    return false;
}
```

**SendRequestAsync** 메서드는 간단한 정수 기반 요청 시스템 및 JSON 기반 데이터 매개 변수를 사용하여 기타 Microsoft Store 조작을 앱에 노출합니다. *requestKind* 매개 변수에 정수 16을 전달하면 평점 및 리뷰 대화 상자를 표시하라는 요청을 실행하고 Microsoft Store에 관련 데이터를 보냅니다. 이 메서드는 Windows 10 버전, 1607에서 도입되었으며 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다. 이 메서드에 대한 일반적인 개요는 [Microsoft Store에 요청 보내기](send-requests-to-the-store.md)를 참조하세요.

### <a name="response-data-for-the-rating-and-review-request"></a>평점 및 리뷰 요청에 대한 응답 데이터

평점 및 리뷰 대화 상자를 표시하라는 요청을 제출하면 [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 반환 값의 [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) 속성에 요청이 성공했는지 여부를 표시하는 JSON 형식 문자열이 포함됩니다.

다음 예제에서는 고객이 평점이나 리뷰를 제출한 후의 이 요청에 대한 반환 값에 대해 설명합니다.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

다음 예제에서는 고객이 평점이나 리뷰를 제출하지 않도록 선택한 후의 이 요청에 대한 반환 값에 대해 설명합니다.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

다음 표에서는 JSON 형식 데이터 문자열의 필드에 대해 설명합니다.

|  필드  |  설명  |
|----------------------|---------------|
|  *status*                   |  고객이 평점 또는 리뷰를 성공적으로 제출했는지 여부를 나타내는 문자열입니다. 지원되는 값은 **success** 및 **aborted**입니다.   |
|  *data*                   |  이름이 *updated*인 단일 부울 값이 포함된 개체입니다. 이 값은 고객이 기존 평점 또는 리뷰를 업데이트했는지 여부를 나타냅니다. *data* 개체는 성공 응답에만 포함됩니다.   |
|  *errorDetails*                   |  요청에 대한 오류 세부 정보가 있는 문자열입니다. |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Microsoft Store에서 앱의 평점 및 리뷰 페이지 시작

Microsoft Store에서 앱의 평점 및 리뷰 페이지를 프로그래밍 방식으로 열고 싶은 경우 이 코드 예제에서 설명된 대로 ```ms-windows-store://review``` URI 스키마와 함께 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 메서드를 사용할 수 있습니다.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

자세한 내용은 [Microsoft Store 앱 실행](../launch-resume/launch-store-app.md)을 참조하세요.

## <a name="analyze-your-ratings-and-reviews-data"></a>평점 및 리뷰 데이터 분석

고객의 평점 및 리뷰 데이터를 분석하는 데 몇 가지 옵션이 있습니다.
* Windows 개발자 센터 대시보드에서 [리뷰](../publish/reviews-report.md) 보고서를 사용하여 고객의 평점 및 리뷰를 확인할 수 있습니다. 이 보고서를 다운로드하여 오프라인에서 볼 수도 있습니다.
* Microsoft Store 분석 API에서 [앱 평점 가져오기](get-app-ratings.md) 및 [앱 리뷰 가져오기](get-app-reviews.md) 메서드를 사용하여 JSON 형식으로 된 고객의 평점 및 리뷰를 프로그래밍 방식으로 검색할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Store에 요청 보내기](send-requests-to-the-store.md)
* [Microsoft Store 앱 실행](../launch-resume/launch-store-app.md)
* [리뷰 보고서](../publish/reviews-report.md)
