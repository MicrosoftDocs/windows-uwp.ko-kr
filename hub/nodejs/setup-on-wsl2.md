---
title: WSL 2에 NodeJS 설치
description: WSL(Linux용 Windows 하위 시스템)에 Node.js 개발 환경을 설치하는 방법을 안내하는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, nodejs 학습, windows의 노드, wsl의 노드, windows 기반 linux의 노드, windows에 노드 설치, vs code를 사용하는 nodejs, windows에서 노드를 사용하여 개발, windows에서 nodejs를 사용하여 개발, WSL에 노드 설치, Linux용 Windows 하위 시스템의 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75835377"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>WSL 2를 사용하여 Node.js 개발 환경 설치

다음은 WSL(Linux용 Windows 하위 시스템)을 사용하여 Node.js 개발 환경을 설치하는 방법을 안내하는 단계별 가이드입니다. 이 가이드를 사용하려면 [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)를 설치하여 사용할 수 있도록 Windows Insider Preview 빌드를 설치하여 실행해야 합니다. WSL 2는 특히 Node.js와 관련하여 WSL 1보다 속도와 성능이 크게 향상되었습니다. 많은 Node.js 웹 개발용 npm 모듈과 자습서는 Linux 사용자를 위해 작성되었으며, Linux 기반 패키징 및 설치 도구를 사용합니다. 대부분의 웹앱은 Linux에도 배포되므로 WSL 2를 사용하면 개발 환경과 프로덕션 환경 간에 일관성을 유지할 수 있습니다.

> [!NOTE]
> Windows에서 직접 Node.js를 사용하는 경우 또는 Windows Server 프로덕션 환경을 사용할 계획인 경우 [Windows에 직접 Node.js 개발 환경 설치](./setup-on-windows.md) 가이드를 참조하세요.

## <a name="install-windows-10-insider-preview-build"></a>Windows 10 Insider Preview 빌드 설치

