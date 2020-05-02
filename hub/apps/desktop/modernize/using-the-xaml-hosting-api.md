---
description: 이 문서에서는 데스크톱 C++ Win32 앱에서 UWP XAML UI를 호스트하는 방법을 설명합니다.
title: C++ Win32 앱에서 UWP XAML 호스팅 API 사용
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80218463"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>C++ Win32 앱에서 UWP XAML 호스팅 API 사용

Windows 10, 버전 1903부터, 비 UWP 데스크톱 앱(C++ Win32, WPF 및 Windows Forms 앱 포함)은 *UWP XAML 호스팅 API*를 사용하여 창 핸들(HWND)과 연결된 모든 UI 요소에서 UWP 컨트롤을 호스트할 수 있습니다. 이 API에서는 UWP 이외의 앱이 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 사용할 수 있습니다. 예를 들어 비 UWP 데스크톱 앱은 이 API를 사용하여 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/index)을 사용하고 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)를 지원하는 UWP 컨트롤을 호스트할 수 있습니다.

UWP XAML 호스팅 API는 개발자가 비 UWP 데스크톱 앱에 흐름 UI를 가져올 수 있도록 하는 광범위한 컨트롤 세트의 토대를 제공합니다. 이 기능을 *XAML Islands*라고 합니다. 이 기능의 개요는 [데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)](xaml-islands.md)를 참조하세요.

> [!NOTE]
> XAML Island에 대한 피드백이 있는 경우 [Microsoft.Toolkit.Win32 리포지토리](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)에서 새 이슈를 만들고 의견을 남겨 주세요. 피드백을 개인적으로 제출하는 것을 선호하는 경우 XamlIslandsFeedback@microsoft.com으로 보낼 수 있습니다. 귀하의 인사이트와 시나리오는 Microsoft에 매우 중요합니다.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML 호스팅 API는 데스크톱 애플리케이션에 적합한 선택인가요?

UWP XAML 호스팅 API는 데스크톱 앱에서 UWP 컨트롤을 호스트하기 위한 하위 수준의 인프라를 제공합니다. 일부 유형의 데스크톱 앱에서는 이 목표를 달성하기 위해 좀 더 편리한 다른 API를 사용할 수 있습니다.

* C++ Win32 데스크톱 앱이 있고 앱에서 UWP 컨트롤을 호스트하려는 경우 UWP XAML 호스팅 API를 사용해야 합니다. 이러한 유형의 앱에는 대안이 없습니다.

