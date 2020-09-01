---
title: 백그라운드 작업에서 라이브 타일 업데이트
description: 백그라운드 작업을 사용 하 여 최신 콘텐츠를 사용 하 여 앱의 라이브 타일을 업데이트 합니다.
Search.SourceType: Video
ms.assetid: 9237A5BD-F9DE-4B8C-B689-601201BA8B9A
ms.date: 01/11/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 50ed0246941645824ee0705582a9efbf2b193dc9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155777"
---
# <a name="update-a-live-tile-from-a-background-task"></a>백그라운드 작업에서 라이브 타일 업데이트

**중요 API**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

백그라운드 작업을 사용 하 여 최신 콘텐츠를 사용 하 여 앱의 라이브 타일을 업데이트 합니다.

앱에 라이브 타일을 추가 하는 방법을 보여 주는 비디오는 다음과 같습니다.

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Updating-a-live-tile-from-a-background-task/player" width="720" height="405" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="create-the-background-task-project"></a>백그라운드 작업 프로젝트 만들기  

앱에 대 한 라이브 타일을 사용 하도록 설정 하려면 새 Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다. 사용자가 앱을 설치할 때 백그라운드에서 OS가 로드 되 고 실행 되는 별도의 어셈블리입니다.

1.  솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **추가**를 클릭한 후 **새 프로젝트**를 클릭합니다.
2.  **새 프로젝트 추가** 대화 상자의 **설치 된 &gt; 다른 언어 &gt; Visual c # &gt; Windows 유니버설** 섹션에서 **Windows 런타임 구성 요소** 템플릿을 선택 합니다.
3.  프로젝트 이름을 BackgroundTasks로, **확인**을 클릭 하거나 탭 합니다. Microsoft Visual Studio 새 프로젝트가 솔루션에 추가 됩니다.
4.  주 프로젝트에서 BackgroundTasks 프로젝트에 대 한 참조를 추가 합니다.

## <a name="implement-the-background-task"></a>백그라운드 작업 구현


