---
title: Windows 런타임 구성 요소에서 이벤트 발생
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: JavaScript가 이벤트를 받을 수 있도록 백그라운드 스레드에서 사용자 정의 대리자 형식의 이벤트를 발생 시키는 방법입니다.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5b5678ad1a0666e6f008a2ec69ba63c35441edf
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493518"
---
# <a name="raising-events-in-windows-runtime-components"></a>Windows 런타임 구성 요소에서 이벤트 발생

> [!NOTE]
> [C + +/winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Windows 런타임 구성 요소에서 이벤트를 발생 시키는 방법에 대 한 자세한 내용은 [c + +/Winrt의 Author 이벤트](/windows/uwp/cpp-and-winrt-apis/author-events)를 참조 하세요.

Windows 런타임 구성 요소가 백그라운드 스레드 (작업자 스레드)에서 사용자 정의 대리자 형식의 이벤트를 발생 시키고 JavaScript가 이벤트를 받을 수 있도록 하려는 경우 이러한 방법 중 하나를 사용 하 여 구현 하거나 생성할 수 있습니다.

-   (옵션 1) 이벤트를 JavaScript 스레드 컨텍스트로 마샬링하기 위해 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) 를 통해 이벤트를 발생 시킵니다. 일반적으로 이 방법이 최선의 옵션이지만 이 방법을 사용해도 가장 빠른 성능이 제공되지 않는 경우도 있습니다.
-   (옵션 2) ** [Windows 기반 처리기](/uwp/api/windows.foundation.eventhandler-1) \<Object\> ** 를 사용 합니다 (이벤트 유형 정보는 손실 되지 않음). 옵션 1을 사용할 수 없거나 성능이 적절 하지 않은 경우 형식 정보 손실이 허용 되는 경우이 옵션을 선택 하는 것이 좋습니다. C # Windows 런타임 구성 요소를 작성 하는 경우에는 **Windows \<Object\> ** 를 사용할 수 없습니다. 대신 해당 형식이 [**system.object**](/dotnet/api/system.eventhandler)로 프로젝션 되므로이를 대신 사용 해야 합니다.
-   (옵션 3) 구성 요소에 대 한 고유한 프록시 및 스텁을 만듭니다. 이 옵션은 구현하기가 가장 어렵지만 형식 정보를 유지하며, 요구되는 시나리오에서 옵션 1에 비해 더 나은 성능을 제공할 수 있습니다.

이러한 옵션 중 하나를 사용하지 않고 백그라운드 스레드에서 이벤트를 발생시키면 JavaScript 클라이언트가 이벤트를 받지 못합니다.

## <a name="background"></a>배경

모든 Windows 런타임 구성 요소와 앱은 만들 때 사용한 언어에 관계없이 기본적으로 COM 개체입니다. Windows API에서 대부분의 구성 요소는 백그라운드 스레드 및 UI 스레드의 개체와 동일하게 제대로 통신할 수 있는 Agile COM 개체입니다. COM 개체를 Agile 개체로 만들 수 없는 경우에는 UI 스레드 백그라운드 스레드 경계의 다른 COM 개체와 통신하기 위해서는 프록시 및 스텁이라는 도우미 개체가 필요합니다. COM 용어로는 이를 스레드 아파트 간의 통신이라고 합니다.

Windows API에서 대부분의 개체는 Agile 개체이거나 기본 제공되는 프록시 및 스텁을 가지고 있습니다. 그러나 Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler) 같은 제네릭 형식의 경우 형식 인수를 제공할 때까지 완전한 형식이 아니기 때문에 해당 프록시 및 스텁을 만들 수 없습니다. JavaScript 클라이언트에서만 프록시나 스텁이 부족한 것이 문제가 되지만 JavaScript뿐 아니라 C++ 또는 .NET 언어에서 구성 요소를 사용할 수 있도록 하려면 다음 세 옵션 중 하나를 사용해야 합니다.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(옵션 1) CoreDispatcher를 통해 이벤트 발생

[CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)를 사용 하 여 사용자 정의 대리자 형식의 이벤트를 보낼 수 있으며 JavaScript는 이러한 이벤트를 받을 수 있습니다. 어떤 옵션을 사용해야 할지 잘 모를 경우 먼저 이 옵션을 사용해 보세요. 이벤트 발생과 이벤트 처리 간 대기 시간이 문제가 되는 경우에는 다른 옵션 중 하나를 사용해 보십시오.

