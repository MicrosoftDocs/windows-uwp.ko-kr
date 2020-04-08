---
title: WPF에서 시각적 계층 사용
description: 고급 애니메이션 및 효과를 만드는 데 기존 WPF 콘텐츠와 함께 시각적 계층 API를 사용하기 위한 기술을 알아봅니다.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215151"
---
# <a name="using-the-visual-layer-with-wpf"></a>WPF에서 시각적 계층 사용

WPF(Windows Presentation Foundation) 앱에서 Windows 런타임 컴퍼지션 API([시각적 계층](/windows/uwp/composition/visual-layer)이라고도 함)를 사용하여 Windows 10 사용자를 위한 최신 환경을 만들 수 있습니다.

이 자습서의 전체 코드는 다음 GitHub에서 사용할 수 있습니다. [WPF HelloComposition 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)

## <a name="prerequisites"></a>필수 구성 요소

UWP XAML 호스팅 API에는 다음과 같은 필수 구성 요소가 있습니다.

- WPF 및 UWP를 사용하여 앱 개발에 대해 잘 알고 있다고 가정합니다. 자세한 내용은 다음의 정보를 참조하세요.
  - [시작(WPF)](/dotnet/framework/wpf/getting-started/).
  - [Windows 10 앱 시작](/windows/uwp/get-started/)
  - [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 이상
- Windows 10 버전 1803 이상
- Windows 10 SDK 17134 이상

## <a name="how-to-use-composition-apis-in-wpf"></a>WPF에서 컴퍼지션 API를 사용하는 방법

이 자습서에서는 간단한 WPF 앱 UI를 만들고 애니메이션 효과를 주는 컴퍼지션 요소를 추가합니다. WPF 및 컴퍼지션 구성 요소는 모두 간단하게 유지되지만 표시되는 interop 코드는 구성 요소의 복잡성에 관계없이 동일합니다. 완성된 앱은 다음과 같습니다.

![실행 중인 앱 UI](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>WPF 프로젝트 만들기

첫 번째 단계는 UI에 대한 애플리케이션 정의 및 XAML 페이지를 포함하는 WPF 앱 프로젝트를 만드는 것입니다.

_HelloComposition_이라는 새 WPF 애플리케이션 프로젝트를 Visual C#에서 만들려면 다음을 수행합니다.

1. Visual Studio를 열고 **파일** > **새로 만들기** > **프로젝트**를 선택합니다.

    **새 프로젝트** 대화 상자가 열립니다.
1. **설치됨** 범주 아래에서 **Visual C#** 노드를 확장하고 **Windows 데스크톱**을 선택합니다.
1. **WPF 앱(.NET Framework)** 템플릿을 선택합니다.
1. 이름 _HelloComposition_을 입력하고 프레임워크 **.NET Framework 4.7.2**를 선택한 후 **확인**을 클릭합니다.

    Visual Studio에서 프로젝트를 만들고 Mainwindow.xaml이라는 기본 애플리케이션 창에 대한 디자이너를 엽니다.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows 런타임 API를 사용하도록 프로젝트 구성

WPF 앱에서 WinRT(Windows 런타임) API를 사용하려면 Windows 런타임에 액세스하도록 Visual Studio 프로젝트를 구성해야 합니다. 또한 벡터는 컴퍼지션 API에서 광범위하게 사용되므로 벡터를 사용하는 데 필요한 참조를 추가해야 합니다.

NuGet 패키지는 이러한 두 가지 요구를 해결하는 데 사용할 수 있습니다. 이러한 패키지의 최신 버전을 설치하여 필요한 참조를 프로젝트에 추가합니다.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts)(기본 패키지 관리 형식을 PackageReference로 설정해야 함)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> NuGet 패키지를 사용하여 프로젝트를 구성하는 것이 좋지만 필요한 참조를 수동으로 추가할 수 있습니다. 자세한 내용은 [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)을 참조하세요. 다음 표에서는 참조를 추가하는 데 필요한 파일을 보여 줍니다.

|파일|위치|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>모니터당 DPI 인식으로 프로젝트 구성

앱에 추가하는 시각적 계층 콘텐츠는 표시되는 화면의 DPI 설정과 일치하도록 자동으로 크기가 조정되지 않습니다. 앱에 대해 모니터당 DPI 인식을 사용하도록 설정한 다음, 시각적 계층 콘텐츠를 만드는 데 사용하는 코드가 앱을 실행할 때 현재 DPI 크기를 고려하는지 확인해야 합니다. 여기서는 DPI를 인식하도록 프로젝트를 구성합니다. 이후 섹션에서는 DPI 정보를 사용하여 시각적 계층 콘텐츠의 크기를 조정하는 방법을 보여 줍니다.

WPF 앱은 기본적으로 시스템 DPI를 인식하지만, app.manifest 파일에서 자체적으로 모니터별 DPI 인식으로 선언해야 합니다. 앱 매니페스트 파일에서 Windows 수준 모니터별 DPI 인식 기능을 설정하려면 다음을 수행합니다.

1. **솔루션 탐색기**에서 _HelloComposition_ 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
1. 상황에 맞는 메뉴에서 **추가** > **새 항목...** 을 선택합니다.
1. **새 항목 추가** 대화 상자에서 '애플리케이션 매니페스트 파일'을 선택한 다음, **추가**를 클릭합니다. (기본 이름을 그대로 둘 수 있습니다.)
1. app.manifest 파일에서 이 xml을 찾고 주석 처리를 제거합니다.

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. 여는 `<windowsSettings>` 태그 뒤에 이 설정을 추가합니다.

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. 또한 App.config 파일에서 **DoNotScaleForDpiChanges** 설정을 지정해야 합니다.

    App.Config를 열고 `<configuration>` 요소 내에 이 xml을 추가합니다.

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides**는 한 번만 설정할 수 있습니다. 애플리케이션에서 이미 한번 설정되었으면 값 특성 내에서 이 스위치를 세미콜론으로 구분해야 합니다.

(자세한 내용은 GitHub의 [모니터별 DPI 개발자 가이드 및 샘플](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI)을 참조하세요.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>HwndHost 파생 클래스를 만들어 컴퍼지션 요소 호스트

만든 콘텐츠를 시각적 계층으로 호스트하려면 [HwndHost](/dotnet/api/system.windows.interop.hwndhost)에서 파생되는 클래스를 만들어야 합니다. 여기서 컴퍼지션 API 호스트를 위한 대부분의 구성을 수행합니다. 이 클래스에서 PInvoke([플랫폼 호출 서비스](/cpp/dotnet/calling-native-functions-from-managed-code)) 및 를 [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute)을 사용하여 컴퍼지션 API를 WPF 앱으로 가져옵니다. PInvoke 및 COM Interop에 대한 자세한 내용은 [비관리 코드와의 상호 운용](/dotnet/framework/interop/index)을 참조하세요.

> [!TIP]
> 필요한 경우 자습서의 끝 부분에 있는 전체 코드를 확인하여 자습서의 작업을 진행할 때 모든 코드가 올바른 위치에 있도록 합니다.

1. [HwndHost](/dotnet/api/system.windows.interop.hwndhost)에서 파생되는 프로젝트에 새 클래스 파일을 추가합니다.
    - **솔루션 탐색기**에서 _HelloComposition_ 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
    - 상황에 맞는 메뉴에서 **추가** > **클래스...** 를 선택합니다.
    - **새 항목 추가** 대화 상자에서 클래스 이름을 _CompositionHost.cs_로 지정한 다음, **추가**를 클릭합니다.
1. CompositionHost.cs에서 **HwndHost**에서 파생되도록 클래스 정의를 편집합니다.

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. 클래스에 다음 코드 및 생성자를 추가합니다.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. **BuildWindowCore** 및 **DestroyWindowCore** 메서드를 재정의합니다.
    > [!NOTE]
    > BuildWindowCore에서 _InitializeCoreDispatcher_ 및 _InitComposition_ 메서드를 호출합니다. 다음 단계에서 이러한 메서드를 만듭니다.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) 및 [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow)에는 PInvoke 선언이 필요합니다. 이 클래스의 경우 코드의 끝에 이 선언을 삽입합니다.

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)를 사용하여 스레드를 초기화합니다. 코어 디스패처는 WinRT API에 대한 창 메시지를 처리하고 이벤트를 디스패치합니다. CoreDispatcher가 있는 스레드에서 **CoreDispatcher**의 새 인스턴스를 만들어야 합니다.
    - _InitializeCoreDispatcher_라는 메서드를 만들고 디스패처 큐를 설정하는 코드를 추가합니다.

    ```csharp
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

    - 디스패처 큐에도 PInvoke 선언이 필요합니다. 이전 단계에서 만든 _PInvoke 선언_ 영역 내에 이 선언을 배치합니다.

    ```csharp
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
    ```

    이제 디스패처 큐가 준비되었으며 컴퍼지션 콘텐츠 초기화 및 만들기를 시작할 수 있습니다.

1. [Compositor](/uwp/api/windows.ui.composition.compositor)를 초기화합니다. Compositor는 시각적 개체, 효과 시스템 및 애니메이션 시스템에 적용되는 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 네임스페이스에서 다양한 형식을 만드는 팩터리입니다. Compositor 클래스는 팩터리에서 만든 개체의 수명도 관리합니다.

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
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** 및 **ICompositionTarget**에는 COM 가져오기가 필요합니다. _CompositionHost_ 클래스 뒤, 네임스페이스 선언 내에 이 코드를 배치합니다.

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

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>WPF 시각적 트리에 콘텐츠를 추가하는 UserControl 만들기

컴퍼지션 콘텐츠를 호스트하는 데 필요한 인프라를 설정하는 마지막 단계는 WPF 시각적 트리에 HwndHost를 추가하는 것입니다.

### <a name="create-a-usercontrol"></a>UserControl 만들기

UserControl은 컴퍼지션 콘텐츠를 만들고 관리하는 코드를 패키지하고 XAML에 콘텐츠를 쉽게 추가할 수 있는 편리한 방법입니다.

1. 프로젝트에 새 사용자 정의 컨트롤 파일을 추가합니다.
    - **솔루션 탐색기**에서 _HelloComposition_ 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
    - 상황에 맞는 메뉴에서 **추가** > **사용자 정의 컨트롤...** 을 선택합니다.
    - **새 항목 추가** 대화 상자에서 사용자 정의 컨트롤의 이름을 _CompositionHostControl.xaml_로 지정하고 **추가**를 클릭합니다.

    CompositionHostControl.xaml 및 CompositionHostControl.xaml.cs 파일이 생성되어 프로젝트에 추가됩니다.
1. CompositionHostControl.xaml에서 `<Grid> </Grid>` 태그를 HwndHost가 이동할 XAML 컨테이너인이 **Border** 요소로 바꿉니다.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

사용자 정의 컨트롤에 대한 코드에서는 이전 단계에서 만든 CompositionHost 클래스의 인스턴스를 만든 후 XAML 페이지에서 만든 Border인 _CompositionHostElement_의 자식 요소로 추가합니다.

1. CompositionHostControl.xaml.cs에서 컴퍼지션 코드에 사용할 개체에 대한 프라이빗 변수를 추가합니다. 클래스 정의 뒤에 이러한 변수를 추가합니다.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. 사용자 정의 컨트롤의 **Loaded** 이벤트에 대한 처리기를 추가합니다. 여기에서 CompositionHost 인스턴스를 설정합니다.

    - 생성자에서 여기에 표시된 대로 이벤트 처리기를 연결합니다(`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - 이름이 *CompositionHostControl_Loaded*인 이벤트 처리기 메서드를 추가합니다.
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    이 메서드에서 컴퍼지션 코드에 사용할 개체를 설정합니다. 진행되는 과정은 다음과 같습니다.

    - 먼저 CompositionHost 인스턴스가 이미 있는지 여부를 확인하여 설정을 한번만 수행하도록 합니다.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - 현재 DPI를 가져옵니다. 컴퍼지션 요소의 크기를 적절히 조정하는 데 사용됩니다.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - CompositionHost의 인스턴스를 만들고 Border _CompositionHostElement_의 자식으로 할당합니다.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - CompositionHost에서 Compositor를 가져옵니다.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Compositor를 사용하여 컨테이너 시각적 개체를 만듭니다. 컴퍼지션 요소를 추가하는 컴퍼지션 컨테이너입니다.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>컴퍼지션 요소 추가

