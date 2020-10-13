---
Description: 헤더를 사용 하 여 알림 메시지를 중앙에서 관리 하는 방법을 알아봅니다.
title: 알림 헤더
label: Toast headers
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: windows 10, uwp, 알림, 헤더, 알림 헤더, 알림, 그룹 알림을, 동작 센터
ms.localizationpriority: medium
ms.openlocfilehash: 95cd6083cf4430f25b1514a7e163d04892097903
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984479"
---
# <a name="toast-headers"></a>알림 헤더

알림에서 알림 헤더를 사용 하 여 관리 센터 내에서 관련 알림 집합을 시각적으로 그룹화 할 수 있습니다.

> [!IMPORTANT]
> **바탕 화면 작성자 업데이트 및 알림 라이브러리 1.4.0 필요**: 알림 헤더를 보려면 데스크톱 빌드 15063 이상을 실행 해야 합니다. 1.4.0 버전 이상의 [UWP Community Toolkit Notification NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 를 사용 하 여 알림 콘텐츠에서 헤더를 생성 해야 합니다. 헤더는 데스크톱 에서만 지원 됩니다.

아래 표시 된 것 처럼이 그룹 대화는 단일 헤더 "캠핑!!"로 통합 됩니다. 대화의 각 개별 메시지는 동일한 알림 헤더를 공유 하는 별도의 알림 메시지입니다.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

또한 비행 미리 알림, 패키지 추적 등과 같이 범주별로 알림을 시각적으로 그룹화 하도록 선택할 수도 있습니다.

## <a name="add-a-header-to-a-toast"></a>알림 메시지에 헤더 추가

알림 메시지에 헤더를 추가 하는 방법은 다음과 같습니다.

> [!NOTE]
> 헤더는 데스크톱 에서만 지원 됩니다. 헤더를 지원 하지 않는 장치는 단지 헤더를 무시 합니다.

#### <a name="builder-syntax"></a>[작성기 구문](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddHeader("6289", "Camping!!", "action=openConversation&id=6289")
    .AddText("Anyone have a sleeping bag I can borrow?");
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---

요약 ...

1. **Toa 내용** 에 **헤더** 추가
2. 필요한 **Id**, **제목**및 **인수** 속성을 할당 합니다.
3. 알림 보내기 ([자세한 정보](send-local-toast.md))
4. 다른 알림에서 동일한 헤더 **Id** 를 사용 하 여 헤더에서 통합 합니다. **Id** 는 알림을 그룹화 해야 하는지 여부를 결정 하는 데 사용 되는 유일한 속성입니다. 즉, **제목** 및 **인수** 는 다를 수 있습니다. 그룹 내의 가장 최근 알림의 **제목과** **인수가** 사용 됩니다. 해당 알림이 제거 되 면 **제목** 및 **인수** 는 다음으로 가장 최근의 알림으로 돌아옵니다.


## <a name="handle-activation-from-a-header"></a>헤더에서 활성화 처리

사용자가 헤더를 클릭 하 여 앱에서 더 많은 정보를 찾을 수 있도록 사용자가 머리글을 클릭할 수 있습니다.

따라서 앱은 알림 자체의 시작 인수와 마찬가지로 헤더에 **인수** 를 제공할 수 있습니다.

활성화는 [일반적인 알림 활성화](send-local-toast.md#step-4-handling-activation)와 동일 하 게 처리 됩니다. 즉, 사용자가 알림 본문을 클릭 하거나 알림 단추를 클릭 하는 것 처럼 **onactivated** 된 메서드에서 이러한 인수를 검색할 수 있습니다 `App.xaml.cs` .

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

헤더는 알림을 시각적으로 구분 하 고 그룹화 합니다. 앱에 포함할 수 있는 최대 알림 수 (20)와 알림 목록의 첫 번째-기본-아웃 동작에 대 한 다른 물류는 변경 되지 않습니다.

헤더 내의 알림 순서는 다음과 같습니다. 지정 된 앱의 경우 앱에서 가장 최근의 알림과 헤더의 일부인 경우 전체 헤더 그룹이 먼저 표시 됩니다.

**Id** 는 선택한 모든 문자열일 수 있습니다. **ToastHeader**의 속성에는 길이 또는 문자 제한이 없습니다. 유일한 제약 조건은 전체 XML 알림 콘텐츠가 5kb 보다 클 수 없다는 것입니다.

헤더를 만들면 "자세히 보기" 단추가 표시 되기 전에 작업 센터 내에 표시 되는 알림 수를 변경 하지 않습니다 .이 값은 기본적으로 3 이며 알림에 대 한 시스템 설정의 각 앱에 대해 사용자가 구성할 수 있습니다.

응용 프로그램 제목을 클릭 하는 것 처럼 헤더를 클릭 하면이 헤더에 속하는 알림이 제거 되지 않습니다. 앱에서 알림 Api를 사용 하 여 관련 알림을 지워야 합니다.


## <a name="related-topics"></a>관련 항목

- [로컬 알림 보내기 및 활성화 처리](send-local-toast.md)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
