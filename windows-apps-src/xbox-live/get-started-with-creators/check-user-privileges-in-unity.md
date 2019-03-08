---
title: Unity의 사용자 권한 설정 확인
description: 로그인에 대 한 권한 설정을 확인 하는 방법을 알아봅니다 Xbox Live 계정에 있습니다.
ms.assetid: ''
ms.date: 10/26/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 계정, 테스트 계정, 자녀 보호, 사용자 권한, 업셀 적용 금지
ms.openlocfilehash: b55ebf9b53cadf2e57317347adce19c3578f9d56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620408"
---
# <a name="check-user-privilege-settings-in-unity"></a>Unity의 사용자 권한 설정 확인
Xbox Live에서 모든 인증 된 사용자의 계정 권한 연결 되어 있습니다. 권한은 Xbox live 기능 사용자가 액세스할 수 지정 시점으로 제어 합니다. 이러한 권한 중 일부는 시스템 제어 방식의 기능에 대 한 다른 사용자가 특정 게임 또는 확장 구독을 연결할 수 있습니다. 또한 자녀 및 Xbox Live 적용 팀에서 발급 한 차단을 사용자의 권한을 제한할 수 있습니다. 이러한 권한 또는 스트리밍 비디오 채팅, 멀티 플레이, 액세스 사용자 생성 콘텐츠를 포함 하 여 일반적인 시나리오의 수를 설명 합니다. 게임이이 정보를 사용 하 여 액세스 제어 및 개인 설정 결정을 내릴 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소
사용자 권한 설정을 확인 하기 위해 있어야 Xbox Live를 사용한 인증을 위해 게임을 구성 하 고 사용자에 성공적으로 로그인 합니다.

>[!IMPORTANT]
> Unity 편집기에서 게임을 테스트 하는 경우 게임에 Xbox Live 서비스에 연결 되지 않은 및 모조 데이터를 사용 하 여 연결을 시뮬레이션 하는 합니다. 이 인해 null 값을 권한에 대 한 확인 합니다. 실제 데이터를 테스트 하려면 Unity 게임의 유니버설 Windows 플랫폼 빌드를 수행 하 고 Visual Studio에서 생성된 된 프로젝트 파일을 엽니다.

다음 문서를 사용할 수 있는 단계에 설명 합니다.

* [(빌드 및 테스트 로그인) 하는 Unity에서 Xbox Live에 로그인](unity-prefabs-and-sign-in.md#build-and-test-sign-in)
* [Visual Studio에서 Unity 게임 빌드를 테스트 합니다.](test-visual-studio-build.md)

## <a name="determine-privileges"></a>권한 확인
인증 시에 받은 토큰을 사용자의 권한은 전달 됩니다. Unity에는 사용자가 권한 목록을 액세스할 수 있습니다는 `XboxLiveUser` 사용자가 성공적으로 로그인 한 후 클래스입니다. 권한 공백으로 구분 된 단일 문자열로 저장 됩니다. 예를 들어, 다음 권한이 있는 사용자를 표시 될 수 있습니다.

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

특정 사용 권한에 대해 확인 하려는 경우 있는지 확인할 수 있습니다는 `Privileges` 속성 관련된 코드를 포함 합니다.

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>권한 코드
다음은 반환 될 수 있는 가능한 권한 코드의 목록입니다.

| 코드  | 권한  | 설명   |
|------ |-----------------------------  |-------------------    |
| 190   | 브로드캐스트             | 라이브 게임 플레이 브로드캐스트할 수 있습니다.     |
| 197   | view_friends_list     | 다른 사용자의 친구 목록을 볼 수 있습니다.   |
| 198   | game_dvr              | 업로드 클라우드 게임 내 비디오를 기록할 수 있습니다.      |
| 199   | share_kinect_content          | Kinect 기록 된 콘텐츠를 클라우드에 업로드 하 고 사용자에 대 한 모든 사용자가 액세스할 수 있게 합니다. |
| 203   | multiplayer_parties           | 파티 세션에 참가할 수 있습니다.     |
| 205   | communication_voice_ingame    | 파티 및 멀티 플레이 게임 세션 동안 음성 채팅에 참여할 수 있습니다.    |
| 206   | communication_voice_skype     | Xbox One에서 Skype를 사용 하 여 음성 통신을 사용할 수 있습니다.   |
| 207   | cloud_gaming_manage_session   | 할당 하 고 호스팅된 게임 세션에 대 한 클라우드 계산 클러스터를 관리할 수 있습니다.    |
| 208   | cloud_gaming_join_session     | 클라우드 계산 세션에 참가할 수 있습니다.     |
| 209   | cloud_saved_games     | 클라우드 제목 저장소에서 게임을 저장할 수 있습니다.    |
| 211   | share_content     | 다른 사용자와 콘텐츠를 공유할 수 있습니다.    |
| 214   | premium_content   | 구매, 다운로드를 Xbox Live gold 정기 구독을 사용 하 여 사용 가능한 프리미엄 콘텐츠를 시작 합니다.     |
| 219   | subscription_content  | 구매 및 premium 구독 콘텐츠 다운로드를 프리미엄 구독 기능을 사용 합니다.     |
| 220   | social_network_sharing    | 소셜 네트워크의 진행률 정보를 공유할 수 있습니다.    |
| 224   | premium_video     | 프리미엄 비디오 서비스에 액세스할 수 있습니다.    |
| 235   | purchase_content  | 콘텐츠를 구입할 수 있습니다.     |
| 247   | user_created_content  | 다운로드 하 고 콘텐츠를 생성 하는 온라인 사용자를 볼 수 있습니다.    |
| 249   | profile_viewing   | 다른 사용자의 프로필을 볼 수 있습니다.   |
| 252   | 통신    | 다른 사람과 메시징 비동기 텍스트를 사용할 수 있습니다.    |
| 254   | multiplayer_sessions  | 멀티 플레이 게임에 대 한 세션을 연결할 수 있습니다.   |
| 255   | add_friend    | 다른 Xbox Live 사용자가 수행 하 고 Xbox Live 친구를 추가할 수 있습니다.   |
