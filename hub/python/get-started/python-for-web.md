---
title: Windows에서 Python을 사용한 웹 개발
description: Flask 및 Django와 같은 프레임 워크에 대 한 설정을 포함 하 여 Windows에서 웹 개발을 위한 Python 사용을 시작 하는 방법입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, windows의 python, wsl이 포함 된 python 웹 앱, windows의 python 웹 개발, windows에서 flask 앱, windows의 django 앱, python 웹, windows의 flask 웹 개발자, django web dev in windows python, vs code python 웹 개발, 원격 wsl 확장, ubuntu, wsl, venv, pip, microsoft python 확장, windows에서 python 실행, windows에서 python 사용, windows에서 python으로 빌드
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: fa6da9f5151d9457aafd255c9d10c91e3d219cee
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959080"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Windows에서 웹 개발을 위한 Python 사용 시작

다음은 Linux 용 Windows 하위 시스템 (WSL)을 사용 하 여 Windows에서 웹 개발을 위한 Python 사용을 시작 하는 단계별 가이드입니다.

## <a name="set-up-your-development-environment"></a>개발 환경 설정

웹 응용 프로그램을 빌드할 때 WSL에 Python을 설치 하는 것이 좋습니다. Python 웹 개발에 대 한 다양 한 자습서와 지침은 Linux 사용자를 위해 작성 되었으며 Linux 기반 패키징 및 설치 도구를 사용 합니다. 대부분의 웹 앱은 Linux에도 배포 되므로 개발 환경과 프로덕션 환경 간에 일관성을 유지할 수 있습니다.