인프라가 준비되었으므로 이제 표시하려는 컴퍼지션 콘텐츠를 생성할 수 있습니다.

이 예제에서는 간단한 사각형 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)을 만들고 애니메이션 효과를 주는 코드를 추가합니다.

1. 컴퍼지션 요소를 추가합니다. CompositionHostControl.xaml.cs에서 CompositionHostControl 클래스에 다음 메서드를 추가합니다.

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
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

### <a name="handle-dpi-changes"></a>DPI 변경 내용 처리

요소를 추가하고 애니메이션 효과를 주는 코드는 요소를 만들 때의 현재 DPI 크기를 고려하지만, 앱이 실행되는 동안의 DPI 변경 내용도 고려해야 합니다. [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) 이벤트를 처리하여 변경에 대한 알림을 받고 새 DPI를 기준으로 계산을 조정할 수 있습니다.

1. CompositionHostControl_Loaded 메서드에서 마지막 줄 뒤에 이 이벤트를 추가하여 DpiChanged 이벤트 처리기를 연결합니다.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. 이름이 _CompositionHostDpiChanged_인 이벤트 처리기 메서드를 추가합니다. 이 코드는 각 요소의 크기와 오프셋을 조정하고 완전하지 않은 애니메이션을 다시 계산합니다.

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>XAML 페이지에 사용자 정의 컨트롤 추가

