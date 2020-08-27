---
title: 장치 포털 SMB API 참조
description: Xbox 장치 포털 REST API/ext/smb/developerfolder를 사용 하 여 파일 탐색기를 통해 Xbox One 콘솔의 개발자 폴더에 액세스 하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: 80a49d324c27754a2686ba4d954b47e7529df330
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970211"
---
# <a name="developer-folder-api-reference"></a>개발자 폴더 API 참조

표준 파일 탐색기를 사용 하 여 Xbox One에서 개발 관련 파일에 액세스할 수 있습니다. 이를 통해 PC에서 콘솔로 파일을 쉽게 보고 바꿀 수 있습니다.

**요청**

다음 요청을 사용 하 여 개발자 폴더에 액세스할 수 있습니다. 요청은 다음을 반환 합니다.

* 파일 공유의 위치입니다. 이 위치는 파일 탐색기의 주소 표시줄에 입력할 수 있습니다.
* 파일 공유에 액세스할 사용자 이름입니다.
* 파일 공유에 액세스 하는 데 사용할 암호입니다.

방법      | 요청 URI
:------     | :-----
GET | /ext/smb/developerfolder

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답이**   
Path-파일 개발자 파일 공유에 대 한 경로입니다.   
사용자 이름-개발자 파일 공유에 액세스 하는 데 필요한 사용자 이름입니다.   
암호-개발자 파일 공유에 액세스 하는 데 필요한 암호입니다.   

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | 파일 공유에 대 한 자격 증명에 대 한 액세스 요청이 부여 되었습니다.
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 장치 패밀리**

* Windows Xbox
