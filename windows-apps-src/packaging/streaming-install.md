---
author: laurenhughes
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP 앱 스트리밍 설치
description: UWP(유니버설 Windows 플랫폼) 앱 스트리밍 설치는 Microsoft Store에서 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있도록 해줍니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, 설치, 스트리밍 uwp 스트리밍 uwp 응용 프로그램 설치
ms.localizationpriority: medium
ms.openlocfilehash: 087226cad4bcf7ea0294d8878564c345d6cfb9d0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "304850"
---
# <a name="uwp-app-streaming-install"></a>UWP 앱 스트리밍 설치
UWP(유니버설 Windows 플랫폼) 앱 스트리밍 설치는 Microsoft Store에서 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있도록 해줍니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다. 

응용 프로그램을 설치 스트리밍 UWP를 사용 하 여 앱의 파일 섹션으로 나누는 필요 합니다. 이 작업을 수행 만들게 앱으로 패키지화 하는 XML 파일을 하는 콘텐츠 그룹 맵 집합 다운로드 우선순위 및 주문 수 있게 해줍니다. 자세한 내용은 아래에 연결 된 항목을 참조 하십시오.

UWP 앱에 앱을 설치 스트리밍 UWP를 추가 하는 방법에 완전 한 설명서에 대 한이 [블로그 시리즈](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)를 체크아웃 합니다.

| 항목 | 설명 | 
|-------|-------------|
| [소스 콘텐츠 그룹 맵 생성 및 변환](create-cgm.md) | 유니버설 Windows 플랫폼(UWP) 앱을 UWP 앱 스트리밍 설치에 사용하도록 준비하려면 콘텐츠 그룹 맵을 생성해야 합니다. 이 문서는 콘텐츠 그룹 맵을 생성 및 변환하는 구체적인 방법을 알려주는 한편, 몇 가지 팁과 요령을 제공합니다. |