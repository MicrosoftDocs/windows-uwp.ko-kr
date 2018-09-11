---
Description: Troubleshoot Microsoft Take a Test events and errors with the event viewer.
title: 이벤트 뷰어를 사용하여 Microsoft 시험 응시 문제 해결
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 교육
ms.localizationpriority: medium
ms.openlocfilehash: 3193525316d085e56244d6f03da99e3e07c6539f
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3846787"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>이벤트 뷰어를 사용하여 Microsoft 시험 응시 문제 해결

이벤트 뷰어를 사용하여 시험 응시 이벤트 및 오류를 볼 수 있습니다. 잠금 요청이 수신되고, 디바이스가 등록되고, 잠금 정책이 성공적으로 적용되는 등의 경우에 시험 응시에서 이벤트를 기록합니다.

이벤트 뷰어에서 이벤트를 볼 수 있도록 설정하려면
1. 다음을 엽니다. `Event Viewer`
2. 다음으로 이동합니다. `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. `Operational`를 마우스 오른쪽 단추로 클릭하고 다음을 선택합니다. `Enable Log`

이벤트 로그를 저장하려면
1. 다음을 마우스 오른쪽 단추로 클릭합니다. `Operational`
2. 다음을 클릭합니다. `Save All Events As…`
