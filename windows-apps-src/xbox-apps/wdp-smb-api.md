---
author: payzer
title: 디바이스 포털 SMB API 참조
description: 프로그래밍 방식으로 SMB API에 액세스하는 방법을 알아봅니다.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: 2a337fe722d73a08c1c75a84478fc31e5bdf6b03
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993421"
---
# <a name="developer-folder-api-reference"></a>개발자 폴더 API 참조   
표준 파일 탐색기를 사용하여 Xbox One의 개발 관련 파일에 액세스할 수 있습니다. 이렇게 하면 PC에서 쉽게 파일을 보고 콘솔로 이동할 수 있습니다.

**요청**

다음 요청을 사용하여 개발자 폴더에 액세스할 수 있습니다. 요청은 다음을 반환합니다.    
* 파일 공유 위치. 이 위치를 파일 탐색기의 주소 표시줄에 입력할 수 있습니다.
* 파일 공유에 액세스하는 사용자 이름
* 파일 공유에 액세스하는 암호

메서드      | 요청 URI
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**   
경로 - 개발자 파일에서 공유하는 파일 경로입니다.   
사용자 이름 - 개발자 파일 공유에 액세스하는 데 필요한 사용자 이름입니다.   
암호 - 개발자 파일 공유에 액세스하는 데 필요한 암호입니다.   

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
