---
description: 이 자습서에는 UWP XAML 사용자 인터페이스를 추가, MSIX 패키지 만들기 및 WPF 앱에 다른 최신 구성 요소를 통합 하는 방법을 보여 줍니다.
title: '자습서: WPF 응용 프로그램 현대화'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420083"
---
# <a name="tutorial-modernize-a-wpf-app"></a>자습서: WPF 응용 프로그램 현대화 

하는 방법은 여러 가지가 [현대화](index.md) 기존에 최신 Windows 기능을 통합 하 여 기존 데스크톱 앱 소스 코드 처음부터 앱을 다시 작성 하는 대신 합니다. 이 자습서에서는 이러한 기능을 사용 하 여 기존 WPF 기간 업무 앱을 현대화 하는 여러 방법을 살펴봅니다.

* .NET Core 3
* XAML 제도 사용 하 여 UWP XAML 컨트롤
* Adaptive Card 및 Windows 10 알림
* MSIX 배포

이 자습서에는 다음 개발 기술이 필요합니다.

* WPF 사용 하 여 Windows 데스크톱 앱을 개발 해 본 경험 합니다.
* 기본 지식 C# 및 XAML입니다.
* UWP의 기본 지식입니다.

## <a name="overview"></a>개요

이 자습서에서는 Contoso Expenses 라는 간단한 WPF 기간 업무 앱에 대 한 코드를 제공 합니다. 자습서의 가상의 시나리오에서 Contoso 경비 보고서 제출한 비용을 추적할 수 Contoso Corporation의 관리자에서 사용 하는 내부 앱입니다. 관리자는 이제 장착 되어 터치 사용 장치 및 Contoso expenses 없이 마우스나 키보드를 사용 하려고 합니다. 그러나 현재 버전의 앱 되지 터치 식별 합니다.

Contoso는 직원이 비용 보고서를 보다 효율적으로 만들 수 있도록 하는 새로운 Windows 기능을 사용 하 여이 앱을 현대화 하려고 합니다. 다양 한 기능은 새 UWP 앱을 작성 하 여 쉽게 구현할 수 있습니다. 그러나 기존 앱 복잡 하며 다른 팀에서 개발의 수년간의 결과입니다. 따라서 새 기술 사용 하 여 처음부터 다시 쓰기를 사용할 수 없습니다. 팀은 기존 코드 베이스에 새 기능을 추가 하는 좋은 방법이 찾습니다.

자습서의 부분을 Contoso 비용.NET Framework 4.7.2를 대상으로 하 고는 다음 타사 라이브러리를 사용 하 여:

* MVVM Light에서 MVVM 패턴에 대 한 기본 구현입니다.
* Unity에서 종속성 주입 컨테이너입니다.
* LiteDb, 데이터를 저장 하는 포함 된 NoSQL 솔루션입니다.
* 가짜, 모조 데이터를 생성 하는 도구입니다.

자습서에서 새로운 Windows 기능을 사용 하 여 Contoso 비용 개선:

* .NET Core 3.0으로 기존 WPF 앱을 마이그레이션하십시오. 열립니다 새롭고 중요 한 시나리오를 나중에.
* 호스트를 사용 하 여 XAML 제도 **InkCanvas** 하 고 **MapControl** 래핑된 Windows 커뮤니티 도구 키트에서 제공 하는 컨트롤입니다.
* 모든 표준 UWP XAML 컨트롤을 호스트를 사용 하 여 XAML 제도 (이 경우에 **CalendardView**).
* Adaptive Card 알림과 Windows 10 앱에 통합 합니다.
* 패키지 있습니다 제공할 수 있도록 자동으로 새 버전의 앱이 테스터와 사용자에 게 사용할 수 있는 즉시 MSIX와 CI/CD 설정 앱에 Azure DevOps 파이프라인입니다.

## <a name="prerequisites"></a>사전 요구 사항

이 자습서를 수행 하려면 개발 컴퓨터에 이러한 필수 구성이 요소가 설치 되어 있어야 합니다.

