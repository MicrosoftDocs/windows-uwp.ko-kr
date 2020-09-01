---
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: JavaScript/HTML 앱에서 Microsoft 광고 라이브러리의 일반적인 개발 문제에 대 한 해결 방법을 알아보세요.
title: HTML 및 JavaScript 문제 해결 가이드
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, AdControl, 문제 해결, HTML, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 53c2d18c56626d4a71b4326b1ab7e292a2267dca
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174987"
---
# <a name="html-and-javascript-troubleshooting-guide"></a>HTML 및 JavaScript 문제 해결 가이드

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 항목에는 JavaScript/HTML 앱의 Microsoft 광고 라이브러리와 관련 된 일반적인 개발 문제에 대 한 해결 방법이 포함 되어 있습니다.

* [HTML](#html)
  * [AdControl이 나타나지 않음](#html-notappearing)
  * [검은색 상자가 깜박이 고 사라집니다.](#html-blackboxblinksdisappears)
  * [광고 새로 고침 안 함](#html-adsnotrefreshing)

* [JavaScript](#js)
  * [AdControl이 나타나지 않음](#js-adcontrolnotappearing)
  * [검은색 상자가 깜박이 고 사라집니다.](#js-blackboxblinksdisappears)
  * [광고 새로 고침 안 함](#js-adsnotrefreshing)

## <a name="html"></a>HTML

<span id="html-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl이 나타나지 않음

1.  Appxmanifest.xml에서 **인터넷 (클라이언트)** 기능이 선택 되어 있는지 확인 합니다.

2.  JavaScript 참조가 있는지 확인 합니다. &lt;default.js 참조 이후 헤드 섹션에 ad.js 참조가 없으면 &gt; **adcontrol** 은 표시 될 수 없으며 빌드 중에 오류가 발생 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <head>
        ...
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        ...
    </head>
    ```

3.  응용 프로그램 ID 및 ad 단위 ID를 확인 합니다. 이러한 Id는 파트너 센터에서 가져온 응용 프로그램 ID 및 ad 단위 ID와 일치 해야 합니다. 자세한 내용은 [앱에서 ad 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조 하세요.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  **Height** 및 **width** 속성을 확인 하십시오. [배너 광고에 대해 지원 되는 ad 크기](supported-ad-sizes-for-banner-ads.md)중 하나로 설정 해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  요소 위치를 확인 합니다. [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 은 볼 수 있는 영역 내에 있어야 합니다.

6.  **표시 유형** 속성을 선택 합니다. 이 속성은 축소 또는 숨기기로 설정 하면 안 됩니다. 이 속성은 아래와 같이 인라인으로 설정 하거나 외부 스타일 시트에서 설정할 수 있습니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  **Position** 속성을 선택 합니다. Position 속성은 요소의 다른 속성 (예: 부모 요소의 여백 및 z 인덱스)에 따라 적절 한 값으로 설정 되어야 합니다. 이 속성은 아래와 같이 인라인으로 설정 하거나 외부 스타일 시트에서 설정할 수 있습니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  **Z 인덱스** 속성을 확인 하십시오. **Z-인덱스** 속성은 항상 충분히 높게 설정 해야 하므로 **adcontrol** 이 항상 다른 요소 위에 표시 됩니다. 이 속성은 아래와 같이 인라인으로 설정 하거나 외부 스타일 시트에서 설정할 수 있습니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  외부 스타일 시트를 확인 합니다. 외부 스타일 시트를 통해 **AdControl** 요소에 대해 속성이 설정된 경우 위의 모든 속성을 올바르게 설정되었는지 확인합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. **Adcontrol**의 부모를 확인 합니다. **Adcontrol** 이 부모 요소에 있는 경우 부모는 활성 상태이 고 표시 되어야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. 뷰포트에 **Adcontrol** 이 숨겨지지 않았는지 확인 합니다. 광고가 제대로 표시 되려면 **Adcontrol** 이 표시 되어야 합니다.

12. [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 및 [ad단위 id](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 의 라이브 값은 에뮬레이터에서 테스트 하면 안 됩니다. **Adcontrol** 이 예상 대로 작동 하는지 확인 하려면 **ApplicationId** 및 **adcontrol id**모두에 대해 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 을 사용 합니다.

<span id="html-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>검은색 상자가 깜박이 고 사라집니다.

1.  이전 [Adcontrol](#html-notappearing) 이 나타나지 않음 섹션의 모든 단계를 두 번 확인 합니다.

2.  **Onerroroccurred** 이벤트를 처리 하 고 이벤트 처리기에 전달 되는 메시지를 사용 하 여 오류가 발생 했는지 여부와 throw 된 오류의 유형을 확인 합니다. 자세한 내용은 [JavaScript 연습의 오류 처리](error-handling-in-javascript-walkthrough.md)에서 찾을 수 있습니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    블랙 박스를 발생 시키는 가장 일반적인 오류는 "ad를 사용할 수 없습니다."입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미 합니다.

3.  **Adcontrol** 은 정상적으로 동작 합니다. 기본적으로 **Adcontrol** 은 광고를 표시할 수 없을 때 축소 됩니다. 다른 요소가 동일한 부모의 자식인 경우 축소 된 **Adcontrol** 의 차이를 채우도록 이동 하 고 다음 요청이 수행 될 때 확장할 수 있습니다.

<span id="html-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>광고 새로 고침 안 함

1.  **IsAutoRefreshEnabled** 속성을 확인 합니다. 기본적으로이 선택적 속성은 true로 설정 됩니다. False로 설정 하면 **refresh** 메서드를 사용 하 여 다른 광고를 검색 해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  **Refresh** 메서드 호출을 확인 합니다. 자동 새로 고침을 사용 하는 경우 **새로 고침** 을 사용 하 여 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용 하는 경우 **새로 고침** 은 장치의 현재 데이터 연결에 따라 최소 30 ~ 60 초 후에만 호출 해야 합니다.

    이 예제에서는 **refresh** 메서드를 사용 하는 방법을 보여 줍니다. 다음 HTML 코드는 **isAutoRefreshEnabled** 가 false로 설정 된 **adcontrol** 을 인스턴스화하는 방법의 예를 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    예를 들어 **refresh** 함수를 사용 하는 방법을 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **Adcontrol** 은 정상적으로 동작 합니다. 경우에 따라 광고가 새로 고쳐지지 않는 모양을 제공 하는 동일한 광고가 한 행에 두 번 이상 표시 됩니다.

<span id="js"/>

## <a name="javascript"></a>JavaScript

<span id="js-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl이 나타나지 않음

1.  Appxmanifest.xml에서 **인터넷 (클라이언트)** 기능이 선택 되어 있는지 확인 합니다.

2.  **Adcontrol** 이 인스턴스화되어 있는지 확인 합니다. **Adcontrol** 이 인스턴스화되지 않은 경우입니다. 사용할 수 없습니다.

    다음 코드 조각에서는 **Adcontrol**을 인스턴스화하는 예제를 보여 줍니다. 이 HTML 코드는 **Adcontrol** 에 대 한 UI를 설정 하는 예제를 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    다음 JavaScript 코드는 **Adcontrol** 을 인스턴스화하는 예제를 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                ...
            }
        }
    }
    ```

3.  부모 요소를 확인 합니다. 부모 ** &lt; div &gt; ** 를 올바르게 할당 하 고 활성 상태로 표시 해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  응용 프로그램 ID 및 ad 단위 ID를 확인 합니다. 이러한 Id는 파트너 센터에서 가져온 응용 프로그램 ID 및 ad 단위 ID와 일치 해야 합니다. 자세한 내용은 [앱에서 ad 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조 하세요.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  **Adcontrol**의 부모 요소를 확인 합니다. 부모는 활성 상태이 고 표시 되어야 합니다.

6.  **ApplicationId** 및 **ad단위 id** 의 라이브 값은 에뮬레이터에서 테스트 하면 안 됩니다. **Adcontrol** 이 예상 대로 작동 하는지 확인 하려면 **ApplicationId** 및 **adcontrol id**모두에 대해 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 을 사용 합니다.

<span id="js-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>검은색 상자가 깜박이 고 사라집니다.

1.  [Adcontrol 표시 되지 않음](#js-adcontrolnotappearing) 섹션의 모든 단계를 두 번 확인 합니다.

2.  **Onerroroccurred** 이벤트를 처리 하 고 이벤트 처리기에 전달 되는 메시지를 사용 하 여 오류가 발생 했는지 여부와 throw 된 오류의 유형을 확인 합니다. 자세한 내용은 [JavaScript 연습의 오류 처리](error-handling-in-javascript-walkthrough.md)에서 찾을 수 있습니다.

    이 예제에서는 오류 메시지를 보고 하는 오류 처리기를 구현 하는 방법을 보여 줍니다. 이 HTML 코드 조각은 오류 메시지를 표시 하는 UI를 설정 하는 방법에 대 한 예제를 제공 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    이 예제에서는 **Adcontrol**를 인스턴스화하는 방법을 보여 줍니다. 이 함수는 app-v 활성화 파일에 삽입 됩니다.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    이 예에서는 오류를 보고 하는 방법을 보여 줍니다. 이 함수는 default.js 파일의 자체 실행 함수 아래에 삽입 됩니다.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    블랙 박스를 발생 시키는 가장 일반적인 오류는 "ad를 사용할 수 없습니다."입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미 합니다.

3.  **Adcontrol** 은 정상적으로 동작 합니다. 경우에 따라 광고가 새로 고쳐지지 않는 모양을 제공 하는 동일한 광고가 한 행에 두 번 이상 표시 됩니다.

<span id="js-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>광고 새로 고침 안 함

1.  **Adcontrol** 의 [IsAutoRefreshEnabled](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) 속성이 false로 설정 되어 있는지 여부를 확인 합니다. 기본적으로이 선택적 속성은 **true**로 설정 됩니다. **False**로 설정 하면 **Refresh** 메서드를 사용 하 여 다른 광고를 검색 해야 합니다.

2.  [Refresh](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) 메서드 호출을 확인 합니다. 자동 새로 고침을 사용 하는 경우 (**IsAutoRefreshEnabled** 가 **true**인 경우) **새로 고침** 을 사용 하 여 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용 하는 경우 (**IsAutoRefreshEnabled** 가 **false**인 경우), 장치의 현재 데이터 연결에 따라 최소 30 ~ 60 초 후에만 **새로 고침이** 호출 되어야 합니다.

    이 예제에서는 **Adcontrol**의 **div** 를 만드는 방법을 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    이 예제에서는 **Refresh** 함수를 사용 하는 방법을 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    ...
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **Adcontrol** 은 정상적으로 동작 합니다. 경우에 따라 광고가 새로 고쳐지지 않는 모양을 제공 하는 동일한 광고가 한 행에 두 번 이상 표시 됩니다.

 

 