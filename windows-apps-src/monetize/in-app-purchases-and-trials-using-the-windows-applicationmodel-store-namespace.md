---
author: Xansky
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: Windows10 버전 1607 이전 릴리스를 대상으로 하는 UWP 앱에서, 앱에서 바로 구매 및 평가판을 사용하는 방법을 알아봅니다.
title: Windows.ApplicationModel.Store 네임스페이스를 사용하여 앱에서 바로 구매 및 평가판
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: uwp, 앱에서 바로 구매, IAP, 추가 기능, 평가판, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: bb2c242ea4b7e3881751874c096165279854aa5e
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4959108"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>Windows.ApplicationModel.Store 네임스페이스를 사용하여 앱에서 바로 구매 및 평가판

앱으로 수익을 창출할 수 있도록 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스의 멤버를 사용하여 UWP(유니버설 Windows 플랫폼) 앱에 앱에서 바로 구매 및 평가판 기능을 추가할 수 있습니다. 이러한 API를 통해 앱에 대한 라이선스 정보에 액세스할 수도 있습니다.

이 섹션의 문서에서는 몇 가지 일반적인 시나리오에서 **Windows.ApplicationModel.Store** 네임스페이스의 멤버를 사용하기 위한 자세한 지침과 코드 예제를 제공합니다. UWP 앱의 앱에서 바로 구매와 관련된 기본 개념의 개요를 보려면 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요. **Windows.ApplicationModel.Store** 네임스페이스를 사용하여 평가판 및 앱에서 바로 구매를 구현하는 방법을 보여 주는 전체 샘플은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)을 참조하세요.

