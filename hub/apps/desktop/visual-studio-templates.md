---
description: 이 문서에서는 Windows 앱용 Visual Studio 프로젝트 및 항목 템플릿에 대해 간략히 설명합니다.
title: Windows 앱용 Visual Studio 프로젝트 및 항목 템플릿
ms.date: 07/02/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.openlocfilehash: 30f190b43b9156a92cdaf2ad533ededfc79c3a74
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86494085"
---
# <a name="visual-studio-project-and-item-templates-for-windows-apps"></a>Windows 앱용 Visual Studio 프로젝트 및 항목 템플릿

Visual Studio 2019는 C\# 또는 C++를 사용하여 Windows 10 디바이스용 앱을 빌드하는 데 도움이 되는 다양한 프로젝트 및 항목 템플릿을 제공합니다. 이 항목에서는 템플릿에 대해 설명하며, 시나리오에 적합한 템플릿을 선택할 수 있도록 도움을 줍니다.

* 프로젝트 템플릿에는 애플리케이션 또는 앱에서 로드하고 사용할 수 있는 앱 또는 구성 요소를 빌드하도록 구성된 프로젝트 파일, 코드 파일 및 기타 자산이 포함됩니다.
* 항목 템플릿은 일반적으로 사용되는 코드와 XAML이 포함된 프로젝트 파일이며, 개발 시간을 줄이기 위해 프로젝트에 추가할 수 있습니다. 예를 들어 항목 템플릿을 사용하여 새 창, 페이지 또는 컨트롤을 앱에 추가할 수 있습니다.

## <a name="winui-templates"></a>WinUI 템플릿

[WinUI(Windows UI) 라이브러리](../winui/index.md)는 데스크톱(.NET 및 네이티브 Win32)과 UWP 앱 플랫폼에서 Windows 앱을 위한 최신 네이티브 UI(사용자 인터페이스) 플랫폼입니다. [WinUI 3](../winui/winui3/index.md)(현재 개발자 미리 보기로 제공)은 WinUI의 최신 주요 버전이며, WinUI를 완전한 데스크톱 Windows 앱용 UX 프레임워크로 전환합니다.

