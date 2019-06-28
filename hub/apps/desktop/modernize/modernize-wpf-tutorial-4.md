---
description: 이 자습서에는 UWP XAML 사용자 인터페이스를 추가, MSIX 패키지 만들기 및 WPF 앱에 다른 최신 구성 요소를 통합 하는 방법을 보여 줍니다.
title: Windows 10 사용자 동작 및 알림을 추가 합니다.
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420119"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>4부: Windows 10 사용자 동작 및 알림을 추가 합니다.

Contoso Expenses 라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법에 설명 하는 자습서의 네 번째 부분입니다. 자습서, 필수 구성 요소 및 샘플 앱을 다운로드 하기 위한 지침의 개요를 참조 하세요. [자습서: WPF 앱을 현대화](modernize-wpf-tutorial.md)합니다. 이 문서에서는 이미 완료 했다고 가정 [3 부](modernize-wpf-tutorial-3.md)합니다.

이 자습서의 이전 부분에서 UWP XAML 컨트롤 XAML 제도 사용 하 여 앱을 추가 했습니다. 으로이 제품에 사용 하도록 설정한 모든 WinRT API를 호출 하는 앱입니다. UWP XAML 컨트롤 뿐 아니라 Windows 10에서 제공 하는 다른 많은 기능을 사용 하 여 앱에 대 한 기회가 열립니다.

이 자습서에서는 가상의 시나리오에서 Contoso 개발팀 하기로 결정 했습니다 앱에 두 가지 새로운 기능을 추가 합니다: 활동 및 알림. 이 자습서의이 부분에는 이러한 기능을 구현 하는 방법을 보여 줍니다.

## <a name="add-a-user-activity"></a>사용자 활동을 추가 합니다.

Windows 10에서 앱에는 파일을 열거나 특정 페이지를 표시 하는 등 사용자가 수행한 작업을 추적할 수 있습니다. 이러한 활동 타임 라인 버전 1803에서 Windows 10에 도입 된 기능을 사용 하면 신속 하 게 과거로 다시 이동 하 고 이전에 시작 작업을 다시 시작 하는 통해 사용할 수 있습니다.

![Windows 타임 라인 이미지](images/wpf-modernize-tutorial/WindowsTimeline.png)