> [!IMPORTANT]
> **Windows.ApplicationModel.Store** 네임스페이스는 새 기능에서 더 이상 업데이트되지 않습니다. 프로젝트가 Visual Studio에서 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 이상 릴리스를 대상으로 하는 경우(즉, Windows 10, 버전 1607 이상을 대상으로 함), [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스를 대신 사용하는 것이 좋습니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials)을 참조하세요. **Windows.ApplicationModel.Store** 네임스페이스는 [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 Windows 데스크톱 응용 프로그램 또는 개발자 센터에서 개발 샌드박스를 사용하는 앱 또는 게임(예: Xbox Live와 통합되는 모든 게임)에서 지원되지 않습니다. 해당 제품은 **Windows.Services.Store** 네임스페이스를 사용하여 앱에서 바로 구매 및 평가판을 구현해야 합니다.

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>CurrentApp 및 CurrentAppSimulator 클래스 시작

**Windows.ApplicationModel.Store** 네임스페이스의 기본 진입점은 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 클래스입니다. 이 클래스는 현재 앱과 사용 가능한 추가 기능에 대한 정보 가져오기, 현재 앱 또는 추가 기능에 대한 라이선스 정보 가져오기, 현재 사용자를 위한 앱 또는 추가 기능 구매 및 기타 작업에 사용할 수 있는 고정 속성 메서드를 제공합니다.

[CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 클래스는 Microsoft Store에서 해당 데이터를 가져오므로 개발자 계정이 있고 앱을 Microsoft Store에 게시해야 앱에서 이 클래스를 성공적으로 사용할 수 있습니다. Microsoft Store에 앱을 제출하기 전에 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx)라는 이 클래스의 시뮬레이트된 버전을 사용하여 코드를 테스트할 수 있습니다. 앱을 테스트한 후 Microsoft Store에 제출하기 전에 **CurrentAppSimulator** 인스턴스를 **CurrentApp**으로 바꾸어야 합니다. 앱이 **CurrentAppSimulator**를 사용하는 경우 인증에 실패합니다.

**CurrentAppSimulator**를 사용하는 경우 앱 라이선싱 및 앱에서 바로 구매 제품의 초기 상태는 WindowsStoreProxy.xml이라는 개발 컴퓨터의 로컬 파일에서 설명됩니다. 이 파일에 대한 자세한 내용은 [CurrentAppSimulator와 함께 WindowsStoreProxy.xml 파일 사용](#proxy)을 참조하세요.

**CurrentApp** 및 **CurrentAppSimulator**를 사용하여 수행할 수 있는 일반적인 작업에 대한 자세한 내용은 다음 문서를 참조하세요.

| 항목       | 설명                 |
|----------------------------|-----------------------------|
| [평가판의 기능 제외 또는 제한](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | 평가 기간 동안 고객이 앱을 무료로 사용할 수 있게 하는 경우 평가 기간 동안 일부 기능을 제외하거나 제한하여 고객이 앱 정식 버전으로 업그레이드하도록 유도할 수 있습니다. |
| [앱에서 바로 구매 제품 구매 사용](enable-in-app-product-purchases.md)      |  앱이 무료인지 여부와 상관없이, 앱 내에서 바로 콘텐츠, 기타 앱 또는 새 앱 기능(예: 게임의 다음 단계 잠금 해제)을 판매할 수 있습니다. 여기서는 앱에서 이러한 제품을 사용하도록 설정하는 방법을 보여 줍니다.  |
| [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)      | 스토어 상거래 플랫폼을 통해 앱에서 바로 구매 소모성 제품(구매, 사용 및 필요에 따라 다시 구매할 수 있는 항목)을 제공하여 강력하고 안정적인 구매 환경을 고객에게 제공합니다. 이 기능은 구매한 후 특정 회복 아이템을 구매하는 데 사용할 수 있는 게임 내 통화(금, 동전 등) 등에 특히 유용합니다. |
| [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)      |   앱에서 대규모 앱에서 바로 구매 제품 카탈로그를 제공하는 경우 이 항목에 설명된 프로세스를 선택적으로 수행하여 카탈로그를 관리할 수 있습니다.    |
| [영수증을 사용하여 제품 구매 확인](use-receipts-to-verify-product-purchases.md)      |   제품 구매를 성공적으로 이행한 각 Microsoft Store 거래에서 고객에게 나열된 제품 및 통화 비용에 대한 정보를 제공하는 거래 영수증을 선택적으로 반환할 수 있습니다. 사용자가 앱을 구매했거나, Microsoft Store에서 앱에서 바로 구매 제품을 구매했는지 앱이 확인해야 하는 경우 이 정보에 액세스할 수 있습니다. |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>CurrentAppSimulator와 함께 WindowsStoreProxy.xml 파일 사용

**CurrentAppSimulator**를 사용하는 경우 앱 라이선싱 및 앱에서 바로 구매 제품의 초기 상태는 WindowsStoreProxy.xml이라는 개발 컴퓨터의 로컬 파일에서 설명됩니다. 예를 들어 라이선스를 구입하거나 앱에서 바로 구매를 처리하여 앱 상태를 변경하는 **CurrentAppSimulator** 메서드는 메모리에 있는 **CurrentAppSimulator** 개체의 상태만 업데이트합니다. WindowsStoreProxy.xml의 내용은 변경되지 않습니다. 앱을 다시 시작하면 라이선스 상태가 WindowsStoreProxy.xml에 설명된 상태로 돌아갑니다.

WindowsStoreProxy.xml 파일은 기본적으로 다음 위치에 생성됩니다. %UserProfile%\AppData\Local\Packages\\&lt;앱 패키지 폴더&gt;\LocalState\Microsoft\Windows Store\ApiData. 이 파일을 편집하여 시뮬레이트할 시나리오를 **CurrentAppSimulator** 속성에서 정의할 수 있습니다.

이 파일의 값을 수정할 수는 있지만 **CurrentAppSimulator**에 대한 고유한 WindowsStoreProxy.xml 파일(Visual Studio 프로젝트의 데이터 폴더)을 만들어 대신 사용하는 것이 좋습니다. 거래를 시뮬레이트하는 경우 [ReloadSimulatorAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync)를 호출하여 파일을 로드합니다. **ReloadSimulatorAsync**를 호출하여 고유한 WindowsStoreProxy.xml 파일을 로드하지 않으면 **CurrentAppSimulator**에서 기본 WindowsStoreProxy.xml 파일을 만들고 로드합니다(덮어쓰지 않음).

> [!NOTE]
> **ReloadSimulatorAsync**가 완료되기 전에는 **CurrentAppSimulator**가 완전히 초기화되지 않은 것입니다. 또한 **ReloadSimulatorAsync**는 비동기 메서드이므로 다른 스레드에서 초기화되는 동안 한 스레드에서 **CurrentAppSimulator**를 쿼리하는 경합 상태가 발생하지 않도록 주의해야 합니다. 한 가지 방법은 플래그를 사용하여 초기화가 완료되었음을 나타내는 것입니다. Microsoft Store에서 설치된 앱은 **CurrentAppSimulator** 대신 **CurrentApp**을 사용해야 하며, 이 경우 **ReloadSimulatorAsync**가 호출되지 않으므로 방금 언급한 경합 상태가 적용되지 않습니다. 이 때문에 비동기적 및 동기적으로 두 경우에서 모두 작동하도록 코드를 설계합니다.


<span id="proxy-examples" />

### <a name="examples"></a>예제

이 예제는 2015년 1월 19일 5시(UTC)에 만료되는 평가판 모드의 앱을 설명하는 WindowsStoreProxy.xml 파일(UTF-16으로 인코드됨)입니다.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="UTF-16"?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>2B14D306-D8F8-4066-A45B-0FB3464C67F2</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/2B14D306-D8F8-4066-A45B-0FB3464C67F2</LinkUri>
      <CurrentMarket>en-US</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with a trial license</Name>
        <Description>Sample app for demonstrating trial license management</Description>
        <Price>4.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>true</IsTrial>
      <ExpirationDate>2015-01-19T05:00:00.00Z</ExpirationDate>
    </App>
  </LicenseInformation>
  <Simulation SimulationMode="Automatic">
    <DefaultResponse MethodName="LoadListingInformationAsync_GetResult" HResult="E_FAIL"/>
  </Simulation>
</CurrentApp>
```

다음 예제는 구입했으며 2015년 1월 19일 5시(UTC)에 만료되는 기능이 있고 앱에서 바로 구매 소모성이 있는 앱을 설명하는 WindowsStoreProxy.xml 파일(UTF-16으로 인코드됨)입니다.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-16" ?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>988b90e4-5d4d-4dea-99d0-e423e414ffbc</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/988b90e4-5d4d-4dea-99d0-e423e414ffbc</LinkUri>
      <CurrentMarket>en-us</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with several in-app products</Name>
        <Description>Sample app for demonstrating an expiring in-app product and a consumable in-app product</Description>
        <Price>5.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
    <Product ProductId="feature1" LicenseDuration="10" ProductType="Durable">
      <MarketData xml:lang="en-us">
        <Name>Expiring Item</Name>
        <Price>1.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
    <Product ProductId="consumable1" LicenseDuration="0" ProductType="Consumable">
      <MarketData xml:lang="en-us">
        <Name>Consumable Item</Name>
        <Price>2.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>false</IsTrial>
    </App>
    <Product ProductId="feature1">
      <IsActive>true</IsActive>
      <ExpirationDate>2015-01-19T00:00:00.00Z</ExpirationDate>
    </Product>
  </LicenseInformation>
  <ConsumableInformation>
    <Product ProductId="consumable1" TransactionId="00000001-0000-0000-0000-000000000000" Status="Active"/>
  </ConsumableInformation>
</CurrentApp>
```


<span id="proxy-schema" />

### <a name="schema"></a>스키마

이 섹션에서는 WindowsStoreProxy.xml 파일의 구조를 정의하는 XSD 파일을 보여 줍니다. WindowsStoreProxy.xml 파일을 사용할 때 Visual Studio의 XML 편집기에 이 스키마를 적용하려면 다음을 수행합니다.

1. Visual Studio에서 WindowsStoreProxy.xml 파일을 엽니다.
2. **XML** 메뉴에서 **스키마 만들기**를 클릭합니다. XML 파일의 내용을 기반으로 하여 임시 WindowsStoreProxy.xsd 파일이 생성됩니다.
3. .xsd 파일의 내용을 아래 스키마로 바꿉니다.
4. 여러 앱 프로젝트에 적용할 수 위치에 파일을 저장합니다.
5. Visual Studio에서 WindowsStoreProxy.xml 파일로 전환합니다.
6. **XML** 메뉴에서 **스키마**를 클릭한 다음 목록에서 WindowsStoreProxy.xsd 파일의 행을 찾습니다. 파일 위치가 원하는 위치가 아닌 경우(예: 임시 파일이 여전히 표시되는 경우) **추가**를 클릭합니다. 올바른 파일을 찾은 다음 **확인**을 클릭합니다. 이제 해당 파일이 목록에 표시됩니다. 해당 스키마에 대한 **사용** 열에 확인 표시가 있는지 확인합니다.

이 작업을 완료하면 WindowsStoreProxy.xml에서 편집하는 내용에 스키마가 적용됩니다. 자세한 내용은 [방법: 사용할 XML 스키마 선택](http://go.microsoft.com/fwlink/p/?LinkId=403014)을 참조하세요.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:import namespace="http://www.w3.org/XML/1998/namespace"/>
  <xs:element name="CurrentApp" type="CurrentAppDefinition"></xs:element>
  <xs:complexType name="CurrentAppDefinition">
    <xs:sequence>
      <xs:element name="ListingInformation" type="ListingDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LicenseInformation" type="LicenseDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ConsumableInformation" type="ConsumableDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Simulation" type="SimulationDefinition" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="ResponseCodes">
    <xs:restriction base="xs:string">
      <xs:enumeration value="S_OK">
        <xs:annotation>
          <xs:documentation>0x00000000</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_INVALIDARG">
        <xs:annotation>
          <xs:documentation>0x80070057</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_CANCELLED">
        <xs:annotation>
          <xs:documentation>0x800704C7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_FAIL">
        <xs:annotation>
          <xs:documentation>0x80004005</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_OUTOFMEMORY">
        <xs:annotation>
          <xs:documentation>0x8007000E</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="ERROR_ALREADY_EXISTS">
        <xs:annotation>
          <xs:documentation>0x800700B7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ConsumableStatus">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Active"/>
      <xs:enumeration value="PurchaseReverted"/>
      <xs:enumeration value="PurchasePending"/>
      <xs:enumeration value="ServerError"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StoreMethodName">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RequestAppPurchaseAsync_GetResult" id="RPPA"/>
      <xs:enumeration value="RequestProductPurchaseAsync_GetResult" id="RFPA"/>
      <xs:enumeration value="LoadListingInformationAsync_GetResult" id="LLIA"/>
      <xs:enumeration value="ReportConsumableFulfillmentAsync_GetResult" id="RPFA"/>
      <xs:enumeration value="LoadListingInformationByKeywordsAsync_GetResult" id="LLIKA"/>
      <xs:enumeration value="LoadListingInformationByProductIdAsync_GetResult" id="LLIPA"/>
      <xs:enumeration value="GetUnfulfilledConsumablesAsync_GetResult" id="GUC"/>
      <xs:enumeration value="GetAppReceiptAsync_GetResult" id="GARA"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="SimulationMode">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Interactive"/>
      <xs:enumeration value="Automatic"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ListingDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppListingDefinition"/>
      <xs:element name="Product" type="ProductListingDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ConsumableDefinition">
    <xs:sequence>
      <xs:element name="Product" type="ConsumableProductDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppListingDefinition">
    <xs:sequence>
      <xs:element name="AppId" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LinkUri" type="xs:anyURI" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrentMarket" type="xs:language" minOccurs="1" maxOccurs="1"/>
      <xs:element name="AgeRating" type="xs:unsignedInt" minOccurs="1" maxOccurs="1"/>
      <xs:element name="MarketData" type="MarketSpecificAppData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="MarketSpecificAppData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="MarketSpecificProductData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Tag" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Keywords" type="KeywordDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ImageUri" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="ProductListingDefinition">
    <xs:sequence>
      <xs:element name="MarketData" type="MarketSpecificProductData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="LicenseDuration" type="xs:integer" use="optional"/>
    <xs:attribute name="ProductType" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="guid">
    <xs:restriction base="xs:string">
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConsumableProductDefinition">
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="TransactionId" type="guid" use="required"/>
    <xs:attribute name="Status" type="ConsumableStatus" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="LicenseDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppLicenseDefinition"/>
      <xs:element name="Product" type="ProductLicenseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="IsTrial" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ProductLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ProductId" type="xs:string" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="SimulationDefinition" >
    <xs:sequence>
      <xs:element name="DefaultResponse" type="DefaultResponseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="SimulationMode" type="SimulationMode" use="optional"/>
  </xs:complexType>
  <xs:complexType name="DefaultResponseDefinition">
    <xs:attribute name="MethodName" type="StoreMethodName" use="required"/>
    <xs:attribute name="HResult" type="ResponseCodes" use="required"/>
  </xs:complexType>
  <xs:complexType name="KeywordDefinition">
    <xs:sequence>
      <xs:element name="Keyword" type="xs:string" minOccurs="0" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


<span id="proxy-descriptions" />

### <a name="element-and-attribute-descriptions"></a>요소 및 특성 설명

이 섹션에서는 WindowsStoreProxy.xml 파일의 요소와 특성에 대해 설명합니다.

이 파일의 루트 요소는 현재 앱을 나타내는 **CurrentApp** 요소입니다. 이 요소에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  |  설명   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    예        |  1  |  앱 목록의 데이터를 포함합니다.            |
|  [LicenseInformation](#licenseinformation)  |     예       |   1    |   이 앱과 지속형 추가 기능에 사용할 수 있는 라이선스를 설명합니다.     |
|  [ConsumableInformation](#consumableinformation)  |      아니요      |   0 또는 1   |   이 앱에 사용할 수 있는 소모성 추가 기능을 설명합니다.      |
|  [Simulation](#simulation)  |     아니요       |      0 또는 1      |   테스트 중에 다양한 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) 메서드 호출이 앱에서 작동하는 방식을 설명합니다.    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>ListingInformation 요소

이 요소는 앱 목록의 데이터를 포함합니다. **ListingInformation**은 **CurrentApp** 요소의 필수 자식입니다.

**ListingInformation**에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  |  설명   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-listinginformation)  |    예   |  1   |    앱에 대한 데이터를 제공합니다.         |
|  [Product](#product-child-of-listinginformation)  |    아니요  |  0개 이상   |      앱용 추가 기능을 설명합니다.     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>App 요소(ListingInformation의 자식)

이 요소는 앱의 라이선스를 설명합니다. **App**은 [ListingInformation](#listinginformation) 요소의 필수 자식입니다.

**App**에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  | 설명   |
|-------------|------------|--------|--------|
|  **AppId**  |    예   |  1   |   스토어에서 앱을 식별하는 GUID입니다. 테스트할 임의 GUID일 수 있습니다.        |
|  **LinkUri**  |    예  |  1   |    스토어 목록 페이지의 URI입니다. 테스트할 임의의 유효한 URI일 수 있습니다.         |
|  **CurrentMarket**  |    예  |  1   |    고객의 국가/지역입니다.         |
|  **AgeRating**  |    예  |  1   |     앱의 최소 연령별 등급을 나타내는 정수입니다. 앱을 제출할 때 개발자 센터 대시보드에서 지정하는 값과 같습니다. 스토어에서 사용되는 값은 3, 7, 12, 16입니다. 이러한 등급에 대한 자세한 내용은 [연령별 등급](../publish/age-ratings.md)을 참조하세요.        |
|  [MarketData](#marketdata-child-of-app)  |    예  |  1개 이상      |    지정된 국가/지역에 대한 앱 정보를 포함합니다. 앱이 나열되는 각 국가/지역에 대한 **MarketData** 요소를 포함해야 합니다.       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>MarketData 요소(App의 자식)

이 요소는 지정된 국가/지역에 대한 앱 정보를 제공합니다. 앱이 나열되는 각 국가/지역에 대한 **MarketData** 요소를 포함해야 합니다. **MarketData**는 [App](#app-child-of-listinginformation) 요소의 필수 자식입니다.

**MarketData**에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  | 설명   |
|-------------|------------|--------|--------|
|  **Name**  |    예   |  1   |   이 국가/지역의 앱 이름입니다.        |
|  **Description**  |    예  |  1   |      이 국가/지역에 대한 앱 설명입니다.       |
|  **Price**  |    예  |  1   |     이 국가/지역의 앱 가격입니다.        |
|  **CurrencySymbol**  |    예  |  1   |     이 국가/지역에서 사용되는 통화 기호입니다.        |
|  **CurrencyCode**  |    아니요  |  0 또는 1      |      이 국가/지역에서 사용되는 통화 코드입니다.         |  |

**MarketData**에는 다음 특성이 있습니다.

|  특성  |  필수  |  설명   |
|-------------|------------|----------------|
|  **xml:lang**  |    예        |     지역/국가 데이터 정보가 적용되는 국가/지역을 지정합니다.          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Product 요소(ListingInformation의 자식)

이 요소는 앱용 추가 기능을 설명합니다. **Product**는 [ListingInformation](#listinginformation) 요소의 선택적 자식이며 [MarketData](#marketdata-child-of-product) 요소를 하나 이상 포함합니다.

**Product**에는 다음 특성이 있습니다.

|  특성  |  필수  |  설명   |
|-------------|------------|----------------|
|  **ProductId**  |    예        |    앱에서 추가 기능을 식별하는 데 사용하는 문자열을 포함합니다.           |
|  **LicenseDuration**  |    아니요        |    항목을 구매한 후 라이선스가 유효한 일수를 나타냅니다. 제품 구매를 통해 생성된 새 라이선스의 만료 날짜는 구매 날짜에 라이선스 기간을 더한 날짜입니다. 이 특성은 **ProductType** 특성이 **Durable**인 경우에만 사용됩니다. 소모성 추가 기능의 경우 이 특성이 무시됩니다.           |
|  **ProductType**  |    아니요        |    앱에서 바로 구매 제품의 지속성을 식별하는 값을 포함합니다. 지원되는 값은 **Durable**(기본값) 및 **Consumable**입니다. 지속성 유형의 경우 [LicenseInformation](#licenseinformation) 아래의 [Product](#product-child-of-licenseinformation) 요소에서 추가 정보를 설명하고, 소모성 유형의 경우 [ConsumableInformation](#consumableinformation) 아래의 [Product](#product-child-of-consumableinformation) 요소에서 추가 정보를 설명합니다.           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>MarketData 요소(Product의 자식)

이 요소는 지정된 국가/지역에 대한 추가 기능 정보를 제공합니다. 추가 기능이 나열되는 각 국가/지역에 대한 **MarketData** 요소를 포함해야 합니다. **MarketData**는 [Product](#product-child-of-listinginformation) 요소의 필수 자식입니다.

**MarketData**에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  | 설명   |
|-------------|------------|--------|--------|
|  **Name**  |    예   |  1   |   이 국가/지역의 추가 기능 이름입니다.        |
|  **Price**  |    예  |  1   |     이 국가/지역의 추가 기능 가격입니다.        |
|  **CurrencySymbol**  |    예  |  1   |     이 국가/지역에서 사용되는 통화 기호입니다.        |
|  **CurrencyCode**  |    아니요  |  0 또는 1      |      이 국가/지역에서 사용되는 통화 코드입니다.         |  
|  **설명**  |    아니요  |   0 또는 1   |      이 국가/지역에 대한 추가 기능 설명입니다.       |
|  **Tag**  |    아니요  |   0 또는 1   |      추가 기능에 대한 [사용자 지정 개발자 데이터](../publish/enter-add-on-properties.md#custom-developer-data)(태그라고도 함)입니다.       |
|  **Keywords**  |    아니요  |   0 또는 1   |      추가 기능에 대한 [키워드](../publish/enter-add-on-properties.md#keywords)가 포함된 **Keyword** 요소를 최대 10개까지 포함합니다.       |
|  **ImageUri**  |    아니요  |   0 또는 1   |      추가 기능 목록의 [이미지 URI](../publish/create-add-on-store-listings.md#icon)입니다.           |  |

**MarketData**에는 다음 특성이 있습니다.

|  특성  |  필수  |  설명   |
|-------------|------------|----------------|
|  **xml:lang**  |    예        |     지역/국가 데이터 정보가 적용되는 국가/지역을 지정합니다.          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>LicenseInformation 요소

이 요소는 이 앱과 앱에서 바로 구매 지속형 제품에 사용할 수 있는 라이선스를 설명합니다. **LicenseInformation**은 **CurrentApp** 요소의 필수 자식입니다.

**LicenseInformation**에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  | 설명   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-licenseinformation)  |    예   |  1   |    앱의 라이선스를 설명합니다.         |
|  [Product](#product-child-of-licenseinformation)  |    아니요  |  0개 이상   |      앱에서 지속형 추가 기능의 라이선스 상태를 설명합니다.         |   |

다음 표에서는 **App** 및 **Product** 요소 아래의 값을 결합하여 몇 가지 일반적인 조건을 시뮬레이트하는 방법을 보여 줍니다.

|  시뮬레이트할 조건  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  완전히 사용이 허가됨  |    true   |  false  |    없음. 실제로 만료 날짜가 있고 이후 날짜를 지정할 수도 있지만 XML 파일에서는 이 요소를 생략하는 것이 좋습니다. 만료 날짜가 있고 이전 날짜를 지정하는 경우에는 **IsActive**가 무시되고 false로 간주됩니다.          |
|  체험 기간 내  |    true  |  true   |      &lt;a datetime in the future&gt; **IsTrial**이 true이기 때문에 이 요소를 표시해야 합니다. 현재 UTC(협정 세계시)를 표시하는 웹 사이트를 방문하여 원하는 남은 체험 기간을 얻기 위해 이 날짜를 얼마나 이후로 설정해야 하는지 확인할 수 있습니다.         |
|  만료된 평가판  |    false  |  true   |      &lt;a datetime in the past&gt; **IsTrial**이 true이기 때문에 이 요소를 표시해야 합니다. 현재 UTC(협정 세계시)를 표시하는 웹 사이트를 방문하여 "이전 날짜"가 UTC로 언제인지 확인할 수 있습니다.         |
|  잘못됨  |    false  | false       |     &lt;임의 값 또는 생략&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>App 요소(LicenseInformation의 자식)

이 요소는 앱의 라이선스를 설명합니다. **App**은 [LicenseInformation](#licenseinformation) 요소의 필수 자식입니다.

**App**에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  | 설명   |
|-------------|------------|--------|--------|
|  **IsActive**  |    예   |  1   |    이 앱의 현재 라이선스 상태를 설명합니다. 값이 **true**이면 라이선스가 유효함을 나타냅니다. **false**이면 잘못된 라이선스를 나타냅니다. 일반적으로 이 값은 앱이 평가판 모드인지 여부에 관계없이 **true**입니다.  잘못된 라이선스가 있을 경우 앱의 동작을 테스트하려면 이 값을 **false**로 설정합니다.           |
|  **IsTrial**  |    예  |  1   |      이 앱의 현재 평가판 상태를 설명합니다. 값이 **true**이면 체험 기간 동안 앱이 사용되고 있음을 나타냅니다. **false**이면 앱을 구매했거나 체험 기간이 만료되어 앱이 평가판 상태가 아님을 나타냅니다.         |
|  **ExpirationDate**  |    아니요  |  0 또는 1       |     이 앱의 체험 기간이 만료되는 UTC(협정 세계시) 날짜입니다. 날짜는 yyyy-mm-ddThh:mm:ss.ssZ로 표시되어야 합니다. 예를 들어 2015년 1월 19일 5시는 2015-01-19T05:00:00.00Z로 지정됩니다. **IsTrial**이 **true**이면 이 요소는 필수입니다. 그렇지 않으면 필요하지 않습니다.          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Product 요소(LicenseInformation의 자식)

이 요소는 앱에서 지속형 추가 기능의 라이선스 상태를 설명합니다. **Product**는 [LicenseInformation](#licenseinformation) 요소의 선택적 자식입니다.

**Product**에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  | 설명   |
|-------------|------------|--------|--------|
|  **IsActive**  |    예   |  1     |    이 추가 기능의 현재 라이선스 상태를 설명합니다. 값이 **true**이면 추가 기능을 사용할 수 있음을 나타냅니다. **false**이면 추가 기능을 사용할 수 없거나 구매하지 않았음을 나타냅니다.           |
|  **ExpirationDate**  |    아니요   |  0 또는 1     |     추가 기능이 만료되는 UTC(협정 세계시) 날짜입니다. 날짜는 yyyy-mm-ddThh:mm:ss.ssZ로 표시되어야 합니다. 예를 들어 2015년 1월 19일 5시는 2015-01-19T05:00:00.00Z로 지정됩니다. 이 요소가 있으면 추가 기능에 만료 날짜가 있습니다. 이 요소가 없으면 추가 기능이 만료되지 않습니다.  |  

**Product**에는 다음 특성이 있습니다.

|  특성  |  필수  |  설명   |
|-------------|------------|----------------|
|  **ProductId**  |    예        |   앱에서 추가 기능을 식별하는 데 사용하는 문자열을 포함합니다.            |
|  **OfferId**  |     아니요       |   앱에서 추가 기능이 속하는 범주를 식별하는 데 사용하는 문자열을 포함합니다. [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)에 설명된 대로 큰 항목 카탈로그를 지원합니다.           |

<span id="simulation"/>

#### <a name="simulation-element"></a>Simulation 요소

이 요소는 테스트 중에 다양한 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) 메서드 호출이 앱에서 작동하는 방식을 설명합니다. **Simulation**은 **CurrentApp**의 선택적 자식이며 [DefaultResponse](#defaultresponse) 요소를 0개 이상 포함합니다.

**Simulation**에는 다음 특성이 있습니다.

|  특성  |  필수  |  설명   |
|-------------|------------|----------------|
|  **SimulationMode**  |    아니요        |      값은 **Interactive** 또는 **Automatic**일 수 있습니다. 이 특성이 **Automatic**으로 설정된 경우 메서드가 지정된 HRESULT 오류 코드를 자동으로 반환합니다. 자동화된 테스트 사례를 실행할 때 사용할 수 있습니다.       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>DefaultResponse 요소

이 요소는 **CurrentAppSimulator** 메서드에서 반환되는 기본 오류 코드를 설명합니다. **DefaultResponse**는 [Simulation](#simulation) 요소의 선택적 자식입니다.

**DefaultResponse**에는 다음 특성이 있습니다.

|  특성  |  필수  |  설명   |
|-------------|------------|----------------|
|  **MethodName**  |    예        |   [schema](#schema)의 **StoreMethodName** 형식에 대해 표시되는 열거형 값 중 하나에 이 특성을 할당합니다. 이 열거형 값은 각각 테스트 중에 앱에서 오류 코드 반환 값을 시뮬레이트하려는 **CurrentAppSimulator** 메서드를 나타냅니다. 예를 들어 값이 **RequestAppPurchaseAsync_GetResult**이면 [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync) 메서드의 오류 코드 반환 값을 시뮬레이트하려는 것입니다.            |
|  **HResult**  |     예       |   [schema](#schema)의 **ResponseCodes** 형식에 대해 표시되는 열거형 값 중 하나에 이 특성을 할당합니다. 이 열거형 값은 각각 이 **DefaultResponse** 요소의 **MethodName** 특성에 할당된 메서드에 대해 반환하려는 오류 코드를 나타냅니다.           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>ConsumableInformation 요소

이 요소는 이 앱에 사용할 수 있는 소모성 추가 기능을 설명합니다. **ConsumableInformation**은 **CurrentApp**의 선택적 자식이며 [Product](#product-child-of-consumableinformation) 요소를 0개 이상 포함할 수 있습니다.

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Product 요소(ConsumableInformation의 자식)

이 요소는 소모성 추가 기능을 설명합니다. **Product**는 [ConsumableInformation](#consumableinformation) 요소의 선택적 자식입니다.

**Product**에는 다음 특성이 있습니다.

|  특성  |  필수  |  설명   |
|-------------|------------|----------------|
|  **ProductId**  |    예        |   앱에서 소모성 추가 기능을 식별하는 데 사용하는 문자열을 포함합니다.            |
|  **TransactionId**  |     예       |   앱에서 이행 프로세스를 통해 소모성의 구매 거래를 추적하는 데 사용하는 GUID(문자열)를 포함합니다. [앱에서 바로 구매 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)을 참조하세요.            |
|  **Status**  |      예      |  앱에서 소모성의 이행 상태를 나타내는 데 사용하는 문자열을 포함합니다. 값은 **Active**, **PurchaseReverted**, **PurchasePending** 또는 **ServerError**일 수 있습니다.             |
|  **OfferId**  |     아니요       |    앱에서 소모성이 속하는 범주를 식별하는 데 사용하는 문자열을 포함합니다. [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)에 설명된 대로 큰 항목 카탈로그를 지원합니다.           |
