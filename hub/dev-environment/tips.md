---
title: Windows 10에서 개발 워크플로를 개선하기 위한 팁
description: Windows 10에서 개발 워크플로를 개선하기 위한 팁입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, 개발자, 팁, 성능, WSL
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 1135be4797893a74e398e69fcbc1c43d60e9fdb9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172667"
---
# <a name="tips-for-improving-performance-and-development-workflows"></a>성능 및 개발 워크플로를 개선하기 위한 팁

워크플로를 보다 효율적이고 즐겁게 만드는 데 도움이 되는 몇 가지 팁을 수집했습니다. 공유할 추가 팁이 있나요? 위의 "편집" 단추를 사용하여 끌어오기 요청을 작성하거나 아래의 "피드백" 단추를 사용하여 문제를 제출하면 저희가 해당 항목을 목록에 추가할 수 있습니다.

## <a name="use-shortcuts-to-open-a-project-in-vs-code-or-windows-file-explorer"></a>바로 가기를 사용하여 VS Code 또는 Windows 파일 탐색기에서 프로젝트 열기

명령줄에서 `code .` 명령을 사용하여 연 프로젝트로 VS Code를 시작하거나, Windows 또는 WSL 배포판의 Windows 파일 탐색기 명령줄에서 `explorer.exe .` 명령을 사용하여 프로젝트 디렉터리를 열 수 있습니다. 이렇게 해도 기본적으로 작동하지 않는 경우 PATH 환경 변수에 VS Code 실행 파일을 추가해야 할 수도 있습니다. [명령줄에서 시작](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)에 대해 자세히 알아보세요.

![Windows 파일 탐색기 스크린샷](../images/wsl-file-explorer.png)

## <a name="use-the-credential-manager-to-your-streamline-authentication-process"></a>자격 증명 관리자를 사용하여 인증 프로세스 간소화

버전 제어 및 협업에 Git을 사용하는 경우 Windows 자격 증명 관리자에 토큰을 저장하도록 [Git 자격 증명 관리자를 설정](/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)하여 인증 프로세스를 간소화할 수 있습니다. 또한 프로젝트에 [.gitignore 파일을 추가](/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)하는 것이 좋습니다.

## <a name="use-wsl-for-testing-your-production-pipeline-before-deploying-to-the-cloud"></a>클라우드에 배포하기 전에 WSL을 사용하여 프로덕션 파이프라인 테스트

Linux용 Windows 하위 시스템을 사용하면 개발자가 기존 가상 머신의 오버헤드 또는 듀얼 부팅 설정 없이 대부분의 명령줄 도구, 유틸리티 및 애플리케이션을 비롯한 GNU/Linux 환경을 수정하지 않고 Windows에서 직접 실행할 수 있습니다.

WSL은 내부 개발 루프의 일부로 사용하려는 개발자를 대상으로 합니다. Sam이 CI/CD 파이프라인(Continuous Integration & Continuous Delivery)을 만들고 있으며 클라우드에 배포하기 전에 로컬 머신(랩톱)에서 먼저 테스트하려 한다고 가정해 보겠습니다. Sam은 WSL(및 WSL 2)을 사용하여 속도와 성능을 향상시킨 다음, 원하는 Bash 명령어와 도구를 사용하여 로컬(랩톱 상의)에서 정품 Linux Ubuntu 인스턴스를 사용할 수 있습니다. 개발 파이프라인이 로컬에서 확인되면 Sam은 해당 CI/CD 파이프라인을 Docker 컨테이너로 만들고 프로덕션 준비가 된 Ubuntu VM에서 실행되는 클라우드 인스턴스로 푸시하여 이 컨테이너를 클라우드(예: Azure)까지 푸시할 수 있습니다.

WSL을 사용하는 더 많은 방법은 이 [WSL 2의 탭 vs 공간 에피소드](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)를 참조하세요.

## <a name="improve-performance-speed-for-wsl-by-not-crossing-over-file-systems"></a>파일 시스템에서 교차하지 않게 하여 WSL의 성능 속도 향상

Windows 및 Linux용 windows 하위 시스템을 모두 사용하는 경우 두 개의 파일 시스템 NTSF(Windows) 및 WSL(Linux 배포판)이 설치되어 있습니다. 빠른 성능을 얻을 수 있도록 프로젝트 파일을 사용 중인 도구와 동일한 시스템에 저장해야 합니다. [빠른 성능을 얻을 수 있도록 올바른 파일 시스템을 선택하는 방법](/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)에 대해 알아보세요.

## <a name="improve-build-speeds-by-adding-windows-defender-exclusions"></a>Windows Defender 제외를 추가하여 빌드 속도 향상

보안 위협 검사를 생략해도 될 만큼 충분히 신뢰하는 프로젝트 폴더 또는 파일 형식에 대한 제외를 추가하도록 Windows Defender 설정을 업데이트하여 빌드 속도를 향상할 수 있습니다. [성능 향상을 위한 Windows Defender 설정 업데이트](../android/defender-settings.md) 방법에 대해 자세히 알아보세요.

![Windows Defender 스크린샷](../images/windows-defender-exclusions.png)

## <a name="launch-all-your-command-lines-in-windows-terminal-at-once"></a>Windows 터미널에서 한 번에 모든 명령줄 시작

* [Windows 터미널 명령줄 인수](/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)를 사용하여 PowerShell, Ubuntu, Azure CLI 등의 여러 명령줄을 여러 블레이드가 있는 단일 창으로 시작할 수 있습니다. [Windows 터미널](/windows/terminal/get-started), [WSL/Ubuntu](/windows/wsl/install-win10) 및 [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)를 설치한 후에는 PowerShell에 다음 명령을 입력하여 세 명령줄이 모두 있는 새로운 다중 블레이드 창을 엽니다.

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

## <a name="share-your-tips"></a>팁 공유

Windows를 사용하는 다른 개발자가 워크플로를 개선하는 데 도움이 되는 팁이 있나요? 특정 항목에 대한 팁을 추가하려면 [끌어오기 요청을 제출](https://github.com/MicrosoftDocs/windows-uwp/edit/docs/hub/dev-environment/overview.md)하여 페이지에 팁을 추가하거나 [문제를 제출](https://github.com/MicrosoftDocs/windows-uwp/issues/new?title=&body=%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20Details%0A%0A%E2%9A%A0%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20docs.microsoft.com%20%E2%9E%9F%20GitHub%20issue%20linking.*%0A%0A*%20ID%3A%207779352b-7b4e-dad8-7c1b-b9aba2c5e561%0A*%20Version%20Independent%20ID%3A%20a5b81b80-87a1-b6e2-8936-baf6c1a0b9c5%0A*%20Content%3A%20%5BSet%20up%20your%20Windows%2010%20development%20environment%5D(https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fdev-environment%2Foverview)%0A*%20Content%20Source%3A%20%5Bhub%2Fdev-environment%2Foverview.md%5D(https%3A%2F%2Fgithub.com%2FMicrosoftDocs%2Fwindows-uwp%2Fblob%2Fdocs%2Fhub%2Fdev-environment%2Foverview.md)%0A*%20Product%3A%20**dev-environment**%0A*%20Technology%3A%20**windows-nodejs**)하세요.

해결되기를 바라는 성능 관련 문제가 있나요? 새 [WinDev 문제 리포지토리](https://github.com/microsoft/windev)에 문제를 제출해 주세요.

모든 개발자들에게 감사드립니다. 저희는 개발자의 의견에 귀기울이고 있으며 개발자 환경을 개선하기 위해 노력하고 있습니다.