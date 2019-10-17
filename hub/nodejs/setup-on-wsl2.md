---
title: WSL 2에서 NodeJS 설정
description: Linux 용 Windows 하위 시스템 (WSL)에서 node.js 개발 환경을 설정 하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node.js, windows 10, microsoft, learning NodeJS, windows의 노드, windows의 노드, windows에서 노드 설치, windows에서 노드 설치, windows에서 노드를 사용 하 여 개발, windows에서 NodeJS를 사용 하 여 개발, windows에서 노드 설치, windows의 NodeJS Linux 용 하위 시스템
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: e5875f0bf7ce73d3615aa131d57c2384c73dd8a1
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517839"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>WSL 2를 사용 하 여 node.js 개발 환경 설정

다음은 Linux 용 Windows 하위 시스템 (WSL)을 사용 하 여 node.js 개발 환경을 설정 하는 데 도움이 되는 단계별 가이드입니다. 이 가이드에서는 현재 [Wsl 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)를 설치 및 사용 하기 위해 Windows Insider preview 빌드를 설치 하 고 실행 해야 합니다. WSL 2는 특히 node.js와 관련 하 여 WSL 1을 통한 속도와 성능이 크게 향상 되었습니다. Node.js 웹 개발에 대 한 많은 npm 모듈 및 자습서는 Linux 사용자를 위해 작성 되었으며 Linux 기반 패키징 및 설치 도구를 사용 합니다. 대부분의 웹 앱은 Linux에도 배포 되므로 WSL 2를 사용 하면 개발 환경과 프로덕션 환경 간에 일관성을 유지할 수 있습니다.

> [!NOTE]
> Windows에서 직접 node.js를 사용 하거나 Windows Server 프로덕션 환경을 사용할 계획인 경우 [windows에서 직접 node.js 개발 환경 설정](./setup-on-windows.md)가이드를 참조 하세요.

## <a name="install-windows-10-insider-preview-build"></a>Windows 10 Insider preview 빌드 설치

