---
title: IL2CPP 백엔드 포함 UWP용 Unity
description: IL2CPP 스크립팅 백 엔드를 사용 하 여 UWP에 대 한 Unity에 Xbox Live 지원을 추가 ID@Xbox 및 파트너 관리
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622918"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>IL2CPP 스크립팅 백 엔드를 사용 하 여 UWP에 대 한 Unity에 Xbox Live 지원을 추가 ID@Xbox 및 파트너 관리

## <a name="overview"></a>개요

Unity에서 IL2CPP에 대 한 Windows 런타임 지원

Unity 5.6f3 릴리스의 엔진이 개발자가 스크립트에서 직접 (WinRT) Windows 런타임 구성 요소를 사용 하 여 직접 게임 프로젝트에 포함 하 여 새로운 기능을 포함 합니다. 까지 플러그 인 또는 기능을 지 원하는 모든 플랫폼 (Xbox Live SDK 포함) UWP의 게임 스크립트에서 dll을 5.6 개발자가 필요 합니다. 이 새 프로젝션 계층 요구 사항에 플러그 인을 제거 하 고 스크립팅 IL2CPP 백 엔드를 선택 하는 게임 에서만 지원 새롭고 간소화 된 워크플로 소개 합니다.

시작 하는 방법에 대 한 자세한 내용은 Unity 설명서를 참조 하세요. https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>단계

**1)-Unity를 설치 하는 중**

Unity 5.6 이상 설치 하 고 있는지 확인 합니다 **백 엔드 스크립팅 하는 Windows 스토어 Il2CPP** 설치 중에 선택

**2)-Visual Studio Tools for Unity 버전 3.1 설치 하는 중 및 위의 intellisense를 사용할 때 지원 Winmd** Visual Studio 2015에 대 한이에서 찾을 수 있습니다 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity합니다.  Visual Studio 2017 용 Visual Studio 2017 설치 관리자 내에서 구성 요소를 추가할 수 있습니다.

**3) 새 또는 기존 Unity 프로젝트를 열려면**

**4) 플랫폼에서에서 전환 하는 유니버설 Windows 플랫폼의 Unity 빌드 설정 메뉴**

**5)-Unity 플레이어 설정 IL2CPP 스크립팅 백 엔드를 사용 하는 중 및.NET 4.6으로 API 호환성 설정**

![](../images/unity/unity-il2cpp-1.png)

**6) 가져오기 Xbox Live WinRT Unity 자산 패키지의 최신 버전** 에서 찾을 수 있습니다 https://github.com/Microsoft/xbox-live-api/releases

**7) 추가 하 고 새 C 연결\# Unity 개체에는 스크립트입니다.**

예를 들어 "주 카메라", 예: Unity 개체를 클릭 하 고 "추가 구성 요소"를 클릭 \| "새 스크립트" \| C\# 스크립트 \| "XboxLiveScript" 이라는 이름을 지정 합니다. 모든 게임 개체를 수행 합니다.

**8) (VSTU 3.1 + 설치)와 Visual Studio에서 스크립트를 열거나 합니다.**

VSTU에서 생성 되는 "Player" 프로젝트에서 XboxLiveTest.cs 게임 스크립트를 열고 두 개의 프로젝트가 표시 됩니다.

![](../images/unity/unity-il2cpp-2.png)

UWP에 대해 생성 된 특수 프로젝트 이며 자산에 배치한 winmd 파일에 대 한 참조가 포함 되어 있습니다.
"ENABLE_WINMD_SUPPORT #if" 정의 IntelliSense 구문 강조 표시 하 고 제대로 작동 됩니다 있도록를 정의 합니다.

**9) XboxLiveTest.cs 소스 파일에 다음 Xbox Live 코드를 추가 합니다.**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new   Microsoft.Xbox.Services.System.XboxLiveUser();

    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
#endif
    string debugText = "";
    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;
        UIDispatcher = cw.Dispatcher;
        SignIn();
#endif
    }
    // Update is called once per frame
    void Update()
    {
    }
    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }
#if ENABLE_WINMD_SUPPORT
    async void SignIn()
    {
        Microsoft.Xbox.Services.System.SignInResult result = await m_user.SignInAsync(UIDispatcher);
        if (result.Status == Microsoft.Xbox.Services.System.SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }

    }
#endif
}

