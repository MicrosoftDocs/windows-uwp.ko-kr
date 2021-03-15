---
description: 프로젝트 리유니언, 개발자에게 제공되는 혜택, 현재 개발자에게 준비된 사항 및 피드백 제공 방법에 대해 알아보세요.
title: 프로젝트 리유니언
ms.topic: article
ms.date: 03/09/2021
keywords: windows win32, 데스크톱 개발, 프로젝트 리유니언
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 9e2e4bda8440a9a504cba78286de491236d6bd49
ms.sourcegitcommit: 2a71bb5c56bed80f26fb67a47ae8f9198c431760
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103366238"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05-preview-march-2021"></a>프로젝트 리유니언 0.5 미리 보기(2021년 3월 출시 예정)를 사용하여 데스크톱 Windows 앱 빌드

프로젝트 리유니언은 Windows 앱 개발 플랫폼의 차세대 발전상을 보여주는 새로운 개발자 구성 요소 및 도구 세트입니다. 프로젝트 리유니언은 광범위한 대상 Windows 10 OS 버전의 모든 앱에서 일관적인 방식으로 사용할 수 있는 통합 API 및 도구 세트를 제공합니다. 

프로젝트 리유니언은 .NET(Windows Forms 및 WPF 포함)이나 C++/Win32 같은 기존 데스크톱 Windows 앱 플랫폼과 프레임워크를 대체하지 않습니다. 대신 개발자가 이러한 기존 플랫폼에서 신뢰할 수 있는 공통 API 및 도구 집합으로 이러한 플랫폼을 보완합니다.

> [!NOTE]
> 프로젝트 리유니언 0.5 미리 보기는 개발자용 미리 보기입니다. 개발 환경에서 이 릴리스를 사용해 보는 것이 좋습니다. 하지만 프로젝트 리유니언 현재 릴리스의 여러 가지 기능이 1.0 릴리스에서 변경될 것입니다. 프로젝트 리유니언 0.5 미리 보기는 프로덕션 환경에서 사용되는 앱을 지원하지 않습니다. **프로젝트 리유니언** 은 이후 릴리스에서 변경될 수 있는 코드 이름입니다.

## <a name="benefits-of-project-reunion-for-windows-app-developers"></a>Windows 앱 개발자가 프로젝트 리유니언을 사용하면 좋은 점

프로젝트 리유니언은 OS에서 분리되어 NuGet 패키지를 통해 개발자에게 릴리스되는 구현이 포함된 광범위한 Windows API 집합을 제공합니다. 프로젝트 리유니언은 Windows SDK를 대체하기 위한 것이 아닙니다. Windows SDK는 계속 그대로 작동하며 OS 및 Windows SDK 릴리스를 통해 제공되는 API를 통해 계속해서 발전하는 Windows의 핵심 구성 요소가 많이 있습니다. 개발자는 각자의 속도대로 프로젝트를 도입하는 것이 좋습니다.

#### <a name="unified-api-surface-across-desktop-app-platforms"></a>데스크톱 앱 플랫폼 간 통합 API 표면

데스크톱 Windows 앱을 만들려는 개발자는 여러 앱 플랫폼과 프레임워크 중에서 선택해야 합니다. 각 플랫폼은 다른 플랫폼을 사용하여 빌드된 앱에서 사용할 수 있는 많은 기능과 API를 제공하지만 일부 기능과 API는 특정 플랫폼에서만 사용할 수 있습니다. 프로젝트 리유니언은 모든 데스크톱 Windows 10 앱에 사용되는 Windows API 액세스를 통합합니다. 선택한 앱 모델에 관계 없이 프로젝트 리유니언에서 사용할 수 있는 동일한 Windows API 집합에 액세스할 수 있습니다.

앞으로 다른 앱 모델 간의 차이를 없애기 위해 프로젝트 리유니온에 더 많은 투자를 할 계획입니다. 프로젝트 리유니언에는 WinRT API와 네이티브 C API가 포함됩니다.

#### <a name="consistent-support-across-windows-10-versions"></a>Windows 10 버전에서 일관된 지원

Windows API가 새로운 OS 버전으로 계속 발전함에 따라 개발자는 애플리케이션 대상자를 위해 버전 간의 모든 차이를 고려하여 [버전 적응 코드](/windows/uwp/debug-test-perf/version-adaptive-code)와 같은 기술을 사용해야 합니다. 이로 인해 코드와 개발 환경이 복잡해질 수 있습니다.

프로젝트 리유니언 API는 Windows 10 버전 1809 이상의 모든 Windows 10 버전에서 작동합니다. 즉, 고객이 Windows 10 버전 1809 이상을 사용하는 한, 새 프로젝트 리유니언 API 및 기능이 출시되는 즉시 버전 적응 코드를 작성하지 않고 바로 사용할 수 있습니다.

