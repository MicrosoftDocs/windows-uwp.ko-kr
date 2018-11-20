---
author: Xansky
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: 제품 구매를 성공적으로 이행한 각 Microsoft Store 거래에서 거래 영수증을 선택적으로 반환할 수 있습니다.
title: 영수증을 사용하여 제품 구매 확인
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp. 앱에서 바로 구매, IAP, 영수증, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: ed79a3ac50fb3a6cbe735e0ea713256845d39441
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7284700"
---
# <a name="use-receipts-to-verify-product-purchases"></a>영수증을 사용하여 제품 구매 확인

제품 구매를 성공적으로 이행한 각 Microsoft Store 거래에서 거래 영수증을 선택적으로 반환할 수 있습니다. 이 영수증은 나열된 제품 및 통화 비용에 대한 정보를 고객에게 제공합니다.

사용자가 앱을 구매했거나, Microsoft Store에서 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)을 구매했는지 확인해야 하는 경우 이 정보에 액세스할 수 있습니다. 예를 들어 다운로드 콘텐츠를 제공하는 게임을 가정해 보겠습니다. 게임 콘텐츠를 구매한 사용자가 다른 장치에서 플레이하려는 경우 사용자가 이미 콘텐츠를 소유하고 있는지 확인해야 합니다. 방법은 다음과 같습니다.

> [!IMPORTANT]
> 이 문서에서는 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 네임스페이스의 멤버를 사용하여 앱에서 바로 구매의 영수증을 가져오고 유효성을 검사하는 방법을 보여 줍니다. 앱에서 바로 구매(Windows 10, 버전 1607에 도입되었으며 Visual Studio에서 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 이상 릴리스를 대상으로 하는 프로젝트에 사용 가능)를 위해 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store) 네임스페이스를 사용하는 경우, 이 네임스페이스는 앱에서 바로 구매 건의 영수증을 받기 위해 API를 제공하지 않습니다. 그러나 Microsoft Store 컬렉션 API의 REST 메서드를 사용하여 구매 거래 데이터를 가져올 수 있습니다. 자세한 내용은 [앱에서 바로 구매의 영수증](in-app-purchases-and-trials.md#receipts)을 참조하세요.

## <a name="requesting-a-receipt"></a>영수증 요청


**Windows.ApplicationModel.Store** 네임스페이스는 영수증을 가져오는 다음 몇 가지 방법을 지원합니다.

* [CurrentApp.RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 또는 [CurrentApp.RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)(또는 이 메서드의 다른 오버로드 중 하나)를 사용하여 구매하는 경우 반환 값에 영수증이 포함되어 있습니다.
* [CurrentApp.GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) 메서드를 호출하여 앱과 앱의 모든 추가 기능에 대한 현재 영수증 정보를 검색할 수 있습니다.

앱 영수증은 다음과 같습니다.

> [!NOTE]
> 이 예제는 XML을 쉽게 읽을 수 있도록 형식화되었습니다. 실제 앱 영수증에는 요소 사이에 공백이 포함되지 않습니다.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

제품 영수증은 다음과 같습니다.

> [!NOTE]
> 이 예제는 XML을 쉽게 읽을 수 있도록 형식화되었습니다. 실제 제품 영수증에는 요소 사이에 공백이 포함되지 않습니다.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

이러한 영수증 예제 중 하나를 사용하여 유효성 검사 코드를 테스트할 수 있습니다. 영수증 내용에 대한 자세한 내용은 [요소 및 특성 설명](#receipt-descriptions)을 참조하세요.

## <a name="validating-a-receipt"></a>영수증 유효성 검사

영수증의 신뢰성을 확인하려면 공용 인증서를 사용하여 영수증의 서명을 확인할 백 엔드 시스템(웹 서비스 또는 이와 유사한 것)이 필요합니다. 이 인증서를 가져오려면 URL ```https://go.microsoft.com/fwlink/p/?linkid=246509&cid=CertificateId```를 사용합니다. 여기서 ```CertificateId```는 영수증의 **CertificateId** 값입니다.

다음은 해당 유효성 검사 프로세스 예입니다. 이 코드는 **System.Security** 어셈블리에 대한 참조를 포함하는 .NET Framework 콘솔 응용 프로그램에서 실행됩니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>영수증의 요소 및 특성 설명

이 섹션에서는 영수증의 요소와 특성을 설명합니다.

### <a name="receipt-element"></a>영수증 요소

이 파일의 루트 요소는 앱과 앱에서 바로 구매에 대한 정보를 포함하는 **Receipt** 요소입니다. 이 요소에는 다음과 같은 자식 요소가 있습니다.

|  요소  |  필수  |  수량  |  설명   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    아니요        |  0 또는 1  |  현재 앱에 대한 구매 정보를 포함합니다.            |
|  [ProductReceipt](#productreceipt)  |     아니요       |  0개 이상    |   현재 앱에 대한 앱에서 바로 구매 정보를 포함합니다.     |
|  Signature  |      예      |  1   |   이 요소는 표준 [DSIG XML 구문](http://go.microsoft.com/fwlink/p/?linkid=251093)입니다. 영수증 유효성 검사에 사용할 수 있는 서명이 포함된 **SignatureValue** 요소와 **SignedInfo** 요소를 포함합니다.      |

**Receipt**에는 다음 특성이 있습니다.

|  특성  |  설명   |
|-------------|-------------------|
|  **Version**  |    영수증의 버전 번호입니다.            |
|  **CertificateId**  |     영수증에 서명하는 데 사용된 인증서 지문입니다.          |
|  **ReceiptDate**  |    영수증이 서명되고 다운로드된 날짜입니다.           |  
|  **ReceiptDeviceId**  |   이 영수증을 요청하는 데 사용된 디바이스를 식별합니다.         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>AppReceipt 요소

이 요소는 현재 앱에 대한 구매 정보를 포함합니다.

**AppReceipt**에는 다음 특성이 있습니다.

|  특성  |  설명   |
|-------------|-------------------|
|  **Id**  |    구매를 식별합니다.           |
|  **AppId**  |     OS에서 앱에 사용하는 패키지 패밀리 이름 값입니다.           |
|  **LicenseType**  |    **Full** - 사용자가 앱의 처음 사용자용 버전을 구매한 경우 **Trial** - 사용자가 앱의 평가판을 다운로드한 경우           |  
|  **PurchaseDate**  |    앱을 구매한 날짜입니다.          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>ProductReceipt 요소

이 요소는 현재 앱에 대한 앱에서 바로 구매 정보를 포함합니다.

**ProductReceipt**에는 다음 특성이 있습니다.

|  특성  |  설명   |
|-------------|-------------------|
|  **Id**  |    구매를 식별합니다.           |
|  **AppId**  |     사용자가 구매를 수행한 앱을 식별합니다.           |
|  **ProductId**  |     구매한 제품을 식별합니다.           |
|  **ProductType**  |    제품 유형을 결정합니다. 현재 **Durable** 값만 지원합니다.          |  
|  **PurchaseDate**  |    구매가 발생한 날짜입니다.          |  |

 

 