```

**플레이어 설정에서 게시 설정에서 선택한 'InternetClient' 기능이 있는 10)**

![](../images/unity/unity-il2cpp-3.png)

**11) Unity에서 프로젝트를 빌드하십시오.**

1.  파일로 이동 \| 빌드 설정 클릭 **유니버설 Windows 플랫폼** 클릭 하면 확인 **플랫폼 전환**

2.  현재 장면 빌드에 추가할 "Open 장면 추가"를 클릭 합니다.

3.  SDK 콤보 상자에 "유니버설 10" 선택

4.  UWP 빌드 유형 콤보 상자에서 "D3D"를 선택 하지만 원하는 경우에 "XAML" 작동 합니다.

5.  UWP 응용 프로그램에서 Unity 게임을 래핑하는 UWP Visual Studio 프로젝트를 생성 하려면 Unity에 대 한 "빌드"를 클릭 합니다. 위치에 대 한 메시지가 나타나면 경우 혼동을 피하기 위해 많은 새 파일이 만들어질 수 있으므로 새 폴더를 만듭니다. "빌드" 폴더를 호출 하 고 해당 폴더를 선택한 것이 좋습니다.

**12) 프로젝트에 구성을 Xbox Live를 추가 합니다.**

Xboxservices.config 파일을 추가 합니다.

![](../images/unity/unity-il2cpp-4.png)

라는 문서 페이지에 따라 [신규 또는 기존 UWP 프로젝트에 추가 Xbox Live](get-started-with-visual-studio-and-uwp.md)

> [!NOTE]
> Xboxservices.config 내 모든 값은 대/소문자 구분입니다.

**13) 컴파일 및 Visual Studio에서 UWP 앱 실행**

일반 UWP 앱 같은 앱이 시작 되 고 Xbox Live 호출 함수에 UWP 앱 컨테이너를 필요한 대로 작동 하도록 허용 됩니다.

**14) 다시 Unity의 어떤 데이터에 변경한 경우**  
Unity에서 모든 항목을 변경한 경우 UWP 프로젝트를 다시 만들어야 합니다.

이 문제를 방지 하려면 Unity 프로젝트 내에서 업데이트 해야 하므로 Xbox Live에 로그인 실패, 그러면 컴파일할 때 Unity pfx 파일은 대체는 note 합니다.

이렇게 하려면 파일로 이동 \| 빌드 설정에서 "빌드 설정"을 클릭 합니다 **유니버설 Windows 플랫폼** 플레이어를 위에서 얻은 하나를 사용 하 여 파일 PFX를 바꾸려면 PFX 단추를 클릭 합니다. 또는 Unity 내에서 프로젝트를 다시 작성 될 때마다 PFX 파일을 삭제할 수 있습니다.

## <a name="troubleshooting-common-issues"></a>일반적인 문제 해결

**1)** 경우 Unity에 연결 된 스크립트를 로드할 수 다음 Unity 프로젝트 자산 패널에는 WinMD 끌어 3 단계 했던 것을 확인 하지 수

**2)** 때나이 코드 줄을 실행 하는 동안 앱이 시작 시 즉시 충돌 하는 경우:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Xboxservices.config 텍스트 파일 및 해당 속성, 집합 "콘텐츠", "빌드 동작" 및 "출력 디렉터리로 복사" 집합을 프로젝트에 추가한 "항상 복사"를 확인 합니다.
또한 적절 한 JSON의 서식을 10 진수 형식으로 TitleId와 같은 포함 된 확인 합니다.

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** 앱이 시작 하지만 로그인에 실패 하는 경우 다음을 확인 합니다.

로 설정 된 컴퓨터)에 개발자 샌드박스에 합니다.  Xbox Live SDK의 \Tools 폴더 SwitchSandbox.cmd 스크립트를 사용 하 여이 작업을 수행 합니다.

b) 개발자 샌드박스에 대 한 액세스 권한이 있는 Xbox Live 계정으로 로그인 합니다.  일반 소매점 Xbox Live 계정을 액세스를 권한이 없습니다.  테스트 계정을 만들려면 XDP 또는 파트너 센터를 사용할 수 있습니다.

UWP 앱에서 c) package.appxmanfiest 올바른 Id로 설정 됩니다.  이 작업을 수동으로 편집할 수 있지만이 문제를 해결 하는 가장 쉬운 방법은 Visual Studio에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 "Store"를 선택 하는 것 \| "은 저장소와 앱 연결"입니다.

Unity에서 제공 된 주식.pfx 파일 d) 디스크에서 삭제 하 고, 참조 하는.csproj에 줄을 제거 하거나 올바른 id 없습니다 또는 Visual Studio에서 프로젝트를 클릭 하 고 "Store"를 선택 하는 오른쪽 \| "연결 앱을 스토어 "는에 적절 한.pfx 파일을 배치 합니다.  해야 클릭 한 후 Unity를 돌아가려면 "빌드 설정"에 **유니버설 Windows 플랫폼** player 및 Visual Studio의 "the 저장소와 앱 연결" 작업에서 가져온 하를 사용 하 여.pfx 파일을 바꾸려면 PFX 단추를 클릭 합니다.
