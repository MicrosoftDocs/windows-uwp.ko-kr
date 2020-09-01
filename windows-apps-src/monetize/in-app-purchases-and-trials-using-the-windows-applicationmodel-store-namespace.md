---
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: Windows 10, 버전 1607 이전 버전을 대상으로 하는 UWP 앱에서 앱 내 구매 및 평가판을 사용 하도록 설정 하는 방법에 대해 알아봅니다.
title: 앱에서 Windows. ApplicationModel. Store 네임 스페이스를 사용 하 여 구매 및 평가판
ms.date: 08/25/2017
ms.topic: article
keywords: uwp, 앱 내 구매, IAPs, 추가 기능, 평가판, Windows ApplicationModel. 스토어
ms.localizationpriority: medium
ms.openlocfilehash: 1ee18b6ea4cc8a2e77a970a273bc4a8a51516247
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162357"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>앱에서 Windows. ApplicationModel. Store 네임 스페이스를 사용 하 여 구매 및 평가판

수익 창출 네임 스페이스의 멤버를 사용 하 여 앱을 쉽게 수 있도록 앱 내 구매 및 평가판 기능을 UWP (유니버설 Windows 플랫폼) 앱에 추가할 수 [있습니다.](/uwp/api/windows.applicationmodel.store) 또한 이러한 Api는 앱에 대 한 라이선스 정보에 대 한 액세스를 제공 합니다.

이 단원의 문서에서는 몇 가지 일반적인 시나리오에 대 한 자세한 지침과 코드 예제를 제공 합니다 **.** UWP 앱에서 앱에서 바로 구매와 관련 된 기본 개념의 개요는 [앱 내 구매 및 평가판](in-app-purchases-and-trials.md)을 참조 하세요. **Windows. ApplicationModel. store** 네임 스페이스를 사용 하 여 평가판 및 앱 내 구매를 구현 하는 방법을 보여 주는 전체 샘플은 [store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)을 참조 하세요.

> [!IMPORTANT]
> 더 이상 새 기능으로 업데이트 되지 **않습니다.** 프로젝트가 **Windows 10 기념일 버전을 대상으로 하는 경우 (10.0; 빌드 14393)** 또는 Visual Studio의 이후 릴리스 (즉, windows 10, 버전 1607 이상)를 대신 사용 하는 것이 [좋습니다.](/uwp/api/windows.services.store) 자세한 내용은 [앱에서 바로 구매 및 평가판](./in-app-purchases-and-trials.md)을 참조 하세요. [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop) 를 사용 하는 windows 데스크톱 응용 프로그램 또는 파트너 센터에서 개발 샌드박스를 사용 하는 앱 또는 게임 (예: Xbox Live와 통합 되는 게임의 경우)은 **windows** 데스크톱 응용 프로그램에서 지원 되지 않습니다. 이러한 제품은 앱 내 구매 및 평가판을 구현 하기 위해 **Windows. Store** 네임 스페이스를 사용 해야 합니다.

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>CurrentApp 및 Currentapp시뮬레이터 클래스 시작

**Windows.** c o m a c e. Store 네임 스페이스에 대 한 주 진입점은 [currentapp](/uwp/api/windows.applicationmodel.store.currentapp) 클래스입니다. 이 클래스는 현재 앱 및 사용 가능한 추가 기능에 대 한 정보를 가져오고 현재 앱 또는 해당 추가 기능에 대 한 라이선스 정보를 가져오고 현재 사용자에 대 한 앱 또는 추가 기능을 구입 하 고 기타 작업을 수행 하는 데 사용할 수 있는 정적 속성 및 메서드를 제공 합니다.

[Currentapp](/uwp/api/windows.applicationmodel.store.currentapp) 클래스는 Microsoft Store에서 데이터를 가져오므로 개발자 계정이 있어야 하 고 앱이 저장소에 게시 되어야 앱에서이 클래스를 사용할 수 있습니다. 스토어에 앱을 제출 하기 전에 [Currentappsimulator](/uwp/api/windows.applicationmodel.store.currentappsimulator)라는이 클래스의 시뮬레이션 된 버전으로 코드를 테스트할 수 있습니다. 앱을 테스트 한 후 Microsoft Store에 제출 하기 전에 **Currentappsimulator** 인스턴스를 **currentapp**으로 바꾸어야 합니다. 앱이 **Currentappsimulator**를 사용 하는 경우 인증에 실패 합니다.

