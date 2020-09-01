---
title: 장치 포털 Xbox Live sandbox API 참조
description: Xbox 장치 포털 REST API를 사용 하 여 장치의 Xbox Live sandbox에 대 한 값을 가져오고 설정 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: ebe280af4279719109db2e36904b501623956339
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166807"
---
# <a name="xbox-live-sandbox-api-reference"></a>Xbox Live Sandbox API 참조   
이 REST API 사용 하 여 Xbox Live 샌드박스를 가져오고 설정할 수 있습니다.

## <a name="get-the-xbox-live-sandbox"></a>Xbox Live 샌드박스 가져오기

**요청**

다음 요청을 사용 하 여 장치의 Xbox Live sandbox에 대 한 현재 값을 읽을 수 있습니다.

방법      | 요청 URI
:------     | :-----
GET | /ext/xboxlive/sandbox

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답이**   
Sandbox-(문자열) 장치가 있는 현재 샌드박스입니다.   

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 파일 공유에 대 한 자격 증명에 대 한 액세스 요청이 부여 되었습니다.
4XX | 오류 코드
5XX | 오류 코드

## <a name="set-the-xbox-live-sandbox"></a>Xbox Live 샌드박스 설정
다음 요청을 사용 하 여 장치에 대 한 Xbox Live 샌드박스를 변경할 수 있습니다. Xbox One에서는 설정이 적용 되기 전에 장치를 다시 시작 해야 합니다.

**요청**

다음 요청을 사용 하 여 장치의 Xbox Live sandbox에 대 한 현재 값을 설정할 수 있습니다.

방법      | 요청 URI
:------     | :-----
PUT | /ext/xboxlive/sandbox

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   
요청 본문은 다음 필드를 포함 하는 JSON 개체입니다.   
Sandbox-(문자열) 장치의 샌드박스를 설정할 새 값입니다.

**응답이**   
Sandbox-(문자열) 장치가 있는 현재 샌드박스입니다.   

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 파일 공유에 대 한 자격 증명에 대 한 액세스 요청이 부여 되었습니다.
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 장치 패밀리**

* Windows Xbox