다음 예제에서는 CoreDispatcher를 사용하여 강력한 형식의 이벤트를 발생시키는 방법을 보여 줍니다. 형식 인수는 개체가 아니라 알림입니다.

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

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(옵션 2) EventHandler 개체를 사용 &lt; &gt; 하지만 형식 정보를 손실 합니다.

> [!NOTE]
> C # Windows 런타임 구성 요소를 작성 하는 경우에는 **Windows \<Object\> ** 를 사용할 수 없습니다. 대신 해당 형식이 [**system.object**](/dotnet/api/system.eventhandler)로 프로젝션 되므로이를 대신 사용 해야 합니다.

백그라운드 스레드에서 이벤트를 보내는 또 다른 방법은 [Windows.Foundation.EventHandler](/uwp/api/windows.foundation.eventhandler) &lt; &gt; 이벤트의 형식으로 Windows를 사용 하는 것입니다. Windows에서는 이러한 제네릭 형식의 구체적인 인스턴스화를 제공하며 이를 위한 프록시 및 스텁을 제공합니다. 단점은 이벤트 인수 및 전송자의 형식 정보가 손실된다는 점입니다. C++ 및 .NET 클라이언트는 이벤트를 받았을 때 어떤 형식을 캐스팅해야 할지 설명서를 통해 알아야 합니다. JavaScript 클라이언트에는 원래 형식 정보가 필요하지 않습니다. JavaScript 클라이언트는 메타데이터에 있는 이름을 기반으로 인수 속성을 찾습니다.

이 예제에서는 &lt; &gt; c #에서 다음과 같이 Windows를 사용 하는 방법을 보여 줍니다.

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

다음과 같이 JavaScript 쪽에서 이 이벤트를 사용합니다.

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(옵션 3) 사용자 고유 프록시 및 스텁 만들기

완전히 유지되는 형식 정보가 있는 사용자 정의 이벤트 형식에서 잠재적인 성능 향상 이점을 얻으려면 사용자 고유 프록시 및 스텁 개체를 만들어 앱 패키지에 포함해야 합니다. 일반적으로 드문 경우지만 다른 두 옵션 모두 적절하지 않은 경우에만 이 옵션을 사용해야 합니다. 또한 이 옵션을 사용할 경우 다른 두 옵션을 사용할 때보다 성능이 더 낫다는 보장이 없습니다. 실제 성능은 여러 요인에 따라 달라집니다. Visual Studio 프로파일러 또는 기타 프로파일링 도구를 사용하여 실제 응용 프로그램 성능을 측정하고 이벤트에 병목 현상이 있는지 여부를 확인합니다.

이 문서의 나머지 부분에서는 c #을 사용 하 여 기본 Windows 런타임 구성 요소를 만든 다음 c + +를 사용 하 여 JavaScript가 &lt; &gt; 비동기 작업에서 구성 요소에 의해 발생 하는 Windows Foundation. TypedEventHandler tsender, TResult 이벤트를 사용할 수 있도록 하는 프록시 및 스텁을 위한 DLL을 만드는 방법을 보여 줍니다. C++ 또는 Visual Basic을 사용하여 구성 요소를 만들 수도 있습니다. 프록시 및 스텁 만들기와 관련 된 단계는 동일 합니다. 이 연습은 Windows 런타임 in-process 구성 요소 샘플 만들기 (c + +/CX)를 기반으로 하며 용도를 설명 하는 데 도움이 됩니다.

이 연습에는 이러한 부분이 있습니다.

-   여기서는 두 가지 기본 Windows 런타임 클래스를 만듭니다. 한 클래스는 TValue 형식의 이벤트를 노출 하 고, 다른 클래스는 JavaScript에 반환 되는 형식으로 서 [ &lt; , &gt; ](/uwp/api/windows.foundation.typedeventhandler-2) JavaScript로 반환 됩니다. 이후 단계를 완료할 때까지 이러한 클래스는 JavaScript와 통신할 수 없습니다.
-   이 앱은 기본 클래스 개체를 활성화 하 고, 메서드를 호출 하 고, Windows 런타임 구성 요소에 의해 발생 하는 이벤트를 처리 합니다.
-   프록시 및 스텁 클래스를 생성 하는 도구에 필요 합니다.
-   그런 다음 IDL 파일을 사용 하 여 프록시 및 스텁 용 C 소스 코드를 생성 합니다.
-   COM 런타임이 해당 개체를 찾을 수 있도록 프록시-스텁 개체를 등록 하 고 앱 프로젝트에서 프록시-스텁 DLL을 참조할 수 있습니다.

