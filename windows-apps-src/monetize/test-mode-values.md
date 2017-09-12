---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "이 문서의 테스트 응용 프로그램 ID 및 광고 단위 ID 값을 사용하여 테스트 동안 앱이 광고를 렌더링하는 방법을 확인합니다."
title: "테스트 모드 값"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 광고, 광고, 테스트"
ms.openlocfilehash: 0c3e713d9a2bb7c10bda0d9517f5cb882d5e2e57
ms.sourcegitcommit: 6b772d2a224f8a9c557dc517c6ec0592545e9a43
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/02/2017
---
# <a name="test-mode-values"></a>테스트 모드 값

[AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx), 또는 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx)를 사용하여 앱에서 광고를 표시하는 경우 **AdUnitId** 및 **ApplicationId** 속성에서 광고 단위 ID와 응용 프로그램 ID를 지정해야 합니다. 앱을 개발하는 동안 이 문서의 테스트 응용 프로그램 ID 및 광고 단위 ID 값을 사용하여 테스트 동안 앱이 광고를 렌더링하는 방법을 확인합니다.

게시한 후에 앱에서 테스트 값을 사용하려고 하면 앱에 광고가 수신되지 않습니다. 게시된 앱에서 광고를 수신하려면 Windows 개발자 센터 대시보드에서 제공한 응용 프로그램 ID와 광고 단위 ID를 사용하여 코드를 업데이트해야 합니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)을 참조하세요.
 
여러 광고에 사용할 테스트 값은 다음과 같습니다.

* 중간 광고의 경우:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">대상 OS</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP(Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    <tr class="odd">
    <td align="left"><p>Windows 8.x 및 Windows Phone 8.x</p></td>
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* 배너 광고의 경우:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">대상 OS</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP(Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    <tr class="even">
    <td align="left"><p>Windows 8.x 및 Windows Phone 8.x</p></td>
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>

* 기본 광고:

    <table>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">대상 OS</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP(Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tbody>
    </table>

> [!IMPORTANT]
> **AdControl**에서 라이브 광고의 크기는 **Width** 및 **Height** 속성으로 정의됩니다. 최상의 결과를 얻으려면 코드의 **Width** 및 **Height** 속성이 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나인지 확인합니다. **Width** 및 **Height** 속성은 라이브 광고의 크기에 따라 변경되지 않습니다.


 

 
