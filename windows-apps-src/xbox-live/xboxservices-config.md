---
title: XboxServices.config
description: UWP 게임을 Xbox Live 구성과 연결 하기 위한 XboxServices.config 파일에 설명 합니다.
ms.date: 03/29/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 서비스 구성, xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: 8ff538d691627bf4bb12b3ef6f8b1360e59ac701
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2018
ms.locfileid: "8322577"
---
# <a name="xboxservicesconfig-file-description"></a>XboxServices.config 파일 설명

개발할 때 Xbox Live UWP 게임에 사용 하도록 설정, 프로젝트에 XboxServices.config 파일을 포함 해야 합니다.  이 파일에는 Xbox Live SDK 파트너 센터의 앱 및 Xbox Live 서비스 구성을 사용 하 여 게임을 연결할 수 있습니다. 이 파일 서비스 구성 ID, 제목 ID 등과 같은 세부 정보는 JSON 개체를 포함 합니다.

Unity를 Xbox Live 플러그 인을 사용 하 여 Xbox Live 크리에이터 스 프로그램 게임 디자인을 사용 하는 경우이 파일은 자동으로 생성 하 여 Xbox Live 연결 마법사.

## <a name="xboxservicesconfig-fields"></a>XboxServices.config 필드

>[!NOTE]
> Xbox Live 연결 마법사에서 생성 된 파일, 아래에 설명 된 것을 넘어 추가 필드가 포함 하지만 서비스에서 사용 되지 않습니다.

구성 파일의 JSON 개체에는 다음과 같은 필드가 정의 됩니다.

필드 | 설명
--- | ---
PrimaryServiceConfigId  |  Xbox Live 서비스 구성 ID (서비스 안내). [파트너 센터](https://partner.microsoft.com/dashboard)앱에 대 한 **서비스** 섹션 아래에서 **Xbox Live** 페이지 (크리에이터 스 프로그램) 또는 (게임에 대 한 전체 Xbox Live), **Xbox Live 설정** 페이지에서이 값을 찾을 수 있습니다.
TitleId  |  앱에 대 한 10 진수 타이틀 ID입니다. [파트너 센터](https://partner.microsoft.com/dashboard)앱에 대 한 **서비스** 섹션 아래에서 **Xbox Live** 페이지 (크리에이터 스 프로그램) 또는 (게임에 대 한 전체 Xbox Live), **Xbox Live 설정** 페이지에서이 값을 찾을 수 있습니다.
XboxLiveCreatorsTitle  |  "True" 인 경우에 앱 Xbox Live 크리에이터 스 프로그램 앱 임을 나타냅니다. 그렇지 않으면 "false"입니다.
범위  |  **(선택 사항)** 앱에서 사용 되는 기능의 범위를 정의 합니다. 자세한 설명 아래를 참조 하세요.

### <a name="scope-field"></a>범위 필드

**범위** 필드는 게임에서 사용 되는 기능을 나타내는 데 사용할 수 있는 선택적 필드.


**범위** 필드를 지정 하지 않은 경우 다음 범위는 값으로 설정 기본 **XboxLiveCreatorsTitle** 필드의 값에 따라 다음 표에 설명 된 대로:

XboxLiveCreatorsTitle 값 | 기본값 범위
--- | ---
"true"  |  "xbl.signin xbl.friends"
"false"  |  "xboxlive.signin"



다음은 **범위** 필드에 대 한 유효한 값을 설명합니다.

범위 값 | 설명
--- | ---
xbl.signin  | 크리에이터 스 프로그램 게임에 대 한 기능에 기호를 포함합니다. 크리에이터 스 프로그램 게임에 필요합니다.
xbl.friends | 친구 및 크리에이터 스 프로그램 게임 소셜 순위표 기능이 포함 됩니다.
xboxlive.signin | Xbox Live의 모든 기능에 액세스 하는 게임에 대 한 기능에 기호를 포함 합니다. 비 크리에이터 스 프로그램 게임에 필요합니다.

현재 **범위** 필드를 지정 하는 이유는 경우 하는 Xbox Live 크리에이터 스 프로그램 게임 및 게임 친구 목록이 나 소셜 순위표 (친구 범위는 순위표)에 액세스할 필요가 없습니다. 이 경우 XboxServices.config 파일에 다음 줄을 추가할 수 있습니다.

```
  "Scope" : "xbl.signin"
```

이 줄을 추가 UWP 앱을에서 처음으로 앱을 시작할 때 친구 목록 액세스 권한을 요청 하지 않습니다.

## <a name="example-xboxservicesconfig-file"></a>XboxServices.config 파일 예제

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
