---
title: Unity 게임에 다중 사용자 지원 추가
description: Xbox Live Unity 플러그 인을 사용 하 여 Unity 게임에 다중 사용자 지원 추가
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity, 다중 사용자
ms.localizationpriority: medium
ms.openlocfilehash: 483a0e966be756de483bf7e2ab8458647397687b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613738"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>Unity 게임에 다중 사용자 지원 추가
> [!IMPORTANT]
> Xbox Live Unity 플러그 인 성과 또는 멀티 플레이어 온라인 게임을 지원 하지 않으며에 권장 됩니다 [Xbox Live 크리에이터 스 프로그램](../developer-program-overview.md) 멤버입니다.

이제은 Xbox Live Unity 플러그 인을 통해 여러 로컬 플레이어를 포함 하는 시나리오에 대 한 다중 사용자를 지원 합니다. 각 플레이어 통계 및 로그인에 자체 Xbox Live 계정을 사용할 수 있습니다. 게임에 Xbox Live 계정을 사용 하 여 재생에 없는 사용자에 대 한 게스트를 사용할 수도 있습니다. 이 기능은 Xbox 콘솔 에서만 지원 됩니다.

이 자습서에서는 다중 사용자 지원 다른 Xbox Live Prefabs 추가 하는 방법을 안내 합니다.

> [!IMPORTANT]
> 다중 사용자 시나리오는 Xbox 콘솔 에서만 지원 됩니다. PC에이 기능이 작동 하지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소
수행 해야 합니다 [Getting Started](configure-xbox-live-in-unity.md) Xbox Live를 구성 하는 자습서입니다.

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>추가 하 고 여러 Xbox Live 사용자 로그인

1. **자산** > **Xbox Live** > **Prefabs** 폴더를 만큼 장면으로 끌어서 **XboxLiveUser** prefab 인스턴스 만큼 플레이어 됩니다. 이 자습서에서는 있을 것 이므로 두 두 플레이어가 **XboxLiveUser** 인스턴스 장면에 추가 됩니다. 이라고 부르는 **XboxLiveUser** 하 고 **XboxLiveUser2** 편의 위해.

2. 사용자를 추가 하 고 로그인에 제대로, 추가의 두 인스턴스를 **UserProfile** 마다 하나씩 장면 prefab **XboxLiveUser**합니다. 인스턴스에 있는지 확인 합니다 `XboxLiveServices` 장면에서 prefab 합니다. 또한 두 이동 해야 **UserProfile** 어렵지요 장면에서 분리 하는 개체입니다. 이러한 prefabs Unity Eventsystem를 사용 하므로 했는지 인스턴스에 있다고는 `EventSystem` 가 등장 합니다.

    ![Xbox에서 다중 사용자 지원 계층 Live Unity 플러그 인 자습서 프로젝트](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![Xbox Live Unity 플러그 인 Tutorial 프로젝트의 다중 사용자 지원 게임 장면](../images/unity/MUA-Tutorial-GameScene.png)

3. 인스턴스 하나를 할당 합니다 **XboxLiveUser** prefabs 각 장면에 있는 합니다 **UserProfile** 개체입니다.

    ![다중 사용자 지원에 대 한 사용자 프로필 prefab](../images/unity/user-profile-for-mua.png)

4. 다중 사용자 응용 프로그램은 Xbox One 장치 에서만 지원, 때문에 컨트롤러 지원을 추가 합니다 **UserProfile** 개체가 필요 합니다. 각 **UserProfile** 개체를 호출 하는 필드가 `InputControllerButton` 여기서 조이스틱을 지정할 수 있습니다 하 고 각 단추 번호 **UserProfile** 수신 대기 해야 합니다.

이 자습서에서는 `joystick 1 button 0` 에 대 한 합니다 **UserProfile** 플레이어 1에 할당 되는 및 `joystick 2 button 0` 플레이어 2와 두 번째 **UserProfile** 게임 개체입니다. 할당 합니다 `A` 상호 작용 하기 위한 단추 **UserProfile** 두 컨트롤러에 대 한 합니다.

> [!Note]
> Xbox Live Unity 플러그인에서 Xbox 컨트롤러 지원에 대 한 자세한 내용은를 참조 하세요. [Xbox Live Prefabs에 컨트롤러 지원 추가](add-controller-support-to-xbox-live-prefabs.md)

5. 편집기에서 장면을 실행 하 고 각 단추 하는지 확인 하기 위해를 실행 하는 적중 올바르게 구성 되어 있습니다.

    ![다중 사용자 지원 Unity 편집기에서 테스트](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>빌드 및 UWP 테스트

1. 에 설명 된 단계를 수행 하는 후 합니다 [Unity 사용한 개발 작성자 제목](configure-xbox-live-in-unity.md) 자습서에서는 Visual Studio에서 내보낸된 UWP 솔루션을 엽니다.

2. 게임의 UWP 프로젝트에서 마우스 오른쪽 단추로 클릭 합니다 `package.appxmanifest.xml` 파일을 선택 **코드 보기**합니다.

3. 아래는 `<Properties></Properties>` 섹션에서 앱에 대 한 다중 사용자 입력을 사용 하도록 설정 하는 다음을 추가 합니다. `<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. Xbox를 테스트 하려면 설명서에 따라 합니다 [Xbox 개발 환경에서 프로그램 UWP 설정](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup) 자습서입니다.

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>여러 사용자를 다른 Xbox Live Prefabs 사용

에 **예제** 폴더의 플러그 인, 방법이 **MultiUserExample** 다른 prefabs 사용 하는 방법을 보여 주는 장면을 **XboxLiveUser** 특정 표시할 인스턴스 각 사용자에 대 한 정보입니다.
