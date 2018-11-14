---
title: .NET 스크립팅 UWP 용 unity
author: KevinAsgari
description: 에 대 한.NET 스크립팅 백 엔드를 사용 하 여 UWP 용 Unity에 Xbox Live 지원 추가 ID@Xbox 관리 파트너 및
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나 Unity
ms.localizationpriority: medium
ms.openlocfilehash: ba2460e64d63892aa1ff02d2178f2632109b3655
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6276086"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-net-scripting-backend-for-idxbox-and-managed-partners"></a>에 대 한.NET 스크립팅 백 엔드를 사용 하 여 UWP 용 Unity에 Xbox Live 지원 추가 ID@Xbox 관리 파트너 및

**Unity를 설치 하는 1)**

Unity 5.3 또는 더 높고는 Unity 하는 동안 설치 프로세스, "Windows 스토어.NET 스크립팅 백 엔드" 구성 요소를 확인 합니다.

![](../images/unity/unity1-install.png)

**2) 신규 또는 기존 Unity 프로젝트를 엽니다.**

2D 또는 3D 프로젝트 수 있습니다. 입력 하거나 Xbox Live SDK와 함께 작동 합니다.

**3)에서 확인할 수 있습니다 Xbox Live WinRT Unity 자산 패키지의 최신 버전 가져오기https://github.com/Microsoft/xbox-live-api/releases**

**4) 추가 하 고 새 C\ # 스크립트 Unity 개체에 연결 합니다.**

예를 들어 "기본 카메라"와 같은 Unity 개체를 클릭 하 고 "구성 요소 추가"를 클릭 합니다. \ | "새 스크립트" \ | C\ # 스크립트 \ | 하 고 "XboxLiveScript" 라는 이름을 지정 합니다. 모든 게임 개체를 수행 합니다.

**5) Unity에서 프로젝트를 빌드하십시오.**

1.  파일을 이동 \ | 빌드 설정, Windows 스토어를 클릭 하 고, "스위치 플랫폼"를 클릭 하 고 있는지 확인

2.  현재 장면 빌드에 추가 하려면 "열기 장면 추가"를 클릭 합니다.

3.  SDK 콤보 상자에서 "유니버설 10"를 선택 합니다.

4.  UWP 빌드 형식 콤보 상자에 "D3D" 선택 하지만 원하는 경우 "XAML"도 작동 합니다.

5.  어셈블리 Csharp.dll 프로젝트를 생성 하려면 "Unity C\ # 프로젝트" 확인란을 클릭

6.  Unity UWP 응용 프로그램에서 Unity 게임을 래핑하는 UWP Visual Studio 프로젝트를 생성에 대 한 "빌드"를 클릭 합니다. 위치에 대 한 메시지가 나타나면 많은 새 파일이 생성 됩니다 때문에 혼동을 피하기 위해 새 폴더를 만듭니다. "빌드" 폴더를 호출 하 고 해당 폴더를 선택 하면 좋습니다.

![](../images/unity/unity3-buildsettings.png)


**6) Visual Studio에서 생성 되는 UWP 프로젝트를 열고 합니다.**

Unity는 탐색기에서 프로젝트 출력 폴더를 열립니다.  .sln 파일을 무시 합니다.  대신 빌드 폴더로 이동 하 고 Visual Studio에서 생성 된.sln을 엽니다.  

이 솔루션의 3 개의 프로젝트를 볼 수 있습니다.

1.  어셈블리 CSharp 합니다. Xbox Live 스크립트 상주입니다.

2.  어셈블리-Csharp firstpass 합니다. 우리 목적에 대해이 프로젝트를 무시할 수 있습니다.

3.  UWP 앱 프로젝트의 이름을 기반으로 합니다. Unity 엔진을 호스트 하는 일반적인 UWP 앱입니다. 이 위치 됩니다 수 설정할 때 일부 Xbox Live 구성과 기존 UWP 앱과 유사 합니다.


**7) UWP 앱에 Xbox Live 구성을 추가 합니다.**

[새로운 또는 기존 UWP 프로젝트에 Xbox Live 추가](get-started-with-visual-studio-and-uwp.md) 라는 doc 페이지를 수행 합니다.

**8) 스크립트에 Xbox Live 코드를 추가 합니다.**

