---
description: 연락처 카드의 작업 옆에 앱을 추가 하는 방법을 보여 줍니다.
MSHAttr: PreferredLib:/library/windows/apps
title: 연락처 카드의 작업에 앱 연결
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 연락처, 연락처 카드, 주석
ms.assetid: 0edabd9c-ecfb-4525-bc38-53f219d744ff
ms.localizationpriority: medium
ms.openlocfilehash: 246a74ca008e1b8c89460aabb652accf35c842b1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154717"
---
# <a name="connect-your-app-to-actions-on-a-contact-card"></a>연락처 카드의 작업에 앱 연결

앱은 연락처 카드 또는 미니 연락처 카드의 작업 옆에 표시 될 수 있습니다. 사용자는 앱을 선택하여 프로필 페이지 열기, 전화 걸기, 메시지 보내기 등의 작업을 수행할 수 있습니다.

![연락처 카드 및 미니 연락처 카드](images/all-contact-cards.png)

시작 하려면 기존 연락처를 찾거나 새 연락처를 만듭니다. 다음으로, *주석* 및 몇 가지 패키지 매니페스트 항목을 만들어 앱이 지 원하는 작업을 설명 합니다. 그런 다음 작업을 수행 하는 코드를 작성 합니다.

전체 샘플은 [연락처 카드 통합 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)을 참조 하세요.

## <a name="find-or-create-a-contact"></a>연락처 찾기 또는 만들기

앱이 다른 사용자와 연결 하는 데 도움이 되는 경우 Windows에서 연락처를 검색 한 다음 주석을 답니다. 앱에서 연락처를 관리 하는 경우 Windows 연락처 목록에 추가 하 고 주석을 달 수 있습니다.

### <a name="find-a-contact"></a>연락처 찾기

이름, 전자 메일 주소 또는 전화 번호를 사용 하 여 연락처를 찾습니다.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```

### <a name="create-a-contact"></a>연락처 만들기

앱이 주소록과 같은 경우 연락처를 만든 다음 연락처 목록에 추가 합니다.

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);

```

## <a name="tag-each-contact-with-an-annotation"></a>주석을 사용 하 여 각 연락처 태그

각 연락처에 앱에서 수행할 수 있는 작업 (작업) 목록 (예: 비디오 통화 및 메시징)에 태그를 지정할 수 있습니다.

그런 다음 연락처 ID를 앱에서 내부적으로 사용 하는 ID에 연결 하 여 해당 사용자를 식별 합니다.

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.Message |
  ContactAnnotationOperations.AudioCall |
  ContactAnnotationOperations.VideoCall |
 ContactAnnotationOperations.ContactProfile;

await annotationList.TrySaveAnnotationAsync(annotation);
```

## <a name="register-for-each-operation"></a>각 작업에 대해 등록

패키지 매니페스트에서 주석에 나열 된 각 작업에 대해 등록 합니다.

매니페스트의 요소에 프로토콜 처리기를 추가 하 여 등록 ``Extensions`` 합니다.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-contact-profile">
      <uap:DisplayName>TestProfileApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-ipmessaging">
      <uap:DisplayName>TestMsgApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-video">
      <uap:DisplayName>TestVideoApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-call">
      <uap:DisplayName>TestCallApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
</Extensions>
```
Visual Studio의 매니페스트 디자이너의 **선언** 탭에서 이러한 항목을 추가할 수도 있습니다.

![매니페스트 디자이너의 선언 탭](images/manifest-designer-protocols.png)

## <a name="find-your-app-next-to-actions-in-a-contact-card"></a>연락처 카드의 작업 옆에서 앱 찾기

피플 앱을 엽니다. 사용자의 앱은 주석 및 패키지 매니페스트에 지정 된 각 작업 (작업) 옆에 표시 됩니다.

![연락처 카드](images/a-contact-card.png)

사용자가 작업에 대 한 앱을 선택 하는 경우 다음 번에 사용자가 연락처 카드를 열 때 해당 작업에 대 한 기본 앱으로 표시 됩니다.

## <a name="find-your-app-next-to-actions-in-a-mini-contact-card"></a>미니 연락처 카드의 작업 옆에서 앱 찾기

미니 연락처 카드에서 작업을 나타내는 탭에 앱이 표시 됩니다.

![미니 연락처 카드](images/mini-contact-card.png)

**메일** 앱과 같은 앱은 미니 연락처 카드를 엽니다. 앱을 열 수도 있습니다. 이 코드에서는이 작업을 수행 하는 방법을 보여 줍니다.

```cs
public async void OpenContactCard(object sender, RoutedEventArgs e)
{
    // Get the selection rect of the button pressed to show contact card.
    FrameworkElement element = (FrameworkElement)sender;

    Windows.UI.Xaml.Media.GeneralTransform buttonTransform = element.TransformToVisual(null);
    Windows.Foundation.Point point = buttonTransform.TransformPoint(new Windows.Foundation.Point());
    Windows.Foundation.Rect rect =
        new Windows.Foundation.Rect(point, new Windows.Foundation.Size(element.ActualWidth, element.ActualHeight));

   // helper method to find a contact just for illustrative purposes.
    Contact contact = await findContact("contoso@contoso.com");

    ContactManager.ShowContactCard(contact, rect, Windows.UI.Popups.Placement.Default);

}
```

미니 연락처 카드를 사용 하는 더 많은 예제를 보려면 [연락처 카드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards)을 참조 하세요.

연락처 카드와 마찬가지로 각 탭에서 사용자가 마지막으로 사용한 앱을 기억 하므로 앱에 쉽게 돌아올 수 있습니다.

## <a name="perform-operations-when-users-select-your-app-in-a-contact-card"></a>사용자가 연락처 카드에서 앱을 선택 하는 경우 작업 수행

**App.cs** 파일에서 [Application. onactivated](/uwp/api/windows.ui.xaml.application.onactivated) 된 메서드를 재정의 하 고 사용자를 앱의 페이지로 이동 합니다. [연락처 카드 통합 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration) 에서는이 작업을 수행 하는 한 가지 방법을 보여 줍니다.

페이지의 코드 숨겨진 파일에서 [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) 메서드를 재정의 합니다. 연락처 카드는이 메서드에 작업 이름 및 사용자 ID를 전달 합니다.

비디오 또는 오디오 통화를 시작 하려면이 샘플: [VoIP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/VoIP)을 참조 하세요. 전체 API는 [WIndows. ApplicationModel.](/uwp/api/windows.applicationmodel.calls) 네임 스페이스에서 찾을 수 있습니다.

메시징을 용이 하 게 하려면 [Windows ApplicationModel. 채팅](/uwp/api/windows.applicationmodel.chat) 네임 스페이스를 참조 하세요.

다른 앱을 시작할 수도 있습니다. 이 코드가 수행 하는 작업입니다.

```cs
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    var args = e.Parameter as ProtocolActivatedEventArgs;
    // Display the result of the protocol activation if we got here as a result of being activated for a protocol.

    if (args != null)
    {
        var options = new Windows.System.LauncherOptions();
        options.DisplayApplicationPicker = true;

        options.TargetApplicationPackageFamilyName = "ContosoApp";

        string launchString = args.uri.Scheme + ":" + args.uri.Query;
        var launchUri = new Uri(launchString);
        await Windows.System.Launcher.LaunchUriAsync(launchUri, options);
    }
}
```

속성에는 ```args.uri.scheme``` 작업 이름이 포함 되 고, 속성에는 ```args.uri.Query``` 사용자의 ID가 포함 됩니다.