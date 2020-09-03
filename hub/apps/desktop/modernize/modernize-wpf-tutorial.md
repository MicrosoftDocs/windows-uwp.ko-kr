---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, WPF 앱에 다른 최신 구성 요소를 통합하는 방법을 보여줍니다.
title: '자습서: WPF 앱 현대화'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: aa8991e7fd0bbb825ff5280f01693f092125f573
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161377"
---
# <a name="tutorial-modernize-a-wpf-app"></a>자습서: WPF 앱 현대화 

앱을 처음부터 다시 작성하는 대신 최신 Windows 기능을 기존 소스 코드에 통합하여 기존 데스크톱 앱을 [현대화](index.md)하는 여러 가지 방법이 있습니다. 이 자습서에서는 다음과 같은 기능을 사용하여 기존 WPF 기간 업무 앱을 현대화하는 여러 가지 방법을 살펴봅니다.

* .NET Core 3
* XAML Islands를 사용하는 UWP XAML 컨트롤
* 적응형 카드 및 Windows 10 알림
* MSIX 배포

이 자습서를 수행하려면 다음과 같은 개발 기술이 필요합니다.

* WPF를 사용하여 Windows 데스크톱 앱을 개발한 경험
* C# 및 XAML에 대한 기본 지식
* UWP에 대한 기본 지식

## <a name="overview"></a>개요

이 자습서에서는 Contoso 지출이라는 간단한 WPF 기간 업무 앱의 코드를 제공합니다. 이 자습서의 가상 시나리오에 등장하는 Contoso 지출 앱은 Contoso Corporation의 관리자가 보고서에 제출된 비용을 추적하는 데 사용하는 내부용 앱입니다. 현재 관리자는 터치 지원 디바이스를 보유하고 있으며, 마우스나 키보드 없이 Contoso 지출 앱을 사용하려 합니다. 하지만 안타깝게도 현재 앱 버전은 터치를 지원하지 않습니다.

Contoso에서는 직원들이 지출 보고서를 보다 효율적으로 작성할 수 있도록 이 앱을 새 Windows 기능으로 현대화하려고 합니다. 새 UWP 앱을 빌드하여 여러 기능을 쉽게 구현할 수 있습니다. 그러나 기존 앱은 복잡하며 여러 팀에서 수년간 개발한 결과물입니다. 따라서 신기술을 사용하여 기존 앱을 처음부터 새로 작성하는 것은 옵션이 아닙니다. 팀에서는 기존 코드 베이스에 새 기능을 추가하는 가장 좋은 방법을 찾고 있습니다.

이 자습서의 시작 부분에서 Contoso 지출 앱은 .NET Framework 4.7.2를 대상으로 하며 다음과 같은 타사 라이브러리를 사용합니다.

* MVVM Light - MVVM 패턴의 기본 구현입니다.
* Unity - 종속성 주입 컨테이너입니다.
* LiteDb - 데이터를 저장하는 임베디드 NoSQL 솔루션입니다.
* Bogus - 가짜 데이터를 생성하는 도구입니다.

이 자습서에서는 다음과 같은 방법으로 새 Windows 기능을 사용하여 Contoso 지출 앱을 개선합니다.

* 기존 WPF 앱을 .NET Core 3.0으로 마이그레이션합니다. 이렇게 하면 이후에 중요한 새 시나리오가 열립니다.
* XAML Islands를 사용하여 Windows 커뮤니티 도구 키트에서 제공하는 **InkCanvas** 및 **MapControl** 래핑된 컨트롤을 호스팅합니다.
* XAML Islands를 사용하여 표준 UWP XAML 컨트롤(여기서는 **CalendardView**)을 호스팅합니다.
* 적응형 카드 및 Windows 10 알림을 앱에 통합합니다.
* 새 버전의 앱이 준비되는 즉시 테스터 및 사용자에게 자동으로 제공할 수 있도록 MSIX를 사용하여 앱을 패키징하고 Azure DevOps에서 CI/CD 파이프라인을 설정합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 자습서를 수행하려면 개발 컴퓨터에 다음과 같은 필수 구성 요소가 설치되어 있어야 합니다.

* Windows 10 버전 1903(빌드 18362) 이상
* [Visual Studio 2019](https://www.visualstudio.com)
* [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)(최신 버전 설치)

Visual Studio 2019를 사용하여 다음 워크로드 및 선택적 기능을 설치해야 합니다.

* .NET 데스크톱 개발
* 유니버설 Windows 플랫폼 개발
* Windows 10 SDK(10.0.18362.0 이상)

## <a name="get-the-contoso-expenses-sample-app"></a>Contoso 지출 샘플 앱 다운로드

자습서를 시작하기 전에, Contoso 지출 앱의 소스 코드를 다운로드하고 Visual Studio에서 코드를 빌드할 수 있는지 확인합니다.

1. [AppConsult WinAppsModernization 워크숍 리포지토리](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)의 **릴리스** 탭에서 앱 소스 코드를 다운로드합니다. 직접 링크는 [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases)입니다.
2. zip 파일을 열고 **C:\\** 드라이브의 루트에 모든 콘텐츠를 추출합니다. **C:\WinAppsModernizationWorkshop**이라는 폴더가 생성됩니다.
3. Visual Studio 2019를 열고 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** 파일을 두 번 클릭하여 솔루션을 엽니다.
4. **시작** 단추 또는 CTRL + F5 키를 눌러 Contoso 지출 WPF 프로젝트를 빌드, 실행 및 디버그할 수 있는지 확인합니다.

## <a name="get-started"></a>시작

Contoso 지출 샘플 앱의 소스 코드를 다운로드하여 확인했으면 다음 자습서를 시작할 수 있습니다.

* [1부: Contoso Expenses 앱을 .NET Core 3으로 마이그레이션](modernize-wpf-tutorial-1.md)
* [2부: XAML Islands를 사용하여 UWP InkCanvas 컨트롤 추가](modernize-wpf-tutorial-2.md)
* [3부: XAML Islands를 사용하여 UWP CalendarView 컨트롤 추가](modernize-wpf-tutorial-3.md)
* [4부: Windows 10 사용자 작업 및 알림 추가](modernize-wpf-tutorial-4.md)
* [5부: MSIX로 패키징 및 배포](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>중요 개념

다음 섹션에서는 이 자습서에서 다루는 주요 개념의 배경 지식을 제공합니다. 이러한 개념을 이미 잘 알고 있는 경우에는 이 섹션을 건너뛰어도 됩니다.

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

Windows 8에서 Microsoft는 WinRT(Windows 런타임)라는 새로운 프레임워크를 도입했습니다. .NET Framework와 달리, WinRT는 앱에 직접 공개되는 API의 기본 레이어입니다. 또한 WinRT에는 개발자가 C++뿐 아니라 C#이나 JavaScript 같은 언어를 사용하여 상호 작용할 수 있도록 런타임 위에 추가되는 레이어인 언어 프로젝션이 도입되었습니다. 이 프로젝션을 통해 개발자는 .NET Framework를 사용하여 앱을 빌드할 때 얻은 C# 및 XAML 지식을 활용하는 앱을 WinRT 위에 빌드할 수 있습니다. 

Windows 10에서 Microsoft는 WinRT 위에 빌드되는 [UWP(유니버설 Windows 플랫폼)](/windows/uwp/get-started/universal-application-platform-guide)를 도입했습니다. UWP의 가장 중요한 기능은 모든 디바이스 플랫폼에서 공통 API 세트를 제공한다는 것입니다. 즉, 앱이 데스크톱에서 실행되든, Xbox One에서 실행되든, HoloLens에서 실행되든 동일한 API를 사용할 수 있습니다.

앞으로 타임라인, 프로젝트 로마, Windows Hello를 포함하여 대부분의 새 Windows 10 기능은 WinRT API를 통해 공개됩니다.

### <a name="msix-packaging"></a>MSIX 패키징

[MSIX](/windows/msix/)는 Windows 앱에 사용되는 최신 패키징 모델입니다. MSIX는 UWP 앱뿐 아니라 Win32, WPF, Windows Forms, Java, Electron 등과 같은 기술을 사용한 데스크톱 앱 빌드를 지원합니다. MSIX 패키지에 데스크톱 앱을 패키징할 때 Microsoft Store에 앱을 게시할 수 있습니다. 또한 데스크톱 앱은 설치될 때 패키지 ID를 받게 되며, 그러면 데스크톱 앱에서 광범위한 WinRT API 세트를 사용할 수 있습니다.

자세한 내용은 다음 문서를 참조하세요.

* [데스크톱 애플리케이션 패키징](/windows/uwp/porting/desktop-to-uwp-root)
* [패키징된 데스크톱 애플리케이션의 백그라운드 작업](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML Islands

Windows 10 버전 1903부터 *XAML Island*라는 기능을 사용하여 UWP 이외의 데스크톱 앱에서 UWP 컨트롤을 호스트할 수 있습니다. 이 기능을 사용하면 UWP 컨트롤을 통해서만 제공되는 최신 Windows 10 UI 기능을 통해 기존 데스크톱 앱의 모양, 느낌 및 기능을 개선할 수 있습니다. 즉, Windows Ink와 같은 UWP 기능과 Fluent Design 시스템을 지원하는 컨트롤을 기존 WPF, Windows Forms 및 C++ Win32 애플리케이션에서 사용할 수 있습니다.

자세한 내용은 [데스크톱 애플리케이션의 UWP 컨트롤(XAML Islands)](/windows/uwp/xaml-platform/xaml-host-controls)을 참조하세요. 이 자습서에서는 다음과 같은 두 가지 형식의 XAML Island 컨트롤을 사용하는 과정을 안내합니다.

* Windows 커뮤니티 도구 키트의 [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol). 이러한 WPF 컨트롤은 해당 UWP 컨트롤의 인터페이스 및 기능을 래핑하며 Visual Studio 디자이너의 다른 WPF 컨트롤과 마찬가지 방법으로 사용할 수 있습니다.

* UWP [달력 보기](/windows/uwp/design/controls-and-patterns/calendar-view) 컨트롤. Windows 커뮤니티 도구 키트의 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 호스트하게 될 표준 UWP 컨트롤입니다.

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](/dotnet/core/)는 가볍고 쉽게 확장할 수 있는 전체 .NET Framework의 플랫폼 간 버전을 구현하는 오픈 소스 프레임워크입니다. 전체 .NET Framework에 비해 .NET Core 시작 시간이 훨씬 빠르고 여러 API가 최적화되었습니다.

초기에 출시된 여러 릴리스에서 .NET Core는 웹 또는 백 엔드 앱을 지원하는 데 중점을 두었습니다. .NET Core를 사용하면 Windows, Linux 또는 Docker 컨테이너 같은 마이크로서비스 아키텍처에서 호스팅할 수 있는 확장 가능한 웹앱 또는 API를 쉽게 빌드할 수 있습니다.

.NET Core 3는 .NET Core의 최신 릴리스입니다. 이 릴리스의 주요 기능은 Windows Forms 및 WPF 앱을 포함하는 Windows 데스크톱 앱에 대한 지원입니다. .NET Core 3에서 신규 및 기존 Windows 데스크톱 앱을 실행하고 .NET Core에서 제공하는 모든 이점을 누릴 수 있습니다. [XAML Islands](xaml-islands.md)에 호스트되는 UWP 컨트롤을 .NET Core 3를 대상으로 하는 Windows Forms 및 WPF 앱에서도 사용할 수 있습니다.

> [!NOTE]
> WPF 및 Windows Forms는 여러 플랫폼에서 사용할 수 없으며, Linux 및 MacOS에서 WPF 또는 Windows Forms를 실행할 수 없습니다. WPF 및 Windows Forms의 UI 구성 요소는 여전히 Windows 렌더링 시스템에 종속되어 있습니다.

자세한 내용은 [.NET Core 3.0의 새로운 기능](/dotnet/core/whats-new/dotnet-core-3-0)을 참조하세요.