---
title: 2017년 8월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2017년 8월 Windows 10 개발자 설명서에 추가된 새로운 기능, 동영상 및 개발자 지침
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 1708
ms.date: 08/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aeb6f60396b270a78df5203106635436fe2dabe5
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736203"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>2017년 8월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침 및 동영상이 Windows 개발자를 위한 새 정보와 업데이트된 정보를 포함하여 최근에 추가되었습니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/your-first-app.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="windows-template-studio"></a>Windows Template Studio

새로운 Visual Studio 2017용 [Windows Template Studio](https://aka.ms/wtsinstall) 확장을 사용하여 원하는 페이지, 프레임워크 및 기능으로 신속하게 UWP 앱을 빌드할 수 있습니다. 이 마법사 기반 환경은 시간을 절약하고 앱에 기능을 추가하는 데 발생하는 문제를 줄일 수 있는 입증된 패턴과 모범 사례를 구현합니다.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>조건부 XAML

이제 [버전 적응 앱](../debug-test-perf/version-adaptive-apps.md)을 만들 수 있는 [조건부 XAML](../debug-test-perf/conditional-xaml.md)을 미리 볼 수 있습니다. 조건부 XAML을 통해 XAML 태그에서 ApiInformation.IsApiContractPresent 메서드를 사용할 수 있습니다. 이를 통해 코드 숨김을 사용하지 않고도 API의 존재 여부를 기반으로 태그에서 속성을 설정하고 개체를 인스턴스화할 수 있습니다.

### <a name="game-mode"></a>게임 모드

UWP(유니버설 Windows 플랫폼)을 위한 [게임 모드](https://msdn.microsoft.com/library/windows/desktop/mt808808) API를 사용하여 Windows 10의 게임 모드가 제공하는 장점을 활용하여 가장 최적화된 게임 환경을 만들 수 있습니다. 이러한 API는 **&lt;expandedresources.h&gt;** 헤더에 있습니다.

![게임 모드](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>제출 API가 비디오 예고편 및 게임 옵션 지원

[Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)가 이제 앱 제출에 [비디오 예고편](../monetize/manage-app-submissions.md#trailer-object)과 [게임 옵션](../monetize/manage-app-submissions.md#gaming-options-object)을 포함하도록 지원합니다.


## <a name="developer-guidance"></a>개발자 지침

### <a name="data-schemas-for-store-products"></a>스토어 제품용 데이터 스키마

[스토어 제품용 데이터 스키마](../monetize/data-schemas-for-store-products.md) 문서가 추가되었습니다. 이 문서는 [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense)를 포함하여 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스의 몇 가지 개체에 사용할 수 있는 스토어 관련 데이터에 대한 스키마를 제공합니다.

### <a name="desktop-bridge"></a>데스크톱 브리지

Windows 10 사용자를 위한 최신 환경을 추가하는 데 도움이 되는 두 개의 가이드를 추가했습니다.

올바른 파일을 찾아 참고하고, Windows 10 사용자를 위한 UWP 환경을 돋보이게 하려면 [Windows 10용 데스크톱 응용 프로그램 향상](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance)을 참조하세요.  

UWP 앱 컨테이너에서 실행해야 하는 최신 XAML UI 및 다른 UWP 환경을 통합하려면 [최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend)을 참조하세요.

### <a name="getting-started-with-point-of-service"></a>서비스 지점 시작

[서비스 지점 디바이스를 시작](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started)하는 데 도움이 되는 새 가이드를 추가했습니다. 여기에서는 디바이스 열거, 디바이스 기능 확인, 디바이스 클레임, 디바이스 공유에 대해 다룹니다. 

### <a name="xbox-live"></a>Xbox Live

UWP 및 XDK(Xbox 개발자 키트) 게임 모두에 대한 Xbox Live 개발자를 위한 문서를 추가했습니다.

Xbox Live API를 사용하여 게임을 Xbox Live 소셜 게임 네트워크에 연결하는 방법은 [Xbox Live 개발자 가이드](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/)를 참조하세요.

[Xbox Live 크리에이터스 프로그램](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators)을 사용하면 모든 UWP 게임 개발자가 PC와 Xbox One에 모두에 Xbox Live 지원 게임을 게시할 수 있습니다.

Xbox Live 개발자가 사용할 수 있는 프로그램 및 기능에 대한 자세한 내용은 [Xbox Live 개발자 프로그램 개요](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview)를 참조하세요.

## <a name="videos"></a>동영상

### <a name="mixed-reality"></a>혼합 현실

[Microsoft HoloLens Course 250](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250)에 대한 새로운 자습서 동영상 시리즈가 추가되었습니다. 혼합 현실을 위한 도구를 이미 설치했고 개발의 기본에 친숙하다면, 이 동영상 과정을 확인하여 혼합 현실 디바이스 간에 공유 환경을 만드는 정보를 얻으십시오.

### <a name="narrator-and-dev-mode"></a>내레이터 및 개발자 모드

[내레이터](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)를 사용하여 앱의 화면 읽기 환경을 테스트할 수 있다는 것을 이미 알고 계실 수 있습니다. 하지만 내레이터에는 개발자 모드도 있으며, 이를 통해 내레이터에 노출되는 정보를 한눈에 확인할 수 있습니다. [동영상을 시청](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode)하고 [내레이터 개발자 모드](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode)에 대해 자세히 알아보세요.

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio에 대한 자세한 개요는 [이 동영상](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio)에서 볼 수 있습니다. 준비가 되었다면 [확장을 설치](https://aka.ms/wtsinstall)하거나 [소스 코드 및 설명서를 확인](https://aka.ms/wtsinstall)하십시오.