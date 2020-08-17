---
description: '[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)에서 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)로 점진적으로 이식하는 방법을 설명하는 고급 항목입니다. PPL(병렬 패턴 라이브러리) 작업 및 코루틴이 동일한 프로젝트에 나란히 존재할 수 있는 방법을 보여줍니다.'
title: 비동시성 및 C++/WinRT와 C++/CX 간의 상호 운용성
ms.date: 08/06/2020
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, Interop, C++/CX, PPL, 작업, 코루틴
ms.localizationpriority: medium
ms.openlocfilehash: daa263836e9024a0aaa55b239b1a0db9437f1cd5
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88181079"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>비동시성 및 C++/WinRT와 C++/CX 간의 상호 운용성

> [!TIP]
> 이 항목을 처음부터 읽는 것이 좋지만, [C++/CX 비동기를 C++/WinRT로 이식하는 방법에 대한 개요](#overview-of-porting-ccx-async-to-cwinrt) 섹션의 interop 기술 요약으로 바로 이동할 수 있습니다.

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)에서 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)로 점진적으로 이식하는 방법을 설명하는 고급 항목입니다. 이 항목은 [C++/WinRT와 C++/CX 간의 상호 운용성](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx) 항목이 중단된 부분에서 다시 시작됩니다.

코드베이스의 크기나 복잡성으로 인해 프로젝트를 점진적으로 이식해야 하는 경우에는 C++/CX와 C++/WinRT 코드가 동일한 프로젝트에 한동안 나란히 존재하는 이식 프로세스가 필요합니다. 비동기 코드가 있는 경우에는 소스 코드를 점진적으로 이식할 때 프로젝트에 PPL(병렬 패턴 라이브러리) 작업 체인과 코루틴이 나란히 존재해야 할 수 있습니다. 이 항목에서는 비동기 C++/CX 코드와 비동기 C++/WinRT 코드 간의 상호 운용을 위한 기법을 중점적으로 다룹니다. 이러한 기법을 개별적으로 사용하거나 함께 사용할 수 있습니다.

이 항목을 읽기 전에 [C++/WinRT와 C++/CX 간의 상호 운용성](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)을 참조하는 것이 좋습니다. 점진적 이식을 위해 프로젝트를 준비하는 방법을 보여주는 항목입니다. 또한, C++/CX 개체를 C++/WinRT 개체로(또는 그 반대로) 변환하는 데 사용할 수 있는 두 가지 도우미 함수도 보여줍니다. 비 동시성에 대한 이 항목은 해당 정보를 기반으로 하며 해당 도우미 함수를 사용합니다.

> [!NOTE]
> 점진적인 이식에는 몇 가지 제한 사항이 있습니다. Windows 런타임 구성 요소 프로젝트가 있는 경우에는 점진적으로 이식하는 것이 불가능하며 프로젝트를 한 번에 이식해야 합니다. XAML 프로젝트의 경우, 언제든지, XAML 페이지 형식이 모두 C++/WinRT 또는 모두 C++/CX 중 하나여야 합니다.  자세한 내용은 [C++/CX에서 C++/WinRT로 이동](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)을 참조하세요.

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>전체 항목에서 비동기 코드 interop에 주력하는 이유

C++/CX에서 C++/WinRT로 이식하는 작업은 [PPL(병렬 패턴 라이브러리)](/cpp/parallel/concrt/parallel-patterns-library-ppl) 작업에서 코루틴으로 이동하는 작업을 제외하면 일반적으로 간단합니다. 모델이 서로 다릅니다. PPL 작업에서 코루틴으로 자연스러운 일대일 매핑은 없으며 모든 경우에 작동하는 코드를 기계적으로 이식하는 간단한 방법도 없습니다.

작업에서 코루틴으로 변환하면 상당히 간소화된다는 좋은 점이 있습니다. 또한, 개발 팀이 비동기 코드 이식의 어려움을 극복하고 나면 이식 작업의 나머지는 대체로 기계적인 부분이라고 보고하는 경우가 많습니다.

알고리즘은 원래 동기 API에 맞게 작성된 경우가 많습니다. 그런 다음, 작업 및 명시적 연속으로 변환되며, &mdash;결과가 기본 논리를 의도와 달리 난독 처리하는 경우가 많습니다. 예를 들어, 루프는 재귀가 됩니다. if-else 분기는 작업의 중첩 트리(체인)가 됩니다. 공유 변수는 **shared_ptr**이 됩니다. PPL 소스 코드의 부자연스러운 구조를 분석하기 위해서는 먼저 뒤로 물러나 원래 코드의 의도를 이해(즉, 원래 동기 버전을 파악)하는 것이 좋습니다. 그런 다음, `co_await`를 적절한 위치에 삽입합니다.

따라서 이식을 시작할 비동기 코드의 C#(C++/CX 보다는) 버전이 있으면 더 간편하고 깔끔하게 이식을 진행할 수 있습니다. C# 코드는 `await`를 사용합니다. 따라서 C# 코드는 동기 버전으로 시작한 다음, 적절한 위치에 `await`를 삽입하는 원칙을 이미 본질적으로 따릅니다.

