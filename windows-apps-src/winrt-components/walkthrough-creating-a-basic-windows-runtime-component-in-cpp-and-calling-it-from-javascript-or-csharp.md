---
author: msatranjr
title: C++/CX로 기본적인 Windows 런타임 구성 요소를 만들고 JavaScript 또는 C#에서 호출
description: 이 연습에서는 JavaScript, C# 또는 Visual Basic에서 호출할 수 있는 기본 Windows 런타임 구성 요소 DLL을 만드는 방법을 보여 줍니다.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.author: misatran
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8d56e220a65d0de261ab0cc64b8ac417e1f480eb
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7570761"
---
# <a name="walkthrough-creating-a-windows-runtime-component-in-ccx-and-calling-it-from-javascript-or-c"></a>연습: C++/CX로 Windows 런타임 구성 요소를 만들고 JavaScript 또는 C#에서 호출
> [!NOTE]
> 이 항목은 C++/CX 응용 프로그램 유지에 도움을 주기 위해 작성되었습니다. 하지만 새로운 응용 프로그램에 대해 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 것이 좋습니다. C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일 기반 라이브러리로 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다. C +를 사용 하 여 Windows 런타임 구성 요소를 만드는 방법은 + /winrt에 참조 [C +의 이벤트 작성 + WinRT](../cpp-and-winrt-apis/author-events.md)합니다.

이 연습에서는 JavaScript, C# 또는 Visual Basic에서 호출할 수 있는 기본 Windows 런타임 구성 요소 DLL을 만드는 방법을 보여 줍니다. 이 연습을 시작하기 전에 쉽게 ref 클래스를 사용할 수 있게 해주는 ABI(추상 이진 인터페이스), ref 클래스, Visual C++ 구성 요소 확장 등의 개념을 이해해야 합니다. 자세한 내용은 [C++로 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-cpp.md) 및 [Visual C++ 언어 참조(C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)를 참조하세요.

## <a name="creating-the-c-component-dll"></a>C++ 구성 요소 DLL 만들기
이 예제에서는 구성 요소 프로젝트를 먼저 만들지만 JavaScript 프로젝트를 먼저 만들 수도 있습니다. 순서는 중요하지 않습니다.

구성 요소의 기본 클래스에는 속성 및 메서드 정의의 예와 이벤트 선언이 포함되어 있습니다. 이러한 항목은 작업 방식을 보여 주기 위해서만 제공됩니다. 필수는 아니며, 이 예제에서는 생성된 코드를 고유한 코드로 바꿀 것입니다.

## **<a name="to-create-the-c-component-project"></a>C++ 구성 요소 프로젝트를 만들려면**
Visual Studio 메뉴 모음에서 **파일, 새로 만들기, 프로젝트**를 선택합니다.

**새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C++** 를 확장하고 유니버설 Windows 앱에 대한 노드를 선택합니다.

가운데 창에서 **Windows 런타임 구성 요소**를 선택하고 프로젝트 이름을 WinRT\_CPP로 지정합니다.

**확인** 단추를 선택합니다.

