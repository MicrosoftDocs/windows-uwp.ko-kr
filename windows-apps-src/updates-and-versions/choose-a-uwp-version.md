---
title: UWP 버전 선택
description: Microsoft Visual Studio에서 UWP 앱을 작성하는 경우 대상 버전을 선택할 수 있습니다. 다른 UWP 버전 간 차이점과 새 프로젝트 및 기존 프로젝트에서 선택을 구성하는 방법을 알아봅니다.
ms.date: 5/12/2019
ms.topic: article
keywords: Windows 10, UWP, 버전, 빌드, 버전, Windows, 선택, 업데이트, 업데이트
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
ms.localizationpriority: medium
f1_keywords:
- UniversalProjects.TargetPlatformWizardPicker
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 1987ad1267b3a48c0488e2d0af985f316097d3e2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173807"
---
# <a name="choose-a-uwp-version"></a>UWP 버전 선택

각 Windows 10 버전은 UWP 플랫폼에 새로운 기능과 향상된 기능을 가져 왔습니다. Microsoft Visual Studio에서 UWP 앱을 만들 때 대상 버전을 선택할 수 있습니다. [.NET 표준 2.0](/dotnet/standard/net-standard)을 사용하는 프로젝트는 **최소 버전**이 빌드 16299 이상이어야 합니다.

> [!WARNING]
> 최신 버전의 Visual Studio에서 만든 모든 UWP 프로젝트는 Visual Studio 2015에서 열 수 없습니다.

