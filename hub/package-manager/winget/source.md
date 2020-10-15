---
title: source 명령
description: Windows 패키지 관리자에서 액세스하는 리포지토리를 관리합니다.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 08af76389627bb8c21bf7a4ddb856d09119dc917
ms.sourcegitcommit: 837ef4b2c2375d023ee85204f72a029f9ec8f4ee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92079278"
---
# <a name="source-command-winget"></a>source 명령(winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

> [!NOTE]
> **source** 명령은 현재 내부용으로만 사용됩니다. 추가 원본은 현재 지원되지 않습니다.

[winget](index.md) 도구의 **source** 명령은 Windows 패키지 관리자에서 액세스하는 리포지토리를 관리합니다. **source** 명령을 사용하면 리포지토리를 **추가**, **제거**, **나열** 및 **업데이트**할 수 있습니다.

원본은 애플리케이션을 검색하여 설치할 수 있는 데이터를 제공합니다. 안전한 위치로 신뢰하는 경우에만 새 원본을 추가합니다.

## <a name="usage"></a>사용

`winget source \<sub command> \<options>`

![원본 이미지](images\source.png)

## <a name="arguments"></a>인수

사용할 수 있는 인수는 다음과 같습니다.

| 인수  | 설명 |
|--------------|-------------|
| **-?, --help** |  이 명령에 대한 추가 도움말을 가져옵니다. |

## <a name="sub-commands"></a>하위 명령

원본을 조작하기 위해 원본에서 지원하는 하위 명령은 다음과 같습니다.

| 하위 명령  | 설명 |
|--------------|-------------|
|  **add** |  새 원본을 추가합니다. |
|  **list** | 사용하도록 설정된 원본의 목록을 열거합니다. |
|  **update** | 원본을 업데이트합니다. |
|  **remove** | 원본을 제거합니다. |
|  **reset** | **winget**을 초기 구성으로 다시 설정합니다.  |

## <a name="options"></a>옵션

**source** 명령에서 지원하는 옵션은 다음과 같습니다.

| 옵션  | 설명 |
|--------------|-------------|
|  **-n,--name** | 원본을 식별하기 위한 기준이 되는 이름입니다. |
|  **-a,--arg** | 원본의 URL 또는 UNC입니다. |
|  **-t,--type** | 원본의 형식입니다. |
| **-?, --help** |  이 명령에 대한 추가 도움말을 가져옵니다. |

## <a name="add"></a>추가

**add** 하위 명령은 새 원본을 추가합니다. 이 하위 명령을 사용하려면 **--name** 옵션 및 **name** 인수가 필요합니다.

사용법: `winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

예제: `winget source add --name Contoso  https://www.contoso.com/cache`

또한 **add** 하위 명령은 선택적 **type** 매개 변수를 지원합니다. **type** 매개 변수는 연결되는 리포지토리 유형을 클라이언트에 전달합니다. 지원되는 유형은 다음과 같습니다.

| 유형  | 설명 |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | 원본 \<default>의 유형입니다. |

## <a name="list"></a>list

**list** 하위 명령은 현재 사용하도록 설정된 원본을 열거합니다. 이 하위 명령은 특정 원본에 대한 세부 정보도 제공합니다.

사용법: `winget source list [-n, --name] \<name>`

### <a name="list-all"></a>list all

**list** 하위 명령 자체는 지원되는 원본의 전체 목록을 표시합니다. 예:

```CMD
> C:\winget source list
> Name   Arg
> -----------------------------------------
> winget https://winget.azureedge.net/cache

```

### <a name="list-source-details"></a>list source details

원본에 대한 전체 세부 정보를 가져오려면 원본을 식별하는 데 사용되는 이름을 전달합니다. 예:

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

**Name**은 원본을 식별하기 위한 기준이 되는 이름을 표시합니다.
**Type**은 리포지토리 유형을 표시합니다.
**Arg**는 원본에서 사용하는 URL 또는 경로를 표시합니다.
**Data**는 해당하는 경우 사용되는 선택적 패키지 이름을 표시합니다.
**Updated**는 원본이 업데이트된 마지막 날짜와 시간을 표시합니다.

## <a name="update"></a>업데이트

**update** 하위 명령은 개별 원본 또는 모든 원본을 강제로 업데이트합니다.

사용법: `winget source update [-n, --name] \<name>`

### <a name="update-all"></a>update all

**update** 하위 명령 자체는 각 리포지토리에 요청하고 업데이트합니다. 예: `C:\winget update`

### <a name="update-source"></a>update source

**--name** 옵션과 결합된 **update** 하위 명령은 개별 원본으로 이동하여 업데이트할 수 있습니다. 예: `C:\winget source update --name contoso`

## <a name="remove"></a>remove

**remove** 하위 명령은 원본을 제거합니다. 이 하위 명령에는 원본을 식별하기 위해  **--name** 옵션 및 **name 인수**가 필요합니다.

사용법: `winget source remove [-n, --name] \<name>`

예: `winget source remove --name Contoso`

## <a name="reset"></a>reset

**reset** 하위 명령은 클라이언트를 원래 구성으로 다시 설정합니다. **reset** 하위 명령은 모든 원본을 제거하고 원본을 기본값으로 설정합니다. 이 하위 명령은 드문 경우에만 사용해야 합니다.

사용법: `winget source reset`

예: `winget source reset`

## <a name="default-repository"></a>기본 리포지토리

Windows 패키지 관리자에서 기본 리포지토리를 지정합니다. **list** 명령을 사용하여 리포지토리를 식별할 수 있습니다. 예: `winget source list`

## <a name="related-topics"></a>관련 항목

* [winget 도구를 사용하여 애플리케이션 설치 및 관리](index.md)
