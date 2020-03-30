---
Description: 고객이 앱을 평가 하 고 검토 하는 데 프로그래밍 방식으로 사용할 수 있는 여러 가지 방법에 대해 알아봅니다.
title: 앱에 대한 평점 및 리뷰 요청
ms.date: 01/22/2019
ms.topic: article
keywords: windows 10, uwp, 평점, 리뷰
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210719"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>앱에 대한 평점 및 리뷰 요청

유니버설 Windows 플랫폼(UWP) 앱에 코드를 추가하여 앱에 대한 등급과 리뷰를 지정하도록 프로그래밍 방식으로 고객에게 메시지를 표시할 수 있습니다. 이 작업을 수행할 수 있는 여러 가지 방법이 있습니다.
* 앱의 컨텍스트에서 평점 및 리뷰 대화 상자를 직접 표시할 수 있습니다.
* 프로그래밍 방식으로 Microsoft Store에서 앱의 평점 및 리뷰 페이지를 열 수 있습니다.

등급을 분석 하 고 데이터를 검토할 준비가 되 면 파트너 센터에서 데이터를 보거나 Microsoft Store 분석 API를 사용 하 여이 데이터를 프로그래밍 방식으로 검색할 수 있습니다.

> [!IMPORTANT]
> 앱 내에서 등급 함수를 추가 하는 경우 선택한 별 등급에 관계 없이 모든 검토가 사용자를 매장 등급 메커니즘으로 보내야 합니다. 사용자의 의견이 나 의견을 수집 하는 경우에는 스토어의 앱 등급과 검토와 관련이 없지만 앱 개발자에 게 직접 전송 된다는 것을 명확 하 게 알고 있어야 합니다. [사기성 또는 정직 작업과](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)관련 된 자세한 내용은 개발자를 위한 개발자 코드를 참조 하세요.

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>앱에서 평점 및 리뷰 대화 상자 표시

사용자가 앱을 평가 하 고 리뷰를 제출 하도록 요청 하는 대화 상자를 앱에서 프로그래밍 방식으로 표시 하려면 [RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) 네임 스페이스에서  메서드를 호출 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). 

> [!IMPORTANT]
> 평점 및 리뷰 대화 상자를 표시하라는 요청은 앱의 UI 스레드에서 호출되어야 합니다.

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

### <a name="response-data-for-the-rating-and-review-request"></a>평점 및 리뷰 요청에 대한 응답 데이터

등급 및 검토 대화 상자를 표시 하는 요청을 제출 하면 [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult) 클래스의 [ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) 속성에는 요청이 성공 했는지 여부를 나타내는 JSON 형식의 문자열이 포함 됩니다.

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

| 필드          | 설명                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *상태*       | 고객이 평점 또는 리뷰를 성공적으로 제출했는지 여부를 나타내는 문자열입니다. 지원되는 값은 **success** 및 **aborted**입니다. |
| *data*         | 이름이 *updated*인 단일 부울 값이 포함된 개체입니다. 이 값은 고객이 기존 평점 또는 리뷰를 업데이트했는지 여부를 나타냅니다. *data* 개체는 성공 응답에만 포함됩니다. |
| *errorDetails* | 요청에 대한 오류 세부 정보가 있는 문자열입니다.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Microsoft Store에서 앱의 평점 및 리뷰 페이지 시작

Microsoft Store에서 앱의 평점 및 리뷰 페이지를 프로그래밍 방식으로 열고 싶은 경우 이 코드 예제에서 설명된 대로 ```ms-windows-store://review``` URI 스키마와 함께 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 메서드를 사용할 수 있습니다.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

자세한 내용은 [Microsoft Store 앱 실행](../launch-resume/launch-store-app.md)을 참조하세요.

## <a name="analyze-your-ratings-and-reviews-data"></a>평점 및 리뷰 데이터 분석

고객의 평점 및 리뷰 데이터를 분석하는 데 몇 가지 옵션이 있습니다.
* 파트너 센터에서 [리뷰](../publish/reviews-report.md) 보고서를 사용 하 여 고객의 등급과 리뷰를 확인할 수 있습니다. 이 보고서를 다운로드하여 오프라인에서 볼 수도 있습니다.
* Microsoft Store 분석 API에서 [앱 평점 가져오기](get-app-ratings.md) 및 [앱 리뷰 가져오기](get-app-reviews.md) 메서드를 사용하여 JSON 형식으로 된 고객의 평점 및 리뷰를 프로그래밍 방식으로 검색할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [저장소에 요청 보내기](send-requests-to-the-store.md)
* [Microsoft Store 앱 열기](../launch-resume/launch-store-app.md)
* [리뷰 보고서](../publish/reviews-report.md)
