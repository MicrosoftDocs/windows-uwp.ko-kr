---
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: 성공적인 제품 구매를 발생 시킨 각 Microsoft Store 트랜잭션에서는 트랜잭션 수신 확인을 선택적으로 반환할 수 있습니다.
title: 확인 메일을 사용하여 제품 구매 검증
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, 앱 내 구매, IAPs, 수신, Windows. ApplicationModel 스토어
ms.localizationpriority: medium
ms.openlocfilehash: ba87de0755469f373f9000f3d96d3021c9197985
ms.sourcegitcommit: 28bd367ab8acc64d4b6f3f73adca12100cbd359f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82148883"
---
# <a name="use-receipts-to-verify-product-purchases"></a>확인 메일을 사용하여 제품 구매 검증

성공적인 제품 구매를 발생 시킨 각 Microsoft Store 트랜잭션에서는 트랜잭션 수신 확인을 선택적으로 반환할 수 있습니다. 이 영수증은 나열 된 제품 및 금액 비용에 대 한 정보를 고객에 게 제공 합니다.

이 정보에 대 한 액세스 권한이 있으면 앱에서 사용자가 앱을 구매 했는지 확인 하거나 Microsoft Store에서 추가 기능 (앱 내 제품 또는 IAP 라고도 함)을 구입 해야 하는 시나리오를 지원 합니다. 예를 들어 다운로드 한 콘텐츠를 제공 하는 게임을 가정해 보겠습니다. 게임 콘텐츠를 구매한 사용자가 다른 장치에서 재생 하려는 경우에는 사용자가 이미 콘텐츠를 소유 하 고 있는지 확인 해야 합니다. 방법은 다음과 같습니다.