## **<a name="to-add-an-activatable-class-to-the-component"></a>구성 요소에 활성화 가능 클래스를 추가하려면**
활성화 가능 클래스는 **new** 식(Visual Basic의 **New** 또는 C++의 **ref new**)을 사용하여 클라이언트 코드에서 만들 수 있는 클래스입니다. 구성 요소에서 **public ref class sealed**로 선언합니다. 실제로 Class1.h 및 .cpp 파일에는 이미 ref 클래스가 있습니다. 이름을 변경할 수 있지만 이 예제에서는 기본 이름인 Class1을 사용합니다. 필요한 경우 구성 요소에서 추가 ref 클래스 또는 일반 클래스를 정의할 수 있습니다. ref 클래스에 대한 자세한 내용은 [형식 시스템(C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx)을 참조하세요.

Class1.h에 다음 \#include 지시문을 추가합니다.

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

collection.h는 Windows 런타임에서 정의된 언어 중립 인터페이스를 구현하는 Platform::Collections::Vector 클래스 및 Platform::Collections::Map 클래스 등의 C++ 구체적 클래스에 대한 헤더 파일입니다. amp 헤더는 GPU에서 계산을 실행하는 데 사용됩니다. 동등한 Windows 런타임 항목은 없으며 private이므로 문제가 되지 않습니다. 일반적으로 성능상의 이유로 구성 요소 내에서는 내부적으로 ISO C++ 코드 및 표준 라이브러리를 사용해야 합니다. Windows 런타임 형식으로 표시해야 하는 것은 Windows 런타임 인터페이스뿐입니다.

## <a name="to-add-a-delegate-at-namespace-scope"></a>네임스페이스 범위에서 대리자를 추가하려면
대리자는 메서드에 대한 매개 변수 및 반환 형식을 정의하는 구문입니다. 이벤트는 특정 대리자 형식의 인스턴스이며 이벤트를 구독하는 모든 이벤트 처리기 메서드에 대리자에 지정된 서명이 있어야 합니다. 다음 코드는 int를 받아서 void를 반환하는 대리자 형식을 정의합니다. 그런 후에 코드는 이 형식의 public 이벤트를 선언합니다. 이 경우 클라이언트 코드를 통해 이벤트 발생 시 호출되는 메서드를 제공할 수 있습니다.

Class1.h의 네임스페이스 범위에서 Class1 선언 바로 앞에 다음 대리자 선언을 추가합니다.

```cpp
public delegate void PrimeFoundHandler(int result);
```

Visual Studio에 붙여넣을 때 코드가 올바르게 정렬되지 않는 경우 Ctrl+K+D를 눌러 전체 파일에 대한 들여쓰기를 수정합니다.

## <a name="to-add-the-public-members"></a>public 멤버를 추가하려면
클래스는 public 메서드 3개와 public 이벤트 1개를 노출합니다. 첫 번째 메서드는 항상 매우 빠르게 실행되기 때문에 동기적입니다. 다른 두 메서드는 시간이 걸릴 수 있으므로 UI 스레드를 차단하지는 않도록 비동기적입니다. 이러한 메서드는 IAsyncOperationWithProgress 및 IAsyncActionWithProgress를 반환합니다. 전자는 결과를 반환하는 비동기 메서드를 정의하고 후자는 void를 반환하는 비동기 메서드를 정의합니다. 이러한 인터페이스를 통해 클라이언트 코드에서 작업 진행률에 대한 업데이트를 받을 수도 있습니다.

```cpp
public:

        // Synchronous method.
        Windows::Foundation::Collections::IVector<double>^  ComputeResult(double input);

        // Asynchronous methods
        Windows::Foundation::IAsyncOperationWithProgress<Windows::Foundation::Collections::IVector<int>^, double>^
            GetPrimesOrdered(int first, int last);
        Windows::Foundation::IAsyncActionWithProgress<double>^ GetPrimesUnordered(int first, int last);

        // Event whose type is a delegate "class"
        event PrimeFoundHandler^ primeFoundEvent;

```
## <a name="to-add-the-private-members"></a>private 멤버를 추가하려면
클래스는 private 멤버 3개를 포함합니다. 숫자 계산에 사용되는 도우미 메서드 2개와 작업자 스레드의 이벤트 호출을 다시 UI 스레드로 마샬링하는 데 사용되는 CoreDispatcher 개체 1개입니다.

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

## <a name="to-add-the-header-and-namespace-directives"></a>헤더 및 네임스페이스 지시문을 추가하려면
Class1.cpp에서 다음 #include 지시문을 추가합니다.

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

이제 다음 using 문을 추가하여 필요한 네임스페이스를 끌어옵니다.

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>ComputeResult 구현을 추가하려면
Class1.cpp에서 다음 메서드 구현을 추가합니다. 이 메서드는 호출 스레드에서 동기적으로 실행되지만 C++ AMP를 사용하여 GPU에서 계산을 병렬화하기 때문에 매우 빠릅니다. 자세한 내용은 C++ AMP 개요를 참조하세요. 결과는 반환 시 Windows::Foundation::Collections::IVector<T>로 암시적으로 변환되는 Platform::Collections::Vector<T> 구체적 형식에 추가됩니다.

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/en-us/library/hh305254.aspx
    parallel_for_each(
        logs.extent,
        [=] (index<1> idx) restrict(amp)
    {
        logs[idx] = concurrency::fast_math::log10(logs[idx]);
    }
    );

    // Return a Windows Runtime-compatible type across the ABI
    auto res = ref new Vector<double>();
    int len = safe_cast<int>(logs.extent.size());
    for(int i = 0; i < len; i++)
    {      
        res->Append(logs[i]);
    }

    // res is implicitly cast to IVector<double>
    return res;
}
```
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>GetPrimesOrdered 및 해당 도우미 메서드 구현을 추가하려면
Class1.cpp에서 GetPrimesOrdered 및 is_prime 도우미 메서드 구현을 추가합니다. GetPrimesOrdered는 concurrent_vector 클래스 및 parallel_for 함수 루프를 사용하여 작업을 나누고 프로그램이 실행되는 컴퓨터의 최대 리소스를 사용하여 결과를 생성합니다. 결과가 계산, 저장 및 정렬된 후 Platform::Collections::Vector<T>에 추가되며 클라이언트 코드에 Windows::Foundation::Collections::IVector<T>로 반환됩니다.

클라이언트가 진행률 표시줄이나 다른 UI를 연결하여 사용자에게 작업이 얼마나 더 오래 걸릴지 보여 줄 수 있는 진행률 보고자에 대한 코드를 확인합니다. 진행률 보고에는 비용이 있습니다. 구성 요소 쪽에서 이벤트가 발생하고 UI 스레드에서 처리해야 하며, 각 반복에서 진행률 값을 저장해야 합니다. 비용을 최소화하는 한 가지 방법은 진행률 이벤트가 발생하는 빈도를 제한하는 것입니다. 그래도 비용이 부담되는 경우 또는 작업 길이를 예측할 수 없는 경우 작업이 진행 중임을 보여 주지만 완료 시까지 남은 시간을 표시하지 않는 진행률 링을 사용하는 것이 좋습니다.

```cpp
// Determines whether the input value is prime.
bool Class1::is_prime(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
    {
        if ((n % i) == 0)
            return false;
    }
    return true;
}