이제 XAML UI에 사용자 정의 컨트롤을 추가할 수 있습니다.

1. MainWindow.xaml에서 창 높이를 600으로 설정하고 너비를 840으로 설정합니다.
1. UI에 대한 XAML을 추가합니다. MainWindow.xaml에서 루트 `<Grid> </Grid>` 태그 사이에 이 XAML을 추가합니다.

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. 단추 클릭을 처리하여 새 요소를 만듭니다. (Click 이벤트는 XAML에 이미 연결되어 있습니다.)

    MainWindow.xaml.cs에서 이 *Button_Click* 이벤트 처리기 메서드에 추가합니다. 이 코드는 _CompositionHost.AddElement_를 호출하여 임의로 생성된 크기와 오프셋으로 새 요소를 만듭니다.

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

이제 WPF 앱을 빌드하고 실행할 수 있습니다. 필요한 경우 자습서의 끝 부분에 있는 전체 코드를 확인하여 모든 코드가 올바른 위치에 있도록 합니다.

앱을 실행하고 단추를 클릭하면 UI에 애니메이션 효과가 있는 사각형이 추가된 것을 볼 수 있습니다.

## <a name="next-steps"></a>다음 단계

동일한 인프라를 기준으로 하는 전체 예제는 GitHub의 [WPF 시각적 계층 통합 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)을 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [시작(WPF)](/dotnet/framework/wpf/getting-started/)(.NET)
- [비관리 코드와의 상호 운용](/dotnet/framework/interop/)(.NET)
- [Windows 10 앱 시작](/windows/uwp/get-started/)(UWP)
- [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)(UWP)
- [Windows.UI.Composition 네임스페이스](/uwp/api/windows.ui.composition)(UWP)

## <a name="complete-code"></a>전체 코드

다음은 이 자습서의 전체 코드입니다.

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
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
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
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

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
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


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


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