1. **[최신 버전의 Windows 10 설치](https://www.microsoft.com/software-download/windows10)** : 업데이트 도우미를 다운로드 하려면 **지금 업데이트** 를 선택 합니다. 다운로드 한 후 업데이트 도우미를 열어 현재 최신 버전의 Windows를 실행 하 고 있는지 확인 하 고, 그렇지 않은 경우에는 길잡이 창 내부에서 **지금 업데이트** 를 선택 하 여 컴퓨터를 업데이트 합니다. *(이 단계는 Windows 10의 최신 버전을 실행 하는 경우 선택 사항입니다.)*

    ![Windows 업데이트 도우미](../images/windows-update-assistant2019.png)

2. **[시작 > 설정 > Windows Insider program](ms-settings:windowsinsider)** : Windows 참가자 프로그램 창 내에서 **시작** , **계정 연결**을 차례로 선택 합니다.

    ![Windows 참가자 프로그램 설정](../images/windows-insider-program-settings.png)

3. **[Windows 참가자로 등록](https://insider.windows.com/getting-started/#register)** : Insider program에 등록 하지 않은 경우 [Microsoft 계정](https://account.microsoft.com/account)를 사용 하 여 등록 해야 합니다.

    ![Windows 참가자 등록](../images/windows-insider-account.png)

4. **빠른 링** 업데이트를 수신 하도록 선택 하거나 **다음 Windows 릴리스 콘텐츠로 건너뜁니다** . 를 확인 하 고 **나중에 다시 시작**하도록 선택 합니다. 다시 시작 하기 전에 몇 가지 추가 설정을 변경 해야 합니다.

    ![Windows Insider Fast 링](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Linux 및 가상 컴퓨터 플랫폼에 대 한 Windows 하위 시스템 사용

1. **Windows 설정**에서 **windows 기능 사용/사용 안 함**을 검색 합니다.
2. **Windows 기능** 목록이 표시 되 면를 스크롤하여 **가상 컴퓨터 플랫폼** 및 **Linux 용 Windows 하위 시스템**을 찾습니다. 확인란을 모두 사용 하도록 선택 했는지 확인 한 다음 **확인**을 선택 합니다.
3. 메시지가 표시되면 컴퓨터를 다시 시작합니다.

    ![Windows 기능 사용](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Linux 배포 설치

WSL에서 실행할 수 있는 몇 가지 Linux 배포판이 있습니다. Microsoft Store에서 즐겨찾기를 찾아 설치할 수 있습니다. [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 는 최신의 인기 있고 잘 지원 되는 것으로 시작 하는 것이 좋습니다.

1. 이 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 링크를 열고 Microsoft Store를 연 다음 **가져오기**를 선택 합니다. *(이것은 매우 큼 다운로드 이며 설치 하는 데 약간의 시간이 걸릴 수 있습니다.)*

2. 다운로드가 완료 되 면 Microsoft Store에서 **시작** 을 선택 하거나 **시작** 메뉴에 "Ubuntu 18.04 lts"를 입력 하 여 시작 합니다.

3. 처음으로 배포를 실행할 때 계정 이름 및 암호를 만들라는 메시지가 표시 됩니다. 그 후에는 기본적으로이 사용자로 자동 로그인 됩니다. 사용자 이름 및 암호를 선택할 수 있습니다. Windows 사용자 이름에는 영향을 미치지 않습니다.

    ![Microsoft Store의 Linux 배포판](../images/store-linux-distros.png)

@No__t-0을 입력 하 여 현재 사용 중인 Linux 배포를 확인할 수 있습니다. Ubuntu 배포를 업데이트 하려면 `sudo apt update && sudo apt upgrade`을 사용 합니다. 최신 패키지를 유지 하기 위해 정기적으로 업데이트 하는 것이 좋습니다. 이 업데이트는 Windows에서 자동으로 처리 되지 않습니다. Microsoft Store, 대체 설치 방법 또는 문제 해결에서 사용할 수 있는 다른 Linux 배포에 대 한 링크는 windows [10 용 Windows 하위 시스템 설치 가이드](https://docs.microsoft.com/windows/wsl/install-win10)를 참조 하세요.

## <a name="install-wsl-2"></a>WSL 2 설치

WSL 2는 Linux 배포판이 Windows와 상호 작용 하 여 성능을 향상 시키고 전체 시스템 호출 호환성을 추가 하는 방법을 변경 하는 WSL의 [새 아키텍처 버전](https://docs.microsoft.com/windows/wsl/wsl2-about) 입니다.

1. PowerShell에서 다음 명령을 입력 합니다. `wsl -l`을 입력 하 여 컴퓨터에 설치한 WSL 배포 목록을 표시 합니다. 이제이 목록에 Ubuntu-18.04이 표시 됩니다.
2. 이제 @no__t를 입력 하 여 Ubuntu 설치를 WSL 2를 사용 하도록 설정 합니다.
3. 설치 된 각 배포에 대해 `wsl --list --verbose` (또는 `wsl -l -v`)과 함께 사용 하는 WSL의 버전을 확인 합니다.

    ![Linux 용 Windows 하위 시스템 집합 버전](../images/wsl-versions.png)

> [!TIP]
> 동일한 지침 (PowerShell 사용)을 사용 하 여 설치 된 모든 Linux 배포를 WSL 2로 설정할 수 있습니다. ' Ubuntu-18.04 '을 대상으로 하는 설치 된 배포판의 이름으로 변경 합니다. WSL 1로 다시 변경 하려면 위와 동일한 명령을 실행 하지만 ' 2 '를 ' 1 '로 바꿉니다.  @No__t-0을 입력 하 여 새로 설치 된 배포에 대 한 기본으로 WSL 2를 설정할 수도 있습니다.

## <a name="install-nvm-nodejs-and-npm"></a>Nvm, node.js 및 npm 설치

Node.js를 설치 하는 방법에는 여러 가지가 있습니다. 버전 변경이 매우 빠르게 변경 되 면 버전 관리자를 사용 하는 것이 좋습니다. 작업 하는 다른 프로젝트의 요구에 따라 여러 버전 간에 전환 해야 할 수도 있습니다. 노드 버전 관리자 (일반적으로 nvm 이라고 함)는 여러 버전의 node.js를 설치 하는 가장 인기 있는 방법입니다. Nvm을 설치 하 고이를 사용 하 여 node.js 및 npm (Node Package Manager)를 설치 하는 단계를 안내 합니다. 다음 섹션에서 설명 하는 것과 같은 [대체 버전 관리자](#alternative-version-managers) 가 있습니다.

> [!IMPORTANT]
> 버전 관리자를 설치 하기 전에 운영 체제에서 node.js 또는 npm의 기존 설치를 제거 하는 것이 좋습니다 .이 경우에는 다른 유형의 설치로 인해 비정상적이 고 혼란 스러운 충돌이 발생할 수 있습니다. 예를 들어 Ubuntu의 `apt-get` 명령으로 설치할 수 있는 노드 버전은 현재 오래 된 버전입니다. 이전 설치 제거에 대 한 도움말은 [ubuntu에서 nodejs를 제거 하는 방법](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)을 참조 하세요.

1. Ubuntu 18.04 명령줄을 엽니다.
2. 다음을 사용 하 여 말아 (명령줄에서 인터넷에서 콘텐츠를 다운로드 하는 데 사용 되는 도구)를 설치 합니다. `sudo apt-get install curl`
3. 다음을 사용 하 여 nvm 설치: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
4. 설치를 확인 하려면 다음을 입력 합니다. `command -v nvm` ... ' n e t e r '를 반환 해야 합니다. ' command not found '를 수신 하거나 응답이 없으면 현재 터미널을 닫고 다시 연 후 다시 시도 하세요. [Nvm github 리포지토리에서 자세히 알아보세요](https://github.com/nvm-sh/nvm).
5. 현재 설치 되어 있는 노드 버전 나열 (이 시점에서 없어야 함): `nvm ls`

    ![노드 버전을 표시 하지 않는 n VM 목록](../images/nvm-no-node.png)

6. 최신 기능 개선 사항을 테스트 하기 위해 최신 버전의 node.js를 설치 하 고 문제를 일으킬 가능성이 높습니다. `nvm install node`
7. Node.js의 안정적인 최신 LTS 릴리스를 설치 합니다 (권장). `nvm install --lts`
8. 설치 된 노드 버전 나열: `nvm ls` ... 이제 방금 설치한 두 버전이 표시 됩니다.

    ![LTS 및 현재 노드 버전을 보여 주는 NVM 목록](../images/nvm-node-installed.png)

9. Node.js가 설치 되어 있고 현재 기본 버전인 `node --version` 인지 확인 합니다. 그런 다음 `npm --version`을 사용 하 여 npm를 확인 합니다. 즉, `which node` 또는 `which npm`를 사용 하 여 기본 버전에 사용 되는 경로를 확인할 수도 있습니다.
10. 프로젝트에 사용 하려는 node.js의 버전을 변경 하려면 새 프로젝트 디렉터리 `mkdir NodeTest`을 만들고 @no__t 디렉터리를 입력 한 다음 `nvm use node`를 입력 하 여 현재 버전으로 전환 하거나 `nvm use --lts`을 입력 하 여 LTS 버전으로 전환 합니다. @No__t-0과 같이 설치한 모든 추가 버전에 대해 특정 번호를 사용할 수도 있습니다. 사용 가능한 node.js의 모든 버전을 나열 하려면 다음 명령을 사용 합니다. `nvm ls-remote`).

> [!TIP]
> NVM을 사용 하 여 node.js 및 NPM을 설치 하는 경우 SUDO 명령을 사용 하 여 새 패키지를 설치할 필요가 없습니다.

> [!NOTE]
> 게시 당시 NVM v 0.34.0는 사용 가능한 최신 버전 이었습니다. 최신 버전의 [NVM에 대 한 GitHub 프로젝트 페이지](https://github.com/nvm-sh/nvm)를 확인 하 고 위의 명령을 조정 하 여 최신 버전을 포함 시킬 수 있습니다.
새 버전의 NVM을 설치 하면 이전 버전을 대체 하 여 NVM을 사용한 노드 버전이 그대로 유지 됩니다. 예를 들면 다음과 같습니다. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash`

## <a name="alternative-version-managers"></a>대체 버전 관리자

Nvm은 현재 노드당 가장 널리 사용 되는 버전 관리자 이지만 다음과 같은 몇 가지 방법을 고려해 야 합니다.

- [n](https://www.npmjs.com/package/n#installation) 은 약간 다른 명령으로 동일한 작업을 수행 하 고 bash 스크립트 대신 `npm`를 통해 설치 되는 장기 `nvm` 대체입니다.
- [fnm](https://github.com/Schniz/fnm#using-a-script) 은 최신 버전 관리자로, `nvm` 보다 훨씬 빠르게 주장 합니다. (또한 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)를 사용 합니다.)
- [Volta](https://github.com/volta-cli/volta#installing-volta) 는 향상 된 속도 및 플랫폼 간 지원을 제공 하는 LinkedIn 팀의 새로운 버전 관리자입니다.
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) 은 ike gvm, nvm, rssisdb v & pyenv 등의 여러 언어에 대 한 단일 CLI입니다.
- [nvs](https://github.com/jasongin/nvs) (Node 버전 전환기)는 [VS Code와 통합할](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)수 있는 플랫폼 간 `nvm` 대안입니다.

## <a name="install-your-favorite-code-editor"></a>즐겨 찾는 코드 편집기 설치

Node.js 프로젝트용 **원격-WSL 확장** 에 **Visual Studio Code** 를 사용 하는 것이 좋습니다. 이는 Windows 컴퓨터에서 실행 되는 클라이언트 (사용자 인터페이스)와 원격으로 실행 되는 서버 (코드, Git, 플러그 인 등)를 사용 하 여 "클라이언트 서버" 아키텍처로 VS Code 분할 됩니다.

- Linux 기반 Intellisense 및 lint 지원 됩니다.
- 프로젝트는 Linux에서 자동으로 빌드됩니다.
- Linux에서 실행 되는 모든 확장 ([ES, NPM Intellisense, ES6 코드 조각 등](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack))을 사용할 수 있습니다.

터미널 기반 텍스트 편집기 (vim, emacs, nano)는 콘솔 내에서 바로 빠르게 변경 하는 데에도 유용 합니다. ([이 문서](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) 에서는 차이점을 설명 하 고 각각을 사용 하는 방법을 설명 하는 유용한 작업을 수행 합니다.)

> [!NOTE]
> 일부 GUI 편집기 (Atom, Sublime, Eclipse)는 WSL 공유 네트워크 위치에 액세스 하는 데 문제가 있을 수 있습니다 (\\wsl $ \Ubuntu\home @ no__t-1) .이는 사용자가 원하는 대로 Windows 도구를 사용 하 여 Linux 파일 빌드를 시도 합니다. VS Code의 원격 WSL 확장은이 호환성을 처리 합니다.

VS Code 및 원격-WSL 확장을 설치 하려면:

1. [Windows 용 VS Code를 다운로드 하 고 설치](https://code.visualstudio.com)합니다. VS Code은 Linux 에서도 사용할 수 있지만 Linux 용 Windows 하위 시스템은 GUI 앱을 지원 하지 않으므로 Windows에 설치 해야 합니다. 걱정 하지 마세요. 여전히 원격-WSL 확장을 사용 하 여 Linux 명령줄 및 도구와 통합할 수 있습니다.

2. VS Code에 [원격-WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) 을 설치 합니다. 이를 통해 사용자는 통합 개발 환경으로 WSL을 사용 하 고 호환성 및 pathing를 처리할 수 있습니다. [자세한 내용을 알아보세요](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> VS Code 이미 설치 되어 있는 경우에는 [원격-WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)을 설치 하기 위해 [1.35 이상 버전을 릴리스할 수](https://code.visualstudio.com/updates/v1_35) 있는지 확인 해야 합니다. 자동 완성, 디버깅, lint 등에 대 한 지원이 손실 될 것 이므로, 원격-WSL 확장이 없으면 VS Code에서 WSL을 사용 하지 않는 것이 좋습니다. 흥미로운 사실:이 WSL 확장은 $HOME/.vscode-server/extensions.에 설치 됩니다.

### <a name="helpful-vs-code-extensions"></a>유용한 VS Code 확장

VS Code는 기본적으로 node.js 개발을 위한 다양 한 기능을 제공 하지만 [Node.js 확장 팩](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)에서 사용할 수 있는 설치를 고려 하는 몇 가지 유용한 확장이 있습니다. 모두 설치 하거나 선택 하 고 가장 유용 하다 고 생각 하는 선택 합니다.

Node.js 확장 팩을 설치 하려면:

1. VS Code에서 **확장** 창 (Ctrl + Shift + X)을 엽니다.

    이제 확장 창은 원격 WSL 확장을 설치 했기 때문에 세 개의 섹션으로 구분 됩니다.
    - "로컬 설치": Windows 운영 체제에서 사용 하기 위해 설치 된 확장입니다.
    - "WSL: Ubuntu-18.04-Installed": Ubuntu 운영 체제 (WSL)와 함께 사용 하기 위해 설치 된 확장입니다.
    - "권장": 현재 프로젝트의 파일 형식에 따라 VS Code에서 권장 하는 확장명입니다.

    ![로컬 및 원격 확장 VS Code](../images/vscode-extensions-local-remote.png)

2. 확장 창의 맨 위에 있는 검색 상자에 **노드 확장 팩** (또는 찾고 있는 모든 확장의 이름)을 입력 합니다. 현재 프로젝트가 열려 있는 위치에 따라 VS Code의 로컬 또는 WSL 인스턴스에 대해 확장이 설치 됩니다. VS Code 창의 왼쪽 아래 모서리에 있는 원격 링크 (녹색)를 선택 하 여 알 수 있습니다. 원격 연결을 열거나 닫는 옵션이 제공 됩니다. "WSL: Ubuntu-18.04" 환경에 node.js 확장을 설치 합니다.

    ![VS Code 원격 링크](../images/wsl-remote-extension.png)

고려해 야 할 몇 가지 추가 확장은 다음과 같습니다.

- [Chrome 용 디버거](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): node.js를 사용 하 여 서버 쪽에서 개발을 마친 후에는 클라이언트 쪽을 개발 하 고 테스트 해야 합니다. 이 확장은 VS Code 편집기를 Chrome 브라우저 디버깅 서비스와 통합 하 여 좀 더 효율적으로 만듭니다.
- [다른 편집기의 Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): 이러한 확장을 통해 다른 텍스트 편집기에서 전환 하는 경우 (예: Atom, Sublime, Vim, EMacs, 메모장 + + 등) 환경에 적합 합니다.
- [설정 동기화](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub를 사용 하 여 여러 설치에서 VS Code 설정을 동기화 할 수 있습니다. 여러 컴퓨터에서 작업 하는 경우이를 통해 환경을 일관 되 게 유지할 수 있습니다.

## <a name="install-windows-terminal-optional"></a>Windows 터미널 설치 (선택 사항)

새 Windows 터미널을 사용 하면 여러 탭 (명령 프롬프트, PowerShell 또는 여러 Linux 배포 간을 신속 하 게 전환), 사용자 지정 키 바인딩 (열기 또는 닫기 탭에 대 한 바로 가기 키 만들기, 복사 + 붙여넣기 등),이 모 지 ☺ 및 사용자 지정 테마 ( 색 구성표, 글꼴 스타일 및 크기, 배경 이미지/흐림/투명도). [자세한 내용을 알아보세요](https://devblogs.microsoft.com/commandline/).

1. [Microsoft Store에서 Windows 터미널 (미리 보기)](https://www.microsoft.com/store/apps/9n0dx20hk701)가져오기: 스토어를 통해 설치 하면 업데이트가 자동으로 처리 됩니다.

2. 설치 되 면 Windows 터미널을 열고 **설정** 을 선택 하 여 `profile.json` 파일을 사용 하 여 터미널을 사용자 지정 합니다. [Windows 터미널 설정 편집에 대해 자세히 알아보세요](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Windows 터미널 설정](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Git 설정 (선택 사항)

다른 사용자와 공동 작업 하거나, GitHub와 같은 오픈 소스 사이트에서 프로젝트를 호스트 하는 경우 VS Code [Git에서 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원 합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적 하 고 UI에 바로 적용 되는 일반적인 Git 명령 (추가, 커밋, 푸시, 끌어오기)을 포함 합니다.

1. Git은 Linux 배포판에 대 한 Windows 하위 시스템과 함께 설치 되지만 git 구성 파일을 설정 해야 합니다. 이렇게 하려면 터미널에서 `git config --global user.name "Your Name"`을 입력 한 다음-1을 @no__t 합니다. 아직 Git 계정이 없는 경우 [GitHub에서 하나에 등록할](https://github.com/join)수 있습니다. 이전에 Git로 작업 한 적이 없는 경우 [GitHub 가이드](https://guides.github.com/) 를 사용 하 여 시작 하는 데 도움을 받을 수 있습니다. Git config를 편집 해야 하는 경우 nano: `nano ~/.gitconfig`과 같은 기본 제공 텍스트 편집기를 사용 하 여이 작업을 수행할 수 있습니다.

2. 노드 프로젝트에 [.gitignore 파일](https://help.github.com/en/articles/ignoring-files) 을 추가 하는 것이 좋습니다. [Node.js에 대 한 GitHub의 기본 .gitignore 템플릿은](https://github.com/github/gitignore/blob/master/Node.gitignore)다음과 같습니다. [GitHub 웹 사이트를 사용 하 여 새 리포지토리를 만들도록](https://help.github.com/articles/create-a-repo)선택한 경우에는 추가 정보 파일인 .gitignore 파일을 사용 하 여 리포지토리를 초기화 하 고, 필요한 경우 라이선스를 추가 하는 옵션을 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

이제 node.js 개발 환경을 설정 했습니다. Node.js 환경 사용을 시작 하려면 다음 자습서 중 하나를 시도해 보세요.

- [초보자를 위한 node.js 시작](./beginners.md)
- [Windows에서 node.js 웹 프레임 워크 시작](./web-frameworks.md)
- [Node.js 앱을 데이터베이스에 연결 하기 시작](./databases.md)
- [Node.js에서 Docker 컨테이너를 사용 하 여 시작](./containers.md)
