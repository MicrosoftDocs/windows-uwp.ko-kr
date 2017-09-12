---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Microsoft Advertising 라이브러리를 사용하여 Windows10, Windows8.1 또는 Windows Phone 8.1 앱에 중간 광고를 포함하는 방법을 알아봅니다."
title: "중간 광고"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, 광고 관리, 중간 광고"
ms.openlocfilehash: 16cb78d9419d54a1bd56fb855ca1fad6fedf665a
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2017
---
# <a name="interstitial-ads"></a>중간 광고

이 연습에서는 Windows 10용 UWP(유니버설 Windows 플랫폼) 앱 및 게임과 Windows 8.1 또는 Windows Phone 8.1용 앱에 중간 광고를 포함하는 방법을 보여 줍니다. C# 및 C++를 사용하여 JavaScript/HTML 앱 및 XAML 앱에 중간 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>중간 광고란?

앱 또는 게임에서 UI의 일부로 제한되는 표준 배너 광고와 달리 중간 광고는 전체 화면에 표시됩니다. 두 가지 기본 형태는 게임에서 자주 사용됩니다.

* *유료* 광고를 사용하는 경우 사용자는 정기적으로 광고를 시청해야 합니다. 예를 들어 게임 레벨 중간에 광고를 사용할 수 있습니다.

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* *보상 기반* 광고를 사용하면 사용자는 레벨을 완료하기 위한 힌트나 추가 시간 등의 혜택을 명시적으로 원하고 앱의 사용자 인터페이스를 통해 광고를 초기화합니다.

Microsoft에서는 앱과 게임에서 사용할 수 있는 두 가지 유형의 중간 광고를 제공합니다.

* **동영상 중간 광고**: Windows 10용 UWP(유니버설 Windows 플랫폼) 앱과 Windows 8.1 또는 Windows Phone 8.1용 앱에 사용할 수 있습니다.

* **배너 중간 광고**: Windows 10용 UWP 앱에만 사용할 수 있습니다.

> [!NOTE]
> 중간 광고용 API는 동영상 재생 시를 제외하고 어떠한 사용자 인터페이스도 처리하지 않습니다. 앱에 중간 광고를 통합하는 방법을 고려할 때 수행할 작업 및 피할 작업에 대한 지침을 보려면 [중간 광고 모범 사례](ui-and-user-experience-guidelines.md#interstitialbestpractices10)를 참조하세요.

## <a name="build-an-app-with-interstitial-ads"></a>중간 광고를 사용하여 앱 빌드

앱에 중간 광고를 게재하려면 프로젝트 유형에 대한 지침을 따르세요.

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++(DirectX interop)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>필수 구성 요소

* UWP 앱의 경우 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) 및 Visual Studio 2015 이상 릴리스를 설치합니다.

* Windows8.1 또는 Windows Phone 8.1 앱의 경우 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)와 Visual Studio 2015 또는 Visual Studio 2013을 설치합니다.

<span id="interstitialadsxaml10"/>
### <a name="xamlnet"></a>XAML/.NET

이 섹션에서는 C# 예제를 제공하지만 Visual Basic 및 C++도 XAML/.NET에 지원됩니다. 전체 C# 코드 예제는 [C#으로 작성한 중간 광고 샘플 코드](interstitial-ad-sample-code-in-c.md)를 참조하세요.

1. Visual Studio에서 프로젝트를 엽니다.

2. **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

  * UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.

  * Windows8.1 프로젝트: **Windows8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

  * Windows Phone 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows Phone 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

