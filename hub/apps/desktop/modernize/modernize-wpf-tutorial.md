---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가 하 고, MSIX 개의 패키지를 만들고, 기타 최신 구성 요소를 WPF 앱에 통합 하는 방법을 보여 줍니다.
title: '자습서: WPF 앱 현대화'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521292"
---
# <a name="tutorial-modernize-a-wpf-app"></a>자습서: WPF 앱 현대화 

앱을 처음부터 다시 작성 하는 대신 기존 소스 코드에 최신 Windows 기능을 통합 하 여 기존 데스크톱 응용 프로그램을 [현대화](index.md) 하는 방법에는 여러 가지가 있습니다. 이 자습서에서는 다음과 같은 기능을 사용 하 여 기존 WPF lob (기간 업무) 앱을 현대화 하는 여러 가지 방법을 살펴봅니다.

* .NET Core 3
* XAML 아일랜드를 사용 하는 UWP XAML 컨트롤
* 적응 카드 및 Windows 10 알림
* MSIX 배포

이 자습서에는 다음과 같은 개발 기술이 필요 합니다.

* WPF를 사용 하 여 Windows 데스크톱 앱을 개발 하는 환경입니다.
* 및 XAML에 C# 대 한 기본 지식.
* UWP에 대 한 기본 지식.

## <a name="overview"></a>개요

이 자습서에서는 Contoso 비용 이라는 간단한 WPF lob (기간 업무) 앱에 대 한 코드를 제공 합니다. 이 자습서의 가상 시나리오에서 Contoso 비용은 Contoso Corporation의 관리자가 보고서에서 제출한 비용을 추적 하는 데 사용 하는 내부 앱입니다. 이제 관리자는 터치 가능 장치를 장착 하 고, 마우스나 키보드 없이 Contoso 지출 앱을 사용 하려고 합니다. 불행 하 게 앱의 현재 버전은 터치가 쉽지 않습니다.

Contoso는 직원 들이 비용 보고서를 보다 효율적으로 만들 수 있도록 새 Windows 기능을 사용 하 여이 앱을 현대화 하려고 합니다. 새 UWP 앱을 빌드하여 대부분의 기능을 쉽게 구현할 수 있습니다. 그러나 기존 앱은 복잡 하며 다양 한 팀에서 개발 하는 여러 년간의 결과입니다. 따라서 새로운 기술을 사용 하 여 처음부터 다시 작성 하는 것은 옵션이 아닙니다. 팀에서 기존 코드 베이스에 새 기능을 추가 하는 가장 좋은 방법을 찾고 있습니다.

이 자습서의 시작 부분에서 Contoso 비용은 .NET Framework 4.7.2를 대상으로 하며 다음의 타사 라이브러리를 사용 합니다.

* MVVM 패턴에 대 한 기본 구현인 MVVM Light입니다.
* Unity, 종속성 주입 컨테이너.
* LiteDb-데이터를 저장 하는 임베디드 NoSQL 솔루션입니다.
* 가짜 데이터를 생성 하는 도구인 가짜.

이 자습서에서는 새로운 Windows 기능을 사용 하 여 Contoso 비용을 개선 합니다.

* 기존 WPF 앱을 .NET Core 3.0로 마이그레이션합니다. 이렇게 하면 나중에 새로운 시나리오와 중요 한 시나리오가 열립니다.
* XAML 아일랜드를 사용 하 여 Windows 커뮤니티 도구 키트에서 제공 하는 **InkCanvas** 및 **없습니다** 래핑된 컨트롤을 호스팅합니다.
* XAML 아일랜드를 사용 하 여 표준 UWP XAML 컨트롤 (이 경우 **CalendardView**)을 호스팅합니다.
* 적응 카드와 Windows 10 알림을 앱에 통합 합니다.
* MSIX으로 앱을 패키지 하 고 Azure DevOps에서 CI/CD 파이프라인을 설정 하 여 새 버전의 앱을 사용할 수 있는 즉시 테스터 및 사용자에 게 자동으로 제공할 수 있도록 합니다.

## <a name="prerequisites"></a>필수 조건

이 자습서를 수행 하려면 개발 컴퓨터에 다음과 같은 필수 구성 요소가 설치 되어 있어야 합니다.

