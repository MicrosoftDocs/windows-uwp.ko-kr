---
title: 내 피플 공유
description: 내 피플 공유 지원을 추가하는 방법 설명
author: muhsinking
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7084c4dde7bdf2d59842a04fe9fd52bc029c264a
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434490"
---
# <a name="my-people-sharing"></a>내 피플 공유

사용자는 내 피플 기능을 사용하여 작업 표시줄에 연락처를 고정해 두면 어떤 응용 프로그램을 통해 연결하든 Windows 어디서나 간편하게 연락을 취할 수 있습니다. 이제 사용자는 파일 탐색기에서 내 피플 핀으로 파일을 끌어서 고정된 연락처와 콘텐츠를 공유할 수 있습니다. 또한 표준 공유 참을 통해 Windows 연락처 저장소의 모든 연락처와 공유할 수 있습니다. 응용 프로그램을 내 피플 공유 대상으로 사용하는 방법을 계속 알아보세요.

![내 피플 공유 패널](images/my-people-sharing.png)

## <a name="requirements"></a>요구 사항

+ Windows 10 및 Microsoft Visual Studio 2017. 설치 세부 정보는 [Visual Studio를 사용하여 설정](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)을 참조하세요.
+ C# 또는 유사한 개체 중심 프로그래밍 언어에 대한 기본 지식. C#을 시작하려면 ["Hello, world" 앱 만들기](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)를 참조하세요.

## <a name="overview"></a>개요

응용 프로그램을 내 피플 공유 대상으로 사용하려면 세 단계를 거쳐야 합니다.

1. [응용 프로그램 매니페스트에서 shareTarget 정품 인증 계약에 대한 지원을 선언합니다.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [사용자가 앱을 사용하여 공유할 수 있는 연락처에 주석을 답니다.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3. 동시에 실행되는 여러 응용 프로그램 인스턴스를 지원합니다.  사용자가 다른 사람과 공유하기 위해 응용 프로그램을 사용하는 동안 응용 프로그램의 전체 버전과 상호 작용할 수 있어야 합니다. 사용자가 동시에 여러 공유 창에서 응용 프로그램을 사용할 수도 있습니다. 이 상황을 지원하려면 응용 프로그램이 여러 보기를 동시에 실행할 수 있어야 합니다. 자세한 방법은 ["앱에 대한 여러 보기 표시"](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views)를 참조하세요.

여기까지 마치면 응용 프로그램이 내 피플 공유 창에 공유 대상으로 표시되며, 다음과 같은 두 가지 방법으로 시작할 수 있습니다.
1. 공유 참을 통해 연락처를 선택합니다.
2. 작업 표시줄에 고정된 연락처에 파일을 끌어서 놓습니다.

## <a name="declaring-support-for-the-share-contract"></a>공유 계약에 대한 지원 선언

공유 대상으로써 응용 프로그램에 대한 지원을 선언하려면 먼저 Visual Studio에서 응용 프로그램을 엽니다. **솔루션 탐색기**에서 **Package.appxmanifest**를 오른쪽 단추로 클릭하고**연결 프로그램**을 선택합니다. 메뉴에서 **XML(텍스트) 편집기**를 선택하고 **확인**을 클릭합니다. 그런 다음 매니페스트를 다음과 같이 변경합니다.


**이전**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**이후**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

이 코드는 모든 파일 및 데이터 형식에 대한 지원을 추가하지만, 지원되는 파일 유형과 데이터 형식을 직접 선택할 수도 있습니다(자세한 내용은 [ShareTarget 클래스 설명서](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) 참조).

## <a name="annotating-contacts"></a>연락처 주석 달기

내 피플 공유 창에 응용 프로그램을 연락처의 공유 대상으로 표시하려면 Windows 연락처 저장소에 연락처를 작성해야 합니다. 연락처를 작성하는 방법은 [연락처 카드 통합 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)을 참조하세요. 

연락처에 공유할 때 응용 프로그램이 내 피플 공유 대상으로 표시되도록 하려면 응용 프로그램이 해당 연락처에 주석을 작성해야 합니다. 주석은 연락처와 연결된 응용 프로그램의 데이터 조각입니다. 주석은 **ProviderProperties** 구성원에서 개발자가 원하는 보기에 해당하는 활성화 가능한 클래스를 포함하고 있어야 하며, **공유** 작업에 대한 지원을 선언해야 합니다.

앱이 실행되는 동안 언제든지 연락처에 주석을 달 수 있지만, 일반적으로 Windows 연락처 저장소에 연락처가 추가되는 즉시 주석을 달아야 합니다.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

“appId”는 패키지 패밀리 이름이며, 그 뒤에 ‘!’ 그리고 활성화 가능한 클래스 ID가 붙습니다. 패키지 패밀리 이름을 찾으려면 기본 편집기를 사용 하 여 **Package.appxmanifest** 를 열고 "패키징" 탭에서 찾아봅니다. 여기서 "앱"은 공유 대상 보기에 해당 하는 활성화 가능한 클래스입니다.

## <a name="running-as-a-my-people-share-target"></a>내 피플 공유 대상으로 실행

마지막으로 앱을 실행하기 위해, 앱의 기본 클래스에서 공유 대상 활성화를 처리하는 [OnShareTargetActivated](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) 메서드를 재정의합니다. [ShareTargetActivatedEventArgs.ShareOperation.Contacts](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) 속성은 공유되는 연락처를 포함하거나, 또는 이 작업이 내 피플 공유가 아닌 표준 공유 작업인 경우에는 비어 있습니다.

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>참고 항목
+ [내 피플 지원 추가](my-people-support.md)
+ [ShareTarget 클래스](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [연락처 카드 통합 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
