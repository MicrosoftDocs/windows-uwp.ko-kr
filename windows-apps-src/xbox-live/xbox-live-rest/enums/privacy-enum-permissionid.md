---
title: PermissionId 열거형
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " PermissionId 열거형"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594688"
---
# <a name="permissionid-enumeration"></a>PermissionId 열거형
PermissionId 열거형을 자세히 설명합니다.
사용 권한 유효성 검사 Url을 사용 하 여 권한 Id는 사용할 수 있습니다.

   * [GET (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

이러한 Id를 대상 또는 단일 권한 행위자를 단일 개인 정보 설정을 확인 하는 등의 사용자에 대 한 특정 설정에 대 한 직접 검사를 포함 합니다. 또한 권한 사용 권한 API 사용 하 여 사용할 수 있으며 특정 사용자 작업에 대 한 여러 설정에 대 한 검사를 통합 하는 Id 이며

<a id="ID4EIB"></a>


## <a name="permissions"></a>권한

이러한 값은 호출자에 게 특정 작업을 수행할 수 있는지 여부를 확인 하는 데 사용할 수 있는 값입니다. 위의 설정을 이러한 서비스에 의해 정의 된 정책을 캡슐화과 달리 대부분의 경우에서 정책 값을 가진 사용자가 정의 된 하나 이상의 설정을 기반으로 구축 되지만 사용자가 직접 변경할 수 없습니다. 다음은 위에 정의 된 둘 이상의 설정에 대 한 일반적으로 복합 검사입니다. 예제: 합니다 <b>ViewProfile</b> 사용 권한에 대상의 검사 <b>ShareProfile</b> 개인 정보 설정 및 요청자 <b>AllowProfileViewing</b> 권한.

일반적으로 호출자에 게 직접 권한 및 개인 정보 설정을 확인 하는 대신 확인 해야 하는 작업에 대 한 사용 권한 ID를 요청 하는 것이 좋습니다. 이 개인 정보 보호 정책을으로 새 검사는 통합 서비스에서 일관 되 게 변경 될 수 있습니다.

| 권한 이름| 설명|
| --- | --- |
| CommunicateUsingText| 사용자 대상 사용자에 게 텍스트 콘텐츠를 사용 하 여 메시지를 보낼 수 있는지 여부를 확인 합니다.|
| CommunicateUsingVideo| 사용자 비디오를 사용 하 여 대상 사용자와 통신할 수 있는지 여부를 확인 합니다.|
| CommunicateUsingVoice| 사용자 음성을 사용 하 여 대상 사용자와 통신할 수 있는지 여부를 확인 합니다.|
| ViewTargetProfile| 사용자가 대상 사용자의 프로필을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetGameHistory| 사용자는 대상 사용자의 게임 기록을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetVideoHistory| 대상 사용자의 자세한 비디오 시청 기록을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetMusicHistory| 사용자는 대상 사용자의 자세한 음악 수신 기록을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetExerciseInfo| 사용자는 대상 사용자의 연습 정보를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetPresence| 사용자는 대상 사용자의 온라인 상태를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetVideoStatus| 대상 비디오 상태 (확장 된 온라인 상태)의 세부 정보를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetMusicStatus| 대상 음악 상태 (확장 된 온라인 상태)의 세부 정보를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetUserCreatedContent| 사용자가 다른 사용자의 사용자가 만든 콘텐츠를 볼 수 있는지 여부를 확인 합니다.|
