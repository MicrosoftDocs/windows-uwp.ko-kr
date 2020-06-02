---
title: 리포지토리에 매니페스트 제출
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c5ebcc564b4db16c1d16385cbeaf7fd6d82c8f18
ms.sourcegitcommit: 8193aef04deb3514eb2d34bfe5cb9424ba12cd76
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83865030"
---
# <a name="submit-your-manifest-to-the-repository"></a>리포지토리에 매니페스트 제출

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

애플리케이션을 설명하는 [패키지 매니페스트](manifest.md)가 만들어지면 이 매니페스트를 Windows 패키지 관리자 리포지토리에 제출할 수 있습니다. 이는 **winget** 도구에서 액세스할 수 있는 매니페스트 컬렉션이 포함된 공용 대상 리포지토리입니다. 매니페스트를 제출하려면 GitHub의 오픈 소스 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 리포지토리에 업로드합니다.

새 매니페스트를 GitHub 리포지토리에 추가하기 위한 끌어오기 요청을 제출하면 자동화된 프로세스에서 매니페스트 파일의 유효성을 검사하고 악성으로 알려진 패키지가 아닌지 확인합니다. 이 유효성 검사에 성공하면 패키지가 공용 Windows 패키지 관리자 리포지토리에 추가되어 **winget** 클라이언트 도구에서 검색할 수 있습니다. 오픈 소스 GitHub 리포지토리와 공용 Windows 패키지 관리자 리포지토리의 매니페스트에 대한 차이점을 확인합니다.

> [!IMPORTANT]
> Microsoft에는 어떤 이유로든 제출을 거부할 권리가 있습니다.

## <a name="third-party-repositories"></a>타사 리포지토리

현재 알려진 타사 리포지토리가 없습니다. Microsoft는 여러 파트너와 협력하여 타사 리포지토리를 사용하도록 설정하는 프로토콜 또는 API를 개발하고 있습니다.

## <a name="manifest-validation"></a>매니페스트 유효성 검사

매니페스트를 GitHub의 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) 리포지토리에 제출하면 매니페스트의 유효성이 자동으로 검사되고 Windows 에코시스템의 안전성이 평가됩니다. 매니페스트는 수동으로 검토할 수도 있습니다.

## <a name="how-to-submit-your-manifest"></a>매니페스트를 제출하는 방법

매니페스트를 리포지토리에 제출하려면 다음 단계를 수행합니다.

### <a name="step-1-validate-your-manifest"></a>1단계: 매니페스트 유효성 검사

**winget** 도구는 [validate](..\winget\validate.md) 명령을 제공하여 매니페스트를 올바르게 만들었는지 확인합니다. 매니페스트의 유효성을 검사하려면 이 명령을 사용합니다.

```CMD
winget validate \<manifest-file>
```

유효성 검사에 실패하면 오류를 사용하여 줄 번호를 찾아서 수정합니다. 매니페스트의 유효성이 검사되면 리포지토리에 제출할 수 있습니다.

### <a name="step-2-clone-the-repository"></a>2단계: 리포지토리 복제

다음으로, 리포지토리의 포크를 만들고 복제합니다.

1. 브라우저에서 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs)로 이동하고 **포크**를 클릭합니다.
    ![포크 그림](images\fork.png)

2. Windows 명령 프롬프트 또는 PowerShell과 같은 명령줄 환경에서 다음 명령을 사용하여 포크를 복제합니다.
    ```CMD
    git clone \<your-fork-name>
    ```

 3. 여러 제출을 만드는 경우 포크 대신 분기를 만듭니다. 현재 제출당 하나의 매니페스트 파일만 허용합니다.
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>3단계: 로컬 리포지토리에 매니페스트 추가

매니페스트 파일을

**manifests** / **publisher** / **application** / **version.yaml** 폴더 구조의 리포지토리에 추가해야 합니다.

* **manifests** 폴더는 리포지토리의 모든 매니페스트에 대한 루트 폴더입니다.
* **publisher** 폴더는 소프트웨어를 게시하는 회사의 이름입니다. 예를 들어 **Microsoft**입니다.
* **application** 폴더는 애플리케이션 또는 도구의 이름입니다. 예를 들어 **VSCode**입니다.
* **version.yaml**은 매니페스트의 파일 이름입니다. 파일 이름은 애플리케이션의 현재 버전으로 설정해야 합니다. 예를 들어 **1.0.0.yaml**입니다.

