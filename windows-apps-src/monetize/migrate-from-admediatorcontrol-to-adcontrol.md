---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: "UWP 앱에서 AdMediatorControl을 AdControl로 마이그레이션하는 방법에 대해 알아봅니다."
title: "UWP 앱을 위해 AdMediatorControl을 AdControl로 마이그레이션"
translationtype: Human Translation
ms.sourcegitcommit: 07baa54990ec31dc0cb9c289f9f0222754da9d7c
ms.openlocfilehash: 3abef943530cc756de117edccc5ab16e5f178604

---

# UWP 앱을 위해 AdMediatorControl을 AdControl로 마이그레이션

이전 Microsoft Advertising SDK 릴리스에서는 UWP(유니버설 Windows 플랫폼) 앱에서 **AdMediatorControl** 클래스를 사용하여 배너 광고를 표시할 수 있었습니다. 따라서 개발자가 AdDuplex뿐만 아니라 파트너 네트워크(AOL 및 AppNexus)의 배너 광고를 표시하여 광고 수익을 최적화했습니다. [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)는 더 이상 **AdMediatorControl** 클래스를 지원하지 않습니다. 이전 SDK의 **AdMediatorControl** 클래스를 사용하는 기존 앱을 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)를 사용하는 UWP 앱으로 마이그레이션하려는 경우 이 문서의 지침에 따라 **AdMediatorControl** 클래스 대신 **AdControl** 클래스를 사용하도록 코드를 업데이트합니다. 필요에 따라 AdDuplex를 사용하여 가중치 또는 순위 접근 방식으로 광고를 조정하도록 앱을 구성할 수 있습니다.

>**참고**&nbsp;&nbsp;이 문서의 코드 예제는 설명 목적으로만 제공됩니다. 앱에서 작동하도록 코드 예제를 조정해야 할 수 있습니다.

## 필수 조건

* 현재 AdMediatorControl을 사용하고 Windows 스토어에 게시하는 UWP 앱
* Visual Studio 2015와 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)가 설치된 개발 컴퓨터
* AdDuplex를 사용하여 광고를 조정하려면 개발 컴퓨터에 [AdDuplex Windows 10 SDK](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077)가 설치되어 있어야 함

  >**참고**&nbsp;&nbsp;위의 링크에서 AdDuplex SDK 설치 프로그램을 실행하는 대신 Visual Studio 2015에 있는 UWP 앱 프로젝트용 AdDuplex 라이브러리를 설치할 수 있습니다. Visual Studio 2015에서 UWP 앱 프로젝트를 열고 **프로젝트** > **NuGet 패키지 관리**를 클릭하고 **AdDuplexWin10**이라는 NuGet 패키지를 검색하여 패키지를 설치합니다.

## 1단계: 응용 프로그램 ID 및 광고 단위 ID 검색

**AdControl** 클래스를 사용하도록 코드를 마이그레이션할 때 응용 프로그램 ID와 광고 단위 ID를 알아야 합니다. 최신 ID를 가져오려면 조정 구성 파일에서 검색하는 것이 가장 좋습니다.

1. Windows 개발자 센터 대시보드에 로그인하여 현재 **AdMediatorControl**을 사용 중인 앱을 클릭합니다.
2. **수익 창출**을 클릭하고 **광고로 수익 창출**을 클릭합니다.
3. **Windows 광고 조정** 섹션에서 **조정 구성 다운로드** 링크를 클릭하고 메모장 등의 텍스트 편집기에서 AdMediator.config 파일을 엽니다.
4. 파일에서 ```<AdAdapterInfo>``` 요소와 자식 요소 ```<Name>MicrosoftAdvertising</Name>```을 찾습니다. 이 섹션에는 Microsoft 유료 광고 구성이 포함되어 있습니다.
5. 이 ```<AdAdapterInfo>``` 요소에서 값이 **WApplicationId** 및 **WAdUnitId**인 ```<Key>``` 요소가 포함된 ```<Property>``` 요소를 찾습니다. 아래 예제에서 ```<Value>``` 요소의 값은 예를 든 것입니다.

  ```xml
  <Metadata>
      <Property>
          <Key>WApplicationId</Key>
          <Value>364d4938-c0f5-4c3d-8aae-090206211dc9</Value>
      </Property>
      <Property>
          <Key>WAdUnitId</Key>
          <Value>301568</Value>
      </Property>
  </Metadata>
  ```
