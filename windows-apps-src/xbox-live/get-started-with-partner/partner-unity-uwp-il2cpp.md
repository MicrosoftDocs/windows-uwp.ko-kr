---
title: IL2CPP 백엔드 포함 UWP용 Unity
description: 에 대 한 IL2CPP 스크립팅 백 엔드를 사용 하 여 UWP 용 Unity에 Xbox Live 지원 추가 ID@Xbox 관리 파트너 및
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나 Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2018
ms.locfileid: "8328014"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>에 대 한 IL2CPP 스크립팅 백 엔드를 사용 하 여 UWP 용 Unity에 Xbox Live 지원 추가 ID@Xbox 관리 파트너 및

## <a name="overview"></a>개요

Unity에서 IL2CPP에 대 한 Windows 런타임 지원

Unity 5.6f3 릴리스되면서 엔진에 스크립트에서 직접 Windows 런타임 (WinRT) 구성 요소를 사용 하 여 직접 게임 프로젝트에 포함 하 여 개발자 수 있도록 하는 새로운 기능 포함 되어 있습니다. 까지 5.6 개발자 플러그 인, 또는 dll UWP의 게임 스크립트에서 Xbox Live SDK 등 모든 플랫폼 기능을 지원 하기 위해 필요 합니다. 이 새 프로젝션 계층 플러그인 요건을 제거 하 고 IL2CPP 스크립팅 백 엔드를 선택 하는 게임에만 지원 새롭고 간소화 된 워크플로 소개 합니다.

시작 하는 방법에 대 한 자세한 내용은 Unity 설명서를 참조 하세요.https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>단계

**Unity를 설치 하는 1)**

Unity 5.6 이상을 설치 하 고 있다면, **Windows 스토어 Il2CPP 스크립팅 백 엔드** 설치 중에 선택

**2) 설치 Visual Studio Tools 버전 3.1 Unity 이상 WinMDs를 사용 하는 경우 IntelliSense 지원에 대 한** Visual Studio 2015에 대 한이에서 확인할 수 있습니다 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Visual Studio 2017 용 Visual Studio 2017 설치 관리자 구성 요소를 추가할 수 있습니다.

**3) 새로운 또는 기존 Unity 프로젝트를 엽니다.**

**4) 스위치 Unity 빌드 설정 메뉴에서 유니버설 Windows 플랫폼으로 플랫폼**

**Unity 플레이어 설정에서 IL2CPP 스크립팅 백 엔드를 사용 하는 5) 및.NET 4.6 API 호환성 설정**

![](../images/unity/unity-il2cpp-1.png)

**6) 최신 버전의 Xbox Live WinRT Unity 자산 패키지를 가져올** 에서 확인할 수 있습니다https://github.com/Microsoft/xbox-live-api/releases

**7) 추가 하 고 Unity 개체에 새 C\ # 스크립트를 연결 합니다.**

예를 들어 "기본 카메라" 같은 Unity 개체를 클릭 하 고 "구성 요소 추가"를 클릭 합니다. \ | "새 스크립트" \ | C\ # 스크립트 \ | 하 고 "XboxLiveScript" 라는 이름을 지정 합니다. 모든 게임 개체를 수행 합니다.

**8) (VSTU 3.1 + 설치)와 Visual Studio에서 스크립트를 열어 합니다.**

게임 스크립트 XboxLiveTest.cs VSTU에서 생성 된 "플레이어" 프로젝트에서 열고 두 프로젝트를 알 수 있습니다.

![](../images/unity/unity-il2cpp-2.png)

UWP 용 생성 하는 특별 한 프로젝트 이며 자산에 배치한 winmd 파일에 대 한 참조를 포함 합니다.
"#If ENABLE_WINMD_SUPPORT"를 정의 하는 또한 되므로 IntelliSense 및 구문을 강조 제대로 작동을 정의 합니다.

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

**플레이어 설정에서 게시 설정에서 선택한 'InternetClient' 기능이 있는 10) 확인**

![](../images/unity/unity-il2cpp-3.png)

**11) Unity에서 프로젝트를 빌드하십시오.**

1.  파일을 이동 \ | **유니버설 Windows 플랫폼** 빌드 설정 누르고 **스위치 플랫폼** 을 클릭 하 고 있는지 확인

2.  현재 장면 빌드에 추가 하려면 "열기 장면 추가"를 클릭 합니다.

3.  SDK 콤보 상자에 "유니버설 10"를 선택 합니다.

4.  UWP 빌드 형식 콤보 상자에 "D3D" 선택 하지만 원하는 경우 "XAML"도 작동 합니다.

