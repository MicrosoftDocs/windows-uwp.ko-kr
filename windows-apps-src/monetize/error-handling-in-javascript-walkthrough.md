---
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: 이 연습을 수행 하 여 JavaScript 및 HTML5 앱에서 AdControl의 오류를 catch 하 고 처리 하는 방법에 대해 알아봅니다.
title: JavaScript에서 오류 처리 연습
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 오류 처리, javascript
ms.localizationpriority: medium
ms.openlocfilehash: f6567432714fb68618510923e49f2467daefdc54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167687"
---
# <a name="error-handling-in-javascript-walkthrough"></a>JavaScript에서 오류 처리 연습

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 연습에서는 JavaScript 앱에서 광고 관련 오류를 catch 하는 방법을 보여 줍니다. 이 연습에서는 [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 을 사용 하 여 배너 광고를 표시 하지만이의 일반적인 개념은 중간 광고 및 네이티브 광고에도 적용 됩니다.

이러한 예제에서는 **Adcontrol**을 포함 하는 JavaScript 앱이 있다고 가정 합니다. 응용 프로그램에 **Adcontrol** 을 추가 하는 방법을 보여 주는 단계별 지침은 [HTML 5 및 Javascript의 adcontrol](adcontrol-in-html-5-and-javascript.md)을 참조 하세요. JavaScript/HTML 앱에 배너 광고를 추가 하는 방법을 보여 주는 전체 샘플 프로젝트는 [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)을 참조 하세요.

1.  default.html 파일에서 **Adcontrol**에 대해 **div** 의 **데이터-win 옵션** 을 정의 하는 **onerroroccurred** 이벤트에 대 한 값을 추가 합니다. default.html 파일에서 다음 코드를 찾습니다.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    **Adunitid** 특성에 따라 **onerroroccurred** 이벤트에 대 한 값을 추가 합니다.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  생성 되는 메시지를 볼 수 있도록 텍스트를 표시 하는 **div** 를 만듭니다. 이렇게 하려면 **Myad**의 **div** 뒤에 다음 코드를 추가 합니다.
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  오류 이벤트를 트리거하는 **Adcontrol** 을 만듭니다. 앱의 모든 **Adcontrol** 개체에는 응용 프로그램 id가 하나만 있을 수 있습니다. 따라서 다른 응용 프로그램 id를 사용 하 여 추가 항목을 만들면 런타임에 오류가 발생 합니다. 이렇게 하려면 추가한 이전 **div** 섹션 뒤에 다음 코드를 default.html 페이지의 본문에 추가 합니다.
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  프로젝트의 default.js 파일에서 기본 초기화 함수 다음에는 **Errorlogger 거**에 대 한 이벤트 처리기를 추가 합니다. 파일의 끝 부분으로 스크롤하고 마지막 세미 콜론 뒤에 다음 코드를 입력 합니다.
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  파일을 빌드하고 실행 합니다. 이전에 작성 한 샘플 앱의 원본 ad와 해당 ad의 텍스트에서 오류를 설명 하는 것을 볼 수 있습니다. Id가 **liveAd**인 ad가 표시 되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)