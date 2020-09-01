---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: AdScheduler 클래스를 사용 하 여 HTML로 JavaScript를 사용 하 여 작성 된 UWP (유니버설 Windows 플랫폼) 앱에서 비디오 콘텐츠에 광고를 표시 하는 방법을 알아봅니다.
title: 비디오 콘텐츠에 광고 표시
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 비디오, scheduler, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 6baf26b083cce08557a9b09f2ba95d5ad889f4a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175107"
---
# <a name="show-ads-in-video-content"></a>비디오 콘텐츠에 광고 표시

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 연습에서는 **Adscheduler** 클래스를 사용 하 여 HTML로 JavaScript를 사용 하 여 작성 된 UWP (유니버설 Windows 플랫폼) 앱에서 비디오 콘텐츠에 광고를 표시 하는 방법을 보여 줍니다.

> [!NOTE]
> 이 기능은 현재 HTML로 JavaScript를 사용 하 여 작성 된 UWP 앱에 대해서만 지원 됩니다.

**Adscheduler** 는 프로그레시브 미디어와 스트리밍 미디어 모두에서 작동 하며 IAB 표준 비디오 Ad 서비스 템플릿 (방대한) 2.0/3.0 및 vmap 페이로드 형식을 사용 합니다. **Adscheduler** 는 표준을 사용 하 여 상호 작용 하는 ad 서비스에 독립적입니다.

비디오 콘텐츠에 대 한 광고는 프로그램이 10 분 (약식) 인지 또는 10 분 이상 (긴 형식) 인지에 따라 달라 집니다. 후자는 서비스에 설정 하는 것이 더 복잡 하지만 실제로는 클라이언트 쪽 코드를 작성 하는 방법에는 차이가 없습니다. **Adscheduler** 는 매니페스트가 아닌 단일 광고를 사용 하 여 방대한 페이로드를 수신 하는 경우 단일 프리 롤오버 ad (00:00 시간에 한 번)에 대해 매니페스트가 호출 된 것 처럼 처리 됩니다.

## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2015 이상 릴리스를 사용 하 여 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 를 설치 합니다.

