---
Description: 고객이 앱을 평가 하 고 검토 하는 데 프로그래밍 방식으로 사용할 수 있는 여러 가지 방법에 대해 알아봅니다.
title: 앱에 대 한 요청 등급 및 검토
ms.date: 01/22/2019
ms.topic: article
keywords: windows 10, uwp, 등급, 리뷰
ms.localizationpriority: medium
ms.openlocfilehash: c0a668ac66f48e386a6299a64e5bcc18cec4fccc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158377"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>앱에 대 한 요청 등급 및 검토

UWP (유니버설 Windows 플랫폼) 앱에 코드를 추가 하 여 고객에 게 앱을 평가 하거나 검토 하도록 프로그래밍 방식으로 요청할 수 있습니다. 이 작업을 수행 하는 방법에는 여러 가지가 있습니다.
* 앱의 컨텍스트에서 등급 및 검토 대화 상자를 직접 표시할 수 있습니다.
* Microsoft Store에서 앱에 대 한 등급 및 검토 페이지를 프로그래밍 방식으로 열 수 있습니다.

등급을 분석 하 고 데이터를 검토할 준비가 되 면 파트너 센터에서 데이터를 보거나 Microsoft Store 분석 API를 사용 하 여이 데이터를 프로그래밍 방식으로 검색할 수 있습니다.

> [!IMPORTANT]
> 앱 내에서 등급 함수를 추가 하는 경우 선택한 별 등급에 관계 없이 모든 검토가 사용자를 매장 등급 메커니즘으로 보내야 합니다. 사용자의 의견이 나 의견을 수집 하는 경우에는 스토어의 앱 등급과 검토와 관련이 없지만 앱 개발자에 게 직접 전송 된다는 것을 명확 하 게 알고 있어야 합니다. [사기성 또는 정직 작업과](/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)관련 된 자세한 내용은 개발자를 위한 개발자 코드를 참조 하세요.

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>앱에서 등급 및 검토 대화 상자 표시

사용자가 앱을 평가 하 고 리뷰를 제출 하도록 요청 하는 대화 상자를 앱에서 프로그래밍 방식으로 표시 하려면 RequestRateAndReviewAppAsync 네임 스페이스에서 [RequestRateAndReviewAppAsync](/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) 메서드를 호출 [합니다.](/uwp/api/windows.services.store) 

> [!IMPORTANT]
> 등급 및 검토 대화 상자를 표시 하는 요청은 앱의 UI 스레드에서 호출 해야 합니다.

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

**RequestRateAndReviewAppAsync** 메서드는 windows 10 버전 1809에서 도입 되었으며 windows 10 10 월 2018 업데이트를 대상으로 하는 프로젝트 에서만 사용할 수 있습니다 **(10.0; 빌드 17763)** 또는 Visual Studio의 이후 릴리스.

### <a name="response-data-for-the-rating-and-review-request"></a>등급 및 검토 요청에 대 한 응답 데이터

등급 및 검토 대화 상자를 표시 하는 요청을 제출 하면 [StoreRateAndReviewResult](/uwp/api/windows.services.store.storerateandreviewresult) 클래스의 [ExtendedJsonData](/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) 속성에는 요청이 성공 했는지 여부를 나타내는 JSON 형식의 문자열이 포함 됩니다.

다음 예에서는 고객이 등급 또는 검토를 성공적으로 제출한 후이 요청에 대 한 반환 값을 보여 줍니다.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

다음 예에서는 고객이 등급 또는 검토를 제출 하지 않기로 선택한 후이 요청에 대 한 반환 값을 보여 줍니다.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

다음 표에서는 JSON 형식 데이터 문자열의 필드에 대해 설명 합니다.

| 필드          | Description                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *status*       | 고객이 등급과 리뷰를 성공적으로 제출 했는지 여부를 나타내는 문자열입니다. 지원 되는 값은 **success** 이며 **중단**됩니다. |
| *data*         | *업데이트*된 이라는 단일 부울 값을 포함 하는 개체입니다. 이 값은 고객이 기존 등급 또는 검토를 업데이트 했는지 여부를 나타냅니다. *데이터* 개체는 성공 응답에만 포함 됩니다. |
| *errorDetails* | 요청에 대 한 오류 정보를 포함 하는 문자열입니다.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>스토어에서 앱에 대 한 등급 및 검토 페이지 시작

스토어에서 앱에 대 한 등급 및 검토 페이지를 프로그래밍 방식으로 열려면 [LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync) ```ms-windows-store://review``` 이 코드 예제에 나와 있는 것 처럼 URI 체계와 함께 LaunchUriAsync 메서드를 사용할 수 있습니다.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

자세한 내용은 [Microsoft Store 앱 시작](../launch-resume/launch-store-app.md)을 참조 하세요.

## <a name="analyze-your-ratings-and-reviews-data"></a>등급을 분석 하 고 데이터를 검토 합니다.

고객의 등급을 분석 하 고 데이터를 검토 하려면 다음과 같은 몇 가지 옵션을 사용할 수 있습니다.
* 파트너 센터에서 [리뷰](../publish/reviews-report.md) 보고서를 사용 하 여 고객의 등급과 리뷰를 확인할 수 있습니다. 이 보고서를 다운로드 하 여 오프 라인으로 볼 수도 있습니다.
* 스토어 분석 API에서 [앱 등급 가져오기](get-app-ratings.md) 및 [앱 리뷰 가져오기](get-app-reviews.md) 메서드를 사용 하 여 JSON 형식으로 고객의 등급과 리뷰를 프로그래밍 방식으로 검색할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Store에 요청 보내기](send-requests-to-the-store.md)
* [Microsoft Store 앱 열기](../launch-resume/launch-store-app.md)
* [리뷰 보고서](../publish/reviews-report.md)