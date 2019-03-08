---
title: IL2CPP 백엔드 포함 XDK용 Unity
description: IL2CPP 스크립팅 백 엔드를 사용 하 여 XDK에 대 한 Unity에 Xbox Live 지원을 추가 ID@Xbox 및 파트너 관리
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, Unity
ms.localizationpriority: medium
ms.openlocfilehash: cfd722ca0d0b080f6395680cd62000cea9b402fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608468"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>IL2CPP 스크립팅 백 엔드를 사용 하 여 XDK에 대 한 Unity에 Xbox Live 지원을 추가 ID@Xbox 및 파트너 관리

## <a name="overview"></a>개요

Unity에서 IL2CPP에 대 한 Windows 런타임 지원

Unity 5.6f3 릴리스의 엔진이 개발자가 스크립트에서 직접 (WinRT) Windows 런타임 구성 요소를 사용 하 여 직접 게임 프로젝트에 포함 하 여 새로운 기능을 포함 합니다. 까지 플러그 인 또는 기능을 지 원하는 모든 플랫폼 (Xbox Live SDK 포함) 게임 스크립트에서 dll을 5.6 개발자가 필요 합니다. 이 새 프로젝션 계층 요구 사항에 플러그 인을 제거 하 고 스크립팅 IL2CPP 백 엔드를 선택 하는 게임 에서만 지원 새롭고 간소화 된 워크플로 소개 합니다.

시작 하는 방법에 대 한 자세한 내용은 Unity 설명서를 참조 하세요. https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>단계

**1)-Unity를 설치 하는 중**

Unity 5.6 이상을 설치 및 Xbox One 편집기 확장이 설치 되어 있는지 확인 합니다.

**2)-Visual Studio Tools for Unity 버전 3.1 설치 하는 중 및 위의 intellisense를 사용할 때 지원 Winmd** Visual Studio 2015에 대 한이에서 찾을 수 있습니다 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity합니다.  Visual Studio 2017 용 Visual Studio 2017 설치 관리자 내에서 구성 요소를 추가할 수 있습니다.

**3) 새 또는 기존 Unity 프로젝트를 열려면**

**4) 플랫폼에서에서 전환 하는 Xbox One Unity 빌드 설정 메뉴**

**5)-Unity 플레이어 설정 IL2CPP 스크립팅 백 엔드를 사용 하는 중 및.NET 4.6으로 API 호환성 설정**

![](../images/unity/unity-il2cpp-1.png)

**6) Roslyn으로 스크립트 컴파일러를 전환 합니다.**

**Xbox One 7) 적절 한 시스템 라이브러리는 모두 자동으로 추가 프로젝트 및 플랫폼 이진 파일을 포함 하는 데 필요한 추가 단계 없이 합니다.**

**8) 추가 하 고 새 C 연결\# Unity 개체에는 스크립트입니다.**

예를 들어 "주 카메라", 예: Unity 개체를 클릭 하 고 "추가 구성 요소"를 클릭 \| "새 스크립트" \| C\# 스크립트 \| "XboxLiveScript" 이라는 이름을 지정 합니다. 모든 게임 개체를 수행 합니다.

**9) (VSTU 3.1 + 설치)와 Visual Studio에서 스크립트를 열거나 합니다.**

VSTU에서 생성 되는 "Player" 프로젝트에서 XboxLiveTest.cs 게임 스크립트를 열고 두 개의 프로젝트가 표시 됩니다.

![](../images/unity/unity-il2cpp-2.png)

XDK에서에 대해 생성 된 특수 프로젝트 이며 자산에 배치한 winmd 파일에 대 한 참조가 포함 되어 있습니다.
"ENABLE_WINMD_SUPPORT #if" 정의 IntelliSense 구문 강조 표시 하 고 제대로 작동 됩니다 있도록를 정의 합니다.

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

**플레이어 설정에서 게시 설정에서 선택한 'InternetClient' 기능 해야 11)**

![](../images/unity/unity-il2cpp-3.png)

**12) Unity에서 프로젝트를 빌드하십시오.**
