---
title: 새로운 기능에 대 한 Xbox Live SDK-2016 년 8 월
author: KevinAsgari
description: 새로운 기능에 대 한 Xbox Live SDK-2016 년 8 월
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fd78e2ff061f8cf1268a340ad63d954993f563fc
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5741722"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>새로운 기능에 대 한 Xbox Live SDK-2016 년 8 월

추가 된 항목에 대 한 [새로운-6 월 2016](1606-whats-new.md) 문서를 참조 하십시오 6 월 2016 릴리스에서 합니다.

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [버전 10.0.10240] 및 Visual Studio 2015 RTM [버전 14.0.23107.0]를 지원합니다.

## <a name="documentation"></a>설명서
- 에 대 한 지침이 있습니다 UWP 응용 프로그램을 작성 하는 사용자 게임에 초대 하는 기능을 구현 하는 경우는 ```.appxmanifest``` [구성 Your AppXManifest에 대 한 멀티 플레이어](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md)에 필요한 변경 합니다.  [포럼](https://forums.xboxlive.com) 및 [코드에서 uwp로 연대 live xbox 포팅](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 문서에서 이전에 설명한이
- [소셜 관리자를 소개](../social-platform/intro-to-social-manager.md) 문서 기능 중 일부에 대 한 반환 코드에 대 한 자세한 정보를 제공 하 고 최신 API 변경 내용을 반영 하도록 업데이트 되었습니다.

## <a name="unity-samples"></a>Unity 샘플
UWP 응용 프로그램을 작성 하는 Unity 개발자에 대 한 몇 가지 새로운 샘플이 추가 되었습니다.
- 소셜 샘플의 Unity 버전은 이제, 샘플/소셜/Unity 디렉터리 아래에서 찾을 수 있습니다.
- 연결 된 저장소를 사용 하는 방법을 설명 하는 샘플 이기도 합니다.  샘플 Unity/GameSave/샘플을 참조 하세요.
AchievementsLeaderboard 샘플/AchievementsLeaderboard/Unity에서의 Unity 버전

## <a name="social-manager"></a>소셜 관리자
위에서 언급 한 설명서 업데이트 하는 것 외에도 추가 된 새로운 Api 몇 가지 있습니다.  아래는 이러한 및 social_manager.h에서 자세히 볼 수 있습니다.

- 레크리에이션 없이 소셜 그룹 업데이트 허용 하는 추가 된 새로운 API:

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- 소셜 그룹의 완료 된 업데이트도 표시 되는 ```social_user_group_updated``` 이벤트


## <a name="multiplayer"></a>멀티 플레이
향상 된 세션 검색을 새로운 멀티 플레이어 Api를 사용 하 여이 활용 하 합니다.

새로운 Api를 사용 하 여 태그, 문자열 및 다른 사용자가 재생 하려는 세션을 보다 쉽게 찾을 수 있도록 다양 한 데이터를 필터링 할 수 있습니다.

향후 몇 개월에 더 광범위 한 문서를 게시 하 하지만 간단 하 게 연결할 수 있는 이제 "검색 핸들" 사용 하 여 MPSD 세션 ```set_search_handle``` 한 다음 사용자가 제목 호출 하 여 강력한 필터링 메커니즘을 사용 하 여 세션 검색 수 ```get_search_handles```

새로운 Api는 다음과 같습니다.  로그 아웃 하 고 하십시오 [포럼](https://forums.xboxlive.com) 에서 지원 스레드를 게시 하거나에 댐을에 도달 하 여 문제가 발생 하는 경우.  곧 이러한 Api를 사용 하는 방법의 예는 했습니다.

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

### <a name="xbox-integrated-multiplayer"></a>Xbox 통합 멀티 플레이어

설명서에는 Xbox 통합 멀티 플레이어 (XIM) API 포함 되어 있습니다.  자체 API Xbox Live SDK의 이후 릴리스에서 사용할 수 있지만 설명서 및 헤더는 시간차 미리 보기에 사용할 수 있습니다.

XIM는 쉽게 실시간 멀티 플레이어 네트워킹 및 채팅 통신 Xbox Live 서비스의 기능을 통해 게임에 추가 하기 위한 독립적인된 인터페이스입니다.

API 설명서의이 미리 보기는 고객 의견 및 질문을 추구 하기 여기 공유 됩니다. Xfest 2016 이전에이 API에 대 한 설명 했던 및 보관 된 [관리 파트너 개발자 사이트에서 프레젠테이션 자료](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx) 는 "턴 키 멀티 플레이어 네트워킹 및 채팅"에서 이야기를 볼 수 있습니다. 이 미리 보기 설명서는 c + + API에 대 한 노트 올해 후반에 해당 하는 C# 및 다른 언어에 대 한 WinRT 해제 됩니다.

XIM의 기능에 관심이 있다면 의견이 나이 프로젝트에 대 한 다른 질문, [Xbox 개발자 포럼](https://forums.xboxlive.com/) 에 게시 하거나 개발자 계정 관리자를 통해 자유롭게 합니다.

Xbox Live SDK xbox_integrated_multiplayer.chm 문서 디렉터리에서에서이 새 문서를 볼 수 있습니다.  Include 파일이 \include\xim\XboxIntegratedMultiplayer.h에서 미리 보기를 사용할 수 있습니다.  
