---
author: jnHs
Description: "대량으로 추가 기능 관리를 사용하면 개별적으로 각 업데이트를 제출하는 대신 한 번에 여러 추가 기능을 변경할 수 있습니다."
title: "대량으로 추가 기능 관리"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 6d1ffcc1-b3c6-4e2f-8fbe-d243b20a6272
ms.openlocfilehash: d5ef4c90aa9bb66ff6589d3dbe7aa6491fbf80a5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-add-ons-in-bulk"></a>대량으로 추가 기능 관리

> **중요** 이 기능은 현재 [개발자 센터 참가자 프로그램](dev-center-insider-program.md)에 가입한 개발자 계정으로만 사용할 수 있습니다. 모든 개발자에게 제공되기 전에 이 기능의 구현이 변경될 수 있습니다. 이 예비 설명서에서는 기능의 작동 방식에 대한 몇 가지 기본 정보를 제공합니다.

대량으로 추가 기능 관리를 사용하면 개별적으로 각 업데이트를 제출하는 대신 한 번에 여러 추가 기능을 변경할 수 있습니다. **대량으로 추가 기능 관리**를 클릭하면 앱의 개요 페이지에서 이 기능에 액세스할 수 있습니다.

## <a name="export-current-add-on-info"></a>현재 추가 기능 정보 내보내기

먼저 .csv 템플릿 파일을 다운로드해야 합니다. 이미 추가 기능을 만든 경우 이 파일에 관련 정보가 포함됩니다. 그렇지 않으면 새 추가 기능에 대한 정보를 입력하는 데 사용할 수 있는 빈 파일이 됩니다.

이 템플릿 파일을 생성하고 다운로드하려면 **추가 기능 내보내기**를 클릭하고 .csv 파일을 컴퓨터에 저장합니다.

.csv 파일에는 다음 열이 포함되어 있습니다. 

| 열 이름               | 설명                            | 필수 여부      |
|---------------------------|----------------------------------|----------------------|
| 제품 ID    |  추가 기능의 고유한 [제품 ID](set-your-add-on-product-id.md#product-id)입니다.  | 예. 추가 기능이 게시된 후에는 변경할 수 없습니다. |
| 액션 |템플릿을 가져올 때 적용하려는 동작입니다. 지원되는 값은 **제출**(새 추가 기능 제출 또는 이전에 게시한 추가 기능 업데이트) 및 **CreateDraft**(스토어에 제출하지 않고 변경 내용 저장)입니다. |     예 |
| 제품 유형    | 추가 기능의 [제품 유형](set-your-add-on-product-id.md#product-type)입니다. 지원되는 값은 **소모성** 또는 **지속형**입니다. |    예. 추가 기능이 게시된 후에는 변경할 수 없습니다. |
| 제품 수명    | 지속형 추가 기능의 경우 **계속**(만료되지 않는 제품) 또는 설정된 기간입니다. 허용되는 기간 값은 **1일, 3일, 5일, 7일, 14일, 30일, 60일, 90일, 180일, 365일**입니다.    | 예(제품 유형이 지속형인 경우) |
| 콘텐츠 형식    | 추가 기능의 [콘텐츠 형식](enter-add-on-properties.md#content-type)입니다. 대부분의 추가 기능의 경우 콘텐츠 형식은 **ElectronicSoftwareDownload**여야 합니다. 허용되는 다른 값은 **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService**입니다. |    예 |
| Tag    | 앱의 구현에 사용되는 선택적 [태그](enter-add-on-properties.md#custom-developer-data)(**사용자 지정 개발자 데이터**라고도 함) 정보입니다. | 아니요 |
| 기본 가격    | 추가 기능을 제공하려는 [기준 가격](set-add-on-pricing-and-availability.md#base-price)입니다. **무료** 또는 **0.99USD** 형식의 유효한 기준 가격이어야 합니다. |    예 |
| 출시일    | 추가 기능을 게시하려는 날짜입니다. 허용되는 값은 **즉시**, **수동** 또는 [ISO 8601 표준](http://go.microsoft.com/fwlink/p/?LinkId=817237)을 준수하는 날짜 문자열입니다. | 예 |
| 타이틀    | 추가 기능에 대해 고객에게 표시되는 이름으로, 앞에 언어 코드와 세미콜론이 옵니다. 예를 들어 영어/미국에서 "Example Title"이라는 타이틀을 사용하려면 *en-us;Example Title*을 입력합니다. 다른 언어의 추가 타이틀은 세미콜론으로 구분할 수 있습니다. 각 타이틀은 100자 이하여야 합니다.     | 예 |
|설명    | 고객에게 표시할 선택적 추가 정보로, 앞에 언어 로캘 코드와 세미콜론이 옵니다. 예를 들어 영어/미국에서 "This is an example"이라는 설명을 사용하려면 *en-us;This is an example*을 입력합니다. 다른 언어의 추가 타이틀은 세미콜론으로 구분할 수 있습니다. 각 설명은 200자 이하여야 합니다.    | 아니요 |
| 지역/국가 |    추가 기능을 제공하려는 하나 이상의 [지역/국가](define-pricing-and-market-selection.md#windows-store-consumer-markets)입니다. 각 지역/국가를 세미콜론으로 구분합니다. |    예 |
|키워드 |    앱 구현에서 사용된 선택적 [키워드](enter-add-on-properties.md#keywords)입니다. | 아니요 |

## <a name="import-add-ons"></a>추가 기능 가져오기

변경 내용을 가져오려면 먼저 다운로드한 .csv 파일을 원하는 변경 내용으로 업데이트해야 합니다.

이미 게시한 추가 기능을 변경하려면 스프레드시트의 복사본에서 변경하려는 값을 업데이트합니다. 업데이트하려는 추가 기능 행을 제거하거나 현재 상태대로 둘 수 있습니다. 해당 추가 기능에 대해 이미 진행 중인 제출이 있는 경우 .csv 파일을 사용하여 변경할 수 없습니다.

> **중요** 이미 게시한 추가 기능에 대한 업데이트를 제출하는 경우에는 **제품 ID** 및 **제품 유형** 필드를 변경할 수 없습니다.

새 추가 기능을 제출하려면 새 행을 추가하고 새 추가 기능에 대한 정보를 입력합니다. 모든 필수 정보를 입력해야 합니다. 

모든 변경 내용을 적용했으면 동일한 파일 이름으로 .csv 파일을 저장한 다음 지정한 필드로 끌어 파일을 업로드하거나 **파일 찾아보기**를 클릭합니다. 제출하기 전에 수정해야 하는 오류와 함께 변경 내용의 요약이 표시됩니다. 정보가 올바른지 확인한 후 **스토어에 제출**을 클릭할 수 있습니다. 제공한 정보를 사용하여 각 추가 기능이 제출 프로세스를 통과합니다.