## <a name="to-create-the-windows-runtime-component"></a>Windows 런타임 구성 요소를 만들려면

Visual Studio의 메뉴 모음에서 **파일 &gt; 새로 만들기 프로젝트**를 선택 합니다. **새 프로젝트** 대화 상자에서 **JavaScript &gt; 유니버설 Windows** 를 확장 한 다음 새 **앱**을 선택 합니다. 프로젝트 이름을 ToasterApplication로 지정한 다음 **확인** 단추를 선택 합니다.

C # Windows 런타임 구성 요소를 솔루션에 추가 합니다. 솔루션 탐색기에서 솔루션에 대 한 바로 가기 메뉴를 열고 ** &gt; 새 프로젝트 추가**를 선택 합니다. **Visual c # &gt; Microsoft Store** 를 확장 한 다음 **Windows 런타임 구성 요소**를 선택 합니다. 프로젝트 이름을 ToasterComponent로 지정한 다음 **확인** 단추를 선택 합니다. ToasterComponent는 이후 단계에서 만들 구성 요소의 루트 네임스페이스가 됩니다.

솔루션 탐색기에서 솔루션에 대 한 바로 가기 메뉴를 열고 **속성**을 선택 합니다. **속성 페이지** 대화 상자의 왼쪽 창에서 **구성 속성** 을 선택한 다음 대화 상자의 위쪽에서 **구성** 을 **디버그** 로 설정 하 고 **플랫폼** 을 x86, x64 또는 ARM으로 설정 합니다. **확인** 단추를 선택합니다.

**중요**   Platform = 모든 CPU는 나중에 솔루션에 추가할 네이티브 코드 Win32 DLL에 사용할 수 없으므로 작동 하지 않습니다.

솔루션 탐색기에서 class1.cs의 이름을 프로젝트 이름과 일치하도록 ToasterComponent.cs로 바꿉니다. 파일에 있는 클래스가 새 파일 이름에 맞게 자동으로 바뀝니다.

.Cs 파일에서 Windows Foundation 네임 스페이스에 대 한 using 지시문을 추가 하 여 TypedEventHandler를 범위로 가져옵니다.

프록시 및 스텁이 필요한 경우 구성 요소가 인터페이스를 사용하여 public 멤버를 노출해야 합니다. ToasterComponent.cs에서 토스터에 대한 인터페이스를 하나 정의하고 토스터가 생성하는 알림에 대해 다른 인터페이스를 정의합니다.

**참고**   C #에서는이 단계를 건너뛸 수 있습니다. 대신 먼저 클래스를 만든 다음 바로 가기 메뉴를 열고 **리팩터링 &gt; 인터페이스 추출**을 선택 합니다. 생성되는 코드에서 인터페이스에 public 액세스 가능성을 수동으로 제공합니다.

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

IToast 인터페이스에는 알림 형식을 설명 하기 위해 검색할 수 있는 문자열이 있습니다. IToaster 인터페이스에는 알림을 만드는 메서드와 알림이 생성 되었음을 나타내는 이벤트가 있습니다. 이 이벤트는 알림의 특정 부분(형식)을 반환하므로 형식화된 이벤트라고 합니다.

이제 나중에 프로그래밍할 JavaScript 앱에서 액세스할 수 있도록 이러한 인터페이스를 구현하는 봉인된 공용 클래스가 필요합니다.

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

위의 코드에서는 알림을 만든 다음 스레드 풀 작업 항목을 스핀업하여 알림을 발생시킵니다. IDE는 비동기 호출에 wait 키워드를 적용 하도록 제안할 수 있지만 메서드는 작업 결과에 따라 달라 지는 작업을 수행 하지 않으므로이 경우에는 필요 하지 않습니다.

