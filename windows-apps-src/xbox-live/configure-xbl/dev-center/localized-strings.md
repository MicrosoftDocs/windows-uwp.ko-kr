---
title: 지역화 문자열
author: shrutimundra
description: Windows 개발자 센터에서 문자열을 지역화 하는 방법을 알아봅니다
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.author: kevinasg
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 게임, uwp, windows 10, Xbox one, 지역화 된 문자열, Windows 개발자 센터
ms.openlocfilehash: fac642adad099bf930a4ddabba151384a83db17c
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5171783"
---
# <a name="configuring-localized-strings-on-windows-dev-center"></a>Windows 개발자 센터에서 지역화 된 문자열을 구성

이 페이지를 사용 하 여 게임을 지 원하는 모든 언어에 모든 Xbox Live 구성 지역화 하 수 있습니다. 모든 후속 Xbox Live 페이지에서 만든 서비스 구성 파일을 다운로드 하는 추가 됩니다.

게임와 관련 된 모든 언어로 지역화 된 문자열을 구성 하려면 [Windows 개발자 센터](https://developer.microsoft.com/dashboard) 를 사용할 수 있습니다. 다음을 수행 하 여 구성을 추가 합니다.

1. **서비스**아래의 타이틀에 대 한 **지역화 된 문자열** 섹션으로 이동 > **Xbox Live** > **지역화 된 문자열**입니다.
2. 로컬 컴퓨터에서 localization.xml 파일을 다운로드 하는 **다운로드** 단추를 클릭 합니다.

![개발자 센터에서 지역화 된 문자열 구성 페이지의 스크린샷](../../images/dev-center/localized-strings/localized-strings-1.png)

3. 복제 하 여 지역화 된 문자열을 추가할 수는 <Value locale="en-US">재생 maze</Value> 태그 및 선택한 언어로 지역화 된 문자열의 값을 로캘 값을 변경 합니다. 최소 하나의 값 태그 개발자 내에서 오류가 발생 하지 않도록 로캘 표시 있어야 합니다.

![지역화 된 문자열 편집](../../images/dev-center/localized-strings/localized-strings.gif)

4. 모든 지역화 된 문자열을 추가한 후 끌거나 로컬 컴퓨터를 검색 하 여 파일을 업로드할 수 있습니다.

![Localization.xml 파일을 업로드 하는 단추 이미지](../../images/dev-center/localized-strings/localized-strings-2.png)

Localization.xml 파일을 업로드 하는 경우 다음과 같은 오류가 발생할 note 하세요.

| 오류 | 이유 |
|---------------------------|-------------|
| 실패 한 XSD 유효성 검사: 요소 'LocalizedString' 네임 스페이스에 'http://config.mgt.xboxlive.com/schema/localization/1' 텍스트를 포함할 수 없습니다. 필요한 요소 목록: '값' 네임 스페이스에 'http://config.mgt.xboxlive.com/schema/localization/1' | XML 문서는 잘못 된 형식의 때 발생 |
| 지역화 문자열 개발자 디스플레이 로캘에 대 한 항목이 없습니다. | 지역화 된 문자열은 해당 로캘 개발자 디스플레이 로캘와 일치 하지 않는 항목 누락 된 경우이 |
| 실패 한 XSD 유효성 검사: '로캘' 특성이 유효 하지-값 ' '유효 하지는 데이터 형식에 따라'http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString'-The 패턴 제약 조건에 실패 했습니다. | 지역화 된 문자열의 로캘 값이 누락 되어 때 발생 합니다 <Value> 태그|