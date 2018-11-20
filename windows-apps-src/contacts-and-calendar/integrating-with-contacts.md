---
author: normesta
description: 연락처 카드의 작업 옆에 앱을 추가하는 방법을 보여 줍니다.
MSHAttr: PreferredLib:/library/windows/apps
title: 연락처 카드의 작업에 앱 연결
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 연락처, 연락처 카드, 주석
ms.assetid: 0edabd9c-ecfb-4525-bc38-53f219d744ff
ms.localizationpriority: medium
ms.openlocfilehash: eb1c01a4fe370f899da185dc39b7d3abe6a1904e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7417169"
---
# <a name="connect-your-app-to-actions-on-a-contact-card"></a>연락처 카드의 작업에 앱 연결

연락처 카드 또는 미니 연락처 카드의 작업 옆에 앱을 표시할 수 있습니다. 사용자는 앱을 선택하여 프로필 페이지 열기, 전화 걸기, 메시지 보내기 등의 작업을 수행할 수 있습니다.

![연락처 카드 및 미니 연락처 카드](images/all-contact-cards.png)

시작하려면 기존 연락처를 찾거나 새로 만듭니다. *주석*과 몇 개의 패키지 매니페스트 항목을 만들어 앱에서 지원하는 작업을 설명합니다. 그런 다음 작업을 수행하는 코드를 작성합니다.

전체 샘플을 보려면 [연락처 카드 통합 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)을 참조하세요.

## <a name="find-or-create-a-contact"></a>연락처 찾기 또는 만들기

앱에서 사람들 간의 연결을 도와주는 경우 Windows에서 연락처를 검색한 다음 주석을 답니다. 앱에서 연락처를 관리하는 경우 Windows 연락처 목록에 추가한 다음 주석을 달 수 있습니다.

### <a name="find-a-contact"></a>연락처 찾기

이름, 메일 주소 또는 전화 번호를 사용하여 연락처를 찾습니다.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```

### <a name="create-a-contact"></a>연락처 만들기

앱이 주소록과 유사한 경우 연락처를 만든 다음 연락처 목록에 추가합니다.

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

## <a name="tag-each-contact-with-an-annotation"></a>주석을 사용하여 각 연락처에 태그 지정

앱에서 수행할 수 있는 작업 목록(예: 영상 통화, 메시지)을 사용하여 각 연락처에 태그를 지정합니다.

그런 다음 앱이 해당 사용자를 식별하기 위해 내부적으로 사용하는 ID에 연락처 ID를 연결합니다.

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

## <a name="register-for-each-operation"></a>각 작업 등록

패키지 매니페스트에서 주석에 나열한 각 작업을 등록합니다.

매니페스트의 ``Extensions`` 요소에 프로토콜 처리기를 추가하여 등록합니다.

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
Visual Studio 매니페스트 디자이너의 **선언** 탭에서 이러한 처리기를 추가할 수도 있습니다.

![매니페스트 디자이너의 선언 탭](images/manifest-designer-protocols.png)

## <a name="find-your-app-next-to-actions-in-a-contact-card"></a>연락처 카드의 작업 옆에서 앱 찾기

피플 앱을 엽니다. 주석 및 패키지 매니페스트에서 지정한 각 작업 옆에 앱이 나타납니다.

![연락처 카드](images/a-contact-card.png)

사용자가 작업에 대해 해당 앱을 선택할 경우 다음에 연락처 카드를 열면 이 작업의 기본 앱으로 표시됩니다.

## <a name="find-your-app-next-to-actions-in-a-mini-contact-card"></a>미니 연락처 카드의 작업 옆에서 앱 찾기

미니 연락처 카드에서 작업을 나타내는 탭에 앱이 나타납니다.

![미니 연락처 카드](images/mini-contact-card.png)

**메일** 앱 등의 앱에서는 미니 연락처 카드가 열립니다. 해당 앱에서도 열 수 있습니다. 다음 코드는 이 작업을 수행하는 방법을 보여 줍니다.

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

미니 연락처 카드를 사용하는 더 많은 예제를 보려면 [연락처 카드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards)을 참조하세요.

연락처 카드와 마찬가지로 각 탭은 사용자가 앱으로 돌아가기 쉽도록 마지막으로 사용한 앱을 저장합니다.

## <a name="perform-operations-when-users-select-your-app-in-a-contact-card"></a>사용자가 연락처 카드에서 앱을 선택할 때 작업 수행

**App.cs** 파일의 [Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330) 메서드를 재정의하고 사용자를 앱의 페이지로 이동합니다. [연락처 카드 통합 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)에서는 이 작업을 수행하는 한 가지 방법을 보여 줍니다.

페이지의 코드 숨김 파일에서 [Page.OnNavigatedTo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.onnavigatedto.aspx) 메서드를 재정의합니다. 연락처 카드는 이 메서드에 작업 이름과 사용자 ID를 전달합니다.

영상 통화나 음성 통화를 시작하려면 [VoIP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/VoIP)을 참조하세요. [WIndows.ApplicationModel.Calls](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.calls.aspx) 네임스페이스에서 전체 API를 확인할 수 있습니다.

메시지를 간편하게 하려면 [Windows.ApplicationModel.Chat](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.aspx) 네임스페이스를 참조하세요.

다른 앱을 시작할 수도 있습니다. 다음 코드에서는 이 작업을 수행합니다.

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

        options.TargetApplicationPackageFamilyName = “ContosoApp”;

        string launchString = args.uri.Scheme + ":" + args.uri.Query;
        var launchUri = new Uri(launchString);
        await Windows.System.Launcher.LaunchUriAsync(launchUri, options);
    }
}
```

```args.uri.scheme``` 속성에는 작업 이름이 포함되고 ```args.uri.Query``` 속성에는 사용자 ID가 포함됩니다.
