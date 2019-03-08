---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: UWP 앱 스트리밍 설치
description: UWP(유니버설 Windows 플랫폼) 앱 스트리밍 설치는 Microsoft Store에서 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있도록 해줍니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, uwp, 스트리밍 설치, uwp 앱 스트리밍 설치
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608658"
---
# <a name="uwp-app-streaming-install"></a>UWP 앱 스트리밍 설치
UWP(유니버설 Windows 플랫폼) 앱 스트리밍 설치는 Microsoft Store에서 먼저 다운로드하고 싶은 앱의 부분을 지정할 수 있도록 해줍니다. 앱의 기본 파일이 먼저 다운로드되면, 백그라운드에서 나머지 부분이 완전히 다운로드되는 동안 사용자는 앱을 실행하고 상호 작용할 수 있습니다. 

UWP 앱 스트리밍 설치를 사용하려면 앱의 파일을 섹션으로 나눠야 합니다. 이를 위해 다운로드 우선 순위와 순서를 설정할 수 있도록 앱과 패키지로 제공되는 XML 파일인 콘텐츠 그룹 앱을 생성합니다. 자세한 내용은 아래에 연결된 항목을 참조하세요.

UWP 앱에 UWP 앱 스트리밍 설치를 추가하는 방법에 대한 전체 설명서는 이 [블로그 시리즈](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)를 확인합니다.

| 항목 | 설명 | 
|-------|-------------|
| [생성 한 원본 콘텐츠 그룹 맵을 변환합니다](create-cgm.md) | 유니버설 Windows 플랫폼(UWP) 앱을 UWP 앱 스트리밍 설치에 사용하도록 준비하려면 콘텐츠 그룹 맵을 생성해야 합니다. 이 문서는 콘텐츠 그룹 맵을 생성 및 변환하는 구체적인 방법을 알려주는 한편, 몇 가지 팁과 요령을 제공합니다. |