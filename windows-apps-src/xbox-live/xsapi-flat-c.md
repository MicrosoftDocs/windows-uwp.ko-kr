---
title: Xbox Live C Api
author: KevinAsgari
description: Xbox Live 서비스와 상호 작용 하는 데 사용할 수 있는 플랫 C API 모델에 알아봅니다.
ms.author: kevinasg
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, c, xsapi
ms.localizationpriority: medium
ms.openlocfilehash: ac47d3877c44cfa9891753c49be8a5749fba9185
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4753589"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Xbox Live C Api 소개

2018 년 6 월에에서 새 플랫 C API 계층 XSAPI에 추가 되었습니다. 이 새로운 API 계층에는 c + + 및 WinRT API 계층을 사용 하 여 발생 하는 몇 가지 문제가 해결 되었습니다.

아직 C API 모든 XSAPI 기능을 다루지 않습니다 하지만 추가 기능에서 작업 중인 합니다. 모든 3 API 계층, C, c + + 및 WinRT는 계속 지원 하 고 추가 기능 시간이 지남에 따라 추가 해야 합니다.

> [!NOTE]
> 현재 C Api (XDK (Xbox 개발자 키트)를 사용 하는 제목을 가진 에서만 작동 합니다. 지금은 UWP 게임을 지원 하지 않습니다.

## <a name="features-covered-by-the-c-apis"></a>C Api를 통해 다루는 기능

C API는 현재 다음과 같은 기능 및 서비스를 지원합니다.

- 도전 과제
- 현재 상태
- 프로필
- 소셜
- 소셜 관리자

## <a name="benefits-of-the-c-api-for-xsapi"></a>XSAPI C API의 이점

- 타이틀을 XSAPI를 호출할 때 메모리 할당을 제어할 수 있습니다.
- 타이틀을 XSAPI를 호출할 때 처리 스레드를 완전히 제어할 수 있습니다.
- 새 HTTP 라이브러리 libHttpClient, 게임 개발자를 위한 디자인을 사용 합니다.

C + + XSAPI 함께 C Api를 사용할 수 있지만 c + + Api를 사용 하 여 이전에 나열 된 혜택을 얻지 않습니다.

### <a name="managing-memory-allocations"></a>메모리 할당을 관리

새 C API와 XSAPI 메모리를 할당 하려고 할 때마다 호출 되는 함수 콜백을 지정할 수 있습니다. 콜백 함수를 지정 하지 않으면 XSAPI 표준 메모리 할당 루틴을 사용 합니다.

메모리 루틴을 수동으로 지정 하려면 다음을 수행할 수 있습니다.

- 게임의 시작:
  - 호출 `XblMemSetFunctions(memAllocFunc, memFreeFunc)` 할당을 메모리를 해제 하기 위한 할당 콜백을 지정할 수 있습니다.
  - 호출 `XblInitialize()` 라이브러리 인스턴스를 초기화 합니다.  
- 게임 실행 중:
  - 할당의 모든 새 C Api에서에서 호출 XSAPI는 또는 무료 메모리 XSAPI 지정된 된 메모리 처리 콜백을 호출 하면 됩니다.  
- 게임 종료 하는 경우:
  - 호출 `XblCleanup()` XSAPI 라이브러리와 관련 된 모든 리소스를 확보 합니다.
  - 게임의 사용자 지정 메모리 관리자를 정리 합니다.

### <a name="managing-asynchronous-threads"></a>비동기 스레드를 관리

C API는 개발자가 스레딩 모델을 완전히 제어할 수 있는 패턴을 호출 하는 비동기 새 스레드를 소개 합니다. 자세한 내용은 [XSAPI에 대 한 호출 패턴 평면 C 계층 비동기 호출을](flatc-async-patterns.md)참조 하세요.

## <a name="migrating-code-to-use-c-xsapi"></a>C XSAPI를 사용 하 여 코드 마이그레이션

하므로 한 번에 하나의 기능으로 마이그레이션하는 것이 좋습니다 XSAPI C Api는 프로젝트의 XSAPI c + + Api와 함께 사용할 수 있습니다.

C Api 및 c + + Api 되므로 실제로 씬 래퍼는 공통 핵심 진입점 마찬가지로 기능 변경 되지 않도록 합니다. 그러나 C Api만 이용 하는 사용자 지정 메모리 및 스레드 관리 기능 걸릴 수 있습니다.

> [!IMPORTANT]
> C Api를 사용 하 여 XSAPI WinRT Api를 혼합할 수 없습니다.

## <a name="where-to-view-the-c-apis"></a>C Api를 볼 수 있는 위치

- [C API 헤더 파일](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [새 C Api를 사용 하는 샘플 코드](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
