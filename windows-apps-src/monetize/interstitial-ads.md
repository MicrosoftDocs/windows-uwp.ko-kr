---
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Microsoft Advertising SDK를 사용 하 여 Windows 10 용 UWP 앱에 중간 광고를 포함 하는 방법을 알아봅니다.
title: 중간 광고
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, ad 컨트롤, 중간
ms.localizationpriority: medium
ms.openlocfilehash: eedb3f12bfc9da51afb2d5205122cbd42d7b31bf
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363096"
---
# <a name="interstitial-ads"></a>중간 광고

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 연습에서는 Windows 10 용 UWP (유니버설 Windows 플랫폼) 앱 및 게임에 중간 광고를 포함 하는 방법을 보여 줍니다. C # 및 c + +를 사용 하 여 JavaScript/HTML 앱 및 XAML 앱에 중간 광고를 추가 하는 방법을 보여 주는 전체 샘플 프로젝트는 [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)을 참조 하세요.

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>중간 광고 란?

앱 또는 게임의 UI 부분으로 제한 되는 표준 배너 광고와 달리 중간 광고는 전체 화면에 표시 됩니다. 게임에서 주로 사용 되는 두 가지 기본 폼입니다.

* *Paywall* ads를 사용 하는 경우 사용자는 일정 한 간격으로 광고를 감시 해야 합니다. 예를 들어 게임 수준 간의 예는 다음과 같습니다.

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* *보상 기반* 광고를 사용 하면 사용자는 힌트 또는 수준을 완료 하기 위한 추가 시간과 같은 몇 가지 혜택을 명시적으로 찾고 앱의 사용자 인터페이스를 통해 광고를 초기화 합니다.

앱과 게임에서 사용할 수 있는 중간 광고에는 **중간 비디오 광고** 및 **중간 배너 광고**라는 두 가지 유형의 중간 광고를 제공 합니다.

