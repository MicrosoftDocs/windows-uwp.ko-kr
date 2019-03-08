---
title: 멀티 플레이 게임 초대에 대 한 업데이트 된 흐름
description: Xbox Live 초대 하는 멀티 플레이 게임을 구현 하는 것에 대 한 업데이트 된 흐름을 제공 합니다.
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 멀티 플레이 2015, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1092c84521271e996db0b89630d22f7e51727bfa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596888"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>멀티 플레이 게임 초대에 대 한 업데이트 된 흐름

Xbox One 베타 피드백의 결과로 사용자 경험 흐름 멀티 플레이 게임 초대에 대 한 바뀌었습니다 Xbox 하나의 복구 업데이트 24, 2013 년 11 월 6 일에 릴리스된 기준으로 합니다. 이 변경 내용에 **사용자 환경 (UX)만** 게임 타이틀의 관점에서 기능이 나 동작에 영향을 주지 것입니다. 제목 개발자가 코드를 변경 하려면 않아도 됩니다.

## <a name="summary-of-changes"></a>변경 내용 요약

-   초기 초대 알림에서 "내 파티 조인"으로 바뀌었습니다 "&lt;제목 게임&gt; 하시 죠" 및 사용자가 게임을 시작 하 고 게임 플레이를 직접 이동할 수 있는 단추를 얻었습니다.

-   타사 앱은 기본 뷰가 아닌 기본적으로 사용자가 새 "&lt;title 게임&gt; 하시 죠" 옵션입니다. 사용자는 게임 플레이를 오른쪽을 이동할 수 있도록 변경도 됩니다.

-   보낸 사람 쪽에서 추가한 새 알림 "추가 \[ *수* \] 게임에는 친구" 합니다. 이렇게 하면 명확 하 게 게임 세션이 사용자의 파티와 연결 된 경우 초대 전송 되었습니다.

다음 예제에서는 자세한 사용자 경험 흐름을 설명 합니다. 각 테이블에는 두 사용자, David Laura에 대 한 예제 흐름을 보여 줍니다. 이러한 흐름 각 열에 표시 되 고 병렬로 발생 합니다. 합니다 <b style="background-color: #FFFF00">텍스트를 강조 표시 된</b> 이전 UX 흐름에서 적용 된 조정 작업을 보여 줍니다.

## <a name="inviting-users-from-within-a-game"></a>게임 내에서 사용자를 초대

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
David는 게임 실행은 그에 대 한 멀티 플레이 로비입니다.<p><br>David 선택 <b>친구 초대</b>합니다.<p><br>David Laura를 선택합니다.<p><br>알림 메시지는 초대를 보냈음을 나타내는 팝업 됩니다. | Laura에서 다른 게임을 재생 합니다.
    </td>
    <td style="border-bottom:solid 1px #fff">
Laura에서 다른 게임을 재생 합니다.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
알림, David에서 초대를 나타내는 팝업 하 고 <b style="background-color: #FFFF00">게임 이름 및 아이콘 표시</b>합니다. (파티 앱은 하지 자동 맞춤.) <p><br>
알림 센터에서 <b style="background-color: #FFFF00">Laura 시작을 선택할 수 있으며 초대를 수락</b>를 <b>초대를 수락</b>, 또는 <b style="background-color: #FFFF00">거부 초대</b>합니다.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">사례 1: Laura 시작을 선택 하 고 초대를 수락</b> (이 새로운 옵션)</td>
  </tr>
  <tr>
    <td>
알림은 Laura David의 파티에 연결 하 고 있는지를 나타내는 팝업 됩니다.             
    <p><br>
David 멀티 플레이 로비에서 게임을 시작합니다.                              
    <p><br>
    <b style="background-color: #FFFF00">알림은 Laura에 게임 초대를 보냈음을 나타내는 팝업 됩니다.</b>
    </td>
    <td>
게임 시작 하 고 타사 앱 스냅 되지 않습니다.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>사례 2: Laura는 Accept 초대를 선택합니다.</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">Laura 파티를 조인합니다.</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">알림은 Laura에 게임 초대를 보냈음을 나타내는 팝업 됩니다.</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>알림 메시지는 파티에 대 한 게임을 찾을 수 있는지를 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 Laura 선택할 수 있습니다. <ul>
    <li>   <b>게임 초대를 수락 합니다.</b> 게임 시작 합니다.
    <li>   <b>게임 초대를 거부 합니다.</b> 게임이 없는 시작합니다. Laura는 당사자 이며 후속 게임 초대를 받게 됩니다.         
    <li>   <b style="background-color: #FFFF00">파티를 그대로 둡니다. 게임이 없는 시작합니다. Laura는 파티에서 제거 됩니다.</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>게임 내 초대 흐름: 파티를 및 제목 전환

