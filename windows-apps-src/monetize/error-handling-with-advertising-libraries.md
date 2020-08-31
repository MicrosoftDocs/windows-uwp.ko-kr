---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Microsoft 광고 라이브러리의 AdControl 클래스에서 생성 된 오류를 처리 하는 방법에 대해 알아봅니다.
title: 광고 오류 처리
ms.date: 02/18/2020
ms.topic: article
keywords: 'windows 10, uwp, 광고, 광고, 오류 처리, javascript, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 2a1a82c9977bfbe61712d39e4d23fdd68cd598ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171557"
---
# <a name="handle-ad-errors"></a>광고 오류 처리

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

[Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)및 [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 클래스는 각각 ad 관련 오류가 발생할 경우 발생 하는 **erroroccurred** 이벤트를 포함 합니다. 앱 코드에서이 이벤트를 처리 하 고 이벤트 args 개체의 [ErrorCode](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) 및 [ErrorMessage](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) 속성을 검사 하 여 오류의 원인을 확인할 수 있습니다.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>XAML 앱

XAML 앱에서 ad 관련 오류를 처리 하려면 다음을 수행 합니다.

1. **Adcontrol**의 **errorststalad**또는 **NativeAdsManagerV2** 개체를 이벤트 처리기 대리자 이름에 할당 합니다. **ErrorOccurred**

2. 두 개의 매개 변수 (보낸 사람에 대 한 **개체** 및 [aderroreventargs](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs) 개체)를 사용 하도록 오류 이벤트 처리 대리자를 코딩 합니다.

*MyBannerAdControl*라는 **adcontrol** 개체의 **Erroroccurred** 한 이벤트에 **onaderror** 라는 대리자를 할당 하는 예제는 다음과 같습니다.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

Visual Studio의 출력 창에 오류 정보를 기록 하는 **Onaderror** 대리자의 예제 정의는 다음과 같습니다.

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

XAML 및 c #에서 **Adcontrol** 오류 처리를 보여 주는 연습은 [xaml/c #의 오류 처리 연습](error-handling-in-xamlc-walkthrough.md) 을 참조 하세요.

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>JavaScript/HTML 앱

JavaScript 앱에서 **erroroccur** 오류를 처리 하려면 다음을 수행 합니다.

1.  이벤트 처리기에 **Onerroroccurred** 이벤트를 할당 합니다.

2.  이벤트 처리기를 코딩 합니다.

다음은 **Adcontrol** 개체의 **errorlogger** 이벤트에 **errorlogger** 라는 이벤트 처리기를 할당 하는 예제입니다.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

오류 처리 함수는 선언적 이며 [Marksupportedforprocessing](/previous-versions/windows/apps/hh967819(v=win.10)) 함수로 묶어야 합니다.

오류 처리기는 오류가 발생할 때 JavaScript 오류 개체를 catch 합니다. Error 개체는 오류 처리기에 대 한 두 개의 인수를 제공 합니다. 자세한 내용은 [비동기 Windows 런타임 메서드의 특수 오류 속성](/scripting/jswinrt/special-error-properties-from-asynchronous-windows-runtime-methods)을 참조 하세요.

다음은 **onerroroccurred** 이벤트를 처리 하는 **errorlogger** 라는 오류 처리 함수의 예입니다.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

JavaScript에서 **Adcontrol** 오류 처리를 보여 주는 연습은 [Javascript의 오류 처리 연습](error-handling-in-javascript-walkthrough.md) 을 참조 하세요.