---
ms.assetid: 66a9cfe2-b212-4c73-8a36-963c33270099
description: 이 문서에서는 UWP 앱에서 지원되는 HLS(HTTP 라이브 스트리밍) 프로토콜 태그를 보여 줍니다.
title: HLS(HTTP 라이브 스트리밍) 태그 지원
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c6664d13e76a5774172094d632de9db25109fdc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613088"
---
# <a name="http-live-streaming-hls-tag-support"></a>HLS(HTTP 라이브 스트리밍) 태그 지원
다음 표에서는 UWP 앱에서 지원되는 HLS 태그를 보여 줍니다.

> [!NOTE] 
> [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md) 문서에 설명된 대로 "X-"로 시작하는 사용자 지정 태그는 시간이 제한된 메타데이터로 액세스할 수 있습니다.

|Tag |도입된 HLS 프로토콜 버전|HLS 프로토콜 문서 초안 버전|필수인 클라이언트|Windows 10 7월 릴리스|Windows 10 버전 1511|Windows 10 버전 1607 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  기본 태그                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|필수|지원함|지원함|지원함|
| 4.3.1.2.  EXT-X-VERSION |2|3|필수|지원함|지원함|지원함
|4.3.2.  미디어 세그먼트 태그                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|필수|지원함|지원함|지원함
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|옵션|지원함|지원함|지원함|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|옵션|지원함|지원함|지원함|
| 4.3.2.4.  EXT-X-KEY |1|0|옵션|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp; 메서드|1|0|특성|"NONE, AES-128"|"NONE, AES-128"|"NONE, AES-128, SAMPLE-AES"|
|&nbsp;&nbsp;&nbsp; URI|1|0|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp; IV|2|3|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.2.5.  EXT-X-MAP |5|9|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp; URI|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
|4.3.3.  미디어 재생 목록 태그                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|필수|지원함|지원함|지원함|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|옵션|지원함|지원함|지원함|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|옵션|지원함|지원함|지원함|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|옵션|지원함|지원함|지원함|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
|4.3.4.  마스터 재생 목록 태그                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|옵션|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  형식|4|7|특성|"AUDIO, VIDEO"|"AUDIO, VIDEO"|"AUDIO, VIDEO, SUBTITLES"|
|&nbsp;&nbsp;&nbsp;  URI|4|7|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  그룹 ID|4|7|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  언어|4|7|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  ASSOC 언어|6|13|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  이름|4|7|특성|지원되지 않음|지원되지 않음|지원함|
|&nbsp;&nbsp;&nbsp;  기본|4|7|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  자동 선택|4|7|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  강제 적용|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  INSTREAM ID|6|12|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  특성|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|필수|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  대역폭|1|0|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  프로그램 ID|1|0|특성|해당 없음|해당 없음|해당 없음|
|&nbsp;&nbsp;&nbsp;  평균 대역폭|7|14|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  코덱|1|0|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  해결 방법|2|3|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  프레임 속도|7|15|특성|해당 없음|해당 없음|해당 없음|
|&nbsp;&nbsp;&nbsp;  오디오|4|7|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  비디오|4|7|특성|지원함|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  자막|5|9|특성|지원되지 않음|지원되지 않음|지원함|
|&nbsp;&nbsp;&nbsp;  닫힌 캡션|6|12|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
|4.3.5.  미디어 또는 마스터 재생 목록 태그                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|옵션|지원되지 않음|지원함|지원함|
| 4.3.5.2.  EXT-X-START  |6|12|옵션|지원되지 않음|부분적으로 지원됨|부분적으로 지원됨|
|&nbsp;&nbsp;&nbsp;  시간 오프셋|6|12|특성|지원되지 않음|지원함|지원함|
|&nbsp;&nbsp;&nbsp;  정확한|6|12|특성|지원되지 않음|기본값 "아니요"로 지원됨|기본값 "아니요"로 지원됨|



## <a name="related-topics"></a>관련 항목

* [미디어 재생](media-playback.md)
* [적응 스트리밍](adaptive-streaming.md)
 

 




