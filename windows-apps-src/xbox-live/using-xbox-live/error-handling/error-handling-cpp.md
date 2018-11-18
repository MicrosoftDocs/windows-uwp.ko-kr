---
title: C++ API 오류 처리
author: KevinAsgari
description: C + + Api를 사용 하 여 Xbox Live 서비스 호출을 만들 때 오류를 처리 하는 방법을 알아봅니다.
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox, 오류 처리
ms.localizationpriority: medium
ms.openlocfilehash: 4a9947180764d196579569536ec979fc31f3570d
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7170132"
---
# <a name="c-api-error-handling"></a>C++ API 오류 처리

C + + API 대신 예외를 throw, 대부분의 호출은 xbox_live_result < payload_type >를 적절 하 게 반환 합니다.

## <a name="xboxliveresult-structure"></a>xbox_live_result 구조
xbox_live_result에 3 개의 항목이 있습니다.
1. 작업에서 반환한 오류
2. 디버깅 목적으로 사용 되는 특정 오류 메시지 및
3. (오류가 발생 하는 경우에 비어 있을 수 있습니다) 결과의 페이로드 "

뿐만 아니라 어떤 오류 코드는 Xbox Live 설명서에서으로 xbox_live_result에 대 한 자세한 정보를 얻을 수 있습니다.

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

**오류** -오류를 반환 합니다.  오류 없이 NULL 참조가 됩니다.  이 오류는 직접 호출 하 여 기본 값을 가져올 수 있다는 점에서 c + + STL으로 동작 합니다.  호출 message() 문자열 표현을 받게 됩니다.  오류 코드는 다음 "잘못 된 인수"를 의미 하는 경우 ```err().message()``` 텍스트 "잘못 된 인수" 됩니다.

**err_message** -오류를 설명 합니다.  예를 들어 **err_message** 있는 인수에 대해 자세히는 "잘못 된 인수"가 **오류** 하지 유효 합니다.

**페이로드** -관심 있는 항목을 반환 합니다.  예를 들어 ```xbox_live_result<achievement>``` 는 호출 get_achievement에서 가져올 수 있습니다.  이 예제에서는 페이로드 (오류가 없는 경우) 하는 경우 자체 도전 과제 것입니다.

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
위의 예제에서는 403 오류 뿐 아니라 라는 것에 대 한 오류 코드 테스트 ```xbox_live_error_condition::auth```.

 Xbox_live_result err() 기능을 사용 하는 경우 하나를 테스트할 수 오류 코드에 대해 개별적으로.  예를 들어 400 클래스 오류에 대 한 개별 테스트,에 대 한 흐름 제어를 가질 수 있습니다.

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* 등

일반적으로 원하지 작업을 수행 하는이 있고 하나로 오류의 클래스에 대해 테스트 하려는 합니다.  클래스에서 사용할 수 있는 열거형을 사용 하 여 오류에 대해 테스트할 수 있도록 합니다 ```xbox_live_error_condition``` 클래스.  많은 오류 코드에 대 한 테스트를 자동화 하는 같음 연산자에 대 한 오버 로드를 구현 합니다.  외에 ```auth```, 같은 종류 ```rta``` 및 ```http```합니다.  전체 목록은 *xblsdk_cpp.chm*또는 *errors.h* 에서 찾을 수 있습니다.

이 및 c + + Xbox 서비스 API의 일부 다른 기능에 설명 하는 비디오를 확인 하세요 [Xfest 2015 동영상](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) 아래에서 우리의 XFest 음성 채팅 *XSAPI: c + +, 예외 없음!*
