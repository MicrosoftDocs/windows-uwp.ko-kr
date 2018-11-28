---
Description: You can log custom events from your UWP app and review those events in the Usage report in Partner Center.
title: 파트너 센터에 대해 사용자 지정 이벤트 로깅
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, 이벤트 기록
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: d7b338fd3b34d530ad365b0377d6b6c6c65398b7
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7841579"
---
# <a name="log-custom-events-for-partner-center"></a>파트너 센터에 대해 사용자 지정 이벤트 로깅

파트너 센터에서 [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report) 에 유니버설 Windows 플랫폼 (UWP) 앱에서 정의한 사용자 지정 이벤트에 대 한 정보를 얻을 수 있습니다. 사용자 지정 이벤트는 앱의 이벤트 또는 활동을 나타내는 임의의 문자열입니다. 예를 들어, 게임에서 *firstLevelPassed*, *secondLevelPassed* 등과 같이 사용자 지정 이벤트를 정의할 수 있어 사용자가 게임 단계를 통과할 때마다 기록됩니다.

앱에서 사용자 지정 이벤트를 기록하려면 Microsoft Store Services SDK에서 제공한 [로그](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 메서드에 사용자 지정 이벤트 문자열을 전달합니다. 파트너 센터에서 [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report) 의 **사용자 지정 이벤트** 섹션에서 사용자 지정 이벤트에 대 한 총 발생 횟수를 검토할 수 있습니다.

> [!NOTE]
> 파트너 센터에 로그온 하는 사용자 지정 이벤트 [Windows 이벤트](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx)와 관련 되지 않으며 **이벤트**뷰어에서 표시 되지 않습니다.

## <a name="prerequisites"></a>필수 조건

파트너 센터에서 앱에 대 한 **사용 보고서** 에서 사용자 지정 로깅 이벤트를 검토 하려면, 먼저 앱 스토어에 게시 해야 합니다.

## <a name="how-to-log-custom-events"></a>사용자 지정 이벤트를 기록하는 방법

1. 아직 수행하지 않은 경우에는 개발 컴퓨터에 [Microsoft Store Services SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk)하세요.

2. Visual Studio에서 프로젝트를 엽니다.

3. 솔루션 탐색기에서 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.

4. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.

5. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 클릭하고 **확인**을 클릭합니다.

6. 사용자 지정 이벤트를 기록할 각 코드 파일의 맨 위에 다음 문을 추가합니다.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. 사용자 지정 이벤트를 기록할 각 코드 섹션에서 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 개체를 가져온 다음 [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 메서드를 호출합니다. 사용자 지정 이벤트 문자열을 메서드에 전달합니다.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > 앱 로그에 이름이 긴 사용자 지정 이벤트가 많을 경우 [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report)를 로드하는 데 시간이 오래 걸릴 수 있습니다. 사용자 지정 이벤트에 간략한 이름을 사용하는 것이 좋습니다. 

## <a name="related-topics"></a>관련 항목

* [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [로그 메서드](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