// This method computes all primes, orders them, then returns the ordered results.
IAsyncOperationWithProgress<IVector<int>^, double>^ Class1::GetPrimesOrdered(int first, int last)
{
    return create_async([this, first, last]
    (progress_reporter<double> reporter) -> IVector<int>^ {
        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }
        // Perform the computation in parallel.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        parallel_for(first, last + 1, [this, &primes, &operation,
            range, &lastPercent, reporter](int n) {

                // Increment and store the number of times the parallel
                // loop has been called on all threads combined. There
                // is a performance cost to maintaining a count, and
                // passing the delegate back to the UI thread, but it's
                // necessary if we want to display a determinate progress
                // bar that goes from 0 to 100%. We can avoid the cost by
                // setting the ProgressBar IsDeterminate property to false
                // or by using a ProgressRing.
                if(InterlockedIncrement(&operation) % 100 == 0)
                {
                    reporter.report(100.0 * operation / range);
                }

                // If the value is prime, add it to the local vector.
                if (is_prime(n)) {
                    primes.push_back(n);
                }
        });

        // Sort the results.
        std::sort(begin(primes), end(primes), std::less<int>());        
        reporter.report(100.0);

        // Copy the results to a Vector object, which is
        // implicitly converted to the IVector return type. IVector
        // makes collections of data available to other
        // Windows Runtime components.
        return ref new Vector<int>(primes.begin(), primes.end());
    });
}
```

## <a name="to-add-the-implementation-for-getprimesunordered"></a>GetPrimesUnordered 구현을 추가하려면
C++ 구성 요소를 만드는 마지막 단계는 Class1.cpp에 GetPrimesUnordered 구현을 추가하는 것입니다. 이 메서드는 모든 결과를 찾을 때까지 기다리지 않고 발견되는 대로 각 결과를 반환합니다. 각 결과가 이벤트 처리기에 반환되고 실시간으로 UI에 표시됩니다. 다시 진행률 보고자가 사용되는 것을 확인합니다. 이 메서드는 is_prime 도우미 메서드도 사용합니다.

```cpp
// This method returns no value. Instead, it fires an event each time a
// prime is found, and passes the prime through the event.
// It also passes progress info.
IAsyncActionWithProgress<double>^ Class1::GetPrimesUnordered(int first, int last)
{

    auto window = Windows::UI::Core::CoreWindow::GetForCurrentThread();
    m_dispatcher = window->Dispatcher;


    return create_async([this, first, last](progress_reporter<double> reporter) {

        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }

        // In this particular example, we don't actually use this to store
        // results since we pass results one at a time directly back to
        // UI as they are found. However, we have to provide this variable
        // as a parameter to parallel_for.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        // Perform the computation in parallel.
        parallel_for(first, last + 1,
            [this, &primes, &operation, range, &lastPercent, reporter](int n)
        {
            // Store the number of times the parallel loop has been called  
            // on all threads combined. See comment in previous method.
            if(InterlockedIncrement(&operation) % 100 == 0)
            {
                reporter.report(100.0 * operation / range);
            }

            // If the value is prime, pass it immediately to the UI thread.
            if (is_prime(n))
            {                
                // Since this code is probably running on a worker
                // thread, and we are passing the data back to the
                // UI thread, we have to use a CoreDispatcher object.
                m_dispatcher->RunAsync( CoreDispatcherPriority::Normal,
                    ref new DispatchedHandler([this, n, operation, range]()
                {
                    this->primeFoundEvent(n);

                }, Platform::CallbackContext::Any));

            }
        });
        reporter.report(100.0);
    });
}
```

## <a name="creating-a-javascript-client-app"></a>JavaScript 클라이언트 앱 만들기
C# 클라이언트를 만들려는 경우 이 섹션을 건너뛸 수 있습니다.

## <a name="to-create-a-javascript-project"></a>JavaScript 프로젝트를 만들려면
솔루션 탐색기에서 솔루션 노드의 바로 가기 메뉴를 열고 **추가, 새 프로젝트**를 선택합니다.

JavaScript를 확장하고(**기타 언어** 아래에 중첩되어 있을 수 있음) **빈 앱(유니버설 Windows)** 을 선택합니다.

**확인** 단추를 선택하여 기본 이름인 App1을 적용합니다.

App1 프로젝트 노드의 바로 가기 메뉴를 열고 **시작 프로젝트로 설정**을 선택합니다.

WinRT_CPP에 대한 프로젝트 참조를 추가합니다.

참조 노드의 바로 가기 메뉴를 열고 **참조 추가**를 선택합니다.

참조 관리자 대화 상자의 왼쪽 창에서 **프로젝트**를 선택한 다음 **솔루션**을 선택합니다.

가운데 창에서 WinRT_CPP를 선택한 다음 **확인** 단추를 선택합니다.

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>JavaScript 이벤트 처리기를 호출하는 HTML을 추가하려면
default.html 페이지의 <body> 노드에 이 HTML을 붙여 넣습니다.

```HTML
<div id="LogButtonDiv">
     <button id="logButton">Logarithms using AMP</button>
 </div>
 <div id="LogResultDiv">
     <p id="logResult"></p>
 </div>
 <div id="OrderedPrimeButtonDiv">
     <button id="orderedPrimeButton">Primes using parallel_for with sort</button>
 </div>
 <div id="OrderedPrimeProgress">
     <progress id="OrderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="OrderedPrimeResultDiv">
     <p id="orderedPrimes">
         Primes found (ordered):
     </p>
 </div>
 <div id="UnorderedPrimeButtonDiv">
     <button id="ButtonUnordered">Primes returned as they are produced.</button>
 </div>
 <div id="UnorderedPrimeDiv">
     <progress id="UnorderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="UnorderedPrime">
     <p id="unorderedPrimes">
         Primes found (unordered):
     </p>
 </div>
 <div id="ClearDiv">
     <button id="Button_Clear">Clear</button>
 </div>
