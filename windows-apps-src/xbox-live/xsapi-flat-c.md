---
title: Xbox Live C Api
description: Xbox Live 서비스와 상호 작용 하는 데 사용할 수 있는 플랫 C API 모델에 알아봅니다.
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, c, xsapi
ms.localizationpriority: medium
ms.openlocfilehash: a1c73661b561d586f9e28957c7caa6a1b1f9cb03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597528"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Xbox Live C Api 소개

2018 년 6 월의에서 새로운 플랫 C API 계층 XSAPI에 추가 되었습니다. 이 새 API 계층에는 c + + 및 WinRT API 계층을 사용 하 여 발생 한 문제가 해결 되었습니다.

C API는 아직 모든 XSAPI 기능을 다루지 않습니다 하지만 추가 기능에서 작업 중인 합니다. 모든 3 API 계층, C, c + + 및 WinRT 계속 지원 하 고 기능 시간이 지남에 따라 추가 합니다.

> [!NOTE]
> 현재 C Api Xbox 개발자 키트 (XDK)를 사용 하는 제목와만 작동 합니다. 지금은 UWP 게임 지원 하지 않습니다.

## <a name="features-covered-by-the-c-apis"></a>C Api를 포함 하는 기능

C API는 현재 다음 기능 및 서비스를 지원합니다.

- 도전 과제
- 상태
- 프로필
- 소셜
- 주민 등록 관리자

## <a name="benefits-of-the-c-api-for-xsapi"></a>XSAPI에 대 한 C API의 이점

- 제목을 XSAPI를 호출 하는 경우 메모리 할당을 제어할 수 있습니다.
- 제목을 XSAPI를 호출 하는 경우를 처리 하는 스레드의 모든 권한을 얻을 수 있습니다.
- 새 HTTP 라이브러리를 libHttpClient, 게임 개발자를 위한 설계를 사용 합니다.

C + + XSAPI와 함께 C Api를 사용할 수 있지만 c + + Api를 사용 하 여 앞에 나열 된 이점을 얻지 않습니다.

### <a name="managing-memory-allocations"></a>메모리 할당 관리

새로운 C API를 사용 하 여 메모리를 할당 하려고 할 때마다 XSAPI를 호출 하는 함수 콜백 이제 지정할 수 있습니다. 콜백 함수를 지정 하지 않으면 경우 XSAPI 표준 메모리 할당 루틴을 사용 합니다.

메모리 루틴을 수동으로 지정 하려면 다음을 수행할 수 있습니다.

- 게임 시작 합니다.
  - 호출 `XblMemSetFunctions(memAllocFunc, memFreeFunc)` 할당 및 메모리를 해제에 대 한 할당 콜백을 지정 합니다.
  - 호출 `XblInitialize()` 라이브러리 인스턴스를 초기화 합니다.  
- 게임 실행 하는 동안:
  - 새 C api에서 호출 XSAPI는 할당 또는 사용 가능한 메모리가 지정된 된 메모리 처리 콜백을 호출할지 XSAPI 하면 합니다.  
- 게임 종료 되는 경우:
  - 호출 `XblCleanup()` XSAPI 라이브러리와 연결 된 모든 리소스를 확보 합니다.
  - 게임의 사용자 지정 메모리 관리자를 정리 합니다.

### <a name="managing-asynchronous-threads"></a>비동기 스레드 관리

C API를 개발자가 스레딩 모델을 완전히 제어할 수 있는 패턴을 호출 하는 비동기 새 스레드를 소개 합니다. 자세한 내용은 [XSAPI에 대 한 호출 패턴 C 계층 비동기 호출을 플랫](flatc-async-patterns.md)합니다.

## <a name="migrating-code-to-use-c-xsapi"></a>C XSAPI를 사용 하는 코드 마이그레이션

되므로 한 번에 하나의 기능을 마이그레이션하는 것이 좋습니다 XSAPI C Api는 프로젝트에 XSAPI c + + Api와 함께 사용할 수 있습니다.

기능 변경 되지 않아야 하므로 C Api 및 c + + Api는 다른 진입점이 포함 된 일반적인 코어, 대만 씬 래퍼입니다. 그러나 C Api만 활용 하는 사용자 지정 메모리 및 스레드 관리 기능.

> [!IMPORTANT]
> C Api를 사용 하 여 XSAPI WinRT Api를 혼합할 수 없습니다.

## <a name="where-to-view-the-c-apis"></a>C Api를 볼 수 있는 위치

- [C API 헤더 파일](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [새 C Api를 사용 하 여 샘플 코드](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
