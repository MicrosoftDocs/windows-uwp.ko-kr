---
title: 파트너 센터에서 풍부한 존재 구성
description: 파트너 센터에서 풍부한 상태 문자열을 구성 하는 방법 알아보기
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 게임, uwp, windows 10, 하나는 Xbox, 서식 있는 현재 상태 문자열의 경우 파트너 센터
ms.openlocfilehash: 5c2add99c6fc6ad1c7f085eb35dc4fdba9e0688d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624128"
---
# <a name="configure-rich-presence-in-partner-center"></a>파트너 센터에서 풍부한 현재 상태를 구성

대화 상대의 문자열에 사용자의 게임 활동을 표시합니다. 플레이어의 게이머 태그 아래에 표시 됩니다는 **친구 & 클럽** 목록과 Xbox Live 사용자 프로필입니다. 구성 된 대화 상대의 문자열의 게임 이름 재생 중인에 추가 됩니다. BubblePop 호출 게임을 만들고 대화 상대의 문자열 "친구 들과 Popping 거품"을 구성 하는 경우 구성 된 문자열에 "BubblePop-친구 들과 Popping 거품" 상태를 생성 합니다. 다음 컨텍스트에서 대화 상대의 문자열로 표시 되는 방식 볼 수 있습니다.

다음 스크린 샷에서 Xbox Live 사용자에서 **마지막 외침** 하 고 **Lucha Uno** 대화 상대의 문자열을 사용 하 여 게임을 재생 하는 합니다.

