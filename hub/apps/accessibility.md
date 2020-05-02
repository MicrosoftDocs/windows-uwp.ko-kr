---
title: Windows 10의 접근성
description: 이 페이지에서는 접근성 지원 Windows 앱 개발을 시작하기 위한 정보를 제공합니다.
ms.topic: article
ms.date: 09/12/2019
keywords: Windows 10의 접근성, 접근성, 접근성 지원 win32 앱 빌드, 접근성 지원 UWP 앱 빌드, 접근성 지원 WPF 앱 빌드, 접근성 지원 WinForms 앱 빌드
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 2739546061febcfc96403ed5520fdcbd8bc2c462
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76725936"
---
# <a name="accessibility-in-windows-10"></a>Windows 10의 접근성

![접근성 히어로 이미지](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>애플리케이션에 사용자에게 모든 기능을 제공할 수 있는 접근성 빌드

전자 미디어를 포함한 제품 및 서비스는 가능한 한 많은 사용자에게 완벽하고 성공적인 환경을 제공하도록 설계된 경우에 액세스할 수 있습니다.

장애(일시적 및 영구적 모두), 개인 기본 설정, 특정 작업 스타일 또는 상황에 따른 제약(예: 공유 작업 공간, 운전, 요리, 눈부심 등)이 있는 사용자를 위해 향상된 기능성 및 유용성을 갖춘 포괄적인 접근성 지원 Windows 애플리케이션을 빌드합니다. 몇 가지 일반적인 솔루션에서는 대체 형식(예: 비디오 자막)으로 정보를 제공하거나 보조 기술(예: 화면 읽기)을 사용할 수 있습니다.

**계단 또는 엘리베이터를 사용해야 하는지에 관계없이 모든 사람은 건물 안의 동일한 방에 접근할 수 있어야 합니다.**

이 페이지에서는 다양한 Windows 개발 프레임워크에서 Windows 애플리케이션을 빌드하는 개발자, 화면 읽기 및 돋보기와 같은 도구를 빌드하는 보조 기술 개발자, 자동화된 애플리케이션 테스트 스크립트를 만드는 소프트웨어 테스트 엔지니어에게 접근성 지원을 제공하는 방법에 대한 정보를 제공합니다.

## <a name="platform-specific-documentation"></a>플랫폼별 설명서

:::row:::
   :::column:::
      ![UWP(유니버설 Windows 플랫폼)](images/platform-uwp.png)

      **UWP(유니버설 Windows 플랫폼)**

      모든 Windows 디바이스(PC, 휴대폰, Xbox One, HoloLens 등)의 Windows 10 애플리케이션 및 게임용 최신 플랫폼에서 접근성 지원 앱과 도구를 개발하여 Microsoft Store에 게시합니다.

      [포괄 소프트웨어 디자인](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [포괄 Windows 앱 개발](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [접근성 테스트](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [스토어의 접근성](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32 플랫폼 앱](images/platform-win32.png)

      **Win32 플랫폼**

      C/C++ Windows 애플리케이션용 원본 플랫폼에서 접근성 지원 앱과 도구를 개발합니다.

      [Windows 접근성 및 자동화의 새로운 기능](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [접근성 지원 Windows용 애플리케이션 개발](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [접근성 지원 Windows용 UI 프레임워크 개발](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Windows용 보조 기술 개발](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [접근성 테스트](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [레거시 접근성 및 자동화 기술 - MSAA 및 UI 자동화](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Windows 접근성 기능](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [접근성 있는 앱 디자인을 위한 지침](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF 플랫폼](images/platform-wpf2-small.png)

      **WPF(Windows Presentation Foundation)**

      XAML UI 모델 및 .NET Framework를 사용하여 관리형 Windows 애플리케이션용으로 설정된 플랫폼에서 접근성 지원 앱과 도구를 개발합니다.

      [접근성 모범 사례](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [UI 자동화 기본 사항](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [관리 코드에 대한 UI 자동화 공급자](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [관리 코드에 대한 UI 자동화 클라이언트](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [UI 자동화 컨트롤 패턴](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [UI 자동화 텍스트 패턴](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [UI 자동화 컨트롤 형식](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [UI 자동화 사양 및 커뮤니티 약속](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Windows Forms 플랫폼 앱](images/platform-winforms.png)

      **WinForms(Windows Forms)**

      XAML UI 모델 및 .NET Framework를 사용하여 관리형 Windows 애플리케이션용 접근성 지원 앱과 도구를 개발합니다.

      [Windows Forms 접근성](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [접근성 지원 Windows 애플리케이션 만들기](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [접근성 지침을 지원하는 Windows Forms 컨트롤의 속성](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Windows Form의 컨트롤에 접근성 정보 제공](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **웹 접근성**

      Microsoft Edge에서 접근성 지원 웹 사이트를 설계, 빌드 및 테스트합니다.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [웹 접근성 소개](https://docs.microsoft.com/microsoft-edge/accessibility)

      [접근성 있는 웹 사이트 디자인](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [접근성 있는 웹 사이트 빌드](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [접근성 지원 웹 사이트 테스트](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>샘플

다양한 접근성의 특징과 기능을 보여 주는 Windows 샘플 전체를 다운로드하고 실행합니다.

:::row:::
   :::column:::
      [코드 샘플 브라우저](https://docs.microsoft.com/samples/browse/)

      MSDN 코드 갤러리를 대체하는 새로운 샘플 브라우저입니다.
   :::column-end:::
   :::column:::
      [MSDN 코드 갤러리(사용 중지됨)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight 및 기타 제품용 샘플을 다운로드합니다.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub의 Windows 클래식 샘플](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      이러한 샘플에서는 Windows 및 Windows Server의 기능 및 프로그래밍 모델을 보여 줍니다. 
   :::column-end:::
   :::column:::
      [GitHub의 UWP(유니버설 Windows 플랫폼) 코드 샘플](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      이러한 샘플에서는 Windows 10용 Windows SDK(소프트웨어 개발 키트)의 UWP(유니버설 Windows 플랫폼)에 대한 API 사용 패턴을 보여 줍니다.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML 컨트롤 갤러리](https://github.com/microsoft/Xaml-Controls-Gallery)

      이 앱에서는 흐름 디자인 시스템에서 지원되는 다양한 Xaml 컨트롤을 보여 줍니다.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>동영상

일반적인 접근성 문제에 맞게 접근성 지원 Windows 애플리케이션을 빌드하는 방법과 Microsoft에서 이를 해결하는 방법을 설명하는 다양한 비디오입니다.

:::row:::
   :::column:::
      **Windows 앱에서 접근성 기능으로 시작하는 방법**
   :::column-end:::
   :::column:::
      **One Dev Minute: 접근성 앱 개발**
   :::column-end:::
   :::column:::
      **장애 및 접근성 소개**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **해킹에서 제품까지, Windows 10용 아이 컨트롤**
   :::column-end:::
   :::column:::
      **Windows 10의 접근성**
   :::column-end:::
   :::column:::
      **접근성 지원 UWP 앱 빌드에 대한 소개**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Windows 접근성 기능 설계**
   :::column-end:::
   :::column:::
      **모든 사용자에게 Windows 10 접근성 기능 제공**
   :::column-end:::
   :::column:::
      **더 쉽게 볼 수 있도록 마우스 포인터 만들기**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>다른 리소스

:::row:::
   :::column span="3":::
      **블로그 및 뉴스**

      Microsoft 접근성 세계에서 가장 최신입니다.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [뉴스](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [접근성 블로그](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Windows UI 자동화 블로그](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **커뮤니티 및 지원**

      Windows 개발자와 사용자가 함께 만나서 학습하는 장소입니다.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Windows 커뮤니티 - 접근성](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Windows 접근성 및 자동화 개발 포럼](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [장애인용 Answer Desk](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
