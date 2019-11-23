---
title: Windows 런타임 구성 요소에서 이벤트 발생
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: JavaScript가 이벤트를 수신할 수 있도록 백그라운드 스레드에서 사용자 정의 대리자 형식의 이벤트를 발생시키는 방법입니다.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78bc43c26a73a6184e5788ad7d003813e567a8d0
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73052016"
---
# <a name="raising-events-in-windows-runtime-components"></a>Windows 런타임 구성 요소에서 이벤트 발생
> [!NOTE]
> /Winrt Windows 런타임 구성 요소에서 이벤트를 발생 시키는 방법에 대 한 자세한 내용은 [/winrt의 C++Author 이벤트](../cpp-and-winrt-apis/author-events.md)를 참조 하세요. [ C++](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)

Windows 런타임 구성 요소가 백그라운드 스레드(작업자 스레드)에서 사용자 정의 대리자 형식의 이벤트를 발생시키며 JavaScript가 이벤트를 받을 수 있게 하려는 경우 다음 방법 중 하나로 구현 및/또는 발생시킬 수 있습니다.

-   (옵션 1) [Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher)를 통해 이벤트를 발생시켜 이벤트를 JavaScript 스레드 컨텍스트로 마샬링합니다. 일반적으로 이것이 최상의 옵션이지만 일부 시나리오에서는 가장 빠른 성능을 제공하지 않을 수도 있습니다.
-   (옵션 2) [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt;를 사용하지만 형식 정보가 손실됩니다(이벤트 유형 정보가 손실됨). 옵션 1이 불가능하거나 성능이 부적절한 경우 형식 정보가 손실되어도 되면 두 번째 선택 항목으로 사용하기에 적합합니다.
-   (옵션 3) 구성 요소에 대한 고유한 프록시 및 스텁을 만듭니다. 이 옵션은 구현하기 가장 어렵지만 형식 정보가 유지되며 까다로운 시나리오에서 옵션 1에 비해 더 나은 성능을 제공할 수 있습니다.

이러한 옵션 중 하나를 사용하지 않고 백그라운드 스레드에서 이벤트를 발생시킬 경우 JavaScript 클라이언트에 이벤트가 수신되지 않습니다.

## <a name="background"></a>배경

모든 Windows 런타임 구성 요소 및 앱은 어떤 언어로 만들든지 관계없이 기본적으로 COM 개체입니다. Windows API에서는 대부분의 구성 요소가 백그라운드 스레드 및 UI 스레드에서 개체와 똑같이 잘 통신할 수 있는 Agile COM 개체입니다. COM 개체를 Agile로 설정할 수 없는 경우 UI 스레드-백그라운드 스레드 경계를 넘어 다른 COM 개체와 통신하려면 프록시 및 스텁으로 알려진 도우미 개체가 필요합니다. COM 용어로 이를 스레드 아파트 간 통신이라고 합니다.

Windows API의 개체는 대부분 Agile이거나 프록시 및 스텁이 기본 제공되어 있습니다. 그러나 Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler) 같은 제네릭 형식의 경우 형식 인수를 제공할 때까지 완전한 형식이 아니기 때문에 해당 프록시 및 스텁을 만들 수 없습니다. 프록시 또는 스텁이 없을 경우 문제가 되는 것은 JavaScript 클라이언트뿐이지만 C++ 또는 .NET 언어뿐 아니라 JavaScript에서 구성 요소를 사용할 수 있게 하려면 다음 세 가지 옵션 중 하나를 사용해야 합니다.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(옵션 1) CoreDispatcher를 통해 이벤트 발생

[Windows.UI.Core.CoreDispatcher](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher)를 사용하여 사용자 정의 대리자 형식의 이벤트를 보낼 수 있으며, JavaScript가 받을 수 있습니다. 어떤 옵션을 사용할지 잘 모르겠으면 이 옵션을 먼저 시도해 보세요. 이벤트 발생과 이벤트 처리 간의 대기 시간이 문제가 되는 경우 다른 옵션 중 하나를 시도합니다.

