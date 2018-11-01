---
author: mcleanbyron
description: 데스크톱 응용 프로그램에 UWP XAML UI를 호스팅하는 방법을 설명 합니다.
title: 데스크톱 응용 프로그램에서 호스팅 API UWP XAML을 사용 하 여
ms.author: mcleans
ms.date: 09/21/2018
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: 2ba64e32a25feaee9245bbfe2b598c756b29df98
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919701"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>데스크톱 응용 프로그램에서 호스팅 API UWP XAML을 사용 하 여

> [!NOTE]
> UWP XAML 호스팅 API는 현재 개발자 미리 보기가 있습니다. 이 API 프로토타입 코드에 해는 것이 좋습니다, 하지만 좋지 않습니다 사용 하는 것 프로덕션 코드에서이 시점에서. 이 API는 성숙 하 고 안정화 이후 Windows 릴리스에 계속 됩니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

17709 빌드 10 내부자 미리 SDK Windows에서에서 시작, 비 UWP 데스크톱 응용 프로그램 (WPF, Windows Forms 및 Win32 c + + 응용 프로그램을 포함 하 여) 모든 UI 요소에 연결 된 창 핸들 ( *UWP XAML 호스팅 API* 호스트 UWP 컨트롤을 사용할 수 있습니다. HWND)입니다. 이 API를 사용 하면 비 UWP 에서만 UWP 컨트롤을 통해 사용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 데스크톱 응용 프로그램입니다. 예를 들어, 비 UWP 데스크톱 응용 프로그램 [Fluent 시스템 디자인을](../design/fluent-design-system/index.md) 사용 하는 [Windows 잉크](../design/input/pen-and-stylus-interactions.md)를 지 원하는 호스트 UWP 컨트롤에이 API를 사용할 수 있습니다.

UWP XAML 호스팅 API에 게 Fluent UI를 비 UWP 데스크톱 응용 프로그램 개발자가 사용할 수 있도록 제공 하 고 있는 컨트롤의 광범위 한 집합에 대 한 기반을 제공 합니다. 이 시나리오는 *XAML 제도*라고도 합니다. 이 개발자 시나리오에 대 한 자세한 내용은 [데스크톱 응용 프로그램에서 UWP 컨트롤](xaml-host-controls.md)을 참조 하십시오.

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>UWP XAML 바로 데스크톱 응용 프로그램에 대 한 API를 호스팅하는?

UWP XAML 호스팅 API 데스크톱 응용 프로그램에서 UWP 컨트롤을 호스팅하기 위한 낮은 수준의 인프라를 제공 합니다. 데스크톱 응용 프로그램의 일종이 대체 하 고 보다 편리한 Api를 사용 하 여가이 목표를 달성 하기.  

* C + + Win32 데스크톱 응용 프로그램이 있고 응용 프로그램에서 호스트 UWP 컨트롤 원하는 UWP XAML 호스팅 API를 사용 해야 합니다. 이러한 유형의 응용 프로그램에 대 한 대안이 없습니다.

* WPF 및 Windows Forms 응용 프로그램에 대 한 권장 [래핑된 컨트롤](xaml-host-controls.md#wrapped-controls) 및 [호스트 컨트롤](xaml-host-controls.md#host-controls) UWP XAML 대신 Windows 커뮤니티 Toolkit에서 사용할 호스팅 API입니다. 이러한 컨트롤 호스팅 API를 내부적으로 UWP XAML을 사용 하 고 단순한 개발 환경을 제공. 그러나 선택 하면 이러한 유형의 응용 프로그램에서 직접 API를 호스팅 UWP XAML을 사용할 수 있습니다.

## <a name="related-samples"></a>관련 샘플

UWP XAML 코드에서 호스팅 API를 사용 하 여 디자인 응용 프로그램 및 기타 요소에는 응용 프로그램 종류에 따라 다릅니다. 완전 한 응용 프로그램의 컨텍스트에서이 API를 사용 하는 방법에 설명 하려면 다음 예제에서이 문서의 코드를 참조.

### <a name="c-win32"></a>C + + Win32

UWP XAML 호스팅 API는 c + + Win32 응용 프로그램에서을 사용 하는 방법을 보여 주는 샘플 GitHub에 여러 가지가 있습니다.

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). 이 샘플에서는 c + + Win32 응용 프로그램에 [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)UWP, [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)및 [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 컨트롤을 추가 하는 방법을 보여 줍니다.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). 이 샘플에서는 c + + Win32 응용 프로그램에 몇 가지 기본적인 UWP 컨트롤을 추가 하 고 DPI 변경을 처리 하는 방법을 보여 줍니다.

