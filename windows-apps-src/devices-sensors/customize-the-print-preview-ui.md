---
author: PatrickFarley
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: 인쇄 미리 보기 UI 사용자 지정
description: 이 섹션에서는 인쇄 옵션 및 인쇄 미리 보기 UI의 설정을 사용자 지정하는 방법을 설명합니다.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: fe4086cc87699083304594eb4ccc8e7bb137b19f
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3851023"
---
# <a name="customize-the-print-preview-ui"></a>인쇄 미리 보기 UI 사용자 지정



**중요 API**

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426)

이 섹션에서는 인쇄 옵션 및 인쇄 미리 보기 UI의 설정을 사용자 지정하는 방법을 설명합니다. 인쇄에 대한 자세한 내용은 [앱에서 인쇄](print-from-your-app.md)를 참조하세요.

**팁**  이 항목의 예제는 대부분 인쇄 샘플을 기반으로 합니다. 전체 코드를 보려면 GitHub의 [Windows-universal-samples repo](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 [UWP(유니버설 Windows 플랫폼) 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)을 다운로드하세요.

 

## <a name="customize-print-options"></a>인쇄 옵션 사용자 지정

기본적으로 인쇄 미리 보기 UI에는 [**ColorMode**](https://msdn.microsoft.com/library/windows/apps/BR226478), [**Copies**](https://msdn.microsoft.com/library/windows/apps/BR226479) 및 [**Orientation**](https://msdn.microsoft.com/library/windows/apps/BR226486) 인쇄 옵션이 표시됩니다. 이외에도 인쇄 미리 보기 UI에 추가할 수 있는 몇 가지 다른 일반 프린터 옵션이 있습니다.

-   [**바인딩**](https://msdn.microsoft.com/library/windows/apps/BR226476)
-   [**한 부씩 인쇄**](https://msdn.microsoft.com/library/windows/apps/BR226477)
-   [**Duplex**](https://msdn.microsoft.com/library/windows/apps/BR226480)
-   [**HolePunch**](https://msdn.microsoft.com/library/windows/apps/BR226481)
-   [**InputBin**](https://msdn.microsoft.com/library/windows/apps/BR226482)
-   [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483)
-   [**MediaType**](https://msdn.microsoft.com/library/windows/apps/BR226484)
-   [**NUp**](https://msdn.microsoft.com/library/windows/apps/BR226485)
-   [**PrintQuality**](https://msdn.microsoft.com/library/windows/apps/BR226487)
-   [**스테이플**](https://msdn.microsoft.com/library/windows/apps/BR226488)

이러한 옵션은 [**StandardPrintTaskOptions**](https://msdn.microsoft.com/library/windows/apps/BR226475) 클래스에 정의되어 있습니다. 인쇄 미리 보기 UI에 표시되는 옵션 목록에서 옵션을 추가하거나 제거할 수 있습니다. 또한 옵션이 표시되는 순서를 변경하고 사용자에게 표시되는 기본 설정을 지정할 수도 있습니다.

단, 이 메서드를 사용하여 수정한 사항은 인쇄 미리 보기 UI에만 적용됩니다. 사용자는 인쇄 미리 보기 UI에서 **기타 설정**을 탭하여 프린터에서 지원하는 모든 옵션에 항상 액세스할 수 있습니다.

**참고**  앱에서는 표시될 인쇄 옵션을 어떤 것이든 지정할 수 있지만 선택한 프린터에서 지원하는 옵션만 인쇄 미리 보기 UI에 표시됩니다. 선택한 프린터가 지원하지 않는 옵션은 인쇄 UI에 표시되지 않습니다.

 

### <a name="define-the-options-to-display"></a>표시할 옵션 정의

앱의 화면이 로드되면 앱이 인쇄 계약을 등록합니다. 등록 과정에 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 이벤트 처리기를 정의하는 것이 포함됩니다. 인쇄 미리 보기 UI에 표시되는 옵션을 사용자 지정할 코드가 **PrintTaskRequested** 이벤트 처리기에 추가됩니다.

[**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 이벤트 처리기가 [**printTask.options**](https://msdn.microsoft.com/library/windows/apps/BR226469) 지시문을 포함하도록 수정합니다. 이 지시문은 인쇄 미리 보기 UI에 표시할 인쇄 설정을 구성합니다. 인쇄 옵션의 사용자 지정된 목록을 표시할 앱 화면에 대해서는 이 화면이 인쇄될 때 표시할 옵션을 지정하는 코드를 포함하도록 기본 클래스에서 **PrintTaskRequested** 이벤트 처리기를 재정의합니다.

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

**중요**  [**displayedOptions.clear**](https://msdn.microsoft.com/library/windows/apps/BR226453)()을 호출하면 인쇄 미리 보기 UI에서 **기타 설정** 링크를 비롯한 모든 인쇄 옵션이 제거됩니다. 인쇄 미리 보기 UI에 표시할 옵션을 추가해야 합니다.

### <a name="specify-default-options"></a>기본 옵션 지정

또한 인쇄 미리 보기 UI에서 옵션의 기본값을 설정할 수 있습니다. 이전 예제에서 가져온 다음 코드 줄은 [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483) 옵션의 기본값을 설정합니다.

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>새 인쇄 옵션 추가

이 섹션에서는 새 인쇄 옵션을 만들고, 옵션이 지원하는 값 목록을 정의한 다음 이 옵션을 인쇄 미리 보기에 추가하는 방법을 보여 줍니다. 이전 섹션과 마찬가지로 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 이벤트 처리기에서 새 인쇄 옵션을 추가합니다.

먼저 [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256) 개체를 가져옵니다. 이 개체는 새 인쇄 옵션을 인쇄 미리 보기 UI에 추가하는 데 사용됩니다. 그런 다음 인쇄 미리 보기 UI에 표시된 옵션 목록을 지우고 사용자가 앱에서 인쇄를 하고자 할 때 표시할 옵션을 추가합니다. 이제 새 인쇄 옵션을 만들고 옵션 값 목록을 초기화합니다. 마지막으로, 새 옵션을 추가하고 **OptionChanged** 이벤트에 대한 처리기를 할당합니다.

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

추가된 순서대로 첫 번째 옵션이 창의 맨 위에 오도록 인쇄 미리 보기 UI에 옵션이 표시됩니다. 이 예제에서는 사용자 지정 옵션이 옵션 목록의 맨 아래에 나타나도록 마지막에 추가됩니다. 그러나 사용자 지정 인쇄 옵션은 마지막에 추가할 필요가 없으며 목록에서 원하는 곳에 배치할 수 있습니다.

사용자가 개발자의 사용자 지정 옵션에서 선택한 옵션을 변경하는 경우 인쇄 미리 보기 이미지를 업데이트합니다. [**InvalidatePreview**](https://msdn.microsoft.com/library/windows/apps/Hh702146) 메서드를 호출하여 아래 그림과 같이 인쇄 미리 보기 UI에서 이미지를 다시 그립니다.

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

* [인쇄에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//빌드 2015 동영상: Windows 10에서 인쇄하는 앱 개발](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 인쇄 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619984)
