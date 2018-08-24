---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: 네이티브 이미지와.NET 데스크톱 앱을 최적화 합니다.
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, 네이티브 이미지 컴파일러
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2839095"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>네이티브 이미지와.NET 데스크톱 앱을 최적화 합니다.

> [!NOTE]
> 일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

사전에 바이너리 컴파일하는 중 하 여.NET Framework 응용 프로그램의 시작 시간을 향상 시킬 수 있습니다. 이 기술 패키지 및 Windows 스토어를 통해 배포 하는 대규모 응용 프로그램에서 사용할 수 있습니다. 경우에 따라 20%의 성능 개선을 관찰 했습니다. [기술 개요 (영문)이](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)이 기술에 대 한 자세한 수 있습니다.

[NuGet 패키지](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)으로 미리 보기 버전의 네이티브 이미지 컴파일러가 발표 했을 때 되었습니다. 이 패키지는.NET Framework 버전 4.6.2 대상으로 하는 모든.NET Framework 응용 프로그램에 적용할 수 이상입니다. 이 패키지를 응용 프로그램에서 사용 하는 모든 이진 파일을 기본 페이로드 포함 된 게시물 빌드 단계를 추가 합니다. 이전 버전은 MSIL 코드의 부하 계속 하는 동안.net 4.7.2 및 위에 응용 프로그램이 실행 하는 경우이 최적화 된 페이로드 로드 됩니다.

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) [10 년 4 월 2018 Windows 업데이트](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)에 포함 됩니다. Pc의 Windows 7 + 및 Windows Server 2008 r 2 +를 실행 하는이 버전의.NET Framework를 설치할 수도 있습니다.

> [!IMPORTANT]
> Windows 응용 프로그램 패키지 프로젝트별 패키지 된 응용 프로그램에 대 한 기본 이미지를 생성 하려면 Windows 기념일 업데이트에는 프로젝트의 대상 플랫폼 최소 버전으로 설정 해야 합니다.

## <a name="how-to-produce-native-images"></a>네이티브 이미지를 생성 하는 방법

프로젝트를 구성 하려면 다음이 지침을 따릅니다.

1. 4.6.2 또는 위에 대상 프레임 워크를 구성 합니다.

2. 대상 플랫폼 x86 또는 x64로 구성 

3. NuGet 패키지를 추가 합니다.

4. 릴리스 빌드를 만듭니다.

## <a name="configure-the-target-framework-as-462-or-above"></a>4.6.2 또는 위에 대상 프레임 워크를 구성 합니다.

프로젝트 대상.NET Framework 4.6.2 구성 하는.NET Framework 4.6.2 개발 도구 갖추어야 이상입니다. 이러한 도구를.NET 데스크톱 개발 작업 아래에서 선택적 구성 요소와 Visual Studio 설치 관리자를 통해 사용할 수 있습니다.

![.NET 4.6.2 설치 개발 도구](images/desktop-to-uwp/install-4.6.2-devpack.png)

또는에서.NET 개발자 팩을 얻을 수 있습니다.[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>대상 플랫폼 x86 또는 x64로 구성

네이티브 이미지 컴파일러는 지정된 된 플랫폼에 대 한 코드를 최적화합니다. 이 기능을 사용 하려면 예: x86 또는 x64 하나의 특정 플랫폼 대상 응용 프로그램을 구성 해야 합니다.

솔루션에서 여러 프로젝트를 포함 하는 경우 x86 또는 x64로 컴파일할 수를 항목 지점 프로젝트에만 (실행 파일을 생성 하는 프로젝트 대부분의 경우)에 있습니다. 주 프로젝트에서 참조 되는 추가 바이너리 AnyCPU로 컴파일되 하는 경우에 주 프로젝트에 지정 된 아키텍처를 통해 처리 됩니다.

프로젝트를 구성 합니다.

1. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **구성 관리자**를 선택 합니다.

2. **선택 < 새로 만들기... >** 실행 파일을 생성 하는 프로젝트의 이름 옆에 있는 **플랫폼** 드롭다운 메뉴에서 합니다.

3. **새 프로젝트 플랫폼** 대화 상자에서 **에서 복사 설정** 드롭다운 목록 **Any CPU**로 설정 되어있는지 확인 합니다.

![X86 구성](images/desktop-to-uwp/configure-x86.png)

에 대해이 단계를 반복 `Release/x64` x64를 생성 하려면 이진 파일입니다.

>[!IMPORTANT]
> 네이티브 이미지 컴파일러에 의해 AnyCPU 구성은 지원 되지 않습니다.

## <a name="add-the-nuget-packages"></a>NuGet 패키지를 추가 합니다.

네이티브 이미지 컴파일러는 실행 파일을 생성 하는 Visual Studio 프로젝트에 추가 하는 NuGet 패키지도 제공 됩니다. 이 일반적으로 Windows Forms 또는 WPF 프로젝트입니다. 그렇게 하려면이 PowerShell 명령을 사용 합니다.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 미리 보기 패키지 목록에 없는으로 NuGet.org에 게시 됩니다. 탐색 NuGet.org 또는 Visual Studio에서 패키지 관리자 UI를 사용 하 여 찾아서 없습니다. 그러나 패키지 관리자 콘솔에서 설치할 수 및 사용 해야하는 경우 다른 컴퓨터에서 복원할 수 있습니다. 첫번째 미리 보기 아닌 버전을 게시는 패키지를 완벽 하 게 액세스할 수 있는 만들 하겠습니다 것입니다.

## <a name="create-a-release-build"></a>릴리스 빌드 만들기

NuGet 패키지 릴리스 빌드에는 추가 도구를 실행 하려면 프로젝트를 구성 합니다. 이 도구는 동일한 이진 파일에 네이티브 코드를 추가합니다.
도구 바이너리 처리 된 되었는지 확인 하려면이 하나와 같은 메시지를 포함 되도록 출력 빌드를 검토할 수 있습니다.

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>FAQ

**Q: 합니다. .NET Framework 4.7.2 없이 컴퓨터에서 새 바이너리 작동 합니까?**

A. 최적화 된 이진 파일은.NET framework 4.7.2 실행할 때의 성능은 향상 도움이 됩니다. 이전.NET framework 버전을 실행 하는 클라이언트는 이진 파일에서 최적화 되지 않은 MSIL 코드를 로드 합니다.

**Q: 합니다. 의견을 제공 하거나 문제를 보고 수 어떻게 합니까?**

A. Visual Studio 2017의 피드백 도구를 사용 하 여 문제를 보고 합니다. [자세한 내용은](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)입니다.

**Q: 합니다. 기존 이진 파일에 원시 이미지를 추가할 경우의 영향이 이란?**

A. 최적화 된 바이너리 최종 파일 큰 요청 관리와 네이티브 코드를 포함 합니다.

**Q: 합니다. 이 기술을 사용 하 여 바이너리를 해제할 수 있습니까?**

A. 이 버전에는 현재 사용할 수 있는 Live 이동 라이선스가 포함 됩니다.