6. 나중에 사용할 수 있도록 이러한 ```<Value>``` 요소의 값을 모두 복사합니다. 이러한 값에는 Microsoft 유료 광고의 비모바일 광고 단위를 위한 응용 프로그램 ID와 광고 단위 ID가 포함되어 있습니다.
5. 동일한 ```<AdAdapterInfo>``` 요소에서 값이 **MApplicationId** 및 **MAdUnitId**인 ```<Key>``` 요소가 포함된 ```<Property>``` 요소를 찾습니다. 아래 예제에서 ```<Value>``` 요소의 값은 예를 든 것입니다.

  ```xml
  <Metadata>
      <Property>
          <Key>MApplicationId</Key>
          <Value>e2cf8388-7018-4a11-8ab0de90f2a7a401</Value>
      </Property>
      <Property>
          <Key>MAdUnitId</Key>
          <Value>301056</Value>
      </Property>
  </Metadata>
  ```
6. 나중에 사용할 수 있도록 ```<Value>``` 요소의 값을 모두 복사합니다. 이러한 값에는 Microsoft 유료 광고의 모바일 광고 단위를 위한 응용 프로그램 ID와 광고 단위 ID가 포함되어 있습니다.
7. [하우스 광고](../publish/about-house-ads.md)를 사용하는 경우 자식 요소가 ```<Name>MicrosoftAdvertisingHouse</Name>```인 ```<AdAdapterInfo>``` 요소를 찾습니다. 이 요소에서 값이 **MAdUnitId** 및 **WAdUnitId**인 ```<Key>``` 요소를 찾고 나중에 사용할 수 있도록 해당 ```<Value>``` 요소의 값을 저장합니다. 이 값은 각각 Microsoft 하우스 광고에 대한 모바일 및 비모바일 광고 단위 ID입니다.
8. AdDuplex를 사용하는 경우 자식 요소가 ```<Name>AdDuplex</Name>```인 ```<AdAdapterInfo>``` 요소를 찾습니다. 이 요소에서 값이 **AppKey** 및 **AdUnitId**인 ```<Key>``` 요소를 찾고 나중에 사용할 수 있도록 해당 ```<Value>``` 요소의 값을 저장합니다. 이 값은 각각 AdDuplex 앱 키 및 광고 단위 ID입니다.

## 2단계: 앱 코드 업데이트

응용 프로그램 ID와 광고 단위 ID를 찾았으면 **AdMediatorControl** 대신 **AdControl**을 사용하도록 앱의 코드를 업데이트합니다.

### Microsoft 유료 광고만

광고 조정 구성에 Microsoft 유료 광고만 사용하는 경우 다음 단계를 따릅니다.

  >**참고**&nbsp;&nbsp;이러한 단계는 광고를 표시하려는 앱 페이지에 **myAdGrid**라는 빈 그리드가 포함되어 있다고 가정합니다(예: ```<Grid x:Name="myAdGrid"/>```). 이 단계에서는 XAML 대신 전적으로 코드를 사용하여 광고 컨트롤을 만들고 구성합니다.

1. Visual Studio에서 UWP 앱 프로젝트를 엽니다.
2.  **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...**를 선택합니다.
**참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.
3. **참조 관리자**에서 확인을 클릭합니다.
4. XAML에서 **AdMediatorControl** 선언을 제거하고 관련 이벤트 처리기를 포함하여 이 **AdMediatorControl** 개체를 사용하는 모든 코드를 제거합니다.
5. 광고를 표시하려는 앱 **Page**에 대한 코드 파일을 엽니다.
6. 다음 문을 코드 파일의 맨 위에 추가합니다(없는 경우).

  ```csharp
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. 다음 상수 선언을 **Page** 클래스에 추가합니다.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID = "<tbd>";
  ```