**참고**   이전 코드의 비동기 호출에서는 스레드 풀을 사용 하 여 백그라운드 스레드에서 이벤트를 발생 시키는 간단한 방법을 보여 줍니다. 다음 예제와 같이 이러한 특정 메서드를 작성할 수 있으며 .NET 작업 스케줄러가 async/await 호출을 UI 스레드에 다시 자동으로 마샬링해야 하므로 이 메서드는 제대로 작동할 것입니다.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

지금 프로젝트를 빌드하는 경우 정상적으로 빌드됩니다.

## <a name="to-program-the-javascript-app"></a>JavaScript 앱을 프로그래밍하려면

이제 알림을 만들기 위해 방금 정의한 클래스를 사용하도록 JavaScript 앱에 단추를 추가할 수 있습니다. 이 작업을 수행하기 전에, 방금 만든 ToasterComponent 프로젝트에 대한 참조를 추가해야 합니다. 솔루션 탐색기에서 ToasterApplication 프로젝트에 대 한 바로 가기 메뉴를 열고 ** &gt; 참조 추가**를 선택한 다음 **새 참조 추가** 단추를 선택 합니다. 참조 추가 대화 상자의 솔루션 아래에 있는 왼쪽 창에서 구성 요소 프로젝트를 선택한 다음 가운데 창에서 ToasterComponent를 선택합니다. **확인** 단추를 선택합니다.

솔루션 탐색기에서 ToasterApplication 프로젝트에 대 한 바로 가기 메뉴를 열고 **시작 프로젝트로 설정**을 선택 합니다.

Default.js 파일의 끝에 구성 요소를 호출하고 구성 요소에 의해 다시 호출되는 함수를 포함하는 네임스페이스를 추가합니다. 네임스페이스는 두 개의 함수를 포함하게 됩니다. 하나는 알림을 만드는 함수이고, 다른 하나는 알림 완료 이벤트를 처리하는 함수입니다. MakeToast의 구현은 Toaster 개체를 만들고 이벤트 처리기를 등록 하 고 알림을 만듭니다. 지금까지는 이벤트 처리기가 아래와 같이 작업을 많이 수행하지 않습니다.

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

MakeToast 함수는 단추로 후크 되어야 합니다. 알림을 만든 결과를 출력할 공간과 단추를 포함하도록 default.html을 업데이트합니다.

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

TypedEventHandler를 사용 하지 않는 경우 이제 로컬 컴퓨터에서 앱을 실행 하 고 단추를 클릭 하 여 알림을 만들 수 있습니다. 그러나 앱에서 아무 변화가 없습니다. 이유를 확인 하려면 To Completedevent를 실행 하는 관리 코드를 디버그 하겠습니다. 프로젝트를 중지 한 다음 메뉴 모음에서 **디버그 &gt; Toaster 응용 프로그램 속성**을 선택 합니다. **디버거 형식** 을 **관리 전용**으로 변경 합니다. 메뉴 모음에서 **디버그 &gt; 예외**를 선택한 다음 **공용 언어 런타임 예외**를 선택 합니다.

이제 앱을 실행하고 알림 만들기 단추를 클릭합니다. 디버거가 잘못된 캐스팅 예외를 catch합니다. 메시지만으로 확실하지는 않지만 이 예외는 해당 인터페이스에 대한 프록시가 누락되어 발생하고 있습니다.

![누락 된 프록시](./images/debuggererrormissingproxy.png)

구성 요소에 대한 프록시 및 스텁을 만드는 첫 번째 단계는 인터페이스에 고유 ID 또는 GUID를 를 추가하는 것입니다. 그러나 사용할 GUID 형식은 코딩에 사용 중인 언어가 C#, Visual Basic, 다른 .NET 언어 또는 C++인지에 따라 달라집니다.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>구성 요소의 인터페이스에 대 한 Guid를 생성 하려면 (c # 및 기타 .NET 언어)

메뉴 모음에서 도구 &gt; GUID 만들기를 선택 합니다. 대화 상자에서 5를 선택 합니다. \[Guid ("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx ... xxxx ") \] . 새 GUID 단추를 선택한 다음 복사 단추를 선택합니다.

![guid 생성기 도구](./images/guidgeneratortool.png)

