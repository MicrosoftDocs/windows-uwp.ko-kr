---
title: C++/CX Windows 런타임 구성 요소를 만들고 JavaScript 또는 C#에서 호출하는 연습
description: 이 연습에서는 JavaScript, C# 또는 Visual Basic에서 호출할 수 있는 기본 Windows 런타임 구성 요소 DLL을 만드는 방법을 보여 줍니다.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 155fd01b16f93ac8419282e5b2256f7e8a939a7f
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988755"
---
# <a name="walkthrough-of-creating-a-ccx-windows-runtime-component-and-calling-it-from-javascript-or-c"></a>C++/CX Windows 런타임 구성 요소를 만들고 JavaScript 또는 C#에서 호출하는 연습

> [!NOTE]
> 이 항목은 C++/CX 애플리케이션 유지에 도움을 주기 위해 작성되었습니다. 하지만 새로운 응용 프로그램에 대해 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 것이 좋습니다. C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 최신의 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT를 사용하여 Windows 런타임 구성 요소를 만드는 방법에 자세한 내용은 [C++/WinRT를 사용한 Windows 런타임 구성 요소](./create-a-windows-runtime-component-in-cppwinrt.md)를 참조하세요.

이 연습에서는 JavaScript, C# 또는 Visual Basic에서 호출할 수 있는 기본 Windows 런타임 구성 요소 DLL을 만드는 방법을 보여 줍니다. 이 연습을 시작하기 전에 쉽게 ref 클래스를 사용할 수 있게 해주는 ABI(추상 이진 인터페이스), ref 클래스, Visual C++ 구성 요소 확장 등의 개념을 이해해야 합니다. 자세한 내용은 [c + +/cx를 사용 하는 Windows 런타임 구성 요소](creating-windows-runtime-components-in-cpp.md) 및 [Visual C++ 언어 참조 (c + +/cx)](/cpp/cppcx/visual-c-language-reference-c-cx)를 참조 하세요.

## <a name="creating-the-c-component-dll"></a>C++ 구성 요소 DLL 만들기
이 예제에서는 구성 요소 프로젝트를 먼저 만들지만 실제로는 JavaScript 프로젝트를 먼저 만들 수도 있습니다. 만드는 순서는 중요하지 않습니다.

구성 요소의 주 클래스에는 속성 및 메서드 정의의 예제와 이벤트 선언이 포함되어 있습니다. 이러한 항목은 작업이 수행되는 방법을 보여 주기 위해 제공됩니다. 이러한 항목이 반드시 필요하지는 않지만 이 예제에서는 생성된 모든 코드를 고유한 코드로 바꿉니다.

### <a name="to-create-the-c-component-project"></a>**C++ 구성 요소 프로젝트를 만들려면**
1. Visual Studio 메뉴 모음에서 **파일, 새로 만들기, 프로젝트** 를 차례로 선택 합니다.

2. **새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C++** 을 확장 한 다음 유니버설 Windows 앱에 대 한 노드를 선택 합니다.

3. 가운데 창에서 **Windows 런타임 구성 요소** 를 선택 하 고 프로젝트 이름을 WinRT CPP로 선택 \_ 합니다.

4. **확인** 단추를 선택합니다.

## <a name="to-add-an-activatable-class-to-the-component"></a>**구성 요소에 활성화 가능한 클래스를 추가하려면**
활성화할 수 있는 클래스는 클라이언트 코드에서 **새** 식을 사용 하 여 만들 수 있는 클래스입니다 (Visual Basic의 **새로운** 또는 c + +의 **ref new** ). 구성 요소에서 **public ref 클래스로 sealed** 로 선언 합니다. 사실 Class1.h 및 .cpp 파일에는 ref 클래스가 이미 있습니다. 이름을 변경할 수 있지만이 예제에서는 기본 이름인 Class1을 사용 합니다. ref 클래스 또는 일반 클래스가 추가로 필요한 경우 구성 요소에서 해당 클래스를 정의할 수 있습니다. Ref 클래스에 대 한 자세한 내용은 [형식 시스템 (c + +/cx)](/cpp/cppcx/type-system-c-cx)을 참조 하세요.

