---
title: 내 피플 알림
description: 새로운 알림 메시지인 내 피플 알림을 만들고 사용하는 방법을 설명합니다.
author: muhsinking
ms.author: mukin
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 943d236699ccab6d61e5394426077a32d7249592
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5708490"
---
# <a name="my-people-notifications"></a>내 피플 알림

내 피플 알림은 사용자가 빠른 표현력이 우수한 제스처를 통해 중요한 사람들과 연락할 수 있는 새로운 방법을 제공합니다. 이번 문서에서는 응용 프로그램에서 내 피플 알림을 디자인하고 구현하는 방법에 대해서 설명합니다. 전체 구현체에 대한 자세한 내용은 [내 피플 알림 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)을 참조하세요.

![하트 이모지 알림](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>요구 사항

+ Windows 10 및 Microsoft Visual Studio 2017. 설치 세부 정보는 [Visual Studio를 사용하여 설정](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)을 참조하세요.
+ C# 또는 유사한 개체 중심 프로그래밍 언어에 대한 기본 지식. C#을 시작하려면 ["Hello, world" 앱 만들기](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)를 참조하세요.

## <a name="how-it-works"></a>작동 방식

이제 일반적인 알림 메시지 대신 내 피플 기능을 통해 알림을 전송하여 사용자에게 보다 개인적인 경험을 제공할 수 있습니다. 이 새로운 종류의 알림 메시지는 내 피플 기능과 함께 사용자의 작업 표시줄에 고정된 연락처에서 전송됩니다. 알림이 수신되면 피플 알림이 시작됨을 알리기 위해 보낸 사람의 연락처 사진이 작업 표시줄에 애니메이션으로 표시되고 소리가 재생됩니다. 페이로드에 지정된 애니메이션 또는 이미지가 5초간 표시됩니다(또는 페이로드가 5초 미만 동안 유지되는 애니메이션인 경우 5초가 경과할 때까지 반복).

## <a name="supported-image-types"></a>지원되는 이미지 형식

+ GIF
+ 정적 이미지(JPEG, PNG)
+ 스프라이트시트(세로만)

> [!NOTE]
> 스프라이트시트는 정적 이미지(JPEG 또는 PNG)에서 파생된 애니메이션입니다. 개별 프레임은 세로로 정렬됩니다. 예를 들어, 첫 번째 프레임이 맨 위에 옵니다(하지만 알림 페이로드에서 다른 시작 프레임을 지정할 수 있음). 각 프레임의 높이가 같아야 하며, 프로그램에서 애니메이션 시퀀스를 만들기 위해 각 프레임을 반복합니다(페이지가 세로로 배치되는 플립 북처럼). 아래는 스프라이트시트의 예입니다.

![무지개 스프라이트시트](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>알림 매개 변수
내 피플 알림은 [알림 메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) 프레임워크를 사용하지만 알림 페이로드에 추가 바인딩 노드가 필요합니다. 이 두 번째 바인딩에 다음 매개 변수가 포함되어야 합니다.

```xml
experienceType=”shoulderTap”
```

이것은 알림 메시지를 내 피플 알림으로 처리해야 함을 나타냅니다.

바인딩 내부의 이미지 노드에 다음 매개 변수가 포함되어야 합니다.

+ **src**
    + 자산의 URI입니다. HTTP/HTTPS 웹 URI, msappx URI 또는 로컬 파일의 경로일 수 있습니다.
+ **spritesheet-src**
    + 자산의 URI입니다. HTTP/HTTPS 웹 URI, msappx URI 또는 로컬 파일의 경로일 수 있습니다. 스프라이트시트 애니메이션에만 필요합니다.
+ **spritesheet-height**
    + 프레임 높이(픽셀)입니다. 스프라이트시트 애니메이션에만 필요합니다.
+ **spritesheet-fps**
    + 초당 프레임(FPS)입니다. 스프라이트시트 애니메이션에만 필요합니다. 1-120 범위의 값만 지원됩니다.
+ **spritesheet-startingFrame**
    + 애니메이션을 시작하는 프레임 수입니다. 스프라이트시트 애니메이션에만 사용되며 값을 입력하지 않으면 기본값 0이 사용됩니다.
+ **alt**
    + 화면 읽기 프로그램 내레이터에 사용되는 텍스트 문자열입니다.

> [!NOTE]
> 애니메이션 알림을 만들 때도 "원본" 매개 변수에서 정적 이미지를 지정해야 합니다. 애니메이션이 표시되지 않으면 이 정적 이미지가 대체로 사용됩니다.

또한 최상위 알림 메시지 노드에는 보내는 연락처를 지정하는 **hint-people** 매개 변수가 포함되어야 합니다. 이 매개 변수에 가능한 값은 다음과 같습니다.

+ **메일 주소** 
    + 예: mailto:johndoe@mydomain.com
+ **전화 번호** 
    + 예: tel:888-888-8888
+ **원격 ID** 
    + 예: remoteid:1234

> [!NOTE]
> 앱에서 PC에 저장된 연락처를 원격으로 저장된 연락처와 연결하기 위해 [ContactStore Api](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) 및 [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) 속성을 사용하는 경우 RemoteId 속성의 값이 반드시 안정적이고 고유해야 합니다. 즉, 원격 ID는 단일 사용자 계정을 일관적으로 식별해야 하며, 다른 앱 소유의 연락처를 포함하여 PC에 있는 다른 연락처의 원격 ID와 충돌하지 않도록 보장하는 고유의 태그를 포함해야 합니다.
> 앱에서 사용하는 원격 ID가 안정적이고 고유하다는 보장이 없는 경우 [RemoteIdHelper 클래스](https://msdn.microsoft.com/en-us/library/windows/apps/jj207024(v=vs.105).aspx#BKMK_UsingtheRemoteIdHelperclass)를 사용하여 모든 원격 ID를 시스템에 추가하기 전에 고유의 태그를 추가하면 됩니다. 또는 RemoteId 속성을 전혀 사용하지 않고, 그 대신 연락처의 원격 ID를 저장하는 사용자 지정 확장 속성을 만드는 방법을 선택할 수 있습니다.

두 번째 바인딩 및 페이로드 외에도 첫 번째 바인딩에 대체 알림을 위한 또 다른 페이로드를 포함해야 합니다. 일반 알림으로 되돌려야 할 경우 알림에서 사용합니다([이 문서의 후반부](https://review.docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications#falling-back-to-toast)에서 자세하게 설명함).

## <a name="creating-the-notification"></a>알림 만들기
[알림 메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)와 마찬가지 방법으로 내 피플 알림 템플릿을 만들 수 있습니다.

다음은 정적 이미지 페이로드를 사용하여 내 피플 알림을 만드는 예입니다.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

알림을 시작하면 다음과 같이 표시됩니다.

![정적 이미지 알림](images/static-image-notification-small.gif)

다음은 애니메이션 스프라이트시트 페이로드를 사용하여 알림을 만드는 예입니다. 이 스프라이트시트의 프레임 높이는 80픽셀이며, 초당 25프레임의 속도로 애니메이션할 것입니다. 시작 프레임을 15로 설정하고 "src" 매개 변수에 정적 대체 이미지를 제공합니다. 대체 이미지는 스프라이트시트 애니메이션이 표시되지 않는 경우에 사용됩니다.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

알림을 시작하면 다음과 같이 표시됩니다.

![스프라이트시트 알림](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>알림 시작
내 피플 알림을 시작하려면 알림 메시지 템플릿을 [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx) 개체로 변환해야 합니다. XML 파일(여기서는 "content.xml")에 알림을 정의했다면, 이 코드를 사용하여 알림을 시작할 수 있습니다.

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

그런 다음 이 코드를 사용하여 알림을 만들고 보낼 수 있습니다.

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>대체 알림 메시지
내 피플 알림이 일반 알림으로 대신 표시되는 경우가 가끔 있습니다. 다음과 같은 경우 내 피플 알림이 알림 메시지로 대체됩니다.

+ 알림이 표시되지 않음
+ 받는 사람이 내 피플 알림을 사용하지 않음
+ 보낸 사람의 연락처가 받는 사람의 작업 표시줄에 고정되지 않음

내 피플 알림이 알림 메시지로 대체되면 두 번째 내 피플 관련 바인딩은 무시되고 첫 번째 바인딩만 알림 메시지 표시에 사용됩니다. 이는 첫 번째 알림 바인딩에서 대체 페이로드를 제공하는 것이 중요한 이유입니다.

## <a name="see-also"></a>참고 항목
+ [내 피플 알림 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [내 피플 지원 추가](my-people-support.md)
+ [적응형 알림 메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [ToastNotification 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)