인터페이스 정의로 돌아가서 다음 예제와 같이 IToaster 인터페이스 바로 앞에 새 GUID를 붙여 넣습니다. 예제의 GUID는 사용하지 마세요. 모든 고유 인터페이스마다 고유 GUID를 가져야 합니다.

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

T e m 네임 스페이스에 대 한 using 지시문을 추가 합니다.

IToast 인터페이스에 대해 이러한 단계를 반복 합니다.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>구성 요소의 인터페이스에 대 한 Guid를 생성 하려면 (c + +)

메뉴 모음에서 도구 &gt; GUID 만들기를 선택 합니다. 대화 상자에서 3을 선택 합니다. static const struct GUID = {...}. 새 GUID 단추를 선택한 다음 복사 단추를 선택합니다.

IToaster 인터페이스 정의 바로 앞에 GUID를 붙여 넣습니다. 붙여 넣은 후 GUID는 다음 예제와 비슷해야 합니다. 예제의 GUID는 사용하지 마세요. 모든 고유 인터페이스마다 고유 GUID를 가져야 합니다.
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
GuidAttribute에 대 한 using 지시문을 추가 하 여 범위를 범위로 가져옵니다.

이제 다음 예제와 같이 서식이 지정되도록 수동으로 const GUID를 GuidAttribute로 변환합니다. 중괄호가 대괄호 및 괄호로 바뀌고 후행 세미콜론은 제거됩니다.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
IToast 인터페이스에 대해 이러한 단계를 반복 합니다.

인터페이스에는 고유한 Id가 있기 때문에 winmdidl 명령줄 도구에 winmd 파일을 공급 하 여 IDL 파일을 만든 다음 해당 IDL 파일을 MIDL 명령줄 도구에 공급 하 여 프록시와 스텁을 위한 C 소스 코드를 생성할 수 있습니다. 다음 단계에서와 같이 빌드 후 이벤트를 만들면 Visual Studio가 자동으로 이 작업을 수행해 줍니다.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>프록시 및 스텁 소스 코드를 생성하려면

사용자 지정 빌드 후 이벤트를 추가하려면 솔루션 탐색기에서 ToasterComponent 프로젝트에 대한 바로 가기 메뉴를 열고 속성을 선택합니다. 속성 페이지의 왼쪽 창에서 빌드 이벤트를 선택한 다음 빌드 후 편집 단추를 선택합니다. 빌드 후 명령줄에 다음 명령을 추가합니다. Winmdidl 도구를 찾기 위해 환경 변수를 설정 하려면 먼저 배치 파일을 호출 해야 합니다.

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**중요**    ARM 또는 x64 프로젝트 구성의 경우 MIDL/env 매개 변수를 x64 또는 arm32로 변경 합니다.

Winmd 파일이 변경 될 때마다 IDL 파일이 다시 생성 되도록 하려면 **빌드에서 프로젝트 출력을 업데이트할 때** **빌드 후 이벤트 실행** 을 변경 합니다.
빌드 이벤트 속성 페이지는 빌드 이벤트와 비슷해야 합니다. ![](./images/buildevents.png)

솔루션을 다시 빌드하여 IDL을 생성하고 컴파일합니다.

ToasterComponent 프로젝트 디렉터리에서 ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c 및 dlldata.c를 찾아 MIDL이 솔루션을 올바르게 컴파일했는지 확인할 수 있습니다.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>프록시 및 스텁 코드를 DLL로 컴파일하려면

필요한 파일을 가지고 있으므로 해당 파일을 컴파일하여 C++ 파일인 DLL을 생성할 수 있습니다. 이 작업을 최대한 쉽게 수행하려면 프록시 빌드를 지원하는 새 프로젝트를 추가합니다. ToasterApplication 솔루션에 대 한 바로 가기 메뉴를 열고 **추가 > 새 프로젝트**를 선택 합니다. **새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C++ &gt; windows &gt; 유니버설 windows**를 확장 한 다음 가운데 창에서 **DLL (UWP 앱)** 을 선택 합니다. 이는 c + + Windows 런타임 구성 요소 프로젝트가 아닙니다. 프로젝트 이름을 프록시로 지정한 다음 **확인** 단추를 선택 합니다. C# 클래스를 변경하면 빌드 후 이벤트에 의해 이 파일들이 업데이트됩니다.

