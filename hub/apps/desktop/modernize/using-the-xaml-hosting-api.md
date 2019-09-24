---
description: 이 문서에서는 데스크톱 C++ Win32 앱에서 UWP XAML UI를 호스트 하는 방법을 설명 합니다.
title: C++ Win32 앱에서 UWP XAML 호스팅 API 사용
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, xaml 제도
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 6c1f45b4bd3da74ea150c05800eba7ec10568894
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643402"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>C++ Win32 앱에서 UWP XAML 호스팅 API 사용

Windows 10, 버전 1903, 비 UWP 데스크톱 앱 (Win32, WPF 및 C++ Windows Forms 앱 포함)부터 *UWP XAML 호스팅 API* 를 사용 하 여 창 핸들 (HWND)과 연결 된 모든 UI 요소에서 uwp 컨트롤을 호스트할 수 있습니다. 이 API는 UWP 이외의 앱이 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 사용할 수 있도록 합니다. 예를 들어 비 UWP 데스크톱 앱은이 API를 사용 하 여 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/index) 을 사용 하 고 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)를 지 원하는 UWP 컨트롤을 호스트할 수 있습니다.

UWP XAML 호스팅 API는 개발자가 비 UWP 데스크톱 앱에 흐름 UI를 가져올 수 있도록 하는 광범위 한 컨트롤 집합에 대 한 토대를 제공 합니다. 이 기능을 *XAML 아일랜드*라고 합니다. 이 기능에 대 한 개요는 [데스크톱 앱에서 UWP xaml 컨트롤 호스트 (Xaml 아일랜드)](xaml-islands.md)를 참조 하세요.

> [!NOTE]
> XAML 아일랜드에 대 한 피드백이 있는 경우 [Microsoft Toolkit 리포지토리의](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) 새 문제를 만들고 의견을 남겨 두세요. 사용자 의견을 개인적으로 제출 하는 것을 선호 하는 경우 XamlIslandsFeedback@microsoft.com로 보낼 수 있습니다. 귀하의 통찰력 및 시나리오는 매우 중요 합니다.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>UWP XAML 호스팅 API를 사용 해야 하나요?

UWP XAML 호스팅 API는 데스크톱 앱에서 UWP 컨트롤을 호스팅하기 위한 하위 수준의 인프라를 제공 합니다. 일부 유형의 데스크톱 앱에는이 목표를 달성 하는 데 더 편리한 대체 Api를 사용할 수 있는 옵션이 있습니다.  

* C++ Win32 데스크톱 앱이 있고 앱에서 uwp 컨트롤을 호스트 하려는 경우 uwp XAML 호스팅 API를 사용 해야 합니다. 이러한 유형의 앱에 대 한 대안이 없습니다.