WinUI 3에는 Visual Studio 2019용 VSIX 패키지가 포함되어 있습니다. 이 패키지는 WinUI 기반 인터페이스를 사용하여 앱 빌드를 시작하는 데 도움이 되는 프로젝트와 항목 템플릿을 제공합니다. WinUI 3 VSIX 패키지 및 이 패키지에서 제공하는 프로젝트 템플릿에 대한 자세한 내용은 [이 섹션](../winui/winui3/index.md#install-winui-3-preview-2)을 참조하세요.

> [!IMPORTANT]
> 관련 Visual Studio 템플릿을 포함한 WinUI 3은 현재 개발자 커뮤니티에서 초기에 평가하고 피드백을 수집하기 위한 개발자 미리 보기로 제공됩니다. 현재 프로덕션 앱에는 사용하면 안 됩니다.

## <a name="uwp-templates"></a>UWP 템플릿

Visual Studio는 C# 또는 C++를 사용하여 UWP 앱을 빌드하기 위한 다양한 프로젝트 템플릿을 제공합니다. 이러한 프로젝트 템플릿을 사용하려면 Visual Studio를 설치할 때 **유니버설 Windows 플랫폼 개발** 워크로드를 포함해야 합니다. C++ 프로젝트 템플릿의 경우 **유니버설 Windows 플랫폼 개발** 워크로드용 선택적 **C++(v142) 유니버설 Windows 플랫폼 도구** 구성 요소도 포함해야 합니다.

### <a name="project-templates-for-c-and-uwp"></a>C# 및 UWP용 프로젝트 템플릿

Visual Studio에서 새 프로젝트를 만들 때 UWP C# 프로젝트 템플릿에 액세스하려면 언어를 **C#** 으로, 플랫폼을 **Windows**로, 프로젝트 형식을 **UWP**로 필터링합니다.

![UWP C# 프로젝트 템플릿](images/uwp-projects-csharp.png)

C# UWP 앱을 만드는 데 사용할 수 있는 프로젝트 템플릿은 다음과 같습니다.

| 템플릿 | 설명 |
|----------|-------------|
| 빈 앱(유니버설 Windows) | UWP 앱을 만듭니다. 생성되는 프로젝트에는 UI 빌드를 시작하는 데 사용할 수 있는 [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 클래스에서 파생되는 기본 페이지가 포함됩니다. |
| 단위 테스트 앱(유니버설 Windows) | C#에서 UWP 앱용 단위 테스트 프로젝트를 만듭니다. 자세한 내용은 [C# 코드 단위 테스트](https://docs.microsoft.com/visualstudio/test/unit-testing-visual-csharp-code-in-a-store-app)를 참조하세요. |

C# UWP 앱의 일부를 빌드하는 데 사용할 수 있는 프로젝트 템플릿은 다음과 같습니다.

| 템플릿 | 설명 |
|----------|-------------|
| 클래스 라이브러리(유니버설 Windows) | C#에서 관리 코드로 작성된 다른 UWP 앱에서 사용할 수 있는 관리형 클래스 라이브러리(DLL)를 만듭니다. |
| Windows 런타임 구성 요소(유니버설 Windows) | C#에서 앱이 작성된 프로그래밍 언어에 관계없이 모든 UWP 앱에서 사용할 수 있는 [Windows 런타임 구성 요소](https://docs.microsoft.com/windows/uwp/winrt-components/)를 만듭니다. |
| 선택적 코드 패키지(유니버설 Windows) | 앱에서 로드할 수 있는 C# 실행 코드를 사용하여 선택적 패키지를 만듭니다. 자세한 내용은 [실행 코드가 있는 선택적 패키지](https://docs.microsoft.com/windows/msix/package/optional-packages-with-executable-code)를 참조하세요.  |

### <a name="project-templates-for-c-and-uwp"></a>C++ 및 UWP용 프로젝트 템플릿

C++ UWP 앱을 빌드하는 데 사용할 수 있는 두 가지 기술이 있습니다.

* 추천 기술은 [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)입니다. 이는 전적으로 헤더 파일에서만 구현되는 C++ 언어 프로젝션이며, 최신 WinRT API에 대한 최고 수준의 액세스를 제공하도록 설계되었습니다.
* 또는 이전의 [C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx) 확장 세트를 사용할 수 있습니다. C++/CX는 계속 지원되지만 C++/WinRT를 대신 사용하는 것이 좋습니다.

Visual Studio에서 새 프로젝트를 만들 때 UWP C++ 프로젝트 템플릿에 액세스하려면 언어를 **C++** 로, 플랫폼을 **Windows**로, 프로젝트 형식을 **UWP**로 필터링합니다. 

> [!NOTE]
> Visual Studio의 **유니버설 Windows 플랫폼 개발** 워크로드는 기본적으로 C++/CX 프로젝트 템플릿에 대한 액세스만 제공합니다. C++/WinRT 프로젝트 템플릿에 액세스하려면 [C++/WinRT VSIX](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 패키지를 설치해야 합니다.

![UWP C++ 프로젝트 템플릿](images/uwp-projects-cpp.png)

C++ UWP 앱을 만드는 데 사용할 수 있는 프로젝트 템플릿은 다음과 같습니다.

| 템플릿 | 설명 |
|----------|-------------|
| 비어 있는 앱(C++/WinRT) | XAML 사용자 인터페이스를 사용하여 C++/WinRT UWP 앱을 만듭니다. 생성되는 프로젝트에는 UI 빌드를 시작하는 데 사용할 수 있는 [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 클래스에서 파생되는 기본 페이지가 포함됩니다. |
| 주요 앱(C++/WinRT) | XAML 사용자 인터페이스 대신 [CoreApplication](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication)을 사용하여 다양한 UI 프레임워크와 통합하는 C++/WinRT UWP 앱을 만듭니다. 이 프로젝트 템플릿을 사용하여 DirectX를 사용하는 간단한 게임을 만드는 방법을 보여 주는 연습은 [DirectX를 사용하여 간단한 UWP 게임 만들기](https://docs.microsoft.com/windows/uwp/gaming/tutorial--create-your-first-uwp-directx-game)를 참조하세요. |
| 빈 앱(유니버설 Windows - C++/CX) | XAML 사용자 인터페이스를 사용하여 C++/WinRT UWP 앱을 만듭니다. 생성되는 프로젝트에는 UI 빌드를 시작하는 데 사용할 수 있는 WinUI 라이브러리의 [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 클래스에서 파생되는 기본 페이지가 포함됩니다. |
| DirectX 11 및 XAML 앱(유니버설 Windows - C++/CX) | XAML UI 컨트롤을 사용할 수 있도록 DirectX 11 및 **SwapChainPanel**을 사용하는 UWP 앱을 만듭니다. 자세한 내용은 [DirectX 게임 프로젝트 템플릿](https://docs.microsoft.com/windows/uwp/gaming/user-interface)을 참조하세요. |
| DirectX 11 앱(유니버설 Windows - C++/CX) | DirectX 11을 사용하는 UWP 앱을 만듭니다. 자세한 내용은 [DirectX 게임 프로젝트 템플릿](https://docs.microsoft.com/windows/uwp/gaming/user-interface)을 참조하세요. |
| DirectX 12 앱(유니버설 Windows - C++/CX) | DirectX 12를 사용하는 UWP 앱을 만듭니다. 자세한 내용은 [DirectX 게임 프로젝트 템플릿](https://docs.microsoft.com/windows/uwp/gaming/user-interface)을 참조하세요. |
| 단위 테스트 앱(유니버설 Windows - C++/CX) | C++/CX에서 UWP 앱용 단위 테스트 프로젝트를 만듭니다. 자세한 내용은 [C++ UWP DLL을 테스트하는 방법](https://docs.microsoft.com/visualstudio/test/unit-testing-a-visual-cpp-dll-for-store-apps)을 참조하세요. |

C++ UWP 앱의 일부를 빌드하는 데 사용할 수 있는 프로젝트 템플릿은 다음과 같습니다.

| 템플릿 | 설명 |
|----------|-------------|
| Windows 런타임 구성 요소(C++/WinRT) | C++/WinRT에서 앱이 작성된 프로그래밍 언어에 관계없이 모든 UWP 앱에서 사용할 수 있는 [Windows 런타임 구성 요소](https://docs.microsoft.com/windows/uwp/winrt-components/)를 만듭니다. |
| Windows 런타임 구성 요소(유니버설 Windows) | C++/CX에서 앱이 작성된 프로그래밍 언어에 관계없이 모든 UWP 앱에서 사용할 수 있는 [Windows 런타임 구성 요소](https://docs.microsoft.com/windows/uwp/winrt-components/)를 만듭니다. |
| DLL(유니버설 Windows) | C++/CX에서 UWP 앱에서 사용할 수 있는 DLL(동적 연결 라이브러리)을 만드는 프로젝트입니다. 자세한 내용은 [DLL(C++/CX)](https://docs.microsoft.com/cpp/cppcx/dlls-c-cx)을 참조하세요. |
| 정적 라이브러리(유니버설 Windows) | C++/CX에서 UWP 앱에서 사용할 수 있는 정적 라이브러리(LIB)를 만드는 프로젝트입니다. 자세한 내용은 [정적 라이브러리(C++/CX)](https://docs.microsoft.com/cpp/cppcx/static-libraries-c-cx)를 참조하세요. |

## <a name="cwin32-templates"></a>C++/Win32 템플릿

Visual Studio는 네이티브 C++를 사용하고 Win32 API에 직접 액세스하여 데스크톱 Windows 앱을 빌드하기 위한 다양한 프로젝트 템플릿을 제공합니다. 이러한 프로젝트 템플릿을 사용하려면 Visual Studio를 설치할 때 **C++를 사용한 데스크톱 개발** 워크로드를 포함해야 합니다. 이 워크로드에는 데스크톱 앱, 콘솔 앱 및 라이브러리를 빌드하는 프로젝트 템플릿이 포함되어 있습니다.

### <a name="project-templates-for-desktop-apps"></a>데스크톱 앱용 프로젝트 템플릿

Visual Studio에서 새 프로젝트를 만들 때 클래식 데스크톱 앱용 C++ 프로젝트 템플릿에 액세스하려면 언어를 **C++** 로, 플랫폼을 **Windows**로, 프로젝트 형식을 **데스크톱**으로 필터링합니다.

![네이티브 C++ 앱 프로젝트 템플릿](images/desktop-app-projects-cpp.png)

| 템플릿 | 설명 |
|----------|----------|
| Windows 데스크톱 애플리케이션 | C++를 사용하여 클래식 Windows 데스크톱 앱을 만듭니다. 자세한 내용은 [연습: 기존 Windows 데스크톱 애플리케이션 만들기](https://docs.microsoft.com/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)를 참조하세요. |
| Windows 데스크톱 마법사 | 클래식 Windows 데스크톱 앱, 콘솔 앱, DLL(동적 연결 라이브러리) 또는 정적 라이브러리와 같은 프로젝트 형식 중 하나를 만드는 데 사용할 수 있는 단계별 마법사를 제공합니다. 자세한 내용은 [Windows 데스크톱 마법사](https://docs.microsoft.com/cpp/windows/windows-desktop-wizard) 및 [연습: 기존 Windows 데스크톱 애플리케이션 만들기](https://docs.microsoft.com/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)를 참조하세요.         |
| Windows 애플리케이션 패키징 프로젝트 | 데스크톱 앱을 [MSIX 패키지](https://docs.microsoft.com/windows/msix/overview)로 빌드하는 데 사용할 수 있는 프로젝트를 만듭니다. 이는 최신 배포 환경, 패키지 확장을 통해 Windows 10 기능과 통합하는 기능 등을 제공합니다. 자세한 내용은 [Windows 애플리케이션 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 참조하세요.  |

### <a name="project-templates-for-console-apps"></a>콘솔 앱용 프로젝트 템플릿

콘솔 앱용 C++ 프로젝트 템플릿에 액세스하려면 언어를 **C++** 로, 플랫폼을 **Windows**로, 프로젝트 형식을 **콘솔**로 필터링합니다.

![네이티브 C++ 콘솔 프로젝트 템플릿](images/desktop-console-projects-cpp.png)

| 템플릿 | 설명 |
|----------|----------|
| Windows 콘솔 애플리케이션(C++/WinRT) | 사용자 인터페이스가 없는 [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) 콘솔 앱을 만듭니다. 자세한 내용은 [C++/WinRT 빠른 시작](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#a-cwinrt-quick-start)을 참조하세요. 이 프로젝트 템플릿에는 [C++/WinRT VSIX](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)가 필요합니다.  |
| 콘솔 앱 | 사용자 인터페이스가 없는 콘솔 앱을 만듭니다. 자세한 내용은 [연습: 표준 C++ 프로그램 만들기](https://docs.microsoft.com/cpp/windows/walkthrough-creating-a-standard-cpp-program-cpp)를 참조하세요. |
| 빈 프로젝트 | 애플리케이션, 라이브러리 또는 DLL을 만드는 빈 프로젝트입니다. 필요한 모든 코드 또는 리소스를 추가해야 합니다. |

### <a name="project-templates-for-libraries"></a>라이브러리용 프로젝트 템플릿

라이브러리용 C++ 프로젝트 템플릿에 액세스하려면 언어를 **C++** 로, 플랫폼을 **Windows**로, 프로젝트 형식을 **라이브러리**로 필터링합니다.

![네이티브 C++ 라이브러리 프로젝트 템플릿](images/desktop-library-projects-cpp.png)

| 템플릿 | 설명 |
|----------|----------|
| DLL(동적 연결 라이브러리) | DLL(동적 연결 라이브러리)을 만드는 프로젝트입니다. 자세한 내용은 [연습: 동적 연결 라이브러리 만들기 및 사용](https://docs.microsoft.com/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp)을 참조하세요. |
| 정적 라이브러리 | 정적 라이브러리(LIB)를 만드는 프로젝트입니다. 자세한 내용은 [연습: 정적 라이브러리 만들기 및 사용](https://docs.microsoft.com/cpp/build/walkthrough-creating-and-using-a-static-library-cpp)을 참조하세요. |

### <a name="item-templates-for-native-c-and-win32"></a>네이티브 C++ 및 Win32용 항목 템플릿

C++ 프로젝트 템플릿에는 새 파일과 리소스를 프로젝트에 추가하는 것과 같은 작업을 수행하는 데 사용할 수 있는 다양한 항목 템플릿이 포함됩니다. 포괄적인 목록은 [Visual C++ 새 항목 템플릿 추가 사용](https://docs.microsoft.com/cpp/build/reference/using-visual-cpp-add-new-item-templates)을 참조하세요.

## <a name="net-templates"></a>.NET 템플릿

Visual Studio는 .NET 및 C#을 사용하는 데스크톱 Windows 앱을 빌드하기 위한 다양한 프로젝트 템플릿을 제공합니다. 이러한 프로젝트 템플릿을 사용하려면 Visual Studio를 설치할 때 **.NET 데스크톱 개발** 워크로드를 포함해야 합니다.

Visual Studio에서 새 프로젝트를 만들 때 .NET C# 프로젝트 템플릿에 액세스하려면 언어를 **C#** 으로, 플랫폼을 **Windows**로, 프로젝트 형식을 **데스크톱**으로 필터링합니다.

![.NET C# 프로젝트 템플릿](images/dotnet-projects-csharp.png)

C# 및 .NET을 사용하여 앱을 만드는 데 사용할 수 있는 프로젝트 템플릿은 다음과 같습니다.

| 템플릿 | 설명 |
|----------|----------|
| WPF 앱(.NET Core) | [.NET Core](https://docs.microsoft.com/dotnet/core/)를 대상으로 하는 [WPF](https://docs.microsoft.com/dotnet/framework/wpf/) 앱을 만듭니다. 이 프로젝트 템플릿에 대한 연습은 [WPF 애플리케이션 만들기](https://docs.microsoft.com/visualstudio/get-started/csharp/tutorial-wpf)를 참조하세요. |
| WPF 앱(.NET Framework) | [.NET Framework](https://docs.microsoft.com/dotnet/framework/)를 대상으로 하는 [WPF](https://docs.microsoft.com/dotnet/framework/wpf/) 앱을 만듭니다. 이 프로젝트 템플릿에 대한 연습은 [자습서: 첫 번째 WPF 애플리케이션 만들기](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application)를 참조하세요. |
| Windows Forms 앱(.NET Core) | [.NET Core](https://docs.microsoft.com/dotnet/core/)를 대상으로 하는 [Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/) 앱을 만듭니다.  |
| Windows Forms 앱(.NET Framework) | [.NET Framework](https://docs.microsoft.com/dotnet/framework/)를 대상으로 하는 [Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/) 앱을 만듭니다. 이 프로젝트 템플릿에 대한 연습은 [C#을 사용하여 Visual Studio에서 Windows Forms 앱 만들기](https://docs.microsoft.com/visualstudio/ide/create-csharp-winform-visual-studio)를 참조하세요. |
| Windows 애플리케이션 패키징 프로젝트 | WPF 또는 Windows Forms 앱을 [MSIX 패키지](https://docs.microsoft.com/windows/msix/overview)로 빌드하는 데 사용할 수 있는 프로젝트를 만듭니다. 이는 최신 배포 환경, 패키지 확장을 통해 Windows 10 기능과 통합하는 기능 등을 제공합니다. 자세한 내용은 [Windows 애플리케이션 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 참조하세요. |
