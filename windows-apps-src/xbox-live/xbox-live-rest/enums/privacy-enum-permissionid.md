---
title: PermissionId 열거형
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
author: KevinAsgari
description: " PermissionId 열거형"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58c2d0f68e1f65820104928e45a09ccfdb259cb
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4530826"
---
# <a name="permissionid-enumeration"></a>PermissionId 열거형
PermissionId 열거형에 자세히 설명합니다.
사용 권한 유효성 검사 Url과 권한 Id는 사용할 수 있습니다.

   * [GET (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

이러한 Id는 대상 또는 단일 권한 행위자의 단일 개인 정보 설정을 확인 하는 등의 사용자에 대 한 특정 설정에 대 한 직접 검사를 포함 합니다. 또한 권한 사용 권한 API 사용 하 여 사용할 수 있으며 특정 사용자 작업에 대 한 여러 설정에 대 한 검사를 통합 하는 Id 있습니다.

<a id="ID4EIB"></a>


## <a name="permissions"></a>사용 권한

이들은 호출자는 특정 작업을 수행할 수 있는지 여부를 확인 하는 데 사용할 수 있는 값입니다. 위의 설정 달리 이러한 캡슐화 정책 서비스에 의해 정의 하 고 대부분의 경우, 정책을 값을 가진 사용자가 정의 된 하나 이상의 설정을 기반으로 빌드 하지만, 사용자가 직접 변경할 수 없습니다. 다음은 위에 정의 된 여러 설정에 대 한 일반적으로 복합 검사입니다. 예: <b>ViewProfile</b> 권한을 대상의 <b>ShareProfile</b> 개인 정보 설정 및 요청자의 <b>AllowProfileViewing</b> 권한 검사를 수행합니다.

일반적으로 호출자에 게 직접 개인 정보 설정 및 권한을 확인 하는 대신 확인 하는 작업에 대 한 권한 ID를 요청 하는 것이 좋습니다. 따라서 개인 정보 취급 방침에 통합 되어 새로운 검사에 따라 서비스에서 일관 되 게 변경할 수 있습니다.

| 사용 권한 이름| 설명|
| --- | --- |
| CommunicateUsingText| 사용자 대상 사용자에 게 텍스트 콘텐츠가 포함 된 메시지를 보낼 수 있는지 여부를 확인 합니다.|
| CommunicateUsingVideo| 사용자가 비디오를 사용 하 여 대상 사용자와 통신할 수 있는지 여부 확인|
| CommunicateUsingVoice| 사용자가 음성을 사용 하 여 대상 사용자와 통신할 수 있는지 여부 확인|
| ViewTargetProfile| 사용자가 대상 사용자의 프로필을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetGameHistory| 사용자는 대상 사용자의 게임 내역을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetVideoHistory| 사용자가 대상 사용자의 자세한 비디오 시청 기록을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetMusicHistory| 사용자가 대상 사용자의 자세한 음악 듣기 기록을 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetExerciseInfo| 사용자는 대상 사용자의 연습 정보를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetPresence| 사용자는 대상 사용자의 온라인 상태를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetVideoStatus| 사용자 대상 비디오 상태 (확장 된 온라인 상태)의 세부 정보를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetMusicStatus| 사용자 대상 음악 상태 (확장 된 온라인 상태)의 세부 정보를 볼 수 있는지 여부를 확인 합니다.|
| ViewTargetUserCreatedContent| 사용자가 다른 사용자의 사용자가 만든 콘텐츠를 볼 수 있는지 여부를 확인 합니다.|
