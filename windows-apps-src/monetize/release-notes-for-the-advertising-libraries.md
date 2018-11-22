---
author: Xansky
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Microsoft Advertising 라이브러리에 대한 릴리스 정보를 검토합니다.
title: Advertising 라이브러리에 대한 릴리스 정보
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: Windows 10, uwp, 광고, 광고, 릴리스 정보
ms.localizationpriority: medium
ms.openlocfilehash: dbe932eb9391a4de0304b4be42944b2bced3287a
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7564389"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Advertising 라이브러리에 대한 릴리스 정보




이 섹션에서는 Microsoft Advertising 라이브러리의 최신 릴리스 정보를 제공합니다. 이러한 라이브러리 Windows10, Windows8.1, Windows Phone 8.1 및 WindowsPhone8에 대 한 XAML 및 JavaScript/HTML 앱을 지원합니다.

## <a name="installation"></a>설치


Microsoft 광고 라이브러리는 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)의 일부로 제공되고 있습니다. SDK 설치에 대한 자세한 내용은 [Microsoft Advertising SDK 설치](install-the-microsoft-advertising-libraries.md)를 참조하세요.

## <a name="uninstall-previous-versions"></a>이전 버전 제거

최신 Microsoft Advertising SDK를 설치하기 전에 이전 SDK의 인스턴스를 모두 제거하는 것이 좋습니다. 자세한 내용은 [Microsoft Advertising SDK 설치](install-the-microsoft-advertising-libraries.md)를 참조하세요.

## <a name="target-architecture-specific-build-outputs"></a>아키텍처별 빌드 출력 대상 지정

Microsoft Advertising 라이브러리를 사용할 때는 프로젝트의 **어떤 CPU**도 대상으로 지정할 수 없습니다. 프로젝트가 **임의 CPU** 플랫폼을 대상으로 하는 경우 Microsoft Advertising 라이브러리에 대한 참조를 추가한 후 프로젝트에 경고가 표시될 수 있습니다. 이 경고를 제거하려면 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 자세한 내용은 [알려진 문제](known-issues-for-the-advertising-libraries.md)를 참조하세요.

## <a name="c-support"></a>C++ 지원

Microsoft Advertising 라이브러리(**AdControl** 및 **InterstitialAd** 클래스 포함)는 Windows 런타임 상호 운용성(*interop*)을 사용하여 C++ 및 DirectX로 작성한 앱을 지원합니다. XAML 및 C++를 사용한 프로그래밍에 대한 자세한 내용 및 예제를 보려면 [형식 시스템](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)을 참조하세요.

## <a name="no-toolbox-control"></a>도구 상자 컨트롤 없음

[Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)에 있는 Microsoft Advertising 라이브러리의 현재 릴리스에는 **AdControl** 또는 **InterstitialAd**를 앱의 Design Surface로 끌기 위한 도구 상자 컨트롤이 없습니다. 태그 및 코드에서 이러한 컨트롤을 추가하는 방법에 대한 자세한 내용은 [개발자 연습](developer-walkthroughs.md)을 참조하세요.

## <a name="latitude-and-longitude-properties-no-longer-available"></a>위도 및 경도 속성을 더 이상 사용할 수 없음

**AdControl** 클래스에는 더 이상 UWP 앱에 대한 **Latitude** 및 **Longitude** 속성이 없습니다. 대신, 광고 컨트롤에 기본 제공된 코드가 앱을 대신해서 이러한 값을 검색한 후 광고 서버로 보냅니다.


 

 
