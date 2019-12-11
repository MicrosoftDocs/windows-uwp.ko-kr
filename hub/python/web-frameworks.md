---
title: Windows에서 Python을 사용한 웹 개발
description: Flask 및 Django와 같은 프레임워크 설정을 포함하여 Windows에서 웹 개발을 위해 Python 사용을 시작하는 방법입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Windows의 Python, WSL을 사용하는 Python 웹, Linux용 Windows 하위 시스템을 사용하는 Python 웹앱, Windows에서 Python 웹 개발, Windows의 Flask 앱, Windows의 Django 앱, Python 웹, Windows에서 Flask 웹 개발, Windows에서 Django 웹 개발, Python을 사용한 Windows 웹 개발, VS Code Python 웹 개발, Remote - WSL 확장, Ubuntu, WSL, venv, pip, Microsoft Python 확장, Windows에서 Python 실행, Windows에서 Python 사용, Windows에서 Python으로 빌드
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d883007168e0baf35f8a0ab0827505b683cfd291
ms.sourcegitcommit: f5bb4e35d1373b982259e61547b3b1765da0e78c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881288"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>웹 개발을 위해 Windows에서 Python 사용 시작

WSL(Linux용 Windows 하위 시스템)을 사용하여 Windows에서 웹 개발을 위해 Python 사용을 시작하는 단계별 가이드입니다.

## <a name="set-up-your-development-environment"></a>개발 환경 설정

웹 애플리케이션을 빌드하는 경우 Python을 WSL에 설치하는 것이 좋습니다. Python 웹 개발에 대한 많은 자습서와 지침은 Linux 사용자를 위해 작성되었으며, Linux 기반 패키징 및 설치 도구를 사용합니다. 대부분의 웹앱은 Linux에도 배포되므로 개발 환경과 프로덕션 환경 간에 일관성을 유지할 수 있습니다.

