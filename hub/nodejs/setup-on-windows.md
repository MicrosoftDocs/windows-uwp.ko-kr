---
title: 네이티브 Windows에서 NodeJS 설치
description: Windows에 직접 Node.js 개발 환경을 설치하는 방법에 대한 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, 네이티브 windows, windows에 직접
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: fe1943da8c1de4f4fced5dec67079522d83f9a19
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82173469"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Windows에 직접 Node.js 개발 환경 설치

다음은 네이티브 Windows 개발 환경에서 Node.js 사용을 시작하는 방법에 대한 단계별 가이드입니다.

## <a name="install-nvm-windows-nodejs-and-npm"></a>nvm-windows, node.js 및 npm 설치

Node.js를 설치하는 여러 가지 방법이 있습니다. 버전이 매우 빠르게 바뀌므로 버전 관리자를 사용하는 것이 좋습니다. 작업하는 여러 프로젝트의 요구 사항에 따라 여러 버전 간에 전환해야 하는 상황이 많을 것입니다. 흔히 nvm으로 불리는 노드 버전 관리자는 여러 버전의 Node.js를 설치하는 가장 인기 있는 방법이지만, Mac/Linux에만 사용할 수 있으며 Windows에서는 지원되지 않습니다. 그 대신 nvm-windows를 설치하고 이를 사용하여 Node.js 및 npm(노드 패키지 관리자)을 설치하는 단계를 설명하겠습니다. 생각해 볼 수 있는 [또 다른 버전 관리자](#alternative-version-managers)도 있으며, 다음 섹션에서 설명하겠습니다.

> [!IMPORTANT]
> 여러 유형을 설치하면 비정상적이고 혼란스러운 충돌이 발생할 수 있으므로, 항상 운영 체제에 설치된 기존 Node.js 또는 npm을 제거한 후 버전 관리자를 설치하는 것이 좋습니다. 여기에는 혹시 남아 있을 수 있는 기존 nodejs 설치 디렉터리(예: "C:\Program Files\nodejs")를 삭제하는 작업도 포함됩니다. NVM에서 생성한 symlink는 기존(비어 있더라도) 설치 디렉터리를 덮어쓰지 않습니다. 이전 설치를 제거하는 방법에 대한 도움말은 [Windows에서 node.js를 완전히 제거하는 방법](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)을 참조하세요.)

1. 인터넷 브라우저에서 [windows-nvm repository](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows)를 열고 **지금 다운로드** 링크를 선택합니다.
2. 최신 릴리스의 **nvm-setup.zip** 파일을 다운로드합니다.
3. 다운로드한 zip 파일을 연 다음, **nvm-setup.exe** 파일을 엽니다.
4. Setup-NVM-for-Windows 설치 마법사가 nvm-windows 및 Node.js를 설치할 디렉터리를 선택하는 과정을 포함하여 설치 단계를 안내합니다.

    ![Windows용 NVM 설치 마법사](../images/install-nvm-for-windows-wizard.png)

5. 설치가 완료되면 PowerShell을 열고 windows-nvm `nvm ls`를 사용하여 현재 설치된 노드 버전을 나열합니다(지금은 설치된 버전이 없음).

    ![노드 버전을 표시하지 않는 NVM 목록](../images/windows-nvm-powershell-no-node.png)

6. `nvm install latest`를 사용하여 Node.js 최신 릴리스를 설치합니다(최신 기능 개선 사항을 테스트하는 것이 목적이지만 LTS 버전보다 이슈가 있을 가능성이 높음).

7. 먼저 `nvm list available`을 사용하여 현재 LTS 버전 번호를 확인하고 안정적인 Node.js 최신 LTS 릴리스(권장)를 설치한 다음, `nvm install <version>`을 사용하여(`<version>`을 버전 번호로 변경) LTS 버전을 설치합니다(예: `nvm install 12.14.0`).

    ![사용 가능한 NVM 버전 목록](../images/windows-nvm-list.png)

8. `nvm ls`를 사용하여 설치된 노드 버전을 나열합니다. 방금 설치한 두 가지 버전이 표시될 것입니다.

    ![설치된 노드 버전을 표시하는 NVM 목록](../images/windows-nvm-node-installs.png)

9. 필요한 Node.js 버전 번호가 설치되면 `nvm use <version>`을 입력하여 사용할 버전을 선택합니다(`<version>`을 숫자로 바꿈, 즉, `nvm use 12.9.0`).