다음 예제에서는 CoreDispatcher를 사용하여 강력한 형식의 이벤트를 발생시키는 방법을 보여 줍니다. 형식 인수가 개체가 아니라 알림인지 확인합니다.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(옵션 2) EventHandler&lt;Object&gt;를 사용하지만 형식 정보 손실

백그라운드 스레드에서 이벤트를 전송하는 또 다른 방법은 [Windows.Foundation.EventHandler](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)&lt;Object&gt;를 이벤트 유형으로 사용하는 것입니다. Windows는 제네릭 형식의 구체적인 인스턴스화와 해당 프록시 및 스텁을 제공합니다. 단점은 이벤트 인수의 형식 정보와 보낸 사람이 손실된다는 것입니다. C++ 및 .NET 클라이언트는 설명서를 통해 이벤트가 수신될 때 다시 캐스팅할 형식을 알고 있어야 합니다. JavaScript 클라이언트는 원래 형식 정보가 필요하지 않습니다. 메타데이터의 해당 이름에 따라 인수 속성을 찾습니다.

이 예제에서는 C#에서 Windows.Foundation.EventHandler&lt;Object&gt;를 사용하는 방법을 보여 줍니다.

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

아래와 같이 JavaScript 쪽에서 이 이벤트를 사용합니다.

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(옵션 3) 고유한 프록시 및 스텁 만들기

완전하게 보존된 형식 정보가 있는 사용자 정의 이벤트 유형에서 성능을 향상시키려면 고유한 프록시 및 스텁 개체를 만들고 앱 패키지에 포함해야 합니다. 일반적으로 다른 두 옵션이 모두 부적절한 드문 경우에만 이 옵션을 사용해야 합니다. 또한 이 옵션이 다른 두 옵션보다 더 나은 성능을 제공한다는 보장은 없습니다. 실제 성능은 여러 요인에 따라 달라집니다. Visual Studio 프로파일러 또는 다른 프로파일링 도구를 사용하여 응용 프로그램의 실제 성능을 측정하고 이벤트가 사실상 병목인지 여부를 확인할 수 있습니다.

이 문서의 나머지 부분에서는 C#을 사용하여 Windows 런타임 구성 요소를 만든 후 C++를 사용하여 비동기 작업에서 구성 요소에 의해 발생한 Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; 이벤트를 JavaScript가 사용할 수 있도록 하는 프록시 및 스텁에 대한 DLL을 만드는 방법을 보여 줍니다. C++ 또는 Visual Basic을 사용하여 구성 요소를 만들 수도 있습니다. 프록시와 스텁을 만드는 단계는 동일합니다. 이 연습은 Windows 런타임 In Process 구성 요소 샘플(C++/CX) 만들기를 기반으로 하며 해당 용도를 설명하는 데 도움이 됩니다.

이 연습은 다음 부분으로 이루어져 있습니다.

