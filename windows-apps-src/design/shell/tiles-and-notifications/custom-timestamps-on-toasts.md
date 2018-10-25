---
author: andrewleader
Description: Learn how to use custom timestamps on your toast notifications.
title: 알림에 대한 사용자 지정 타임스탬프
label: Custom timestamps on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 알림, 사용자 지정 타임스탬프, 타임스탬프, 알림, 알림 센터
ms.localizationpriority: medium
ms.openlocfilehash: 7ef01feaf422674977dc4549d4cc68a2ca0052c7
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5519167"
---
# <a name="custom-timestamps-on-toasts"></a>알림에 대한 사용자 지정 타임스탬프

기본적으로 알림 메시지에 대한 타임스탬프(알림 센터 내에 표시)는 알림을 보낸 시간으로 설정됩니다.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

선택적으로 사용자 지정 날짜 및 시간으로 타임스탬프를 재정의할 수 있으므로 타임스탬프는 알림이 전송된 시간이라기 보다는 메시지/정보/콘텐츠가 실제로 작성된 시간을 나타냅니다. 또한 이것은 알림 센터 내에서 알림이 올바른 순서(시간순 정렬)로 표시되도록 보장합니다. 대다수 앱에서 사용자 지정 타임스탬프를 지정하는 것이 좋습니다.

> [!IMPORTANT]
> **Creators Update 및 알림 라이브러리 1.4.0 필요**: 사용자 지정 타임스탬프를 표시하려면 빌드 15063 이상을 실행해야 합니다. 버전 1.4.0 이상의 [UWP 커뮤니티 도구 키트 알림 NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용해야만 알림의 콘텐츠에 대한 타임스탬프를 할당할 수 있습니다.

사용자 지정 타임스탬프를 사용하려면 간단히 **ToastContent**에서 **DisplayTimestamp** 속성을 할당합니다.

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

XML를 사용하는 경우 [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)에서 날짜 서식을 지정해야 합니다.

> [!NOTE]
> 초 단위로 소수점 이하 3자리까지만 사용할 수 있습니다(사실상 자세한 값을 제공하는 것이 유용하지 않음). 더 제공하면 페이로드는 무효화되고 '새 알림' 알림을 받습니다.


## <a name="usage-guidance"></a>사용법 지침

일반적으로 대다수 앱에서 사용자 지정 타임스탬프를 지정하는 것이 좋습니다. 이렇게 하면 네트워크 지연, 비행기 모드 또는 정기적인 백그라운드 작업 간격에 관계없이 메시지/정보/콘텐츠가 생성될 때 알림의 타임스탬프를 정확하게 나타낼 수 있습니다.

예를 들어 뉴스 앱은 새 기사를 확인하고 알림을 표시하는 15분마다 백그라운드 작업을 실행할 수 있습니다. 사용자 정의 타임스탬프 이전 타임스탬프는 알림 메시지가 생성된 시간에 해당합니다(즉, 항상 15분 간격). 그러나 이제 앱은 기사가 실제로 게시된 시간으로 타임스탬프를 설정할 수 있습니다. 마찬가지로, 알림에서 유사한 패턴으로 정기적으로 가져오는 방식을 사용할 경우 이메일 앱과 소셜 네트워크 앱에서도 이 기능을 활용할 수 있습니다.

또한 사용자 정의 타임스탬프를 제공하면 사용자가 인터넷에 연결되지 않은 경우에도 타임스탬프가 정확합니다. 예를 들어 사용자가 컴퓨터를 켜고 백그라운드 작업을 실행하면 알림에 대한 타임스탬프는 사용자가 컴퓨터를 켠 시간이 아니라 메시지를 보낸 시간을 정확하게 나타낼 수 있습니다.


## <a name="default-timestamp"></a>기본 타임스탬프

사용자 지정 타임스탬프를 제공하지 않으면 알림을 보낸 시간이 사용됩니다.

WNS를 통해 푸시 알림을 보낸 경우 WNS 서버에서 알림을 수신한 시간을 사용하므로, 알림을 디바이스로 전달할 때 발생하는 대기 시간은 타임스탬프에 영향을 주지 않습니다.

로컬 알림을 보낸 경우 알림 플랫폼이 알림을 받은 시간(즉각적이어야 함)이 사용됩니다.


## <a name="related-topics"></a>관련 항목

- [로컬 알림 보내기](send-local-toast.md)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)