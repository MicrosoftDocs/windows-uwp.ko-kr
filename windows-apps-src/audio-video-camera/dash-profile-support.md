---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: "이 문서는 UWP 앱에 대해 지원되는 DASH(Dynamic Adaptive Streaming over HTTP) 프로필을 나열합니다."
title: "DASH(Dynamic Adaptive Streaming over HTTP) 프로필 지원"
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9ca8f2d1783a73ae38fed1d3ee1e58b809ddd2aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>DASH(Dynamic Adaptive Streaming over HTTP) 프로필 지원
다음 표는 UWP 앱에서 지원되는 DASH 프로필을 보여 줍니다.



|태그 | 매니페스트 유형 | 참고|Windows 10 7월 릴리스|Windows 10 버전 1511|Windows 10 버전 1607 |Windows 10 버전 1607 |Windows 10 버전 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg:dash:profile:isoff-live:2011 | 정적 |     |지원            |  지원              | 지원        |지원| 지원|
|urn:mpeg:dash:profile:isoff-main:2011 |        | 최상 | 지원            |  지원              | 지원        |지원| 지원|
|urn:mpeg:dash:profile:isoff-live:2011 | 동적 | 세그먼트 템플릿에서 $Time$은 지원되지만 $Number$는 지원되지 않음 | 지원되지 않음            | 지원되지 않음              | 지원되지 않음        |지원되지 않음| 지원|


## <a name="related-topics"></a>관련 항목

* [미디어 재생](media-playback.md)
* [적응 스트리밍](adaptive-streaming.md)
 

 




