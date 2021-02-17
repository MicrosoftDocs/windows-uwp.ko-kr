---
description: 프로젝트 리유니언, 개발자에게 제공되는 혜택, 현재 개발자에게 준비된 사항 및 피드백 제공 방법에 대해 알아보세요.
title: 프로젝트 리유니언
ms.topic: article
ms.date: 01/07/2021
keywords: windows win32, 데스크톱 개발, 프로젝트 리유니언
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e4b5507c36da520c7356b07857b8532162e05785
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100335094"
---
# <a name="build-windows-apps-with-project-reunion-prerelease"></a>프로젝트 리유니언을 사용하여 Windows 앱 빌드(시험판)

프로젝트 리유니언 0.1 시험판은 Windows 앱 개발 플랫폼의 한 단계 발전을 나타내는 새 개발자 구성 요소 및 도구 집합의 미리 보기입니다. 프로젝트 리유니언은 광범위한 대상 Windows 10 OS 버전 세트의 모든 앱에서 일관된 방식으로 사용할 수 있는 통합 API 및 도구 집합을 제공합니다. 프로젝트 리유니언은 기존 Windows 앱 플랫폼과 UWP, 네이티브 Win32, .NET과 같은 프레임워크를 대체하지 않습니다. 대신 개발자가 이러한 기존 플랫폼에서 신뢰할 수 있는 공통 API 및 도구 집합으로 이러한 플랫폼을 보완합니다.

> [!NOTE]
> 프로젝트 리유니언 0.1 시험판은 초기 개발자 미리 보기입니다. 개발 환경에서 이 릴리스를 사용해 보는 것이 좋습니다. 그러나 프로젝트 리유니언은 현재 릴리스와 최종 릴리스 사이에 여러 가지 면에서 변경될 것입니다. 프로젝트 리유니언 0.1 시험판은 프로덕션 환경에서 사용되는 앱을 지원하지 않습니다. **프로젝트 리유니언** 은 이후 릴리스에서 변경될 수 있는 코드 이름입니다.

## <a name="goals-of-project-reunion"></a>프로젝트 리유니언의 목표

프로젝트 리유니언은 OS에서 분리되어 NuGet 패키지를 통해 개발자에게 릴리스되는 구현이 포함된 광범위한 Windows API 집합을 제공합니다. 프로젝트 리유니언은 Windows SDK를 대체하기 위한 것이 아닙니다. Windows SDK는 계속 그대로 작동하며 OS 및 Windows SDK 릴리스를 통해 제공되는 API를 통해 계속해서 발전하는 Windows의 핵심 구성 요소가 많이 있습니다. 개발자는 각자의 속도대로 프로젝트를 도입하는 것이 좋습니다.

프로젝트 리유니언은 다음 목표를 지원하기 위해 만들어졌습니다.

#### <a name="unified-api-surface-across-different-types-of-windows-apps"></a>다양한 유형의 Windows 앱에서 통합된 API 표면

Windows 앱을 만들려는 개발자는 여러 앱 플랫폼과 프레임워크 중에서 선택해야 합니다. 각 플랫폼은 다른 플랫폼을 사용하여 빌드된 앱에서 사용할 수 있는 많은 기능과 API를 제공하지만 일부 기능과 API는 특정 플랫폼에서만 사용할 수 있습니다. 프로젝트 리유니언은 모든 Windows 10 앱에 대한 Windows API 액세스를 통합합니다. 선택한 앱 모델에 관계 없이 프로젝트 리유니언에서 사용할 수 있는 동일한 Windows API 집합에 액세스할 수 있습니다.

앞으로 다른 앱 모델 간의 차이를 없애기 위해 프로젝트 리유니온에 더 많은 투자를 할 계획입니다. 프로젝트 리유니언에는 WinRT API와 네이티브 C API가 포함됩니다.

#### <a name="consistent-support-across-windows-10-versions"></a>Windows 10 버전에서 일관된 지원

Windows API가 새로운 OS 버전으로 계속 발전함에 따라 개발자는 애플리케이션 대상자를 위해 버전 간의 모든 차이를 고려하여 [버전 적응 코드](/windows/uwp/debug-test-perf/version-adaptive-code)와 같은 기술을 사용해야 합니다. 이로 인해 코드와 개발 환경이 복잡해질 수 있습니다. 프로젝트 리유니언을 사용하면 다양한 OS 버전과 모든 Windows 10 디바이스에서 프로젝트 리뉴니언 API에 대한 액세스를 통합할 수 있으므로 최신 버전의 Windows뿐만 아니라 더 많은 개발자가 업데이트를 사용할 수 있습니다. 현재 계획은 프로젝트 리뉴니언이 Windows 10, 버전 1809 및 모든 이후 버전의 Windows 10을 지원하는 것입니다.

#### <a name="faster-release-cadence"></a>더 빠른 릴리스 주기

새로운 Windows API 및 기능은 일반적으로 1년에 1~2회의 릴리스 주기로 발생하는 OS 릴리스와 연계되어 있습니다. 프로젝트 리유니언을 사용하면 새로운 프로덕션 품질의 API 및 기능을 보다 자주 그리고 빠르게 릴리스할 수 있습니다.

## <a name="get-started"></a>시작하기

프로젝트 리유니언 0.1 시험판에는 다음 기능 영역에 대한 새로운 API가 포함되어 있습니다.

