---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "AdControl 속성을 값에 할당하는 방법을 알아봅니다."
title: "XAML 속성 예제"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: fb0533aa0ea760bca686276f886f0afcb21bf6f7


---

# XAML 속성 예제




다음 XAML 예제에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 속성을 값에 할당하는 방법을 보여 줍니다. 이 속성이 설정되지 않은 경우 **AdControl**은 기본값을 사용하여 앱의 사용자 환경과 일치하는 광고를 만듭니다.

값은 예제로 제공된 것입니다. 코드에서 이러한 함수 및 속성의 값을 앱에 적절하게 설정합니다.

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


