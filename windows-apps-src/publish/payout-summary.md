---
description: 지급 보고서는 앱 및 추가 기능을 사용 하 여 달성 한 비용에 대 한 세부 정보를 표시 합니다. 또한 지불을 받는 시기와 유료 정도를 알 수 있습니다.
title: 지급 보고서
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, 지급 요약, 문, 지불, 소득, 지급, 지불, 진행
ms.localizationpriority: medium
ms.openlocfilehash: 11d83031702a642fa21a711edfdbaff69661c853
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035026"
---
# <a name="payout-reports"></a>지급 보고서

**지급 요약** 은 Microsoft에서 달성 한 비용에 대 한 세부 정보를 보여줍니다. 또한 지불을 받는 시기와 유료 정도를 알 수 있습니다.

Azure Marketplace에서 제품을 판매 하는 경우 **지급 요약** 에 성공 지급 대 한 정보도 표시 됩니다. Azure Marketplace 결제에 대 한 자세한 내용은 [Microsoft Azure Marketplace 참여 정책](/legal/marketplace/participation-policy) 및 [Microsoft Azure Marketplace 게시자 계약](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)을 참조 하세요.

> [!NOTE]
> 지급을 사용할 수 있으려면 수익이 $50의 [결제 임계값](payment-thresholds-methods-and-timeframes.md)에 도달해야 합니다. 결제 임계값에 대한 자세한 내용은 이 페이지를 참조하고 앱 개발자 계약을 검토하세요.

