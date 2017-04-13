---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "Microsoft Store Services SDK에 있는 Microsoft Advertising 라이브러리에 대한 릴리스 정보를 검토합니다."
title: "Advertising 라이브러리에 대한 릴리스 정보"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 광고, 광고, 릴리스 정보"
ms.openlocfilehash: f3d07df6e64c96e9070cb82bd7ac6e96b9cad1ee
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="release-notes-for-the-advertising-libraries"></a>Advertising 라이브러리에 대한 릴리스 정보




이 섹션에서는 Microsoft Store Services SDK(UWP 앱용)와 Microsoft Advertising SDK for Windows 및 Windows Phone 8.x(Windows 8.1 및 Windows Phone 8.x 앱용)에 있는 Microsoft Advertising 라이브러리의 현재 릴리스에 대한 릴리스 정보를 제공합니다. 이러한 라이브러리는 Windows 10, Windows 8.1, Windows Phone 8.1 및 Windows Phone 8용 XAML 및 JavaScript/HTML 앱을 지원합니다.

## <a name="installation"></a>설치


Microsoft Advertising 라이브러리는 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)(UWP 앱용)와 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)(Windows 8.1 및 Windows Phone 8.x 앱용)의 일부로 사용할 수 있습니다. SDK와 여기에 포함된 라이브러리의 설치에 대한 자세한 내용은 [Microsoft Advertising 라이브러리 설치](install-the-microsoft-advertising-libraries.md)를 참조하세요.

Windows Phone 8.x Silverlight 프로젝트용 Microsoft Advertising 어셈블리를 가져오려면 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)를 설치하고 Visual Studio에서 프로젝트를 연 다음 **프로젝트** > **연결된 서비스 추가** > **Ad Mediator**로 이동하여 어셈블리를 자동으로 다운로드 합니다. 이 작업을 수행한 후 광고 조정을 사용하지 않으려면 프로젝트에서 광고 조정자 참조를 제거할 수 있습니다. 자세한 내용은 [Windows Phone Silverlight의 AdControl](adcontrol-in-windows-phone-silverlight.md)을 참조하세요.


## <a name="uninstall-previous-versions"></a>이전 버전 제거

Microsoft Store Services SDK(UWP 앱용) 또는 Microsoft Advertising SDK for Windows 및 Windows Phone 8.x(Windows 8.1 및 Windows Phone 8.x 앱용)를 설치하기 전에 Microsoft Universal Ad Client SDK 또는 Microsoft Advertising SDK의 이전 인스턴스를 모두 제거하는 것이 좋습니다.

## <a name="target-architecture-specific-build-outputs"></a>아키텍처별 빌드 출력 대상 지정

Microsoft Advertising 라이브러리를 사용할 때는 프로젝트의 **어떤 CPU**도 대상으로 지정할 수 없습니다. 프로젝트가 **임의 CPU** 플랫폼을 대상으로 하는 경우 Microsoft Advertising 라이브러리에 대한 참조를 추가한 후 프로젝트에 경고가 표시될 수 있습니다. 이 경고를 제거하려면 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 자세한 내용은 [알려진 문제](known-issues-for-the-advertising-libraries.md)를 참조하세요.

## <a name="c-support"></a>C++ 지원

Microsoft Advertising 라이브러리(**AdControl** 및 **InterstitialAd** 클래스 포함)는 Windows 런타임 상호 운용성(*interop*)을 사용하여 C++ 및 DirectX로 작성한 앱을 지원합니다. XAML 및 C++를 사용한 프로그래밍에 대한 자세한 내용 및 예제를 보려면 [형식 시스템](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx)을 참조하세요.

## <a name="no-toolbox-control"></a>도구 상자 컨트롤 없음

Microsoft Store Services SDK 또는 Microsoft Advertising SDK for Windows 및 Windows Phone 8.x에 있는 Microsoft Advertising 라이브러리의 현재 릴리스에 에는 **AdControl** 또는 **InterstitialAd**를 앱의 디자인 화면으로 끌기 위한 도구 상자 컨트롤이 없습니다. 태그 및 코드에서 이러한 컨트롤을 추가하는 방법에 대한 자세한 내용은 [개발자 연습](developer-walkthroughs.md)을 참조하세요.

## <a name="latitude-and-longitude-properties-no-longer-available"></a>위도 및 경도 속성을 더 이상 사용할 수 없음

**AdControl** 클래스에는 더 이상 UWP 앱에 대한 **Latitude** 및 **Longitude** 속성이 없습니다. 대신, 광고 컨트롤에 기본 제공된 코드가 앱을 대신해서 이러한 값을 검색한 후 광고 서버로 보냅니다.


 

 
