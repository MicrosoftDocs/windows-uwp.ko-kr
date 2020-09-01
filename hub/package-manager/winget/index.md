---
title: winget 도구를 사용하여 애플리케이션 설치 및 관리
description: winget 명령줄 도구를 사용하면 개발자가 Windows 10 컴퓨터에서 애플리케이션을 검색, 설치, 업그레이드, 제거 및 구성할 수 있습니다.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 4c918dccb2873f47a16669c195c47180e2129476
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168747"
---
# <a name="use-the-winget-tool-to-install-and-manage-applications"></a>winget 도구를 사용하여 애플리케이션 설치 및 관리

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

**winget** 명령줄 도구를 사용하면 개발자가 Windows 10 컴퓨터에서 애플리케이션을 검색, 설치, 업그레이드, 제거 및 구성할 수 있습니다. 이 도구는 Windows 패키지 관리자 서비스에 대한 클라이언트 인터페이스입니다.

**winget** 도구는 현재 미리 보기이므로 계획된 모든 기능 중 일부만 이 시점에서 사용할 수 있습니다.

## <a name="install-winget"></a>winget 설치

**winget** 도구를 설치하는 방법에는 여러 가지가 있습니다.

* **winget** 도구는 [Microsoft 앱 설치 관리자](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab)의 플라이트 또는 미리 보기 버전에 포함되어 있습니다. **winget**을 사용하려면 **앱 설치 관리자**의 미리 보기 버전을 설치해야 합니다. 초기 액세스 권한을 얻으려면 [Windows 패키지 관리자 참가자 프로그램](https://aka.ms/AppInstaller_InsiderProgram)에 요청을 제출합니다. 플라이트 링에 참여하면 최신 미리 보기 업데이트가 표시됩니다.

* [Windows 참가자 플라이트 링](https://insider.windows.com)에 참여합니다.

* [winget 리포지토리](https://github.com/microsoft/winget-cli)의 릴리스 폴더에 있는 Windows 데스크톱 앱 설치 관리자 패키지를 설치합니다.

> [!NOTE]
> **winget** 도구를 사용하려면 Windows 10 버전 1709(10.0.16299) 또는 이후 버전의 Windows 10이 필요합니다.

## <a name="administrator-considerations"></a>관리자 고려 사항

관리자 권한으로 **winget**을 실행하는지 여부에 따라 설치 관리자의 동작이 달라질 수 있습니다.

* 관리자 권한 없이 **winget**을 실행하는 경우 일부 애플리케이션을 설치하려면 [권한 상승](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/)이 필요할 수 있습니다. 설치 관리자가 실행되면 Windows에서 [권한 상승](https://docs.microsoft.com/windows/security/identity-protection/user-account-control)을 요구하는 메시지가 표시됩니다. 권한 상승을 선택하지 않으면 애플리케이션이 설치되지 않습니다.  

* 관리자 명령 프롬프트에서 **winget**을 실행할 때 애플리케이션에서 요구하는 경우 [권한 상승 메시지](/windows/security/identity-protection/user-account-control/how-user-account-control-works)가 표시되지 않습니다. 관리자 권한으로 명령 프롬프트를 실행할 때는 항상 주의해야 하며 신뢰할 수 있는 애플리케이션만 설치합니다.

## <a name="use-winget"></a>winget 사용

**앱 설치 관리자**가 설치되면 명령 프롬프트에서 'winget'을 입력하여 **winget**을 실행할 수 있습니다.

가장 일반적인 사용 시나리오 중 하나는 즐겨찾는 도구를 검색하여 설치하는 것입니다.

1. 도구를 [검색](search.md)하려면 `winget search \<appname>`을 입력합니다.
2. 원하는 도구를 사용할 수 있다고 확인되었으면 `winget install \<appname>`을 입력하여 도구를 [설치](install.md)할 수 있습니다. **winget** 도구에서 설치 관리자를 시작하여 애플리케이션을 PC에 설치합니다.
    ![winget 명령줄](images\install.png)

3. **winget**은 설치 및 검색 외에도 애플리케이션에 대한 [세부 정보 표시](show.md), [원본 변경](source.md) 및 [패키지 유효성 검사](validate.md)를 수행할 수 있는 여러 가지 다른 명령을 제공합니다. 전체 명령 목록을 가져오려면 `winget --help`를 입력합니다.
    ![winget help](images\help.png)

### <a name="commands"></a>명령

**winget** 도구의 현재 미리 보기에서 지원하는 명령은 다음과 같습니다.

| 명령 | 설명 |
|---------|-------------|
| [hash](hash.md) | 설치 관리자에 대한 SHA256 해시를 생성합니다. |
| [help](help.md) | **winget** 도구 명령에 대한 도움말을 표시합니다. |
| [install](install.md) | 지정된 애플리케이션을 설치합니다. |
| [search](search.md) | 애플리케이션을 검색합니다. |
| [show](show.md) | 지정된 애플리케이션에 대한 세부 정보를 표시합니다. |
| [source](source.md) | **winget** 도구에서 액세스하는 Windows 패키지 관리자 리포지토리를 추가, 제거 및 업데이트합니다. |
| [validate](validate.md) | Windows 패키지 관리자 리포지토리에 제출할 매니페스트 파일의 유효성을 검사합니다. |

### <a name="options"></a>옵션

**winget** 도구의 현재 미리 보기에서 지원하는 옵션은 다음과 같습니다.

| 옵션 | 설명 |
|--------------|-------------|
| **-v,--version** | 현재 버전의 winget을 반환합니다. |
| **--info** |  info는 라이선스 및 개인정보처리방침에 대한 링크를 포함하여 winget에 대한 모든 세부 정보를 제공합니다. |
| **-?, --help** |  추가 도움말 winget을 가져옵니다. |

## <a name="supported-installer-formats"></a>지원되는 설치 관리자 형식

**winget** 도구의 현재 미리 보기에서 지원하는 설치 관리자 유형은 다음과 같습니다.

* EXE
* MSIX
* MSI

## <a name="scripting-winget"></a>winget 스크립팅

일괄 처리 스크립트 및 powershell 스크립트를 작성하여 여러 애플리케이션을 설치할 수 있습니다.

``` CMD
@echo off  
Echo Install Powertoys and Terminal  
REM Powertoys  
winget install Microsoft.Powertoys  
if %ERRORLEVEL% EQU 0 Echo Powertoys installed successfully.  
REM Terminal  
winget install Microsoft.WindowsTerminal  
if %ERRORLEVEL% EQU 0 Echo Terminal installed successfully.   %ERRORLEVEL%
```

> [!NOTE]
> 스크립트가 작성되면 **winget**에서 지정된 순서대로 애플리케이션을 시작합니다. 설치 관리자에서 성공 또는 실패를 반환하면 **winget**에서 다음 설치 관리자를 시작합니다. 설치 관리자에서 다른 프로세스를 시작하면 조기에 **winget**으로 돌아갈 수 있습니다. 이 경우 이전 설치 관리자가 완료되기 전에 **winget**에서 다음 설치 관리자를 설치합니다.

## <a name="missing-tools"></a>누락된 도구

도구 또는 애플리케이션이 [커뮤니티 리포지토리](../package/repository.md)에 포함되어 있지 않은 경우입니다. 패키지를 [리포지토리](https://github.com/microsoft/winget-pkgs)에 제출하세요. 즐겨찾는 도구가 추가되면 본인과 다른 모든 사용자가 사용할 수 있습니다.

## <a name="open-source-details"></a>오픈 소스 세부 정보

**winget** 도구는 GitHub의 [https://github.com/microsoft/winget-cli/](https://github.com/microsoft/winget-cli/) 리포지토리에서 사용할 수 있는 오픈 소스 소프트웨어입니다. 클라이언트를 빌드하기 위한 원본은 [src 폴더](https://github.com/microsoft/winget-cli/tree/master/src)에 있습니다.

**winget** 원본은 Visual Studio 2019 C++ 솔루션에 포함되어 있습니다. 솔루션을 제대로 빌드하려면 [C++ 워크로드가 포함된 최신 Visual Studio](https://visualstudio.microsoft.com/downloads/)를 설치합니다.

GitHub의 **winget** 원본에 참여하는 것이 좋습니다. 먼저 Microsoft CLA에 동의하고 서명해야 합니다.