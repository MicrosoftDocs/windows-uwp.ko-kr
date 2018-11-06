---
author: PatrickFarley
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: 앱에서 인쇄하기
description: 유니버설 Windows 앱에서 문서를 인쇄하는 방법을 알아봅니다. 이 항목에서는 특정 페이지를 인쇄하는 방법도 보여 줍니다.
ms.author: pafarley
ms.date: 01/29/2018
ms.topic: article
keywords: windows 10, uwp, 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: b35d11e9dcf1e79296e0eeaff85c975c24d65920
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6041094"
---
# <a name="print-from-your-app"></a>앱에서 인쇄하기



**중요 API**

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)

유니버설 Windows 앱에서 문서를 인쇄하는 방법을 알아봅니다. 이 항목에서는 특정 페이지를 인쇄하는 방법도 보여 줍니다. 인쇄 미리 보기 UI에 대한 고급 변경에 대한 자세한 내용은 [인쇄 미리 보기 UI 사용자 지정](customize-the-print-preview-ui.md)을 참조하세요.

> [!TIP]
> 이 항목의 예제는 대부분 인쇄 샘플을 기반으로 합니다. 전체 코드를 보려면 GitHub의 [Windows-universal-samples repo](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 [Universal Windows Platform (UWP) print sample](http://go.microsoft.com/fwlink/p/?LinkId=619984)을 다운로드하세요.

## <a name="register-for-printing"></a>인쇄 등록

앱에 인쇄 기능을 추가하는 첫 번째 단계는 인쇄 계약을 등록하는 것입니다. 사용자가 인쇄할 수 있도록 모든 화면에 대해 앱이 이 작업을 수행해야 합니다. 사용자에게 표시되는 화면에서만 인쇄를 등록 할 수 있습니다. 앱의 한 화면에서 인쇄를 등록한 경우 이 화면을 나갈 때 인쇄 등록을 취소해야 합니다. 이 화면이 다른 화면으로 바뀔 경우 다음 화면이 열릴 때 새 인쇄 계약을 등록해야 합니다.

> [!TIP]
> 앱에서 하나 이상의 페이지 인쇄를 지원해야 하는 경우, 이 인쇄 코드를 공통 도우미 클래스에 넣고 앱 페이지에서 이를 재사용할 수 있습니다. 이 작업을 수행하는 방법의 예제를 보려면 [UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)에서 `PrintHelper` 클래스를 참조하세요.

먼저 [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) 및 [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)를 선언합니다. **PrintManager** 유형은 다른 Windows 인쇄 기능을 지원하는 유형과 함께 [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489) 네임스페이스에 있습니다. **PrintDocument** 유형은 인쇄용 XAML 콘텐츠 준비를 지원하는 다른 유형과 함께 [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325) 네임스페이스에 있습니다. 페이지에 다음 **using** 또는 **Imports** 문을 추가하여 인쇄 코드를 쉽게 작성할 수 있습니다.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

[**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) 클래스는 앱과 [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) 간의 많은 상호 작용을 처리하는 데 사용되지만 여러 가지 자체 콜백을 표시합니다. 등록하는 동안 **PrintManager** 및 **PrintDocument**의 인스턴스를 만들고 해당 인쇄 이벤트에 대해 처리기를 등록합니다.

[UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)에서는 `RegisterForPrinting` 메서드를 통해 등록이 수행됩니다.

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

사용자가 인쇄를 지원하는 페이지로 이동하면 `OnNavigatedTo` 메서드 내에서 등록이 시작됩니다.

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

사용자가 페이지를 나갈 때 인쇄 이벤트 처리기 연결을 끊습니다. 다중 페이지 앱에서 인쇄 연결을 끊지 않고 사용자가 페이지에서 나갔다가 다시 돌아오면 예외가 발생합니다.

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

인쇄 단추를 표시하려는 앱 화면에 추가합니다. 인쇄 단추가 인쇄할 콘텐츠에 방해가 되지 않도록 합니다.

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

다음으로, 클릭 이벤트를 처리할 이벤트 처리기를 앱 코드에 추가합니다. [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printmanager.showprintuiasync) 메서드를 사용하여 인쇄에서 인쇄를 시작합니다. **ShowPrintUIAsync**는 적절한 인쇄 창을 표시하는 비동기 메서드입니다. 앱이 인쇄를 지원하는 디바이스에서 실행 중인지 확인하고 지원하지 않는 경우 이를 처리하려면 먼저 [**IsSupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Printing.PrintManager.IsSupported) 메서드를 호출하는 것이 좋습니다. 다른 이유로 인쇄를 수행할 수 없는 경우 **ShowPrintUIAsync**에 예외가 발생합니다. 이러한 예외를 catch하고 인쇄를 계속 진행할 수 없을 때 사용자에게 알려 주는 것이 좋습니다.

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

이 예제에서는 단추 클릭에 대한 이벤트 처리기에 인쇄 창이 표시됩니다. 당시에 인쇄를 수행할 수 없기 때문에 메서드가 예외를 throw한 경우 [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/Dn633972) 컨트롤은 사용자에게 상황을 알려 줍니다.

## <a name="format-your-apps-content"></a>앱 콘텐츠 형식 지정

**ShowPrintUIAsync**가 호출되면 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 이벤트가 발생합니다. 이 단계에 나오는 **PrintTaskRequested** 이벤트 처리기는 [**PrintTaskRequest.CreatePrintTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtaskrequest.createprinttask.aspx) 메서드를 호출하여 [**PrintTask**](https://msdn.microsoft.com/library/windows/apps/BR226436)를 만들고 인쇄 페이지의 제목과 [**PrintTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtask.source) 대리자의 이름을 전달합니다. 이 예제에서 **PrintTaskSourceRequestedHandler**는 인라인으로 정의되어 있습니다. **PrintTaskSourceRequestedHandler**는 인쇄용 콘텐츠를 제공하고 나중에 설명됩니다.

이 예제에서는 완료 처리기가 catch 오류에도 정의되어 있습니다. 완료 이벤트를 처리하는 것은 오류가 발생했을 때 이를 사용자에게 알리고 가능한 해결 방법도 제공할 수 있으므로 좋은 방법입니다. 또한 인쇄 작업이 성공한 후 사용자가 수행할 다음 단계를 안내하는 데도 완료 이벤트를 사용할 수 있습니다.

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

인쇄 작업이 만들어진 후 [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426)는 [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate) 이벤트를 발생시켜 인쇄 미리 보기 UI에 인쇄 페이지 모음을 표시하도록 요청합니다. 이는 **IPrintPreviewPageCollection** 인터페이스의 **Paginate** 메서드에 해당합니다. 이때는 등록하는 동안 만든 이벤트 처리기가 호출됩니다.

> [!IMPORTANT]
> 사용자가 인쇄 설정을 변경한 경우에는 콘텐츠를 재배치할 수 있도록 페이지 매김 이벤트 처리기가 다시 호출됩니다. 최상의 사용자 환경을 위해 콘텐츠를 재배치하기 전에 설정을 확인하고 필요 없을 때 페이지가 매겨진 콘텐츠를 다시 초기화하지 않는 것이 좋습니다.

[**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate) 이벤트 처리기([UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)의 `CreatePrintPreviewPages` 메서드)에서 인쇄 미리 보기 UI에 표시되고 프린터로 전송되는 페이지를 만듭니다. 인쇄용 앱 콘텐츠를 준비하는 데 사용되는 코드는 앱 및 인쇄할 콘텐츠마다 다릅니다. 인쇄용 콘텐츠의 형식을 지정하는 방법을 보려면 [UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984) 소스 코드를 참조하세요.

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

특정 페이지를 인쇄 미리 보기 창에 표시해야 하는 경우 [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426)는 [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage) 이벤트를 발생시킵니다. 이는 **IPrintPreviewPageCollection** 인터페이스의 **MakePage** 메서드에 해당합니다. 이때는 등록하는 동안 만든 이벤트 처리기가 호출됩니다.

[**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage) 이벤트 처리기([UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)의 `GetPrintPreviewPage` 메서드)에서 인쇄 문서에 대한 적절한 페이지를 설정합니다.

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

마지막으로 사용자가 인쇄 단추를 클릭하면 [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426)에서 **IDocumentPageSource** 인터페이스의 **MakeDocument** 메서드를 호출하여 프린터로 전송할 페이지의 최종 컬렉션을 요청합니다. XAML에서는 [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages) 이벤트를 발생시킵니다. 이때는 등록하는 동안 만든 이벤트 처리기가 호출됩니다.

[**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages) 이벤트 처리기([UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)의 `AddPrintPages` 메서드)에서 페이지 모음의 페이지를 프린터로 보낼 [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) 개체에 추가합니다. 사용자가 특정 페이지 또는 페이지 범위를 인쇄하도록 지정하면 해당 정보를 사용하여 프린터로 실제로 보낼 페이지만 추가합니다.

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

이제 인쇄 옵션을 준비합니다. 예를 들어 이 섹션에서는 특정 페이지를 인쇄할 수 있도록 페이지 범위 옵션을 설정하는 방법을 설명합니다. 고급 옵션에 대한 자세한 내용은 [인쇄 미리 보기 UI 사용자 지정](customize-the-print-preview-ui.md)을 참조하세요.

이 단계에서는 새 인쇄 옵션을 만들고, 옵션이 지원하는 값 목록을 정의한 다음 이 옵션을 인쇄 미리 보기 UI에 추가합니다. 페이지 범위 옵션의 설정은 다음과 같습니다.

| 옵션 이름          | 작업 |
|----------------------|--------|
| **모두 인쇄**        | 문서의 모든 페이지를 인쇄합니다.|
| **인쇄 선택**  | 사용자가 선택한 콘텐츠만 인쇄합니다.|
| **인쇄 범위**      | 사용자가 인쇄할 페이지를 입력할 수 있는 편집 컨트롤을 표시합니다.|

먼저 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 이벤트 처리기를 수정하여 [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256) 개체를 가져오는 코드를 추가합니다.

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

인쇄 미리 보기 UI에 표시된 옵션 목록을 지우고 사용자가 앱에서 인쇄를 하고자 할 때 표시할 옵션을 추가합니다.

> [!NOTE]
> 추가된 순서대로 첫 번째 옵션이 창의 맨 위에 오도록 인쇄 미리 보기 UI에 옵션이 표시됩니다.

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

새 인쇄 옵션을 만들고 옵션 값 목록을 초기화합니다.

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

사용자 지정 인쇄 옵션을 추가하고 이벤트 처리기를 할당합니다. 사용자 지정 옵션이 옵션 목록의 맨 아래에 나타나도록 마지막에 추가됩니다. 그러나 사용자 지정 인쇄 옵션은 마지막에 추가할 필요가 없으며 목록에서 원하는 곳에 배치할 수 있습니다.

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

[**CreateTextOption**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption) 메서드는 **범위** 입력란을 만듭니다. 이 텍스트 상자에는 사용자가 **인쇄 범위** 옵션을 선택할 때 인쇄할 특정 페이지를 입력합니다.

## <a name="handle-print-option-changes"></a>인쇄 옵션 변경 처리

**OptionChanged** 이벤트 처리기는 두 가지 기능을 담당합니다. 첫째, 사용자가 선택한 페이지 범위 옵션에 따라 페이지 범위의 텍스트 편집 필드를 표시하거나 숨깁니다. 둘째, 페이지 범위 텍스트 상자에 입력된 텍스트가 문서에서 유효한 페이지 범위를 나타내는지 테스트합니다.

이 예제에서는 [UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)이 변경 이벤트를 처리하는 방법을 보여 줍니다.

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
> 사용자가 범위 입력란에 입력하는 페이지 범위를 구문 분석하는 방법은 [UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)의 `GetPagesInRange` 메서드를 참조하세요.

## <a name="preview-selected-pages"></a>선택한 페이지 미리 보기

앱의 인쇄용 콘텐츠 형식을 지정하는 방법은 앱 및 앱 콘텐츠의 특성에 따라 다릅니다. [UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)은 인쇄 도우미 클래스를 사용하여 인쇄할 콘텐츠 형식을 지정합니다.

페이지의 하위 집합을 인쇄할 때 인쇄 미리 보기에서 콘텐츠를 표시하는 여러 가지 방법이 있습니다. 인쇄 미리 보기에 페이지 범위를 표시하기 위해 선택한 방법에 관계없이 인쇄 출력에는 선택한 페이지만 포함되어야 합니다.

-   페이지 범위의 지정 여부에 관계없이 인쇄 미리 보기에 모든 페이지를 표시하고 실제로 인쇄될 페이지를 아는 것은 사용자에게 맡깁니다.
-   사용자의 페이지 범위로 선택한 페이지만 인쇄 미리 보기에 표시하고 사용자가 페이지 범위를 변경할 때마다 표시를 업데이트합니다.
-   인쇄 미기 보기에 모든 페이지를 표시하지만 사용자가 선택한 페이지 범위에 없는 페이지는 회색으로 표시합니다.

## <a name="related-topics"></a>관련 항목

* [인쇄에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//빌드 2015 동영상: Windows 10에서 인쇄하는 앱 개발](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)
