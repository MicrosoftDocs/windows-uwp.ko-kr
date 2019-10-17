---
title: 네이티브 창에서 NodeJS 설정
description: Windows에서 직접 node.js 개발 환경을 설정 하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, 네이티브 windows, windows에서 직접
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 18a8d07f790c391a6e10577ff512347106e1cf21
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517833"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Windows에서 직접 node.js 개발 환경 설정

다음은 네이티브 Windows 개발 환경에서 node.js를 사용 하 여 시작 하는 단계별 가이드입니다.

> [!NOTE]
> Windows에서 node.js를 사용 하는 것은 매우 유용한 옵션 이지만 일반적으로 node.js 웹 앱을 개발 하는 데 Linux 용 Windows 하위 시스템 (WSL)을 사용 하는 것이 좋습니다. 많은 node.js 패키지 및 프레임 워크는 * nix 환경을 염두에 두어야 하며 대부분의 node.js 앱은 Linux에 배포 되므로 WSL을 개발 하면 개발 환경과 프로덕션 환경 간에 일관성을 유지할 수 있습니다. WSL dev environment를 설정 하려면 [wsl 2를 사용 하 여 node.js 개발 환경 설정](./setup-on-wsl2.md)을 참조 하세요.

## <a name="install-nvm-windows-nodejs-and-npm"></a>Nvm-windows, node.js 및 npm 설치

