---
author: Xansky
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: AdScheduler 클래스를 사용하여 비디오 콘텐츠에 광고를 표시하는 방법을 알아봅니다.
title: 비디오 콘텐츠에 광고 표시
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 비디오, 스케줄러, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 158817aa0abea1ddb1247188ec69389a7682e899
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6674614"
---
# <a name="show-ads-in-video-content"></a>비디오 콘텐츠에 광고 표시

이 연습에서는 HTML과 함께 JavaScript를 사용하여 작성된 UWP(유니버설 Windows 플랫폼) 앱에서 **AdScheduler** 클래스를 사용하여 비디오 콘텐츠에 광고를 표시하는 방법을 보여 줍니다.

> [!NOTE]
> 이 기능은 현재 HTML과 함께 JavaScript를 사용하여 작성된 UWP 앱에만 지원됩니다.

**AdScheduler**는 프로그레시브 및 스트리밍 미디어에서 모두 작동하며 IAB 표준인 VAST(Video Ad Serving Template) 2.0/3.0과 VMAP 페이로드 형식을 사용합니다. 이러한 표준이 적용되는 **AdScheduler**는 상호 작용하는 광고 서비스에 독립적입니다.

비디오 콘텐츠의 광고는 프로그램이 10분 이내(짧은 양식)인지 또는 10분을 초과(긴 양식)하는지에 따라 다릅니다. 후자는 서비스에 설정하기가 더 복잡하지만 실제로는 클라이언트 쪽 코드를 작성하는 방법에서 차이가 없습니다. **AdScheduler**가 매니페스트 대신에 단일 광고가 있는 VAST 페이로드를 받는 경우 매니페스트에서 단일 프리롤 광고를 요구한 것처럼 처리됩니다(00:00 시점에 한 번 중단).

## <a name="prerequisites"></a>필수 조건

* Visual Studio 2015 이상 릴리스와 함께 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 설치합니다.

* 프로젝트는 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 컨트롤을 사용하여 광고가 예약되는 비디오 콘텐츠를 제공해야 합니다. 이 컨트롤은 GitHub의 Microsoft에서 사용할 수 있는 라이브러리의 [TVHelpers](https://github.com/Microsoft/TVHelpers) 컬렉션에서 사용할 수 있습니다.

  다음 예제는 HTML 태그로 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)를 선언하는 방법을 보여 줍니다. 일반적으로 이 태그는 index.html 파일(또는 프로젝트에 해당하는 다른 html 파일)의 `<body>` 섹션에 속합니다.

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  다음 예제에서는 JavaScript 코드로 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)를 설정하는 방법을 보여 줍니다.

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>코드에서 AdScheduler 클래스를 사용하는 방법

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising 라이브러리에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3. 프로젝트에 **Microsoft Advertising SDK for JavaScript** 라이브러리에 대한 참조를 추가합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가**를 선택합니다.
    2. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for JavaScript**(버전 10.0) 옆의 확인란을 선택합니다.
    3. **참조 관리자**에서 확인을 클릭합니다.

4.  AdScheduler.js 파일을 프로젝트에 추가합니다.

    1. Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭합니다.
    2. 검색 상자에 **Microsoft.StoreServices.VideoAdScheduler**를 입력하고 Microsoft.StoreServices.VideoAdScheduler 패키지를 설치합니다. AdScheduler.js 파일이 프로젝트의 ../js 하위 디렉터리에 추가됩니다.

5.  index.html 파일(또는 프로젝트에 해당하는 다른 html 파일)을 엽니다. `<head>` 섹션에서 default.css 및 main.js에 대한 프로젝트의 JavaScript 참조 다음에 ad.js 및 adscheduler.js에 대한 참조를 추가합니다.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > 이 줄은 `<head>` 섹션에서 main.js를 포함한 후에 추가해야 합니다. 그러지 않으면 프로젝트를 빌드할 때 오류가 발생합니다.

