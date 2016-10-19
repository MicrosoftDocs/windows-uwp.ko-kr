---
author: jnHs
Description: "Windows 앱에서 실행되는 앱용 광고 캠페인을 만든 후 다른 채널을 사용하여 앱을 홍보할 수도 있습니다."
title: "사용자 지정 앱 홍보 캠페인 만들기"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
translationtype: Human Translation
ms.sourcegitcommit: 3afdf00864e023d913b635beef0c506735881b23
ms.openlocfilehash: a6e97968df4e9ab986d364b2573a31b4ba9d1958

---

# 사용자 지정 앱 홍보 캠페인 만들기



Windows 앱에서 실행되는 [앱용 광고 캠페인](create-an-ad-campaign-for-your-app.md)을 만든 후 다른 채널을 사용하여 앱을 홍보할 수도 있습니다. 예를 들어 타사 앱 마케팅 공급자를 사용하여 앱을 홍보하거나 소셜 미디어 사이트에 앱의 링크를 게시할 수 있습니다. 이러한 활동을 *사용자 지정 캠페인*이라고 합니다.

앱의 사용자 지정 캠페인을 실행하는 경우 각 URL에 서로 다른 캠페인 ID를 포함하여 각 사용자 지정 캠페인에 대한 Windows 스토어 앱 URL을 만들어 각 캠페인의 상대적인 성과를 추적할 수 있습니다. Windows 10을 실행하는 고객이 캠페인 ID가 포함된 URL을 클릭하면 Microsoft에서 이 클릭을 해당 사용자 지정 캠페인과 연결하고 이 데이터를 제공합니다.

