---
Windows 런타임 구성 요소에서 이벤트 발생
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 
---

# Windows 런타임 구성 요소에서 이벤트 발생


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

Windows 런타임 구성 요소가 백그라운드 스레드(작업자 스레드)에서 사용자 정의 대리자 형식의 이벤트를 발생시키며 JavaScript가 이벤트를 받을 수 있게 하려는 경우 다음 방법 중 하나로 구현 및/또는 발생시킬 수 있습니다.

-   (옵션 1) [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx)를 통해 이벤트를 발생시켜 이벤트를 JavaScript 스레드 컨텍스트로 마샬링합니다. 일반적으로 이것이 최상의 옵션이지만 일부 시나리오에서는 가장 빠른 성능을 제공하지 않을 수도 있습니다.
-   (옵션 2) [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt;를 사용하지만 형식 정보가 손실됩니다(이벤트 유형 정보가 손실됨). 옵션 1이 불가능하거나 성능이 부적절한 경우 형식 정보가 손실되어도 되면 두 번째 선택 항목으로 사용하기에 적합합니다.
-   (옵션 3) 구성 요소에 대한 고유한 프록시 및 스텁을 만듭니다. 이 옵션은 구현하기 가장 어렵지만 형식 정보가 유지되며 까다로운 시나리오에서 옵션 1에 비해 더 나은 성능을 제공할 수 있습니다.

이러한 옵션 중 하나를 사용하지 않고 백그라운드 스레드에서 이벤트를 발생시킬 경우 JavaScript 클라이언트에 이벤트가 수신되지 않습니다.

## 배경


모든 Windows 런타임 구성 요소 및 앱은 어떤 언어로 만들든지 관계없이 기본적으로 COM 개체입니다. Windows API에서는 대부분의 구성 요소가 백그라운드 스레드 및 UI 스레드에서 개체와 똑같이 잘 통신할 수 있는 Agile COM 개체입니다. COM 개체를 Agile로 설정할 수 없는 경우 UI 스레드-백그라운드 스레드 경계를 넘어 다른 COM 개체와 통신하려면 프록시 및 스텁으로 알려진 도우미 개체가 필요합니다. COM 용어로 이를 스레드 아파트 간 통신이라고 합니다.

Windows API의 개체는 대부분 Agile이거나 프록시 및 스텁이 기본 제공되어 있습니다. 그러나 Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) 같은 제네릭 형식의 경우 형식 인수를 제공할 때까지 완전한 형식이 아니기 때문에 해당 프록시 및 스텁을 만들 수 없습니다. 프록시 또는 스텁이 없을 경우 문제가 되는 것은 JavaScript 클라이언트뿐이지만 C++ 또는 .NET 언어뿐 아니라 JavaScript에서 구성 요소를 사용할 수 있게 하려면 다음 세 가지 옵션 중 하나를 사용해야 합니다.

## (옵션 1) CoreDispatcher를 통해 이벤트 발생


[Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx)를 사용하여 사용자 정의 대리자 형식의 이벤트를 보낼 수 있으며, JavaScript가 받을 수 있습니다. 어떤 옵션을 사용할지 잘 모르겠으면 이 옵션을 먼저 시도해 보세요. 이벤트 발생과 이벤트 처리 간의 대기 시간이 문제가 되는 경우 다른 옵션 중 하나를 시도합니다.

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

## (옵션 2) EventHandler&lt;Object&gt;를 사용하지만 형식 정보 손실


