---
Description: 이벤트 뷰어를 사용하여 Microsoft 시험 응시 이벤트 및 오류 문제 해결
title: 이벤트 뷰어를 사용하여 Microsoft 시험 응시 문제 해결
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, 교육
ms.localizationpriority: medium
ms.openlocfilehash: 2f4bdcf45c7dd37dd540a666d99b5fa2fd2d49f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598478"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>이벤트 뷰어를 사용하여 Microsoft 시험 응시 문제 해결

이벤트 뷰어를 사용하여 시험 응시 이벤트 및 오류를 볼 수 있습니다. 잠금 요청이 수신되고, 디바이스가 등록되고, 잠금 정책이 성공적으로 적용되는 등의 경우에 시험 응시에서 이벤트를 기록합니다.

이벤트 뷰어에서 이벤트를 볼 수 있도록 설정하려면
1. 열기는 `Event Viewer`
2. 로 이동 `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. 마우스 오른쪽 단추로 클릭 `Operational` 선택 `Enable Log`

이벤트 로그를 저장하려면
1. 마우스 오른쪽 단추로 클릭 `Operational`
2. 클릭 `Save All Events As…`