다음 \# include 지시문을 Class1. h에 추가 합니다.

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

Collections는 Windows 런타임에서 정의한 언어 중립적인 인터페이스를 구현 하는 platform:: collections:: Vector 클래스 및 Platform:: Collections:: Map class와 같은 c + + 구체적 클래스의 헤더 파일입니다. Amp 헤더는 GPU에서 계산을 실행 하는 데 사용 됩니다. 이러한 항목에는 해당 하는 Windows 런타임 없으며 전용 이기 때문에 괜찮습니다. 일반적으로 성능상의 이유로 구성 요소 내에서 내부적으로 ISO c + + 코드 및 표준 라이브러리를 사용 해야 합니다. Windows 런타임 형식으로 표현 되어야 하는 Windows 런타임 인터페이스 일 뿐입니다.

## <a name="to-add-a-delegate-at-namespace-scope"></a>네임스페이스 범위에 대리자를 추가하려면
대리자는 메서드에 대한 매개 변수 및 반환 형식을 정의하는 구문입니다. 이벤트는 특정 대리자 형식의 인스턴스이며, 이벤트를 구독하는 모든 이벤트 처리기 메서드에는 대리자에 지정된 시그니처가 있어야 합니다. 다음 코드에서는 int를 사용 하 고 void를 반환 하는 대리자 형식을 정의 합니다. 그런 다음 코드는이 형식의 공용 이벤트를 선언 합니다. 이를 통해 클라이언트 코드는 이벤트가 발생할 때 호출 되는 메서드를 제공할 수 있습니다.

다음 대리자 선언을 Class1.h에서 Class1 선언 바로 앞의 네임스페이스 범위에 추가합니다.

```cpp
public delegate void PrimeFoundHandler(int result);
```

코드를 Visual Studio에 붙여 넣을 때 코드가 제대로 정렬되지 않으면 Ctrl+K+D를 눌러 전체 파일의 들여쓰기를 수정합니다.

## <a name="to-add-the-public-members"></a>public 멤버를 추가하려면
클래스는 세 개의 공용 메서드와 하나의 공용 이벤트를 노출합니다. 첫 번째 메서드는 항상 매우 빠르게 실행되기 때문에 동기 메서드됩니다. 다른 두 메서드는 시간이 오래 걸릴 수 있기 때문에 UI 스레드를 차단하지 않도록 비동기 메서드입니다. 이러한 메서드는 IAsyncOperationWithProgress 및 IAsyncActionWithProgress를 반환합니다. 앞의 메서드는 결과를 반환하는 비동기 메서드를 정의하고 뒤의 메서드는 void를 반환하는 비동기 메서드를 정의합니다. 이러한 인터페이스를 통해 클라이언트 코드는 작업의 진행 상황에 대한 업데이트를 받을 수도 있습니다.

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
## <a name="to-add-the-private-members"></a>전용 멤버를 추가하려면
클래스에는 세 개의 전용 멤버가 포함되어 있습니다. 숫자 계산을 위한 두 개의 도우미 메서드와 작업자 스레드의 이벤트 호출을 UI 스레드에 다시 마샬링하는 데 사용되는 하나의 CoreDispatcher 개체입니다.

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

### <a name="to-add-the-header-and-namespace-directives"></a>헤더 및 네임스페이스 지시문을 추가하려면
1. Class1 .cpp에서 다음 #include 지시문을 추가 합니다.

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

