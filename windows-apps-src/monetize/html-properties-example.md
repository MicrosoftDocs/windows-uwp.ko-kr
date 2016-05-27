---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: HTML에서 **AdControl** 속성을 할당하는 방법을 알아봅니다.
title: HTML 속성 예제

---

# HTML 속성 예제


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

다음 예제에서는 HTML에서 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 속성을 할당하는 방법을 보여 줍니다. **applicationId** 및 **adUnitId**는 필수 속성입니다. 다른 속성 및 이벤트는 선택적입니다. 선택적 속성의 값을 제공하지 않는 경우 컨트롤에서 앱의 사용자 환경과 일치하는 광고를 만드는 기본값을 사용합니다.

마지막 5개 줄은 **AdControl** 이벤트에 함수를 등록하는 예제입니다. JavaScript 코드에서 정의한 함수만 등록할 수 있습니다.

이러한 값은 예제로 제공된 것입니다. 코드에서 이러한 함수 및 속성의 값을 앱에 적절하게 설정합니다.

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->