* Windows 10, 버전 1903 (빌드 18362) 이상 버전
* [Visual Studio 2019](https://www.visualstudio.com).
* [.Net Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (최신 버전 설치)

Visual Studio 2019을 사용 하 여 다음 작업 및 선택적 기능을 설치 해야 합니다.

* .NET 데스크톱 개발
* 유니버설 Windows 플랫폼 개발
* Windows 10 SDK (10.0.18362.0 이상)

## <a name="get-the-contoso-expenses-sample-app"></a>Contoso 지출 샘플 앱 가져오기

자습서를 시작 하기 전에 Contoso 지출 앱에 대 한 소스 코드를 다운로드 하 고 Visual Studio에서 코드를 빌드할 수 있는지 확인 합니다.

1. [Appconsult 워크숍 리포지토리의](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop) **릴리스** 탭에서 앱 소스 코드를 다운로드 합니다. 직접 링크를 [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases)합니다.
2. Zip 파일을 열고 **C:\\** 드라이브의 루트에 모든 콘텐츠를 추출 합니다. **C:\WinAppsModernizationWorkshop**라는 폴더를 만듭니다.
3. Visual Studio 2019를 열고 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** 파일을 두 번 클릭 하 여 솔루션을 엽니다.
4. **시작** 단추 또는 CTRL + F5 키를 눌러 CONTOSO 지출 WPF 프로젝트를 빌드, 실행 및 디버그할 수 있는지 확인 합니다.

## <a name="get-started"></a>시작

Contoso 지출 샘플 앱에 대 한 소스 코드를 만든 후 Visual Studio에서 빌드할 수 있는지 확인할 수 있습니다. 자습서를 시작할 준비가 되었습니다.

* [1 부: Contoso 지출 앱을 .NET Core 3으로 마이그레이션](modernize-wpf-tutorial-1.md)
* [2 부: XAML 아일랜드를 사용 하 여 UWP InkCanvas 컨트롤 추가](modernize-wpf-tutorial-2.md)
* [3 부: XAML 아일랜드를 사용 하 여 UWP CalendarView 컨트롤 추가](modernize-wpf-tutorial-3.md)
* [4 부: Windows 10 사용자 활동 및 알림 추가](modernize-wpf-tutorial-4.md)
* [5 부: MSIX을 사용 하 여 패키지 및 배포](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>중요 한 개념

다음 섹션에서는이 자습서에서 설명 하는 주요 개념 중 일부에 대 한 배경 지식을 제공 합니다. 이러한 개념을 이미 잘 알고 있는 경우에는이 섹션을 건너뛰어도 됩니다.

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

Windows 8에서 Microsoft는 WinRT (Windows 런타임) 라는 새로운 프레임 워크를 도입 했습니다. .NET Framework와 달리 WinRT는 앱에 직접 노출 되는 Api의 기본 계층입니다. 또한 WinRT에는 개발자가 외에도 C# 및 JavaScript와 같은 언어를 사용 하 여 개발자가 상호 작용할 수 있도록 하는 런타임 위에 추가 된 계층 C++인 언어 프로젝션이 도입 되었습니다. 개발자는 프로젝션을 사용 하 여 응용 프로그램을 빌드하는 데 사용 되 C# 는 것과 동일한 XAML 정보를 활용 하 고 .NET Framework를 사용 하 여 앱을 빌드할 수 있습니다. 

Windows 10에서 Microsoft는 [유니버설 Windows 플랫폼 (UWP)](/windows/uwp/get-started/universal-application-platform-guide)를 도입 했으며,이는 WinRT 위에 빌드됩니다. UWP의 가장 중요 한 기능은 모든 장치 플랫폼에서 공통 된 Api 집합을 제공 하는 것입니다. 즉, 앱이 데스크톱에서 실행 되는 경우, Xbox One 또는 HoloLens에 있든 상관 없이 동일한 Api를 사용할 수 있습니다.

앞으로는 대부분의 새로운 Windows 10 기능이 타임 라인, Project 로마 및 Windows Hello와 같은 기능을 포함 하 여 WinRT Api를 통해 노출 됩니다.

### <a name="msix-packaging"></a>MSIX 패키징

[Msix](/windows/msix/) 은 Windows 앱에 대 한 최신 패키징 모델입니다. MSIX은 UWP 앱 뿐만 아니라 Win32, WPF, Windows Forms, Java, 전자 등과 같은 기술을 사용 하 여 빌드하는 데스크톱 앱을 지원 합니다. MSIX 패키지에서 데스크톱 앱을 패키지 하는 경우 Microsoft Store에 앱을 게시할 수 있습니다. 데스크톱 앱은 패키지 id가 설치 된 경우에도 패키지 id를 가져올 수 있습니다. 그러면 데스크톱 앱에서 광범위 한 WinRT Api 집합을 사용할 수 있습니다.

자세한 내용은 다음 문서를 참조하세요.

* [데스크톱 응용 프로그램 패키지](/windows/uwp/porting/desktop-to-uwp-root)
* [패키지 된 데스크톱 응용 프로그램의 배후](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML 제도

Windows 10 버전 1903부터 *XAML 아일랜드*라는 기능을 사용 하 여 uwp 이외의 데스크톱 앱에서 uwp 컨트롤을 호스트할 수 있습니다. 이 기능을 사용 하면 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 기존 데스크톱 응용 프로그램의 모양, 느낌 및 기능을 향상 시킬 수 있습니다. 즉, 기존 WPF, Windows Forms 및 C++ Win32 앱에서 흐름 디자인 시스템을 지 원하는 Windows 잉크 및 컨트롤과 같은 UWP 기능을 사용할 수 있습니다.

자세한 내용은 [데스크톱 응용 프로그램의 UWP 컨트롤 (XAML 아일랜드)](/windows/uwp/xaml-platform/xaml-host-controls)을 참조 하세요. 이 자습서에서는 두 가지 유형의 XAML 아일랜드 컨트롤을 사용 하는 과정을 안내 합니다.

* Windows 커뮤니티 도구 키트의 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [없습니다](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) . 이러한 WPF 컨트롤은 해당 UWP 컨트롤의 인터페이스 및 기능을 래핑하고 Visual Studio 디자이너의 다른 WPF 컨트롤과 마찬가지로 사용할 수 있습니다.

* UWP [Calendar view](/windows/uwp/design/controls-and-patterns/calendar-view) 컨트롤입니다. Windows 커뮤니티 도구 키트의 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용 하 여 호스트할 표준 UWP 컨트롤입니다.

### <a name="net-core-3"></a>.NET Core 3

[.Net Core](https://docs.microsoft.com/dotnet/core/) 는 전체 .NET Framework의 플랫폼 간, 간단 하 고 확장 가능한 버전을 구현 하는 오픈 소스 프레임 워크입니다. 전체 .NET Framework에 비해 .NET Core 시작 시간이 훨씬 빨라지고 Api의 대부분이 최적화 되었습니다.

처음 몇 가지 릴리스를 통해 .NET Core는 웹 또는 백 엔드 앱을 지 원하는 데 중점을 둡니다. .NET Core를 사용 하 여 Windows, Linux 또는 Docker 컨테이너와 같은 마이크로 서비스 아키텍처에서 호스팅될 수 있는 확장 가능한 웹 앱 또는 Api를 쉽게 빌드할 수 있습니다.

.NET Core 3은 .NET Core의 최신 릴리스입니다. 이 릴리스의 주요 기능은 Windows Forms 및 WPF 앱을 포함하는 Windows 데스크톱 앱에 대한 지원입니다. .NET Core 3에서 신규 및 기존 Windows 데스크톱 앱을 실행하고 .NET Core에서 제공하는 모든 이점을 누릴 수 있습니다. [XAML Islands](xaml-islands.md)에 호스트되는 UWP 컨트롤을 .NET Core 3를 대상으로 하는 Windows Forms 및 WPF 앱에서도 사용할 수 있습니다.

> [!NOTE]
> WPF 및 Windows Forms는 플랫폼 간이 되지 않으며, Linux 및 MacOS에서 WPF 또는 Windows Forms를 실행할 수 없습니다. WPF 및 Windows Forms UI 구성 요소는 여전히 Windows 렌더링 시스템에 종속 되어 있습니다.

자세한 내용은 [.NET Core 3.0의 새로운 기능](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)을 참조하세요.
