---
title: 디바이스 포털 폴더 업로드 API 참조
description: 프로그래밍 방식으로 폴더 업로드 API에 액세스하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 870d203271cb75ecf5531106bb2c10b3736db9b9
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244049"
---
# <a name="upload-a-folder-to-the-development-directory"></a>개발 디렉터리로 폴더 업로드

**요청**

DevelopmentFiles의 알려진 폴더 ID 또는 해당 폴더 내의 하위 폴더에 전체 폴더를 한 번에 업로드할 수 있습니다.

메서드      | 요청 URI
:------     | :------
올리기 | /api/app/packagemanager/upload 

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수      | 설명
:------     | :-----
destinationFolder(필수) | 업로드할 폴더의 대상 폴더 이름입니다. 이 폴더는 콘솔에서 d:\developmentfiles\LooseApps 아래에 배치됩니다. 폴더가 LooseApps 아래의 하위 폴더인 경우 경로 구분 기호를 포함할 수 있으므로 이 폴더 이름은 base64로 인코딩되어야 합니다.


**요청 헤더**

- 없음

**요청 본문**

- 디렉터리 내용의 다중 파트 준수 http 본문입니다.

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 성공
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 디바이스 패밀리**

* Windows Xbox