기본적으로 Proxies 프로젝트는 헤더 .h 파일과 C++ .cpp 파일을 생성합니다. DLL은 MIDL에서 생성된 파일에서 빌드되므로 .h 및 .cpp 파일이 필요하지 않습니다. 솔루션 탐색기에서 해당 메뉴에 대 한 바로 가기 메뉴를 열고 **제거**를 선택한 다음 삭제를 확인 합니다.

프로젝트가 비어 있으므로 MIDL 생성 파일을 다시 추가할 수 있습니다. 프록시 프로젝트에 대 한 바로 가기 메뉴를 열고 **기존 항목 > 추가를 선택 합니다.** 대화 상자에서 ToasterComponent 프로젝트 디렉터리로 이동한 다음 ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c 및 dlldata.c 파일을 선택합니다. **추가** 단추를 선택합니다.

Proxies 프로젝트에서 .def 파일을 만들어 dlldata.c에 설명된 DLL 내보내기를 정의합니다. 프로젝트에 대 한 바로 가기 메뉴를 열고 **추가 > 새 항목**을 선택 합니다. 대화 상자의 왼쪽 창에서 코드를 선태한 다음 가운데 창에서 모듈 정의 파일을 선택합니다. 파일 이름을 프록시와로 지정한 다음 **추가** 단추를 선택 합니다. 이 .def 파일을 열고 dlldata.c에 정의된 EXPORTS를 포함하도록 수정합니다.

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

지금 프로젝트를 빌드하면 프로젝트가 실패합니다. 이 프로젝트를 올바르게 컴파일하려면 프로젝트의 컴파일 및 연결 방법을 변경해야 합니다. 솔루션 탐색기에서 프록시 프로젝트에 대 한 바로 가기 메뉴를 열고 **속성**을 선택 합니다. 속성 페이지를 다음과 같이 변경 합니다.

왼쪽 창에서 **C/c + + > 전처리기**를 선택한 다음 오른쪽 창에서 **전처리기 정의**를 선택 하 고 아래쪽 화살표 단추를 선택한 다음 **편집**을 선택 합니다. 상자에 이러한 정의를 추가합니다.

```cpp
WIN32;_WINDOWS
```
**C/c + + > 미리**컴파일된 헤더에서 미리 컴파일된 **헤더** 를 미리 컴파일된 헤더 **사용 안 함**으로 변경한 다음 **적용** 단추를 선택 합니다.

**링커 > 일반**에서 **가져오기 라이브러리 무시** 를 **변경 하 고 적용 단추를 선택**합니다. **Apply**

**링커 > 입력**에서 **추가 종속성**을 선택 하 고 아래쪽 화살표 단추를 선택한 다음 **편집**을 선택 합니다. 상자에 이 텍스트를 추가합니다.

```cpp
rpcrt4.lib;runtimeobject.lib
```

이러한 라이브러리를 목록 행에 직접 붙여 넣지는 마세요. **편집** 상자를 사용 하 여 Visual Studio의 MSBuild가 올바른 추가 종속성을 유지 하도록 합니다.

이러한 변경을 수행한 후에는 **속성 페이지** 대화 상자에서 **확인** 단추를 선택 합니다.

이제 ToasterComponent 프로젝트에 대한 종속성을 사용합니다. 이렇게 하면 프록시 프로젝트가 빌드되기 전에 Toaster가 빌드됩니다. 이는 Toaster 프로젝트가 프록시를 빌드하기 위한 파일 생성을 담당하기 때문에 필요합니다.

Proxies 프로젝트에 대한 바로 가기 메뉴를 열고 프로젝트 종속성을 선택합니다. Proxies 프로젝트가 ToasterComponent 프로젝트에 종속됨을 나타내는 확인란을 선택하여 프로젝트가 올바른 순서로 빌드되도록 합니다.

Visual Studio 메뉴 모음에서 **빌드 > 솔루션 다시** 빌드를 선택 하 여 솔루션이 올바르게 빌드되는지 확인 합니다.


## <a name="to-register-the-proxy-and-stub"></a>프록시 및 스텁을 등록하려면