| 기능 | 설명 |
|---------|-------------|
| [MRT Core(최신 리소스 관리 시스템)](mrtcore/mrtcore-overview.md) | MRT Core는 프로젝트 리유니언의 일부로 배포되는 최신 [Windows 리소스 관리 시스템](/windows/uwp/app-resources/resource-management-system)의 간소화된 버전입니다. |
| [DWriteCore](dwritecore.md) | DWriteCore는 Window 버전에서 Windows 8까지 실행되는 DirectWrite의 한 형태로, 플랫폼 간에서 사용할 수 있습니다. |

다른 구성 요소를 프로젝트 리유니언에 도입하기 위한 향후 계획에 대해서는 [여기](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)에서 자세히 알아볼 수 있습니다.

> [!NOTE]
> Windows UI 라이브러리(WinUI), MSIX 및 WebView2를 비롯한 특정 기타 기존 구성 요소는 이미 OS에서 분리되어 있으며 프로젝트 리유니언 지침을 따릅니다(예: Windows 10, 버전 1809 이상의 OS 버전에서 지원됨). 그러나 이러한 다른 구성 요소는 현재 프로젝트 리유니언 NuGet 패키지의 일부가 아닙니다.  

### <a name="set-up-your-development-environment"></a>개발 환경 설정

프로젝트 리유니언 0.1 시험판을 사용해 보려는 경우 제공된 C++ 샘플 중 하나로 시작해야 합니다. 이러한 샘플은 프로젝트 리유니언 NuGet 패키지를 사용하도록 미리 구성되어 있습니다. 이 미리 보기는 자체 프로젝트에 프로젝트 리뉴니언 NuGet 패키지 설치를 지원하지 않습니다. 샘플 중 하나를 사용하도록 개발 환경을 설정하려면 다음 단계를 따릅니다.

1. 개발용 컴퓨터에 Windows 10, 버전 1809(빌드 17763) 이상의 OS가 설치되어 있는지 확인합니다.

2. [Visual Studio 2019, 버전 16.9 Preview 2(또는 그 이상)](https://visualstudio.microsoft.com/vs/preview/)를 설치합니다. Visual Studio 설치 관리자에서 다음 항목이 선택되어 있는지 확인하세요.
    - **워크로드** 탭에서 다음 워크로드가 선택되어 있는지 확인합니다.
        - **.NET 데스크톱 개발**
        - **C++를 사용한 데스크톱 개발**
        - **유니버설 Windows 플랫폼 개발**(또한 **설치 세부 정보** 창에서 이 워크로드에 대해 선택적 **C++ (v142) 유니버설 Windows 플랫폼 도구** 구성 요소도 선택되어 있는지 확인)
    - **개별 구성 요소** 탭의 **SDK, 라이브러리 및 프레임워크** 섹션에서 **Windows 10 SDK(10.0.19041.0)** 가 선택되어 있는지 확인합니다.

3. Visual Studio Marketplace에서 최신 버전의 [C++/WinRT Visual Studio 확장(VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)을 설치합니다.

4. **nuget.org** 에 사용하도록 설정된 NuGet 패키지 원본이 시스템에 있는지 확인합니다. 자세한 내용은 [일반적인 NuGet 구성](/nuget/consume-packages/configuring-nuget-behavior)을 참조하십시오.

5. [WinUI 3 Preview 4 VSIX 패키지](https://aka.ms/winui3/preview3-download)를 다운로드하여 설치합니다. 이 단계는 WinUI 3을 사용하도록 이미 구성된 Hello World 및 MRT Core 샘플에만 필요합니다. VSIX 패키지를 Visual Studio에 추가하는 방법에 대한 지침은 [Visual Studio 확장 찾기 및 사용](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)을 참조하세요.

6. 다음 샘플을 복제하고 탐색합니다.
    - [DWriteCore 갤러리 샘플](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery): 이 샘플 애플리케이션은 [DWriteCore](dwritecore.md) API를 보여줍니다.
    - [MRT Core 샘플](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore): 이 샘플 애플리케이션은 [MRT Core](mrtcore/mrtcore-overview.md) API를 보여줍니다.
    - [Hello World 샘플](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp): 이 샘플은 프로젝트 리유니언 NuGet 패키지와의 기본 통합을 보여줍니다.

## <a name="known-issues"></a>알려진 문제

프로젝트 리유니언 0.1 시험판에는 다음과 같은 제한 사항이 있습니다.

 - 이 릴리스는 MSIX 패키지된 C++/Win32 앱에만 지원됩니다.
 - 이 릴리스는 C#을 지원하지 않습니다.
 - 이 릴리스는 .NET 5를 지원하지 않습니다.

## <a name="developer-roadmap"></a>개발자 로드맵

최신 프로젝트 리유니언 계획은 [Github 페이지](https://github.com/microsoft/ProjectReunion)를 참조하세요.

## <a name="give-feedback-and-contribute"></a>피드백 제공 및 기여

프로젝트 리유니언을 오픈 소스 프로젝트로 빌드하고 있습니다. [Github 페이지](https://github.com/microsoft/ProjectReunion)에 프로젝트 리유니언을 실현하기 위한 계획에 대한 자세한 정보가 있으며, 개발 프로세스에 참여하도록 귀하를 초대합니다. 질문을 하거나 토론을 시작하거나 기능을 제안하려면 [기여자 가이드](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)를 확인하세요. 프로젝트 리유니언이 귀하와 같은 개발자에게 가장 큰 혜택을 제공하기를 바랍니다.