## <a name="some-background-in-asynchronous-programming"></a>비동기 프로그래밍에 대한 몇 가지 배경

비동기 프로그래밍 개념 및 용어에 대한 확고한 기준을 갖기 위해, 일반적으로 Windows 런타임 비동기 프로그래밍과 관련된 장면을 설정하고 두 C++ 언어 프로젝션이 각각 다른 방식으로 그 위에 계층화되는 방식을 간략하게 설정하겠습니다.

프로젝트에는 비동기식으로 작동하는 메서드가 있습니다. 대부분의 경우에는 다른 작업을 수행하기 전에 해당 작업이 완료될 때까지 기다려야 합니다. 메서드는 비동기 메서드를 기다릴 수 있도록 비동기 작업 개체를 반환합니다. 하지만, 비동기식으로 수행되는 작업이 완료될 때까지 기다릴 필요가 없거나 기다리지 않으려는 경우가 있습니다. 이런 경우에는 메서드가 비동기 작업 개체를 반환할 필요가 없습니다. 기다리지 않으려는 비동기 메서드를 *fire-and-forget* 메서드라고 합니다.

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Windows 런타임 비동기 개체(**IAsyncXxx**)

**Windows::Foundation** Windows 런타임 네임스페이스에는 네 가지 유형의 비동기 작업 개체가 포함됩니다.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1),
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).

이 항목에서는 **IAsyncXxx**라는 편리한 줄임 속성을 사용하면서, 이러한 형식을 통칭하여 참조하거나 어떤 것인지 지정하지 않고 네 가지 형식 중 하나에 대해 설명합니다.

### <a name="ccx-async"></a>C++/CX 비동기

비동기 C++/CX 코드는 [PPL(병렬 패턴 라이브러리)](/cpp/parallel/concrt/parallel-patterns-library-ppl) 작업을 사용합니다. PPL 작업은 [**concurrency::task**](/cpp/parallel/concrt/reference/task-class) 클래스로 표현됩니다.

대개, 비동기 C++/CX 메서드는 [**concurrency::create_task**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) 및 [**concurrency::task::then**](/cpp/parallel/concrt/reference/task-class#then)과 람다 함수를 사용하여 PPL 작업을 함께 연결합니다. 각 람다 함수는 작업이 완료되면, 작업 *continuation*의 람다로 전달되는 값을 생성하는 작업을 반환합니다.

또는 **create_task**를 호출하여 작업을 만드는 대신, 비동기 C++/CX 메서드는 [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)를 호출하여 **IAsyncXxx**\^를 만들 수 있습니다.

따라서, 비동기 C++/CX 메서드의 반환 유형은 PPL 작업이나 **IAsyncXxx**\^일 수 있습니다.

두 경우 모두, 메서드 자체는 `return` 키워드를 사용하여 완료되면 호출자가 실제로 원하는 값(예: 파일, 바이트 배열 또는 부울)을 생성하는 비동기 개체를 반환합니다.

> [!NOTE]
> 비동기 C++/CX 메서드가 **IAsyncXxx**\^를 반환하면 **TResult**는 Windows 런타임 형식으로 제한됩니다. 예를 들어 부울 값은 Windows 런타임 형식이지만 C++/CX 프로젝션된 형식(예: **Platform::Array<byte>** ^)은 아닙니다.

### <a name="cwinrt-async"></a>C++/WinRT 비동기

C++/WinRT는C++ 코루틴을 프로그래밍 모델에 통합합니다. 코루틴 및 `co_await` 문은 결과를 협력하여 기다리는 자연스러운 방법을 제공합니다.

각 **IAsyncXxx** 형식은 **winrt::Windows::Foundation** C++/WinRT 네임스페이스의 해당 형식으로 프로젝션됩니다. 이것을 **winrt::IAsyncXxx**라고 하겠습니다. C++/WinRT 코루틴의 반환 형식은 **winrt::IAsyncXxx** 또는 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)입니다. 그리고, `return` 키워드를 사용하여 비동기 개체를 반환하는 대신, 코루틴은 `co_return` 키워드를 사용하여 호출자가 실제로 원하는 값(예: 파일, 바이트 배열 또는 부울)을 반환합니다.

메서드에 `co_await` 문이 하나 이상(또는 `co_return`이나 `co_yield`가 하나 이상) 포함되어 있으면, 메서드는 이러한 이유로 코루틴입니다.

자세한 내용은 [C++/WinRT로 동시성 및 비동기 작업](/windows/uwp/cpp-and-winrt-apis/concurrency)을 참조하세요.

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Direct3D 게임 샘플(**Simple3DGameDX**)

이 항목에는 비동기 코드를 점진적으로 이식하는 방법을 보여주는 몇 가지 연습이 포함되어 있습니다. 사례 연구가 가능하도록, [Direct3D 게임 샘플](/samples/microsoft/windows-universal-samples/simple3dgamedx/)(다른 이름: **Simple3DGameDX**)의 C++/CX 버전을 사용하겠습니다. 이 프로젝트에서 C++/CX 소스 코드를 가져와서 비동기 코드를 C++/WinRT로 점진적으로 이식하는 방법에 대한 몇 가지 예를 보여드리겠습니다.