### <a name="wpf-and-windows-forms"></a>WPF 및 Windows Forms

Windows 커뮤니티 Toolkit에서 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤 참조 샘플 WPF 및 Windows Forms 응용 프로그램에서 호스팅 API UWP 사용에 대 한 역할을 합니다. 다음 웹 사이트에서 소스 코드를 사용할 수 있습니다.

  * [여기로 이동](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost)하는 컨트롤의 WPF 버전. WPF 버전 [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 파생 됩니다.
  * [여기로 이동](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost)하는 컨트롤의 Windows Forms 버전입니다. Windows Forms 버전 [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)에서 파생 됩니다.

## <a name="prerequisites"></a>사전 요구 사항

UWP XAML 호스팅 API는 이러한 전제 조건이 있습니다.

* 17709 (또는 이후 빌드) Windows 10 내부자 미리 보기 빌드 및 해당 내부 미리 보기 빌드는 Windows SDK의. 변화 하는 기능 이므로 최신 버전을 사용 하 여 최상의 사용 환경을 위해 빌드하십시오.

* UWP XAML 데스크톱 응용 프로그램에서 호스팅 API를 사용 하려면 UWP Api를 호출할 수 있도록 프로젝트를 구성 해야.

    * **C + + Win32:** 사용 하 여 프로젝트를 구성 하는 것이 좋습니다 [C + + /cli WinRT](../cpp-and-winrt-apis/index.md). 다운로드 및 설치는 [C + + / WinRT Visual Studio 확장 (VSIX)](https://aka.ms/cppwinrt/vsix) Visual Studio 시장에서 추가 ```<CppWinRTEnabled>true</CppWinRTEnabled>``` 로 설명 [여기](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).vcxproj 파일에는 속성.

    * **Windows Forms 및 WPF:** [다음이 지침](../porting/desktop-to-uwp-enhance.md)을 따르십시오.

## <a name="architecture-of-xaml-islands"></a>XAML 제도 아키텍처

UWP XAML 호스팅 API는 [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) 네임 스페이스의 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)및 기타 관련된 형식을 포함 합니다. 데스크톱 응용 프로그램 UWP 컨트롤을 렌더링 하 고 들어오고 요소는 키보드 포커스 탐색 경로에이 API를 사용할 수 있습니다. 또한 데스크톱 응용 프로그램 크기 조정 하 고 원하는 UWP 컨트롤을 배치 수 있습니다.

데스크톱 응용 프로그램에서 호스팅 API는 XAML을 사용 하 여 XAML 아일랜드를 만들면 다음과 같은 개체 계층 구조를 해야 합니다.

* 기본 수준에서 XAML 섬 호스팅할 응용 프로그램의 UI 요소를입니다. 이 UI 요소에 대 한 창 핸들 (HWND) 있어야 합니다. WPF 응용 프로그램, Windows Forms 응용 프로그램의 경우 [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 및 c + + Win32 응용 프로그램의 [창](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) 에 대 한 [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 포함 하는 UI 요소의 XAML 아일랜드를 호스팅할 수 있습니다 예.

* 다음 수준에는 **DesktopWindowXamlSource** 개체입니다. 이 개체는 XAML 섬 호스팅용 인프라를 제공 합니다. 코드는이 개체를 만들고 이것을 상위 UI 요소에 연결 합니다.

* 이 개체는 **DesktopWindowXamlSource**를 만드는 UWP 컨트롤을 호스팅하는 기본 자식 창에 자동으로 만듭니다. 이 기본 자식 창의 코드 로부터 추출 하는 주로 하지만 핸들 (HWND) 필요한 경우에 액세스할 수 있습니다.

* 마지막으로, 최상위 수준에서 데스크톱 응용 프로그램을 호스트 하려면 UWP 컨트롤이입니다. [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), UWP 컨트롤 사용자 지정 컨트롤 뿐만 아니라 Windows SDK에서 제공을 포함 하 여에서 파생 된 UWP 개체 수 있습니다.

다음 다이어그램은 XAML 섬에 있는 개체의 계층 구조를 보여 줍니다.

![DesktopWindowXamlSource 아키텍처](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>UWP XAML 컨트롤을 호스트 하는 방법

UWP 응용 프로그램에서 컨트롤을 호스팅하려면 UWP XAML 호스팅 API를 사용 하기 위한 기본 단계는 다음과 같습니다.

1. 응용 프로그램을 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)에서 [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 개체를 호스팅하는 만들기 전에 현재 스레드의 UWP XAML 프레임 워크를 초기화 합니다.

    * **DesktopWindowXamlSource** 개체를 인스턴스화할 때이 프레임 워크를 초기화 됩니다 응용 프로그램을 만드는 경우 **DesktopWindowXamlSource** 개체 **Windows.UI.Xaml.UIElement** 개체를 만들기 전에 . 이 시나리오에서는 사용자 지정 프레임 워크를 초기화 하는 코드를 추가할 필요가 없습니다.

    * 그러나 응용 프로그램을 만드는 경우 **Windows.UI.Xaml.UIElement** 개체를 호스팅하는 **DesktopWindowXamlSource** 개체를 만들기 전에 응용 프로그램 호출 해야 정적 [** WindowsXamlManager.InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) **Windows.UI.Xaml.UIElement** 개체 인스턴스화 전에 UWP XAML 프레임 워크를 명시적으로 초기화 하는 방법입니다. 일반적으로 응용 프로그램 **DesktopWindowXamlSource** 을 호스팅하는 상위 UI 요소를 인스턴스화할 때이 메서드를 호출 해야 합니다.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 이 메서드는 XAML UWP 프레임 워크에 대 한 참조를 포함 하는 [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 개체를 반환 합니다. 특정된 스레드에 대해 원하는 만큼의 **WindowsXamlManager** 개체를 만들 수 있습니다. 그러나 각 개체 UWP XAML 프레임 워크에 대 한 참조를 보유 하 고 있기 때문에 XAML 리소스가 결국 해제 되도록 개체는 삭제 해야 합니다.

2. [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체를 만들고 창 핸들을 사용 하 여 연결 된 응용 프로그램의 UI 요소에 연결 합니다.

    이렇게 하려면 다음이 단계를 수행 해야 합니다.

    1. **DesktopWindowXamlSource** 개체를 만들고 **IDesktopWindowXamlSourceNative** COM 인터페이스로 캐스팅 합니다. 이 인터페이스에 선언 된 ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` Windows sdk에서 헤더 파일입니다. C + + Win32 프로젝트에서는이 헤더 파일을 직접 참조할 수 있습니다. Windows Forms 또는 WPF 프로젝트에 [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) 특성을 사용 하 여 응용 프로그램 코드에서이 인터페이스를 선언 해야 합니다. 인터페이스 선언에서 인터페이스 선언을 정확 하 게 일치 하는지 확인 ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. **IDesktopWindowXamlSourceNative** 인터페이스의 **AttachToWindow** 메서드를 호출 하 고 상위 UI 요소에서 응용 프로그램의 창 핸들을 전달 합니다.

    3. **DesktopWindowXamlSource**에 포함 된 내부 자식 창의 초기 크기를 설정 합니다. 기본적으로이 내부 자식 창의 너비와 높이가 0으로 설정 됩니다. 창의 크기를 설정 하지 않으면, UWP **DesktopWindowXamlSource** 에 추가한 컨트롤이 표시 되지 않습니다. **DesktopWindowXamlSource**에서 내부 자식 창에 액세스 하려면 **IDesktopWindowXamlSourceNative** 인터페이스의 **WindowHandle** 속성을 사용 합니다. 다음 예제에서는 윈도우의 크기를 설정 하려면 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수를 사용 합니다.

    이 과정을 보여 주는 몇 가지 코드 예제는 다음과 같습니다.

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI elemnt for your XAML island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. **Windows.UI.Xaml.UIElement** 를 **DesktopWindowXamlSource** 개체의 [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성을 설정 합니다. 다음 예제에서는 명명 된 [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) ```myGrid``` **Content** 속성에 있습니다.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

샘플 응용 프로그램 작업의 컨텍스트에서 이러한 작업을 보여 주는 전체 예제는 다음 코드 파일 참조.

  * **C + + Win32:** [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) 샘플에 [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) 파일 또는 [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 샘플의 [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) 파일을 참조 하십시오.
  * **WPF:** Windows 커뮤니티 Toolkit에서 [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조 하십시오.  
  * **Windows Forms:** Windows 커뮤니티 Toolkit에서 [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조 하십시오.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>사용자 지정 호스트에 UWP XAML 컨트롤 하는 방법을

> [!IMPORTANT]
> 현재 C# WPF 및 Windows Forms 응용 프로그램에서 사용자 지정 타사 컨트롤 UWP XAML만 지원 됩니다. 응용 프로그램을 컴파일할 수 있도록 소스 코드 컨트롤에 있어야 합니다.

사용자 지정 XAML UWP 컨트롤 (사용자 정의 컨트롤 또는 타사에서 제공 하는 컨트롤)을 호스트 하려는 경우 [이전 섹션](#how-to-host-uwp-xaml-controls)에서 설명 하는 프로세스 뿐만 아니라 다음과 같은 추가 작업 수행 해야 합니다.

1. [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) 에서 파생 되 고 또한 [**IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider)를 구현 하는 사용자 지정 형식을 정의 합니다. 이 이와 같은 응용 프로그램의 현재 디렉터리에 있는 어셈블리의 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자로 작동 합니다.

    이 작업을 수행 하는 방법을 보여 주는 예로, Windows 커뮤니티 Toolkit의 [XamlApplication.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/XamlApplication.cs) 코드 파일을 참조 하십시오. 이 파일은 WPF 및 Windows Forms UWP XAML 호스팅 이러한 유형의 응용 프로그램에서 API를 사용 하는 방법에 설명 하는 데 도움이 되는 **WindowsXamlHost** 클래스의 공유 구현.

2. 루트 메타 데이터 공급자의 [**GetXamlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) 메서드를 호출 하 여 XAML UWP 컨트롤의 형식 이름이 할당 되 면 런타임에 코드에서 할당 될 수이 등의 Visual Studio 속성 창에서 할당 하려면이 옵션을 사용 하도록 선택할 수 있습니다.

    이 작업을 수행 하는 방법을 보여 주는 예로, Windows 커뮤니티 Toolkit의 [UWPTypeFactory.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/UWPTypeFactory.cs) 코드 파일을 참조 하십시오. 이 파일은 부분에서는 WPF 및 Windows Forms에 대 한 **WindowsXamlHost** 클래스의 구현 공유 합니다.

3. UWP XAML 사용자 지정 컨트롤의 소스 코드를 호스트 응용 프로그램 솔루션에 통합할 하 고 사용자 지정 컨트롤을 빌드하고 다음 [이 지침](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)으로 응용 프로그램에서 사용 합니다.

## <a name="how-to-handle-keyboard-focus-navigation"></a>키보드 포커스 탐색을 처리 하는 방법

(예를 들어, 키를 눌러 **Tab** 키나 화살표 방향 /) 키보드를 사용 하는 응용 프로그램에서 UI 요소를 사용자가 탐색을 들어가고 **DesktopWindowXamlSource** 개체에 프로그래밍 방식으로 포커스를 이동 해야 합니다. 사용자의 키보드 탐색에는 **DesktopWindowXamlSource**, 프로그램 UI에 대 한 탐색 순서에서 첫 번째 [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 개체로 포커스를 이동에 도달 하면 다음으로 포커스를 이동 하 여 계속 ** Windows.UI.Xaml.UIElement** 개체 사용자 순환 요소 및 다음 **DesktopWindowXamlSource** 에서 다시 및 상위 UI 요소로 포커스를 이동 합니다.  

여러 가지 형식 및 멤버가 이러한 작업을 수행 하는 데 도움이 UWP XAML 호스팅 API를 제공 합니다.

1. 키보드 탐색에 **DesktopWindowXamlSource**으로 들어가면 [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 프로그래밍 방식으로 [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) 메서드를 사용 하 여 첫 번째 호스트 **Windows.UI.Xaml.UIElement** 으로 포커스를 이동 합니다.

2. 사용자 프로그램 **DesktopWindowXamlSource** 의 마지막 포커스 가능 요소에 **Tab** 키 또는 화살표 키를 누를 때 [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 호스트 응용 프로그램의 포커스 속성 프로그래밍 방식으로 포커스를 이동 합니다. 예를 들어, **DesktopWindowXamlSource** 는 [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 호스팅되는 WPF 응용 프로그램에서 포커스 속성 호스트 응용 프로그램에서 포커스를 이동 하려면 [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) 메서드를 사용할 수 있습니다.

작업 샘플 응용 프로그램의 컨텍스트에서이 작업을 수행 하는 방법을 보여 주는 예제를 보려면 다음 코드 파일:
  * **WPF:** Windows 커뮤니티 Toolkit에서 [WindowsXamlHostBase.Focus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) 파일을 참조 하십시오.  
  * **Windows Forms:** Windows 커뮤니티 Toolkit에서 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) 파일을 참조 하십시오.

## <a name="how-to-handle-layout-changes"></a>레이아웃 변경 내용을 처리 하는 방법

부모 UI 요소의 크기를 바꿀 때 UWP 컨트롤 제대로 표시 되었는지 확인 하는 데 필요한 레이아웃 변경 내용을 처리 해야 합니다. 다음은 고려해 야 할 중요 한 몇 가지 시나리오입니다.

1. 상위 UI 요소를 표시 하는 **Windows.UI.Xaml.UIElement** **DesktopWindowXamlSource**에서 호스팅하는 데 필요한 사각형 영역의 크기를 얻을 해야 **Windows.UI.Xaml.UIElement의 [**측정**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 메서드 호출 **. 예:
    * WPF 응용 프로그램에서 **DesktopWindowXamlSource**를 호스팅하는 [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 의 [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) 메서드로부터 이렇게 할 수 있습니다.
    * Windows Forms 응용 프로그램에서 호스팅하는 **DesktopWindowXamlSource** [**컨트롤**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 메서드로이 수행할 수 있습니다.

2. 상위 UI 요소 변경 크기 호출 **Windows.UI.Xaml.UIElement** 루트의 [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 **DesktopWindowXamlSource**에서 호스팅하는 합니다. 예:
    * WPF 응용 프로그램에서 [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) 메서드 **DesktopWindowXamlSource**를 호스팅하는 [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 개체에서 이렇게 할 수 있습니다.
    * Windows Forms 응용 프로그램에서 수행할 수 있습니다이 [**컨트롤**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 이벤트의 처리기에서 **DesktopWindowXamlSource**를 호스트 하는.

작업 샘플 응용 프로그램의 컨텍스트에서이 작업을 수행 하는 방법을 보여 주는 예제를 보려면 다음 코드 파일:
  * **WPF:** Windows 커뮤니티 Toolkit에서 [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하십시오.  
  * **Windows Forms:** Windows 커뮤니티 Toolkit에서 [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하십시오.

## <a name="how-to-handle-dpi-changes"></a>DPI 변경 내용을 처리 하는 방법

UWP 컨트롤 렌더링 변환을 사용 하 여 구성, 응용 프로그램에서 DPI 변경을 수신 대기할 필요 합니다 창에서 DPI 변경 UWP 프로그램으로 제어 (예를 들어, 다른 화면 DPI와 모니터 간에 창을 끌) 호스팅하는 처리 하려는 경우 창 위치를 업데이트 하 고 변형 UWP DPI 변경 내용에 따라 컨트롤을 렌더링 합니다.

다음 단계는 c + + Win32 응용 프로그램의 컨텍스트에서이 프로세스를 처리 하는 방법을 설명 합니다. 전체 예제는 GitHub에 [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) 및 [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 샘플에서 코드 파일을 참조 하십시오.

1. [**ScaleTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) 개체는 응용 프로그램을 유지 하 고 UWP 컨트롤의 [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 메서드 할당. 다음 예제는 c + + Win32 응용 프로그램에 [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) 컨트롤 합니다.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. [**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) 함수에서 [**WM_DPICHANGED**](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) 메시지를 수신 합니다. 이 메시지에 대 한 응답:
    * [**SetWindowPos**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수를 사용 하 여 메시지를 전달 하는 사각형에 UWP 컨트롤이 포함 된 창의 크기를 조정.
    * 새 DPI 값에 따라 **ScaleTransform** 개체의 x 축과 y 축 배율 인수를 업데이트 합니다.
    * 모양과 UWP 컨트롤의 레이아웃을 필요한 대로 수정 합니다. 다음 코드 예제는 [**패딩**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) DPI 변경에 대 한 응답으로 **Windows.UI.Xaml.Controls.Grid** 호스팅된 컨트롤의 조정합니다.

    ```cppwinrt
    LRESULT HandleDpiChange(HWND hWnd, WPARAM wParam, LPARAM lParam)
    {
        HWND hWndStatic = GetWindow(hWnd, GW_CHILD);
        if (hWndStatic != nullptr)
        {
            UINT uDpi = HIWORD(wParam);

            // Resize the window
            auto lprcNewScale = reinterpret_cast<RECT*>(lParam);

            SetWindowPos(hWnd, nullptr, lprcNewScale->left, lprcNewScale->top,
                lprcNewScale->right - lprcNewScale->left, lprcNewScale->bottom - lprcNewScale->top,
                SWP_NOZORDER | SWP_NOACTIVATE);

            NewScale(uDpi);
          }
          return 0;
    }

    void NewScale(UINT dpi) {

        auto scaleFactor = (float)dpi / 100;

        if (m_scale != nullptr) {
            m_scale.ScaleX(scaleFactor);
            m_scale.ScaleY(scaleFactor);
        }

        ApplyCorrection(scaleFactor);
    }

    void ApplyCorrection(float scaleFactor) {
        float rightCorrection = (m_rootGrid.Width() * scaleFactor - m_rootGrid.Width()) / scaleFactor;
        float bottomCorrection = (m_rootGrid.Height() * scaleFactor - m_rootGrid.Height()) / scaleFactor;

        m_rootGrid.Padding(Windows::UI::Xaml::ThicknessHelper::FromLengths(0, 0, rightCorrection, bottomCorrection));
    }
    ```

2. 당 모니터 DPI을 알고 있어야 응용 프로그램을 구성 하려면 [병렬 하 여 어셈블리 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 를 프로젝트에 추가 하 고 설정 ```<dpiAwareness>``` 요소를 ```PerMonitorV2```. 이 값에 대 한 자세한 내용은 [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)에 대 한 설명을 참조 하십시오.

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
        <application xmlns="urn:schemas-microsoft-com:asm.v3">
            <windowsSettings>
                <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
            </windowsSettings>
        </application>
    </assembly>
    ```

    전체 예제-병렬 어셈블리 매니페스트의 GitHub에서 [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 샘플의 [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) 파일을 참조 하십시오.

## <a name="limitations"></a>제한 사항

호스팅 API는 XAML 공유 Windows 10에 대 한 다른 모든 종류의 호스트 컨트롤을 XAML로 동일한 제한이 있습니다. 목록을 보려면 [XAML 호스트 컨트롤 제한](xaml-host-controls.md#limitations)을 참조 하십시오.

## <a name="troubleshooting"></a>문제 해결

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>UWP XAML UWP 응용 프로그램에서 호스팅 API를 사용 하 여 오류

| 문제 | 해결 방법 |
|-------|------------|
| 앱 받으면 아래와 같은 메시지가 **인덱스가** : "DesktopWindowXamlSource를 활성화할 수 없습니다. 이 형식은 사용할 수 없습니다 UWP 응용 프로그램에서. " "WindowsXamlManager를 활성화할 수 없습니다 또는. 이 형식은 사용할 수 없습니다 UWP 응용 프로그램에서. " | 이 오류는 UWP XAML 호스팅 API를 사용 하려는 의미 (특히, 하려고 하는 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 또는 [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 형식을 인스턴스화하고) UWP 응용 프로그램에서. 호스팅 API UWP XAML WPF, Windows Forms 및 Win32 c + + 응용 프로그램 등의 비 UWP 데스크톱 응용 프로그램에 사용할 수 에서만 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>다른 스레드에서 창에 연결 오류

| 문제 | 해결 방법 |
|-------|------------|
| 앱 받으면 아래와 같은 메시지가 **인덱스가** : "AttachToWindow 메서드가 다른 스레드에서 지정된 된 HWND 만들어졌기 때문에 못했습니다." | 이 오류는 응용 프로그램이 **IDesktopWindowXamlSourceNative.AttachToWindow** 메서드를 호출 하 고 다른 스레드에서 만든 창의 HWND를 통과 나타냅니다. 이 이렇게 동일한 스레드에서 메서드를 호출 하는 코드에 만들어진 창의 HWND 전달 해야 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>다른 최상위 창에서 창에 연결 오류

| 문제 | 해결 방법 |
|-------|------------|
| 앱 받으면 아래와 같은 메시지가 **인덱스가** : "AttachToWindow 메서드에 지정된 된 HWND HWND 동일한 스레드에서 이전에 AttachToWindow에 전달 된 것 보다 다른 최상위 창에서 상속 하기 때문에 못했습니다." | 이 오류는 응용 프로그램이 **IDesktopWindowXamlSourceNative.AttachToWindow** 메서드를 호출 하 고이 메서드에 대 한 이전 호출에 지정 된 창 보다 다른 최상위 창에서 상속 된 창의 HWND를 전달 의미 동일한 스레드에서.</p></p>같은 스레드에서 모든 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체 같은 최상위 창의 아래에 있는 창에 첨부할 수 응용 프로그램 **IDesktopWindowXamlSourceNative.AttachToWindow** 는 특정 스레드를 호출 후 **IDesktopWindowXamlSourceNative.AttachToWindow**에 대 한 첫 번째 호출에 전달 된. 모든 **DesktopWindowXamlSource** 개체는 특정 스레드에 대해 닫으면 다음 **DesktopWindowXamlSource** 창에 다시 연결 하는.</p></p>이 문제를 해결 하려면 두이 스레드에 바인딩된 다른 최상위 창에 있는이 **DesktopWindowXamlSource**에 대 한 새 스레드를 만들 **DesktopWindowXamlSource** 개체를 모두 닫습니다. |

## <a name="related-topics"></a>관련 항목

* [데스크톱 응용 프로그램의 UWP 컨트롤](xaml-host-controls.md)