**Currentappsimulator** 를 사용 하는 경우 앱 라이선스 및 앱 내 제품의 초기 상태는 WindowsStoreProxy.xml 이라는 개발 컴퓨터의 로컬 파일에 설명 되어 있습니다. 이 파일에 대 한 자세한 내용은 [CurrentAppSimulator에서 WindowsStoreProxy.xml 파일 사용](#proxy)을 참조 하세요.

**Currentapp** 및 **currentapp시뮬레이터**를 사용 하 여 수행할 수 있는 일반적인 작업에 대 한 자세한 내용은 다음 문서를 참조 하세요.

| 항목       | Description                 |
|----------------------------|-----------------------------|
| [평가판의 기능 제외 또는 제한](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | 평가판 기간 동안 고객이 앱을 무료로 사용할 수 있도록 하는 경우 평가판 기간 중 일부 기능을 제외 하거나 제한 하 여 고객이 앱의 전체 버전으로 업그레이드 하도록 유도할 수 있습니다. |
| [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)      |  앱이 무료로 제공 되는지 여부에 관계 없이 앱 내에서 콘텐츠, 기타 앱 또는 새로운 앱 기능 (예: 게임의 다음 수준 잠금 해제)을 바로 판매할 수 있습니다. 앱에서 이러한 제품을 사용 하도록 설정 하는 방법을 보여 줍니다.  |
| [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)      | 스토어 상거래 플랫폼을 통해 고객에 게 강력 하 고 안정적인 구매 환경을 제공 하기 위해 스토어 상거래 플랫폼을 통해 구매, 사용 및 구매할 수 있는 앱 내 제품을 제공 합니다. 이 기능은 구입할 수 있는 게임 내 통화 (골드, 동전 등)와 특정 전원 공급 장치를 구입 하는 데 사용할 수 있는 작업에 특히 유용 합니다. |
| [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)      |   앱이 규모가 크고 앱 내 제품 카탈로그를 제공 하는 경우 필요에 따라이 항목에 설명 된 프로세스에 따라 카탈로그를 쉽게 관리할 수 있습니다.    |
| [확인 메일을 사용하여 제품 구매 검증](use-receipts-to-verify-product-purchases.md)      |   제품이 성공적으로 구매 되는 각 Microsoft Store 트랜잭션에서는 나열 된 제품에 대 한 정보와 통화 비용을 고객에 게 제공 하는 트랜잭션 수신 확인을 선택적으로 반환할 수 있습니다. 이 정보에 대 한 액세스 권한이 있으면 앱에서 사용자 앱을 구입 했는지 또는 Microsoft Store에서 앱 내 제품 구매를 수행 했는지 확인 해야 하는 시나리오를 지원 합니다. |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>CurrentAppSimulator에서 WindowsStoreProxy.xml 파일 사용

**Currentappsimulator** 를 사용 하는 경우 앱 라이선스 및 앱 내 제품의 초기 상태는 WindowsStoreProxy.xml 이라는 개발 컴퓨터의 로컬 파일에 설명 되어 있습니다. 앱의 상태를 변경 하는 **currentappsimulator** 메서드 (예: 라이선스 구매 또는 앱 내 구매 처리)는 메모리의 **currentappsimulator** 개체 상태만 업데이트 합니다. WindowsStoreProxy.xml 내용은 변경 되지 않습니다. 앱이 다시 시작 되 면 라이선스 상태가 WindowsStoreProxy.xml에 설명 된 내용으로 되돌아갑니다.

WindowsStoreProxy.xml 파일은 기본적으로 다음 위치에 만들어집니다 .%UserProfile%\AppData\Local\Packages \\ &lt; app package folder &gt; \LocalState\Microsoft\Windows Store\ApiData. 이 파일을 편집 하 여 **Currentappsimulator** 속성에서 시뮬레이션할 시나리오를 정의할 수 있습니다.

이 파일의 값을 수정할 수 있지만, 대신 사용할 **Currentappsimulator** 에 대 한 사용자 고유의 WindowsStoreProxy.xml 파일 (Visual Studio 프로젝트의 데이터 폴더에 있음)을 만드는 것이 좋습니다. 트랜잭션을 시뮬레이트하는 경우 [ReloadSimulatorAsync](/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync) 를 호출 하 여 파일을 로드 합니다. **ReloadSimulatorAsync** 를 호출 하 여 사용자 고유의 WindowsStoreProxy.xml 파일을 로드 하지 않는 경우 **currentappsimulator** 는 기본 WindowsStoreProxy.xml 파일을 만들고 로드 하지만 덮어쓰지는 않습니다.

> [!NOTE]
> **ReloadSimulatorAsync** 가 완료 될 때까지 **currentappsimulator** 는 완전히 초기화 되지 않습니다. **ReloadSimulatorAsync** 가 비동기 메서드 이기 때문에 다른 스레드에서 **currentappsimulator** 를 쿼리 하는 경합 상태를 방지 하기 위해 주의를 기울여야 합니다. 한 가지 방법은 초기화가 완료 되었음을 나타내는 플래그를 사용 하는 것입니다. Microsoft Store에서 설치 된 앱은 **Currentapp시뮬레이터**대신 **currentapp** 을 사용 해야 합니다 .이 경우에는 **ReloadSimulatorAsync** 가 호출 되지 않으므로 방금 언급 한 경합 상태는 적용 되지 않습니다. 이러한 이유로 두 경우 모두 비동기적으로 동기적으로 작동 하도록 코드를 디자인 합니다.


<span id="proxy-examples" />

### <a name="examples"></a>예

이 예제는 1 월 19 2015 일 05:00 (UTC)에 만료 되는 평가판 모드를 사용 하 여 앱을 설명 하는 WindowsStoreProxy.xml 파일 (UTF-16 인코딩)입니다.

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

다음 예제는 구매 된 앱을 설명 하는 WindowsStoreProxy.xml 파일 (UTF-16 인코딩) 이며, 1 월 19 2015 일에 05:00 (UTC)에 만료 되는 기능을 포함 하며, 사용할 수 있는 앱 내 구매를 포함 합니다.

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

이 섹션에는 WindowsStoreProxy.xml 파일의 구조를 정의 하는 XSD 파일이 나열 되어 있습니다. WindowsStoreProxy.xml 파일을 사용 하 여 작업할 때이 스키마를 Visual Studio의 XML 편집기에 적용 하려면 다음을 수행 합니다.

1. Visual Studio에서 WindowsStoreProxy.xml 파일을 엽니다.
2. **XML** 메뉴에서 **스키마 만들기**를 클릭 합니다. 그러면 XML 파일의 내용에 따라 임시 WindowsStoreProxy .xsd 파일이 생성 됩니다.
3. 해당 .xsd 파일의 내용을 아래 스키마로 바꿉니다.
4. 여러 앱 프로젝트에 적용할 수 있는 위치에 파일을 저장 합니다.
5. Visual Studio에서 WindowsStoreProxy.xml 파일로 전환 합니다.
6. **XML** 메뉴에서 **스키마**를 클릭 한 다음, windowsstoreproxy .xsd 파일의 목록에서 행을 찾습니다. 파일의 위치가 원하는 위치가 아닌 경우 (예: 임시 파일이 계속 표시 되는 경우) **추가**를 클릭 합니다. 오른쪽 파일로 이동한 다음 **확인**을 클릭 합니다. 이제 목록에 해당 파일이 표시 됩니다. 해당 스키마에 대 한 **사용** 열에 확인 표시가 나타나는지 확인 합니다.

이 작업을 완료 하면 WindowsStoreProxy.xml에 대 한 편집 내용이 스키마에 적용 됩니다. 자세한 내용은 [방법: 사용할 XML 스키마 선택](/visualstudio/xml-tools/how-to-select-the-xml-schemas-to-use)을 참조 하세요.

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

이 섹션에서는 WindowsStoreProxy.xml 파일의 요소와 특성에 대해 설명 합니다.

이 파일의 루트 요소는 현재 앱을 나타내는 **currentapp** 요소입니다. 이 요소는 다음과 같은 자식 요소를 포함 합니다.

|  요소  |  필수  |  수량  |  Description   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    예        |  1  |  앱 목록의 데이터를 포함 합니다.            |
|  [LicenseInformation](#licenseinformation)  |     예       |   1    |   이 앱 및 내구성 있는 추가 기능에 사용할 수 있는 라이선스에 대해 설명 합니다.     |
|  [ConsumableInformation](#consumableinformation)  |      예      |   0 또는 1   |   이 앱에 사용할 수 있는 사용 가능한 추가 기능을 설명 합니다.      |
|  [시뮬레이션](#simulation)  |     예       |      0 또는 1      |   테스트 하는 동안 응용 프로그램에서 다양 한 [Currentappsimulator](/uwp/api/windows.applicationmodel.store.currentappsimulator) 메서드를 호출 하는 방법을 설명 합니다.    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>ListingInformation 요소

이 요소는 앱 목록의 데이터를 포함합니다. **Listinginformation** 은 **currentapp** 요소의 필수 자식 항목입니다.

**Listinginformation** 에는 다음 자식 요소가 포함 됩니다.

|  요소  |  필수  |  수량  |  Description   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-listinginformation)  |    예   |  1   |    앱에 대한 데이터를 제공합니다.         |
|  [Product](#product-child-of-listinginformation)  |    예  |  0개 이상   |      앱에 대 한 추가 기능을 설명 합니다.     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>App 요소(ListingInformation의 자식)

이 요소는 앱의 라이선스를 설명 합니다. **App** 은 [listinginformation](#listinginformation) 요소의 필수 자식 항목입니다.

**앱** 에는 다음과 같은 자식 요소가 포함 되어 있습니다.

|  요소  |  필수  |  수량  | Description   |
|-------------|------------|--------|--------|
|  **AppId**  |    예   |  1   |   저장소에서 앱을 식별 하는 GUID입니다. 이는 테스트할 GUID 일 수 있습니다.        |
|  **Linkuri&gt**  |    예  |  1   |    저장소에 있는 목록 페이지의 URI입니다. 이는 테스트에 유효한 URI 일 수 있습니다.         |
|  **CurrentMarket**  |    예  |  1   |    고객의 국가/지역입니다.         |
|  **AgeRating**  |    예  |  1   |     앱의 최소 수명 등급을 나타내는 정수입니다. 이 값은 앱을 제출할 때 파트너 센터에서 지정 하는 것과 동일한 값입니다. 저장소에서 사용 하는 값은 3, 7, 12 및 16입니다. 이러한 등급에 대 한 자세한 내용은 [연령 등급](../publish/age-ratings.md)을 참조 하십시오.        |
|  [MarketData](#marketdata-child-of-app)  |    예  |  1개 이상      |    지정 된 국가/지역에 대 한 앱 정보를 포함 합니다. 앱이 나열 된 각 국가/지역에 대해 **MarketData** 요소를 포함 해야 합니다.       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>MarketData 요소 (앱의 자식)

이 요소는 지정 된 국가/지역에 대 한 앱 정보를 제공 합니다. 앱이 나열 된 각 국가/지역에 대해 **MarketData** 요소를 포함 해야 합니다. **MarketData** 는 [App](#app-child-of-listinginformation) 요소의 필수 자식입니다.

**MarketData** 에는 다음과 같은 자식 요소가 포함 되어 있습니다.

|  요소  |  필수  |  수량  | Description   |
|-------------|------------|--------|--------|
|  **이름**  |    예   |  1   |   이 국가/지역의 앱 이름입니다.        |
|  **설명**  |    예  |  1   |      이 국가/지역에 대 한 앱의 설명입니다.       |
|  **가격**  |    예  |  1   |     이 국가/지역에 있는 앱의 가격입니다.        |
|  **CurrencySymbol**  |    예  |  1   |     이 국가/지역에서 사용 되는 통화 기호입니다.        |
|  **CurrencyCode**  |    예  |  0 또는 1      |      이 국가/지역에서 사용 되는 통화 코드입니다.         |  |

**MarketData** 에는 다음과 같은 특성이 있습니다.

|  특성  |  필수  |  Description   |
|-------------|------------|----------------|
|  **xml: lang**  |    예        |     시장 데이터 정보를 적용할 국가/지역을 지정 합니다.          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Product 요소 (ListingInformation의 자식)

이 요소는 앱에 대 한 추가 기능을 설명 합니다. **Product** 는 [listinginformation](#listinginformation) 요소의 선택적 자식이 며 하나 이상의 [MarketData](#marketdata-child-of-product) 요소를 포함 합니다.

**제품** 에는 다음과 같은 특성이 있습니다.

|  특성  |  필수  |  Description   |
|-------------|------------|----------------|
|  **ProductId**  |    예        |    응용 프로그램에서 추가 기능을 식별 하는 데 사용 하는 문자열을 포함 합니다.           |
|  **LicenseDuration**  |    예        |    항목을 구매한 후 라이선스를 사용할 수 있는 일 수를 나타냅니다. 제품 구매에 의해 생성 된 새 라이선스의 만료 날짜는 구매 날짜에 라이선스 기간을 더한 날짜입니다. 이 특성은 **ProductType** 특성이 **지속**되는 경우에만 사용 됩니다. 이 특성은 사용할 수 있는 추가 기능에 대해서는 무시 됩니다.           |
|  **ProductType**  |    예        |    앱 내 제품의 지 속성을 식별 하는 값을 포함 합니다. 지원 되는 값은 **내구성이** 있는 (기본값) **및 사용할**수 있는 값입니다. 지속형 형식의 경우 [LicenseInformation](#licenseinformation)아래의 [Product](#product-child-of-licenseinformation) 요소에서 추가 정보를 설명 합니다. 사용할 수 있는 형식의 경우 [ConsumableInformation](#consumableinformation)아래의 [Product](#product-child-of-consumableinformation) 요소에서 추가 정보를 설명 합니다.           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>MarketData 요소 (Product의 자식)

이 요소는 지정 된 국가/지역에 대 한 추가 기능 정보를 제공 합니다. 추가 기능이 나열 된 각 국가/지역에 대해 **MarketData** 요소를 포함 해야 합니다. **MarketData** 는 [Product](#product-child-of-listinginformation) 요소의 필수 자식입니다.

**MarketData** 에는 다음과 같은 자식 요소가 포함 되어 있습니다.

|  요소  |  필수  |  수량  | Description   |
|-------------|------------|--------|--------|
|  **이름**  |    예   |  1   |   이 국가/지역에 있는 추가 기능 이름입니다.        |
|  **가격**  |    예  |  1   |     이 국가/지역에 있는 추가 기능 가격입니다.        |
|  **CurrencySymbol**  |    예  |  1   |     이 국가/지역에서 사용 되는 통화 기호입니다.        |
|  **CurrencyCode**  |    예  |  0 또는 1      |      이 국가/지역에서 사용 되는 통화 코드입니다.         |  
|  **설명**  |    예  |   0 또는 1   |      이 국가/지역의 추가 기능에 대 한 설명입니다.       |
|  **Tag**  |    예  |   0 또는 1   |      추가 기능에 대 한 [사용자 지정 개발자 데이터](../publish/enter-add-on-properties.md#custom-developer-data) (태그 라고도 함)입니다.       |
|  **C++ 키워드**  |    예  |   0 또는 1   |      추가 기능에 대 한 [키워드](../publish/enter-add-on-properties.md#keywords) 를 포함 하는 최대 10 개의 **키워드** 요소를 포함 합니다.       |
|  **ImageUri**  |    예  |   0 또는 1   |      추가 기능 목록에 있는 [이미지에 대 한 URI](../publish/create-add-on-store-listings.md#icon) 입니다.           |  |

**MarketData** 에는 다음과 같은 특성이 있습니다.

|  특성  |  필수  |  Description   |
|-------------|------------|----------------|
|  **xml: lang**  |    예        |     시장 데이터 정보를 적용할 국가/지역을 지정 합니다.          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>LicenseInformation 요소

이 요소는이 앱 및 지속적인 앱 내 제품에 사용할 수 있는 라이선스를 설명 합니다. **LicenseInformation** 는 **currentapp** 요소의 필수 자식입니다.

**LicenseInformation** 에는 다음과 같은 자식 요소가 포함 되어 있습니다.

|  요소  |  필수  |  수량  | Description   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-licenseinformation)  |    예   |  1   |    앱의 라이선스에 대해 설명 합니다.         |
|  [Product](#product-child-of-licenseinformation)  |    예  |  0개 이상   |      앱에 있는 내구성이 있는 추가 기능에 대 한 라이선스 상태를 설명 합니다.         |   |

다음 표에서는 **앱** 및 **제품** 요소의 값을 결합 하 여 몇 가지 일반적인 조건을 시뮬레이트하는 방법을 보여 줍니다.

|  시뮬레이션할 조건  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  정식 라이선스  |    true   |  false  |    제외한. 실제로 존재 하 고 미래 날짜를 지정할 수는 있지만 XML 파일에서 요소를 생략 하는 것이 좋습니다. 존재 하 고 과거 날짜를 지정 하는 경우 **IsActive** 는 무시 되 고 false로 간주 됩니다.          |
|  평가 기간에  |    true  |  true   |      &lt;&gt;이 요소는 **istrial** 가 true 이기 때문에 나중에이 요소에 포함 되어야 합니다. 현재 UTC (협정 세계시)를 표시 하는 웹 사이트를 방문 하 여 남은 평가 기간을 확보 하기 위해이를 설정 하는 미래 시간을 확인할 수 있습니다.         |
|  만료 된 평가판  |    false  |  true   |      &lt;&gt; **istrial** 가 true 이기 때문에이 요소의 과거에는 datetime이 있어야 합니다. 현재 utc (협정 세계시)를 표시 하는 웹 사이트를 방문 하 여 "과거"가 UTC에 있는 시기를 알 수 있습니다.         |
|  올바르지 않음  |    false  | false       |     &lt;모든 값 또는 생략&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>App 요소 (LicenseInformation의 자식)

이 요소는 앱의 라이선스를 설명 합니다. **앱** 은 [LicenseInformation](#licenseinformation) 요소의 필수 자식입니다.

**앱** 에는 다음과 같은 자식 요소가 포함 되어 있습니다.

|  요소  |  필수  |  수량  | Description   |
|-------------|------------|--------|--------|
|  **IsActive**  |    예   |  1   |    이 앱의 현재 라이선스 상태를 설명 합니다. **True** 값은 라이선스가 유효함을 나타냅니다. **false** 는 잘못 된 라이선스를 나타냅니다. 일반적으로이 값은 앱에 평가판 모드가 있는지 여부에 관계 없이 **적용**됩니다.  이 값을 **false** 로 설정 하 여 잘못 된 라이선스가 있는 경우 앱의 동작 방식을 테스트 합니다.           |
|  **IsTrial**  |    예  |  1   |      이 앱의 현재 평가판 상태를 설명 합니다. 값이 **true 이면** 평가 기간 동안 앱이 사용 중임을 나타냅니다. **false** 는 앱이 구매 되었거나 평가 기간이 만료 되었기 때문에 앱이 평가판에 없음을 나타냅니다.         |
|  **ExpirationDate**  |    예  |  0 또는 1       |     이 앱의 평가 기간이 만료 되는 날짜 (UTC (협정 세계시))입니다. 날짜는 yyyy-mm-Yyyy-mm-ddthh: mm: ss. ssZ로 표시 되어야 합니다. 예를 들어 01 년 1 월 2015 19 일 05:00은 2015-01-19T05:00:00.00 Z로 지정 됩니다. 이 요소는 **Istrial** 이 **true**일 때 필요 합니다. 그렇지 않은 경우에는 필요 하지 않습니다.          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Product 요소 (LicenseInformation의 자식)

이 요소는 앱에 있는 내구성이 있는 추가 기능에 대 한 라이선스 상태를 설명 합니다. **Product** 는 [LicenseInformation](#licenseinformation) 요소의 선택적 자식 요소입니다.

**Product** 에는 다음과 같은 자식 요소가 포함 되어 있습니다.

|  요소  |  필수  |  수량  | Description   |
|-------------|------------|--------|--------|
|  **IsActive**  |    예   |  1     |    이 추가 기능에 대 한 현재 라이선스 상태를 설명 합니다. **True** 값은 추가 기능을 사용할 수 있음을 나타냅니다. **false** 는 추가 기능을 사용할 수 없거나 구매 하지 못했음을 나타냅니다.           |
|  **ExpirationDate**  |    예   |  0 또는 1     |     추가 기능이 만료 되는 날짜 (UTC (협정 세계시))입니다. 날짜는 yyyy-mm-Yyyy-mm-ddthh: mm: ss. ssZ로 표시 되어야 합니다. 예를 들어 01 년 1 월 2015 19 일 05:00은 2015-01-19T05:00:00.00 Z로 지정 됩니다. 이 요소가 있으면 추가 기능에 만료 날짜가 표시 됩니다. 표시 되지 않는 경우 추가 기능이 만료 되지 않습니다.  |  

**제품** 에는 다음과 같은 특성이 있습니다.

|  특성  |  필수  |  Description   |
|-------------|------------|----------------|
|  **ProductId**  |    예        |   응용 프로그램에서 추가 기능을 식별 하는 데 사용 하는 문자열을 포함 합니다.            |
|  **OfferId**  |     예       |   앱에서 추가 기능이 속한 범주를 식별 하는 데 사용 되는 문자열을 포함 합니다. 이렇게 하면 [앱 내 제품의 대량 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)에 설명 된 대로 대량 항목 카탈로그를 지원할 수 있습니다.           |

<span id="simulation"/>

#### <a name="simulation-element"></a>시뮬레이션 요소

이 요소는 테스트 하는 동안 다양 한 [Currentappsimulator](/uwp/api/windows.applicationmodel.store.currentappsimulator) 메서드에 대 한 호출이 앱에서 작동 하는 방법을 설명 합니다. **시뮬레이션** 은 **currentapp** 요소의 선택적 자식 이며 0 개 이상의 [defaultresponse](#defaultresponse) 요소를 포함 합니다.

**시뮬레이션** 에는 다음과 같은 특성이 있습니다.

|  특성  |  필수  |  Description   |
|-------------|------------|----------------|
|  **SimulationMode**  |    예        |      값은 **대화형** 또는 **자동**이 될 수 있습니다. 이 특성이 **자동**으로 설정 되 면 메서드는 지정 된 HRESULT 오류 코드를 자동으로 반환 합니다. 이는 자동화 된 테스트 사례를 실행할 때 사용할 수 있습니다.       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>DefaultResponse 요소

이 요소는 **Currentappsimulator** 메서드에서 반환 되는 기본 오류 코드를 설명 합니다. **Defaultresponse** 는 [시뮬레이션](#simulation) 요소의 선택적 자식입니다.

**Defaultresponse** 에는 다음과 같은 특성이 있습니다.

|  특성  |  필수  |  Description   |
|-------------|------------|----------------|
|  **MethodName**  |    예        |   [스키마](#schema)에서 **StoreMethodName** 형식에 대해 표시 된 열거형 값 중 하나에이 특성을 할당 합니다. 이러한 각 열거형 값은 테스트 하는 동안 응용 프로그램에서 오류 코드 반환 값을 시뮬레이트하는 **Currentappsimulator** 메서드를 나타냅니다. 예를 들어 **RequestAppPurchaseAsync_GetResult** 값은 [RequestAppPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync) 메서드의 오류 코드 반환 값을 시뮬레이션 하려고 함을 나타냅니다.            |
|  **HResult**  |     예       |   [스키마](#schema)에서 **ResponseCodes** 형식에 대해 표시 된 열거형 값 중 하나에이 특성을 할당 합니다. 이러한 각 열거형 값은이 **Defaultresponse** 요소의 **MethodName** 특성에 할당 된 메서드에 대해 반환 하려는 오류 코드를 나타냅니다.           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>ConsumableInformation 요소

이 요소는이 앱에 사용할 수 있는 추가 기능을 설명 합니다. **ConsumableInformation** 는 **currentapp** 요소의 선택적 자식 이며, 0 개 이상의 [Product](#product-child-of-consumableinformation) 요소를 포함할 수 있습니다.

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Product 요소 (ConsumableInformation의 자식)

이 요소는 소비 추가 기능을 설명 합니다. **Product** 는 [ConsumableInformation](#consumableinformation) 요소의 선택적 자식 요소입니다.

**제품** 에는 다음과 같은 특성이 있습니다.

|  특성  |  필수  |  Description   |
|-------------|------------|----------------|
|  **ProductId**  |    예        |   사용 가능한 추가 기능을 식별 하기 위해 앱에서 사용 하는 문자열을 포함 합니다.            |
|  **TransactionId**  |     예       |   앱이 사용 하는 프로세스를 통해 사용 가능한 구매 트랜잭션을 추적 하는 데 사용 하는 GUID (문자열)를 포함 합니다. [앱 내 제품 구매 사용](enable-consumable-in-app-product-purchases.md)을 참조 하세요.            |
|  **상태**  |      예      |  앱에서 사용 가능한의 처리 상태를 나타내는 데 사용 되는 문자열을 포함 합니다. 값은 **활성**, **PurchaseReverted**, **PurchasePending**또는 **servererror**일 수 있습니다.             |
|  **OfferId**  |     예       |    앱에서 사용이 속한 범주를 식별 하는 데 사용 되는 문자열을 포함 합니다. 이렇게 하면 [앱 내 제품의 대량 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)에 설명 된 대로 대량 항목 카탈로그를 지원할 수 있습니다.           |