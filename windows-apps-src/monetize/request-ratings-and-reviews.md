---
Description: 에 대해 알아봅니다 몇 가지 방법에 대해 설정할 수 있습니다. 프로그래밍 방식으로 고객 평가 하 고 앱을 검토 합니다.
title: 앱에 대한 평점 및 리뷰 요청
ms.date: 01/22/2019
ms.topic: article
keywords: windows 10, uwp, 평점, 리뷰
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601308"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>앱에 대한 평점 및 리뷰 요청

유니버설 Windows 플랫폼(UWP) 앱에 코드를 추가하여 앱에 대한 등급과 리뷰를 지정하도록 프로그래밍 방식으로 고객에게 메시지를 표시할 수 있습니다. 이 작업을 수행할 수 있는 여러 가지 방법이 있습니다.
* 앱의 컨텍스트에서 평점 및 리뷰 대화 상자를 직접 표시할 수 있습니다.
* 프로그래밍 방식으로 Microsoft Store에서 앱의 평점 및 리뷰 페이지를 열 수 있습니다.

평가 및 검토 데이터를 분석할 준비가 되었을 때에 파트너 센터에서 데이터를 볼 수도 있고 Microsoft Store 분석 API를 사용 하 여이 데이터를 프로그래밍 방식으로 검색할 수 있습니다.

> [!IMPORTANT]
> 앱 내에서 등급 함수를 추가할 때 모든 검토를 별 등급 선택에 관계 없이 저장소의 등급 메커니즘으로 사용자를 전송 해야 합니다. 사용자의 의견이 나 의견을 수집 하는 경우는 앱 등급 또는 저장소에서의 검토와 관련 되지 않지만 앱 개발자에 게 직접 보내집니다의 선택을 취소 해야 합니다. 개발자 준수와 관련 된 자세한 내용은 참조 하세요 [Fraudulent 또는 악의적인 활동](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)합니다.

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>앱에서 평점 및 리뷰 대화 상자 표시

프로그래밍 방식으로 고객에 게 앱의 등급 및 리뷰를 제출 하도록 요청 하는 앱에서 대화 상자를 표시 하려면 호출을 [RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) 에서 메서드는 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임 스페이스입니다. 

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

합니다 **RequestRateAndReviewAppAsync** 메서드 버전 1809, Windows 10에서 도입 되었으며 사용할 수 있습니다만 대상으로 하는 프로젝트에서 **Windows 10 년 10 월 2018 업데이트 (10.0; 17763 빌드)** 또는 Visual Studio의 이후 릴리스 합니다.

### <a name="response-data-for-the-rating-and-review-request"></a>평점 및 리뷰 요청에 대한 응답 데이터

등급을 표시 하 고 대화 상자에서 검토를 요청을 제출한 후는 [ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) 의 속성을 [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult) 나타내는 JSON 형식 문자열을 포함 하는 클래스 있는지 여부를 요청에 성공 합니다.

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
| *status*       | 고객이 평점 또는 리뷰를 성공적으로 제출했는지 여부를 나타내는 문자열입니다. 지원되는 값은 **success** 및 **aborted**입니다. |
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
* 사용할 수는 [검토](../publish/reviews-report.md) 등급 및 리뷰 고객 으로부터 확인 하려면 파트너 센터에서 보고서입니다. 이 보고서를 다운로드하여 오프라인에서 볼 수도 있습니다.
* Microsoft Store 분석 API에서 [앱 평점 가져오기](get-app-ratings.md) 및 [앱 리뷰 가져오기](get-app-reviews.md) 메서드를 사용하여 JSON 형식으로 된 고객의 평점 및 리뷰를 프로그래밍 방식으로 검색할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [저장소에 요청을 전송 합니다.](send-requests-to-the-store.md)
* [Microsoft Store 앱 시작](../launch-resume/launch-store-app.md)
* [리뷰 보고서](../publish/reviews-report.md)