10. 프로젝트에 사용할 Node.js 버전을 변경하려면 새 프로젝트 디렉터리 `mkdir NodeTest`를 만들고 `cd NodeTest` 디렉터리로 들어간 다음, `nvm use <version>`을 입력하고 `<version>`을 사용하려는 버전 번호로 바꿉니다(예: v10.16.3`).

11. `npm --version`을 사용하여 설치된 npm 버전을 확인합니다. 이 버전 번호는 현재 Node.js 버전과 연결된 npm 버전으로 자동 변경됩니다.

## <a name="alternative-version-managers"></a>대체 버전 관리자

현재 노드에 가장 많이 사용되는 버전 관리자는 windows-nvm이지만, 고려해 볼 수 있는 다음과 같은 다른 버전 관리자도 있습니다.

- [nvs](https://github.com/jasongin/nvs)(노드 버전 전환기)는 플랫폼 간 `nvm` 대안으로, [VS Code와 통합](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)할 수 있는 기능을 제공합니다.

- [Volta](https://github.com/volta-cli/volta#installing-volta)는 LinkedIn 팀의 새로운 버전 관리자로, 속도 및 플랫폼 간 지원이 향상되었다고 합니다.

windows-nvm 대신 Volta를 버전 관리자로 설치하려면 해당하는 [시작 가이드](https://docs.volta.sh/guide/getting-started)의 **Windows 설치** 섹션으로 이동한 다음, Windows 설치 관리자를 다운로드하여 실행합니다. 그리고 설치 지침을 따릅니다.

> [!IMPORTANT]
> Volta를 설치하기 전에 Windows 머신에서 [개발자 모드](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers)를 사용하도록 설정해야 합니다.

Volta를 사용하여 Windows에 여러 버전의 Node.js를 설치하는 방법에 대한 자세한 정보는 [Volta 문서](https://docs.volta.sh/guide/understanding#managing-your-toolchain)를 참조하세요.

## <a name="install-your-favorite-code-editor"></a>선호하는 코드 편집기 설치

Windows에서 Node.js를 사용하여 앱을 개발하려는 경우 [VS Code](https://code.visualstudio.com) 및 [Node.js 확장 팩](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)을 설치하는 것이 좋습니다. 전부 설치해도 되고 가장 유용하다고 생각되는 부분만 선택해서 설치해도 됩니다.

Node.js 확장 팩을 설치하는 방법은 다음과 같습니다.

1. VS Code에서 **확장** 창을 엽니다(Ctrl+Shift+X).
2. [확장] 창의 맨 위에 있는 검색 상자에 "노드 확장 팩"(또는 찾고 있는 확장의 이름)을 입력합니다.
3. **설치**를 선택합니다. 설치가 완료되면 해당 확장이 **확장** 창의 "사용" 폴더에 표시됩니다. 새 확장의 설명 옆에 있는 기어 아이콘을 선택하여 설정을 해제, 제거 또는 구성할 수 있습니다.

다음과 같은 몇 가지 추가 확장도 고려해 볼 수 있습니다.

- [Chrome용 디버거](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): Node.js를 사용하여 서버 쪽에서 개발을 마친 후에는 클라이언트 쪽에서 개발하고 테스트해야 합니다. 이 확장은 VS Code 편집기를 Chrome 브라우저 디버깅 서비스와 통합하여 효율성을 높입니다.
- [다른 편집기의 키맵](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): Atom, Sublime, Vim, eMacs, 메모장++ 등의 다른 텍스트 편집기에서 전환할 때 이러한 확장을 사용하여 익숙한 환경을 만들 수 있습니다.
- [설정 동기화](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub를 사용하는 여러 설치에서 VS Code 설정을 동기화할 수 있습니다. 여러 머신에서 작업하는 경우 이렇게 하면 여러 머신의 환경을 일관되게 유지할 수 있습니다.

## <a name="install-git-optional"></a>Git 설치(선택 사항)

다른 사람과 협업할 계획이거나 GitHub 같은 오픈 소스 사이트에 프로젝트를 호스팅할 계획인 분들을 위해 VS Code는 [Git을 사용한 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적하며, UI에 바로 빌드된 일반적인 Git 명령(추가, 커밋, 푸시, 끌어오기)를 포함하고 있습니다. 소스 제어 패널을 지원하려면 먼저 Git를 설치해야 합니다.

1. [git-scm 웹 사이트 ](https://git-scm.com/download/win)에서 Git for Windows를 다운로드하여 설치합니다.

2. Git 설치의 설정에 대한 일련의 질문을 하는 설치 마법사가 포함되어 있습니다. 기본 설정을 변경해야 하는 특별한 이유가 없다면 모두 기본 설정을 사용하는 것이 좋습니다.

3. 이전에 Git를 사용한 경험이 없는 경우 [GitHub 가이드](https://guides.github.com/)를 보면 시작하는 데 도움이 될 수 있습니다.

4. 노드 프로젝트에 [.gitignore 파일](https://help.github.com/en/articles/ignoring-files)을 추가하는 것이 좋습니다. 다음은 [GitHub의 Node.js용 기본 gitignore 템플릿](https://github.com/github/gitignore/blob/master/Node.gitignore)입니다.

## <a name="use-windows-subsystem-for-linux-for-production"></a>프로덕션에 Linux용 Windows 하위 시스템 사용

Windows에서 직접 Node.js를 사용해 보면 어떤 작업을 수행할 수 있는지 알아보고 실험할 수 있습니다. 즉시 프로덕션에 사용 가능한 웹앱(일반적으로 Linux 기반 서버에 배포됨)을 빌드할 준비가 되면 Node.js 웹앱 개발에 WSL 2(Linux용 Windows 하위 시스템 버전 2)를 사용하는 것이 좋습니다. 많은 Node.js 패키지와 프레임워크는 *nix 환경을 염두에 두고 제작되며 대부분의 Node.js 앱은 Linux에 배포되므로, WSL에서 개발하면 개발 환경과 프로덕션 환경 간에 일관성을 유지할 수 있습니다. WSL 개발 환경을 설치하려면 [WSL 2를 사용하여 Node.js 개발 환경 설치](./setup-on-wsl2.md)를 참조하세요.

> [!NOTE]
> 드물기는 하지만 Node.js 앱을 Windows 서버에 호스팅해야 하는 가장 일반적인 시나리오는 [역방향 프록시를 사용](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)하는 경우입니다. 이렇게 하는 방법은 다음과 같은 두 가지가 있습니다. 1) [iisnode 사용](https://harveywilliams.net/blog/installing-iisnode) 또는 [직접](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). 저희는 이러한 리소스를 유지 관리하지 않으므로 [Linux 서버를 사용하여 Node.js 앱을 호스팅](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)하는 것이 좋습니다.