6.  프로젝트의 main.js 파일에 새 **AdScheduler** 개체를 만드는 코드를 추가합니다. 비디오 콘텐츠를 호스트하는 **MediaPlayer**를 전달합니다. 이 코드는 [WinJS.UI.processAll](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh440975) 후에 실행되도록 배치해야 합니다.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  **AdScheduler** 개체의 **requestSchedule** 또는 **requestScheduleByUrl** 메서드를 사용하여 서버에서 광고 예약을 요청하고 이를 **MediaPlayer** 타임라인에 삽입한 다음 비디오 미디어를 재생합니다.

    * Microsoft 광고 서버에서 광고 예약을 요청하는 권한을 받은 Microsoft 파트너인 경우 **requestSchedule**을 사용하고 Microsoft 담당자가 제공한 응용 프로그램 ID와 광고 단위 ID를 지정합니다.

        이 메서드는 [프라미스](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript) 양식을 사용합니다. 이는 프라미스가 완료될 때 호출되는 **onComplete** 함수에 대한 포인터 및 오류 발생 시 호출되는 **onError** 함수에 대한 포인터가 전달되는 비동기 구조입니다. **onComplete** 함수에서 비디오 콘텐츠 재생을 시작합니다. 광고는 예약된 시간에 재생을 시작합니다. **onError** 함수에서는 오류를 처리한 다음 비디오 재생을 시작합니다. 광고 없이 비디오 콘텐츠가 재생됩니다. **onError** 함수의 인수는 다음 멤버가 포함된 개체입니다.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * Microsoft가 아닌 타사 광고 서버에서 광고 예약을 요청하려면 **requestScheduleByUrl**을 사용하고 서버 URI를 전달합니다. 이 메서드는 **onComplete** 및 **onError** 함수의 포인터에 액세스하는 **프라미스** 양식을 사용합니다. 서버에서 반환되는 광고 페이로드는 VAST(Video Ad Serving Template) 또는 VMAP(Video Multiple Ad Playlist) 페이로드 형식을 준수해야 합니다.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > **MediaPlayer**에서 기본 비디오 콘텐츠 재생을 시작하기 전에 **requestSchedule** 또는 **requestScheduleByUrl**이 반환되기를 기다려야 합니다. **requestSchedule**이 반환되기 전에 미디어 재생을 시작하면(프리롤 광고의 경우) 프리롤이 기본 비디오 콘텐츠를 인터럽트합니다. **AdScheduler**는 광고를 건너뛰고 바로 콘텐츠로 이동하도록 **MediaPlayer**에 알려주므로 함수가 실패할 경우에도 **play**를 호출해야 합니다. 광고를 원격으로 가져올 수 없는 경우 기본 제공 광고를 삽입하는 등 다른 비즈니스 요구 사항이 있을 수 있습니다.

8.  재생하는 동안 앱 트랙을 진행시키거나 초기 광고 일치 프로세스 후 발생할 수 있는 오류에 대한 추가 이벤트를 처리할 수 있습니다. 다음 코드에서는 **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** 및 **onErrorOccurred** 등 이러한 이벤트 중 일부를 보여 줍니다.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>AdScheduler 멤버

이 섹션에서는 **AdScheduler** 개체의 멤버에 대한 세부 정보를 제공합니다. 이러한 멤버에 대한 자세한 내용은 프로젝트의 AdScheduler.js 파일에 있는 설명과 정의를 참조하세요.

### <a name="requestschedule"></a>requestSchedule

이 메서드는 Microsoft 광고 서버에서 광고 일정을 요청하여 이를 **AdScheduler** 생성자에 전달된 **MediaPlayer**의 타임라인에 삽입합니다.

선택적인 세 번째 매개 변수(*adTags*)는 고급 대상 지정 기능이 있는 앱에 사용할 수 있는 이름/값 쌍의 JSON 컬렉션입니다. 예를 들어 다양한 자동 관련 비디오를 재생하는 앱은 표시되는 자동차의 제조업체 및 모델로 광고 단위 ID를 보완할 수 있습니다. 이 매개 변수는 Microsoft로부터 광고 태그를 사용하도록 승인 받은 파트너만 사용할 수 있습니다.

*adTags*를 참조할 때 다음 항목을 주의해야 합니다.

* 이 매개 변수는 매우 드물게 사용되는 옵션입니다. 게시자는 adTags를 사용하기 전에 Microsoft와 긴밀하게 협력해야 합니다.
* 이름과 값은 둘 다 광고 서비스에서 미리 결정되어야 합니다. 광고 태그는 개방형 검색어나 키워드가 아닙니다.
* 지원되는 최대 태그 수는 10개입니다.
* 태그 이름은 16자로 제한됩니다.
* 태그 값은 최대 128자입니다.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

이 메서드는 URI에 지정된 Microsoft가 아닌 광고 서버에서 광고 일정을 요청하여 이를 **AdScheduler** 생성자에 전달된 **MediaPlayer**의 타임라인에 삽입합니다. 광고 서버에서 반환되는 광고 페이로드는 VAST(Video Ad Serving Template) 또는 VMAP(Video Multiple Ad Playlist) 페이로드 형식을 준수해야 합니다.

### <a name="mediatimeout"></a>mediaTimeout

이 속성은 미디어를 재생할 수 있어야 하는 시간(밀리초)을 가져오거나 설정합니다. 값 0은 시스템이 절대 시간 초과되지 않음을 나타냅니다. 기본값은 30000밀리초(30초)입니다.

### <a name="playskippedmedia"></a>playSkippedMedia

이 속성은 사용자가 예약된 시작 시간이 지난 시점으로 건너뛸 경우 예약된 미디어를 재생할지 여부를 표시하는 **부울** 값을 가져오거나 설정합니다.

광고 클라이언트와 미디어 플레이어는 기본 비디오 콘텐츠의 빨리 감기 및 되감기를 하는 동안 광고에서 이루어지는 활동에 대한 규칙을 강제 적용합니다. 대부분의 경우 앱 개발자는 광고를 완전히 건너뛰도록 허용하지 않지만 사용자에게 적절한 환경을 제공하려고 합니다. 다음 두 옵션은 대부분의 개발자가 필요로 하는 것입니다.

