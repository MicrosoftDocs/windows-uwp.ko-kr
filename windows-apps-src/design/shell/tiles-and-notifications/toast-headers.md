---
Description: 관리 센터에서 알림 메시지를 사용 하 여 시각적으로 그룹화 할 헤더를 사용 하는 방법에 알아봅니다.
title: 알림 헤더
label: Toast headers
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, uwp, 알림, 헤더, 알림 헤더, 알림, 그룹 알림, 알림 센터
ms.localizationpriority: medium
ms.openlocfilehash: c7d1e3ce0a012d36bea671f87efb8df3a5d49b5f
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714089"
---
# <a name="toast-headers"></a>알림 헤더

알림에서 알림 헤더를 사용하여 알림 센터 내부의 관련 알림 세트를 시각적으로 그룹화할 수 있습니다.

> [!IMPORTANT]
> **필요한 데스크톱 크리에이터 업데이트 및 알림 라이브러리의 1.4.0**: 데스크톱 빌드 15063 이상 알림 메시지를 보려면 머리글을 실행 해야 합니다. 버전 1.4.0 이상의 [UWP 커뮤니티 도구 키트 알림 NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용해야만 알림의 콘텐츠에 대한 헤더를 할당할 수 있습니다. 데스크톱에서 헤더만이 지원됩니다.

아래와 같이 이 그룹 대화는 단일 헤더 'Camping!!'에서 통합됩니다. 대화의 각 개별 메시지는 동일한 토스트 헤더를 공유하는 별도의 알림 메시지입니다.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

또한 항공편 미리 알림, 패키지 추적 등과 같은 범주별로 알림을 시각적으로 그룹화할 수도 있습니다.

## <a name="add-a-header-to-a-toast"></a>알림에 헤더 추가

다음은 알림 메시지에 헤더를 추가하는 방법입니다.

> [!NOTE]
> 데스크톱에서 헤더만이 지원됩니다. 헤더를 지원하지 않는 디바이스는 간단히 헤더를 무시합니다.

```csharp
ToastContent toastContent = new ToastContent()
{
    Header = new ToastHeader()
    {
        Id = "6289",
        Title = "Camping!!",
        Arguments = "action=openConversation&id=6289",
    },

    Visual = new ToastVisual() { ... }
};
```

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

요약하면 다음과 같습니다.

1. **ToastContent**에 **헤더** 추가
2. 필요한 **Id**, **제목**, **인수** 속성 할당
3. 알림 보내기([자세히 알아보기](send-local-toast.md))
4. 다른 알림에서 동일한 머리글 **Id**을 사용하여 헤더에서 통일합니다. **Id**은 알림이 그룹화되어야 하는지를 결정하는 데 사용되는 유일한 속성입니다. 즉 **제목**과 **인수**는 다를 수 있습니다. 그룹에서 최근 알림의**제목** 및 **인수**를 사용합니다. 해당 알림이 제거되면 **제목** 및 **인수**는 다음 가장 최근 알림으로 대체됩니다.


## <a name="handle-activation-from-a-header"></a>헤더에서 활성화 처리

헤더는 사용자가 클릭할 수 있으므로, 사용자가 헤더를 클릭하면 앱에서 더 많은 정보를 찾을 수 있습니다.

따라서 앱은 알림 자체의 시작 인수와 마찬가지로 헤더에서 **Arguments**을 제공할 수 있습니다.

활성화는 [일반 알림 활성화](send-local-toast.md#handling-activation-1)과 동일하게 처리됩니다. 즉, `App.xaml.cs`의 **OnActivated** 메서드에서 이러한 인수를 검색할 수 있으며, 이것은 사용자가 알림 본문이나 알림의 단추를 클릭할 때의 작업과 같습니다.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>추가 정보

헤더는 시각적으로 알림을 구분하고 그룹화합니다. 앱이 가질 수 있는 최대 알림 개수(20개) 및 알림 목록의 FIFO(선입 선출) 동작에 대한 다른 내부 프로세스는 변경하지 않습니다.

헤더 내 알림 순서는 아래와 같습니다... 지정된 된 앱에 대 한 앱 (및 전체 헤더 그룹 헤더의 일부인 경우)에서 가장 최근의 알림이 먼저 표시 됩니다.

**Id**는 선택한 문자열일 수 있습니다. **ToastHeader**의 속성에는 길이 또는 문자 제한이 없습니다. 유일한 제약 조건은 전체 XML 알림 콘텐츠가 5KB를 초과할 수 없다는 것입니다.

헤더를 만들면 '자세한 내용' 단추가 표시되기 전에 알림 센터에 표시되는 알림 개수가 변경되지 않습니다. 이 개수는 기본적으로 3개이며 알림용 시스템 설정에서 각 앱에별로 사용자가 구성할 수 있습니다.

앱 제목을 클릭하는 것처럼 헤더를 클릭해도 이 헤더에 속한 알림이 삭제되지 않습니다(앱에서 관련 알림을 지우려면 알림 API 사용).


## <a name="related-topics"></a>관련 항목

- [로컬 알림 및 핸들 활성화 보내기](send-local-toast.md)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
