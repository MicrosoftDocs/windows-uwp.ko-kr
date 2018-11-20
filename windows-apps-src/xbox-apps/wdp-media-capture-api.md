---
author: WilliamsJason
title: 미디어 캡처 API 참조
description: 프로그래밍 방식으로 미디어 캡처 API에 액세스하는 방법을 알아봅니다.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: f58fa4c3a9a1abd407f635f27de3a545c3aafc6c
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7422699"
---
# <a name="media-capture-api-reference"></a>미디어 캡처 API 참조 #

**요청**

다음 요청 형식을 사용하여 현재 화면의 PNG 표현을 캡처할 수 있습니다.

| 메서드        | 요청 URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.


| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| 다운로드(선택 사항)| 브라우저에서 렌더링하는 대신 호스트 브라우저가 스크린샷을 첨부 파일로 다운로드해야 함을 나타내는 HTTP 응답 헤더를 설정해야 하는지 여부를 나타내는 부울 값입니다.  |
<br>

**요청 헤더**

* 없음

**요청 본문**

* 없음

###<a name="response"></a>응답 # # #

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

| HTTP 상태 코드   | 설명     | 
| ------------------ |-----------------|
| 200                | 스크린샷 요청 성공 및 캡처 반환됨 |
| 5XX                | 예기치 않은 오류에 대한 오류 코드 |
<br>

**사용 가능한 디바이스 패밀리**

* Windows Xbox

