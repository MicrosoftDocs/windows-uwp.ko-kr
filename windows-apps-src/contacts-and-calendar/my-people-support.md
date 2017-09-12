---
title: "응용 프로그램에 내 피플 지원 추가"
description: "응용 프로그램에 내 피플 지원을 추가하는 방법과 연락처를 고정 및 고정 해제하는 방법을 설명합니다."
author: mukin
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 61f73fd2fc0d14d0d1d478d67763c9b0e252cd36
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/14/2017
---
# <a name="adding-my-people-support-to-an-application"></a>응용 프로그램에 내 피플 지원 추가

> [!IMPORTANT]
> **시험판 | 가을 크리에이터 업데이트 필요**: 내 피플 API를 사용하려면 [Insider SDK 16225](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)를 대상으로 하고 [Insider 빌드 16226](https://blogs.windows.com/windowsexperience/2017/06/21/announcing-windows-10-insider-preview-build-16226-pc/) 이상을 실행해야 합니다.

사용자는 내 피플 기능을 사용하여 응용 프로그램에서 바로 작업 표시줄에 연락처를 고정할 수 있으며, 이렇게 하면 사용자가 여러 가지 방법으로 상호 작용할 수 있는 새 연락처 개체가 생성됩니다. 이 문서에서는 사용자가 앱에서 바로 연락처를 고정할 수 있도록 이 기능에 대한 지원을 추가하는 방법을 보여 줍니다. 연락처가 고정되면 [내 피플 공유](my-people-sharing.md) 및 [알림](my-people-notifications.md) 같은 새로운 유형의 사용자 상호 작용을 사용할 수 있게 됩니다.

![내 피플 채팅](images/my-people-chat.png)

## <a name="requirements"></a>요구 사항

+ Windows 10 및 Microsoft Visual Studio 2017. 설치 세부 정보는 [Visual Studio를 사용하여 설정](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)을 참조하세요.
+ C# 또는 유사한 개체 중심 프로그래밍 언어에 대한 기본 지식. C#을 시작하려면 ["Hello, world" 앱 만들기](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)를 참조하세요.

## <a name="overview"></a>개요

응용 프로그램에서 내 피플 기능을 사용할 수 있게 하려면 세 가지를 수행해야 합니다.

1. [응용 프로그램 매니페스트에서 shareTarget 정품 인증 계약에 대한 지원을 선언합니다.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [사용자가 앱을 사용하여 공유할 수 있는 연락처에 주석을 답니다.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3.  동시에 실행되는 여러 응용 프로그램 인스턴스를 지원합니다. 사용자가 연락처 패널에서 응용 프로그램을 사용하는 동안 응용 프로그램의 전체 버전과 상호 작용할 수 있어야 합니다.  사용자가 동시에 여러 연락처 패널에서 응용 프로그램을 사용할 수도 있습니다.  이 상황을 지원하려면 응용 프로그램이 여러 보기를 동시에 실행할 수 있어야 합니다. 자세한 방법은 ["앱에 대한 여러 보기 표시"](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views)를 참조하세요.

여기까지 마치면 주석 처리된 연락처의 연락처 패널에 응용 프로그램이 표시됩니다.

## <a name="declaring-support-for-the-contract"></a>계약에 대한 지원 선언

내 피플 계약에 대한 지원을 선언하려면 Visual Studio에서 응용 프로그램을 엽니다. **솔루션 탐색기**에서 **Package.appxmanifest**를 오른쪽 단추로 클릭하고**연결 프로그램**을 선택합니다. 메뉴에서 **XML(텍스트) 편집기**를 선택하고 **확인**을 클릭합니다. 매니페스트를 다음과 같이 변경합니다.

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

이렇게 추가하면 이제 **windows.ContactPanel** 계약을 통해 응용 프로그램을 시작할 수 있으며, 연락처 패널과 상호 작용할 수 있게 됩니다.

## <a name="annotating-contacts"></a>연락처 주석 달기

내 피플 창을 통해 응용 프로그램의 연락처를 작업 표시줄에 표시하려면 Windows 연락처 저장소에 연락처를 작성해야 합니다.  연락처를 작성하는 방법은 [연락처 카드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)을 참조하세요.

또한 응용 프로그램에서 각 연락처에 주석을 작성해야 합니다. 주석은 연락처와 연결된 응용 프로그램의 데이터 조각입니다. 주석은 **ProviderProperties** 구성원에서 개발자가 원하는 보기에 해당하는 활성화 가능한 클래스를 포함하고 있어야 하며, **ContactProfile** 작업에 대한 지원을 선언해야 합니다.

앱이 실행되는 동안 언제든지 연락처에 주석을 달 수 있지만, 일반적으로 Windows 연락처 저장소에 연락처가 추가되는 즉시 주석을 달아야 합니다.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

“appId”는 패키지 패밀리 이름이며, 그 뒤에 ‘!’ 그리고 활성화 가능한 클래스 ID가 붙습니다. 패키지 패밀리 이름을 찾으려면 기본 편집기를 사용하여 **Package.appxmanifest**를 열고 "패키징" 탭에서 찾아봅니다. 여기서 "앱"은 응용 프로그램 시작 보기에 해당하는 활성화 가능한 클래스입니다.

## <a name="allow-contacts-to-invite-new-potential-users"></a>연락처에서 새로운 잠재적 사용자를 초대할 수 있도록 허용

기본적으로 응용 프로그램은 개발자가 구체적으로 주석을 단 연락처의 연락처 패널에만 표시됩니다.  이는 앱을 통해 상호 작용할 수 없는 연락처와 혼동하지 않도록 방지하기 위한 조치입니다.  응용 프로그램이 알지 못하는 연락처에 대해서도 응용 프로그램을 표시하려면(예: 해당 연락처를 계정에 추가하도록 사용자를 초대하기 위해) 다음을 매니페스트에 추가하면 됩니다.

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

이렇게 변경하면 사용자가 고정한 모든 연락처에 대한 연락처 패널에 응용 프로그램이 사용 가능한 옵션으로 표시됩니다.  연락처 패널 계약을 사용하여 응용 프로그램이 활성화되면 연락처가 응용 프로그램에서 알고 있는 연락처인지 확인해야 합니다.  그렇지 않으면 앱의 새로운 사용자 환경을 표시해야 합니다.

![내 피플 연락처 패널](images/my-people.png)

## <a name="support-for-email-apps"></a>전자 메일 앱 지원

전자 메일 앱을 작성하는 경우 모든 연락처에 수동으로 주석을 달 필요가 없습니다.  연락처 패널 및 mailto: 프로토콜에 대한 지원을 선언하면 전자 메일 주소가 있는 사용자에게 응용 프로그램이 자동으로 표시됩니다.

## <a name="running-in-the-contact-panel"></a>연락처 패널에서 실행

이제 일부 또는 모든 사용자에 대한 연락처 패널에 응용 프로그램이 표시되니, 연락처 패널 계약을 사용하여 활성화를 처리해야 합니다.

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

이 계약을 통해 응용 프로그램이 활성화되면 응용 프로그램에서 [ContactPanelActivatedEventArgs 개체](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs)를 수신합니다.  이 개체에는 응용 프로그램이 시작 시 상호 작용을 시도하는 연락처 ID와 [ContactPanel](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactpanel) 개체가 포함되어 있습니다. 이 ContactPanel 개체에 대한 참조를 유지해야 하며, 그래야만 패널과 상호 작용할 수 있습니다.

ContactPanel 개체에는 응용 프로그램에서 수신 대기해야 하는 두 이벤트가 들어 있습니다.
+ 모든 응용 프로그램 전체가 고유의 창에서 실행되도록 요청하는 UI 요소를 사용자가 호출하면 **LaunchFullAppRequested** 이벤트가 전송됩니다.  응용 프로그램은 스스로를 시작하고 필요한 모든 컨텍스트를 전달해야 합니다.  하지만 이 작업은 원하는 대로 자유롭게 수행하면 됩니다(예: 프로토콜 실행을 통해).
+ 응용 프로그램이 종료되기 직전에는 컨텍스트를 저장할 수 있도록 **Closing 이벤트**가 전송됩니다.

또한 ContactPanel 개체를 사용하여 연락처 패널 헤더의 배경색을 설정하고(설정하지 않으면 시스템 테마를 기본값으로 사용) 프로그래밍 방식으로 연락처 패널을 닫을 수 있습니다.



## <a name="the-pinnedcontactmanager-class"></a>PinnedContactManager 클래스

[PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) 클래스는 작업 표시줄에 고정되는 연락처를 관리하는 데 사용됩니다. 이 클래스를 사용하여 연락처를 고정 및 고정 해제하고, 연락처가 고정되었는지 확인하고, 현재 응용 프로그램이 실행되고 있는 시스템에서 특정 화면에 고정하는 것을 지원하는지 여부를 확인할 수 있습니다.

**GetDefault** 메서드를 사용하여 PinnedContactManager 개체를 검색할 수 있습니다.

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>연락처 고정 및 고정 해제
이제 방금 만든 PinnedContactManager를 사용하여 연락처를 고정 및 고정 해제할 수 있습니다. **RequestPinContactAsync** 및 **RequestUnpinContactAsync** 메서드는 사용자에게 확인 대화 상자를 제공하며, 따라서 이러한 메서드는 ASTA(응용 프로그램 단일 스레드 아파트) 스레스에서 호출해야 합니다.

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

또한 동시에 여러 연락처를 고정할 수 있습니다.

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> 현재 연락처를 고정 해제하는 일괄 처리 작업은 없습니다.

**참고:** 

## <a name="see-also"></a>참고 항목
+ [내 피플 공유](my-people-sharing.md)
+ [내 피플 알림](my-people-notifications.md)
+ [연락처 카드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [PinnedContactManager 클래스 설명서](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
