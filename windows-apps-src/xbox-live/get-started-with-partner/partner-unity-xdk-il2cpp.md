---
title: IL2CPP 백엔드 포함 XDK용 Unity
author: KevinAsgari
description: 에 대 한 IL2CPP 스크립팅 백 엔드를 사용 하 여 XDK 용 Unity에 Xbox Live 지원 추가 ID@Xbox 관리 파트너 및
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나 Unity
ms.localizationpriority: medium
ms.openlocfilehash: 58c10ba1a74b3b2cd99c1171d8305b55f68212fe
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5472197"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>에 대 한 IL2CPP 스크립팅 백 엔드를 사용 하 여 XDK 용 Unity에 Xbox Live 지원 추가 ID@Xbox 관리 파트너 및

## <a name="overview"></a>개요

Unity의 IL2CPP에 대 한 Windows 런타임 지원

Unity 5.6f3 릴리스되면서 엔진에 개발자가 스크립트에서 직접 Windows 런타임 (WinRT) 구성 요소를 사용 하 여 직접 게임 프로젝트에 포함 하 여 새로운 기능 포함 되어 있습니다. 까지 5.6 개발자 플러그 인, 또는 dll 게임 스크립트에서 Xbox Live SDK 등 모든 플랫폼 기능을 지원 하기 위해 필요 합니다. 이 새 프로젝션 계층 플러그인 요건을 제거 하 고 IL2CPP 스크립팅 백 엔드를 선택 하는 게임에만 지원 새롭고 간소화 된 워크플로 소개 합니다.

시작 하는 방법에 대 한 자세한 내용은 Unity 설명서를 참조 하세요.https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>단계

**Unity를 설치 하는 1)**

Unity 5.6 이상를 설치 하 고 설치 된 Xbox One 편집기 확장 해야 합니다.

**2) 설치 Visual Studio Tools 버전 3.1 Unity 이상 WinMDs를 사용 하는 경우 IntelliSense 지원에 대 한** Visual Studio 2015에 대 한이에서 확인할 수 있습니다 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Visual Studio 2017 용 Visual Studio 2017 설치 관리자 구성 요소를 추가할 수 있습니다.

**3) 새로운 또는 기존 Unity 프로젝트를 엽니다.**

**4) 스위치 Unity 빌드 설정 메뉴에서 Xbox one 플랫폼**

**Unity 플레이어 설정에서 IL2CPP 스크립팅 백 엔드를 사용 하는 5) 및.NET 4.6 API 호환성 설정**

![](../images/unity/unity-il2cpp-1.png)

**6) 스크립트 컴파일러 Roslyn 전환**

**7) Xbox One 적절 한 시스템 라이브러리는 모두 자동으로 추가 프로젝트 및 플랫폼 바이너리를 포함 하는 데 필요한 단계를 추가 하지 합니다.**

**8) 추가 하 고 Unity 개체에 새 C\ # 스크립트를 연결 합니다.**

예를 들어 "기본 카메라"와 같은 Unity 개체를 클릭 하 고 "구성 요소 추가"를 클릭 합니다. \ | "새 스크립트" \ | C\ # 스크립트 \ | 하 고 "XboxLiveScript" 라는 이름을 지정 합니다. 모든 게임 개체를 수행 합니다.

**9) (VSTU 3.1 + 설치)와 Visual Studio에서 스크립트를 열어 합니다.**

게임 스크립트 XboxLiveTest.cs VSTU에서 생성 된 "플레이어" 프로젝트에서 열고 두 프로젝트를 알 수 있습니다.

![](../images/unity/unity-il2cpp-2.png)

XDK를 생성 하는 특별 한 프로젝트 이며 자산에 배치한 winmd 파일에 대 한 참조를 포함 합니다.
"#If ENABLE_WINMD_SUPPORT"를 정의 하는 또한 되므로 IntelliSense 및 구문 강조 제대로 작동을 정의 합니다.

**10) XboxLiveTest.cs 소스 파일에 다음 Xbox Live 코드를 추가 합니다.**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**플레이어 설정에서 게시 설정에서 선택한 'InternetClient' 기능이 있는 11) 확인**

![](../images/unity/unity-il2cpp-3.png)

**12) Unity에서 프로젝트를 빌드하십시오.**
