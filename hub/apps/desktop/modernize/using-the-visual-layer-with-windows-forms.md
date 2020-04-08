---
title: Windows Forms에서 시각적 계층 사용
description: 기존 Windows Forms 콘텐츠와 함께 시각적 계층 API를 사용하여 고급 애니메이션 및 효과를 만드는 기술을 알아봅니다.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999636"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Windows Forms에서 시각적 계층 사용

Windows Forms 앱에서 Windows 런타임 컴퍼지션 API([시각적 계층](/windows/uwp/composition/visual-layer)이라고도 함)를 사용하여 Windows 10 사용자를 위한 최신 환경을 만들 수 있습니다.

이 자습서의 전체 코드는 GitHub: [Windows Forms HelloComposition 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)에서 확인할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

UWP 호스팅 API에는 다음과 같은 필수 조건이 있습니다.

- 사용자가 Windows Forms 및 UWP를 사용한 앱 개발 경험이 어느 정도 있다고 가정합니다. 자세한 내용은 다음의 정보를 참조하세요.
  - [Windows Forms 시작](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Windows 10 앱 시작](/windows/uwp/get-started/)
  - [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 이상
- Windows 10, 버전 1803 이상
- Windows 10 SDK 17134 이상

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Windows Forms에서 컴퍼지션 API를 사용하는 방법

이 자습서에서는 간단한 Windows Forms UI를 만들고 애니메이션 컴퍼지션 요소를 UI에 추가합니다. Windows Forms 및 컴퍼지션 구성 요소는 모두 단순하게 유지되지만, 표시되는 interop 코드는 구성 요소의 복잡성에 관계없이 동일합니다. 완성된 앱은 다음과 같이 표시됩니다.

![실행 중인 앱 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Windows Forms 프로젝트 만들기

첫 번째 단계는 UI의 애플리케이션 정의와 기본 양식을 포함하는 Windows Forms 앱 프로젝트를 만드는 것입니다.

_HelloComposition_이라는 새 Windows Forms 애플리케이션 프로젝트를 Visual C#으로 만들려면 다음을 수행합니다.

1. Visual Studio를 열고 **파일** > **새로 만들기** > **프로젝트**를 선택합니다.<br/>**새 프로젝트** 대화 상자가 열립니다.
1. **설치됨** 범주에서 **Visual C#** 노드를 펼치고 **Windows 데스크톱**을 선택합니다.
1. **Windows Forms 앱(.NET Framework)** 템플릿을 선택합니다.
1. 이름으로 _HelloComposition_을 입력하고 프레임워크로 **.NET Framework 4.7.2**를 선택한 다음, **확인**을 클릭합니다.

Visual Studio에서 프로젝트를 만들고 Form1.cs라는 기본 애플리케이션 창의 디자이너를 엽니다.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows 런타임 API를 사용하도록 프로젝트 구성

Windows Forms 앱에서 WinRT(Windows 런타임) API를 사용하려면 Windows 런타임에 액세스하도록 Visual Studio 프로젝트를 구성해야 합니다. 또한 컴퍼지션 API에서 벡터가 광범위하게 사용되므로 벡터를 사용하는 데 필요한 참조를 추가해야 합니다.

NuGet 패키지를 사용하면 두 가지 요구 사항을 모두 충족할 수 있습니다. 해당 패키지의 최신 버전을 설치하여 필요한 참조를 프로젝트에 추가합니다.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts)(기본 패키지 관리 형식을 PackageReference로 설정해야 함)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> NuGet 패키지를 사용하여 프로젝트를 구성하는 것이 좋지만 필요한 참조를 수동으로 추가할 수도 있습니다. 자세한 내용은 [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)을 참조하세요. 다음 표에서는 참조를 추가하는 데 필요한 파일을 보여 줍니다.

|파일|위치|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>interop를 관리할 사용자 지정 컨트롤 만들기

시각적 계층으로 만든 콘텐츠를 호스트하려면 [Control](/dotnet/api/system.windows.forms.control)에서 파생된 사용자 지정 컨트롤을 만듭니다. 이 컨트롤을 통해 시각적 계층 콘텐츠의 컨테이너를 만드는 데 필요한 창 [Handle](/dotnet/api/system.windows.forms.control.handle)에 액세스할 수 있습니다.

여기서 컴퍼지션 API 호스트에 필요한 구성을 대부분 수행합니다. 이 컨트롤에서 [PInvoke(플랫폼 호출 서비스)](/cpp/dotnet/calling-native-functions-from-managed-code) 및 [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute)를 사용하여 컴퍼지션 API를 Windows Forms 앱으로 가져옵니다. PInvoke 및 COM Interop에 대한 자세한 내용은 [비관리 코드와의 상호 운용](/dotnet/framework/interop/index)을 참조하세요.

> [!TIP]
> 필요한 경우, 자습서 끝 부분에 있는 전체 코드를 확인하여 자습서의 작업을 진행할 때 모든 코드가 올바른 위치에 있도록 합니다.

1. [Control](/dotnet/api/system.windows.forms.control)에서 파생된 새 사용자 지정 컨트롤 파일을 프로젝트에 추가합니다.
    - **솔루션 탐색기**에서 _HelloComposition_ 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
    - 상황에 맞는 메뉴에서 **추가** > **새 항목...** 을 선택합니다.
    - **새 항목 추가** 대화 상자에서 **사용자 지정 컨트롤**을 선택합니다.
    - 컨트롤 이름을 _CompositionHost.cs_로 지정하고 **추가**를 클릭합니다. CompositionHost.cs가 디자인 뷰에서 열립니다.

1. CompositionHost.cs의 코드 뷰로 전환하고 다음 코드를 클래스에 추가합니다.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. 생성자에 코드를 추가합니다.

    생성자에서 _InitializeCoreDispatcher_ 및 _InitComposition_ 메서드를 호출합니다. 해당 메서드는 다음 단계에서 만듭니다.

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }
    ```

1. [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)를 사용하여 스레드를 초기화합니다. 핵심 디스패처는 WinRT API의 이벤트를 디스패치하고 창 메시지를 처리합니다. CoreDispatcher가 있는 스레드에서 **Compositor**의 새 인스턴스를 만들어야 합니다.
    - _InitializeCoreDispatcher_라는 메서드를 만들고 디스패처 큐를 설정하는 코드를 추가합니다.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - 디스패처 큐에는 PInvoke 선언이 필요합니다. 이 선언은 클래스의 코드 끝에 배치합니다. 클래스 코드를 깔끔하게 유지하려면 이 코드를 영역 안에 배치합니다.

    ```csharp
    #region PInvoke declarations

    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);

    #endregion PInvoke declarations
    ```

    이제 디스패처 큐가 준비되었으므로 컴퍼지션 콘텐츠를 초기화하고 만들 수 있습니다.

1. [Compositor](/uwp/api/windows.ui.composition.compositor)를 초기화합니다. Compositor는 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 네임스페이스에 시각적 계층, 효과 시스템 및 애니메이션 시스템을 비롯한 다양한 형식을 만드는 팩터리입니다. Compositor 클래스는 팩터리에서 만든 개체의 수명도 관리합니다.

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
    }
    ```

    - **ICompositorDesktopInterop** 및 **ICompositionTarget**에는 COM 가져오기가 필요합니다. 이 코드는 _CompositionHost_ 클래스 뒤, 네임스페이스 선언 내에 배치합니다.

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-custom-control-to-host-composition-elements"></a>컴퍼지션 요소를 호스트할 사용자 지정 컨트롤 만들기

