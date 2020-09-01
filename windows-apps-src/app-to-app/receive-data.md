---
description: 이 문서에서는 공유 계약을 사용 하 여 다른 앱에서 공유 되는 유니버설 Windows 플랫폼 (UWP) 앱에서 콘텐츠를 수신 하는 방법을 설명 합니다. 이 공유 계약에서는 사용자가 공유를 호출할 때 앱이 옵션으로 제공될 수 있습니다.
title: 데이터 수신
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2bf345f3b8f72043c72f47d681aa3ed174eb0dc5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175747"
---
# <a name="receive-data"></a>데이터 수신



이 문서에서는 공유 계약을 사용 하 여 다른 앱에서 공유 되는 유니버설 Windows 플랫폼 (UWP) 앱에서 콘텐츠를 수신 하는 방법을 설명 합니다. 이 공유 계약에서는 사용자가 공유를 호출할 때 앱이 옵션으로 제공될 수 있습니다.

## <a name="declare-your-app-as-a-share-target"></a>응용 프로그램을 공유 대상으로 선언

사용자가 공유를 호출 하면 시스템에서 가능한 대상 앱 목록을 표시 합니다. 앱이 목록에 표시 되려면 공유 계약을 지원 하도록 앱을 선언 해야 합니다. 이렇게 하면 응용 프로그램에서 콘텐츠를 받을 수 있음을 시스템에서 알 수 있습니다.

1.  매니페스트 파일을 엽니다. **Appxmanifest.xml**와 같이 호출 되어야 합니다.
2.  **선언** 탭을 엽니다.
3.  **사용 가능한 선언** 목록에서 **공유 대상** 을 선택 하 고 **추가**를 선택 합니다.

## <a name="choose-file-types-and-formats"></a>파일 형식 및 형식 선택

다음으로, 지원 되는 파일 형식 및 데이터 형식을 결정 합니다. 공유 Api는 텍스트, HTML 및 비트맵과 같은 여러 가지 표준 형식을 지원 합니다. 또한 사용자 지정 파일 형식 및 데이터 형식을 지정할 수 있습니다. 이 경우 원본 앱은 이러한 형식 및 형식을 알아야 합니다. 그렇지 않으면 해당 앱은 형식을 사용 하 여 데이터를 공유할 수 없습니다.

앱에서 처리할 수 있는 형식만 등록 합니다. 공유 되는 데이터를 지 원하는 대상 앱만 사용자가 공유를 호출할 때 표시 됩니다.

파일 형식을 설정 하려면:

1.  매니페스트 파일을 엽니다. **Appxmanifest.xml**와 같이 호출 되어야 합니다.
2.  **선언** 페이지의 **지원 되는 파일 형식** 섹션에서 **새로 추가**를 선택 합니다.
3.  지원 하려는 파일 이름 확장명을 입력 합니다 (예: ".docx"). 기간을 포함 해야 합니다. 모든 파일 형식을 지원 하려면 **Supportsanyfiletype** 확인란을 선택 합니다.

데이터 형식을 설정 하려면:

1.  매니페스트 파일을 엽니다.
2.  **선언** 페이지의 **데이터 형식** 섹션을 열고 **새로 추가**를 선택 합니다.
3.  지원 되는 데이터 형식의 이름을 입력 합니다 (예: "Text").

## <a name="handle-share-activation"></a>공유 활성화 처리

사용자가 앱을 선택 하면 (일반적으로 공유 UI에서 사용 가능한 대상 앱 목록에서 선택 하 여) [**OnShareTargetActivated**](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) 이벤트가 발생 합니다. 앱은 사용자가 공유 하려는 데이터를 처리 하기 위해이 이벤트를 처리 해야 합니다.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

사용자가 공유 하려는 데이터는 [**ShareOperation**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation) 개체에 포함 되어 있습니다. 이 개체를 사용 하 여 포함 된 데이터의 형식을 확인할 수 있습니다.

```cs
ShareOperation shareOperation = args.ShareOperation;
if (shareOperation.Data.Contains(StandardDataFormats.Text))
{
    string text = await shareOperation.Data.GetTextAsync();

    // To output the text from this example, you need a TextBlock control
    // with a name of "sharedContent".
    sharedContent.Text = "Text: " + text;
} 
```

## <a name="report-sharing-status"></a>보고서 공유 상태

경우에 따라 앱에서 공유 하려는 데이터를 처리 하는 데 시간이 걸릴 수 있습니다. 예를 들어 파일 또는 이미지의 컬렉션을 공유 하는 사용자가 있습니다. 이러한 항목은 간단한 텍스트 문자열 보다 크므로 처리 하는 데 시간이 더 오래 걸립니다.

```cs
shareOperation.ReportStarted(); 
```

[**ReportStarted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)를 호출한 후에는 앱에 대 한 사용자 조작이 더 이상 필요 하지 않습니다. 따라서 앱이 사용자가 해제할 수 있는 지점에 있지 않는 한이를 호출 하면 안 됩니다.

