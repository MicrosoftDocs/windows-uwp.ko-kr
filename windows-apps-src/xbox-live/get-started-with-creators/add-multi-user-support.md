---
title: Unity 게임에 다중 사용자 지원 추가
author: KevinAsgari
description: 다중 사용자 지원의 Xbox Live Unity 플러그 인을 사용 하 여 Unity 게임에 추가
ms.assetid: ''
ms.author: heba
ms.date: 07/14/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity, 다중 사용자
ms.localizationpriority: medium
ms.openlocfilehash: 73dcb6bb177cd8d6c2e2e532e6f1b7a62ade7adc
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6251031"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>Unity 게임에 다중 사용자 지원 추가
> [!IMPORTANT]
> Xbox Live Unity 플러그 인 도전 과제 또는 온라인 멀티 플레이어 지원 하지 않으며만 [Xbox Live 크리에이터 스 프로그램](../developer-program-overview.md) 구성원에 대 한 권장 합니다.

다중 사용자 지원 여러 로컬 플레이어를 포함 하는 시나리오에 대 한 Xbox Live Unity 플러그인에서 지원 됩니다. 각 플레이어 통계 및 로그인 자신의 Xbox Live 계정을 사용할 수 있습니다. 게임 재생을 Xbox Live 계정을 갖고 있지 않은 사용자에 대 한 게스트 사용 하도록 설정할 수 있습니다. 이 기능은 Xbox 콘솔에서 에서만 지원 됩니다.

이 자습서에서는 다른 Xbox Live 프리 팹에 다중 사용자 지원을 추가 하는 방법을 안내 합니다.

> [!IMPORTANT]
> 다중 사용자 시나리오는 Xbox 콘솔에서만 지원 됩니다. PC에서이 기능이 작동 하지 않습니다.

## <a name="prerequisites"></a>사전 요구 사항
[시작](configure-xbox-live-in-unity.md) 자습서 활성화 하 고 Xbox Live 구성 따랐는지 해야 합니다.

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>추가 하 고 여러 Xbox Live 사용자 로그인

1. **자산**에서 > **Xbox Live** > **프리 팹** 폴더 끌어다 장면의 **XboxLiveUser** prefab 만큼 인스턴스 플레이어 됩니다. 이 자습서에서는 있습니다 두 플레이어 하므로 두 **XboxLiveUser** 인스턴스는 장면에 추가 됩니다. 우리 참조 하 **XboxLiveUser** 및 **XboxLiveUser2** 편의 위해 합니다.

2. 제대로 사용자와 기호를 추가 하려면 각 **XboxLiveUser**에 대 한 장면에 **사용자 프로필** prefab의 두 인스턴스 추가 합니다. 인스턴스 있는지 확인 합니다 `XboxLiveServices` 장면에서 prefab 합니다. 또한 해야 알려 장면에서 떨어져 두 **사용자 프로필** 개체를 이동 분리 합니다. 이러한 프리 팹 Unity 이벤트 시스템을 사용 하는지 확인 하세요 인스턴스 있다고는 `EventSystem` 장면에 있습니다.

    ![Xbox의 다중 사용자 지원의 계층 Live Unity 플러그 인 자습서 프로젝트](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![Xbox Live Unity 플러그 인 자습서 프로젝트의 다중 사용자 지원의 게임 장면](../images/unity/MUA-Tutorial-GameScene.png)

3. **사용자 프로필** 개체 각각 장면에 있는 **XboxLiveUser** 프리 팹의 한 인스턴스만 할당 합니다.

    ![다중 사용자 지원에 대 한 사용자 프로필 프리 팹](../images/unity/user-profile-for-mua.png)

4. 다중 사용자 응용 프로그램은 Xbox One 장치 에서만 지원, **사용자 프로필** 에 컨트롤러 지원을 추가 개체 이므로 필요 합니다. 각 **사용자 프로필** 개체에는 호출 필드 `InputControllerButton` 를 수신 대기할 단추에는 각 **사용자 프로필** 숫자 및 위치 조이스틱 지정할 수 있습니다.

이 자습서에서는 `joystick 1 button 0` 플레이어 1에 할당 된 **사용자 프로필** 에 대 한 및 `joystick 2 button 0` 플레이어 2와 두 번째 **사용자 프로필** 게임 개체입니다. 할당이 `A` 두 컨트롤러에 대 한 **사용자 프로필** 을 사용 하 여 상호 작용 하기 위한 단추입니다.

> [!Note]
> Xbox Live Unity 플러그인에서 Xbox 컨트롤러 지원에 대 한 자세한 내용은 확인 하세요: [Xbox Live 프리 팹에 컨트롤러 지원 추가](add-controller-support-to-xbox-live-prefabs.md)

5. 편집기에서 장면을 실행 하 고 각 단추 하는지 확인 하기 위해를 실행 하는 적중 횟수를 올바르게 구성 되어 있습니다.

    ![다중 사용자 지원 Unity 편집기에서 테스트](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>빌드 및 UWP 테스트

1. [Unity로 크리에이터 스 타이틀 개발](configure-xbox-live-in-unity.md) 자습서에 설명 된 단계를 수행한 후 Visual Studio에서 내보낸된 UWP 솔루션을 엽니다.

2. 게임의 UWP 프로젝트에서 마우스 오른쪽 단추로 클릭은 `package.appxmanifest.xml` 파일 및 **코드 보기를**선택 합니다.

3. 아래는 `<Properties></Properties>` 섹션에서 앱에 대 한 다중 사용자 입력을 수 있도록 다음 코드를 추가 합니다.`<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. Xbox를 테스트 하려면 [Xbox 개발 환경에서의 UWP 설정](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup) 자습서에서 설명서를 따릅니다.

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>여러 사용자와는 다른 Xbox Live 프리 팹을 사용 하 여

플러그인의 **예제** 폴더에서 다른 프리 팹 **XboxLiveUser** 인스턴스를 사용 하 여 각 사용자에 대 한 특정 정보를 표시 하는 방법을 보여 주는 **MultiUserExample** 장면이 있습니다.