```

## <a name="to-add-styles"></a>스타일을 추가하려면
Default.css에서 본문 스타일을 제거하고 다음 스타일을 추가합니다.

```css
#LogButtonDiv {
border: orange solid 1px;
-ms-grid-row: 1; /* default is 1 */
-ms-grid-column: 1; /* default is 1 */
}
#LogResultDiv {
background: black;
border: red solid 1px;
-ms-grid-row: 1;
-ms-grid-column: 2;
}
#UnorderedPrimeButtonDiv, #OrderedPrimeButtonDiv {
border: orange solid 1px;
-ms-grid-row: 2;   
-ms-grid-column:1;
}
#UnorderedPrimeProgress, #OrderedPrimeProgress {
border: red solid 1px;
-ms-grid-column-span: 2;
height: 40px;
}
#UnorderedPrimeResult, #OrderedPrimeResult {
border: red solid 1px;
font-size:smaller;
-ms-grid-row: 2;
-ms-grid-column: 3;
-ms-overflow-style:scrollbar;
}
```

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>DLL 구성 요소를 호출하는 JavaScript 이벤트 처리기를 추가하려면
default.js 파일의 끝에 다음 함수를 추가합니다. 이러한 함수는 기본 페이지의 단추를 선택할 때 호출됩니다. JavaScript에서 C++ 클래스를 활성화한 다음 해당 메서드를 호출하고 반환 값을 사용하여 HTML 레이블을 채우는 방법을 확인합니다.

```JavaScript
var nativeObject = new WinRT_CPP.Class1();

