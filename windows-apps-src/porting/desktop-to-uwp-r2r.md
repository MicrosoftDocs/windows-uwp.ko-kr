---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: 네이티브 이미지를 사용 하 여 데스크톱.NET 응용 프로그램을 최적화 합니다.
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 10, windows 컴파일러 네이티브 이미지
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3931123"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>네이티브 이미지를 사용 하 여 데스크톱.NET 응용 프로그램을 최적화 합니다.

> [!NOTE]
> 일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

이진 파일을 미리 컴파일해서.NET Framework 응용 프로그램의 시작 시간을 향상 시킬 수 있습니다. 패키지 하 고 Windows 스토어를 통해 배포 된 대규모 응용 프로그램에서이 기술을 사용할 수 있습니다. 경우에 따라 20% 성능 향상을 관찰 했습니다. 이 기술은 [기술 개요](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)에 대해 자세히 알아보십시오.

컴파일러 네이티브 이미지의 미리 보기 버전이 [NuGet 패키지](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)건이 발표 했습니다. 4.6.2 버전의.NET Framework 대상으로 하는 모든.NET Framework 응용 프로그램에이 패키지를 적용할 수 있습니다 이상입니다. 이 패키지는 네이티브 응용 프로그램에서 사용 하는 모든 바이너리에 페이로드를 포함 하는 게시 빌드 단계를 추가 합니다. 이 최적화 된 페이로드 이전 버전 여전히 MSIL 코드를 로드 하는 동안.net 4.7.2 이상과 응용 프로그램이 실행 될 때 로드 됩니다.

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) [10 월 2018 Windows 업데이트](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)에 포함 되어 있습니다. 또한 PC에서 Windows 7 + 및 Windows Server 2008 r 2 +를 실행 하는이 버전의.NET Framework 설치할 수 있습니다.

> [!IMPORTANT]
> Windows 응용 프로그램 패키징 프로젝트에서 패키지 된 응용 프로그램에 대 한 네이티브 이미지를 생성 하려면, 프로젝트의 대상 플랫폼의 최소 버전 업데이트가 기념일을 설정할 수 있는지 확인 합니다.

## <a name="how-to-produce-native-images"></a>네이티브 이미지를 생성 하는 방법

프로젝트를 구성 하려면 다음이 지침을 따릅니다.

1. 4.6.2 또는 위에서 대상 프레임 워크를 구성 합니다.

2. 대상 플랫폼에서 x86 또는 x64로 구성 

3. NuGet 패키지를 추가 합니다.

4. 릴리스 빌드를 만듭니다.

## <a name="configure-the-target-framework-as-462-or-above"></a>4.6.2 또는 위에서 대상 프레임 워크를 구성 합니다.

프로젝트의 대상.NET Framework 4.6.2 구성 하려면.NET Framework 4.6.2 개발 도구 필요 합니다 이후입니다. 이러한 도구는.NET 데스크톱 개발 작업 부하에서 선택적 구성 요소로 Visual Studio 설치 관리자를 통해 사용할 수 있습니다.

![4.6.2.NET 설치 개발 도구](images/desktop-to-uwp/install-4.6.2-devpack.png)

또한.NET 개발자 팩을 얻을 수 있습니다.[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>대상 플랫폼에서 x86 또는 x64로 구성

이미지 네이티브 컴파일러는 지정된 된 플랫폼에 대 한 코드를 최적화합니다. 이 기능을 사용 하 여 x86 또는 x64 같은 특정 플랫폼을 대상 응용 프로그램을 구성 해야 합니다.

솔루션에 여러 프로젝트를 설정한 경우 x86 또는 x64로 컴파일되도록 엔트리 포인트 프로젝트만 (대부분의 경우 실행 파일을 생성 하는 프로젝트)에 있습니다. AnyCPU로 컴파일되는 경우에 기본 프로젝트에서 참조 추가 이진 주 프로젝트에 지정 된 아키텍처를 사용 하 여 처리 됩니다.

프로젝트를 구성 하는 방법:

1. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **구성 관리자**를 선택 합니다.

2. 선택 **< 새... >** 실행 파일을 생성 하는 프로젝트의 이름 옆에 **플랫폼** 드롭다운 메뉴.

3. **새 프로젝트 플랫폼** 대화 상자에서 **설정 복사** 드롭다운 목록에서 **Any CPU**로 설정 되어 있는지 확인 합니다.

![X86 구성](images/desktop-to-uwp/configure-x86.png)

에 대해이 단계를 반복 합니다. `Release/x64` x64 생성 하려면 이진 파일입니다.

>[!IMPORTANT]
> AnyCPU 구성 이미지 네이티브 컴파일러에서 지원 되지 않습니다.

## <a name="add-the-nuget-packages"></a>NuGet 패키지 추가

NuGet 패키지 실행 파일을 생성 하는 Visual Studio 프로젝트에 추가 해야 하는 네이티브 이미지 컴파일러가 제공 됩니다. 일반적으로 Windows Forms 또는 WPF 프로젝트입니다. PowerShell 명령을 사용 하 여.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 미리 보기 패키지는 NuGet.org에 목록으로 게시 됩니다. NuGet.org 검색 하거나 Visual Studio 패키지 관리자 UI를 사용 하 여 찾이 없습니다. 그러나 패키지 관리자 콘솔을 설치할 수 및 다른 컴퓨터에서 복원할 수 있습니다. 칠할 패키지 완벽 하 게 액세스할 수 있는 첫 번째 미리 보기 비 버전 게시 하겠습니다.

## <a name="create-a-release-build"></a>릴리스 빌드 만들기

NuGet 패키지 릴리스 빌드에는 추가 도구를 실행 하는 프로젝트를 구성 합니다. 이 도구는 네이티브 코드 같은 이진 파일을 추가합니다.
도구에서 이진 데이터를 처리를 확인 하려면 출력 메시지 등이 포함 되어 있는지 확인 하 여 빌드를 검토할 수 있습니다.

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>FAQ

**Q입니다. .NET Framework 4.7.2 없이 시스템에서 작업을 해야 새 이진?**

A. 4.7.2.NET Framework 실행 하는 경우 최적화 된 이진 파일의 성능은 향상에서 도움이 됩니다. 이전.NET framework 버전을 실행 하는 클라이언트는 이진에서 최적화 되지 않은 MSIL 코드를 로드 합니다.

**Q입니다. 피드백을 제공 하거나 문제도 보고 되지 않으면 수 어떻게 합니까?**

A. Visual Studio 2017 피드백 도구를 사용 하 여 문제를 보고 합니다. [자세한 내용](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)은.

**Q입니다. 네이티브 이미지는 기존 이진 파일을 추가 하는 영향은 무엇입니까?**

A. 큰 최종 파일을 만들기, 관리 및 네이티브 코드를 포함 하는 최적화 된 이진 파일.

**Q입니다. 이 기술을 사용 하 여 이진 파일을 해제할 수 있습니까?**

A. 이 버전에는 현재 사용할 수 있는 라이브 이동 라이센스를 포함 됩니다.
