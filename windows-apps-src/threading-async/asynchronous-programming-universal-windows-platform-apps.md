---
author: TylerMSFT
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: "비동기 프로그래밍"
description: "이 항목에서는 UWP(유니버설 Windows 플랫폼)의 비동기 프로그래밍 및 C#, Microsoft Visual Basic .NET, Visual C\\+\\+ 구성 요소 확장(C\\+\\+/CX) 및 JavaScript에서의 표현에 대해 설명합니다."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8d9a17beb9c637e0a780020ef1cbb7b0b0bddf38

---
# 비동기 프로그래밍

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 항목에서는 UWP(유니버설 Windows 플랫폼)의 비동기 프로그래밍 및 C#, Microsoft Visual Basic .NET, Visual C++ 구성 요소 확장(C++/CX) 및 JavaScript에서의 표현에 대해 설명합니다.

비동기 프로그래밍을 사용하면 앱이 오랜 시간이 소요되는 작업을 수행할 때 응답 가능 상태를 유지하는 데 도움이 됩니다. 예를 들어 인터넷에서 콘텐츠를 다운로드하는 앱은 콘텐츠가 도착할 때까지 몇 초간 기다려야 할 수 있습니다. UI 스레드에서 동기 메서드를 사용하여 콘텐츠를 검색할 경우 메서드가 반환할 때까지 앱이 차단됩니다. 앱은 사용자 조작에 반응하지 않으며, 사용자는 앱이 반응하지 않는 것처럼 보이므로 불만을 느낄 수 있습니다. 따라서 더 좋은 방법은 비동기 프로그래밍을 사용하여 작업이 완료되기를 기다리는 동안에도 앱이 계속 실행되고 UI에 응답하는 것입니다.

완료하는 데 오랜 시간이 걸릴 수 있는 메서드의 경우 비동기 프로그램이 표준이며 UWP에서도 예외가 아닙니다. JavaScript, C#, Visual Basic 및 C++/CX 각각은 비동기 메서드에 언어 지원을 제공합니다.

## UWP의 비동기 프로그래밍

많은 UWP 기능(예: [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/BR241124) API 및 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) API)은 비동기 API로 표시됩니다. 규칙에 따라, 비동기 API의 이름은 API가 호출된 후 실행의 일부가 발생할 수 있음을 나타내기 위해 "Async"로 끝납니다.

UWP(유니버설 Windows 플랫폼) 앱에 비동기 API를 사용할 경우 코드는 일관된 방식으로 비차단 호출을 만듭니다. 개발자 고유의 API 정의에 이러한 비동기 패턴을 구현하면 호출자는 예측 가능한 방식으로 코드를 이해하고 사용할 수 있습니다.

다음은 비동기 UWP API를 호출해야 하는 일반적인 작업의 예입니다.

-   메시지 대화 상자 표시

-   파일 시스템 작업, 파일 선택기 표시

-   인터넷에서 데이터 주고받기

-   소켓, 스트림, 연결 사용

-   약속, 연락처, 일정 작업

-   파일 형식 작업(예: PDF(Portable Document Format) 파일 열기나 이미지 또는 미디어 형식 디코딩)

-   장치 또는 서비스 조작

UWP 비동기 패턴을 사용하면 명시적으로 스레드를 관리하는 것을 완전히 피할 수 있습니다. 각 프로그래밍 언어는 고유한 방식으로 UWP용 비동기 패턴을 지원합니다.

| 프로그래밍 언어 | 비동기 표현           |
|----------------------|---------------------------------------|
| C#                  | **async** 키워드, **await** 연산자 |
| Visual Basic         | **Async** 키워드, **Await** 연산자 |
| C++/CX               | **task** 클래스, **.then** 메서드      |
| JavaScript           | promise 개체, **then** 함수     |

 

## C# 및 Visual Basic으로 작성된 UWP 앱의 비동기 패턴


C# 또는 Visual Basic으로 작성된 일반적인 코드 세그먼트는 동기식으로 실행됩니다. 즉, 실행된 줄은 다음 줄이 실행되기 전에 완료됩니다. 이전의 Microsoft .NET 프로그래밍 모델에서는 비동기 실행이 가능했지만 그 결과 코드는 코드가 수행하려는 작업에 초점을 두는 대신 비동기 코드 실행의 기술을 강조하는 경향이 있습니다. UWP, .NET Framework, C#, Visual Basic 컴파일러에 코드에서 비동기 기술을 추출하는 기능이 추가되었습니다. .NET 및 UWP의 경우 코드가 작업을 수행하는 방법 및 시기 대신 수행할 작업에 초점을 둔 비동기 코드를 작성할 수 있습니다. 비동기 코드는 동기 코드와 상당히 유사하게 보입니다. 자세한 내용은 [C# 또는Visual Basic에서 비동기식 API 호출](call-asynchronous-apis-in-csharp-or-visual-basic.md)을 참조하세요.

## C++로 작성된 UWP의 비동기 패턴


C++/CX에서 비동기 프로그래밍은 [**task class**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750113.aspx) 및 해당 [**then method**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750044.aspx)를 기반으로 합니다. 구문은 JavaScript promise의 구문과 유사합니다. **task class** 및 관련 유형 또한 스레드 컨텍스트에 대한 취소 및 관리 접근 권한 값을 제공합니다. 자세한 내용은 [C++의 비동기 프로그래밍](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)을 참조하세요.

[**create\_async 함수**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750102.aspx)는 JavaScript 또는 UWP를 지원하는 다른 언어에서 사용할 수 있는 비동기 API 작성에 대한 지원을 제공합니다. 자세한 내용은 [C++로 비동기 작업 만들기](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750082.aspx)룰 참조하세요.

## JavaScript로 작성된 UWP의 비동기 패턴

JavaScript에서 비동기 프로그래밍은 비동기 메서드가 promise 개체를 반환하게 하여 [Common JS Promises/A](http://wiki.commonjs.org/wiki/Promises/A) 제안 표준을 따릅니다. Promise는 UWP와 JavaScript용 Windows 라이브러리에 모두 사용됩니다.

Promise 개체는 차후에 수행될 값을 나타냅니다. UWP에서는 factory 함수로부터 promise 개체를 가져옵니다. 규칙에 따라 이 개체의 이름은 "Async"로 끝납니다.

대부분의 경우 비동기 함수 호출은 기본 함수를 호출하는 것만큼 간단합니다. 차이점은 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 또는 [**done**](https://msdn.microsoft.com/library/windows/apps/Hh701079) 메서드를 사용하여 결과나 오류에 대한 처리기를 할당하고 작업을 시작한다는 것입니다.

## 관련 항목

* [C# 또는 Visual Basic에서 비동기식 API 호출](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Async 및 Await를 사용한 비동기 프로그래밍(C# 및 Visual Basic)](http://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Reversi 샘플 기능 시나리오: 비동기 코드](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj712233.aspx#async)




<!--HONumber=Jun16_HO4-->


