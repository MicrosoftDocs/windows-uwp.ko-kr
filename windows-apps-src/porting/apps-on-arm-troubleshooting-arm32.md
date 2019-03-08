---
title: ARM32 UWP 앱 문제 해결
description: ARM을 기반으로 실행되는 경우 ARM32 앱 관련 일반적인 문제 및 이를 해결하는 방법.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, 항상 연결, ARM 기반 ARM32 앱, ARM 기반 Windows 10, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: 75019df4b7d70dad20aea1a256abac05c93a481d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661548"
---
# <a name="troubleshooting-arm-uwp-apps"></a>UWP 앱이 ARM 문제 해결

ARM에서 ARM32 또는 ARM64 UWP 앱 올바르게 작동 하지 않는 경우 도움이 될 수 있는 몇 가지 지침은 다음과 같습니다.

>[!NOTE]
> 고유 하 게 ARM64 플랫폼을 대상으로 UWP 응용 프로그램을 빌드하려면 Visual Studio 2017 버전 15.9 이상이 있어야 합니다. 자세한 내용은 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)합니다.

## <a name="common-issues"></a>일반적인 문제
ARM64 및 ARM32 앱 문제를 해결할 때 염두에 몇 가지 일반적인 문제는 다음과 같습니다.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>ARM 기반 프로세서에서 Windows 10 Mobile 전용 API 사용
모바일 전용 Api를 사용 하 여 ARM 앱 문제가 발생할 수 있습니다 (예를 들어 **HardwareButtons**). 이러한 문제를 해결하려면 이러한 API를 호출하기 전에 앱이 Windows 10 Mobile에서 실행되고 있는지 동적으로 검색할 수 있습니다. [API 계약을 사용하여 동적으로 기능 감지](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) 블로그 게시물의 지침에 따르세요.

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>UWP 앱에서 지원하지 않는 종속성 포함
UWP (유니버설 Windows 플랫폼) 앱에서 Visual Studio 및 UWP SDK를 사용 하 여 제대로 제공 되지 않는 ARM64 시스템에서 실행 되는 ARM 앱 제공 되지 않는 OS 구성 요소에 종속 될 수 있습니다. 이러한 종속성의 예는 다음과 같습니다.

- 사용할 수 있는 것으로 기대되는 .NET Framework 부분.
- UWP와 호환되지 않는 제3자 .NET 구성 요소 참조.

이러한 문제를 해결할 수 있습니다: 사용할 수 없는 종속성을 제거 하 고 최신 Microsoft Visual Studio 및 UWP SDK 버전을 사용 하 여 앱을 다시 작성 또는 Microsoft Store ARM 앱 제거, 최후의 수단으로 있도록 x86 버전의 앱 (있는 경우) 사용자의 Pc에 다운로드 됩니다.

UWP 앱에 사용할 수 있는 .NET API에 대한 자세한 내용은 [UWP 앱용 .NET](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)을 참조하세요.

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>이전 버전의 Visual Studio 및 SDK로 앱 컴파일
문제가 발생한 경우 앱을 컴파일하는 데 최신 버전의 Microsoft Visual Studio 및 Windows SDK를 사용하도록 해야 합니다. 이전 버전의 Visual Studio및 SDK를 사용하여 컴파일한 앱에는 이후 버전에서 수정된 문제가 있을 수 있습니다.

## <a name="debugging"></a>디버깅
ARM 플랫폼에 대 한 앱 개발을 위한 기존 도구를 사용할 수 있습니다. 다음은 몇 가지 유용한 리소스입니다.

- Visual Studio 15.5 미리 보기 1 이상은 유니버설 인증 모드를 사용한 ARM32 앱 실행을 지원합니다. 이를 통해 필수 원격 디버깅 도구를 자동으로 부트스트랩합니다.
- ARM 기반 디버깅을 위한 도구 및 전략에 대한 자세한 내용은 [ARM64 기반 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)을 참조하세요.
