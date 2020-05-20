---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: 비동기 프로그래밍
description: '이 항목에서는 UWP (유니버설 Windows 플랫폼)의 비동기 프로그래밍과 c #, Microsoft Visual Basic .NET, c + + 및 JavaScript의 표현에 대해 설명 합니다.'
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, 비동기
ms.localizationpriority: medium
ms.openlocfilehash: 77c3080728915ae9a288a57fe0200c43e7d119f3
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82729989"
---
# <a name="asynchronous-programming"></a>비동기 프로그래밍
이 항목에서는 UWP (유니버설 Windows 플랫폼)의 비동기 프로그래밍과 c #, Microsoft Visual Basic .NET, c + + 및 JavaScript의 표현에 대해 설명 합니다.

비동기 프로그래밍을 사용 하면 시간이 오래 걸릴 수 있는 작업을 수행 하는 경우 앱의 응답성을 유지할 수 있습니다. 예를 들어 인터넷에서 콘텐츠를 다운로드 하는 앱은 콘텐츠가 도착할 때까지 몇 초 정도 걸릴 수 있습니다. UI 스레드에서 동기 메서드를 사용 하 여 콘텐츠를 검색 하는 경우 메서드가 반환 될 때까지 앱이 차단 됩니다. 앱은 사용자 조작에 반응하지 않으며, 사용자는 앱이 반응하지 않는 것처럼 보이므로 불만을 느낄 수 있습니다. 응용 프로그램이 완료 될 때까지 기다리는 동안 응용 프로그램이 계속 실행 되 고 UI에 응답 하는 비동기 프로그래밍을 사용 하는 것이 훨씬 더 좋은 방법입니다.

완료 하는 데 시간이 오래 걸릴 수 있는 메서드의 경우, 비동기 프로그래밍은 UWP에서 예외가 아니라 일반적인 방법입니다. JavaScript, c #, Visual Basic 및 c + +는 각각 비동기 메서드에 대 한 언어 지원을 제공 합니다.

## <a name="asynchronous-programming-in-the-uwp"></a>UWP의 비동기 프로그래밍
[**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) Api 및 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) api와 같은 대부분의 UWP 기능은 비동기 api로 노출 됩니다. 규칙에 따라 비동기 Api의 이름은 제어가 호출자에 게 반환 된 후 실행 부분이 수행 될 수 있음을 나타내는 "Async"로 끝납니다.

UWP (유니버설 Windows 플랫폼) 앱에서 비동기 Api를 사용 하는 경우 코드에서 차단 되지 않는 호출을 일관 된 방식으로 수행 합니다. 사용자 고유의 API 정의에 이러한 비동기 패턴을 구현 하는 경우 호출자는 예측 가능한 방식으로 코드를 이해 하 고 사용할 수 있습니다.

비동기 Windows 런타임 Api를 호출 해야 하는 몇 가지 일반적인 작업은 다음과 같습니다.

-   메시지 대화 상자 표시

-   파일 시스템 작업, 파일 선택 표시

-   인터넷에서 데이터 송수신

-   소켓, 스트림, 연결 사용

-   약속, 연락처, 일정 작업

-   파일 형식 작업 (예: PDF (이식 가능한 문서 형식) 파일 열기 또는 이미지 또는 미디어 형식 디코딩)

-   장치 또는 서비스와 상호 작용

UWP 비동기 패턴을 사용 하면 명시적으로 스레드를 관리 하는 것을 방지할 수 있습니다. 각 프로그래밍 언어는 고유한 방식으로 UWP에 대 한 비동기 패턴을 지원 합니다.

