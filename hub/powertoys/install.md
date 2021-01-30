---
title: PowerToys 설치
description: 실행 파일 또는 패키지 관리자 (WinGet, Chocolatey, 혜택)를 사용 하 여 Windows 10을 사용자 지정 하는 일련의 유틸리티 인 Powertoy을 설치 합니다.
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: d0018e69e5a107baa595e4d3dd05a924257551a8
ms.sourcegitcommit: d0eef123b167dc63f482a9f4432a237c1c6212db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077221"
---
# <a name="install-powertoys"></a>PowerToys 설치

아래 링크 된 Windows 실행 파일 단추를 사용 하 여 Powertoy을 설치 하는 것이 좋지만, 패키지 관리자를 사용 하려는 경우에도 대체 설치 방법이 나열 됩니다.

## <a name="install-with-windows-executable-file"></a>Windows 실행 파일을 사용 하 여 설치

> [!div class="nextstepaction"]
> [PowerToys 설치](https://aka.ms/installpowertoys)

Windows 실행 파일을 사용 하 여 Powertoy를 설치 하려면:

1. [Microsoft Powertoy GitHub 릴리스 페이지](https://github.com/microsoft/PowerToys/releases/)를 방문 하세요.
2. 사용할 수 있는 Powertoy의 안정적인 버전과 실험적 버전 목록을 찾아봅니다.
3. **자산** 드롭다운 메뉴를 선택 하 여 릴리스에 대 한 파일을 표시 합니다.
4. `PowerToysSetup-0.##.#-x64.exe`Powertoy 실행 파일 설치 관리자를 다운로드할 파일을 선택 합니다.
5. 다운로드 한 후 실행 파일을 열고 설치 프롬프트를 따릅니다.

## <a name="requirements"></a>요구 사항

- Windows 10 1803 (빌드 17134) 이상.
- [.Net Core 3.1 데스크톱 런타임](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer). Powertoy 설치 관리자가이 요구 사항을 처리 합니다.
- x64 아키텍처는 현재 지원 됩니다. ARM 및 x86 지원은 나중에 사용할 수 있게 됩니다.

컴퓨터가 이러한 요구 사항을 충족 하는지 확인 하려면 **⊞ Win** *(windows 키)* D를 선택 하 여 windows 10 버전 및 빌드 번호를 확인  +  한 다음 **winver** 를 입력 하 고 **확인** 을 선택 합니다. (또는 Windows 명령 프롬프트에서 `ver` 명령을 입력합니다.) **설정** 메뉴에서 [최신 Windows 버전으로 업데이트할](ms-settings:windowsupdate) 수 있습니다.

## <a name="alternative-install-methods"></a>대체 설치 방법

<!--  - **[Windows executable .exe file](#install-with-windows-executable-file)** *(Recommended)* -->
- [Windows 패키지 관리자](#install-with-windows-package-manager-preview) *(미리 보기)*
- [커뮤니티 중심 설치 도구](#community-driven-install-tools) *(공식적으로 지원 되지 않음)*

## <a name="install-with-windows-package-manager-preview"></a>Windows 패키지 관리자 (미리 보기)를 사용 하 여 설치

WinGet (Windows 패키지 관리자) 미리 보기를 사용 하 여 Powertoy를 설치 하려면:

1. [Windows 패키지 관리자](https://github.com/microsoft/winget-cli/releases)에서 powertoy를 다운로드 합니다.
2. 명령줄/PowerShell에서 다음 명령을 실행 합니다.

```powershell
WinGet install powertoys
```

## <a name="community-driven-install-tools"></a>커뮤니티 중심 설치 도구

이러한 커뮤니티 중심 대체 설치 방법은 공식적으로 지원 되지 않으며 Powertoy 팀은 이러한 패키지를 업데이트 하거나 관리 하지 않습니다.

### <a name="install-with-chocolatey"></a>Chocolatey를 사용 하 여 설치

[Chocolatey](https://chocolatey.org/)를 사용 하 여 powertoy를 설치 하려면 명령줄/PowerShell에서 다음 명령을 실행 합니다.

```powershell
choco install powertoys
```

Powertoy를 업그레이드 하려면 다음을 실행 합니다.

```powershell
choco upgrade powertoys
```

설치/업그레이드 하는 동안 문제가 발생 하는 경우 [Chocolatey.org에서 powertoy 패키지](https://chocolatey.org/packages/powertoys) 를 방문 하 여 [Chocolatey 심사 프로세스](https://chocolatey.org/docs/package-triage-process)를 따르세요.

### <a name="install-with-scoop"></a>혜택를 사용 하 여 설치

[혜택](https://scoop.sh/)를 사용 하 여 powertoy를 설치 하려면 명령줄/PowerShell에서 다음 명령을 실행 합니다.

```powershell
scoop install powertoys
```

Powertoy를 업데이트 하려면 명령줄/PowerShell에서 다음 명령을 실행 합니다.

```powershell
scoop update powertoys
```

설치/업데이트 하는 동안 문제가 발생 하는 경우 [GitHub의 혜택](https://github.com/lukesampson/scoop/issues)리포지토리에서 문제를 해결 하세요.
