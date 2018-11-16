---
description: 이 문서에서는 공유 계약을 사용하여 다른 앱에서 공유된 콘텐츠를 UWP(유니버설 Windows 플랫폼) 앱에서 받는 방법을 설명합니다. 이 공유 계약에서는 사용자가 공유를 호출할 때 앱이 옵션으로 제공될 수 있습니다.
title: 데이터 수신
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d64e4a2d4251aca6bbce39b5f24e3e5f35295b8
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6988602"
---
# <a name="receive-data"></a>데이터 수신



이 문서에서는 공유 계약을 사용하여 다른 앱에서 공유된 콘텐츠를 UWP(유니버설 Windows 플랫폼) 앱에서 받는 방법을 설명합니다. 이 공유 계약에서는 사용자가 공유를 호출할 때 앱이 옵션으로 제공될 수 있습니다.

## <a name="declare-your-app-as-a-share-target"></a>앱을 공유 대상으로 선언

사용자가 공유를 호출하면 시스템에서 가능한 대상 앱 목록을 표시합니다. 앱에서 공유 계약을 지원한다고 선언해야 앱이 목록에 표시될 수 있습니다. 그러면 시스템에서 앱이 콘텐츠를 받을 수 있음을 알 수 있습니다.

1.  매니페스트 파일을 엽니다. **package.appxmanifest**와 유사한 방식으로 호출됩니다.
2.  **선언** 탭을 엽니다.
3.  **사용 가능한 선언** 목록에서 **대상 공유**를 선택하고 **추가**를 선택합니다.

## <a name="choose-file-types-and-formats"></a>파일 유형 및 형식 선택

다음으로 지원하는 파일 형식 및 데이터 서식을 결정하세요. 공유 API는 텍스트, HTML 및 비트맵 등의 여러 가지 표준 형식을 지원합니다. 사용자 지정 파일 형식 및 데이터 형식도 지정할 수 있습니다. 이 경우 원본 앱에서 해당 형식에 대해 알아야 합니다. 그렇지 않으면 앱에서 해당 형식을 사용하여 데이터를 공유할 수 없습니다.

앱에서 처리할 수 있는 형식만 등록하세요. 사용자가 공유를 호출하면 공유할 데이터를 지원하는 대상 앱만 표시됩니다.

파일 형식을 설정하려면

1.  매니페스트 파일을 엽니다. **package.appxmanifest**와 유사한 방식으로 호출됩니다.
2.  **선언** 페이지의 **지원되는 파일 형식** 섹션에서 **새로 추가**를 클릭합니다.
3.  지원할 파일 이름 확장명(예: ".docx")을 입력합니다. 마침표(.)를 포함해야 합니다. 모든 파일 형식을 지원하려면 **SupportsAnyFileType** 확인란을 선택합니다.

데이터 형식을 설정하려면

1.  매니페스트 파일을 엽니다.
2.  **선언** 페이지의 **데이터 형식** 섹션을 열고 **새로 추가**를 클릭합니다.
3.  예를 들어 "텍스트" 등 지원하는 데이터 형식의 이름을 입력합니다.

## <a name="handle-share-activation"></a>공유 활성화 처리

사용자가 공유 UI의 사용 가능한 대상 앱 목록 등에서 앱을 선택하면 [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Application.OnShareTargetActivated(Windows.ApplicationModel.Activation.ShareTargetActivatedEventArgs)) 이벤트가 발생합니다. 앱은 사용자가 공유하려는 데이터를 처리하기 위해 이 이벤트를 처리해야 합니다.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

사용자가 공유하려는 데이터는 [**ShareOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation) 개체에 포함되어 있습니다. 이 개체를 사용하여 포함된 데이터의 형식을 확인할 수 있습니다.

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

## <a name="report-sharing-status"></a>공유 상태 보고