7. 다음 각 상수 선언에 대해 ```<tbd>``` 값을 바꿉니다.
  * **AD_WIDTH** 및 **AD_HEIGHT**: [배너 광고에 지원되는 광고 크기]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads) 중 하나에 할당합니다.
  * **WAPPLICATIONID** 및 **WADUNITID**: 조정 구성 파일에서 이전에 검색한 Microsoft 유료 광고에 대한 **WApplicationId** 및 **WAdUnitId** 값에 할당합니다. 이러한 값은 유료 광고의 비모바일 광고 단위에 사용됩니다.
  * **MAPPLICATIONID** 및 **MADUNITID**: 조정 구성 파일에서 이전에 검색한 Microsoft 유료 광고에 대한 **MApplicationId** 및 **MAdUnitId** 값에 할당합니다. 이러한 값은 유료 광고의 모바일 광고 단위에 사용됩니다.

8. 다음 변수 선언을 **Page** 클래스에 추가합니다.

  ```csharp
  // Declare an AdControl.
  private AdControl myAdControl = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myAppId = WAPPLICATIONID;
  private string myAdUnitId = WADUNITID;
  ```
5. **InitializeComponent()** 메서드를 호출한 다음 **Page** 클래스 생성자에 다음 코드를 추가합니다.

  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myAppId = MAPPLICATIONID;
      myAdUnitId = MADUNITID;
  }

  // Initialize the AdControl.
  myAdControl = new AdControl();
  myAdControl.ApplicationId = myAppId;
  myAdControl.AdUnitId = myAdUnitId;
  myAdControl.Width = AD_WIDTH;
  myAdControl.Height = AD_HEIGHT;
  myAdControl.IsAutoRefreshEnabled = true;

  myAdGrid.Children.Add(myAdControl);
  ```  

### Microsoft 유료 광고, 하우스 광고 및 AdDuplex

Microsoft 유료 광고뿐만 아니라 Microsoft 하우스 광고 또는 AdDuplex를 사용하고 있으며 AdDuplex를 사용하여 광고를 계속 조정하려는 경우 이 섹션의 단계를 수행합니다. 이 코드 예제에서는 AdDuplex 및 Microsoft 하우스 광고를 모두 지원합니다. AdDuplex와 Microsoft 하우스 광고 중 하나만 사용하는 경우 해당 시나리오에 적용되지 않는 코드를 제거합니다.

  >**참고**&nbsp;&nbsp;이러한 단계는 광고를 표시하려는 앱 페이지에 **myAdGrid**라는 빈 그리드가 포함되어 있다고 가정합니다(예: ```<Grid x:Name="myAdGrid"/>```). 이 단계에서는 XAML 대신 전적으로 코드를 사용하여 광고 컨트롤을 만들고 구성합니다.

1. Visual Studio에서 UWP 앱 프로젝트를 엽니다.
2.  **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...**를 선택합니다.
**참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.
3. **참조 관리자**에서 확인을 클릭합니다.
4. XAML에서 **AdMediatorControl** 선언을 제거하고 관련 이벤트 처리기를 포함하여 이 **AdMediatorControl** 개체를 사용하는 모든 코드를 제거합니다.
5. 광고를 표시하려는 앱 **Page**에 대한 코드 파일을 엽니다.
6. 다음 문을 코드 파일의 맨 위에 추가합니다(없는 경우).

  ```csharp
  using Windows.System.UserProfile;
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. 다음 상수 선언을 **Page** 클래스에 추가합니다.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const int HOUSE_AD_WEIGHT = <tbd>;
  private const int AD_REFRESH_SECONDS = 35;
  private const int MAX_ERRORS_PER_REFRESH = 3;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID_PAID = "<tbd>";
  private const string WADUNITID_HOUSE = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID_PAID = "<tbd>";
  private const string MADUNITID_HOUSE = "<tbd>";
  private const string ADDUPLEX_APPKEY = "<tbd>";
  private const string ADDUPLEX_ADUNIT = "<tbd>";
  ```
4. ```<tbd>```에 할당된 각 상수 선언에 대해 ```<tbd>```를 해당 시나리오의 실제 값으로 바꿉니다.
  * **AD_WIDTH** 및 **AD_HEIGHT**: [배너 광고에 지원되는 광고 크기]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads) 중 하나에 할당합니다.
  * **HOUSE_AD_WEIGHT**: Microsoft 유료 광고와 비교하여 Microsoft 하우스 광고에 적용할 가중치 값을 지정하는 정수(0~100)에 할당합니다. 여기서 0은 하우스 광고를 전혀 표시하지 않는 것이고 100은 항상 표시하는 것입니다.
  * **WAPPLICATIONID** 및 **WADUNITID_PAID**: 조정 구성 파일에서 이전에 검색한 Microsoft 유료 광고에 대한 **WApplicationId** 및 **WAdUnitId** 값에 할당합니다. 이러한 값은 유료 광고의 비모바일 광고 단위에 사용됩니다.
  * **WADUNITID_HOUSE**: 조정 구성 파일에서 이전에 검색한 하우스 광고에 대한 **WAdUnitId**에 할당합니다. 이 값은 하우스 광고의 비모바일 광고 단위에 사용됩니다.
  * **MAPPLICATIONID** 및 **MADUNITID_PAID**: 조정 구성 파일에서 검색한 Microsoft 유료 광고에 대한 **MApplicationId** 및 **MAdUnitId** 값에 할당합니다. 이러한 값은 유료 광고의 모바일 광고 단위에 사용됩니다.
  * **MADUNITID_HOUSE**: 조정 구성 파일에서 이전에 검색한 하우스 광고에 대한 **MAdUnitId**에 할당합니다. 이 값은 하우스 광고의 모바일 광고 단위에 사용됩니다.
  * **ADDUPLEX_APPKEY** 및 **ADDUPLEX_ADUNIT**: 조정 구성 파일에서 이전에 검색한 AdDuplex 앱 키 및 광고 단위 ID 값에 할당합니다.
5. 다음 변수 선언을 **Page** 클래스에 추가합니다.
  ```csharp
  // Dispatch timer to fire at each ad refresh interval.
  private DispatcherTimer myAdRefreshTimer = new DispatcherTimer();

  // Global variables used for mediation decisions.
  private Random randomGenerator = new Random();
  private int errorCountCurrentRefresh = 0;  // Prevents infinite redirects.
  private int adDuplexWeight = 0;            // Will be set by GetAdDuplexWeight().

  // Microsoft and AdDuplex controls for banner ads.
  private AdControl myMicrosoftBanner = null;
  private AdDuplex.AdControl myAdDuplexBanner = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myMicrosoftAppId = WAPPLICATIONID;
  private string myMicrosoftPaidUnitId = WADUNITID_PAID;
  private string myMicrosoftHouseUnitId = WADUNITID_HOUSE;
  ```
5. **InitializeComponent()** 메서드를 호출한 다음 **Page** 클래스 생성자에 다음 코드를 추가합니다.
  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;
  adDuplexWeight = GetAdDuplexWeight();
  RefreshBanner();

  // Start the timer to refresh the banner at the desired interval.
  myAdRefreshTimer.Interval = new TimeSpan(0, 0, AD_REFRESH_SECONDS);
  myAdRefreshTimer.Tick += myAdRefreshTimer_Tick;
  myAdRefreshTimer.Start();

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myMicrosoftAppId = MAPPLICATIONID;
      myMicrosoftPaidUnitId = MADUNITID_PAID;
      myMicrosoftHouseUnitId = MADUNITID_HOUSE;
  }
  ```
