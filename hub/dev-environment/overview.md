---
title: Windows 10에서 개발 환경 설정
description: Windows에서 개발 환경을 설정하고 선호하는 도구와 코드 언어를 설치하는 데 도움이 되는 전체 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Windows 설정, 개발 환경, 개발 도구, 개발 경로, Microsoft, Windows, 개발자, 팁, 성능, WSL, 터미널, nodejs, python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 58c68522fdb3ffe11c4e11dff48fb49faa79b830
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824037"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>Windows 10에서 개발 환경 설정

이 가이드는 Windows 또는 Linux용 Windows 하위 시스템에서 개발할 때 필요한 언어 및 도구를 설치하고 설정하는 방법을 안내합니다.

## <a name="development-paths"></a>개발 경로

:::row:::
    :::column:::
       [![JavaScrip NodeJS 아이콘](../images/nodejs-logo.png)](../nodejs/index.yml)<br>
        **[NodeJS 시작](../nodejs/index.yml)**<br>
        Windows 또는 Linux용 Windows 하위 시스템에서 NodeJS를 설치하고 개발 환경을 설정합니다.
    :::column-end:::
    :::column:::
       [![Python 아이콘](../images/python-logo.png)](../python/index.yml)<br>
        **[Python 시작](../python/index.yml)**<br>
        Windows 또는 Linux용 Windows 하위 시스템에서 Python을 설치하고 개발 환경을 설정합니다.
    :::column-end:::
    :::column:::
       [![Android 아이콘](../images/android-logo.png)](/windows/android)<br>
        **[Android 시작](/windows/android)**<br>
        Android Studio를 설치하거나 Xamarin, React 또는 Cordova 같은 플랫폼 간 솔루션을 선택하고 Windows에서 개발 환경을 설정합니다.
    :::column-end:::
    :::column:::
       [![Windows 데스크톱 아이콘](../images/windows-logo.png)](../apps/index.yml)<br>
        **[Windows 데스크톱 시작](../apps/index.yml)**<br>
        UWP, Win32, WPF, Windows Forms를 사용하여 Windows 10용 데스크톱 앱을 빌드하거나 MSIX 및 XAML Islands를 사용하여 기존 데스크톱 앱을 업데이트하고 배포합니다.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C/C++](../images/c-logo.png)](/cpp/)<br>
        **[C++ 및 C 시작](/cpp/)**<br>
        C++, C 및 어셈블리를 시작하여 앱, 서비스 및 도구를 개발하세요.
    :::column-end:::
    :::column:::
       [![C# 아이콘](../images/csharp-logo.png)](/dotnet/csharp/)<br>
        **[C# 시작](/dotnet/csharp/)**<br>
        C# 및 .NET Core를 사용하여 앱 빌드를 시작하세요.
    :::column-end:::
    :::column:::
       [![Windows용 Docker 데스크톱 아이콘](../images/docker-logo.png)](../dev-environment/docker/overview.md)<br>
        **[Windows용 Docker 데스크톱 시작](../dev-environment/docker/overview.md)**<br>
        Visual Studio, VS Code, .NET, Linux용 Windows 하위 시스템 또는 다양한 Azure 서비스의 지원을 통해 원격 개발 컨테이너를 만듭니다.
    :::column-end:::
    :::column:::
       [![PowerShell 아이콘](../images/powershell.png)](/powershell/)<br>
        **[PowerShell 시작](/powershell/)**<br>
        명령줄 셸이자 스크립트 언어인 PowerShell을 사용하여 플랫폼 간 작업 자동화 및 구성 관리를 시작하세요.
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>도구 및 플랫폼

:::row:::
    :::column:::
       [![WSL 아이콘](../images/windows-linux-dev-env.png)](/windows/wsl/)<br>
        **[Linux용 Windows 하위 시스템](/windows/wsl/)**<br>
        Windows와 완전히 통합된, 선호하는 Linux 배포판을 사용합니다(더 이상 이중 부팅 필요 없음).<br>
        [WSL 설치](/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows 터미널 아이콘](../images/terminal.png)](/windows/terminal/)<br>
        **[Windows 터미널](/windows/terminal/)**<br>
        여러 명령줄 셸에서 작동하도록 터미널 환경을 사용자 지정합니다.
        <br>
        [터미널 설치](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows 패키지 관리자 아이콘](../images/winget.png)](../package-manager/index.md)<br>
        **[Windows 패키지 관리자](../package-manager/index.md)**<br>
        명령줄에서 포괄적인 패키지 관리자인 winget.exe 클라이언트를 사용하여 Windows 10에 애플리케이션을 설치합니다.<br>
        [Windows 패키지 관리자(공개 미리 보기) 설치](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys 아이콘](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        이 고급 사용자 유틸리티 세트로 Windows 환경을 조정하고 간소화하여 생산성을 높일 수 있습니다.<br>
        [PowerToys(공개 미리 보기) 설치](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code 아이콘](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        JavaScript, TypeScript, Node.js, 풍부한 확장 에코시스템(C++, C#, Java, Python, PHP, Go) 및 런타임(예: .NET 및 Unity)을 기본적으로 지원하는 경량의 소스 코드 편집기입니다.<br>
        [VS Code 설치](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio 아이콘](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio](/visualstudio/windows/)**<br>
        컴파일러, intellisense 코드 완성 등의 여러 기능을 포함하여 앱을 편집하고, 디버그하고, 코드를 빌드하고, 앱을 게시하는 데 사용할 수 있는 통합 개발 환경입니다.<br>
        [Visual Studio 설치](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure 아이콘 표시](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        기존 앱을 호스트하고 새 앱 개발의 효율을 높이는 완전한 클라우드 플랫폼입니다. Azure 서비스는 앱을 개발, 테스트, 배포 및 관리하는 데 필요한 모든 것을 통합합니다.<br>
        [Azure 계정 설정](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET 아이콘](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](/dotnet/standard/get-started/)**<br>
        웹, 모바일, 데스크톱, 게임, IoT, 클라우드 및 마이크로서비스를 비롯한 모든 유형의 앱을 빌드하기 위한 도구 및 라이브러리가 포함된 오픈 소스 개발 플랫폼입니다.<br>
        [.NET 설치](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>Windows 및 Linux 실행

개발자는 WSL(Linux용 Windows 하위 시스템)을 사용하여 Linux 운영 체제를 Windows와 함께 실행할 수 있습니다. 두 운영 체제가 동일한 하드 드라이브를 공유하고(따라서 서로 파일에 액세스 가능) 클립보드에서 두 운영 체제 간에 자연스럽게 복사-붙여넣기가 가능하므로 이중 부팅이 필요 없습니다. WSL을 사용하면 BASH를 사용할 수 있으며 Mac 사용자에게 가장 익숙한 환경이 제공됩니다.
- [WSL 문서](/windows/wsl) 또는 [Channel 9의 WSL 비디오](https://channel9.msdn.com/Search?term=wsl&lang-en=true)를 통해 자세히 알아보세요.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

Windows 터미널을 사용하여 여러 탭이 있는 단일 창에서 또는 여러 창에서 PowerShell, Windows 명령 프롬프트, Ubuntu, Debian, Azure CLI, Oh-my-Zsh, Git Bash 등의 선호하는 명령줄 도구 또는 위의 모든 도구를 열 수도 있습니다.

[Windows 터미널 문서](/windows/terminal)에서 또는 [Channel 9의 Windows 터미널 비디오](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true)를 통해 자세히 알아보세요.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>Mac과 Windows 간에 전환

[Mac과 Windows(또는 Linux용 Windows 하위 시스템) 개발 환경 간에 전환 가이드](./mac-to-windows.md)를 확인하세요. 다음 항목 간의 차이점을 매핑하는 데 도움이 될 수 있습니다.

- [바로 가기 키](./mac-to-windows.md#keyboard-shortcuts)
- [트랙 패드 바로 가기](./mac-to-windows.md#trackpad-shortcuts)
- [터미널 및 셸 도구](./mac-to-windows.md#command-line-shells-and-terminals)
- [앱 및 유틸리티](./mac-to-windows.md#apps-and-utilities)

![Office 이미지](../images/flashy-office3.png)

## <a name="additional-resources"></a>추가 리소스

- [워크플로 개선을 위한 팁](./tips.md)
- [Mac에서 Windows로 전환한 개발자 사례](./dev-stories.md)
- [인기 있는 자습서, 강좌 및 코드 샘플](./tutorials.md)
- [Microsoft의 게임 스택 설명서](/gaming/)