5.  Unity UWP 응용 프로그램에서 Unity 게임을 래핑하는 UWP Visual Studio 프로젝트를 생성에 대 한 "빌드"를 클릭 합니다. 위치에 대 한 메시지가 나타나면 때 많은 새 파일이 생성 됩니다 때문에 혼동을 피하기 위해 새 폴더를 만듭니다. "빌드" 폴더를 호출 하 고 해당 폴더를 선택 하면 좋습니다.

**12) 프로젝트에 Xbox Live 구성을 추가 합니다.**

Xboxservices.config 파일을 추가 합니다.

![](../images/unity/unity-il2cpp-4.png)

[새로운 또는 기존 UWP 프로젝트에 Xbox Live 추가](get-started-with-visual-studio-and-uwp.md) 라는 문서 페이지를 수행 합니다.

> [!NOTE]
> Xboxservices.config 내 모든 값은 대/소문자 구분 합니다.

**13) 컴파일 및 Visual Studio에서 UWP 앱을 실행 합니다.**

이 작업은 일반 UWP 앱 처럼 앱을 시작와 Xbox Live 호출 함수는 UWP 앱 컨테이너를 필요로 하는 대로 작동 하도록 허용 합니다.

**14) 다시 Unity에서 항목을 변경 하는 경우**
  
Unity에 아무 것도 변경 하면 UWP 프로젝트를 다시 만들어야 합니다.

Note Unity는 Unity 프로젝트이 문제를 방지 하기 위해 업데이트 해야 하므로 Xbox Live에 로그인에 실패 하 고, 발생 다시 컴파일할 때 pfx 파일을 대체 됩니다.

이렇게 하려면 파일로 이동 \ | 빌드 설정 하 고 **유니버설 Windows 플랫폼** 플레이어의 "빌드 설정" 클릭 위쪽에서 가져온 PFX 파일을 바꾸어야 PFX 단추를 클릭 합니다. Unity에서 프로젝트를 다시 빌드할 때마다 PFX 파일을 삭제할 수도 있습니다.

## <a name="troubleshooting-common-issues"></a>일반적인 문제 해결

**1)** 경우 Unity에 연결된 하는 스크립트, 다음 Unity 프로젝트 자산 패널에는 WinMD 창을 끌어 수는 3 단계 했던 것을 확인 하지 수

**2)** 이 코드 줄을 실행 하려고 할 때 또는 앱 시작 시 즉시 충돌 하는 경우:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Xboxservices.config 텍스트 파일을 프로젝트에 및 추가한 속성, 설정 "콘텐츠", "빌드 작업" 및 "출력 디렉터리로 복사" 설정 "항상 복사"를 확인 합니다.
또한 적절 한 JSON 서식을 10 진수 형식의 TitleId 같은 포함 된 인지 확인 합니다.

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** 앱을 시작 하지만 로그인에 실패 하는 경우 다음을 확인 합니다.

a) 컴퓨터로 설정 되 고 개발자 샌드박스 합니다.  Xbox Live SDK의 \Tools 폴더에 SwitchSandbox.cmd 스크립트를 사용 하 여이 작업을 수행 합니다.

b) 서명 하는 Xbox Live 개발자 샌드박스에 액세스 권한이 있는 계정으로 로그인 합니다.  일반 정품 Xbox Live 계정에 액세스할을 수 없습니다.  XDP 또는 파트너 센터를 사용 하 여 테스트 계정을 만들 수 있습니다.

UWP 앱에서 package.appxmanfiest c)에 올바른 Id로 설정 됩니다.  이 작업을 수동으로 편집할 수 있지만이 문제를 해결 하는 가장 쉬운 방법은 Visual Studio에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 "저장"을 선택 하는 \ | "스토어를 사용 하 여 연결 앱"입니다.

d) 주식.pfx 파일 Unity 제공한 디스크에서 삭제 하 고, 참조 하는.csproj에서 줄을 제거 하거나 올바른 id 없습니다 또는 Visual Studio에서 프로젝트를 클릭 하 고 "저장"을 선택 하는 오른쪽 \ | "연결 앱 스토어를 사용 하 여" 적절 한.pfx 파일 배치 됩니다.  그런 다음 이동 해야 Unity에 다시 클릭 "빌드 설정"에서 **유니버설 Windows 플랫폼** 플레이어 하 고 Visual Studio의 "연결 앱 사용 하 여 스토어에 있음" 작업에서 가져온 것으로.pfx 파일을 대체 하려면 PFX 단추를 클릭 합니다.
