---
title: 디바이스 포털 배포 정보 API 참조
description: 프로그래밍 방식으로 배포 정보 API에 액세스하는 방법을 알아봅니다.
ms.localizationpriority: medium
ms.openlocfilehash: c44089313b100880b419e9b55a26101e877496f3
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8198465"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>설치된 하나 이상의 패키지에 대한 배포 정보를 요청합니다.

**요청**

메서드      | 요청 URI
:------     | :------
POST | /ext/app/deployinfo
<br />
**URI 매개 변수**

 - 없음

**요청 헤더**

- 없음

**요청 본문**

다음과 같은 형식의 JSON 배열입니다.

* DeployInfo
  * PackageFullName - 관련 정보를 요청하는 패키지의 이름입니다.
  * OverlayFolder - 이 기능을 사용하는 경우 오버레이 폴더 경로에 대한 선택적 경로입니다.

###<a name="response"></a>응답

**응답 본문**

다음과 같은 형식의 JSON 배열입니다(일부 필드는 옵션).

* DeployInfo
  * PackageFullName - 관련 정보를 수신하는 패키지의 이름입니다.
  * DeployType - 배포 유형입니다.
  * DeployPathOrSpecifiers - 느슨한 배포의 배포 경로 또는 패키지 배포의 설치된 지정자입니다.
  * DeployDrive - 해당 배포 유형에 대해 패키지가 배포되는 드라이브입니다.
  * DeploySizeInBytes - 해당 배포 형식에 대한 패키지의 바이트 크기입니다.
  * OverlayFolder - 이 기능을 지원하는 배포에 대한 오버레이 폴더입니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 성공
4XX | 오류 코드
5XX | 오류 코드
<br />

**사용 가능한 디바이스 패밀리**

* Windows Xbox