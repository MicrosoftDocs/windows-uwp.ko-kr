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
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4386416"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>멀티 플레이 게임 초대를 위한 업데이트 된 흐름

Xbox One 베타 피드백 결과로 사용자 환경을 흐름 멀티 플레이 게임 초대에 대 한 변경 되었습니다 부터는 Xbox One 복구 업데이트 24 2013 년 11 월 6에 릴리스된. **사용자 환경 (UX)만** 변경 이며 동작이 나 게임 타이틀의 관점에서 기능에 영향을 주지 않습니다. 제목 개발자는 모든 코드를 변경할 필요가 없습니다.

## <a name="summary-of-changes"></a>변경 내용 요약

-   초기 초대 알림 메시지에서 변경 되었습니다 "파티 가입"를 "&lt;제목 게임&gt; 보겠습니다 재생" 이제 사용자가 게임을 시작 하 고 게임 플레이를 이동할 수 있는 단추에서.

-   타사 앱은 기본적으로 맞춰지지 새 선택 하면 "&lt;제목 게임&gt; 보겠습니다 재생" 옵션입니다. 게임 플레이에 사용자가 오른쪽 이동할 수 있도록 변경도 됩니다.

-   새 알림 "추가를 게임 친구 \ [*번호*\]" 이라는 보낸 사람의 쪽에 추가 되었습니다. 따라서 지우기 초대 아웃 보낸 게임 세션 사용자의 상대와 연결 되어 있습니다.

자세한 사용자 환경을 흐름은 다음 예제에 설명 되어 있습니다. 각 테이블 David 및 숙 두 사용자는 예제는 흐름을 보여 줍니다. 이러한 흐름 각 열에 표시 되 고 동시에 발생 합니다. <b style="background-color: #FFFF00">강조 표시 된 텍스트</b> 이전 UX 흐름에서 변경 된 조정 보여 줍니다.

## <a name="inviting-users-from-within-a-game"></a>게임 내에서 사용자를 초대합니다.

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
그 재생 되는 게임에 대 한 멀티 플레이 로비 David입니다.<p><br>David <b>친구 초대</b>를 선택합니다.<p><br>David 숙을 선택합니다.<p><br>알림 초대 전송 되었음을 나타내는 팝업 됩니다. | 숙 다른 게임을 재생 합니다.
    </td>
    <td style="border-bottom:solid 1px #fff">
숙 다른 게임을 재생 합니다.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
알림 메시지에서 David, <b style="background-color: #FFFF00">게임의 이름 및 아이콘을 표시</b>하 고 초대를 나타내는 팝업 됩니다. (타사 앱은 하지 자동-끌기.) <p><br>
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
    <td colspan="2" style="text-align:center"><b>사례 2: 숙이 초대를 수락</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">숙 파티를 조인합니다.</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">알림 게임 초대 숙에 전송 되었음을 나타내는 팝업 됩니다.</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>알림 자 게임을 찾을 수를 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 숙 선택할 수 있습니다. <ul>
    <li>   <b>게임 초대를 수락:</b> 게임을 시작 합니다.
    <li>   <b>게임 초대 거절:</b> 게임이 실행합니다. 숙 파티에서 여전히 이며 후속 게임 초대를 받게 됩니다.         
    <li>   <b style="background-color: #FFFF00">타사 유지: 게임 실행 수 없습니다. 숙 파티에서 제거 됩니다.</b>
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
David 다른 게임으로 전환 하 고 멀티 플레이 메뉴로 이동 합니다.
     <p><br>
Xbox 시스템 UI가 자신의 타사 새로운 전환 하려는 경우 게임는 그 응답할 수 <b>예</b> 또는 <b>아니요</b>David 요청 합니다.
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
    <td>알림 자 게임을 찾을 수를 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 숙 선택할 수 있습니다. <ul>
     <li><b>수락 게임 초대</b>: 새 게임 시작 <li><b>게임 초대 거절:</b> 게임이 시작 하지만 숙은 여전히 파티 하 고 후속 게임 초대를 받게 됩니다.
     <li><b style="background-color: #FFFF00"><b>타사 두고:</b> 게임 시작 및 숙 없음 파티에서 제거 됩니다.</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>사례 2: 없음</b>
    </td>
  </tr>
  <tr>
    <td>
타사 새 제목을 상태로 되지 않습니다.
멀티 플레이 모드에 자신의 파티 구성원 없이 David를 재생합니다.
David 파티입니다.
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>집에서 흐름 초대

<table>
  <tr>
    <td>
David가 <b>홈</b>, 검색 및 숙 온라인 상태 인지 느껴질 그 <b>친구</b> 목록.
    <p><br>
David 숙을 그 자에 게 초대를 선택 합니다. 알림 초대를 보내 나타내는 팝업 됩니다. 타사 앱 David에 대 한 스냅 됩니다.
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
숙 수락 타사 앱이 맞춥니다.                                                                         </td>
  </tr>
  <tr>
    <td>
알림 숙 당사자에 연결 하 고 있는지를 나타내는 팝업 됩니다.
    <p><br>
David 및 숙 재생 하려는 게임에 설명 합니다. David 게임을 시작 하 고 멀티 플레이 게임 모드를 시작 합니다.
    <p><br>
게임, 친구 또는 자동 끌어오기를 초대 하는 옵션을 파티 구성원 제공 하거나 합니다.
    <p><br>
    <b style="background-color: #FFFF00">알림 게임 초대 집합이 전송 되었음을 나타내는 팝업 됩니다.</b>
    </td>
    <td>
알림 자는 게임에서 발견 했음을 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 숙 다음을 수행할 수 있습니다. <ul>
    <li>   <b>게임 초대를 수락:</b> 게임 시작 <li>   <b>게임 초대 거절:</b> 게임 실행 수 없음, 숙은 파티에서 여전히 이며 후속 초대를 받게 됩니다.
    <li>   <b style="background-color: #FFFF00">타사 유지: 게임이 시작 숙 파티에서 제거 됩니다.</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>게임 초대 전송 알림에 대 한 자세한 정보

**게임 초대 전송** 알림만 원격 대상 멤버를 사용 하 여 게임 세션 설정 된 처음으로 나타납니다. 원격 대상 구성원에 게 전송 하는 후속 요청이 알림이 메시지를 생성 하지 않습니다. 이렇게 하면 제목은 **PullReservedPlayersAsync**를 여러 번 호출 하면 반복 된 **게임 초대 전송** 토스트와 스팸 화 되에서 사용자를 않습니다.

**참고** **PullReservedPlayersAsync** 를 한 번만 호출 하 고으로 예약 됨, 친구를 원하는 모든 추가 하는 것이 좋습니다.
