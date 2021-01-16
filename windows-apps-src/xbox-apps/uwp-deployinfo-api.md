---
title: 장치 포털 배포 정보 API 참조
description: 하나 이상의 설치 된 패키지에 대 한 배포 정보를 요청 하는 데 Xbox Device 포털 REST API deployinfo를 사용 하는 방법을 알아봅니다.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2819b21e12d25aca941808e1feeb8a7539750a91
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254228"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>하나 이상의 설치 된 패키지에 대 한 배포 정보를 요청 합니다.

**요청**

| 방법 | 요청 URI |
|--------|-------------|
| POST | /ext/app/deployinfo |

**URI 매개 변수**

 - 없음

**요청 헤더**

- 없음

**요청 본문**

다음 형식의 JSON 배열입니다.

* DeployInfo
  * PackageFullName-정보를 요청 하는 패키지의 이름입니다.
  * OverlayFolder-이 기능을 사용 하는 경우 오버레이 폴더 경로에 대 한 선택적 경로입니다.

### <a name="response"></a>응답

**응답 본문**

다음 형식의 JSON 배열 (일부 필드는 선택 사항)입니다.

* DeployInfo
  * PackageFullName-정보를 수신 중인 패키지의 이름입니다.
  * DeployType-배포의 유형입니다.
  * DeployPathOrSpecifiers-느슨한 배포 또는 패키지 배포에 대해 설치 된 지정자에 대 한 배포 경로입니다.
  * DeployDrive-적용 가능한 배포 유형을 위해 패키지가 배포 되는 드라이브입니다.
  * DeploySizeInBytes-적용 가능한 배포 유형에 대 한 패키지의 크기 (바이트)입니다.
  * OverlayFolder-이 기능을 지 원하는 배포에 대 한 오버레이 폴더입니다.

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

| HTTP 상태 코드 | Description |
|------------------|-------------|
| 200 | Success |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

**사용 가능한 장치 패밀리**

* Windows Xbox