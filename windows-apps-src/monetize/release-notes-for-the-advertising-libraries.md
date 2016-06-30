---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "Microsoft 스토어 참여 및 수익 창출 SDK에 있는 Microsoft Advertising 라이브러리에 대한 릴리스 정보를 검토합니다."
title: "Microsoft Advertising 라이브러리에 대한 릴리스 정보"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 8e2114e969b27d579f62195f026cfcfd9672a94a

---

# Microsoft Advertising 라이브러리에 대한 릴리스 정보


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 섹션에서는 Microsoft 스토어 참여 및 수익 창출 SDK에 있는 Microsoft Advertising 라이브러리 최신 릴리스에 대한 릴리스 정보를 제공합니다. 이러한 라이브러리는 Windows 10, Windows 8.1, Windows Phone 8.1 및 Windows Phone 8용 XAML 및 JavaScript/HTML 앱을 지원합니다.

## 설치


Microsoft Advertising 라이브러리는 [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)의 일부로 사용할 수 있습니다. Windows Phone 8.x Silverlight 이외의 모든 프로젝트 유형에 대해 이전의 독립 실행형 Microsoft 유니버설 광고 클라이언트 SDK 및 Microsoft Advertising SDK 릴리스에 배포되었던 Microsoft Advertising 어셈블리는 이제 Microsoft 스토어 참여 및 수익 창출 SDK와 함께 설치됩니다. SDK와 여기에 포함된 라이브러리의 설치에 대한 자세한 내용은 [Microsoft Advertising 라이브러리 설치](install-the-microsoft-advertising-libraries.md)를 참조하세요.

Windows Phone 8.x Silverlight 프로젝트용 Microsoft Advertising 어셈블리를 가져오려면 [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 설치하고 Visual Studio에서 프로젝트를 연 다음 **프로젝트** > **연결된 서비스 추가** > **광고 중재자**로 이동하여 어셈블리를 자동으로 다운로드 합니다. 이 작업을 수행한 후 광고 조정을 사용하지 않으려면 프로젝트에서 광고 중재자 참조를 제거할 수 있습니다. 자세한 내용은 [Windows Phone Silverlight의 AdControl](adcontrol-in-windows-phone-silverlight.md)을 참조하세요.

## Microsoft Advertising 라이브러리 및 광고 조정 간 차이점 이해

Microsoft Advertising 라이브러리 및 광고 조정 라이브러리는 모두 Microsoft 스토어 참여 및 수익 창출 SDK에 제공되지만 이러한 라이브러리는 여러 가지 용도로 사용됩니다. Microsoft의 배너 또는 동영상 중간 광고를 XAML 또는 JavaScript 앱에 표시하려면 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 및 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 클래스를 사용합니다. XAML 앱에 여러 광고 네트워크의 배너 광고를 표시하려면 **AdMediatorControl** 클래스를 사용합니다(JavaScript/HTML 앱에 대해 광고 조정이 지원되지 않는 경우). 자세한 내용은 [AdMediatorControl과 AdControl의 차이](what-is-the-difference-admediatorcontrol-or-adcontrol.md)를 참조하세요.

## 이전 버전 제거

Microsoft 스토어 참여 및 수익 창출 SDK를 설치하기 전에 Microsoft 범용 광고 클라이언트 SDK 또는 Microsoft Advertising SDK의 모든 이전 인스턴스를 제거하는 것이 좋습니다.

## 아키텍처별 빌드 출력 대상 지정

Microsoft Advertising 라이브러리를 사용할 때는 프로젝트의 **어떤 CPU**도 대상으로 지정할 수 없습니다. 프로젝트가 **임의 CPU** 플랫폼을 대상으로 하는 경우 Microsoft Advertising 라이브러리에 대한 참조를 추가한 후 프로젝트에 경고가 표시될 수 있습니다. 이 경고를 제거하려면 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 자세한 내용은 [알려진 문제](known-issues-for-the-advertising-libraries.md)를 참조하세요.

## C++ 지원

Microsoft Advertising 라이브러리(**AdControl** 및 **InterstitialAd** 클래스 포함)는 Windows 런타임 상호 운용성(*interop*)을 사용하여 C++ 및 DirectX로 작성한 앱을 지원합니다. XAML 및 C++를 사용한 프로그래밍에 대한 자세한 내용 및 예제를 보려면 [형식 시스템](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx)을 참조하세요.

## 도구 상자 컨트롤 없음

Microsoft 스토어 참여 및 수익 창출 SDK에 있는 최신 버전의 Microsoft Advertising 라이브러리에는 **AdControl** 또는 **InterstitialAd**를 앱의 디자인 화면으로 끌기 위한 도구 상자 컨트롤이 없습니다. 태그 및 코드에서 이러한 컨트롤을 추가하는 방법에 대한 자세한 내용은 [개발자 연습](developer-walkthroughs.md)을 참조하세요.

## 위도 및 경도 속성을 더 이상 사용할 수 없음

**AdControl** 클래스에는 더 이상 UWP 앱에 대한 **Latitude** 및 **Longitude** 속성이 없습니다. 대신, 광고 컨트롤에 기본 제공된 코드가 앱을 대신해서 이러한 값을 검색한 후 광고 서버로 보냅니다.

## 중요 정보

EULA(최종 사용자 사용권 계약)를 모두 읽으세요. [중요 알림 - EULA](important-notice-eula.md) 항목을 참조하세요.

 

 



<!--HONumber=Jun16_HO4-->