> [!NOTE]
> 지급 계정 구성, 지급 누락, 지급 보류 등을 포함하여 지급과 관련된 지원이 필요한 경우 [여기](https://developer.microsoft.com/windows/support)의 지원에 문의하세요.

## <a name="access-the-payout-summary-pages"></a>지급 요약 페이지 액세스

지급 요약 페이지 중 하나를 열려면 다음을 수행합니다.

1. 오른쪽 위 모서리에서 [지급] 아이콘을 선택합니다.
2. [거래 기록], [결제] 또는 [데이터 내보내기]를 선택합니다.

## <a name="transaction-history-page"></a>거래 기록 페이지

이 페이지에는 각각에 대한 날짜, 유형 및 수익을 포함한 모든 개별 수익이 표시됩니다. 확인하려는 기간을 선택할 수 있으며, 등록 ID, 프로그램, 결제 ID, 수익 유형, 거래 규칙 및 상태별로 필터링할 수도 있습니다. 데이터는 현재 회계 연도(7월 1일 - 6월 30일) 및 지난 2년간의 회계 연도에 대해 사용할 수 있습니다.

수익에 대한 자세한 내용을 보려면 페이지 오른쪽에 있는 아래쪽 화살표를 선택합니다. 여기에는 거래 규칙, 수익 금액 및 제품이 표시됩니다. 어떤 이유로 든이 데이터를 사용할 수 없지만이 데이터에 액세스 해야 하는 경우 [지원](https://developer.microsoft.com/windows/support)담당자에 게 문의 하세요. 트랜잭션이 아닌 조정 결과가 발생 하는 경우 제품 필드는 표시 되지 않습니다.

이 페이지에서 트랜잭션 데이터를 내보내려면 내보내기를 선택한 다음 데이터 내보내기 페이지의 지시를 따릅니다. 트랜잭션 기록 페이지에서 내보낸 파일에는 트랜잭션 통화, 거래 통화 및 미국 달러의 수익, 유료 통화의 유료 값 등의 데이터가 표시 됩니다.

## <a name="payments-page"></a>결제 페이지

이 페이지의 합계에는 참여하는 모든 프로그램이 표시됩니다. 참가자 ID, 프로그램, 결제 ID 및 수익 유형을 기준으로 필터링할 수 있습니다. 금액은 미국 달러 단위로 제공됩니다. 또한 결제되는 금액은 결제 통화로 표시됩니다.

| 영역                   | Description                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| 해당 연도 총 지급액   | 모든 프로그램에 대해 미국 달러 단위로이 연도에 대 한 총 지불 금액입니다.       |
| 다음 예상 지급액 | 곧 귀하에 게 제공 되는 한 번의 지불 (다른 사용자가 곧 제공 되는 경우에도)은 미국 달러입니다. |
| 최근 결제           | 가장 최근의 지불 금액 (미국 달러), 프로그램 이름 및 프로그램입니다.           |
| 소스별 결제     | 지난 12 개월 동안 프로그램으로 표시 된 지불 금액 (미국 달러)입니다.           |
| 결제               | 유료 또는 보류 중을 선택 하 고 원하는 대로 정렬 합니다. 특정 결제에 대한 자세한 정보를 추가로 보려면 보기를 선택합니다. 지급 송금 명세서의 복사본을 다운로드하려면 다운로드를 선택합니다. 트랜잭션 기록 데이터를 표시 하는 데 최대 24 시간이 걸릴 수 있으므로 관련 소득을 즉시 볼 수 없습니다. |

이 페이지에서 데이터를 내보내려면 내보내기를 선택한 다음 데이터 내보내기 페이지의 지시를 따릅니다.

## <a name="payment-status"></a>결제 상태

| 수익 상태           | 이유                                                                                                                                      | 파트너 작업이 필요한가요?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| 처리 안 됨              | 지급해야 하는 수익입니다. 이 상태는 인센티브 프로그램의 프로그램 가이드에 정의된 냉각 기간 동안 유지됩니다. | 예                                                         |
| 예정                 | 지불을 처리 하기 전에 지불 주문에서 내부 검토를 생성 했습니다.                                                               | 예                                                         |
| 세금 계산서 보류 중      | 세금 송장이 불완전 하거나 잘못 되었습니다.                                                                                                  | 지급하기 전에 세금 계산서를 업데이트해야 합니다. |
| 검토 중 거부됨   | 검토 하는 동안 결제를 거부 했습니다.                                                                                                     | 자세한 내용은 [Microsoft 지원](https://developer.microsoft.com/windows/support)에 문의하세요.                      |
| 실패                   | Microsoft 시스템 오류로 인해 지불 하지 못했습니다.                                                                                         | 자세한 내용은 [Microsoft 지원](https://developer.microsoft.com/windows/support)에 문의하세요.                      |
| 진행 중              | 지불이 진행 중입니다.                                                                                                                 | 예                                                         |
| 잘못된 결제        | 지불 recouping 진행 중입니다.                                                                                                       | 예                                                         |
| 송금                     | 요금을 은행으로 보냈습니다.                                                                                                     | 예                                                         |
| 다시 처리 중             | 지불 중에 Microsoft 시스템 오류가 발생 하 여 다시 처리 하는 중입니다.                                                                  | 예                                                         |
| Reversed                 | 결제는 은행에 의해 반대 되었으며 다음 지불 주기에 다시 전송 됩니다.                                                     | 예                                                         |
| 세금계산서 거부됨     | 검토 중에 세금 계산서가 거부되었습니다. 세금계산서 검토가 완료될 때까지 보류 중인 모든 결제가 보류됩니다.                 | 자세한 내용은 [Microsoft 지원](https://developer.microsoft.com/windows/support)에 문의하세요.                      |
| 세금계산서 검토 중 | 세금계산서를 검토하고 있습니다. 세금계산서가 승인되면 결제가 해제됩니다.                                   | 예                                                         |
| 거부됨                 | 사용자의 은행이 결제를 거부 했습니다.                                                                                                      | 자세한 내용은 은행 담당자에게 문의하세요.                             |

## <a name="export-data-page"></a>데이터 내보내기 페이지

이 페이지의 지침에 따라 원하는 데이터를 내보냅니다.

참고:

- [데이터 내보내기] 페이지는 자체적으로 새로 고쳐지지 않습니다. 최신 데이터를 보려면 페이지를 수동으로 새로 고쳐야 할 수도 있습니다.
- 필터를 사용하면 데이터를 사용할 수 없음 오류가 발생할 수 있습니다. 이는 세 달에 선택한 기본 기간을 떠난 다음 해당 기간 외의 획득에서 지불 ID를 선택 했음을 의미 합니다. 기간을 늘리고 다시 시도합니다.

## <a name="payments"></a>지불

![결제 내보내기](images/pc-export-payments.png)

이 옵션은 지정된 프로그램, 관련 세금 및 수익 집계 금액에 대해 은행에서 받은 결제 보고서를 다운로드합니다. 이 보고서는 많은 파트너 센터 프로그램에 사용되므로 일부 열은 보고서에 적용되지 않을 수 있습니다. 이러한 열은 아래에 나와 있습니다.

| 열 이름              | Description                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | 프로그램에 따라 수익을 창출하는 파트너의 기본 ID                                                                             |
| participantIDType        | 일반적으로는 동기 프로그램의 프로그램 id 및 스토어 프로그램용 판매자 ID                                                                |
| participantName          | 수익 파트너의 이름                                                                                                               |
| programName              | 인센티브/Store 프로그램 이름                                                                                                              |
| earned                   | 해당 프로그램/participantID에 대한 수익 금액(결제 통화)                                                                       |
| earnedUSD                | 프로그램/participantID에 대한 수익 금액(USD)                                                                                      |
| withheldTax              | 프로그램/participantID에 대한 원천징수세액(결제 통화)                                                               |
| salesTax                 | 프로그램/participantID에 대한 판매세 총액(결제 통화)(인센티브 프로그램에만 적용 가능)                   |
| serviceFeeTax            | 프로그램/participantID에 대한 serviceFeeTax 총액(결제 통화)(Store 프로그램 및 Azure Marketplace에만 적용 가능) |
| totalPayment             | 프로그램/participantID에 대한 원천징수세를 제외하고 판매세를 포함한(해당하는 경우) 총 지급액(현지 통화)   |
| currencyCode             | 결제 통화 코드                                                                                                                      |
| paymentMethod            | 파트너를 지불 하는 데 사용 되는 방법 예: 전자 은행 전송, 크레딧 메모                                                     |
| paymentID                | 결제에 대한 고유 식별자. 이 번호는 일반적으로 은행 명세서에 표시 됩니다. (SAP 지불액에만 해당)              |
| paymentStatus            | 결제 상태                                                                                                                            |
| paymentStatusDescription | 결제 상태에 대한 친숙한 설명                                                                                                    |
| paymentDate              | Microsoft에서 결제한 날짜                                                                                                      |

## <a name="transaction-history"></a>거래 기록

![거래 기록 내보내기](images/pc-export-transaction.png)

이 옵션은 거래 기록 페이지에 표시되는 각 수익 품목, 수익 유형, 날짜, 관련 거래 금액, 고객, 제품 및 프로그램에 적용되는 기타 거래 세부 정보를 다운로드합니다.

| 열 이름                    | Description                                                                                                                              | 인센티브/Store/Azure Marketplace에 대한 적용 가능성           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | 각 수익에 대한 고유 식별자                                                                                                       | 모두                                                            |
| participantId                  | 프로그램에 따라 수익을 창출하는 파트너의 기본 ID                                                                            | 모두                                                            |
| participantIdType              | 일반적으로 인센티브 프로그램에 대한 프로그램 ID 및 Store 프로그램/Azure Marketplace에 대한 판매자 ID                                          | 모두                                                            |
| participantName                | 수익 파트너의 이름                                                                                                              | 모두                                                            |
| partnerCountryCode             | 획득 파트너의 위치/국가                                                                                                  | 모두                                                            |
| programName                    | 인센티브/Store 프로그램 이름                                                                                                             | 모두                                                            |
| transactionId                  | 거래에 대한 고유 식별자                                                                                                    | 모두                                                            |
| transactionCurrency            | 원래 고객 거래가 발생한 지역의 통화(파트너 위치의 통화가 아님)                                     | 모두                                                            |
| transactionDate                | 거래 날짜. 많은 거래가 하나의 수익에 기여하는 프로그램에 유용합니다.                                           | 모두                                                            |
| transactionExchangeRate        | 해당 거래 USD 금액을 표시하는 데 사용되는 환율 날짜                                                                 | 모두                                                            |
| transactionAmount              | 생성된 수익에 기반한 거래 금액(원래 거래 통화)                                              | 모두                                                            |
| transactionAmountUSD           | 거래 금액(USD)                                                                                                                | 모두                                                            |
| lever                          | 수익에 대한 비즈니스 규칙을 나타냅니다.                                                                                                  | 모두                                                            |
| earningRate                    | 수익을 생성하기 위해 거래 금액에 적용되는 인센티브율                                                                      | 모두                                                            |
| quantity                       | 프로그램에 따라 다릅니다. 거래 프로그램에 청구된 수량을 나타냅니다.                                                            | 모두                                                            |
| quantityType                   | 수량 유형 (예: 청구 된 수량, MAU)을 나타냅니다.                                                                             | 모두                                                            |
| earningType                    | 요금, 리베이트, coop, 판매 등 인지를 나타냅니다.                                                                                          | 모두                                                            |
| earningAmount                  | 원래 거래 통화의 수익 금액                                                                                      | 모두                                                            |
| earningAmountUSD               | 수익 금액(USD)                                                                                                                    | 모두                                                            |
| earningDate                    | 수익 창출 날짜                                                                                                                      | 모두                                                            |
| calculationDate                | 시스템에서 수익을 계산한 날짜                                                                                            | 모두                                                            |
| earningExchangeRate            | 해당 USD 금액을 표시하는 데 사용되는 환율                                                                                  | 모두                                                            |
| exchangeRateDate               | EarningAmount USD를 계산하는 데 사용되는 환율 날짜                                                                                   | 모두                                                            |
| paymentAmountWOTax             | "보낸 사람" 지불에 대해서만 요금을 지불 하는 금액 (세금 제외)                                                                 | 모두                                                            |
| paymentCurrency                | 결제 프로필에서 파트너가 선택한 통화. 송금 지급에 대해서만 표시됨                                                   | 모두                                                            |
| paymentExchangeRate            | ExchangeRateDate를 사용하여 paymentAmountWOTax를 결제 통화로 계산하는 데 사용되는 환율                                            | 모두                                                            |
| claimId                        | 클레임에 대한 고유 식별자                                                                                                              | 인센티브 - 일부 프로그램만 해당                                |
| planId                         | 플랜에 대한 고유 식별자                                                                                                               | 인센티브 - 일부 프로그램만 해당                                |
| paymentId                      | 결제에 대한 고유 식별자. 이 번호는 일반적으로 은행 거래 내역서에 표시됩니다.                                                 | SAP 결제 전용                                              |
| paymentStatus                  | 결제 상태                                                                                                                           | 모두                                                            |
| paymentStatusDescription       | 결제 상태에 대한 친숙한 설명                                                                                                   | 모두                                                            |
| customerId                     | 항상 비어 있음                                                                                                                     | 인센티브 프로그램만 해당(예외: OEM) 및 Azure Marketplace |
| customerName                   | 항상 비어 있음                                                                                                                     | 인센티브 프로그램만 해당(예외: OEM) 및 Azure Marketplace |
| partNumber                     | 항상 비어 있음                                                                                                                     | 일부 인센티브/Store 프로그램 및 Azure Marketplace        |
| productName                    | 거래에 연결된 제품 이름                                                                                                       | 모두                                                            |
| productId                      | 고유 제품 식별자                                                                                                                | Store 및 Azure Marketplace                                    |
| parentProductId                | 고유한 부모 제품 식별자. 참고: 트랜잭션에 대 한 부모 제품이 없으면 부모 제품 ID = 제품 ID입니다. | Store 및 Azure Marketplace                                    |
| parentProductName              | 부모 제품의 이름. 참고: 트랜잭션에 대 한 부모 제품이 없으면 부모 Product Name = Product Name을 입력 합니다.   | Store 및 Azure Marketplace                                    |
| productType                    | 제품 유형 (예: 앱, 추가 기능, 게임 등)                                                                                        | Store 및 Azure Marketplace                                    |
| invoiceNumber                  | 송장 번호(EA에만 해당)                                                                                                  | 인센티브 및 Azure Marketplace - 일부 프로그램만 해당           |
| subscriptionId                 | 고객에 연결된 구독 식별자                                                                                         | 인센티브 - 일부 프로그램만 해당                                 |
| subscriptionStartDate          | 구독 시작 날짜                                                                                                                  | 인센티브 - 일부 프로그램만 해당                                 |
| subscriptionEndDate            | 구독 종료 날짜                                                                                                                    | 인센티브 - 일부 프로그램만 해당                                 |
| resellerId                     | 재판매인 식별자                                                                                                                      | 인센티브 - 일부 프로그램만 해당                                 |
| resellerName                   | 재판매인 이름                                                                                                                            | 인센티브 - 일부 프로그램만 해당                                 |
| distributorId                  | 배포자 식별자                                                                                                                   | 인센티브 - 일부 프로그램만 해당                                 |
| distributorName                | 배포자 이름                                                                                                                         | 인센티브 - 일부 프로그램만 해당                                 |
| agreementNumber                | 계약 번호                                                                                                                         | 인센티브 - 일부 프로그램만 해당                                 |
| agreementStartDate             | 계약 시작 날짜                                                                                                                     | 인센티브 - 일부 프로그램만 해당                                 |
| agreementEndDate               | 계약 종료 날짜                                                                                                                       | 인센티브 - 일부 프로그램만 해당                                 |
| workload                       | 워크로드                                                                                                                                 | 인센티브 - 일부 프로그램만 해당                                 |
| transactionType                | 트랜잭션 유형 (예: 구매, 환불, 반전, 비용 정산 등)                                                               | Store 및 Azure Marketplace                                    |
| localProviderSeller            | 레코드의 로컬 공급자/판매자                                                                                                          | Store 전용                                                     |
| taxRemitted                    | 세금 납부 금액(판매세, 사용세 또는 VAT/GST 세금)                                                                                   | Store 및 Azure Marketplace                                    |
| taxRemitModel                  | 세금(판매세, 사용세 또는 VAT/GST 세금)을 납부할 책임이 있는 당사자                                                                    | Store 전용                                                     |
| storeFee                       | 스토어에서 앱 또는 추가 기능을 사용할 수 있도록 하기 위한 요금으로 Microsoft에서 보유 하는 금액입니다.                                           | Store 전용                                                     |
| transactionPaymentMethod       | 트랜잭션에 사용 되는 고객 지불 방법 (예: 카드, 모바일 운송 업체 청구, PayPal 등)                                | Store 및 Azure Marketplace                                    |
| tpan                           | 타사 광고 네트워크를 나타냅니다.                                                                                                     | Store - 광고만                                               |
| customerCountry                | 고객 국가                                                                                                                         | Store 및 Azure Marketplace                                    |
| customerCity                   | 고객 구/군/시                                                                                                                            | Store 및 Azure Marketplace                                    |
| customerState                  | 고객 시/도                                                                                                                           | Store 및 Azure Marketplace                                    |
| customerZip                    | 고객 우편 번호                                                                                                                 | Store 및 Azure Marketplace                                    |
| purchaseTypeCode               | 항상 비어 있음                                                                                                                     | 인센티브 프로그램 - CRI                                        |
| purchaseOrderType              | 항상 비어 있음                                                                                                                     | 인센티브 프로그램 - CRI                                        |
| purchaseOrderCoverageStartDate | 항상 비어 있음                                                                                                                     | 인센티브 프로그램 - CRI                                        |
| purchaseOrderCoverageEndDate   | 항상 비어 있음                                                                                                                     | 인센티브 프로그램 - CRI                                        |
| programOfferingLevel           |                                                                                                                                          | 인센티브 프로그램 - CRI                                        |
| TenantID                       |                                                                                                                                          | 인센티브 프로그램                                             |
| externalReferenceId            | 프로그램에 대한 고유 식별자                                                                                                        | 직접 지불 프로그램(인센티브 및 Store)                      |
| externalReferenceIdLabel       | 고유 식별자 레이블                                                                                                                  | 직접 지불 프로그램(인센티브 및 Store)                      |

## <a name="historical-statements"></a>거래 기록 명세서

![거래 기록 명세서 내보내기](images/pc-export-statements.png)

2019년 7월 1일 이전의 거래 이력은 별도로 처리됩니다. 명세서는 현재 필드 대신 다음 필드를 사용합니다.

> [!NOTE]
> 레거시 거래 기록에는 "예약됨"이라는 열이 있습니다. 이 열은 상태에서 "결제 완료"인 모든 수익이 제외된다는 것을 제외하고는 최신 기록의 "수익" 열에 해당합니다.

> [!NOTE]
> 3M, 6M, 12M 등의 필터는 **기록 문** 섹션에 적용 되지 않습니다.

| 필드 이름              | Description                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 수입원          | 트랜잭션이 발생 한 위치 (예: Microsoft Store, Windows Phone 저장소, Windows 스토어 8, 광고 등)를 기반으로 하는 수익의 원본입니다.                  |
| 주문 ID                | 고유한 주문 식별자. 이 ID를 사용 하면 환불, 지불 거절 등의 개별 구매 하지 않은 트랜잭션으로 구매 트랜잭션을 식별할 수 있습니다. 둘 다 동일한 주문 ID를 사용합니다. 또한 단일 구매에 여러 결제 방법을 사용 하는 분할 요금의 경우 구매 트랜잭션을 연결할 수 있습니다. |
| Transaction ID          | 고유한 거래 식별자                                                                                                                                          |
| 거래 날짜/시간   | 거래가 발생한 날짜 및 시간(UTC)                                                                                                                       |
| 부모 제품 ID       | 고유한 부모 제품 식별자. 참고: 트랜잭션에 대 한 부모 제품이 없으면 부모 제품 ID = 제품 ID입니다.                                |
| Product ID              | 고유한 제품 식별자                                                                                                                                              |
| 부모 제품 이름     | 부모 제품의 이름. 참고: 트랜잭션에 대 한 부모 제품이 없으면 부모 Product Name = Product Name을 입력 합니다.                                  |
| 제품 이름            | 제품의 이름입니다.                                                                                                                                                    |
| 제품 유형            | 제품 유형 (예: 앱, 추가 기능, 게임 등)                                                                                                                       |
| 수량                | 수입원이 비즈니스용 Microsoft Store인 경우 수량은 구매한 라이선스 수를 나타냅니다. 다른 모든 수입원의 경우 수량은 항상 1입니다. 참고: 서로 다른 두 지불 방법을 사용 하 여 단일 트랜잭션이 두 줄 항목으로 분할 된 경우에도 각 품목에는 1의 수량이 표시 됩니다. |
| 트랜잭션 유형        | 트랜잭션 유형 (예: 구매, 환불, 반전, 비용 정산 등)                                                                                              |
| 결제 방법          | 트랜잭션에 사용 되는 고객 지불 방법 (예: 카드, 모바일 운송 업체 청구, PayPal 등)                                                               |
| 국가/지역        | 트랜잭션이 발생 한 국가/지역입니다.                                                                                                                          |
| 로컬 공급자/판매자 | 레코드의 로컬 공급자/판매자.                                                                                                                                        |
| 거래 통화    | 트랜잭션의 통화입니다.                                                                                                                                            |
| 거래 금액      | 트랜잭션 양입니다.                                                                                                                                              |
| 세금 납부            | 세금 납부 금액(판매세, 사용세 또는 VAT/GST 세금)                                                                                                                  |
| 순 수령액            | 거래 금액 송금.                                                                                                                                   |
| Store 수수료               | 스토어에서 앱 또는 추가 기능을 사용할 수 있도록 하기 위한 요금으로 Microsoft에서 보유 한 Net 수령액의 백분율입니다.                                                      |
| 앱 수익            | Net 수령액에서 매장 요금을 뺀 값입니다.                                                                                                                                       |
| 원천징수세          | 수입 세금 보안상 이유로의 양입니다. ( **예약** 된 .csv 파일에는 포함 되지 않음)                                                                                                |
| 결제                 | 앱 수익에서 적용 가능한 원천징수세를 뺀 금액(거래 통화로 표시되는 금액) ( **예약** 된 .csv 파일에는 포함 되지 않음)                               |
| 환율                 | 거래 통화를 지불 통화로 변환 하는 데 사용 되는 외부 환율입니다.                                                                                         |
| 결제 통화        | 지불의 통화는에서 수행 됩니다.                                                                                                                                       |
| 변환된 결제       | FX Rate를 사용 하 여 지불 통화로 변환 된 지불 금액입니다.                                                                                                         |
| 세금 납부 모델         | 세금(판매세, 사용세 또는 VAT/GST 세금)을 납부할 책임이 있는 당사자                                                                                                   |
| 적격 날짜/시간   | 거래 수익이 지급될 수 있는 날짜 및 시간(UTC). 지급 생성 되 면 지급 만든 날짜 이전에 적격 날짜 시간이 있는 트랜잭션 진행이 포함 됩니다. ( **예약** 된 .csv 파일에만 포함 됩니다.) |
| Charges                 | 트랜잭션 금액 열에 집계 된 모든 요금 정보를 분석 하 여 보여 줍니다. Azure Marketplace에만 포함 되 고, **예약** 된 .csv 파일에는 포함 되지 않습니다. |
