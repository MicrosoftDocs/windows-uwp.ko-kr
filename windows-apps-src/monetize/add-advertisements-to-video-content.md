---
author: mcleanbyron
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: "AdScheduler 클래스를 사용하여 비디오 콘텐츠에 광고를 추가하는 방법을 알아봅니다."
title: "비디오 콘텐츠에 광고 추가"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, 비디오, 스케줄러, javascript"
ms.openlocfilehash: 88e0bb4ceb9cba12d1eb5857761f5b59afaa15f2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="add-advertisements-to-video-content"></a>비디오 콘텐츠에 광고 추가


이 연습에서는 HTML과 함께 JavaScript를 사용하여 작성된 UWP(유니버설 Windows 플랫폼) 앱에서 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 클래스를 사용하여 비디오 콘텐츠에 광고를 추가하는 방법을 보여 줍니다.

>**참고**&nbsp;&nbsp;이 기능은 현재 HTML과 함께 JavaScript를 사용하여 작성된 UWP 앱에만 지원됩니다.

[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx)는 프로그레시브 및 스트리밍 미디어에서 모두 작동하며 IAB 표준인 VAST(Video Ad Serving Template) 2.0/3.0과 VMAP 페이로드 형식을 사용합니다. 이러한 표준이 적용되는 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx)는 상호 작용하는 광고 서비스에 독립적입니다.

비디오 콘텐츠의 광고는 프로그램이 10분 이내(짧은 양식)인지 또는 10분을 초과(긴 양식)하는지에 따라 다릅니다. 후자는 서비스에 설정하기가 더 복잡하지만 실제로는 클라이언트 쪽 코드를 작성하는 방법에서 차이가 없습니다. [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx)가 매니페스트 대신에 단일 광고가 있는 VAST 페이로드를 받는 경우 매니페스트에서 단일 프리롤 광고를 요구한 것처럼 처리됩니다(00:00 시점에 한 번 중단).

## <a name="prerequisites"></a>필수 조건

* Visual Studio 2015와 함께 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)를 설치합니다.

* 프로젝트는 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 컨트롤을 사용하여 광고가 예약되는 비디오 콘텐츠를 제공해야 합니다. 이 컨트롤은 GitHub의 Microsoft에서 사용할 수 있는 라이브러리의 [TVHelpers](https://github.com/Microsoft/TVHelpers) 컬렉션에서 사용할 수 있습니다.

  다음 예제는 HTML 태그로 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)를 선언하는 방법을 보여 줍니다. 일반적으로 이 태그는 index.html 파일(또는 프로젝트에 해당하는 다른 html 파일)의 `<body>` 섹션에 속합니다.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  다음 예제에서는 JavaScript 코드로 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)를 설정하는 방법을 보여 줍니다.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>코드에서 AdScheduler 클래스를 사용하는 방법

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising 라이브러리에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3. 프로젝트에 **Microsoft Advertising SDK for JavaScript** 라이브러리에 대한 참조를 추가합니다.

  a. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...**를 선택합니다.

  b. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for JavaScript**(버전 10.0) 옆의 확인란을 선택합니다.

  c. **참조 관리자**에서 확인을 클릭합니다.

4.  AdScheduler.js 파일을 프로젝트에 추가합니다.

  a.  Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭합니다.

  b.  검색 상자에 **Microsoft.StoreServices.VideoAdScheduler**를 입력하고 Microsoft.StoreServices.VideoAdScheduler 패키지를 설치합니다. AdScheduler.js 파일이 프로젝트의 ../js 하위 디렉터리에 추가됩니다.

5.  index.html 파일(또는 프로젝트에 해당하는 다른 html 파일)을 엽니다. `<head>` 섹션에서 default.css 및 main.js에 대한 프로젝트의 JavaScript 참조 다음에 ad.js 및 adscheduler.js에 대한 참조를 추가합니다.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  <script src="/js/adscheduler.js"></script>
  ```

  <span/>
  > **참고**&nbsp;&nbsp;이 줄은 `<head>` 섹션에서 main.js를 포함한 후에 추가해야 합니다. 그러지 않으면 프로젝트를 빌드할 때 오류가 발생합니다.

6.  프로젝트의 main.js 파일에 새 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 개체를 만드는 코드를 추가합니다. 비디오 콘텐츠를 호스트하는 **MediaPlayer**를 전달합니다. 이 코드는 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/hh440975.aspx) 후에 실행되도록 배치해야 합니다.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) 또는 [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) 메서드를 사용하여 서버에서 광고 예약을 요청하고 이를 **MediaPlayer** 타임라인에 삽입한 다음 비디오 미디어를 재생합니다.

  * Microsoft 광고 서버에서 광고 예약을 요청하는 권한을 받은 Microsoft 파트너인 경우 [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx)을 사용하고 Microsoft 담당자가 제공한 응용 프로그램 ID와 광고 단위 ID를 지정합니다. 이 메서드는 두 개의 함수 포인터가 성공 및 실패 사례를 각각 처리하도록 전달되는 비동기 구문인 **Promise** 형식을 사용합니다. 자세한 내용은 [JavaScript로 작성된 UWP의 비동기 패턴](https://msdn.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript)을 참조하세요.

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

  * Microsoft가 아닌 타사 광고 서버에서 광고 예약을 요청하려면 [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx)을 사용하고 서버 URL을 전달합니다. 이 메서드 또한 **Promise** 형식을 사용합니다.

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    <span/>
    >**참고**&nbsp;&nbsp;[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx)는 광고를 건너뛰고 바로 콘텐츠로 이동하도록 **MediaPlayer**에 알려주므로 함수가 실패할 경우에도 **play**를 호출해야 합니다. 광고를 원격으로 가져올 수 없는 경우 기본 제공 광고를 삽입하는 등 다른 비즈니스 요구 사항이 있을 수 있습니다.

8.  재생하는 동안 앱 트랙을 진행시키거나 초기 광고 일치 프로세스 후 발생할 수 있는 오류에 대한 추가 이벤트를 처리할 수 있습니다. 다음 코드에서는 [onPodStart](https://msdn.microsoft.com/library/windows/apps/mt732206.aspx), [onPodEnd](https://msdn.microsoft.com/library/windows/apps/mt732205.aspx), [onPodCountdown](https://msdn.microsoft.com/library/windows/apps/mt732204.aspx), [onAdProgress](https://msdn.microsoft.com/library/windows/apps/mt732201.aspx), [onAllComplete](https://msdn.microsoft.com/library/windows/apps/mt732202.aspx) 및 [onErrorOccurred](https://msdn.microsoft.com/library/windows/apps/mt732203.aspx) 등 이러한 이벤트 중 일부를 보여 줍니다.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]
