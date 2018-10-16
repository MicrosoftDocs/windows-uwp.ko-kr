---
title: EDS 공통 헤더
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
author: KevinAsgari
description: " EDS 공통 헤더"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2452be9eacd5efe0b28229a14579e838e9b62d0e
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4618968"
---
# <a name="eds-common-headers"></a>EDS 공통 헤더

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>엔터테인먼트 검색 서비스 (EDS) 일반적인 요청 헤더

| 헤더 이름| 설명| 필수 여부| 메모|
| --- | --- | --- | --- |
| <b>x xbl-계약 버전</b>| EDS 서비스 버전| 예| 3.2|
| <b>x xbl-클라이언트 유형</b>| 클라이언트 유형 헤더| 예| 고유한 클라이언트 유형 가져오려면 팀에 게 말하십시오.|
| <b>x xbl-클라이언트 버전</b>| 클라이언트 버전| 예| 모든 문자열은 비어 있습니다.|
| <b>xbl 부모 ig x</b>| 인상 guid| 예| 로그에서 및 기타 서비스 호출에서 요청을 추적 하는 데 사용 합니다.|
| <b>x-xbl-장치-유형</b>| 장치 유형| 예| 장치는 클라이언트를 나타내는입니다.|
| <b>수락</b>| 유형을 적용합니다| 예| XML 또는 JSON 합니다.|
| <b>권한 부여</b>| 인증 헤더| 예|  |
| <b>x-xbl 자동 제안-시드-텍스트</b>| 자동 제안 시드 텍스트| 아니요| BI에 대 한 사용 및 관련성|
| <b>x-xbl--검색어</b>| 검색어| 아니요|  |
| <b>x-xbl-입력-메서드</b>| 사용자가 사용 되는 입력된 방법| 아니요| 컨트롤러, 음성, Kinect 합니다.|
| <b>x-xbl-kinect 사용</b>| Kinect 사용 하도록 설정| 아니요| 예/아니요.|
| <b>x-xbl-음성-세션-id</b>| 음성 세션 ID| 아니요| 여부 음성을 사용 하 여 세션을 시작 했습니다.|
| <b>x xbl-클라이언트 id</b>| 익명 클라이언트 Id| 아니요| BI 보고 및 관련성 사용 됩니다.|
| <b>x-xbl-장치-id</b>| 장치 ID입니다.| 아니요| BI 보고 및 관련성 사용 됩니다.|
| <b>x xbl-사용자 에이전트</b>| 클라이언트 사용자 에이전트| 아니요| BI에 사용 됩니다. "&lt;이름 > /&lt;버전 > (&lt;OS 버전 >; &lt;플랫폼 >; &lt;접근 권한 값 >; &lt;제조 >; &lt;모델 >) ".|
| <b>xbl 부모 ig x</b>| "Chained" 호출에 대 한 이전 인상을 Guid| 아니요 (이지만 권장)| BI 관련성에 대 한 중요 합니다. 예를 들어 찾아보기 호출의 IG 부모인 IG는 다음에 대 한 세부 정보 호출을 합니다.|
| <b>delid</b>| 대리자 Identity| 아니요| 내부 서비스에서 사용자를 대신 하 여 작동 하도록 하는 데 사용 합니다.|

## <a name="common-response-headers"></a>일반적인 응답 헤더

| 헤더 이름| 설명| 필수 여부| 메모|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>캐시</b>| 캐시 헤더| 예| 참고 <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9">http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9</a>.|
| <b>x-xbl-오류</b>| 오류| 아니요| 다양 한 데이터 공급자의 오류 목록입니다.|
| <b>x-xbl-traceid</b>| 로그에서 추적 Id| 예| 요청 특정 로그를 추적 하는 데 사용 합니다.|
| <b>x-서버 이름</b>| 난독 처리 된 이름 요청을 처리 하는 서버입니다.| 예| 내부 디버깅에 사용 됩니다.|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>자세한 정보

[마켓플레이스 URI](../uri/marketplace/atoc-reference-marketplace.md)
