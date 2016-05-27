---
author: mcleanbyron
ms.assetid: 86D9D3CF-8FDC-4B67-881B-DF33A1BEE8BF
description: 광고 조정을 사용하기 전에 앱에서 사용할 각 광고 네트워크에서 계정을 설정해야 합니다.
title: 광고 네트워크 선택 및 관리
---

# 광고 네트워크 선택 및 관리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

광고 조정을 사용하기 전에 앱에서 사용할 각 광고 네트워크에서 계정을 설정해야 합니다. Microsoft Visual Studio 프로젝트에 광고 조정자 컨트롤을 추가하기 전에 이 작업을 수행하는 것이 좋습니다. 또한 유연성을 극대화하여 광고 조정 성능을 최적화할 수 있도록 최대한 많은 광고 네트워크에서 계정을 설정하는 것이 좋습니다.

## 지원되는 광고 네트워크 및 프로젝트 형식

현재 광고 조정은 다음 광고 네트워크 및 프로젝트 형식에서 지원됩니다.

|  광고 네트워크    | XAML과 함께 C# 또는 Visual Basic을 사용하는 UWP(유니버설 Windows 플랫폼) 앱 | XAML과 함께 C# 또는 Visual Basic을 사용하는 Windows 8.1 앱 | XAML과 함께 C# 또는 Visual Basic을 사용하는 Windows Phone 8.1 앱 | Windows Phone 8.x Silverlight 앱 |               
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|-----------------------------------|---------------|
| [Microsoft](#microsoft)                         | **지원함**                                      | **지원**                                            | **지원**                     | **지원함** |
| [AdDuplex](#adduplex)                                                   | **지원함**                                      | **지원**                                            | **지원**                     | **지원함** |
| [Smaato](#smaato)                                                       | 지원되지 않음                                      | **지원함**                                            | **지원**                     | **지원함** |
| [AdMob(Google)](#admob)                                                | 지원되지 않음                                      | 지원되지 않음                                            | 지원되지 않음                     | **지원함** |
| [Inneractive](#inneractive)                                             | 지원되지 않음                                      | 지원되지 않음                                            | 지원되지 않음                     | **지원함** |
| [Vserv VMAX](#vserv)                                                    | 지원되지 않음                                      | 지원되지 않음                                            | **지원함**                     | 지원되지 않음 | 

일부 프로젝트 형식(예: C++ 및 XAML, JavaScript 및 HTML)은 현재 광고 조정에서 지원되지 않습니다. 이러한 프로젝트 형식의 경우 광고 조정을 사용하지 않고 Microsoft 광고를 표시할 수 있습니다. 자세한 내용은 [XAML 및 .NET의 AdControl](https://msdn.microsoft.com/library/mt313186.aspx)(영문)과 [HTML 5 및 JavaScript의 AdControl](https://msdn.microsoft.com/library/mt313130.aspx)(영문)을 참조하세요.

## 각 네트워크의 특정 매개 변수 및 세부 정보


계정을 설정하는 방법과 앱을 등록하는 방법을 비롯하여 각 광고 네트워크에 필요한 세부 정보는 다음과 같습니다. 또한 [앱을 제출하고 광고 조정을 구성](submit-your-app-and-configure-ad-mediation.md)할 때 및/또는 연결된 서비스([광고 조정 구현 테스트 시](test-your-ad-mediation-implementation.md))에서 제공해야 하는 필수 매개 변수를 나열합니다. 특정 광고 네트워크 사용에 대한 자세한 내용은 해당 웹 사이트를 참조하세요.

각 광고 네트워크에는 필수 매개 변수 외에도 앱에서 코드를 통해 설정할 수 있는 선택적 매개 변수가 더 있습니다. 예제를 보려면 이 항목의 뒷부분에 있는 [선택적 광고 네트워크 매개 변수](#optionalparameters)를 참조하세요. 선택적 매개 변수의 전체 목록은 각 광고 네트워크에서 제공한 설명서를 참조하세요. 이러한 매개 변수 중 일부는 광고 조정에서 무시하거나 덮어씁니다. 해당하는 항목의 목록은 아래 섹션에 나와 있습니다. 예를 들어 현재 위치 관련 매개 변수는 일반적으로 앱 내의 위치 기능에 의해 결정되는 고객 위치에 대한 정보로 덮어씁니다.

[광고 중재자 컨트롤을 추가](add-and-use-the-ad-mediator-control.md)하고 사용할 광고 네트워크를 지정하면 Visual Studio에서 일부 광고 네트워크에 대해 필요한 어셈블리를 프로그래밍 방식으로 가져옵니다. 자동으로 검색할 수 없는 어셈블리가 있는 경우 각 광고 네트워크에 대해 아래 표시된 링크를 사용하여 해당 어셈블리를 설치할 수 있습니다.

### Microsoft

| 웹 사이트                        | 광고 네트워크 매개 변수를 구성하려면 [Windows 개발자 센터 대시보드](https://dev.windows.com/overview)의 [광고 수익 창출](https://msdn.microsoft.com/library/windows/apps/mt170658) 페이지를 사용합니다.   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 위치                   | [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk).                                                                                                                                                                                                                         |
| 앱 등록              | 광고 조정 컨트롤을 앱에 추가하고 Windows 개발자 센터 대시보드에 앱을 제출합니다.                                                                                                                                                                                                            |
| 필수 매개 변수            | ApplicationId 및 AdUnitId: 이러한 매개 변수는 앱 콘텐츠에 따라 앱 패키지를 제출할 때 자동으로 채워집니다. 그러나 [앱을 제출하고 광고 조정을 구성](submit-your-app-and-configure-ad-mediation.md)할 때 이러한 매개 변수를 선택적으로 편집할 수도 있습니다. <br> <br> 높이 및 너비(Windows Phone 8 Silverlight 및 Windows Phone 8.1 Silverlight에만 필요)                                                                                                                                                                                                           |
| 덮어쓰기/무시되는 매개 변수 | 위도(덮어씀)  <br><br> 경도(덮어씀) <br><br> AutoRefreshIntervalInSeconds(무시됨) <br><br> IsAutoRefreshEnabled(무시됨) <br><br> IsAutoCollapsedEnabled(무시됨) <br><br> IsEngaged(무시됨) <br><br> IsSuspended(무시됨) |

 

### AdDuplex

| 웹 사이트                        | [http://adduplex.com](http://go.microsoft.com/fwlink/p/?LinkId=518028)      |
|--------------------------------|-----------------------------------------------------------------------------|
| SDK 위치                   | 먼저, [광고 조정자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)에서 설명한 대로 광고 조정자 컨트롤에서 연결된 서비스를 통해 어셈블리를 검색할 수 있도록 설정합니다. 어셈블리를 수동으로 다운로드해야 하는 경우 AdDuplex 웹 사이트의 [시작](http://go.microsoft.com/fwlink/p/?LinkId=518029)(영문) 섹션으로 이동합니다. |
| 앱 등록              | AdDuplex 포털에서 새 앱을 만듭니다.                                                                                                                                                                                                                                                                                                        |
| 필수 매개 변수            | AppKey <br><br> AdUnitId <br><br> 크기(Windows 8.1 XAML에만 필요)  |
| 덮어쓰기/무시되는 매개 변수 | 위도(덮어씀) <br><br> 경도(덮어씀) <br><br> AutoRefreshIntervalInSeconds(무시됨) <br><br> IsAutoRefreshEnabled(무시됨) <br><br> IsAutoCollapsedEnabled(무시됨) <br><br> IsEngaged(무시됨) <br><br> IsSuspended(무시됨) |
 

### Smaato

| 웹 사이트                        | [http://www.smaato.com/](http://go.microsoft.com/fwlink/p/?LinkId=518030)                                                                                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 위치                   | 먼저, [광고 조정자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)에서 설명한 대로 광고 조정자 컨트롤에서 연결된 서비스를 통해 어셈블리를 검색할 수 있도록 설정합니다. 어셈블리를 수동으로 다운로드해야 하는 경우 Smaato 포털의 다운로드 탭으로 이동합니다. |
| 앱 등록              | Smaato 포털에서 내 공간으로 이동한 다음 새 Adspace를 생성합니다.                                                                                                                                                                                                                |
| 필수 매개 변수            | Pub <br> <br> Adspace <br> <br> 높이 및 너비(Windows 8.1 XAML에만 필요)  |
| 덮어쓰기/무시되는 매개 변수 | GPS(덮어씀)                                                                                                                                                                                                                                                                |

 

### AdMob(Google)

| 웹 사이트                        | [http://apps.admob.com](http://go.microsoft.com/fwlink/p/?LinkId=518031)                                               |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|
| SDK 위치                   | [Google Mobile Ads SDK 웹 사이트](http://go.microsoft.com/fwlink/p/?LinkId=518032)에서 Windows Phone 8 SDK를 가져옵니다. |
| 앱 등록              | AdMob 포털에서 Monetize new app(새 앱으로 수익 창출)을 선택합니다.                                                                          |
| 필수 매개 변수            | AdUnitID <br> <br> 포맷(F)                                                                                              |
| 덮어쓰기/무시되는 매개 변수 | 위치(덮어씀)  <br><br> ForceTesting(무시됨) <br><br> 새로 고침 빈도(무시됨)                                |
 

### Inneractive

| 웹 사이트             | [http://inner-active.com](http://go.microsoft.com/fwlink/p/?LinkId=518035)                                                                                                                                                                                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 위치        | 먼저, [광고 조정자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)에서 설명한 대로 광고 조정자 컨트롤에서 연결된 서비스를 통해 어셈블리를 검색할 수 있도록 설정합니다. 어셈블리를 수동으로 다운로드해야 하는 경우 계정에 로그인하고 대시보드에서 SDK 탭으로 이동하여 Windows Phone 8 SDK를 다운로드합니다. |
| 앱 등록   | Inneractive 포털에서 새 앱을 만듭니다.                                                                                                                                                                                                                                                                                           |
| 필수 매개 변수 | AppID <br> <br> AdType(IaAdType_Banner 또는 IaAdType_Text)                                                                               |
 

### Vserv VMAX

| 웹 사이트             | [http://www.vmax.com](http://www.vmax.com)                                                                                                                                                                                                                                                                         |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 위치        | 먼저, [광고 조정자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)에서 설명한 대로 광고 조정자 컨트롤에서 연결된 서비스를 통해 어셈블리를 검색할 수 있도록 설정합니다. 어셈블리를 수동으로 다운로드해야 하는 경우 VMAX 웹 사이트의 [SDK](http://go.microsoft.com/fwlink/?LinkId=627078) 섹션으로 이동합니다. |
| 앱 등록   | VMAX 웹 사이트에서 앱의 Adspot ID를 가져옵니다(Adspot ID는 VMAX API에서 ZoneId라고 함).                                                                                                                                                                                                                     |
| 필수 매개 변수 | ZoneId                                                                                                                                                                                                                                                                                                                         |12345

 

## 선택적 광고 네트워크 매개 변수


각 광고 네트워크에는 필수 매개 변수 외에도 앱에서 코드를 통해 설정할 수 있는 선택적 매개 변수가 더 있습니다. 선택적 매개 변수의 전체 목록은 각 광고 네트워크에서 제공한 설명서를 참조하세요. 코드에서 이러한 선택적 매개 변수를 설정하려면 **AdMediatorControl** 개체의 **AdSdkOptionalParameters** 속성을 사용하세요.

다음 예제에서는 Microsoft Advertising의 [CountryOrRegion](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.countryorregion.aspx) 속성을 사용자의 2자리 국가 또는 지역 코드로 설정하는 방법을 설명합니다.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["CountryOrRegion"] = "IN";
```

다음 예제에서는 Smaato의 **Width** 및 **Height** 매개 변수를 설정하는 방법을 설명합니다.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 250;
```

## 관련 항목

* [광고 조정 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)
* [광고 조정 구현 테스트](test-your-ad-mediation-implementation.md)
* [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)
* [광고 조정 문제 해결](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->