* Windows 10, 버전 1903 (18362 빌드) 또는 이후 버전
* [Visual Studio 2019](https://www.visualstudio.com).
* [.NET core 3 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (최신 사용 가능한 미리 보기 버전 설치).

Visual Studio 2019를 사용 하 여 다음 워크 로드 및 선택적 기능을 설치 해야 합니다.

* .NET 데스크톱 개발
* 유니버설 Windows 플랫폼 개발
* Windows 10 SDK (10.0.18362.0 이상)

## <a name="get-the-contoso-expenses-sample-app"></a>Contoso 비용 샘플 앱 가져오기

자습서를 시작 하기 전에 Contoso expenses에 대 한 소스 코드를 다운로드 하 고 Visual Studio에서 코드를 빌드할 수 있는지 확인 합니다.

1. 앱 소스 코드를 다운로드 합니다 **릴리스** 탭을 [AppConsult WinAppsModernization 워크샵 리포지토리](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)합니다. 직접 연결이 [ https://aka.ms/wamwc ](https://aka.ms/wamwc)합니다.
2. Zip 파일을 열고 모든 콘텐츠 루트를 추출 하 **c:\\**  드라이브입니다. 라는 폴더를 만듭니다 **C:\WinAppsModernizationWorkshop**합니다.
3. Visual Studio 2019 열고 두 번 클릭 합니다 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** 파일을 솔루션을 엽니다.
4. 수 빌드, 실행 하는 키를 눌러 Contoso 비용 WPF 프로젝트를 디버그를 확인 합니다 **시작** 단추 또는 CTRL + F5입니다.

## <a name="get-started"></a>시작

Contoso 비용 샘플 앱에 대 한 소스 코드가 있는 후 Visual Studio에서 빌드할 수 있는지 확인할 수 있습니다 자습서 시작 준비가 되었습니다.

* [1 부: Contoso 마이그레이션 expenses 3.NET Core로](modernize-wpf-tutorial-1.md)
* [2 부: XAML 제도 사용 하 여 UWP InkCanvas 컨트롤 추가](modernize-wpf-tutorial-2.md)
* [3 부: XAML 제도 사용 하 여 UWP CalendarView 컨트롤 추가](modernize-wpf-tutorial-3.md)
* [4 부: Windows 10 사용자 동작 및 알림을 추가 합니다.](modernize-wpf-tutorial-4.md)
* [5 부: 패키지 및 MSIX를 사용 하 여 배포](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>중요 한 개념

다음 섹션에서는이 자습서에 설명 된 주요 개념 중 일부에 대 한 배경 지식을 제공 합니다. 이미 이러한 개념에 익숙한 경우이 섹션을 건너뛸 수 있습니다.

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

Windows 8의 Microsoft Windows Runtime (WinRT) 이라고 하는 새로운 프레임 워크를 도입 했습니다. .NET Framework와 달리 WinRT는 api 앱에 직접 노출 되는 기본 계층입니다. WinRT 개발자와 같은 언어를 사용 하 여 상호 작용할 수 있도록 런타임 기반의 추가 계층을 수 있는 언어 프로젝션 소개 C# 및 JavaScript 외에 C++합니다. 프로젝션을 통해 동일한를 활용 하는 WinRT 위에 앱을 빌드하는 개발자 C# 및.NET Framework를 사용 하 여 앱 개발에서 얻은 이러한 XAML 기술입니다. 

Microsoft Windows 10에 도입 합니다 [유니버설 Windows 플랫폼 (UWP)](/windows/uwp/get-started/universal-application-platform-guide), WinRT 위에 빌드되는 합니다. UWP의 가장 중요 한 기능은 모든 장치 플랫폼에서 Api의 공통 집합을 제공 하는 것은: 든 관계 없이 데스크톱, Xbox One에 또는 HoloLens에 앱이 실행 하는 동일한 Api를 사용할 수 있습니다.

앞으로 대부분의 새로운 Windows 10 기능 타임 라인, 프로젝트 로마 및 Windows Hello 같은 기능을 포함 하 여 WinRT Api를 통해 노출 됩니다.

### <a name="msix-packaging"></a>MSIX 패키징

[MSIX](http://aka.ms/msix) (이전의 AppX) Windows 앱에 대 한 최신 패키징 모델입니다. MSIX는 UWP 앱 뿐만 아니라 Win32, WPF, Windows Forms, Java, 전자, 등과 같은 기술을 사용 하 여 작성 하는 데스크톱 앱을 지원 합니다. MSIX 패키지에서 데스크톱 앱을 패키지 하는 경우에 Microsoft Store 앱을 게시할 수 있습니다. 데스크톱 앱도 설치 될 때, 광범위 한 WinRT Api를 사용 하 여 데스크톱 앱을 사용 하도록 설정 하는 패키지 id를 가져옵니다.

자세한 내용은 다음 문서를 참조하세요.

* [데스크톱 응용 프로그램 패키지](/windows/uwp/porting/desktop-to-uwp-root)
* [패키지에 포함 된 데스크톱 응용 프로그램의 백그라운드](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML 제도

Windows 10 버전 1903부터 UWP 컨트롤 이라는 기능을 사용 하 여 비 UWP 데스크톱 앱에서 호스팅할 수 있습니다 *XAML 제도*합니다. 이 기능을 사용 하면 모양, 느낌 및 UWP 컨트롤을 통해 이용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 기존 데스크톱 앱의 기능을 향상 시킬 수 있습니다. UWP Windows 잉크 Fluent Design System에 기존 WPF, Windows Forms에서에서 지 원하는 컨트롤 등의 기능을 사용할 수 있다는 의미이 고 C++ Win32 앱.

자세한 내용은 [데스크톱 응용 프로그램 (XAML 제도)에서 UWP 컨트롤](/windows/uwp/xaml-platform/xaml-host-controls)합니다. 이 자습서에서는 두 가지 유형의 XAML 섬 컨트롤을 사용 하는 과정을 안내 합니다.

* 합니다 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 하 고 [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) Windows 커뮤니티 도구 키트에서. WPF 컨트롤 이러한 인터페이스와 해당 하는 UWP 컨트롤의 기능을 래핑하고 Visual Studio 디자이너에 있는 다른 WPF 컨트롤 처럼 사용할 수 있습니다.

* UWP [달력 보기](/windows/uwp/design/controls-and-patterns/calendar-view) 제어 합니다. 사용 하 여 호스트 하는 표준 UWP 컨트롤을 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Windows 커뮤니티 도구 키트에서 제어 합니다.

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/) 는 전체.NET Framework의 플랫폼 간, 간단한 및 쉽게 확장 가능 버전을 구현 하는 오픈 소스 프레임 워크입니다. 전체.NET Framework에 비해,.NET Core 시작 시간이 단축 됩니다 및 대부분의 Api 최적화 되었습니다.

해당 첫 번째 여러 릴리스를 통해.NET Core의 포커스를 웹 또는 백 엔드 앱을 지원 했습니다. .NET Core를 사용 하 여 확장 가능한 웹 앱 또는 Windows, Linux 또는 Docker 컨테이너와 같은 마이크로 서비스 아키텍처에서 호스팅될 수 있는 Api를 쉽게 빌드할 수 있습니다.

.NET Core 3는 .NET Core의 차기 주 릴리스입니다. 이 예정된 릴리스의 주요 기능은 Windows Forms 및 WPF 앱을 포함하는 Windows 데스크톱 앱에 대한 지원입니다. .NET Core 3에서 신규 및 기존 Windows 데스크톱 앱을 실행할 수 있으며.NET Core에서 제공 하는 모든 혜택을 누리실 수 있습니다. [XAML Islands](xaml-islands.md)에 호스트되는 UWP 컨트롤을 .NET Core 3를 대상으로 하는 Windows Forms 및 WPF 앱에서도 사용할 수 있습니다.

> [!NOTE]
> WPF 및 Windows Forms 하지 점점 플랫폼 간 및 WPF 또는 Windows Forms Linux 및 MacOS에서 실행할 수 없습니다. WPF 및 Windows Forms의 UI 구성 요소가 Windows 렌더링 시스템에 종속 되지 않았습니다.

자세한 내용은 다음 문서를 참조하세요.

* [.NET Core 3.0 Preview 1 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [.NET Core 3.0 Preview 2 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [.NET Core 3.0 Preview 2 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [.NET Core 3.0 Preview 4 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [.NET Core 3.0의 새로운 기능](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).