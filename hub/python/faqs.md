---
title: Windows에서 Python 사용에 대 한 질문과 대답
description: Windows에서 Python 사용에 대 한 질문과 대답
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python, windows 10, microsoft, pip, py, 파일 경로, PYTHONPATH, python 배포, python 패키징
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: fd08061858fc97f1427e94c6a92a4c3a9511967d
ms.sourcegitcommit: 210034519678ba1a59744bc3a0b613b000921537
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68473656"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Windows에서 Python 사용에 대 한 질문과 대답

## <a name="why-cant-i-pip-install-a-certain-package"></a>특정 패키지를 "pip 설치" 할 수 없는 이유는 무엇입니까?

설치에 실패 하는 여러 가지 이유가 있습니다. 대부분의 경우 적절 한 솔루션은 패키지 개발자에 게 문의 하는 것입니다.

문제의 가장 일반적인 원인은 수정할 권한이 없는 위치에 설치 하는 것입니다. 예를 들어 기본 설치 위치에는 관리 권한이 필요할 수 있지만 기본적으로 Python에는 포함 되지 않습니다. 가장 좋은 해결 방법은 가상 환경을 만들고이를 설치 하는 것입니다.

일부 패키지에는 C 또는 C++ 컴파일러가 설치 되어야 하는 네이티브 코드가 포함 됩니다. 일반적으로 패키지 개발자는 미리 컴파일된 버전을 게시 해야 하지만 그렇지 않은 경우도 있습니다. [Visual Studio 용 빌드 도구를 설치](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) 하 고 C++ 옵션을 선택 하는 경우 이러한 패키지 중 일부를 사용할 수 있지만 대부분의 경우 패키지 개발자에 게 문의 해야 합니다.

[StackOverflow에 대 한 설명을 따르세요](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

### <a name="what-is-pyexe"></a>Py 란?

여러 유형의 Python 프로젝트에서 작업 하 고 있으므로 컴퓨터에 여러 버전의 Python이 설치 될 수 있습니다. 이러한 모두 `python` 명령을 사용 하기 때문에 사용 중인 Python 버전을 명확 하 게 알 수 없습니다. 표준으로 `python3` 명령을 사용 하거나 `python3.7` 특정 버전을 선택 하는 것이 좋습니다.

[Py 시작 관리자](https://docs.python.org/3/using/windows.html#launcher) 는 설치한 Python의 최신 버전을 자동으로 선택 합니다. 같은 `py -3.7` 명령을 사용 하 여 특정 버전을 선택 하거나 `py --list` 사용할 수 있는 버전을 확인할 수도 있습니다. **그러나** [python.org](https://www.python.org/downloads/windows/)에서 설치 된 Python 버전을 사용 하는 경우에만 py 시작 관리자가 작동 합니다. Microsoft Store에서 Python을 `py` 설치 하는 경우 명령은 **포함 되지 않습니다**. Linux, macos, wsl 및 Python의 Microsoft Store 버전에 대해 `python3` (또는 `python3.7`) 명령을 사용 해야 합니다.

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>파일 경로를 복사 하 여 붙여넣을 때 Python에서 파일 경로가 작동 하지 않는 이유는 무엇 인가요?

Python 문자열은 특수 문자에 대해 "이스케이프"를 사용 합니다. 예를 들어, 문자열에 줄 바꿈 문자를 삽입 하려면를 입력 `\n`합니다. Windows의 파일 경로는 백슬래시를 사용 하기 때문에 일부 부분은 특수 문자로 변환 될 수 있습니다.

Python에서 경로를 문자열로 붙여넣으려면 `r` 접두사를 추가 합니다. 이것은 `raw` 문자열이 문자열이 고 \ "를 제외 하 고 이스케이프 문자를 사용 하지 않음을 나타냅니다 (경로에서 마지막 백슬래시를 제거 해야 할 수 있음). 따라서 경로는 r "C:\Users\MyName\Documents\Document.txt"와 같을 수 있습니다.

Python에서 경로로 작업할 때는 표준 pathlib 모듈을 사용 하는 것이 좋습니다. 이렇게 하면 슬래시 또는 백슬래시를 사용 하 여 여러 운영 체제에서 코드를 효율적으로 사용할 수 있는지 여부에 관계 없이 경로 조작을 일관 되 게 수행할 수 있는 풍부한 경로 개체로 문자열을 변환할 수 있습니다.

## <a name="what-is-pythonpath"></a>PYTHONPATH 란?

PYTHONPATH 환경 변수는 Python에서 모듈을 가져올 수 있는 디렉터리 목록을 지정 하는 데 사용 됩니다. 실행할 때 `sys.path` 변수를 검사 하 여 가져올 때 검색 되는 디렉터리를 확인할 수 있습니다.

명령 프롬프트에서이 변수를 설정 하려면를 사용 `set PYTHONPATH=list;of;paths`합니다.

PowerShell에서이 변수를 설정 하려면 Python을 `$env:PYTHONPATH=’list;of;paths’` 시작 하기 바로 전에을 사용 합니다.

**환경 변수** 설정을 통해 전역으로이 변수를 설정 하는 것은 사용 하려는 것이 아닌 모든 버전의 Python에서 사용할 수 있기 때문에 사용 **하지 않는** 것이 좋습니다.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>패키징 및 배포에 대 한 도움말은 어디에서 찾을 수 있나요?

[Docker](https://code.visualstudio.com/docs/azure/docker): [Vscode 확장](https://code.visualstudio.com/docs/azure/docker) 을 사용 하면 Dockerfile 및 docker-compose.ci.build.yml 템플릿을 사용 하 여 신속 하 게 패키지 하 고 배포할 수 있습니다 (프로젝트에 적합 한 docker 파일 생성).

[AKS (Azure Kubernetes Service)](https://docs.microsoft.com/azure/aks/) 를 사용 하면 주문형 리소스를 확장 하는 동시에 컨테이너 화 된 응용 프로그램을 배포 하 고 관리할 수 있습니다.

## <a name="what-if-i-need-to-work-across-different-machines"></a>여러 컴퓨터에서 작업 해야 하는 경우는 어떻게 되나요?

[설정 동기화](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) 를 사용 하면 GitHub를 사용 하 여 여러 설치에서 VS Code 설정을 동기화 할 수 있습니다. 여러 컴퓨터에서 작업 하는 경우이를 통해 환경을 일관 되 게 유지할 수 있습니다.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>PyCharm, Atom, Sublime Text, Emacs 또는 Vim를 사용 하는 데 사용 되는 경우는 어떻게 되나요?

VSCode 확장 [Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) 는 환경에서 집에 바로 느낌을 주는 데 도움이 될 수 있습니다.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Mac 바로 가기 키를 Windows 바로 가기 키에 매핑하는 방법

Windows 컴퓨터와 Macintosh에서 키보드 단추와 시스템 바로 가기 중 일부가 약간 다릅니다. Microsoft 지원의이 [키보드 매핑 가이드](https://support.microsoft.com/help/970299/keyboard-mappings-using-a-pc-keyboard-on-a-macintosh) 에서는 기본 사항을 다룹니다.
