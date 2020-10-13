---
title: 알림을의 사용자 지정 타임 스탬프
description: 메시지/정보/내용이 생성 된 시기를 나타내는 사용자 지정 타임 스탬프를 사용 하 여 알림 메시지의 기본 타임 스탬프를 재정의 하는 방법에 대해 알아봅니다.
label: Custom timestamps on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, 알림, 사용자 지정 타임 스탬프, 타임 스탬프, 알림, 동작 센터
ms.localizationpriority: medium
ms.openlocfilehash: 23e5337fe2ce30e1172aedc034a8b4eb3821a615
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984429"
---
# <a name="custom-timestamps-on-toasts"></a>알림을의 사용자 지정 타임 스탬프

기본적으로 알림 메시지 (알림 센터 내에 표시 됨)에 대 한 타임 스탬프는 알림이 전송 된 시간으로 설정 됩니다.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

필요에 따라 사용자 지정 날짜 및 시간으로 타임 스탬프를 재정의 하 여 타임 스탬프는 알림이 전송 된 시간이 아닌 메시지/정보/콘텐츠를 실제로 만든 시간을 나타낼 수 있습니다. 또한 알림이 작업 센터 내에서 올바른 순서 대로 표시 되도록 합니다 (시간별로 정렬 됨). 대부분의 앱은 사용자 지정 타임 스탬프를 지정 하는 것이 좋습니다.

> [!IMPORTANT]
> **작성자 업데이트 및 알림 라이브러리의 1.4.0 필요**합니다. 사용자 지정 타임 스탬프를 보려면 빌드 15063 이상을 실행 해야 합니다. 1.4.0 이상의 [UWP 커뮤니티 도구 키트 알림 NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 를 사용 하 여 알림 콘텐츠에 타임 스탬프를 할당 해야 합니다.

사용자 지정 타임 스탬프를 사용 하려면 **Toa 콘텐츠에서** **displaytimestamp** 속성을 할당 하면 됩니다.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
var content = new ToastContent()
    .AddCustomTimeStamp(new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc))
    ...
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

---

XML을 사용 하는 경우 날짜는 [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)형식 이어야 합니다.

> [!NOTE]
> 초에 최대 3 개의 소수 자릿수를 사용할 수 있습니다 (구체적으로 세부적인 값을 제공 하는 것은 아니지만). 더 많은 기능을 제공 하는 경우 페이로드가 유효 하지 않게 되 고 "새 알림" 알림이 표시 됩니다.


## <a name="usage-guidance"></a>사용 지침

일반적으로 대부분의 앱에서 사용자 지정 타임 스탬프를 지정 하는 것이 좋습니다. 이렇게 하면 네트워크 지연, 비행기 모드 또는 정기적인 백그라운드 작업의 고정 된 간격에 관계 없이 메시지/정보/내용이 생성 될 때 알림의 타임 스탬프를 정확 하 게 나타냅니다.

예를 들어 뉴스 앱은 15 분 마다 새 문서를 확인 하 고 알림을 표시 하는 백그라운드 작업을 실행할 수 있습니다. 사용자 지정 타임 스탬프 전에 타임 스탬프는 알림 메시지가 생성 될 때 (따라서 항상 15 분 간격으로) 속하는지를 됩니다. 그러나 이제 앱은 아티클이 실제로 게시 된 시간으로 타임 스탬프를 설정할 수 있습니다. 마찬가지로 전자 메일 앱과 소셜 네트워크 앱은 알림에 대해 비슷한 패턴의 주기적인 가져오기를 사용 하는 경우이 기능을 활용할 수 있습니다.

또한 사용자 지정 타임 스탬프를 제공 하면 사용자가 인터넷에서 연결이 끊어진 경우에도 타임 스탬프가 정확 하 게 유지 됩니다. 예를 들어 사용자가 컴퓨터를 켜고 백그라운드 작업이 실행 될 때 마지막으로 알림의 타임 스탬프가 사용자 컴퓨터에서 사용자가 설정한 시간이 아닌 메시지를 보낸 시간을 나타내도록 할 수 있습니다.


## <a name="default-timestamp"></a>기본 타임 스탬프

사용자 지정 타임 스탬프를 제공 하지 않으면 알림이 전송 된 시간을 사용 합니다.

WNS를 통해 푸시 알림을 보낸 경우 WNS 서버에서 알림을 수신 하는 시간을 사용 합니다. 따라서 장치에 알림을 배달 하는 데 걸리는 대기 시간은 타임 스탬프에 영향을 주지 않습니다.

로컬 알림을 보낸 경우 알림 플랫폼이 알림을 받은 시간을 사용 합니다 (즉시 해야 함).


## <a name="related-topics"></a>관련 항목

- [로컬 알림 보내기](send-local-toast.md)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)