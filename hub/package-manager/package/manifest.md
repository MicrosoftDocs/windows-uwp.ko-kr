---
title: 패키지 매니페스트 만들기
description: 소프트웨어 패키지를 Windows 패키지 관리자 리포지토리에 제출하려면 먼저 패키지 매니페스트를 만듭니다.
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 960caede306228c492bc88c81ac3c1c841b6c18c
ms.sourcegitcommit: 9c2b21081158e712a856158d25dce76b3e213a9c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88129750"
---
# <a name="create-your-package-manifest"></a>패키지 매니페스트 만들기

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

소프트웨어 패키지를 [Windows 패키지 관리자 리포지토리](repository.md)에 제출하려면 먼저 패키지 매니페스트를 만듭니다. 매니페스트는 설치할 애플리케이션을 설명하는 YAML 파일입니다.

이 문서에서는 Windows 패키지 관리자에 대한 패키지 매니페스트의 내용에 대해 설명합니다.

## <a name="yaml-basics"></a>YAML 기본 사항

다른 Microsoft 개발 도구와의 사용자 가독성 및 일관성에 대한 상대적 편의성으로 인해 YAML 형식이 패키지 매니페스트에 대해 선택되었습니다. YAML 구문에 익숙하지 않은 경우 [Y분 내에 YAML 알아보기](https://learnxinyminutes.com/docs/yaml/)에서 기본 사항을 습득할 수 있습니다.

> [!NOTE]
> Windows 패키지 관리자에 대한 매니페스트는 현재 일부 YAML 기능을 지원하지 않습니다. 지원되지 않는 YAML 기능에는 앵커, 복합 키 및 세트가 포함됩니다.

## <a name="conventions"></a>규칙

이 문서에서 사용되는 규칙은 다음과 같습니다.

* `:`의 왼쪽은 매니페스트 정의에 사용되는 리터럴 키워드입니다.
* `:`의 오른쪽은 데이터 형식입니다. 데이터 형식은 **string**과 같은 기본 형식이거나 이 문서의 다른 곳에 정의된 다양한 구조체에 대한 참조일 수 있습니다.
* `[` *datatype* `]` 표기법은 언급된 데이터 형식의 배열을 나타냅니다. 예를 들어 `[ string ]`은 문자열 배열입니다.
* `{` *datatype* `:` *datatype* `}` 표기법은 한 데이터 형식과 다른 데이터 형식의 매핑을 나타냅니다. 예를 들어 `{ string: string }`은 문자열에 대한 문자열의 매핑입니다.

## <a name="manifest-contents"></a>매니페스트 내용

패키지 매니페스트에는 필수 항목 세트가 포함되어야 하며, 소프트웨어를 설치하는 고객 환경을 향상시키는 데 도움이 되는 추가 선택 항목도 포함될 수 있습니다. 이 섹션에서는 필수 매니페스트 스키마 및 전체 매니페스트 스키마에 대해 간략히 설명하고 각 스키마에 대한 예제를 제공합니다.

매니페스트 파일의 각 필드는 파스칼식 대/소문자 구분이어야 하며 중복될 수 없습니다.

매니페스트의 항목에 대한 전체 목록과 설명은 [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli) 리포지토리의 [매니페스트 사양](https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md)을 참조하세요.

### <a name="minimal-required-schema"></a>최소 필수 스키마

#### <a name="minimal-required-schema"></a>[최소 필수 스키마](#tab/minschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
Version: string # Version numbering format.
License: string # The open source license or copyright.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Installers:
  - Arch: string # Enumeration of supported architectures.
  - Url: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[예제](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>전체 스키마

#### <a name="complete-schema"></a>[전체 스키마](#tab/compschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
AppMoniker: string # The common name someone may use to search for the package.
Version: string # Version numbering format for package version.
Channel: string # A string representing the flight ring.
License: string # The open source license or copyright.
LicenseUrl: string # Valid secure URL to license.
MinOSVersion: string # Version numbering format for minimum version of Windows supported.
Description: string # Description of the package.
Homepage: string # Valid secure URL for the package.
Tags: list # Additional strings a user would use to search for the package.
FileExtensions: list # List of file extensions the package could support.
Protocols: list # List of protocols the package provides a handler for.
Commands: list # List of commands or aliases the user would use to run the package.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Custom: string # Custom switches passed to the installer.
Silent: string # Switches passed to the installer for silent installation.
SilentWithProgress: string # Switches passed to the installer for non-interactive install.
Interactive: string # Experimental.
Language: string # Experimental.
Log: string # Specifies log redirection switches and path.
InstallLocation: string # Specifies alternate location to install package.
Installers: # Nested map of keys for specific installer.
  - Arch: string # Enumeration of supported architectures.
  - Url: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file.
  - Switches: # Collection of entries to override root keys. The primary supported values are: Custom, Silent, SilentWithProgress, Interactive. For a complete list see the specification at https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md.
  - Scope: string # Experimental.
  - SystemAppId: string # Experimental.
Localization: # Nested map of keys for localization.
  - Language: string # Locale for display fields and localized URLs.
ManifestVersion: string # Version number format for manifest version.
```

#### <a name="good-example"></a>[좋은 예제](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[더 나은 예제](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

> [!NOTE]
> 설치 관리자가 .exe이고 Nullsoft 또는 Inno를 사용하여 빌드된 경우 해당 값을 대신 지정할 수 있습니다. Nullsoft 또는 Inno를 지정하면 클라이언트는 설치 프로그램의 진행률 설치 동작을 통해 silent and silent를 자동으로 설정합니다.

## <a name="installer-switches"></a>설치 관리자 스위치

종종 명령줄에서 `-?`를 설치 관리자에 전달하여 설치 관리자에 사용할 수 있는 자동 `Switches`를 확인할 수 있습니다. 다음은 다양한 설치 관리자 유형에 사용할 수 있는 일반적인 자동 `Swtiches`입니다.

| 설치 관리자 | 명령  | 문서 |  
| :--- | :-- | :--- |  
| MSI | `/q` | [MSI 명령줄 옵션](https://docs.microsoft.com/windows/win32/msi/command-line-options) |
| InstallShield | `/s`  | [InstallShield 명령줄 매개 변수](https://docs.flexera.com/installshield19helplib/helplibrary/IHelpSetup_EXECmdLine.htm) |
| Inno 설정 | `/SILENT or /VERYSILENT` | [Inno 설정 설명서](https://jrsoftware.org/ishelp/) |
| Nullsoft | `/S` | [Nullsoft 자동 설치 관리자/제거 프로그램](https://nsis.sourceforge.io/Docs/Chapter4.html#silent) |

## <a name="tips-and-best-practices"></a>팁 및 모범 사례

* 소프트웨어를 찾아서 설치할 때 최상의 고객 환경을 위해 필수 스키마 이외의 선택 항목을 최대한 많이 포함하는 것이 좋습니다. 예를 들어 `AppMoniker` 필드는 선택 사항입니다. 그러나 이 필드를 포함하는 경우 [search](../winget/search.md) 명령을 수행하면 `AppMoniker` 값과 관련된 결과가 표시됩니다(예:  **Visual Studio Code**의 경우 **vscode**). 지정된 `AppMoniker` 값의 앱이 하나만 있으면 고객은 정규화된 ID가 아니라 모니커를 지정하여 애플리케이션을 설치할 수 있습니다.
* `Id`는 고유해야 합니다. 동일한 패키지 식별자를 사용하는 제출이 여러 개 있을 수 없습니다. 공백을 사용하지 않습니다. 이렇게 하면 [winget](../index.md) 클라이언트를 사용하는 경우 사용자가 `Id` 주위에 따옴표를 배치해야 합니다.
* 여러 게시자 폴더를 만들지 않습니다. 예를 들어 "Contoso" 폴더가 이미 있으면 "Contoso Ltd"를 만들지 않습니다. 폴더를 만드는 경우에도 공백을 사용하지 않습니다.
* 가능한 경우 자동 설치를 사용하여 모든 패키지를 제출해야 합니다. 자동 설치를 지원하지 않는 실행 파일이 있으면 사용자 환경이 줄어듭니다.
* 매니페스트에서 줄 바꿈 이전의 문자열 길이를 100자로 제한합니다.
* 지정한 버전의 패키지에 대해 설치 관리자 유형이 둘 이상 있는 경우 `InstallerType`의 인스턴스를 각 `Installers` 아래에 배치할 수 있습니다.
