---
title: 미디어 캡처 API 참조
description: Xbox 장치 포털 REST API를 사용 하 여 현재 화면의 PNG 표시를 캡처하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 5d5cd87b95f923fc0303d5cd4ed7ecbff70b8209
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254198"
---
# <a name="media-capture-api-reference"></a>미디어 캡처 API 참조

## <a name="request"></a>요청

다음 요청 형식을 사용 하 여 현재 화면의 PNG 표시를 캡처할 수 있습니다.

| 방법        | 요청 URI     |
| ------------- |-----------------|
| GET           | /ext/screenshot |

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수       | Description     |
| ------------------- |-----------------|
| 다운로드 (선택 사항) | HTTP 응답 헤더를 설정 해야 하는지 여부를 나타내는 부울 값입니다 .이 값은 호스트 브라우저가 브라우저에서 스크린샷 렌더링 하지 않고 스크린샷 파일을 다운로드 해야 함을 나타냅니다. |

**요청 헤더**

* 없음

**요청 본문**

* 없음

## <a name="response"></a>응답

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

| HTTP 상태 코드   | Description     |
| ------------------ |-----------------|
| 200                | 스크린샷 요청 성공 및 캡처 반환 됨 |
| 5XX                | 예기치 않은 오류에 대 한 오류 코드 |

**사용 가능한 장치 패밀리**

* Windows Xbox
