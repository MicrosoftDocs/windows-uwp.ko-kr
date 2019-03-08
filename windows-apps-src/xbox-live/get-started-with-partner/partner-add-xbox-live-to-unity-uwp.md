---
title: .NET 스크립트와 UWP 용 unity
description: Xbox Live 지원에 대 한.NET 스크립팅 백 엔드를 사용 하 여 UWP에 대 한 Unity에 추가 ID@Xbox 및 파트너 관리
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, Unity
ms.localizationpriority: medium
ms.openlocfilehash: 8c4ca9d58f89e215563adcc7985b978641efdf07
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594088"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-net-scripting-backend-for-idxbox-and-managed-partners"></a>Xbox Live 지원에 대 한.NET 스크립팅 백 엔드를 사용 하 여 UWP에 대 한 Unity에 추가 ID@Xbox 및 파트너 관리

**1)-Unity를 설치 하는 중**

Unity 5.3 설치 또는 이상을 사용 하 고 Unity 하는 동안 설치 프로세스, "Windows 스토어 Scripting.NET 백 엔드" 구성 요소를 확인 합니다.

![](../images/unity/unity1-install.png)

**2) 새 또는 기존 Unity 프로젝트를 열려면**

2D 또는 3D 프로젝트를 수 있습니다. 유형 중 하나는 Xbox Live SDK를 사용 하 여 작동 합니다.

**3)에서 찾을 수 있습니다이 Xbox Live WinRT Unity 자산 패키지의 최신 버전 가져오기 https://github.com/Microsoft/xbox-live-api/releases**

**4) 추가 하 고 새 C 연결\# Unity 개체에는 스크립트입니다.**

예를 들어 "주 카메라", 예: Unity 개체를 클릭 하 고 "추가 구성 요소"를 클릭 \| "새 스크립트" \| C\# 스크립트 \| "XboxLiveScript" 이라는 이름을 지정 합니다. 모든 게임 개체를 수행 합니다.

**5)에서 Unity 프로젝트를 빌드하십시오.**

1.  파일로 이동 \| Windows 스토어를 클릭 하 고 "플랫폼 전환"을 클릭 해야 하는 빌드 설정

2.  현재 장면 빌드에 추가할 "Open 장면 추가"를 클릭 합니다.

3.  SDK 콤보 상자에 "유니버설 10" 선택

4.  UWP 빌드 유형 콤보 상자에서 "D3D"를 선택 하지만 원하는 경우에 "XAML" 작동 합니다.

5.  클릭을 "C Unity\# 프로젝트" Csharp.dll 어셈블리 프로젝트를 생성 하려면이 확인란을

6.  UWP 응용 프로그램에서 Unity 게임을 래핑하는 UWP Visual Studio 프로젝트를 생성 하려면 Unity에 대 한 "빌드"를 클릭 합니다. 위치에 대 한 메시지가 나타나면 경우 혼동을 피하기 위해 많은 새 파일이 만들어질 수 있으므로 새 폴더를 만듭니다. "빌드" 폴더를 호출 하 고 해당 폴더를 선택한 것이 좋습니다.

![](../images/unity/unity3-buildsettings.png)


**6) Visual Studio에서 생성 되는 UWP 프로젝트를 열으십시오**

Unity는 탐색기에서 프로젝트 출력 폴더를 엽니다.  에.sln 파일을 무시 합니다.  대신 빌드 폴더에 이동 하 고 Visual Studio에서 생성 된.sln을 엽니다.  

이 솔루션의 3 프로젝트를 볼 수 있습니다.

1.  어셈블리-CSharp입니다. 이 Xbox Live 스크립트는 어디에

2.  어셈블리-Csharp-firstpass 합니다. 이 프로젝트에 따르면 무시할 수 있습니다.

3.  UWP 앱 프로젝트의 이름을 기반으로 합니다. Unity 엔진을 호스팅하는 기존 UWP 앱입니다. 여기서 설정 일부 Xbox Live 구성을 기존 UWP 앱에 유사한입니다.


**7) UWP 앱에 구성 Xbox Live를 추가 합니다.**

라는 문서 페이지에 따라 [신규 또는 기존 UWP 프로젝트에 추가 Xbox Live](get-started-with-visual-studio-and-uwp.md)

**8) 스크립트에 Xbox Live 코드를 추가 합니다.**

게임 개체에 연결 하는 스크립트에이 예제에서는 Xbox Live 코드를 복사/붙여넣습니다. 이 스크립트는 "어셈블리 CSharp" 프로젝트에 표시 됩니다. 필요에 따라 코드를 변경할 수 있습니다.

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

**9) 컴파일 및 Visual Studio에서 UWP 앱 실행**

일반 UWP 앱 같은 앱이 시작 되 고 Xbox Live 호출 함수에 UWP 앱 컨테이너를 필요한 대로 작동 하도록 허용 됩니다.

**10) 다시 Unity의 어떤 데이터에 변경한 경우**  
Unity에서 모든 항목을 변경한 경우 UWP 프로젝트를 다시 만들어야 합니다.

이 문제를 방지 하려면 Unity 프로젝트 내에서 업데이트 해야 하므로 Xbox Live에 로그인 실패, 그러면 컴파일할 때 Unity pfx 파일은 대체는 note 합니다.

이렇게 하려면 파일로 이동 \| 빌드 설정에서 Windows 스토어 플레이어에서 "빌드 설정"을 클릭 하 고 위에서 얻은 하나를 사용 하 여 PFX 파일을 바꾸려면 PFX 단추를 클릭 합니다. 또는 Unity 내에서 프로젝트를 다시 작성 될 때마다 PFX 파일을 삭제할 수 있습니다.

## <a name="troubleshooting-common-issues"></a>일반적인 문제 해결

**1)** 경우 Unity에 연결 된 스크립트를 로드할 수 다음 Unity 프로젝트 자산 패널에는 WinMD 끌어 3 단계 했던 것을 확인 하지 수

**2)** 때나이 코드 줄을 실행 하는 동안 앱이 시작 시 있었거나 충돌 하는 경우:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Xboxservices.config 텍스트 파일 및 해당 속성, 집합 "콘텐츠", "빌드 동작" 및 "출력 디렉터리로 복사" 집합을 프로젝트에 추가한 "항상 복사"를 확인 합니다.

> [!NOTE]
> Xboxservices.config 내 모든 값은 대/소문자 구분입니다.

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

Unity에서 제공 된 주식.pfx 파일 d) 디스크에서 삭제 하 고, 참조 하는.csproj에 줄을 제거 하거나 올바른 id 없습니다 또는 Visual Studio에서 프로젝트를 클릭 하 고 "Store"를 선택 하는 오른쪽 \| "연결 앱을 스토어 "는에 적절 한.pfx 파일을 배치 합니다.  그런 다음 이동할 Unity를 다시 클릭 "빌드 설정"에서 Windows 스토어 플레이어 하 고 Visual Studio의 "the 저장소와 앱 연결" 작업에서 가져온 것을 사용 하 여.pfx 파일을 바꾸려면 PFX 단추를 클릭 해야 합니다.