Node.js를 설치 하는 방법에는 여러 가지가 있습니다. 버전 변경이 매우 빠르게 변경 되 면 버전 관리자를 사용 하는 것이 좋습니다. 작업 하는 다른 프로젝트의 요구에 따라 여러 버전 간에 전환 해야 할 수도 있습니다. 노드 버전 관리자 (일반적으로 nvm 이라고 함)는 여러 버전의 node.js를 설치 하는 가장 인기 있는 방법 이지만, Mac/Linux에만 사용할 수 있으며 Windows에서 지원 되지 않습니다. 대신 nvm-windows를 설치 하 고이를 사용 하 여 node.js 및 노드 패키지 관리자 (npm)를 설치 하는 단계를 안내 합니다. 다음 섹션에서 설명 하는 것과 같은 [대체 버전 관리자](#alternative-version-managers) 가 있습니다.

> [!IMPORTANT]
> 버전 관리자를 설치 하기 전에 운영 체제에서 node.js 또는 npm의 기존 설치를 제거 하는 것이 좋습니다 .이 경우에는 다른 유형의 설치로 인해 비정상적이 고 혼란 스러운 충돌이 발생할 수 있습니다. 여기에는 남아 있을 수 있는 기존 nodejs 설치 디렉터리 (예: "C:\Program Files\nodejs")를 삭제 하는 작업이 포함 됩니다. NVM의 생성 된 symlink는 기존 (빈) 설치 디렉터리를 덮어쓰지 않습니다. 이전 설치를 제거 하는 데 도움이 필요 하면 [Windows에서 node.js를 완전히 제거 하는 방법](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)을 참조 하세요.

1. 인터넷 브라우저에서 [windows-nvm 리포지토리](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 를 열고 **지금 다운로드** 링크를 선택 합니다.
2. 최신 릴리스에 대 한 **nvm-setup** 파일을 다운로드 합니다.
3. 다운로드 한 후 zip 파일을 열고 **nvm-setup** 파일을 엽니다.
4. NVM-windows 설치 마법사는 nvm-windows 및 node.js가 설치 될 디렉터리를 선택 하는 등의 설정 단계를 안내 합니다.

    ![Windows 용 n VM 설치 마법사](../images/install-nvm-for-windows-wizard.png)

5. 설치가 완료 되 면입니다. PowerShell을 열고 windows-nvm을 사용 하 여 현재 설치 된 노드 버전을 나열 합니다 (이 시점에는 없어야 함). `nvm ls`

    ![노드 버전을 표시 하지 않는 n VM 목록](../images/windows-nvm-powershell-no-node.png)

6. 최신 기능 개선 사항을 테스트 하기 위해 현재 node.js의 릴리스를 설치 하 고 LTS 버전 보다 문제가 있을 가능성이 높습니다. `nvm install latest`
7. 먼저 현재 LTS 버전 번호를 사용 하 여 현재 LTS 버전 번호를 확인 하 고 (권장), 다음을 사용 하 여 LTS 버전 번호를 설치 합니다. (권장): `nvm install <version>` (`<version>`를 숫자로 바꿈 `nvm install 10.16.3`)을 (를) @no__t.

    ![사용 가능한 버전의 n v m 목록](../images/windows-nvm-list.png)

8. 설치 된 노드 버전 나열: `nvm ls` ... 이제 방금 설치한 두 버전이 표시 됩니다.

    ![설치 된 노드 버전을 보여 주는 NVM 목록](../images/windows-nvm-node-installs.png)

9. 현재 node.js 버전이 기본값 인지 확인 하려면 다음을 입력 합니다. `node --version`
10. 프로젝트에 사용 하려는 node.js의 버전을 변경 하려면 새 프로젝트 디렉터리 `mkdir NodeTest`을 만들고 @no__t 디렉터리를 입력 한 다음-3 `<version>`을 사용할 버전 번호 (ie v 10.16.3 ')로 바꿉니다 @no__t을 입력 합니다.
11. @No__t-0과 함께 설치 된 npm 버전을 확인 합니다 .이 버전 번호는 현재 node.js 버전과 연결 된 npm 버전으로 자동 변경 됩니다.

## <a name="alternative-version-managers"></a>대체 버전 관리자

Windows-nvm은 현재 노드당 가장 인기 있는 버전 관리자입니다.

- [nvs](https://github.com/jasongin/nvs) (Node 버전 전환기)는 [VS Code와 통합할](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)수 있는 플랫폼 간 `nvm` 대안입니다.
- 
- [Volta](https://github.com/volta-cli/volta#installing-volta) 는 향상 된 속도 및 플랫폼 간 지원을 제공 하는 LinkedIn 팀의 새로운 버전 관리자입니다.

Volta를 버전 관리자로 설치 하려면 [시작 가이드](https://docs.volta.sh/guide/getting-started)의 **windows 설치** 섹션으로 이동한 다음 설치 지침에 따라 windows installer를 다운로드 하 여 실행 합니다.

> [!IMPORTANT]
> Volta를 설치 하기 전에 Windows 컴퓨터에서 [개발자 모드](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) 를 사용 하도록 설정 했는지 확인 해야 합니다.

Volta를 사용 하 여 Windows에 여러 버전의 node.js를 설치 하는 방법에 대 한 자세한 내용은 [Volta 문서](https://docs.volta.sh/guide/understanding#managing-your-toolchain)를 참조 하세요.

## <a name="install-your-favorite-code-editor"></a>즐겨 찾는 코드 편집기 설치

Windows에서 node.js를 사용 하 여 개발 하는 데에는 VS Code 및 [Node.js 확장 팩](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)을 [설치](https://code.visualstudio.com)하는 것이 좋습니다. 모두 설치 하거나 선택 하 고 가장 유용 하다 고 생각 하는 선택 합니다.

Node.js 확장 팩을 설치 하려면:

1. VS Code에서 **확장** 창 (Ctrl + Shift + X)을 엽니다.
2. 확장 창의 맨 위에 있는 검색 상자에 "노드 확장 팩" (또는 찾고 있는 모든 확장의 이름)을 입력 합니다.
3. **설치**를 선택 합니다. 설치가 끝나면 확장이 **확장** 창의 "사용" 폴더에 표시 됩니다. 새 확장의 설명 옆에 있는 기어 아이콘을 선택 하 여 설정을 사용 하지 않도록 설정 하거나, 제거 하거나, 구성할 수 있습니다.

고려해 야 할 몇 가지 추가 확장은 다음과 같습니다.

- [Chrome 용 디버거](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): node.js를 사용 하 여 서버 쪽에서 개발을 마친 후에는 클라이언트 쪽을 개발 하 고 테스트 해야 합니다. 이 확장은 VS Code 편집기를 Chrome 브라우저 디버깅 서비스와 통합 하 여 좀 더 효율적으로 만듭니다.
- [다른 편집기의 Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): 이러한 확장을 통해 다른 텍스트 편집기에서 전환 하는 경우 (예: Atom, Sublime, Vim, EMacs, 메모장 + + 등) 환경에 적합 합니다.
- [설정 동기화](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub를 사용 하 여 여러 설치에서 VS Code 설정을 동기화 할 수 있습니다. 여러 컴퓨터에서 작업 하는 경우이를 통해 환경을 일관 되 게 유지할 수 있습니다.

## <a name="install-git-optional"></a>Git 설치 (선택 사항)

다른 사용자와 공동 작업 하거나, GitHub와 같은 오픈 소스 사이트에서 프로젝트를 호스트 하는 경우 VS Code [Git에서 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원 합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적 하 고 UI에 바로 적용 되는 일반적인 Git 명령 (추가, 커밋, 푸시, 끌어오기)을 포함 합니다. 먼저 Git를 설치 하 여 원본 제어판을 켭니다.

1. Git [scm 웹 사이트](https://git-scm.com/download/win)에서 Windows 용 git를 다운로드 하 여 설치 합니다.

2. Git 설치의 설정에 대 한 일련의 질문을 하는 설치 마법사가 포함 되어 있습니다. 항목을 변경 해야 하는 특별 한 이유가 없다면 모든 기본 설정을 사용 하는 것이 좋습니다.

3. 이전에 Git로 작업 한 적이 없는 경우 [GitHub 가이드](https://guides.github.com/) 를 사용 하 여 시작 하는 데 도움을 받을 수 있습니다.

4. 노드 프로젝트에 [.gitignore 파일](https://help.github.com/en/articles/ignoring-files) 을 추가 하는 것이 좋습니다. [Node.js에 대 한 GitHub의 기본 .gitignore 템플릿은](https://github.com/github/gitignore/blob/master/Node.gitignore)다음과 같습니다.