1. 최종 사용자가 원하는 대로 광고 포드를 건너뛸 수 있습니다.
2. 사용자가 광고 포드를 건너뛸 수 있지만 재생을 다시 시작하면 최근 포드를 재생합니다.

**playSkippedMedia** 속성에는 다음과 같은 조건이 있습니다.

* 광고가 시작되면 광고를 건너뛸 수 없거나 빨리 감기를 할 수 없습니다.
* 포드가 시작되면 광고 포드의 모든 광고가 재생됩니다.
* 광고가 재생되면 기본 콘텐츠(영화, 에피소드 등)가 재생되는 동안에는 다시 재생되지 않습니다. 광고 마커가 재생됨 또는 제거됨으로 표시됩니다.
* 프리롤 광고는 건너뛸 수 없습니다.

광고가 포함된 콘텐츠를 다시 시작하는 경우 **playSkippedMedia**를 **false**로 설정하여 프리롤을 건너뛰고 최근 광고가 재생을 중단하지 않도록 합니다. 그런 다음 콘텐츠가 시작된 후에 **playSkippedMedia**를 **true**로 설정하여 사용자가 후속 광고를 빨리 감지 못하도록 합니다.

> [!NOTE]
> 포드는 중간 광고 시간 동안 재생되는 광고 그룹처럼 순서대로 재생되는 광고 그룹입니다. 자세한 내용은 IAB 디지털 VAST(Video Ad Serving Template) 사양을 참조하세요.

### <a name="requesttimeout"></a>requestTimeout

이 속성은 시간 초과되기 전에 요청 응답을 기다리는 시간(밀리초)을 가져오거나 설정합니다. 값 0은 시스템이 절대 시간 초과되지 않음을 나타냅니다. 기본값은 30000밀리초(30초)입니다.

### <a name="schedule"></a>schedule

이 속성은 광고 서버에서 검색된 일정 데이터를 가져옵니다. 이 개체에는 VAST(Video Ad Serving Template) 또는 VMAP(Video Multiple Ad Playlist) 페이로드 구조에 해당하는 데이터의 전체 계층이 포함되어 있습니다.

### <a name="onadprogress"></a>onAdProgress  

재생이 사분위 검사점에 도달하면 이 이벤트가 발생합니다. 이 이벤트 처리기(*eventInfo*)의 두 번째 매개 변수는 다음 멤버가 포함된 JSON 개체입니다.

* **progress**: 광고 재생 상태(AdScheduler.js에 정의된 **MediaProgress** 열거형 값 중 하나)입니다.
* **clip**: 재생되는 비디오 클립입니다. 이 개체는 사용자 코드에서 사용되지 않습니다.
* **adPackage**: 재생되는 광고에 해당하는 광고 페이로드의 일부를 나타내는 개체입니다. 이 개체는 사용자 코드에서 사용되지 않습니다.

### <a name="onallcomplete"></a>onAllComplete  

이 이벤트는 메인 콘텐츠가 끝에 도달하고 예약된 포스트롤 광고도 종료될 때 발생합니다.

### <a name="onerroroccurred"></a>onErrorOccurred  

이 이벤트는 **AdScheduler**에 오류가 생성될 때 발생합니다. 오류 코드 값에 대한 자세한 내용은 [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.errorcode)를 참조하세요.

### <a name="onpodcountdown"></a>onPodCountdown

이 이벤트는 광고가 재생되는 중에 발생하며 현재 포드에 남아 있는 시간을 표시합니다. 이 이벤트 처리기(*eventData*)의 두 번째 매개 변수는 다음 멤버가 포함된 JSON 개체입니다.

* **remainingAdTime**: 현재 광고의 남아 있는 시간(초)입니다.
* **remainingPodTime**: 현재 포드의 남아 있는 시간(초)입니다.

> [!NOTE]
> 포드는 중간 광고 시간 동안 재생되는 광고 그룹처럼 순서대로 재생되는 광고 그룹입니다. 자세한 내용은 IAB 디지털 VAST(Video Ad Serving Template) 사양을 참조하세요.

### <a name="onpodend"></a>onPodEnd  

이 이벤트는 광고 포드가 종료될 때 발생합니다. 이 이벤트 처리기(*eventData*)의 두 번째 매개 변수는 다음 멤버가 포함된 JSON 개체입니다.

* **startTime**: 포드의 시작 시간(초)입니다.
* **pod**: 포드를 나타내는 개체입니다. 이 개체는 사용자 코드에서 사용되지 않습니다.

### <a name="onpodstart"></a>onPodStart

이 이벤트는 포드가 시작될 때 발생합니다. 이 이벤트 처리기(*eventData*)의 두 번째 매개 변수는 다음 멤버가 포함된 JSON 개체입니다.

* **startTime**: 포드의 시작 시간(초)입니다.
* **pod**: 포드를 나타내는 개체입니다. 이 개체는 사용자 코드에서 사용되지 않습니다.
