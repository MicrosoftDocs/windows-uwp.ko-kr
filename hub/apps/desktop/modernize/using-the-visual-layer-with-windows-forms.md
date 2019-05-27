---
title: 시각적 계층을 사용 하 여 Windows Forms를 사용 하 여
description: 고급 애니메이션을 만들고 결과를 조합 하 여 기존 Windows Forms 콘텐츠로 Visual 계층 Api를 사용 하 여 기술에 알아봅니다.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 23515f8254b026b255491a90c1c8b3a2a8ab12ba
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215159"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>시각적 계층을 사용 하 여 Windows Forms를 사용 하 여

Windows 런타임 구성 Api를 사용할 수 있습니다 (라고도 합니다 [시각적 계층](/windows/uwp/composition/visual-layer))는 Windows 10 사용자에 대해 간단한 최신 환경을 만들기 위해 Windows Forms 앱에서.

이 자습서에 대 한 전체 코드는 GitHub에서 사용할 수 있습니다. [Windows Forms HelloComposition 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)합니다.

## <a name="prerequisites"></a>사전 요구 사항

호스팅 API는 UWP 이러한 필수 조건이 필요 합니다.

- Windows Forms 및 UWP를 사용 하 여 앱 개발 경험이 있다고 가정 합니다. 자세한 내용은 다음의 정보를 참조하세요.
  - [Windows Forms 시작](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Windows 10 앱 시작](/windows/uwp/get-started/)
  - [Windows 10 용 데스크톱 응용 프로그램을 향상합니다](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 이상
- Windows 10 버전 1803 이상이
- Windows 10 SDK 17134 이상

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Windows Forms에서 컴퍼지션 Api를 사용 하는 방법

이 자습서에서는 간단한 Windows Forms UI를 만들고 애니메이션된 컴퍼지션 요소를 추가 합니다. Windows Forms 및 컴퍼지션 구성 요소는 단순, 하지만 interop 코드 구성 요소의 복잡성에 관계 없이 동일 합니다. 완성된 된 앱을 다음과 같이 표시 됩니다.

![실행 중인 앱 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Windows Forms 프로젝트 만들기

첫 번째 단계는 UI에 대 한 응용 프로그램 정 및 기본 폼을 포함 하는 Windows Forms 앱 프로젝트를 만드는 것입니다.

시각적 개체에 새 Windows Forms 응용 프로그램 프로젝트를 만들려면 C# 이라는 _HelloComposition_:

1. Visual Studio를 열고 선택 **파일** > **새로 만들기** > **프로젝트**합니다.<br/>합니다 **새 프로젝트** 대화 상자가 열립니다.
1. 아래는 **설치 됨** 범주를 확장 합니다 **Visual C#**  노드를 선택한 후 **Windows 데스크톱**.
1. 선택 된 **Windows Forms 앱 (.NET Framework)** 템플릿.
1. 이름을 입력 _HelloComposition 합니다_ 프레임 워크를 선택 **.NET Framework 4.7.2**, 클릭 **확인**합니다.

Visual Studio 프로젝트를 만들고 Form1.cs 이라는 기본 응용 프로그램 창에 대 한 디자이너를 엽니다.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows 런타임 Api를 사용 하도록 프로젝트 구성

Windows Runtime (WinRT) Windows Forms 앱에서 Api 사용 하려면 Windows 런타임에서 액세스 하 여 Visual Studio 프로젝트를 구성 해야 합니다. 또한 벡터 널리 사용 됩니다 컴퍼지션 Api에서 벡터를 사용 하는 데 필요한 참조를 추가 해야 합니다.

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

## <a name="create-a-custom-control-to-manage-interop"></a>Interop을 관리 하려면 사용자 지정 컨트롤 만들기

파생 된 사용자 지정 컨트롤을 호스트 시각적 계층을 사용 하 여 만든 콘텐츠를 만든 [제어](/dotnet/api/system.windows.forms.control)입니다. 이 컨트롤을 창에 액세스를 제공 [처리](/dotnet/api/system.windows.forms.control.handle)를 시각적 계층 콘텐츠에 대 한 컨테이너를 만들기 위해 필요 합니다.

대부분의 호스팅 컴퍼지션 Api에 대 한 구성 수행할 수입니다. 이 컨트롤을 사용 하 여 [플랫폼 호출 서비스인 PInvoke](/cpp/dotnet/calling-native-functions-from-managed-code) 하 고 [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) 컴퍼지션 Api Windows Forms 앱에 바인딩할 합니다. PInvoke 및 COM Interop에 대 한 자세한 내용은 참조 하세요. [비관리 코드 상호 운용](/dotnet/framework/interop/index)합니다.

> [!TIP]
> 해야 할 경우 자습서를 진행 하면서 모든 코드를 올바른 위치에 있는지 확인 하려면 자습서의 끝에서 전체 코드를 확인 합니다.

1. 파생 되는 프로젝트에 새 사용자 지정 컨트롤 파일을 추가할 [제어](/dotnet/api/system.windows.forms.control)입니다.
    - **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 _HelloComposition_ 프로젝트입니다.
    - 상황에 맞는 메뉴에서 선택 **추가** > **새 항목...** .
    - 에 **새 항목 추가** 대화 상자에서 **사용자 지정 컨트롤**합니다.
    - 컨트롤의 이름을 _CompositionHost.cs_, 클릭 **추가**합니다. CompositionHost.cs는 디자인 뷰에서 열립니다.

1. CompositionHost.cs에 대 한 코드 보기로 전환한 클래스에 다음 코드를 추가 합니다.

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

1. 생성자에 코드를 추가 합니다.

    생성자에서 호출 된 _InitializeCoreDispatcher_ 하 고 _InitComposition_ 메서드. 다음 단계에서 이러한 메서드를 만듭니다.

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

1. Initialize a thread with a [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). The core dispatcher is responsible for processing window messages and dispatching events for WinRT APIs. New instances of **Compositor** must be created on a thread that has a CoreDispatcher.
    - Create a method named _InitializeCoreDispatcher_ and add code to set up the dispatcher queue.

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

    - 디스패처 큐에는 PInvoke 선언이 필요합니다. 이 선언을 클래스에 대 한 코드의 끝에 배치 합니다. (클래스 코드를 깔끔하게 유지 하는 영역 내에서이 코드 배치할.)

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

    이제 디스패처 큐에서 준비 하 고 초기화 하 고 복합 콘텐츠 만들기를 시작할 수 있습니다.

1. 초기화 된 [Compositor](/uwp/api/windows.ui.composition.compositor)합니다. 작성자는 다양 한 형식에서 만드는 팩터리를 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 시각적 계층, 효과 시스템 및 애니메이션 시스템에 걸쳐 네임 스페이스입니다. Compositor 클래스에는 또한 팩터리에서 생성 하는 개체의 수명을 관리 합니다.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>호스트 구성 요소에 사용자 지정 컨트롤 만들기

생성 및 CompositionHost에서 파생 되는 별도 컨트롤에서 컴퍼지션 요소를 관리 하는 코드를 배치 하는 것이 좋습니다. 이 재사용 가능한 CompositionHost 클래스에서 만든 interop 코드를 유지 합니다.

여기에서 만든 CompositionHost에서 파생 된 사용자 지정 컨트롤입니다. 이 컨트롤을 양식에 추가할 수 있도록 Visual Studio 도구 상자에 추가 됩니다.

1. CompositionHost에서 파생 되는 프로젝트에 새 사용자 지정 컨트롤 파일을 추가 합니다.
    - **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 _HelloComposition_ 프로젝트입니다.
    - 상황에 맞는 메뉴에서 선택 **추가** > **새 항목...** .
    - 에 **새 항목 추가** 대화 상자에서 **사용자 지정 컨트롤**합니다.
    - 컨트롤의 이름을 _CompositionHostControl.cs_, 클릭 **추가**합니다. CompositionHostControl.cs는 디자인 뷰에서 열립니다.

1. CompositionHostControl.cs 디자인 뷰에 대 한 속성 창에서 설정 된 **BackColor** 속성을 **ControlLight**합니다.

    배경색을 설정 하는 것은 선택 사항입니다. 에서는 그렇게 폼 배경에 대 한 사용자 지정 컨트롤을 볼 수 있습니다.

1. CompositionHostControl.cs에 대 한 코드 보기로 전환한 CompositionHost에서 파생 클래스 선언을 업데이트 합니다.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. 기본 생성자를 호출 하는 생성자를 업데이트 합니다.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>컴퍼지션 요소를 추가 합니다.

준비에서 인프라와 앱 UI에 이제 컴퍼지션 콘텐츠를 추가할 수 있습니다.

예를 들어 코드 클래스를 추가 하 여 CompositionHostControl 만들고 간단한 애니메이션을 적용 하는 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)합니다.

1. 컴퍼지션 요소를 추가 합니다.

    CompositionHostControl.cs, CompositionHostControl 클래스에 이러한 메서드를 추가 합니다.

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

## <a name="add-the-control-to-your-form"></a>폼에 컨트롤 추가

컴퍼지션 콘텐츠를 호스트에 사용자 지정 컨트롤을 설정 했으므로 앱 UI 추가할 수 있습니다. 여기에서 이전 단계에서 만든 CompositionHostControl 인스턴스를 추가 합니다. CompositionHostControl에서 Visual Studio 도구 상자에 자동으로 추가 됩니다  **_프로젝트 이름_ 구성 요소**합니다.

1. Form1.CS 디자인 뷰에서 ui 단추를 추가 합니다.

    - Form1 도구 상자에서 단추를 끌어 옵니다. 폼의 왼쪽된 위 모퉁이에 배치 합니다. (컨트롤의 배치를 확인 하려면 자습서의 시작 부분에 이미지 참조).
    - 속성 창에서 변경 합니다 **텍스트** 속성을 _button1_ 에 _추가 컴퍼지션 요소_합니다.
    - 모든 텍스트가 표시 되도록 단추 크기를 조정 합니다.

    (자세한 내용은 참조 하세요. [방법: Windows Forms에 컨트롤 추가](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Ui는 CompositionHostControl를 추가 합니다.

    - Form1 도구 상자에서을 CompositionHostControl를 끕니다. 단추 오른쪽에 놓습니다.
    - 크기를 조정 합니다 CompositionHost 폼의 나머지 부분을 채웁니다.

1. 단추 핸들 이벤트를 클릭 합니다.

   - 속성 창에서 이벤트 보기로 전환 하려면 번개를 클릭 합니다.
   - 이벤트 목록에서 선택 합니다 **클릭** 이벤트를 입력 *Button_Click*, Enter 키를 누릅니다.
   - 이 코드는 Form1.cs에 추가 됩니다.

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 단추에 코드 추가 처리기 새 요소를 만들려면 클릭 합니다.

    - Form1.cs의 코드를 추가 합니다 *Button_Click* 이전에 만든 이벤트 처리기입니다. 이 코드는 호출 _CompositionHostControl1.AddElement_ 임의로 생성 된 크기와 오프셋을 사용 하 여 새 요소를 만들려고 합니다. (CompositionHostControl 인스턴스의 이름이 자동으로 _compositionHostControl1_ 형식 요구 사항에 끌어 사용자 지정 하는 것입니다.)

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

이제 작성 하 고 Windows Forms 앱을 실행할 수 있습니다. 단추를 클릭 하면 UI에 추가 하는 애니메이션된 사각형이 표시 됩니다.

## <a name="next-steps"></a>다음 단계

동일한 인프라에서 작성 하는 보다 완전 한 예제를 참조 하세요. 합니다 [Windows Forms 시각적 계층 통합 예제](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) github입니다.

## <a name="additional-resources"></a>추가 자료

- [Windows Forms 시작](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [비관리 코드와의 상호 운용](/dotnet/framework/interop/) (.NET)
- [Windows 10 앱 시작](/windows/uwp/get-started/) (UWP)
- [Windows 10 용 데스크톱 응용 프로그램을 향상](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 네임 스페이스](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>전체 코드

이 자습서에 대 한 전체 코드는 다음과 같습니다.

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