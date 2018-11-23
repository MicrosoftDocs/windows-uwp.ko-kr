---
title: 파트너 센터에서 다양 한 상태 구성
author: aablackm
description: 파트너 센터에서 다양 한 상태 문자열을 구성 하는 방법에 알아봅니다.
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 게임, uwp, windows 10, Xbox one, 다양 한 상태 문자열, 파트너 센터
ms.openlocfilehash: cb16b7e8c3776f2509207f15a6c26341f9028fe5
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7578826"
---
# <a name="configure-rich-presence-in-partner-center"></a>파트너 센터에서 다양 한 상태를 구성 합니다.

다양 한 상태 문자열 사용자의 게임에서 활동을 표시합니다. 플레이어의 게이머는 **친구 및 클럽** 목록에도 자신의 Xbox Live 사용자 프로필와 마찬가지로 아래 표시 됩니다. 구성 된 다양 한 상태 문자열 재생 중인 이름에 게임에 추가 됩니다. BubblePop 라는 게임을 만들고 다양 한 상태 문자열 "친구와 Popping 풍선"를 구성 하는 경우에 구성 된 문자열은 상태로 "BubblePop-친구와 Popping 풍선"를 만듭니다. 아래는 다양 한 상태 문자열 컨텍스트에서 표시 되는 방식을 확인할 수 있습니다.

Xbox Live 스크린샷에 **마지막 외침** 및 **Lucha Uno** 사용자가 다양 한 상태 문자열을 사용 하 여 게임 재생 중인 합니다.

