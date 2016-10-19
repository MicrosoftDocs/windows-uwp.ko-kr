---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "Microsoft Advertising 라이브러리를 설치하는 방법을 알아봅니다."
title: "Microsoft Advertising 라이브러리 설치"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: a18822b167b7b4dab2dee02439c82c1a0dafcf31


---

# Microsoft Advertising 라이브러리 설치




Windows 10용 UWP(유니버설 Windows 플랫폼) 앱의 경우 Microsoft Advertising 라이브러리는 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)에 포함되어 있습니다. 이 SDK는 Visual Studio 2015의 확장입니다. 이 SDK에 대한 자세한 내용은 [이 문서](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)를 참조하세요.

> **참고**&nbsp;&nbsp;Windows 10 SDK(14393) 이상을 설치한 경우 JavaScript/HTML UWP 앱에 광고를 추가하려면 WinJS 라이브러리도 설치해야 합니다. 이 라이브러리는 Windows 10 SDK 이전 버전에는 포함되었지만 Windows 10 SDK(14393)부터는 별도로 설치해야 합니다. WinJS를 설치하려면 [WinJS 다운로드](http://try.buildwinjs.com/download/GetWinJS/)를 참조하세요.

Windows 8.1 및 Windows Phone 8.x용 XAML 및 JavaScript/HTML 앱의 경우 Microsoft Advertising 라이브러리는 [Windows 및 Windows Phone 8.x용 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)에 포함되어 있습니다. 이 SDK는 Visual Studio 2015 또는 Visual Studio 2013의 확장입니다.

Windows Phone Silverlight 8.x 앱의 경우 Microsoft Advertising 라이브러리는 프로젝트에 다운로드하고 설치할 수 있는 NuGet 패키지에서 사용 가능합니다. 자세한 내용은 [Windows Phone Silverlight의 AdControl](adcontrol-in-windows-phone-silverlight.md)을 참조하세요.

## 광고에 대한 라이브러리 이름


Microsoft Store Services SDK와 Microsoft Advertising SDK for Windows 및 Windows Phone 8.x에서 사용할 수 있는 몇 가지 다른 광고 라이브러리는 다음과 같습니다.

* Microsoft Store Services SDK에는 XAML 및 JavaScript/HTML 앱을 위한 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 및 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 클래스를 제공하는 Microsoft Advertising 라이브러리가 포함되어 있습니다.

* Microsoft Advertising SDK for Windows 및 Windows Phone 8.x에는 두 가지 광고 라이브러리 집합이 포함되어 있습니다. 바로 Microsoft Advertising용 라이브러리(JavaScript/HTML 및 XAML 앱용 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 및 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 클래스)와 광고 조정용 라이브러리(**AdMediatorControl** 클래스 제공)입니다.

이 설명서에서는 Microsoft Advertising 라이브러리의 **AdControl** 및 **InterstitialAd** 클래스를 사용하여 배너 또는 동영상 중간 광고를 표시하는 방법을 설명합니다. Windows 8.1 및 Windows Phone 8.x 앱에 대한 광고 조정 사용에 대한 자세한 내용은 [광고 조정을 사용하여 수익 최대화](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)를 참조하세요.

>**참고**&nbsp;&nbsp;**AdMediatorControl** 클래스를 사용한 광고 조정은 현재 Windows 10용 UWP 앱에 지원되지 않습니다. 동일한 배너 광고용(**AdControl**) 및 동영상 중간 광고용(**InterstitialAd**) API를 사용하는 UWP 앱에 대한 서버 측 중재가 출시 예정입니다.

앱 코드에서 광고 컨트롤 중 하나를 사용하려면 먼저 프로젝트에서 해당 라이브러리를 참조해야 합니다. 다음 표에서는 Visual Studio의 **참조 관리자** 대화 상자에 표시되는 각 라이브러리 이름을 표시합니다.


<table>
    <thead>
        <tr><th>컨트롤 이름</th><th>프로젝트 유형</th><th>참조 관리자의 라이브러리 이름</th><th>버전 번호</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">**AdControl** 및 **InterstitialAd**(XAML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising SDK for XAML</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Ad Mediator SDK for Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>Ad Mediator SDK for Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">**AdControl** 및 **InterstitialAd**(JavaScript/HTML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising SDK for JavaScript</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Microsoft Advertising SDK for Windows 8.1 Native(JS)</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>Microsoft Advertising SDK for Windows Phone 8.1 Native(JS)</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">**AdMediatorControl**(XAML만)</td>
            <td>UWP</td>
            <td>Microsoft Advertising Universal SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Ad Mediator SDK for Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>Ad Mediator SDK for Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 



<!--HONumber=Sep16_HO2-->


