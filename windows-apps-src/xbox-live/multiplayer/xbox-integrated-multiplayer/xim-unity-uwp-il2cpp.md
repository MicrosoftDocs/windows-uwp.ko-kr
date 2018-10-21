---
title: XIM(IL2CPP 포함 Unity) 사용
author: KevinAsgari
description: IL2CPP 스크립팅 백 엔드를 사용 하 여 UWP 용 Unity를 사용 하 여 Xbox 통합 멀티 플레이어 사용
ms.author: kevinasg
ms.date: 04/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, Xbox 통합 멀티 플레이어 하나, Unity, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 4a52501761f75b5d57aa73d919b7c83f3988685c
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5158921"
---
# <a name="use-xim-unity-with-il2cpp"></a>XIM(IL2CPP 포함 Unity) 사용

## <a name="overview"></a>개요

Unity에서 IL2CPP에 대 한 Windows 런타임 지원

Unity 5.6f3 릴리스되면서 엔진에는 새 기능을 직접 게임 프로젝트에 포함 하 여 스크립트에서 직접 Windows 런타임 (WinRT) 구성 요소를 사용 하 여 개발자 포함 되어 합니다. 까지 5.6 개발자는 플러그 인, 또는 dll UWP의 게임 스크립트에서 Xbox 통합 멀티 플레이어 등 모든 플랫폼 기능을 지원 하기 위해 필요 합니다. 이 새 프로젝션 계층 플러그인 요건을 제거 하 고 IL2CPP 스크립팅 백 엔드를 선택 하는 게임에만 지원 새롭고 간소화 된 워크플로 소개 합니다.

- WinRT 및 Unity를 사용 하 여 시작 하는 방법에 대 한 자세한 내용은 [Unity 설명서](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)를 참조 하세요.
- IL2CPP를 사용 하 여 Unity에 Xbox Live 지원 추가 하는 방법에 대 한 자세한 내용은 주제에 [Xbox Live 설명서](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp) 를 참조 하세요.

## <a name="using-the-xim-unity-asset-package"></a>XIM Unity 자산 패키지를 사용 하 여

### <a name="1-install-unity"></a>1. Unity 설치

Unity 5.6 이상을 설치 하 고 확보 하기 위해 **Windows 스토어 Il2CPP 스크립팅 백 엔드** 설치 중에 선택

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2. Visual Studio Tools 버전 3.1 Unity에 대 한 설치 및 위에 IntelliSense에 대 한 지원 WinMDs를 사용 하는 경우

Visual Studio 2015 용 [Visual Studio Marketplace에서](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)찾을 수 있습니다. Visual Studio 2017 용 Visual Studio 2017 설치 관리자 구성 요소를 추가할 수 있습니다.

### <a name="3-open-a-new-or-existing-unity-project"></a>3. 새로운 또는 기존 Unity 프로젝트를 엽니다.

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4. 플랫폼에서에서 전환 하는 유니버설 Windows 플랫폼 Unity 빌드 설정 메뉴

![유니버설 Windows 플랫폼을 사용 하 여 Unity 빌드 설정 메뉴 빌드 설정을 선택](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5. IL2CPP 스크립팅 백 엔드 Unity 플레이어 설정에서 사용 하도록 설정 하 고.NET 4.6 API 호환성 설정

!["Api 호환성" 설정에서 사용 하 여 Unity 플레이어 설정 메뉴의 구성 secion ".NET 4.6"](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6. Xbox 통합 멀티 플레이어 WinRT Unity 자산 패키지의 최신 버전 가져오기

이에서 확인할 수 있습니다.https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7. 이제에서 사용할 수 XIM 스크립트

C#을 사용 하 여 XIM을 사용 하는 방법에 대 한 자세한 지침을 [사용 하 여 XIM (C#)를](using-xim-cs.md)참조 하세요.

다음 코드 조각은 코드를 사용 하 여 XIM 수 통합 하는 방법을 보여 줍니다.

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

대 한 자세한 내용은 합니다 `ENABLE_WINMD_SUPPORT` #define 지시문을 [Windows 런타임](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html) 지원 Unity 설명서를 참조 하세요.

### <a name="8-required-capability-content"></a>8. 필요한 기능 콘텐츠

기본적으로 XIM을 사용 하 여 응용 프로그램에 연결 하 고 모두 인터넷 및 로컬 네트워크를 통해 네트워크 리소스에서 연결을 수락 해야 합니다. 음성 채팅을 지원 하도록 마이크 장치에 액세스를 해야 합니다. 결과적으로, 앱 "InternetClientServer" 및 "PrivateNetworkClientServer" 기능 및 플레이어 설정에서 게시 설정에서 "마이크" 장치 접근 권한 값을 선언 해야 합니다.

![Unity의 기능 메뉴 "InternetClientServer", "PrivateNetworkClientServer"와 "마이크" 기능 선택](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9. Unity에서 프로젝트를 빌드하십시오.

1. 파일을 이동 \ | 빌드 설정, **유니버설 Windows 플랫폼** 을 클릭 하 고, **스위치 플랫폼** 을 클릭 하 고 있는지 확인

2. 현재 장면 빌드를 추가 하려면 "열기 장면 추가"를 클릭 합니다.

3. SDK 콤보 상자에 "유니버설 10" 선택

4. UWP 빌드 형식 콤보 상자에 "D3D" 선택 하지만 원하는 경우에 "XAML" 작동 합니다.

5. Unity UWP 응용 프로그램에서 Unity 게임을 래핑하는 UWP Visual Studio 프로젝트를 생성에 대 한 "빌드"를 클릭 합니다.

    위치에 대 한 메시지가 나타나면 때 많은 새 파일이 생성 됩니다 때문에 혼동을 피하기 위해 새 폴더를 만듭니다. "빌드" 폴더를 호출 하 고 해당 폴더를 선택 하면 것이 좋습니다.

6. XIM의 네트워크 매니페스트의 프로젝트에 추가

    Networkmanifest.xml 파일을 추가 합니다.

    ![Visual Studio의 networkmanifest.xml 속성](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    [XIM 프로젝트 구성](xim-manifest.md) 네트워크 매니페스트 및 해당 콘텐츠에 대 한 자세한 내용은 참조 하세요.

7. 컴파일 및 Visual Studio에서 UWP 앱을 실행 합니다.

이 작업은 일반 UWP 앱과 같은 앱을 실행할와 Xbox Live 호출 함수를 UWP 앱 컨테이너를 필요로 하는 대로 작동 하도록 허용 합니다.

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10. 다시 Unity에서 항목을 변경 하는 경우

Unity에 아무 것도 변경 하면 UWP 프로젝트를 다시 만들어야
