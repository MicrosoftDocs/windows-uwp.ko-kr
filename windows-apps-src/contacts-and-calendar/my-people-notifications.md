---
title: 내 피플 알림
description: 새 유형의 알림으로 내 사용자 알림을 만들고 사용 하는 방법을 설명 합니다.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3e00e3de9445a8b7c63ebaead70173c29b637b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166327"
---
# <a name="my-people-notifications"></a>내 피플 알림

내 사용자 알림은 빠른 표현 제스처를 통해 사용자가 관심 있는 사용자에 게 연결할 수 있는 새로운 방법을 제공 합니다. 이 문서에서는 응용 프로그램에서 내 사용자 알림을 디자인 하 고 구현 하는 방법을 보여 줍니다. 전체 구현은 [내 사용자 알림 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications) 을 참조 하세요.

![하트이 모 지 알림](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>요구 사항

+ Windows 10 및 Microsoft Visual Studio 2019. 설치에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 설정 가져오기](../get-started/get-set-up.md)를 참조 하세요.
+ C# 또는 유사한 개체 중심 프로그래밍 언어에 대한 기본 지식. C #을 시작 하려면 ["Hello, 세계" 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)를 참조 하세요.

## <a name="how-it-works"></a>작동 방법

일반적인 알림 메시지에 대 한 대 안으로, 이제 내 사용자 기능을 통해 알림을 보내 사용자에 게 보다 개인적인 환경을 제공할 수 있습니다. 이는 사용자의 작업 표시줄에 고정 된 연락처에서 내 사용자 기능과 함께 전송 되는 새로운 종류의 알림입니다. 알림이 수신 되 면 발신자의 연락처 그림이 작업 표시줄에 애니메이션 효과를 주고 알림이 시작 됨을 알리는 소리가 재생 됩니다. 페이로드에 지정 된 애니메이션이 나 이미지는 5 초 동안 표시 됩니다. 즉, 페이로드가 5 초 미만의 애니메이션 인 경우 5 초가 경과할 때까지 반복 됩니다.

## <a name="supported-image-types"></a>지원 되는 이미지 형식

+ GIF
+ 정적 이미지 (JPEG, PNG)
+ Spritesheet (세로 전용)

> [!NOTE]
> Spritesheet는 정적 이미지 (JPEG 또는 PNG)에서 파생 된 애니메이션입니다. 개별 프레임은 첫 번째 프레임이 맨 위에 오도록 세로로 정렬 됩니다 (알림 페이로드에 다른 시작 프레임을 지정할 수 있음). 각 프레임의 높이는 동일 해야 합니다. 애니메이션이 적용 되는 시퀀스 (예: 페이지가 세로로 배치 된 flipbook)를 만들기 위해 반복 합니다. Spritesheet의 예는 다음과 같습니다.