CompositionHost에서 파생된 별도의 컨트롤에 컴퍼지션 요소를 생성하고 관리하는 코드를 배치하는 것이 좋습니다. 그러면 CompositionHost 클래스에서 만든 interop 코드를 재사용할 수 있습니다.

여기서는 CompositionHost에서 파생된 사용자 지정 컨트롤을 만듭니다. 이 컨트롤은 Visual Studio 도구 상자에 추가되므로 양식에 추가할 수 있습니다.

1. CompositionHost에서 파생된 새 사용자 지정 컨트롤 파일을 프로젝트에 추가합니다.
    - **솔루션 탐색기**에서 _HelloComposition_ 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
    - 상황에 맞는 메뉴에서 **추가** > **새 항목...** 을 선택합니다.
    - **새 항목 추가** 대화 상자에서 **사용자 지정 컨트롤**을 선택합니다.
    - 컨트롤 이름을 _CompositionHostControl.cs_로 지정하고 **추가**를 클릭합니다. CompositionHostControl.cs가 디자인 뷰에서 열립니다.

1. CompositionHostControl.cs 디자인 뷰의 속성 창에서 **BackColor** 속성을 **ControlLight**로 설정합니다.

    배경색 설정은 선택 사항입니다. 배경색을 설정하면 양식 배경에서 사용자 지정 컨트롤을 확인할 수 있습니다.

