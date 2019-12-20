---
description: 이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 클립보드를 사용하여 복사 및 붙여넣기를 수행하는 방법을 설명합니다.
title: 복사 및 붙여넣기
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dfa5c15c2bd4d82588e0b197dc265c4b529e64c9
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302687"
---
# <a name="copy-and-paste"></a>복사 및 붙여넣기

이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 클립보드를 사용하여 복사 및 붙여넣기를 수행하는 방법을 설명합니다. 복사 및 붙여넣기는 앱 간에 또는 앱 내에서 데이터를 교환하는 기본적인 방법으로, 거의 모든 앱이 클립보드 작업을 어느 정도 지원할 수 있습니다. 여러 가지 다른 복사 및 붙여넣기 시나리오를 보여 주는 전체 코드 예제는 [클립보드 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)을 참조 하세요.

## <a name="check-for-built-in-clipboard-support"></a>기본 제공 클립보드 지원 확인

대부분의 경우에는 클립보드 작업을 지원하는 코드를 작성할 필요가 없습니다. 앱을 만드는 데 사용할 수 있는 많은 기본 XAML 컨트롤이 클립보드 작업을 이미 지원합니다. 

## <a name="get-set-up"></a>설정하기

먼저 앱에 [**Windows.ApplicationModel.DataTransfer**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer) 네임스페이스를 포함합니다. 그런 후 [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 개체의 인스턴스를 추가합니다. 이 개체에는 사용자가 복사하려는 데이터와 포함할 속성(예: 설명)이 모두 포함되어 있습니다.

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>복사 및 잘라내기

복사 및 잘라내기(*이동*이라고도 함)는 거의 정확히 동일하게 작동합니다. [  **RequestedOperation**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 속성을 사용하여 수행하려는 작업을 선택합니다.

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>복사 된 콘텐츠 설정

이제 사용자가 선택한 데이터를 [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 개체에 추가할 수 있습니다. 이 데이터가 **DataPackage** 클래스에서 지원 되는 경우 **DataPackage** 개체의 해당 [메서드](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods) 중 하나를 사용할 수 있습니다. [**SetText**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) 메서드를 사용 하 여 텍스트를 추가 하는 방법은 다음과 같습니다.

```cs
dataPackage.SetText("Hello World!");
```

마지막 단계는 정적 [**Clipboard.SetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent) 메서드를 호출하여 클립보드에 [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)를 추가하는 것입니다.

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>붙여넣기

클립보드 내용을 가져오려면 정적 [**GetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent) 메서드를 호출합니다. 이 메서드는 콘텐츠를 포함하는 [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView)를 반환합니다. 이 개체는 콘텐츠가 읽기 전용이라는 점만 제외하고 [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 개체와 거의 동일합니다. 이 개체에서 [**AvailableFormats**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats) 또는 [**Contains**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains) 메서드를 사용하여 사용 가능한 형식을 식별할 수 있습니다. 그런 다음 해당 [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) 메서드를 호출하여 데이터를 가져올 수 있습니다.

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>클립보드 변경 내용 추적

복사 및 붙여넣기 명령 외에도, 클립보드 변경 내용을 추적할 수 있습니다. 이 작업을 수행하려면 클립보드의 [**ContentChanged**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged) 이벤트를 처리합니다.

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>참고 항목

* [클립보드 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [앱 간 통신](index.md)
* [DataTransfer](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackageView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested 됨]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [ControlsList](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)
* [SetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [적용 되는 형식](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [Contains](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [콘텐츠 변경 됨](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)