- 위 링크에서 ZIP을 다운로드하고 압축을 풉니다.
- Visual Studio에서 C++/CX 프로젝트(`cpp` 폴더에 있음)를 엽니다.
- 그런 다음, C++/WinRT 지원을 프로젝트에 추가해야 합니다. 그렇게 하기 위해 수행하는 단계는 [C++/CX 프로젝트를 가져와서 C++/WinRT 지원 추가](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx#taking-a-ccx-project-and-adding-cwinrt-support)에 설명되어 있습니다. `interop_helpers.h` 헤더 파일을 프로젝트에 추가하는 단계는 특히 중요합니다. 이 항목에서는 해당 도우미 함수에 의존하기 때문입니다.
- 마지막으로 `#include <pplawait.h>`를 `pch.h`에 추가합니다. 그러면 PPL에 대한 코루틴 지원이 제공됩니다(지원에 대한 자세한 내용은 다음 섹션에 있음).

빌드하면 **바이트**가 모호하다는 오류가 발생합니다. 이것을 해결하는 방법은 다음과 같습니다.

- `BasicLoader.cpp`를 열고 `using namespace std;`를 주석으로 처리합니다.
- 동일한 소스 코드 파일에서 **shared_ptr**을 **std::shared_ptr**로 정규화해야 합니다. 이 작업은 해당 파일 내에서 찾기 및 바꾸기를 사용하여 수행할 수 있습니다.
- **vector**를 **std::vector**로, **string**은 **std::string**으로 정규화합니다.

이제 프로젝트가 다시 빌드되고, C++/WinRT가 지원되며, **from_cx** 및 **to_cx** interop 도우미 함수가 포함됩니다.

이제 **Simple3DGameDX** 프로젝트가 준비되어 이 항목의 코드 연습을 따라할 수 있습니다.

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>C++/CX 비동기를 C++/WinRT로 이식하는 방법에 대한 개요

간단히 말해, 이식하는 동안, PPL 작업 체인을 `co_await`에 대한 호출로 변경하게 됩니다. 메서드의 반환 값을 PPL 작업에서 C++/WinRT **IAsyncXxx** 개체나 **IAsyncXxx**\^로 변경합니다. 그리고 **IAsyncXxx**\^는 C++/WinRT **IAsyncXxx**로 변경합니다.

다시 말하지만, 코루틴은 `co_xxx`를 호출하는 메서드입니다. C++/WinRT 코루틴은 `co_return`을 사용하여 해당 값을 반환합니다. PPL에 대한 코루틴 지원 덕분에(`pplawait.h` 제공), `co_return`을 사용하여 코루틴에서 PPL 작업을 반환할 수도 있습니다. 또한 두 작업 모두와 **IAsyncXxx**를 `co_await`할 수 있습니다. 하지만 `co_return`을 사용하여 **IAsyncXxx**\^를 반환할 수 없습니다. 다음 표는 다양한 비동기 기법 간의 상호 운용성 지원을 설명합니다.

|메서드|이 항목을 `co_await`할 수 있습니까?|이 항목에서 `co_return`할 수 있습니까?|
|-|-|-|
|메서드가 task\<void\> 반환|예|예|
|메서드가 task\<T\> 반환|아니요|예|
|메서드가 IAsyncXxx\^ 반환|예|아니요. 단, `co_return`을 사용하는 작업을 **create_async**로 래핑합니다.|
|메서드가 winrt::IAsyncXxx 반환|예|예|

이 표를 사용하여 관심 있는 interop 기법으로 바로 이동할 수 있습니다.

|비동기 interop 기법|이 항목의 섹션|
|-|-|
|`co_await`를 사용하여 fire-and-forget 메서드 내에서 **task\<void\>** 메서드를 기다립니다.|[fire-and-forget 메서드 내에서 **task\<void\>** 대기](#await-taskvoid-within-a-fire-and-forget-method)|
|`co_await`를 사용하여 **task\<void\>** 메서드 내에서 **task\<void\>** 메서드를 기다립니다.|[**task\<void\>** 메서드 내에서 **task\<void\>** 대기](#await-taskvoid-within-a-taskvoid-method)|
|`co_await`를 사용하여 **task\<T\>** 메서드 내에서 **task\<void\>** 메서드를 기다립니다.|[**task\<T\>** 메서드 내에서 **task\<void\>** 대기](#await-taskvoid-within-a-taskt-method)|
|`co_await`를 사용하여 **IAsyncXxx**^ 메서드를 기다립니다.|[프로젝트의 나머지 부분은 변경하지 않고 **task** 메서드에서 **IAsyncXxx**^ 대기](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|**task\<void\>** 메서드 내에서 `co_return`을 사용합니다.|[**task\<void\>** 메서드 내에서 **task\<void\>** 대기](#await-taskvoid-within-a-taskvoid-method)|
|**task\<T\>** 메서드 내에서 `co_return` 사용|[프로젝트의 나머지 부분은 변경하지 않고 **task** 메서드에서 **IAsyncXxx**^ 대기](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|`co_return`을 사용하는 작업을 **create_async**로 래핑|[`co_return`을 사용하는 작업을 **create_async**로 래핑](#wrap-create_async-around-a-task-that-uses-co_return)|
|**concurrency::wait** 이식|[**concurrency::wait**를 `co_await winrt::resume_after`로 이식](#port-concurrencywait-to-co_await-winrtresume_after)|
|**task\<void\>** 대신 **winrt::IAsyncXxx**를 반환합니다.|[**task\<void\>** 반환 형식을 **winrt::IAsyncXxx**로 이식](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|

다음은 몇 가지 지원을 보여주는 짧은 코드 예제입니다.

```cppcx
#include <ppltasks.h>
#include <pplawait.h>
#include <winrt/Windows.Foundation.h>

concurrency::task<bool> TaskAsync()
{
    co_return true;
}

Windows::Foundation::IAsyncOperation<bool>^ IAsyncXxxCppCXAsync()
{
    // co_return true; // Error! Can't do that.
    return concurrency::create_async([=]() -> task<bool> {
        co_return true;
    });
}

winrt::Windows::Foundation::IAsyncOperation<bool> IAsyncXxxCppWinRTAsync()
{
    co_return true;
}

concurrency::task<bool> CppCXAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

이러한 강력한 interop 옵션을 사용하더라도 점진적인 이식은 시스템의 나머지 부분에 영향을 주지 않고 정확하게 수행할 수 있는 변경 사항을 선택하는 데 달려 있습니다. 임의의 명확하지 않은 부분을 고수하여 전체 프로젝트가 흐트러지지 않도록 하는 것이 좋습니다. 이렇게 하려면 특정 순서로 작업을 수행해야 합니다. 이런 종류의 비동기 관련 이식 변경을 수행하는 몇 가지 예를 살펴보겠습니다.

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>프로젝트의 나머지 부분은 변경하지 않고 **task\<void\>** 메서드 대기

**task\<void\>** 를 반환하는 메서드는 비동기식으로 작업을 수행하지만 궁극적으로 값은 생성하지 않습니다. 이런 메서드를 `co_await`할 수 있습니다.

비동기 코드를 점진적으로 이식하는 작업을 시작하기 좋은 부분은 해당 메서드를 호출하는 곳을 찾는 것입니다. 이러한 부분에서는 작업 및/또는 각 작업에서 해당 continuation으로 전달되는 값이 없는 작업 체인을 생성 및/또는 반환합니다. 이와 같은 부분에서 비동기 코드를 `co_await` 문과 바꿀 수 있습니다.

몇 가지 예를 살펴보겠습니다. **Simple3DGameDX** 프로젝트를 엽니다([Direct3D 게임 샘플](#the-direct3d-game-sample-simple3dgamedx) 참조).

> [!IMPORTANT]
> 다음 예제에서는 메서드 구현이 변경되는 것이 보이며, 변경 중인 메서드의 호출자를 변경할 필요가 없다는 점에 유의해야 합니다. 이러한 변경 내용은 지역화되며 프로젝트 전반에 계단식으로 적용되지 않습니다.

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>fire-and-forget 메서드 내에서 **task\<void\>** 대기

*fire-and-forget* 메서드 내에서 **task\<void\>** 를 기다리는 것이 가장 간단한 경우이므로 이 작업부터 시작합니다. 비동기식으로 작동하는 메서드이지만, 메서드의 호출자는 해당 작업이 완료되기를 기다리지 않습니다. 비동기식으로 완료된다는 사실에도 불구하고, 메서드를 호출하고 잊어버립니다.

**create_task** 및/또는 **task\<void\>** 메서드만 호출되는 작업 체인을 포함하는 `void` 메서드에 대한 프로젝트 종속성 그래프의 루트를 확인합니다.

**Simple3DGameDX**에서는 **GameMain::Update** 메서드 구현에서 일치 항목을 찾을 수 있습니다. 소스 코드 파일 `GameMain.cpp`에 있습니다.

#### <a name="gamemainupdate"></a>**GameMain::Update**

다음은 비동기식으로 완료되는 두 부분을 보여주는 메서드의 C++/CX 버전에서 추출한 부분입니다.

```cppcx
void GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    case UpdateEngineState::Dynamics:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    ...
}
```

**Simple3DGame::LoadLevelAsync** 메서드(PPL **task\<void\>** 를 반환함)에 대한 호출을 볼 수 있습니다. 그 다음은 동기 작업을 수행하는 *continuation*입니다. **LoadLevelAsync**는 비동기식이지만 값을 반환하지 않습니다. 따라서 작업에서 continuation으로 전달되는 값이 없습니다.

이 두 곳의 코드를 동일한 방식으로 변경할 수 있습니다. 코드는 아래 목록 뒤에 설명되어 있습니다. 여기서 class-member 코루틴에서 *this* 포인터에 액세스하는 안전한 방법에 대해 언급할 수도 있습니다. 하지만 이 내용은 이후 섹션으로 미루겠습니다. 지금은 이 코드가 작동합니다.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    case UpdateEngineState::Dynamics:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    ...
}
```

보이는 것처럼 **LoadLevelAsync**가 작업을 반환하기 때문에 작업을`co_await`할 수 있습니다. 그리고 명시적인 continuation이 필요하지 않습니다. &mdash;`co_await` 뒤에 오는 코드는 **LoadLevelAsync**가 완료되는 경우에만 실행됩니다.

`co_await`를 도입하면 메서드가 코루틴으로 바뀌므로 `void`를 반환할 수 없습니다. fire-and-forget 메서드이므로 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)을 반환합니다.

또한, `GameMain.h`의 선언에서 **GameMain::Update**의 반환 형식을 `void`에서 **winrt::fire_and_forget**으로 변경해야 합니다.

프로젝트 복사본에서 이 내용을 변경할 수 있으며 게임은 계속 빌드되고 동일하게 실행됩니다. 소스 코드는 여전히 기본적으로 C++/CX이지만 이제는 C++/WinRT와 동일한 패턴을 사용하기 때문에, 나머지 코드를 기계적으로 이식할 수 있는 수준에 조금 더 가까워졌습니다.

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

**GameMain::ResetGame**은 또 다른 fire-and-forget 메서드이며, **LoadLevelAsync**를 호출합니다. 따라서 동일한 변경 작업을 수행할 수 있습니다.

#### <a name="gamemainondevicerestored"></a>**GameMain::OnDeviceRestored**

**GameMain::OnDeviceRestored**의 경우 비동기 코드의 중첩 수준이 더 깊기 때문에(no-op 포함) 더 흥미로운 부분이 있습니다. 다음은 메서드의 비동기 부분에 대한 개요입니다(덜 흥미로운 동기 코드는 줄임표로 표시됨).

```cppcx
void GameMain::OnDeviceRestored()
{
    ...
    create_task([this]()
    {
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            ...
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
    }, task_continuation_context::use_current());
}
```

먼저 `GameMain.h` 및 `.cpp`에서 **GameMain::OnDeviceRestored**의 반환 형식을 `void`에서 **winrt::fire_and_forget**로 변경합니다. `DeviceResources.h`도 열어서 **IDeviceNotify::OnDeviceRestored**의 반환 형식을 동일하게 변경해야 합니다.

비동기 코드를 이식하려면 **create_task**를 모두 제거한 **다음**, 호출과 해당 중괄호를 제거하고 중첩되지 않은 명령문으로 메서드를 간소화합니다.

`return`(작업을 반환함)이 있으면 `co_await`로 변경합니다. 아무것도 반환하지 않는 `return` 하나만 남게 되므로 삭제합니다. 완료되면 no-op 작업이 사라지고 메서드의 비동기 부분 개요가 다음과 같이 표시됩니다. 마찬가지로, 덜 흥미로운 동기 코드는 줄임표로 표시됩니다.

```cppcx
winrt::fire_and_forget GameMain::OnDeviceRestored()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

여기에서 볼 수 있듯이 훨씬 간단하고 읽기 쉽습니다.

#### <a name="gamemaingamemain"></a>**GameMain::GameMain**

**GameMain::GameMain** 생성자는 작업을 비동기식으로 수행하며 작업이 완료될 때까지 기다리는 부분은 프로젝트에 없습니다. 이 목록은 비동기 부분을 간략하게 설명합니다.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    create_task([this]()
    {
        ...
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ....
    }, task_continuation_context::use_current());
}
```

하지만 생성자가 **winrt::fire_and_forget**를 반환할 수 없기 때문에 비동기 코드를 새 **GameMain::ConstructInBackground** fire-and-forget 메서드로 이동하고, 코드를 `co_await` 문으로 평면화하고, 생성자로부터 새 메서드를 호출합니다.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        ...
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

이것이 fire-and-forget 메서드의 전부이며, **GameMain**의 &mdash;모든 비동기 코드는 실제로&mdash; 코루틴으로 변경되었습니다. 원하는 경우, 다른 클래스에서 fire-and-forget 메서드를 찾아서 비슷한 변경을 적용할 수도 있습니다.

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>`co_await` 및 *this* 포인터에 대한 설명

**GameMain::Update**를 변경할 때 *this* 포인터에 대한 설명을 미뤄두었습니다. 이제 설명해보겠습니다.

이 내용은 지금까지 변경한 모든 메서드에 적용됩니다. fire-and-forget 뿐만 아니라 모든 코루틴에 적용됩니다. `co_await`를 메서드에 도입하면 일시 중단 지점이 도입됩니다. 그래서 클래스 멤버에 액세스할 때마다 일시 중단 지점 후에 사용하는 *this* 포인터에 주의해야 합니다.

간단히 말해 솔루션은 [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)을 호출하는 것입니다. 하지만, 이 문제와 솔루션에 대한 자세한 내용은 [class-member 코루틴에서 *this* 포인터에 안전하게 액세스](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)를 참조하세요.

[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 파생된 클래스에서만 **implements::get_strong**을 호출할 수 있습니다.

#### <a name="derive-gamemain-from-winrtimplements"></a>**winrt::implements**에서 **GameMain** 파생

첫 번째로 변경해야 하는 내용은 `GameMain.h`에 있습니다.

```cppcx
class GameMain :
    public DX::IDeviceNotify
```

**GameMain**은 **DX::IDeviceNotify**를 계속 구현하지만 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 파생되도록 변경합니다.

```cppwinrt
class GameMain : 
    public winrt::implements<GameMain, winrt::Windows::Foundation::IInspectable>,
    DX::IDeviceNotify
```

다음으로, `App.cpp`에서 이 메서드를 찾습니다.

```cppcx
void App::Load(Platform::String^)
{
    if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
```

하지만 이제 **GameMain**이 **winrt::implements**에서 파생되기 때문에 다른 방식으로 구성해야 합니다. 이런 경우 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 함수 템플릿을 사용합니다. 자세한 내용은 [구현 형식과 인터페이스의 인스턴스화 및 반환](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces)을 참조하세요.

```cppwinrt
    ...
    m_main = winrt::make_self<GameMain>(m_deviceResources);
    ...
```

이 변경 내용에서 루프를 닫으려면 *m_main* 형식도 변경해야 합니다. `App.h`에서 이 코드를 찾을 수 있습니다.

```cppcx
ref class App sealed :
    public Windows::ApplicationModel::Core::IFrameworkView
{
    ...
private:
    ...
    std::unique_ptr<GameMain> m_main;
};
```

*m_main* 선언을 이렇게 변경합니다.

```cppwinrt
    ...
    winrt::com_ptr<GameMain> m_main;
    ...
```

#### <a name="we-can-now-call-implementsget_strong"></a>이제 **implements::get_strong**을 호출할 수 있습니다.

**GameMain::Update** 및 `co_await`를 추가한 다른 메서드의 경우, 코루틴이 완료될 때까지 강력한 참조가 유지되도록 코루틴 시작 부분에서 **get_strong**을 호출하는 방법은 다음과 같습니다.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    ...
        co_await ...
    ...
}
```

### <a name="await-taskvoid-within-a-taskvoid-method"></a>**task\<void\>** 메서드 내에서 **task\<void\>** 대기

다음으로 간단한 경우는 스스로 **task\<void\>** 를 반환하는 메서드 내에서 **task\<void\>** 를 기다리는 것입니다. **task\<void\>** 를 `co_await`할 수 있고 `co_return`할 수 있기 때문입니다.

**Simple3DGame::LoadLevelAsync** 메서드 구현에서 매우 간단한 예제를 찾을 수 있습니다. 소스 코드 파일 `Simple3DGame.cpp`에 있습니다.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    return m_renderer->LoadLevelResourcesAsync();
}
```

동기 코드가 있고, **GameRenderer::LoadLevelResourcesAsync**에서 생성된 작업을 반환합니다.

이 작업을 반환하는 대신 `co_await`한 다음, 발생한 `void`를 `co_return`합니다.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

### <a name="await-taskvoid-within-a-taskt-method"></a>**task\<T\>** 메서드 내에서 **task\<void\>** 대기

**Simple3DGameDX**에서 찾을 수 있는 적절한 예는 없지만 패턴을 보여주기 위한 가상의 예를 고안할 수 있습니다.

아래 코드 예제는 **task\<void\>** 의 간단한 `co_await`를 보여줍니다. 그런 다음, **task\<T\>** 반환 형식을 충족하기 위해 Windows 런타임 메서드와 `co_return` 결과 파일을 co_awaiting하여 **StorageFile\^** 을 비동기식으로 반환합니다.

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

다음 단계는 더 많은 메서드를 이렇게 C++/WinRT로 이식하는 것입니다.

```cppcx
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder location,
    std::wstring filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location.GetFileAsync(filename);
}
```

이 예에서 *m_renderer* 데이터 멤버는 아직 C++/CX입니다.

## <a name="await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged"></a>프로젝트의 나머지 부분은 변경하지 않고 **task** 메서드에서 **IAsyncXxx**^ 대기

**task\<void\>** 를 `co_await`하는 방법을 살펴보았습니다. 프로젝트의 메서드이든 비동기 Windows API(예: [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync))이든 **IAsyncXxx**를 반환하는 메서드를 `co_await`할 수도 있습니다.

이 코드를 변경할 수 있는 위치에 대한 예는 **BasicReaderWriter::ReadDataAsync**(`BasicReaderWriter.cpp`에 구현된 부분 참조)에서 살펴보겠습니다. 원본 C++/CX 버전은 다음과 같습니다.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

Windows API를 `co_await`할 수 있을 뿐만 아니라 메서드가 비동기식으로 반환하는 값(이 경우 바이트 배열)을 `co_return`할 수도 있습니다. 첫 번째 단계에서는 이러한 변경을 수행하는 방법만 보여줍니다. 다음 섹션에서는 실제로 C++/CX 코드를 C++/WinRT로 이식합니다.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
)
{
    StorageFile^ file = co_await m_location->GetFileAsync(filename);
    IBuffer^ buffer = co_await FileIO::ReadBufferAsync(file);
    auto fileData = ref new Platform::Array<byte>(buffer->Length);
    DataReader::FromBuffer(buffer)->ReadBytes(fileData);
    co_return fileData;
}
```

반환 유형을 변경하지 않았으므로 변경 중인 메서드의 호출자를 변경할 필요가 없습니다.

### <a name="port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged"></a>프로젝트의 나머지 부분은 변경하지 않고 **ReadDataAsync**(대부분)를 C++/WinRT로 이식

한 단계 더 나아가 프로젝트의 다른 부분은 변경할 필요 없이 메서드 거의 전체를 C++/WinRT로 이식할 수 있습니다.

이 메서드가 프로젝트의 나머지 부분에 대해 갖는 유일한 종속성은 **BasicReaderWriter::m_location** 데이터 멤버이고, 이것은 C++/CX **StorageFolder**^입니다. 이 데이터 멤버를 변경되지 않은 상태로 유지하고, 매개 변수 형식 및 반환 형식을 변경하지 않고 두려면, 몇 번의 변환만 수행하면 됩니다. &mdash;한 번은 메서드 시작 부분에서, 나머지는 끝 부분에서 수행하면 됩니다. 이를 위해 **from_cx** 및 **to_cx** interop 도우미 함수를 사용할 수 있습니다.

**BasicReaderWriter::ReadDataAsync** 구현을 C++/WinRT로 대부분 이식한 후 모습은 다음과 같습니다. *점진적인 이식*의 좋은 예입니다. 그리고 이 메서드는 *C++/WinRT 기법을 사용하는 C++/CX 메서드*라는 생각에서 벗어나서 *C++/CX와 상호 운용되는 C++/WinRT 메서드*라고 볼 수 있는 단계에 있습니다.

```cppwinrt
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
#include <robuffer.h>
...
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

> [!NOTE]
> **ReadDataAsync**에서는 새 C++/CX 배열을 생성하고 반환했습니다. 물론 메서드의 반환 유형을 충족하기 위해 그렇게 했습니다(프로젝트의 나머지 부분을 변경할 필요가 없도록).
>
> 자체 프로젝트에서 이식 후 메서드의 끝에 도달하면 C++/WinRT 개체만 있는 다른 예를 접할 수 있습니다. 이것을 `co_return`하려면 **to_cx**를 호출하여 변환합니다. 가상의 예는 다음과 같습니다.

```cppcx
task<Windows::Storage::StorageFile^> RetrieveFileAsync()
{
    winrt::Windows::Storage::StorageFile storageFile = co_await ...
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>`co_return`을 사용하는 작업을 **create_async**로 래핑

**IAsyncXxx**\^를 직접 `co_return`할 수는 없지만 유사한 작업을 수행할 수 있습니다. `co_return`을 사용하는 작업이 있으면 [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)로 래핑할 수 있습니다.

가상의 예는 다음과 같습니다. **Simple3DGameDX**에서 가져올 수 있는 예는 없습니다.

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ Simple3DGame::ReturnBooleanAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await BooleanMemberFunctionAsync();
            co_return result;
        });
}
```

보시다시피 `co_await`할 수 있는 메서드에서 반환 값을 얻을 수 있습니다.

## <a name="port-concurrencywait-to-co_await-winrtresume_after"></a>**concurrency::wait**를 `co_await winrt::resume_after`로 이식

**Simple3DGameDX**가 [**concurrency::wait**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)를 사용하여 짧은 시간 동안 스레드를 일시 중지하는 부분이 몇 곳 있습니다. 예를 들면 다음과 같습니다.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int InitialLoadingDelay = 2000;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]()
    {
        wait(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

**concurrency::wait**의 C++/WinRT 버전은 **winrt::resume_after** 구조체입니다. PPL 작업 내에서 이 구조체를 `co_await`할 수 있습니다. 코드 예제는 다음과 같습니다.

```
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto InitialLoadingDelay = 2000ms;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]() -> task<void>
    {
        co_await winrt::resume_after(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

변경해야 하는 내용이 두 가지 더 있습니다. **GameConstants::InitialLoadingDelay** 형식을 **std::chrono::duration**으로 변경했고 컴파일러가 더 이상 추론이 불가능하여, 람다 함수의 반환 형식을 명시적으로 만들었습니다.

## <a name="port-a-taskvoid-return-type-to-winrtiasyncxxx"></a>**task\<void\>** 반환 형식을 **winrt::IAsyncXxx**로 이식

### <a name="simple3dgameloadlevelasync"></a>**Simple3DGame::LoadLevelAsync**

**Simple3DGameDX** 작업의 단계에서 **Simple3DGame::LoadLevelAsync**를 호출하는 프로젝트의 모든 위치에는 `co_await`를 사용하여 호출을 수행합니다.

즉, 메서드의 반환 형식을 **task\<void\>** 에서 **winrt::Windows::Foundation::IAsyncAction**으로 변경할 수 있습니다.

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

이제 메서드의 나머지 부분과 해당 종속성을 C++/WinRT로 이식하는 것은 상당히 기계적입니다.

### <a name="gamerendererloadlevelresourcesasync"></a>**GameRenderer::LoadLevelResourcesAsync**

**GameRenderer::LoadLevelResourcesAsync**의 원래 C++/CX 버전은 다음과 같습니다.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int LevelLoadingDelay = 500;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        wait(GameConstants::LevelLoadingDelay);
    });
}
```

**Simple3DGame::LoadLevelAsync**는 프로젝트에서 **GameRenderer::LoadLevelResourcesAsync**를 호출하는 유일한 부분이며, 이미 `co_await`를 사용하여 호출하고 있습니다.

따라서 **GameRenderer::LoadLevelResourcesAsync**가 더 이상 작업을 반환할 필요가 없습니다. &mdash;대신 **winrt::Windows::Foundation::IAsyncAction**을 반환할 수 있습니다. 그리고 구현 자체도 C++/WinRT로 완전히 이식할 정도로 간단합니다. 여기에는 [**concurrency::wait**를 `co_await winrt::resume_after`로 이식](#port-concurrencywait-to-co_await-winrtresume_after)하면서 변경한 것과 동일한 내용이 적용됩니다. 그리고 프로젝트의 나머지 부분에 대해 걱정할만한 중요한 종속성은 없습니다.

메서드를 C++/WinRT로 완전히 이식한 후 모습은 다음과 같습니다.

```cppwinrt
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto LevelLoadingDelay = 500ms;
    ...
}

// GameRenderer.cpp
winrt::Windows::Foundation::IAsyncAction GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;
    co_return co_await winrt::resume_after(GameConstants::LevelLoadingDelay);
}
```

### <a name="basicreaderwriterreaddataasync"></a>**BasicReaderWriter::ReadDataAsync**

**BasicReaderWriter::ReadDataAsync**를 C++/WinRT로 완전히 이식하여 연습을 마치겠습니다. 지난번 이 메서드를 볼 때는([프로젝트의 나머지 부분은 변경하지 않고 **ReadDataAsync**(대부분)를 C++/WinRT로 이식](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)에서) 대부분 C++/WinRT로 이식되었습니다. 하지만 **Platform::Array\<byte\>** ^ 작업을 계속 반환했습니다.

```cppwinrt
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

작업을 반환하는 대신, C++/WinRT [**IBuffer**](/uwp/api/windows.storage.streams.ibuffer) 개체를 비동기식으로 반환하도록 변경하겠습니다. 그러면 호출 사이트에서 코드를 변경해야 합니다.

C++/WinRT 구문 개체를 사용하도록 메서드의 구현, 메서드의 매개 변수, *m_location* 데이터 멤버를 이식한 후 모습은 다음과 같습니다.

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::Streams::IBuffer>
BasicReaderWriter::ReadDataAsync(
    _In_ winrt::hstring const& filename)
{
    StorageFile file{ co_await m_location.GetFileAsync(filename) };
    co_return co_await FileIO::ReadBufferAsync(file);
}

winrt::array_view<byte> BasicLoader::GetBufferView(
    winrt::Windows::Storage::Streams::IBuffer const& buffer)
{
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));
    return { bytes, bytes + buffer.Length() };
}
```

보이는 것처럼, **BasicReaderWriter::ReadDataAsync** 자체는 훨씬 간단합니다. 버퍼에서 바이트를 검색하는 동기 논리를 자체 메서드에 반영했기 때문입니다.

하지만 이제 호출 사이트를 C++/CX 형식의 이런 종류 구조에서,

```cppcx
task<void> BasicLoader::LoadTextureAsync(...)
{
    return m_basicReaderWriter->ReadDataAsync(filename).then(
        [=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(...);
    });
}
```

C++/WinRT 형식의 이런 패턴으로 이식해야 합니다.

```cppwinrt
winrt::Windows::Foundation::IAsyncAction BasicLoader::LoadTextureAsync(...)
{
    auto textureBuffer = co_await m_basicReaderWriter.ReadDataAsync(filename);
    auto textureData = GetBufferView(textureBuffer);
    CreateTexture(...);
}
```

## <a name="important-apis"></a>중요 API

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction),
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1),
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>관련 항목

* [C++/CX에서 C++/WinRT로 이동](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [C++/WinRT와 C++/CX 간의 상호 운용성](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)
* [C++/WinRT를 통한 동시성 및 비동기 작업](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [C++/WinRT의 강력하고 약한 참조](/windows/uwp/cpp-and-winrt-apis/weak-references)
* [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)
