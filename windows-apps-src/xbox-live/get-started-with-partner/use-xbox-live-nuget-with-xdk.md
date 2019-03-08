---
title: XDK Xbox Live NuGet 패키지 사용
description: Xbox Live API NuGet 패키지를 사용 하 여 XDK 타이틀을 개발 하는 방법에 알아봅니다.
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, NuGet
ms.localizationpriority: medium
ms.openlocfilehash: c955ca42c09075e5125683588c335cfa47443f00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655038"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>Xbox Live API NuGet 패키지를 사용 하 여 개발 XDK 제목

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.  최신 NuGet 패키지 관리자 설치 했는지 확인
1.  현재 버전을 확인 합니다.
    - 메뉴 선택 도구 모음에서-> 확장 및 업데이트 합니다.
    - 설치 됨 탭에 대 한 확인 `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  현재 버전을 업데이트 합니다.
    - 메뉴 선택 도구 모음에서-> 확장 및 업데이트 합니다.
    - 업데이트-> Visual Studio 갤러리 탭을 선택 합니다 `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2.  프로젝트에 대 한 참조를 추가 합니다.
1.  프로젝트에 대 한 참조를 추가 합니다.
    1.  프로젝트 솔루션을 마우스 오른쪽 단추로 클릭 하 고 "NuGet 패키지 관리"를 선택 합니다.
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  검색할 `Xbox Live` 적절 한 패키지를 선택 하 고 클릭 `Install`합니다.
  - Xbox 서비스 API는 UWP와 XDK, 및 c + + 및 WinRT에 대 한 버전에서 제공 됩니다.  
  - 선택할 `Microsoft.Xbox.Live.SDK.*.UWP` 고 `Microsoft.Xbox.Live.SDK.*.XboxOneXDK`입니다.  `XboxOneXDK` ID@Xbox 및 Xbox One XDK 사용 하는 관리 되는 개발자.  `UWP` PC, Xbox One 또는 Windows Phone 실행할 수 있는 UWP 게임입니다.  자세한 내용은 UWP에서 Xbox One에서 실행 하는 방법에 대 한 [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - 선택할 `Microsoft.Xbox.Live.SDK.Cpp.*` 고 `Microsoft.Xbox.Live.SDK.WinRT.*`입니다. `Cpp` Xbox Live Api를 사용 하 여 c + + 게임 엔진입니다.  `WinRT` c + +로 작성 된 게임 엔진은 C#, 또는 Xbox Live Api를 사용 하 여 Javascript입니다.  WinRT는 c + + 엔진을 사용 하 여 사용 하는 경우 사용 C + + /cli CX hats (^)를 사용 합니다.  `Cpp` c + + 게임 엔진을 사용 하도록 권장 되는 API입니다.    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. 라이선스 TOS를 수락 하면 패키지를 추가 했습니다 될 때까지 기다립니다.  이 로그는 패키지 관리자 출력 창에 표시 됩니다.

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3.  필요에 따라 헤더를 포함 합니다.
* 에 대 한 `Microsoft.Xbox.Live.SDK.Cpp.*` 기반 프로젝트 `#include <xsapi\services.h>` 프로젝트의 원본에 있습니다.
* 에 대 한 `Microsoft.Xbox.Live.SDK.WinRT.*` 기반 프로젝트에서 모든 헤더를 포함할 필요가 없습니다.   