> [!NOTE]
> 중간 광고 용 API는 비디오 재생 시를 제외 하 고 사용자 인터페이스를 처리 하지 않습니다. 앱에서 중간 광고를 통합 하는 방법을 고려 하 여 수행할 작업에 대 한 지침과 사용 하지 않도록 설정 하는 [중간 모범 사례](ui-and-user-experience-guidelines.md#interstitialbestpractices10) 를 참조 하세요.

## <a name="prerequisites"></a>사전 요구 사항

* Visual studio 2015 이상 버전의 Visual Studio를 사용 하 여 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 를 설치 합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조 하세요.

## <a name="integrate-an-interstitial-ad-into-your-app"></a>중간 광고를 앱에 통합

앱에서 중간 광고를 표시 하려면 프로젝트 형식에 대 한 지침을 따르세요.

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C + + (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

이 단원에서는 c # 예제를 제공 하지만 Visual Basic 및 c + +는 XAML/.NET 프로젝트에 대해서도 지원 됩니다. 전체 c # 코드 예제는 [c #의 중간 광고 샘플 코드](interstitial-ad-sample-code-in-c.md)를 참조 하세요.

1. Visual Studio에서 프로젝트를 엽니다.
    > [!NOTE]
    > 기존 프로젝트를 사용 하는 경우 프로젝트에서 appxmanifest.xml 파일을 열고 **인터넷 (클라이언트)** 기능이 선택 되어 있는지 확인 합니다. 앱에서 테스트 광고와 라이브 광고를 수신 하려면이 기능이 필요 합니다.

2. 프로젝트가 **모든 CPU**를 대상으로 하는 경우 프로젝트를 업데이트 하 여 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 합니다. 프로젝트가 **모든 CPU**를 대상으로 하는 경우에는 다음 단계에서 Microsoft 광고 라이브러리에 대 한 참조를 성공적으로 추가할 수 없습니다. 자세한 내용은 [프로젝트의 모든 CPU를 대상으로 하 여 발생 한 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조 하세요.

3. 프로젝트에서 Microsoft Advertising SDK에 대 한 참조를 추가 합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 후 **Microsoft Advertising SDK for XAML** (버전 10.0) 옆의 확인란을 선택 합니다.
    3.  **참조 관리자**에서 확인을 클릭 합니다.

3.  앱의 적절 한 코드 파일 (예: MainPage.xaml.cs 또는 다른 페이지의 코드 파일)에서 다음 네임 스페이스 참조를 추가 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet1":::

4.  앱의 적절 한 위치 (예: ```MainPage``` 또는 다른 페이지)에서 [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 개체와 중간 광고의 응용 프로그램 id 및 ad 단위 id를 나타내는 여러 문자열 필드를 선언 합니다. 다음 코드 예제에서는 `myAppId` 및 필드를 `myAdUnitId` 중간 광고에 대 한 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 에 할당 합니다.

    > [!NOTE]
    > 모든 **Interstalad** 에는 서비스에서 컨트롤에 대 한 광고를 제공 하는 데 사용 하는 해당 하는 *ad 단위가* 있으며 모든 ad 단위는 *ad 단위 id* 와 *응용 프로그램 id*로 구성 됩니다. 이러한 단계에서는 테스트 ad 단위 ID 및 응용 프로그램 ID 값을 컨트롤에 할당 합니다. 이러한 테스트 값은 응용 프로그램의 테스트 버전 에서만 사용할 수 있습니다. 스토어에 앱을 게시 하기 전에 이러한 테스트 값을 파트너 센터의 [라이브 값으로 바꾸어야](#release) 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet2":::

5.  시작 시 실행 되는 코드 (예: 페이지에 대 한 생성자)에서 **Interstitialad** 개체를 인스턴스화하고 개체의 이벤트에 대 한 이벤트 처리기를 연결 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet3":::

6.  *중간 비디오* 광고를 표시 하려면: ad가 필요 하기 전까지 약 30-60 초 전에 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드를 사용 하 여 ad를 미리 인출 합니다. 이렇게 하면 표시 되기 전에 광고를 요청 하 고 준비 하는 데 충분 한 시간이 소요 됩니다. Ad 형식에 대해 **Adtype. Video** 를 지정 해야 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet4":::

    *중간 배너* 광고를 표시 하려면: ad가 필요 하기 전까지 약 5-8 초 전에 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드를 사용 하 여 ad를 미리 인출 합니다. 이렇게 하면 표시 되기 전에 광고를 요청 하 고 준비 하는 데 충분 한 시간이 소요 됩니다. Ad 형식에 대해 **Adtype. Display** 를 지정 해야 합니다.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  중간 비디오 또는 중간 배너 광고를 표시 하려는 코드의 시점에서 **Interstitialad** 가 표시 될 준비가 되었는지 확인 한 다음 [show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 메서드를 사용 하 여 표시 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet5":::

7.  **Interstitialad** 개체에 대 한 이벤트 처리기를 정의 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet6":::

8.  응용 프로그램을 빌드 및 테스트 하 여 테스트 광고가 표시 되는지 확인 합니다.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

다음 지침에서는 Visual Studio에서 JavaScript 용 유니버설 Windows 프로젝트를 만들었으며 특정 CPU를 대상으로 한다고 가정 합니다. 전체 코드 샘플은 [JavaScript의 중간 광고 샘플 코드](interstitial-ad-sample-code-in-javascript.md)를 참조 하세요.

1. Visual Studio에서 프로젝트를 엽니다.

2. 프로젝트가 **모든 CPU**를 대상으로 하는 경우 프로젝트를 업데이트 하 여 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 합니다. 프로젝트가 **모든 CPU**를 대상으로 하는 경우에는 다음 단계에서 Microsoft 광고 라이브러리에 대 한 참조를 성공적으로 추가할 수 없습니다. 자세한 내용은 [프로젝트의 모든 CPU를 대상으로 하 여 발생 한 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조 하세요.

3. 프로젝트에서 Microsoft Advertising SDK에 대 한 참조를 추가 합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 후 **Microsoft Advertising SDK for JavaScript** (버전 10.0) 옆의 확인란을 선택 합니다.
    3.  **참조 관리자**에서 확인을 클릭 합니다.

3.  프로젝트의 HTML 파일의 ** &lt; head &gt; ** 섹션에서 프로젝트의 JavaScript 참조 인 기본 .css 및 default.js 후 ad.js에 대 한 참조를 추가 합니다.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  프로젝트의 .js 파일에서 [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 개체와 중간 광고에 대 한 응용 프로그램 id 및 AD 단위 id를 포함 하는 여러 필드를 선언 합니다. 다음 코드 예제에서는 `applicationId` 및 필드를 `adUnitId` 중간 광고에 대 한 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 에 할당 합니다.

    > [!NOTE]
    > 모든 **Interstalad** 에는 서비스에서 컨트롤에 대 한 광고를 제공 하는 데 사용 하는 해당 하는 *ad 단위가* 있으며 모든 ad 단위는 *ad 단위 id* 와 *응용 프로그램 id*로 구성 됩니다. 이러한 단계에서는 테스트 ad 단위 ID 및 응용 프로그램 ID 값을 컨트롤에 할당 합니다. 이러한 테스트 값은 응용 프로그램의 테스트 버전 에서만 사용할 수 있습니다. 스토어에 앱을 게시 하기 전에 이러한 테스트 값을 파트너 센터의 [라이브 값으로 바꾸어야](#release) 합니다.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet1":::

5.  시작 시 실행 되는 코드 (예: 페이지에 대 한 생성자)에서 **Interstitialad** 개체를 인스턴스화하고 개체에 대 한 이벤트 처리기를 연결 합니다.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet2":::

5. *중간 비디오* 광고를 표시 하려면: ad가 필요 하기 전까지 약 30-60 초 전에 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드를 사용 하 여 ad를 미리 인출 합니다. 이렇게 하면 표시 되기 전에 광고를 요청 하 고 준비 하는 데 충분 한 시간이 소요 됩니다. Ad 형식에 대해 **Interstitialadtype. video** 를 지정 해야 합니다.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet3":::

    *중간 배너* 광고를 표시 하려면: ad가 필요 하기 전까지 약 5-8 초 전에 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드를 사용 하 여 ad를 미리 인출 합니다. 이렇게 하면 표시 되기 전에 광고를 요청 하 고 준비 하는 데 충분 한 시간이 소요 됩니다. Ad 형식에 대해 **Interstitialadtype** 을 지정 해야 합니다.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  광고를 표시 하려는 코드의 지점에서 **Interstitialad** 가 표시 될 준비가 되었는지 확인 한 다음 [show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 메서드를 사용 하 여 표시 합니다.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet4":::

7.  **Interstitialad** 개체에 대 한 이벤트 처리기를 정의 합니다.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet5":::

9.  응용 프로그램을 빌드 및 테스트 하 여 테스트 광고가 표시 되는지 확인 합니다.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C + + (DirectX Interop)

이 샘플은 Visual Studio에서 c + + **DirectX 및 XAML 앱 (유니버설 Windows)** 프로젝트를 만들었으며 특정 CPU 아키텍처를 대상으로 한다고 가정 합니다.
 
1. Visual Studio에서 프로젝트를 엽니다.

3. 프로젝트에서 Microsoft Advertising SDK에 대 한 참조를 추가 합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 후 **Microsoft Advertising SDK for XAML** (버전 10.0) 옆의 확인란을 선택 합니다.
    3.  **참조 관리자**에서 확인을 클릭 합니다.

2.  응용 프로그램의 적절 한 헤더 파일 (예: DirectXPage)에서 [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 개체 및 관련 이벤트 처리기 메서드를 선언 합니다.  

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet1":::

3.  동일한 헤더 파일에서 중간 광고의 응용 프로그램 ID 및 ad 단위 ID를 나타내는 여러 문자열 필드를 선언 합니다. 다음 코드 예제에서는 `myAppId` 및 필드를 `myAdUnitId` 중간 광고에 대 한 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 에 할당 합니다.

    > [!NOTE]
    > 모든 **Interstalad** 에는 서비스에서 컨트롤에 대 한 광고를 제공 하는 데 사용 하는 해당 하는 *ad 단위가* 있으며 모든 ad 단위는 *ad 단위 id* 와 *응용 프로그램 id*로 구성 됩니다. 이러한 단계에서는 테스트 ad 단위 ID 및 응용 프로그램 ID 값을 컨트롤에 할당 합니다. 이러한 테스트 값은 응용 프로그램의 테스트 버전 에서만 사용할 수 있습니다. 스토어에 앱을 게시 하기 전에 이러한 테스트 값을 파트너 센터의 [라이브 값으로 바꾸어야](#release) 합니다.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet2":::

4.  중간 광고를 표시 하는 코드를 추가 하려는 .cpp 파일에서 다음 네임 스페이스 참조를 추가 합니다. 다음 예제에서는 응용 프로그램의 DirectXPage 파일에 코드를 추가 한다고 가정 합니다.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet3":::

6.  시작 시 실행 되는 코드 (예: 페이지에 대 한 생성자)에서 **Interstitialad** 개체를 인스턴스화하고 개체의 이벤트에 대 한 이벤트 처리기를 연결 합니다. 다음 예제에서 ```InterstitialAdSamplesCpp``` 는 프로젝트에 대 한 네임 스페이스입니다. 코드에 필요에 따라이 이름을 변경 합니다.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet4":::

7. 중간 *비디오* 광고를 표시 하려는 경우: 중간 광고를 요구 하기 전까지 약 30-60 초 전에 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드를 사용 하 여 ad를 미리 인출 합니다. 이렇게 하면 표시 되기 전에 광고를 요청 하 고 준비 하는 데 충분 한 시간이 소요 됩니다. Ad 형식에 대해 **Adtype:: Video** 를 지정 해야 합니다.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet5":::

    *중간 배너* 광고를 표시 하려면: ad가 필요 하기 전까지 약 5-8 초 전에 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드를 사용 하 여 ad를 미리 인출 합니다. 이렇게 하면 표시 되기 전에 광고를 요청 하 고 준비 하는 데 충분 한 시간이 소요 됩니다. Ad 형식에 대해 **Adtype::D isplay** 를 지정 해야 합니다.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  광고를 표시 하려는 코드의 지점에서 **Interstitialad** 가 표시 될 준비가 되었는지 확인 한 다음 [show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 메서드를 사용 하 여 표시 합니다.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet6":::

8.  **Interstitialad** 개체에 대 한 이벤트 처리기를 정의 합니다.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet7":::

9. 응용 프로그램을 빌드 및 테스트 하 여 테스트 광고가 표시 되는지 확인 합니다.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>라이브 광고를 사용 하 여 앱 릴리스

1. 앱에서 중간 광고 사용이 [중간 광고에 대 한 지침](ui-and-user-experience-guidelines.md#interstitialbestpractices10)을 따라야 합니다.

2.  파트너 센터에서 [앱 내 광고](../publish/in-app-ads.md) 페이지로 이동 하 여 [ad 단위를 만듭니다](set-up-ad-units-in-your-app.md#live-ad-units). 광고 단위 유형에 대해 표시 하는 중간 광고의 유형에 따라 **비디오 중간** 또는 **배너 중간**을 선택 합니다. Ad 단위 ID와 응용 프로그램 ID를 모두 기록해 둡니다.
    > [!NOTE]
    > 테스트 ad 단위와 라이브 UWP ad 단위의 응용 프로그램 ID 값은 서로 다른 형식입니다. 테스트 응용 프로그램 ID 값은 Guid입니다. 파트너 센터에서 라이브 UWP ad 단위를 만들 때 ad 단위의 응용 프로그램 ID 값은 항상 앱에 대 한 저장소 ID와 일치 합니다. 예를 들어 매장 ID 값은 9NBLGGH4R315와 같습니다.

3. 필요 [에 따라 앱 내 광고](../publish/in-app-ads.md) 페이지의 [중재 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성 하 여 **interstitialad** 에 대 한 ad 중재를 사용 하도록 설정할 수 있습니다. Ad 중재를 통해 여러 ad 네트워크에서 광고를 표시 하 여 ad 수익과 앱 승격 기능을 최대화할 수 있습니다. 여기에는 Taboola 및 Smaato와 같은 다른 유료 ad 네트워크의 광고를 비롯 하 여 Microsoft 앱 판촉 캠페인에 대 한 광고 등이 있습니다.

4.  코드에서 테스트 ad 단위 값을 파트너 센터에서 생성 한 라이브 값으로 바꿉니다.

5.  파트너 센터를 사용 하 여 스토어에 [앱을 제출](../publish/app-submissions.md) 합니다.

6.  파트너 센터에서 [광고 성능 보고서](../publish/advertising-performance-report.md) 를 검토 합니다.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>앱에서 여러 중간 광고 컨트롤의 ad 단위 관리

단일 앱에서 여러 **Interstitialad** 컨트롤을 사용할 수 있습니다. 이 시나리오에서는 각 컨트롤에 다른 ad 단위를 할당 하는 것이 좋습니다. 각 컨트롤에 대해 서로 다른 ad 단위를 사용 하면 [중재 설정을](../publish/in-app-ads.md#mediation) 개별적으로 구성 하 고 각 컨트롤에 대 한 개별 [보고 데이터](../publish/advertising-performance-report.md) 를 가져올 수 있습니다. 이를 통해 서비스는 앱에 제공 하는 광고를 더 효율적으로 최적화할 수 있습니다.

> [!IMPORTANT]
> 하나의 앱 에서만 각 ad 단위를 사용할 수 있습니다. 둘 이상의 앱에서 ad 단위를 사용 하는 경우 해당 ad 단위에 대 한 광고가 제공 되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [중간 광고에 대 한 지침](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [C#의 중간 광고 샘플 코드](interstitial-ad-sample-code-in-c.md)
* [JavaScript의 중간 광고 샘플 코드](interstitial-ad-sample-code-in-javascript.md)
* [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [앱에 대 한 ad 단위 설정](set-up-ad-units-in-your-app.md)
