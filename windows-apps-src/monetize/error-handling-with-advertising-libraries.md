---
author: mcleanbyron
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: "Microsoft Advertising 라이브러리의 AdControl 클래스에 의해 생성되는 오류를 처리하는 방법을 알아봅니다."
title: "Microsoft Advertising 라이브러리로 오류 처리"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5e0c7e6328247e5f686b14b10c80d8aafc13e0e4

---

# Microsoft Advertising 라이브러리로 오류 처리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 항목에서는 Microsoft Advertising 라이브러리의 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 클래스에 의해 생성되는 오류를 처리하는 방법에 대한 기본 정보를 제공합니다.

<span id="bkmk-javascript"/>
## JavaScript/HTML 앱

JavaScript 앱에서 **AdControl** 오류를 처리하려면

1.  **onErrorOccurred** 이벤트를 이벤트 처리기를 할당합니다.

2.  이벤트 처리기를 코딩합니다.

**onErrorOccurred** 이벤트 처리기는 **AdControl**의 **div**에 대한 **data-win-options** 특성에서 설정됩니다. 다음 예제에서 **onErrorOccurred** 이벤트는 **errorLogger**라는 함수에 의해 처리되도록 설정됩니다.

``` syntax
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: 'd25517cb-12d4-4699-8bdc-52040c712cab', adUnitId: 'ADPT33', onErrorOccurred: errorLogger}">
</div>
```

오류 처리 함수는 선언적이며 [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 함수로 묶어야 합니다.

오류 처리기는 **AdControl** 오류가 발생할 때 JavaScript 오류 개체를 검색합니다. Error 개체는 오류 처리기에 두 개의 인수를 제공합니다. 자세한 내용은 [비동기 Windows 런타임 메서드의 특정 오류 속성](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx)을 참조하세요.

다음은 **onErrorOccurred** 이벤트를 처리하는 오류 처리 함수 **errorLogger** 예제입니다.

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage + " error code: " + evt.errorCode + \n");
});
```

JavaScript에서의 **AdControl** 오류 처리를 보여 주는 연습에 대해서는 [JavaScript에서 오류 처리 연습](error-handling-in-javascript-walkthrough.md)을 참조하세요.

<span id="bkmk-dotnet"/>
## XAML 앱

XAML 앱에서 **AdControl** 오류를 처리하려면

* **ErrorOccurred** 이벤트를 이벤트 처리기 대리자의 이름에 할당합니다.

* 발신자에 대한 **Object** 및 **AdErrorEventArgs** 개체의 두 매개 변수를 취하도록 오류 이벤트 처리 대리자를 코딩합니다.

다음은 **OnAdError**라는 대리자를 **ErrorOccurred** 이벤트에 할당하는 예제입니다.

``` syntax
this.ErrorOccurred = OnAdError;
```

다음은 Visual Studio의 출력 창에 오류 정보를 쓰는 **OnAdError** 대리자의 정의 예제입니다.

``` syntax
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error + " ErrorCode: " + e.ErrorCode.ToString());
}
```

XAML 및 C#에서의 **AdControl** 오류 처리를 보여 주는 연습에 대해서는 [XAML/C#에서 오류 처리 연습](error-handling-in-xamlc-walkthrough.md)을 참조하세요.

 

 



<!--HONumber=Jun16_HO4-->


