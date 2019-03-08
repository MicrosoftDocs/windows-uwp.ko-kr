---
title: Xbox Live 추적 분석기
description: Xbox Live 추적 분석기 서비스 호출을 제목 검토를 사용 하는 방법에 알아봅니다.
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 서비스 호출, 테스트, 분석기를 추적 합니다.
ms.localizationpriority: medium
ms.openlocfilehash: fa8ca37842edfbeaab0063cd953f3a34358a82da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623468"
---
# <a name="xbox-live-trace-analyzer"></a>Xbox Live 추적 분석기

Xbox Live Services API에는 모든 서비스 호출을 캡처하고 호출 패턴에 위반에 대 한 분석 하는 오프 라인 상태로 전환 하려면 제목 개발자를 이제 수 있습니다. 고급 시나리오에 대 한 프로토콜 활성화를 통해 또는 xbtrace 명령줄 도구에서 사용할 수 있는 새 기능을 사용 하 여 서비스 호출 추적을 활성화할 수 있습니다. 활성화 제목 코드에서 직접 추적 하는 서비스 호출도 지원 됩니다. 오프 라인 분석 도구, Xbox Live 추적 분석기 (XBLTraceAnalyzer.exe) 호출에서 Xbox Live 도구 패키지의 일부로 찾을 수 있습니다 [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)합니다.


## <a name="gather-logs-and-analyze-the-service-calls"></a>로그 수집 및 분석 서비스 호출

다음 단계를 Xbox Live 추적 분석기를 사용 하 여 분석 및 서비스 호출 레코드를 포함 하는 로그를 수집 해야 합니다.

1.  Xbox Live 서비스 API에는 2015 년 7 월에에서 포함 된 또는 Xbox 개발자 키트 (XDK)의 최신 버전의 버전을 사용 하 여 제목을 빌드하십시오.
2.  제목 아래에 설명 된 대로 추적을 사용 하도록 수정 합니다.
3.  제목에 배포 합니다.
4.  제목 시작 하 고 Xbox Live 서비스는 Xbox Live 서비스 API를 초기화 하기 위해 하나 이상의 호출을 확인 합니다.
5.  분석 하려는 지점을 제목에 추적을 시작 합니다.
6.  추적을 중지 합니다.
7.  개발 PC의 Xbox Live 추적 분석기 도구를 실행 하 고 출력을 확인 합니다.

## <a name="starting-and-stopping-tracing"></a>시작 및 중지 추적

시작 및 추적을 중지 하는 방법은 세 가지가 있습니다.

1.  Xbox Live 서비스 Api 집합을 제목에서 직접 호출할 수 있습니다.
2.  사용할 수는 *xbtrace* 명령줄 도구입니다.
3.  통해 정품 인증 프로토콜을 사용할 수는 *응용 프로그램 관리 (xbapp.exe)* 명령줄 도구입니다.


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>시작 및 중지 추적을 제목에서 직접

제목에서 직접 추적을 시작 하려면 다음을 수행 해야 합니다.

1.  `Microsoft::Xbox::Services::Experimental` 네임 스페이스를 설정 합니다 `EnableServiceCallTracking` 의 속성은 `ServiceCallTrackerSettings` true 클래스.
2.  호출 `StartServiceCallTracking()` 서비스 호출 추적을 시작 합니다.
3.  호출 `StopServiceCallTracking()` 서비스 호출 추적을 중지 합니다.
4.  추적 중지 된 후 결과 추적 파일에서 개발자 콘솔에서 임시 드라이브 다시 복사할 PC 중 하나를 사용 하 여 *파일 복사 (xbcp.exe)* 또는 *Xbox 하나의 환경이* 를 순서 대로 Xbox Live 추적 분석기를 사용 하 여 분석 합니다.

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>시작 및 중지 추적 xbtrace 명령줄 도구를 사용 하 여

추적을 시작 하는 가장 편리 하 고 간단한 방법은 xboxliveservices 추적 형식을 사용 하 여 xbtrace 명령줄 도구를 사용 하는 것입니다. XbTrace를 사용 하면 결과 추적 파일 수에 대 한 사용자 PC에 다시 복사 됩니다.

시작 및 중지 추적 xbtrace를 사용 하 여 정품 인증 프로토콜에 의존 합니다. Xbtrace 시작 및 중지 추적을 사용 하기 전에 호출 하 여 프로토콜 활성화를 초기화 해야 합니다는 `RegisterForProtcolActivation` 메서드는 `ServiceCallTrackerSettings` 클래스입니다.

다음 예제에서는 시작 xbTrace를 사용 하 여 Xbox Live 서비스 추적을 중지 하는 방법을 보여 줍니다.

    xbtrace start xboxliveservices
    xbtrace stop


반드시을 제목 실행 중 이어야 합니다 하 고 프로토콜 활성화 xbtrace를 사용 하 여 추적을 중지 및 시작 하기 전에 초기화 해야 합니다. 추적 중지 된 후 xbtrace 개발 PC에 추적 파일을 복사 하 고 이름이 "xbtrace" 및 타임 스탬프를 포함 하는 디렉터리에 배치 합니다. 이 디렉터리의 이름을 사용 하 여 재정의할 수 있습니다 \[etlfile\] xbtrace 하는 옵션입니다.

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>시작 및 중지 추적 프로토콜 활성화를 사용 하 여
----------------------------------------------------------
또한 "xbApp 시작" 프로토콜 활성화 기능을 사용 하 여 추적을 제어할 수 있습니다. 시작 및 정품 인증 프로토콜을 통해 추적을 중지 하 여 제목 titleid를 알고 있어야 합니다. 타이틀의 매니페스트 파일의 제목 id를 찾을 수 있습니다. 추적 "serviceCallTracking" 매개 변수를 포함 하는 Uri를 통해 제어 됩니다. 다음 예에서는 시작 하 고 제목 id가 12345678 제목에 대 한 추적을 중지 하는 방법을 보여 줍니다.

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

프로토콜 활성화를 사용 하는 경우 결과 추적 파일을 콘솔에 개발자 임시 드라이브에 저장 됩니다. Xbcp 또는 Xbox 하나의 환경 중 하나를 사용 하 여 PC로 다시 파일을 복사 해야 합니다. 파일 복사 되지 않습니다 자동으로 PC를 다시 이므로 xbtrace를 사용 하는 경우.

프로토콜 활성화를 사용 하는 세부 정보 표시와 같은 추가 추적 매개 변수를 설정할 수 있습니다. 네 가지 수준의 자세한 정도 지: quiet, 진단, 자세한 및 최소입니다. 다음 예제에서는 세부 정보 표시 수준을 설정 하는 방법을 보여 줍니다.

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>추적 파일 분석

추적 파일을 PC로 다시 복사한 후에 Xbox Live 서비스의 타이틀의 사용량을 분석할 GNDP에서 Xbox Live 추적 분석기 도구를 사용할 수 있습니다. 이 도구를 호출 하 고 해당 출력을 해석 하는 방법에 대 한 게임 Developer Network에서 Xbox Live 추적 분석기 도구에 포함 된 설명서를 참조 하십시오. -명령줄 옵션을 사용 하 여 XBLTraceAnalyzer.exe 실행할 수도 있습니다. 또는 명령줄 도움말을 보려면-h입니다.