2. 이제 필요한 네임 스페이스를 가져오기 위해 다음 using 문을 추가 합니다.

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>ComputeResult 구현을 추가하려면
Class1.cpp에서 다음과 같은 메서드 구현을 추가합니다. 이 메서드는 호출 스레드에서 동기적으로 실행되지만 C++ AMP를 사용하여 GPU에서 계산을 병렬화하기 때문에 매우 빠릅니다. 자세한 내용은 C++ AMP 개요를 참조하세요. 결과는 <T> 반환 될 때 Windows:: Foundation:: collections:: IVector로 암시적으로 변환 되는 Platform:: Collections:: Vector 구체 형식에 추가 됩니다 <T> .

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/library/hh305254.aspx
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
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>GetPrimesOrdered에 대한 구현과 해당 도우미 메서드를 추가하려면
Class1 .cpp에서 GetPrimesOrdered 및 is_prime 도우미 메서드에 대 한 구현을 추가 합니다. GetPrimesOrdered는 concurrent_vector 클래스 및 parallel_for 함수 루프를 사용 하 여 작업을 나누고 프로그램이 실행 중인 컴퓨터의 최대 리소스를 사용 하 여 결과를 생성 합니다. 결과가 계산, 저장 및 정렬 된 후에는 Platform:: Collections:: Vector에 추가 되 <T> 고 클라이언트 코드에 Windows:: Foundation:: Collections:: IVector로 반환 됩니다 <T> .

클라이언트가 진행률 표시줄 또는 다른 UI를 연결하여 작업 시간이 얼마나 오래 걸리는지 사용자에게 보여줄 수 있는 진행률 보고자를 위한 코드가 있습니다. 진행률을 보고하려면 비용이 듭니다. 이벤트는 구성 요소 쪽에서 발생하고 UI 스레드 쪽에서 처리되어야 하며 진행률 값은 각 반복마다 저장되어야 합니다. 진행률 이벤트의 발생 빈도를 제한하는 것도 비용을 최소화할 수 있는 한 가지 방법입니다. 비용이 너무 많이 들거나 작업 기간을 예측할 수 없는 경우에는 작업이 진행 중임을 보여 주되 작업이 완료될 때까지 남은 시간은 보여 주지 않는 진행률 링을 사용하는 것이 좋습니다.

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

## <a name="to-add-the-implementation-for-getprimesunordered"></a>GetPrimesUnordered에 대한 구현을 추가하려면
C + + 구성 요소를 만드는 마지막 단계는 GetPrimesUnordered에 대 한 구현을 추가 하는 것입니다. 이 메서드는 모든 결과를 찾을 때까지 기다리지 않고 발견되는 각 결과를 반환합니다. 각 결과는 이벤트 처리기에 반환되며 실시간으로 UI에 표시됩니다. 여기에서도 진행률 보고자가 사용됩니다. 또한이 메서드는 is_prime 도우미 메서드를 사용 합니다.

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

## <a name="creating-a-javascript-client-app-visual-studio-2017"></a>JavaScript 클라이언트 앱 만들기 (Visual Studio 2017)

C # 클라이언트를 만들려는 경우이 섹션을 건너뛸 수 있습니다.

