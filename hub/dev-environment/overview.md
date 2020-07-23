---
title: Windows 10 개발 환경 설정
description: Windows 개발 환경을 설정하고 최적화하는 데 도움이 되는 가이드입니다. Windows 또는 Linux용 Windows 하위 시스템을 사용하여 개발할 때 필요한 언어 및 도구를 설치하는 방법을 안내합니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: ''
ms.localizationpriority: medium
ms.date: 07/01/2020
ROBOTS: NOINDEX
ms.openlocfilehash: 237c3e8f58e41007840cf72aa1fd65efdc5763a5
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493826"
---
# <a name="set-up-your-windows-10-development-environment"></a>Windows 10 개발 환경 설정

이 가이드는 Windows 또는 Linux용 Windows 하위 시스템에서 개발할 때 필요한 언어 및 도구를 설치하고 설정하는 방법을 안내합니다.

## <a name="development-paths"></a>개발 경로

:::row:::
    :::column:::
       [![JavaScrip/NodeJS](../images/nodejs-logo.png)](https://docs.microsoft.com/windows/nodejs)<br>
        **[NodeJS 시작](https://docs.microsoft.com/windows/nodejs)**<br>
        Windows 또는 Linux용 Windows 하위 시스템에서 NodeJS를 설치하고 개발 환경을 설정합니다.
    :::column-end:::
    :::column:::
       [![Python](../images/python-logo.png)](https://docs.microsoft.com/windows/python)<br>
        **[Python 시작](https://docs.microsoft.com/windows/python)**<br>
        Windows 또는 Linux용 Windows 하위 시스템에서 Python을 설치하고 개발 환경을 설정합니다.
    :::column-end:::
    :::column:::
       [![Android](../images/android-logo.png)](https://docs.microsoft.com/windows/android)<br>
        **[Android 시작](https://docs.microsoft.com/windows/android)**<br>
        Android Studio를 설치하거나 Xamarin, React 또는 Cordova 같은 플랫폼 간 솔루션을 선택하고 Windows에서 개발 환경을 설정합니다.
    :::column-end:::
    :::column:::
       [![Windows 데스크톱](../images/windows-logo.png)](https://docs.microsoft.com/windows/apps/)<br>
        **[Windows 시작](https://docs.microsoft.com/windows/apps/)**<br>
        UWP, Win32, WPF, Windows Forms를 사용하여 Windows 10용 데스크톱 앱을 빌드하거나 MSIX 및 XAML Islands를 사용하여 기존 데스크톱 앱을 업데이트하고 배포합니다.
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>도구 및 플랫폼

:::row:::
    :::column:::
       [![WSL](../images/windows-linux-dev-env.png)](https://docs.microsoft.com/windows/wsl/)<br>
        **[Linux용 Windows 하위 시스템](https://docs.microsoft.com/windows/wsl/)**<br>
        Windows와 완전히 통합된, 선호하는 Linux 배포판을 사용합니다(더 이상 이중 부팅 필요 없음).<br>
        [WSL 설치](https://docs.microsoft.com/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows 터미널](../images/terminal.png)](https://docs.microsoft.com/windows/terminal/)<br>
        **[Windows 터미널](https://docs.microsoft.com/windows/terminal/)**<br>
        여러 명령줄 셸에서 작동하도록 터미널 환경을 사용자 지정합니다.
        <br>
        [터미널 설치](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows 패키지 관리자](../images/winget.png)](https://docs.microsoft.com/windows/package-manager/)<br>
        **[Windows 패키지 관리자](https://docs.microsoft.com/windows/package-manager/)**<br>
        명령줄에서 포괄적인 패키지 관리자인 WinGet을 사용하여 Windows 10에 애플리케이션을 설치합니다.<br>
        [WinGet 설치](https://docs.microsoft.com/windows/package-manager/winget/#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        이 고급 사용자 유틸리티 세트로 Windows 환경을 조정하고 간소화하여 생산성을 높일 수 있습니다.<br>
        [PowerToys 설치](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        JavaScript, TypeScript, Node.js, 풍부한 확장 에코시스템(C++, C#, Java, Python, PHP, Go) 및 런타임(예: .NET 및 Unity)을 기본적으로 지원하는 경량의 소스 코드 편집기입니다.<br>
        [VS Code 설치](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio](../images/visualstudio.png)](https://docs.microsoft.com/visualstudio/windows/)<br>
        **[Visual Studio](https://docs.microsoft.com/visualstudio/windows/)**<br>
        컴파일러, intellisense 코드 완성 등의 여러 기능을 포함하여 앱을 편집하고, 디버그하고, 코드를 빌드하고, 앱을 게시하는 데 사용할 수 있는 통합 개발 환경입니다.<br>
        [Visual Studio 설치](https://docs.microsoft.com/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure](../images/Azure.png)](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)**<br>
        기존 앱을 호스트하고 새 앱 개발의 효율을 높이는 완전한 클라우드 플랫폼입니다. Azure 서비스는 앱을 개발, 테스트, 배포 및 관리하는 데 필요한 모든 것을 통합합니다.<br>
        [Azure 계정 설정](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](https://docs.microsoft.com/dotnet/standard/get-started/)**<br>
        웹, 모바일, 데스크톱, 게임, IoT, 클라우드 및 마이크로서비스를 비롯한 모든 유형의 앱을 빌드하기 위한 도구 및 라이브러리가 포함된 오픈 소스 개발 플랫폼입니다.<br>
        [.NET 설치](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

---

<br>

![필러 이미지](../images/flashy-office.png)

## <a name="tips-for-improving-your-workflow"></a>워크플로 개선을 위한 팁

워크플로를 보다 효율적이고 즐겁게 만드는 데 도움이 되는 몇 가지 팁을 수집했습니다. 공유할 추가 팁이 있나요? 위의 "편집" 단추를 사용하여 끌어오기 요청을 작성하거나 아래의 "피드백" 단추를 사용하여 문제를 제출하면 저희가 해당 항목을 목록에 추가할 수 있습니다.

* Linux용 Windows 하위 시스템은 내부 개발자 루프의 일부로 사용하기 위한 것입니다. 예제 워크플로는 CI/CD 파이프라인을 만들고, WSL 2를 사용하여 Windows 머신에 Ubuntu를 설치하고, 실제 Linux 인스턴스를 사용하여 로컬로 개발하는 예제 워크플로를 추천합니다. 정상적으로 작동하는 것을 확인한 후에는 해당 CI/CD 파이프라인을 Docker 컨테이너로 만들고 프로덕션에 사용 가능한 Ubuntu VM에서 실행되는 클라우드 인스턴스로 컨테이너를 푸시하여 해당 CI/CD 파이프라인을 클라우드로 푸시할 수 있습니다. WSL을 사용하는 더 많은 방법은 이 [WSL 2의 탭 vs 공간 에피소드](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)를 참조하세요.

* Windows 및 Linux용 windows 하위 시스템을 모두 사용하는 경우 두 개의 파일 시스템 NTSF(Windows) 및 WSL(Linux 배포판)이 설치되어 있습니다. 빠른 성능을 얻을 수 있도록 프로젝트 파일을 사용 중인 도구와 동일한 시스템에 저장해야 합니다. [빠른 성능을 얻을 수 있도록 올바른 파일 시스템을 선택하는 방법](https://docs.microsoft.com/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)에 대해 알아보세요.

* 보안 위협 검사를 생략해도 될 만큼 충분히 신뢰하는 프로젝트 폴더 또는 파일 형식에 대한 제외를 추가하도록 Windows Defender 설정을 업데이트하여 빌드 속도를 향상할 수 있습니다. [성능 향상을 위한 Windows Defender 설정 업데이트](https://docs.microsoft.com/windows/android/defender-settings) 방법에 대해 자세히 알아보세요.

![Windows Defender 스크린샷](../images/windows-defender-exclusions.png)

* 명령줄에서 `code .` 명령을 사용하여 연 프로젝트로 VS Code를 시작하거나, Windows 또는 WSL 배포판의 Windows 파일 탐색기 명령줄에서 `explorer.exe .` 명령을 사용하여 프로젝트 디렉터리를 열 수 있습니다. 이렇게 해도 기본적으로 작동하지 않는 경우 PATH 환경 변수에 VS Code 실행 파일을 추가해야 할 수도 있습니다. [명령줄에서 시작](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)에 대해 자세히 알아보세요.

![Windows 파일 탐색기 스크린샷](../images/wsl-file-explorer.png)

* 버전 제어 및 협업에 Git을 사용하는 경우 Windows 자격 증명 관리자에 토큰을 저장하도록 [Git 자격 증명 관리자를 설정](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)하여 인증 프로세스를 간소화할 수 있습니다. 또한 프로젝트에 [.gitignore 파일을 추가](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)하는 것이 좋습니다.

* [Windows 터미널 명령줄 인수](https://docs.microsoft.com/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)를 사용하여 PowerShell, Ubuntu, Azure CLI 등의 여러 명령줄을 여러 블레이드가 있는 단일 창으로 시작할 수 있습니다. [Windows 터미널](https://docs.microsoft.com/windows/terminal/get-started), [WSL/Ubuntu](https://docs.microsoft.com/windows/wsl/install-win10) 및 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)를 설치한 후에는 PowerShell에 다음 명령을 입력하여 세 명령줄이 모두 있는 새로운 다중 블레이드 창을 엽니다.

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

![필러 이미지](../images/flashy-office2.png)

## <a name="transitioning-between-mac-and-windows"></a>Mac과 Windows 간에 전환

[Mac과 Windows(또는 Linux용 Windows 하위 시스템) 개발 환경 간에 전환 가이드](https://docs.microsoft.com/windows/dev-environment/mac-to-windows)를 확인하세요. 다음 항목 간의 차이점을 매핑하는 데 도움이 될 수 있습니다.

* [바로 가기 키](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#keyboard-shortcuts)
* [트랙 패드 바로 가기](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#trackpad-shortcuts)
* [터미널 및 셸 도구](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#terminal-and-shell)
* [앱 및 유틸리티](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#apps-and-utilities)

## <a name="stories-from-developers-who-have-switched"></a>전환한 개발자 사례

Mac과 Windows 개발 환경 간에 전환한 다른 개발자의 경험담을 들어 보면 도움이 될 수 있습니다. 대부분의 개발자는 프로세스가 매우 간단하고, 선호하는 Linux 및 오픈 소스 도구를 계속 사용할 수 있을 뿐만 아니라 [Microsoft Office](https://www.microsoft.com/microsoft-365/products-apps-services), [Outlook](https://www.microsoft.com/microsoft-365/outlook/email-and-calendar-software-microsoft-outlook), [Teams](https://www.microsoft.com/microsoft-365/microsoft-teams/group-chat-software) 등의 Windows 생산성 도구에 통합 액세스할 수 있게 되었습니다. 다음은 저희가 찾은 몇 가지 문서와 블로그 게시물입니다.

* Ken Wang의 ["Think Different — Software Developer Switching from Mac to Windows"](https://medium.com/@kenwang_57215/software-developer-switching-from-mac-to-windows-66773d331910)
* Owen Williams의 ["The state of switching to Windows from Mac in 2019"](https://char.gd/blog/2019/the-state-of-switching-to-windows-from-mac-in-2019)
* Brent Rose의 ["What Happened When I Switched From Mac to Windows"](https://www.wired.com/story/rant-switching-from-mac-to-windows/)
* Jack Franklin의 ["Using Windows 10 and WSL for frontend web development"](https://www.jackfranklin.co.uk/blog/frontend-development-with-windows-10/)
* Aaron Schlesinger의 ["Coming from a Mac to Windows & WSL 2"](https://arschles.com/blog/coming-from-a-mac-to-windows-wsl-2/)
* David Heinemeier Hansson의 ["Back to windows after twenty years"](https://m.signalvnoise.com/back-to-windows-after-twenty-years/)
* Ray Elenteny의 ["Why I returned to Windows"](https://dzone.com/articles/why-i-returned-to-windows)

## <a name="tutorials-courses-and-code-samples"></a>자습서, 강좌 및 코드 샘플

아래는 일반적인 작업 시나리오를 시작하는 데 도움이 되는 자습서, 강좌 및 코드 샘플입니다.

* [React 및 Azure Cosmos DB를 사용하여 MongoDB 앱 만들기](https://docs.microsoft.com/azure/cosmos-db/tutorial-develop-mongodb-react)

* [끌어서 놓기 기능을 사용하여 Android 이중 화면 앱 빌드](https://docs.microsoft.com/dual-screen/android/samples)

* [Xamarin.Forms를 사용하여 할 일 목록 플랫폼 간 앱 빌드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [Google Play 서비스를 활용하여 Google Maps API를 시연하는 Xamarin.Android 앱 빌드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [Azure App Service에서 PostgreSQL을 사용하여 Python(Django) 웹앱 배포](https://docs.microsoft.com/azure/app-service/containers/tutorial-python-postgresql-app?tabs=bash)

* [Blazor를 사용하여 첫 번째 ASP.Net Core 웹앱 빌드](https://docs.microsoft.com/aspnet/core/tutorials/build-your-first-blazor-app?view=aspnetcore-3.1)

* [Microsoft Graph를 사용하여 Java 앱 빌드](https://docs.microsoft.com/graph/tutorials/java)

* [Azure AD V2를 사용하여 WPF 애플리케이션에서 ASP.NET Core Web API 호출](https://docs.microsoft.com/samples/azure-samples/active-directory-dotnet-native-aspnetcore-v2/calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2/?view=aspnetcore-3.1)

* [클라우드 네이티브 ASP.NET Core 마이크로서비스 만들기 및 배포](https://docs.microsoft.com/learn/modules/microservices-aspnet-core/?view=aspnetcore-3.1)

* [Microsoft Learn의 무료 온라인 강좌 살펴보기](https://docs.microsoft.com/learn/browse/)

![필러 이미지](../images/flashy-office3.png)

## <a name="additional-resources"></a>추가 리소스

* [Microsoft Edge 웹 브라우저 문서](https://docs.microsoft.com/microsoft-edge/)
* [웹 사이트 개선을 위해 WebHint 시도해 보기](https://webhint.io/)
* [Microsoft의 게임 스택 설명서](https://docs.microsoft.com/gaming/)
