---
title: show 명령
description: 애플리케이션 원본에 대한 세부 정보 및 애플리케이션과 연결된 메타데이터를 포함하여 지정된 애플리케이션에 대한 세부 정보를 표시합니다.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 9443bb31133ee9643571a4861af7027272b35d94
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334500"
---
# <a name="show-command-winget"></a>show 명령(winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 도구의 **show** 명령은 애플리케이션 원본에 대한 세부 정보 및 애플리케이션과 연결된 메타데이터를 포함하여 지정된 애플리케이션에 대한 세부 정보를 표시합니다.

**show** 명령은 애플리케이션과 함께 제출된 메타데이터만 표시합니다. 제출된 애플리케이션에서 일부 메타데이터를 제외하는 경우 해당 데이터가 표시되지 않습니다.

## <a name="usage"></a>사용

`winget show [[-q] \<query>] [\<options>]`

![show 명령](images\show.png)

## <a name="arguments"></a>인수

사용할 수 있는 인수는 다음과 같습니다.

| 인수  | 설명 |
|--------------|-------------|
| **-q,--query** |  애플리케이션을 검색하는 데 사용되는 쿼리입니다. |
| **-?, --help** |  이 명령에 대한 추가 도움말을 가져옵니다. |

## <a name="options"></a>옵션

다음 옵션을 사용할 수 있습니다.

| 옵션  | 설명 |
|--------------|-------------|
| **-m,--manifest** | 설치할 애플리케이션의 매니페스트에 대한 경로입니다. |
| **--id**         |  ID를 기준으로 결과를 필터링합니다. |
| **--name**   |      이름을 기준으로 결과를 필터링합니다. |
| **--moniker**   |  애플리케이션 모니커를 기준으로 결과를 필터링합니다. |
| **-v,--version** |  지정된 버전을 사용합니다. 기본값은 최신 버전입니다. |
| **-s,--source** |   지정된 [원본](source.md)을 사용하여 애플리케이션을 찾습니다. |
| **-e,--exact**     | 정확한 일치를 사용하여 애플리케이션을 찾습니다. |
| **--versions**    | 사용 가능한 애플리케이션 버전을 표시합니다. |

## <a name="multiple-selections"></a>여러 선택 항목

**winget**에 제공된 쿼리로 인해 단일 애플리케이션이 생성되지 않으면 **winget**에서 검색 결과를 표시합니다. 이 경우 검색을 구체화하는 데 필요한 추가 데이터가 제공됩니다.

## <a name="results-of-show"></a>show의 결과

단일 애플리케이션이 검색되면 다음 데이터가 표시됩니다.

### <a name="metadata"></a>메타데이터

| Value  | 설명 |
|--------------|-------------|
| **ID**   | 애플리케이션의 ID입니다. |
| **이름**  | 애플리케이션의 이름입니다. |
| **Publisher** | 애플리케이션의 게시자입니다. |
| **버전** | 애플리케이션의 버전입니다. |
| **Author**  | 애플리케이션의 작성자입니다. |
| **AppMoniker** | 애플리케이션의 AppMoniker입니다. |
| **설명** | 애플리케이션에 대한 설명입니다. |
| **라이선스**  | 애플리케이션의 라이선스입니다. |
| **LicenseUrl** | 애플리케이션의 라이선스 파일에 대한 URL입니다. |
| **Homepage**  | 애플리케이션의 홈페이지입니다. |
| **Tags** | 검색을 지원하기 위해 제공되는 태그입니다.  |
| **명령** | 애플리케이션에서 지원하는 명령입니다. |
| **Channel**  | 애플리케이션이 미리 보기인지, 아니면 릴리스인지에 대한 세부 정보입니다.  |
| **Minimum OS Version** | 애플리케이션에서 지원하는 최소 OS 버전입니다. |

### <a name="installer-details"></a>설치 관리자 세부 정보

| Value  | 설명 |
|--------------|-------------|
| **Arch**   | 설치 관리자의 아키텍처입니다. |
| **언어**  | 설치 관리자의 언어입니다. |
| **Installer Type**  | 설치 관리자의 유형입니다. |
| **Download Url** | 설치 관리자의 URL입니다. |
| **Hash** | 설치 관리자의 Sha-256입니다.  |
| **범위** | 설치 관리자가 머신 단위인지, 아니면 사용자 단위인지를 표시합니다. |

## <a name="related-topics"></a>관련 항목

* [winget 도구를 사용하여 애플리케이션 설치 및 관리](index.md)