Python을 웹 개발 이외의 용도로 사용하는 경우 Microsoft Store를 사용하여 Python을 Windows 10에 직접 설치하는 것이 좋습니다. WSL은 GUI 데스크톱 또는 애플리케이션(예: PyGame, Gnome, KDE 등)을 지원하지 않습니다. 이러한 경우 Python을 Windows에 직접 설치하고 사용합니다. Python을 처음 사용하는 경우 [초보자를 위한 Windows에서 Python 사용 시작](./beginners.md) 가이드를 참조하세요. 운영 체제에서 일반적인 작업을 자동화하는 데 관심이 있는 경우 [스크립팅 및 자동화를 위해 Windows에서 Python 사용 시작](./scripting.md) 가이드를 참조하세요. 일부 고급 시나리오의 경우 [python.org](https://www.python.org/downloads/windows/)에서 특정 Python 릴리스를 직접 다운로드하거나 Anaconda, Jython, PyPy, WinPython, IronPython 등과 같은 [대체 구현](https://www.python.org/download/alternatives)을 설치하는 것이 좋습니다. 대체 구현을 선택하는 특별한 이유가 있는 고급 Python 프로그래머인 경우에만 이를 추천합니다.

## <a name="enable-windows-subsystem-for-linux"></a>Linux용 Windows 하위 시스템 사용

WSL을 사용하면 대부분의 명령줄 도구, 유틸리티 및 애플리케이션이 포함된 GNU/Linux 환경을 수정하지 않고 Windows 파일 시스템 및 즐겨찾는 도구(예: Visual Studio Code)와 완벽하게 통합하여 Windows에서 직접 실행할 수 있습니다. WSL을 사용하도록 설정하기 전에 [최신 버전의 Windows 10](https://www.microsoft.com/software-download/windows10)이 있는지 확인하세요.

컴퓨터에서 WSL을 사용하도록 설정하려면 다음을 수행해야 합니다.

1. **시작** 메뉴(왼쪽 아래의 Windows 아이콘)로 이동하여 "Windows 기능 켜기/끄기"를 입력하고, **제어판**에 대한 링크를 선택하여 **Windows 기능** 팝업 메뉴를 엽니다. 목록에서 "Linux용 Windows 하위 시스템"을 찾고, 이 확인란을 선택하여 해당 기능을 설정합니다.

2. 메시지가 표시되면 컴퓨터를 다시 시작합니다.

## <a name="install-a-linux-distribution"></a>Linux 배포 설치

WSL에서 실행할 수 있는 몇 가지 Linux 배포가 있습니다. Microsoft Store에서 원하는 배포를 찾아 설치할 수 있습니다. 현재 있기 있고 잘 지원되고 있는 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)부터 시작하는 것이 좋습니다.

1. 이 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 링크, Microsoft Store를 차례로 열고, **가져오기**를 선택합니다. *(이는 매우 큰 다운로드이며 설치하는데 시간이 걸릴 수 있습니다.)*

2. 다운로드가 완료되면 Microsoft Store에서 **시작**을 선택하거나 **시작** 메뉴에서 "Ubuntu 18.04 LTS"를 입력하여 시작합니다.

3. 배포를 처음 실행하는 경우 계정 이름과 암호를 만들라는 메시지가 표시됩니다. 이후에는 기본적으로 이 사용자로 자동으로 로그인됩니다. 임의의 사용자 이름과 암호를 선택할 수 있습니다. Windows 사용자 이름과는 관련이 없습니다.

`lsb_release -d`를 입력하여 현재 사용 중인 Linux 배포를 확인할 수 있습니다. Ubuntu 배포를 업데이트하려면 `sudo apt update && sudo apt upgrade`를 사용합니다. 최신 패키지를 유지하기 위해 정기적으로 업데이트하는 것이 좋습니다. 이 업데이트는 Windows에서 자동으로 처리하지 않습니다. Microsoft Store에서 사용 가능한 다른 Linux 배포에 대한 링크, 대체 구현 설치 방법 또는 문제 해결은 [Windows 10에 Linux용 Windows 하위 시스템 설치 가이드](https://docs.microsoft.com/windows/wsl/install-win10)를 참조하세요.

> [!TIP]
> 여러 명령줄(Ubuntu, PowerShell, Windows 명령 프롬프트 등)을 사용하려는 경우 또는 텍스트, 배경색, 키 바인딩 등을 비롯한 [터미널을 사용자 지정](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)하려는 경우 새 [Windows 터미널](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)을 사용해 보세요.

## <a name="set-up-visual-studio-code"></a>Visual Studio Code 설정

VS Code를 사용하여 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), [Linting](https://code.visualstudio.com/docs/python/linting), [디버그 지원](https://code.visualstudio.com/docs/python/debugging), [코드 조각](https://code.visualstudio.com/docs/editor/userdefinedsnippets) 및 [단위 테스트](https://code.visualstudio.com/docs/python/unit-testing)를 활용합니다. VS Code는 Linux용 Windows 하위 시스템과 원활하게 통합되어 코드 편집기와 명령줄 간에 원활한 워크플로를 설정할 수 있는 [기본 제공 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal)을 제공하며, UI에 기본적으로 직접 제공되는 일반 Git 명령(add, commit, push, pull)을 사용하여 [버전 제어를 위한 Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)을 지원합니다.

1. [Windows용 VS Code를 다운로드하여 설치합니다](https://code.visualstudio.com). VS Code는 Linux에서도 사용할 수 있지만, Linux용 Windows 하위 시스템은 GUI 앱을 지원하지 않으므로 Windows에 설치해야 합니다. 걱정하지 마세요. 여전히 Remote - WSL 확장을 사용하여 Linux 명령줄 및 도구와 통합할 수 있습니다.

2. [Remote - WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)을 VS Code에 설치합니다. 이를 통해 WSL을 통합 개발 환경으로 사용하고, 호환성과 패치를 처리할 수 있습니다. [자세한 내용을 알아보십시오](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> VS Code가 이미 설치되어 있는 경우 [Remote - WSL 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)을 설치하려면 [1.35 5월 릴리스](https://code.visualstudio.com/updates/v1_35) 이상이 있어야 합니다. 자동 완성, 디버깅, linting 등의 지원이 손실될 수 있으므로 VS Code에서 Remote - WSL 확장 없이 WSL을 사용하지 않는 것이 좋습니다. 재미있게도 이 WSL 확장은 $HOME/.vscode-server/extensions에 설치됩니다.

## <a name="create-a-new-project"></a>새 프로젝트 만들기

새 프로젝트 디렉터리를 Linux(Ubuntu) 파일 시스템에 만들어 보겠습니다. 그러면 VS Code를 사용하여 Linux 애플리케이션 및 도구에서 작업할 수 있습니다.

1. VS Code를 닫고, **시작** 메뉴(왼쪽 아래의 Windows 아이콘)로 이동하고 "Ubuntu 18.04"를 입력하여 Ubuntu 18.04(WSL 명령줄)를 엽니다.

2. Ubuntu 명령줄에서 프로젝트를 배치할 위치로 이동하고, `mkdir HelloWorld` 디렉터리를 만듭니다.

![Ubuntu 터미널](../images/ubuntu-terminal.png)

> [!TIP]
> WSL(Linux용 Windows 하위 시스템)을 사용할 때 기억해야 할 중요한 사항은 **현재 두 개의 서로 다른 파일 시스템**, 즉 1) Windows 파일 시스템 및 2) Linux 파일 시스템(WSL, 여기서는 Ubuntu) 간에 작업하고 있다는 것입니다. 패키지를 설치하고 파일을 저장하는 위치에 주의해야 합니다. Windows 파일 시스템에는 한 버전의 도구 또는 패키지를 설치하고, Linux 파일 시스템에는 완전히 다른 버전을 설치할 수 있습니다. Windows 파일 시스템에서 도구를 업데이트해도 Linux 파일 시스템의 도구에는 영향을 주지 않으며, 그 반대의 경우도 마찬가지입니다. WSL은 컴퓨터의 고정 드라이브를 Linux 배포의 `/mnt/<drive>` 폴더 아래에 탑재합니다. 예를 들어 Windows C: 드라이브는 `/mnt/c/` 아래에 탑재됩니다. Ubuntu 터미널에서 Windows 파일에 액세스하고 해당 파일에서 Linux 애플리케이션 및 도구를 사용할 수 있으며, 그 반대의 경우도 마찬가지입니다. 대부분의 웹 도구가 원래 Linux용으로 작성되어 Linux 프로덕션 환경에 배포되는 경우 Python 웹 개발을 위해 Linux 파일 시스템에서 작업하는 것이 좋습니다. 또한 파일 이름과 관련하여 대/소문자를 구분하지 않는 Windows와 같이 파일 시스템 의미 체계를 혼합하지 않도록 방지합니다. 즉, WSL은 이제 Linux 및 Windows 파일 시스템 간의 이동을 지원하므로 어느 쪽에서든 파일을 호스팅할 수 있습니다. [자세한 내용을 알아보십시오](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/). 또한 [WSL2가 Windows에 곧 출시](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)될 예정이며, 몇 가지 향상된 기능을 제공합니다. 이제 [Windows 참가자 빌드 18917에서 사용](https://docs.microsoft.com/windows/wsl/wsl2-install)해 볼 수 있습니다.

## <a name="install-python-pip-and-venv"></a>Python, pip 및 venv 설치

Ubuntu 18.04 LTS는 Python 3.6이 이미 설치된 상태로 제공되지만, 다른 Python 설치에서 제공할 것으로 예상되는 일부 모듈은 제공되지 않습니다. Python용 표준 패키지 관리자인 **pip** 및 간단한 가상 환경을 만들고 관리하는 데 사용되는 표준 모듈인 **venv**도 설치해야 합니다.  

1. Ubuntu 터미널을 열고 `python3 --version`을 입력하여 Python3이 이미 설치되어 있는지 확인합니다. 그러면 Python 버전 번호가 반환됩니다. Python 버전을 업데이트해야 하는 경우 먼저 `sudo apt update && sudo apt upgrade`를 입력하여 Ubuntu 버전을 업데이트한 다음, `sudo apt upgrade python3`을 사용하여 Python을 업데이트합니다.

2. `sudo apt install python3-pip`를 입력하여 **pip**를 설치합니다. pip를 사용하면 Python 표준 라이브러리에 포함되지 않은 추가 패키지를 설치하고 관리할 수 있습니다.

3. `sudo apt install python3-venv`를 입력하여 **venv**를 설치합니다.

## <a name="create-a-virtual-environment"></a>가상 환경 만들기

Python 개발 프로젝트에는 가상 환경을 사용하는 것이 좋습니다. 가상 환경을 만들면 프로젝트 도구를 격리하고 버전이 다른 프로젝트의 도구와 충돌하지 않도록 방지할 수 있습니다. 예를 들어 Django 1.2 웹 프레임워크가 필요한 이전 웹 프로젝트를 유지 관리할 수 있지만, Django 2.2를 사용하면 흥미로운 새 프로젝트가 제공됩니다. 가상 환경 외부에서 Django를 전역적으로 업데이트하면 나중에 일부 버전 관리 문제가 발생할 수 있습니다. 가상 환경에서는 실수로 인한 버전 충돌 방지 외에도 관리자 권한 없이 패키지를 설치하고 관리할 수 있습니다.

1. 터미널을 열고, *HelloWorld* 프로젝트 폴더 내에서 `python3 -m venv .venv` 명령을 사용하여 **.venv**라는 가상 환경을 만듭니다.

2. 가상 환경을 활성화하려면 `source .venv/bin/activate`를 입력합니다. 정상적으로 작동하면 명령 프롬프트 앞에 **(.venv)** 가 표시됩니다. 이제 코드를 작성하고 패키지를 설치할 수 있는 자체 포함 환경이 준비되었습니다. 가상 환경 작업이 완료되면 `deactivate` 명령을 입력하여 가상 환경을 비활성화합니다.

    ![가상 환경 만들기](../images/wsl-venv.png)

> [!TIP]
> 프로젝트를 만들려는 디렉터리 내에 가상 환경을 만드는 것이 좋습니다. 각 프로젝트에는 자체의 고유한 디렉터리가 있어야 하므로 각 디렉터리마다 고유한 가상 환경을 만듭니다. 이 경우 고유한 이름을 지정할 필요가 없습니다. Python 규칙에 따라 **.venv** 이름을 사용하는 것이 좋습니다. 프로젝트 디렉터리에 설치하면 pipenv와 같은 일부 도구에도 기본적으로 이 이름이 설정됩니다. **.env**는 환경 변수 정의 파일과 충돌하므로 사용하지 않으려고 합니다. 일반적으로 디렉터리가 있음을 지속적으로 알려주는 `ls`가 필요하지 않으므로 점으로 구분되지 않는 이름을 사용하지 않는 것이 좋습니다. 또한 **.venv**를 .gitignore 파일에 추가하는 것이 좋습니다. ([GitHub의 Python용 기본 gitignore 템플릿](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106)을 참조하세요.) VS Code의 가상 환경 작업에 대한 자세한 내용은 [VS Code에서 Python 환경 사용](https://code.visualstudio.com/docs/python/environments)을 참조하세요.

## <a name="open-a-wsl---remote-window"></a>Remote - WSL 창 열기

VS Code는 이전에 설치된 Remote - WSL 확장을 사용하여 Linux 하위 시스템을 원격 서버로 처리합니다. 그러면 WSL을 통합 개발 환경으로 사용할 수 있습니다. [자세한 내용을 알아보십시오](https://code.visualstudio.com/docs/remote/wsl). 

1. `code .`를 입력하여 Ubuntu 터미널에서 VS Code의 프로젝트 폴더를 엽니다("."는 현재 폴더를 열도록 VS Code에 지시함).

2. Windows Defender에서 [보안 경고]가 팝업됩니다. 그러면 "액세스 허용"을 선택합니다. VS Code가 열리면 왼쪽 아래 모서리에 [원격 연결 호스트] 표시기가 표시되어 **WSL: Ubuntu-18.04**에서 편집하고 있음을 알 수 있습니다.

    ![VS Code 원격 연결 호스트 표시기](../images/wsl-remote-extension.png)

3. Ubuntu 터미널을 닫습니다. 앞으로 이동하면 VS Code에 통합된 WSL 터미널을 사용할 수 있습니다.

4. **Ctrl+`** (백틱 문자 사용)을 누르거나 **보기** > **터미널**을 차례로 선택하여 VS Code에서 WSL 터미널을 엽니다. 그러면 Ubuntu 터미널에서 만든 프로젝트 폴더 경로에 열려 있는 Bash(WSL) 명령줄이 열립니다.

    ![WSL 터미널을 사용하는 VS Code](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 확장 설치

Remote - WSL용 VS Code 확장을 설치해야 합니다. VS Code에 이미 로컬로 설치된 확장은 자동으로 사용할 수 없습니다. [자세한 내용을 알아보십시오](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. **Ctrl+Shift+X**를 입력하거나 메뉴에서 **보기** > **확장**으로 이동하여 VS Code 확장 창을 엽니다.

2. 위쪽의 **마켓플레이스에서 확장 검색** 상자에  **Python**을 입력합니다.

3. **Python (ms-python.python) by Microsoft** 확장을 찾아 녹색 **설치** 단추를 선택합니다.

4. 확장 설치가 완료되면 파란색 **다시 로드 필요** 단추를 선택해야 합니다. 그러면 VS Code가 다시 로드되고, VS Code 확장 창에 Python 확장이 설치되었음을 보여 주는 **WSL: UBUNTU-18.04 - 설치됨** 섹션이 표시됩니다.

## <a name="run-a-simple-python-program"></a>간단한 Python 프로그램 실행

Python은 해석된 언어이며 다양한 유형의 인터프리터(Python2, Anaconda, PyPy 등)를 지원합니다. VS Code는 프로젝트와 연결되는 인터프리터를 기본값으로 설정해야 합니다. 변경해야 하는 이유가 있으면 VS Code 창의 아래쪽에 있는 파란색 표시줄에 현재 표시된 인터프리터를 선택하거나, **명령 팔레트**(Ctrl+Shift+P)를 열고 **Python: Select Interpreter**(인터프리터 선택) 명령을 입력합니다. 그러면 현재 설치된 Python 인터프리터 목록이 표시됩니다. [Python 환경 구성에 대해 자세히 알아보세요](https://code.visualstudio.com/docs/python/environments).

간단한 Python 프로그램을 테스트용으로 만들어 실행하고, 올바른 Python 인터프리터를 선택했는지 확인합니다.

1. **Ctrl+Shift+E**를 입력하거나 메뉴에서 **보기** > **탐색기**로 차례로 이동하여 VS Code 파일 탐색기 창을 엽니다.

2. 아직 열려 있지 않은 경우 **Ctrl+Shift+`** 을 입력하여 통합 WSL 터미널을 열고, **HelloWorld** Python 프로젝트 선택되어 있는지 확인합니다.

3. `touch test.py`를 입력하여 Python 파일을 만듭니다. 방금 만든 파일이 탐색기 창의 프로젝트 디렉터리에 이미 있는 .venv 및 .vscode 폴더 아래에 표시됩니다.

4. 탐색기 창에서 방금 만든 **test.py** 파일을 선택하여 VS Code에서 엽니다. 파일 이름의 .py는 VS Code에 이 파일이 Python 파일임을 알려주므로 이전에 로드한 Python 확장에서 VS Code 창의 아래쪽에 표시되는 Python 인터프리터를 자동으로 선택하고 로드합니다.

    ![VS Code에서 Python 인터프리터 선택](../images/interpreterselection.gif)

5. 이 Python 코드를 test.py 파일에 붙여넣은 다음, 파일을 저장합니다(Ctrl+S). 

    ```python
    print("Hello World")
    ```

6. 방금 만든 Python "Hello World" 프로그램을 실행하려면 VS Code 탐색기 창에서 **test.py** 파일을 선택한 다음, 마우스 오른쪽 단추로 해당 파일을 클릭하여 옵션 메뉴를 표시합니다. **터미널에서 Python 파일 실행**을 선택합니다. 또는 통합 WSL 터미널 창에서 `python test.py`를 입력하여 "Hello World" 프로그램을 실행합니다. Python 인터프리터의 터미널 창에서 "Hello World"를 출력합니다.

축하합니다. Python 프로그램을 만들고 실행하도록 모두 설정되었습니다! 이제 가장 인기 있는 두 개의 Python 웹 프레임워크(Flask 및 Django)를 사용하여 Hello World 앱을 만들어 보겠습니다.

## <a name="hello-world-tutorial-for-flask"></a>Flask용 Hello World 자습서

[Flask](http://flask.pocoo.org/)는 Python용 웹 애플리케이션 프레임워크입니다. 이 간결한 자습서에서는 VS Code와 WSL을 사용하여 간단한 "Hello World" Flask 앱을 만듭니다.

1. **시작** 메뉴(왼쪽 아래의 Windows 아이콘)로 이동하고 "Ubuntu 18.04"를 입력하여 Ubuntu 18.04(WSL 명령줄)를 엽니다.

2. 프로젝트에 대한 디렉터리를 만든(`mkdir HelloWorld-Flask`) 다음, 해당 디렉터리로 이동합니다(`cd HelloWorld-Flask`).

3. 프로젝트 도구를 설치할 가상 환경을 만듭니다(`python3 -m venv .venv`).

4. `code .` 명령을 입력하여 VS Code에서 **HelloWorld-Flask** 프로젝트를 엽니다.

5. VS Code 내에서 **Ctrl+Shift+`** 을 입력하여 통합 WSL 터미널(즉, Bash)을 엽니다(**HelloWorld-Flask** 프로젝트 폴더가 이미 선택되어 있어야 함). *앞으로 VS Code와 통합된 WSL 터미널에서 작업할 예정이므로 Ubuntu 명령줄을 닫으세요.*

6. VS Code에서 Bash 터미널을 사용하여 3단계에서 만든 가상 환경을 활성화합니다(`source .venv/bin/activate`). 정상적으로 작동하면 명령 프롬프트 앞에 (.venv)가 표시됩니다.

7. `python3 -m pip install flask`를 입력하여 Flask를 가상 환경에 설치합니다. `python3 -m flask --version`을 입력하여 설치되었는지 확인합니다.

8. Python 코드에 대한 새 파일을 만듭니다(`touch app.py`).

9. VS Code의 파일 탐색기에서 **app.py** 파일을 엽니다(`Ctrl+Shift+E`를 입력한 다음, app.py 파일을 선택함). 그러면 Python 확장이 활성화되어 인터프리터를 선택합니다. 기본값은 **Python 3.6.8 64-bit ('.venv': venv)** 입니다. 가상 환경도 검색되었습니다.

    ![활성화된 가상 환경](../images/virtual-environment.png)

10. **app.py**에서 Flask를 가져오고 Flask 개체의 인스턴스를 만드는 코드를 추가합니다.

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. **app.py**에서 콘텐츠를 반환하는 함수도 추가합니다. 이 경우 간단한 문자열입니다. Flask의 **app.route** 데코레이터를 사용하여 "/" URL 경로를 해당 함수에 매핑합니다.

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 동일한 함수에 매핑할 다양한 경로 수에 따라 동일한 함수에서 한 줄당 하나씩 여러 데코레이터를 사용할 수 있습니다.

12. **app.py** 파일을 저장합니다(**Ctrl+S**).

13. 터미널에서 다음 명령을 입력하여 앱을 실행합니다.

    ```python
    python3 -m flask run
    ```

    그러면 Flask 개발 서버가 실행됩니다. 개발 서버는 기본적으로 **app.py**를 찾습니다. Flask를 실행하면 다음과 비슷한 출력이 표시됩니다.

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 기본 웹 브라우저를 렌더링된 페이지로 엽니다. 터미널에서 **Ctrl+클릭**을 사용하여 http://127.0.0.1:5000/ URL을 클릭합니다. 브라우저에서 다음 메시지가 표시됩니다.

    ![Hello, Flask!](../images/hello-flask.png)

15. "/"와 같은 URL을 방문하는 경우 HTTP 요청을 보여 주는 메시지가 디버그 터미널에 표시되는지 확인합니다.

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. 터미널에서 **Ctrl+C**를 사용하여 앱을 중지합니다.

> [!TIP]
> **app.py**가 아닌 다른 파일 이름(예: **program.py**)을 사용하려면 **FLASK_APP**이라는 환경 변수를 정의하고 해당 값을 선택한 파일로 설정합니다. 그러면 Flask의 개발 서버에서 기본 **app.py** 파일 대신 **FLASK_APP** 값을 사용합니다. 자세한 내용은 [Flask 명령줄 인터페이스 설명서](http://flask.pocoo.org/docs/1.0/cli/)를 참조하세요.

축하합니다. Visual Studio Code 및 Linux용 Windows 하위 시스템을 사용하여 Flask 웹 애플리케이션을 만들었습니다! VS Code 및 Flask를 사용하는 방법에 대한 자세한 자습서는 [Visual Studio Code의 Flask 자습서](https://code.visualstudio.com/docs/python/tutorial-flask)를 참조하세요.

## <a name="hello-world-tutorial-for-django"></a>Django용 Hello World 자습서

[Django](https://www.djangoproject.com)는 Python용 웹 애플리케이션 프레임워크입니다. 이 간결한 자습서에서는 VS Code와 WSL을 사용하여 간단한 "Hello World" Django 앱을 만듭니다.

1. **시작** 메뉴(왼쪽 아래의 Windows 아이콘)로 이동하고 "Ubuntu 18.04"를 입력하여 Ubuntu 18.04(WSL 명령줄)를 엽니다.

2. 프로젝트에 대한 디렉터리를 만든(`mkdir HelloWorld-Django`) 다음, 해당 디렉터리로 이동합니다(`cd HelloWorld-Django`).

3. 프로젝트 도구를 설치할 가상 환경을 만듭니다(`python3 -m venv .venv`).

4. `code .` 명령을 입력하여 VS Code에서 **HelloWorld-DJango** 프로젝트를 엽니다.

5. VS Code 내에서 **Ctrl+Shift+`** 을 입력하여 통합 WSL 터미널(즉, Bash)을 엽니다(**HelloWorld-Django** 프로젝트 폴더가 이미 선택되어 있어야 함). *앞으로 VS Code와 통합된 WSL 터미널에서 작업할 예정이므로 Ubuntu 명령줄을 닫으세요.*

6. VS Code에서 Bash 터미널을 사용하여 3단계에서 만든 가상 환경을 활성화합니다(`source .venv/bin/activate`). 정상적으로 작동하면 명령 프롬프트 앞에 (.venv)가 표시됩니다.

7. `python3 -m pip install django` 명령을 사용하여 Django를 가상 환경에 설치합니다. `python3 -m django --version`을 입력하여 설치되었는지 확인합니다.

8. 다음으로, 다음 명령을 실행하여 Django 프로젝트를 만듭니다.

    ```bash
    django-admin startproject web_project .
    ```

    `startproject` 명령은 끝부분에 `.`를 사용하여 현재 폴더가 프로젝트 폴더라고 가정하고 그 안에 다음을 만듭니다.

    - `manage.py`: 프로젝트에 대한 Django 명령줄 관리 유틸리티입니다. `python manage.py <command> [options]`를 사용하여 프로젝트에 대한 관리 명령을 실행합니다.

    - 다음 파일이 포함되는 `web_project`라는 하위 폴더입니다.
        - `__init__.py`: 이 폴더가 Python 패키지임을 Python에 알려주는 빈 파일입니다.
        - `wsgi.py`: WSGI 호환 웹 서버에서 프로젝트를 제공하는 진입점입니다. 일반적으로 이 파일은 프로덕션 웹 서버에 대한 후크를 제공하므로 그대로 둡니다.
        - `settings.py`: Django 프로젝트에 대한 설정을 포함하며, 웹앱을 개발하는 과정에서 수정할 수 있습니다.
        - `urls.py`: Django 프로젝트의 목차를 포함하며, 개발 과정에서 수정할 수도 있습니다.

9. Django 프로젝트를 확인하려면 `python3 manage.py runserver` 명령을 사용하여 Django의 개발 서버를 시작합니다. 서버는 기본 8000 포트에서 실행되며, 터미널 창에 다음과 같은 출력이 표시됩니다.

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    서버를 처음 실행하면 `db.sqlite3` 파일에 기본 SQLite 데이터베이스가 만들어집니다. 이 데이터베이스는 개발 목적으로 사용되지만 작은 웹앱의 프로덕션에서 사용할 수 있습니다. 또한 Django의 기본 제공 웹 서버는 *로컬 개발 목적으로만* 사용됩니다. 그러나 웹 호스트에 배포하는 경우 Django는 호스트의 웹 서버를 대신 사용합니다. Django 프로젝트의 `wsgi.py` 모듈은 프로덕션 서버에 대한 후크를 처리합니다.

    기본 8000 포트가 아닌 다른 포트를 사용하려면 명령줄에서 `python3 manage.py runserver 5000`과 같은 포트 번호를 지정합니다.

10. 터미널 출력 창에서 `Ctrl+click`으로 `http://127.0.0.1:8000/` URL을 클릭하여 기본 브라우저를 해당 주소로 엽니다. Django가 제대로 설치되어 있고 프로젝트가 올바르면 기본 페이지가 표시됩니다. VS Code 터미널 출력 창에도 서버 로그가 표시됩니다.

11. 완료되면 브라우저 창을 닫고, 터미널 출력 창에 표시된 대로 `Ctrl+C`를 사용하여 VS Code에서 서버를 중지합니다.

12. 이제 Django 앱을 만들기 위해 프로젝트 폴더(`manage.py` 상주)에서 관리 유틸리티의 `startapp` 명령을 실행합니다.

    ```bash
    python3 manage.py startapp hello
    ```

    이 명령은 여러 코드 파일과 하나의 하위 폴더를 포함하는 `hello`라는 폴더를 만듭니다. 이 중에서 `views.py`(웹앱에서 페이지를 정의하는 함수 포함) 및 `models.py`(데이터 개체를 정의하는 클래스 포함)를 사용하는 경우가 많습니다. `migrations` 폴더는 Django의 관리 유틸리티에서 사용하여 이 자습서의 뒷부분에서 설명한 대로 데이터베이스 버전을 관리합니다. 또한 `apps.py`(애플리케이션 구성), `admin.py`(관리 인터페이스 만들기용) 및 `tests.py`(테스트용) 파일도 있습니다.

13. 다음 코드와 일치하도록 `hello/views.py`를 수정하여 앱의 홈 페이지에 대한 단일 보기를 만듭니다.

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 아래 내용이 포함된 `hello/urls.py` 파일을 만듭니다. `urls.py` 파일은 다양한 URL을 적절한 보기로 라우팅하는 패턴을 지정합니다. 아래 코드에는 앱(`""`)의 루트 URL을 방금 `hello/views.py`에 추가한 `views.home` 함수에 매핑하는 하나의 경로가 있습니다.

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. `web_project` 폴더에는 URL 라우팅을 실제로 처리하는 `urls.py` 파일도 포함되어 있습니다. `web_project/urls.py`를 열고, 다음 코드와 일치하도록 수정합니다(원하는 경우 유용한 주석을 유지할 수 있음). 이 코드는 `django.urls.include`를 사용하여 앱의 `hello/urls.py`를 끌어와서 앱 내에 포함된 앱의 경로를 유지합니다. 이 분리는 프로젝트에 여러 앱이 포함된 경우에 유용합니다.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 모든 수정된 파일을 저장합니다.

17. VS Code 터미널에서 `python manage.py runserver`를 사용하여 개발 서버를 실행하고, 브라우저를 `http://127.0.0.1:8000/`으로 열어 "Hello, Django"를 렌더링하는 페이지를 확인합니다.

축하합니다. VS Code 및 Linux용 Windows 하위 시스템을 사용하여 Django 웹 애플리케이션을 만들었습니다! VS Code 및 Django를 사용하는 방법에 대한 자세한 자습서는 [Visual Studio Code의 Django 자습서](https://code.visualstudio.com/docs/python/tutorial-django)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [VS Code를 사용하는 Python 자습서](https://code.visualstudio.com/docs/python/python-tutorial): VS Code를 Python 환경으로 소개하는 자습서이며, 주로 코드를 편집, 실행 및 디버그하는 방법을 설명합니다.
- [VS Code의 Git 지원](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): VS Code에서 Git 버전 제어의 기본 기능을 사용하는 방법을 알아봅니다.  
- [WSL 2에서 곧 제공될 업데이트에 대해 자세히 알아보기](https://docs.microsoft.com/windows/wsl/wsl2-index): 이 새 버전은 Linux 배포에서 Windows와 상호 작용하는 방식을 변경하여 파일 시스템 성능을 높이고 전체 시스템 호출 호환성을 추가합니다.
- [Windows에서 여러 Linux 배포 사용](https://docs.microsoft.com/windows/wsl/wsl-config): Windows 머신에서 여러 Linux 배포를 관리하는 방법을 알아봅니다.
