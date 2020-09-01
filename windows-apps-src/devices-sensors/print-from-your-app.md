---
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: 앱에서 인쇄하기
description: 유니버설 Windows 앱에서 문서를 인쇄 하는 방법을 알아봅니다. 또한이 항목에서는 특정 페이지를 인쇄 하는 방법을 보여 줍니다.
ms.date: 01/29/2018
ms.topic: article
keywords: windows 10, uwp, 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: 29f82ce7c9040b6c0a9f826db886d0a164f9f3e3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163237"
---
# <a name="print-from-your-app"></a>앱에서 인쇄하기



**중요 API**

-   [**Windows. Graphics. 인쇄**](/uwp/api/Windows.Graphics.Printing)
-   [**Windows.UI.Xaml.Printing**](/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintDocument**](/uwp/api/Windows.UI.Xaml.Printing.PrintDocument)

유니버설 Windows 앱에서 문서를 인쇄 하는 방법을 알아봅니다. 또한이 항목에서는 특정 페이지를 인쇄 하는 방법을 보여 줍니다. 인쇄 미리 보기 UI에 대 한 고급 변경 사항은 [인쇄 미리 보기 Ui 사용자 지정](customize-the-print-preview-ui.md)을 참조 하세요.

> [!TIP]
> 이 항목에 포함 된 대부분의 예제는 GitHub의 [유니버설 Windows 플랫폼 (uwp) 앱 샘플](https://github.com/Microsoft/Windows-universal-samples) 리포지토리의 일부인 [uwp (유니버설 Windows 플랫폼) 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)을 기반으로 합니다.

## <a name="register-for-printing"></a>인쇄 등록

앱에 인쇄를 추가 하는 첫 번째 단계는 인쇄 계약에 등록 하는 것입니다. 앱은 사용자가 인쇄할 수 있도록 하려는 모든 화면에서이 작업을 수행 해야 합니다. 사용자에 게 표시 되는 화면만 인쇄를 위해 등록할 수 있습니다. 앱의 한 화면에서 인쇄를 위해 등록 한 경우 인쇄를 종료할 때 등록을 취소 해야 합니다. 다른 화면으로 대체 된 경우 다음 화면에서 새 인쇄 계약이 열리면 등록 해야 합니다.

> [!TIP]
> 응용 프로그램에서 둘 이상의 페이지에서 인쇄를 지원 해야 하는 경우에는이 인쇄 코드를 공용 도우미 클래스에 넣고 앱 페이지에서 다시 사용 하도록 할 수 있습니다. 이 작업을 수행 하는 방법에 대 한 예제는 `PrintHelper` [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)에서 클래스를 참조 하세요.

먼저 [**Printmanager**](/uwp/api/Windows.Graphics.Printing.PrintManager) 및 [**PrintDocument**](/uwp/api/Windows.UI.Xaml.Printing.PrintDocument)를 선언 합니다. **Printmanager** 형식은 다른 windows 인쇄 기능을 지원 하기 위해 형식과 함께 [**windows. Graphics. 인쇄**](/uwp/api/Windows.Graphics.Printing) 네임 스페이스에 있습니다. PrintDocument 형식에는 인쇄용 XAML 콘텐츠 준비를 지 원하는 다른 형식과 함께 **PrintDocument** [**네임 스페이스가 있습니다.**](/uwp/api/Windows.UI.Xaml.Printing) 다음 **using** 또는 **Imports** 문을 페이지에 추가 하 여 인쇄 코드를 보다 쉽게 작성할 수 있습니다.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

[**PrintDocument**](/uwp/api/Windows.UI.Xaml.Printing.PrintDocument) 클래스는 앱과 [**printmanager**](/uwp/api/Windows.Graphics.Printing.PrintManager)간의 많은 상호 작용을 처리 하는 데 사용 되지만 자체의 여러 콜백을 노출 합니다. 등록 하는 동안 **Printmanager** 및 **PrintDocument** 의 인스턴스를 만들고 해당 인쇄 이벤트에 대 한 처리기를 등록 합니다.

[UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)에서 등록은 메서드를 통해 수행 됩니다 `RegisterForPrinting` .

```csharp
public virtual void RegisterForPrinting()
{
   printDocument = new PrintDocument();
   printDocumentSource = printDocument.DocumentSource;
   printDocument.Paginate += CreatePrintPreviewPages;
   printDocument.GetPreviewPage += GetPrintPreviewPage;
   printDocument.AddPages += AddPrintPages;

   PrintManager printMan = PrintManager.GetForCurrentView();
   printMan.PrintTaskRequested += PrintTaskRequested;
}
```

사용자가 인쇄를 지 원하는 페이지로 이동 하면 메서드 내에서 등록을 시작 합니다 `OnNavigatedTo` .

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Initialize common helper class and register for printing
   printHelper = new PrintHelper(this);
   printHelper.RegisterForPrinting();

   // Initialize print content for this scenario
   printHelper.PreparePrintContent(new PageToPrint());

   // Tell the user how to print
   MainPage.Current.NotifyUser("Print contract registered with customization, use the Print button to print.", NotifyType.StatusMessage);
}
```

이 샘플에서는 이벤트 처리기가 메서드에서 등록 취소 됩니다 `UnregisterForPrinting` .

```csharp
public virtual void UnregisterForPrinting()
{
    if (printDocument == null)
    {
        return;
    }

    printDocument.Paginate -= CreatePrintPreviewPages;
    printDocument.GetPreviewPage -= GetPrintPreviewPage;
    printDocument.AddPages -= AddPrintPages;

    PrintManager printMan = PrintManager.GetForCurrentView();
    printMan.PrintTaskRequested -= PrintTaskRequested;
}
```

사용자가 인쇄를 지 원하는 페이지를 벗어나면 메서드 내에서 이벤트 처리기의 등록이 취소 됩니다 `OnNavigatedFrom` . 

> [!NOTE]
> 여러 페이지로 된 앱이 있고 인쇄 연결을 끊지 않는 경우 사용자가 페이지를 남기고 나면 예외가 throw 됩니다.

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
   if (printHelper != null)
   {
         printHelper.UnregisterForPrinting();
   }
}
```

