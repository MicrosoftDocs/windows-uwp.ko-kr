---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: 이 문서는 UWP 앱에 대해 지원되는 DASH(Dynamic Adaptive Streaming over HTTP) 프로필을 나열합니다.
title: DASH(Dynamic Adaptive Streaming over HTTP) 프로필 지원
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a4ec9f9e81010d39af496da156afa676f4b3714
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6280079"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>DASH(Dynamic Adaptive Streaming over HTTP) 프로필 지원


## <a name="supported-dash-profiles"></a>지원되는 DASH 프로필
다음 표는 UWP 앱에서 지원되는 DASH 프로필을 보여 줍니다.

|태그 | 매니페스트 유형 | 참고|Windows 10 7월 릴리스|Windows 10 버전 1511|Windows 10 버전 1607 |Windows 10 버전 1607 |Windows 10 버전 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 정적 |     |지원            |  지원              | 지원        |지원| 지원|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | 최상 | 지원            |  지원              | 지원        |지원| 지원|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 동적 | 세그먼트 템플릿에서 $Time$은 지원되지만 $Number$는 지원되지 않음 | 지원되지 않음            | 지원되지 않음              | 지원되지 않음        |지원되지 않음| 지원|


## <a name="unsupported-dash-profiles"></a>지원되지 않는 DASH 프로필
위의 표에 나열되지 않은 프로필은 지원되지 않으며 다음을 포함하지만 이에 제한되지 않습니다.

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:isoff-on-demand:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>관련 항목

* [미디어 재생](media-playback.md)
* [적응 스트리밍](adaptive-streaming.md)
 

 