확장 공유를 사용 하면 앱이 DataPackage 개체의 모든 데이터를 포함 하기 전에 사용자가 원본 앱을 해제할 수 있습니다. 따라서 앱이 필요한 데이터를 획득 한 경우 시스템에 알려 주는 것이 좋습니다. 이러한 방식으로 시스템은 필요에 따라 원본 앱을 일시 중단 하거나 종료할 수 있습니다.

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

문제가 발생 하는 경우 [**ReportError**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation#Windows_ApplicationModel_DataTransfer_ShareTarget_ShareOperation_ReportError_System_String_) 를 호출 하 여 시스템에 오류 메시지를 보냅니다. 사용자가 공유 상태를 확인할 때 메시지가 표시 됩니다. 그러면 앱이 종료 되 고 공유가 종료 됩니다. 사용자가 앱에 콘텐츠를 공유 하려면 다시 시작 해야 합니다. 시나리오에 따라 특정 오류가 공유 작업을 종료 하는 데 충분 하지 않다고 결정할 수 있습니다. 이 경우 **ReportError** 를 호출 하지 않고 공유를 계속 진행 하도록 선택할 수 있습니다.

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

마지막으로 앱이 공유 콘텐츠를 성공적으로 처리 한 경우 [**ReportCompleted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted) 를 호출 하 여 시스템에서 알 수 있도록 해야 합니다.

```cs
shareOperation.ReportCompleted();
```

이러한 메서드를 사용 하는 경우 일반적으로 설명 된 순서 대로 호출 하 고 두 번 이상 호출 하지 않습니다. 그러나 대상 앱이 [**ReportStarted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)전에 [**검색 된 reportdataretrieved**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved) 를 호출할 수 있는 경우가 있습니다. 예를 들어 앱은 활성화 처리기에서 작업의 일부로 데이터를 검색할 수 있지만 사용자가 **공유** 단추를 선택할 때까지 **ReportStarted** 를 호출 하지 않습니다.

## <a name="return-a-quicklink-if-sharing-was-successful"></a>공유가 성공한 경우 QuickLink 반환

사용자가 앱을 선택 하 여 콘텐츠를 수신 하는 경우 [**QuickLink**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink)를 만드는 것이 좋습니다. **QuickLink** 는 사용자가 앱과 정보를 쉽게 공유할 수 있도록 하는 바로 가기와 비슷합니다. 예를 들어 친구의 이메일 주소로 미리 구성 된 새 메일 메시지를 여는 **QuickLink** 을 만들 수 있습니다.

**QuickLink** 에는 제목, 아이콘 및 Id가 있어야 합니다. 사용자가 공유 참을 누르면 제목 (예: "이메일 Mom") 및 아이콘이 표시 됩니다. Id는 앱에서 전자 메일 주소 또는 로그인 자격 증명과 같은 사용자 지정 정보에 액세스 하는 데 사용 됩니다. 앱에서 **QuickLink**를 만들 때 앱은 [**ReportCompleted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted)을 호출 하 여 시스템에 **QuickLink** 을 반환 합니다.

**QuickLink** 는 실제로 데이터를 저장 하지 않습니다. 대신, 선택 하는 경우 앱에 전송 되는 식별자가 포함 됩니다. 앱은 **QuickLink** 의 Id와 해당 사용자 데이터를 저장 하는 일을 담당 합니다. 사용자가 **QuickLink**를 탭 하면 [**QuickLinkId**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.quicklinkid) 속성을 통해 해당 Id를 가져올 수 있습니다.

```cs
async void ReportCompleted(ShareOperation shareOperation, string quickLinkId, string quickLinkTitle)
{
    QuickLink quickLinkInfo = new QuickLink
    {
        Id = quickLinkId,
        Title = quickLinkTitle,

        // For quicklinks, the supported FileTypes and DataFormats are set 
        // independently from the manifest
        SupportedFileTypes = { "*" },
        SupportedDataFormats = { StandardDataFormats.Text, StandardDataFormats.Uri, 
                StandardDataFormats.Bitmap, StandardDataFormats.StorageItems }
    };

    StorageFile iconFile = await Windows.ApplicationModel.Package.Current.InstalledLocation.CreateFileAsync(
            "assets\\user.png", CreationCollisionOption.OpenIfExists);
    quickLinkInfo.Thumbnail = RandomAccessStreamReference.CreateFromFile(iconFile);
    shareOperation.ReportCompleted(quickLinkInfo);
}
```

## <a name="see-also"></a>참고 항목 

* [앱 간 통신](index.md)
* [데이터 공유](share-data.md)
* [OnShareTargetActivated](/uwp/api/windows.ui.xaml.application.onsharetargetactivated)
* [ReportStarted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)
* [ReportError](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror)
* [ReportCompleted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted)
* [ReportDataRetrieved 됨](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved)
* [ReportStarted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)
* [QuickLink](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.quicklink)
* [QuickLInkId](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.quicklink.id)