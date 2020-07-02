---
title: search 명령
description: 설치할 수 있는 사용 가능한 애플리케이션에 대한 원본을 쿼리합니다.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 366d2176fe9465495d8ed8f2eeb7912229bd07f1
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334476"
---
# <a name="search-command-winget"></a>search 명령(winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 도구의 **search** 명령은 설치할 수 있는 사용 가능한 애플리케이션에 대한 원본을 쿼리합니다.  

**search** 명령은 사용 가능한 모든 애플리케이션을 표시하거나 특정 애플리케이션으로 필터링할 수 있습니다. **search** 명령은 일반적으로 특정 애플리케이션을 설치하는 데 사용할 문자열을 식별하는 데 사용됩니다.

## <a name="usage"></a>사용

`winget search [[-q] \<query>] [\<options>]`

![검색](images\search.png)

## <a name="arguments"></a>인수

사용할 수 있는 인수는 다음과 같습니다.

| 인수  | 설명 |
 --------------|-------------|
| **-q,--query** |  앱을 검색하는 데 사용되는 쿼리입니다. |
| **-?, --help** |  이 명령에 대한 추가 도움말을 가져옵니다. |

## <a name="show-all"></a>모두 표시

검색 명령에 필터 또는 옵션이 없는 경우 기본 원본에 사용 가능한 모든 애플리케이션이 표시됩니다. **source** 옵션만 전달하는 경우 다른 원본의 모든 애플리케이션을 검색할 수도 있습니다.

## <a name="search-strings"></a>검색 문자열

검색 문자열을 필터링하는 데 사용할 수 있는 옵션은 다음과 같습니다.

| 옵션  | 설명 |
 --------------|-------------|
| **--id**        |   검색을 애플리케이션 ID로 제한합니다. ID에는 게시자 및 애플리케이션 이름이 포함됩니다. |
| **--name**      |  검색을 애플리케이션 이름으로 제한합니다. |
| **--moniker**  |    검색을 지정된 모니커로 제한합니다. |
| **--tag**    |  검색을 애플리케이션에 대해 나열된 태그로 제한합니다. |
| **--command**   |   검색을 애플리케이션 이름으로 제한합니다. |

문자열이 하위 문자열로 처리됩니다. 또한 검색은 기본적으로 대/소문자를 구분하지 않습니다. 예를 들어 `winget search micro`는 다음을 반환할 수 있습니다.

* Microsoft
* microscope
* MyMicro

## <a name="search-options"></a>search 옵션

search 명령은 결과를 제한하는 데 도움이 되는 여러 옵션 또는 필터를 지원합니다.

| 옵션  | 설명 |
 --------------|-------------|
| **-e, --exact**  |     대/소문자 구분 검사를 포함하여 쿼리에서 정확한 문자열을 사용합니다. 하위 문자열의 기본 동작을 사용하지 않습니다.  |  
| **-n, --count**      |  표시 출력을 지정된 개수로 제한합니다. |
| **-s, --source**     |  검색을 지정된 [source](source.md) 이름으로 제한합니다.  |

## <a name="related-topics"></a>관련 항목

* [winget 도구를 사용하여 애플리케이션 설치 및 관리](index.md)
