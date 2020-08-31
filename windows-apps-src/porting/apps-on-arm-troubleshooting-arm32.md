---
title: ARM32 UWP 앱 문제 해결
description: ARM에서 실행할 때 ARM32 apps와 관련 된 일반적인 문제 및 해결 방법
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, 항상 연결 됨, ARM의 ARM32 apps, ARM의 windows 10, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: 60c7a129844d69d18287ea7885a0acaf01f1f625
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171227"
---
# <a name="troubleshooting-arm-uwp-apps"></a>ARM UWP 앱 문제 해결

ARM32 또는 ARM64 UWP 앱이 ARM에서 제대로 작동 하지 않는 경우 도움이 될 수 있는 몇 가지 지침은 다음과 같습니다.

>[!NOTE]
> ARM64 플랫폼을 기본적으로 대상으로 하는 UWP 응용 프로그램을 빌드하려면 Visual Studio 2017 버전 15.9 이상 또는 Visual Studio 2019이 있어야 합니다. 자세한 내용은 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)을 참조하세요.


## <a name="common-issues"></a>일반적인 문제
ARM32 및 ARM64 앱 문제를 해결할 때 유의할 몇 가지 일반적인 문제는 다음과 같습니다.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>ARM 기반 프로세서에서 Windows 10 Mobile 전용 Api 사용
모바일 전용 Api (예: **HardwareButtons**)를 사용 하는 경우 ARM 앱이 문제가 발생할 수 있습니다. 이를 완화 하기 위해 이러한 Api를 호출 하기 전에 Windows 10 Mobile에서 앱이 실행 되 고 있는지 여부를 동적으로 검색할 수 있습니다. 블로그 게시물의 지침에 따라 동적으로 [API 계약으로 기능을 검색](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)합니다.

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>UWP 앱에서 지원 되지 않는 종속성 포함
Visual Studio 및 UWP SDK를 사용 하 여 제대로 빌드되지 않은 UWP (유니버설 Windows 플랫폼) 앱은 ARM64 시스템에서 실행 되는 ARM 앱에서 사용할 수 없는 OS 구성 요소에 대 한 종속성이 있을 수 있습니다. 이러한 종속성의 예는 다음과 같습니다.

- .NET Framework의 일부를 사용할 수 있어야 합니다.
- UWP와 호환 되지 않는 타사 .NET 구성 요소를 참조 합니다.

이러한 문제는 사용 불가능 한 종속성을 제거 하 고 최신 Microsoft Visual Studio 및 UWP SDK 버전을 사용 하 여 앱을 다시 작성 하 여 해결할 수 있습니다. 또는 Microsoft Store에서 ARM 앱을 제거 하 여 앱의 x86 버전 (사용 가능한 경우)이 사용자의 Pc에 다운로드 되도록 하는 마지막 수단입니다.

UWP 앱에 사용할 수 있는 .NET Api에 대 한 자세한 내용은 [uwp 앱 용 .net](/dotnet/api/index?view=dotnet-uwp-10.0) 을 참조 하세요.

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>이전 버전의 Visual Studio 및 SDK를 사용 하 여 응용 프로그램 컴파일
문제가 발생 하는 경우 최신 버전의 Microsoft Visual Studio 및 Windows SDK를 사용 하 여 앱을 컴파일해야 합니다. 이전 버전의 Visual Studio 및 SDK를 사용 하 여 컴파일된 앱에는 이후 버전에서 수정 된 문제가 있을 수 있습니다.

## <a name="debugging"></a>디버깅
ARM 플랫폼용 앱을 개발 하는 데 기존 도구를 사용할 수 있습니다. 다음은 몇 가지 유용한 리소스입니다.

- Visual Studio 15.5 Preview 1 이상은 유니버설 인증 모드를 사용 하 여 ARM32 apps 실행을 지원 합니다. 그러면 필요한 원격 디버깅 도구가 자동으로 부트스트랩 됩니다.
- ARM에서 디버깅 하기 위한 도구 및 전략에 대해 자세히 알아보려면 [ARM64에서 디버깅](/windows-hardware/drivers/debugger/debugging-arm64) 을 참조 하세요.