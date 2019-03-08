---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Microsoft Advertising 라이브러리의 AdControl 클래스에 의해 생성되는 오류를 처리하는 방법을 알아봅니다.
title: 광고 오류 처리
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 오류 처리, javascript, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: d0e2e1c019497fc22e8d922ba5f0a02a30034b65
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617758"
---
# <a name="handle-ad-errors"></a>광고 오류 처리

[AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol),  [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad), [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 클래스에는 각각 광고 관련 오류가 발생할 경우 발생하는 **ErrorOccurred** 이벤트가 있습니다. 앱 코드가 이 이벤트를 처리하고, 오류의 원인을 파악하는 데 도움이 되는 이벤트 args 개체의 [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) 및 [ErrorMessage](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) 속성을 검사할 수 있습니다.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>XAML 앱

XAML 앱에서 광고 관련 오류를 처리하려면

1. **AdControl**, **InterstitialAd** 또는 **NativeAdsManagerV2** 개체의 **ErrorOccurred** 이벤트를 이벤트 처리기 대리자의 이름에 할당합니다.

2. 발신자에 대한 **Object** 및 [AdErrorEventArgs](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs) 개체의 두 매개 변수를 취하도록 오류 이벤트 처리 대리자를 코딩합니다.

다음은 **OnAdError**라는 대리자를 *myBannerAdControl*이라는 **AdControl** 개체의 **ErrorOccurred** 이벤트에 할당하는 예제입니다.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

다음은 Visual Studio의 출력 창에 오류 정보를 쓰는 **OnAdError** 대리자의 정의 예제입니다.

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

XAML 및 C#에서의 **AdControl** 오류 처리를 보여 주는 연습에 대해서는 [XAML/C#에서 오류 처리 연습](error-handling-in-xamlc-walkthrough.md)을 참조하세요.

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>JavaScript/HTML 앱

JavaScript 앱에서 **ErrorOccur** 오류를 처리하려면

1.  **onErrorOccurred** 이벤트를 이벤트 처리기를 할당합니다.

2.  이벤트 처리기를 코딩합니다.

다음은 **errorLogger** 라는 이벤트 처리기를 **AdControl** 개체의 **ErrorOccurred** 이벤트에 할당하는 예제입니다.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

오류 처리 함수는 선언적이며 [markSupportedForProcessing](https://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 함수로 묶어야 합니다.

오류 처리기는 오류가 발생할 때 JavaScript 오류 개체를 검색합니다. Error 개체는 오류 처리기에 두 개의 인수를 제공합니다. 자세한 내용은 [비동기 Windows 런타임 메서드의 특정 오류 속성](https://msdn.microsoft.com/library/windows/apps/hh994690.aspx)을 참조하세요.

다음은 **onErrorOccurred** 이벤트를 처리하는 오류 처리 함수 **errorLogger** 예제입니다.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

JavaScript에서의 **AdControl** 오류 처리를 보여 주는 연습에 대해서는 [JavaScript에서 오류 처리 연습](error-handling-in-javascript-walkthrough.md)을 참조하세요.