>[!IMPORTANT]
> 매니페스트의 `Id` 값은 매니페스트 폴더 경로의 게시자 및 애플리케이션 이름과 일치해야 하며, 매니페스트의 `version` 값은 파일 이름의 버전과 일치해야 합니다. 자세한 내용은 [패키지 매니페스트 만들기](manifest.md#tips-and-best-practices)를 참조하세요.

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>4단계: 원격 리포지토리에 매니페스트 제출

이제 새 매니페스트를 원격 리포지토리로 푸시할 준비가 되었습니다.

1. `add` 명령을 사용하여 제출을 준비합니다.
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. `commit` 명령을 사용하여 변경 내용을 커밋하고 제출에 대한 정보를 제공합니다.
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. `push` 명령을 사용하여 변경 내용을 원격 리포지토리로 푸시합니다.
    ```CMD
    git push
    ```

### <a name="step-5-create-a-pull-request"></a>5단계: 끌어오기 요청 만들기

변경 내용이 푸시되면 [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs)로 돌아가서 포크 또는 분기를 **마스터** 분기에 병합하는 끌어오기 요청을 만듭니다.

![끌어오기 요청 탭의 그림](images\pull-request.png)

## <a name="validation-process"></a>유효성 검사 프로세스

끌어오기 요청이 만들어지면 매니페스트의 유효성을 확인하고 끌어오기 요청을 처리하는 자동화 프로세스가 시작됩니다. 진행 상황을 추적할 수 있도록 레이블이 끌어오기 요청에 추가됩니다.

### <a name="submission-expectations"></a>제출에 대한 요구 사항

Windows 패키지 관리자 리포지토리에 대한 모든 애플리케이션 제출은 제대로 작동해야 합니다. 제출에 대한 몇 가지 요구 사항은 다음과 같습니다.

* 매니페스트는 [스키마 요구 사항](manifest.md#manifest-contents)을 준수합니다.
* 매니페스트의 모든 URL은 안전한 웹 사이트로 연결됩니다.
* 설치 관리자 및 애플리케이션에는 바이러스가 없습니다.
* 애플리케이션은 관리자 및 비 관리자 모두에 대해 올바르게 설치 및 제거됩니다.
* 설치 관리자는 비 대화형 모드를 지원합니다.
* 모든 매니페스트 항목은 정확하고 잘못 해석되지 않습니다.
* 설치 관리자는 게시자의 웹 사이트에서 직접 제공됩니다.

### <a name="pull-request-labels"></a>끌어오기 요청 레이블

유효성 검사 중에 일련의 레이블을 끌어오기 요청에 적용하여 진행 상황을 전달합니다.

* **요구 사항: 작성자 피드백**: 제출하는 동안 오류가 발생했습니다. 사용자에게 끌어오기 요청을 다시 할당합니다. 10일 이내에 문제가 해결되지 않으면 끌어오기 요청을 닫습니다.
* **Manifest-Validation-Error**: 제출된 매니페스트에 구문 오류가 있습니다.
* **URL-Validation-Error**: 제출에서 하나 이상의 URL이 [SmartScreen](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview) 유효성 검사에 실패했습니다.
* **Binary-Validation-Error**: 제출된 애플리케이션 설치 관리자에서 바이러스 검사 테스트에 실패했거나 해시가 일치하지 않습니다.
* **Pull-Request-Error**: 끌어오기 요청에 문제가 있습니다. 예를 들어 폴더 구조에 [필요한 형식](#step-3-add-your-manifest-to-the-local-repository)이 없습니다.
* **Validation-Error**: 제출된 애플리케이션에서 일반 유효성 검사 테스트에 실패했습니다.
* **Validation-Installation-Error**: 제출된 애플리케이션에서 설치 테스트에 실패했습니다.
* **Validation-Uninstall-Error**: 제출된 애플리케이션에서 제거 테스트에 실패했습니다.
* **Validation-Virus-Scan-Error**: 제출된 애플리케이션에서 바이러스 검사 테스트에 실패했습니다.
* **Azure-Pipeline-Passed**: 매니페스트에서 유효성 검사의 첫 번째 부분이 완료되었습니다. 이 단계 후에는 최종 유효성 검사를 위해 끌어오기 요청이 테스트 팀에 할당됩니다.
* **Validation-Completed**: 유효성 검사가 완료되고 끌어오기 요청이 병합됩니다.
