---
author: QuinnRadich
title: 2017년 12월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2017년 12월 Windows 10 개발자 설명서에 추가된 새로운 기능, 동영상 및 개발자 지침
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 12월
ms.author: quradic
ms.date: 12/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 93ef3de0dc86ab9708f7be99836204c2232dfef4
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4565309"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>2017년 12월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. Fall Creators Update 이후 Windows 개발자를 위한 새 정보와 업데이트된 정보를 포함하는 다음과 같은 기능 개요, 개발자 지침 및 샘플이 추가되었습니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality: 마니아의 가이드

Mixed Reality의 세계에 대해 보다 깊이 알아보려는 기술 매니아를 대상으로 [마니아 가이드](https://docs.microsoft.com/en-us/windows/mixed-reality/enthusiast-guide/)는 Windows Mixed Reality에 대해 사람들이 가지는 주요 질문에 답변합니다. 

가이드에는 다음이 나와 있습니다. 
- 구입하기 전 FAQ 
- PC 호환성 확인 방법 
- 설치 지침 
- 헤드셋 및 컨트롤러 사용 방법 
- 몰입형 게임, 360 동영상, 2D 앱, WebVR, SteamVR을 다운로드 및 재생하는 방법 
- 문제 해결 방법 등

![Windows Mixed Reality 헤드셋 사용자 및 동작 컨트롤러](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>키보드 조작

업데이트된 [상호 작용 키보드](../design/input/keyboard-interactions.md)로 고급 사용자에게 액세스할 수 있는 환경 및 기능을 제공하기 위해 UWP 앱을 디자인하고 최적화합니다. 이러한 상호 작용에 대한 Fall Creators Update에 추가된 새로운 개선 사항을 반영하도록 권장 사항 및 지침을 업데이트했습니다.

앱의 키보드 기능을 확장하려면 [키보드 가속기](../design/input/keyboard-accelerators.md) 및 [사용자 지정 키보드 상호 작용](../design/input/custom-keyboard-interactions.md)을 참조하세요.

터치 상호 작용을 지원하는 디바이스에서 [터치 키보드의 현재 상태에 응답](../design/input/respond-to-the-presence-of-the-touch-keyboard.md) 및 [입력된 범위 변경 터치 키보드를 사용 하 여](../design/input/use-input-scope-to-change-the-touch-keyboard.md) 문서를 참고하여 키보드 기능을 추가합니다.

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

Microsoft Collaborate 포털은 엔지니어링 시스템 작업 항목(버그, 기능 요청 등)의 공유와 콘텐츠(빌드, 문서, 사양)의 배포를 를 활성화하여 Microsoft 에코 시스템 내에서 엔지니어링 협업을 간소화하는 도구와 서비스를 제공합니다. [자세한 내용을 알아보세요](https://docs.microsoft.com/en-us/collaborate).

![개발자 센터 대시보드의 Microsoft Collaborate](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>UWP 프로젝트가 있는 데스크톱 응용 프로그램 패키지

Visual Studio 2017 버전 15.5에서는 **Windows 응용 프로그램 패키징 프로젝트** 템플릿이 업데이트되어 UWP 프로젝트를 훨씬 쉽게 포함시킬 수 있습니다. 더 이상 JavaScript 기반 패키징 프로젝트를 사용하여 패키지 매니페스트를 수동으로 조정할 필요가 없습니다.  

이 새로운 템플릿을 사용하여 데스크톱 응용 프로그램을 패키징하는 방법에 대한 지침은 [Visual Studio를 사용한 앱 패키징](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)을 참조하세요.

패키지에 UWP 프로젝트를 추가하는 방법에 대한 지침은 [최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend)을 참조하세요.

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>Windows 개발자 센터 참가자 프로그램의 개발자는 이제 구독 추가 기능을 사용할 수 있습니다.

[개발자 센터 참가자 프로그램](../publish/dev-center-insider-program.md)에 가입한 모든 개발자는 이제 구독 추가 기능을 사용하여 그들의 앱 내에서 자동화된 반복 청구 기간으로 디지털 제품을 판매할 수 있습니다(예: 앱 기능 또는 디지털 콘텐츠). 자세한 내용은 [앱에 구독 추가 기능을 사용하도록 설정](../monetize/enable-subscription-add-ons-for-your-app.md)을 참조하세요.

## <a name="developer-guidance"></a>개발자 지침

### <a name="color"></a>색상

최적의 사용자 경험을 위해 앱에서 색을 사용하는 방법에 대해 새로운 몇 가지 지침을 추가했습니다. UI 디자인 및 접근성에 대한 일반적인 지침뿐 아니라 API 사용 시나리오 또한 포함됩니다. 또한 Xbox에서 사용할 수 있는 사용자 테마 컬러 목록을 업데이트했습니다. [여기에서 업데이트된 색 문서를 확인하세요.](../design/style/color.md)

![유니버설 windows 색상표](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>데이터 액세스 가이드

앱에서 직접 SQL Server 데이터베이스에 액세스하는 방법을 안내하기 위해 [SQL Server 가이드](../data-access/sql-server-databases.md)가 추가되었습니다. 서비스 계층은 필요하지 않습니다.

또한 [SQLite 가이드](../data-access/sqlite-databases.md)를 보다 깔끔한 모습과 느낌으로 완전히 리모델링했으며 사용자 디바이스의 경량 데이터베이스에 데이터를 저장 및 검색하는 최신 모범 사례를 포함했습니다.

### <a name="forms"></a>양식

사용자의 데이터를 수집 및 전송하기 위해 [앱에서 양식을 작성하는 방법](../design/controls-and-patterns/forms.md)에 대한 새 문서를 추가했습니다. 양식을 구현하는 구체적인 정보 및 이를 언제 또한 어디에 사용하는지에 대한 일반적인 가이드가 포함됩니다.

### <a name="intro-to-app-design"></a>앱 디자인 소개

유니버설 Windows 플랫폼(UWP) 디자인 지침은 세련되고 아름다운 앱을 디자인 및 빌드하기 위한 리소스입니다. [새로운 소개](../design/basics/design-and-ui-intro.md)는 모든 UWP 앱에 포함된 유니버설 디자인 기능에 대한 개요 및 여러 장치에 걸쳐 확장되는 UI(사용자 인터페이스)를 빌드하기 위해 문서를 사용할 수 있는 방법을 포함합니다.


### <a name="request-ratings-and-reviews"></a>평점 및 리뷰 요청

[앱에 대한 평점 및 리뷰를 요청](../monetize/request-ratings-and-reviews.md)하는 방법을 보여주는 새 문서를 추가했습니다. 앱의 컨텍스트 내에서 평점을 표시하고 대화를 검토하거나 Store에서 앱에 대한 평점 및 리뷰 페이지를 열 수 있습니다.

## <a name="samples"></a>샘플

### <a name="customer-orders"></a>고객 주문

[고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 샘플이 저장소 패턴을 사용하고 여러 데이터 소스(Sqlite, SQL Azure, REST 서비스 포함)에 연결하는 방법과 같은 데이터 액세스 관련 모범 사례를 안내하도록 업데이트되었습니다.

## <a name="videos"></a>비디오

### <a name="package-a-net-app-in-visual-studio"></a>Visual Studio에서 .NET 앱 패키징

그 어느 때보다도 쉽게 데스크톱 앱을 유니버설 Windows 플랫폼으로 가져올 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=fJkbYPyd08w)하여 배포를 위해 .NET 앱을 패키징하는 방법을 알아보고 자세한 내용은 [이 페이지를 확인](../porting/desktop-to-uwp-packaging-dot-net.md)하세요.