#### <a name="faster-release-cadence"></a>더 빠른 릴리스 주기

새로운 Windows API 및 기능은 일반적으로 1년에 1~2회의 릴리스 주기로 발생하는 OS 릴리스와 연계되어 있습니다. Windows 개발 플랫폼의 혁신적인 기능이 출시되는 즉시 개발자들이 신속하게 사용할 수 있도록 프로젝트 리유니언은 더 빠르게 업데이트를 제공합니다.

## <a name="components-available-with-project-reunion"></a>프로젝트 리유니언과 함께 제공되는 구성 요소

프로젝트 리유니언 0.5 미리 보기에는 다음 구성 요소가 포함되어 있습니다.

| 구성 요소 | 설명 |
|---------|-------------|
| [Windows UI 라이브러리 3](../winui/winui3/index.md) | WinUI(Windows UI 라이브러리) 3은 데스크톱(.NET 및 C++/Win32) 및 UWP 앱에 모두 사용할 수 있는 차세대 Windows 사용자 환경(UX) 플랫폼입니다. 이 릴리스에는 [WinUI 기반 사용자 인터페이스를 사용하여 앱 빌드](..\winui\winui3\index.md#create-winui-projects)를 시작하는 데 도움이 되는 Visual Studio 프로젝트 템플릿, 그리고 WinUI 라이브러리가 포함된 NuGet 패키지가 들어 있습니다.  |
| [MRT Core를 사용하여 리소스 관리](mrtcore/mrtcore-overview.md) | MRT Core는 앱에서 사용하는 리소스를 로드하고 관리하기 위한 API를 제공합니다. MRT Core는 최신 [Windows 리소스 관리 시스템](/windows/uwp/app-resources/resource-management-system)의 간소화된 버전입니다. |
| [DWriteCore를 사용하여 텍스트 렌더링](dwritecore.md) | DWriteCore는 디바이스 독립적인 텍스트 레이아웃 시스템, 하드웨어 가속 텍스트, 다중 형식 텍스트 및 광범위한 언어 지원을 포함하여 텍스트 렌더링을 위한 모든 현재 DirectWrite 기능을 제공합니다.  |

다른 구성 요소를 프로젝트 리유니언에 도입하기 위한 향후 계획에 대해서는 [여기](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)에서 자세히 알아볼 수 있습니다.

## <a name="set-up-your-development-environment"></a>개발 환경 설정

1. 개발용 컴퓨터에 Windows 10, 버전 1809(빌드 17763) 이상의 OS가 설치되어 있는지 확인합니다.

2. 아직 [Visual Studio 2019 버전 16.10 미리 보기](https://visualstudio.microsoft.com/vs/preview/)(이상)를 설치하지 않은 경우 지금 설치합니다. 

    > [!NOTE]
    > Visual Studio 2019, 버전 16.9도 Project Reunion을 지원하지만 WinUI 3 도구 기능은 지원하지 않습니다. WinUI 3 도구 지원에 대한 자세한 내용은 Windows UI 라이브러리 3 - 프로젝트 Reunion 0.5 미리 보기(2021년 3월)를 참조하세요.

    Visual Studio를 설치할 때 다음 구성 요소를 포함해야 합니다.
    - **워크로드** 탭에서 **유니버설 Windows 플랫폼 개발** 을 선택합니다.
    - **개별 구성 요소** 탭의 **SDK, 라이브러리 및 프레임워크** 섹션에서 **Windows 10 SDK(10.0.19041.0)** 가 선택되어 있는지 확인합니다.

    .NET 앱을 빌드하려면 다음 구성 요소도 포함해야 합니다.
    - **.NET 데스크톱 개발** 워크로드

    C++ 앱을 빌드하려면 다음 구성 요소도 포함해야 합니다.
    - **C++를 사용하여 데스크톱 개발** 워크로드
    - **유니버설 Windows 플랫폼 개발** 워크로드의 선택적 구성 요소인 **C++(v142) 유니버설 Windows 플랫폼 도구**

3. 이전 WinUI 3 미리 보기의 [WinUI 3 미리 보기 확장](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates)을 설치한 경우 확장을 제거합니다. 확장을 제거하는 방법에 대한 자세한 내용은 [Visual Studio용 확장 관리](/visualstudio/ide/finding-and-using-visual-studio-extensions)를 참조하세요.

4. **nuget.org** 에 사용하도록 설정된 NuGet 패키지 원본이 시스템에 있는지 확인합니다. 자세한 내용은 [일반적인 NuGet 구성](/nuget/consume-packages/configuring-nuget-behavior)을 참조하십시오.

5. Visual Studio 2019에서 **확장** > **확장 관리** 를 클릭하고, **프로젝트 리유니언** 을 검색하고, **프로젝트 리유니언** 확장을 설치합니다. 또는 Visual Studio Marketplace에서 직접 [프로젝트 리유니언 확장](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion)을 다운로드하여 설치할 수도 있습니다. Visual Studio에 VSIX 패키지를 추가하는 방법에 대한 지침은 [Visual Studio용 확장 관리](/visualstudio/ide/finding-and-using-visual-studio-extensions)를 참조하세요.

6. 라이브 시각적 트리, 핫 다시 로드, 라이브 속성 탐색기와 같은 WinUI 3 도구를 사용하려면 [여기 지침](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)에 설명된 대로 Visual Studio 미리 보기 기능과 함께 WinUI 3 도구를 사용하도록 설정해야 합니다.

## <a name="get-started-developing-with-project-reunion"></a>프로젝트 리유니언을 사용하여 개발 시작

프로젝트 리유니언 확장에 포함된 WinUI 3 프로젝트 템플릿으로 만든 새 앱에서 프로젝트 리유니언 0.5 미리 보기를 사용할 수도 있고, NuGet 패키지를 설치하여 기존 프로젝트에서 프로젝트 리유니언 구성 요소를 사용할 수도 있습니다.

> [!NOTE]
> 프로젝트 리유니언 0.5 미리 보기는 MSIX 패키지 앱만 지원합니다.

#### <a name="create-a-new-project-that-uses-project-reunion"></a>프로젝트 리유니언을 사용하는 새 프로젝트 만들기

Visual Studio 2019용 프로젝트 리유니언 0.5 미리 보기 확장에는 WinUI 3 기반 UI 계층으로 프로젝트를 생성하고 다른 모든 프로젝트 리유니언 API에 대한 액세스를 제공하는 프로젝트 템플릿이 포함되어 있습니다. 이러한 템플릿을 사용하여 MSIX 패키지 데스크톱 앱(C#/.NET 5 또는 C++/WinRT) 또는 프로젝트 리유니언을 사용하는 UWP 앱을 빌드할 수 있습니다. 사용 가능한 프로젝트 템플릿에 대한 자세한 내용은 [WinUI 프로젝트 만들기](..\winui\winui3\index.md#create-winui-projects)를 참조하세요.

프로젝트 리유니언 0.5 미리 보기를 사용하는 새 프로젝트를 만들려면 다음을 수행합니다.

1. 다음 문서의 지침을 따르세요.

    - [데스크톱 앱용 WinUI 3 시작](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [UWP 앱용 WinUI 3 시작](..\winui\winui3\get-started-winui3-for-uwp.md)

2. 프로젝트를 만든 후에는 데스크톱 및 UWP 앱에서 일반적으로 사용할 수 있는 모든 Windows 및 .NET API는 물론이고 다음과 같은 프로젝트 리유니언 API에 액세스할 수 있습니다.

    - [Windows UI 라이브러리 3](../winui/winui3/index.md)
    - [리소스 관리 MRT Core](mrtcore/mrtcore-overview.md)
    - [DWriteCore를 사용하여 텍스트 렌더링](dwritecore.md)

새 프로젝트에서 프로젝트 리유니언을 사용하는지 확인하려면 **솔루션 탐색기** 의 프로젝트 아래에서 **종속성** > **패키지** 노드를 확장합니다. 다음 그림처럼 이 노드 아래에 여러 **Microsoft.ProjectReunion** 패키지가 나열되어야 합니다.

![솔루션 탐색기 창에 표시되는 프로젝트 리유니언 패키지의 스크린샷](images/reunion-packages.png)

#### <a name="use-project-reunion-in-an-existing-project"></a>기존 프로젝트에서 프로젝트 리유니언 사용

기존 데스크톱 프로젝트(C#/.NET 5 또는 C++/WinRT)에서 프로젝트 리유니언을 사용하려는 경우 해당 프로젝트에 프로젝트 리유니언 0.5 미리 보기 NuGet 패키지를 설치하면 됩니다. 

> [!NOTE]
> 이 시나리오에서는 프로젝트 리유니언에 포함된 비-WinUI 3 구성 요소만 프로젝트에 사용할 수 있습니다. WinUI 3을 사용하려면 이전 섹션의 설명에 따라 WinUI 3 프로젝트 템플릿 중 하나를 사용하여 새 프로젝트를 만들어야 합니다.

1. Visual Studio 2019에서 기존 데스크톱 프로젝트(C#/.NET 5 또는 C++/WinRT) 또는 UWP 프로젝트를 엽니다.

2. [패키지 참조](/nuget/consume-packages/package-references-in-project-files)를 사용하도록 설정합니다.

    1. Visual Studio에서 **도구 -> NuGet 패키지 관리자 -> 패키지 관리자 설정** 을 클릭합니다.
    2. **기본 패키지 관리 형식** 으로 **PackageReference** 를 선택합니다.

3. **솔루션 탐색기** 에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리** 를 선택합니다.

4. **NuGet 패키지 관리자** 창에서 **찾아보기** 탭을 선택하고 `Microsoft.ProjectReunion`를 검색합니다.

5. **Microsoft.ProjectReunion** 패키지가 검색되면 **NuGet 패키지 관리자** 창의 오른쪽 창에서 **설치** 를 클릭합니다.

6. 패키지를 설치한 후에는 다음 프로젝트 리유니언 API 및 구성 요소를 프로젝트에 사용할 수 있습니다.

    - [리소스 관리 MRT Core](mrtcore/mrtcore-overview.md)
    - [DWriteCore를 사용하여 텍스트 렌더링](dwritecore.md)

#### <a name="deploying-apps-that-use-project-reunion"></a>프로젝트 리유니언을 사용하는 앱 배포

현재 프로젝트 리유니언을 사용하는 앱은 [MSIX](/windows/msix)를 사용하여 패키징해야 합니다. 기본적으로 Visual Studio용 프로젝트 리유니언 확장과 함께 제공되는 WinUI 프로젝트 템플릿 중 하나로 프로젝트를 만들면 앱을 MSIX 패키지에 빌드하도록 구성된 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)가 프로젝트에 포함됩니다. 앱의 MSIX 패키지를 빌드하도록 이 프로젝트를 구성하는 방법에 대한 자세한 내용은 [Visual Studio에서 데스크톱 또는 UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps)을 참조하세요.

앱의 MSIX 패키지를 빌드한 후에는 여러 옵션 중에 선택하여 다른 컴퓨터에 배포할 수 있습니다. 자세한 내용은 [MSIX 배포 관리](/windows/msix/desktop/managing-your-msix-deployment-overview)를 참조하세요.

> [!NOTE]
> 프로젝트 리유니언 0.5 미리 보기를 사용하는 앱은 Microsoft Store에 게시할 수 없습니다.

## <a name="samples"></a>샘플

현재 살펴볼 수 있는 프로젝트 리유니언 샘플은 다음과 같습니다.

- [DWriteCore 갤러리 샘플](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery): 이 샘플 애플리케이션은 [DWriteCore](dwritecore.md) API를 보여줍니다.
- [MRT Core 샘플](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore): 이 샘플 애플리케이션은 [MRT Core](mrtcore/mrtcore-overview.md) API를 보여줍니다.
- [Hello World 샘플](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp): 이 샘플은 프로젝트 리유니언 NuGet 패키지와의 기본 통합을 보여줍니다.

## <a name="limitations-and-known-issues"></a>제한 사항 및 알려진 문제

- 이 릴리스는 프로덕션 환경에서 사용되는 앱을 지원하지 않습니다. 버그, 제한 사항 및 기타 문제가 발생할 수도 있습니다.
- 이 릴리스는 MSIX 패키지 데스크톱 앱(C#/.NET 5 또는 C++/Win32)에서만 사용할 수 있습니다. 패키징되지 않은 데스크톱 앱에는 사용할 수 없습니다.
- 프로젝트 리유니언 0.5 미리 보기를 사용하는 모든 프로젝트에도 [WinUI 3의 도구 제한](..\winui\winui3\index.md#developer-tools)이 적용됩니다.

## <a name="developer-roadmap"></a>개발자 로드맵

최신 프로젝트 리유니언 계획은 [Github 페이지](https://github.com/microsoft/ProjectReunion)를 참조하세요.

## <a name="give-feedback-and-contribute"></a>피드백 제공 및 기여

프로젝트 리유니언을 오픈 소스 프로젝트로 빌드하고 있습니다. [Github 페이지](https://github.com/microsoft/ProjectReunion)에 프로젝트 리유니언을 실현하기 위한 계획에 대한 자세한 정보가 있으며, 개발 프로세스에 참여하도록 귀하를 초대합니다. 질문을 하거나 토론을 시작하거나 기능을 제안하려면 [기여자 가이드](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)를 확인하세요. 프로젝트 리유니언이 귀하와 같은 개발자에게 가장 큰 혜택을 제공하기를 바랍니다.