<table>
  <tr>
    <td>
게임을 함께 실행 합니다.
    <p><br>
David 다른 게임으로 전환 하 고 멀티 플레이 메뉴로 이동 합니다.
     <p><br>
Xbox 시스템 UI David 그 대답할 수 있는 새 게임을 자신의 파티 전환 하려고 하는 경우 요청 <b>Yes</b> 또는 <b>No</b>합니다.
    </td>
    <td style="text-align:top">
게임을 함께 실행 합니다.
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>사례 1: 예</b>
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
당사자는 새 제목을 제공 됩니다.
    <p><br>
멀티 플레이 로비에서 David 게임을 시작합니다.
    <p><br>
    <b style="background-color: #FFFF00">알림 메시지는 게임 초대를 보낸 Laura 나타내는 팝업 됩니다.
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>알림 메시지는 파티에 대 한 게임을 찾을 수 있는지를 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서 Laura 선택할 수 있습니다. <ul>
     <li><b>게임 초대를 수락</b>: 새로운 게임 출시 <li><b>게임 초대를 거부 합니다.</b> 게임이 없는 시작 하지만 Laura 파티 되 고 후속 게임 초대를 받게 됩니다.
     <li><b style="background-color: #FFFF00"><b>파티를 그대로 둡니다.</b> 게임 출시 및 Laura 없는 파티에서 제거 됩니다.</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>사례 2: No</b>
    </td>
  </tr>
  <tr>
    <td>
당사자는 새 제목을 제공 되지 않습니다.
David가 파티 멤버 없이 멀티 플레이 모드에서 재생 됩니다.
David는 아직 파티입니다.
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>집에서 흐름을 초대

<table>
  <tr>
    <td>
David가 검색 하는 <b>홈</b>, 및 his <b>친구</b> Laura 온라인 상태임을 확인 목록입니다.
    <p><br>
David는 Laura 파티로 자신의 초대를 선택 합니다. 알림 메시지는 초대를 보내 나타내는 팝업 됩니다. 타사 앱은 David에 맞춰집니다.
    </td>
    <td>
Laura에서 게임을 재생 합니다.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
알림 메시지는 David 귀하를 초대 했습니다 Laura 자신의 파티를 나타내는 팝업 됩니다.
    <p><br>
알림 센터 Laura에 하는 옵션이 <b>초대를 수락</b>합니다.
    <p><br>
Laura 허용 하는 경우 타사 앱에 맞춥니다.                                                                         </td>
  </tr>
  <tr>
    <td>
알림은 Laura 파티에 연결 하 고 있는지를 나타내는 팝업 됩니다.
    <p><br>
David와 Laura 게임은 어떤 재생할 것인지를 설명 합니다. David는 게임을 시작 하 고 멀티 플레이 게임 모드를 시작 합니다.
    <p><br>
게임 친구 또는 자동 끌어오기를 초대 하는 옵션이 파티 멤버를 제공 하거나 합니다.
    <p><br>
    <b style="background-color: #FFFF00">알림 게임 초대를 보냈음을 나타내는 팝업 됩니다.</b>
    </td>
    <td>
알림 메시지는 파티에 대 한 게임 찾았습니다는 나타내는 팝업 됩니다.
    <p><br>
알림 센터에서에 Laura는 다음을 수행할 수 있습니다. <ul>
    <li>   <b>게임 초대를 수락 합니다.</b> 게임 시작 <li>   <b>게임 초대를 거부 합니다.</b> 없는 게임 출시 Laura는 파티에서 여전히 및 후속 초대를 받게 됩니다.
    <li>   <b style="background-color: #FFFF00">파티를 그대로 둡니다. 게임이 없는 시작, Laura 파티에서 제거 됩니다.</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>게임 보낸 초대 알림에 대 한 자세한 정보

합니다 **게임 초대 전송** 알림 원격측 멤버를 사용 하 여 게임 세션이 설정 될 첫 번째 시간에만 표시 됩니다. 원격 상대방 멤버에 게 전송 하는 후속 요청에서이 알림 메시지를 생성 하지 않습니다. 인해에서 사용자를 이렇게 사용 하 여 반복 **게임 보낸 초대** 제목에 여러 호출을 하는 경우 팝업 **PullReservedPlayersAsync**합니다.

**참고** 친구를 원하는 모든 예약을 대로 추가 하 고 다음 호출 하는 것이 좋습니다 **PullReservedPlayersAsync** 한 번만 합니다.
