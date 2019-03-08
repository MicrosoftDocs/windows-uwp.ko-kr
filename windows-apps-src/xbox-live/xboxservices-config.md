---
title: XboxServices.config
description: UWP 게임을 Xbox Live 구성을 사용 하 여 연결에 대 한 XboxServices.config 파일을 설명 합니다.
ms.date: 03/29/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 서비스 구성, xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: 8ff538d691627bf4bb12b3ef6f8b1360e59ac701
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606688"
---
# <a name="xboxservicesconfig-file-description"></a>XboxServices.config 파일 설명

개발 하는 경우 UWP 게임을 사용 하는 Xbox Live, 프로젝트가 XboxServices.config 파일을 포함 해야 합니다.  이 파일에는 Xbox Live SDK 파트너 센터 앱 및 Xbox Live 서비스 구성을 사용 하 여 게임을 연결할 수 있습니다. 이 파일에서 서비스 구성 ID, 제목 ID 등과 같은 세부 정보는 JSON 개체를 포함 합니다.

Unity Xbox Live 플러그 인을 사용 하 여 Xbox Live 크리에이터 스 프로그램 게임을 디자인 하려면를 사용 하는 경우이 파일 자동으로 만들어집니다 마법사로 Xbox Live 연결 합니다.

## <a name="xboxservicesconfig-fields"></a>XboxServices.config fields

>[!NOTE]
> Xbox Live 연결 마법사에서 만든 파일 아래에 설명 된 것 이상의 추가 필드가 포함 될 수 있습니다 하지만 서비스에서 사용 되지 않습니다.

다음 필드를 구성 파일에 있는 JSON 개체에서 정의 됩니다.

필드 | 설명
--- | ---
PrimaryServiceConfigId  |  Xbox Live 서비스 구성 서비스 (안내 ID)입니다. [파트너 센터](https://partner.microsoft.com/dashboard)에서이 값을 찾을 수 있습니다 합니다 **Xbox Live** 페이지 (크리에이터 스 프로그램) 또는 **Xbox Live 설치** (전체 Xbox Live 게임에) 페이지의 **Services** 앱에 대 한 섹션입니다.
TitleId  |  앱에 대 한 10 진수 제목 ID입니다. [파트너 센터](https://partner.microsoft.com/dashboard)에서이 값을 찾을 수 있습니다 합니다 **Xbox Live** 페이지 (크리에이터 스 프로그램) 또는 **Xbox Live 설치** (전체 Xbox Live 게임에) 페이지의 **Services** 앱에 대 한 섹션입니다.
XboxLiveCreatorsTitle  |  "True" 일 경우에 앱 Xbox Live 크리에이터 스 프로그램 앱 임을 나타냅니다. 그렇지 않으면, "false"입니다.
범위  |  **(선택 사항)**  앱에서 사용 되는 기능의 범위를 정의 합니다. 아래의 자세한 설명을 참조 하세요.

### <a name="scope-field"></a>범위 필드

합니다 **범위** 필드는 게임에서 사용 하는 기능을 나타내기 위해 사용할 수 있는 선택적 필드입니다.


경우는 **범위** 필드를 지정 하지 않으면 다음 범위 값에 따라 달라 지는 기본 값으로 설정 되는 **XboxLiveCreatorsTitle** 다음 표에 설명 된 대로 필드:

XboxLiveCreatorsTitle 값 | 기본 범위 값
--- | ---
"true"  |  "xbl.signin xbl.friends"
"false"  |  "xboxlive.signin"



다음 목록에 대 한 유효한 값을 설명 합니다 **범위** 필드입니다.

범위 값 | 설명
--- | ---
xbl.signin  | 크리에이터 스 프로그램 게임에 대 한 기능에는 로그인을 포함합니다. 크리에이터 스 프로그램 게임에 필요합니다.
xbl.friends | 친구와 크리에이터 스 프로그램 게임 순위표 소셜 기능을 포함합니다.
xboxlive.signin | Xbox Live의 전체 기능에 액세스 하는 게임에 대 한 기능에 로그인을 포함 합니다. 비-크리에이터 스 프로그램 게임에 필요합니다.

지정 하는 유일한 이유 현재 합니다 **범위** 필드 이며 경우 하는 Xbox Live 크리에이터 스 프로그램 게임, 게임 친구 목록 또는 소셜 순위표 (친구에 게 범위가 설정 되는 순위표)에 액세스할 필요가 없습니다. 이 경우 XboxServices.config 파일에 다음 줄을 추가할 수 있습니다.

```
  "Scope" : "xbl.signin"
```

이 줄을 추가 UWP 앱을에서 friend 목록을 처음으로 앱을 시작 하면 액세스할 수 있는 권한을 요청 하지 않습니다.

## <a name="example-xboxservicesconfig-file"></a>예제 XboxServices.config 파일

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
