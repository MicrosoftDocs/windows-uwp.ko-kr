---
title: ARM32 UWP 앱 문제 해결
author: msatranjr
description: ARM을 기반으로 실행되는 경우 ARM32 앱 관련 일반적인 문제 및 이를 해결하는 방법.
ms.author: misatran
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 s, 항상 연결, ARM 기반 ARM32 앱, ARM 기반 Windows 10, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: 71d92ec26311514e0eebdfa4a1dab39e86ce72fc
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595172"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>ARM32 UWP 앱 문제 해결
ARM32 UWP 앱이 ARM에서 제대로 작동하지 않는 경우 도움이 될 수 있는 몇 가지 지침은 다음과 같습니다. 

## <a name="common-issues"></a>일반적인 문제
다음은 ARM32 앱 문제를 해결할 때 주의해야 할 몇 가지 일반적인 문제입니다.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>ARM 기반 프로세서에서 Windows 10 Mobile 전용 API 사용 
모바일 전용 API(예: **HardwareButtons**)를 사용하는 경우 ARM32 앱에 문제가 발생할 수 있습니다. 이러한 문제를 해결하려면 이러한 API를 호출하기 전에 앱이 Windows 10 Mobile에서 실행되고 있는지 동적으로 검색할 수 있습니다. [API 계약을 사용하여 동적으로 기능 감지](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) 블로그 게시물의 지침에 따르세요.

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>UWP 앱에서 지원하지 않는 종속성 포함
Visual Studio 및 UWP SDK를 사용하여 제대로 빌드되지 않은 UWP(유니버설 Windows 플랫폼) 앱은 ARM64 시스템에서 실행되는 ARM32 앱에서 사용할 수 없는 OS 구성 요소에 종속될 수 있습니다. 이러한 종속성의 예는 다음과 같습니다.

- 사용할 수 있는 것으로 기대되는 .NET Framework 부분.
- UWP와 호환되지 않는 제3자 .NET 구성 요소 참조.

이러한 문제는 사용할 수 없는 종속성을 제거하고 최신 Microsoft Visual Studio 및 UWP SDK 버전을 사용하여 앱을 다시 작성하거나, 최후의 수단으로 Microsoft Store에서 ARM32 앱을 제거하여, 앱의 x86 버전(사용 가능한 경우)을 사용자의 PC에 다운로드하여 해결할 수 있습니다. 

UWP 앱에 사용할 수 있는 .NET API에 대한 자세한 내용은 [UWP 앱용 .NET](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)을 참조하세요.

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>이전 버전의 Visual Studio 및 SDK로 앱 컴파일
문제가 발생한 경우 앱을 컴파일하는 데 최신 버전의 Microsoft Visual Studio 및 Windows SDK를 사용하도록 해야 합니다. 이전 버전의 Visual Studio및 SDK를 사용하여 컴파일한 앱에는 이후 버전에서 수정된 문제가 있을 수 있습니다.

## <a name="debugging"></a>디버깅
ARM 플랫폼용 ARM32 앱 개발을 위해 기존 도구를 사용할 수 있습니다. 다음은 몇 가지 유용한 리소스입니다.

- Visual Studio 15.5 미리 보기 1 이상은 유니버설 인증 모드를 사용한 ARM32 앱 실행을 지원합니다. 이를 통해 필수 원격 디버깅 도구를 자동으로 부트스트랩합니다.
- ARM 기반 디버깅을 위한 도구 및 전략에 대한 자세한 내용은 [ARM64 기반 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)을 참조하세요.