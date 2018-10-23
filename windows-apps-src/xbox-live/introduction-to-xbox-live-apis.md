---
title: Xbox Live API 소개
author: KevinAsgari
description: Xbox Live 서비스와 상호 작용 하는 데 사용할 수 있는 다양 한 API 모델에 알아봅니다.
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.author: kevinasg
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f295c1f1b432f90e12d3e628cd35a54412812ec
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5398658"
---
# <a name="introduction-to-xbox-live-apis"></a>Xbox Live API 소개

## <a name="use-xbox-live-services"></a>Xbox Live 서비스를 사용 합니다.

Xbox Live 서비스에서 정보를 얻을 수 있는 방법은 두 가지가 있습니다.

- Xbox Live 서비스 API (**XSAPI**)를 호출 하는 클라이언트 쪽 API를 사용 합니다.
- **Xbox Live REST 끝점** 을 직접 호출

Xbox Live 서비스 API (**XSAPI**)를 사용 하 여의 장점은 다음과 같습니다.

- 인증, 인코딩 및 HTTP 주고받기의 세부 정보를 담당 됩니다.
- 인수, 및 데이터에서 반환 된, 래퍼 API에서에서 처리 되는 원시 데이터 유형이 있습니다. 따라서 인코딩 및 디코딩 JSON을 수행할 필요가 없습니다.
- 웹 서비스를 직접 호출 하는; 래퍼 API를 캡슐화 하는 여러 비동기 단계로 이루어집니다. 이 제목 코드를를 읽고 쓰는 쉬워집니다.
- 게임 이벤트를 작성 하는 등의 일부 기능을 XSAPI에만 나와 있습니다.

**Xbox Live REST 끝점** 을 직접 사용 하는 장점은 다음과 같습니다.

- 웹 서비스에서 Xbox Live 끝점을 호출 하는 기능
- XSAPI에 포함 되어 있지 않은 끝점을 호출 하는 기능.  XSAPI 믿습니다 게임을 사용 하는 이므로 포럼을 통해 알 수 있도록 누락 된 것 이면 우리는 Api를 포함 합니다.
- REST 끝점을 통해 사용할 수 있는 일부 기능에는 해당 XSAPI 래퍼를 가질 수 없습니다.

게임과 앱 다음이 방법 중 하나를 사용 하 여 제한 되지 않습니다. XSAPI 래퍼를 사용 하 고 필요한 경우 직접에서 REST 끝점을 계속 호출할 수 있습니다.

## <a name="xbox-live-services-api-overview"></a>Xbox Live 서비스 API 개요 ##

Xbox Live 서비스 API (**XSAPI**)는 세 가지 클라이언트 쪽 다양 한 고객 시나리오를 지 원하는 Api 제공 합니다.

- [XSAPI WinRT API](#xsapi-winrt-based-api)
- [C + + 11 XSAPI 기반 API](#xsapi-c++11-based-api)
- [XSAPI C 기반 API](#xsapi-c-based-api) (**2018 년 6 월 부터는 새**)

Api를 비교 합니다.

### <a name="xsapi-winrt-based-api"></a>XSAPI WinRT API를 기반

- C +로 작성 된 응용 프로그램 지원 + CX, C# 및 JavaScript 합니다.
    - C + + /CX는 WinRT 프로그래밍을 사용 하 여 쉽게 조정할 Microsoft c + + 확장 ^ WinRT 포인터로 합니다.
- Xbox One XDK 플랫폼 및 유니버설 Windows 플랫폼 (UWP) x86, x64 및 ARM 아키텍처를 대상으로 하는 응용 프로그램을 지원 합니다.
- C + 등 모든 언어에서 예외를 통한 오류 처리 + CX 합니다.
- C + + WinRT도 지원 됩니다.  자세한 내용은 C + +에서 WinRT를 확인할 수 있습니다[https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

다음은 예로 XSAPI WinRT api를 사용 하 여 C + + WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

C + 혼합 하려는 경우 + CX 및 C + + 처럼 WinRT는 코드 마이그레이션 너무 수행할 수 있지만 약간 더 복잡 합니다.  
다음은 예로 XSAPI WinRT api를 사용 하 여 C + + WinRT 지정 C + + CX 사용자 ^ 개체입니다.

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>C + + 11 XSAPI 기반 API

- 사용 하 여 교차 플랫폼 ISO 표준 C + + 11
- C + +로 작성 된 지원 응용 프로그램
- Xbox One XDK 플랫폼 및 유니버설 Windows 플랫폼 (UWP) x86, x64 및 ARM 아키텍처를 대상으로 하는 응용 프로그램을 지원 합니다.
- 오류는 std::error_code를 통해 처리 됩니다.
- C + + 11 기반된 API는 더 나은 성능 및 향상 된 디버깅에 대 한 c + + 게임 엔진에 사용 하도록 권장된 API입니다.
- Xbox Live 크리에이터 스 프로그램에 사용 하는 경우 XSAPI 헤더를 포함 하기 전에 XBOX_LIVE_CREATORS_SDK를 정의 합니다. 해당 Xbox Live 크리에이터 스 프로그램 개발자가 사용할 수 있는 API 노출 영역을 제한 하 고 로그인 메서드가 타이틀 크리에이터 스 프로그램에 대 한 작동할 수를 변경 합니다.  예:

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- C + + WinRT도 지원 됩니다.  자세한 내용은 C + +에서 WinRT를 확인할 수 있습니다[https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

사용 하 여 C + + XSAPI 헤더를 포함 하기 전에 XSAPI c + + API를 사용 하 여 WinRT XSAPI_CPPWINRT를 정의 합니다.  예:

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

다음은 예로 XSAPI c + + api를 사용 하 여 C + + WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>XSAPI C 기반 API

- 타이틀을 XSAPI를 호출할 때 메모리 할당을 제어할 수 있습니다.
- 타이틀을 XSAPI를 호출할 때 처리 스레드를 완전히 제어할 수 있습니다.
- 새 HTTP 라이브러리 libHttpClient, 게임 개발자를 위한 디자인을 사용 합니다.

자세한 내용은 [Xbox Live C Api 소개](xsapi-flat-c.md)를 참조 하세요.
