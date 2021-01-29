---
title: Windows에서 Python을 사용한 스크립팅 및 자동화
description: Windows에서 스크립팅, 자동화 및 시스템 관리에 Python을 사용하는 방법을 설명합니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python 시스템 관리, python 파일 자동화, windows에서 python 스크립트, windows에서 python 설정, windows의 python 개발자 환경, windows의 python 개발 환경, powershell을 사용하는 python, 파일 시스템 작업을 위한 python 스크립트
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: a8f13243f3501b2af42d38c13bff580be2e5b42a
ms.sourcegitcommit: 8040760f5520bd1732c39aedc68144c4496319df
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98691328"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>스크립팅 및 자동화를 위해 Windows에서 Python 사용 시작

다음은 개발자 환경을 설정하고 Windows에서 파일 시스템 작업을 스크립팅 및 자동화하는 데 Python을 사용하는 방법을 안내하는 단계별 가이드입니다.

> [!NOTE]
> 이 문서에서는 Windows 중앙 집중식 방법에서 파일 시스템을 검색하고, 인터넷에 액세스하고, 파일 형식을 구문 분석하는 것처럼 여러 플랫폼에서 작업을 자동화할 수 있는 Python의 유용한 라이브러리를 사용하도록 환경을 설정하는 방법을 다룹니다. Windows 관련 작업의 경우 Python용 C 호환 외래 함수 라이브러리인 [ctypes](https://docs.python.org/3/library/ctypes.html), Windows 레지스트리 API를 Python에 노출하는 함수인 [winreg](https://docs.python.org/3/library/winreg.html), Python에서 Windows Runtime API에 액세스할 수 있게 해주는 [Python/WinRT](https://pypi.org/project/winrt/)를 살펴보세요.

## <a name="set-up-your-development-environment"></a>개발 환경 설정

Python을 사용하여 파일 시스템 작업을 수행하는 스크립트를 작성할 때 [Microsoft Store에서 Python을 설치](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)하는 것이 좋습니다. Microsoft Store를 통해 설치하면 기본 Python3 인터프리터를 사용하지만, 자동 업데이트를 제공할 뿐 아니라 현재 사용자의 PATH 설정까지 처리합니다(관리자가 액세스할 필요 없음).

Windows에서 **웹 개발** 에 Python을 사용하는 경우 Linux용 Windows 하위 시스템을 사용하는 다른 설치 프로그램을 이용하는 것이 좋습니다. 자세한 설명은 [웹 개발을 위해 Windows에서 Python 사용 시작](./web-frameworks.md) 가이드를 참조하세요. Python을 처음 접하는 경우 [초보자를 위한 Windows에서 Python 사용 시작](./beginners.md) 가이드를 참조하세요. 고급 시나리오의 경우(예: Python의 설치 파일을 액세스/수정하거나, 이진 파일을 복사하거나, Python DLL을 직접 사용해야 하는 시나리오) [python.org](https://www.python.org/downloads/)에서 특정 Python 릴리스를 직접 다운로드하거나 Anaconda, Jython, PyPy, WinPython, IronPython 등과 같은 [대체 구현](https://www.python.org/download/alternatives)을 설치하는 것이 좋습니다. 대체 구현을 선택하는 특별한 이유가 있는 고급 Python 프로그래머인 경우에만 이를 추천합니다.

## <a name="install-python"></a>Python 설치

Microsoft Store를 사용하여 Python을 설치하는 방법은 다음과 같습니다.

1. **시작** 메뉴(왼쪽 아래 Windows 아이콘)로 이동하여 "Microsoft Store"를 입력하고, 링크를 선택하여 스토어를 엽니다.

2. 스토어가 열리면 오른쪽 위 메뉴에서 **검색** 을 선택하고 "Python"을 입력합니다. 앱 아래의 결과에서 "Python 3.7"을 엽니다. **가져오기** 를 선택합니다.

3. Python에서 다운로드 및 설치 프로세스를 완료하면 **시작** 메뉴(왼쪽 아래 Windows 아이콘)를 사용하여 Windows PowerShell을 엽니다. PowerShell이 열리면 `Python --version`을 입력하여 머신에 Python3가 설치되었는지 확인합니다.

4. Python의 Microsoft Store 설치에는 표준 패키지 관리자인 **pip** 가 포함되어 있습니다. pip를 사용하면 Python 표준 라이브러리에 포함되지 않은 추가 패키지를 설치하고 관리할 수 있습니다. 패키지의 설치 및 관리에 사용할 수 있는 pip도 있는지 확인하려면 `pip --version`을 입력합니다.

## <a name="install-visual-studio-code"></a>Visual Studio Code 설치

VS Code를 텍스트 편집기/IDE(통합 개발 환경)로 사용하면 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)(코드 완성 지원), [Lint](https://code.visualstudio.com/docs/python/linting)(코드에서 오류 방지), [디버그 지원](https://code.visualstudio.com/docs/python/debugging)(코드를 실행한 후 코드에서 오류 검색), [코드 조각](https://code.visualstudio.com/docs/editor/userdefinedsnippets)(재사용 가능한 작은 코드 블록에 대한 템플릿) 및 [단위 테스트](https://code.visualstudio.com/docs/python/unit-testing)(다양한 유형의 입력을 사용하여 코드의 인터페이스를 테스트)의 장점을 활용할 수 있습니다.

Windows용 VS Code를 다운로드하고 설치 지침([https://code.visualstudio.com](https://code.visualstudio.com))을 따릅니다.

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 확장 설치

VS Code 지원 기능을 활용하려면 Microsoft Python 확장을 설치해야 합니다. [자세한 정보를 알아보세요](https://code.visualstudio.com/docs/languages/python).

1. **Ctrl+Shift+X** 를 입력하거나 메뉴에서 **보기** > **확장** 으로 이동하여 VS Code 확장 창을 엽니다.

2. 위쪽의 **마켓플레이스에서 확장 검색** 상자에  **Python** 을 입력합니다.

3. **Python (ms-python.python) by Microsoft** 확장을 찾아 녹색 **설치** 단추를 선택합니다.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>VS Code에서 통합 PowerShell 터미널 열기

VS Code에는 PowerShell을 사용하여 Python 명령줄을 열고 코드 편집기와 명령줄 간에 원활한 워크플로를 설정할 수 있는 [기본 제공 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal)이 포함되어 있습니다.

1. VS Code에서 터미널을 열고, **보기** > **터미널** 을 선택하거나 바로 가기 키 **Ctrl+`** (백틱 문자 사용)을 사용합니다.

    > [!NOTE]
    > 기본 터미널은 PowerShell이지만, 변경해야 하는 경우 **Ctrl+Shift+P** 를 사용하여 팔레트 명령을 입력합니다. **터미널: 기본 셸 선택** 을 입력하면 PowerShell, 명령 프롬프트, WSL 등을 포함하고 있는 터미널 옵션 목록이 표시됩니다. 사용할 옵션을 선택하고, **Ctrl+Shift+`** (백틱 사용)을 입력하여 새 터미널을 만듭니다.

2. VS Code 터미널 내에서 `python`을 입력하여 Python을 엽니다.

3. `print("Hello World")`를 입력하여 Python 인터프리터를 사용해 봅니다. Python이 "Hello World" 문을 반환합니다.

    ![VS Code의 Python 명령줄](../images/python-in-vscode.png)

4. Python을 종료하려면 `exit()`, `quit()`를 입력하거나 Ctrl-Z 키를 누릅니다.

## <a name="install-git-optional"></a>Git 설치(선택 사항)

Python 코드를 다른 사람과 협업할 생각이거나 GitHub 같은 오픈 소스 사이트에 프로젝트를 호스팅할 계획인 분들을 위해 VS Code는 [Git를 사용한 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적하며, UI에 바로 빌드된 일반적인 Git 명령(추가, 커밋, 푸시, 끌어오기)를 포함하고 있습니다. 소스 제어 패널을 지원하려면 먼저 Git를 설치해야 합니다.

1. [git-scm 웹 사이트 ](https://git-scm.com/download/win)에서 Git for Windows를 다운로드하여 설치합니다.

2. Git 설치의 설정에 대한 일련의 질문을 하는 설치 마법사가 포함되어 있습니다. 기본 설정을 변경해야 하는 특별한 이유가 없다면 모두 기본 설정을 사용하는 것이 좋습니다.

3. 이전에 Git를 사용한 경험이 없는 경우 [GitHub 가이드](https://guides.github.com/)를 보면 시작하는 데 도움이 될 수 있습니다.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>파일 시스템 디렉터리의 구조를 표시하는 예제 스크립트

일반적인 시스템 관리 작업에는 상당한 시간이 걸릴 수 있지만, Python 스크립트를 사용하여 이러한 작업을 자동화하면 시간이 전혀 걸리지 않습니다. 예를 들어 Python은 컴퓨터 파일 시스템의 콘텐츠를 읽고 파일 및 디렉터리의 개요를 출력하거나, 한 디렉터리에서 다른 디렉터리로 폴더를 이동하거나, 수백 개의 파일 이름을 바꾸는 등의 작업을 수행할 수 있습니다. 일반적으로 이러한 작업을 수동으로 수행하려면 엄청나게 긴 시간이 걸릴 수 있습니다. 수동 대신 Python 스크립트를 사용하세요.

디렉터리 트리를 안내하고 디렉터리 구조를 표시하는 간단한 스크립트부터 시작하겠습니다.

1. **시작** 메뉴(왼쪽 아래 Windows 아이콘)를 사용하여 PowerShell을 엽니다.

2. 프로젝트에 대한 디렉터리를 만든(`mkdir python-scripts`) 다음, 해당 디렉터리를 엽니다(`cd python-scripts`).

3. 다음과 같이 예제 스크립트에 사용할 디렉터리를 몇 개 만듭니다.

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. 다음과 같이 이러한 디렉터리 내에 스크립트에 사용할 파일을 몇 개 만듭니다.

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. 다음과 같이 python-scripts 디렉터리에 새 python 파일을 만듭니다.

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. `code .`를 입력하여 VS Code에서 프로젝트를 엽니다.

7. **Ctrl+Shift+E** 를 입력하여(또는 메뉴를 사용하여 **보기** > **탐색기** 로 이동) VS Code 파일 탐색기 창을 열고 방금 만든 list-directory-contents.py 파일을 선택합니다. Microsoft Python 확장은 Python 인터프리터를 자동으로 로드합니다. VS Code 창의 아래쪽에서 로드된 인터프리터를 확인할 수 있습니다.

    > [!NOTE]
    > Python은 해석형 언어입니다. 즉, 가상 머신으로 작동하면서 물리적 컴퓨터를 에뮬레이션합니다. 사용할 수 있는 Python 인터프리터 유형으로는 Python 2, Python 3, Anaconda, PyPy 등이 있습니다. Python 코드를 실행하고 Python IntelliSense를 가져오려면 어떤 인터프리터를 사용할 것인지 VS Code에 알려주어야 합니다. 다른 인터프리터를 선택해야 하는 특별한 이유가 없는 한, VS Code에서 기본적으로 선택하는 인터프리터(이 예제에서는 Python 3)를 사용하는 것이 좋습니다. Python 인터프리터를 변경하려면 VS Code 창의 아래쪽에 있는 파란색 표시줄에 현재 표시된 인터프리터를 선택하거나, **명령 팔레트** 를 열고(Ctrl+Shift+P) **Python: 인터프리터 선택** 명령을 입력합니다. 그러면 현재 설치된 Python 인터프리터 목록이 표시됩니다. [Python 환경 구성에 대해 자세히 알아보세요](https://code.visualstudio.com/docs/python/environments).

    ![VS Code에서 Python 인터프리터 선택](../images/interpreterselection.gif)

8. 다음 코드를 list-directory-contents.py 파일에 붙여넣고 **저장** 을 선택합니다.

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. VS Code 통합 터미널을 열고(**Ctrl+`** , 백틱 문자 사용), 방금 Python 스크립트를 저장한 src 디렉터리를 입력합니다.

    ```powershell
    cd src
    ```

10. 다음 명령을 사용하여 PowerShell에서 스크립트를 실행합니다.

    ```powershell
    python3 .\list-directory-contents.py
    ```

    다음과 비슷한 출력이 표시됩니다.

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. Python에서 PowerShell 터미널에 `python3 list-directory-contents.py > food-directory.txt` 명령을 직접 입력하여 해당 파일 시스템 디렉터리 출력을 자체적인 텍스트 파일로 출력합니다.

축하합니다! 방금 만든 디렉터리와 파일을 읽고 Python을 사용하여 디렉터리 구조를 표시한 다음, 자체 텍스트 파일에 출력하는 자동화된 시스템 관리 스크립트가 완성되었습니다.

> [!NOTE]
> Microsoft Store에서 Python 3를 설치할 수 없는 경우 이 [이슈](https://github.com/MicrosoftDocs/windows-uwp/issues/2901)에서 이 샘플 스크립트의 경로 처리 방법에 대한 예제를 참조하세요.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>디렉터리의 모든 파일을 수정하는 예제 스크립트

이 예제에서는 방금 만든 파일과 디렉터리를 사용하며, 파일 이름 앞부분에 파일이 마지막으로 수정된 날짜를 추가하여 각 파일의 이름을 바꿉니다.

1. **python-scripts** 디렉터리의 **src** 폴더 안에서 스크립트에 대한 새 Python 파일을 만듭니다.

    ```powershell
    new-item update-filenames.py
    ```

2. update-filenames.py 파일을 열고 다음 코드를 파일에 붙여넣은 후 저장합니다.

    > [!NOTE]
    > os.getmtime은 쉽게 읽을 수 없는 타임스탬프(틱 단위)를 반환합니다. 우선 표준 날짜/시간 문자열로 변환해야 합니다.

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. update-filenames.py 스크립트를 실행하여 테스트(`python3 update-filenames.py`)한 다음, list-directory-contents.py 스크립트를 다시 실행합니다(`python3 list-directory-contents.py`).

4. 결과는 다음과 같습니다.

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. Python에서 PowerShell 터미널에 `python3 list-directory-contents.py > food-directory-last-modified.txt` 명령을 직접 입력하여 마지막으로 수정된 타임스탬프가 접두어로 추가된 새 파일 시스템 디렉터리 이름을 자체적인 텍스트 파일로 출력합니다.

이처럼 Python 스크립트를 사용하여 기본 시스템 관리 작업을 자동화하는 것은 흥미로운 작업입니다. 아직 배워야 할 내용이 한참 남았지만, 이 정도면 좋은 출발이라 하겠습니다. 학습을 계속할 수 있도록 아래에 추가 리소스를 공유해 두었습니다.

## <a name="additional-resources"></a>추가 리소스

- [Python Docs: 파일 및 디렉터리 액세스](https://docs.python.org/3.7/library/filesys.html): 파일 시스템을 작업하는 방법과 모듈을 사용하여 파일 속성을 읽고, 이식 가능한 방식으로 경로를 조작하고, 임시 파일을 만드는 방법에 대한 Python 설명서입니다.
- [Python 학습: String_Formatting 자습서](https://www.learnpython.org/en/String_Formatting): 문자열 서식 지정에 "%" 연산자를 사용하는 방법에 대해 자세히 알아봅니다.
- [알아야 하는 10가지 Python 파일 시스템 메서드](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2): `os` 및 `shutil`을 사용하여 파일과 폴더를 조작하는 방법에 대한 중간 문서입니다.
- [Python에 대한 Hitchhikers 가이드: 시스템 관리](https://docs.python-guide.org/scenarios/admin/): Python 관련 토픽에 대한 개요와 모범 사례를 제공하는 "독자적인 가이드"입니다. 이 섹션에서는 시스템 관리 도구 및 프레임워크를 다룹니다. 이 가이드는 GitHub에서 호스팅되므로 이슈를 제출하고 글을 작성할 수 있습니다.