ToasterApplication 프로젝트에서 appxmanifest.xml에 대 한 바로 가기 메뉴를 연 다음 **연결 프로그램**을 선택 합니다. 연결 프로그램 대화 상자에서 **XML 텍스트 편집기** 를 선택한 다음 **확인** 단추를 선택 합니다. ActivatableClass proxyStub 확장 등록을 제공 하 고 프록시의 Guid를 기반으로 하는 일부 XML로 붙여넣을 예정입니다. .appxmanifest 파일에서 사용할 GUID를 찾으려면 ToasterComponent_i.c를 엽니다. 다음 예제에서와 유사한 항목을 찾습니다. 또한 IToast, IToaster 및 세 번째 인터페이스 (Toaster 및 Toast 라는 두 매개 변수가 있는 형식화 된 이벤트 처리기)에 대 한 정의를 확인 합니다. 이는 Toaster 클래스에 정의 된 이벤트와 일치 합니다. IToast 및 IToaster의 Guid는 c # 파일의 인터페이스에 정의 된 Guid와 일치 합니다. 형식화된 이벤트 처리기 인터페이스는 자동 생성되므로 이 인터페이스의 GUID도 자동 생성됩니다.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

이제 Guid를 복사 하 여 확장명을 추가 하 고 이름을 지정 하는 노드의 appxmanifest.xml에 붙여넣습니다. 매니페스트 항목은 다음 예제와 비슷하지만 반드시 사용자 고유 GUID를 사용하세요. XML의 ClassId ClassId가 ITypedEventHandler2와 동일합니다. GUID가 ToasterComponent_i.c에 첫 번째로 나열되는 항목이기 때문입니다. 여기에서 GUID는 대소문자를 구분합니다. IToast 및 IToaster에 대 한 Guid를 수동으로 다시 포맷 하는 대신 인터페이스 정의로 돌아가서 올바른 형식의 GuidAttribute 값을 가져올 수 있습니다. C++에서는 주석에 올바르게 서식이 지정된 GUID가 있습니다. 어떤 경우에도 ClassId 및 이벤트 처리기에 사용된 GUID의 서식을 수동으로 다시 지정해야 합니다.

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

Extensions XML 노드를 패키지 노드의 직계 자식으로 붙여넣고, 예를 들어 Resources 노드와 같은 피어로 붙여 넣습니다.

넘어가기 전에 다음 사항을 확인해야 합니다.

-   ProxyStub ClassId는 ToasterComponent 파일의 첫 번째 GUID로 설정 됩니다 \_ . 이 파일에서 classId로 정의된 첫 번째 GUID를 사용하세요. 이 GUID는 ITypedEventHandler2의 GUID와 동일할 수 있습니다.
-   경로는 프록시 이진 파일의 패키지 상대 경로입니다. 이 연습에서는 proxies.dll이 ToasterApplication.winmd와 동일한 폴더에 있습니다.
-   GUID가 올바른 형식입니다. 잘못된 형식일 가능성이 높습니다.
-   매니페스트의 인터페이스 Id는 ToasterComponent 파일의 Iid와 일치 합니다 \_ .
-   매니페스트에서 인터페이스 이름은 고유합니다. 이 이름은 시스템에서 사용되지 않으므로 사용자가 값을 선택할 수 있습니다. 정의한 인터페이스와 확실하게 일치하는 인터페이스 이름을 선택하는 것이 좋습니다. 생성된 인터페이스의 이름은 생성된 인터페이스를 나타내야 합니다. ToasterComponent 파일을 사용 하 여 \_ 인터페이스 이름을 생성 하는 데 도움이 됩니다.

지금 솔루션을 실행하려고 시도하면 proxies.dll이 페이로드의 일부가 아니라는 오류가 발생합니다. ToasterApplication 프로젝트의 **참조** 폴더에 대 한 바로 가기 메뉴를 열고 **참조 추가**를 선택 합니다. Proxies 프로젝트 옆의 확인란을 선택합니다. ToasterComponent 옆의 확인란도 선택되어 있는지 확인합니다. **확인** 단추를 선택합니다.

이제 프로젝트가 빌드되어야 합니다. 프로젝트를 실행하고 알림을 만들 수 있는지 확인합니다.

## <a name="related-topics"></a>관련 항목

* [C++/CX가 포함된 Windows 런타임 구성 요소](creating-windows-runtime-components-in-cpp.md)