## <a name="create-a-print-button"></a>인쇄 단추 만들기

표시 하려는 앱 화면에 인쇄 단추를 추가 합니다. 인쇄 하려는 콘텐츠를 방해 하지 않는지 확인 합니다.

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

다음으로 앱의 코드에 이벤트 처리기를 추가 하 여 click 이벤트를 처리 합니다. [**Showprintuiasync**](/uwp/api/windows.graphics.printing.printmanager.showprintuiasync) 메서드를 사용 하 여 앱에서 인쇄를 시작 합니다. **ShowPrintUIAsync**는 적절한 인쇄 창을 표시하는 비동기 메서드입니다. 인쇄를 지 원하는 장치에서 앱이 실행 되 고 있는지 확인 하기 위해 [**Issupported**](/uwp/api/windows.graphics.printing.printmanager.issupported) 메서드를 먼저 호출 하는 것이 좋습니다 (및 해당 하지 않는 경우 처리). 다른 이유로 인해 인쇄를 수행할 수 없는 경우 **Showprintuiasync** 는 예외를 throw 합니다. 이러한 예외를 catch 하 고 인쇄를 계속할 수 없는 경우 사용자에 게 알리는 것이 좋습니다.

이 예제에서는 단추 클릭에 대한 이벤트 처리기에 인쇄 창이 표시됩니다. 해당 시점에 인쇄를 수행할 수 없으므로 메서드가 예외를 throw 하는 경우 [**Contentdialog**](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) 컨트롤은 사용자에 게 상황을 알려 줍니다.

```csharp
async private void OnPrintButtonClick(object sender, RoutedEventArgs e)
{
    if (Windows.Graphics.Printing.PrintManager.IsSupported())
    {
        try
        {
            // Show print UI
            await Windows.Graphics.Printing.PrintManager.ShowPrintUIAsync();

        }
        catch
        {
            // Printing cannot proceed at this time
            ContentDialog noPrintingDialog = new ContentDialog()
            {
                Title = "Printing error",
                Content = "\nSorry, printing can' t proceed at this time.", PrimaryButtonText = "OK"
            };
            await noPrintingDialog.ShowAsync();
        }
    }
    else
    {
        // Printing is not supported on this device
        ContentDialog noPrintingDialog = new ContentDialog()
        {
            Title = "Printing not supported",
            Content = "\nSorry, printing is not supported on this device.",PrimaryButtonText = "OK"
        };
        await noPrintingDialog.ShowAsync();
    }
}
```

## <a name="format-your-apps-content"></a>앱 콘텐츠 서식 지정