function LogButton_Click() {

    var val = nativeObject.computeResult(0);
    var result = "";

    for (i = 0; i < val.length; i++) {
        result += val[i] + "<br/>";
    }

    document.getElementById('logResult').innerHTML = result;
}

function ButtonOrdered_Click() {
    document.getElementById('orderedPrimes').innerHTML = "Primes found (ordered): ";

    nativeObject.getPrimesOrdered(2, 10000).then(
        function (v) {
            for (var i = 0; i < v.length; i++)
                document.getElementById('orderedPrimes').innerHTML += v[i] + " ";
        },
        function (error) {
            document.getElementById('orderedPrimes').innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("OrderedPrimesProgressBar");
            progressBar.value = p;
        });
}

function ButtonUnordered_Click() {
    document.getElementById('unorderedPrimes').innerHTML = "Primes found (unordered): ";
    nativeObject.onprimefoundevent = handler_unordered;

    nativeObject.getPrimesUnordered(2, 10000).then(
        function () { },
        function (error) {
            document.getElementById("unorderedPrimes").innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("UnorderedPrimesProgressBar");
            progressBar.value = p;
        });
}

var handler_unordered = function (n) {
    document.getElementById('unorderedPrimes').innerHTML += n.target.toString() + " ";
};

function ButtonClear_Click() {

    document.getElementById('logResult').innerHTML = "";
    document.getElementById("unorderedPrimes").innerHTML = "";
    document.getElementById('orderedPrimes').innerHTML = "";
    document.getElementById("UnorderedPrimesProgressBar").value = 0;
    document.getElementById("OrderedPrimesProgressBar").value = 0;
}
```

default.js의 app.onactivated에서 WinJS.UI.processAll에 대한 기존 호출을 then 블록에서 이벤트 등록을 구현하는 다음 코드로 대체하여 이벤트 수신기를 추가하는 코드를 추가합니다. 자세한 내용은 "Hello World" 앱 만들기(JS)를 참조하세요.

```JavaScript
args.setPromise(WinJS.UI.processAll().then( function completed() {
    var logButton = document.getElementById("logButton");
    logButton.addEventListener("click", LogButton_Click, false);
    var orderedPrimeButton = document.getElementById("orderedPrimeButton");
    orderedPrimeButton.addEventListener("click", ButtonOrdered_Click, false);
    var buttonUnordered = document.getElementById("ButtonUnordered");
    buttonUnordered.addEventListener("click", ButtonUnordered_Click, false);
    var buttonClear = document.getElementById("Button_Clear");
    buttonClear.addEventListener("click", ButtonClear_Click, false);
}));
```

F5 키를 눌러 앱을 실행합니다.

## <a name="creating-a-c-client-app"></a>C# 클라이언트 앱 만들기

## <a name="to-create-a-c-project"></a>C# 프로젝트를 만들려면
솔루션 탐색기에서 솔루션 노드의 바로 가기 메뉴를 열고 **추가, 새 프로젝트**를 선택합니다.

Visual C#을 확장하고(**기타 언어** 아래에 중첩되어 있을 수 있음) **Windows**를 선택한 다음 왼쪽 창에서 **유니버설**을 선택하고 가운데 창에서 **빈 앱**을 선택합니다.

이 앱의 이름을 CS_Client로 지정하고 **확인** 단추를 선택합니다.

CS_Client 프로젝트 노드의 바로 가기 메뉴를 열고 **시작 프로젝트로 설정**을 선택합니다.

WinRT_CPP에 대한 프로젝트 참조를 추가합니다.

**참조** 노드의 바로 가기 메뉴를 열고 **참조 추가**를 선택합니다.

**참조 관리자** 대화 상자의 왼쪽 창에서 **프로젝트**를 선택한 다음 **솔루션**을 선택합니다.

가운데 창에서 WinRT_CPP를 선택한 다음 **확인** 단추를 선택합니다.

## <a name="to-add-the-xaml-that-defines-the-user-interface"></a>사용자 인터페이스를 정의하는 XAML을 추가하려면
MainPage.xaml의 Grid 요소에 다음 코드를 복사합니다.

```xaml
<ScrollViewer>
            <StackPanel Width="1400">

                <Button x:Name="Button1" Width="340" Height="50"  Margin="0,20,20,20" Content="Synchronous Logarithm Calculation" FontSize="16" Click="Button1_Click_1"/>
                <TextBlock x:Name="Result1" Height="100" FontSize="14"></TextBlock>
            <Button x:Name="PrimesOrderedButton" Content="Prime Numbers Ordered" FontSize="16" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesOrderedButton_Click_1"></Button>
            <ProgressBar x:Name="PrimesOrderedProgress" IsIndeterminate="false" Height="40"></ProgressBar>
                <TextBlock x:Name="PrimesOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>
            <Button x:Name="PrimesUnOrderedButton" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesUnOrderedButton_Click_1" Content="Prime Numbers Unordered" FontSize="16"></Button>
            <ProgressBar x:Name="PrimesUnOrderedProgress" IsIndeterminate="false" Height="40" ></ProgressBar>
            <TextBlock x:Name="PrimesUnOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>

            <Button x:Name="Clear_Button" Content="Clear" HorizontalAlignment="Left" Margin="0,20,20,20" VerticalAlignment="Top" Width="341" Click="Clear_Button_Click" FontSize="16"/>
        </StackPanel>