사용자 지정 캠페인과 연결되는 데이터에는 앱의 방문자 수와 *변환*의 두 가지 주요 유형이 있습니다. 변환은 고객이 사용자 지정 캠페인을 통해 홍보된 URL에서 앱에 대한 Windows 스토어 페이지를 클릭한 결과로 이루어진 앱 설치입니다. 변환에 대한 자세한 내용은 이 항목의 [앱 설치가 변환으로 인정되는 방법 이해](#understanding-how-app-installs-qualify-as-conversions)를 참조하세요.

앱에 대한 사용자 지정 캠페인 성과 데이터는 다음과 같은 방식으로 검색할 수 있습니다.

-   앱이 UWP(유니버설 Windows 플랫폼) 앱인 경우 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 메서드를 사용하여 변환으로 이어진 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다.
-   개발자 센터 대시보드의 [채널 및 변환 보고서](channels-and-conversions-report.md)에서 앱 또는 추가 기능에 대한 페이지 보기 및 변환에 대한 데이터를 볼 수 있습니다.

> **중요** 이 데이터는 Windows 10을 실행하는 고객에 대해서만 추적됩니다. 다른 운영 체제를 사용하는 고객도 앱 목록 링크를 따라갈 수 있지만 해당 고객의 활동에 대한 데이터는 포함되지 않습니다.

 

## 예제 사용자 지정 캠페인 시나리오


새 게임 빌드를 완료하고 플레이어에게 기존 게임을 홍보하려는 게임 개발자를 예로 들어 보겠습니다. 이 개발자는 게임의 Windows 스토어 페이지 링크를 포함하여 새 게임 릴리스에 대한 알림을 자신의 Facebook 페이지에 게시합니다. 또한 이 개발자는 많은 플레이어가 Twitter에서 자신을 팔로잉하므로 게임의 Windows 스토어 페이지 링크가 포함된 알림을 트윗합니다.

이러한 각 홍보 채널의 성공을 추적하기 위해 이 개발자는 게임의 Windows 스토어 페이지 URL에 대한 두 가지 변형을 만듭니다.

-   Facebook 페이지에 게시할 URL에는 사용자 지정 캠페인 ID `my-facebook-campaign`을 포함합니다.
-   Twitter에 게시할 URL에는 사용자 지정 캠페인 ID `my-twitter-campaign`을 포함합니다.

Facebook 및 Twitter 팔로워가 URL을 클릭하면 Microsoft에서 각 클릭을 추적하여 해당 사용자 지정 캠페인에 연결합니다. 이후의 게임 다운로드 및 추가 기능 구매는 사용자 지정 캠페인에 연결되고 변환으로 보고됩니다.

## 앱 설치가 변환으로 인정되는 방법 이해


*변환*은 고객이 사용자 지정 캠페인을 통해 홍보된 URL에서 앱에 대한 Windows 스토어 페이지를 클릭한 결과로 이루어진 앱 설치입니다. 개발자 센터 대시보드의 [채널 및 변환 보고서](channels-and-conversions-report.md)에서 변환으로 인정되는 조건과 [프로그래밍 방식으로 캠페인 ID 검색](#programmatically)에서 변환으로 인정되는 조건은 서로 다릅니다.

[채널 및 변환 보고서](channels-and-conversions-report.md)에서 변환으로 인정되려면 다음 조건을 충족해야 합니다.

-   정식 Microsoft 계정을 가진 고객이 사용자 지정 캠페인 ID가 포함된 앱 URL을 클릭하고 앱의 Windows 스토어 페이지로 이동합니다.
-   이 고객(동일한 Microsoft 계정으로 식별)이 사용자 지정 캠페인 ID가 포함된 Windows 스토어 URL을 처음 클릭한 후 24시간 이내에 앱을 설치합니다. 이는 고객이 사용자 지정 캠페인 ID가 포함된 Windows 스토어 URL을 클릭한 것과 다른 컴퓨터 또는 장치에 앱을 설치한 경우에도 변환으로 인정됩니다.
    > **참고** 사용자 지정 캠페인에 대한 변환으로 계산되는 앱 설치의 경우 해당 앱의 모든 추가 기능 구매는 동일한 사용자 지정 캠페인에 대한 변환으로도 계산됩니다.

     

앱과 연결된 캠페인 ID를 프로그래밍 방식으로 검색할 때 변환으로 인정되려면 다음 조건을 충족해야 합니다.

-   정식 Microsoft 계정을 가진 고객이 사용자 지정 캠페인 ID가 포함된 앱 URL을 클릭하고 앱의 Windows 스토어 페이지로 이동합니다.
-   고객은 URL을 클릭 한 후 Windows 스토어 페이지 보기에서 앱을 설치합니다. 사용자가 페이지를 벗어났다가 24시간 이내에 돌아와(다른 컴퓨터나 장치 또는 다른 컴퓨터나 장치) 앱을 설치한 경우 이는 [채널 및 변환 보고서](channels-and-conversions-report.md)에서는 변환으로 인정되지만 캠페인 ID를 프로그래밍 방식으로 검색하는 경우에는 변환으로 인정되지 않습니다.

## 앱의 Windows 스토어 페이지 URL에 사용자 지정 캠페인 ID를 포함


사용자 지정 캠페인 ID를 사용하여 앱의 Windows 스토어 페이지 URL을 만들려면

1.  사용자 지정 캠페인에 대한 ID 문자열을 만듭니다. 이 문자열은 최대 100자를 포함할 수 있지만 식별하기 쉽도록 짧은 캠페인 ID를 정의하는 것이 좋습니다.
2.  앱의 Windows 스토어 페이지 URL을 HTML 또는 프로토콜 형식으로 가져옵니다. HTML 형식의 URL은 개발자 센터 대시보드의 [**앱 ID** 페이지에서 사용할 수 있습니다](link-to-your-app.md).
    -   고객이 브라우저에서 앱의 Windows 스토어 페이지를 탐색할 수 있도록 하려면 HTTP 형식을 사용합니다. 이 URL은 Windows 스토어 앱이 설치된 경우 앱 목록에서 Windows 스토어 앱을 시작합니다. 이 URL 형식은 **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**입니다. 예를 들어 Skype의 HTTP URL은 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`입니다.
        > **참고** HTTP 형식의 URL은 Windows 7 이상을 실행하는 컴퓨터와 태블릿 및 Windows Phone 8 이상을 실행하는 휴대폰의 브라우저에서 Windows 스토어를 탐색하는 데 사용될 수 있습니다.
- Windows 스토어 앱이 설치된 디바이스나 컴퓨터에서 실행되는 다른 Windows 앱에서 앱을 홍보하고 고객이 Windows 스토어 앱에서 앱 페이지를 열기를 원하는 경우 프로토콜 형식을 사용합니다. 이 URL 형식은 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**입니다. 예를 들어 Skype의 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`입니다.
3.  앱의 URL 끝에 다음 문자열을 추가합니다.
    -   HTTP 형식 URL의 경우 **`?cid=*my custom campaign ID*`**를 추가합니다. 예를 들어 Skype에서 값이 **custom\_campaign**인 캠페인 ID를 소개하는 경우 해당 캠페인 ID를 포함하는 새 HTTP URL은 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`입니다.
    -   프로토콜 형식 URL의 경우 **`&cid=*my custom campaign ID*`**를 추가합니다. 예를 들어 Skype에서 값이 **custom\_campaign**인 캠페인 ID를 소개하는 경우 해당 캠페인 ID를 포함하는 새 프로토콜 URL은 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`입니다.

## 프로그래밍 방식으로 앱의 사용자 지정 캠페인 ID 검색


앱이 UWP 앱인 경우 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 메서드를 사용하여 앱과 연결된 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색할 수 있습니다. 이 메서드는 많은 분석 및 수익 창출 시나리오를 가능하게 합니다. 예를 들어 현재 사용자가 Facebook 캠페인을 통해 검색한 후 앱을 다운로드했는지 확인한 다음 그에 따라 앱 환경을 사용자 지정할 수 있습니다. 또는 타사 앱 마케팅 공급자를 사용하는 경우 해당 공급자에게 데이터를 다시 보낼 수 있습니다.

> **참고** [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 메서드는 고객이 캠페인 ID가 포함된 URL을 클릭하고 앱의 Windows 스토어 페이지로 이동한 후 이 페이지를 나가지 않고 앱을 설치한 경우에만 캠페인 ID 문자열을 반환합니다. 사용자가 페이지를 벗어났다가 나중에 돌아와 앱을 설치한 경우에는 **GetAppPurchaseCampaignIdAsync** 사용 시 변환으로 인정되지 않습니다. 자세한 내용은 이 항목의 [변환 이해](#conversions)를 참조하세요.

 

다음 예제에서는 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 메서드를 사용하여 앱의 사용자 지정 캠페인 ID를 검색하는 방법을 보여 줍니다. 앱이 성공적인 변환의 일부로 설치되지 않은 경우 이 메서드는 빈 문자열을 반환합니다.

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

[**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 메서드는 Windows 스토어에서 데이터에 액세스합니다. 이 메서드를 사용하는 경우 다음 지침을 따르세요.

-   호출을 완료하려면 이 메서드 호출을 비동기 작업에 래핑합니다.
-   Windows 스토어에 앱이 아직 게시되지 않은 경우 사용자 지정 캠페인을 테스트하려면 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 클래스 대신 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 클래스의 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 메서드를 사용합니다. 다음 지침을 따르세요.
    -   다음 예제와 같이 **AppPurchaseCampaignId** 요소를 WindowsStoreProxy.xml 파일에 추가합니다. 요소의 값을 사용자 지정 캠페인 ID에 할당합니다. 앱을 실행하는 경우 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 메서드는 항상 이 값을 반환합니다. WindowsStoreProxy.xml 파일에 대한 자세한 내용은 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 설명서를 참조하세요.

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```
    
    -   [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)과 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 간에 쉽게 전환하려면 다음 문을 코드에 추가하여 **Store** 별칭을 정의한 다음 **Store** 별칭으로 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 호출을 수행하는 것이 좋습니다.

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## 사용자 지정 캠페인 테스트


사용자 지정 캠페인 URL을 홍보하기 전에 다음을 수행하여 사용자 지정 캠페인을 테스트하는 것이 좋습니다.

1.  테스트에 사용할 컴퓨터 또는 장치에서 Microsoft 계정에 로그인합니다.
2.  사용자 지정 캠페인 URL을 클릭합니다. Windows 스토어에서 앱 페이지를 올바르게 로드하는지 확인한 다음 Windows 스토어 앱 또는 브라우저 페이지를 닫습니다.
3.  URL을 여러 번 클릭하고 앱 페이지를 방문할 때마다 Windows 스토어 앱 또는 브라우저 페이지를 닫습니다. 여러 번의 앱 페이지 방문 중에서 앱을 설치하여 변환을 생성합니다. URL을 클릭한 총 횟수를 계산합니다.
4.  앱에서 사용자 지정 캠페인 ID를 프로그래밍 방식으로 검색하는 경우에는 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 클래스 대신 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 클래스의 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 메서드를 사용하여 이 작업을 테스트합니다.

 

 







<!--HONumber=Aug16_HO3-->


