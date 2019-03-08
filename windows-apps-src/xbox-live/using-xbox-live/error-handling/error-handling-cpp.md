---
title: C++ API 오류 처리
description: C + + Api를 사용 하 여 Xbox Live 서비스 호출을 수행할 때 오류를 처리 하는 방법을 알아봅니다.
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox, 오류 처리
ms.localizationpriority: medium
ms.openlocfilehash: 90fd816f8d44b27c1df0ded9bee6473f642478a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636528"
---
# <a name="c-api-error-handling"></a>C++ API 오류 처리

C + + API 대신 예외 throw, 대부분의 호출 xbox_live_result < payload_type > 적절 하 게 반환 됩니다.

## <a name="xboxliveresult-structure"></a>xbox_live_result 구조
xbox_live_result 3 개 항목에 있습니다.
1. 작업에서 반환 된 오류
2. 디버깅 목적으로 사용 되는 특정 오류 메시지 및
3. 페이로드 (오류가 발생 한 경우에 비어 있을 수 있습니다) 결과를 "

Xbox Live 설명서의 코드는 발생 한 오류의 뿐만으로 xbox_live_result 대 한 자세한 정보를 가져올 수 있습니다.

구조는 다음과 같습니다.

```cpp
template<typename T>
class xbox_live_result
{
    const std::error_code& err();
    const std::string& err_message();
    T& payload();
};
```

**err** -오류를 반환 합니다.  오류 없이 NULL 참조가 됩니다.  이 오류는 value () 호출 하 여 기본 값을 가져올 수 있다는 점에서 c + + STL 처럼 작동 합니다.  호출 message ()는 문자열 표현을 가져옵니다.  오류 코드를 다음 "인수가 잘못 되었습니다."를 의미 하는 경우 ```err().message()``` 텍스트 "잘못 된 인수"는 것입니다.

**err_message** -오류에 설명 합니다.  예를 들어 있으면 **err** "인수가 잘못 되었습니다." 이면 **err_message** 중점적으로 설명 하는에 인수가 잘못 되었습니다.

**페이로드** -관심 있는 항목을 반환 합니다.  예를 들어 ```xbox_live_result<achievement>``` 호출 get_achievement에서 얻을 수 있습니다.  이 예에서 페이로드 (오류가 있는 경우) 자체 도전 과제 것입니다.

## <a name="example"></a>예

```cpp
// Function which returns an xbox_live_result
xbox_live_result<std::shared_ptr<title_presence_change_subscription>> presenceChangeSubscriptionResult =
xbox::services::presence::subscribe_to_title_presence_change(
    xboxUserId,
    titleId
    );

printf("Error value %d, string %s", achievementResult.err().value(), achievementResult.err().message());

// Would output:
// "0 Success" if successful
// "401 Unauthorized" if auth issue

if (!achievementResult.err())
{
  // Do things on success.  Payload will be populated if applicable.
  std::shared_ptr<title_presence_change_subscription> presenceChangeSubscription = presenceChangeSubscriptionResult->payload();

  // ...
}
else if (achievement.err() == xboxlive_error_code::http_status_403_forbidden)
{
  // Special handling for 403 errors
}
else if (achievementResult.err() == xbox_live_error_condition::auth)
{
  // Handle broad auth failures.  See below section for more info on xbox_live_error_condition
}

```

## <a name="using-xboxliveerrorcondition-to-test-against-broad-error-categories"></a>Xbox_live_error_condition를 사용 하 여 광범위 한 오류 범주에 대 한 테스트
위의 예제에서 오류 코드 403 오류 뿐 이라는 것에 대해 테스트 ```xbox_live_error_condition::auth```합니다.

 Xbox_live_result err() 함수를 사용 하는 경우 하나 수 오류 코드에 대해 개별적으로 테스트 합니다.  예를 들어 400 클래스 오류에 대 한 개별 테스트,에 대 한 흐름 제어를 가질 수 있습니다.

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* etc

일반적으로 이것은 원하지 관련이 있고 하나로 오류의 클래스에 대해 테스트 하려는 합니다.  클래스에서 사용할 수 있는 열거형을 사용 하 여 오류에 대해 테스트할 수 있도록 합니다 ```xbox_live_error_condition``` 클래스입니다.  많은 오류 코드에 대 한 테스트를 자동화 하는 같음 연산자에 대 한 오버 로드를 구현 합니다.  외에 ```auth```와 같은 범주가 ```rta``` 및 ```http```합니다.  전체 목록에서 찾을 수 있습니다 *errors.h* 나 *xblsdk_cpp.chm*합니다.

따라서 및 c + + Xbox 서비스 API의 다른 기능에 설명 하는 비디오를 확인 하세요에서 우리의 XFest 말하는 [Xfest 2015 비디오](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) 아래에서 *XSAPI: C + +에서 예외 없음!*
