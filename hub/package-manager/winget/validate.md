---
title: winget validate 명령
description: 소프트웨어를 GitHub의 Microsoft 커뮤니티 패키지 매니페스트 리포지토리에 제출하기 위한 매니페스트 파일의 유효성을 검사합니다.
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec1f15ef9086c1083430c9bbe55ea52827ae4bfb
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334461"
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