백그라운드 스레드에서 이벤트를 전송하는 또 다른 방법은 [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt;를 이벤트 유형으로 사용하는 것입니다. Windows는 제네릭 형식의 구체적인 인스턴스화와 해당 프록시 및 스텁을 제공합니다. 단점은 이벤트 인수의 형식 정보와 보낸 사람이 손실된다는 것입니다. C++ 및 .NET 클라이언트는 설명서를 통해 이벤트가 수신될 때 다시 캐스팅할 형식을 알고 있어야 합니다. JavaScript 클라이언트는 원래 형식 정보가 필요하지 않습니다. 메타데이터의 해당 이름에 따라 인수 속성을 찾습니다.

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

## (옵션 3) 고유한 프록시 및 스텁 만들기


완전하게 보존된 형식 정보가 있는 사용자 정의 이벤트 유형에서 성능을 향상시키려면 고유한 프록시 및 스텁 개체를 만들고 앱 패키지에 포함해야 합니다. 일반적으로 다른 두 옵션이 모두 부적절한 드문 경우에만 이 옵션을 사용해야 합니다. 또한 이 옵션이 다른 두 옵션보다 더 나은 성능을 제공한다는 보장은 없습니다. 실제 성능은 여러 요인에 따라 달라집니다. Visual Studio 프로파일러 또는 다른 프로파일링 도구를 사용하여 응용 프로그램의 실제 성능을 측정하고 이벤트가 사실상 병목인지 여부를 확인할 수 있습니다.

이 문서의 나머지 부분에서는 C#을 사용하여 Windows 런타임 구성 요소를 만든 후 C++를 사용하여 비동기 작업에서 구성 요소에 의해 발생한 Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; 이벤트를 JavaScript가 사용할 수 있도록 하는 프록시 및 스텁에 대한 DLL을 만드는 방법을 보여 줍니다. C++ 또는 Visual Basic을 사용하여 구성 요소를 만들 수도 있습니다. 프록시와 스텁을 만드는 단계는 동일합니다. 이 연습은 Windows 런타임 In Process 구성 요소 샘플(C++/CX) 만들기를 기반으로 하며 해당 용도를 설명하는 데 도움이 됩니다.

이 연습은 다음 부분으로 이루어져 있습니다.

-   여기서는 두 가지 기본 Windows 런타임 클래스를 만듭니다. 한 클래스는 [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) 형식의 이벤트를 표시하고 다른 클래스는 JavaScript에 TValue의 인수로 반환되는 형식입니다. 두 클래스는 이후 단계를 완료할 때까지 JavaScript와 통신할 수 없습니다.
-   이 앱은 기본 클래스 개체를 활성화하고, 메서드를 호출한 다음 Windows 런타임 구성 요소에 의해 발생한 이벤트를 처리합니다.
-   이러한 작업은 프록시 및 스텁 클래스를 생성하는 도구에 필요합니다.
-   그런 다음 IDL 파일을 사용하여 프록시 및 스텁에 대한 C 소스 코드를 생성합니다.
-   COM 런타임이 찾을 수 있도록 프록시-스텁 개체를 등록하고 앱 프로젝트에서 프록시-스텁 DLL을 참조합니다.

## Windows 런타임 구성 요소를 만들려면

1.  Visual Studio의 메뉴 모음에서 **파일 &gt; 새 프로젝트**를 선택합니다. **새 프로젝트** 대화 상자에서 **JavaScript &gt; 유니버설 Windows**를 확장하고 **빈 앱**을 선택합니다. 프로젝트 이름을 ToasterApplication으로 지정하고 **확인** 단추를 선택합니다.
2.  C# Windows 런타임 구성 요소를 솔루션에 추가합니다. 솔루션 탐색기에서 솔루션의 바로 가기 메뉴를 열고 **추가 &gt; 새 프로젝트**를 선택합니다. **Visual C# &gt; Windows 스토어** 를 확장하고 **Windows 런타임 구성 요소**를 선택합니다. 프로젝트 이름을 ToasterComponent로 지정하고 **확인** 단추를 선택합니다. ToasterComponent는 이후 단계에서 만들 구성 요소의 루트 네임스페이스가 됩니다.

    솔루션 탐색기에서 솔루션의 바로 가기 메뉴를 열고 **속성**을 선택합니다. **속성 페이지** 대화 상자의 왼쪽 창에서 **구성 속성**을 선택하고 대화 상자 맨 위에서 **구성**을 **디버그**로, **플랫폼**을 x86, x64 또는 ARM으로 설정합니다. **확인** 단추를 선택합니다.

    > **중요** 플랫폼 = 나중에 솔루션에 추가할 네이티브 코드 Win32 DLL에 유효하지 않으므로 모든 CPU는 작동하지 않습니다.

3.  솔루션 탐색기에서 class1.cs의 이름을 프로젝트 이름과 일치하도록 ToasterComponent.cs로 바꿉니다. Visual Studio에서 파일의 클래스 이름을 새 파일 이름과 일치하도록 자동으로 바꿉니다.
4.  .cs 파일에서 Windows.Foundation 네임스페이스에 대한 using 지시문을 추가하여 TypedEventHandler를 범위로 가져옵니다.
5.  프록시 및 스텁이 필요한 경우 구성 요소에서 인터페이스를 사용하여 공용 멤버를 표시해야 합니다. ToasterComponent.cs에서 toaster에 대한 인터페이스와 toaster가 생성하는 알림에 대한 인터페이스를 정의합니다.

    > **참고** C#에서는 이 단계를 건너뛸 수 있습니다. 대신, 먼저 클래스를 만든 후 해당 바로 가기 메뉴를 열고 **리팩터링 &gt; 인터페이스 추출**을 선택합니다. 생성된 코드에서 수동으로 인터페이스에 공개 접근성을 제공합니다.

    ```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler&lt;Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
    ```

    IToast 인터페이스에는 알림 유형을 설명하기 위해 검색할 수 있는 문자열이 있습니다. IToaster 인터페이스에는 알림을 만들 메서드 및 알림이 만들어졌음을 나타내는 이벤트가 있습니다. 이 이벤트는 알림의 특정 부분(즉, 형식)을 반환하므로 형식화된 이벤트라고 합니다.

6.  다음에는 이러한 인터페이스를 구현하며 나중에 프로그래밍할 JavaScript 앱에서 액세스할 수 있도록 public 및 sealed인 클래스가 필요합니다.

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
            public event TypedEventHandler&lt;Toaster, Toast> ToastCompletedEvent;

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

    > **참고** 앞의 코드에서 비동기 호출은 ThreadPool.RunAsync만 사용하여 백그라운드 스레드에서 이벤트를 발생시키는 간단한 방법을 보여 줍니다. 다음 예제와 같이 이 특정 메서드를 작성할 수 있으며, .NET 작업 스케줄러에서 자동으로 async/await 호출을 UI 스레드로 다시 마샬링하기 때문에 제대로 작동합니다.
  
    ````csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
    ```

    If you build the project now, it should build cleanly.

## To program the JavaScript app

1.  Now we can add a button to the JavaScript app to cause it to use the class we just defined to make toast. Before we do this, we must add a reference to the ToasterComponent project we just created. In Solution Explorer, open the shortcut menu for the ToasterApplication project, choose **Add &gt; References**, and then choose the **Add New Reference** button. In the Add Reference dialog box, in the left pane under Solution, select the component project, and then in the middle pane, select ToasterComponent. Choose the **OK** button.
2.  In Solution Explorer, open the shortcut menu for the ToasterApplication project and then choose **Set as Startup Project**.
3.  At the end of the default.js file, add a namespace to contain the functions to call the component and be called back by it. The namespace will have two functions, one to make toast and one to handle the toast-complete event. The implementation of makeToast creates a Toaster object, registers the event handler, and makes the toast. So far, the event handler doesn’t do much, as shown here:

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

4.  The makeToast function must be hooked up to a button. Update default.html to include a button and some space to output the result of making toast:

    ```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
    ```

    If we weren’t using a TypedEventHandler, we would now be able to run the app on the local machine and click the button to make toast. But in our app, nothing happens. To find out why, let’s debug the managed code that fires the ToastCompletedEvent. Stop the project, and then on the menu bar, choose **Debug &gt; Toaster Application properties**. Change **Debugger Type** to **Managed Only**. Again on the menu bar, choose **Debug &gt; Exceptions**, and then select **Common Language Runtime Exceptions**.

    Now run the app and click the make-toast button. The debugger catches an invalid cast exception. Although it’s not obvious from its message, this exception is occurring because proxies are missing for that interface.

    ![missing proxy](./images/debuggererrormissingproxy.png)

    The first step in creating a proxy and stub for a component is to add a unique ID or GUID to the interfaces. However, the GUID format to use differs depending on whether you're coding in C#, Visual Basic, or another .NET language, or in C++.

## To generate GUIDs for the component's interfaces (C# and other .NET languages)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 5. \[Guid(“xxxxxxxx-xxxx...xxxx)\]. Choose the New GUID button and then choose the Copy button.

    ![guid generator tool](./images/guidgeneratortool.png)

2.  Go back to the interface definition, and then paste the new GUID just before the IToaster interface, as shown in the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp
    [Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
    ```

3.  Add a using directive for the System.Runtime.InteropServices namespace.
4.  Repeat these steps for the IToast interface.

## To generate GUIDs for the component's interfaces (C++)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 3. static const struct GUID = {...}. Choose the New GUID button and then choose the Copy button.

2.  Paste the GUID just before the IToaster interface definition. After you paste, the GUID should resemble the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp 
      <Extensions> <!—Use your own GUIDs!!!-->
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

    Paste the Extensions XML node as a direct child of the Package node, and a peer of, for example, the Resources node.

2.  Before moving on, it’s important to ensure that:

    -   The ProxyStub ClassId is set to the first GUID in the ToasterComponent\_i.c file. Use the first GUID that's defined in this file for the classId. (This might be the same as the GUID for ITypedEventHandler2.)
    -   The Path is the package relative path of the proxy binary. (In this walkthrough, proxies.dll is in the same folder as ToasterApplication.winmd.)
    -   The GUIDs are in the correct format. (This is easy to get wrong.)
    -   The interface IDs in the manifest match the IIDs in ToasterComponent\_i.c file.
    -   The interface names are unique in the manifest. Because these are not used by the system, you can choose the values. It is a good practice to choose interface names that clearly match interfaces that you have defined. For generated interfaces, the names should be indicative of the generated interfaces. You can use the ToasterComponent\_i.c file to help you generate interface names.

3.  If you try to run the solution now, you will get an error that proxies.dll is not part of the payload. Open the shortcut menu for the **References** folder in the ToasterApplication project and then choose **Add Reference**. Select the check box next to the Proxies project. Also, make sure that the check box next to ToasterComponent is also selected. Choose the **OK** button.

    The project should now build. Run the project and verify that you can make toast.

## Related topics

* [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=Mar16_HO1-->