</ScrollViewer>
```

## <a name="to-add-the-event-handlers-for-the-buttons"></a>단추에 대한 이벤트 처리기를 추가하려면
솔루션 탐색기에서 MainPage.xaml.cs를 엽니다. MainPage.xaml 아래에 파일이 중첩되어 있을 수 있습니다. System.Text에 대해 using 지시문을 추가한 다음 MainPage 클래스에 로그 계산에 대한 이벤트 처리기를 추가합니다.

```csharp
private void Button1_Click_1(object sender, RoutedEventArgs e)
{
    // Create the object
    var nativeObject = new WinRT_CPP.Class1();

    // Call the synchronous method. val is an IList that
    // contains the results.
    var val = nativeObject.ComputeResult(0);
    StringBuilder result = new StringBuilder();
    foreach (var v in val)
    {
        result.Append(v).Append(System.Environment.NewLine);
    }
    this.Result1.Text = result.ToString();
}
```

순서가 지정된 결과에 대한 이벤트 처리기를 추가합니다.

```csharp
async private void PrimesOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (ordered): ");

    PrimesOrderedResult.Text = sb.ToString();

    // Call the asynchronous method
    var asyncOp = nativeObject.GetPrimesOrdered(2, 100000);

    // Before awaiting, provide a lambda or named method
    // to handle the Progress event that is fired at regular
    // intervals by the asyncOp object. This handler updates
    // the progress bar in the UI.
    asyncOp.Progress = (asyncInfo, progress) =>
        {
            PrimesOrderedProgress.Value = progress;
        };

    // Wait for the operation to complete
    var asyncResult = await asyncOp;

    // Convert the results to strings
    foreach (var result in asyncResult)
    {
        sb.Append(result).Append(" ");
    }

    // Display the results
    PrimesOrderedResult.Text = sb.ToString();
}
```

순서가 지정되지 않은 결과 및 코드를 다시 실행할 수 있도록 결과를 지우는 단추에 대한 이벤트 처리기를 추가합니다.

```csharp
private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (unordered): ");
    PrimesUnOrderedResult.Text = sb.ToString();

    // primeFoundEvent is a user-defined event in nativeObject
    // It passes the results back to this thread as they are produced
    // and the event handler that we define here immediately displays them.
    nativeObject.primeFoundEvent += (n) =>
    {
        sb.Append(n.ToString()).Append(" ");
        PrimesUnOrderedResult.Text = sb.ToString();
    };

    // Call the async method.
    var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

    // Provide a handler for the Progress event that the asyncResult
    // object fires at regular intervals. This handler updates the progress bar.
    asyncResult.Progress += (asyncInfo, progress) =>
        {
            PrimesUnOrderedProgress.Value = progress;
        };
}

