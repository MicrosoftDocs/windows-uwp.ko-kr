---
title: Xbox Live API 소개
description: Xbox Live 서비스와 상호 작용 하는 데 사용할 수 있는 다양 한 API 모델에 알아봅니다.
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f52e379b524952c3361b432a577a7137b02155b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657698"
---
# <a name="introduction-to-xbox-live-apis"></a>Xbox Live API 소개

## <a name="use-xbox-live-services"></a>Xbox Live 서비스 사용

두 가지 방법으로 Xbox Live 서비스에서 정보를 가져올 수 있습니다.

- Xbox Live Services API를 호출 하는 클라이언트 쪽 API를 사용 하 여 (**XSAPI**)
- 호출 된 **Xbox Live REST 끝점** 직접

Xbox Live Services API를 사용 하는 이점은 (**XSAPI**)를 포함 합니다.

- 인증, 인코딩 및 HTTP 보내기 및 받기의 세부 정보를 취급 됩니다.
- 에 인수 및 데이터에서 반환 된, 래퍼 API 형식; 기본 데이터에서 처리 됩니다. 따라서 JSON 인코딩 및 디코딩을 수행할 필요가 없습니다.
- 웹 서비스를 직접 호출 래퍼 API 캡슐화; 하는 여러 비동기 단계로 이루어집니다. 이렇게 하면 제목 코드 쉽게 읽고 쓸 수 있습니다.
- 게임 이벤트를 작성 하는 등의 일부 기능은 XSAPI에서 제공 됩니다.

사용 하는 이점은 합니다 **Xbox Live REST 끝점** 직접 포함 합니다.

- 웹 서비스에서 Xbox Live 끝점을 호출 하는 기능
- XSAPI 포함 되지 않을 수 있는 끝점을 호출할 수 있습니다.  XSAPI는 게임을 사용 하는 포럼을 통해 알려주세요 let 누락 된 부분이 있으면 되므로 Api에만 포함 되어 있습니다.
- REST 끝점을 통해 사용할 수 있는 일부 기능에는 해당 XSAPI 래퍼를 가질 수 없습니다.

게임 및 앱에 이러한 메서드 중 하나만 사용 하도록 제한 되지 않습니다. XSAPI 래퍼를 사용 하 고 필요한 경우 직접에서 REST 끝점을 여전히 호출할 수 있습니다.

## <a name="xbox-live-services-api-overview"></a>Xbox Live 서비스 API 개요 ##

Xbox Live Services API (**XSAPI**) 세 가지 클라이언트 쪽 광범위 한 고객 시나리오를 지 원하는 Api 노출 합니다.

- [XSAPI WinRT API](#xsapi-winrt-based-api)
- [C + + 11 XSAPI 기반 API](#xsapi-c11-based-api)
- [XSAPI C 기반 API](#xsapi-c-based-api) (**2018 년 6 월을 기준으로 새**)

Api를 비교 합니다.

### <a name="xsapi-winrt-based-api"></a>XSAPI WinRT 기반 API

- C +를 사용 하 여 작성 된 응용 프로그램을 지 원하는 + /CX에서는 C#, 및 JavaScript입니다.
    - C + + /cli CX는 쉽게 WinRT 프로그래밍 예를 들어 사용 하 여 Microsoft c + + 확장 ^ WinRT 포인터로 합니다.
- Xbox One XDK 플랫폼과 Windows 플랫폼 (UWP (유니버설) x86, x64 및 ARM 아키텍처를 대상으로 하는 응용 프로그램을 지원 합니다.
- C +를 비롯 한 모든 언어로 예외를 통해 오류를 처리 하는 + CX 합니다.
- C + + /cli WinRT도 지원 됩니다.  자세한 내용은 C + + /cli WinRT에서 찾을 수 있습니다 [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

C +를 사용 하 여 XSAPI WinRT API 호출의 예로 + WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

하려면 혼합 C + + /CX 및 C + + /cli WinRT 것은 너무이 수행할 수 있지만 좀 더 복잡 한 코드 마이그레이션.  
C +를 사용 하 여 XSAPI WinRT API 호출의 예로 + WinRT 지정 C + + /cli CX 사용자 ^ 개체입니다.

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>C + + 11 XSAPI 기반 API

- 사용 하 여 크로스 플랫폼 ISO 표준 C + + 11
- C + +로 작성 된 지원 응용 프로그램
- Xbox One XDK 플랫폼과 Windows 플랫폼 (UWP (유니버설) x86, x64 및 ARM 아키텍처를 대상으로 하는 응용 프로그램을 지원 합니다.
- 오류는 std::error_code 통해 처리 됩니다.
- C + + 11 기반된 API는 c + + 게임 엔진 성능 향상 및 디버깅 향상에 대 한 사용 하는 권장 되는 API입니다.
- Xbox Live 크리에이터 스 프로그램에 있는 경우 XSAPI 헤더를 포함 하기 전에 XBOX_LIVE_CREATORS_SDK를 정의 합니다. 이 제한 된 Xbox Live 크리에이터 스 프로그램 개발자가 사용할 수 있는 API 노출 영역 및 타이틀 크리에이터 스 프로그램에 대 한 작업에 로그인 메서드를 변경 합니다.  예를 들어 다음과 같은 가치를 제공해야 합니다.

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- C + + /cli WinRT도 지원 됩니다.  자세한 내용은 C + + /cli WinRT에서 찾을 수 있습니다 [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

사용 하 여 C + + /cli XSAPI 헤더를 포함 하기 전에 XSAPI c + + API를 사용 하 여 WinRT XSAPI_CPPWINRT를 정의 합니다.  예를 들어 다음과 같은 가치를 제공해야 합니다.

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

C +를 사용 하 여 XSAPI c + + API 호출의 예로 + WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>XSAPI C 기반 API

- 제목을 XSAPI를 호출 하는 경우 메모리 할당을 제어할 수 있습니다.
- 제목을 XSAPI를 호출 하는 경우를 처리 하는 스레드의 모든 권한을 얻을 수 있습니다.
- 새 HTTP 라이브러리를 libHttpClient, 게임 개발자를 위한 설계를 사용 합니다.

자세한 내용은 [Xbox Live C Api 소개](xsapi-flat-c.md)합니다.
