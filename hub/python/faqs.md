---
title: Windows에서 Python 사용에 대한 질문과 대답입니다.
description: 개발을 위해 Windows에서 Python을 사용하는 방법에 대한 FAQ(질문과 대답)의 답변을 확인하세요.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, pip, py.exe, 파일 경로, PYTHONPATH, python 배포, python 패키징
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 7bc1d159bac5ebc9877db5d879b6acd82c51079b
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823557"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Windows에서 Python 사용에 대한 질문과 대답입니다.

## <a name="trouble-installing-a-package-with-pip-install"></a>pip install을 사용하여 패키지를 설치할 수 없는 문제

설치가 실패하는 여러 이유가 있으며, 대부분은 패키지 개발자에게 문의하는 것이 가장 좋은 방법입니다.

문제가 발생하는 가장 일반적인 원인은 수정할 권한이 없는 위치에 설치하려고 시도하는 것입니다. 예를 들어 기본 설치 위치는 관리 권한이 필요할 수 있지만, 기본적으로 Python은 관리 권한을 갖고 있지 않습니다. 가장 좋은 해결 방법은 [가상 환경](./web-frameworks.md#create-a-virtual-environment)을 만들고 거기에 설치하는 것입니다.

일부 패키지에는 C 또는 C++ 컴파일러를 설치해야 하는 네이티브 코드가 포함되어 있습니다. 일반적으로 패키지 개발자는 미리 컴파일된 버전을 게시해야 하지만, 게시하지 않는 경우도 자주 있습니다. [Build Tools for Visual Studio를 설치](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)하고 C++ 옵션을 선택하면 이러한 패키지 중 일부가 작동하기도 하지만, 대부분은 패키지 개발자에게 문의해야 합니다.

[StackOverflow의 설명을 따르세요](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

### <a name="trouble-installing-pip-with-wsl"></a>WSL로 pip를 설치할 수 없는 문제

Linux용 Windows 하위 시스템(WSL 또는 WSL2)에서 pip를 사용하여 Flask 같은 패키지를 설치할 때(예: `python3 -m pip install flask`) 다음과 같은 오류가 발생할 수 있습니다.

```bash
WARNING: Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None))
after connection broken by 'NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection
object at 0x7f655471da30>: Failed to establish a new connection: [Errno -3]
Temporary failure in name resolution')': /simple/flask/
```

이 문제를 조사할 때 미궁에 빠질 수 있으며, WSL linux 배포판에서 특별히 잘 해결되는 것은 아닙니다. (경고: WSL에서 `resolv.conf` 파일을 편집하지 마세요. 이 파일은 바로 가기 링크이며 이 파일을 수정하면 상황이 더 복잡해집니다.) 애프터마켓 방화벽을 실행하지 않는 한, 가능성이 높은 해결 방법은 간단하게 pip를 다시 설치하는 것입니다.

```bash
sudo apt -y purge python3-pip
sudo python3 -m pip uninstall pip
sudo apt -y install python3-pip --fix-missing
```

* *[GitHub의 WSL 제품 리포지토리](https://github.com/microsoft/WSL/issues/4020)에서 자세히 알아보세요. 문서에 [이 문제를 올려 주신](https://github.com/MicrosoftDocs/windows-uwp/issues/2679) 사용자 커뮤니티에 감사하다는 말씀을 드립니다.*

## <a name="what-is-pyexe"></a>py.exe란?

여러 유형의 Python 프로젝트를 작업하게 되므로, 머신에 여러 버전의 Python이 설치될 수 있습니다. 모든 버전이 `python` 명령을 사용하기 때문에, 현재 어떤 Python 버전을 사용 중인지 명확하게 알 수 없는 경우가 있습니다. 일반적으로 `python3` 명령(또는 특정 버전을 선택하려면 `python3.7`)을 사용하는 것이 좋습니다.

[py.exe 시작 관리자](https://docs.python.org/3/using/windows.html#launcher)는 설치된 가장 최신 버전의 Python을 자동으로 선택합니다. `py -3.7` 명령을 사용하여 특정 버전을 선택하거나, `py --list` 명령을 사용하여 어떤 버전을 사용할 수 있는지 확인할 수도 있습니다. **그러나**[python.org](https://www.python.org/downloads/windows/)에서 설치된 Python 버전을 사용하는 경우에만 py.exe 시작 관리자가 작동합니다. Microsoft Store에서 Python을 설치하는 경우 `py` 명령이 **포함되지 않습니다**. Linux, macOS, WSL 및 Microsoft Store 버전의 Python에서는 `python3`(또는 `python3.7`) 명령을 사용해야 합니다.

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>python.exe를 실행하면 Microsoft Store가 열리는 이유는 무엇인가요?

신규 사용자가 Python을 올바르게 설치하도록 도와주기 위해, Microsoft Store에 게시된 최신 버전의 커뮤니티 패키지로 바로 이동하는 바로 가기가 Windows에 추가되었습니다. 이 패키지는 관리자 권한 없이 쉽게 설치할 수 있으며, 기본 `python` 및 `python3` 명령을 실제 명령으로 바꿉니다.

명령줄 인수를 사용하여 바로 가기 실행 파일을 실행하면 Python이 설치되지 않았다는 오류 코드가 반환됩니다. 이는 일괄 처리 파일 및 스크립트가 의도치 않게 Store 앱을 열지 못하게 방지하는 동작입니다.

[python.org](https://www.python.org/downloads/windows/)의 설치 관리자를 사용하여 Python을 설치하고 "PATH에 추가" 옵션을 선택하면 새 `python` 명령이 바로 가기보다 우선적으로 적용됩니다. 다른 설치 관리자는 기본 제공 바로 가기보다 _낮은_ 우선 순위로 `python`을 추가할 수 있습니다.

[시작]에서 "앱 실행 별칭 관리"를 열고, "앱 설치 관리자" Python 항목을 찾아 "끄기"로 전환하면 Python을 설치하지 않고 바로 가기를 비활성화할 수 있습니다.

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>파일 경로를 복사하여 붙여넣을 때 Python에서 파일 경로가 작동하지 않는 이유는 무엇인가요?

Python 문자열은 특수 문자에 "이스케이프"를 사용합니다. 예를 들어 문자열에 줄 바꿈 문자를 삽입하려면 `\n`을 입력합니다. Windows의 파일 경로는 백슬래시를 사용하기 때문에 경로의 일부가 특수 문자로 변환될 수 있습니다.

Python에서 경로를 문자열로 붙여넣으려면 `r` 접두사를 추가합니다. 이렇게 하면 `raw` 문자열임을 나타내며, \”를 제외하고 이스케이프 문자를 사용하지 않습니다(경로에서 마지막 백슬래시를 제거해야 할 수 있음). 따라서 경로는 `r"C:\Users\MyName\Documents\Document.txt"`와 같을 수 있습니다.

Python에서 경로를 작업할 때는 표준 pathlib 모듈을 사용하는 것이 좋습니다. 이렇게 하면 슬래시 또는 백슬래시 사용 여부에 관계없이 일관적으로 경로를 조작할 수 있는 풍부한 Path 개체로 문자열을 변환할 수 있으며, 코드가 여러 운영 체제에서 보다 효율적으로 작동합니다.

## <a name="what-is-pythonpath"></a>PYTHONPATH란?

PYTHONPATH 환경 변수는 Python에서 모듈을 가져올 수 있는 디렉터리 목록을 지정하는 데 사용됩니다. 실행 중인 동안 `sys.path` 변수를 검사하면 항목을 가져올 때 어떤 디렉터리가 검색되는지 확인할 수 있습니다.

명령 프롬프트에서 이 변수를 설정하려면 `set PYTHONPATH=list;of;paths`를 사용합니다.

PowerShell에서 이 변수를 설정하려면 Python을 시작하기 직전에 `$env:PYTHONPATH=’list;of;paths’`를 사용합니다.

**환경 변수** 설정을 통해 이 변수를 전역적으로 설정하면 사용하려는 버전이 아닌 다른 Python 버전에서 이 변수를 사용할 수 있으므로, 권장하지 **않습니다**.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>패키징 및 배포에 대한 도움은 어디서 찾을 수 있나요?

[Docker](https://code.visualstudio.com/docs/azure/docker): [VSCode 확장](https://code.visualstudio.com/docs/azure/docker)을 사용하면 Dockerfile 및 docker-compose.yml 템플릿을 사용하여 신속하게 패키징하고 배포할 수 있습니다(프로젝트에 적합한 Docker 파일 생성).

[AKS(Azure Kubernetes Service)](/azure/aks/)를 사용하면 컨테이너화된 애플리케이션을 배포하고 관리하는 한편, 주문형 리소스를 확장할 수 있습니다.

## <a name="what-if-i-need-to-work-across-different-machines"></a>여러 머신에서 작업해야 하는 경우에는 어떻게 하나요?

[설정 동기화](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)를 사용하면 GitHub를 사용하는 여러 설치에서 VS Code 설정을 동기화할 수 있습니다. 여러 머신에서 작업하는 경우 이렇게 하면 여러 머신의 환경을 일관되게 유지할 수 있습니다.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>PyCharm, Atom, Sublime Text, Emacs 또는 Vim에 익숙한 경우에는 어떻게 하나요?

VSCode 확장 [Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)를 사용하면 편안한 환경에서 작업할 수 있습니다.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Mac 바로 가기 키는 어떤 방식으로 Windows 바로 가기 키에 매핑되나요?

Windows 머신과 Macintosh의 일부 키보드 단추 및 시스템 바로 가기는 서로 약간 다릅니다. [Mac에서 Windows로 전환 가이드](../dev-environment/mac-to-windows.md)에서 이에 대한 기본 사항을 다룹니다.
