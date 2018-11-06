---
author: WilliamsJason
title: 디바이스 포털 느슨한 폴더 등록 API 참조
description: 프로그래밍 방식으로 느슨한 폴더 등록 API에 액세스하는 방법을 알아봅니다.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: efdf4214-9738-4df6-bf1f-ed7141696ef6
ms.localizationpriority: medium
ms.openlocfilehash: cb80e2dbd7ebdfbb05bd642b9875a9cd7cc356f3
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6041262"
---
# <a name="register-an-app-in-a-loose-folder"></a>느슨한 폴더에 앱 등록  

**요청**

다음 요청 형식을 사용하여 느슨한 폴더에 앱을 등록할 수 있습니다.

메서드      | 요청 URI
:------     | :------
POST | /api/app/packagemanager/register
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수      | 설명
:------     | :-----
폴더(필수) | 등록할 패키지의 대상 폴더 이름입니다. 이 폴더는 콘솔에서 d:\developmentfiles\LooseApps 아래에 있어야 합니다. 폴더가 LooseApps 아래의 하위 폴더인 경우 경로 구분 기호를 포함할 수 있으므로 이 폴더 이름은 base64로 인코딩되어야 합니다.
<br />

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 수락되어 처리 중인 요청 배포
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox

**노트**

콘솔의 원하는 폴더에 느슨한 앱을 가져오는 세 가지 이상의 방법이 있습니다. 가장 쉬운 방법은 SMB를 통해 파일을 \\&lt;IP_Address&gt;\DevelopmentFiles\LooseApps로 복사하는 것입니다. 이 경우 [/ext/smb/developerfolder](wdp-smb-api.md)를 통해 가져올 수 있는 UWA 키트에 사용자 이름과 암호가 있어야 합니다. 

두 번째 방법은 /api/filesystem/apps/file에 대해 POST를 수행하여 개별 파일을 올바른 위치로 복사하는 것입니다. 여기서 knownfolderid는 DevelopmentFiles이고, packagefullname은 비어 있고, 파일 이름 및 경로를 올바르게 제공해야 합니다(경로는 LooseApps로 시작해야 함).

세 번째 방법은 [/api/app/packagemanager/upload](wdp-folder-upload.md)를 통해 전체 폴더를 한 번에 복사하는 것입니다. destinationFolder는 d:\developmentfiles\looseapps 아래에 배치할 폴더의 이름이고 페이로드는 디렉터리 내용의 다중 파트 준수 http 본문입니다.

