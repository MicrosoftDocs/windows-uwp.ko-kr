---
title: 장치 포털 폴더 업로드 API 참조
description: Xbox 장치 포털 REST API를 사용 하 여 개발 디렉터리에 폴더를 업로드 하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: b71f60350bf5c8318adb2a4741bb1a275a4b0276
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157757"
---
# <a name="upload-a-folder-to-the-development-directory"></a>개발 디렉터리에 폴더 업로드

**요청**

한 번에 전체 폴더를 DevelopmentFiles에 대 한 알려진 폴더 Id (또는 해당 폴더의 하위 폴더)에 업로드할 수 있습니다.

방법      | 요청 URI
:------     | :------
POST | /api/app/packagemanager/upload 

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수      | Description
:------     | :-----
destinationFolder (필수) | 업로드할 폴더의 대상 폴더 이름입니다. 이 폴더는 콘솔의 d:\developmentfiles\LooseApps 아래에 배치 됩니다. 이 폴더 이름은 LooseApps 아래의 하위 폴더인 경우 경로 구분 기호를 포함할 수 있으므로 b a s e 64로 인코딩해야 합니다.


**요청 헤더**

- 없음

**요청 본문**

- 여러 부분으로 구성 된 디렉터리 콘텐츠의 http 본문입니다.

**Response**

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드      | Description
:------     | :-----
200 | Success
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 장치 패밀리**

* Windows Xbox