* WPF 및 Windows Forms apps의 경우 UWP XAML 호스팅 API를 직접 사용 하는 대신 Windows Community Toolkit의 [Xaml 아일랜드 .net 컨트롤](xaml-islands.md#wpf-and-windows-forms-applications) 을 사용 하는 것이 좋습니다. 이러한 컨트롤은 내부적으로 UWP XAML 호스팅 API를 사용 하며, 키보드 탐색 및 레이아웃 변경을 포함 하 여 UWP XAML 호스팅 API를 직접 사용 하는 경우 직접 처리 해야 하는 모든 동작을 구현 합니다.

Win32 앱만 C++ UWP XAML 호스팅 API를 사용 하는 것이 좋지만이 문서에서는 주로 win32 앱에 대 C++ 한 지침과 예제를 제공 합니다. 그러나 선택 하는 경우 WPF에서 UWP XAML 호스팅 API를 사용 하 고 앱을 Windows Forms 수 있습니다. 이 문서에서는 Windows 커뮤니티 도구 키트에서 WPF 및 Windows Forms에 대 한 [호스트 컨트롤](xaml-islands.md#host-controls) 의 관련 소스 코드를 안내 하 여 UWP XAML 호스팅 API가 해당 컨트롤에서 어떻게 사용 되는지 확인할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

XAML 아일랜드에는 Windows 10, 버전 1903 이상 및 Windows SDK의 해당 빌드가 필요 합니다. C++ Win32 앱에서 XAML 아일랜드를 사용 하려면 먼저 프로젝트를 설정 해야 합니다.

### <a name="support-for-cwinrt"></a>C++/Winrt 지원

프로젝트가 [ C++/winrt](/windows/uwp/cpp-and-winrt-apis)를 지원 하는지 확인 합니다.

* 새 프로젝트의 경우 [ C++/winrt Visual Studio 확장 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 을 설치 하 고 해당 확장에 포함 된 C++/winrt 프로젝트 템플릿 중 하나를 사용할 수 있습니다.
* 기존 프로젝트의 경우 프로젝트에 [CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 패키지를 설치할 수 있습니다.

이러한 옵션에 대 한 자세한 내용은 [이 문서](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)를 참조 하세요.

### <a name="configure-your-project-for-app-deployment"></a>앱 배포를 위한 프로젝트 구성

배포를 위해 프로젝트를 준비 하려면 다음 옵션 중 하나를 선택 합니다.

* **MSIX 패키지에서 앱을 패키지**합니다. [Msix 패키지](https://docs.microsoft.com/windows/msix/) 에서 앱을 패키징하 면 다양 한 배포 및 런타임 이점이 제공 됩니다.
    1. Windows 10 버전 1903 SDK (버전 10.0.18362) 이상 릴리스를 설치 합니다.
    2. [Windows 응용 프로그램 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 를 솔루션에 추가 하 고 C++/win32 프로젝트에 대 한 참조를 추가 하 여 msix 패키지에 앱을 패키지 합니다.

* **Microsoft Toolkit**. i n k. MSIX 패키지에서 앱을 패키지 하지 않으려는 경우에는 6.0.0 (버전 v-preview7 이상 [)를 설치할](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 수 있습니다. 이 패키지는 XAML 아일랜드가 앱에서 작동할 수 있도록 하는 몇 가지 빌드 및 런타임 자산을 제공 합니다. 이 패키지의 최신 미리 보기 버전을 볼 수 있도록 **시험판 포함** 옵션이 선택 되어 있는지 확인 합니다.

> [!NOTE]
> 이러한 지침의 이전 버전에서는 프로젝트의 응용 `maxversiontested` 프로그램 매니페스트에 요소를 추가 했습니다. 위에 나열 된 옵션 중 하나를 사용 하는 동안에는이 요소를 매니페스트에 더 이상 추가할 필요가 없습니다.

### <a name="additional-requirements-for-custom-uwp-controls"></a>사용자 지정 UWP 컨트롤에 대 한 추가 요구 사항

사용자 지정 UWP 컨트롤을 호스트 하는 경우 (예: 함께 작동 하는 여러 UWP 컨트롤로 구성 된 사용자 정의 컨트롤) [이 섹션](#host-a-custom-uwp-control)의 추가 지침을 따라야 합니다. 또한 사용자 지정 컨트롤에 대 한 소스 코드를 사용 하 여 앱으로 컴파일할 수 있어야 합니다.

## <a name="architecture-of-the-api"></a>API의 아키텍처

UWP XAML 호스팅 API는 이러한 기본 Windows 런타임 형식 및 COM 인터페이스를 포함 합니다.

|  형식 또는 인터페이스 | 설명 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 이 클래스는 UWP XAML 프레임 워크를 나타냅니다. 이 클래스는 데스크톱 응용 프로그램의 현재 스레드에서 UWP XAML 프레임 워크를 초기화 하는 단일 정적 [Initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 메서드를 제공 합니다. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 이 클래스는 데스크톱 앱에서 호스트 하는 UWP XAML 콘텐츠의 인스턴스를 나타냅니다. 이 클래스의 가장 중요 한 멤버는 [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성입니다. 이 속성은 호스팅할를 지정 하는 [Windows.](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 이 클래스에는 XAML 아일랜드에 대 한 키보드 포커스 탐색을 라우팅하는 다른 멤버도 있습니다. |
| IDesktopWindowXamlSourceNative | 이 COM 인터페이스는 응용 프로그램의 XAML 아일랜드를 부모 UI 요소에 연결 하는 데 사용 하는 **AttachToWindow** 메서드를 제공 합니다. 모든 **Desktopwindowxamlsource** 개체는이 인터페이스를 구현 합니다. |
| IDesktopWindowXamlSourceNative2 | 이 COM 인터페이스는 UWP XAML 프레임 워크가 특정 Windows 메시지를 올바르게 처리할 수 있도록 하는 **PreTranslateMessage** 메서드를 제공 합니다. 모든 **Desktopwindowxamlsource** 개체는이 인터페이스를 구현 합니다. |

다음 다이어그램은 데스크톱 응용 프로그램에서 호스팅되는 XAML 아일랜드의 개체 계층 구조를 보여 줍니다.

* 기본 수준에서는 XAML 아일랜드를 호스팅할 응용 프로그램의 UI 요소입니다. 이 UI 요소에는 창 핸들 (HWND)이 있어야 합니다. C++ Win32의 [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)를 포함하는 XAML 아일랜드를 호스팅할 수 있는 UI 요소에 대한 예로는 WPE 앱용 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 및 Windows Forms 앱용 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)이 있습니다.

* 다음 수준은 **Desktopwindowxamlsource** 개체입니다. 이 개체는 XAML 아일랜드 호스팅을 위한 인프라를 제공 합니다. 코드에서이 개체를 만들어 부모 UI 요소에 연결 하는 일을 담당 합니다.

* **Desktopwindowxamlsource**를 만들 때이 개체는 자동으로 네이티브 자식 창을 만들어 UWP 컨트롤을 호스팅합니다. 이 네이티브 자식 창은 주로 코드에서 추상화 되지만 필요한 경우 해당 핸들 (HWND)에 액세스할 수 있습니다.

* 마지막으로, 최상위 수준에는 데스크톱 앱에서 호스트 하려는 UWP 컨트롤이 있습니다. 이는 Windows SDK에서 제공 하는 모든 UWP 컨트롤과 사용자 지정 사용자 정의 컨트롤을 포함 하 여 [Windows. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생 되는 모든 uwp 개체 일 수 있습니다.

![DesktopWindowXamlSource 아키텍처](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>관련 샘플

코드에서 UWP XAML 호스팅 API를 사용 하는 방법은 앱 유형, 앱 디자인 및 기타 요인에 따라 달라 집니다. 전체 앱의 컨텍스트에서이 API를 사용 하는 방법을 설명 하기 위해이 문서에서는 다음 샘플의 코드를 참조 합니다.

### <a name="c-win32"></a>C++(Win32

다음 샘플에서는 C++ Win32 앱에서 UWP XAML 호스팅 API를 사용 하는 방법을 보여 줍니다.

* [간단한 XAML 아일랜드 샘플](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)입니다. 이 샘플에서는 패키지 되지 않은 C++ Win32 앱 (즉, msix 패키지에 내장 되지 않은 앱)에서 UWP 컨트롤을 호스트 하는 기본 구현을 보여 줍니다.

* [사용자 지정 컨트롤 샘플을 사용 하는 XAML 아일랜드](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App). 이 샘플에서는 패키지 되지 않은 C++ Win32 앱에서 사용자 지정 UWP 컨트롤을 호스트 하는 전체 구현을 보여 주고 키보드 입력 및 포커스 탐색과 같은 기타 동작을 처리 하는 방법을 보여 줍니다. 

### <a name="wpf-and-windows-forms"></a>WPF 및 Windows Forms

Windows Community Toolkit의 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤은 WPF 및 Windows Forms APPS에서 UWP 호스팅 API를 사용 하기 위한 참조 샘플로 사용 됩니다. 소스 코드는 다음 위치에서 사용할 수 있습니다.

* WPF 버전 컨트롤의 경우 [여기로 이동](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)합니다. WPF 버전은 [system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 파생 됩니다.

* 컨트롤의 Windows Forms 버전을 [보려면 여기로 이동](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)하세요. Windows Forms 버전은 [system.object](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)에서 파생 됩니다.

## <a name="host-a-standard-uwp-control"></a>표준 UWP 컨트롤 호스팅

이 섹션에서는 새 C++ Win32 앱에서 uwp XAML 호스팅 API를 사용 하 여 표준 uwp 컨트롤 (즉, Windows SDK 또는 WinUI 라이브러리에서 제공 하는 컨트롤)을 호스트 하는 과정을 안내 합니다. 이 코드는 [간단한 XAML 아일랜드 샘플](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)을 기반으로 하며,이 섹션에서는 코드의 가장 중요 한 부분 중 일부에 대해 설명 합니다. 기존 C++ Win32 응용 프로그램 프로젝트가 있는 경우 프로젝트에 대 한 이러한 단계 및 코드 예제를 적용할 수 있습니다.

### <a name="configure-the-project"></a>프로젝트 구성

1. Visual Studio 2019에서 Windows 10 버전 1903 SDK (버전 10.0.18362) 또는 이후 릴리스가 설치 된 경우 새 **Windows 데스크톱 응용 프로그램** 프로젝트를 만듭니다. 이 프로젝트는 **C++** , **Windows**및 **데스크톱** 프로젝트 필터 아래에서 사용할 수 있습니다.

2. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고, **솔루션**대상 변경을 클릭 하 고, **10.0.18362.0** 또는 이후 버전의 SDK 릴리스를 선택한 다음 **확인**을 클릭 합니다.

3. [CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 패키지를 설치 합니다.

    1. **솔루션 탐색기** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.
    2. **찾아보기** 탭을 선택 하 고 [CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 패키지를 검색 한 후이 패키지의 최신 버전을 설치 합니다.

4. [Microsoft Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) . i n k.

    1. **NuGet 패키지 관리자** 창에서 **시험판 포함** 이 선택 되어 있는지 확인 합니다.
    2. **찾아보기** 탭을 선택 하 고, 6.0.0 [패키지를](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 검색 하 고,이 패키지의 버전 v-preview7 이상을 설치 합니다.

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>XAML 호스팅 API를 사용 하 여 UWP 컨트롤 호스팅

XAML 호스팅 API를 사용 하 여 UWP 컨트롤을 호스트 하는 기본 프로세스는 다음과 같은 일반적인 단계를 따릅니다.

1. 현재 스레드에 대 한 UWP XAML 프레임 워크를 초기화 합니다. 그러면 응용 프로그램에서 호스팅할 모든 [Windows .xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) . 이 작업을 수행 하는 방법에는 컨트롤을 호스트 하는 [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체를 만들 시기에 따라 여러 가지가 있습니다.

    * 응용 프로그램에서 호스팅할 모든 **Windows. .xaml** 개체를 만들기 전에 **Desktopwindowxamlsource** 개체를 만드는 경우이 프레임 워크는를 인스턴스화할 **때 초기화 됩니다. DesktopWindowXamlSource** 개체입니다. 이 시나리오에서는 프레임 워크를 초기화 하는 사용자 고유의 코드를 추가할 필요가 없습니다.

    * 그러나 응용 프로그램에서 응용 프로그램을 호스팅할 **Desktopwindowxamlsource** 개체를 만들기 전에 응용 프로그램에서 **해당 개체를** 만드는 경우에는 응용 프로그램에서 정적 [메서드를 호출 해야 합니다. ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) **Windows .Xaml. .xaml** 개체가 인스턴스화되기 전에 UWP xaml 프레임 워크를 명시적으로 초기화 하는 windowsxamlmanager. InitializeForCurrentThread 메서드. 응용 프로그램은 일반적으로 **Desktopwindowxamlsource** 를 호스팅하는 부모 UI 요소가 인스턴스화될 때이 메서드를 호출 해야 합니다.

    > [!NOTE]
    > 이 메서드는 UWP XAML 프레임 워크에 대 한 참조를 포함 하는 [Windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 개체를 반환 합니다. 지정 된 스레드에서 원하는 수 만큼 **Windowsxamlmanager** 개체를 만들 수 있습니다. 그러나 각 개체는 UWP XAML 프레임 워크에 대 한 참조를 포함 하기 때문에 개체를 삭제 하 여 XAML 리소스가 최종적으로 해제 되도록 해야 합니다.

2. [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체를 만들고 창 핸들과 연결 된 응용 프로그램의 부모 UI 요소에 연결 합니다.

    이렇게 하려면 다음 단계를 수행 해야 합니다.

    1. **Desktopwindowxamlsource** 개체를 만들고 **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** COM 인터페이스로 캐스팅 합니다.
        > [!NOTE]
        > 이러한 인터페이스는 Windows SDK의 **windows.** x. x. x x x. x x x. h 헤더 파일에 선언 됩니다. 기본적으로이 파일은% programfiles (x86)% \ Windows Kits\10\Include\\< 빌드 번호\>\um.에 있습니다.

    2. **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** 인터페이스의 **AttachToWindow** 메서드를 호출 하 고 응용 프로그램에서 부모 UI 요소의 창 핸들을 전달 합니다.

    3. **Desktopwindowxamlsource**에 포함 된 내부 자식 창의 초기 크기를 설정 합니다. 기본적으로이 내부 자식 창에는 너비와 높이가 0으로 설정 되어 있습니다. 창 크기를 설정 하지 않으면 **Desktopwindowxamlsource** 에 추가 하는 모든 UWP 컨트롤이 표시 되지 않습니다. **Desktopwindowxamlsource**의 내부 자식 창에 액세스 하려면 **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** 인터페이스의 **WindowHandle** 속성을 사용 합니다.

3. 마지막으로, 호스트할 **Windows.UI.Xaml.UIElement**을 **DesktopWindowXamlSource** 개체의 [콘텐츠](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성에 할당합니다.

다음 단계 및 코드 예제에서는 위의 프로세스를 구현 하는 방법을 보여 줍니다.

1. 프로젝트의 **소스 파일** 폴더에서 기본 **windowsproject .cpp** 파일을 엽니다. 파일의 전체 내용을 삭제 하 고 다음 `include` 및 `using` 문을 추가 합니다. 이러한 문에는 표준 C++ 및 UWP 헤더와 네임 스페이스 외에도 XAML 아일랜드와 관련 한 여러 항목이 포함 되어 있습니다.

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. 이전 섹션 뒤에 다음 코드를 복사 합니다. 이 코드는 앱에 대 한 **WinMain** 함수를 정의 합니다. 이 함수는 기본 창을 초기화 하 고 XAML 호스팅 API를 사용 하 여 창에서 간단한 UWP **TextBlock** 컨트롤을 호스팅합니다.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. 이전 섹션 뒤에 다음 코드를 복사 합니다. 이 코드는 창에 대 한 [창 프로시저](https://docs.microsoft.com/en-us/windows/win32/learnwin32/writing-the-window-procedure) 를 정의 합니다.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. 코드 파일을 저장 하 고 앱을 빌드하고 실행 합니다. 앱 창에서 UWP **TextBlock** 컨트롤이 표시 되는지 확인 합니다.
    > [!NOTE]
    > `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` 및`manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`를 비롯 한 몇 가지 빌드 경고가 표시 될 수 있습니다. 이러한 경고는 현재 도구 및 NuGet 패키지와 관련 하 여 알려진 문제 이며 무시 해도 됩니다.

이러한 작업을 보여 주는 전체 예제는 다음 코드 파일을 참조 하세요.

* **C++(Win32**
  * [SIMPLE XAML 아일랜드 샘플](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)의 [HelloWindowsDesktop](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_SimpleApp/Win32DesktopApp/HelloWindowsDesktop.cpp) 파일을 참조 하세요.
  * [XAML 아일랜드와 사용자 지정 컨트롤 샘플](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)에서 [xamlbridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) 파일을 참조 하세요.

* **WPF** Windows 커뮤니티 도구 키트에서 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조 하세요.  

* **Windows Forms:** Windows 커뮤니티 도구 키트에서 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조 하세요.

## <a name="host-a-custom-uwp-control"></a>사용자 지정 UWP 컨트롤 호스트

사용자 지정 UWP 컨트롤 (자신을 정의 하는 컨트롤 또는 타사에서 제공 하는 컨트롤) C++ 을 호스트 하는 프로세스는 [표준 컨트롤을 호스트](#host-a-standard-uwp-control)하는 것과 동일한 단계를 시작 합니다. 그러나 사용자 지정 UWP 컨트롤을 호스트 하려면 추가 프로젝트와 단계가 필요 합니다.

사용자 지정 UWP 컨트롤을 호스트 하려면 다음 프로젝트 및 구성 요소가 필요 합니다.

* **앱에 대 한 프로젝트 및 소스 코드**입니다. XAML 아일랜드 호스팅을 위한 [필수 구성 요소](#prerequisites) 를 충족 하도록 프로젝트를 구성 했는지 확인 합니다.

* **사용자 지정 UWP 컨트롤**입니다. 앱을 사용 하 여 컴파일할 수 있도록 호스트 하려는 사용자 지정 UWP 컨트롤에 대 한 소스 코드가 필요 합니다. 일반적으로 사용자 지정 컨트롤은 C++ Win32 프로젝트와 동일한 솔루션에서 참조 하는 UWP 클래스 라이브러리 프로젝트에서 정의 됩니다.

* **XamlApplication 개체를 정의 하는 UWP 앱 프로젝트**입니다. C++ Win32 프로젝트에는 Windows 커뮤니티 도구 키트에서 제공 하 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 는 클래스의 인스턴스에 대 한 액세스 권한이 있어야 합니다. 이 형식은 응용 프로그램의 현재 디렉터리에 있는 어셈블리의 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자로 작동 합니다. 이 작업을 수행 하는 권장 방법은 C++ Win32 프로젝트와 동일한 솔루션에 **빈 앱 (유니버설 Windows)** 프로젝트를 추가 하 고이 프로젝트에서 기본 `App` 클래스를 수정 하는 것입니다.
  > [!NOTE]
  > 솔루션에는 개체를 `XamlApplication` 정의 하는 프로젝트가 하나만 포함 될 수 있습니다. 앱의 모든 사용자 지정 UWP 컨트롤은 같은 `XamlApplication` 개체를 공유 합니다. 개체를 `XamlApplication` 정의 하는 프로젝트에는 XAML 아일랜드에서 uwp 컨트롤을 호스트 하는 데 사용 되는 다른 모든 UWP 라이브러리 및 프로젝트에 대 한 참조가 포함 되어야 합니다.

C++ Win32 앱에서 사용자 지정 UWP 컨트롤을 호스팅하려면 다음 일반적인 단계를 수행 합니다.

1. C++ Win32 데스크톱 앱 프로젝트를 포함 하는 솔루션에서 **비어 있는 앱 (유니버설 Windows)** 프로젝트를 추가 하 고 관련 WPF 연습에서 [이 단원의](host-custom-control-with-xaml-islands.md#create-a-xamlapplication-object-in-a-uwp-app-project) 자세한 지침에 따라 구성 합니다.

2. 동일한 솔루션에서 사용자 지정 UWP XAML 컨트롤에 대 한 소스 코드가 포함 된 프로젝트를 추가 하 고 (일반적으로 UWP 클래스 라이브러리 프로젝트) 프로젝트를 빌드합니다.

3. UWP 앱 프로젝트에서 UWP 클래스 라이브러리 프로젝트에 대 한 참조를 추가 합니다.

4. C++ Win32 프로젝트에서 uwp 앱 프로젝트 및 uwp 클래스 라이브러리 프로젝트에 대 한 참조를 솔루션에 추가 합니다.

5. [Xaml 호스팅 API를 사용 하 여 UWP 컨트롤](#use-the-xaml-hosting-api-to-host-a-uwp-control) 호스팅 섹션에 설명 된 프로세스에 따라 앱의 xaml 아일랜드에서 사용자 지정 컨트롤을 호스팅합니다. 호스팅할 사용자 지정 컨트롤의 인스턴스를 코드의 **Desktopwindowxamlsource** 개체의 [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성에 할당 합니다.

C++ Win32 응용 프로그램에 대 한 전체 예제는 [사용자 지정 컨트롤을 사용 하 여 XAML 아일랜드 샘플](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)에서 다음 프로젝트를 참조 하세요.

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): 이 프로젝트는 텍스트 상자, 여러 단추 및 `MyUserControl` 콤보 상자가 포함 된 이라는 사용자 지정 UWP XAML 컨트롤을 구현 합니다.
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): 위에서 설명한 변경 내용이 포함 된 UWP 앱 프로젝트입니다.
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): 이는 XAML C++ 아일랜드에서 사용자 지정 UWP xaml 컨트롤을 호스트 하는 Win32 앱 프로젝트입니다.

## <a name="handle-keyboard-layout-and-dpi"></a>키보드, 레이아웃 및 DPI를 처리 합니다.

다음 섹션에서는 키보드 및 레이아웃 시나리오를 처리 하는 방법에 대 한 지침과 코드 예제에 대 한 링크를 제공 합니다.

* [키보드 입력](#keyboard-input)
* [키보드 포커스 탐색](#keyboard-focus-navigation)
* [레이아웃 변경 내용 처리](#handle-layout-changes)
* [DPI 변경 내용 처리](#handle-dpi-changes)

### <a name="keyboard-input"></a>키보드 입력

각 XAML 아일랜드에 대 한 키보드 입력을 올바르게 처리 하려면 특정 메시지가 올바르게 처리 될 수 있도록 응용 프로그램에서 모든 Windows 메시지를 UWP XAML 프레임 워크로 전달 해야 합니다. 이렇게 하려면 메시지 루프에 액세스할 수 있는 응용 프로그램의 일부에서 각 XAML 아일랜드의 **Desktopwindowxamlsource** 개체를 **IDesktopWindowXamlSourceNative2** COM 인터페이스로 캐스팅 합니다. 그런 다음이 인터페이스의 **PreTranslateMessage** 메서드를 호출 하 고 현재 메시지를 전달 합니다.

  * **C++ Win32:** : 앱은 기본 메시지 루프에서 직접 **PreTranslateMessage** 를 호출할 수 있습니다. 예제를 보려면 [ C++ Win32 샘플](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)에서 [xamlbridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) 파일을 참조 하세요.

  * **WPF** 앱은 [Componentdispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) 이벤트에 대 한 이벤트 처리기에서 **PreTranslateMessage** 를 호출할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) 파일을 참조 하세요.

  * **Windows Forms:** 앱은 **PreTranslateMessage** 를 호출할 수 있습니다 [. preprocessmessage](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) 메서드. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) 파일을 참조 하세요.

### <a name="keyboard-focus-navigation"></a>키보드 포커스 탐색

사용자가 키보드를 사용 하 여 응용 프로그램의 UI 요소를 탐색 하는 경우 (예: **tab** 키 또는 방향/화살표 키 누르기), 프로그래밍 방식으로 **Desktopwindowxamlsource** 개체와 외부로 포커스를 이동 해야 합니다. 사용자의 키보드 탐색이 **Desktopwindowxamlsource**에 도달 하면 ui의 탐색 순서에서 첫 번째 [Windows. .xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 개체에 포커스를 이동 하 여 포커스를 다음 **으로 이동 합니다.** 사용자가 요소를 순환 하 고 **Desktopwindowxamlsource** 와 부모 UI 요소로 포커스를 다시 이동 하는 경우에는 Windows ui.  

UWP XAML 호스팅 API는 이러한 작업을 수행 하는 데 도움이 되는 몇 가지 형식과 멤버를 제공 합니다.

* 키보드 탐색이 **Desktopwindowxamlsource**에 들어가면 [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) 메서드를 사용 하 여 프로그래밍 방식으로 포커스를 첫 번째 호스팅된 **창** 으로 이동 합니다.

* 사용자가 **Desktopwindowxamlsource** 에서 가장 포커스를 받을 수 있는 요소에 있고 **Tab** 키 또는 화살표 키를 누르면 [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 호스트 응용 프로그램에서 포커스를 받을 수 있는 다음 요소로 포커스를 프로그래밍 방식으로 이동 합니다. 예를 들어 **Desktopwindowxamlsource** [system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 호스트 되는 WPF 응용 프로그램에서 [movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) 메서드를 사용 하 여 호스트 응용 프로그램에서 포커스를 받을 수 있는 다음 요소로 포커스를 이동할 수 있습니다.

작업 중인 샘플 응용 프로그램의 컨텍스트에서이 작업을 수행 하는 방법을 보여 주는 예제는 다음 코드 파일을 참조 하세요.

  * **C++/Win32**: Win32 샘플에서 [xamlbridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) 파일을 참조 하세요. [ C++ ](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)

  * **WPF** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) 파일을 참조 하세요.  

  * **Windows Forms:** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) 파일을 참조 하세요.

### <a name="handle-layout-changes"></a>레이아웃 변경 내용 처리

사용자가 부모 UI 요소의 크기를 변경 하는 경우 UWP 컨트롤이 원하는 대로 표시 되도록 필요한 레이아웃 변경 내용을 처리 해야 합니다. 고려해 야 할 몇 가지 중요 한 시나리오는 다음과 같습니다.

* C++ Win32 응용 프로그램에서 응용 프로그램이 WM_SIZE 메시지를 처리할 때 [setwindowpos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수를 사용 하 여 호스팅된 XAML 아일랜드의 위치를 변경할 수 있습니다. 예제를 보려면 [ C++ Win32 샘플](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)에서 [sampleapp.exe](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) 코드 파일을 참조 하세요.

* 부모 UI 요소가 **Desktopwindowxamlsource**에서 **호스팅하는 데** 필요한 사각형 영역의 크기를 조정 해야 하는 경우에는 Windows. .xaml의 [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 호출 합니다.. 예를 들어 다음과 같은 가치를 제공해야 합니다.

    * WPF 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스트 하는 [system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 의 [system.windows.frameworkelement.measureoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) 메서드에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

    * Windows Forms 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스트 하는 [컨트롤](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 메서드에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

* 부모 UI 요소의 크기가 변경 되 면 **Desktopwindowxamlsource**에서 호스트 하는 루트 **Windows. .xaml. UIElement** 의 [정렬](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 호출 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

    * WPF 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스팅하는 [System.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 개체의 [system.windows.frameworkelement.arrangeoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) 메서드에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

    * Windows Forms 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스트 하는 [컨트롤](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 이벤트에 대 한 처리기에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

### <a name="handle-dpi-changes"></a>DPI 변경 내용 처리

UWP XAML 프레임 워크는 호스트 된 UWP 컨트롤에 대 한 DPI 변경을 자동으로 처리 합니다 (예: 사용자가 다른 화면 DPI를 사용 하는 모니터 사이에서 창을 끌 때). 최상의 환경을 위해 Windows Forms, WPF 또는 C++ Win32 응용 프로그램이 모니터 당 DPI를 인식 하도록 구성 하는 것이 좋습니다.

모니터 별로 응용 프로그램 DPI를 인식 하도록 구성 하려면 프로젝트에 [side-by-side 어셈블리 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 를 추가 하 고 **PerMonitorV2**에 대 한 자세한 내용을  **\<\> 설정 합니다.** 이 값에 대 한 자세한 내용은 [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)에 대 한 설명을 참조 하세요.

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

## <a name="troubleshooting"></a>문제 해결

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>UWP 앱에서 UWP XAML 호스팅 API를 사용 하는 동안 오류 발생

| 문제점 | 해결 방법 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** 를 수신 합니다. "DesktopWindowXamlSource를 활성화할 수 없습니다. 이 형식은 UWP 앱에서 사용할 수 없습니다. " 또는 "WindowsXamlManager를 활성화할 수 없습니다. 이 형식은 UWP 앱에서 사용할 수 없습니다. " | 이 오류는 uwp 앱에서 UWP XAML 호스팅 API (특히 [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 또는 [windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 유형을 인스턴스화하려고 시도)를 사용 하 고 있음을 나타냅니다. UWP XAML 호스팅 API는 WPF, Windows Forms 및 C++ Win32 응용 프로그램과 같은 비 UWP 데스크톱 앱 에서만 사용할 수 있습니다. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>WindowsXamlManager 또는 DesktopWindowXamlSource 유형을 사용 하는 동안 오류 발생

| 문제점 | 해결 방법 |
|-------|------------|
| 앱에서 다음 메시지와 함께 예외를 수신 합니다. "WindowsXamlManager 및 DesktopWindowXamlSource는 Windows 버전 10.0.18226.0 이상 버전을 대상으로 하는 앱에 대해 지원 됩니다. 응용 프로그램 매니페스트 또는 패키지 매니페스트를 확인 하 고 MaxTestedVersion 속성이 업데이트 되었는지 확인 하십시오. " | 이 오류는 응용 프로그램이 UWP XAML 호스팅 API에서 **Windowsxamlmanager** 또는 **Desktopwindowxamlsource** 형식을 사용 하려고 했지만, OS에서 Windows 10 버전 1903 이상을 대상으로 하는 앱을 빌드 했는지 여부를 확인할 수 없음을 나타냅니다. UWP XAML 호스팅 API는 이전 버전의 Windows 10에서 미리 보기로 처음 도입 되었지만 Windows 10 버전 1903부터만 지원 됩니다.</p></p>이 문제를 해결 하려면 앱에 대 한 MSIX 패키지를 만들고 패키지에서 실행 하거나 프로젝트에 [Microsoft Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) . n a m a. n a m a 패키지를 설치 합니다. 자세한 내용은 [이 섹션](#configure-your-project-for-app-deployment)을 참조하세요. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>다른 스레드의 창에 연결 하는 동안 오류가 발생 했습니다.

| 문제점 | 해결 방법 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** 를 수신 합니다. 지정 된 HWND가 다른 스레드에 생성 되었으므로 "AttachToWindow 메서드가 실패 했습니다." | 이 오류는 응용 프로그램에서 **IDesktopWindowXamlSourceNative:: AttachToWindow** 메서드를 호출 하 고 다른 스레드에서 만든 창의 HWND를 전달 했음을 나타냅니다. 이 메서드는 메서드를 호출 하는 코드와 동일한 스레드에 생성 된 창의 HWND를 전달 해야 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>다른 최상위 창의 창에 연결 하는 동안 오류가 발생 했습니다.

| 문제점 | 해결 방법 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** 를 수신 합니다. 지정 된 HWND가 이전에 동일한 스레드에서 AttachToWindow에 전달 된 HWND와 다른 최상위 창에서 계층이 이어집니다 AttachToWindow 메서드가 실패 했습니다. " | 이 오류는 응용 프로그램에서 **IDesktopWindowXamlSourceNative:: AttachToWindow** 메서드를 호출 하 고이 메서드에 대 한 이전 호출에서 지정한 창과 다른 최상위 창에서 계층이 이어집니다 하는 창의 HWND를 전달 했음을 나타냅니다. 동일한 스레드에서.</p></p>응용 프로그램이 특정 스레드에 대해 **AttachToWindow** 를 호출 하면 동일한 스레드의 다른 모든 [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체는에 대 한 첫 번째 호출에서 전달 된 것과 동일한 최상위 창의 하위 항목인 windows에만 연결할 수 있습니다. **AttachToWindow**. 모든 **desktopwindowxamlsource** 개체가 특정 스레드에 대해 닫히면 다음 **desktopwindowxamlsource** 를 사용 하 여 모든 창에 다시 연결할 수 있습니다.</p></p>이 문제를 해결 하려면이 스레드의 다른 최상위 창에 바인딩된 모든 **desktopwindowxamlsource** 개체를 닫거나이 **desktopwindowxamlsource**에 대 한 새 스레드를 만듭니다. |

## <a name="related-topics"></a>관련 항목

* [데스크톱 응용 프로그램의 UWP 컨트롤](xaml-islands.md)
* [C++Win32 XAML 아일랜드 샘플](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
