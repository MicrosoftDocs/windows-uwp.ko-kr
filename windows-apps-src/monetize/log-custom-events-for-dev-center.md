---
author: mcleanbyron
Description: "UWP 앱에서 사용자 지정 이벤트를 기록하고 해당 이벤트를 Windows 개발자 센터 대시보드의 사용 보고서에서 검토할 수 있습니다."
title: "개발자 센터에 대한 사용자 지정 이벤트 로깅"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: 61874c700ecd31c7246effef5b05ffbf1153dfd5

---

# 개발자 센터에 대한 사용자 지정 이벤트 로깅

Windows 개발자 센터 대시보드에서 [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report)를 사용하면 UWP(유니버설 Windows 플랫폼) 앱에서 정의한 사용자 지정 이벤트에 대한 정보를 얻을 수 있습니다. 사용자 지정 이벤트는 앱의 이벤트 또는 활동을 나타내는 임의의 문자열입니다. 예를 들어, 게임에서 *firstLevelPassed*, *secondLevelPassed* 등과 같이 사용자 지정 이벤트를 정의할 수 있어 사용자가 게임 단계를 통과할 때마다 기록됩니다.

앱에서 사용자 지정 이벤트를 기록하려면 Microsoft Store Services SDK에서 제공한 [로그](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 메서드에 사용자 지정 이벤트 문자열을 전달합니다. 사용자 지정 이벤트에 대한 총 발생 횟수는 개발자 센터 대시보드에서 [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report)의 **사용자 지정 이벤트** 섹션에서 검토할 수 있습니다.

## 필수 조건

대시보드에서 앱에 대한 **사용 보고서**에서 사용자 지정 로깅 이벤트를 검토하려면 먼저 스토어에 앱을 게시해야 합니다.

## 사용자 지정 이벤트를 기록하는 방법

1. 아직 수행하지 않은 경우에는 관리 컴퓨터에 [Microsoft Store Services SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk)하세요. 사용자 지정 이벤트 기록을 위한 API 외에, 이 SDK는 A/B 테스트로 앱에서 실험 실행, 광고 표시 등의 다른 기능을 위한 API도 제공합니다. 
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
4. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
5. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 클릭하고 **확인**을 클릭합니다.
7. 사용자 지정 이벤트를 기록할 각 코드 파일의 맨 위에 다음 문을 추가합니다.

  ```csharp
  using Microsoft.Services.Store.Engagement;
  ```
8. 사용자 지정 이벤트를 기록할 각 코드 섹션에서 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 개체를 가져온 다음 [로그](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 메서드를 호출합니다. 사용자 지정 이벤트 문자열을 메서드로 전달합니다.

  ```csharp
  StoreServicesCustomEventLogger logger = StoreServicesCustomEventLogger.GetDefault();
  logger.Log("myCustomEvent");
  ```

## 관련 항목

* [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [로그 메서드](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Nov16_HO1-->