6. 마지막으로 다음 메서드를 **Page** 클래스에 추가합니다. 이러한 메서드는 Microsoft **AdControl** 및 AdDuplex **AdControl** 개체를 인스턴스화하고 지정된 가중치 값과 함께 임의 개수의 생성기를 사용하여 이러한 컨트롤의 배너 광고를 정기적으로 새로 고칩니다.
  ```csharp
  private int GetAdDuplexWeight()
  {
      // TODO: Change this logic to fit your needs.
      // This example uses Microsoft ads first in Canada and Mexico, then
      // AdDuplex as fallback. In France, AdDuplex is first. In other regions,
      // this example uses a weighted average approach, with 50% to AdDuplex.

      int returnValue = 0;
      switch (GlobalizationPreferences.HomeGeographicRegion)
      {
          case "CA":
          case "MX":
              returnValue = 0;
              break;
          case "FR":
              returnValue = 100;
              break;
          default:
              returnValue = 50;
              break;
      }
      return returnValue;
  }

  private void ActivateMicrosoftBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Use random number generator and house ads weight to determine whether
      // to use paid ads or house ads. Paid is the default. You could alternatively
      // write a method similar to GetAdDuplexWeight and override by region.
      string myAdUnit = myMicrosoftPaidUnitId;
      int houseWeight = HOUSE_AD_WEIGHT;
      int randomInt = randomGenerator.Next(0, 100);
      if (randomInt < houseWeight)
      {
          myAdUnit = myMicrosoftHouseUnitId;
      }

      // Hide the AdDuplex control if it is showing.
      if (null != myAdDuplexBanner)
      {
          myAdDuplexBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the Microsoft control.
      if (null == myMicrosoftBanner)
      {
          myMicrosoftBanner = new AdControl();
          myMicrosoftBanner.ApplicationId = myMicrosoftAppId;
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Width = AD_WIDTH;
          myMicrosoftBanner.Height = AD_HEIGHT;
          myMicrosoftBanner.IsAutoRefreshEnabled = false;

          myMicrosoftBanner.AdRefreshed += myMicrosoftBanner_AdRefreshed;
          myMicrosoftBanner.ErrorOccurred += myMicrosoftBanner_ErrorOccurred;

          myAdGrid.Children.Add(myMicrosoftBanner);
      }
      else
      {
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Visibility = Visibility.Visible;
          myMicrosoftBanner.Refresh();
      }
  }

  private void ActivateAdDuplexBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Hide the Microsoft control if it is showing.
      if (null != myMicrosoftBanner)
      {
          myMicrosoftBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the AdDuplex control.
      if (null == myAdDuplexBanner)
      {
          myAdDuplexBanner = new AdDuplex.AdControl();
          myAdDuplexBanner.AppKey = ADDUPLEX_APPKEY;
          myAdDuplexBanner.AdUnitId = ADDUPLEX_ADUNIT;
          myAdDuplexBanner.Width = AD_WIDTH;
          myAdDuplexBanner.Height = AD_HEIGHT;
          myAdDuplexBanner.RefreshInterval = AD_REFRESH_SECONDS;

          myAdDuplexBanner.AdLoaded += myAdDuplexBanner_AdLoaded;
          myAdDuplexBanner.AdCovered += myAdDuplexBanner_AdCovered;
          myAdDuplexBanner.AdLoadingError += myAdDuplexBanner_AdLoadingError;
          myAdDuplexBanner.NoAd += myAdDuplexBanner_NoAd;

          myAdGrid.Children.Add(myAdDuplexBanner);
      }
      myAdDuplexBanner.Visibility = Visibility.Visible;
  }

  private void myAdRefreshTimer_Tick(object sender, object e)
  {
      RefreshBanner();
  }

  private void RefreshBanner()
  {
      // Reset the error counter for this refresh interval and
      // make sure the ad grid is visible.
      errorCountCurrentRefresh = 0;
      myAdGrid.Visibility = Visibility.Visible;

      // Display ad from AdDuplex.
      if (100 == adDuplexWeight)
      {
          ActivateAdDuplexBanner();
      }
      // Display Microsoft ad.
      else if (0 == adDuplexWeight)
      {
          ActivateMicrosoftBanner();
      }
      // Use weighted approach.
      else
      {
          int randomInt = randomGenerator.Next(0, 100);
          if (randomInt < adDuplexWeight) ActivateAdDuplexBanner();
          else ActivateMicrosoftBanner();
      }
  }

  private void myMicrosoftBanner_AdRefreshed(object sender, RoutedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myMicrosoftBanner_ErrorOccurred(object sender, AdErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateAdDuplexBanner();
  }

  private void myAdDuplexBanner_AdLoaded(object sender, AdDuplex.Banners.Models.BannerAdLoadedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myAdDuplexBanner_NoAd(object sender, AdDuplex.Common.Models.NoAdEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdLoadingError(object sender, AdDuplex.Common.Models.AdLoadingErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdCovered(object sender,   AdDuplex.Banners.Core.AdCoveredEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }
  ```



<!--HONumber=Sep16_HO1-->