웹 개발 이외의 다른 항목에 Python을 사용 하는 경우 Microsoft Store를 사용 하 여 Windows 10에 직접 Python을 설치 하는 것이 좋습니다. WSL은 GUI 데스크톱 또는 응용 프로그램 (예: PyGame, Gnome, KDE 등)을 지원 하지 않습니다. 이러한 경우 Windows에서 직접 Python을 설치 하 고 사용 합니다. Python을 처음 접하는 경우 가이드를 참조 하세요. [초보자를 위해 Windows에서 Python을 사용 하 여 시작](./python-for-education.md)하세요. 운영 체제에서 일반적인 작업을 자동화 하는 데 관심이 있는 경우 다음 가이드를 참조 하세요. [Windows에서 Python을 사용 하 여 스크립팅 및 자동화를 시작](./python-for-scripting.md)합니다. 일부 고급 시나리오의 경우 [python.org](https://www.python.org/downloads/windows/) 에서 직접 특정 Python 릴리스를 다운로드 하거나 Anaconda, Jython, PyPy, Winpython, IronPython 등의 [대안](https://www.python.org/download/alternatives)을 설치 하는 것을 고려해 볼 수 있습니다. 대체 구현을 선택 하는 특별 한 이유가 있는 고급 Python 프로그래머 인 경우에만이를 권장 합니다.

## <a name="enable-windows-subsystem-for-linux"></a>Linux 용 Windows 하위 시스템 사용

WSL을 사용 하면 대부분의 명령줄 도구, 유틸리티 및 응용 프로그램을 포함 하는 GNU/Linux 환경을 Windows에서 직접 실행할 수 있으며, 수정 되지 않고 Windows 파일 시스템 및 Visual Studio Code 같은 즐겨 사용 하는 도구와 완전히 통합 됩니다. WSL을 사용 하도록 설정 하기 전에 [최신 버전의 Windows 10](https://www.microsoft.com/software-download/windows10)이 있는지 확인 하세요.

컴퓨터에서 WSL을 사용 하도록 설정 하려면 다음을 수행 해야 합니다.

1. **시작** 메뉴 (왼쪽 아래에 있는 windows 아이콘)로 이동 하 고 "Windows 기능 사용/사용 안 함"을 입력 한 다음 **제어판** 의 링크를 선택 하 여 **windows 기능** 팝업 메뉴를 엽니다. 목록에서 "Linux 용 Windows 하위 시스템"을 찾고 확인란을 선택 하 여 기능을 켭니다.

2. 메시지가 표시 되 면 컴퓨터를 다시 시작 합니다.

## <a name="install-a-linux-distribution"></a>Linux 배포 설치

WSL에서 실행할 수 있는 몇 가지 Linux 배포판이 있습니다. Microsoft Store에서 즐겨찾기를 찾아 설치할 수 있습니다. [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 는 최신의 인기 있고 잘 지원 되는 것으로 시작 하는 것이 좋습니다.

1. 이 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 링크를 열고 Microsoft Store를 연 다음 **가져오기**를 선택 합니다. *(이것은 매우 큼 다운로드 이며 설치 하는 데 약간의 시간이 걸릴 수 있습니다.)*

2. 다운로드가 완료 되 면 Microsoft Store에서 **시작** 을 선택 하거나 **시작** 메뉴에 "Ubuntu 18.04 lts"를 입력 하 여 시작 합니다.

3. 처음으로 배포를 실행할 때 계정 이름 및 암호를 만들라는 메시지가 표시 됩니다. 그 후에는 기본적으로이 사용자로 자동 로그인 됩니다. 사용자 이름 및 암호를 선택할 수 있습니다. Windows 사용자 이름에는 영향을 미치지 않습니다.

을 입력 `lsb_release -d`하 여 현재 사용 중인 Linux 배포를 확인할 수 있습니다. Ubuntu 배포를 업데이트 하려면 다음 `sudo apt update && sudo apt upgrade`을 사용 합니다. 최신 패키지를 유지 하기 위해 정기적으로 업데이트 하는 것이 좋습니다. 이 업데이트는 Windows에서 자동으로 처리 되지 않습니다. Microsoft Store, 대체 설치 방법 또는 문제 해결에서 사용할 수 있는 다른 Linux 배포에 대 한 링크는 windows [10 용 Windows 하위 시스템 설치 가이드](https://docs.microsoft.com/windows/wsl/install-win10)를 참조 하세요.

## <a name="set-up-visual-studio-code"></a>Visual Studio Code 설정

VS Code를 사용 하 여 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), [lint](https://code.visualstudio.com/docs/python/linting), [디버그 지원](https://code.visualstudio.com/docs/python/debugging), [코드 조각](https://code.visualstudio.com/docs/editor/userdefinedsnippets)및 [단위 테스트](https://code.visualstudio.com/docs/python/unit-testing) 를 활용 합니다. VS Code은 Linux 용 Windows 하위 시스템에 원활 하 게 통합 되며, 코드 편집기와 명령줄 사이에 원활한 워크플로를 설정 하는 [기본 제공 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal) 을 제공 하 고, 일반 git에서 [버전 제어를 위해 git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) 를 지원 합니다. UI에 바로 빌드된 명령 (추가, 커밋, 푸시, 끌어오기)

1. [Windows 용 VS Code를 다운로드 하 고 설치](https://code.visualstudio.com)합니다. VS Code은 Linux 에서도 사용할 수 있지만 Linux 용 Windows 하위 시스템은 GUI 앱을 지원 하지 않으므로 Windows에 설치 해야 합니다. 걱정 하지 마세요. 여전히 원격-WSL 확장을 사용 하 여 Linux 명령줄 및 도구와 통합할 수 있습니다.

2. VS Code에 [원격-WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) 을 설치 합니다. 이를 통해 사용자는 통합 개발 환경으로 WSL을 사용 하 고 호환성 및 pathing를 처리할 수 있습니다. [자세한 내용을 알아보십시오](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> VS Code 이미 설치 되어 있는 경우에는 [원격-WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)을 설치 하기 위해 [1.35 이상 버전을 릴리스할 수](https://code.visualstudio.com/updates/v1_35) 있는지 확인 해야 합니다. 자동 완성, 디버깅, lint 등에 대 한 지원이 손실 될 것 이므로, 원격-WSL 확장이 없으면 VS Code에서 WSL을 사용 하지 않는 것이 좋습니다. 흥미로운 사실: 이 WSL 확장은 $HOME/.vscode-server/extensions.에 설치 됩니다.

## <a name="create-a-new-project"></a>새 프로젝트 만들기

Linux (Ubuntu) 파일 시스템에 새 프로젝트 디렉터리를 만들어 VS Code를 사용 하 여 Linux 앱 및 도구에서 작업할 수 있습니다.

1. **시작** 메뉴 (왼쪽 아래에 있는 Windows 아이콘)로 이동 하 고 다음을 입력 하 여 VS Code를 닫고 Ubuntu 18.04 (wsl 명령줄)를 엽니다. "Ubuntu 18.04".

2. Ubuntu 명령줄에서 프로젝트를 배치할 위치로 이동 하 고 해당 `mkdir HelloWorld`디렉터리를 만듭니다.

![Ubuntu 터미널](../../images/ubuntu-terminal.png)

> [!TIP]
> WSL (Linux 용 Windows 하위 시스템)을 사용할 때 기억해 야 할 중요 한 사항은 다음과 같이 **서로 다른 두 파일 시스템 간에 작업**하는 것입니다. 1) Windows 파일 시스템 및 2) 예제를 위한 Ubuntu 인 WSL (Linux 파일 시스템) 패키지를 설치 하 고 파일을 저장 하는 위치에 주의 해야 합니다. Windows 파일 시스템에는 도구 또는 패키지의 한 버전을 설치 하 고, Linux 파일 시스템에는 완전히 다른 버전을 설치할 수 있습니다. Windows 파일 시스템에서 도구를 업데이트 해도 Linux 파일 시스템의 도구에는 영향을 주지 않으며 그 반대의 경우도 마찬가지입니다. Wsl은 Linux 배포판의/mnt/<drive> 폴더 아래에 있는 컴퓨터의 고정 드라이브를 탑재 합니다. 예를 들어 Windows C: 드라이브는에 탑재 `/mnt/c/`되어 있습니다. Ubuntu 터미널에서 Windows 파일에 액세스 하 고 해당 파일에서 Linux 앱 및 도구를 사용할 수 있으며 그 반대의 경우도 마찬가지입니다. 대부분의 웹 도구는 처음에 Linux 용으로 작성 되 고 Linux 프로덕션 환경에 배포 되는 경우 Python 웹 개발용 Linux 파일 시스템에서 작업 하는 것이 좋습니다. 또한 파일 시스템 의미 체계를 혼합 하지 않습니다 (예: 파일 이름에 대 한 대/소문자를 구분 하지 않음). 즉, WSL은 이제 Linux 및 Windows 파일 시스템 간에 이동 하는 것을 지원 하므로 하나에서 파일을 호스트할 수 있습니다. [자세한 내용을 알아보십시오](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/). 또한 [WSL2가 Windows에 곧](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) 제공 될 예정 이며 몇 가지 향상 된 기능을 제공 합니다. [이제 Windows 참가자 빌드 18917에서 사용해](https://docs.microsoft.com/windows/wsl/wsl2-install)볼 수 있습니다.

## <a name="install-python-pip-and-venv"></a>Python, pip 및 venv 설치

Ubuntu 18.04 LTS는 이미 설치 된 Python 3.6과 함께 제공 되지만, 다른 Python 설치를 사용 하 여 얻을 수 있는 일부 모듈은 제공 되지 않습니다. 경량 가상 환경을 만들고 관리 하는 데 사용 되는 표준 모듈인 **pip**, Python용 표준 패키지 관리자를 설치 해야 합니다.  

1. Ubuntu 터미널을 열고 다음 `python3 --version`을 입력 하 여 Python3이 이미 설치 되어 있는지 확인 합니다. Python 버전 번호를 반환 해야 합니다. Python 버전을 업데이트 해야 하는 경우 먼저 다음 `sudo apt update && sudo apt upgrade`을 입력 하 여 Ubuntu 버전을 업데이트 한 다음를 사용 하 여 `sudo apt upgrade python3`python을 업데이트 합니다.

2. `sudo apt install python3-pip`을 입력 하 여 **pip** 를 설치 합니다. Pip를 사용 하면 Python 표준 라이브러리에 포함 되지 않은 추가 패키지를 설치 하 고 관리할 수 있습니다.

3. 다음`sudo apt install python3-venv`을 입력 하 여 **venv** 를 설치 합니다.

## <a name="create-a-virtual-environment"></a>가상 환경 만들기

가상 환경을 사용 하는 것은 Python 개발 프로젝트에 권장 되는 모범 사례입니다. 가상 환경을 만들면 프로젝트 도구를 분리 하 고 다른 프로젝트에 대 한 도구와의 버전 충돌을 방지할 수 있습니다. 예를 들어 Django 1.2 웹 프레임 워크를 필요로 하는 이전 웹 프로젝트를 유지 관리할 수 있지만 Django 2.2을 사용 하 여 흥미로운 새 프로젝트가 제공 됩니다. 가상 환경 외부에서 Django를 전역적으로 업데이트 하는 경우 나중에 일부 버전 관리 문제가 발생할 수 있습니다. 가상 환경에서는 실수로 인 한 버전 관리 충돌을 방지 하는 것 외에도 관리 권한 없이 패키지를 설치 하 고 관리할 수 있습니다.

1. 터미널을 열고 *HelloWorld* 프로젝트 폴더 내에서 다음 명령을 사용 하 여 이름이 **. venv**: `python3 -m venv .venv`인 가상 환경을 만듭니다.

2. 가상 환경을 활성화 하려면 다음 `source .venv/bin/activate`을 입력 합니다. 정상적으로 작동 하는 경우 명령 프롬프트 앞에 **(venv)** 가 표시 되어야 합니다. 이제 코드를 작성 하 고 패키지를 설치할 수 있는 자체 포함 환경이 준비 되었습니다. 가상 환경에서 작업 `deactivate`을 마친 후 다음 명령을 입력 하 여 비활성화 합니다.

    ![가상 환경 만들기](../../images/wsl-venv.png)

> [!TIP]
> 프로젝트를 만들려는 디렉터리 내에 가상 환경을 만드는 것이 좋습니다. 각 프로젝트에는 자체의 고유한 디렉터리가 있어야 하므로 각 프로젝트에는 고유한 가상 환경이 있으므로 고유한 이름을 지정할 필요가 없습니다. 여기서는 이름 **. venv** 를 사용 하 여 Python 규칙을 따르는 것이 좋습니다. Pipenv와 같은 일부 도구는 프로젝트 디렉터리에를 설치 하는 경우에도 기본적으로이 이름으로 표시 됩니다. 환경 변수 정의 파일과 충돌 하는 경우에는 **env** 를 사용 하지 않는 것이 좋습니다. 디렉터리가 존재 한다는 것을 지속적으로 확인할 필요가 `ls` 없으므로 일반적으로는 점으로 구분 되지 않는 이름을 권장 하지 않습니다. 또한. **venv** 를 .gitignore 파일에 추가 하는 것이 좋습니다. (참조를 위해 [Python 용 GitHub의 기본 .gitignore 템플릿이](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) 있습니다.) VS Code에서 가상 환경을 사용 하는 방법에 대 한 자세한 내용은 [VS Code에서 Python 환경 사용](https://code.visualstudio.com/docs/python/environments)을 참조 하세요.

## <a name="open-a-wsl---remote-window"></a>WSL-원격 창 열기

VS Code은 원격-WSL 확장 (이전에 설치 됨)을 사용 하 여 Linux 하위 시스템을 원격 서버로 처리 합니다. 이를 통해 통합 개발 환경으로 WSL을 사용할 수 있습니다. [자세한 내용을 알아보십시오](https://code.visualstudio.com/docs/remote/wsl). 

1. 다음 `code .` 을 입력 하 여 Ubuntu 터미널에서 VS Code의 프로젝트 폴더를 엽니다. ("."는 VS Code에 게 현재 폴더를 열도록 지시 합니다).

2. Windows Defender에서 보안 경고가 표시 되 면 "액세스 허용"을 선택 합니다. VS Code 열리면 원격 연결 호스트 표시기가 왼쪽 아래 모서리에 표시 되어 wsl에서 **편집 하 고 있음을 알 수 있습니다. Ubuntu-18.04**.

    ![VS Code 원격 연결 호스트 표시기](../../images/wsl-remote-extension.png)

3. Ubuntu 터미널을 닫습니다. 앞으로 이동 하면 VS Code에 통합 된 WSL 터미널을 사용 합니다.

4. **Ctrl + '** (억음 문자 사용)를 누르거나**터미널** **보기** > 를 선택 하 여 VS Code에서 wsl 터미널을 엽니다. Ubuntu 터미널에서 만든 프로젝트 폴더 경로에 대해 bash (WSL) 명령줄이 열립니다.

    ![WSL 터미널을 사용 하 여 VS Code](../../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 확장 설치

VS Code 확장은 원격 WSL에 대해 설치 해야 합니다. 이미 VS Code에 로컬로 설치 된 확장은 자동으로 사용할 수 없습니다. [자세한 내용을 알아보십시오](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. **Ctrl + Shift + X** 를 눌러 VS Code 확장 창을 열거나 메뉴를 사용 하 여**확장** **보기** > 로 이동 합니다.

2. **Marketplace의 위쪽 검색 확장** 상자에 다음을 입력 합니다.  **Python**.

3. Microsoft extension **에서 python (python)** 을 찾고 녹색 **설치** 단추를 선택 합니다.

4. 확장 설치가 완료 되 면 파란색 **다시 로드 필요** 단추를 선택 해야 합니다. VS Code를 다시 로드 하 고 wsl을 **표시 합니다. UBUNTU-18.04-VS Code** 확장 창에 설치 된 섹션이 며 Python 확장을 설치 했다는 것을 보여 줍니다.

## <a name="run-a-simple-python-program"></a>간단한 Python 프로그램 실행

Python은 해석 된 언어 이며 다양 한 유형의 interpretors (Python2, Anaconda, PyPy 등)를 지원 합니다. VS Code는 프로젝트와 연결 된 인터프리터를 기본값으로 지정 해야 합니다. 변경 해야 하는 이유가 있는 경우 VS Code 창의 아래쪽에 파란색 막대에 현재 표시 된 인터프리터를 선택 하거나 **명령 팔레트** (Ctrl + Shift + P)를 열고 명령 **Python을 입력 합니다. 인터프리터**를 선택 합니다. 현재 설치 된 Python 인터프리터의 목록이 표시 됩니다. [Python 환경 구성에 대해 자세히 알아보세요](https://code.visualstudio.com/docs/python/environments).

간단한 Python 프로그램을 만들어 테스트로 실행 하 고 올바른 Python 인터프리터를 선택 했는지 확인해 보겠습니다.

1. **Ctrl + Shift + E** 를 눌러 VS Code 파일 탐색기 창을 열거나 메뉴를 사용 하 여 **보기** > **탐색기**로 이동 합니다.

2. 아직 열려 있지 않은 경우 **Ctrl + Shift + '** 를 입력 하 여 통합 wsl 터미널을 열고 **HelloWorld** python 프로젝트 폴더가 선택 되어 있는지 확인 합니다.

3. 다음 `touch test.py`을 입력 하 여 python 파일을 만듭니다. 방금 만든 파일이 프로젝트 디렉터리에 이미 있는. venv 및 vscode 폴더의 탐색기 창에 표시 됩니다.

4. 탐색기 창에서 방금 만든 **test.py** 파일을 선택 하 여 VS Code에서 엽니다. 파일 이름에 있는 py는 Python 파일 임을 VS Code에 이전에 로드 한 Python 확장 프로그램은 VS Code 창의 아래쪽에 표시 되는 Python 인터프리터를 자동으로 선택 하 고 로드 합니다.

    ![VS Code에서 Python 인터프리터를 선택 합니다.](../../images/interpreterselection.gif)

5. 이 Python 코드를 test.py 파일에 붙여넣고 파일을 저장 합니다 (Ctrl + S). 

    ```python
    print("Hello World")
    ```

6. 방금 만든 Python "Hello World" 프로그램을 실행 하려면 VS Code 탐색기 창에서 **test.py** 파일을 선택한 다음 파일을 마우스 오른쪽 단추로 클릭 하 여 옵션 메뉴를 표시 합니다. **터미널에서 Python 파일 실행**을 선택 합니다. 또는 통합 wsl 터미널 창에서 다음 `python test.py` 을 입력 하 여 "Hello World" 프로그램을 실행 합니다. Python 인터프리터는 터미널 창에서 "Hello World"를 인쇄 합니다.

집들이. Python 프로그램을 만들고 실행 하기 위한 모든 설정이 완료 되었습니다. 이제 가장 인기 있는 두 가지 Python 웹 프레임 워크를 사용 하 여 Hello World 앱을 만들어 보겠습니다. Flask 및 Django.

## <a name="hello-world-tutorial-for-flask"></a>Flask에 대 한 Hello World 자습서

[Flask](http://flask.pocoo.org/) 는 Python 용 웹 응용 프로그램 프레임 워크입니다. 이 간략 한 자습서에서는 VS Code 및 WSL을 사용 하 여 작은 "Hello World" Flask 앱을 만듭니다.

1. **시작** 메뉴 (왼쪽 아래에 있는 Windows 아이콘)로 이동 하 여 다음을 입력 하 여 Ubuntu 18.04 (wsl 명령줄)를 엽니다. "Ubuntu 18.04".

2. `mkdir HelloWorld-Flask` 프로젝트`cd HelloWorld-Flask` 에 대 한 디렉터리를 만들고 디렉터리를 입력 합니다.

3. 프로젝트 도구를 설치 하는 가상 환경을 만듭니다.`python3 -m venv .venv`

4. 명령을 입력 하 여 VS Code에서 **HelloWorld-Flask** 프로젝트를 엽니다.`code .`

5. VS Code 내에서 **Ctrl + Shift + '** 를 입력 하 여 통합 wsl 터미널 (즉, Bash)을 엽니다 ( **HelloWorld-flask** 프로젝트 폴더가 이미 선택 되어 있어야 함). *앞으로 VS Code와 통합 된 WSL 터미널에서 작업할 때 Ubuntu 명령줄을 닫습니다.*

6. VS Code에서 Bash 터미널을 사용 하 여 #3 단계에서 만든 가상 환경을 활성화 `source .venv/bin/activate`합니다. 정상적으로 작동 하는 경우 명령 프롬프트 앞에 (venv)가 표시 되어야 합니다.

7. 다음 `python3 -m pip install flask`을 입력 하 여 가상 환경에서 flask를 설치 합니다. 다음 `python3 -m flask --version`을 입력 하 여 설치 되어 있는지 확인 합니다.

8. Python 코드에 대 한 새 파일을 만듭니다.`touch app.py`

9. VS Code의 파일 탐색기`Ctrl+Shift+E`에서 app.py 파일을 연 다음 app.py 파일을 선택 합니다. 이렇게 하면 Python 확장을 활성화 하 여 인터프리터를 선택 합니다. **Python 3.6.8 64 비트 ('. venv ': venv)** 로 기본값을 지정 해야 합니다. 가상 환경도 검색 되었습니다.

    ![활성화 된 가상 환경](../../images/virtual-environment.png)

10. **App.py**에서 flask를 가져오는 코드를 추가 하 고 flask 개체의 인스턴스를 만듭니다.

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. 또한 **app.py**에서 콘텐츠를 반환 하는 함수를 추가 합니다 .이 경우에는 간단한 문자열입니다. Flask의 **app. 경로** 데코레이터를 사용 하 여 URL 경로 "/"를 해당 함수에 매핑합니다.

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 동일한 함수에 매핑할 여러 경로 수에 따라 한 줄에 하나씩 동일한 함수에서 여러 데코레이터를 사용할 수 있습니다.

12. **App.py** 파일을 저장 합니다 (**Ctrl + S**).

13. 터미널에서 다음 명령을 입력 하 여 앱을 실행 합니다.

    ```python
    python3 -m flask run
    ```

    그러면 Flask 개발 서버가 실행 됩니다. 개발 서버는 기본적으로 **app.py** 를 찾습니다. Flask를 실행 하면 다음과 유사한 출력이 표시 됩니다.

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 렌더링 된 페이지에서 기본 웹 브라우저를 열고 터미널에서 http://127.0.0.1:5000/ URL을 **Ctrl + 클릭** 합니다. 브라우저에서 다음 메시지가 표시 됩니다.

    ![Hello, Flask!](../../images/hello-flask.png)

15. "/"와 같은 URL을 방문할 때 HTTP 요청을 보여 주는 메시지가 디버그 터미널에 표시 되는지 확인 합니다.

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. 터미널에서 **Ctrl + C** 를 사용 하 여 앱을 중지 합니다.

> [!TIP]
> **Program.py**와 같은 **app.py**와 다른 파일 이름을 사용 하려는 경우 **FLASK_APP** 이라는 환경 변수를 정의 하 고 해당 값을 선택한 파일로 설정 합니다. Flask의 개발 서버는 기본 파일 **app.py**대신 **FLASK_APP** 값을 사용 합니다. 자세한 내용은 [Flask의 명령줄 인터페이스 설명서](http://flask.pocoo.org/docs/1.0/cli/)를 참조 하세요.

축 하 합니다. Visual Studio Code 및 Linux 용 Windows 하위 시스템을 사용 하 여 Flask 웹 응용 프로그램을 만들었습니다. VS Code 및 Flask를 사용 하는 자세한 자습서는 [Visual Studio Code에서 Flask 자습서](https://code.visualstudio.com/docs/python/tutorial-flask)를 참조 하세요.

## <a name="hello-world-tutorial-for-django"></a>Django에 대 한 Hello World 자습서

[Django](https://www.djangoproject.com) 는 Python 용 웹 응용 프로그램 프레임 워크입니다. 이 간략 한 자습서에서는 VS Code 및 WSL을 사용 하 여 작은 "Hello World" Django 앱을 만듭니다.

1. **시작** 메뉴 (왼쪽 아래에 있는 Windows 아이콘)로 이동 하 여 다음을 입력 하 여 Ubuntu 18.04 (wsl 명령줄)를 엽니다. "Ubuntu 18.04".

2. `mkdir HelloWorld-Django` 프로젝트`cd HelloWorld-Django` 에 대 한 디렉터리를 만들고 디렉터리를 입력 합니다.

3. 프로젝트 도구를 설치 하는 가상 환경을 만듭니다.`python3 -m venv .venv`

4. 명령을 입력 하 여 VS Code에서 **HelloWorld DJango** 프로젝트를 엽니다.`code .`

5. VS Code 내에서 **Ctrl + Shift + '** ( **HelloWorld-Django** project 폴더가 이미 선택 되어 있어야 함)를 입력 하 여 통합 wsl 터미널 (즉, Bash)을 엽니다. *앞으로 VS Code와 통합 된 WSL 터미널에서 작업할 때 Ubuntu 명령줄을 닫습니다.*

6. VS Code에서 Bash 터미널을 사용 하 여 #3 단계에서 만든 가상 환경을 활성화 `source .venv/bin/activate`합니다. 정상적으로 작동 하는 경우 명령 프롬프트 앞에 (venv)가 표시 되어야 합니다.

7. 명령을 사용 하 여 가상 환경에 Django를 설치 `python3 -m pip install django`합니다. 다음 `python3 -m django --version`을 입력 하 여 설치 되어 있는지 확인 합니다.

8. 다음 명령을 실행 하 여 Django 프로젝트를 만듭니다.

    ```bash
    django-admin startproject web_project .
    ```

    명령은 끝에를 `.` 사용 하 여 현재 폴더를 프로젝트 폴더로 가정 하 고 그 안에 다음을 만듭니다. `startproject`

    - `manage.py`: 프로젝트에 대 한 Django 명령줄 관리 유틸리티입니다. 를 사용 하 여 `python manage.py <command> [options]`프로젝트에 대 한 관리 명령을 실행 합니다.

    - 다음 파일을 `web_project`포함 하는 라는 하위 폴더입니다.
        - `__init__.py`: Python에이 폴더가 Python 패키지 임을 알려주는 빈 파일입니다.
        - `wsgi.py`: 프로젝트를 제공 하기 위해 WSGI 호환 웹 서버에 대 한 진입점입니다. 일반적으로이 파일은 프로덕션 웹 서버에 대 한 후크를 제공 하는 그대로 둡니다.
        - `settings.py`: 웹 앱을 개발 하는 과정에서 수정 하는 Django 프로젝트에 대 한 설정을 포함 합니다.
        - `urls.py`: Django 프로젝트에 대 한 목차를 포함 하며, 개발 과정 에서도이를 수정 합니다.

9. Django 프로젝트를 확인 하려면 명령을 `python3 manage.py runserver`사용 하 여 Django의 development server를 시작 합니다. 서버는 기본 포트 8000에서 실행 되 고 터미널 창에 다음 출력과 같은 출력이 표시 됩니다.

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    서버를 처음 실행 하는 경우이 파일 `db.sqlite3`에는 개발 목적으로 사용 되는 기본 SQLite 데이터베이스가 생성 되지만, 낮은 볼륨 웹 앱의 경우 프로덕션 환경에서 사용할 수 있습니다. 또한 Django의 기본 제공 웹 서버는 로컬 개발 목적 *으로만* 사용 됩니다. 그러나를 웹 호스트에 배포 하는 경우 Django는 호스트의 웹 서버를 대신 사용 합니다. Django `wsgi.py` 프로젝트의 모듈은 프로덕션 서버에 연결 하는 작업을 처리 합니다.

    기본 8000이 아닌 다른 포트를 사용 하려면 명령줄에서 포트 번호를 지정 `python3 manage.py runserver 5000`합니다 (예:).

10. `Ctrl+click`터미널 `http://127.0.0.1:8000/` 출력 창의 URL을 통해 해당 주소로 기본 브라우저를 엽니다. Django가 올바르게 설치 되어 있고 프로젝트가 올바르면 기본 페이지가 표시 됩니다. VS Code 터미널 출력 창에도 서버 로그가 표시 됩니다.

11. 완료 되 면 브라우저 창을 닫고 터미널 출력 창에 표시 된 대로를 사용 하 `Ctrl+C` 여 VS Code에서 서버를 중지 합니다.

12. 이제 Django 앱을 만들려면 프로젝트 폴더에서 관리 유틸리티의 `startapp` 명령 (있는 경우 `manage.py` )을 실행 합니다.

    ```bash
    python3 manage.py startapp hello
    ```

    명령은 여러 개의 코드 파일과 하나의 `hello` 하위 폴더를 포함 하는 라는 폴더를 만듭니다. 이러한 작업 중에는 (웹 `views.py` 앱에서 페이지를 정의 하는 함수가 포함 된) 및 `models.py` (데이터 개체를 정의 하는 클래스가 포함 된)으로 작업 하는 경우가 많습니다. 이 `migrations` 폴더는 Django의 관리 유틸리티에서이 자습서의 뒷부분에 설명 된 대로 데이터베이스 버전을 관리 하는 데 사용 됩니다. 여기에 포함 되지 않은 `apps.py` 파일 (앱 구성) `admin.py` , (관리 인터페이스를 만드는 경우) 및 `tests.py` (테스트의 경우)도 있습니다.

13. 앱 `hello/views.py` 의 홈 페이지에 대 한 단일 뷰를 만드는 다음 코드와 일치 하도록 수정 합니다.

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 아래 내용으로 파일 `hello/urls.py`을 만듭니다. 이 `urls.py` 파일은 다양 한 url을 적절 한 뷰로 라우팅하는 패턴을 지정 합니다. 아래 코드에는 앱의 루트 URL (`""`)을 방금 추가한 `views.home` `hello/views.py`함수에 매핑하는 하나의 경로가 포함 되어 있습니다.

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. 이 `web_project` 폴더에는 URL `urls.py` 라우팅이 실제로 처리 되는 파일이 포함 되어 있습니다. 을 `web_project/urls.py` 열고 다음 코드와 일치 하도록 수정 합니다. 원하는 경우에는 사용할 수 있는 주석을 유지할 수 있습니다. 이 코드는 앱 내에 포함 `hello/urls.py` 된 `django.urls.include`앱의 경로를 유지 하는을 사용 하 여 앱을 가져옵니다. 이러한 분리는 프로젝트가 여러 앱을 포함 하는 경우에 유용 합니다.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 모든 수정 된 파일을 저장 합니다.

17. VS Code 터미널에서를 사용 `python manage.py runserver` 하 여 개발 서버를 실행 하 고 `http://127.0.0.1:8000/` 브라우저를 열어 "Hello, Django"를 렌더링 하는 페이지를 표시 합니다.

축 하 합니다. VS Code 및 Linux 용 Windows 하위 시스템을 사용 하 여 Django 웹 응용 프로그램을 만들었습니다. VS Code 및 Django를 사용 하는 자세한 자습서는 [Visual Studio Code의 Django 자습서](https://code.visualstudio.com/docs/python/tutorial-django)를 참조 하세요.

## <a name="additional-resources"></a>추가 자료

- [VS Code 포함 된 Python 자습서](https://code.visualstudio.com/docs/python/python-tutorial): Python 환경으로 VS Code 하기 위한 소개 자습서로, 주로 코드를 편집, 실행 및 디버그 하는 방법을 설명 합니다.
- [VS Code에서 Git 지원](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): VS Code에서 Git 버전 제어 기본 사항을 사용 하는 방법에 대해 알아봅니다.  
- [WSL 2!를 사용 하 여 곧 출시 될 업데이트에 대해 알아보세요](https://docs.microsoft.com/windows/wsl/wsl2-index). 이 새로운 버전은 Linux 배포판이 Windows와 상호 작용 하 여 파일 시스템 성능을 높이고 전체 시스템 호출 호환성을 추가 하는 방식을 변경 합니다.
- [Windows에서 여러 Linux 배포판 사용](https://docs.microsoft.com/windows/wsl/wsl-config): Windows 컴퓨터에서 여러 다른 Linux 배포를 관리 하는 방법을 알아봅니다.
