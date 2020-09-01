---
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: 인쇄 미리 보기 UI 사용자 지정
description: 이 섹션에서는 인쇄 미리 보기 UI에서 인쇄 옵션 및 설정을 사용자 지정 하는 방법을 설명 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: 6b73f595dc4dd6a255a439eecfccf7fce1d61204
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175477"
---
# <a name="customize-the-print-preview-ui"></a>인쇄 미리 보기 UI 사용자 지정



**중요 API**

-   [**Windows. Graphics. 인쇄**](/uwp/api/Windows.Graphics.Printing)
-   [**Windows.UI.Xaml.Printing**](/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintManager**](/uwp/api/Windows.Graphics.Printing.PrintManager)

이 섹션에서는 인쇄 미리 보기 UI에서 인쇄 옵션 및 설정을 사용자 지정 하는 방법을 설명 합니다. 인쇄에 대 한 자세한 내용은 [앱에서 인쇄](print-from-your-app.md)를 참조 하세요.

**팁**    이 항목의 대부분 예제는 인쇄 샘플을 기반으로 합니다. 전체 코드를 보려면 GitHub의 [Windows 유니버설 샘플](https://github.com/Microsoft/Windows-universal-samples) 리포지토리에서 [UWP (유니버설 Windows 플랫폼) 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) 을 다운로드 하세요.

 

## <a name="customize-print-options"></a>인쇄 옵션 사용자 지정

기본적으로 인쇄 미리 보기 UI에는 [**ColorMode**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.colormode), [**복사본**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.copies)및 [**용지 방향**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.orientation) 인쇄 옵션이 표시 됩니다. 그 외에도 인쇄 미리 보기 UI에 추가할 수 있는 몇 가지 다른 일반적인 프린터 옵션이 있습니다.

-   [**바인딩되**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.binding)
-   [**부씩**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.collation)
-   [**이중**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.duplex)
-   [**HolePunch**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.holepunch)
-   [**InputBin**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.inputbin)
-   [**MediaSize**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize)
-   [**유형과**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediatype)
-   [**정리**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.nup)
-   [**PrintQuality**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.printquality)
-   [**스테이플링을**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.staple)

이러한 옵션은 [**StandardPrintTaskOptions**](/uwp/api/Windows.Graphics.Printing.StandardPrintTaskOptions) 클래스에서 정의 됩니다. 인쇄 미리 보기 UI에 표시 되는 옵션 목록에서 옵션을 추가 하거나 제거할 수 있습니다. 표시 되는 순서를 변경 하 고 사용자에 게 표시 되는 기본 설정을 지정할 수도 있습니다.

그러나이 방법을 사용 하 여 변경한 내용은 인쇄 미리 보기 UI에만 영향을 줍니다. 사용자는 인쇄 미리 보기 UI에서 **기타 설정** 을 눌러 프린터가 지 원하는 모든 옵션에 항상 액세스할 수 있습니다.

**참고**    앱에서 표시할 인쇄 옵션을 지정할 수 있지만 선택한 프린터에서 지 원하는 인쇄 옵션은 인쇄 미리 보기 UI에 표시 됩니다. 선택한 프린터가 지원하지 않는 옵션은 인쇄 UI에 표시되지 않습니다.

 

### <a name="define-the-options-to-display"></a>표시할 옵션 정의

앱의 화면이 로드 되 면 인쇄 계약을 등록 합니다. 이 등록의 일부에는 [**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 이벤트 처리기 정의가 포함 됩니다. 인쇄 미리 보기 UI에 표시 되는 옵션을 사용자 지정 하는 코드는 **PrintTaskRequested** 이벤트 처리기에 추가 됩니다.

[**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 이벤트 처리기를 수정 하 여 인쇄 미리 보기 UI에 표시할 인쇄 설정을 구성 하는 옵션 지침을 포함 합니다 [**.**](/uwp/api/windows.graphics.printing.printtask.options) 사용자 지정 된 인쇄 옵션 목록을 표시 하려는 응용 프로그램의 화면에 대해 도우미 클래스의 **PrintTaskRequested** 이벤트 처리기를 재정의 하 여 화면 인쇄 시 표시할 옵션을 지정 하는 코드를 포함 합니다.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**중요**    [**DisplayedOptions**](/uwp/api/windows.graphics.printing.printtaskoptions.displayedoptions)()를 호출 하면 **추가 설정** 링크를 포함 하 여 인쇄 미리 보기 UI에서 인쇄 옵션이 모두 제거 됩니다. 인쇄 미리 보기 UI에 표시 하려는 옵션을 추가 해야 합니다.

### <a name="specify-default-options"></a>기본 옵션 지정

인쇄 미리 보기 UI에서 옵션의 기본값을 설정할 수도 있습니다. 다음 코드 줄은 마지막 예제에서 [**MediaSize**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize) 옵션의 기본값을 설정 합니다.

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>새 인쇄 옵션 추가

이 섹션에서는 새 인쇄 옵션을 만들고, 옵션에서 지 원하는 값 목록을 정의한 후 인쇄 미리 보기에 옵션을 추가 하는 방법을 보여 줍니다. 이전 섹션에서와 같이 [**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 이벤트 처리기에 새 인쇄 옵션을 추가 합니다.

먼저 [**PrintTaskOptionDetails**](/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails) 개체를 가져옵니다. 인쇄 미리 보기 UI에 새 인쇄 옵션을 추가 하는 데 사용 됩니다. 그런 다음 인쇄 미리 보기 UI에 표시 되는 옵션 목록을 지우고 사용자가 앱에서 인쇄 하려는 경우 표시 하려는 옵션을 추가 합니다. 그런 다음 새 인쇄 옵션을 만들고 옵션 값 목록을 초기화 합니다. 마지막으로 새 옵션을 추가 하 고 옵션 **변경** 이벤트에 대 한 처리기를 할당 합니다.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

옵션은 추가 된 순서 대로 인쇄 미리 보기 UI에 표시 되며, 첫 번째 옵션은 창 위쪽에 표시 됩니다. 이 예제에서는 옵션 목록의 맨 아래에 표시 되도록 사용자 지정 옵션이 마지막에 추가 됩니다. 그러나 목록에서 아무 곳에 나 배치할 수 있습니다. 사용자 지정 인쇄 옵션은 마지막에 추가 하지 않아도 됩니다.

사용자가 사용자 지정 옵션에서 선택한 옵션을 변경 하는 경우 인쇄 미리 보기 이미지를 업데이트 합니다. 아래와 같이 [**Invalidatepreview**](/uwp/api/windows.ui.xaml.printing.printdocument.invalidatepreview) 메서드를 호출 하 여 인쇄 미리 보기 UI에서 이미지를 다시 그립니다.

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## <a name="related-topics"></a>관련 항목

* [인쇄용 디자인 지침](./printing-and-scanning.md)
* [빌드 2015 비디오: Windows 10에서 인쇄 되는 앱 개발](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)