| 프로그래밍 언어 | 비동기 표현           |
|----------------------|---------------------------------------|
| C#                   | **async** 키워드, **wait** 연산자 |
| Visual Basic         | **Async** 키워드, **wait** 연산자 |
| C++/WinRT            | 코 루틴 및 **co_await** 연산자  |
| C++/CX               | **작업** 클래스, **. then** 메서드      |
| JavaScript           | 약속 **개체, 함수**     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>C # 및 Visual Basic를 사용 하는 UWP의 비동기 패턴
C # 또는 Visual Basic로 작성 된 일반적인 코드 세그먼트는 동기적으로 실행 됩니다. 즉, 줄이 실행 될 때 다음 줄이 실행 되기 전에 완료 됩니다. 비동기 실행을 위한 이전 Microsoft .NET 프로그래밍 모델이 있었지만, 결과 코드는 코드에서 수행 하려는 작업에 집중 하는 대신 비동기 코드를 실행 하는 메커니즘을 강조 하는 경향이 있습니다. UWP, .NET framework 및 c # 및 Visual Basic 컴파일러에는 코드에서 비동기 메커니즘을 추상화 하는 기능이 추가 되었습니다. .NET 및 UWP의 경우,이 작업을 수행 하는 방법 대신 코드가 수행 하는 작업을 중심으로 하는 비동기 코드를 작성할 수 있습니다. 비동기 코드는 동기 코드와 매우 유사 하 게 보입니다. 자세한 내용은 [c #에서 비동기 Api 호출 또는 Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)를 참조 하세요.

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>C + +/WinRT를 사용 하는 UWP의 비동기 패턴
C + +/WinRT를 사용 하는 경우 코 루틴 및 **co_await** 연산자를 사용 합니다. 자세한 내용과 코드 예제는 [c + +/WinRT의 비동기 프로그래밍](../cpp-and-winrt-apis/concurrency.md)을 참조 하세요.

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>C + +/CX를 사용 하는 UWP의 비동기 패턴
C + +/CX에서 비동기 프로그래밍은 [**작업 클래스**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class)와 그 [**뒤의 메서드**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class?view=vs-2017)를 기반으로 합니다. 구문은 JavaScript 약속의 구문과 유사 합니다. **작업 클래스** 및 관련 형식은 스레드 컨텍스트를 취소 하 고 관리 하는 기능도 제공 합니다. 자세한 내용은 [c + +/cx의 비동기 프로그래밍](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)을 참조 하세요.

[**Create \_ async 함수**](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) 는 JavaScript 또는 UWP를 지 원하는 다른 언어에서 사용할 수 있는 비동기 api를 생성 하는 기능을 제공 합니다. 자세한 내용은 [c + +/cx로 비동기 작업 만들기](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)를 참조 하세요.

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>JavaScript를 사용 하는 UWP의 비동기 패턴
JavaScript에서 비동기 프로그래밍은 비동기 메서드가 약속 개체를 반환 하도록 하 여 [공용 JS 약속/](https://wiki.commonjs.org/wiki/Promises/A) 제안 된 표준을 따릅니다. 약속은 JavaScript 용 UWP 및 Windows 라이브러리 모두에서 사용 됩니다.

Promise 개체는 차후에 수행될 값을 나타냅니다. UWP에서 규칙에 따라 이름이 "Async"로 끝나는 팩터리 함수에서 약속 개체를 가져옵니다.

대부분의 경우 비동기 함수를 호출 하는 것은 대체로 기존 함수를 호출 하는 것 만큼 간단 합니다. 차이점은 [**then**](https://docs.microsoft.com/previous-versions/windows/apps/br229728(v=win.10)) 또는 [**done**](https://docs.microsoft.com/previous-versions/windows/apps/hh701079(v=win.10)) 메서드를 사용 하 여 결과 또는 오류에 대 한 처리기를 할당 하 고 작업을 시작 하는 것입니다.

## <a name="related-topics"></a>관련 항목
* [C# 또는Visual Basic에서 비동기식 API 호출](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Async 및 Await를 사용한 비동기 프로그래밍(C# 및 Visual Basic)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2012/hh191443(v=vs.110))
* [Reversi 샘플 기능 시나리오: 비동기 코드](https://docs.microsoft.com/previous-versions/windows/apps/jj712233(v=win.10))
