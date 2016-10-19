---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Microsoft Store Services SDK의 Microsoft Advertising 라이브러리를 사용하여 Windows 10, Windows 8.1 또는 Windows Phone 8.1 앱에 중간 광고를 포함하는 방법에 알아봅니다."
title: "중간 광고"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 4082fdd17ba42fd2b6a7659095b019c1ad4875a0

---

# 중간 광고




이 연습에서는 Microsoft Store Services SDK의 Microsoft Advertising 라이브러리를 사용하여 Windows 10, Windows 8.1 또는 Windows Phone 8.1 앱에 중간 광고를 포함하는 방법을 보여 줍니다.

C# 및 C++를 사용하여 JavaScript/HTML 앱 및 XAML 앱에 중간 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

<span id="whatareinterstitialads10"/>
## 중간 광고란?

배너 광고와 달리 중간 광고(또는 *interstitials*)는 앱의 전체 화면에 표시됩니다. 두 가지 기본 형태는 게임에서 자주 사용됩니다.

* *유료* 광고를 사용하는 경우 사용자는 정기적으로 광고를 시청해야 합니다. 예를 들어 게임 레벨 중간에 광고를 사용할 수 있습니다.

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* *보상 기반* 광고를 사용하면 사용자는 레벨을 완료하기 위한 힌트나 추가 시간 등의 혜택을 명시적으로 원하고 앱의 사용자 인터페이스를 통해 비디오 광고를 초기화합니다.

    이 SDK는 비디오 재생 시를 제외하고 어떤 사용자 인터페이스도 처리하지 않습니다. 앱에 중간 광고를 통합하는 방법을 고려할 때 수행할 작업 및 피할 작업에 대한 지침을 보려면 [중간 광고 모범 사례](ui-and-user-experience-guidelines.md#interstitialbestpractices10)를 참조하세요.

## 중간 광고를 사용하여 앱 빌드


### 필수 조건

* UWP 앱: Visual Studio 2015와 함께 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)를 설치합니다.
* Windows 8.1 또는 Windows Phone 8.1 앱의 경우 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)와 Visual Studio 2015 또는 Visual Studio 2013을 설치합니다.

### 코드 개발

* [XAML/.NET 앱에 대한 단계](#interstitialadsxaml10)
* [HTML/JavaScript에 대한 단계](#interstitialadshtml10)
* [C++(DirectX Interop)에 대한 단계](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### 중간 광고(XAML/.NET)

> **참고** 이 섹션에서는 C# 예제를 제공하지만 Visual Basic 및 C++도 지원됩니다.
 
1. Visual Studio에서 프로젝트를 엽니다.
2. **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

    -   UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

    -   Windows Phone 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows Phone 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

3.  앱 코드에 다음 네임스페이스 참조를 포함합니다.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  `MyAppId` 및 `MyAdUnitId` 속성을 선언합니다.

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **참고** 앱을 제출하기 전에 테스트 값을 라이브 값으로 바꿉니다.

5.  [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)를 인스턴스화하고, 모든 이벤트 처리기를 연결하고, 광고를 요청합니다.

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  광고를 표시해야 하는 코드 지점에서 광고가 준비되었는지 확인하고 표시합니다.

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  이벤트를 정의하고 코딩합니다.

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 `MyAppId` 속성을 할당합니다. 이 값은 테스트용으로만 사용됩니다. 따라서 앱을 게시하기 전에 라이브 값으로 바꿉니다.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 `MyAdUnitId` 속성을 할당합니다. 이 값은 테스트용으로만 사용됩니다. 따라서 앱을 게시하기 전에 라이브 값으로 바꿉니다.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  앱을 빌드 및 테스트하여 테스트 광고가 표시되는지 확인합니다.

<span id="interstitialadshtml10"/>
### 중간 광고(HTML/JavaScript)

이 샘플에서는 사용자가 Visual Studio 2015에서 JavaScript용 범용 앱 프로젝트를 만들었으며 특정 CPU를 대상으로 한다고 가정합니다.

1. Visual Studio에서 프로젝트를 엽니다.
2.  **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

    -   UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for JavaScript**(버전 10.0) 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows 8.1 Native(JS)** 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows Phone 8.1 Native(JS)** 옆의 확인란을 선택합니다.

3.  HTML에서 다음 스크립트 참조를 포함합니다.

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  `myAppId` 및 `myAdUnitId` 속성을 선언합니다.

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  **InterstitialAd**를 인스턴스화하고, 모든 이벤트 처리기를 연결하고, 광고를 요청합니다.

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  광고를 표시해야 하는 코드 지점에서 광고가 준비되었는지 확인하고 표시합니다.

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  이벤트를 정의하고 코딩합니다.

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 `MyAppId` 속성을 할당합니다. 이 값은 테스트용으로만 사용됩니다. 따라서 앱을 게시하기 전에 라이브 값으로 바꿉니다.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 `MyAdUnitId` 속성을 할당합니다. 이 값은 테스트용으로만 사용됩니다. 따라서 앱을 게시하기 전에 라이브 값으로 바꿉니다.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  앱을 빌드 및 테스트하여 테스트 광고가 표시되는지 확인합니다.

<span id="interstitialadsdirectx10"/>
### 중간 광고(XAML interop과 함께 C++ 및 DirectX 사용)

이 샘플에서는 사용자가 Visual Studio 2015에서 XAML용 범용 앱 프로젝트를 만들었으며 특정 CPU 아키텍처를 대상으로 한다고 가정합니다.

> **중요** 이 코드는 DirectX에 필요하므로 C++으로 작성됩니다.

 
1. Visual Studio에서 프로젝트를 엽니다.
1.  **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

    -   UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

    -   Windows Phone 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows Phone 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 조정자 라이브러리를 프로젝트에 추가하지만 광고 조정자 라이브러리는 무시해도 됩니다.

2.  앱에 대한 해당 헤더 파일에서 중간 광고 개체 및 관련된 속성/메서드를 선언합니다.

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  `AppId` 및 `AdUnitId` 속성을 선언합니다.

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  .cpp 파일에서 네임스페이스 참조를 추가합니다.

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  **InterstitialAd**를 인스턴스화하고, 모든 이벤트 처리기를 연결하고, 광고를 요청합니다.

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  광고를 표시해야 하는 코드 지점에서 광고가 준비되었는지 확인하고 표시합니다.

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  이벤트를 정의하고 코딩합니다.

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 `AppId` 속성을 할당합니다. 이 값은 테스트용으로만 사용됩니다. 따라서 앱을 게시하기 전에 라이브 값으로 바꿉니다.

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 `AdUnitId` 속성을 할당합니다. 이 값은 테스트용으로만 사용됩니다. 따라서 앱을 게시하기 전에 라이브 값으로 바꿉니다.

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. 앱을 빌드 및 테스트하여 테스트 광고가 표시되는지 확인합니다.

### Windows 개발자 센터를 사용하여 라이브 광고와 함께 앱 출시

1.  개발자 센터 대시보드에서 앱의 **수익 창출** &gt; **광고로 수익 창출** 페이지로 이동한 후 [독립 실행형 Microsoft Advertising 단위를 만듭니다](../publish/monetize-with-ads.md). 광고 단위 유형으로 **동영상 중간 광고**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.

2.  코드에서 테스트 광고 단위 값을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

3.  Windows 개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

4.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.

<span id="interstitialbestpractices10"/>
## 중간 광고 모범 사례


중간 광고를 효과적으로 사용하는 방법에 대한 자세한 내용은 [UI 및 사용자 환경 지침](ui-and-user-experience-guidelines.md)을 참조하세요.

<span id="targetplatform10"/>
## 참조 오류 제거: 특정 CPU 플랫폼(XAML 및 HTML) 대상


Microsoft Advertising 라이브러리를 사용할 때는 프로젝트의 **어떤 CPU**도 대상으로 지정할 수 없습니다. 프로젝트가 **임의 CPU** 플랫폼을 대상으로 하는 경우 Microsoft Advertising 라이브러리에 대한 참조를 추가한 후 프로젝트에 경고가 표시될 수 있습니다. 이 경고를 제거하려면 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 자세한 내용은 [알려진 문제](known-issues-for-the-advertising-libraries.md)를 참조하세요.

## 관련 항목


* [C의 중간 광고 샘플 코드#](interstitial-ad-sample-code-in-c.md)
* [JavaScript의 중간 광고 샘플 코드](interstitial-ad-sample-code-in-javascript.md)
* [GitHub의 광고 샘플](http://aka.ms/githubads)

 

 



<!--HONumber=Sep16_HO2-->


