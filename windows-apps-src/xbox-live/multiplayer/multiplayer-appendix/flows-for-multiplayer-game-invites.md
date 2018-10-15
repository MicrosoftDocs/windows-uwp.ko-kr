---
title: 멀티 플레이 게임 초대를 위한 업데이트 된 흐름
author: KevinAsgari
description: Xbox Live 멀티 플레이 게임 초대를 구현 하기 위한 업데이트 된 흐름을 제공 합니다.
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 멀티 플레이 2015, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 66de8c34857d667b66cdfb091f83106fed777279
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4609853"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>멀티 플레이 게임 초대를 위한 업데이트 된 흐름

Xbox One 베타 피드백의 결과로 사용자 환경을 흐름 멀티 플레이 게임 초대에 대 한 변경 되었습니다 Xbox One 복구 업데이트 24 2013 년 11 월 6에 릴리스된 부터는. 이 **사용자 환경 (UX)만** 변경 이며 게임 타이틀의 관점에서 기능 이나 동작에 영향을 주지 않습니다. 제목 개발자는 모든 코드를 변경할 필요가 없습니다.

## <a name="summary-of-changes"></a>변경 내용 요약

-   초기 초대 알림으로 변경 되었습니다에서 "내 타사 가입" "&lt;제목 게임&gt; 보겠습니다 재생" 이제 사용자가 게임을 시작 하 고 게임 플레이에 이동할 수 있는 단추에서.

-   타사 앱은 기본적으로 맞춰지지 새 선택 하면 "&lt;제목 게임&gt; 보겠습니다 재생" 옵션입니다. 이 또한 변경 사용자 게임 플레이에 오른쪽을 이동할 수 있도록 합니다.

-   새 알림 "추가를 게임 친구 \ [*번호*\]" 이라는 보낸 사람의 쪽에 추가 되었습니다. 따라서 지우기 초대 아웃 보낸 게임 세션을 사용자의 상대와 연결 되어 있습니다.

자세한 사용자 환경을 흐름은 다음 예제에 설명 되어 있습니다. 각 표에서 David 및 숙 두 사용자에 대 한 예제 흐름을 보여 줍니다. 이러한 흐름 각 열에 표시 되 고 동시에 발생 합니다. <b style="background-color: #FFFF00">강조 표시 된 텍스트</b> 이전 UX 흐름에서 변경 된 조정 보여 줍니다.

## <a name="inviting-users-from-within-a-game"></a>게임 내에서 사용자를 초대합니다.

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
그 재생 되는 게임에 대 한 멀티 플레이 로비 David입니다.<p><br>David <b>친구 초대</b>를 선택합니다.<p><br>David 숙을 선택합니다.<p><br>알림 초대 전송 되었음을 나타내는 팝업 됩니다. | 숙 다른 게임을 플레이 됩니다.
    </td>
    <td style="border-bottom:solid 1px #fff">
숙 다른 게임을 플레이 됩니다.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
알림은 David, <b style="background-color: #FFFF00">게임 이름 및 아이콘을 표시</b>하 고에서 초대를 나타내는 팝업 됩니다. (타사 앱은 하지 자동-끌기.) <p><br>
알림 센터, <b style="background-color: #FFFF00">숙 초대를 수락 및 시작을 선택할 수 있습니다</b>, <b>초대를 수락</b>또는 <b style="background-color: #FFFF00">거절 초대</b>합니다.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">사례 1: 숙 시작을 선택 하 고 초대를 수락</b> (이 새 옵션)</td>
  </tr>
  <tr>
    <td>
알림 숙 David의 외부에 연결 하 고 있는지를 나타내는 팝업 됩니다.             
    <p><br>
David 멀티 플레이 로비에서 게임을 시작합니다.                              
    <p><br>
    <b style="background-color: #FFFF00">알림 게임 초대 숙에 전송 되었음을 나타내는 팝업 됩니다.</b>
    </td>
    <td>
게임 시작 하 고 타사 앱 스냅 되지 않습니다.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>사례 2: 숙 선택 수락 초대</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">숙 보유할을 결합합니다.</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">알림 게임 초대 숙에 전송 되었음을 나타내는 팝업 됩니다.</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>알림 게임 자 찾을 수 있는지를 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 숙 선택할 수 있습니다. <ul>
    <li>   <b>게임 초대를 수락:</b> 게임을 시작 합니다.
    <li>   <b>게임 초대 거절:</b> 게임이 실행합니다. 숙 파티에서 여전히 이며 후속 게임 초대를 받게 됩니다.         
    <li>   <b style="background-color: #FFFF00">타사의 두고: 게임 실행 수 없습니다. 숙 파티에서 제거 됩니다.</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>게임 초대 흐름: 파티, 및 제목 전환

