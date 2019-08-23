---
title: 초보자를 위한 Windows에서 Python 사용 시작
description: Windows에서 Python을 처음 사용 하는 경우 시작 하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python, 초보자를 위한 windows의 python, microsoft store를 사용 하 여 python 설치, vs code를 사용 하는 python, windows에서 pygame
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 5c1861d76a98ff76b130f3012d730980482cda75
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959090"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>초보자를 위한 Windows에서 Python 사용 시작

다음은 Windows 10을 사용 하는 Python 학습에 관심이 있는 초보자를 위한 단계별 가이드입니다.

## <a name="set-up-your-development-environment"></a>개발 환경 설정

Python을 처음 접하는 초보자를 위해 [Microsoft Store에서 python을 설치](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)하는 것이 좋습니다. Microsoft Store를 통해 설치 하는 경우 기본 Python3 인터프리터를 사용 하지만, 자동 업데이트를 제공 하는 것 외에도 현재 사용자에 대 한 경로 설정 설정 (관리자 액세스 필요 방지)을 처리 합니다. 이는 시스템에서 사용 권한 또는 관리 액세스를 제한 하는 교육 환경 또는 조직의 일부에 있는 경우에 특히 유용 합니다.

Windows에서 **웹 개발**에 Python을 사용 하는 경우 개발 환경에 대해 다른 설정을 사용 하는 것이 좋습니다. Windows에 직접 설치 하는 대신 Linux 용 Windows 하위 시스템을 통해 Python을 설치 하 고 사용 하는 것이 좋습니다. 도움말은 다음을 참조 하세요. [Windows에서 웹 개발용 Python을 사용 하 여 시작](./python-for-web.md)하세요. 운영 체제에서 일반적인 작업을 자동화 하는 데 관심이 있는 경우 다음 가이드를 참조 하세요. [Windows에서 Python을 사용 하 여 스크립팅 및 자동화를 시작](./python-for-scripting.md)합니다. 일부 고급 시나리오의 경우 (예: Python의 설치 된 파일에 액세스/수정, 이진 파일 복사 또는 Python Dll을 직접 사용 해야 하는 경우), [python.org](https://www.python.org/downloads/) 에서 직접 특정 Python 릴리스를 다운로드 하거나 설치를 고려 하는 것이 좋습니다. Anaconda, Jython, PyPy, WinPython, IronPython 등의 [대안](https://www.python.org/download/alternatives)입니다. 대체 구현을 선택 하는 특별 한 이유가 있는 고급 Python 프로그래머 인 경우에만이를 권장 합니다.

## <a name="install-python"></a>Python 설치

Microsoft Store를 사용 하 여 Python을 설치 하려면:

1. **시작** 메뉴 (왼쪽 아래에 있는 Windows 아이콘)로 이동 하 고 "Microsoft Store"을 입력 한 다음 링크를 선택 하 여 스토어를 엽니다.

2. 저장소가 열리면 오른쪽 위에 있는 메뉴에서 **검색** 을 선택 하 고 "Python"을 입력 합니다. 앱 아래의 결과에서 "Python 3.7"을 엽니다. **가져오기**를 선택 합니다.

3. Python에서 다운로드 및 설치 프로세스를 완료 한 후 **시작** 메뉴 (왼쪽 아래에 있는 windows 아이콘)를 사용 하 여 windows PowerShell을 엽니다. PowerShell이 열리면를 입력 `Python --version` 하 여 Python3이 컴퓨터에 설치 되어 있는지 확인 합니다.

4. Python의 Microsoft Store 설치에는 표준 패키지 관리자 인 **pip**가 포함 되어 있습니다. Pip를 사용 하면 Python 표준 라이브러리에 포함 되지 않은 추가 패키지를 설치 하 고 관리할 수 있습니다. 패키지를 설치 하 고 관리 하는 데 사용할 수 있는 pip가 있는지도 `pip --version`확인 하려면을 입력 합니다.

## <a name="install-visual-studio-code"></a>Visual Studio Code 설치

텍스트 편집기/IDE (통합 개발 환경)로 VS Code를 사용 하 여 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (코드 완성 지원), [lint](https://code.visualstudio.com/docs/python/linting) (코드에 오류를 방지 하는 데 도움이 됨), [디버그 지원](https://code.visualstudio.com/docs/python/debugging) (에서 오류를 찾을 수 있도록 지원)을 활용할 수 있습니다. 코드를 실행 한 후에 코드 조각), [코드 조각](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (재사용 가능한 작은 코드 블록에 대 한 템플릿) 및 [단위 테스트](https://code.visualstudio.com/docs/python/unit-testing) (다른 형식의 입력으로 코드 인터페이스 테스트)가 있습니다.

또한 VS Code에는 Windows 명령 프롬프트, PowerShell 또는 원하는 모든 항목을 사용 하 여 Python 명령줄을 열어 코드 편집기와 명령줄 간에 원활한 워크플로를 설정할 수 있는 [기본 제공 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal) 이 포함 되어 있습니다.

1. VS Code를 설치 하려면 Windows [https://code.visualstudio.com](https://code.visualstudio.com)용 VS Code를 다운로드 합니다.

2. Python은 해석 된 언어 이며 Python 코드를 실행 하기 위해 사용할 인터프리터를 VS Code에 알려야 합니다. 다른 항목을 선택 해야 하는 특별 한 이유가 없다면 Python 3.7를 사용 하는 것이 좋습니다. **명령 팔레트** (Ctrl + Shift + P)를 열고 python 명령 **입력을 시작 하 여 python 3 인터프리터를 선택 합니다. 검색에** 인터프리터를 선택 하 고 명령을 선택 합니다. 사용 가능한 경우 아래쪽 상태 표시줄에서 **Python 환경 선택** 옵션을 사용할 수도 있습니다 (이미 선택 된 인터프리터를 표시할 수 있음). 명령은 가상 환경을 포함 하 여 자동으로 찾을 수 VS Code 있는 사용 가능한 인터프리터 목록을 표시 합니다. 원하는 인터프리터가 표시 되지 않으면 [Python 환경 구성](https://code.visualstudio.com/docs/python/environments)을 참조 하세요.

    ![VS Code에서 Python 인터프리터를 선택 합니다.](../../images/interpreterselection.gif)

3. VS Code에서 터미널을 열려면**터미널** **보기** > 를 선택 하거나, 바로 가기 **Ctrl + '** (억음 문자 사용)를 사용 합니다. 기본 터미널은 PowerShell입니다.

4. VS Code 터미널 내에서 단순히 명령을 입력 하 여 Python을 엽니다.`python`

5. 을 입력 `print("Hello World")`하 여 Python 인터프리터를 시도 합니다. Python은 "Hello World" 문을 반환 합니다.

    ![VS Code의 Python 명령줄](../../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Git 설치 (선택 사항)

Python 코드에서 다른 사용자와 공동으로 작업 하거나, GitHub와 같은 오픈 소스 사이트에서 프로젝트를 호스트 하는 경우 VS Code [Git에서 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원 합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적 하 고 UI에 바로 적용 되는 일반적인 Git 명령 (추가, 커밋, 푸시, 끌어오기)을 포함 합니다. 먼저 Git를 설치 하 여 원본 제어판을 켭니다.

1. Git [scm 웹 사이트](https://git-scm.com/download/win)에서 Windows 용 git를 다운로드 하 여 설치 합니다.

2. Git 설치의 설정에 대 한 일련의 질문을 하는 설치 마법사가 포함 되어 있습니다. 항목을 변경 해야 하는 특별 한 이유가 없다면 모든 기본 설정을 사용 하는 것이 좋습니다.

3. 이전에 Git로 작업 한 적이 없는 경우 [GitHub 가이드](https://guides.github.com/) 를 사용 하 여 시작 하는 데 도움을 받을 수 있습니다.

## <a name="hello-world-tutorial-for-some-python-basics"></a>일부 Python 기본 사항에 대 한 Hello World 자습서

Python은 해당 작성자 Guido van Rossum에 따라 "개략적인 프로그래밍 언어 이며, 핵심 디자인 철학은 코드 가독성 및 프로그래머 들이 몇 줄의 코드에서 개념을 표현할 수 있도록 하는 구문에 대 한 것입니다."

Python은 해석 된 언어입니다. 사용자가 작성 하는 코드를 컴퓨터의 프로세서에서 실행 하기 위해 기계어 코드로 변환 해야 하는 컴파일된 언어와 달리 Python 코드는 인터프리터에 바로 전달 되 고 직접 실행 됩니다. 코드를 입력 하 고 실행 하기만 하면 됩니다. 사용해 보겠습니다.

1. PowerShell 명령줄을 열고를 입력 `python` 하 여 Python 3 인터프리터를 실행 합니다. (일부 지침은 명령 `py` 또는 `python3`를 사용 하는 것이 좋습니다.) 기호가 두 개 이상 포함 된 > > > 프롬프트가 표시 되기 때문에 성공 했다는 것을 알 수 있습니다.

2. Python에서 문자열을 수정할 수 있도록 하는 몇 가지 기본 제공 메서드가 있습니다. 을 사용 하 여 `variable = 'Hello World!'`변수를 만듭니다. 새 줄에 대해 Enter 키를 누릅니다.

3. 을 사용 하 여 `print(variable)`변수를 출력 합니다. 텍스트 "Hello World!"이 표시 됩니다.

4. 을 사용 하 여 문자열 변수의 `len(variable)`길이, 사용 되는 문자 수를 확인 합니다. 12 개의 문자가 사용 된 것으로 표시 됩니다. 빈 공간은 전체 길이에서 문자로 계산 됩니다.

5. 문자열 변수를 대문자 문자로 `variable.upper()`변환 합니다. 이제 문자열 변수를 소문자 문자로 `variable.lower()`변환 합니다.

6. 문자열 변수 `variable.count("l")`에서 문자 "l"이 사용 되는 횟수를 계산 합니다.

7. 문자열 변수에서 특정 문자를 검색 하 고를 사용 `variable.find("!")`하 여 느낌표를 찾습니다. 그러면 문자열의 11 번째 위치 문자에 느낌표가 표시 됩니다.

8. 느낌표를 물음표: `variable.replace("!", "?")`로 바꿉니다.

9. Python을 종료 하려면, `exit()` `quit()`또는를 입력 하 여 Ctrl + Z를 선택 합니다.

![이 자습서의 PowerShell 스크린샷](../../images/hello-world-basics.png)

Python의 기본 제공 문자열 수정 방법 중 일부를 사용 하는 것이 좋습니다. 이제 Python 프로그램 파일을 만들고 VS Code를 사용 하 여 실행 해 봅니다.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>VS Code에서 Python을 사용 하기 위한 Hello World 자습서

VS Code 팀은 을 사용하여 Hello World 프로그램을 만들고, 프로그램 파일을 실행 하 고, 디버거를 구성 하 고 실행 하 고, *matplotlib* 와 *numpy* 가상 환경 내에서 그래픽 플롯을 만듭니다. 같은 패키지를 설치 하는 방법을 안내 하는 [Getting Started with Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) 자습서를 활용 하 고 있습니다.

1. PowerShell을 열고 "hello" 라는 빈 폴더를 만든 다음이 폴더로 이동 하 여 VS Code에서 엽니다.

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. VS Code 열리면 왼쪽 **탐색기** 창에 새 *hello* 폴더를 표시 하 고 **Ctrl + '** (억음 문자 사용)를 누르거나 **보기**  > 를선택하여VSCode의아래쪽패널에서명령줄창을엽니다. **터미널**. 폴더에서 VS Code를 시작 하 여 해당 폴더는 "작업 영역"이 됩니다. VS Code에는 전역으로 저장 된 사용자 설정과는 별개의. a s o Code/settings에서 해당 작업 영역과 관련 된 설정이 저장 됩니다.

3. VS Code 문서에서 자습서를 계속 진행 합니다. [Python Hello World 소스 코드 파일을 만듭니다](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Pygame를 사용 하 여 간단한 게임 만들기

![샘플 게임을 실행 하는 Pygame](../../images/pygame-shmup.jpg)

Pygame는 게임을 작성 하는 데 사용 하는 인기 있는 Python 패키지로 서 흥미로운 작업을 만드는 동시에 프로그래밍을 배우는 데 사용 됩니다. Pygame는 새 창에 그래픽을 표시 하므로 WSL의 명령줄 전용 방법에서 작동 하지 않습니다. 그러나이 자습서에 설명 된 대로 Microsoft Store를 통해 Python을 설치한 경우에는 제대로 작동 합니다.

1. Python을 설치한 후에는를 입력 `python -m pip install -U pygame --user`하 여 명령줄에서 pygame (또는 VS Code 내 터미널)를 설치 합니다.

2. 샘플 게임을 실행 하 여 설치를 테스트 합니다.`python -m pygame.examples.aliens`

3. 모든 것이 게임에서 창이 열립니다. 재생이 완료 되 면 창을 닫습니다.

게임 작성을 시작 하는 방법은 다음과 같습니다.

1. PowerShell 또는 Windows 명령 프롬프트를 열고 "바운스" 라는 빈 폴더를 만듭니다. 이 폴더로 이동 하 여 "bounce.py" 라는 파일을 만듭니다. VS Code에서 폴더를 엽니다.

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. VS Code를 사용 하 여 다음 Python 코드를 입력 하거나 복사 하 여 붙여 넣습니다.

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

3. 로 `bounce.py`저장 합니다.

4. PowerShell 터미널에서을 입력 `python bounce.py`하 여 실행 합니다.

    ![다음 큰 사물을 실행 하는 Pygame](../../images/pygame.jpg)

일부 숫자를 조정 하 여 바운스 구슬에 미치는 영향을 확인 하세요.

Pygame에서 게임 작성에 대 한 자세한 내용은 [pygame.org](http://www.pygame.org)를 참조 하세요.

## <a name="resources-for-continued-learning"></a>지속적인 학습을 위한 리소스

Windows에서 Python 개발에 대해 계속 학습할 때 지원 하기 위해 다음 리소스를 권장 합니다.

### <a name="online-courses-for-learning-python"></a>Python 학습을 위한 온라인 과정

- [Microsoft Learn의 Python 소개](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/): 대화형 Microsoft Learn 플랫폼을 사용해 보고, 기본 Python 코드를 작성 하 고, 변수를 선언 하 고, 콘솔 입력 및 출력을 사용 하는 방법에 대 한 기본 사항을 설명 하는이 모듈을 완료 하기 위한 경험 사항을 알아봅니다. 대화형 샌드박스 환경을 사용 하면 Python 개발 환경이 아직 설정 되지 않은 사람들에 게이를 시작할 수 있습니다.

- [Pluralsight의 Python: 8 과정, 29 시간](https://app.pluralsight.com/paths/skills/python): Pluralsight의 Python 학습 경로는 기술을 측정 하 고 차이를 찾는 도구를 비롯 하 여 Python과 관련 된 다양 한 주제를 다루는 온라인 과정을 제공 합니다.

- [LearnPython.org 자습서](https://www.learnpython.org/): DataCamp의 사람들에 게 제공 되는 무료 대화형 Python 자습서를 사용 하 여 모든 것을 설치 하거나 설정할 필요 없이 Python 학습을 시작 하세요.

- [Python.org 자습서](https://docs.python.org/3/tutorial/index.html): 에서는 Python 언어 및 시스템의 기본 개념 및 기능을 비공식적으로 소개 합니다.

- [Lynda.com에서 Python 학습](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): Python에 대 한 기본적인 소개

### <a name="working-with-python-in-vs-code"></a>VS Code에서 Python 작업

- [VS Code에서 Python 편집](https://code.visualstudio.com/docs/python/editing): Behvior를 사용자 지정 하는 방법을 비롯 하 여 Python에 대 한 VS Code의 자동 완성 및 IntelliSense 지원을 활용 하는 방법에 대해 자세히 알아보세요. 또는 그냥 꺼야 합니다.

- [Lint Python](https://code.visualstudio.com/docs/python/linting): Lint는 잠재적 오류에 대 한 코드를 분석 하는 프로그램을 실행 하는 프로세스입니다. Python에 제공 VS Code 다양 한 형태의 lint 지원 및 설정 하는 방법에 대해 알아봅니다.

- [Python 디버깅](https://code.visualstudio.com/docs/python/debugging): 디버깅은 컴퓨터 프로그램에서 오류를 식별 하 고 제거 하는 프로세스입니다. 이 문서에서는 VS Code를 사용 하 여 Python에 대해 디버깅을 초기화 하 고 구성 하는 방법, 중단점을 설정 하 고 유효성을 검사 하 고, 로컬 스크립트를 연결 하 고, 다양 한 앱 형식 또는 원격 컴퓨터에서 디버깅을 수행 하 고, 몇 가지 기본적인 문제 해결

- [Python 유닛 테스트](https://code.visualstudio.com/docs/python/unit-testing): 단위 테스트의 의미, 예제 연습, 테스트 프레임 워크 활성화, 테스트 생성 및 실행, 테스트 디버깅, 테스트 구성 설정 등에 대해 설명 하는 배경 정보를 제공 합니다. 