[**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하 여 앱의 라이브 타일을 업데이트 하는 클래스를 만듭니다. 백그라운드 작업은 Run 메서드에 포함됩니다. 이 경우 작업은 MSDN 블로그의 배포 피드를 가져옵니다. 비동기 코드가 실행 되는 동안 작업을 계속 닫지 않으려면 지연을 가져옵니다.

1.  솔루션 탐색기에서 자동으로 생성 된 파일인 Class1.cs을 BlogFeedBackgroundTask.cs로 바꿉니다.
2.  BlogFeedBackgroundTask.cs에서 자동으로 생성 된 코드를 **BlogFeedBackgroundTask** 클래스의 스텁 코드로 바꿉니다.
3.  Run 메서드 구현에서 **GetMSDNBlogFeed** 및 **updatetile** 메서드에 대 한 코드를 추가 합니다.

```cs
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// Added during quickstart
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.UI.Notifications;
using Windows.Web.Syndication;

namespace BackgroundTasks
{
    public sealed class BlogFeedBackgroundTask  : IBackgroundTask
    {
        public async void Run( IBackgroundTaskInstance taskInstance )
        {
            // Get a deferral, to prevent the task from closing prematurely
            // while asynchronous code is still running.
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();

            // Download the feed.
            var feed = await GetMSDNBlogFeed();

            // Update the live tile with the feed items.
            UpdateTile( feed );

            // Inform the system that the task is finished.
            deferral.Complete();
        }

        private static async Task<SyndicationFeed> GetMSDNBlogFeed()
        {
            SyndicationFeed feed = null;

            try
            {
                // Create a syndication client that downloads the feed.  
                SyndicationClient client = new SyndicationClient();
                client.BypassCacheOnRetrieve = true;
                client.SetRequestHeader( customHeaderName, customHeaderValue );

                // Download the feed.
                feed = await client.RetrieveFeedAsync( new Uri( feedUrl ) );
            }
            catch( Exception ex )
            {
                Debug.WriteLine( ex.ToString() );
            }

            return feed;
        }

        private static void UpdateTile( SyndicationFeed feed )
        {
            // Create a tile update manager for the specified syndication feed.
            var updater = TileUpdateManager.CreateTileUpdaterForApplication();
            updater.EnableNotificationQueue( true );
            updater.Clear();

            // Keep track of the number feed items that get tile notifications.
            int itemCount = 0;

            // Create a tile notification for each feed item.
            foreach( var item in feed.Items )
            {
                XmlDocument tileXml = TileUpdateManager.GetTemplateContent( TileTemplateType.TileWide310x150Text03 );

                var title = item.Title;
                string titleText = title.Text == null ? String.Empty : title.Text;
                tileXml.GetElementsByTagName( textElementName )[0].InnerText = titleText;

                // Create a new tile notification.
                updater.Update( new TileNotification( tileXml ) );

                // Don't create more than 5 notifications.
                if( itemCount++ > 5 ) break;
            }
        }

        // Although most HTTP servers do not require User-Agent header, others will reject the request or return
        // a different response if this header is missing. Use SetRequestHeader() to add custom headers.
        static string customHeaderName = "User-Agent";
        static string customHeaderValue = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";

        static string textElementName = "text";
        static string feedUrl = @"http://blogs.msdn.com/b/MainFeed.aspx?Type=BlogsOnly";
    }
}
```

## <a name="set-up-the-package-manifest"></a>패키지 매니페스트 설정


패키지 매니페스트를 설정 하려면 해당 매니페스트를 열고 새 백그라운드 작업 선언을 추가 합니다. 해당 네임 스페이스를 포함 하 여 작업에 대 한 진입점을 클래스 이름으로 설정 합니다.

1.  솔루션 탐색기에서 appxmanifest.xml을 엽니다.
2.  **선언** 탭을 클릭 하거나 탭 합니다.
3.  **사용 가능한 선언**에서 **BackgroundTasks** 을 선택 하 고 **추가**를 클릭 합니다. Visual Studio는 **지원 되는 선언**아래에 **BackgroundTasks** 를 추가 합니다.
4.  지원 되는 **작업 유형**에서 **타이머** 가 선택 되어 있는지 확인 합니다.
5.  **앱 설정**에서 진입점을 **BackgroundTasks. BlogFeedBackgroundTask**로 설정 합니다.
6.  **응용 프로그램 UI** 탭을 클릭 하거나 탭 합니다.
7.  **화면 잠금 알림을** **배지 및 타일 텍스트**로 설정 합니다.
8.  **배지 로고** 필드에 24x24 픽셀 아이콘의 경로를 설정 합니다.
    **중요**    이 아이콘은 단색 및 투명 픽셀만 사용 해야 합니다.
9.  **작은 로고** 필드에서 경로를 30x30 픽셀 아이콘으로 설정 합니다.
10. **와이드 로고** 필드에서 경로를 310x150 픽셀 아이콘으로 설정 합니다.

## <a name="register-the-background-task"></a>백그라운드 작업 등록


[**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 를 만들어 작업을 등록 합니다.

> **참고**    Windows 8.1부터 백그라운드 작업 등록 매개 변수는 등록 시 유효성 검사가 수행 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱은 백그라운드 작업 등록이 실패 하는 시나리오를 처리할 수 있어야 합니다. 예를 들어 조건문을 사용 하 여 등록 오류를 확인 한 다음 다른 매개 변수 값을 사용 하 여 실패 한 등록을 다시 시도할 수 있습니다.
 

앱의 기본 페이지에서 **RegisterBackgroundTask** 메서드를 추가 하 고 **OnNavigatedTo** 이벤트 처리기에서 호출 합니다.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Syndication;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace ContosoApp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.  The Parameter
        /// property is typically used to configure the page.</param>
        protected override void OnNavigatedTo( NavigationEventArgs e )
        {
            this.RegisterBackgroundTask();
        }


        private async void RegisterBackgroundTask()
        {
            var backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();
            if( backgroundAccessStatus == BackgroundAccessStatus.AllowedSubjectToSystemPolicy ||
                backgroundAccessStatus == BackgroundAccessStatus.AlwaysAllowed )
            {
                foreach( var task in BackgroundTaskRegistration.AllTasks )
                {
                    if( task.Value.Name == taskName )
                    {
                        task.Value.Unregister( true );
                    }
                }

                BackgroundTaskBuilder taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = taskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger( new TimeTrigger( 15, false ) );
                var registration = taskBuilder.Register();
            }
        }

        private const string taskName = "BlogFeedBackgroundTask";
        private const string taskEntryPoint = "BackgroundTasks.BlogFeedBackgroundTask";
    }
}
```

## <a name="debug-the-background-task"></a>백그라운드 작업 디버그


백그라운드 작업을 디버깅 하려면 작업의 Run 메서드에서 중단점을 설정 합니다. **디버그 위치** 도구 모음에서 백그라운드 작업을 선택 합니다. 이렇게 하면 시스템에서 Run 메서드를 즉시 호출 합니다.

1.  작업의 실행 메서드에서 중단점을 설정 합니다.
2.  F5 키를 누르거나 **디버그 &gt; 디버깅 시작** 을 탭 하 여 앱을 배포 하 고 실행 합니다.
3.  앱이 시작 되 면 Visual Studio로 다시 전환 합니다.
4.  **디버그 위치** 도구 모음이 표시 되는지 확인 합니다. ** &gt; 도구 모음 보기** 메뉴에 있습니다.
5.  **디버그 위치** 도구 모음에서 **일시 중단** 드롭다운을 클릭 하 고 **BlogFeedBackgroundTask**를 선택 합니다.
6.  Visual Studio는 중단점에서 실행을 일시 중단 합니다.
7.  F5 키를 누르거나 **디버그 &gt; 계속** 을 탭 하 여 앱을 계속 실행 합니다.
8.  Shift + F5 키를 누르거나 **디버그 &gt; 디버깅 중지** 를 탭 하 여 디버깅을 중지 합니다.
9.  시작 화면에서 앱의 타일로 돌아갑니다. 몇 초 후에 응용 프로그램의 타일에 타일 알림이 표시 됩니다.

## <a name="related-topics"></a>관련 항목


* [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
* [**TileUpdateManager**](/uwp/api/Windows.UI.Notifications.TileUpdateManager)
* [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification)
* [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)
* [타일 및 배지에 대 한 지침 및 검사 목록](../design/shell/tiles-and-notifications/creating-tiles.md)

 

 