---
title: winget validate 명령
description: 소프트웨어를 GitHub의 Microsoft 커뮤니티 패키지 매니페스트 리포지토리에 제출하기 위한 매니페스트 파일의 유효성을 검사합니다.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5772e144a4226be9fbb4a4949aaac1e3d4408e6
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824934"
---
# <a name="validate-command-winget"></a>validate 명령(winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 도구의 **validate** 명령은 소프트웨어를 GitHub의 **Microsoft 커뮤니티 패키지 매니페스트 리포지토리**에 제출하기 위한 [매니페스트 파일](../package/manifest.md)의 유효성을 검사합니다. 매니페스트는 이 [사양](https://github.com/microsoft/winget-pkgs/YamlSpec.md)을 따르는 YAML 파일이어야 합니다.

## <a name="usage"></a>사용

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>인수

사용할 수 있는 인수는 다음과 같습니다.

| 인수  | 설명 |
|--------------|-------------|
| **--manifest** |  유효성을 검사할 매니페스트의 경로입니다. |
| **-?, --help** |  이 명령에 대한 추가 도움말을 가져옵니다. |

## <a name="related-topics"></a>관련 항목

* [winget 도구를 사용하여 애플리케이션 설치 및 관리](index.md)
* [Windows 패키지 관리자에 패키지 제출](../package/index.md)
