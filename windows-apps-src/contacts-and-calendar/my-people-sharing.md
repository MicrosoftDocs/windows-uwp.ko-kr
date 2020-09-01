---
title: 내 피플 공유
description: 내 사용자 공유를 사용 하 여 사용자가 자신의 작업 표시줄에 연락처를 고정 하 고 Windows의 어디에서 나 쉽게 연락할 수 있습니다.
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 76d52fe3ed7e7fb74ae5338e589ab34751bedebe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173667"
---
# <a name="my-people-sharing"></a>내 피플 공유

사용자는 내 사용자 기능을 통해 사용자가 연결 된 응용 프로그램에 관계 없이 작업 표시줄에 연락처를 고정 하 여 Windows의 어디에서 나 쉽게 터치를 유지할 수 있습니다. 이제 사용자는 파일 탐색기에서 내 사용자 pin으로 파일을 끌어서 고정 된 연락처와 콘텐츠를 공유할 수 있습니다. 또한 표준 공유 참을 통해 Windows 연락처 저장소의 모든 연락처로 공유할 수 있습니다. 내 사용자 공유 대상으로 응용 프로그램을 사용 하도록 설정 하는 방법을 알아보려면 계속 읽어 보세요.

![내 사용자 공유 패널](images/my-people-sharing.png)

## <a name="requirements"></a>요구 사항

+ Windows 10 및 Microsoft Visual Studio 2019. 설치에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 설정 가져오기](../get-started/get-set-up.md)를 참조 하세요.
+ C# 또는 유사한 개체 중심 프로그래밍 언어에 대한 기본 지식. C #을 시작 하려면 ["Hello, 세계" 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)를 참조 하세요.

## <a name="overview"></a>개요

내 사용자 공유 대상으로 응용 프로그램을 사용 하도록 설정 하기 위해 수행 해야 하는 세 가지 단계가 있습니다.

1. [응용 프로그램 매니페스트에서 Windows.sharetarget 활성화 계약에 대 한 지원을 선언 합니다.](#declaring-support-for-the-share-contract)
2. [사용자가 앱을 사용 하 여 공유할 수 있는 연락처에 주석을 추가 합니다.](#annotating-contacts)
3. 동시에 실행 되는 응용 프로그램의 여러 인스턴스를 지원 합니다.  사용자는 응용 프로그램의 전체 버전과 상호 작용 하 고 다른 사용자와 공유 하는 데 사용할 수 있어야 합니다. 한 번에 여러 공유 창에서 사용할 수 있습니다. 이를 지원 하기 위해 응용 프로그램은 여러 보기를 동시에 실행할 수 있어야 합니다. 이 작업을 수행 하는 방법에 [대 한 자세한 내용은 "앱에 대 한 여러 뷰 표시"](../design/layout/show-multiple-views.md)문서를 참조 하세요.

이 작업을 완료 하면 응용 프로그램이 다음 두 가지 방법으로 시작할 수 있는 내 사용자 공유 창에서 공유 대상으로 표시 됩니다.
1. 연락처는 공유 참을 통해 선택 됩니다.
2. 작업 표시줄에 고정 된 연락처에서 파일을 끌어서 놓을 수 있습니다.

## <a name="declaring-support-for-the-share-contract"></a>공유 계약에 대 한 지원 선언

응용 프로그램에 대 한 지원을 공유 대상으로 선언 하려면 먼저 Visual Studio에서 응용 프로그램을 엽니다. **솔루션 탐색기**에서 **appxmanifest.xml** 을 마우스 오른쪽 단추로 클릭 하 고 **연결 프로그램**을 선택 합니다. 메뉴에서 **XML (텍스트) 편집기** 를 선택 하 고 **확인**을 클릭 합니다. 그런 다음 매니페스트를 다음과 같이 변경 합니다.


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

이 코드는 모든 파일 및 데이터 형식에 대 한 지원을 추가 하지만 지원 되는 파일 형식 및 데이터 형식을 지정 하도록 선택할 수 있습니다. 자세한 내용은 [windows.sharetarget 클래스 설명서](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) 를 참조 하십시오.

## <a name="annotating-contacts"></a>연락처에 주석 달기

내 사용자 공유 창에서 사용자의 연락처에 대 한 공유 대상으로 응용 프로그램을 표시 하도록 허용 하려면 Windows 연락처 저장소에 기록해 야 합니다. 연락처를 작성 하는 방법에 대 한 자세한 내용은 [Contact Card Integration 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)을 참조 하세요. 

연락처에 공유할 때 응용 프로그램이 내 사용자 공유 대상으로 표시 되 게 하려면 해당 연락처에 주석을 써야 합니다. 주석은 응용 프로그램에서 연락처와 연결 된 데이터의 일부입니다. 주석은 해당 **Providerproperties** 멤버에서 원하는 뷰에 해당 하는 활성화 가능한 클래스를 포함 해야 하며 **공유** 작업에 대 한 지원을 선언 해야 합니다.

앱이 실행 되는 동안 언제 든 지 연락처에 주석을 추가할 수 있지만, 일반적으로 Windows 연락처 저장소에 추가 되는 즉시 연락처에 주석을 추가 해야 합니다.

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

"AppId"는 패키지 제품군 이름 뒤에 '! '가 옵니다. 및 활성화 가능한 클래스 ID입니다. 패키지 제품군 이름을 찾으려면 기본 편집기를 사용 하 여 **appxmanifest.xml** 를 열고 "패키징" 탭을 확인 합니다. 여기서 "App"은 공유 대상 뷰에 해당 하는 활성화 가능한 클래스입니다.

## <a name="running-as-a-my-people-share-target"></a>내 사용자 공유 대상으로 실행

마지막으로, 응용 프로그램을 실행 하려면 앱의 주 클래스에서 [OnShareTargetActivated](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) 메서드를 재정의 하 여 공유 대상 활성화를 처리 합니다. [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) 속성은로 공유 되는 연락처를 포함 하거나, 표준 공유 작업 (내 사용자 공유 아님) 인 경우 비어 있게 됩니다.

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
+ [내 사용자 지원 추가](my-people-support.md)
+ [Windows.sharetarget 클래스](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [연락처 카드 통합 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)