<table>
  <tr>
    <td>
함께 게임을 플레이 합니다.
    <p><br>
David 멀티 플레이 메뉴로 이동 하 고 다른 게임으로 전환 합니다.
     <p><br>
Xbox 시스템 UI 그 새 자신의 타사 전환 하려는 경우 게임에는 그 응답할 수 <b>예</b> 또는 <b>아니요</b>David 요청 합니다.
    </td>
    <td style="text-align:top">
함께 게임을 플레이 합니다.
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>사례 1: 예</b>
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
타사 새 제목에 제공 됩니다.
    <p><br>
멀티 플레이 로비에서 David 게임을 시작합니다.
    <p><br>
    <b style="background-color: #FFFF00">알림 숙 게임 초대 집합이 전송 되었음을 나타내는 팝업 됩니다.
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>알림 게임 자 찾을 수 있는지를 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 숙 선택할 수 있습니다. <ul>
     <li><b>수락 게임 초대</b>: 새 게임 시작 <li><b>게임 초대 거절:</b> 게임이 실행 하지만 숙은 여전히 보유할 이며 후속 게임 초대를 받게 됩니다.
     <li><b style="background-color: #FFFF00"><b>타사 유지:</b> 게임 시작 및 숙 없음 파티에서 제거 됩니다.</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>사례 2: 아니요</b>
    </td>
  </tr>
  <tr>
    <td>
타사 새 제목을 상태가 되지 못합니다.
멀티 플레이 모드에서 자신의 파티 구성원 없이 David를 재생합니다.
David 보유할 아직입니다.
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>집에서 흐름 초대

<table>
  <tr>
    <td>
David가 <b>홈</b>, 검색 및 숙 온라인 상태 인지 느껴질 자신의 <b>친구</b> 목록에서.
    <p><br>
David 자신의 자 숙 초대를 선택 합니다. 알림 초대를 보내 나타내는 팝업 됩니다. 타사 앱 David에 대 한 스냅 됩니다.
    </td>
    <td>
숙 게임을 플레이 됩니다.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
알림은 David 숙 자신의 자에 초대를 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 숙 <b>초대를 수락</b>옵션이 있습니다.
    <p><br>
숙을 수락 하면 타사 앱이 맞춥니다.                                                                         </td>
  </tr>
  <tr>
    <td>
알림 숙 당사자에 연결 하 고 있는지를 나타내는 팝업 됩니다.
    <p><br>
David 및 숙 재생 하려는 게임에 설명 합니다. David 게임을 시작 하 고 멀티 플레이 게임 모드를 시작 합니다.
    <p><br>
게임, 친구 또는 자동 끌어오기를 초대 하는 옵션이 타사 멤버 제공 하거나 합니다.
    <p><br>
    <b style="background-color: #FFFF00">알림 게임 초대 집합이 전송 되었음을 나타내는 팝업 됩니다.</b>
    </td>
    <td>
알림 자는 게임에서 발견 했음을 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 숙 다음을 수행할 수 있습니다. <ul>
    <li>   <b>게임 초대를 수락:</b> 게임 시작 <li>   <b>게임 초대 거절:</b> 게임 실행 수 없음, 숙 되는 타사에서 여전히 이며 후속 초대를 받게 됩니다.
    <li>   <b style="background-color: #FFFF00">타사의 두고: 게임이 시작 숙 파티에서 제거 됩니다.</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>게임 초대 전송 알림에 대 한 추가 정보

**게임 초대 전송** 알림 게임 세션 원격 타사 멤버를 사용 하 여 설정 된 처음만 표시 됩니다. 원격 타사 구성원에 게 전송 하는 후속 요청이이 알림을 생성 되지 않습니다. 이 제목 **PullReservedPlayersAsync**를 여러 번 호출 하면 반복 된 **게임 초대 전송** 토스트와 스팸 화 되에서 사용자를 방지 합니다.

**참고** 으로 예약 됨, 친구를 원하는 모든 추가 하 고 다음 **PullReservedPlayersAsync** 를 한 번만 호출 하는 것이 좋습니다.
