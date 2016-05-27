---
author: mcleanbyron
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: JavaScript/HTML 앱에서 Microsoft Advertising 라이브러리를 사용할 때 발생하는 일반적인 개발 문제에 대한 해결 방법을 읽어보세요.
title: HTML 및 JavaScript 문제 해결 가이드

---

# HTML 및 JavaScript 문제 해결 가이드


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 항목에서는 JavaScript/HTML 앱에서 Microsoft Advertising 라이브러리를 사용할 때 발생하는 일반적인 개발 문제에 대한 해결 방법을 제공합니다.

-   [HTML](#html)

    -   [AdControl이 표시되지 않음](#html-notappearing)

    -   [블랙 박스가 깜박거리다가 사라짐](#html-blackboxblinksdisappears)

    -   [광고가 새로 고쳐지지 않음](#html-adsnotrefreshing)

-   [JavaScript](#js)

    -   [AdControl이 표시되지 않음](#js-adcontrolnotappearing)

    -   [블랙 박스가 깜박거리다가 사라짐](#js-blackboxblinksdisappears)

    -   [광고가 새로 고쳐지지 않음](#js-adsnotrefreshing)

## HTML

<span id="html-notappearing"/>
### AdControl이 표시되지 않음

1.  Package.appxmanifest에서**인터넷(클라이언트)** 기능이 선택되어 있는지 확인합니다.

2.  JavaScript 참조가 있는지 확인합니다. &lt;head&gt; 섹션(default.js 참조 다음)에 ad.js 참조가 없으면 **AdControl**을 표시할 수 없으며 빌드하는 동안 오류가 발생합니다.

    Windows 10:

    ``` syntax
    <head>
        …
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        …
    </head>
    ```

    Windows 8.x:

    ``` syntax
    <head>
        …
        <script src="//Microsoft.Advertising.JavaScript/ads/ad.js"></script>
        …
    </head>
    ```

3.  응용 프로그램 ID 및 광고 단위 ID를 확인합니다. 이러한 ID는 Windows 개발자 센터에서 가져온 응용 프로그램 ID 및 광고 단위 ID와 일치해야 합니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)을 참조하세요.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  **height** 및 **width** 속성을 확인합니다. 이러한 속성은 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나로 설정되어야 합니다.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  요소 위치 지정을 확인합니다. [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)은 볼 수 있는 영역 내에 있어야 합니다.

6.  **visibility** 속성을 확인합니다. 이 속성은 collapsed 또는 hidden으로 설정되면 안 됩니다. 이 속성은 인라인으로(아래 참조) 또는 외부 스타일 시트에 설정할 수 있습니다.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  **position** 속성을 확인합니다. position 속성은 요소의 다른 속성(예: 부모 요소의 여백 및 z-인덱스)에 따라 적절한 값으로 설정해야 합니다. 이 속성은 인라인으로(아래 참조) 또는 외부 스타일 시트에 설정할 수 있습니다.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  **z-index** 속성을 확인합니다. **z-index** 속성은 높게 설정해야 하므로 **AdControl**은 항상 다른 요소 위에 나타납니다. 이 속성은 인라인으로(아래 참조) 또는 외부 스타일 시트에 설정할 수 있습니다.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  외부 스타일 시트를 확인합니다. 외부 스타일 시트를 통해 **AdControl** 요소에 대해 속성이 설정된 경우 위의 모든 속성을 올바르게 설정되었는지 확인합니다.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. **AdControl**의 부모를 확인합니다. **AdControl**이 부모 요소에 있으면 부모 요소가 활성화되고 표시되어야 합니다.

    ``` syntax
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. **AdControl**이 뷰포트에서 숨겨져 있지 않은지 확인합니다. 광고가 제대로 표시되려면 **AdControl**이 표시되어야 합니다.

12. [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 및 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx)의 라이브 값은 에뮬레이터에서 테스트하지 말아야 합니다. **AdControl**이 예상대로 작동하는지 확인하려면 [테스트 모드 값](test-mode-values.md)에 있는 **ApplicationId** 및 **AdUnitId** 둘 다의 테스트 ID를 사용합니다.

<span id="html-blackboxblinksdisappears"/>
### 블랙 박스가 깜박거리다가 사라짐

1.  이전에 나온 [AdControl이 표시되지 않음](#html-notappearing) 섹션의 모든 단계를 한 번 더 확인합니다.

2.  **onErrorOccurred** 이벤트를 처리하고 이벤트 처리기에 전달된 메시지를 사용하여 오류가 발생했는지와 발생한 오류 유형을 확인합니다. 자세한 내용은 [JavaScript에서 오류 처리 연습](error-handling-in-javascript-walkthrough.md)에서 찾을 수 있습니다.

    ``` syntax
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

    블랙 박스를 발생하는 가장 일반적인 오류는 "사용할 수 있는 광고가 없음”입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미합니다.

3.  **AdControl**은 정상적으로 동작하고 있습니다. 기본적으로 **AdControl**은 광고를 표시할 수 없을 때 축소됩니다. 다른 요소가 같은 부모의 자식인 경우 축소된 **AdControl**의 간격을 채우기 위해 이동된 후 다음 요청이 있을 때 확장될 수 있습니다.

<span id="html-adsnotrefreshing"/>
### 광고가 새로 고쳐지지 않음

1.  **isAutoRefreshEnabled** 속성을 확인합니다. 기본적으로 이 선택적 속성은 true로 설정됩니다. False로 설정된 경우 **refresh** 메서드를 사용해서 다른 광고를 검색해야 합니다.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  **refresh** 메서드에 대한 호출을 확인합니다. 자동 새로 고침을 사용하는 경우 **refresh**를 사용해서 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용하는 경우 디바이스의 현재 데이터 연결에 따라 30-60초경과된 후에만 **refresh**가 호출됩니다.

    다음 예제에서는 **refresh** 메서드를 사용하는 방법을 보여 줍니다. 다음 HTML 코드에서는 **isAutoRefreshEnabled**를 false로 설정한 상태로 **AdControl**을 인스턴스화하는 방법의 예를 보여 줍니다.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    다음 예제에서는 **refresh** 함수를 사용하는 방법을 보여 줍니다.

    ``` syntax
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl**은 정상적으로 동작하고 있습니다. 동일한 광고가 차례대로 두 번 이상 나타나면서 광고는 새로 고쳐지지 않는 경우가 있습니다.

<span id="js"/>
## JavaScript

<span id="js-adcontrolnotappearing"/>
### AdControl이 표시되지 않음

1.  Package.appxmanifest에서**인터넷(클라이언트)** 기능이 선택되어 있는지 확인합니다.

2.  **AdControl**이 인스턴스화되었는지 확인합니다. **AdControl**이 인스턴스화되지 않은 경우 사용할 수 없습니다.

    다음 코드 조각에서는 **AdControl**을 인스턴스화하는 방법의 예를 보여 줍니다. 이 HTML 코드에서는 **AdControl**의 UI를 설정하는 방법의 예를 보여 줍니다.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    다음 JavaScript 코드에서는 **AdControl**을 인스턴스화하는 방법의 예를 보여 줍니다.

    ``` syntax
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
                …
            }
        }
    }
    ```

3.  부모 요소를 확인합니다. 부모 **&lt;div&gt;**가 올바르게 할당되고, 활성화되고, 표시되어야 합니다.

    ``` syntax
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  응용 프로그램 ID 및 광고 단위 ID를 확인합니다. 이러한 ID는 Windows 개발자 센터에서 가져온 응용 프로그램 ID 및 광고 단위 ID와 일치해야 합니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)을 참조하세요.

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  **AdControl**의 부모 요소를 확인합니다. 부모는 활성화되고 표시되어야 합니다.

6.  **ApplicationId** 및 **AdUnitId**의 라이브 값은 에뮬레이터에서 테스트하지 말아야 합니다. **AdControl**이 예상대로 작동하는지 확인하려면 [테스트 모드 값](test-mode-values.md)에 있는 **ApplicationId** 및 **AdUnitId** 둘 다의 테스트 ID를 사용합니다.

<span id="js-blackboxblinksdisappears"/>
### 블랙 박스가 깜박거리다가 사라짐

1.  [AdControl이 표시되지 않음](#js-adcontrolnotappearing) 섹션의 모든 단계를 한 번 더 확인합니다.

2.  **onErrorOccurred** 이벤트를 처리하고 이벤트 처리기에 전달된 메시지를 사용하여 오류가 발생했는지와 발생한 오류 유형을 확인합니다. 자세한 내용은 [JavaScript에서 오류 처리 연습](error-handling-in-javascript-walkthrough.md)에서 찾을 수 있습니다.

    이 예제에서는 오류 메시지를 보고하는 오류 처리기의 구현 방법을 보여 줍니다. 이 HTML 코드 조각에서는 오류 메시지를 표시하도록 UI를 설정하는 방법의 예를 제공합니다.

    ``` syntax
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    이 예제에서는 **AdControl**을 인스턴스화하는 방법을 보여 줍니다. 이 함수는 app.onactivated 파일에 삽입됩니다.

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    이 예제에서는 오류 보고 방법을 보여 줍니다. 이 함수는 default.js 파일에서 자체 실행 함수 아래에 삽입됩니다.

    ``` syntax
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

    블랙 박스를 발생하는 가장 일반적인 오류는 "사용할 수 있는 광고가 없음”입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미합니다.

3.  **AdControl**은 정상적으로 동작하고 있습니다. 동일한 광고가 차례대로 두 번 이상 나타나면서 광고는 새로 고쳐지지 않는 경우가 있습니다.

<span id="js-adsnotrefreshing"/>
### 광고가 새로 고쳐지지 않음

1.  **isAutoRefreshEnabled** 속성을 확인합니다. 기본적으로 이 선택적 속성은 **true**로 설정합니다. **false**로 설정된 경우 **refresh** 메서드를 사용해서 다른 광고를 검색해야 합니다.

    이 예제에서는 **isAutoRefreshEnabled** 속성을 사용하는 방법을 보여 줍니다.

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: true
    });  
    ```

2.  **refresh** 메서드에 대한 호출을 확인합니다. 자동 새로 고침을 사용하는 경우 **refresh**를 사용해서 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용하는 경우 디바이스의 현재 데이터 연결에 따라 30-60초경과된 후에만 **refresh**가 호출됩니다.

    이 예제에서는 **AdControl**의 **div**를 만드는 방법을 보여 줍니다.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    다음 예제에서는 **refresh** 함수를 사용하는 방법을 보여 줍니다.

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    …
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl**은 정상적으로 동작하고 있습니다. 동일한 광고가 차례대로 두 번 이상 나타나면서 광고는 새로 고쳐지지 않는 경우가 있습니다.

 

 


<!--HONumber=May16_HO2-->