-   여기서는 두 가지 기본 Windows 런타임 클래스를 만듭니다. 한 클래스는 [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://docs.microsoft.com/uwp/api/windows.foundation.typedeventhandler) 형식의 이벤트를 표시하고 다른 클래스는 JavaScript에 TValue의 인수로 반환되는 형식입니다. 두 클래스는 이후 단계를 완료할 때까지 JavaScript와 통신할 수 없습니다.
-   이 앱은 기본 클래스 개체를 활성화하고, 메서드를 호출한 다음 Windows 런타임 구성 요소에 의해 발생한 이벤트를 처리합니다.
-   이러한 작업은 프록시 및 스텁 클래스를 생성하는 도구에 필요합니다.
-   그런 다음 IDL 파일을 사용하여 프록시 및 스텁에 대한 C 소스 코드를 생성합니다.
-   COM 런타임이 찾을 수 있도록 프록시-스텁 개체를 등록하고 앱 프로젝트에서 프록시-스텁 DLL을 참조합니다.

## <a name="to-create-the-windows-runtime-component"></a>Windows 런타임 구성 요소를 만들려면

Visual Studio의 메뉴 모음에서 **파일 &gt; 새 프로젝트**를 선택합니다. **새 프로젝트** 대화 상자에서 **JavaScript &gt; 유니버설 Windows**를 확장하고 **빈 앱**을 선택합니다. 프로젝트 이름을 ToasterApplication으로 지정하고 **확인** 단추를 선택합니다.

C# Windows 런타임 구성 요소를 솔루션에 추가합니다. 솔루션 탐색기에서 솔루션의 바로 가기 메뉴를 열고 **추가 &gt; 새 프로젝트**를 선택합니다. **C# Visual &gt; Microsoft Store** 를 확장 하 고 **Windows 런타임 구성 요소**를 선택 합니다. 프로젝트 이름을 ToasterComponent로 지정하고 **확인** 단추를 선택합니다. ToasterComponent는 이후 단계에서 만들 구성 요소의 루트 네임스페이스가 됩니다.

솔루션 탐색기에서 솔루션의 바로 가기 메뉴를 열고 **속성**을 선택합니다. **속성 페이지** 대화 상자의 왼쪽 창에서 **구성 속성**을 선택하고 대화 상자 맨 위에서 **구성**을 **디버그**로, **플랫폼**을 x86, x64 또는 ARM으로 설정합니다. **확인** 단추를 선택합니다.

**중요** Platform = 모든 CPU는 나중에 솔루션에 추가할 네이티브 코드 Win32 DLL에 사용할 수 없으므로 작동 하지 않습니다.

솔루션 탐색기에서 class1.cs의 이름을 프로젝트 이름과 일치하도록 ToasterComponent.cs로 바꿉니다. Visual Studio에서 파일의 클래스 이름을 새 파일 이름과 일치하도록 자동으로 바꿉니다.

.cs 파일에서 Windows.Foundation 네임스페이스에 대한 using 지시문을 추가하여 TypedEventHandler를 범위로 가져옵니다.

프록시 및 스텁이 필요한 경우 구성 요소에서 인터페이스를 사용하여 공용 멤버를 표시해야 합니다. ToasterComponent.cs에서 toaster에 대한 인터페이스와 toaster가 생성하는 알림에 대한 인터페이스를 정의합니다.

**참고** C# 이 단계를 건너뛸 수 있습니다. 대신, 먼저 클래스를 만든 후 해당 바로 가기 메뉴를 열고 **리팩터링 &gt; 인터페이스 추출**을 선택합니다. 생성된 코드에서 수동으로 인터페이스에 공개 접근성을 제공합니다.

```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
```

IToast 인터페이스에는 알림 유형을 설명하기 위해 검색할 수 있는 문자열이 있습니다. IToaster 인터페이스에는 알림을 만들 메서드 및 알림이 만들어졌음을 나타내는 이벤트가 있습니다. 이 이벤트는 알림의 특정 부분(즉, 형식)을 반환하므로 형식화된 이벤트라고 합니다.

다음에는 이러한 인터페이스를 구현하며 나중에 프로그래밍할 JavaScript 앱에서 액세스할 수 있도록 public 및 sealed인 클래스가 필요합니다.

```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
```

앞의 코드에서 알림을 만든 후 스레드 풀 작업 항목을 회전하여 알림을 발생시킵니다. IDE에서 비동기 호출에 awaiIt 키워드를 적용하도록 제안할 수도 있지만 이 경우에는 메서드가 작업 결과에 따라 달라지는 작업을 수행하지 않으므로 필요하지 않습니다.

이전 코드의 비동기 호출에서는 ThreadPool. RunAsync를 사용 하 여 백그라운드 스레드에서 이벤트를 발생 시키는 간단한 방법을 **보여 합니다** . 다음 예제와 같이 이 특정 메서드를 작성할 수 있으며, .NET 작업 스케줄러에서 자동으로 async/await 호출을 UI 스레드로 다시 마샬링하기 때문에 제대로 작동합니다.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

프로젝트를 빌드할 때는 완전하게 빌드해야 합니다.

## <a name="to-program-the-javascript-app"></a>JavaScript 앱을 프로그래밍하는 방법

이제, 알림을 만들기 위해 정의한 클래스를 사용하도록 JavaScript 앱에 단추를 추가할 수 있습니다. 이 작업을 수행하기 앞서 방금 만든 ToasterComponent 프로젝트에 참조를 추가해야 합니다. 솔루션 탐색기에서 ToasterApplication 프로젝트를 위한 바로 가기 메뉴를 열고 **참조&gt; 추가**를 선택한 다음, **새 참조 추가** 버튼을 선택합니다. 솔루션 아래의 왼쪽 창에 있는 참조 추가 대화 상자에서 구성 요소 프로젝트를 선택한 다음, 가운데 창에서 ToasterComponent를 선택합니다. **확인** 단추를 선택합니다.

솔루션 탐색기에서 ToasterApplication 프로젝트에 대한 바로 가기 메뉴를 열고 **시작 프로젝트로 설정**을 선택합니다.

default.js 파일의 끝에 네임스페이스를 추가하여 구성 요소를 호출하고 다시 호출을 받는 기능을 포함시킵니다. 네임스페이스는 두 가지 기능을 수행하는데, 하나는 알림을 만드는 기능이고 또 하나는 알림 완료 이벤트를 처리하는 기능입니다. makeToast를 구현하면 Toaster 개체를 만들고 이벤트 처리기를 등록하며 알림을 만들 수 있습니다. 다음과 같이 지금까지는 이벤트 처리기가 많은 역할을 하지 않습니다.

```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
```

makeToast 기능은 버튼에 연결되어야 합니다. 만들어진 알림을 출력할 버튼과 공간이 포함되도록 default.html을 업데이트합니다.

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

TypedEventHandler를 사용하지 않았다면 이제 로컬 컴퓨터에서 앱을 실행하고 버튼을 클릭해 알림을 만들 수 있습니다. 하지만 앱에서는 어떤 일도 벌어지지 않습니다. 이유를 확인하려면 ToastCompletedEvent를 발생시키는 관리 코드를 디버깅합니다. 프로젝트를 중지하고 메뉴 모음에서 **&gt;Toaster 응용 프로그램 특성 디버깅**을 선택합니다. **디버거 형식**을 **관리 전용**으로 변경합니다. 메뉴 모음에서 다시 **디버그&gt; 예외**를 선택한 다음, **일반적인 언어 런타임 예외**를 선택합니다.

이제 앱을 실행하고 알림 만들기 버튼을 클릭합니다. 디버거는 잘못된 캐스팅 예외를 파악합니다. 메시지에 확실히 나타난 것은 아니지만, 이 예외는 해당 인터페이스에서 프록시가 누락되고 있기 때문에 발생합니다.

![누락 프록시](./images/debuggererrormissingproxy.png)

구성 요소에 대한 프록시와 스텁을 생성하는 첫 번째 단계는 고유 ID 또는 GUID를 인터페이스에 추가하는 것입니다. 그러나 사용되는 GUID 형식은 C#, Visual Basic, 또 다른 .NET 언어, C++ 중 어디에서 코딩을 하는지에 따라 다릅니다.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>구성 요소의 인터페이스에 대한 GUID를 생성하는 방법 (C# 및 기타 .NET 언어)

메뉴 모음에서 도구 &gt; GUID 만들기를 선택합니다. 대화 상자에서 5를 선택합니다. \[Guid ("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx ... xxxx ")\]. 새 GUID 단추를 선택하고 복사 버튼을 선택합니다.

![GUID 생성기 도구](./images/guidgeneratortool.png)

다시 인터페이스 정의로 이동한 다음, IToaster 인터페이스 바로 앞에 새 GUID를 붙여 넣습니다(아래 예를 참조). (예에서는 GUID를 사용하지 마십시오. 모든 고유한 인터페이스는 자체 GUID를 가지고 있습니다.)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

System.Runtime.InteropServices 네임스페이스에서 using 지시문을 추가합니다.

IToast 인터페이스에서 이러한 단계들을 반복합니다.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>구성 요소의 인터페이스(C++)에 대한 GUID를 생성하는 방법

메뉴 모음에서 도구 &gt; GUID 만들기를 선택합니다. 대화 상자에서 3을 선택합니다. static const struct GUID = {...}. 새 GUID 단추를 선택하고 복사 버튼을 선택합니다.

IToaster 인터페이스 정의 바로 앞에 GUID를 붙여 넣습니다. 붙여 넣기를 하고 나면 GUID는 아래 예와 비슷해집니다. (예에서는 GUID를 사용하지 마십시오. 모든 고유한 인터페이스는 자체 GUID를 가지고 있습니다.)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Windows.Foundation.Metadata에 대한 using 지시문을 추가해서 GuidAttribute를 범위로 가져옵니다.

이제, 아래 예처럼 포맷이 되도록 const GUID를 GuidAttribute으로 수동으로 변환합니다. 중괄호가 대괄호와 괄호로 바뀌고 뒤 세미콜론이 제거됩니다.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
IToast 인터페이스에서 이러한 단계들을 반복합니다.

인터페이스가 고유 ID를 가지고 있으므로 .winmd 파일을 winmdidl 명령줄 도구에 제공하여 IDL 파일을 생성한 다음, 해당 IDL 파일을 MIDL 명령줄 도구에 제공하여 프록시 및 스텁에 대한 C 소스 코드를 생성할 수 있습니다. 아래 단계에 나와 있듯이 사후 빌드 이벤트를 생성하면 Visual Studio가 이러한 작업을 수행합니다.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>프록시 및 스텁 소스 코드를 생성하는 방법

사용자 지정 사후 빌드 이벤트를 추가하려면 솔루션 탐색기에서 ToasterComponent 프로젝트에 대한 바로 가기 메뉴를 열고 "특성"을 선택합니다. 특성 페이지의 왼쪽 창에서 "빌드 이벤트"를 선택한 다음, "사후 빌드 편집" 버튼을 선택합니다. 다음 명령을 사후 빌드 명령줄에 추가합니다. (winmdidl 도구를 찾도록 환경 변수를 설정하려면 먼저 배치 파일을 호출해야 합니다.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**중요**  ARM 또는 x64 프로젝트 구성의 경우 MIDL/env 매개 변수를 x64 또는 arm32로 변경 합니다.

.winmd 파일이 변경될 때마다 IDL 파일이 다시 생성되도록 하려면 **사후 빌드 이벤트 실행**을 **빌드가 프로젝트 출력을 업데이트 할 때**로 변경합니다.
빌드 이벤트 속성 페이지는 ![빌드 이벤트와 비슷합니다](./images/buildevents.png)

IDL을 생성하고 컴파일할 솔루션을 다시 빌드합니다.

MIDL이 ToasterComponent 프로젝트 디렉터리에서 ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c 및 dlldata.c를 찾아서 솔루션을 올바르게 컴파일했는지 확인할 수 있습니다.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>프록시 및 스텁 코드를 DLL로 컴파일하는 방법

필요한 파일을 가지게 되었으므로 C++ 파일인 DLL을 만들도록 컴파일할 수 있습니다. 가능한 쉽게 이 작업을 수행하려면 프록시 빌드를 지원하도록 새 프로젝트를 추가합니다. ToasterApplication 솔루션에서 바로 가기 메뉴를 열고 **추가 > 새 프로젝트**를 선택합니다. **새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C++ &gt; windows &gt; 유니버설 windows**를 확장 한 다음 가운데 창에서 **DLL (UWP 앱)** 을 선택 합니다. (C++ Windows 런타임 구성 요소 프로젝트가 아니라는 점에 유의합니다.) 프로젝트 프록시의 이름을 설정하고 **확인** 단추를 선택합니다. 이러한 파일은 C# 클래스에서 뭔가 변경이 될 때 사후 빌드 이벤트에 의해 업데이트 됩니다.

기본적으로 프록시 프로젝트는 .h 파일과 C++ .cpp 파일을 생성합니다. DLL은 MIDL에서 생성된 파일에서 빌드가 되기 때문에 .h 및 .cpp 파일이 필요하지 않습니다. 솔루션 탐색기에서 이에 대한 바로 가기 메뉴를 열고 **제거**를 선택한 다음, 삭제를 확인합니다.

이제 프로젝트가 비었으므로 MIDL 생성 파일을 다시 추가할 수 있습니다. 프록시 프로젝트에 대한 바로 가기 메뉴를 연 다음, **추가 > 기존 항목**을 선택합니다. 대화 상자에서 ToasterComponent 프로젝트 디렉터리를 탐색하고 ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c 및 dlldata.c 파일을 선택합니다. **추가** 단추를 선택합니다.

프록시 프로젝트에서 .def 파일을 생성해 dlldata.c에 설명된 DLL 내보내기를 정의합니다. 프로젝트에 대한 바로 가기 메뉴를 열고 **추가 > 새 항목**을 선택합니다. 대화 상자의 왼쪽 창에서 "코드"를 선택하고 가운데 창에서 "모듈 정의 파일"을 선택합니다. proxies.def 파일의 이름을 지정한 다음 **추가** 단추를 선택합니다. 이.def 파일을 dlldata.c에 정의된 내보내기를 포함하도록 이를 수정합니다.

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

이제 프로젝트를 빌드하면 실패할 것입니다. 이 프로젝트 올바르게 컴파일하려면 프로젝트가 컴파일 및 연결되는 방법을 변경해야 합니다. 솔루션 탐색기에서 프록시 프로젝트에 대한 바로 가기 메뉴를 열고 **특성**을 선택합니다. 다음과 같이 특성 페이지를 변경합니다.

왼쪽된 창에서 **C/C++ > 전처리기**를 선택하고 오른쪽 창에서 **전처리기 정의**를 선택하고 아래쪽 화살표 단추를 선택한 다음, **편집**을 선택합니다. 상자에 이러한 정의를 추가합니다.

```cpp
WIN32;_WINDOWS
```
**C/C++ > 사전 컴파일된 헤더**에서 **사전 컴파일된 헤더**를 **미리 컴파일된 헤더 사용 안 함**으로 변경하고 **적용** 단추를 선택합니다.

**링커 > 일반**에서 **가져오기 라이브러리 무시**를 **예**로 변경한 다음, **적용** 단추를 선택합니다.

**링커 > 입력**에서 **추가 종속성**을 선택하고 아래쪽 화살표 단추를 선택한 다음, **편집**을 선택합니다. 상자에 이 텍스트를 추가합니다.

```cpp
rpcrt4.lib;runtimeobject.lib
```

이러한 라이브러리를 목록 행에 직접 붙여 넣지 마십시오. **편집** 박스를 사용해 Visual Studio의 MSBuild가 올바른 추가 종속성을 유지하는지 확인합니다.

이렇게 변경을 하면 **특성 페이지** 대화 상자에서  **확인** 단추를 선택합니다.

그런 다음, ToasterComponent 프로젝트에 종속성을 적용합니다. 이렇게 하면 프록시 프로젝트 빌드 전에 Toaster 빌드가 이뤄집니다. Toaster 프로젝트는 프록시 필드를 위한 파일을 생성할 책임이 있기 때문에 이것이 필요합니다.

프록시 프로젝트에 대한 바로 가기 메뉴를 연 다음, "프로젝트 종속성"을 선택합니다. 확인란을 선택해서 프록시 프로젝트가 ToasterComponent 프로젝트에 의존하는지 표시하여 Visual Studio이 올바른 순서로 이들을 빌드하도록 만듭니다.

Visual Studio 메뉴 모음에서 **빌드 > 솔루션 다시 빌드**를 선택하여 솔루션이 올바르게 빌드되는지 확인합니다.


## <a name="to-register-the-proxy-and-stub"></a>프록시 및 스텁을 등록하는 방법

ToasterApplication 프로젝트에서 package.appxmanifest에 대한 바로 가기를 열고 **연결 프로그램**을 엽니다. "연결 프로그램" 대화 상자에서 **XML 텍스트 편집기**를 선택하고 **확인** 단추를 선택합니다. windows.activatableClass.proxyStub 확장 등록을 제공하고 프록시의 GUID를 기반으로 하는 일부 XML에서 붙여 넣기를 해보겠습니다. .appxmanifest 파일에서 사용할 GUID를 찾으려면 ToasterComponent_i.c를 엽니다. 아래 예에 나온 것과 유사한 항목을 찾습니다. 또한 IToast, IToaster 및 세 번째 인터페이스(Toaster와 Toast라는 두 개의 매개 변수를 가진 입력된 이벤트 처리기)에 대한 정의를 확인합니다. 이것은 Toaster 클래스에 정의되어 있는 이벤트와 일치합니다. IToast 및 IToaster를 위한 GUID가 C# 파일의 인터페이스에 정의된 GUID와 일치하는지 확인합니다. 입력된 이벤트 처리기 인터페이스는 자동 생성이 되기 때문에 이 인터페이스에 대한 GUID 역시 자동 생성됩니다.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

이제 GUID를 복사했으면 이를 추가한 노드의 package.appxmanifest에 붙여 넣고 Extensions라고 이름을 지정한 다음, 다시 포맷합니다. 매니페스트 항목은 아래 예와 유사하지만, 자체 GUID를 사용한다는 점을 다시 한 번 기억하십시오. XML의 ClassId GUID가 ITypedEventHandler2와 동일한지 확인합니다. 이 GUID는 ToasterComponent_i.c에 나열된 첫 번째 ID이기 때문입니다. GUID는 대/소문자를 구분하지 않습니다. IToast 및 IToaster에 대한 GUID를 수동으로 다시 포맷하는 대신, 인터페이스 정의로 돌아가서 올바른 형식을 가진 GuidAttribute 값을 얻을 수 있습니다. C++에서는 주석에 올바르게 포맷된 GUID가 표시됩니다. 어떤 경우에는 ClassId와 이벤트 처리기 모두에서 사용되는 GUID를 수동으로 다시 포맷해야 합니다.

```cpp
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
```

확장 XML 노드를 패키지 노드의 직계 자식과 리소스 노드의 피어로 붙여 넣습니다.

계속 진행하기 전에 다음을 반드시 확인하십시오.

-   ProxyStub ClassId는 ToasterComponent\_i. c 파일의 첫 번째 GUID로 설정 됩니다. 이 파일에 정의된 첫 번째 GUID를 ClassId로 사용합니다. (이것은 ITypedEventHandler2를 위한 GUID와 같을 수 있습니다.)
-   경로는 프록시 이진 파일의 패키지 상대 경로입니다. (이 연습에서는 proxies.dll이 ToasterApplication.winmd와 동일한 폴더에 있습니다.)
-   GUID가 올바른 형식입니다. (형식 오류가 발생하기 쉽습니다.)
-   매니페스트의 인터페이스 Id는 ToasterComponent\_i. c 파일의 Iid와 일치 합니다.
-   인터페이스 이름은 매니페스트에서 고유합니다. 이들은 시스템에서 사용되지 않기 때문에 값을 선택할 수 있습니다. 정의한 인터페이스와 명확하게 일치하는 인터페이스 이름을 선택하는 것이 좋습니다. 이름은 생성된 인터페이스를 나타낼 수 있어야 합니다. ToasterComponent\_i. c 파일을 사용 하면 인터페이스 이름을 생성 하는 데 도움이 됩니다.

솔루션을 실행하려고 하면 proxies.dll이 페이로드에 속하지 않는 오류가 발생하게 됩니다. ToasterApplication 프로젝트의 **참조** 폴더에 있는 바로 가기 메뉴를 열고 **참조 추가**를 선택합니다. 프록시 프로젝트 옆의 확인란을 선택합니다. 또한 ToasterComponent 옆의 확인란도 선택되어 있는지 확인합니다. **확인** 단추를 선택합니다.

프로젝트가 이제 빌드되어야 합니다. 프로젝트를 실행하고 알림을 만들 수 있는지 확인합니다.

## <a name="related-topics"></a>관련 항목

* [C++/CX가 포함된 Windows 런타임 구성 요소](creating-windows-runtime-components-in-cpp.md)
