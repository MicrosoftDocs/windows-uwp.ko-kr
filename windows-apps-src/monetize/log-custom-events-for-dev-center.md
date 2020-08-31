---
Description: UWP 앱에서 사용자 지정 이벤트를 기록 하 고 파트너 센터의 사용량 보고서에서 해당 이벤트를 검토할 수 있습니다.
title: 파트너 센터의 사용자 지정 이벤트 로깅
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, 로그 이벤트
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: ec4bee888d055b5331252e91bfd979d81b976f3c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158437"
---
# <a name="log-custom-events-for-partner-center"></a>파트너 센터의 사용자 지정 이벤트 로깅

파트너 센터의 [사용량 보고서](../publish/usage-report.md) 를 사용 하 여 UWP (유니버설 Windows 플랫폼) 앱에서 정의한 사용자 지정 이벤트에 대 한 정보를 얻을 수 있습니다. 사용자 지정 이벤트는 앱의 이벤트 또는 활동을 나타내는 임의의 문자열입니다. 예를 들어 게임에서 사용자가 게임의 각 수준을 전달할 때 기록 되는 *Firstlevelpassed*, *secondLevelPassed*등의 사용자 지정 이벤트를 정의할 수 있습니다.

앱에서 사용자 지정 이벤트를 기록 하려면 Microsoft Store Services SDK에서 제공 하는 [log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 메서드에 사용자 지정 이벤트 문자열을 전달 합니다. 파트너 센터의 [사용량 보고서](../publish/usage-report.md) 의 **사용자 지정 이벤트** 섹션에서 사용자 지정 이벤트에 대 한 총 발생 횟수를 검토할 수 있습니다.

> [!NOTE]
> 파트너 센터에 로그인 하는 사용자 지정 이벤트는 [Windows 이벤트](/windows/desktop/Events/windows-events)와 관련이 없으며 **이벤트 뷰어**에 표시 되지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소

파트너 센터에서 앱에 대 한 **사용 현황 보고서** 의 사용자 지정 로깅 이벤트를 검토 하려면 먼저 스토어에 앱을 게시 해야 합니다.

## <a name="how-to-log-custom-events"></a>사용자 지정 이벤트를 기록 하는 방법

1. 아직 수행 하지 않은 경우 개발 컴퓨터에 [Microsoft Store 서비스 SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk) 합니다.

2. Visual Studio에서 프로젝트를 엽니다.

3. 솔루션 탐색기에서 프로젝트에 대 한 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 클릭 합니다.

4. **참조 관리자**에서 **유니버설 Windows** 를 확장 하 고 **확장**을 클릭 합니다.

5. Sdk 목록에서 **Microsoft Engagement 프레임 워크** 옆의 확인란을 클릭 하 고 **확인**을 클릭 합니다.

6. 사용자 지정 이벤트를 기록 하려는 각 코드 파일의 맨 위에 다음 문을 추가 합니다.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. 사용자 지정 이벤트를 기록 하려는 코드의 각 섹션에서 [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 개체를 가져온 다음 [log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 메서드를 호출 합니다. 사용자 지정 이벤트 문자열을 메서드에 전달 합니다.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > 앱에서 긴 이름으로 많은 사용자 지정 이벤트를 기록 하는 경우 [사용량 보고서](../publish/usage-report.md) 를 로드 하는 데 시간이 오래 걸릴 수 있습니다. 사용자 지정 이벤트에 대해 간단한 이름을 사용 하는 것이 좋습니다. 

## <a name="related-topics"></a>관련 항목

* [사용 보고서](../publish/usage-report.md)
* [Log 메서드](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)