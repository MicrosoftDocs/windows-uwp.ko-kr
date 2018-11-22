---
author: laurenhughes
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP 앱 스트리밍 설치
description: UWP(유니버설 Windows 플랫폼) 앱 스트리밍 설치는 Microsoft Store에서 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있도록 해줍니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, uwp, 스트리밍 설치, uwp 앱 스트리밍 설치
ms.localizationpriority: medium
ms.openlocfilehash: e4915d2fb4d1133cd190d766d38c79934d9f3956
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7576557"
---
# <a name="uwp-app-streaming-install"></a>UWP 앱 스트리밍 설치
UWP(유니버설 Windows 플랫폼) 앱 스트리밍 설치는 Microsoft Store에서 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있도록 해줍니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다. 

UWP 앱 스트리밍 설치를 사용 하 여 앱의 파일 섹션으로 나눌 해야 합니다. 이렇게 하려면 만들어야 앱과 함께 패키지 되는 XML 파일 콘텐츠 그룹 맵, 설정 다운로드 우선 순위 및 순서 수 있도록 합니다. 자세한 내용은 아래 링크 항목을 참조 하세요.

UWP 앱을 UWP 앱 스트리밍 설치를 추가 하는 방법에 전체 가이드에 대 한이 [블로그 시리즈](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)를 확인 합니다.

| 항목 | 설명 | 
|-------|-------------|
| [소스 콘텐츠 그룹 맵 생성 및 변환](create-cgm.md) | 유니버설 Windows 플랫폼(UWP) 앱을 UWP 앱 스트리밍 설치에 사용하도록 준비하려면 콘텐츠 그룹 맵을 생성해야 합니다. 이 문서는 콘텐츠 그룹 맵을 생성 및 변환하는 구체적인 방법을 알려주는 한편, 몇 가지 팁과 요령을 제공합니다. |