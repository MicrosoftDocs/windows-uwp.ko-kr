---
description: 지불 요청 API는 사용자에 게 지불 정보를 입력 하 고 배송 방법을 선택 하도록 요구 하는 프로세스를 바이패스 하는 UWP 앱에 대 한 통합 솔루션을 제공 합니다.
title: 결제 요청 API를 사용하여 결제 간소화
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp, 지불 요청
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685041"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>결제 요청 API를 사용하여 결제 간소화
UWP 앱에 대 한 결제 요청 API는 [W3C 지불 요청 api 사양을](https://w3c.github.io/browser-payment-api/)기반으로 합니다. UWP 앱에서 체크 아웃 프로세스를 간소화 하는 기능을 제공 합니다. 사용자는 이미 Microsoft 계정와 함께 저장 된 결제 옵션 및 배송 주소를 사용 하 여 빠르게 체크아웃할 수 있습니다. 지불 정보는 토큰화 되기 때문에 변환 률을 높이고 데이터 위반 위험을 줄일 수 있습니다. Windows 10 크리에이터 업데이트부터 사용자는 저장 된 지불 옵션을 사용 하 여 UWP 앱의 여러 환경에서 쉽게 지불할 수 있습니다.

## <a name="prerequisites"></a>전제 조건
지불 요청 API 사용을 시작 하기 전에 수행 해야 하는 몇 가지 사항이 있습니다.

### <a name="getting-a-merchant-id"></a>판매자 ID 가져오기
지불 요청 프로세스의 일환으로 Microsoft는 서비스 공급자에 게 사용자를 대신 하 여 결제 토큰을 요청 합니다. 따라서 API 사용을 시작 하기 전에 해당 토큰을 요청할 수 있는 권한이 있어야 합니다.  판매자 계정에 등록 하 고 필요한 권한 부여를 제공 하려면 몇 가지 단계를 수행 해야 합니다. 이렇게 하려면 [Microsoft 판매자 센터](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp)로 이동 합니다. 이 작업을 완료 한 후 지불 요청을 생성할 때 파트너 센터에서 앱으로 결과 판매자 ID를 복사 합니다. 그런 다음 응용 프로그램에서 지불 요청을 제출 하면 결제를 제출 하는 데 필요한 프로세서에서 지불 토큰을 받게 됩니다.

### <a name="geographic-restrictions-and-language-support"></a>지리적 제한 및 언어 지원
지불 요청 API는 미국 기반 비즈니스 에서만 미국에서 트랜잭션을 처리 하는 데 사용할 수 있습니다.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>앱에서 지불 요청 API 사용: 단계별
이 섹션에서는 앱에서 [UWP 지불 요청 API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) 를 사용 하는 방법을 보여 줍니다. 여기서는 명확 하 게 이해 하기 위해 가장 간단한 형태로 API를 사용 합니다. 이러한 Api의 고급 사용에 대 한 예제는 [GitHub의 UWP 쇼핑 앱 샘플](https://github.com/Microsoft/Windows-appsample-shopping)을 참조 하세요.

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. 수락한 모든 지불 옵션 집합을 만듭니다.
> [!Note]
> 판매자- **id-판매자-포털** 텍스트를 판매자 센터에서 받은 판매자 id로 바꿉니다.

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. 지불 세부 정보를 함께 가져옵니다. 

이러한 세부 정보는 결제 앱에서 사용자에 게 표시 됩니다. 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. 판매 세금을 포함 합니다. 

> [!Important]
> API는 항목을 추가 하거나 판매 세금을 계산 하지 않습니다. 세금 요금은 관할지 마다 다릅니다. 명확 하 게 하기 위해 가상 9.5% 세율을 사용 합니다.

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (선택 사항) 합계에 할인이 나 기타 한정자를 추가 합니다. 

특정 Contoso 신용 카드를 사용 하 여 표시 항목에 할인을 추가 하는 예는 다음과 같습니다. (*Contoso* 는 가상의 이름입니다.)

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. 모든 지불 정보를 어셈블합니다.

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. 지불 요청을 제출 합니다. 

**SubmitPaymentRequestAsync** 메서드를 호출 하 여 결제 요청을 제출 합니다. 그러면 지불 앱이 제공 되어 사용 가능한 결제 옵션이 표시 됩니다.

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

사용자에 게 Microsoft 계정에 로그인 하 라는 메시지가 표시 됩니다.

사용자가 로그인 하면 이전에 저장 된 결제 옵션 및 배송 주소를 선택할 수 있습니다.

![지불 요청 UI](./images/33.png "지불 요청 UI")

앱은 사용자가 **지불**을 탭 하 여 주문을 완료할 때까지 기다립니다.

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

지불이 완료 되 면 사용자에 게 **주문 확인** 화면이 표시 됩니다.

![주문 확인 됨](./images/44.png "주문 확인 됨")

## <a name="see-also"></a>참고 항목
- [Windows ApplicationModel. 지불 참조 설명서](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [GitHub의 UWP 쇼핑 앱 샘플](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C 지불 요청 API 사양](https://www.w3.org/TR/payment-request/)
- [지불 요청 API](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