![친구 목록 예제](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

다음 스크린샷에 보이는 **Lucha Uno** 전체 프로필에서 문자열 서식 있는 현재 상태입니다.

![프로필 예제](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> 대화 상대의 Xbox Live 크리에이터 스 프로그램 제목에 사용할 수 없는 문자열과 하므로 해당 타이틀에 대 한 구성 가능 하지 않습니다. 이 문서의 내용은입니다 ID@Xbox 와 관리 되는 파트너 제목입니다.

## <a name="requirements"></a>요구 사항

하 여 제목 서식 있는 현재 상태 문자열을 구성 하기 전에 다음 조건을 충족 해야 합니다.

- Windows 개발 계정이 있어야 합니다.
- 개발 계정에 등록 되어야 합니다는 ID@Xbox 프로그램 또는 관리 되는 파트너 개발자 계정으로 합니다.
- 프로그램 제목 파트너 센터에 등록 되어야 합니다 및 Xbox Live를 사용 하도록 설정 되어야 합니다.

서식 있는 현재 상태 문자열을 사용 하 여 파트너 센터에서 구성 해야 합니다.

## <a name="rich-presence-configuration-page"></a>서식 있는 현재 상태 구성 페이지

서식 있는 현재 상태 문자열을 제목에서 Xbox Live 서비스의 일부로 구성 됩니다 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.

다음 지침에 따라 대화 상대의 구성 페이지로 이동 합니다.

1. 로 이동 [파트너 센터](https://partner.microsoft.com/dashboard) developer.microsoft.com에 있습니다.
2. 로그인 요청 되 면 등록 된 Windows 개발자 계정으로 로그인 합니다.
3. Xbox Live를 사용 하도록 설정 제목 또는에서 앱을 선택 합니다 **개요** 페이지입니다. 대화 상대의 문자열 구성에 대 한 설정 되지 않습니다 크리에이터 스 프로그램 제목을 선택 하지 마십시오.
4. 클릭 합니다 **Services** 드롭다운 및 Xbox Live를 선택 합니다.
5. 아래로 스크롤하여 합니다 **상대의** 링크를 클릭 하 고 합니다.

대화 상대의 페이지 단추를 새 대화 상대의 문자열을 만들고 이전에 구성 된 문자열 목록을 검색할 수 있는 서비스의 간략 한 설명을 표시 합니다. 이 페이지에서 새 문자열 뿐만 아니라 편집을 구성한 구성 된 문자열을 검토 합니다.

![대화 상대의 문자열 구성 페이지의 예](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> "놀이 Net Runner-10 대를 사용 하 여 태양과 달 자료에서 매치 게임."와 같은 문자열 데이터 플랫폼 2017 업데이트를 기준으로 하는 개발자에 게 제공 되지 않습니다. 데이터 플랫폼 2013 *변수* 는 데이터 플랫폼 2017에서 사용할 수 없습니다. 이 경우이 수가 종료 "10" 변수가 있습니다. 데이터 플랫폼 2017 업데이트는 "놀이 Net Runner-매치 게임 태양과 달 자료에서 합니다." 후에 해당 문자열 "롤플레잉 Net Runner-메뉴에" 여전히 유효한 대화 상대의 문자열입니다.

## <a name="create-a-new-rich-presence-string"></a>새 대화 상대의 문자열을 만들려면

서식 있는 현재 상태 문자열을 만들려면 단추를 클릭 **새 대화 상대의 문자열**합니다. 에 맞게 UI를 사용 하 여 표시 됩니다는 **현재 상태 세부 정보** 를 포함 하는 **고유 다양 한 상태 ID** 뿐만 **문자열 표시** 새 대화 상대의 문자열에.

![새 대화 상대의 문자열 UI](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**고유 다양 한 상태 ID** -고유한 다양 한 상태 ID가 문자열에 다양 한 현재 상태를 식별 하는 데 사용 하는 문자열입니다. 이 문자열의 플레이어 게임 상태를 설정 하는 데 사용할 이며 표시 하려는 특정 문자열을 사용 하 여 연결 됩니다. 사용자 ID에는 최대 50 자의 수 있습니다.

**표시 문자열** -문자열의 표시를 표시 하려는 일부 게이머 재생 게임의 상태에 추가 된 문자열입니다. 입력 위치는이 대화 상대의 문자열에 표시할 게임에 관심을 생성 합니다. 디스플레이 최대 100 자 있지만 인스턴스는 처음 40 자만 표시 됩니다 됩니다.

새 대화 상대의 문자열을 만들기 위해 입력 필드와 키를 눌러 합니다 **저장** 단추입니다.
열립니다 다시 구성 된 문자열의 목록에 추가할 새 대화 상대의 문자열에 나와 있는 다양 한 현재 상태 구성 페이지에 저장 클릭 하면 합니다.

## <a name="review-edit-and-delete-strings"></a>검토, 편집 및 문자열 삭제

여기 몇 가지 구성 된 문자열을 사용 하 여 대화 상대의 구성 페이지를 볼 수 있습니다.
![서식 있는 현재 상태 페이지 예제 구성](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

이전에 만든된 문자열을 검토 하려면 대화 상대의 구성 페이지의 목록을 찾아보고 하기만 하면 됩니다. 있습니다 수 표시 고유 다양 한 상태 ID와 표시 문자열 함께 합니다. 이 다양 한 현재 상태 문자열을 지정 하려면 타이틀의 코드에서 고유 다양 한 상태 Id를 사용 해야 하는 경우에 유용 합니다.

다양 한 현재 상태를 클릭 하기만 하면 문자열을 편집 하는 **고유 다양 한 상태 ID** 편집 하려는 문자열에 대 한 링크입니다. 현재 편집 하기 위한 입력 문자열 설정을 사용 하 여 새 대화 상대의 문자열을 만드는 데 동일한 UI에 연결 됩니다. 편집을 클릭 한 후 합니다 **저장** 변경 내용으로 구성 된 문자열을 업데이트 하는 단추입니다.

구성 된 대화 상대의 문자열 클릭을 삭제 하는 **삭제** 삭제 하려는 대화 상대의 문자열로 같은 행의 서식 있는 현재 상태 구성 페이지의 링크입니다. 삭제를 확인 하 라는 메시지가 표시 됩니다.

## <a name="further-reading"></a>추가 정보

대화 상대의 문자열 기능 및 구현 하는 방법에 대 한 깊이 있는 자세한 개념 정보를 가져오려면 읽기를 [상대의 설명서](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview)합니다.