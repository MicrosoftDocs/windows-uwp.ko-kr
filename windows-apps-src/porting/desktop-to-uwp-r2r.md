---
Description: 이 가이드에서는 네이티브 이미지를 사용 하 여 응용 프로그램 이진 파일을 최적화 하기 위해 Visual Studio 솔루션을 구성 하는 방법을 설명 합니다.
Search.Product: eADQiWindows 10XVcnh
title: 네이티브 이미지를 사용 하 여.NET 데스크톱 앱을 최적화할 수
ms.date: 06/11/2018
ms.topic: article
keywords: windows 10, 네이티브 이미지 컴파일러
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642878"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>네이티브 이미지를 사용 하 여.NET 데스크톱 앱을 최적화할 수

> [!NOTE]
> 일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

이진 파일을 미리 컴파일하여.NET Framework 응용 프로그램의 시작 시간을 향상할 수 있습니다. 패키지 및 Microsoft Store 통해 배포 하는 큰 응용 프로그램에서이 기술을 사용할 수 있습니다. 일부 경우에 20% 성능 향상을 관찰해 왔습니다. 이 기술에 대해 자세히 알아볼 수 있습니다 합니다 [기술 개요](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)합니다.

네이티브 이미지 컴파일러의 미리 보기 버전 출시를 [NuGet 패키지](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)합니다. .NET Framework 버전 4.6.2 대상으로 하는 모든.NET Framework 응용 프로그램에이 패키지를 적용할 수 있습니다 이상. 이 패키지는 응용 프로그램에서 사용 하는 모든 이진 파일에는 네이티브 페이로드를 포함 하는 사후 빌드 단계를 추가 합니다. 이전 버전은 MSIL 코드를 여전히 로드 하는 동안 응용 프로그램이.net 4.7.2 이상을 실행 하는 경우이 최적화 된 페이로드를 로드 됩니다.

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) 에 포함 되는 [Windows 10 2018 년 4 월 업데이트](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)합니다. 또한 PC에 Windows 7 이상 및 Windows Server 2008 R2 이상 실행 하는이 버전의.NET Framework를 설치할 수 있습니다.

> [!IMPORTANT]
> Windows 응용 프로그램 패키징 프로젝트에서 패키지 응용 프로그램에 대 한 네이티브 이미지를 생성 하려는 경우에 프로젝트의 대상 플랫폼 최소 버전은 Windows 1 주년 업데이트를 설정 해야 합니다.

## <a name="how-to-produce-native-images"></a>네이티브 이미지를 생성 하는 방법

프로젝트를 구성 하려면 다음이 지침을 따릅니다.

1. 4.6.2 또는 위의 대상 프레임 워크 구성

2. X86 또는 x64 대상 플랫폼 구성 

3. NuGet 패키지를 추가 합니다.

4. 릴리스 빌드를 만듭니다.

## <a name="configure-the-target-framework-as-462-or-above"></a>4.6.2 또는 위의 대상 프레임 워크 구성

.NET Framework 4.6.2 대상으로 하도록 프로젝트를 구성 해야.NET Framework 4.6.2 개발 도구 이상입니다. 이러한 도구는.NET 데스크톱 개발 워크 로드에서 선택적 구성 요소로 Visual Studio 설치 관리자를 통해 사용할 수 있습니다.

![설치할.NET 4.6.2 개발 도구](images/desktop-to-uwp/install-4.6.2-devpack.png)

또는에서.NET 개발자 팩을 가져올 수 있습니다. [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>X86 또는 x64 대상 플랫폼 구성

네이티브 이미지 컴파일러는 지정된 된 플랫폼에 대 한 코드를 최적화합니다. 를 사용 하려면 x86 또는 x64 같은 하나의 특정 플랫폼을 대상으로 응용 프로그램을 구성 해야 합니다.

솔루션에 여러 프로젝트가 있는 경우 항목 지점 프로젝트만 (대개 실행 파일을 생성 하는 프로젝트) x86 또는 x64로 컴파일되도록 해야 합니다. 기본 프로젝트에서 참조 하는 추가 바이너리는 AnyCPU로 컴파일되는 경우에 기본 프로젝트의 아키텍처를 사용 하 여 처리 됩니다.

프로젝트를 구성 합니다.

1. 솔루션을 마우스 오른쪽 단추로 클릭 한 다음 선택 **Configuration Manager**합니다.

2. 선택 **< 새로 만들기... >** 에 **플랫폼** 실행 파일을 생성 하는 프로젝트의 이름 옆에 있는 드롭다운 메뉴입니다.

3. 에 **새 프로젝트 플랫폼** 대화 상자에서 확인 합니다 **에서 설정 복사** 드롭다운 목록이로 설정 되어 **Any CPU**.

![X86 구성](images/desktop-to-uwp/configure-x86.png)

이 단계를 반복 `Release/x64` x64를 생성 하려는 경우 이진 파일.

>[!IMPORTANT]
> 네이티브 이미지 컴파일러로 AnyCPU 구성은 지원 되지 않습니다.

## <a name="add-the-nuget-packages"></a>NuGet 패키지 추가

네이티브 이미지 컴파일러는 실행 파일을 생성 하는 Visual Studio 프로젝트에 추가 해야 하는 NuGet 패키지로 제공 됩니다. 이것이 일반적으로 Windows Forms 또는 WPF 프로젝트입니다. 이 PowerShell 명령을 사용 하 여 작업을 수행 합니다.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 미리 보기 패키지 목록에 없는으로 NuGet.org에 게시 됩니다. 검색 NuGet.org 또는 Visual Studio에서 패키지 관리자 UI를 사용 하 여 찾이 없습니다. 하지만 패키지 관리자 콘솔에서이 설치 하는 시점과 다른 컴퓨터에서 복원할 수 있습니다. 첫 번째 비-preview 버전을 게시할 때 패키지를 완벽 하 게 액세스할 수 있도록 하겠습니다 했습니다.

## <a name="create-a-release-build"></a>릴리스 빌드 만들기

NuGet 패키지를 릴리스 빌드에 대 한 추가 도구를 실행 하도록 프로젝트를 구성 합니다. 이 도구는 동일한 이진 파일에 네이티브 코드를 추가합니다.
도구에서 이진 파일을 처리 했는지 확인 하려면 빌드 이와 같은 메시지를 포함 하는지 확인 하려면 출력을 검토할 수 있습니다.

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>FAQ

**질문 및 답변 합니다. 새 이진 파일이.NET Framework 4.7.2에서 작동 하나요?**

A. 최적화 된 이진 파일은.NET framework 4.7.2 이상 실행 하는 경우 향상 된 기능에서 도움이 됩니다. 이전.NET framework 버전을 실행 하는 클라이언트 이진 파일에서 최적화 되지 않은 MSIL 코드를 로드 합니다.

**질문 및 답변 합니다. 피드백을 제공 하거나 문제를 보고 수 어떻게 하나요?**

A. Visual Studio 2017에서 피드백 도구를 사용 하 여 문제를 보고 합니다. [자세한 내용은](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)합니다.

**질문 및 답변 합니다. 기존 이진 파일에 네이티브 이미지를 추가 하는 영향을 무엇입니까?**

A. 최종 파일이 큰 만들기, 관리 및 네이티브 코드를 포함 하는 최적화 된 이진 파일.

**질문 및 답변 합니다. 이 기술을 사용 하 여 이진 파일을 해제할 수 있나요?**

A. 이 버전에는 현재 사용할 수 있는 Go Live 라이선스를 포함 합니다.
