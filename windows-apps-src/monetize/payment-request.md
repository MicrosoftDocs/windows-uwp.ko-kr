---
description: 지불 요청 API에는 결제 정보를 입력 하 고 배송 방법을 선택 하도록 요구 하는 과정을 무시 하려면 UWP 앱에 대 한 통합된 솔루션을 제공 합니다.
title: 결제 요청 API로 결제를 간단하게
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp, 지불 요청
ms.openlocfilehash: a40b8265e3445319bd7baa530df0f9e9eaae0f31
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63804492"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>결제 요청 API로 결제를 간단하게
UWP 앱에 대 한 지불 요청 API 기반으로 합니다 [W3C 지불 요청 API 사양](https://w3c.github.io/browser-payment-api/)합니다. UWP 앱에서 체크 아웃 프로세스를 간소화 하는 기능 제공. 사용자가 체크 아웃을 통해 지불 옵션을 사용 하 고 이미 Microsoft 계정으로 저장 하는 주소를 전달 하 여 단축할 수 있습니다. 전환율을 높일 수 있으며 결제 정보를 토큰화 있으므로 데이터 침해 위험을 줄일 수 있습니다. Windows 10 크리에이터 스 업데이트부터, 사용자는 해당 저장 된 지불 옵션 UWP 앱에서 환경을 통해 쉽게 비용을 지불 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항
지불 요청 API를 사용 하 여 시작 하기 전에 수행 하거나 주의 해야 하는 몇 가지 있습니다.

### <a name="getting-a-merchant-id"></a>가맹점 ID를 가져오는 중
지불 요청 프로세스의 일부로, Microsoft 서비스 공급자 로부터 사용자를 대신해 결제 토큰을 요청합니다. 따라서 API 사용을 시작 하기 전에 해당 토큰을 요청 하려면 권한이 필요 합니다.  판매자 계정을 등록 하 고 필요한 권한을 제공 하는 몇 가지 단계를 따라야 합니다. 이렇게 하려면로 이동 [Microsoft 판매자 센터](https://seller.microsoft.com/en-us/dashboard/registration/seller/?accountprogram=uwp)합니다. 이 작업을 수행한 후 복사 결과 가맹점 ID 지불 요청을 생성할 때 앱에 대 한 파트너 센터에서. 그런 다음 응용 프로그램가 지불 요청을 제출 하는 경우는 토큰을 받게 지불 결제 금액을 제출 해야 하는 프로세서에서.

### <a name="geographic-restrictions-and-language-support"></a>지리적 제한 사항 및 언어 지원
미국 기반 비즈니스 미국에서 트랜잭션 처리 하기에 의해서만 지불 요청 API는 사용할 수 있습니다.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>앱에서 지불 요청 API 사용: 단계별 가이드
이 섹션에서는 사용 하는 방법에 설명 합니다 [UWP 지불 요청 API](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments) 앱에서. 여기에서 API는 명확한 설명을 위해 가장 간단한 형태로 사용합니다. 이러한 Api의 고급 사용법의 예제를 참조 하세요. 합니다 [GitHub의 쇼핑 UWP 앱 샘플](https://github.com/Microsoft/Windows-appsample-shopping)합니다.

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. 동의 하는 모든 지불 옵션의 집합을 만듭니다.
> [!Note]
> 대체는 **seller 포털에서 가맹점 id** 판매 업체를 사용 하 여 텍스트 ID 판매자 센터에서 받은 것입니다.

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. 지불 정보를 함께 가져오기. 

이러한 세부 정보는 지불 앱에서 사용자에 게 표시 됩니다. 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. 판매세가 포함 되어 있습니다. 

> [!Important]
> API 항목을 추가 하지 않거나를 판매 세금을 계산 합니다. 세율 관할지 다를 기억 합니다. 이해를 돕기 위해 가상 9.5% 세율을 사용합니다.

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (선택 사항)  총 할인 또는 다른 한정자를 추가 합니다. 

표시 항목에 특정 Contoso 신용 카드를 사용 하 여에 대 한 할인을 추가 하는 예는 다음과 같습니다. (*Contoso* 가상의 이름입니다.)

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. 모든 payment 세부 정보를 조합 합니다.

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. 지불 요청을 제출 합니다. 

호출 된 **SubmitPaymentRequestAsync** 지불 요청을 제출 하는 방법입니다. 그러면 사용 가능한 지불 옵션을 보여 주는 결제 앱.

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

사용자는 Microsoft 계정으로 로그인 하 라는 메시지가 표시 됩니다.

사용자가 로그인 한 후에 결제 옵션 및 이전에 저장 된 배송 주소를 선택할 수 있습니다.

![지불 요청 UI](./images/33.png "지불 요청 UI")

앱을 탭 하 여 사용자에 대 한 대기 **지불**, 다음 순서를 완료 합니다.

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

결제를 완료 한 후 사용자가 사용 하 여 표시 됩니다는 **순서 확인** 화면.

![확인 순서](./images/44.png "순서 확인 ")

## <a name="see-also"></a>참조
- [Windows.ApplicationModel.Payments 참조 설명서](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)
- [GitHub의 UWP 쇼핑 앱 샘플](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C 지불 요청 API 사양](https://www.w3.org/TR/payment-request/)
- [지불 요청 API ](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/device/payment-request-api)