> [!IMPORTANT]
> 이 문서에서는 Windows 앱에서 제공 하는 구매에 대 한 확인을 가져오고 유효성을 검사 하기 위해 [Windows ApplicationModel. Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 네임 스페이스의 멤버를 사용 하는 방법을 보여 줍니다. 앱 내 구매를 위해 Windows 10, 버전 1607에 도입 되 고 Windows 10 기념일 버전을 대상으로 하는 프로젝트에서 사용할 수 있는 [windows. Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store) 네임 스페이스를 사용 하는 경우 **(10.0; 빌드 14393)** 또는 Visual Studio의 이후 버전에서이 네임 스페이스는 앱에서 바로 구매에 대 한 구매 영수증을 얻기 위한 API를 제공 하지 않습니다. 그러나 Microsoft Store collection API에서 REST 메서드를 사용 하 여 구매 트랜잭션에 대 한 데이터를 가져올 수 있습니다. 자세한 내용은 [앱에서 바로 구매에 대 한 수신](in-app-purchases-and-trials.md#receipts)확인을 참조 하세요.

## <a name="requesting-a-receipt"></a>수신 요청


**Windows** 네임 스페이스는 다음과 같은 여러 가지 방법을 지원 합니다.

* [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 또는 [Currentapp. RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) (또는이 메서드의 다른 오버 로드 중 하나)를 사용 하 여 구매할 경우 반환 값에는 수신이 포함 됩니다.
* [GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) 메서드를 호출 하 여 앱 및 앱의 모든 추가 기능에 대 한 현재 수신 정보를 검색할 수 있습니다.

앱 수신은 다음과 같이 표시 됩니다.

> [!NOTE]
> 이 예제는 XML을 읽을 수 있도록 하기 위해 형식이 지정 되었습니다. 실제 앱 수신 확인은 요소 사이에 공백을 포함 하지 않습니다.

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

제품 확인은 다음과 같습니다.

> [!NOTE]
> 이 예제는 XML을 읽을 수 있도록 하기 위해 형식이 지정 되었습니다. 실제 제품 확인은 요소 사이에 공백을 포함 하지 않습니다.

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

이러한 수신 예 중 하나를 사용 하 여 유효성 검사 코드를 테스트할 수 있습니다. 수신의 내용에 대 한 자세한 내용은 [요소 및 특성 설명을](#receipt-descriptions)참조 하십시오.

## <a name="validating-a-receipt"></a>수신 확인

수신의 신뢰성을 확인 하려면 공용 인증서를 사용 하 여 수신의 서명을 확인 하는 백 엔드 시스템 (웹 서비스 또는 이와 유사한 항목)이 필요 합니다. 이 인증서를 가져오려면 URL ```https://lic.apps.microsoft.com/licensing/certificateserver/?cid=CertificateId%60%60%60, where ```certificateid ' ' '는 수신의 **certificateid** 값입니다.

다음은 유효성 검사 프로세스의 예입니다. 이 코드는 **System. 보안** 어셈블리에 대 한 참조를 포함 하는 .NET Framework 콘솔 응용 프로그램에서 실행 됩니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>수신 확인에 대 한 요소 및 특성 설명

이 섹션에서는 수신의 요소와 특성에 대해 설명 합니다.

### <a name="receipt-element"></a>수신 요소

이 파일의 루트 요소는 앱 및 앱에서 바로 구매에 대 한 정보를 포함 하는 **수신** 요소입니다. 이 요소는 다음과 같은 자식 요소를 포함 합니다.

|  요소  |  필수  |  수량  |  Description   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    예        |  0 또는 1  |  현재 앱에 대 한 구매 정보가 포함 되어 있습니다.            |
|  [제품 수령](#productreceipt)  |     예       |  0개 이상    |   현재 앱에 대 한 앱에서의 구매에 대 한 정보를 포함 합니다.     |
|  서명  |      예      |  1   |   이 요소는 표준 [DSIG 구문](https://www.w3.org/TR/xmldsig-core/)입니다. 여기에는 수신 확인 및 **SignedInfo** 요소를 확인 하는 데 사용할 수 있는 서명이 포함 된가 수 **값** 요소가 포함 됩니다.      |

**수신** 에는 다음과 같은 특성이 있습니다.

|  특성  |  Description   |
|-------------|-------------------|
|  **버전**  |    수신 확인의 버전 번호입니다.            |
|  **CertificateId**  |     수신에 서명 하는 데 사용 되는 인증서 지문입니다.          |
|  **ReceiptDate**  |    영수증이 서명 되 고 다운로드 된 날짜입니다.           |  
|  **ReceiptDeviceId**  |   이 확인을 요청 하는 데 사용 되는 장치를 식별 합니다.         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>AppReceipt 요소

이 요소에는 현재 앱에 대 한 구매 정보가 포함 됩니다.

**Appreceipt** 에는 다음과 같은 특성이 있습니다.

|  특성  |  Description   |
|-------------|-------------------|
|  **Id**  |    구매를 식별 합니다.           |
|  **AppId**  |     OS에서 앱에 사용 하는 패키지 패밀리 이름 값입니다.           |
|  **LicenseType**  |    사용자가 앱의 전체 버전을 구매한 경우 **전체**입니다. **평가판**-사용자가 앱의 평가판을 다운로드 한 경우           |  
|  **PurchaseDate**  |    앱을 구입한 날짜입니다.          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>제품 수령 요소

이 요소는 현재 앱에 대 한 앱에서의 구매에 대 한 정보를 포함 합니다.

**제품 확인** 에는 다음과 같은 특성이 있습니다.

|  특성  |  Description   |
|-------------|-------------------|
|  **Id**  |    구매를 식별 합니다.           |
|  **AppId**  |     사용자가 구매한 앱을 식별 합니다.           |
|  **ProductId**  |     구매한 제품을 식별 합니다.           |
|  **ProductType**  |    제품 유형을 결정 합니다. 현재는 **영 속**값만 지원 합니다.          |  
|  **PurchaseDate**  |    구매가 발생 한 날짜입니다.          |  |

 

 
