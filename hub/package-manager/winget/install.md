---
title: install 명령
description: 지정된 애플리케이션을 설치합니다.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5daae6dabee1201dd9df0b83dc56f98b06b15487
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025177"
---
# <a name="install-command-winget"></a>install 명령(winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 도구의 **install** 명령은 지정된 애플리케이션을 설치합니다. [**search**](search.md) 명령을 사용하여 설치하려는 애플리케이션을 식별합니다.  

**install** 명령을 사용하려면 설치하기 위한 정확한 문자열을 지정해야 합니다. 문자열이 명확하지 않으면 **install** 명령을 정확한 애플리케이션으로 추가로 필터링하라는 메시지가 표시됩니다.

## <a name="usage"></a>사용

`winget install [[-q] \<query>] [\<options>]`

![search 명령](images\install.png)

## <a name="arguments"></a>인수

사용할 수 있는 인수는 다음과 같습니다.

| 인수      | 설명 |
|-------------|-------------|  
| **-q,--query**  |  앱을 검색하는 데 사용되는 쿼리입니다. |
| **-?, --help** |  이 명령에 대한 추가 도움말을 가져옵니다. |

## <a name="options"></a>옵션

옵션을 사용하면 설치 환경을 요구 사항에 맞게 사용자 지정할 수 있습니다.

| 옵션      | 설명 |
|-------------|-------------|  
| **-m, --manifest** |   매니페스트(YAML) 파일의 경로가 뒤에 나와야 합니다. 매니페스트를 사용하여 [로컬 YAML 파일](#local-install)에서 설치 환경을 실행할 수 있습니다. |
| **--id**    |  설치를 애플리케이션 ID로 제한합니다.   |  
| **--name**   |  검색을 애플리케이션 이름으로 제한합니다. |  
| **--moniker**   | 검색을 애플리케이션에 대해 나열된 모니커로 제한합니다. |  
| **-v, --version**  |  설치할 정확한 버전을 지정할 수 있습니다. 지정되지 않으면 가장 높은 버전의 최신 애플리케이션을 설치합니다. |  
| **-s, --source**   |  검색을 제공된 원본 이름으로 제한합니다. 원본 이름이 뒤에 나와야 합니다. |  
| **-e, --exact**   |   대/소문자 구분 검사를 포함하여 쿼리에서 정확한 문자열을 사용합니다. 하위 문자열의 기본 동작을 사용하지 않습니다. |  
| **-i, --interactive** |  대화형 모드에서 설치 관리자를 실행합니다. 기본 환경에는 설치 관리자 진행률이 표시됩니다. |  
| **-h, --silent** |  자동 모드에서 설치 관리자를 실행합니다. 이 경우 모든 UI가 표시되지 않습니다. 기본 환경에는 설치 관리자 진행률이 표시됩니다. |  
| **-o, --log**  |  로깅을 로그 파일에 보냅니다. 쓰기 권한이 있는 파일의 경로를 제공해야 합니다. |
| **--override** | 설치 관리자에 직접 전달되는 문자열입니다.    |
| **-l, --location** |    설치할 위치입니다(지원되는 경우). |

### <a name="example-queries"></a>예제 쿼리

다음 예제에서는 애플리케이션의 특정 버전을 설치합니다.

```CMD
winget install powertoys --version 0.15.2
```

다음 예제에서는 해당 ID에서 애플리케이션을 설치합니다.

```CMD
winget install --id Microsoft.PowerToys
```

다음 예제에서는 버전 및 ID별로 애플리케이션을 설치합니다.

```CMD
winget install --id Microsoft.PowerToys --version 0.15.2
```

## <a name="multiple-selections"></a>여러 선택 항목

**winget** 에 제공된 쿼리로 인해 단일 애플리케이션이 생성되지 않으면 **winget** 에서 검색 결과를 표시합니다. 이 경우 올바른 설치를 위해 검색을 구체화하는 데 필요한 추가 데이터가 제공됩니다.

선택 항목을 하나의 파일로 제한하는 가장 좋은 방법은 애플리케이션 **id** 를 **정확한** 쿼리 옵션과 결합하여 사용하는 것입니다.  예:

```CMD
winget install --id Git.Git -e 
```

## <a name="local-install"></a>로컬 설치

**manifest** 옵션을 사용하면 YAML 파일을 클라이언트에 직접 전달하여 애플리케이션을 설치할 수 있습니다. **manifest** 옵션의 사용법은 다음과 같습니다.

사용법: `winget install --manifest \<file>`

| 옵션  | 설명 |
|-------------|-------------|  
|  **-m, --manifest** | 설치할 애플리케이션의 매니페스트에 대한 경로입니다. |

### <a name="log-files"></a>로그 파일

리디렉션되지 않는 경우 winget에 대한 로그 파일은 **\%temp%\\AICLI\\*.log** 폴더에 있습니다.

## <a name="related-topics"></a>관련 항목

* [winget 도구를 사용하여 애플리케이션 설치 및 관리](index.md)
