---
title: 지역화 문자열
description: 파트너 센터의 문자열을 지역화 하는 방법 알아보기
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 11/17/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 게임, uwp, windows 10, Xbox, 지역화 된 문자열을 파트너 센터
ms.openlocfilehash: 127f566dc5ae57b920d396623b6a84ff5d5eed96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656598"
---
# <a name="configuring-localized-strings-in-partner-center"></a>파트너 센터에서 지역화 된 문자열을 구성합니다.

게임을 지 원하는 모든 언어에 모든 Xbox Live 구성을 지역화 하려면이 페이지를 사용할 수 있습니다. 모든 후속 Xbox Live 페이지에서 만든 서비스 구성을 다운로드 한 파일에 추가 됩니다.

사용할 수 있습니다 [파트너 센터](https://partner.microsoft.com/dashboard) 게임와 연결 된 모든 언어로 지역화 된 문자열을 구성 합니다. 다음을 수행 하 여 구성을 추가 합니다.

1. 로 이동 합니다 **지역화 된 문자열** 아래에 있는 제목으로 섹션 **Services** > **Xbox Live** > **지역화 문자열**합니다.
2. 클릭 합니다 **다운로드** 단추 localization.xml 파일이 로컬 컴퓨터에 다운로드 됩니다.

![파트너 센터에서 지역화 된 문자열 구성 페이지의 스크린샷](../../images/dev-center/localized-strings/localized-strings-1.png)

3. 복제 하 여 지역화 된 문자열을 추가할 수는 <Value locale="en-US">Mazes 재생</Value> 태그 및 원하는 언어의 지역화 된 문자열의 값을 로캘 값을 변경 합니다. 오류를 방지 하려면 개발자 표시 로캘 내의 태그 값이 하나 이상 있어야 합니다.

![지역화 된 문자열 편집](../../images/dev-center/localized-strings/localized-strings.gif)

4. 모든 지역화 된 문자열을 추가한 후에 끌거나 로컬 컴퓨터를 검색 하 여 파일을 업로드할 수 있습니다.

![Localization.xml 파일을 업로드 하는 단추의 이미지](../../images/dev-center/localized-strings/localized-strings-2.png)

Localization.xml 파일을 업로드할 때 다음 오류가 표시 될 수 있습니다 note 하십시오.

| 오류 | 이유 |
|---------------------------|-------------|
| 실패 한 XSD 유효성 검사: 네임 스페이스의 'LocalizedString' 요소의 'http://config.mgt.xboxlive.com/schema/localization/1' 텍스트를 포함할 수 없습니다. 필요한 요소 목록: 'Value' 네임 스페이스의 'http://config.mgt.xboxlive.com/schema/localization/1' | 이 경우에 XML 문서 형식이 잘못 되었습니다. |
| 지역화 문자열에 개발자 표시 로캘에 대 한 항목이 없습니다. | 지역화 된 문자열 로캘이 영어인 개발 표시 로캘이 일치 하지 않는 항목이 없는 경우 |
| 실패 한 XSD 유효성 검사: '로캘' 특성이 잘못 되었습니다. 값 ' '올바르지의 데이터 타입'http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString'-패턴 제약 조건이 실패 했습니다. | 이 지역화 된 문자열에서 로캘 값이 없는 경우에 발생 합니다 <Value> 태그|
