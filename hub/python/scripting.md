---
title: Windows에서 Python을 사용한 스크립팅 및 자동화
description: Windows에서 스크립팅, 자동화 및 시스템 관리를 위해 Python을 사용 하 여 시작 하는 방법입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python 시스템 관리, python 파일 자동화, windows에서 python 스크립트, windows에서 python 개발자 환경, windows의 python 개발자 환경, windows의 python 개발 환경, powershell을 사용한 python, python 스크립트 파일 시스템 작업
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 3f8a17de8121fed27e69442d5560f702a04c8e42
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314867"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Windows에서 Python을 사용 하 여 스크립팅 및 자동화를 시작 합니다.

다음은 개발자 환경을 설정 하 고 Windows에서 파일 시스템 작업을 스크립팅 및 자동화 하는 데 Python을 사용 하기 시작 하는 단계별 가이드입니다.

> [!NOTE]
> 이 문서에서는 Windows 중앙 접근 방식에서 파일 시스템 검색, 인터넷 액세스, 파일 형식 구문 분석 등 여러 플랫폼에서 작업을 자동화할 수 있는 Python의 유용한 라이브러리 중 일부를 사용 하도록 환경을 설정 하는 방법을 설명 합니다. Windows 관련 작업의 경우 [ctypes](https://docs.python.org/3/library/ctypes.html), Python 용 C 호환 외래 함수 라이브러리, [winreg](https://docs.python.org/3/library/winreg.html), Windows 레지스트리 API를 python에 노출 하는 함수 및 [python/WinRT](https://pypi.org/project/winrt/)를 확인 하 고, 다음에서 api Windows 런타임 액세스를 사용 하도록 설정 합니다. Python.

## <a name="set-up-your-development-environment"></a>개발 환경 설정

Python을 사용 하 여 파일 시스템 작업을 수행 하는 스크립트를 작성 하는 경우 [Microsoft Store에서 python을 설치](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)하는 것이 좋습니다. Microsoft Store를 통해 설치 하는 경우 기본 Python3 인터프리터를 사용 하지만, 자동 업데이트를 제공 하는 것 외에도 현재 사용자에 대 한 경로 설정 설정 (관리자 액세스 필요 방지)을 처리 합니다.

Windows에서 **웹 개발** 에 Python을 사용 하는 경우 Linux 용 Windows 하위 시스템을 사용 하는 다른 설정을 사용 하는 것이 좋습니다. 가이드에서 연습을 찾아보세요. [Windows에서 웹 개발용 Python을 사용 하 여 시작](./web-frameworks.md)하세요. Python을 처음 접하는 경우 다음 가이드를 사용해 보세요. [초보자를 위해 Windows에서 Python을 사용 하 여 시작](./beginners.md)하세요. 일부 고급 시나리오의 경우 (예: Python의 설치 된 파일에 액세스/수정, 이진 파일 복사 또는 Python Dll을 직접 사용 해야 하는 경우), [python.org](https://www.python.org/downloads/) 에서 직접 특정 Python 릴리스를 다운로드 하거나 설치를 고려 하는 것이 좋습니다. Anaconda, Jython, PyPy, WinPython, IronPython 등의 [대안](https://www.python.org/download/alternatives)입니다. 대체 구현을 선택 하는 특별 한 이유가 있는 고급 Python 프로그래머 인 경우에만이를 권장 합니다.

## <a name="install-python"></a>Python 설치

Microsoft Store를 사용 하 여 Python을 설치 하려면:

1. **시작** 메뉴 (왼쪽 아래에 있는 Windows 아이콘)로 이동 하 고 "Microsoft Store"을 입력 한 다음 링크를 선택 하 여 스토어를 엽니다.

2. 저장소가 열리면 오른쪽 위에 있는 메뉴에서 **검색** 을 선택 하 고 "Python"을 입력 합니다. 앱 아래의 결과에서 "Python 3.7"을 엽니다. **가져오기**를 선택 합니다.

3. Python에서 다운로드 및 설치 프로세스를 완료 한 후 **시작** 메뉴 (왼쪽 아래에 있는 windows 아이콘)를 사용 하 여 windows PowerShell을 엽니다. PowerShell이 열리면 @no__t를 입력 하 여 컴퓨터에 Python3이 설치 되어 있는지 확인 합니다.

4. Python의 Microsoft Store 설치에는 표준 패키지 관리자 인 **pip**가 포함 되어 있습니다. Pip를 사용 하면 Python 표준 라이브러리에 포함 되지 않은 추가 패키지를 설치 하 고 관리할 수 있습니다. 패키지를 설치 하 고 관리 하는 데 사용할 수 있는 pip가 있는지도 확인 하려면 `pip --version`을 입력 합니다.

## <a name="install-visual-studio-code"></a>Visual Studio Code 설치

텍스트 편집기/IDE (통합 개발 환경)로 VS Code를 사용 하 여 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (코드 완성 지원), [lint](https://code.visualstudio.com/docs/python/linting) (코드에 오류를 방지 하는 데 도움이 됨), [디버그 지원](https://code.visualstudio.com/docs/python/debugging) (에서 오류를 찾을 수 있도록 지원)을 활용할 수 있습니다. 코드를 실행 한 후에 코드 조각), [코드 조각](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (재사용 가능한 작은 코드 블록에 대 한 템플릿) 및 [단위 테스트](https://code.visualstudio.com/docs/python/unit-testing) (다른 형식의 입력으로 코드 인터페이스 테스트)가 있습니다.

Windows 용 VS Code를 다운로드 하 고 설치 지침을 따릅니다. [https://code.visualstudio.com](https://code.visualstudio.com).

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 확장 설치

VS Code 지원 기능을 활용 하려면 Microsoft Python 확장을 설치 해야 합니다. [자세히 알아보기](https://code.visualstudio.com/docs/languages/python).

1. **Ctrl + Shift + X** **를 눌러**VS Code 확장 창을 열고 (또는 메뉴를 사용 하 여 탐색 @no__t**확장**).

2. **Marketplace의 위쪽 검색 확장** 상자에 다음을 입력 합니다.  **Python**.

3. Microsoft extension **에서 python (python)** 을 찾고 녹색 **설치** 단추를 선택 합니다.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>VS Code에서 통합 PowerShell 터미널을 엽니다.

VS Code에는 PowerShell을 사용 하 여 Python 명령줄을 열고 코드 편집기와 명령줄 간에 원활한 워크플로를 설정 하는 데 사용할 수 있는 [기본 제공 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal) 이 포함 되어 있습니다.

1. VS Code에서 터미널을 열고 **보기** > **터미널**을 선택 하거나, 바로 가기 **키** 를 사용 하 여 (억음 문자 사용)

    > [!NOTE]
    > 기본 터미널은 PowerShell 이어야 하지만, 변경 해야 하는 경우에는 **Ctrl + Shift + P** 를 사용 하 여 팔레트 명령을 입력 합니다. @No__t-0Terminal를 입력 합니다. 기본 Shell @ no__t-0을 선택 하면 PowerShell, 명령 프롬프트, WSL 등을 포함 하는 터미널 옵션 목록이 표시 됩니다. 사용할 항목을 선택 하 고 **Ctrl + Shift + '** (backtick 사용)를 입력 하 여 새 터미널을 만듭니다.

2. VS Code 터미널 내에서: `python`을 입력 하 여 Python을 엽니다.

3. @No__t-0을 입력 하 여 Python 인터프리터를 실행 합니다. Python은 "Hello World" 문을 반환 합니다.

    ![VS Code의 Python 명령줄](../images/python-in-vscode.png)

4. Python을 종료 하려면 `exit()` `quit()`을 입력 하거나 Ctrl + Z를 선택할 수 있습니다.

## <a name="install-git-optional"></a>Git 설치 (선택 사항)

Python 코드에서 다른 사용자와 공동으로 작업 하거나, GitHub와 같은 오픈 소스 사이트에서 프로젝트를 호스트 하는 경우 VS Code [Git에서 버전 제어](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)를 지원 합니다. VS Code의 소스 제어 탭은 모든 변경 내용을 추적 하 고 UI에 바로 적용 되는 일반적인 Git 명령 (추가, 커밋, 푸시, 끌어오기)을 포함 합니다. 먼저 Git를 설치 하 여 원본 제어판을 켭니다.

1. Git [scm 웹 사이트](https://git-scm.com/download/win)에서 Windows 용 git를 다운로드 하 여 설치 합니다.

2. Git 설치의 설정에 대 한 일련의 질문을 하는 설치 마법사가 포함 되어 있습니다. 항목을 변경 해야 하는 특별 한 이유가 없다면 모든 기본 설정을 사용 하는 것이 좋습니다.

3. 이전에 Git로 작업 한 적이 없는 경우 [GitHub 가이드](https://guides.github.com/) 를 사용 하 여 시작 하는 데 도움을 받을 수 있습니다.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>파일 시스템 디렉터리의 구조를 표시 하는 예제 스크립트

일반적인 시스템 관리 작업에는 상당한 시간이 걸릴 수 있지만 Python 스크립트를 사용 하면 이러한 작업을 자동화 하 여 시간이 전혀 걸리지 않습니다. 예를 들어 Python은 컴퓨터의 파일 시스템의 내용을 읽고 파일 및 디렉터리에 대 한 개요를 인쇄 하거나 한 디렉터리에서 다른 디렉터리로 폴더를 이동 하거나 수백 개의 파일 이름을 바꾸는 등의 작업을 수행할 수 있습니다. 일반적으로 이러한 작업은 수동으로 수행 하는 경우에는 몇 시간이 걸릴 수 있습니다. 대신 Python 스크립트를 사용 하세요.

디렉터리 트리를 안내 하 고 디렉터리 구조를 표시 하는 간단한 스크립트부터 살펴보겠습니다.

1. **시작** 메뉴 (왼쪽 아래 창 아이콘)를 사용 하 여 PowerShell을 엽니다.

2. 프로젝트에 대 한 디렉터리를 만듭니다. `mkdir python-scripts`을 클릭 한 다음 해당 디렉터리를 엽니다. `cd python-scripts`.

3. 예제 스크립트에 사용할 몇 가지 디렉터리를 만듭니다.

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. 스크립트에 사용할 디렉터리 내에 몇 개의 파일을 만듭니다.

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. Python 스크립트 디렉터리에 새 python 파일을 만듭니다.

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. 다음을 입력 하 여 VS Code에서 프로젝트를 엽니다. `code .`

7. **Ctrl + Shift + E** 를 눌러 VS Code 파일 탐색기 창을 열고 (또는 메뉴를 사용 하 여 **보기** > **탐색기**로 이동) 방금 만든 list-directory-contents.py 파일을 선택 합니다. Microsoft Python 확장은 Python 인터프리터를 자동으로 로드 합니다. VS Code 창의 아래쪽에 로드 된 인터프리터를 확인할 수 있습니다.

    > [!NOTE]
    > Python은 해석 된 언어입니다. 즉, 물리적 컴퓨터를 에뮬레이션 하는 가상 컴퓨터 역할을 합니다. 다음과 같은 다양 한 유형의 Python 인터프리터를 사용할 수 있습니다. Python 2, Python 3, Anaconda, PyPy 등 Python 코드를 실행 하 고 Python IntelliSense를 가져오기 위해 사용할 인터프리터 VS Code를 알려 주어 야 합니다. 다른 항목을 선택 해야 하는 특별 한 이유가 없는 경우 기본적으로 (이 경우 Python 3) VS Code 선택 하는 인터프리터를 사용 하는 것이 좋습니다. Python 인터프리터를 변경 하려면 VS Code 창 아래쪽의 파란색 표시줄에 현재 표시 된 인터프리터를 선택 하거나 **명령 팔레트** (Ctrl + Shift + P)를 열고 명령을 입력 **python: 인터프리터 @ no__t-0을 선택 합니다. 현재 설치 된 Python 인터프리터의 목록이 표시 됩니다. [Python 환경 구성에 대해 자세히 알아보세요](https://code.visualstudio.com/docs/python/environments).

    ![VS Code에서 Python 인터프리터를 선택 합니다.](../images/interpreterselection.gif)

8. List-directory-contents.py 파일에 다음 코드를 붙여넣고 **저장**을 선택 합니다.

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

9. VS Code 통합 터미널 (억음 문자 사용 **)을 열고**Python 스크립트를 방금 저장 한 src 디렉터리를 입력 합니다.

    ```powershell
    cd src
    ```

10. 다음을 사용 하 여 PowerShell에서 스크립트를 실행 합니다.

    ```powershell
    python3 .\list-directory-contents.py
    ```

    다음과 같은 출력이 표시 됩니다.

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

11. PowerShell 터미널에서 직접이 명령을 입력 하 여 파일 시스템 디렉터리 출력을 자신의 텍스트 파일로 인쇄 하려면 Python을 사용 합니다. `python3 list-directory-contents.py > food-directory.txt`

지금까지 방금 만든 디렉터리와 파일을 읽고 Python을 사용 하 여 자체 텍스트 파일에 디렉터리 구조를 표시 한 다음 인쇄 하는 자동화 된 시스템 관리 스크립트를 작성 했습니다.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>디렉터리의 모든 파일을 수정 하는 예제 스크립트

이 예에서는 파일의 마지막 수정 날짜를 파일 이름 앞에 추가 하 여 방금 만든 파일과 디렉터리를 사용 하 여 각 파일의 이름을 바꿉니다.

1. **Python 스크립트** 디렉터리의 **src** 폴더 내에서 스크립트에 대 한 새 python 파일을 만듭니다.

    ```powershell
    new-item update-filenames.py
    ```

2. Update-filenames.py 파일을 열고 다음 코드를 파일에 붙여 넣고 저장 합니다.

    > [!NOTE]
    > os. getmtime은 쉽게 읽을 수 없는 틱 타임 스탬프를 반환 합니다. 표준 datetime 문자열로 먼저 변환 해야 합니다.

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

3. Update-filenames.py 스크립트를 실행 하 여 테스트 합니다. @no__t 하 고 list-directory-contents.py 스크립트를 다시 실행 합니다. `python3 list-directory-contents.py`

4. 다음과 같은 출력이 표시 됩니다.

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

5. Python을 사용 하 여 PowerShell 터미널에서 직접이 명령을 입력 하 여 해당 텍스트 파일에 마지막으로 수정 된 타임 스탬프를 사용 하 여 새 파일 시스템 디렉터리 이름을 인쇄 합니다. `python3 list-directory-contents.py > food-directory-last-modified.txt`

기본 시스템 관리 작업을 자동화 하는 데 Python 스크립트를 사용 하는 데 대 한 흥미로운 작업을 배웠습니다. 물론, 더 많은 정보를 확인할 수 있지만,이를 위해 오른쪽 피트에서 시작 하는 것이 좋습니다. 아래 학습을 계속 하기 위해 몇 가지 추가 리소스를 공유 했습니다.

## <a name="additional-resources"></a>추가 자료

- [Python Docs: 파일 및 디렉터리 액세스 @ no__t-0: 파일 시스템 작업 및 모듈을 사용 하 여 파일 속성을 읽고, 이식 가능한 방식으로 경로를 조작 하 고, 임시 파일을 만드는 방법에 대 한 Python 설명서입니다.
- [Learn 학습: String_Formatting 자습서 @ no__t-0: 문자열 서식 지정에 "%" 연산자를 사용 하는 방법에 대해 자세히 알아봅니다.
- 다음을 [알아야 할 Python 파일 시스템 메서드 10 개](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2): @No__t-0 및 `shutil`을 사용 하 여 파일 및 폴더 조작을 위한 중간 문서입니다.
- Python에 대 한 Hitchhikers 가이드를 @no__t 합니다. Systems 관리 @ no__t-0: Python과 관련 된 항목에 대 한 개요 및 모범 사례를 제공 하는 "독단적인 guide"입니다. 이 섹션에서는 시스템 관리 도구 및 프레임 워크에 대해 설명 합니다. 이 가이드는 GitHub에서 호스트 되므로 문제를 파일 하 고 기여를 내릴 수 있습니다.
