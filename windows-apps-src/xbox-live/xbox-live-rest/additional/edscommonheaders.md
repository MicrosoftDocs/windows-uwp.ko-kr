---
title: EDS 공통 헤더
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
description: " EDS 공통 헤더"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a85383ee75fa51cb8376451ac955dcc4fa2edf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630708"
---
# <a name="eds-common-headers"></a>EDS 공통 헤더

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>엔터테인먼트 검색 서비스 (EDS) 일반적인 요청 헤더

| 헤더 이름| 설명| 필수 여부| 참고|
| --- | --- | --- | --- |
| <b>x-xbl-contract-version</b>| EDS 서비스 버전| 예| 3.2|
| <b>x-xbl-client-type</b>| 클라이언트 형식 헤더| 예| 사용자 고유의 클라이언트 형식을 가져올 수는 팀에 게 문의 합니다.|
| <b>x-xbl-client-version</b>| Client 버전| 예| 모든 비어 있지 않은 문자열입니다.|
| <b>x-xbl-parent-ig</b>| 인상 guid| 예| 로그에 다른 서비스 호출에서 요청을 추적 하는 데 사용 합니다.|
| <b>x-xbl-device-type</b>| 장치 유형| 예| 장치 클라이언트를 나타내는입니다.|
| <b>수락</b>| 형식을 수락합니다| 예| XML 또는 JSON입니다.|
| <b>권한 부여</b>| 인증 헤더| 예|  |
| <b>x-xbl-autoSuggest-seed-text</b>| 자동 시드 텍스트 제안| 아니요| BI에 대 한 사용 및 관련성|
| <b>x-xbl-search-term</b>| 검색 용어| 아니요|  |
| <b>x-xbl-input-method</b>| 사용자가 사용 되는 입력된 메서드| 아니요| 컨트롤러에서 음성 Kinect 합니다.|
| <b>x-xbl-kinect-enabled</b>| Kinect를 사용 하도록 설정| 아니요| 예/아니요.|
| <b>x-xbl-speech-session-id</b>| 음성 세션 ID| 아니요| 여부 음성을 사용 하 여 세션을 시작 했습니다.|
| <b>x-xbl-client-id</b>| 익명 클라이언트 Id| 아니요| BI 보고 및 관련성 사용 됩니다.|
| <b>x-xbl-device-id</b>| 장치 ID| 아니요| BI 보고 및 관련성 사용 됩니다.|
| <b>x-xbl-user-agent</b>| 클라이언트 사용자 에이전트| 아니요| BI에 사용 됩니다. "&lt;이름 > /&lt;버전 > (&lt;OS 버전 >; &lt;플랫폼 >; &lt;기능 >; &lt;제조 >; &lt;모델 >) ".|
| <b>x-xbl-parent-ig</b>| "Chained" 호출에 대 한 이전 인상을 Guid| 아니요 (이지만 권장 사항임)| BI 관련성에 대 한 정보가 중요 합니다. 예를 들어 찾아보기 호출의 무시 부모인 무시는 다음에 대 한 세부 정보 호출을 합니다.|
| <b>delid</b>| Id 위임| 아니요| 사용자를 대신 하 여 작업에 내부 서비스에서 사용 합니다.|

## <a name="common-response-headers"></a>일반적인 응답 헤더

| 헤더 이름| 설명| 필수 여부| 참고|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>Cache</b>| 캐시 헤더| 예| 참조 <a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9"> http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9 </a>합니다.|
| <b>x-xbl-errors</b>| 오류| 아니요| 다양 한 데이터 공급자에서 발생 한 오류 목록입니다.|
| <b>x-xbl-traceid</b>| 로그에서 추적 Id| 예| 특정 로그 요청을 추적 하는 데 사용 합니다.|
| <b>x-server-name</b>| 요청을 처리 하는 서버의 난독 처리 된 이름입니다.| 예| 내부 디버깅에 사용 합니다.|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>추가 정보

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)
