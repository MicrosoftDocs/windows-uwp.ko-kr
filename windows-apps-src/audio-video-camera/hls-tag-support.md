---
author: drewbatgit
ms.assetid: 
description: "이 문서에서는 UWP 앱에서 지원되는 HLS(HTTP 라이브 스트리밍) 프로토콜 태그를 보여 줍니다."
title: "HLS(HTTP 라이브 스트리밍) 태그 지원"
translationtype: Human Translation
ms.sourcegitcommit: 3d61f5272e4d11acfb7e0a85436ca60ba458dcae
ms.openlocfilehash: a561f11a1638d5fea21d1d3b3f8bc47f71271f3f

---

# HLS(HTTP 라이브 스트리밍) 태그 지원
다음 표에서는 UWP 앱에서 지원되는 HLS 태그를 보여 줍니다.

> [!NOTE] 
> [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md) 문서에 설명된 대로 "X-"로 시작하는 사용자 지정 태그는 시간이 제한된 메타데이터로 액세스할 수 있습니다.

|태그 |도입된 HLS 프로토콜 버전|HLS 프로토콜 문서 초안 버전|필수인 클라이언트|Windows 10 7월 릴리스|Windows 10 버전 1511|Windows 10 버전 1606 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  기본 태그                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|필수|지원함|지원|지원함|
| 4.3.1.2.  EXT-X-VERSION |2|3|필수|지원함|지원|지원함
|4.3.2.  미디어 세그먼트 태그                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|필수|지원함|지원|지원함
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|옵션|지원함|지원|지원함|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|옵션|지원함|지원|지원함|
| 4.3.2.4.  EXT-X-KEY |1|0|옵션|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp; METHOD|1|0|특성|"NONE, AES-128"|"NONE, AES-128"|"NONE, AES-128, SAMPLE-AES"|
|&nbsp;&nbsp;&nbsp; URI|1|0|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp; IV|2|3|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.2.5.  EXT-X-MAP |5|9|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp; URI|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
|4.3.3.  미디어 재생 목록 태그                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|필수|지원함|지원|지원함|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|옵션|지원함|지원|지원함|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|옵션|지원함|지원|지원함|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|옵션|지원함|지원|지원함|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
|4.3.4.  마스터 재생 목록 태그                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|옵션|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  TYPE|4|7|특성|"AUDIO, VIDEO"|"AUDIO, VIDEO"|"AUDIO, VIDEO, SUBTITLES"|
|&nbsp;&nbsp;&nbsp;  URI|4|7|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  GROUP-ID|4|7|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  언어|4|7|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  NAME|4|7|특성|지원되지 않음|지원되지 않음|지원함|
|&nbsp;&nbsp;&nbsp;  DEFAULT|4|7|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  AUTOSELECT|4|7|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  FORCED|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  CHARACTERISTICS|5|9|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|필수|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  BANDWIDTH|1|0|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  PROGRAM-ID|1|0|특성|NA|미지원|NA|
|&nbsp;&nbsp;&nbsp;  AVERAGE-BANDWIDTH|7|14|특성|지원되지 않음|지원되지 않음|지원되지 않음|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  RESOLUTION|2|3|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  FRAME-RATE|7|15|특성|NA|미지원|NA|
|&nbsp;&nbsp;&nbsp;  AUDIO|4|7|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  비디오|4|7|특성|지원함|지원|지원함|
|&nbsp;&nbsp;&nbsp;  SUBTITLES|5|9|특성|지원되지 않음|지원되지 않음|지원함|
|&nbsp;&nbsp;&nbsp;  CLOSED-CAPTIONS|6|12|특성|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|옵션|지원되지 않음|지원되지 않음|지원되지 않음|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|옵션|지원되지 않음|지원되지 않음|지원되지 않음|




## 관련 항목

* [미디어 재생](media-playback.md)
* [적응 스트리밍](adaptive-streaming.md)
 

 







<!--HONumber=Nov16_HO1-->


