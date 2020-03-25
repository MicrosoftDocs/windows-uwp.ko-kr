---
description: 이 문서에서는 데스크톱 C++ Win32 앱에서 UWP XAML UI를 호스트 하는 방법을 설명 합니다.
title: C++ Win32 앱에서 UWP XAML 호스팅 API 사용
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, xaml 제도
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218463"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>C++ Win32 앱에서 UWP XAML 호스팅 API 사용

Windows 10, 버전 1903, 비 UWP 데스크톱 앱 (Win32, WPF 및 C++ Windows Forms 앱 포함)부터 *UWP XAML 호스팅 API* 를 사용 하 여 창 핸들 (HWND)과 연결 된 모든 UI 요소에서 uwp 컨트롤을 호스트할 수 있습니다. 이 API는 UWP 이외의 앱이 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 사용할 수 있도록 합니다. 예를 들어 비 UWP 데스크톱 앱은이 API를 사용 하 여 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/index) 을 사용 하 고 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)를 지 원하는 UWP 컨트롤을 호스트할 수 있습니다.

UWP XAML 호스팅 API는 개발자가 비 UWP 데스크톱 앱에 흐름 UI를 가져올 수 있도록 하는 광범위 한 컨트롤 집합에 대 한 토대를 제공 합니다. 이 기능을 *XAML 아일랜드*라고 합니다. 이 기능에 대 한 개요는 [데스크톱 앱에서 UWP xaml 컨트롤 호스트 (Xaml 아일랜드)](xaml-islands.md)를 참조 하세요.

> [!NOTE]
> XAML Island에 대한 피드백이 있는 경우 [Microsoft.Toolkit.Win32 리포지토리](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)에서 새 이슈를 만들고 의견을 남겨 주세요. 피드백을 개인적으로 제출하는 것을 선호하는 경우 XamlIslandsFeedback@microsoft.com으로 보낼 수 있습니다. 귀하의 인사이트와 시나리오는 Microsoft에 매우 중요합니다.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML 호스팅 API가 데스크톱 응용 프로그램에 적합 한 선택 인가요?

UWP XAML 호스팅 API는 데스크톱 앱에서 UWP 컨트롤을 호스팅하기 위한 하위 수준의 인프라를 제공 합니다. 일부 유형의 데스크톱 앱에는이 목표를 달성 하는 데 더 편리한 대체 Api를 사용할 수 있는 옵션이 있습니다.

* C++ Win32 데스크톱 앱이 있고 앱에서 uwp 컨트롤을 호스트 하려는 경우 uwp XAML 호스팅 API를 사용 해야 합니다. 이러한 유형의 앱에 대 한 대안이 없습니다.

