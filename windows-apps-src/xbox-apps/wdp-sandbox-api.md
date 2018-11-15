---
author: payzer
title: 디바이스 포털 Xbox Live 샌드박스 API 참조
description: 프로그래밍 방식으로 Xbox Live 샌드박스에 액세스하는 방법을 알아봅니다.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: 6f1729f07734b181dc5e0e8c97d702d8592302c2
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6655873"
---
# <a name="xbox-live-sandbox-api-reference"></a>Xbox Live 샌드박스 API 참조   
이 REST API를 사용하여 Xbox Live 샌드박스를 가져오고 설정할 수 있습니다.

## <a name="get-the-xbox-live-sandbox"></a>Xbox Live 샌드박스 가져오기

**요청**

다음 요청을 사용하여 디바이스의 Xbox Live 샌드박스에 대한 현재 값을 읽을 수 있습니다.

메서드      | 요청 URI
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**   
샌드박스 - (문자열) 디바이스가 있는 현재 샌드박스입니다.   

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 파일 공유의 자격 증명에 대한 액세스 요청이 허가되었습니다.
4XX | 오류 코드
5XX | 오류 코드

## <a name="set-the-xbox-live-sandbox"></a>Xbox Live 샌드박스를 설정합니다.
다음 요청을 사용하여 디바이스의 Xbox Live 샌드박스를 변경할 수 있습니다. Xbox One에서 디바이스를 다시 시작해야 설정이 적용됩니다.

**요청**

다음 요청을 사용하여 디바이스의 Xbox Live 샌드박스에 대한 현재 값을 설정할 수 있습니다.

메서드      | 요청 URI
:------     | :-----
PUT | /ext/xboxlive/sandbox
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   
요청 본문은 다음 필드를 포함하는 JSON 개체입니다.   
샌드박스 - (문자열) 디바이스의 샌드박스를 설정할 새 값입니다.

**응답**   
샌드박스 - (문자열) 디바이스가 있는 현재 샌드박스입니다.   

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 파일 공유의 자격 증명에 대한 액세스 요청이 허가되었습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox

