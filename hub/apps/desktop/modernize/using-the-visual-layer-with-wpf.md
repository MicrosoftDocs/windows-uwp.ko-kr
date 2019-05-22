---
title: 시각적 계층을 사용 하 여 WPF를 사용 하 여
description: 고급 애니메이션을 만들고 결과를 조합 하 여 기존 WPF 콘텐츠를 사용 하 여 시각적 계층 API를 사용 하 여 기술에 알아봅니다.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4b1074bbc3e98df3bb7138f906053d95dcd0fd5
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985113"
---
# <a name="using-the-visual-layer-with-wpf"></a>시각적 계층을 사용 하 여 WPF를 사용 하 여

Windows 런타임 구성 Api를 사용할 수 있습니다 (라고도 합니다 [시각적 계층](/windows/uwp/composition/visual-layer))는 Windows 10 사용자에 대해 간단한 최신 환경을 만들기 위해 Windows Presentation Foundation (WPF) 앱에서.

이 자습서에 대 한 전체 코드는 GitHub에서 사용할 수 있습니다. [WPF HelloComposition 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)합니다.

## <a name="prerequisites"></a>사전 요구 사항

호스팅 API는 UWP XAML 이러한 필수 조건이 필요 합니다.

- WPF 및 UWP를 사용 하 여 앱 개발 경험이 있다고 가정 합니다. 자세한 내용은 다음의 정보를 참조하세요.
  - [시작 하기 (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Windows 10 앱 시작](/windows/uwp/get-started/)
  - [Windows 10 용 데스크톱 응용 프로그램을 향상합니다](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 이상
- Windows 10 버전 1803 이상이
- Windows 10 SDK 17134 이상

## <a name="how-to-use-composition-apis-in-wpf"></a>WPF의 컴퍼지션 Api를 사용 하는 방법

이 자습서에서는 간단한 WPF 앱 UI 만들고 애니메이션된 컴퍼지션 요소를 추가 합니다. WPF 및 컴퍼지션 구성 요소는 단순, 하지만 interop 코드 구성 요소의 복잡성에 관계 없이 동일 합니다. 완성된 된 앱을 다음과 같이 표시 됩니다.

![실행 중인 앱 UI](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>WPF 프로젝트 만들기

첫 번째 단계는 UI에 대 한 응용 프로그램 정 및 XAML 페이지를 포함 하는 WPF 앱 프로젝트를 만드는 것입니다.

시각적 개체에 새 WPF 응용 프로그램 프로젝트를 만들려면 C# 이라는 _HelloComposition_:

1. Visual Studio를 열고 선택 **파일** > **새로 만들기** > **프로젝트**합니다.

    합니다 **새 프로젝트** 대화 상자가 열립니다.
1. 아래는 **설치 됨** 범주를 확장 합니다 **Visual C#**  노드를 선택한 후 **Windows 데스크톱**.
1. 선택 된 **WPF 앱 (.NET Framework)** 템플릿.
1. 이름을 입력 _HelloComposition_, 프레임 워크 선택 **.NET Framework 4.7.2**, 클릭 **확인**합니다.

    Visual Studio 프로젝트를 만들고 MainWindow.xaml 이라는 기본 응용 프로그램 창에 대 한 디자이너를 엽니다.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows 런타임 Api를 사용 하도록 프로젝트 구성

Windows Runtime (WinRT) WPF 앱에서 Api 사용 하려면 Windows 런타임에서 액세스 하 여 Visual Studio 프로젝트를 구성 해야 합니다. 또한 벡터 널리 사용 됩니다 컴퍼지션 Api에서 벡터를 사용 하는 데 필요한 참조를 추가 해야 합니다.

NuGet 패키지는 이러한 요구 사항을 해결 하기 위해 사용할 수 있습니다. 프로젝트에 필요한 참조를 추가 하려면 이러한 패키지의 최신 버전을 설치 합니다.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (PackageReference로 기본 패키지 관리 형식 집합을 필요 합니다.)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> NuGet 패키지를 사용 하 여 프로젝트를 구성 하는 것이 좋지만, 필수 참조를 수동으로 추가할 수 있습니다. 자세한 내용은 참조 하세요. [Windows 10 용 데스크톱 응용 프로그램을 향상](/windows/uwp/porting/desktop-to-uwp-enhance)합니다. 다음 테이블에 대 한 참조를 추가 해야 하는 파일을 보여 줍니다.

|파일|위치|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>수 모니터 당 DPI 인식 하도록 프로젝트 구성

앱에 추가 하는 시각적 계층 내용에 표시 되는 화면 DPI 설정에 맞게 자동으로 확장 되지 않습니다. 모니터별 DPI 인식 앱에 대해 사용 하도록 설정 하 고 다음 사용 하 여 시각적 계층 콘텐츠를 만드는 코드는 계정에 현재 DPI 배율 앱을 실행할 때 선택 되어 있는지 확인 해야 합니다. 여기서 프로젝트 DPI를 인식를 구성 합니다. 이후 섹션에서는 시각적 계층 콘텐츠 크기를 조정 하는 DPI 정보를 사용 하는 방법을 보여 줍니다.

WPF 앱 시스템 DPI를 기본적으로 인식 되지만 하 게 될 모니터별 DPI 인식 app.manifest 파일에 선언 해야 합니다. Windows 수준 모니터별 DPI 인식 앱 매니페스트 파일에서을 켜려면:

1. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 _HelloComposition_ 프로젝트입니다.
1. 상황에 맞는 메뉴에서 선택 **추가** > **새 항목...** .
1. 에 **새 항목 추가** 대화 상자에서 ' 응용 프로그램 매니페스트 파일 '을 선택한 다음 클릭 **추가**합니다. (기본 이름을 그대로 둘 수 있습니다.)
1. App.manifest 파일에서 찾은이 xml 주석 처리를 제거 하기:

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. 연 후이 설정을 추가 `<windowsSettings>` 태그:

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. 설정 해야 합니다 **DoNotScaleForDpiChanges** App.config 파일에서 설정 합니다.

    App.Config를 열고 내에이 xml을 추가 합니다 `<configuration>` 요소:

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** 한 번만 설정할 수 있습니다. 세미콜론 해야 응용 프로그램이 이미 집합에 있으면 value 특성 내에서이 스위치를 구분 합니다.

(자세한 내용은 참조는 [모니터 당 DPI 개발자 가이드 및 샘플](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) github.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>호스트 구성 요소에 HwndHost 파생 된 클래스 만들기

시각적 계층을 사용 하 여 만든 콘텐츠 호스트를에서 파생 된 클래스를 만들어야 할 [HwndHost](/dotnet/api/system.windows.interop.hwndhost)합니다. 대부분의 호스팅 컴퍼지션 Api에 대 한 구성 수행할 수입니다. 이 클래스를 사용 하 여 [플랫폼 호출 서비스인 PInvoke](/cpp/dotnet/calling-native-functions-from-managed-code) 하 고 [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) 컴퍼지션 Api WPF 앱을 합니다. PInvoke 및 COM Interop에 대 한 자세한 내용은 참조 하세요. [비관리 코드 상호 운용](/dotnet/framework/interop/index)합니다.

> [!TIP]
> 해야 할 경우 자습서를 진행 하면서 모든 코드를 올바른 위치에 있는지 확인 하려면 자습서의 끝에서 전체 코드를 확인 합니다.

1. 파생 되는 프로젝트에 새 클래스 파일을 추가할 [HwndHost](/dotnet/api/system.windows.interop.hwndhost)합니다.
    - **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 _HelloComposition_ 프로젝트입니다.
    - 상황에 맞는 메뉴에서 선택 **추가** > **클래스...** .
    - 에 **새 항목 추가** 대화 상자에서 클래스의 이름 _CompositionHost.cs_, 클릭 **추가**합니다.
1. CompositionHost.cs에서 파생할 클래스 정의 편집할 **HwndHost**합니다.

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

1. 클래스에 다음 코드와 생성자를 추가 합니다.

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

1. 재정의 된 **BuildWindowCore** 하 고 **DestroyWindowCore** 메서드.
    > [!NOTE]
    > BuildWindowCore를에서는 호출 하는 _InitializeCoreDispatcher_ 하 고 _InitComposition_ 메서드. 다음 단계에서 이러한 메서드를 만듭니다.

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

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) 하 고 [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) PInvoke 선언이 필요 합니다. 이 선언을 클래스에 대 한 코드의 끝에 배치 합니다.

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

1. 사용 하 여 스레드를 초기화 된 [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)합니다. Core 발송자는 창 메시지를 처리 및 WinRT Api에 대 한 이벤트를 디스패치 하는 일을 담당 합니다. 새 인스턴스의 **CoreDispatcher** 는 CoreDispatcher 있는 스레드에서 만들어야 합니다.
    - 라는 메서드를 만듭니다 _InitializeCoreDispatcher_ 디스패처 큐를 설정 하는 코드를 추가 합니다.

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

    - 디스패처 큐에 PInvoke 선언을 해야합니다. 이 선언 안에 배치 합니다 _PInvoke 선언_ 이전 단계에서 만든 지역입니다.

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

    이제 디스패처 큐에서 준비 하 고 초기화 하 고 복합 콘텐츠 만들기를 시작할 수 있습니다.

1. 초기화 된 [Compositor](/uwp/api/windows.ui.composition.compositor)합니다. 작성자는 다양 한 형식에서 만드는 팩터리를 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 시각적 개체, 효과 시스템 및 애니메이션 시스템 네임 스페이스입니다. Compositor 클래스에는 또한 팩터리에서 생성 하는 개체의 수명을 관리 합니다.

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

    - **ICompositorDesktopInterop** 하 고 **ICompositionTarget** COM 가져오기 필요 합니다. 뒤에 다음이 코드를 배치 합니다 _CompositionHost_ 하지만 내부 네임 스페이스 선언 합니다.

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

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>WPF 시각적 트리에 콘텐츠를 추가 하려면 UserControl 만들기

인프라를 설정 하기 위한 마지막 단계는 콘텐츠 HwndHost WPF 시각적 트리에 추가 하는 컴퍼지션 호스트에 필요 합니다.

### <a name="create-a-usercontrol"></a>UserControl 만들기

UserControl은 만들고 컴퍼지션 콘텐츠를 관리 하는 코드 패키지 및 콘텐츠에 XAML에 쉽게 추가 하는 편리한 방법입니다.

1. 프로젝트에 새 사용자 정의 컨트롤 파일을 추가 합니다.
    - **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 _HelloComposition_ 프로젝트입니다.
    - 상황에 맞는 메뉴에서 선택 **추가** > **사용자 제어...** .
    - 에 **새 항목 추가** 대화 상자에서 이름 사용자 정의 컨트롤 _CompositionHostControl.xaml_, 클릭 **추가**합니다.

    CompositionHostControl.xaml와 CompositionHostControl.xaml.cs 파일을 만들고 프로젝트에 추가 합니다.
1. CompositionHostControl.xaml, 바꿉니다 합니다 `<Grid> </Grid>` 이 사용 하 여 태그 **테두리** 프로그램 HwndHost가 진행 하는 XAML 컨테이너 요소의.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

사용자 정의 컨트롤에 대 한 코드에서 이전 단계에서 만든 CompositionHost 클래스의 인스턴스를 만들 수 및이 자식 요소로 추가 _CompositionHostElement_, XAML 페이지에서 만든 테두리입니다.

1. CompositionHostControl.xaml.cs, 컴퍼지션 코드에서 사용 하 여 개체에 대 한 개인 변수를 추가 합니다. 클래스 정의 후에이 추가 합니다.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. 사용자 정의 컨트롤에 대 한 처리기를 추가 **Loaded** 이벤트입니다. CompositionHost 인스턴스를 설정 하는 위치입니다.

    - 생성자에서 이벤트 처리기를 여기에 나와 있는 것 처럼 후크 됩니다. (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - 이벤트 처리기 메서드 이름 추가 *CompositionHostControl_Loaded*합니다.
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

    이 방법에서는 컴퍼지션 코드에서 사용 하 여 개체를 설정 합니다. 일어나 개요는 다음과 같습니다.

    - 우선, 설정은 한 번만 수행을 확인 하 여 CompositionHost 인스턴스에 이미 있는지 여부를 확인 합니다.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - 현재 DPI를 가져옵니다. 이 컴퍼지션 요소를 적절히 조정에 사용 됩니다.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - 테두리의 자식으로 할당 하 고 CompositionHost의 인스턴스를 만듭니다 _CompositionHostElement_합니다.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - CompositionHost에서 작성자를 가져옵니다.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Compositor를 사용 하 여 시각적 컨테이너를 만듭니다. 이것이 프로그램 컴퍼지션 요소를 추가 하는 컴퍼지션 컨테이너입니다.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>컴퍼지션 요소를 추가 합니다.

준비에서 인프라를 사용 하 여 이제 표시 하려는 컴퍼지션 콘텐츠를 생성할 수 있습니다.

예를 들어 만들고 간단한 사각형에 애니메이션을 적용 하는 코드를 추가 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)합니다.

1. 컴퍼지션 요소를 추가 합니다. CompositionHostControl.xaml.cs, CompositionHostControl 클래스에 이러한 메서드를 추가 합니다.

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

### <a name="handle-dpi-changes"></a>DPI 변경 내용을 처리합니다

코드를 추가 하 고 요소에 애니메이션 효과 주기 고려 현재 DPI 배율 요소 생성 되지만 앱이 실행 되는 동안 DPI 변경을 고려해 야 하는 경우. 처리할 수 있습니다 합니다 [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) 새 DPI를 기반으로 한 변경 알림을 받을 계산을 조정 하는 이벤트입니다.

1. CompositionHostControl_Loaded 메서드에서 마지막 줄 다음 DpiChanged 이벤트 처리기를 연결 하려면이 추가 합니다.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. 이벤트 처리기 메서드 이름 추가 _CompositionHostDpiChanged_합니다. 이 코드는 확장성과 각 요소의 오프셋을 조정 하 고 완료 되지 않은 모든 애니메이션을 다시 계산 됩니다.

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

이제 XAML UI를 사용자 정의 컨트롤을 추가할 수 있습니다.

1. MainWindow.xaml에서 600으로 너비 840 창 높이 설정 합니다.
1. Ui는 XAML을 추가 합니다. MainWindow.xaml에서이 XAML 루트 간에 추가 `<Grid> </Grid>` 태그입니다.

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

1. 새 요소를 만드는 단추 클릭을 처리 합니다. (Click 이벤트가 이미 후크 되었습니다는 XAML에서.)

    MainWindow.xaml.cs에서이 추가 *Button_Click* 이벤트 처리기 메서드. 이 코드는 호출 _CompositionHost.AddElement_ 임의로 생성 된 크기와 오프셋을 사용 하 여 새 요소를 만들려고 합니다.

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

이제 작성 하 고 WPF 앱을 실행할 수 있습니다. 해야 할 경우 모든 코드를 올바른 위치 인지 확인 하려면 자습서의 끝에서 전체 코드를 확인 합니다.

앱을 실행 하 고 단추를 클릭 하는 경우 UI에 추가 하는 애니메이션된 사각형이 표시 됩니다.

## <a name="next-steps"></a>다음 단계

동일한 인프라에서 작성 하는 보다 완전 한 예제를 참조 하세요. 합니다 [계층 통합 예제를 WPF Visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) github입니다.

## <a name="additional-resources"></a>추가 자료

- [시작 하기 (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [비관리 코드와의 상호 운용](/dotnet/framework/interop/) (.NET)
- [Windows 10 앱 시작](/windows/uwp/get-started/) (UWP)
- [Windows 10 용 데스크톱 응용 프로그램을 향상](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 네임 스페이스](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>전체 코드

이 자습서에 대 한 전체 코드는 다음과 같습니다.

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