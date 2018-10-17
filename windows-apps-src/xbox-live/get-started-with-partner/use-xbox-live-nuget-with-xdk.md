---
title: Xbox Live NuGet 패키지는 XDK 사용
author: KevinAsgari
description: XDK 타이틀을 개발 하는 Xbox Live API NuGet 패키지를 사용 하는 방법을 알아봅니다.
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나 NuGet
ms.localizationpriority: medium
ms.openlocfilehash: b8b12201c0511339c4dd38824e17f7586e03708e
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4744671"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>Xbox Live API NuGet 패키지를 사용 하 여 XDK 타이틀 개발

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.는 최신 NuGet 패키지 관리자가 설치 되어 있는지 확인
1.  현재 버전을 확인 합니다.
    - 메뉴 선택 도구 모음에서 확장 및 업데이트-> 합니다.
    - 검색할 설치 됨 탭 `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  현재 버전을 업데이트:
    - 메뉴 선택 도구 모음에서 확장 및 업데이트-> 합니다.
    - 업데이트에서 Visual Studio 갤러리 탭->, 선택 `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2. 프로젝트에 참조를 추가 합니다.
1.  프로젝트에 참조를 추가 합니다.
    1.  프로젝트 솔루션을 마우스 오른쪽 단추로 클릭 하 고 "NuGet 패키지 관리"를 선택 합니다.
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  검색 `Xbox Live` 적절 한 패키지를 선택 하 고 클릭 `Install`.
  - Xbox 서비스 API는 c + + 및 WinRT 및 UWP 및 XDK, 모두에 대 한 버전에서 제공 됩니다.  
  - 중에서 선택 `Microsoft.Xbox.Live.SDK.*.UWP` 및 `Microsoft.Xbox.Live.SDK.*.XboxOneXDK`.  `XboxOneXDK` ID@Xbox 및 Xbox One XDK를 사용 하는 관리 되는 개발자.  `UWP` PC, Xbox One 또는 Windows Phone 실행할 수 있는 UWP 게임을 위한 것입니다.  자세한 내용은에 Xbox One에서 UWP를 실행 하는 방법에 대 한[https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - 중에서 선택 `Microsoft.Xbox.Live.SDK.Cpp.*` 및 `Microsoft.Xbox.Live.SDK.WinRT.*`. `Cpp` Xbox Live Api를 사용 하 여 c + + 게임 엔진입니다.  `WinRT` c + +, C# 또는 Xbox Live Api를 사용 하 여 Javascript로 작성 된 게임 엔진입니다.  C + + 엔진을 사용 하 여 WinRT을 사용할 때 사용 하 여 C + + CX hat (^)을 사용 합니다.  `Cpp` c + + 게임 엔진에 대 한 사용 하는 권장된 API입니다.    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. 라이선스 TOS를 수락한 후 패키지가 성공적으로 추가 될 때까지 기다립니다.  이 로그 패키지 관리자 출력 창에 표시 됩니다.

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3. 필요한 경우 헤더를 포함
* 에 대 한 `Microsoft.Xbox.Live.SDK.Cpp.*` 프로젝트 기반 `#include <xsapi\services.h>` 에서 프로젝트의 소스입니다.
* 에 대 한 `Microsoft.Xbox.Live.SDK.WinRT.*` 기반 프로젝트의 경우 있는 헤더를 포함할 필요가 없습니다.   