* WPF 및 Windows Forms 앱의 경우, UWP XAML 호스팅 API를 직접 사용하는 대신, Windows 커뮤니티 도구 키트에서 [XAML Islands .NET 컨트롤](xaml-islands.md#wpf-and-windows-forms-applications)을 사용하는 것이 좋습니다. 이러한 컨트롤은 내부적으로 UWP XAML 호스팅 API를 사용하고, 사용자가 UWP XAML 호스팅 API를 직접 사용한 경우 키보드 탐색 및 레이아웃 변경을 포함하여 직접 처리해야 하는 모든 동작을 구현합니다.

C++ Win32 앱만 C++ UWP XAML 호스팅 API를 사용하는 것이 좋기 때문에 이 문서에서는 주로 C++ Win32 앱에 대한 지침과 예제를 제공합니다. 그러나 선택하는 경우 WPF 및 Windows Forms 앱에서 UWP XAML 호스팅 API를 사용할 수 있습니다. 이 문서에서는 Windows 커뮤니티 도구 키트의 WPF 및 Windows Forms에 대한 [호스트 컨트롤](xaml-islands.md#host-controls) 관련 소스 코드를 제공하므로 UWP XAML 호스팅 API가 해당 컨트롤에서 어떻게 사용되는지 확인할 수 있습니다.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>XAML 호스팅 API를 사용하는 방법 알아보기

C++ Win32 앱에서 XAML 호스팅 API를 사용하기 위한 코드 예제를 통해 단계별 지침을 따르려면 다음 문서를 참조하세요.

* [표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands-cpp.md)
* [사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)
* [고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>샘플

코드에서 UWP XAML 호스팅 API를 사용하는 방법은 앱 유형, 앱 디자인 및 기타 요인에 따라 달라집니다. 전체 앱의 컨텍스트에서 이 API를 사용하는 방법을 설명하기 위해 이 문서에서는 다음 샘플의 코드를 참조합니다.

### <a name="c-win32"></a>C++ Win32

다음 샘플에서는 C++ Win32 앱에서 UWP XAML 호스팅 API를 사용하는 방법을 보여 줍니다.

* [간단한 XAML Island 샘플](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)입니다. 이 샘플에서는 패키지되지 않은 C++ Win32 앱(즉, MSIX 패키지에 기본적으로 제공되는 앱)에서 UWP 컨트롤을 호스트하는 기본 구현을 보여 줍니다.

* [사용자 지정 컨트롤 샘플이 있는 XAML Island](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32) 이 샘플은 패키지된 C++ Win32 앱에서 사용자 지정 UWP 컨트롤을 호스트하는 전체 구현과, 키보드 입력 및 포커스 탐색과 같은 기타 동작을 처리하는 방법을 보여 줍니다.

### <a name="wpf-and-windows-forms"></a>WPF 및 Windows Forms

Windows 커뮤니티 도구 키트의 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤은 WPF 및 Windows Forms 앱에서 UWP 호스팅 API를 사용하기 위한 참조 샘플로 사용됩니다. 소스 코드는 다음 위치에서 확인할 수 있습니다.

* WPF 버전 컨트롤의 경우 [여기로 이동](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)합니다. WPF 버전은 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 파생됩니다.

* Windows Forms 버전 컨트롤을 보려면 [여기로 이동](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)합니다. Windows Forms 버전은 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)에서 파생됩니다.

> [!NOTE]
> WPF 및 Windows Forms 앱에서는 UWP XAML 호스팅 API를 직접 사용하는 대신, Windows 커뮤니티 도구 키트에서 [XAML Islands .NET 컨트롤](xaml-islands.md#wpf-and-windows-forms-applications)을 사용하는 것이 좋습니다. 이 문서의 WPF 및 Windows Forms 샘플 링크는 설명을 위한 목적으로만 제공됩니다.

## <a name="architecture-of-the-api"></a>API의 아키텍처

UWP XAML 호스팅 API는 이러한 기본 Windows 런타임 형식 및 COM 인터페이스를 포함합니다.

|  형식 또는 인터페이스 | 설명 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 이 클래스는 UWP XAML 프레임워크를 나타냅니다. 이 클래스는 데스크톱 앱의 현재 스레드에서 UWP XAML 프레임워크를 초기화하는 단일 정적 [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 메서드를 제공합니다. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 이 클래스는 데스크톱 앱에서 호스트하는 UWP XAML 콘텐츠의 인스턴스를 나타냅니다. 이 클래스의 가장 중요한 멤버는 [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성입니다. 이 속성을 호스트하려는 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에 할당합니다. 이 클래스에는 키보드 포커스 탐색을 XAML Islands로/부터 라우팅하는 다른 멤버도 있습니다. |
| IDesktopWindowXamlSourceNative | 이 COM 인터페이스는 앱의 XAML Island를 부모 UI 요소에 연결하는 데 사용하는 **AttachToWindow** 메서드를 제공합니다. 모든 **DesktopWindowXamlSource** 개체는 이 인터페이스를 구현합니다. 이 인터페이스는 windows.ui.xaml.hosting.desktopwindowxamlsource.h에서 선언됩니다. |
| IDesktopWindowXamlSourceNative2 | 이 COM 인터페이스는 UWP XAML 프레임워크에서 특정 Windows 메시지를 올바르게 처리할 수 있도록 하는 **PreTranslateMessage** 메서드를 제공합니다. 모든 **DesktopWindowXamlSource** 개체는 이 인터페이스를 구현합니다. 이 인터페이스는 windows.ui.xaml.hosting.desktopwindowxamlsource.h에서 선언됩니다. |

다음 다이어그램은 데스크톱 앱에 호스트되는 XAML Island의 개체 계층 구조를 보여 줍니다.

* 기본 수준에는 XAML Island를 호스트할 앱의 UI 요소가 있습니다. 이 UI 요소에는 창 핸들(HWND)이 있어야 합니다. C++ Win32의 [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)를 포함하는 XAML 아일랜드를 호스팅할 수 있는 UI 요소에 대한 예로는 WPE 앱용 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 및 Windows Forms 앱용 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)이 있습니다.

* 다음 수준에는 **DesktopWindowXamlSource** 개체가 있습니다. 이 개체는 XAML Island를 호스트하기 위한 인프라를 제공합니다. 코드는 이 개체를 만들고 부모 UI 요소에 연결하는 일을 담당합니다.

* **DesktopWindowXamlSource**를 만들 때 이 개체는 자동으로 네이티브 자식 창을 만들어 UWP 컨트롤을 호스트합니다. 이 네이티브 자식 창은 주로 코드에서 추상화되지만 필요한 경우 해당 핸들(HWND)에 액세스할 수 있습니다.

* 마지막으로, 최상위 수준에는 데스크톱 앱에서 호스트하려는 UWP 컨트롤이 있습니다. Windows SDK에서 제공하는 UWP 컨트롤과 사용자 지정 컨트롤을 포함하여 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생되는 UWP 개체일 수 있습니다.

![DesktopWindowXamlSource 아키텍처](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> 데스크톱 앱에서 XAML 아일랜드를 호스팅하는 경우 동일한 스레드에서 여러 개의 XAML 콘텐츠 트리를 동시에 실행할 수 있습니다. XAML 아일랜드에서 XAML 콘텐츠 트리의 루트 요소에 액세스하고 해당 요소가 호스팅되는 컨텍스트에 대한 관련 정보를 가져오려면 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 클래스를 사용합니다. [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) 및 [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) API는 XAML Islands에 대한 올바른 정보를 제공하지 않습니다. 자세한 내용은 [이 섹션](xaml-islands.md#window-host-context-for-xaml-islands)을 참조하세요.

## <a name="troubleshooting"></a>문제 해결

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>UWP 앱에서 UWP XAML 호스팅 API를 사용하는 동안 오류 발생

| 문제 | 해결 방법 |
|-------|------------|
| 앱은 다음 메시지와 함께 **COMException**을 수신합니다. "DesktopWindowXamlSource를 활성화할 수 없습니다. 이 형식은 UWP 앱에서 사용할 수 없습니다." 또는 "WindowsXamlManager를 활성화할 수 없습니다. 이 형식은 UWP 앱에서 사용할 수 없습니다." | 이 오류는 UWP 앱에서 UWP XAML 호스팅 API를 사용하려고 할 때, 특히 [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 또는 [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 형식을 인스턴스화하려고 함을 나타냅니다. UWP XAML 호스팅 API는 WPF, Windows Forms 및 C++ Win32 애플리케이션과 같은 비 UWP 데스크톱 앱에서만 사용할 수 있습니다. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>WindowsXamlManager 또는 DesktopWindowXamlSource 형식을 사용하는 동안 오류 발생

| 문제 | 해결 방법 |
|-------|------------|
| 앱은 다음 메시지와 함께 예외를 수신합니다. "WindowsXamlManager 및 DesktopWindowXamlSource는 Windows 버전 10.0.18226.0 이상을 대상으로 하는 앱에 대해 지원됩니다. 애플리케이션 매니페스트 또는 패키지 매니페스트를 확인하고 MaxTestedVersion 속성이 업데이트되었는지 확인하세요." | 이 오류는 애플리케이션에서 UWP XAML 호스팅 API의 **WindowsXamlManager** 또는 **DesktopWindowXamlSource** 형식을 사용하려고 했지만, OS에서 Windows 10 버전 1903 이상을 대상으로 하는 앱을 빌드했는지 여부를 확인할 수 없음을 나타냅니다. UWP XAML 호스팅 API는 이전 버전의 Windows 10에서 미리 보기로 처음 도입되었지만 Windows 10 버전 1903부터만 지원됩니다.</p></p>이 문제를 해결하려면 앱에 대한 MSIX 패키지를 만들고 패키지에서 실행하거나 프로젝트에 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 패키지를 설치합니다.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>다른 스레드의 창에 연결하는 동안 오류 발생

| 문제 | 해결 방법 |
|-------|------------|
| 앱은 다음 메시지와 함께 **COMException**을 수신합니다. "지정된 HWND가 다른 스레드에 생성되었으므로 AttachToWindow 메서드가 실패했습니다." | 이 오류는 애플리케이션이 **IDesktopWindowXamlSourceNative::AttachToWindow** 메서드를 호출하고 다른 스레드에서 만든 창의 HWND를 전달했음을 나타냅니다. 이 메서드는 메서드를 호출하는 코드와 동일한 스레드에서 생성된 창의 HWND를 전달해야 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>다른 최상위 창에서 창에 연결하는 동안 오류 발생

| 문제 | 해결 방법 |
|-------|------------|
| 앱은 다음 메시지와 함께 **COMException**을 수신합니다. "지정된 HWND가 이전에 동일한 스레드에서 AttachToWindow에 전달된 HWND와 다른 최상위 창의 하위 항목이므로 AttachToWindow 메서드가 실패했습니다." | 이 오류는 애플리케이션이 **IDesktopWindowXamlSourceNative::AttachToWindow** 메서드를 호출하고, 동일한 스레드에서 이 메서드에 대한 이전 호출에서 지정한 창과는 다른 최상위 창의 하위에 있는 창의 HWND로 전달되었음을 나타냅니다.</p></p>애플리케이션이 특정 스레드에서 **AttachToWindow**를 호출하면 같은 스레드의 다른 모든 [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체는 **AttachToWindow**에 대한 첫 번째 호출에서 전달된 것과 동일한 최상위 창의 하위에 있는 창에만 연결할 수 있습니다. 모든 **DesktopWindowXamlSource** 개체가 특정 스레드에 대해 닫히면 다음 **DesktopWindowXamlSource**를 사용하여 임의 창에 다시 연결할 수 있습니다.</p></p>이 문제를 해결하려면 이 스레드의 다른 최상위 창에 바인딩된 **DesktopWindowXamlSource** 개체를 모두 닫거나 이 **DesktopWindowXamlSource**에 대한 새 스레드를 만듭니다. |

## <a name="related-topics"></a>관련 항목

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)](xaml-islands.md)
* [C++ Win32 앱에서 표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 앱의 XAML Islands에 대한 고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML Islands 샘플](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