앱에서 공유할 데이터를 처리하는 데 많은 시간이 걸리는 경우도 있습니다. 예로는 파일 또는 이미지의 사용자 공유 컬렉션이 있습니다. 이러한 항목은 간단한 텍스트 공유보다 크기 때문에 처리하는 데 더 오랜 시간이 걸립니다.

```cs
shareOperation.ReportStarted(); 
```

[**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted)를 호출한 후에는 앱과의 사용자 상호 작용이 더 이상 필요하지 않습니다. 따라서 사용자가 앱을 종료해도 되는 경우에만 이 메서드를 호출해야 합니다.

확장 공유에서는 앱이 DataPackage 개체로부터 모든 데이터를 가져오기 전에 사용자가 원본 앱을 해제할 수 있습니다. 따라서 앱에서 필요한 데이터 가져오기를 완료한 경우 이를 시스템에서 알 수 있도록 하는 것이 좋습니다. 그러면 시스템에서 필요에 따라 원본 앱을 일시 중단하거나 종료할 수 있습니다.

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

오류가 발생한 경우 [**ReportError**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportError(System.String))를 호출하여 시스템에 오류 메시지를 보내세요. 사용자가 공유 상태를 확인할 때 메시지가 표시됩니다. 이때 앱이 종료되고 공유가 끝나므로 사용자가 다시 시작하여 콘텐츠를 앱에 공유해야 합니다. 시나리오에 따라 특정 오류는 공유 작업을 종료할 만큼 심각하지 않다고 결정할 수 있습니다. 이 경우 **ReportError**를 호출하지 않고 공유를 계속할 수 있습니다.

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

앱에서 공유 콘텐츠를 성공적으로 처리하면 [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted)를 호출하여 시스템에 알려야 합니다.

```cs
shareOperation.ReportCompleted();
```

이러한 메서드를 사용할 때는 일반적으로 설명된 순서대로 메서드를 호출하고 두 번 이상 호출하지 않습니다. 그러나 대상 앱이 [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted) 전에 [**ReportDataRetrieved**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportDataRetrieved)를 호출할 수 있는 경우가 있습니다. 예를 들어 앱은 활성화 처리기의 작업의 일부로 데이터를 검색할 수 있지만 사용자가 **공유** 단추를 선택할 때까지 **ReportStarted**를 호출하지 않습니다.

## <a name="return-a-quicklink-if-sharing-was-successful"></a>공유가 성공한 경우 QuickLink 반환

사용자가 콘텐츠를 받기 위해 앱을 선택한 경우 [**QuickLink**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink)를 만드는 것이 좋습니다. **QuickLink**는 사용자가 앱과 정보를 쉽게 공유할 수 있도록 해주는 바로 가기와 비슷합니다. 예를 들어 친구의 메일 주소로 미리 구성된 새 메일 메시지를 여는 **QuickLink**를 만들 수 있습니다.

**QuickLink**에는 제목, 아이콘 및 ID가 있어야 합니다. 사용자가 공유 참을 탭하면 제목(예: "엄마에게 전자 메일 보내기")과 아이콘이 표시됩니다. ID는 앱에서 메일 주소, 로그인 자격 증명 등과 같은 사용자 지정 정보에 액세스하는 데 사용됩니다. 앱에서 **QuickLink**를 만들 때 [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted)를 호출하여 **QuickLink**를 시스템에 반환합니다.

**QuickLink**에서는 실제로 데이터를 저장하지 않습니다. 대신 선택한 경우에 앱으로 전송되는 식별자를 포함합니다. 앱에서 **QuickLink**의 ID와 해당 사용자 데이터를 저장해야 합니다. 사용자가 **QuickLink**를 탭할 때 [**QuickLinkId**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.QuickLinkId) 속성을 통해 해당 ID를 가져올 수 있습니다.

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
* [OnShareTargetActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onsharetargetactivated.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [ReportError](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror.aspx)
* [ReportCompleted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted.aspx)
* [ReportDataRetrieved](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [QuickLink](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.aspx)
* [QuickLInkId](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.id.aspx)
