---
title: 초보자를 위한 Windows에서 Python 사용 시작
description: Windows에서 Python을 처음 사용하는 분들이 쉽게 시작할 수 있도록 도와주는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python 학습, 초보자를 위한 windows 기반 python, microsoft store에서 python 설치, vs code를 사용하는 python, windows의 pygame
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 688ae004dad8653e70d86b3b91652b6898c1e9d3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74881281"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>초보자를 위한 Windows에서 Python 사용 시작

다음은 Windows 10을 사용하는 Python에 관심이 있는 초보자를 위한 단계별 가이드입니다.

## <a name="set-up-your-development-environment"></a>개발 환경 설정

Python을 처음 접하는 초보자는 [Microsoft Store에서 Python 설치](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)를 권장합니다. Microsoft Store를 통해 설치하면 기본 Python3 인터프리터를 사용하지만, 자동 업데이트를 제공할 뿐 아니라 현재 사용자의 PATH 설정까지 처리합니다(관리자가 액세스할 필요 없음). 교육 환경에 속해 있거나 머신에서 사용 권한 또는 관리 액세스를 제한하는 조직에 소속된 경우에 특히 유용합니다.

**웹 개발**에 Windows 기반 Python을 사용하는 경우 개발 환경에 다른 설정을 사용하는 것이 좋습니다. Windows에 직접 설치하는 대신, Linux용 Windows 하위 시스템을 통해 Python을 설치하고 사용하는 것이 좋습니다. 도움말은 [웹 개발을 위해 Windows에서 Python 사용 시작](./web-frameworks.md) 가이드를 참조하세요. 운영 체제에서 일반적인 작업을 자동화하는 데 관심이 있는 경우 [스크립팅 및 자동화를 위해 Windows에서 Python 사용 시작](./scripting.md) 가이드를 참조하세요. 고급 시나리오의 경우(예: Python의 설치 파일을 액세스/수정하거나, 이진 파일을 복사하거나, Python DLL을 직접 사용해야 하는 시나리오) [python.org](https://www.python.org/downloads/)에서 특정 Python 릴리스를 직접 다운로드하거나 Anaconda, Jython, PyPy, WinPython, IronPython 등과 같은 [대체 구현](https://www.python.org/download/alternatives)을 설치하는 것이 좋습니다. 대체 구현을 선택하는 특별한 이유가 있는 고급 Python 프로그래머인 경우에만 이를 추천합니다.

## <a name="install-python"></a>Python 설치

Microsoft Store를 사용하여 Python을 설치하는 방법은 다음과 같습니다.

1. **시작** 메뉴(왼쪽 아래 Windows 아이콘)로 이동하여 "Microsoft Store"를 입력하고, 링크를 선택하여 스토어를 엽니다.

2. 스토어가 열리면 오른쪽 위 메뉴에서 **검색**을 선택하고 "Python"을 입력합니다. 앱 아래의 결과에서 "Python 3.7"을 엽니다. **가져오기**를 선택합니다.

3. Python에서 다운로드 및 설치 프로세스를 완료하면 **시작** 메뉴(왼쪽 아래 Windows 아이콘)를 사용하여 Windows PowerShell을 엽니다. PowerShell이 열리면 `Python --version`을 입력하여 머신에 Python3가 설치되었는지 확인합니다.

4. Python의 Microsoft Store 설치에는 표준 패키지 관리자인 **pip**가 포함되어 있습니다. pip를 사용하면 Python 표준 라이브러리에 포함되지 않은 추가 패키지를 설치하고 관리할 수 있습니다. 패키지의 설치 및 관리에 사용할 수 있는 pip도 있는지 확인하려면 `pip --version`을 입력합니다.

## <a name="install-visual-studio-code"></a>Visual Studio Code 설치

VS Code를 텍스트 편집기/IDE(통합 개발 환경)로 사용하면 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)(코드 완성 지원), [Lint](https://code.visualstudio.com/docs/python/linting)(코드에서 오류 방지), [디버그 지원](https://code.visualstudio.com/docs/python/debugging)(코드를 실행한 후 코드에서 오류 검색), [코드 조각](https://code.visualstudio.com/docs/editor/userdefinedsnippets)(재사용 가능한 작은 코드 블록에 대한 템플릿) 및 [단위 테스트](https://code.visualstudio.com/docs/python/unit-testing)(다양한 유형의 입력을 사용하여 코드의 인터페이스를 테스트)의 장점을 활용할 수 있습니다.

VS Code에는 Windows 명령 프롬프트, PowerShell 또는 원하는 도구를 사용하여 Python 명령줄을 열고 코드 편집기와 명령줄 간에 원활한 워크플로를 설정할 수 있는 [기본 제공 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal)이 포함되어 있습니다.

1. VS Code를 설치하려면 Windows용 VS Code를 다운로드하여 설치합니다([https://code.visualstudio.com](https://code.visualstudio.com)).

2. VS Code가 설치되면 Python 확장도 설치해야 합니다. Python 확장을 설치하려면 [VS Code Marketplace 링크](https://marketplace.visualstudio.com/items?itemName=ms-python.python)를 선택하거나 VS Code를 열고 확장 메뉴(Ctrl+Shift+X)에서 **Python**을 검색합니다.

3. Python은 해석형 언어이며, Python 코드를 실행하려면 어떤 인터프리터를 사용할 것인지 VS Code에 알려주어야 합니다. 다른 항목을 선택해야 하는 특별한 이유가 없다면 Python 3.7를 사용하는 것이 좋습니다. Python 확장을 설치했으면 **명령 팔레트**를 열고(Ctrl+Shift+P) **Python: 인터프리터 선택** 명령을 입력하기 시작한 다음, 명령을 선택하여 Python 3 인터프리터를 선택합니다. 사용 가능한 경우 아래쪽 상태 표시줄에서 **Python 환경 선택** 옵션을 사용할 수도 있습니다(선택한 인터프리터가 이미 표시될 수 있음). 이 명령은 가상 환경을 포함하여 VS Code가 자동으로 찾을 수 있는 사용 가능한 인터프리터 목록을 표시합니다. 원하는 인터프리터가 보이지 않으면 [Python 환경 구성](https://code.visualstudio.com/docs/python/environments)을 참조하세요.

    ![VS Code에서 Python 인터프리터 선택](../images/interpreterselection.gif)

4. VS Code에서 터미널을 열려면 **보기** > **터미널**을 선택하거나 바로 가기 키 **Ctrl+`** (백틱 문자 사용)을 사용합니다. 기본 터미널은 PowerShell입니다.

5. VS Code 터미널 내에서 간단하게 `python` 명령을 입력하여 Python을 엽니다.

6. `print("Hello World")`를 입력하여 Python 인터프리터를 사용해 봅니다. Python이 "Hello World" 문을 반환합니다.

    ![VS Code의 Python 명령줄](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Git 설치(선택 사항)

Python 코드를 다른 사람과 협업할 생각이거나 GitHub 같은 오픈 소스 사이트에 프로젝트를 호스팅할 계획인 분들을 위해 VS Code는 [Git를 사용한 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적하며, UI에 바로 빌드된 일반적인 Git 명령(추가, 커밋, 푸시, 끌어오기)를 포함하고 있습니다. 소스 제어 패널을 지원하려면 먼저 Git를 설치해야 합니다.

1. [git-scm 웹 사이트 ](https://git-scm.com/download/win)에서 Git for Windows를 다운로드하여 설치합니다.

2. Git 설치의 설정에 대한 일련의 질문을 하는 설치 마법사가 포함되어 있습니다. 기본 설정을 변경해야 하는 특별한 이유가 없다면 모두 기본 설정을 사용하는 것이 좋습니다.

3. 이전에 Git를 사용한 경험이 없는 경우 [GitHub 가이드](https://guides.github.com/)를 보면 시작하는 데 도움이 될 수 있습니다.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Python 기본 사항을 설명하는 Hello World 자습서

Python을 개발한 Guido van Rossum의 말에 따르면, Python은 "고급 프로그래밍 언어이며, 핵심 디자인 철학은 프로그래머가 코드 몇 줄로 개념을 표현할 수 있는 코드 가독성 및 구문입니다."

Python은 해석형 언어입니다. 개발자가 작성하는 코드를 컴퓨터의 프로세서에서 실행하려면 기계어 코드로 변환해야 하는 컴파일러형 언어와 달리, Python 코드는 인터프리터에 직접 전달되고 바로 실행됩니다. 코드를 입력하고 실행하면 됩니다. 직접 경험해 보겠습니다.

1. PowerShell 명령줄을 열고, `python`을 입력하여 Python 3 인터프리터를 실행합니다. (일부 지침에서는 `py` 또는 `python3` 명령을 사용합니다. 두 명령 역시 작동합니다.) 보다 큼 기호가 3개 있는 >>> 프롬프트가 표시되므로 성공했다는 것을 알 수 있습니다.

2. Python에서 문자열을 수정할 수 있는 여러 기본 제공 메서드가 있습니다. `variable = 'Hello World!'` 명령을 사용하여 변수를 만듭니다. Enter 키를 눌러 새 줄을 시작합니다.

3. `print(variable)` 명령을 사용하여 변수를 출력합니다. "Hello World!" 텍스트가 표시됩니다.

4. `len(variable)` 명령을 사용하여 문자열 변수의 길이와 사용된 문자 수를 확인합니다. 12개 문자가 사용된 것으로 표시됩니다. (빈 공간은 전체 길이에서 문자로 계산됩니다.)

5. `variable.upper()` 명령을 사용하여 문자열 변수를 대문자로 변환합니다. 이제 `variable.lower()` 명령을 사용하여 문자열 변수를 소문자로 변환합니다.

6. `variable.count("l")` 명령을 사용하여 문자열 변수에 문자 "l"가 사용된 횟수를 계산합니다.

7. 문자열 변수에서 특정 문자를 검색합니다. `variable.find("!")` 명령을 사용하여 느낌표를 찾아보겠습니다. 문자열의 11번째 위치에서 느낌표를 찾았다고 표시됩니다.

8. `variable.replace("!", "?")` 명령을 사용하여 느낌표를 물음표로 바꿉니다.

9. Python을 종료하려면 `exit()`, `quit()`를 입력하거나 Ctrl-Z 키를 누릅니다.

![이 자습서의 PowerShell 스크린샷](../images/hello-world-basics.png)

Python의 기본 제공 문자열 수정 방법 중 일부를 사용해 보았습니다. 이제 VS Code에서 Python 프로그램 파일을 만들고 실행하겠습니다.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>VS Code에서 Python을 사용하는 방법에 대한 Hello World 자습서

VS Code 팀에서 Python을 사용하여 Hello World 프로그램을 만들고, 프로그램 파일을 실행하고, 디버거를 구성 및 실행하고, *matplotlib* 및 *numpy* 같은 패키지를 설치하여 가상 환경 내에서 그래픽 플롯을 만드는 방법을 안내하는 유용한 [Python 시작](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) 자습서를 만들었습니다.

1. PowerShell을 열고 "hello"라는 빈 폴더를 만든 다음, 이 폴더로 이동하여 VS Code에서 이 폴더를 엽니다.

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. VS Code가 열리고 왼쪽 **탐색기** 창에 새 *hello* 폴더를 표시되면 **Ctrl+`** (백틱 문자 사용) 키를 누르거나 **보기** > **터미널**을 선택하여 VS Code의 아래쪽 패널에서 명령줄 창을 엽니다. 폴더에서 VS Code를 시작하면 해당 폴더는 "작업 영역"이 됩니다. VS Code는 해당 작업 영역과 관련된 설정을 .vscode/settings.json으로 저장하며, 이 설정은 전역적으로 저장되는 사용자 설정과는 별개입니다.

3. VS Code 문서의 [Python Hello World 소스 코드 파일 만들기](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file) 자습서를 계속 진행합니다.

## <a name="create-a-simple-game-with-pygame"></a>Pygame으로 간단한 게임 만들기

![샘플 게임을 실행하는 Pygame](../images/pygame-shmup.jpg)

Pygame은 인기 있는 게임 개발용 Python 패키지로, 재미있는 게임을 만들면서 프로그래밍을 배울 수 있습니다. Pygame은 새 창에 그래픽을 표시하므로 WSL의 명령줄 전용 방식에서는 작동하지 않습니다. 그러나 이 자습서에 설명된 대로 Microsoft Store를 통해 Python을 설치한 경우에는 정상적으로 작동합니다.

1. Python을 설치한 후에는 `python -m pip install -U pygame --user` 명령을 입력하여 명령줄에서(또는 VS Code 내의 터미널에서) pygame을 설치합니다.

2. `python -m pygame.examples.aliens` 명령으로 샘플 게임을 실행하여 설치를 테스트합니다.

3. 모든 것이 정상이면 게임에서 창이 하나 열립니다. 게임 플레이를 마쳤으면 창을 닫습니다.

게임 작성을 시작하는 방법은 다음과 같습니다.

1. PowerShell(또는 Windows 명령 프롬프트)을 열고 "bounce"라는 빈 폴더를 만듭니다. 이 폴더로 이동하여 "bounce.py"라는 파일을 만듭니다. 다음과 같이 VS Code에서 폴더를 엽니다.

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. VS Code를 사용하여 다음 Python 코드를 입력합니다(또는 복사하여 붙여넣기).

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. `bounce.py`로 저장합니다.

4. PowerShell 터미널에서 `python bounce.py`를 입력하여 파일을 실행합니다.

    ![다음 큰 항목을 실행하는 Pygame](../images/pygame.jpg)

일부 숫자를 조정하고 튀어 오르는 공에 어떤 영향이 있는지 살펴봅니다.

[pygame.org](http://www.pygame.org)에서 Pygame을 사용하여 게임을 작성하는 방법에 대해 더 알아보세요.

## <a name="resources-for-continued-learning"></a>지속적인 학습을 위한 리소스

Windows에서 Python으로 개발하는 방법을 계속 학습할 수 있도록 다음 리소스를 권장합니다.

### <a name="online-courses-for-learning-python"></a>Python 학습을 위한 온라인 과정

- [Microsoft Learn의 Python 소개](https://docs.microsoft.com/learn/modules/intro-to-python/): 대화형 Microsoft Learn 플랫폼을 사용해 보고 기본 Python 코드를 작성하고, 변수를 선언하고, 콘솔 입력 및 출력을 작업하는 방법에 대한 기본 사항을 다루는 이 모듈을 완료하기 위한 경험을 쌓을 수 있습니다. 대화형 샌드박스 환경은 아직 Python 개발 환경을 설정하지 않은 사람들이 개발을 시작하기에 좋은 장소입니다.

- [Pluralsight의 Python: 8개 과정, 29시간](https://app.pluralsight.com/paths/skills/python): Pluralsight의 Python 학습 경로는 학습자의 기술을 측정하고 격차를 알아볼 수 있는 도구를 포함하여 Python과 관련된 다양한 토픽을 다루는 온라인 과정을 제공합니다.

- [LearnPython.org 자습서](https://www.learnpython.org/): DataCamp 사용자들이 제공하는 이 무료 대화형 Python 자습서를 사용하면 아무 것도 설치하거나 설정할 필요 없이 Python 학습을 시작할 수 있습니다.

- [Python.org 자습서](https://docs.python.org/3/tutorial/index.html): Python 언어 및 시스템의 기본 개념과 기능을 자유로운 형식으로 소개합니다.

- [Lynda.com의 Python 학습](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): Python에 대한 기본적인 내용을 소개합니다.

### <a name="working-with-python-in-vs-code"></a>VS Code에서 Python 작업

- [VS Code에서 Python 편집](https://code.visualstudio.com/docs/python/editing): 동작을 사용자 지정하는 방법 또는 끄는 방법을 포함하여 VS Code가 Python에 제공하는 자동 완성 및 IntelliSense 지원을 활용하는 방법을 자세히 알아보세요.

- [Python Lint](https://code.visualstudio.com/docs/python/linting): Lint는 코드의 잠재적 오류를 분석하는 프로그램을 실행하는 프로세스입니다. VS Code가 Python에 제공하는 다양한 형태의 Lint 지원 및 설정 방법을 알아봅니다.

- [Python 디버깅](https://code.visualstudio.com/docs/python/debugging): 디버깅은 컴퓨터 프로그램의 오류를 식별하고 제거하는 프로세스입니다. 이 문서에서는 VS Code를 사용하여 Python에 대한 디버깅을 초기화하고 구성하는 방법, 중단점을 설정하고 유효성을 검사하고, 로컬 스크립트를 연결하고, 다양한 앱 형식에 대해 또는 원격 컴퓨터에서 디버깅을 수행하는 방법, 그리고 몇 가지 기본적인 문제 해결 방법을 다룹니다.

- [Python 단위 테스트](https://code.visualstudio.com/docs/python/unit-testing): 단위 테스트의 의미, 예제 연습, 테스트 프레임워크 사용, 테스트 생성 및 실행, 테스트 디버깅, 테스트 구성 설정에 대해 설명하는 배경 정보를 다룹니다. 