* 프로젝트에서 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 컨트롤을 사용 하 여 광고를 예약할 비디오 콘텐츠를 제공 해야 합니다. 이 컨트롤은 GitHub의 Microsoft에서 제공 하는 라이브러리의 [Tvhelpers](https://github.com/Microsoft/TVHelpers) 컬렉션에서 사용할 수 있습니다.

  다음 예제에서는 HTML 태그에서 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 를 선언 하는 방법을 보여 줍니다. 일반적으로이 태그는 `<body>` index.html 파일 (또는 프로젝트에 적합 한 다른 html 파일)의 섹션에 속합니다.

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  다음 예제에서는 JavaScript 코드에서 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 를 설정 하는 방법을 보여 줍니다.

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>코드에서 AdScheduler 클래스를 사용 하는 방법

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

2. 프로젝트가 **모든 CPU**를 대상으로 하는 경우 프로젝트를 업데이트 하 여 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 합니다. 프로젝트가 **모든 CPU**를 대상으로 하는 경우에는 다음 단계에서 Microsoft 광고 라이브러리에 대 한 참조를 성공적으로 추가할 수 없습니다. 자세한 내용은 [프로젝트의 모든 CPU를 대상으로 하 여 발생 한 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조 하세요.

3. **JavaScript 라이브러리에 대 한 MICROSOFT ADVERTISING SDK에 대 한** 참조를 프로젝트에 추가 합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    2. **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 후 **Microsoft Advertising SDK for JavaScript** (버전 10.0) 옆의 확인란을 선택 합니다.
    3. **참조 관리자**에서 확인을 클릭 합니다.

4.  프로젝트에 AdScheduler.js 파일을 추가 합니다.

    1. Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭 합니다.
    2. 검색 상자에 **StoreServices** 를 입력 하 고 StoreServices를 설치 합니다 .이 패키지는 microsoft. AdScheduler.js 파일이에 추가 됩니다. 프로젝트의/wjs 하위 디렉터리입니다.

5.  index.html 파일 (또는 프로젝트에 적합 한 다른 html 파일)을 엽니다. 섹션에서 `<head>` 프로젝트의 JavaScript 참조의 기본 .css 및 main.js 뒤에 ad.js 및 adscheduler.js에 대 한 참조를 추가 합니다.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > 이 줄 `<head>` 은 main.js 포함 후 섹션에 배치 해야 합니다. 그렇지 않으면 프로젝트를 빌드할 때 오류가 발생 합니다.

6.  프로젝트의 main.js 파일에서 새 **Adscheduler** 개체를 만드는 코드를 추가 합니다. 비디오 콘텐츠를 호스트 하는 **MediaPlayer** 를 전달 합니다. [WinJS](/previous-versions/windows/apps/hh440975)후에 실행 되도록 코드를 배치 해야 합니다.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  **Adscheduler** 개체의 **Requestschedule** 또는 **requestScheduleByUrl** 메서드를 사용 하 여 서버에서 ad 일정을 요청 하 고이를 **MediaPlayer** 타임 라인에 삽입 한 다음 비디오 미디어를 재생 합니다.

    * Microsoft ad 서버에서 ad 일정을 요청 하는 권한을 받은 Microsoft 파트너인 경우 **Requestschedule** 를 사용 하 고 microsoft 담당자가 제공한 응용 프로그램 id 및 AD 단위 id를 지정 합니다.

        이 [메서드는 두](../threading-async/asynchronous-programming-universal-windows-platform-apps.md#asynchronous-patterns-in-uwp-using-javascript)개의 함수 포인터가 전달 되는 비동기 구문으로, 약속이 성공적으로 완료 될 때 호출할 **onComplete** 함수에 대 한 포인터와 오류가 발생 한 경우 호출 하는 **onError** 함수에 대 한 포인터를 사용 합니다. **OnComplete** 함수에서 비디오 콘텐츠 재생을 시작 합니다. 예약 된 시간에 ad가 재생을 시작 합니다. **OnError** 함수에서 오류를 처리 한 다음 비디오 재생을 시작 합니다. 비디오 콘텐츠는 광고 없이 재생 됩니다. **OnError** 함수의 인수는 다음 멤버를 포함 하는 개체입니다.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * 타사 ad 서버에서 ad 일정을 요청 하려면 **requestScheduleByUrl**를 사용 하 고 서버 URI를 전달 합니다. 또한이 메서드는 **onComplete** 및 **onError** 함수에 대 한 포인터를 허용 하는 **약속** 의 형태를 사용 합니다. 서버에서 반환 되는 ad 페이로드는 비디오 광고 서비스 템플릿 (방대한) 또는 비디오 다중 Ad 재생 목록 (VMAP) 페이로드 형식을 준수 해야 합니다.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > **MediaPlayer**에서 기본 비디오 콘텐츠 재생을 시작 하기 전에 **Requestschedule** 또는 **requestScheduleByUrl** 가 반환 될 때까지 기다려야 합니다. 이전 롤오버 광고의 경우 **Requestschedule** 가 반환 되기 전에 미디어를 재생 하기 시작 하면 기본 비디오 콘텐츠가 미리 롤오버 됩니다. **Adscheduler** 는이 ad를 건너뛰고 바로 콘텐츠로 이동 하도록 **MediaPlayer** 에 지시 하기 때문에 함수가 실패 하더라도 **play** 를 호출 해야 합니다. 광고를 원격으로 가져올 수 없는 경우 기본 제공 광고를 삽입 하는 것과 같은 다른 비즈니스 요구 사항이 있을 수 있습니다.

8.  재생 하는 동안 앱에서 초기 ad 일치 프로세스 후 발생할 수 있는 진행률 및/또는 오류를 추적할 수 있는 추가 이벤트를 처리할 수 있습니다. 다음 코드에서는 **onPodStart**, **onPodEnd**, **onPodCountdown**, **Onadprogress**, **onAllComplete**및 **onadprogress**를 비롯 하 여 이러한 이벤트 중 일부를 보여 줍니다.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>AdScheduler 멤버

이 섹션에서는 **Adscheduler** 개체의 멤버에 대 한 몇 가지 세부 정보를 제공 합니다. 이러한 멤버에 대 한 자세한 내용은 프로젝트의 AdScheduler.js 파일에서 주석 및 정의를 참조 하세요.

### <a name="requestschedule"></a>requestSchedule

이 메서드는 Microsoft ad 서버에서 ad 일정을 요청 하 고 **Adscheduler** 생성자에 전달 된 **MediaPlayer** 의 타임 라인에 삽입 합니다.

선택적 세 번째 매개 변수 (*Adtags*)는 고급 대상이 있는 앱에 사용할 수 있는 이름/값 쌍의 JSON 컬렉션입니다. 예를 들어 다양 한 자동 관련 비디오를 재생 하는 앱은 표시 되는 자동차의 제조업체 및 모델과 함께 ad 유닛 ID를 보완할 수 있습니다. 이 매개 변수는 ad 태그를 사용 하기 위해 Microsoft에서 승인을 받는 파트너만 사용 하도록 설계 되었습니다.

*Adtags*를 참조할 때 다음 항목이 표시 되어야 합니다.

* 이 매개 변수는 거의 사용 되지 않는 옵션입니다. AdTags를 사용 하기 전에 게시자가 Microsoft와 긴밀 하 게 작업 해야 합니다.
* Ad 서비스에서 이름과 값을 모두 미리 결정 해야 합니다. Ad 태그가 열려 있지 않습니다. 검색 용어 또는 키워드가 열려 있지 않습니다.
* 지원 되는 최대 태그 수는 10 개입니다.
* 태그 이름은 16 자로 제한 됩니다.
* 태그 값의 최대 크기는 128 자입니다.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

이 메서드는 URI에 지정 된 타사 ad 서버에서 ad 일정을 요청 하 고 **Adscheduler** 생성자에 전달 된 **MediaPlayer** 의 타임 라인에 삽입 합니다. Ad 서버에서 반환 되는 ad 페이로드는 비디오 Ad 서비스 템플릿 (방대한) 또는 비디오 다중 Ad 재생 목록 (VMAP) 페이로드 형식을 준수 해야 합니다.

### <a name="mediatimeout"></a>mediaTimeout

이 속성은 미디어를 재생할 수 있는 시간 (밀리초)을 가져오거나 설정 합니다. 값이 0 이면 시스템에 시간 제한이 없음을 알립니다. 기본값은 3만 밀리초 (30 초)입니다.

### <a name="playskippedmedia"></a>playSkippedMedia

이 속성은 사용자가 예약 된 시작 시간을 지난 시점까지 건너뛴 경우 예약 된 미디어를 재생할지 여부를 나타내는 **부울** 값을 가져오거나 설정 합니다.

Ad 클라이언트 및 미디어 플레이어는 기본 비디오 콘텐츠를 신속 하 게 전달 하 고 되감기를 수행 하는 동안 광고에 발생 하는 상황에 따라 규칙을 적용 합니다. 대부분의 경우 앱 개발자는 보급 알림이 완전히 생략 되는 것을 허용 하지 않지만 사용자에 게 적절 한 환경을 제공 합니다. 다음 두 옵션은 대부분의 개발자의 요구 사항에 포함 됩니다.

1. 최종 사용자가 pod에서 보급 알림을 건너뛸 수 있습니다.
2. 사용자가 보급 알림 pod를 건너뛰고 재생을 다시 시작할 때 최근 pod를 재생할 수 있습니다.

**PlaySkippedMedia** 속성의 조건은 다음과 같습니다.

* 보급 알림이 시작 된 후에는 보급 알림을 건너뛰거나 빠르게 전달할 수 없습니다.
* Pod가 시작 되 면 광고 pod의 모든 광고가 재생 됩니다.
* 재생 한 후에는 기본 콘텐츠 (영화, 에피소드 등) 중에 알림이 다시 재생 되지 않습니다. 보급 알림 표식은 재생 또는 제거 됨으로 표시 됩니다.
* 출시 전 알림은 건너뛸 수 없습니다.

광고가 포함 된 콘텐츠를 다시 시작 하는 경우 **playSkippedMedia** 을 **false** 로 설정 하 여 사전 롤오버를 건너뛰고 최신 광고를 재생 하지 않도록 합니다. 그런 다음 콘텐츠가 시작 된 후 **playSkippedMedia** 를 **true** 로 설정 하 여 사용자가 후속 광고를 빠르게 전달할 수 없도록 합니다.

> [!NOTE]
> Pod는 상업적 중단 중에 실행 되는 광고 그룹과 같이 특정 순서로 재생 되는 광고 그룹입니다. 자세한 내용은 IAB Digital Video Ad 서비스 템플릿 (방대한) 사양을 참조 하세요.

### <a name="requesttimeout"></a>requestTimeout

이 속성은 제한 시간이 초과 되기 전에 ad 요청 응답을 대기 하는 시간 (밀리초)을 가져오거나 설정 합니다. 값이 0 이면 시스템에 시간 제한이 없음을 알립니다. 기본값은 3만 밀리초 (30 초)입니다.

### <a name="schedule"></a>schedule

이 속성은 ad 서버에서 검색 된 일정 데이터를 가져옵니다. 이 개체는 비디오 Ad 서비스 템플릿 (방대한) 또는 비디오 다중 Ad 재생 목록 (VMAP) 페이로드의 구조에 해당 하는 데이터의 전체 계층 구조를 포함 합니다.

### <a name="onadprogress"></a>onAdProgress  

이 이벤트는 ad 재생이 분 단위로 검사점에 도달할 때 발생 합니다. 이벤트 처리기 (*eventInfo*)의 두 번째 매개 변수는 다음 멤버를 포함 하는 JSON 개체입니다.

* **진행률**: ad 재생 상태 (AdScheduler.js에 정의 된 **mediaprogress** 열거형 값 중 하나)입니다.
* **clip**: 재생 중인 비디오 클립입니다. 이 개체는 코드에서 사용 하기에 적합 하지 않습니다.
* **Adpackage**: 재생 중인 광고에 해당 하는 ad 페이로드의 일부를 나타내는 개체입니다. 이 개체는 코드에서 사용 하기에 적합 하지 않습니다.

### <a name="onallcomplete"></a>onAllComplete  

이 이벤트는 주 콘텐츠가 끝에 도달 하 고 예약 된 후 롤링 광고도 종료 될 때 발생 합니다.

### <a name="onerroroccurred"></a>onErrorOccurred  

이 이벤트는 **Adscheduler** 에 오류가 발생할 때 발생 합니다. 오류 코드 값에 대 한 자세한 내용은 [ErrorCode](/uwp/api/microsoft.advertising.errorcode)를 참조 하십시오.

### <a name="onpodcountdown"></a>onPodCountdown

이 이벤트는 광고가 재생 중일 때 발생 하며 현재 pod에 남아 있는 시간을 나타냅니다. 이벤트 처리기의 두 번째 매개 변수 (*eventData*)는 다음 멤버를 포함 하는 JSON 개체입니다.

* **remainingAdTime**: 현재 광고에 대해 남아 있는 시간 (초)입니다.
* **remainingPodTime**: 현재 pod에 대해 남아 있는 시간 (초)입니다.

> [!NOTE]
> Pod는 상업적 중단 중에 실행 되는 광고 그룹과 같이 특정 순서로 재생 되는 광고 그룹입니다. 자세한 내용은 IAB Digital Video Ad 서비스 템플릿 (방대한) 사양을 참조 하세요.

### <a name="onpodend"></a>onPodEnd  

이 이벤트는 ad pod가 종료 될 때 발생 합니다. 이벤트 처리기의 두 번째 매개 변수 (*eventData*)는 다음 멤버를 포함 하는 JSON 개체입니다.

* **startTime**: pod의 시작 시간 (초)입니다.
* **pod**: pod를 나타내는 개체입니다. 이 개체는 코드에서 사용 하기에 적합 하지 않습니다.

### <a name="onpodstart"></a>onPodStart

이 이벤트는 ad pod가 시작 될 때 발생 합니다. 이벤트 처리기의 두 번째 매개 변수 (*eventData*)는 다음 멤버를 포함 하는 JSON 개체입니다.

* **startTime**: pod의 시작 시간 (초)입니다.
* **pod**: pod를 나타내는 개체입니다. 이 개체는 코드에서 사용 하기에 적합 하지 않습니다.