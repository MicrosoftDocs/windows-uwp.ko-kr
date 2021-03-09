---
title: Windows 10에서 웹 개발
description: Microsoft의 도구, API 및 다양한 리소스에 대한 링크가 포함된 Windows 10의 웹 개발 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: 웹 개발, 웹 개발, windows의 웹, api, edge
ms.date: 01/06/2021
ms.openlocfilehash: 3149e49b258f970989df75b87300a2dce23ff1f2
ms.sourcegitcommit: 5e40a6f8755aca1e97226ce8f85cda239bdcdad0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2021
ms.locfileid: "102259152"
---
# <a name="web-development-on-windows-10"></a>Windows 10에서 웹 개발

Microsoft는 Windows 10을 사용하여 웹 개발을 지원하는 새로운 도구 및 기능을 포함하여 웹 개발자를 위한 다양한 리소스를 제공합니다. 이 가이드에서는 사용 가능한 많은 도구를 다루며 Windows를 웹용으로 개발하기에 이상적인 환경으로 만들기 위한 [피드백을 남기는](../dev-environment/overview.md#additional-resources) 장소를 제공합니다. API 목록은 [웹 개발용 API](/apis.md)를 참조하세요. 시작에 대한 자세한 도움말은 [Windows 10에서 개발 환경 설정](../dev-environment/overview.md)을 참조하세요.

## <a name="webview-devtools-pwas"></a>WebView, DevTools, PWA

:::row:::
    :::column:::
        [![WebView 아이콘](../images/webview2.png)](https://developer.microsoft.com/microsoft-edge/webview2/)<br>
        **[WebView 2](https://developer.microsoft.com/microsoft-edge/webview2/)**<br>
        Microsoft Edge WebView2를 사용하여 네이티브 애플리케이션에 웹 콘텐츠(HTML, CSS 및 JavaScript)를 포함합니다.
        [WebView 2 다운로드](https://developer.microsoft.com/microsoft-edge/webview2/#download-section)
    :::column-end:::
    :::column:::
        [![Microsoft Edge DevTools 아이콘](../images/microsoftedge-devtools.png)](/microsoft-edge/devtools-guide-chromium/index.md)<br>
        **[Microsoft Edge DevTools](/microsoft-edge/devtools-guide-chromium/)**<br>
        Microsoft Edge 개발자 도구는 Microsoft Edge 브라우저에 직접 구축된 검사 및 디버깅 도구 집합입니다.
    Microsoft Edge를 중심으로 DevTools를 열려면 다음을 수행합니다.
        - 마우스 오른쪽 단추를 클릭하고 검사
        - `F12` 키 선택
        - `Ctrl` + `Shift` + `i`
    :::column-end:::
    :::column:::
       [![PWA 아이콘](../images/pwa-icon.png)](/microsoft-edge/progressive-web-apps-chromium/)<br>
        **[Windows의 점진적 웹앱](/microsoft-edge/progressive-web-apps-chromium/)**<br>
        PWA(프로그레시브 웹앱)는 사용자에게 디바이스에 맞게 사용자 지정된 앱과 유사한 기본 경험을 제공합니다. 이는 지원 플랫폼에서 네이티브 앱처럼 작동하도록 점진적으로 향상되는 웹 사이트입니다.<br>
        [PWA 시작](/microsoft-edge/progressive-web-apps-chromium/get-started)
    :::column-end:::
:::row-end:::

## <a name="microsoft-edge-browser"></a>Microsoft Edge 브라우저

:::row:::
    :::column:::
       [![Microsoft Edge 아이콘](../images/microsoftedge.png)](https://www.microsoft.com/en-us/edge)<br>
        **[개발자용 Microsoft Edge](https://developer.microsoft.com/microsoft-edge/)**<br>
        새로운 Microsoft Edge는 Chromium을 기반으로 하여 웹 호환성을 개선하고 기본 웹 플랫폼의 조각화를 줄입니다. 2020년 1월 15일 출시되었으며, Windows, macOS, iOS 및 Android에서 지원됩니다. <br>
        [새로운 Microsoft Edge 설치](https://www.microsoft.com/edge)
    :::column-end:::
    :::column:::
        [![비즈니스용 Microsoft Edge](../images/microsoftedge-enterprise.png)](/deployedge/)<br>
        **[비즈니스용 Microsoft Edge](/deployedge/)**<br>
        Microsoft Edge는 Chromium을 기반으로 하며 엔터프라이즈 지원을 제공합니다. 사용 가능한 여러 채널을 구성하고 배포하는 방법에 대한 단계별 지침을 확인하세요.<br>
        [Microsoft Edge 채널 다운로드](https://www.microsoft.com/edge/business/download)
    :::column-end:::
    :::column:::
        [![Microsoft Edge Insider 아이콘](../images/microsoftedge-beta.png)](https://www.microsoftedgeinsider.com/whats-new)<br>
        **[Microsoft Edge Insider](https://www.microsoftedgeinsider.com/whats-new)**<br>
        우리는 매일 Microsoft Edge를 위한 새로운 것을 빌드하고 있습니다. 최근 진행 상황과 참여 방법에 대해 알아보세요.
        [Microsoft Edge 베타 버전 다운로드](https://www.microsoftedgeinsider.com/)
    :::column-end:::
    :::column:::
        [![Microsoft Edge 지원 아이콘](../images/microsoftedge-support.png)](https://support.microsoft.com/microsoft-edge)<br>
        **[Microsoft Edge 지원](https://support.microsoft.com/microsoft-edge)**<br>
        브라우저 사용자 지정, 확장 추가, 추적 방지, 문제 해결 등에 대한 도움을 받을 수 있습니다.
        [Microsoft Edge에 대한 도움말 보기](https://support.microsoft.com/microsoft-edge)
    :::column-end:::
:::row-end:::

## <a name="debugging-testing-and-accessibility"></a>디버깅, 테스트 및 액세스 가능성

:::row:::
    :::column:::
       [![VS Marketplace Edge 디버거 확장](../images/visualstudio-edge-debugger.png)](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)<br>
        **[VS Code: Microsoft Edge용 디버거](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge/)**<br>
        이 VS Code 확장을 사용하여 Microsoft Edge 브라우저에서 JavaScript 코드를 디버깅합니다. Visual Studio의 ASP.NET 프로젝트에서 사용할 수도 있습니다.<br>
        [VS Code 설치- Microsoft Edge용 디버거](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)
    :::column-end:::
    :::column:::
       [![가상 머신 아이콘](../images/virtualmachine.png)](https://developer.microsoft.com/microsoft-edge/tools/vms/)<br>
        **[테스트용 가상 머신](https://developer.microsoft.com/microsoft-edge/tools/vms/)**<br>
        로컬에서 다운로드하고 관리하는 무료 Windows 10 가상 머신을 사용하여 IE11 및 Microsoft Edge Legacy를 테스트합니다.<br>
        [가상 머신 다운로드](https://developer.microsoft.com/microsoft-edge/tools/vms/)
    :::column-end:::
    :::column:::
       [![WebHint 아이콘](../images/webhint.png)](https://webhint.io/)<br>
        **[접근성을 위한 WebHint](https://webhint.io/)**<br>
        코드에서 모범 사례와 일반적인 오류를 확인하여 사이트의 접근성, 속도, 브라우저 간 호환성 등을 개선하는 데 도움이 되는 사용자 지정 가능한 linting 도구입니다.<br>
        [VS Code 확장 설치](https://webhint.io/docs/user-guide/extensions/vscode-webhint/)<br>
        [브라우저 확장 설치](https://webhint.io/docs/user-guide/extensions/extension-browser/)<br>
        [CLI 설치](https://webhint.io/docs/user-guide/)
    :::column-end:::
    :::column:::
       [![WebDriver 아이콘](../images/webdriver.png)](https://docs.microsoft.com/microsoft-edge/webdriver-chromium/)<br>
        **[WebDriver](https://docs.microsoft.com/microsoft-edge/webdriver-chromium/)**<br>
        Microsoft WebDriver를 사용하여 Microsoft Edge의 웹 사이트 테스트를 자동화하여 개발자 주기의 루프를 종료합니다.<br>
        [WebDriver 설치](https://developer.microsoft.com/microsoft-edge/tools/webdriver/)
    :::column-end:::
:::row-end:::

## <a name="visual-studio-code-editors"></a>Visual Studio Code 편집기

:::row:::
    :::column:::
       [![VS Code 아이콘](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        JavaScript, TypeScript, Node.js, 풍부한 확장 에코시스템(C++, C#, Java, Python, PHP, Go) 및 런타임(예: .NET 및 Unity)을 기본적으로 지원하는 경량의 소스 코드 편집기입니다.<br>
        [VS Code 설치](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio 아이콘](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio(IDE)](/visualstudio/windows/)**<br>
        컴파일러, intellisense 코드 완성 등의 여러 기능을 포함하여 앱을 편집하고, 디버그하고, 코드를 빌드하고, 앱을 게시하는 데 사용할 수 있는 통합 개발 환경입니다.<br>
        [Visual Studio 설치](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![VS Code Marketplace 아이콘](../images/vs-code-marketplace.png)](https://marketplace.visualstudio.com/vscode)<br>
        **[확장용 VS Code Marketplace](https://marketplace.visualstudio.com/vscode)**<br>
        Visual Studio Code 편집기를 사용자 지정하는 데 사용할 수 있는 여러 가지 확장을 살펴보세요.<br>
        [확장 설치](https://marketplace.visualstudio.com/vscode)
    :::column-end:::
    :::column:::
       [![Visual Studio Marketplace 아이콘](../images/vs-marketplace.png)](https://marketplace.visualstudio.com/vs/)<br>
        **[확장용 Visual Studio Marketplace](https://marketplace.visualstudio.com/vs)**<br>
        Visual Studio 통합 개발 환경을 사용자 지정하는 데 사용할 수 있는 여러 가지 확장을 살펴보세요.<br>
        [확장 설치](https://marketplace.visualstudio.com/vs)
    :::column-end:::
:::row-end:::

## <a name="wsl-terminal-package-manager-docker-desktop"></a>WSL, 터미널, 패키지 관리자, Docker Desktop

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
        Windows 10에 앱을 설치하려면 명령줄과 함께 winget.exe 클라이언트를 사용합니다.<br>
        [Windows 패키지 관리자(공개 미리 보기) 설치](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![Windows용 Docker 데스크톱 아이콘](../images/docker-icon.png)](../dev-environment/docker/overview.md)<br>
        **[Windows용 Docker Desktop](../dev-environment/docker/overview.md)**<br>
        Visual Studio, VS Code, .NET, Linux용 Windows 하위 시스템 또는 다양한 Azure 서비스의 지원을 통해 원격 개발 컨테이너를 만듭니다.<br>
        [Windows용 Docker Desktop 설치](https://docs.docker.com/docker-for-windows/install/)
    :::column-end:::
:::row-end:::

## <a name="aspnet-typescript-xamarin"></a>ASP.NET, Typescript, Xamarin

:::row:::
    :::column:::
       [![ASP.NET 아이콘](../images/aspnet.png)](https://dotnet.microsoft.com/apps/aspnet)<br>
        **[ASP.NET](/aspnet/)**<br>
        .NET 및 C#을 사용하여 웹앱 및 서비스, 사물 인터넷(IoT) 앱 또는 모바일 백엔드를 빌드하기 위한 교차 플랫폼 프레임워크입니다. Windows, macOS 및 Linux에서 즐겨 사용하는 개발 도구를 사용합니다. 클라우드 또는 온-프레미스에 배포할 수 있습니다. .NET Core에서 실행합니다.<br>
        [ASP.NET 설치](https://dotnet.microsoft.com/download)
    :::column-end:::
    :::column:::
       [![TypeScript 아이콘](../images/typescript-icon.png)](https://www.typescriptlang.org/)<br>
        **[TypeScript](https://www.typescriptlang.org/)**<br>
        TypeScript는 언어에 유형을 추가하여 JavaScript를 확장합니다. 예를 들어 JavaScript는 문자열, 숫자 및 개체와 같은 언어 기본 요소를 제공하지만, 이를 일관되게 할당했는지 확인하지 않습니다. TypeScript는 이를 확인합니다.<br>
        [브라우저에서 시도해보기](https://www.typescriptlang.org/play/) [로컬로 설치](https://www.typescriptlang.org/#installation)
    :::column-end:::
    :::column:::
       [![Xamarin 리포지토리 아이콘](../images/xamarin-icon.png)](/xamarin/)<br>
        **[Xamarin](/xamarin/)**<br>
        Xamarin을 사용하면 .NET 코드 및 플랫폼별 사용자 인터페이스를 사용하여 Android, iOS 및 macOS용 네이티브 앱을 빌드할 수 있습니다. Xamarin.Forms를 사용하면 C# 또는 XAML에서 작성된 공유 UI 코드를 통해 네이티브 앱을 빌드할 수 있습니다.
        <br>
        [Xamarin 설치](/xamarin/get-started/installation/)
    :::column-end:::
:::row-end:::

## <a name="open-source-contributions"></a>오픈 소스 기여

:::row:::
    :::column:::
       [![OpenSource 아이콘](../images/opensource-icon.png)](https://opensource.microsoft.com/)<br>
        **[Microsoft의 오픈 소스](https://opensource.microsoft.com/)**<br>
        수천 명의 Microsoft 엔지니어가 매일 오픈 소스를 사용하고, 기여하고, 릴리스합니다. 인기 있는 프로젝트에는 Visual Studio Code, TypeScript, .NET 및 ChakraCore가 있습니다.<br>
        [참여하기](https://opensource.microsoft.com/collaborate)
    :::column-end:::
    :::column:::
       [![WinDev 리포지토리 아이콘](../images/windev-repo.png)](https://github.com/microsoft/WinDev)<br>
        **[Windows 개발자 성능 문제 리포지토리](https://github.com/microsoft/WinDev)**<br>
        Windows용으로 개발하든 Windows에서 개발하든 플랫폼 간 개발 머신으로 사용하든, 문제를 일으키는 모든 성능 문제에 대해 듣고자 합니다.
        <br>
        [성능 문제 제출](https://github.com/microsoft/WinDev/issues)
    :::column-end:::
    :::column:::
       [![문서 아이콘](../images/docs.png)](https://docs.microsoft.com/contribute/)<br>
        **[문서 작성에 참여](https://docs.microsoft.com/contribute/)**<br>
        Microsoft의 대부분 설명서 세트는 오픈 소스이며 GitHub에서 호스팅됩니다. 문제를 제출하거나 풀 요청을 작성하여 기여하세요.
        <br>
        [방법 알아보기](https://docs.microsoft.com/contribute/)
    :::column-end:::
:::row-end:::

## <a name="cloud-development-with-azure"></a>Azure를 사용하여 클라우드 개발

:::row:::
    :::column:::
       [![Azure 아이콘 표시](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        기존 앱을 호스트하고 새 앱 개발의 효율을 높이는 완전한 클라우드 플랫폼입니다. Azure 서비스는 앱을 개발, 테스트, 배포 및 관리하는 데 필요한 모든 것을 통합합니다.<br>
        [Azure 계정 설정](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![Azure Cognitive Services 아이콘](../images/azure-cognitive-services.png)](/azure/cognitive-services/what-are-cognitive-services)<br>
        **[Azure Cognitive Services](/azure/cognitive-services/what-are-cognitive-services)**<br>
        애플리케이션에 인식 인텔리전스를 빌드하는 데 도움이 되는 REST API 및 클라이언트 라이브러리 SDK가 포함된 클라우드 기반 서비스입니다.<br>
        [Cognitive Service 사용해보기](https://azure.microsoft.com/en-us/services/cognitive-services/)
    :::column-end:::
    :::column:::
       [![Azure 개발자 가이드 아이콘](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure 학습](/azure/guides/developer/azure-developer-guide)**<br>
        기존 앱을 호스트하고 새 앱 개발의 효율을 높이는 완전한 클라우드 플랫폼입니다. Azure 서비스는 앱을 개발, 테스트, 배포 및 관리하는 데 필요한 모든 것을 통합합니다.<br>
        [Azure 계정 설정](https://azure.microsoft.com/free/)
    :::column-end:::
:::row-end:::

## <a name="addtional-resources"></a>추가 리소스

:::row:::
    :::column:::
       [![개발자 환경 아이콘 설정](../images/dev-environment-icon.png)](../dev-environment/overview.md)<br>
        **[Windows 10에서 개발 환경 설정](../dev-environment/overview.md)**<br>
        Python, NodeJS, C#, C, C++로 작업하고, Android 앱을 빌드하고, Windows 데스크톱 앱을 만들고, Docker 컨테이너를 만들고, PowerShell 스크립트를 실행하는 등 개발 환경 설정에 도움을 받으세요.
        <br>
        [시작](../dev-environment/overview.md)
    :::column-end:::
    :::column:::
       [![Windows용 React Native 아이콘](../images/reactnative-windows.png)](https://microsoft.github.io/react-native-windows/)<br>
        **[Windows + macOS용 React Native](https://microsoft.github.io/react-native-windows/)**<br>
        Windows 10 SDK 및 macOS 10.13 SDK에 React Native 지원을 가져옵니다. JavaScript를 사용하여 PC, 태블릿, 투인원, Xbox, 혼합 현실 디바이스 등을 포함하여 Windows 10에서 지원하는 모든 디바이스와 macOS 데스크톱 및 랩톱 에코시스템을 위한 기본 Windows 앱을 빌드합니다.
        <br>
        [Windows용 React Native 설치](https://microsoft.github.io/react-native-windows/docs/getting-started)<br>
        [macOS용 React Native 설치](https://microsoft.github.io/react-native-windows/docs/rnm-getting-started)
    :::column-end:::
    :::column:::
       [![학습 아이콘](../images/learn-icon.png)](https://docs.microsoft.com/learn/browse/?terms=web)<br>
        **[웹 개발과 관련된 Microsoft Learn 과정](https://docs.microsoft.com/learn/browse/?terms=web)**<br>
        Microsoft Learn은 다양한 새로운 기술을 배우고 단계별 지침을 통해 Microsoft 제품 및 서비스를 검색할 수 있는 무료 온라인 과정을 제공합니다.
        <br>
        [학습 시작](https://docs.microsoft.com/learn/browse/?terms=web)
    :::column-end:::
:::row-end:::

## <a name="transitioning-between-mac-and-windows"></a>Mac과 Windows 간에 전환

[Mac과 Windows(또는 Linux용 Windows 하위 시스템) 개발 환경 간에 전환 가이드](../dev-environment/mac-to-windows.md)를 확인하세요.

- [바로 가기 키](../dev-environment/mac-to-windows.md#keyboard-shortcuts)
- [트랙 패드 바로 가기](../dev-environment/mac-to-windows.md#trackpad-shortcuts)
- [터미널 및 셸 도구](../dev-environment/mac-to-windows.md#command-line-shells-and-terminals)
- [앱 및 유틸리티](../dev-environment/mac-to-windows.md#apps-and-utilities)
- [Mac에서 Windows로 전환한 개발자 사례](../dev-environment/dev-stories.md)
