---
Description: 지불을 받을 수 있는 예상 지불 시간, 적용 가능한 지불 임계값 및 지불을 받을 수 있는 Microsoft 마켓플레이스는 country/region 및 지급 계정 유형에 따라 달라질 수 있습니다.
title: 지불 임계값, 방법 및 기간
ms.date: 03/16/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: d82276d8-f094-4d60-90f6-f836ce90e823
ms.localizationpriority: medium
ms.openlocfilehash: 686ed66622efa9c73aa0ed7de64a719d7c441eb4
ms.sourcegitcommit: a937963ce63a14c254420926661b9b68be28a8ee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84746763"
---
# <a name="payment-thresholds-methods-and-timeframes"></a>지불 임계값, 방법 및 기간

지불을 전송 하는 데 예상 되는 시간 및 지불을 받을 수 있는 Microsoft 마켓플레이스는 country/region 및 지급 계정 유형에 따라 달라질 수 있습니다. 이 항목에서는 각 국가/지역에서 지원 되는 지불 방법에 대해 간략하게 설명 합니다.

지정 된 국가/지역에 대해 ACH/SEPA 또는 실시간 전송을 사용 하 여 지불을 제공 합니다. 또한 일부 국가/지역은 지불 방법으로 PayPal을 지원 합니다.

> [!NOTE]
> 외부 환율이 변경 되기 때문에 정확한 판매량은 통화 마다 약간씩 다를 수 있습니다. 환율이 매월 계산 됩니다. 트랜잭션이 발생 한 시기에 따라 적절 한 환율이 적용 됩니다. 환율 및이에 해당 하는 날짜 범위는 각각 exchangeRate 및 exchangeRateDate 열의 지급 보고서에 표시 됩니다.

## <a name="number-of-days-for-payments-to-reach-payout-account"></a>지불 지급 계정에 도달 하는 기간 (일)

일반적으로 해당 월의 15 일을 기준으로 특정 월에 지불을 전송 하지만 계정에 결제 하는 데 추가 시간이 소요 됩니다. 이 일의 양은 아래에 설명 된 대로 계정에 사용 하는 지불 방법에 따라 달라 집니다.

> [!NOTE]
> 아래 표시 된 일은 대략적입니다. 지정 된 지급 시간이 길거나 더 짧을 수 있습니다.

| 결제 방법     | 지급 계정에 도달할 일 수     |
|--------------------|--------------------------------------------|
| PayPal             | 영업일 1 일                             |
| ACH/SEPA           | 근무일 2-3                          |
| 회선 전송      | 근무일 7-10                         |

각 marketplace에 이러한 지불 방법을 사용 하는 국가/지역을 보려면 아래 표를 검토 하세요.

## <a name="payment-methods-in-countriesregions"></a>국가/지역의 지불 방법

> [!NOTE]
> 모든 지역의 지불 임계값은 $50 USD입니다.

