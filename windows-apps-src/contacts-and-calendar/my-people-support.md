---
title: 애플리케이션에 내 피플 지원 추가
description: 내 사용자 지원을 응용 프로그램에 추가 하는 방법 및 연락처를 고정 및 고정 해제 하는 방법을 설명 합니다.
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c0e174613357ad9e4e45d2776f3fbc618535b30
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339461"
---
# <a name="adding-my-people-support-to-an-application"></a>애플리케이션에 내 피플 지원 추가

> [!Note]
> Windows 10에서 2019 업데이트 (버전 1903)를 기준으로 새 Windows 10 설치에는 기본적으로 ' 작업 표시줄의 사람 '이 더 이상 표시 되지 않습니다. 고객은 작업 표시줄을 마우스 오른쪽 단추로 클릭 하 고 "작업 표시줄에 사람 표시"를 눌러 기능을 사용 하도록 설정할 수 있습니다. 개발자는 내 사용자 지원을 응용 프로그램에 추가 하지 않는 것이 좋습니다. windows 10 용 응용 프로그램을 최적화 하는 방법에 대 한 자세한 내용은 [Windows 개발자 블로그](https://blogs.windows.com/windowsdeveloper/) 를 참조 하세요.

사용자는 내 사용자 기능을 사용 하 여 응용 프로그램의 연락처를 작업 표시줄에 직접 고정할 수 있습니다. 그러면 여러 가지 방법으로 상호 작용할 수 있는 새 연락처 개체가 만들어집니다. 이 문서에서는 사용자가 앱에서 직접 연락처를 고정할 수 있도록이 기능에 대 한 지원을 추가 하는 방법을 보여 줍니다. 연락처가 고정 되어 있으면 [내 사용자 공유](my-people-sharing.md) 및 [알림과](my-people-notifications.md)같은 새로운 유형의 사용자 상호 작용을 사용할 수 있게 됩니다.

![내 사용자 채팅](images/my-people-chat.png)

## <a name="requirements"></a>요구 사항

+ Windows 10 및 Microsoft Visual Studio 2019. 설치에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 설정 가져오기](/windows/apps/get-started/get-set-up)를 참조 하세요.
+ C# 또는 유사한 개체 중심 프로그래밍 언어에 대한 기본 지식. C #을 시작 하려면 ["Hello, 세계" 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)를 참조 하세요.

## <a name="overview"></a>개요

응용 프로그램에서 내 사용자 기능을 사용할 수 있도록 하려면 다음 세 가지 작업을 수행 해야 합니다.