3.  앱의 적절한 코드 파일(예: MainPage.xaml.cs 또는 다른 페이지의 코드 파일)에 다음 네임스페이스 참조를 추가합니다.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  앱의 적절한 위치(예: ```MainPage``` 또는 다른 페이지)에서 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 개체를 비롯하여 중간 광고 응용 프로그램 ID 및 광고 단위 ID를 나타내는 여러 문자열 필드를 선언합니다. 다음 코드 예제에서는 `myAppId` 및 `myAdUnitId` 필드를 [테스트 모드 값](test-mode-values.md)에 제공된 동영상 중간 광고용 테스트 값에 할당합니다.

  > [!NOTE]
  > 모든 **InterstitialAd**에는 컨트롤 할 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 앱을 스토어에 게시하기 전, Windows 개발자 센터에서 [이 테스트 값을 라이브 값으로](#release) 변경하세요.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  시작할 때 실행되는 코드에서(예를 들면 해당 페이지에 대한 생성자에서) **InterstitialAd** 개체를 인스턴스화하고 개체 이벤트에 이벤트 처리기를 연결합니다.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  *동영상 중간* 광고를 표시하려는 경우: 중간 광고가 필요한 시점에서 약 30~60초 전에 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 메서드를 사용하여 광고를 미리 가져옵니다. 이렇게 하면 광고가 게재되기 전에 광고를 요청하고 준비할 시간이 충분해집니다. 광고 유형에는 **AdType.Video**를 지정합니다.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

  *배너 중간* 광고(UWP 앱 전용)를 표시하려는 경우: 광고가 필요한 시점에서 약 5-8초 전에 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 메서드를 사용하여 광고를 미리 가져옵니다. 이렇게 하면 광고가 게재되기 전에 광고를 요청하고 준비할 시간이 충분해집니다. 광고 유형에는 **AdType.Display**를 지정합니다.

  ```csharp
  myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
  ```

6.  동영상 또는 배너 중간 광고를 게재하려는 코드 지점에서 **InterstitialAd**를 게재할 준비가 되었는지 확인한 다음 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 메서드를 사용하여 게재합니다.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  **InterstitialAd** 개체에 대한 이벤트 처리기를 정의합니다.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  앱을 빌드 및 테스트하여 테스트 광고가 표시되는지 확인합니다.

<span id="interstitialadshtml10"/>
### <a name="htmljavascript"></a>HTML/JavaScript

다음 지침에서는 사용자가 Visual Studio에서 JavaScript용 유니버설 Windows 프로젝트를 만들었으며 특정 CPU를 대상으로 한다고 가정합니다. 전체 코드 샘플을 보려면 [JavaScript로 작성한 중간 광고 샘플 코드](interstitial-ad-sample-code-in-javascript.md)를 참조하세요.

1. Visual Studio에서 프로젝트를 엽니다.

2.  **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

  * UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for JavaScript**(버전 10.0) 옆의 확인란을 선택합니다.

  * Windows 8.1 프로젝트: **Windows 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows 8.1 Native(JS)** 옆의 확인란을 선택합니다.

  * Windows8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows Phone 8.1 Native(JS)** 옆의 확인란을 선택합니다.

3.  프로젝트 내 HTML 파일의 **&lt;head&gt;** 섹션에서 default.css 및 default.js에 대한 프로젝트의 JavaScript 참조 다음에 ad.js에 대한 참조를 추가합니다. UWP 프로젝트에서 다음 참조를 추가합니다.

  ``` HTML
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  Windows 8.1 또는 Windows Phone 8.1 프로젝트에서 다음 참조를 추가합니다.

  ``` HTML
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  프로젝트의 .js 파일에서 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 개체를 비롯하여 중간 광고 응용 프로그램 ID 및 광고 단위 ID를 포함하는 여러 필드를 선언합니다. 다음 코드 예제에서는 `applicationId` 및 `adUnitId` 필드를 [테스트 모드 값](test-mode-values.md)에 제공된 동영상 중간 광고용 테스트 값에 할당합니다.

  > [!NOTE]
  > 모든 **InterstitialAd**에는 컨트롤할 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 앱을 스토어에 게시하기 전, Windows 개발자 센터에서 [이 테스트 값을 라이브 값으로](#release) 변경하세요.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  시작할 때 실행되는 코드에서(예를 들면 해당 페이지에 대한 생성자에서) **InterstitialAd** 개체를 인스턴스화하고 개체에 대한 이벤트 처리기를 연결합니다.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. *동영상 중간* 광고를 표시하려는 경우: 중간 광고가 필요한 시점에서 약 30~60초 전에 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 메서드를 사용하여 광고를 미리 가져옵니다. 이렇게 하면 광고가 게재되기 전에 광고를 요청하고 준비할 시간이 충분해집니다. 광고 유형에는 **InterstitialAdType.video**를 지정합니다.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

  *배너 중간* 광고(UWP 앱 전용)를 표시하려는 경우: 광고가 필요한 시점에서 약 5-8초 전에 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 메서드를 사용하여 광고를 미리 가져옵니다. 이렇게 하면 광고가 게재되기 전에 광고를 요청하고 준비할 시간이 충분해집니다. 광고 유형에는 **InterstitialAdType.display**를 지정합니다.

  ```js
  if (interstitialAd) {
      interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
  }
  ```

6.  광고를 게재하려는 코드 지점에서 **InterstitialAd**를 게재할 준비가 되었는지 확인한 다음 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 메서드를 사용하여 게재합니다.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  **InterstitialAd** 개체에 대한 이벤트 처리기를 정의합니다.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  앱을 빌드 및 테스트하여 테스트 광고가 표시되는지 확인합니다.

<span id="interstitialadsdirectx10"/>
### <a name="c-directx-interop"></a>C++(DirectX Interop)

이 샘플에서는 사용자가 Visual Studio에서 C++ **DirectX 및 XAML 앱(유니버설 Windows)** 프로젝트를 만들었으며 특정 CPU 아키텍처를 대상으로 한다고 가정합니다.
 
1. Visual Studio에서 프로젝트를 엽니다.

1.  **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

  * UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.

  * Windows8.1 프로젝트: **Windows8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

  * Windows Phone 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows Phone 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

2.  사용자의 앱에 대한 적절한 헤더 파일(예: DirectXPage.xaml.h)에서 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 개체 및 관련 이벤트 처리기 메서드를 선언합니다.  

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  동일한 헤더 파일에서 중간 광고 응용 프로그램 ID 및 광고 단위 ID를 나타내는 여러 문자열 필드를 선언합니다. 다음 코드 예제에서는 `myAppId` 및 `myAdUnitId` 필드를 [테스트 모드 값](test-mode-values.md)에 제공된 동영상 중간 광고용 테스트 값에 할당합니다.

  > [!NOTE]
  > 모든 **InterstitialAd**에는 컨트롤 할 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 앱을 스토어에 게시하기 전, Windows 개발자 센터에서 [이 테스트 값을 라이브 값으로](#release) 변경하세요.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  중간 광고 게재를 위해 코드를 추가하고자 하는 .cpp 파일에 다음 네임스페이스 참조를 추가합니다. 다음 예제에서는 앱의 DirectXPage.xaml.cpp 파일에 코드를 추가한다고 가정합니다.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  시작할 때 실행되는 코드에서(예를 들면 해당 페이지에 대한 생성자에서) **InterstitialAd** 개체를 인스턴스화하고 개체 이벤트에 이벤트 처리기를 연결합니다. 다음 예제에서 ```InterstitialAdSamplesCpp```은 프로젝트에 대한 네임스페이스입니다. 사용자의 코드에 필요할 경우 이 이름을 변경할 수 있습니다.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. *동영상 중간* 광고를 표시하려는 경우: 중간 광고가 필요한 시점에서 약 30~60초 전에 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 메서드를 사용하여 광고를 미리 가져옵니다. 이렇게 하면 광고가 게재되기 전에 광고를 요청하고 준비할 시간이 충분해집니다. 광고 유형에는 **AdType::Video**를 지정합니다.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

  *배너 중간* 광고(UWP 앱 전용)를 표시하려는 경우: 광고가 필요하기 약 5-8초 전에 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 메서드를 사용하여 광고를 미리 가져옵니다. 이렇게 하면 광고가 게재되기 전에 광고를 요청하고 준비할 시간이 충분해집니다. 광고 유형에는 **AdType::Display**를 지정합니다.

  ```cpp
  m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
  ```

7.  광고를 게재하려는 코드 지점에서 **InterstitialAd**를 게재할 준비가 되었는지 확인한 다음 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 메서드를 사용하여 게재합니다.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  **InterstitialAd** 개체에 대한 이벤트 처리기를 정의합니다.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. 앱을 빌드 및 테스트하여 테스트 광고가 표시되는지 확인합니다.

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Windows 개발자 센터를 사용하여 라이브 광고와 함께 앱 출시

1.  개발자 센터 대시보드에서 [앱의 광고 수익 창출](../publish/monetize-with-ads.md) 페이지로 이동한 후 [광고 단위를 만듭니다](../monetize/set-up-ad-units-in-your-app.md). 광고 단위 유형에는 표시하려는 중간 광고의 유형에 따라 **동영상 중간 광고** 또는 **배너 중간 광고**를 선택합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.

2. 앱이 Windows 10용 UWP 앱인 경우, [광고 수익 창출](../publish/monetize-with-ads.md) 페이지의 [광고 조정](../publish/monetize-with-ads.md#mediation) 섹션에서 설정을 구성하여 **InterstitialAd**에 대한 광고 조정을 활성화할 수 있습니다. 광고 조정을 통해 Taboola 및 Smaato 같은 기타 유료 광고 네트워크와 Microsoft 앱 프로모션 캠페인에 대한 광고를 포함하여 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 프로모션 기능을 최대화할 수 있습니다.

3.  코드에서 테스트 광고 단위 값을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

4.  Windows 개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

5.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>앱에서 여러 중간광고 컨트롤에 대한 광고 단위 관리

단일 앱에서 여러 **InterstitialAd** 컨트롤을 사용할 수 있습니다. 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [미디어 설정을 구성](../publish/monetize-with-ads.md#mediation)할 수 있고, 각 컨트롤에 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 생성할 수 있습니다. 또 이렇게 하면 저희 서비스가 지원하는 앱에서 더 효과적으로 광고를 최적화 할 수 있습니다.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>중간 광고 모범 사례 및 정책


중간 광고를 효과적으로 사용하는 방법과 준수할 정책에 대한 자세한 내용은 [중간 모범 사례 및 정책](ui-and-user-experience-guidelines.md#interstitialbestpractices10)을 참조하세요.

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>참조 오류 제거: 특정 CPU 플랫폼(XAML 및 HTML) 대상


Microsoft Advertising 라이브러리를 사용할 때는 프로젝트의 **어떤 CPU**도 대상으로 지정할 수 없습니다. 프로젝트가 **임의 CPU** 플랫폼을 대상으로 하는 경우 Microsoft Advertising 라이브러리에 대한 참조를 추가한 후 프로젝트에 경고가 표시될 수 있습니다. 이 경고를 제거하려면 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 자세한 내용은 [알려진 문제](known-issues-for-the-advertising-libraries.md)를 참조하세요.

## <a name="related-topics"></a>관련 항목


* [C#의 중간 광고 샘플 코드](interstitial-ad-sample-code-in-c.md)
* [JavaScript의 중간 광고 샘플 코드](interstitial-ad-sample-code-in-javascript.md)
* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [앱에서 광고 단위 설정](../monetize/set-up-ad-units-in-your-app.md)