private void Clear_Button_Click(object sender, RoutedEventArgs e)
{
    PrimesOrderedProgress.Value = 0;
    PrimesUnOrderedProgress.Value = 0;
    PrimesUnOrderedResult.Text = "";
    PrimesOrderedResult.Text = "";
    Result1.Text = "";
}
```

## <a name="running-the-app"></a>앱 실행
솔루션 탐색기에서 프로젝트 노드의 바로 가기 메뉴를 열고 **시작 프로젝트로 설정**을 선택하여 C# 프로젝트 또는 JavaScript 프로젝트를 시작 프로젝트로 선택합니다. 그런 다음 디버깅을 사용하여 실행하려면 F5 키를 누르고, 디버깅을 사용하지 않고 실행하려면 Ctrl+F5를 누릅니다.

## <a name="inspecting-your-component-in-object-browser-optional"></a>개체 브라우저에서 구성 요소 검사(옵션)
개체 브라우저에서 .winmd 파일에 정의된 모든 Windows 런타임 형식을 검사할 수 있습니다. 여기에는 플랫폼 네임스페이스와 기본 네임스페이스의 형식이 포함됩니다. 그러나 Platform::Collections 네임스페이스의 형식은 winmd 파일이 아니라 헤더 파일 collections.h에 정의되어 있으므로 개체 브라우저에 표시되지 않습니다.

## **<a name="to-inspect-a-component"></a>구성 요소를 검사하려면**
메뉴 모음에서 **보기, 개체 브라우저**(Ctrl+Alt+J)를 선택합니다.

개체 브라우저의 왼쪽 창에서 WinRT\_CPP 노드를 확장하여 구성 요소에 정의된 형식 및 메서드를 표시합니다.

## <a name="debugging-tips"></a>디버깅 팁
디버깅 환경을 개선하려면 공용 Microsoft 기호 서버에서 디버깅 기호를 다운로드하세요.

## **<a name="to-download-debugging-symbols"></a>디버깅 기호를 다운로드하려면**
메뉴 모음에서 **도구, 옵션**을 선택합니다.

**옵션** 대화 상자에서 **디버깅**을 확장하고 **기호**를 선택합니다.

**Microsoft 기호 서버**를 선택한 다음 **확인** 단추를 선택합니다.

처음으로 기호를 다운로드하는 경우 시간이 걸릴 수도 있습니다. 다음에 F5 키를 누를 때 성능을 향상시키려면 기호를 캐시할 로컬 디렉터리를 지정합니다.

구성 요소 DLL이 있는 JavaScript 솔루션을 디버그하는 경우 구성 요소에서 네이티브 코드를 단계별로 실행하거나 스크립트를 단계별로 실행하도록 디버거를 설정할 수 있지만 둘 다 동시에 실행하도록 설정할 수는 없습니다. 설정을 변경하려면 솔루션 탐색기에서 JavaScript 프로젝트 노드의 바로 가기 메뉴를 열고 **속성, 디버깅, 디버거 형식**을 선택합니다.

패키지 디자이너에서 적절한 기능을 선택해야 합니다. Package.appxmanifest 파일을 열어 패키지 디자이너를 열 수 있습니다. 예를 들어 사진 폴더의 파일을 프로그래밍 방식으로 액세스하는 경우 패키지 디자이너의 **기능** 창에서 **사진 라이브러리** 확인란을 선택해야 합니다.

JavaScript 코드가 구성 요소의 public 속성 또는 메서드를 인식할 수 없는 경우 JavaScript에서 Camel Casing을 사용 중인지 확인합니다. 예를 들어 `ComputeResult` C++ 메서드는 JavaScript에서 `computeResult`로 참조해야 합니다.

솔루션에서 C++ Windows 런타임 구성 요소 프로젝트를 제거하는 경우 JavaScript 프로젝트에서 프로젝트 참조를 수동으로 제거해야 합니다. 이렇게 하지 않으면 이후 디버그 또는 빌드 작업을 수행할 수 없습니다. 필요한 경우 DLL에 대한 어셈블리 참조를 추가할 수 있습니다.

## <a name="related-topics"></a>관련 항목
* [C++/CX로 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-cpp.md)
