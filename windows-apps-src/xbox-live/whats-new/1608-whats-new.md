---
title: 새로운 기능-2016 년 8 월 Xbox Live SDK 용
description: 새로운 기능-2016 년 8 월 Xbox Live SDK 용
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2a498fea1ed0974935a273c9ee72ba2c95d15959
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627908"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>새로운 기능-2016 년 8 월 Xbox Live SDK 용

참조 하세요 합니다 [새로운 기능-2016 년 6 월](1606-whats-new.md) 추가 된 기능에 대 한 문서는 2016 년 6 월 릴리스에서 합니다.

## <a name="os-and-tool-support"></a>OS 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [Version 10.0.10240] 및 Visual Studio 2015 RTM [Version 14.0.23107.0]를 지원합니다.

## <a name="documentation"></a>설명서
- 에 대 한 지침이 있습니다 UWP 응용 프로그램을 작성 하는 게임에 사용자를 초대 하는 기능을 구현 하는 경우에 ```.appxmanifest``` 에서 필요한 변경 내용을 [구성 Your AppXManifest에 대 한 다중 접속](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md)합니다.  이전에 설명한이 [포럼](https://forums.xboxlive.com) 하며 [uwp 연대에서 xbox live 코드 이식](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 문서
- 합니다 [소셜 Manager 소개](../social-platform/intro-to-social-manager.md) 문서 최근 API 변경 내용을 반영 하 고 일부 함수에 대 한 반환 코드에 대 한 자세한 정보를 제공 하도록 업데이트 되었습니다.

## <a name="unity-samples"></a>Unity 샘플
UWP 응용 프로그램을 작성 하는 Unity 개발자를 위한 몇 가지 새로운 샘플이 추가 되었습니다.
- 이제는 Unity의 버전이 소셜 샘플, 샘플/소셜/Unity 디렉터리에서 찾을 수 있습니다.
- 연결 된 저장소를 사용 하는 방법을 설명 하는 샘플 이기도 합니다.  이 샘플에 대 한 샘플/GameSave/Unity를 참조 하세요.
AchievementsLeaderboard 샘플/AchievementsLeaderboard/Unity에서 Unity 버전

## <a name="social-manager"></a>주민 등록 관리자
위에서 언급 한 설명서 업데이트 외에도 추가 된 몇 가지 새로운 Api 있습니다.  이러한 아래에서 설명 하 고 social_manager.h에서 자세한 내용을 볼 수 있습니다.

- 다시 만들기를 포함 하지 않고 소셜 그룹의 업데이트를 허용 하는 추가 된 새 API:

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- 소셜 그룹의 업데이트를 완료로 표시 됩니다는 ```social_user_group_updated``` 이벤트


## <a name="multiplayer"></a>멀티 플레이
향상 된 세션 검색을 새 멀티 플레이 게임 Api를 사용 하 여는 데 사용 합니다.

새 Api를 사용에서 태그, 문자열 및 기타 다양 한 데이터를 재생 하려는 세션을 보다 쉽게 찾을 수 있도록 하려면 필터링 할 수 있습니다.

다음 달의 포괄적인 설명서를 게시 했습니다 있지만 간단 하 게 연결할 수 있습니다 이제 "검색 핸들" MPSD 세션 사용 ```set_search_handle``` 프로그램 제목 호출 하 여 강력한 필터링 메커니즘을 사용 하 여 세션에 대 한 사용자를 검색할 수 다음 ```get_search_handles```

새 Api는 다음과 같습니다.  시도 하 고 문제가 발생 하는 경우 게시에서 지원 스레드는 [포럼](https://forums.xboxlive.com) 하거나 프로그램 파악 합니다.  곧 이러한 Api를 사용 하는 방법의 예제를 얻게 됩니다.

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> set_search_handle(
    _In_ multiplayer_search_handle_request searchHandleRequest
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<std::vector<multiplayer_search_handle_details>>> get_search_handles(
    _In_ const string_t& serviceConfigurationId,
    _In_ const string_t& sessionTemplateName,
    _In_ const string_t& orderBy,
    _In_ bool orderAscending,
    _In_ const string_t& searchQuery
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> clear_search_handle(_In_ const string_t& handleId);
```

### <a name="xbox-integrated-multiplayer"></a>Xbox 통합된 멀티 플레이 게임

설명서에는 Xbox 통합 멀티 플레이 게임 (XIM) API 포함 되어 있습니다.  API 자체 Xbox Live SDK의 이후 릴리스에서 사용할 수 있지만 문서 및 헤더는 이루어집니다 미리 보기로 제공.

XIM는 Xbox Live 서비스의 기능을 통해 게임에 실시간 다중 접속 네트워킹 및 채팅 통신을 쉽게 추가 하는 것에 대 한 자체 포함 된 인터페이스입니다.

API 설명서의이 미리 보기 고객 의견 및 조회를 장려 하기 위해 여기에 공유 됩니다. Xfest 2016 이전에이 API에 이야기 하 고 보관을 볼 수 있습니다 [관리 되는 파트너 개발자 사이트에서 프레젠테이션 자료](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx) "턴키 다중 접속 네트워킹 및 채팅" talk에서. C + + API에 대해서만이 미리 보기 설명서는 참고 합니다. WinRT에 대 한에 해당 하는 C# 다른 언어는 올해 후반에 출시 됩니다.

의견이 XIM의 기능에 관심이 있는 경우 또는이 프로젝트에 대 한 다른 질문이 있으시면에 게시 합니다 [Xbox 개발자 포럼](https://forums.xboxlive.com/) 개발자 계정 관리자를 통해 연락 또는 합니다.

Xbox Live SDK xbox_integrated_multiplayer.chm Docs 디렉터리에서에서이 새 설명서를 볼 수 있습니다.  포함 파일이 \include\xim\XboxIntegratedMultiplayer.h에서 미리 보기로 사용할 수 있습니다.  