1. 다음과 같이 **[최신 버전의 Windows 10을 설치합니다](https://www.microsoft.com/software-download/windows10)** . **지금 업데이트**를 선택하여 업데이트 도우미를 다운로드합니다. 다운로드가 완료되면 업데이트 도우미를 열어 현재 최신 버전의 Windows를 실행하고 있는지 확인하고, 그렇지 않은 경우 도우미 창 내부에서 **지금 업데이트**를 선택하여 머신을 업데이트합니다. *(Windows 10 최신 버전을 실행 중인 경우 이 단계는 선택 사항입니다.)*

    ![Windows 업데이트 도우미](../images/windows-update-assistant2019.png)

2. **[시작 > 설정 > Windows 참가자 프로그램](ms-settings:windowsinsider)** 으로 이동합니다. Windows 참가자 프로그램 창 내에서 **시작**을 선택한 다음, **계정 연결**을 선택합니다.

    ![Windows 참가자 프로그램 설정](../images/windows-insider-program-settings.png)

3. 다음과 같이 **[Windows 참가자로 등록](https://insider.windows.com/getting-started/#register)** 합니다. 참가자 프로그램에 등록하지 않은 경우 [Microsoft 계정](https://account.microsoft.com/account)을 사용하여 등록해야 합니다.

    ![Windows 참가자 등록](../images/windows-insider-account.png)

4. **초기 링** 업데이트를 받도록 선택하거나 **다음 Windows 릴리스 콘텐츠로 건너뜁니다**. **나중에 다시 시작**을 확인하고 선택합니다. 다시 시작하기 전에 몇 가지 설정을 더 변경해야 합니다.

    ![Windows 참가자 초기 링](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Linux용 Windows 하위 시스템 및 Virtual Machine 플랫폼 사용

1. **Windows 설정**에 있는 상태에서 **Windows 기능 설정 또는 해제**를 검색합니다.
2. **Windows 기능** 목록이 표시되면 목록을 스크롤하여 **Virtual Machine 플랫폼** 및 **Linux용 Windows 하위 시스템**을 찾아 둘 다 사용하도록 확인란을 선택한 다음, **확인**을 선택합니다.
3. 메시지가 표시되면 컴퓨터를 다시 시작합니다.

    ![Windows 기능 사용](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Linux 배포 설치

WSL에서 실행할 수 있는 몇 가지 Linux 배포가 있습니다. Microsoft Store에서 원하는 배포를 찾아 설치할 수 있습니다. 현재 있기 있고 잘 지원되고 있는 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)부터 시작하는 것이 좋습니다.

1. 이 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 링크, Microsoft Store를 차례로 열고, **가져오기**를 선택합니다. *(이는 매우 큰 다운로드이며 설치하는데 시간이 걸릴 수 있습니다.)*

2. 다운로드가 완료되면 Microsoft Store에서 **시작**을 선택하거나 **시작** 메뉴에서 "Ubuntu 18.04 LTS"를 입력하여 시작합니다.

3. 배포를 처음 실행하는 경우 계정 이름과 암호를 만들라는 메시지가 표시됩니다. 이후에는 기본적으로 이 사용자로 자동으로 로그인됩니다. 원하는 사용자 이름과 암호를 선택할 수 있습니다. Windows 사용자 이름과는 관련이 없습니다.

    ![Microsoft Store의 Linux 배포판](../images/store-linux-distros.png)

`lsb_release -dc`를 입력하여 현재 사용 중인 Linux 배포를 확인할 수 있습니다. Ubuntu 배포를 업데이트하려면 `sudo apt update && sudo apt upgrade`를 사용합니다. 최신 패키지를 유지하기 위해 정기적으로 업데이트하는 것이 좋습니다. 이 업데이트는 Windows에서 자동으로 처리하지 않습니다. Microsoft Store에서 사용 가능한 다른 Linux 배포에 대한 링크, 대체 구현 설치 방법 또는 문제 해결은 [Windows 10에 Linux용 Windows 하위 시스템 설치 가이드](https://docs.microsoft.com/windows/wsl/install-win10)를 참조하세요.

## <a name="install-wsl-2"></a>WSL 2 설치

WSL 2는 WSL의 [새 아키텍처 버전](https://docs.microsoft.com/windows/wsl/wsl2-about)으로, Linux 배포판이 Windows와 상호 작용하는 방식을 변경하여 성능을 높이고 전체 시스템 호출 호환성을 추가합니다.

1. PowerShell에서 `wsl -l` 명령을 입력하여 머신에 설치된 WSL 배포판 목록을 볼 수 있습니다. 이 목록에 Ubuntu-18.04가 표시될 것입니다.
2. 이제 `wsl --set-version Ubuntu-18.04 2` 명령을 입력하여 WSL 2를 사용하도록 Ubuntu 설치를 설정합니다.
3. `wsl --list --verbose`(또는 `wsl -l -v`) 명령을 사용하여 설치된 각 배포판에서 사용하는 WSL 버전을 확인합니다.

    ![Linux용 Windows 하위 시스템 버전 설정](../images/wsl-versions.png)

> [!TIP]
> 설치한 Linux 배포판을 WSL 2로 설정할 수 있습니다. 동일한 지침을 따르되(PowerShell 사용), 'Ubuntu-18.04'가 설치된 배포판 중 대상으로 지정하고 싶은 배포판의 이름으로 변경하기만 하면 됩니다. 다시 WSL 1로 변경하려면 위와 동일한 명령을 실행하되, '2'를 '1'로 바꾸면 됩니다.  `wsl --set-default-version 2` 명령을 입력하여 WSL 2를 새로 설치된 배포판의 기본값으로 설정할 수도 있습니다.

## <a name="install-nvm-nodejs-and-npm"></a>nvm, node.js 및 npm 설치

Node.js를 설치하는 여러 가지 방법이 있습니다. 버전이 매우 빠르게 바뀌므로 버전 관리자를 사용하는 것이 좋습니다. 작업하는 여러 프로젝트의 요구 사항에 따라 여러 버전 간에 전환해야 하는 상황이 많을 것입니다. 흔히 nvm으로 불리는 노드 버전 관리자는 여러 버전의 Node.js를 설치하는 가장 인기 있는 방법입니다. nvm을 설치하는 단계와 nvm을 사용하여 Node.js 및 npm(노드 패키지 관리자)을 설치하는 단계를 살펴보겠습니다. 생각해 볼 수 있는 [또 다른 버전 관리자](#alternative-version-managers)도 있으며, 다음 섹션에서 설명하겠습니다.

> [!IMPORTANT]
> 여러 유형을 설치하면 비정상적이고 혼란스러운 충돌이 발생할 수 있으므로, 항상 운영 체제에 설치된 기존 Node.js 또는 npm을 제거한 후 버전 관리자를 설치하는 것이 좋습니다. 예를 들어 Ubuntu의 `apt-get` 명령을 사용하여 설치할 수 있는 노드 버전은 오래된 버전입니다. 이전 설치를 제거하는 방법에 대한 도움말은 [ubuntu에서 nodejs를 제거하는 방법](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)을 참조하세요.

1. Ubuntu 18.04 명령줄을 엽니다.
2. `sudo apt-get install curl` 명령을 사용하여 cURL(명령줄을 사용하여 인터넷에서 콘텐츠를 다운로드하는 데 사용되는 도구)을 설치합니다.
3. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash` 명령을 사용하여 nvm을 설치합니다.
4. 설치를 확인하려면 `command -v nvm` 명령을 입력합니다. 그러면 'nvm'이 반환됩니다. '명령을 찾을 수 없음' 메시지가 수신되거나 응답이 없으면 현재 터미널을 닫았다가 열고 다시 시도해 보세요. [nvm github 리포지토리에서 자세한 정보를 알아보세요](https://github.com/nvm-sh/nvm).
5. `nvm ls` 명령을 사용하여 현재 설치된 노드 버전을 나열합니다.

    ![노드 버전을 표시하지 않는 NVM 목록](../images/nvm-no-node.png)

6. `nvm install node`를 사용하여 Node.js 최신 릴리스를 설치합니다(최신 기능 개선 사항을 테스트하는 것이 목적이지만 이슈가 있을 가능성이 높음).
7. `nvm install --lts` 명령을 사용하여 Node.js의 안정적인 최신 LTS 릴리스를 설치합니다(권장).
8. `nvm ls`를 사용하여 설치된 노드 버전을 나열합니다. 방금 설치한 두 가지 버전이 표시될 것입니다.

    ![LTS 및 현재 노드 버전을 보여주는 NVM 목록](../images/nvm-node-installed.png)

9. `node --version` 명령을 사용하여 Node.js가 설치되어 있는지 여부 및 현재 기본 버전을 확인합니다. 그리고 `npm --version` 명령을 사용하여 npm이 설치되어 있는지 확인합니다(`which node` 또는 `which npm` 명령을 사용하여 기본 버전에 사용되는 경로도 확인 가능).
10. 프로젝트에 사용할 Node.js 버전을 변경하려면 새 프로젝트 디렉터리 `mkdir NodeTest`를 만들고 `cd NodeTest` 디렉터리로 들어간 다음, `nvm use node`를 입력하여 현재 버전으로 전환하거나 `nvm use --lts`를 입력하여 LTS 버전으로 전환합니다. `nvm use v8.2.1`처럼 설치한 버전의 특정 번호를 사용할 수도 있습니다. (사용 가능한 모든 Node.js 버전을 나열하려면 `nvm ls-remote` 명령을 사용합니다.)

> [!TIP]
> NVM을 사용하여 Node.js 및 NPM을 설치하는 경우 SUDO 명령을 사용하여 새 패키지를 설치할 필요가 없습니다.

> [!NOTE]
> 출시 당시에는 NVM v0.35.2가 사용 가능한 최신 버전이었습니다. [GitHub 프로젝트 페이지에서 최신 버전의 NVM](https://github.com/nvm-sh/nvm)을 확인할 수 있으며, 최신 버전만 포함하도록 위의 명령을 조정할 수 있습니다.
cURL을 사용하여 최신 버전의 NVM을 설치하면 이전 버전이 대체되고, NVM을 사용하여 설치한 노드 버전은 그대로 유지됩니다. 예를 들면 다음과 같습니다. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>대체 버전 관리자

현재 노드에 가장 많이 사용되는 버전 관리자는 nvm이지만, 고려해 볼 수 있는 다음과 같은 다른 버전 관리자도 있습니다.

- 오래전부터 `nvm` 대용으로 사용된 [n](https://www.npmjs.com/package/n#installation)은 약간 다른 명령으로 동일한 작업을 수행하며, bash 스크립트 대신 `npm`을 통해 설치됩니다.
- [fnm](https://github.com/Schniz/fnm#using-a-script)은 최신 버전의 관리자이며 `nvm`보다 훨씬 빠르다고 합니다. (마찬가지로 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)를 사용합니다.)
- [Volta](https://github.com/volta-cli/volta#installing-volta)는 LinkedIn 팀의 새로운 버전 관리자로, 속도 및 플랫폼 간 지원이 향상되었다고 합니다.
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm)은 gvm, nvm, rbenv 및 pyenv 등의 여러 언어를 모두 관리할 수 있는 단일 CLI입니다.
- [nvs](https://github.com/jasongin/nvs)(노드 버전 전환기)는 플랫폼 간 `nvm` 대안으로, [VS Code와 통합](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)할 수 있는 기능을 제공합니다.

## <a name="install-your-favorite-code-editor"></a>선호하는 코드 편집기 설치

Node.js 프로젝트에는 **Visual Studio Code**와 **원격 WSL 확장**을 사용하는 것이 좋습니다. 이렇게 하면 VS Code가 “클라이언트-서버” 아키텍처로 분할되어 클라이언트(사용자 인터페이스)는 Windows 머신에서 실행되고 서버(코드, Git, 플러그 인 등)는 원격으로 실행됩니다.

- Linux 기반 Intellisense 및 린트가 지원됩니다.
- 프로젝트는 Linux에서 자동으로 빌드됩니다.
- Linux에서 실행되는 모든 확장([ES Lint, NPM Intellisense, ES6 코드 조각 등](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack))을 사용할 수 있습니다.

터미널 기반 텍스트 편집기(vim, emacs, nano)는 콘솔 내에서 바로 신속하게 변경할 때도 유용합니다. ([이 문서](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68)에서는 차이점 및 각각의 사용 방법을 설명합니다.)

> [!NOTE]
> 일부 GUI 편집기(Atom, Sublime, Eclipse)는 WSL 공유 네트워크 위치(\\wsl $ \Ubuntu\home\)에 액세스할 때 문제가 발생하여 Windows 도구를 통해 Linux 파일을 빌드하려고 시도할 수 있습니다. 이는 우리가 원하는 결과가 아닙니다. VS Code의 원격 WSL 확장은 이 호환성을 알아서 처리합니다.

VS Code 및 원격 WSL 확장을 설치하는 방법은 다음과 같습니다.

1. [Windows용 VS Code를 다운로드하여 설치합니다](https://code.visualstudio.com). VS Code는 Linux에서도 사용할 수 있지만, Linux용 Windows 하위 시스템은 GUI 앱을 지원하지 않으므로 Windows에 설치해야 합니다. 걱정하지 마세요. 여전히 Remote - WSL 확장을 사용하여 Linux 명령줄 및 도구와 통합할 수 있습니다.

2. [Remote - WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)을 VS Code에 설치합니다. 이를 통해 WSL을 통합 개발 환경으로 사용하고, 호환성과 패치를 처리할 수 있습니다. [자세한 정보를 알아보세요](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> VS Code가 이미 설치되어 있는 경우 [Remote - WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)을 설치하려면 [1.35 5월 릴리스](https://code.visualstudio.com/updates/v1_35) 이상이 있어야 합니다. 자동 완성, 디버깅, linting 등의 지원이 손실될 수 있으므로 VS Code에서 Remote - WSL 확장 없이 WSL을 사용하지 않는 것이 좋습니다. 재미있게도 이 WSL 확장은 $HOME/.vscode-server/extensions에 설치됩니다.

### <a name="helpful-vs-code-extensions"></a>유용한 VS Code 확장

VS Code는 기본적으로 Node.js 개발을 위한 여러 기능과 함께 제공되지만, [Node.js 확장 팩](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)에 제공되는 확장 중에도 설치를 고려해 볼 수 있는 유용한 확장이 몇 개 있습니다. 전부 설치해도 되고 가장 유용하다고 생각되는 부분만 선택해서 설치해도 됩니다.

Node.js 확장 팩을 설치하는 방법은 다음과 같습니다.

1. VS Code에서 **확장** 창을 엽니다(Ctrl+Shift+X).

    이제 확장 창은 다음과 같은 세 가지 섹션으로 구분됩니다(원격 WSL 확장을 설치했기 때문에).
    - "로컬 - 설치": Windows 운영 체제에서 사용하기 위해 설치된 확장입니다.
    - "WSL:Ubuntu-18.04-설치": Ubuntu 운영 체제(WSL)에 사용하기 위해 설치된 확장입니다.
    - "권장": 현재 프로젝트의 파일 형식에 따라 VS Code에서 권장하는 확장입니다.

    ![VS Code 로컬 확장과 원격 확장의 비교](../images/vscode-extensions-local-remote.png)

2. [확장] 창의 맨 위에 있는 검색 상자에 **노드 확장 팩**(또는 찾고 있는 확장의 이름)을 입력합니다. 현재 프로젝트가 열려 있는 위치에 따라 VS Code의 로컬 또는 WSL 인스턴스에 대한 확장이 설치됩니다. VS Code 창의 왼쪽 아래 모서리에 있는 원격 링크(녹색)를 선택하여 알 수 있습니다. 원격 연결을 열거나 닫는 옵션이 제공됩니다. "WSL:Ubuntu-18.04" 환경에 Node.js 확장을 설치합니다.

    ![VS Code 원격 링크](../images/wsl-remote-extension.png)

다음과 같은 몇 가지 추가 확장도 고려해 볼 수 있습니다.

- [Chrome용 디버거](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): Node.js를 사용하여 서버 쪽에서 개발을 마친 후에는 클라이언트 쪽에서 개발하고 테스트해야 합니다. 이 확장은 VS Code 편집기를 Chrome 브라우저 디버깅 서비스와 통합하여 효율성을 높입니다.
- [다른 편집기의 키맵](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): Atom, Sublime, Vim, eMacs, 메모장++ 등의 다른 텍스트 편집기에서 전환할 때 이러한 확장을 사용하여 익숙한 환경을 만들 수 있습니다.
- [설정 동기화](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub를 사용하는 여러 설치에서 VS Code 설정을 동기화할 수 있습니다. 여러 머신에서 작업하는 경우 이렇게 하면 여러 머신의 환경을 일관되게 유지할 수 있습니다.

## <a name="install-windows-terminal-optional"></a>Windows 터미널 설치(선택 사항)

새 Windows 터미널을 사용하면 여러 탭(명령 프롬프트, PowerShell 또는 여러 Linux 배포판 간에 신속하게 전환), 사용자 지정 키 바인딩(탭을 열거나 닫는 개발자 고유의 바로 가기 키 만들기, 복사+붙여넣기 등), 이모지 ☺ 및 사용자 지정 테마(색 구성표, 글꼴 스타일 및 크기, 배경 이미지/흐림/투명도)를 사용할 수 있습니다. [자세한 정보를 알아보세요](https://devblogs.microsoft.com/commandline/).

1. 다음과 같이 [Microsoft Store에서 Windows 터미널(미리 보기)](https://www.microsoft.com/store/apps/9n0dx20hk701)을 받습니다. Microsoft Store를 통해 설치하면 업데이트가 자동으로 처리됩니다.

2. 설치가 완료되면 Windows 터미널을 열고 **설정**을 선택한 다음, `profile.json` 파일을 사용하여 터미널을 사용자 지정합니다. [Windows 터미널 설정 편집에 대해 자세히 알아보세요](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Windows 터미널 설정](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Git 설치(선택 사항)

다른 사람과 협업할 계획이거나 GitHub 같은 오픈 소스 사이트에 프로젝트를 호스팅할 계획인 분들을 위해 VS Code는 [Git을 사용한 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적하며, UI에 바로 빌드된 일반적인 Git 명령(추가, 커밋, 푸시, 끌어오기)를 포함하고 있습니다.

1. Git은 Linux용 Windows 하위 시스템 배포판과 함께 설치되지만, git 구성 파일을 설정해야 합니다. 이렇게 하려면 터미널에서 `git config --global user.name "Your Name"`을 입력한 다음, `git config --global user.email "youremail@domain.com"`을 입력합니다. 아직 Git 계정이 없으면 [GitHub에서 가입](https://github.com/join)할 수 있습니다. 이전에 Git를 사용한 경험이 없는 경우 [GitHub 가이드](https://guides.github.com/)를 보면 시작하는 데 도움이 될 수 있습니다. git config를 편집해야 하는 경우 nano: `nano ~/.gitconfig`와 같은 기본 제공 텍스트 편집기를 사용하여 편집할 수 있습니다.

2. 노드 프로젝트에 [.gitignore 파일](https://help.github.com/en/articles/ignoring-files)을 추가하는 것이 좋습니다. 다음은 [GitHub의 Node.js용 기본 gitignore 템플릿](https://github.com/github/gitignore/blob/master/Node.gitignore)입니다. [GitHub 웹 사이트를 사용하여 새 리포지토리 만들기](https://help.github.com/articles/create-a-repo)를 선택하면 Node.js 프로젝트용으로 설정된 .gitignore 파일인 README 파일을 사용하여 리포지토리를 초기화하는 확인란과 필요에 따라 라이선스를 추가할 수 있는 옵션이 제공됩니다.

## <a name="next-steps"></a>다음 단계

Node.js 개발 환경 설정이 완료되었습니다. Node.js 환경 사용을 시작하려면 다음 자습서 중 하나를 진행하세요.

- [초보자용 Node.js 시작](./beginners.md)
- [Windows에서 Node.js 웹 프레임워크 시작](./web-frameworks.md)
- [Node.js 앱을 데이터베이스에 연결](./databases.md)
- [Node.js에서 Docker 컨테이너 사용 시작](./containers.md)