사용자 활동을 사용 하 여 추적할지 [Microsoft Graph](https://developer.microsoft.com/graph/)합니다. 그러나 Windows 10 앱을 빌드하는 경우에 Microsoft graph REST 끝점과 직접 상호 작용할 필요가 없습니다. 대신, WinRT Api 집합을 편리 하 게 사용할 수 있습니다. Contoso expenses에 이러한 WinRT Api를 사용 하 여 사용자가 앱 내에서 비용을 열 때마다 추적 하 고 활동을 만들 수 있도록 하려면 Adaptive Card를 사용 하려고 합니다.

### <a name="introduction-to-adaptive-cards"></a>Adaptive Card 소개

이 섹션의 간략 한 개요를 제공 [Adaptive Card](https://docs.microsoft.com/adaptive-cards/)합니다. 이 정보가 필요 하지 않으면,이 건너뛸 하 수 오른쪽으로 이동 합니다 [Adaptive Card 추가](#add-an-adaptive-card) 지침입니다.

Adaptive Card를 통해 공통적 이며 일관 된 방식으로 카드 콘텐츠를 교환 하는 개발자. Adaptive Card는 텍스트, 이미지, 작업 등을 포함할 수 있는 내용을 정의 하는 JSON 페이로드를 통해 설명 됩니다.

Adaptive Card 콘텐츠 및 콘텐츠의 시각적 모양이 없습니다를 정의합니다. Adaptive Card는 수신 되는 플랫폼 가장 적합 한 스타일을 사용 하 여 콘텐츠를 렌더링할 수 있습니다. Adaptive Card는 설계 하는 방법은 [렌더러를](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started), JSON 페이로드를 되려면 및 네이티브 UI 요소로 변환할 수는 있습니다. 예를 들어, UI는 WPF 또는 UWP 앱의 경우 Android 앱에 대 한 AXML 또는 HTML 웹 사이트 또는 봇 채팅에 대 한 XAML을 수 있습니다.

간단한 Adaptive Card 페이로드의 예제는 다음과 같습니다.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

아래 이미지에서는이 JSON이 렌더링 되는 방식을 하 여 다양 한 방식에서 ta 팀 채널, Cortana 및 Windows 알림을 보여 줍니다.

![적응 카드 렌더링 이미지](images/wpf-modernize-tutorial/AdaptiveCards.png)

Adaptive card Windows 작업을 렌더링 하는 방식을 이기 때문에 타임 라인에서 중요 한 역할을 수행 합니다. 타임 라인 내에 표시 하는 각 미리 보기는 실제로 적응 카드. 따라서 앱 내에서 사용자 활동을 만들려면 하려는 경우 렌더링 하는 Adaptive Card를 제공 하 라는 메시지가 표시 됩니다.

> [!NOTE]
> Adaptive Card의 디자인에 토론 하는 좋은 방법 사용 하 여 [온라인 디자이너](https://adaptivecards.io/designer/)합니다. 구성 요소 (이미지, 텍스트, 열 등)를 사용 하 여 카드를 디자인 하 고 해당 JSON을 가져올 수가 있습니다. 최종 디자인의 아이디어를 설정한 후 라는 라이브러리를 사용할 수 있습니다 [Adaptive Card](https://www.nuget.org/packages/AdaptiveCards/) Adaptive Card를 통해 빌드를 쉽게 수행할 수 있도록 C# 클래스 대신 일반 JSON 빌드하고 디버그 하기 어려울 수 있습니다.

### <a name="add-an-adaptive-card"></a>Adaptive Card를 추가 합니다.

1. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 솔루션 탐색기에서 프로젝트를 선택 **NuGet 패키지 관리**합니다.

2. 에 **NuGet 패키지 관리자** 창에서 클릭 **찾아보기**합니다. 검색 된 `Newtonsoft.Json` 패키지 및 사용 가능한 최신 버전을 설치 합니다. 이 JSON 문자열을 필요한 Adaptive Card mainipulate 하는 데 사용할 인기 있는 JSON 조작 라이브러리입니다.

    ![NewtonSoft.Json NuGet 패키지](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > 설치 하지 않는 경우는 `Newtonsoft.Json` 별도로 패키지, The Adaptive Card 라이브러리는 이전 버전의 참조는 `Newtonsoft.Json` .NET Core 3.0을 지원 하지 않는 패키지 합니다.

2. 에 **NuGet 패키지 관리자** 창에서 클릭 **찾아보기**합니다. 검색 된 `AdaptiveCards` 패키지 및 사용 가능한 최신 버전을 설치 합니다.

    ![적응 카드 NuGet 패키지](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트 **추가-> 클래스**합니다. 클래스의 이름을 **TimelineService.cs** 누릅니다 **확인**합니다.

4. 에 **TimelineService.cs** 파일, 파일의 맨 위에 다음 문을 추가 합니다.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. 파일에 네임 스페이스 선언 변경 `ContosoExpenses.Core` 에 `ContosoExpenses`입니다.

5. 다음 메서드를 추가 합니다 `TimelineService` 클래스입니다.

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>코드에 대 한

이 메서드에 **비용** 개체를 렌더링 하는 비용 및 해당 하는 방법에 대 한 모든 정보를 사용 하 여 새 빌드 **AdaptiveCard** 개체입니다. 카드에 다음 항목을 추가 하는 메서드:

- 제목-비용에 대 한 설명을 사용 하 여 합니다.
- 이미지는 Contoso 로고입니다.
- 비용의 양입니다.
- 날짜는 비용입니다.

마지막 3 개의 요소는 Contoso 로고 및 비용에 대 한 세부 정보를 나란히 배치 수 있도록 두 개의 다른 열으로 분할 됩니다. 메서드를 사용 하 여 해당 JSON 문자열의 반환 개체를 빌드한 후 합니다 **ToJson** 메서드.

### <a name="define-the-user-activity"></a>사용자 동작을 정의 합니다.

Adaptive Card를 정의한 했으므로에 따라 사용자 작업을 만들 수 있습니다.

1. 맨 위에 다음 문을 추가 **TimelineService.cs** 파일:

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > 이들은 UWP 네임 스페이스입니다. 때문에 해결 하는 이러한를 `Microsoft.Toolkit.Wpf.UI.Controls` 2 단계에서 설치한 NuGet 패키지에 대 한 참조를 포함 합니다 `Microsoft.Windows.SDK.Contracts` 수 있도록 패키지를 **ContosoExpenses.Core** .NET 이더라도 WinRT Api 참조를 프로젝트 코어 3 프로젝트입니다.

2. 에 다음 필드 선언을 추가 합니다 `TimelineService` 클래스입니다.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 다음 메서드를 추가 합니다 `TimelineService` 클래스입니다.

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. 변경 내용을 저장 **TimelineService.cs**합니다.

#### <a name="about-the-code"></a>코드에 대 한

합니다 `AddToTimeline` 메서드는 먼저 가져옵니다는 **UserActivityChannel** 사용자 활동을 저장 하는 데 필요한 개체입니다. 사용 하 여 새 사용자 활동을 만듭니다는 **GetOrCreateUserActivityAsync** 메서드를 고유한 식별자가 필요 합니다. 이 따라서 활동 이미 있는 경우 앱을 업데이트할 수; 그렇지 않으면 새로 만들어집니다. 빌드하는 응용 프로그램의 종류별로 전달할 식별자에 따라 달라 집니다.

* 시간 표시 막대는 가장 최근 표시 되도록 동일한 활동을 항상 업데이트 하려는 경우에 고정된 식별자를 사용할 수 있습니다 (같은 **비용**).
* 추적 하려는 모든 작업을 다른 것으로 모든 타임 라인 표시 됩니다, 경우에 동적 식별자를 사용할 수 있습니다.

이 시나리오에서는 앱에서 추적 각 열린된 비용을 다른 사용자 작업으로 하므로 코드는 키워드를 사용 하 여 각 식별자를 만듭니다 **비용-** 뒤에 고유한 비용 id입니다.

메서드를 만든 후에 **UserActivity** 개체를 다음 정보를 사용 하 여 개체를 채웁니다.

* **ActivationUri** 타임 라인에서 활동을 클릭할 때 호출 되는 합니다. 라는 사용자 지정 프로토콜을 사용 하 여 코드 **contosoexpenses** 앱을 나중에 처리할 수 있습니다.
* 합니다 **VisualElements** 활동의 시각적 모양을 정의 하는 속성 집합이 포함 된 개체입니다. 이 코드를 설정 합니다 **DisplayText** (되 타임 라인의 항목 위에 표시 되는 제목) 및 **콘텐츠**합니다. 

이전에 정의한 Adaptive Card 역할을 담당 하는 위치입니다. 앱 디자인 Adaptive Card 이전 내용으로 메서드에 전달 합니다. Windows 10에서 사용 하는 것에 비해 카드를 나타내는 다른 개체를 사용 하는 반면는 `AdaptiveCards` NuGet 패키지. 메서드를 사용 하 여 카드를 다시 만듭니다는 따라서 합니다 **CreateAdaptiveCardFromJson** 에 의해 노출 되는 메서드는 **AdaptiveCardBuilder** 클래스입니다. 메서드는 사용자 활동을 만든 후 작업을 저장 하 고 새 세션을 만듭니다.

사용자 타임 라인에서 활동을 클릭할 때 합니다 **contosoexpenses: / /** 프로토콜을 활성화할 및 URL에 앱이 선택한 비용을 검색 하는 데 필요한 정보가 포함 됩니다. 선택적 작업으로 사용자 타임 라인을 사용 하는 경우 응용 프로그램이 올바르게 반응 있도록 프로토콜 활성화를 구현할 수 있습니다.

### <a name="integrate-the-application-with-timeline"></a>타임 라인을 사용 하 여 응용 프로그램 통합

타임 라인을 사용 하 여 상호 작용 하는 클래스를 만든 했으므로 사용 하 여 응용 프로그램의 환경을 개선 하기 위해 시작할 수 있습니다. 사용 하기에 가장 적합 합니다 **AddToTimeline** 에 의해 노출 되는 메서드는 **TimelineService** 클래스는 사용자가 비용의 세부 정보 페이지를 열면 합니다.

1. 에 **ContosoExpenses.Core** 프로젝트를 확장 하 고는 **Viewmodel** 폴더를 **ExpenseDetailViewModel.cs** 파일입니다. ViewModel을 지 원하는 비용 세부 정보 창입니다.

2. public 생성자를 찾을 합니다 **ExpenseDetailViewModel** 클래스 및 생성자의 끝에 다음 코드를 추가 합니다. 메서드를 호출 하는 비용 창이 열릴 때마다 합니다 **AddToTimeline** 메서드 현재 비용을 전달 합니다. 합니다 **TimelineService** 클래스 비용 정보를 사용 하 여 사용자 활동을 만들어이 정보를 사용 합니다.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    완료 되 면 생성자는 다음과 같습니다.

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. F5 키를 눌러 빌드하고 디버거에서 앱을 실행 합니다. 목록에서 직원을 선택 하 고 비용을 선택 합니다. 세부 정보 페이지에서 비용, 날짜 및 시간 일자를 note 합니다.

4. 키를 눌러 **시작 + 탭** 를 타임 라인을 엽니다.

5. 현재 열려 있는 응용 프로그램 섹션에 표시 될 때까지 목록 아래로 스크롤하여 **오늘 일찍**합니다. 이 섹션에서는 가장 최근의 사용자 활동의 일부를 보여 줍니다. 클릭 합니다 **모든 작업이** 링크 옆에 **오늘 일찍** 제목입니다.

6. 응용 프로그램에서 방금 선택한 비용에 대 한 정보를 사용 하 여 새 카드가 있는지 확인 합니다.

    ![Contoso 비용 타임 라인](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. 이제 다른 비용을 열면 사용자 활동으로 추가할 새 카드 표시 됩니다. 코드는 다른 식별자를 사용 하 여 각 활동에 대해을 앱에서 열면 없으므로 각 비용에 대 한 카드 만듭니다 해야 합니다.

8. 앱을 종료합니다.

## <a name="add-a-notification"></a>알림 추가

Contoso 개발 팀에서 추가 하려는 두 번째 기능은는 새 비용 데이터베이스에 저장 될 때마다 사용자에 게 표시 되는 알림입니다. 이렇게 하려면 WinRT Api를 통해 개발자에 게 노출 되는 Windows 10에 기본 제공 알림 시스템을 활용할 수 있습니다. 이 알림 시스템에 많은 이점이 있습니다.

- 알림은 OS의 나머지 부분과 일치 합니다.
- 실행 가능한 됩니다.
- 이러한 가져오기 나중에 검토할 수 있도록 관리 센터에 저장 됩니다.

앱에 알림을 추가:

1. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트 **추가-> 클래스**합니다. 클래스의 이름을 **NotificationService.cs** 누릅니다 **확인**합니다.

2. 에 **NotificationService.cs** 파일, 파일의 맨 위에 다음 문을 추가 합니다.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. 파일에 네임 스페이스 선언 변경 `ContosoExpenses.Core` 에 `ContosoExpenses`입니다.

4. 다음 메서드를 추가 합니다 `NotificationService` 클래스입니다.

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    알림은 텍스트, 이미지, 작업 등을 포함할 수 있는 XML 페이로드를 표시 됩니다. 지원 되는 모든 요소를 찾을 수 있습니다 [여기](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema)합니다. 이 코드는 매우 간단한 스키마를 사용 하 여 두 줄 텍스트를 사용 하 여: 제목 및 본문입니다. 코드는 XML 페이로드를 정의 하 고에서 로드 한 후는 **XmlDocument** 개체를 XML에 래핑하는 **ToastNotification** 개체를 사용 하 여 표시를 **ToastNotificationManager** 클래스입니다.

5. 에 **ContosoExpenses.Core** 프로젝트를 확장 하 고는 **Viewmodel** 폴더를 **AddNewExpenseViewModel.cs** 파일입니다. 

6. 찾을 `SaveExpenseCommand` 메서드를 사용자가 새 비용을 저장 하려면 시스템 단추를 누를 때 트리거됩니다. 에 대 한 호출 바로 뒤에이 메서드에 다음 코드를 추가 합니다 `SaveExpense` 메서드.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    완료 되 면를 `SaveExpenseCommand` 메서드는 다음과 같습니다.

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. F5 키를 눌러 빌드하고 디버거에서 앱을 실행 합니다. 목록에서 직원을 선택 하 고 클릭 합니다 **새 비용 추가** 단추입니다. 폼 및 키를 눌러 모든 필드를 완성 **저장할**합니다.

8. 다음 예외를 받게 됩니다.

    ![토스트 알림 오류](images/wpf-modernize-tutorial/ToastNotificationError.png)

이 예외는 않기 때문에 Contoso expenses 패키지 id를 아직 없습니다. 알림 API를 비롯 한 일부 WinRT Api 패키지 id가 있어야 앱에서 사용할 수 있습니다. UWP 앱 MSIX 패키지를 통해 배포할 수만 있습니다 때문에 기본적으로 패키지 id를 수신 합니다. Windows 앱, WPF 앱을 비롯 한 다른 유형의 패키지 id를 가져오려면 MSIX 패키지를 통해 배포할 수도 있습니다. 이 자습서의 다음 부분에는이 작업을 수행 하는 방법을 살펴봅니다.

## <a name="next-steps"></a>다음 단계

이 시점에서 자습서를 성공적으로 추가한 사용자 활동을 Windows 타임 라인와 통합 되는 앱에도 사용자가 새 비용을 만들 때 트리거되는 앱에 알림을 추가한 하며 그러나 알림을 앱에 알림 API를 사용 하는 패키지 id 필요 하기 때문에 아직 작동 하지 않습니다. 앱 패키지 id를 얻어 배포 이점이 다른 용 MSIX 패키지를 빌드하는 방법에 알아보려면 참조 [5 부: 패키지 및 MSIX를 사용 하 여 배포](modernize-wpf-tutorial-5.md)합니다.
