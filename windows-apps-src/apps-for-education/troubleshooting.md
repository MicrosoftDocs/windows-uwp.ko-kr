---
description: Microsoft에서 이벤트 뷰어를 사용 하 여 테스트 이벤트 및 오류를 해결 합니다.
title: Microsoft에서 이벤트 뷰어를 사용 하 여 테스트를 수행 합니다.
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, 교육
ms.localizationpriority: medium
ms.openlocfilehash: cd30d54f1bff5fd43fbeb6e286e327fed9f8a585
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031486"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Microsoft의 이벤트 뷰어로 테스트 수행 문제 해결

이벤트 뷰어를 사용 하 여 테스트 이벤트와 오류를 볼 수 있습니다. 잠금 요청을 수신 하 고, 장치 등록에 성공 하 고, 잠금 정책이 성공적으로 적용 된 경우 테스트 로그 이벤트를 가져옵니다.

이벤트 뷰어에서 이벤트를 볼 수 있도록 설정 하려면 다음을 수행 합니다.
1. 을 엽니다. `Event Viewer`
2. `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment` 로 이동합니다.
3. 마우스 오른쪽 단추 `Operational` 를 클릭 하 고 `Enable Log`

이벤트 로그를 저장 하려면:
1. 마우스 오른쪽 단추 클릭 `Operational`
2. `Save All Events As…`을 클릭합니다.