![친구 목록 예제](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

스크린샷에 **Lucha Uno** 피 자신의 프로필의 다양 한 상태 문자열 전체 합니다.

![프로필 예제](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> 다양 한 상태 문자열 Xbox Live 크리에이터 스 프로그램 타이틀 사용할 수 있으며 따라서는 해당 제목에 대해 구성할 수 없습니다. 이 문서에서 콘텐츠를 ID@Xbox 및 관리 파트너 타이틀 합니다.

## <a name="requirements"></a>요구 사항

다양 한 상태 문자열을 구성 하기 전에 및 타이틀 다음 조건을 충족 해야 합니다.

- Windows 개발 계정이 있어야 합니다.
- 개발 계정에 등록 해야 합니다 ID@Xbox 프로그램 또는 관리 파트너 개발자 계정으로 합니다.
- 타이틀은 파트너 센터에 등록 해야 하 고 Xbox Live가 지원 됩니다.

다양 한 상태 문자열을 사용 하려면 먼저 파트너 센터에서 구성 해야 합니다.

## <a name="rich-presence-configuration-page"></a>서식 있는 존재 구성 페이지

다양 한 상태 문자열은 [파트너 센터](https://partner.microsoft.com/dashboard)에서 타이틀에 Xbox Live 서비스의 일환으로 구성 됩니다.

다음 지침을 사용 하 여 다양 한 상태 구성 페이지로 이동 합니다.

1. Developer.microsoft.com에서 [파트너](https://partner.microsoft.com/dashboard) 센터로 이동 합니다.
2. 요청에 로그인 하면 등록 된 Windows 개발자 계정으로 로그인 합니다.
3. 제목이 나 앱 **개요** 페이지에서 활성화 Xbox Live를 선택 합니다. 다양 한 상태 문자열 구성에 사용할 수는 크리에이터 스 프로그램 타이틀을 선택 하지 마십시오.
4. **서비스** 드롭다운 클릭 하 고 Xbox Live를 선택 합니다.
5. **다양 한 상태** 링크 나타날 때까지 아래로 스크롤한 클릭 합니다.

다양 한 상태 페이지에는 새 다양 한 상태 문자열을 만들고 이전에 구성 된 문자열 목록을 검색 하는 단추는 서비스에 대 한 간략 한 설명을 표시 됩니다. 이 페이지에서 새 문자열 구성 뿐 아니라 편집 및 구성 된 문자열 리뷰 수 있습니다.

![다양 한 상태 문자열 구성 페이지의 예](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> "게임 순 Runner-매치 10 파괴 기준 달에 게임."와 같은 문자열 데이터 플랫폼 2017 업데이트 부터는 개발자에 게 제공 되지 않습니다. 데이터 플랫폼 2013 *변수* 데이터 플랫폼 2017에서 제공 되지 않습니다. 이 경우이 수가 "10" 중단 변수가 있습니다. 데이터 플랫폼 2017 업데이트 "게임 순 Runner-매치 달 베이스에서 게임." 것 후 해당 하는 문자열 "재생 순 Runner-메뉴에서" 여전히 유효한 다양 한 상태 문자열입니다.

## <a name="create-a-new-rich-presence-string"></a>새 다양 한 상태 문자열을 만듭니다

다양 한 상태 문자열을 만들려면 **새 다양 한 상태 문자열**단추를 클릭 합니다. 새 다양 한 상태 문자열에 대 한 **디스플레이 문자열** 뿐 아니라 **다양 한 상태의 고유 ID** 를 포함 하는 **상태 세부 정보** 에 맞게 UI를 사용 하 여 제공 됩니다.

![새 다양 한 상태 문자열 UI](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**고유 풍부한 존재 ID** -고유한 풍부한 존재 ID에 다양 한 상태 문자열을 식별 하는 데 문자열입니다. 이 문자열 플레이어 게임의 상태를 설정 하는 데 사용 될 이며 표시 하려면 특정 문자열을 사용 하 여 연결 됩니다. 사용자의 ID는 최대 50 자까지 될 수 있습니다.

게임을 플레이 **문자열을 표시 하** 는 문자열은 표시 하려는 일부 게이머의 상태를 추가 하는 문자열은 표시 합니다. 이것은 입력할 수 있는 다양 한 상태 문자열에 게임에 관심을 생성 하는 표시 하려는 합니다. 디스플레이 최대 100 자 수 있지만 먼저 40 자를 보입니다 인스턴스가 됩니다.

새 다양 한 상태 문자열을 만들려면 두 필드 및 **저장** 단추를 누릅니다.
구성 된 문자열 목록에 추가 된 새 다양 한 상태 문자열에 표시 서식 있는 존재 구성 페이지로 다시 이동 저장을 클릭 하면 합니다.

## <a name="review-edit-and-delete-strings"></a>검토, 편집 하 고 문자열을 삭제 합니다.

여기 몇 가지 구성 된 문자열을 사용 하 여 다양 한 상태 구성 페이지를 볼 수 있습니다.
![다양 한 상태 페이지 구성 예제](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

이전에 만든된 문자열을 검토 하려면 다양 한 상태 구성 페이지에서 목록을 탐색할 하기만 하면 됩니다. 있습니다 피 고유 풍부한 존재 ID와 디스플레이 문자열 함께 합니다. 이 타이틀의 코드에서 고유한 풍부한 존재 Id를 사용 하 여 다양 한 상태 문자열을 지정 해야 할 경우에 유용 합니다.

다양 한 상태를 편집 하려면 문자열 간단히 편집 하려는 문자열에 대 한 **고유 풍부한 존재 ID** 링크 클릭 합니다. 편집을 위해 입력 현재 문자열 설정을 사용 하 여 새 다양 한 상태 문자열을 만드는 데 동일한 UI로 이동 합니다. 변경 내용으로 구성 된 문자열을 업데이트 하는 있어서 편집을 클릭 한 후 단추 **저장** 합니다.

구성 된 다양 한 상태를 삭제 하려면 문자열 삭제 하려는 다양 한 상태 문자열와 같은 행에 풍부한 존재 구성 페이지에서 **삭제** 링크를 클릭 합니다. 삭제를 확인 하 라는 메시지가 표시 됩니다.

## <a name="further-reading"></a>추가 정보

다양 한 상태 문자열 기능 및 구현 하는 방법에 대 한 더 자세한 개념 정보를 가져오려면 [다양 한 상태 설명서](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview)를 참조 합니다.