![무지개 spritesheet](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>알림 매개 변수
내 사용자 알림은 알림 [메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) 프레임 워크를 사용 하지만 알림 페이로드에 추가 바인딩 노드가 필요 합니다. 이 두 번째 바인딩에는 다음 매개 변수가 포함 되어야 합니다.

```xml
experienceType="shoulderTap"
```

이는 알림이 내 사용자 알림으로 처리 되어야 함을 나타냅니다.

바인딩 내의 이미지 노드에는 다음 매개 변수가 포함 되어야 합니다.

+ **src**
    + 자산의 URI입니다. HTTP/HTTPS 웹 URI, msappx URI 또는 로컬 파일 경로 중 하나일 수 있습니다.
+ **spritesheet-src**
    + 자산의 URI입니다. HTTP/HTTPS 웹 URI, msappx URI 또는 로컬 파일 경로 중 하나일 수 있습니다. Spritesheet 애니메이션에만 필요 합니다.
+ **spritesheet-높이**
    + 프레임 높이 (픽셀)입니다. Spritesheet 애니메이션에만 필요 합니다.
+ **spritesheet-fps**
    + FPS (초당 프레임 수)입니다. Spritesheet 애니메이션에만 필요 합니다. 1-120 값만 지원 됩니다.
+ **spritesheet-startingFrame**
    + 애니메이션을 시작할 프레임 번호입니다. Spritesheet 애니메이션에만 사용 되며, 제공 되지 않은 경우 기본값은 0입니다.
+ **alt**
    + 화면 판독기 내레이션에 사용 되는 텍스트 문자열입니다.

> [!NOTE]
> 애니메이션을 적용 하는 경우에도 "src" 매개 변수에서 정적 이미지를 지정 해야 합니다. 애니메이션을 표시 하지 못하는 경우이를 다시 사용 합니다.

또한 최상위 알림 노드에는 보내는 연락처를 지정 하기 위한 **hint** 매개 변수가 포함 되어야 합니다. 이 매개 변수는 다음 값을 가질 수 있습니다.

+ **메일 주소** 
    + 예를 들어 ` mailto:johndoe@mydomain.com `
+ **전화 번호** 
    + 예를 들어 tel: 888-888-8888
+ **원격 ID** 
    + 예를 들어 remoteid: 1234

> [!NOTE]
> 앱이 지 속성 [저장소 api](/uwp/api/windows.applicationmodel.contacts.contactstore) 를 사용 하 고 [StoredContact](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) 속성을 사용 하 여 PC에 저장 된 연락처를 원격으로 저장 된 연락처와 연결 하는 경우 RemoteId 속성의 값을 안정적이 고 고유 하 게 설정 하는 것이 중요 합니다. 즉, 원격 ID는 단일 사용자 계정을 지속적으로 식별 해야 하며, 다른 앱에서 소유 하는 연락처를 비롯 하 여 PC의 다른 연락처에 대 한 원격 Id와 충돌 하지 않도록 보장 하는 고유한 태그를 포함 해야 합니다.
> 앱에서 사용 하는 원격 Id가 안정적이 고 고유 하지 않을 경우 시스템에 추가 하기 전에 모든 원격 Id에 고유한 태그를 추가 하기 위해 [RemoteIdHelper 클래스](/previous-versions/windows/apps/jj207024(v=vs.105)#BKMK_UsingtheRemoteIdHelperclass) 를 사용할 수 있습니다. 또는 RemoteId 속성을 전혀 사용 하지 않도록 선택 하 고 대신 연락처에 대 한 원격 Id를 저장할 사용자 지정 확장 속성을 만들 수 있습니다.

두 번째 바인딩과 페이로드 외에도 대체 알림에 대 한 첫 번째 바인딩에 다른 페이로드를 포함 해야 합니다. 알림을 정기적으로 되돌려야 하는 경우이를 사용 합니다 ( [이 문서의 끝](#falling-back-to-toast)에 자세히 설명 되어 있음).

## <a name="creating-the-notification"></a>알림 만들기
알림 [메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)와 마찬가지로 내 사용자 알림 템플릿을 만들 수 있습니다.

다음은 정적 이미지 페이로드를 사용 하 여 내 사용자 알림을 만드는 방법의 예입니다.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

알림을 시작 하는 경우 다음과 같이 표시 됩니다.

![정적 이미지 알림](images/static-image-notification-small.gif)

애니메이션 spritesheet 페이로드를 사용 하 여 알림을 만드는 방법의 예는 다음과 같습니다. 이 spritesheet에는 80 픽셀의 프레임 높이가 있으므로 초당 25 개의 프레임에 애니메이션 효과를 적용할 수 있습니다. 시작 프레임을 15로 설정 하 고 "src" 매개 변수에 정적 대체 (fallback) 이미지를 제공 합니다. Spritesheet 애니메이션을 표시 하지 못하는 경우에는 대체 (fallback) 이미지가 사용 됩니다.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

알림을 시작 하는 경우 다음과 같이 표시 됩니다.

![spritesheet 알림](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>알림 시작
내 사용자 알림을 시작 하려면 알림 템플릿을 [XmlDocument](/uwp/api/windows.data.xml.dom.xmldocument) 개체로 변환 해야 합니다. XML 파일 (여기서는 "content.xml")에서 알림을 정의한 경우이 코드를 사용 하 여 시작할 수 있습니다.

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

그런 다음이 코드를 사용 하 여 알림을 만들고 보낼 수 있습니다.

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>알림으로 다시 대체
일부 경우에는 내 사용자 알림이 일반 알림 메시지로 표시 되는 경우가 있습니다. 내 사용자 알림은 다음 조건에 따라 알림 메시지로 대체 됩니다.

+ 알림이 표시 되지 않습니다.
+ 받는 사람이 내 사용자 알림을 사용 하도록 설정 하지 않았습니다.
+ 보낸 사람의 연락처가 받는 사람의 작업 표시줄에 고정 되어 있지 않습니다.

내 사용자 알림이 알림로 대체 되는 경우 두 번째 내 사용자 관련 바인딩은 무시 되 고 첫 번째 바인딩만 알림 메시지를 표시 하는 데 사용 됩니다. 첫 번째 알림 바인딩에 대체 페이로드를 제공 하는 것이 중요 한 이유입니다.

## <a name="see-also"></a>참고 항목
+ [내 사용자 알림 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [내 사용자 지원 추가](my-people-support.md)
+ [적응 알림 메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [To Notification 클래스](/uwp/api/windows.ui.notifications.toastnotification)