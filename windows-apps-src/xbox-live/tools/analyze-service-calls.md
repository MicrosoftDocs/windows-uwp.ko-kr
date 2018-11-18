---
title: Xbox Live 추적 분석기
author: KevinAsgari
description: Xbox Live 추적 분석기를 사용 하 여 서비스 호출 타이틀을 검토 하는 방법을 알아봅니다.
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox 서비스 호출, 테스트, 추적 분석기
ms.localizationpriority: medium
ms.openlocfilehash: 0eaec6d830dece484d3300ea244f05bf2add5a60
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7158183"
---
# <a name="xbox-live-trace-analyzer"></a>Xbox Live 추적 분석기

Xbox Live 서비스 API에는 이제 제목 개발자를 모든 서비스 호출을 캡처하고 패턴 호출에서 모든 위반에 대 한 분석 하는 오프 수 있습니다. 고급 시나리오에 대 한 xbtrace 명령줄 도구 또는 프로토콜 활성화를 통해 사용할 수 있는 새로운 기능을 사용 하 여 서비스 호출 추적을 활성화할 수 있습니다. 서비스 호출 제목 코드에서 직접 추적을 활성화 에서도 지원 됩니다. Xbox Live 추적 분석기 (XBLTraceAnalyzer.exe) 라는 오프 라인 분석 도구에서 Xbox Live 도구 패키지의 일부로 찾을 수 있습니다 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools).


## <a name="gather-logs-and-analyze-the-service-calls"></a>로그를 수집 하 고 서비스를 호출 하 여 분석

다음 단계는 서비스 호출의 레코드를 포함 하 고 분석 하는 Xbox Live 추적 분석기를 사용 하 여 로그를 수집 해야 합니다.

1.  Xbox Live 서비스 API 2015 년 7 월에에서 포함 된 또는 Xbox 개발자 키트 (XDK)의 최신 버전의 버전을 사용 하 여 타이틀을 빌드하십시오.
2.  아래 설명 된 대로 추적을 활성화 하려면 제목을 수정 합니다.
3.  타이틀을 배포 합니다.
4.  제목을 시작 하 고 Xbox Live 서비스 API를 초기화 하기 위해 Xbox Live 서비스를 하나 이상 호출 합니다.
5.  타이틀을 분석 하려면 지점에서 추적을 시작 합니다.
6.  추적을 중지 합니다.
7.  개발 PC에서 Xbox Live 추적 분석기 도구를 실행 하 고 출력을 확인 합니다.

## <a name="starting-and-stopping-tracing"></a>시작 및 중지 추적

시작 하 고 추적을 중지 하는 방법은 세 가지가 있습니다.

1.  Xbox Live 서비스 Api 집합을 타이틀에서 직접 호출할 수 있습니다.
2.  *Xbtrace* 명령줄 도구를 사용할 수 있습니다.
3.  *응용 프로그램 관리 (xbapp.exe)* 명령줄 도구를 통해 프로토콜 활성화를 사용할 수 있습니다.


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>시작 및 중지 추적 타이틀에서 직접

타이틀에서 직접 추적을 시작 하려면 다음을 수행 해야 합니다.

1.  `Microsoft::Xbox::Services::Experimental` 네임 스페이스를 설정 합니다 `EnableServiceCallTracking` 의 속성은 `ServiceCallTrackerSettings` 클래스를 true로 합니다.
2.  호출 `StartServiceCallTracking()` 서비스 호출을 추적을 시작 합니다.
3.  호출 `StopServiceCallTracking()` 서비스 호출을 추적을 중지 합니다.
4.  추적을 중지 한 후 결과 추적 파일에서에서 복사 콘솔에서 개발자 임시 드라이브를 PC에 Xbox Live 추적 분석기를 사용 하 여 분석 하기 위해 *파일 복사 (xbcp.exe)* 또는 *Xbox One 주변* 을 사용 하 여 합니다.

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>시작 및 중지 추적 xbtrace 명령줄 도구를 사용 하 여

추적을 시작 하는 가장 편리 하 고 간편한 방법은 xboxliveservices 추적 형식을 가진 xbtrace 명령줄 도구를 사용 하는 것입니다. XbTrace를 사용 하면 결과 추적 파일 수를 PC에 복사 됩니다.

시작 및 중지 xbtrace를 사용 하 여 추적 프로토콜 활성화에 의존 합니다. Xbtrace 시작 하 고 추적을 중지를 사용 하기 전에 호출 하 여 프로토콜 활성화를 초기화 해야 합니다 `RegisterForProtcolActivation` 메서드는 `ServiceCallTrackerSettings` 클래스.

다음 예제에서는 시작 및 xbTrace를 사용 하 여 Xbox Live 서비스 추적을 중지 하는 방법을 보여 줍니다.

    xbtrace start xboxliveservices
    xbtrace stop


타이틀을 실행 해야 하 고 시작 하 고 xbtrace를 사용 하 여 추적을 중지 하기 전에 프로토콜 활성화를 초기화 합니다 기억 하세요. 추적을 중지 한 후 xbtrace 개발 PC에 추적 파일을 복사 하 고 이름이 "xbtrace" 및 타임 스탬프를 포함 한 디렉터리에 배치 합니다. \[Etlfile\를 사용 하 여이 디렉터리의 이름을 재정의할 수] xbtrace 하는 옵션입니다.

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>시작 및 중지 추적 프로토콜 활성화를 사용 하 여
----------------------------------------------------------

또한 "xbApp launch"의 프로토콜 활성화 기능을 사용 하 여 추적을 제어할 수 있습니다. 시작 및 프로토콜 활성화를 통해 추적을 중지 하 여 제목 titleid를 알고 있어야 합니다. 제목 id 타이틀의 매니페스트 파일에서 찾을 수 있습니다. 추적 "serviceCallTracking" 매개 변수를 포함 하는 Uri를 통해 제어 됩니다. 다음 예제에서는 시작 및 제목 id가 12345678 제목에 대 한 추적을 중지 하는 방법을 보여 줍니다.

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

프로토콜 활성화를 사용 하면 결과 추적 파일 콘솔에서 개발자 임시 드라이브에 저장 됩니다. Xbcp 또는 Xbox One 환경을 사용 하 여 PC를 다시 파일을 복사 해야 합니다. 파일 복사 되지 않습니다 자동으로 PC를 다시 이므로 xbtrace를 사용 하는 경우.

프로토콜 활성화를 사용 하는 세부 정보 표시 등의 추가 추적 매개 변수를 설정할 수 있습니다. 네 가지 수준의 세부 정보는 지원: 자동, 진단, 자세한 및 최소화 합니다. 다음 예제에서는 세부 정보 표시 수준을 설정 하는 방법을 보여 줍니다.

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>추적 파일 분석

PC에 다시 추적 파일을 복사한 후 Xbox Live 서비스의 타이틀의 사용을 분석 하려면 GNDP에서 Xbox Live 추적 분석기 도구를 사용할 수 있습니다. 도구를 호출 하 고 출력을 해석 하는 방법에 대 한 설명에 대 한 게임 개발자 네트워크에서 Xbox Live 추적 분석기 도구와 함께 제공 되는 설명서를 참조 하세요. 또한의 명령줄 옵션을 사용 하 여 XBLTraceAnalyzer.exe를 실행할 수 있습니까? 또는-h 명령줄 도움말을 볼 수 있습니다.