> [!NOTE]
> JavaScript를 사용 하는 UWP (유니버설 Windows 플랫폼) 프로젝트는 Visual Studio 2019에서 지원 되지 않습니다. [JavaScript 및 TypeScript In Visual Studio 2019을](/visualstudio/javascript/javascript-in-vs-2019#projects)참조 하세요. 이 섹션과 함께 수행 하려면 Visual Studio 2017을 사용 하는 것이 좋습니다. [Visual Studio 2017의 JavaScript를](/visualstudio/javascript/javascript-in-vs-2017)참조 하세요.

### <a name="to-create-a-javascript-project"></a>JavaScript 프로젝트를 만들려면
1. 솔루션 탐색기 (Visual Studio 2017의 경우 위의 **참고 사항** 참조), 솔루션 노드에 대 한 바로 가기 메뉴를 열고 **추가, 새 프로젝트를** 차례로 선택 합니다.

2. JavaScript ( **다른 언어** 아래에 중첩 될 수 있음)를 확장 하 고 **비어 있는 앱 (유니버설 Windows)** 을 선택 합니다.

3. &mdash; &mdash; **확인** 단추를 선택 하 여 기본 이름인 App1을 적용 합니다.

4. App1 프로젝트 노드에 대 한 바로 가기 메뉴를 열고 **시작 프로젝트로 설정** 을 선택 합니다.

5. 프로젝트 참조를 WinRT_CPP에 추가합니다.

6. 참조 노드에 대 한 바로 가기 메뉴를 열고 **참조 추가** 를 선택 합니다.

7. 참조 관리자 대화 상자의 왼쪽 창에서 **프로젝트** 를 선택 하 고 **솔루션** 을 선택 합니다.

8. 가운데 창에서 WinRT_CPP을 선택 하 고 **확인** 단추를 선택 합니다.

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>JavaScript 이벤트 처리기를 호출하는 HTML을 추가하려면
이 HTML을 <body> default.html 페이지의 노드:

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
기본 .css에서 본문 스타일을 제거 하 고 다음 스타일을 추가 합니다.

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

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>구성 요소 DLL을 호출하는 JavaScript 이벤트 처리기를 추가하려면
다음 함수를 default.js 파일 끝에 추가합니다. 이러한 함수는 기본 페이지의 단추를 선택할 때 호출됩니다. JavaScript에서 C++ 클래스를 활성화한 다음 메서드를 호출하고 반환 값을 사용하여 HTML 레이블을 채우는 방식을 살펴보세요.

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

WinJS에 대 한 기존 호출을에 대 한 기존 호출을 다음 코드로 대체 하 여 이벤트 수신기를 추가 하는 코드를 추가 합니다 default.js. 이에 대 한 자세한 설명은 ["Hello, 세계" 앱 만들기 (JS)](/windows/apps/get-started/)를 참조 하세요.

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

### <a name="to-create-a-c-project"></a>C# 프로젝트를 만들려면
1. 솔루션 탐색기에서 솔루션 노드에 대 한 바로 가기 메뉴를 열고 **추가, 새 프로젝트를** 차례로 선택 합니다.

2. Visual c # ( **다른 언어** 아래에 중첩 될 수 있음)을 확장 하 고 왼쪽 창에서 **Windows** 를 선택한 다음 **범용** 을 선택 하 고 가운데 창에서 **새 앱** 을 선택 합니다.

3. 이 앱의 이름을 CS_Client로 지정한 다음 **확인** 단추를 선택 합니다.

4. CS_Client 프로젝트 노드에 대 한 바로 가기 메뉴를 열고 **시작 프로젝트로 설정** 을 선택 합니다.

5. 프로젝트 참조를 WinRT_CPP에 추가합니다.

   - **참조** 노드에 대 한 바로 가기 메뉴를 열고 **참조 추가** 를 선택 합니다.

   - **참조 관리자** 대화 상자의 왼쪽 창에서 **프로젝트** 를 선택 하 고 **솔루션** 을 선택 합니다.

   - 가운데 창에서 WinRT_CPP을 선택한 다음 **확인** 단추를 선택 합니다.

## <a name="to-add-the-xaml-that-defines-the-user-interface"></a>사용자 인터페이스를 정의하는 XAML을 추가하려면
다음 코드를 MainPage의 Grid 요소에 복사 합니다.

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
솔루션 탐색기에서 MainPage.xaml.cs을 엽니다. 이 파일은 MainPage .xaml 아래에 중첩 될 수 있습니다. System.object에 using 지시문을 추가한 다음 MainPage 클래스에서 로그 계산에 대 한 이벤트 처리기를 추가 합니다.

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

정렬된 결과에 대한 이벤트 처리기를 추가합니다.

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

코드를 다시 실행할 수 있도록 정렬되지 않은 결과 및 결과를 지우는 단추에 대한 이벤트 처리기를 추가합니다.

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
솔루션 탐색기에서 프로젝트 노드에 대 한 바로 가기 메뉴를 열고 **시작 프로젝트로 설정** 을 선택 하 여 c # 프로젝트 또는 JavaScript 프로젝트를 시작 프로젝트로 선택 합니다. 그런 다음 F5 키를 눌러 디버깅이 설정된 상태로 실행하거나 Ctrl+F5를 눌러 디버깅 없이 실행합니다.

## <a name="inspecting-your-component-in-object-browser-optional"></a>개체 브라우저에서 구성 요소 검사(선택 사항)
개체 브라우저에서 winmd 파일에 정의 된 모든 Windows 런타임 유형을 검사할 수 있습니다. 여기에는 Platform 네임 스페이스 및 기본 네임 스페이스의 형식이 포함 됩니다. 그러나 Platform:: Collections 네임 스페이스의 형식은 winmd 파일이 아닌 헤더 파일 컬렉션에 정의 되어 있기 때문에 개체 브라우저에 표시 되지 않습니다.

### <a name="to-inspect-a-component"></a>**구성 요소를 검사 하려면**
1. 메뉴 모음에서 **보기, 개체 브라우저** (Ctrl + Alt + J)를 선택 합니다.

2. 개체 브라우저의 왼쪽 창에서 WinRT \_ CPP 노드를 확장 하 여 구성 요소에 정의 된 형식과 메서드를 표시 합니다.

## <a name="debugging-tips"></a>디버깅 팁
더 나은 디버깅 환경을 사용하려면 공용 Microsoft 기호 서버에서 디버깅 기호를 다운로드합니다.

### <a name="to-download-debugging-symbols"></a>**디버깅 기호를 다운로드 하려면**
1. 메뉴 모음에서 **도구, 옵션** 을 선택 합니다.

2. **옵션** 대화 상자에서 **디버깅** 을 확장 하 고 **기호** 를 선택 합니다.

3. **Microsoft 기호 서버** 를 선택 하 고 **확인** 단추를 선택 합니다.

기호를 처음 다운로드할 때는 시간이 걸릴 수도 있습니다. 다음에 F5 키를 누를 때 성능을 향상시키려면 기호를 캐시할 로컬 디렉터리를 지정합니다.

구성 요소 DLL이 들어 있는 JavaScript 솔루션을 디버깅하는 경우 스크립트를 단계별로 실행하거나 구성 요소의 네이티브 코드를 단계별로 실행하도록 디버거를 설정할 수는 있지만 동시에 두 가지를 다 실행하도록 디버거를 설정할 수는 없습니다. 설정을 변경 하려면 솔루션 탐색기에서 JavaScript 프로젝트 노드에 대 한 바로 가기 메뉴를 열고 **속성, 디버깅, 디버거 형식** 을 차례로 선택 합니다.

패키지 디자이너에서 적절한 기능을 선택해야 합니다. Appxmanifest.xml 파일을 열어 패키지 디자이너를 열 수 있습니다. 예를 들어 그림 폴더에 있는 파일에 프로그래밍 방식으로 액세스 하려는 경우 패키지 디자이너의 **기능** 창에서 **그림 라이브러리** 확인란을 선택 해야 합니다.

JavaScript 코드가 구성 요소의 공용 속성 또는 메서드를 인식하지 못하는 경우 JavaScript에서 카멜식 대/소문자를 사용하고 있는지 확인하세요. 예를 들어 `ComputeResult` c + + 메서드는 JavaScript에서로 참조 되어야 합니다 `computeResult` .

솔루션에서 c + + Windows 런타임 구성 요소 프로젝트를 제거 하는 경우에는 JavaScript 프로젝트 에서도 프로젝트 참조를 수동으로 제거 해야 합니다. 이렇게 하지 않으면 이후 디버그 또는 빌드 작업을 수행할 수 없습니다. 필요한 경우 DLL에 대한 어셈블리 참조를 추가할 수 있습니다.

## <a name="related-topics"></a>관련 항목
* [C++/CX가 포함된 Windows 런타임 구성 요소](creating-windows-runtime-components-in-cpp.md)
