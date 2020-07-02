---
title: winget hash 명령
description: 설치 관리자에 대한 SHA256 해시를 생성합니다.
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4b6599a2b538829c6d9107b20f5f22d22f646542
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334563"
---
# <a name="hash-command-winget"></a>hash 명령(winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 도구의 **hash** 명령은 설치 관리자에 대한 SHA256 해시를 생성합니다. 이 명령은 소프트웨어를 GitHub의 **Microsoft 커뮤니티 패키지 매니페스트 리포지토리**에 제출하기 위한 [매니페스트 파일](../package/manifest.md)을 만들어야 하는 경우에 사용됩니다. 또한 **hash** 명령은 MSIX 파일에 대한 SHA256 인증서 해시 생성도 지원합니다.

## <a name="usage"></a>사용

`winget hash [-f] \<file> [\<options>]`

**hash** 하위 명령은 로컬 파일에서만 실행할 수 있습니다. **hash** 하위 명령을 사용하려면 설치 관리자를 알려진 위치에 다운로드합니다. 그런 다음, 파일 경로를 인수로 **hash** 하위 명령에 전달합니다.

## <a name="arguments"></a>인수

사용할 수 있는 인수는 다음과 같습니다.

| 인수  | 설명 |
|--------------|-------------|
| **-f,--file** |  해시할 파일의 경로입니다. |
| **-m,--msix**  | hash 명령이 MSIX 설치 관리자에서 사용할 SHA 256 SignatureSha256도 만들도록 지정합니다. |
| **-?, --help** |  이 명령에 대한 추가 도움말을 가져옵니다. |

## <a name="related-topics"></a>관련 항목

* [winget 도구를 사용하여 애플리케이션 설치 및 관리](index.md)
* [Windows 패키지 관리자에 패키지 제출](../package/index.md)