**Showprintuiasync** 가 호출 되 면 [**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 이벤트가 발생 합니다. 이 단계에 표시 된 **PrintTaskRequested** 이벤트 처리기는 [**PrintTaskRequest**](/uwp/api/windows.graphics.printing.printtaskrequest.createprinttask) 메서드를 호출 하 여 [**printtask**](/uwp/api/Windows.Graphics.Printing.PrintTask) 를 만들고 인쇄 페이지의 제목과 [**PrintTaskSourceRequestedHandler**](/uwp/api/windows.graphics.printing.printtask.source) 대리자의 이름을 전달 합니다. 이 예제에서 **PrintTaskSourceRequestedHandler** 는 인라인으로 정의 됩니다. **PrintTaskSourceRequestedHandler** 은 인쇄를 위해 형식이 지정 된 콘텐츠를 제공 하 고 나중에 설명 합니다.

이 예제에서는 오류를 catch 하기 위해 완료 처리기도 정의 됩니다. 응용 프로그램에서 오류가 발생 한 경우 사용자에 게 알릴 수 있으며 가능한 해결 방법을 제공 하기 때문에 완료 이벤트를 처리 하는 것이 좋습니다. 마찬가지로, 앱은 완료 이벤트를 사용 하 여 인쇄 작업이 성공한 후 사용자가 수행할 후속 단계를 나타낼 수 있습니다.

```csharp
protected virtual void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequested =>
   {
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

         sourceRequested.SetSource(printDocumentSource);
   });
}
```

인쇄 작업이 만들어진 후 [**Printmanager**](/uwp/api/Windows.Graphics.Printing.PrintManager) 는 페이지 [**매김**](/uwp/api/windows.ui.xaml.printing.printdocument.paginate) 이벤트를 발생 시켜 인쇄 미리 보기 UI에 표시할 인쇄 페이지 컬렉션을 요청 합니다. 이는 IPrintPreviewPageCollection 인터페이스의 **IPrintPreviewPageCollection** **메서드에 해당** 합니다. 등록할 때 만든 이벤트 처리기가 현재 호출 됩니다.

> [!IMPORTANT]
> 사용자가 인쇄 설정을 변경 하는 경우에는 콘텐츠를 다시 흐르게 할 수 있도록 [인쇄] 이벤트 처리기가 다시 호출 됩니다. 최상의 사용자 환경을 위해 콘텐츠를 다시 설정 하기 전에 설정을 확인 하 고 필요 하지 않은 경우 페이지가 매겨진 콘텐츠를 다시 초기화 하지 않는 것이 좋습니다.

페이지 [**매김**](/uwp/api/windows.ui.xaml.printing.printdocument.paginate) 이벤트 처리기 ( `CreatePrintPreviewPages` [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)의 메서드)에서 인쇄 미리 보기 UI에 표시 하 고 프린터로 보낼 페이지를 만듭니다. 앱의 인쇄용 콘텐츠를 준비 하는 데 사용 하는 코드는 사용자의 앱 및 인쇄 콘텐츠에 따라 다릅니다. 인쇄용으로 콘텐츠의 서식을 지정 하는 방법은 [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) 소스 코드를 참조 하세요.

```csharp
protected virtual void CreatePrintPreviewPages(object sender, PaginateEventArgs e)
{
   // Clear the cache of preview pages
   printPreviewPages.Clear();

   // Clear the print canvas of preview pages
   PrintCanvas.Children.Clear();

   // This variable keeps track of the last RichTextBlockOverflow element that was added to a page which will be printed
   RichTextBlockOverflow lastRTBOOnPage;

   // Get the PrintTaskOptions
   PrintTaskOptions printingOptions = ((PrintTaskOptions)e.PrintTaskOptions);

   // Get the page description to deterimine how big the page is
   PrintPageDescription pageDescription = printingOptions.GetPageDescription(0);

   // We know there is at least one page to be printed. passing null as the first parameter to
   // AddOnePrintPreviewPage tells the function to add the first page.
   lastRTBOOnPage = AddOnePrintPreviewPage(null, pageDescription);

   // We know there are more pages to be added as long as the last RichTextBoxOverflow added to a print preview
   // page has extra content
   while (lastRTBOOnPage.HasOverflowContent && lastRTBOOnPage.Visibility == Windows.UI.Xaml.Visibility.Visible)
   {
         lastRTBOOnPage = AddOnePrintPreviewPage(lastRTBOOnPage, pageDescription);
   }

   if (PreviewPagesCreated != null)
   {
         PreviewPagesCreated.Invoke(printPreviewPages, null);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Report the number of preview pages created
   printDoc.SetPreviewPageCount(printPreviewPages.Count, PreviewPageCountType.Intermediate);
}
```

인쇄 미리 보기 창에 특정 페이지를 표시 하는 경우 [**Printmanager**](/uwp/api/Windows.Graphics.Printing.PrintManager) 는 [**GetPreviewPage**](/uwp/api/windows.ui.xaml.printing.printdocument.getpreviewpage) 이벤트를 발생 시킵니다. 이는 **IPrintPreviewPageCollection** 인터페이스의 **makepage** 메서드에 해당 합니다. 등록할 때 만든 이벤트 처리기가 현재 호출 됩니다.

[**GetPreviewPage**](/uwp/api/windows.ui.xaml.printing.printdocument.getpreviewpage) 이벤트 처리기 ( `GetPrintPreviewPage` [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)의 메서드)에서 인쇄 문서에 해당 하는 페이지를 설정 합니다.

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

마지막으로, 사용자가 인쇄 단추를 클릭 하면 [**Printmanager**](/uwp/api/Windows.Graphics.Printing.PrintManager) 가 **Idocumentpagesource** 인터페이스의 **makedocument** 메서드를 호출 하 여 프린터로 보낼 최종 페이지 컬렉션을 요청 합니다. XAML에서는 [**Addpages**](/uwp/api/windows.ui.xaml.printing.printdocument.addpages) 이벤트를 발생 시킵니다. 등록할 때 만든 이벤트 처리기가 현재 호출 됩니다.

[**AddPages**](/uwp/api/windows.ui.xaml.printing.printdocument.addpages) 이벤트 처리기([UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)의 `AddPrintPages` 메서드)에서 페이지 모음의 페이지를 프린터로 보낼 [**PrintDocument**](/uwp/api/Windows.UI.Xaml.Printing.PrintDocument) 개체에 추가합니다. 사용자가 특정 페이지 또는 인쇄할 페이지 범위를 지정 하는 경우 여기에서 해당 정보를 사용 하 여 실제로 프린터로 전송 될 페이지만 추가 합니다.

```csharp
protected virtual void AddPrintPages(object sender, AddPagesEventArgs e)
{
   // Loop over all of the preview pages and add each one to  add each page to be printied
   for (int i = 0; i < printPreviewPages.Count; i++)
   {
         // We should have all pages ready at this point...
         printDocument.AddPage(printPreviewPages[i]);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Indicate that all of the print pages have been provided
   printDoc.AddPagesComplete();
}
```

## <a name="prepare-print-options"></a>인쇄 옵션 준비

다음 인쇄 옵션을 준비 합니다. 예를 들어이 섹션에서는 특정 페이지 인쇄를 허용 하도록 페이지 범위 옵션을 설정 하는 방법을 설명 합니다. 고급 옵션 [은 인쇄 미리 보기 UI 사용자 지정](customize-the-print-preview-ui.md)을 참조 하세요.

이 단계에서는 새 인쇄 옵션을 만들고,이 옵션으로 지원 되는 값 목록을 정의한 후 인쇄 미리 보기 UI에 옵션을 추가 합니다. 페이지 범위 옵션에는 다음과 같은 설정이 있습니다.

| 옵션 이름          | 작업 |
|----------------------|--------|
| **모두 인쇄**        | 문서의 모든 페이지를 인쇄 합니다.|
| **선택 영역 인쇄**  | 사용자가 선택한 콘텐츠만 인쇄 합니다.|
| **인쇄 범위**      | 사용자가 인쇄할 페이지를 입력할 수 있는 편집 컨트롤을 표시 합니다.|

먼저 [**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 이벤트 처리기를 수정 하 여 [**PrintTaskOptionDetails**](/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails) 개체를 가져오는 코드를 추가 합니다.

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

인쇄 미리 보기 UI에 표시 되는 옵션 목록을 지우고 사용자가 앱에서 인쇄 하려는 경우 표시 하려는 옵션을 추가 합니다.

> [!NOTE]
> 옵션은 추가 된 순서 대로 인쇄 미리 보기 UI에 표시 되며, 첫 번째 옵션은 창 위쪽에 표시 됩니다.

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

새 인쇄 옵션을 만들고 옵션 값 목록을 초기화 합니다.

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

사용자 지정 인쇄 옵션을 추가하고 이벤트 처리기를 할당합니다. 옵션 목록의 맨 아래에 표시 되도록 사용자 지정 옵션이 마지막에 추가 됩니다. 하지만 사용자 지정 인쇄 옵션은 마지막에 추가 하지 않아도 됩니다.

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

[**CreateTextOption**](/uwp/api/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption) 메서드는 **범위** 텍스트 상자를 만듭니다. 사용자가 **인쇄 범위** 옵션을 선택할 때 인쇄 하려는 특정 페이지를 입력 하는 위치입니다.

## <a name="handle-print-option-changes"></a>인쇄 옵션 변경 내용 처리

기능 **변경** 이벤트 처리기는 두 가지 작업을 수행 합니다. 먼저 사용자가 선택한 페이지 범위 옵션에 따라 페이지 범위에 대 한 텍스트 편집 필드를 표시 하 고 숨깁니다. 둘째, 페이지 범위 텍스트 상자에 입력 된 텍스트를 테스트 하 여 문서에 대해 유효한 페이지 범위를 나타내는지 확인 합니다.

이 예제에서는 [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)에서 인쇄 옵션 변경 이벤트를 처리 하는 방법을 보여 줍니다.

```csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   if (args.OptionId == null)
   {
         return;
   }

   string optionId = args.OptionId.ToString();

   // Handle change in Page Range Option
   if (optionId == "PageRange")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         string pageRangeValue = pageRange.Value.ToString();

         selectionMode = false;

         switch (pageRangeValue)
         {
            case "PrintRange":
               // Add PageRangeEdit custom option to the option list
               sender.DisplayedOptions.Add("PageRangeEdit");
               pageRangeEditVisible = true;
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               break;
            case "PrintSelection":
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     Scenario4PageRange page = (Scenario4PageRange)scenarioPage;
                     PageToPrint pageContent = (PageToPrint)page.PrintFrame.Content;
                     ShowContent(pageContent.TextContentBlock.SelectedText);
               });
               RemovePageRangeEdit(sender);
               break;
            default:
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               RemovePageRangeEdit(sender);
               break;
         }

         Refresh();
   }
   else if (optionId == "PageRangeEdit")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         // Expected range format (p1,p2...)*, (p3-p9)* ...
         if (!Regex.IsMatch(pageRange.Value.ToString(), @"^\s*\d+\s*(\-\s*\d+\s*)?(\,\s*\d+\s*(\-\s*\d+\s*)?)*$"))
         {
            pageRange.ErrorText = "Invalid Page Range (eg: 1-3, 5)";
         }
         else
         {
            pageRange.ErrorText = string.Empty;
            try
            {
               GetPagesInRange(pageRange.Value.ToString());
               Refresh();
            }
            catch (InvalidPageException ipex)
            {
               pageRange.ErrorText = ipex.Message;
            }
         }
   }
}
```

> [!TIP]
> `GetPagesInRange`사용자가 범위 텍스트 상자에 입력 한 페이지 범위를 구문 분석 하는 방법에 대 한 자세한 내용은 [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) 에서 메서드를 참조 하세요.

## <a name="preview-selected-pages"></a>선택한 페이지 미리 보기

앱 콘텐츠를 인쇄 하도록 서식 지정 하는 방법은 앱과 해당 콘텐츠의 특성에 따라 다릅니다. [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) 에서 인쇄를 위해 콘텐츠의 서식을 지정 하는 데 사용 되는 인쇄 도우미 클래스입니다.

페이지의 하위 집합을 인쇄할 때 인쇄 미리 보기에서 콘텐츠를 표시 하는 방법에는 여러 가지가 있습니다. 인쇄 미리 보기에서 페이지 범위를 표시 하도록 선택한 방법에 관계 없이 인쇄 된 출력에는 선택한 페이지만 포함 되어야 합니다.

-   인쇄 미리 보기에서 페이지 범위를 지정 했는지 여부에 관계 없이 모든 페이지를 표시 합니다. 사용자는 실제로 인쇄할 페이지를 알 수 있습니다.
-   인쇄 미리 보기에서 사용자의 페이지 범위에서 선택한 페이지만 표시 하 고 사용자가 페이지 범위를 변경할 때마다 화면 표시를 업데이트 합니다.
-   인쇄 미리 보기의 모든 페이지를 표시 하지만 사용자가 선택한 페이지 범위에 속하지 않는 페이지는 회색으로 표시 됩니다.

## <a name="related-topics"></a>관련 항목

* [인쇄용 디자인 지침](./printing-and-scanning.md)
* [빌드 2015 비디오: Windows 10에서 인쇄 되는 앱 개발](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 인쇄 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)