| 국가                          | Azure Marketplace | 매장, 광고, 믹서 및 Minecraft | Office | PayPal 지불액 |
|----------------------------------|-------------------|------------------------------------------|--------|-----------------|
| 아프가니스탄                      | Yes               | 예                                      | 아니요     | 아니요              |
| 알바니아                          | Yes               | Yes                                      | Yes    | Yes             |
| 알제리                          | Yes               | Yes                                      | Yes    | Yes             |
| 안도라                          | 아니요                | 예                                       | 예    | Yes             |
| 앙골라                           | Yes               | Yes                                      | 예    | No              |
| 앤티가 바부다              | Yes               | 예                                      | 예     | 예             |
| 아르헨티나                        | Yes               | Yes                                      | Yes    | Yes             |
| 아르메니아                          | Yes               | 예                                      | 아니요     | 아니요              |
| 오스트레일리아                        | Yes               | Yes                                      | Yes    | Yes             |
| 오스트리아                          | Yes               | Yes                                      | Yes    | Yes             |
| 아제르바이잔                       | Yes               | Yes                                      | 예    | No              |
| 바레인                          | Yes               | Yes                                      | Yes    | Yes             |
| 방글라데시                       | Yes               | Yes                                      | 예    | No              |
| 벨라루스                          | Yes               | Yes                                      | 예    | No              |
| 벨기에                          | Yes               | Yes                                      | Yes    | Yes             |
| 베냉                            | Yes               | Yes                                      | 예    | No              |
| 볼리비아                          | Yes               | Yes                                      | 예    | No              |
| 보스니아 헤르체고비나           | Yes               | Yes                                      | Yes    | Yes             |
| 보츠와나                         | Yes               | 예                                      | 예     | 예             |
| 브라질                           | Yes               | Yes                                      | Yes    | Yes             |
| 불가리아                         | Yes               | Yes                                      | Yes    | Yes             |
| 부르키나파소                     | Yes               | Yes                                      | 예    | No              |
| 부룬디                          | Yes               | Yes                                      | 예    | No              |
| 캄보디아                         | Yes               | Yes                                      | 예    | No              |
| 카메룬                         | Yes               | Yes                                      | 예    | No              |
| 캐나다                           | Yes               | Yes                                      | Yes    | Yes             |
| 중앙 아프리카 공화국         | Yes               | Yes                                      | 예    | No              |
| 차드                             | Yes               | Yes                                      | 예    | No              |
| 칠레                            | Yes               | Yes                                      | Yes    | Yes             |
| 중국                            | 예                | 예                                      | Yes    | Yes             |
| 콜롬비아                         | Yes               | Yes                                      | Yes    | Yes             |
| 코모로                          | Yes               | 예                                      | 아니요     | 아니요              |
| 콩고민주공화국                      | Yes               | Yes                                      | 예    | No              |
| 콩고 공화국               | Yes               | Yes                                      | 예    | No              |
| 코스타리카                       | Yes               | Yes                                      | Yes    | Yes             |
| 코트디부아르                    | Yes               | Yes                                      | 예    | No              |
| 크로아티아                          | Yes               | Yes                                      | 예    | No              |
| 키프로스                           | Yes               | Yes                                      | Yes    | Yes             |
| 체코                          | Yes               | Yes                                      | Yes    | Yes             |
| 덴마크                          | Yes               | Yes                                      | Yes    | Yes             |
| 도미니카                         | Yes               | 예                                      | 예     | 예             |
| 도미니카 공화국               | Yes               | Yes                                      | Yes    | Yes             |
| 에콰도르                          | Yes               | Yes                                      | Yes    | Yes             |
| 이집트                            | Yes               | Yes                                      | Yes    | Yes             |
| 엘살바도르                      | Yes               | Yes                                      | Yes    | Yes             |
| 에리트리아                          | Yes               | Yes                                      | 예    | No              |
| 에스토니아                          | Yes               | Yes                                      | Yes    | Yes             |
| 에티오피아                         | Yes               | Yes                                      | 예    | No              |
| 피지                     | Yes               | 예                                      | 아니요     | 아니요              |
| 핀란드                          | Yes               | Yes                                      | Yes    | Yes             |
| 프랑스                           | Yes               | Yes                                      | Yes    | Yes             |
| 조지아                          | Yes               | Yes                                      | Yes    | Yes             |
| 독일                          | Yes               | Yes                                      | Yes    | Yes             |
| 가나                            | Yes               | Yes                                      | 예    | No              |
| 그리스                           | Yes               | 예                                      | 예    | 예             |
| 과테말라                        | Yes               | 예                                      | 예    | 예             |
| 기니                           | Yes               | 예                                      | 예    | No              |
| 아이티                            | Yes               | 예                                      | 예    | No              |
| 온두라스                         | Yes               | 예                                      | 예    | 예             |
| 홍콩                        | Yes               | 예                                      | 예    | 예             |
| 헝가리                          | Yes               | 예                                      | 예    | 예             |
| 아이슬란드                          | Yes               | 예                                      | 예    | No              |
| 인도                            | Yes               | 예                                      | 예    | 예             |
| 인도네시아                        | Yes               | 예                                      | 예    | 예             |
| 이라크                             | Yes               | 예                                      | 아니요     | 아니요              |
| 아일랜드                          | Yes               | 예                                      | 예    | 예             |
| 이스라엘                           | Yes               | 예                                      | 예    | 예             |
| 이탈리아                            | Yes               | 예                                      | 예    | 예             |
| 자메이카                          | Yes               | 예                                      | 예    | 예             |
| 일본                            | Yes               | 예                                      | 예    | 예             |
| 요르단                           | Yes               | 예                                      | 예    | 예             |
| 카자흐스탄                       | Yes               | 예                                      | 예    | 예             |
| 케냐                            | Yes               | 예                                      | 예    | 예             |
| 대한민국 (남부)                    | Yes               | 예                                      | 예    | No              |
| 쿠웨이트                           | Yes               | Yes                                      | Yes    | Yes             |
| 키르기스스탄                       | 아니요                | 예                                       | 예    | No              |
| 라오스                             | Yes               | Yes                                      | 예    | No              |
| 라트비아                           | Yes               | Yes                                      | 예    | No              |
| 레바논                          | Yes               | 예                                      | 아니요     | 아니요              |
| 라이베리아                          | Yes               | Yes                                      | 예    | No              |
| 리히텐슈타인                    | Yes               | Yes                                      | Yes    | Yes             |
| 리투아니아                        | Yes               | Yes                                      | 예    | No              |
| 룩셈부르크                       | Yes               | Yes                                      | Yes    | Yes             |
| 마다가스카르                       | Yes               | Yes                                      | 예    | No              |
| 말라위                           | Yes               | Yes                                      | Yes    | Yes             |
| 말레이시아                         | Yes               | Yes                                      | Yes    | Yes             |
| 말리                             | Yes               | Yes                                      | 예    | No              |
| 몰타                            | Yes               | Yes                                      | Yes    | Yes             |
| 모리셔스                        | Yes               | 예                                      | 예     | 예             |
| 멕시코                           | Yes               | Yes                                      | Yes    | Yes             |
| 모나코                           | Yes               | 예                                      | 아니요     | 아니요              |
| 몽골                         | Yes               | Yes                                      | 예    | No              |
| 몬테네그로 공화국                       | Yes               | Yes                                      | 예    | No              |
| 모로코                          | Yes               | Yes                                      | 예    | No              |
| 모잠비크                       | Yes               | Yes                                      | Yes    | Yes             |
| 네팔                            | Yes               | Yes                                      | 예    | No              |
| 네덜란드                 | Yes               | Yes                                      | Yes    | Yes             |
| 뉴질랜드                      | Yes               | Yes                                      | Yes    | Yes             |
| 니카라과                        | Yes               | Yes                                      | Yes    | Yes             |
| 니제르                            | Yes               | Yes                                      | 예    | No              |
| 나이지리아                          | Yes               | Yes                                      | 예    | No              |
| 북마케도니아                  | 예                | 예                                      | 예    | No              |
| 노르웨이                           | Yes               | Yes                                      | Yes    | Yes             |
| 오만                             | Yes               | Yes                                      | Yes    | Yes             |
| 파키스탄                         | Yes               | Yes                                      | 예    | No              |
| 파나마                           | Yes               | Yes                                      | Yes    | Yes             |
| 파라과이                         | Yes               | Yes                                      | 예    | No              |
| 페루                             | Yes               | Yes                                      | Yes    | Yes             |
| 필리핀                      | Yes               | Yes                                      | Yes    | Yes             |
| 폴란드                           | Yes               | Yes                                      | Yes    | Yes             |
| 포르투갈                         | Yes               | Yes                                      | Yes    | Yes             |
| 푸에르토리코                      | 아니요                | 예                                       | 예    | No              |
| 카타르                            | Yes               | Yes                                      | Yes    | Yes             |
| 루마니아                          | Yes               | Yes                                      | 예    | No              |
| 러시아                           | Yes               | 예                                      | 예     | 예             |
| 르완다                           | Yes               | Yes                                      | 예    | No              |
| 세인트빈센트그레나딘 | 예                | 예                                      | 아니요     | 아니요              |
| 사우디아라비아                     | Yes               | Yes                                      | Yes    | Yes             |
| 세네갈                          | Yes               | Yes                                      | 예    | No              |
| 세르비아                           | Yes               | Yes                                      | 예    | No              |
| 시에라리온                     | Yes               | Yes                                      | 예    | No              |
| 싱가포르                        | Yes               | Yes                                      | Yes    | Yes             |
| 슬로바키아                         | Yes               | Yes                                      | Yes    | Yes             |
| 슬로베니아                         | Yes               | Yes                                      | Yes    | Yes             |
| 소말리아                          | Yes               | Yes                                      | 예    | No              |
| 남아프리카 공화국                     | Yes               | Yes                                      | Yes    | Yes             |
| 스페인                            | Yes               | Yes                                      | Yes    | Yes             |
| 스리랑카                        | Yes               | Yes                                      | 예    | No              |
| 스웨덴                           | Yes               | Yes                                      | Yes    | Yes             |
| 스위스                      | Yes               | Yes                                      | 예    | No              |
| 대만                           | 예                | 예                                      | Yes    | Yes             |
| 타지키스탄                       | Yes               | 예                                      | 아니요     | 아니요              |
| 탄자니아                         | Yes               | Yes                                      | 예    | No              |
| 태국                         | Yes               | Yes                                      | 예    | 예             |
| 동티모르(Timor-Leste)                      | Yes               | 예                                      | 예    | No              |
| 토고                             | Yes               | 예                                      | 예    | No              |
| 통가                            | Yes               | 예                                      | 아니요     | 아니요              |
| 트리니다드 토바고              | Yes               | 예                                      | 예    | 예             |
| 튀니지                          | Yes               | 예                                      | 예    | No              |
| 터키                           | Yes               | 예                                      | 예    | No              |
| 투르크메니스탄                     | Yes               | 예                                      | 아니요     | 아니요              |
| 우간다                           | Yes               | 예                                      | 예    | No              |
| 우크라이나                          | Yes               | 예                                      | 아니요     | 아니요              |
| 아랍에미리트연합국             | Yes               | 예                                      | 예    | 예             |
| 영국                   | Yes               | 예                                      | 예    | 예             |
| 미국                    | Yes               | 예                                      | 예    | 예             |
| 우루과이                          | Yes               | 예                                      | 예    | 예             |
| 우즈베키스탄                       | Yes               | 예                                      | 아니요     | 아니요              |
| 베네수엘라                        | Yes               | 예                                      | 예    | 예             |
| 베트남                          | Yes               | 예                                      | 예    | 예             |
| 잠비아                           | Yes               | 예                                      | 예    | No              |
| 짐바브웨                         | Yes               | 예                                      | 예    | 예              |