게임 개체에 연결 하는 스크립트에이 예제에서는 Xbox Live 코드를 복사/붙여넣기. 이 스크립트는 "CSharp 어셈블리" 프로젝트에 표시 됩니다. 코드를 원하는 대로 변경할 수 있습니다.

```csharp
#if NETFX_CORE

using UnityEngine;
using System;
using Microsoft.Xbox.Services.System;

public class XboxLiveScript : MonoBehaviour
{
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();
    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
    string debugText = "";

    void Start()
    {
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;

        UIDispatcher = cw.Dispatcher;
        SignIn();
    }

    void Update()
    {
    }

    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }

    async void SignIn()
    {
        SignInResult result = await m_user.SignInAsync(UIDispatcher);

        if (result.Status == SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }
    }
}

#endif
```

**9) 컴파일 및 Visual Studio에서 UWP 앱을 실행 합니다.**

이 작업은 일반 UWP 앱 처럼 앱 시작와 Xbox Live 호출 함수에 UWP 앱 컨테이너를 필요로 하는 대로 작동 하도록 허용 합니다.

**10) 다시 Unity에서 항목을 변경 하는 경우**
  
Unity에 아무 것도 변경 하면 UWP 프로젝트를 다시 만들어야 합니다.

Note Unity는 Unity 프로젝트이 문제를 방지 하기 위해 업데이트 해야 하므로 Xbox Live 로그인에 실패 하 고, 발생 다시 컴파일할 때 pfx 파일을 대체 됩니다.

이렇게 하려면 파일로 이동 \ | 빌드 설정 하 고 Windows 스토어 플레이어의 "빌드 설정" 클릭 위에서 가져온 PFX 파일을 바꾸어야 PFX 단추를 클릭 합니다. Unity에서 프로젝트를 다시 빌드할 때마다 PFX 파일을 삭제할 수도 있습니다.

## <a name="troubleshooting-common-issues"></a>일반적인 문제 해결

**1)** 경우 Unity가 관련된 스크립트를 로드한 다음 Unity 프로젝트 자산 패널에는 WinMD를 끌 수는 3 단계 했던 것을 확인 하지 수

**2)** 이 코드 줄을 실행 하려고 할 때 또는 앱 시작 시 immedately 충돌 하는 경우:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

프로젝트에 및 속성을 설정 "콘텐츠", "빌드 작업" 및 "출력 디렉터리로 복사" 설정에서을 "항상 복사" xboxservices.config 텍스트 파일을 추가 없는지 확인 합니다.

> [!NOTE]
> Xboxservices.config 내에서 모든 값은 대/소문자 구분 합니다.

또한 적절 한 JSON 서식을 10 진수 형식의 TitleId 같은 포함 된 인지 확인 합니다.

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** 앱을 시작 하지만 로그인에 실패 하는 경우 다음을 확인 합니다.

a) 컴퓨터로 설정 되는 개발자 샌드박스 합니다.  Xbox Live SDK의 \Tools 폴더에서 SwitchSandbox.cmd 스크립트를 사용 하 여이 작업을 수행 합니다.

b) 서명 하는 Xbox Live 개발자 샌드박스에 액세스 권한이 있는 계정으로 로그인 합니다.  일반 정품 Xbox Live 계정에 액세스할을 수 없습니다.  XDP 또는 파트너 센터를 사용 하 여 테스트 계정을 만들 수 있습니다.

UWP 앱에서 c) package.appxmanfiest 올바른 Id로 설정 됩니다.  이 작업을 수동으로 편집할 수 있지만이 문제를 해결 하는 가장 쉬운 방법은 Visual Studio에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 "저장"을 선택 하는 \ | "스토어를 사용 하 여 연결 앱"입니다.

d) 주식.pfx 파일 Unity 제공한 디스크에서 삭제 하 고, 참조 하는.csproj 줄을 제거 하거나 올바른 id 없습니다 또는 Visual Studio에서 프로젝트를 클릭 하 고 "저장"을 선택 하는 오른쪽 \ | "연결 앱 스토어를 사용 하 여" 적절 한.pfx 파일 배치 됩니다.  Unity를 다시 클릭 "빌드 설정" Windows 스토어 플레이어에서 하 고 Visual Studio의 "의 저장소와 앱 연결" 작업에서 가져온.pfx 파일로 대체 하려면 PFX 단추 클릭 이동 하는 다음 해야 합니다.