1. [응용 프로그램 매니페스트에서 Windows.sharetarget 활성화 계약에 대 한 지원을 선언 합니다.](./my-people-sharing.md#declaring-support-for-the-share-contract)
2. [사용자가 앱을 사용 하 여 공유할 수 있는 연락처에 주석을 추가 합니다.](./my-people-sharing.md#annotating-contacts)
3.  동시에 실행 되는 응용 프로그램의 여러 인스턴스를 지원 합니다. 사용자는 연락처 패널에서 응용 프로그램을 사용 하는 동안 전체 버전의 응용 프로그램과 상호 작용할 수 있어야 합니다.  여러 연락처 패널에서 한 번에 사용할 수도 있습니다.  이를 지원 하기 위해 응용 프로그램은 여러 보기를 동시에 실행할 수 있어야 합니다. 이 작업을 수행 하는 방법에 [대 한 자세한 내용은 "앱에 대 한 여러 뷰 표시"](../design/layout/show-multiple-views.md)문서를 참조 하세요.

이 작업을 완료 하면 주석이 추가 된 연락처에 대 한 연락처 패널에 응용 프로그램이 표시 됩니다.

## <a name="declaring-support-for-the-contract"></a>계약에 대 한 지원 선언

내 사용자 계약에 대 한 지원을 선언 하려면 Visual Studio에서 응용 프로그램을 엽니다. **솔루션 탐색기** 에서 **appxmanifest.xml** 을 마우스 오른쪽 단추로 클릭 하 고 **연결 프로그램** 을 선택 합니다. 메뉴에서 **XML (텍스트) 편집기)** 를 선택 하 고 **확인** 을 클릭 합니다. 매니페스트를 다음과 같이 변경 합니다.

**이전**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
        </Application>
    </Applications>

```

**이후**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
          <Extensions>
            <uap4:Extension Category="windows.contactPanel" />
          </Extensions>
        </Application>
    </Applications>

```

이 외에도 이제 windows를 통해 응용 프로그램을 시작할 수 있습니다 **.** 연락처 패널과 상호 작용할 수 있게 해 주는 연락처 패널 계약입니다.

## <a name="annotating-contacts"></a>연락처에 주석 달기

응용 프로그램의 연락처를 내 사용자 창에서 작업 표시줄에 표시 하도록 허용 하려면 Windows 연락처 저장소에 기록해 야 합니다.  연락처를 작성 하는 방법에 대 한 자세한 내용은 [Contact Card 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)을 참조 하세요.

또한 응용 프로그램은 각 연락처에 주석을 써야 합니다. 주석은 응용 프로그램에서 연락처와 연결 된 데이터의 일부입니다. 주석은 해당 **Providerproperties** 멤버에서 원하는 뷰에 해당 하는 활성화 가능한 클래스를 포함 해야 하며, 동료 **프로필** 작업에 대 한 지원을 선언 해야 합니다.

앱이 실행 되는 동안 언제 든 지 연락처에 주석을 추가할 수 있지만, 일반적으로 Windows 연락처 저장소에 추가 되는 즉시 연락처에 주석을 추가 해야 합니다.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations.ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

"AppId"는 패키지 제품군 이름 뒤에 '! '가 옵니다. 및 활성화 가능한 클래스 ID입니다. 패키지 제품군 이름을 찾으려면 기본 편집기를 사용 하 여 **appxmanifest.xml** 를 열고 "패키징" 탭을 확인 합니다. 여기서 "App"은 응용 프로그램 시작 뷰에 해당 하는 활성화 가능한 클래스입니다.

## <a name="allow-contacts-to-invite-new-potential-users"></a>연락처에 새 잠재적 사용자 초대 허용

기본적으로 응용 프로그램은 특별히 주석이 추가 된 연락처에 대 한 연락처 패널에만 표시 됩니다.  이는 앱을 통해 상호 작용할 수 없는 연락처와 혼동 하지 않도록 하기 위한 것입니다.  응용 프로그램에서 알지 못하는 연락처에 대해 응용 프로그램을 표시 하려면 (예: 사용자에 게 해당 사용자를 계정에 추가 하도록 초대 하는 경우) 다음을 매니페스트에 추가 하면 됩니다.

**이전**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel" />
      </Extensions>
    </Application>
</Applications>
```

**이후**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel">
            <uap4:ContactPanel SupportsUnknownContacts="true" />
        </uap4:Extension>
      </Extensions>
    </Application>
</Applications>
```

이 변경으로 응용 프로그램은 사용자가 고정 한 모든 연락처에 대 한 연락처 패널에 사용할 수 있는 옵션으로 표시 됩니다.  연락처 패널 계약을 사용 하 여 응용 프로그램을 활성화 한 경우에는 응용 프로그램에서 알고 있는 연락처 인지 확인 해야 합니다.  그렇지 않은 경우 앱의 새 사용자 환경을 표시 해야 합니다.

![내 피플 연락처 패널](images/my-people.png)

## <a name="support-for-email-apps"></a>전자 메일 앱에 대 한 지원

전자 메일 앱을 작성 하는 경우 모든 연락처에 수동으로 주석을 추가할 필요가 없습니다.  연락처 창과 mailto: 프로토콜에 대 한 지원을 선언 하면 응용 프로그램이 전자 메일 주소를 가진 사용자에 대해 자동으로 표시 됩니다.

## <a name="running-in-the-contact-panel"></a>연락처 패널에서 실행

이제 응용 프로그램이 일부 또는 모든 사용자에 대 한 연락처 패널에 표시 되었으므로 연락처 패널 계약을 사용 하 여 정품 인증을 처리 해야 합니다.

```Csharp
override protected void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.ContactPanel)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        var rootFrame = new Frame();

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        // Navigate to the page that shows the Contact UI.
        rootFrame.Navigate(typeof(ContactPage), e);

        // Ensure the current window is active
        Window.Current.Activate();
    }
}
```

이 계약을 사용 하 여 응용 프로그램이 활성화 되 면 [ContactPanelActivatedEventArgs 개체가](/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs)수신 됩니다.  여기에는 시작 시 응용 프로그램에서 상호 작용을 시도 하는 연락처의 ID와 지 수 [패널](/uwp/api/windows.applicationmodel.contacts.contactpanel) 개체가 포함 됩니다. 패널과 상호 작용할 수 있게 해 주는이 교류 패널 개체에 대 한 참조를 유지 해야 합니다.

연락처 패널 개체에는 응용 프로그램에서 수신 해야 하는 두 개의 이벤트가 있습니다.
+ 사용자가 전체 응용 프로그램을 자체 창에서 시작 하도록 요청 하는 UI 요소를 호출 하면 **Launchfullapprequested** 이벤트가 전송 됩니다.  응용 프로그램은 자체를 시작 하 여 필요한 모든 컨텍스트를 전달 합니다.  이 작업은 무료로 수행할 수 있지만 (예: 프로토콜 시작을 통해)
+ **닫기 이벤트** 는 응용 프로그램이 닫힐 때 전송 되므로 모든 컨텍스트를 저장할 수 있습니다.

또한 연락처 패널 개체를 사용 하 여 연락처 패널 헤더의 배경색을 설정 (설정 되지 않은 경우 기본값은 시스템 테마) 하 고 프로그래밍 방식으로 연락처 패널을 닫을 수 있습니다.

## <a name="supporting-notification-badging"></a>알림 배지 지원

앱에서 새 알림이 도착할 때 작업 표시줄에 고정 된 연락처를 직장 배지가 달린 하려면 알림 [메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) 에 **힌트-** person 매개 변수를 포함 하 고 [내 사용자 알림을](./my-people-notifications.md)표현 해야 합니다.

![사용자 알림 배지](images/my-people-badging.png)

연락처를 배지 하려면 최상위 알림 노드에 전송 또는 관련 연락처를 나타내는 hint 매개 변수를 포함 해야 합니다. 이 매개 변수는 다음 값 중 하나를 가질 수 있습니다.
+ **메일 주소** 
    + 예: mailto:johndoe@mydomain.com
+ **전화 번호** 
    + 예: tel: 888-888-8888
+ **원격 ID** 
    + 예: remoteid: 1234

다음은 특정 사용자와 관련 된 알림 메시지를 식별 하는 방법의 예입니다.
```XML
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastText01">
            <text>John Doe posted a comment.</text>
        </binding>
    </visual>
</toast>
```

> [!NOTE]
> 앱이 지 속성 [저장소 api](/uwp/api/windows.applicationmodel.contacts.contactstore) 를 사용 하 고 [StoredContact](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) 속성을 사용 하 여 PC에 저장 된 연락처를 원격으로 저장 된 연락처와 연결 하는 경우 RemoteId 속성의 값을 안정적이 고 고유 하 게 설정 하는 것이 중요 합니다. 즉, 원격 ID는 단일 사용자 계정을 일관 되 게 식별 해야 하며 다른 앱에서 소유 하는 연락처를 비롯 하 여 PC의 다른 연락처에 대 한 원격 Id와 충돌 하지 않도록 보장 하는 고유한 태그를 포함 해야 합니다.
> 앱에서 사용 하는 원격 Id가 안정적이 고 고유 하지 않을 수 있는 경우이 항목의 뒷부분에 나와 있는 RemoteIdHelper 클래스를 사용 하 여 시스템에 추가 하기 전에 모든 원격 Id에 고유한 태그를 추가할 수 있습니다. 또는 RemoteId 속성을 전혀 사용 하지 않도록 선택 하 고 대신 연락처에 대 한 원격 Id를 저장할 사용자 지정 확장 속성을 만들 수 있습니다.

## <a name="the-pinnedcontactmanager-class"></a>Pinne6stcmanager 클래스

[고정 된 연락처 관리자](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) 는 작업 표시줄에 고정 된 연락처를 관리 하는 데 사용 됩니다. 이 클래스를 사용 하 여 연락처를 고정 및 고정 해제 하 고, 연락처가 고정 되는지 여부를 확인 하 고, 응용 프로그램이 현재 실행 중인 시스템에서 특정 화면에 대 한 고정이 지원 되는지 여부를 결정할 수 있습니다.

**Getdefault** 메서드를 사용 하 여 Pinnewstcmanager 개체를 검색할 수 있습니다.

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>연락처 고정 및 고정 해제
이제 방금 만든 PinnedContactManager를 사용 하 여 연락처를 고정 및 고정 해제할 수 있습니다. **RequestPinContactAsync** 및 **RequestUnpinContactAsync** 메서드는 사용자에 게 확인 대화 상자를 제공 하므로 응용 프로그램 Single-Threaded 아파트 (ASTA 또는 UI) 스레드에서 호출 해야 합니다.

```Csharp
async void PinContact (Contact contact)
{
    await pinnedContactManager.RequestPinContactAsync(contact,
                                                      PinnedContactSurface.Taskbar);
}

async void UnpinContact (Contact contact)
{
    await pinnedContactManager.RequestUnpinContactAsync(contact,
                                                        PinnedContactSurface.Taskbar);
}
```

동시에 여러 연락처를 고정할 수도 있습니다.

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> 현재 연락처를 고정 해제 하기 위한 일괄 처리 작업이 없습니다.

**참고:** 

## <a name="see-also"></a>참고 항목
+ [내 피플 공유](my-people-sharing.md)
+ [내 사람들 알림](my-people-notifications.md)
+ [응용 프로그램에 사용자 지원을 추가 하는 Channel 9 비디오](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [내 피플 통합 샘플](https://github.com/tonyPendolino/MyPeopleBuild2017)
+ [연락처 카드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Pinnee지도 관리자 클래스 설명서](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [연락처 카드의 작업에 앱 연결](./integrating-with-contacts.md)