다음 표에는 사용 가능한 Windows 10 버전이 설명되어 있습니다. 이 표는 UWP 앱 빌드에만 적용되며, UWP 앱은 Windows 10에서만 지원됩니다. 이전 버전의 Windows에 대한 UWP 앱은 개발할 수 없으며, 대상 버전을 지정하려면 [적절한 SDK 빌드를 설치](https://developer.microsoft.com/windows/downloads#_blank)해야 합니다.

| Version | 설명 |
| --- | --- |
| 빌드 19041(버전 2004) | 2020년 5월에 출시된 최신 버전의 Windows 10입니다. 다음은 이 릴리스의 주요 기능입니다. </br> \* **WSL2:** Linux용 Windows 하위 시스템이 새 아키텍처 모델로 업데이트되었으며 이제 Windows에서 실제 Linux 커널을 실행합니다. [WSL2](/windows/wsl/wsl2-about)에 대해 자세히 알아봅니다. </br> \*  **MSIX:** Windows 내의 새로운 기능은 포함된 서비스를 사용하여 패키지를 만드는 기능, 호스트되는 앱을 만드는 기능 및 패키지되지 않은 앱에서 패키지 ID가 필요한 기능을 비롯한 최신 MSIX 앱 패키징 형식을 추가로 지원합니다. [MSIX 문서](/windows/msix/overview)에서 자세히 알아봅니다. </br> 이러한 사항과 Windows의 이 릴리스에 추가된 다른 많은 기능에 대한 자세한 내용은 [개발자 센터](https://developer.microsoft.com/windows/windows-10-for-developers) 또는 [개발자를 위한 Windows 10의 새로운 기능](../whats-new/windows-10-build-19041.md)에 있는 세부 정보 페이지를 방문하세요.
| 빌드 18362(버전 1903) | Windows 10의 이 버전은 2019년 4월에 릴리스되었습니다. 이 릴리스의 몇 가지 주요 기능은 다음과 같습니다. </br> \* **XAML 아일랜드:** 이제 Windows 10을 사용하면 비 UWP 데스크톱 애플리케이션에서 UWP 컨트롤을 사용할 수 있습니다. WPF, Windows Forms 또는 C++ Win32용 프로그램을 개발하는 경우 [기존 앱에 최신 Windows 10 UI 기능을 추가하는 방법을 확인](/windows/apps/desktop/modernize/xaml-islands)하세요. </br> \* **Linux용 Windows 하위 시스템:** 이제 Windows 내에서 직접 Linux 파일에 액세스하고 몇 가지 새로운 명령줄 옵션을 사용할 수 있습니다. [WSL 정보](/windows/wsl/about.md)에서 최신 내용을 참조하세요. </br> 이 Windows 릴리스에 추가된 이러한 기능 및 기타 여러 기능에 대한 자세한 내용은 [빌드 18362의 새로운 기능](../whats-new/windows-10-build-18362.md)을 참조하세요.
| 빌드 17763(버전 1809) | Windows 10의 이 버전은 2018년 10월에 릴리스되었습니다. **이 Windows 버전을 대상으로 지정하려면 _반드시_ Visual Studio 2017 또는 Visual Studio 2019를 사용해야 합니다.** 이 릴리스의 몇 가지 주요 기능은 다음과 같습니다. </br> \* **Windows Machine Learning:** Windows Machine Learning은 공식적으로 출시되었으며 최첨단 기계 학습 모델에 대한 보다 빠른 평가 및 지원 등의 기능을 제공합니다. 플랫폼에 대한 자세한 내용은 [Windows Machine Learning](/windows/ai/)을 참조하세요. </br> \* **Fluent Design:** 메뉴 모음, 명령 모음 플라이아웃 및 XAML 속성 애니메이션과 같은 새로운 기능이 Windows 10에 추가되었습니다. [Fluent 디자인 개요](/windows/apps/fluent-design-system)에서 최신 정보를 참조하세요. </br> 이 Windows 릴리스에 추가된 이러한 기능 및 기타 여러 기능에 대한 자세한 내용은 [빌드 17763의 새로운 기능](../whats-new/windows-10-build-17763.md)을 참조하세요.
| 빌드 17134(버전 1803) | Windows 10의 이 버전은 2018년 4월에 릴리스되었습니다. **이 Windows 버전을 대상으로 지정하려면 _반드시_ Visual Studio 2017 또는 Visual Studio 2019를 사용해야 합니다.** 이 릴리스의 몇 가지 주요 기능은 다음과 같습니다. </br> \* **Fluent Design:** 트리 보기, 당겨서 새로 고침 및 탐색 보기와 같은 새로운 기능이 Windows 10에 추가되었습니다. [Fluent 디자인 개요](/windows/apps/fluent-design-system)에서 최신 정보를 참조하세요. </br> \* **콘솔 UWP 앱:** 이제 DOS 또는 PowerShell 콘솔 창 등의 콘솔 창에서 실행되는 C++ /WinRT 또는 /CX UWP 콘솔 앱을 작성할 수 있습니다. </br> 이 Windows 릴리스에 추가된 이러한 기능 및 기타 여러 기능에 대한 자세한 내용은 [빌드 17134의 새로운 기능](../whats-new/windows-10-build-17134.md)을 참조하세요.
| 빌드 16299(Fall Creators Update, 버전 1709) | 이 Windows 10 버전은 2017년 10월에 출시되었습니다. **이 Windows 버전을 대상으로 지정하려면 _반드시_ Visual Studio 2017 또는 Visual Studio 2019를 사용해야 합니다.** 이 릴리스의 몇 가지 주요 기능은 다음과 같습니다. </br> \* **.NET Standard 2.0:** .NET API 수를 크게 늘리고 즐겨 찾는 NuGet 패키지 및 타사 라이브러리를 .NET 표준에 통합할 수 있습니다. [여기](/dotnet/standard/net-standard)에서 세부 정보를 확인하고 설명서를 살펴보세요. 이러한 새로운 API에 액세스하려면 **최소 버전**을 빌드 16299으로 설정해야 합니다. </br> \* **Fluent Design:** 조명, 깊이, 관점 및 동작을 사용하여 앱을 향상시키고 사용자가 중요한 UI 요소에 초점을 맞출 수 있도록 도와줍니다. </br> \* **조건부 XAML:** 런타임 시 API의 존재 여부에 따라 속성을 손쉽게 설정하고 개체를 인스턴스화하여 디바이스 및 버전에서 앱이 원활하게 실행될 수 있도록 합니다. </br> 이 Windows 릴리스에 추가된 이러한 기능 및 기타 여러 기능에 대한 자세한 내용은 [개발자를 위한 Windows 10의 새로운 기능](../whats-new/windows-10-build-16299.md)을 참조하세요.
| 빌드 15063(크리에이터스 업데이트, 버전 1703) | 이 Windows 10 버전은 2017년 3월에 출시되었습니다. **이 Windows 버전을 대상으로 지정하려면 _반드시_ Visual Studio 2017 또는 Visual Studio 2019를 사용해야 합니다.** 이 릴리스의 몇 가지 주요 기능은 다음과 같습니다.  </br> \* **Ink Analysis:** 이제 Windows Ink는 잉크 스트로크를 쓰기 또는 그리기 스트로크로 분류하고 텍스트, 모양 및 기본 레이아웃 구조를 인식할 수 있습니다. </br> \* **Windows.Ui.Composition API:** 앱에서 애니메이션을 쉽게 결합하고 적용합니다. </br> \* **라이브 편집:** 앱이 실행 중인 XAML을 편집하고 변경 내용이 적용되는 것을 실시간으로 볼 수 있습니다. </br> 이 Windows 릴리스에 추가된 이러한 기능 및 기타 여러 기능에 대한 자세한 내용은 [빌드 15063의 새로운 기능](../whats-new/windows-10-build-15063.md)을 참조하세요.  |
| 빌드 14393(1주년 업데이트, 버전 1607) | 이 Windows 10 버전은 2016년 7월에 출시되었습니다. 이 릴리스의 몇 가지 주요 기능은 다음과 같습니다. </br> \* **Windows Ink:** 새 InkCanvas 및 InkToolbar 컨트롤입니다. </br> \* **Cortana API:** 새 Cortana 작업을 사용하여 Cortana 지원과 앱의 특정 기능을 통합합니다. </br> \* **Windows Hello:** 이제 Microsoft Edge에서 Windows Hello를 지원하여 웹 개발자가 생체 인식 인증에 액세스할 수 있습니다. </br> 이 Windows 릴리스에 추가된 이러한 기능 및 기타 여러 기능에 대한 자세한 내용은 [빌드 14393의 새로운 기능](../whats-new/windows-10-build-14393.md)을 참조하세요.  |
| 빌드 10586(11월 업데이트, 버전 1511) | Windows 10의 이 버전은 2015년 11월에 릴리스되었습니다. 주요 기능으로 Microsoft Edge의 비디오 통신을 위한 ORTC(개체 실시간 통신) API와 앱에서 Windows Hello 얼굴 인증을 사용할 수 있도록 해주는 공급자 API가 있습니다. [이 빌드에 도입된 기능에 대한 자세한 내용입니다.](../whats-new/windows-10-build-10586.md) |
| 빌드 10240(Windows 10, 버전 1507) | 이 버전은 2015년 7월의 Windows 10 초기 릴리스 버전입니다. [이 빌드에 도입된 기능에 대한 자세한 내용입니다.](../whats-new/windows-10-build-10240.md) |

새 개발자와 모든 연령 시청가용 코드를 작성하는 개발자는 항상 Windows의 최신 빌드(19041)를 사용하는 것이 좋습니다. 엔터프라이즈 앱을 작성하는 개발자는 이전 **최소 버전**을 지원하는 것을 고려해야 합니다.

## <a name="whats-different-in-each-uwp-version"></a>각 UWP 버전의 차이점은 무엇인가요?

UWP의 새 API 및 변경된 API는 Windows 10의 모든 후속 버전에서 사용할 수 있습니다. 버전에 추가된 기능에 대한 특정 정보는 [Windows 10 개발자를 위한 새로운 기능](../whats-new/windows-10-build-19041.md)을 참조하세요.

모든 디바이스 패밀리 및 해당 버전과 모든 API 계약 및 해당 버전을 열거하는 참조 항목은 [디바이스 패밀리](/uwp/extension-sdks/) 및 [API 계약](/uwp/extension-sdks/)을 참조하세요.

## <a name="net-api-availability-in-uwp-versions"></a>UWP 버전의 .NET API 가용성

UWP는 프로젝트의 **대상 버전** 또는 **최소 버전**에 관계없이 사용할 수 있는 제한된 .NET API 하위 세트를 지원합니다. [이 페이지에서는 사용할 수 있는 형식에 자세한 내용을 제공합니다](/dotnet/api/index?view=dotnet-uwp-10.0).

다시 사용할 수 있는 플랫폼 간 라이브러리를 만들려는 경우 UWP에서 .NET Standard가 지원됩니다. [.NET Standard 설명서](/dotnet/standard/net-standard)는 어떤 .NET Standard가 어떤 UWP 버전에서 지원되는지에 대한 정보를 제공합니다.

데스크톱 앱을 개발하는 경우 대신, [.NET Framework 버전 및 종속성](/dotnet/framework/migration-guide/versions-and-dependencies)에서 .NET Framework 가용성에 대한 자세한 내용을 참조하세요.

## <a name="choose-which-version-to-use-for-your-app"></a>앱에 사용할 버전 선택

Visual Studio의 **새 유니버설 Windows 프로젝트** 대화 상자에서 **대상 버전** 및 **최소 버전**용 버전을 선택할 수 있습니다. 또한 앱 **속성**의 **애플리케이션** 섹션에서 UWP 앱의 *대상 버전* 및 **최소 버전**을 변경할 수 있습니다.

* **대상 버전**. 앱이 실행되는 Windows 10 버전입니다. 프로젝트 파일에서 *TargetPlatformVersion* 설정을 설정합니다. 또한 앱 패키지 매니페스트에서 *TargetDeviceFamily@MaxVersionTested* 특성의 값을 결정합니다. 선택하는 값은 프로젝트가 대상으로 하는 UWP 플랫폼의 버전을 지정하므로(따라서 앱에 사용할 수 있는 API 집합) 가능한 한 최신 버전을 선택하는 것이 좋습니다. 앱 패키지 매니페스트에 대한 자세한 내용과 TargetDeviceFamily를 수동으로 구성하는 데 대한 몇 가지 지침은 [TargetDeviceFamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)를 참조하세요.
* **최소 버전**. 가장 초기 버전의 Windows 10은 앱의 기본 기능을 지원하는 데 필요합니다. 프로젝트 파일에서 *TargetPlatformMinVersion* 설정을 설정합니다. 또한 앱 패키지 매니페스트에서 *TargetDeviceFamily@MinVersion* 특성의 값을 결정합니다. 선택하는 값은 프로젝트가 작동할 수 있는 UWP 플랫폼의 최소 버전을 지정합니다.

앱이 **최소 버전**에서 **대상 버전**까지의 범위에 있는 모든 버전의 Windows에서 작동함을 선언하는 것입니다. 두 가지가 동일한 버전인 경우 특별한 작업을 수행할 필요가 없습니다. 다른 경우에는 다음과 같이 몇 가지 사항을 주의해야 합니다.

* 코드에서 자유롭게,즉 조건부 확인 없이 **최소 버전**에 지정된 버전에 있는 모든 API를 호출할 수 있습니다.
* **대상 버전**에만 있는 API 없이도 코드가 작동하는지 확인하기 위해 **최소 버전**을 실행하는 디바이스에서 코드를 테스트해야 합니다.
* **대상 버전** 값은 프로젝트를 컴파일하는 데 사용되는 모든 참조(계약 winmds)를 식별하기 위해 사용합니다. 하지만 해당 참조를 사용하면 **최소 버전**을 통해 지원한다고 선언한 디바이스에 반드시 존재하지 않을 수도 있는 API에 대한 호출로 코드를 컴파일할 수 있습니다. 따라서 **최소 버전** 후 도입된 모든 API는 적응 코드를 통해 호출해야 합니다. 적응 코드에 대한 자세한 내용은 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 참조하세요.