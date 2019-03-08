---
title: XIM(IL2CPP 포함 Unity) 사용
description: 사용 하 여 Xbox 통합 멀티 플레이 게임 Unity를 사용 하 여 UWP 용 IL2CPP 스크립팅 백 엔드를 사용 하 여
ms.date: 04/03/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, Xbox 통합 멀티 플레이 게임 하나, Unity, xbox
ms.localizationpriority: medium
ms.openlocfilehash: a600fd253efae1daca34241b105a69514561e01d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646448"
---
# <a name="use-xim-unity-with-il2cpp"></a>XIM(IL2CPP 포함 Unity) 사용

## <a name="overview"></a>개요

Unity에서 IL2CPP에 대 한 Windows 런타임 지원

Unity 5.6f3 릴리스의 엔진이 개발자가 스크립트에서 직접 (WinRT) Windows 런타임 구성 요소를 사용 하 여 직접 게임 프로젝트에 포함 하 여 새로운 기능을 포함 합니다. 까지 플러그 인 또는 UWP의 게임 스크립트에서 통합 멀티 플레이어 Xbox 등 모든 플랫폼 기능을 지원 하기 위해 dll 5.6 개발자가 필요 합니다. 이 새 프로젝션 계층 요구 사항에 플러그 인을 제거 하 고 스크립팅 IL2CPP 백 엔드를 선택 하는 게임 에서만 지원 새롭고 간소화 된 워크플로 소개 합니다.

- WinRT 및 Unity를 사용 하 여 시작 하는 방법에 대 한 자세한 내용은 참조는 [Unity 설명서](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)합니다.
- IL2CPP를 사용 하 여 Unity에 Xbox Live 지원을 추가 하는 방법에 대 한 자세한 내용은 참조 하세요. 합니다 [Xbox Live 설명서](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp) 주제입니다.

## <a name="using-the-xim-unity-asset-package"></a>XIM Unity 자산 패키지를 사용 하 여

### <a name="1-install-unity"></a>1. Unity를 설치 합니다.

Unity 5.6 이상 설치 하 고 있는지 확인 합니다 **백 엔드 스크립팅 하는 Windows 스토어 Il2CPP** 설치 중에 선택

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2. Visual Studio Tools for Unity 버전 3.1 설치 하 고 위의 IntelliSense에 대 한 지원 Winmd를 사용 하는 경우

Visual Studio 2015를 찾을 수 있습니다 [Visual Studio Marketplace에서](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)합니다. Visual Studio 2017 용 Visual Studio 2017 설치 관리자 내에서 구성 요소를 추가할 수 있습니다.

### <a name="3-open-a-new-or-existing-unity-project"></a>3. 기존 또는 새 Unity 프로젝트를 열으십시오

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4. Unity 빌드 설정 메뉴에서 유니버설 Windows 플랫폼으로 플랫폼 전환

![Unity 빌드 설정을 메뉴 유니버설 Windows 플랫폼 빌드 설정 선택](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5. Unity 플레이어 설정 IL2CPP 스크립팅 백 엔드를 사용 하도록 설정 하 고.NET 4.6으로 API 호환성 설정

![구성 섹션 설정 "Api 호환성" 설정 사용 하 여 Unity 플레이어 설정 메뉴의 ".NET 4.6"](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6. 다중 접속 WinRT Unity를 통합 하는 Xbox 자산 패키지의 최신 버전 가져오기

찾을 수 있습니다. https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7. 스크립트에서 XIM를 이제 사용할 수 있습니다.

사용 하 여 XIM를 사용 하는 방법에 대 한 자세한 지침에 대 한 C#를 참조 하세요 [XIM 사용 (C#)](using-xim-cs.md)합니다.

다음 코드 조각은 코드를 사용 하 여 XIM 있습니다 통합 하는 방법을 보여 줍니다.

```cs

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

#if ENABLE_WINMD_SUPPORT
using Microsoft.Xbox.Services.XboxIntegratedMultiplayer;
#endif

public class XimScript
{
    public void Start()
    {
#if ENABLE_WINMD_SUPPORT
        XboxIntegratedMultiplayer.TitleId = XboxLiveAppConfiguration.SingletonInstance.TitleId;
        XboxIntegratedMultiplayer.ServiceConfigurationId = XboxLiveAppConfiguration.SingletonInstance.ServiceConfigurationId;
#endif
    }

    public void Update()
    {
#if ENABLE_WINMD_SUPPORT
        using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
        {
            foreach (var stateChange in stateChanges)
            {
                switch (stateChange.Type)
                {
                    case XimStateChangeType.PlayerJoined:
                        HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                        break;

                    case XimStateChangeType.PlayerLeft:
                        HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                        break;
                    [...]
                }
            }
        }
#endif
    }
}
```

에 대 한 자세한 합니다 `ENABLE_WINMD_SUPPORT` #define 지시문에서 Unity 설명서를 참조 하십시오 [Windows 런타임 지원](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)

### <a name="8-required-capability-content"></a>8. 필요한 기능이 콘텐츠

XIM를 기본적으로 사용 하 여 응용 프로그램에 연결 하 고 네트워크 리소스 모두를 통해 인터넷 및 로컬 네트워크에서에서 연결을 허용 해야 합니다. 음성 채팅 지원 하려면 마이크 장치에 대 한 액세스도 필요 합니다. 결과적으로, 앱은 "InternetClientServer" 및 "PrivateNetworkClientServer" 기능 및 플레이어 설정에서 게시 설정에서 "마이크" 장치 기능을 선언 해야 합니다.

!["InternetClientServer", "PrivateNetworkClientServer" 및 선택한 "마이크" 기능을 사용 하 여 unity의 기능 메뉴](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9. Unity에서 프로젝트를 빌드하십시오.

1. 파일로 이동 \| 빌드 설정 클릭 **유니버설 Windows 플랫폼** 클릭 하면 확인 **플랫폼 전환**

2. 현재 장면 빌드에 추가할 "Open 장면 추가"를 클릭 합니다.

3. SDK 콤보 상자에 "유니버설 10" 선택

4. UWP 빌드 유형 콤보 상자에서 "D3D"를 선택 하지만 원하는 경우에 "XAML" 작동 합니다.

5. UWP 응용 프로그램에서 Unity 게임을 래핑하는 UWP Visual Studio 프로젝트를 생성 하려면 Unity에 대 한 "빌드"를 클릭 합니다.

    위치에 대 한 메시지가 나타나면 경우 혼동을 피하기 위해 많은 새 파일이 만들어질 수 있으므로 새 폴더를 만듭니다. "빌드" 폴더를 호출 하 고 해당 폴더를 선택한 것이 좋습니다.

6. 프로젝트에 XIM의 네트워크 매니페스트 추가

    Networkmanifest.xml 파일을 추가 합니다.

    ![Visual Studio의 networkmanifest.xml 속성](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    참조 [XIM 프로젝트 구성](xim-manifest.md) 네트워크 매니페스트 및 해당 콘텐츠에 대 한 자세한 내용은 합니다.

7. 컴파일 및 Visual Studio에서 UWP 앱 실행

일반 UWP 앱 같은 앱이 시작 되 고 Xbox Live 호출 함수에 UWP 앱 컨테이너를 필요한 대로 작동 하도록 허용 됩니다.

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10. Unity의 값으로 변경 하는 경우 다시 작성

Unity에서 모든 항목을 변경한 경우 UWP 프로젝트를 다시 만들어야