1. CompositionHostControl.cs의 코드 뷰로 전환하고 CompositionHost에서 파생되도록 클래스 선언을 업데이트합니다.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. 기본 생성자를 호출하도록 생성자를 업데이트합니다.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>컴퍼지션 요소 추가

인프라가 준비되었으므로 이제 앱 UI에 컴퍼지션 콘텐츠를 추가할 수 있습니다.

이 예제에서는 단순한 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)을 만들고 애니메이션 효과를 적용하는 코드를 CompositionHostControl 클래스에 추가합니다.

1. 컴퍼지션 요소를 추가합니다.

    CompositionHostControl.cs에서 CompositionHostControl 클래스에 다음 메서드를 추가합니다.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

## <a name="add-the-control-to-your-form"></a>양식에 컨트롤 추가

이제 컴퍼지션 콘텐츠를 호스트할 사용자 지정 컨트롤을 만들었으므로 앱 UI에 추가할 수 있습니다. 여기서는 이전 단계에서 만든 CompositionHostControl 인스턴스를 추가합니다. CompositionHostControl은 Visual Studio 도구 상자의 **_project name_ 구성 요소** 아래에 자동으로 추가됩니다.

1. Form1.CS 디자인 뷰에서 UI에 단추를 추가합니다.

    - 도구 상자의 Button을 Form1로 끌어옵니다. 양식의 왼쪽 위에 배치합니다. 컨트롤 배치를 확인하려면 자습서 시작 부분에 있는 이미지를 참조하세요.
    - 속성 창에서 **Text** 속성을 _button1_에서 _Add composition element_로 변경합니다.
    - 모든 텍스트가 표시되도록 단추 크기를 조정합니다.

    자세한 내용은 [방법: Windows Forms에 컨트롤 추가](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)를 참조하세요.

1. UI에 CompositionHostControl을 추가합니다.

    - 도구 상자의 CompositionHostControl을 Form1로 끌어옵니다. Button 오른쪽에 배치합니다.
    - 양식의 나머지 부분을 채우도록 CompositionHost 크기를 조정합니다.

1. 단추 클릭 이벤트를 처리합니다.

   - 속성 창에서 번개 볼트를 클릭하여 이벤트 뷰로 전환합니다.
   - 이벤트 목록에서 **Click** 이벤트를 선택하고 *Button_Click*을 입력한 다음, Enter 키를 누릅니다.
   - 이 코드는 Form1.cs에 추가됩니다.

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 새 요소를 만드는 코드를 단추 클릭 처리기에 추가합니다.

    - Form1.cs에서 이전에 만든 *Button_Click* 이벤트 처리기에 코드를 추가합니다. 이 코드는 _CompositionHostControl1.AddElement_를 호출하여 임의로 생성된 크기와 오프셋으로 새 요소를 만듭니다. 양식으로 끌어올 때 CompositionHostControl 인스턴스의 이름이 자동으로 _compositionHostControl1_로 지정되었습니다.

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

이제 Windows Forms 앱을 빌드하고 실행할 수 있습니다. 단추를 클릭하면 UI에 애니메이션 사각형이 추가되는 것을 확인할 수 있습니다.

## <a name="next-steps"></a>다음 단계

동일한 인프라를 기반으로 하는 전체 예제는 GitHub의 [Windows Forms 시각적 계층 통합 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)을 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Windows Forms 시작](/dotnet/framework/winforms/getting-started-with-windows-forms)(.NET)
- [비관리 코드와의 상호 운용](/dotnet/framework/interop/)(.NET)
- [Windows 10 앱 시작](/windows/uwp/get-started/)(UWP)
- [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)(UWP)
- [Windows.UI.Composition 네임스페이스](/uwp/api/windows.ui.composition)(UWP)

## <a name="complete-code"></a>전체 코드

다음은 이 자습서의 전체 코드입니다.

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}
```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                 [MarshalAs(UnmanagedType.IUnknown)]
                                        out object dispatcherQueueController);

        #endregion PInvoke declarations
    }

    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }
    #endregion COM Interop
}
```