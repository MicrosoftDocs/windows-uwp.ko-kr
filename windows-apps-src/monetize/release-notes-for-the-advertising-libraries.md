---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Windows 10, Windows 8.1, Windows Phone 8.1 및 Windows Phone 8 용 XAML 및 JavaScript/HTML 앱을 지 원하는 Microsoft 광고 라이브러리의 릴리스 정보를 확인 하세요.
title: 광고 라이브러리의 릴리스 정보
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 릴리스 정보
ms.localizationpriority: medium
ms.openlocfilehash: 10762d28191dfe59ae6f63f06cbeb0dd3e8a9f51
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88969921"
---
# <a name="release-notes-for-the-advertising-libraries"></a>광고 라이브러리의 릴리스 정보

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세히 알아보기](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 섹션에서는 Microsoft 광고 라이브러리의 현재 릴리스에 대 한 릴리스 정보를 제공 합니다. 이러한 라이브러리는 Windows 10, Windows 8.1 Windows Phone 8.1 및 Windows Phone 8 용 XAML 및 JavaScript/HTML 앱을 지원 합니다.

## <a name="installation"></a>설치


Microsoft 광고 라이브러리는 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)의 일부로 제공 됩니다. SDK를 설치 하는 방법에 대 한 자세한 내용은 [MICROSOFT ADVERTISING Sdk 설치](install-the-microsoft-advertising-libraries.md)를 참조 하세요.

## <a name="uninstall-previous-versions"></a>이전 버전 제거

최신 Microsoft Advertising SDK를 설치 하기 전에 SDK의 이전 인스턴스를 모두 제거 하는 것이 좋습니다. 자세한 내용은 [MICROSOFT ADVERTISING SDK 설치](install-the-microsoft-advertising-libraries.md)를 참조 하세요.

## <a name="target-architecture-specific-build-outputs"></a>대상 아키텍처 관련 빌드 출력

Microsoft 광고 라이브러리를 사용 하는 경우 프로젝트의 **모든 CPU** 를 대상으로 할 수 없습니다. 프로젝트가 **모든 CPU** 플랫폼을 대상으로 하는 경우 Microsoft 광고 라이브러리에 대 한 참조를 추가한 후 프로젝트에 경고가 표시 될 수 있습니다. 이 경고를 제거 하려면 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 하도록 프로젝트를 업데이트 합니다. 자세한 내용은 [알려진 문제](known-issues-for-the-advertising-libraries.md)를 참조 하세요.

## <a name="c-support"></a>C + + 지원

Microsoft 광고 라이브러리 ( **Adcontrol** 및 **Interstitialad** 클래스 포함)는*interop*(Windows 런타임 상호 운용성)을 사용 하 여 c + + 및 DirectX로 작성 된 앱을 지원 합니다. XAML 및 c + +를 사용한 프로그래밍에 대 한 자세한 내용 및 예제는 [형식 시스템](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)을 참조 하세요.

## <a name="no-toolbox-control"></a>도구 상자 컨트롤 없음

[MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)의 Microsoft 광고 라이브러리 현재 릴리스에서는 **Adcontrol** 또는 **interstalad** 를 앱의 디자인 화면으로 끌기 위한 도구 상자 컨트롤이 없습니다. 태그와 코드에 이러한 컨트롤을 추가 하는 방법에 대 한 지침은 [개발자 연습](developer-walkthroughs.md)을 참조 하세요.

## <a name="latitude-and-longitude-properties-no-longer-available"></a>위도 및 경도 속성을 더 이상 사용할 수 없음

**Adcontrol** 클래스는 더 이상 UWP 앱에 대 한 **위도** 및 **경도** 속성을 포함 하지 않습니다. 대신, ad 컨트롤에 기본 제공 되는 코드는 이러한 값을 검색 하 여 앱을 대신 하 여 ad 서버에 보냅니다.


 

 