* WPF 및 Windows Forms apps의 경우 UWP XAML 호스팅 API를 직접 사용 하는 대신 Windows Community Toolkit의 [Xaml 아일랜드 .net 컨트롤](xaml-islands.md#wpf-and-windows-forms-applications) 을 사용 하는 것이 좋습니다. 이러한 컨트롤은 내부적으로 UWP XAML 호스팅 API를 사용 하며, 키보드 탐색 및 레이아웃 변경을 포함 하 여 UWP XAML 호스팅 API를 직접 사용 하는 경우 직접 처리 해야 하는 모든 동작을 구현 합니다.

Win32 앱만 C++ UWP XAML 호스팅 API를 사용 하는 것이 좋지만이 문서에서는 주로 win32 앱에 대 C++ 한 지침과 예제를 제공 합니다. 그러나 선택 하는 경우 WPF에서 UWP XAML 호스팅 API를 사용 하 고 앱을 Windows Forms 수 있습니다. 이 문서에서는 Windows 커뮤니티 도구 키트에서 WPF 및 Windows Forms에 대 한 [호스트 컨트롤](xaml-islands.md#host-controls) 의 관련 소스 코드를 안내 하 여 UWP XAML 호스팅 API가 해당 컨트롤에서 어떻게 사용 되는지 확인할 수 있습니다.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>XAML 호스팅 API를 사용 하는 방법을 알아봅니다.

Win32 앱에서 C++ XAML 호스팅 API를 사용 하기 위한 코드 예제를 사용 하는 단계별 지침을 따르려면 다음 문서를 참조 하세요.

* [표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands-cpp.md)
* [사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)
* [고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>샘플

코드에서 UWP XAML 호스팅 API를 사용 하는 방법은 앱 유형, 앱 디자인 및 기타 요인에 따라 달라 집니다. 전체 앱의 컨텍스트에서이 API를 사용 하는 방법을 설명 하기 위해이 문서에서는 다음 샘플의 코드를 참조 합니다.

### <a name="c-win32"></a>C++(Win32

다음 샘플에서는 C++ Win32 앱에서 UWP XAML 호스팅 API를 사용 하는 방법을 보여 줍니다.

* [간단한 XAML 아일랜드 샘플](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)입니다. 이 샘플에서는 패키지 되지 않은 C++ Win32 앱 (즉, msix 패키지에 내장 되지 않은 앱)에서 UWP 컨트롤을 호스트 하는 기본 구현을 보여 줍니다.

* [사용자 지정 컨트롤 샘플을 사용 하는 XAML 아일랜드](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). 이 샘플은 패키지 C++ 된 Win32 앱에서 사용자 지정 UWP 컨트롤을 호스트 하는 전체 구현을 보여 주고 키보드 입력 및 포커스 탐색과 같은 기타 동작을 처리 하는 방법을 보여 줍니다.

### <a name="wpf-and-windows-forms"></a>WPF 및 Windows Forms

Windows Community Toolkit의 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤은 WPF 및 Windows Forms APPS에서 UWP 호스팅 API를 사용 하기 위한 참조 샘플로 사용 됩니다. 소스 코드는 다음 위치에서 사용할 수 있습니다.

* WPF 버전 컨트롤의 경우 [여기로 이동](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)합니다. WPF 버전은 [system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 파생 됩니다.

* 컨트롤의 Windows Forms 버전을 [보려면 여기로 이동](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)하세요. Windows Forms 버전은 [system.object](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)에서 파생 됩니다.

> [!NOTE]
> WPF 및 Windows Forms apps에서 UWP XAML 호스팅 API를 직접 사용 하는 대신 Windows Community Toolkit의 [Xaml 아일랜드 .net 컨트롤](xaml-islands.md#wpf-and-windows-forms-applications) 을 사용 하는 것이 좋습니다. 이 문서의 WPF 및 Windows Forms 샘플 링크는 설명을 위한 목적 으로만 제공 됩니다.

## <a name="architecture-of-the-api"></a>API의 아키텍처

UWP XAML 호스팅 API는 이러한 기본 Windows 런타임 형식 및 COM 인터페이스를 포함 합니다.

|  형식 또는 인터페이스 | 설명 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 이 클래스는 UWP XAML 프레임 워크를 나타냅니다. 이 클래스는 데스크톱 응용 프로그램의 현재 스레드에서 UWP XAML 프레임 워크를 초기화 하는 단일 정적 [Initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 메서드를 제공 합니다. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 이 클래스는 데스크톱 앱에서 호스트 하는 UWP XAML 콘텐츠의 인스턴스를 나타냅니다. 이 클래스의 가장 중요 한 멤버는 [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성입니다. 이 속성은 호스팅할를 지정 하는 [Windows.](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 이 클래스에는 XAML 아일랜드에 대 한 키보드 포커스 탐색을 라우팅하는 다른 멤버도 있습니다. |
| IDesktopWindowXamlSourceNative | 이 COM 인터페이스는 응용 프로그램의 XAML 아일랜드를 부모 UI 요소에 연결 하는 데 사용 하는 **AttachToWindow** 메서드를 제공 합니다. 모든 **Desktopwindowxamlsource** 개체는이 인터페이스를 구현 합니다. 이 인터페이스는 windows. .xaml. x x x. x. x. x. x. h. |
| IDesktopWindowXamlSourceNative2 | 이 COM 인터페이스는 UWP XAML 프레임 워크가 특정 Windows 메시지를 올바르게 처리할 수 있도록 하는 **PreTranslateMessage** 메서드를 제공 합니다. 모든 **Desktopwindowxamlsource** 개체는이 인터페이스를 구현 합니다. 이 인터페이스는 windows. .xaml. x x x. x. x. x. x. h. |

다음 다이어그램은 데스크톱 응용 프로그램에서 호스팅되는 XAML 아일랜드의 개체 계층 구조를 보여 줍니다.

* 기본 수준에서는 XAML 아일랜드를 호스팅할 응용 프로그램의 UI 요소입니다. 이 UI 요소에는 창 핸들 (HWND)이 있어야 합니다. C++ Win32의 [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)를 포함하는 XAML 아일랜드를 호스팅할 수 있는 UI 요소에 대한 예로는 WPE 앱용 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 및 Windows Forms 앱용 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)이 있습니다.

* 다음 수준은 **Desktopwindowxamlsource** 개체입니다. 이 개체는 XAML 아일랜드 호스팅을 위한 인프라를 제공 합니다. 코드에서이 개체를 만들어 부모 UI 요소에 연결 하는 일을 담당 합니다.

* **Desktopwindowxamlsource**를 만들 때이 개체는 자동으로 네이티브 자식 창을 만들어 UWP 컨트롤을 호스팅합니다. 이 네이티브 자식 창은 주로 코드에서 추상화 되지만 필요한 경우 해당 핸들 (HWND)에 액세스할 수 있습니다.

* 마지막으로, 최상위 수준에는 데스크톱 앱에서 호스트 하려는 UWP 컨트롤이 있습니다. 이는 Windows SDK에서 제공 하는 모든 UWP 컨트롤과 사용자 지정 사용자 정의 컨트롤을 포함 하 여 [Windows. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생 되는 모든 uwp 개체 일 수 있습니다.

![DesktopWindowXamlSource 아키텍처](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> 데스크톱 앱에서 XAML 아일랜드를 호스팅하는 경우 동일한 스레드에서 여러 개의 XAML 콘텐츠 트리를 동시에 실행할 수 있습니다. XAML 아일랜드에서 XAML 콘텐츠 트리의 루트 요소에 액세스하고 해당 요소가 호스팅되는 컨텍스트에 대한 관련 정보를 가져오려면 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 클래스를 사용합니다. [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [Applicationview](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)및 [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) api는 XAML 아일랜드에 대 한 올바른 정보를 제공 하지 않습니다. 자세한 내용은 [이 섹션](xaml-islands.md#window-host-context-for-xaml-islands)을 참조하세요.

## <a name="troubleshooting"></a>문제 해결

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>UWP 앱에서 UWP XAML 호스팅 API를 사용 하는 동안 오류 발생

| 문제 | 해상도 |
|-------|------------|
| 앱이 "DesktopWindowXamlSource를 활성화할 수 없습니다." 라는 메시지와 함께 **COMException** 를 수신 합니다. 이 형식은 UWP 앱에서 사용할 수 없습니다. " 또는 "WindowsXamlManager를 활성화할 수 없습니다. 이 형식은 UWP 앱에서 사용할 수 없습니다. " | 이 오류는 uwp 앱에서 UWP XAML 호스팅 API (특히 [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 또는 [windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 유형을 인스턴스화하려고 시도)를 사용 하 고 있음을 나타냅니다. UWP XAML 호스팅 API는 WPF, Windows Forms 및 C++ Win32 응용 프로그램과 같은 비 UWP 데스크톱 앱 에서만 사용할 수 있습니다. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>WindowsXamlManager 또는 DesktopWindowXamlSource 유형을 사용 하는 동안 오류 발생

| 문제 | 해상도 |
|-------|------------|
| 앱이 다음 메시지와 함께 예외를 수신 합니다. "WindowsXamlManager 및 DesktopWindowXamlSource는 Windows 버전 10.0.18226.0 이상을 대상으로 하는 앱에 대해 지원 됩니다. 응용 프로그램 매니페스트 또는 패키지 매니페스트를 확인 하 고 MaxTestedVersion 속성이 업데이트 되었는지 확인 하십시오. " | 이 오류는 응용 프로그램이 UWP XAML 호스팅 API에서 **Windowsxamlmanager** 또는 **Desktopwindowxamlsource** 형식을 사용 하려고 했지만, OS에서 Windows 10 버전 1903 이상을 대상으로 하는 앱을 빌드 했는지 여부를 확인할 수 없음을 나타냅니다. UWP XAML 호스팅 API는 이전 버전의 Windows 10에서 미리 보기로 처음 도입 되었지만 Windows 10 버전 1903부터만 지원 됩니다.</p></p>이 문제를 해결 하려면 앱에 대 한 MSIX 패키지를 만들고 패키지에서 실행 하거나 프로젝트에 [Microsoft Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) . n a m a. n a m a 패키지를 설치 합니다.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>다른 스레드의 창에 연결 하는 동안 오류가 발생 했습니다.

| 문제 | 해상도 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** 를 수신 합니다. "지정 된 HWND가 다른 스레드에 생성 되었으므로 AttachToWindow 메서드가 실패 했습니다." | 이 오류는 응용 프로그램에서 **IDesktopWindowXamlSourceNative:: AttachToWindow** 메서드를 호출 하 고 다른 스레드에서 만든 창의 HWND를 전달 했음을 나타냅니다. 이 메서드는 메서드를 호출 하는 코드와 동일한 스레드에 생성 된 창의 HWND를 전달 해야 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>다른 최상위 창의 창에 연결 하는 동안 오류가 발생 했습니다.

| 문제 | 해상도 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** 를 수신 합니다. 지정 된 hwnd가 이전에 동일한 스레드의 AttachToWindow에 전달 된 hwnd와 다른 최상위 창에서 계층이 이어집니다 때문에 "AttachToWindow 메서드가 실패 했습니다." | 이 오류는 응용 프로그램에서 **IDesktopWindowXamlSourceNative:: AttachToWindow** 메서드를 호출 하 고, 동일한 스레드에서이 메서드에 대 한 이전 호출에서 지정한 창과 다른 최상위 창에서 계층이 이어집니다 하는 창의 HWND를 전달 했음을 나타냅니다.</p></p>응용 프로그램이 특정 스레드에 대해 **AttachToWindow** 를 호출 하면 동일한 스레드에 있는 다른 모든 [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체는 **AttachToWindow**에 대 한 첫 번째 호출에서 전달 된 것과 동일한 최상위 창의 하위 항목인 windows에만 연결할 수 있습니다. 모든 **desktopwindowxamlsource** 개체가 특정 스레드에 대해 닫히면 다음 **desktopwindowxamlsource** 를 사용 하 여 모든 창에 다시 연결할 수 있습니다.</p></p>이 문제를 해결 하려면이 스레드의 다른 최상위 창에 바인딩된 모든 **desktopwindowxamlsource** 개체를 닫거나이 **desktopwindowxamlsource**에 대 한 새 스레드를 만듭니다. |

## <a name="related-topics"></a>관련 항목

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트 (XAML 제도)](xaml-islands.md)
* [C++ Win32 앱에서 표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)
* [Win32 앱의 C++ XAML 아일랜드에 대 한 고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 아일랜드 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML 아일랜드 샘플](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
