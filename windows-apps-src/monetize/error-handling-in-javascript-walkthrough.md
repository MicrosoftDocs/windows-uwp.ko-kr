---
author: Xansky
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: 앱에서 AdControl 오류를 검색하는 방법을 알아봅니다.
title: JavaScript에서 오류 처리 연습
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 오류 처리, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 65887bba125d8d7db1b224c1842da8c9ab34f117
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700525"
---
# <a name="error-handling-in-javascript-walkthrough"></a>JavaScript에서 오류 처리 연습

이 연습에서는 JavaScript 앱에서 광고 관련 오류를 검색하는 방법을 보여 줍니다. 이 연습은 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)을 사용하여 배너를 표시하는 것을 안내하지만 일반적인 개념은 중간 광고 및 기본 광고에도 적용됩니다.

이러한 예제에서는 **AdControl**이 포함된 JavaScript 앱이 있다고 가정합니다. 앱에 **AdControl**을 추가하는 방법을 보여 주는 단계별 지침은 [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)을 참조하세요. JavaScript/HTML 앱에 배너 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

1.  default.html 파일에서 **AdControl**의 **div**에서 **data-win-options**를 정의하는 경우 **onErrorOccurred**에 대한 값을 추가합니다. default.html 파일에서 다음 코드를 찾습니다.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    **adUnitId** 특성 다음에 **onErrorOccurred** 이벤트에 대한 값을 추가합니다.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  생성되는 메시지를 볼 수 있도록 텍스트를 표시하는 **div**를 만듭니다. 이렇게 하려면 **myAd**에 대한 **div** 뒤에 다음 코드를 추가합니다.
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  오류 이벤트를 트리거하는 **AdControl**을 만듭니다. 앱의 모든 **AdControl** 개체에 대해 응용 프로그램 ID가 하나만 있을 수 있습니다. 따라서 다른 응용 프로그램 ID로 추가 개체를 만들면 런타임에 오류가 트리거됩니다. 이렇게 하려면 default.html 페이지의 본문에서 추가한 이전 **div** 섹션 뒤에 다음 코드를 추가합니다.
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  프로젝트의 default.js 파일에서 기본 초기화 함수 뒤에 **errorLogger**에 대한 이벤트 처리기를 추가합니다. 파일의 끝으로 스크롤하고 마지막 세미콜론 뒤에 다음 코드를 입력합니다.
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  파일을 빌드하고 실행합니다. 이전에 빌드한 샘플 앱의 원래 광고가 표시되고 광고 아래에는 오류를 설명하는 텍스트가 표시됩니다. ID가 **liveAd**인 광고는 표시되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
