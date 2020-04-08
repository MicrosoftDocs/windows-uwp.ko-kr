---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, 다른 최신 구성 요소를 WPF 앱에 통합하는 방법을 보여 줍니다.
title: Windows 10 사용자 작업 및 알림 추가
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420119"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>4부: Windows 10 사용자 작업 및 알림 추가

Contoso Expenses라는 샘플 WPF 데스크톱 앱을 현대화하는 방법을 보여 주는 자습서의 네 번째 부분입니다. 자습서의 개요, 필수 조건 및 샘플 앱 다운로드 지침은 [자습서: WPF 앱 현대화](modernize-wpf-tutorial.md)를 참조하세요. 이 문서에서는 [3부](modernize-wpf-tutorial-3.md)를 이미 완료했다고 가정합니다.

이 자습서의 이전 부분에서는 XAML Islands를 사용하여 UWP XAML 컨트롤을 앱에 추가했습니다. 그 부작용으로, 앱이 모든 WinRT API를 호출할 수 있게 되었습니다. 이로써 앱이 UWP XAML 컨트롤뿐 아니라 Windows 10에서 제공하는 다른 여러 기능을 사용할 수 있는 기회가 열립니다.

이 자습서의 가상 시나리오에서 Contoso 개발 팀은 활동 및 알림이라는 두 가지 새로운 기능을 앱에 추가하기로 결정했습니다. 자습서의 이 부분에서는 해당 기능을 구현하는 방법을 보여 줍니다.

## <a name="add-a-user-activity"></a>사용자 활동 추가

Windows 10에서 앱은 파일을 열거나 특정 페이지를 표시하는 등 사용자가 수행한 활동을 추적할 수 있습니다. 해당 활동은 Windows 10, 버전 1803에서 도입된 기능인 타임라인을 통해 제공됩니다. 이 기능을 통해 사용자는 빠르게 과거로 돌아가서 이전에 시작한 활동을 다시 시작할 수 있습니다.

![Windows 타임라인 이미지](images/wpf-modernize-tutorial/WindowsTimeline.png)

사용자 활동은 [Microsoft Graph](https://developer.microsoft.com/graph/)를 사용하여 추적됩니다. 그러나 Windows 10 앱을 빌드하는 경우에는 Microsoft Graph에서 제공하는 REST 엔드포인트를 직접 조작할 필요가 없습니다. 대신, 편리한 WinRT API 세트를 사용할 수 있습니다. Contoso Expenses 앱에서 이러한 WinRT API를 사용하여 사용자가 앱 내에서 경비를 열 때마다 추적하고, 적응형 카드를 사용하여 사용자가 활동을 만들 수 있게 하려고 합니다.

### <a name="introduction-to-adaptive-cards"></a>적응형 카드 소개

이 섹션에서는 [적응형 카드](https://docs.microsoft.com/adaptive-cards/)에 대해 간략하게 설명합니다. 이 정보가 필요하지 않은 경우 건너뛰고 [적응형 카드 추가](#add-an-adaptive-card) 지침으로 바로 넘어가도 됩니다.

적응형 카드를 통해 개발자는 카드 콘텐츠를 공통적이고 일관된 방식으로 교환할 수 있습니다. 적응형 카드는 텍스트, 이미지, 작업 등을 포함할 수 있는 콘텐츠를 정의하는 JSON 페이로드로 설명됩니다.

적응형 카드는 콘텐츠만 정의하고 콘텐츠의 시각적 표시는 정의하지 않습니다. 적응형 카드가 수신된 플랫폼은 가장 적합한 스타일을 사용하여 콘텐츠를 렌더링할 수 있습니다. 적응형 카드는 [렌더러](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started)를 통해 디자인되며, 렌더러는 JSON 페이로드를 받아서 네이티브 UI로 변환할 수 있습니다. 예를 들어 UI는 WPF 또는 UWP 앱용 XAML, Android 앱용 AXML 또는 웹 사이트나 봇 채팅용 HTML일 수 있습니다.

간단한 적응형 카드 페이로드의 예는 다음과 같습니다.

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

아래 이미지는 ta Teams 채널, Cortana 및 Windows 알림을 통해 이 JSON을 다양한 방법으로 렌더링하는 방법을 보여 줍니다.

![적응형 카드 렌더링 이미지](images/wpf-modernize-tutorial/AdaptiveCards.png)

적응형 카드는 Windows에서 활동을 렌더링하는 방법이기 때문에 타임라인에서 중요한 역할을 합니다. 실제로 타임라인 내에 표시되는 각 축소판 그림은 적응형 카드입니다. 따라서 앱 내에서 사용자 활동을 만들려고 하면, 활동을 렌더링할 적응형 카드를 제공하라는 메시지가 표시됩니다.

> [!NOTE]
> 적응형 카드의 디자인을 브레인스토밍하는 좋은 방법은 [온라인 디자이너](https://adaptivecards.io/designer/)를 사용하는 것입니다. 구성 요소(이미지, 텍스트, 열 등)를 사용하여 카드를 디자인하고 해당 JSON을 가져오도록 하겠습니다. 최종 디자인을 결정한 후 [적응형 카드](https://www.nuget.org/packages/AdaptiveCards/)라는 라이브러리를 사용하여 디버그 및 빌드 작업이 어려운 일반 JSON 대신 C# 클래스로 적응형 카드를 더 쉽게 빌드할 수 있습니다.

### <a name="add-an-adaptive-card"></a>적응형 카드 추가

1. 솔루션 탐색기에서 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.

2. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭합니다. `Newtonsoft.Json` 패키지를 검색하고 사용 가능한 최신 버전을 설치합니다. 인기 있는 JSON 조작 라이브러리로, 여기서는 적응형 카드에 필요한 JSON 문자열을 조작하는 데 사용합니다.

    ![NewtonSoft.Json NuGet 패키지](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > `Newtonsoft.Json` 패키지를 별도로 설치하지 않을 경우, 적응형 카드 라이브러리는 .NET Core 3.0을 지원하지 않는 이전 버전의 `Newtonsoft.Json` 패키지를 참조합니다.

2. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭합니다. `AdaptiveCards` 패키지를 검색하고 사용 가능한 최신 버전을 설치합니다.

    ![적응형 카드 NuGet 패키지](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. **솔루션 탐색기**에서 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 -> 클래스**를 선택합니다. 클래스 이름을 **TimelineService.cs**로 지정하고 **확인**을 클릭합니다.

4. **TimelineService.cs** 파일에서 다음 문을 파일 맨 위에 추가합니다.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. 파일에서 선언된 네임스페이스를 `ContosoExpenses.Core`에서 `ContosoExpenses`로 변경합니다.

5. 다음 메서드를 `TimelineService` 클래스에 추가합니다.

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

#### <a name="about-the-code"></a>코드 정보

이 메서드는 렌더링할 경비에 대한 모든 정보가 포함된 **Expense** 개체를 받고 새 **AdaptiveCard** 개체를 빌드합니다. 메서드는 카드에 다음을 추가합니다.

- 제목 - 경비 설명 사용
- 이미지 - Contoso 로고
- 경비 금액
- 경비 날짜

마지막 3개의 요소는 서로 다른 두 개의 열로 분할되므로 Contoso 로고와 경비 세부 정보를 나란히 배치할 수 있습니다. 개체를 빌드한 후 메서드는 **ToJson** 메서드를 통해 해당 JSON 문자열을 반환합니다.

### <a name="define-the-user-activity"></a>사용자 활동 정의

이제 적응형 카드를 정의했으므로, 적응형 카드를 기반으로 해서 사용자 활동을 만들 수 있습니다.

1. **TimelineService.cs** 파일 맨 위에 다음 문을 추가합니다.

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > 해당 항목은 UWP 네임스페이스입니다. 2단계에서 설치한 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 패키지에 `Microsoft.Windows.SDK.Contracts` 패키지 참조가 포함되어 있어서 네임스페이스가 확인되므로 **ContosoExpenses.Core** 프로젝트에서 .NET Core 3 프로젝트인 WinRT API도 참조할 수 있습니다.

2. `TimelineService` 클래스에 다음 필드 선언을 추가합니다.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 다음 메서드를 `TimelineService` 클래스에 추가합니다.

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

4. **TimelineService.cs**의 변경 내용을 저장합니다.

#### <a name="about-the-code"></a>코드 정보

`AddToTimeline` 메서드는 먼저 사용자 활동을 저장하는 데 필요한 **UserActivityChannel** 개체를 가져옵니다. 그런 다음, **GetOrCreateUserActivityAsync** 메서드를 사용하여 새 사용자 활동을 만듭니다. 여기에는 고유 식별자가 필요합니다. 따라서 활동이 이미 있는 경우 앱에서 업데이트할 수 있습니다. 활동이 없으면 새로 만듭니다. 전달할 식별자는 빌드 중인 애플리케이션 종류에 따라 다릅니다.

* 타임라인에 가장 최근 항목만 표시되도록 동일한 활동을 항상 업데이트하려면 고정 식별자(예: **Expenses**)를 사용할 수 있습니다.
* 모든 활동을 각기 다른 활동으로 추적하여 타임라인에 모든 활동을 표시하려면 동적 식별자를 사용할 수 있습니다.

이 시나리오에서 앱은 각 열린 경비를 다른 사용자 활동으로 추적하므로 코드에서 **Expense-** 키워드 뒤에 고유한 경비 ID를 사용하여 각 식별자를 만듭니다.

메서드는 **UserActivity** 개체를 만든 후 다음 정보를 사용하여 개체를 채웁니다.

* **ActivationUri** - 사용자가 타임라인에서 활동을 클릭할 때 호출됩니다. 코드는 **contosoexpenses**라는 사용자 지정 프로토콜을 사용하며, 앱이 나중에 처리합니다.
* **VisualElements** 개체 - 활동의 시각적 모양을 정의하는 속성 세트를 포함합니다. 이 코드는 **DisplayText**(타임라인에서 항목 위에 표시되는 제목) 및 **Content**를 설정합니다. 

앞에서 정의한 적응형 카드는 여기서 역할을 수행합니다. 앱은 앞에서 디자인한 적응형 카드를 메서드에 콘텐츠로 전달합니다. 그러나 Windows 10에서는 `AdaptiveCards` NuGet 패키지가 사용하는 것과 다른 개체를 사용하여 카드를 나타냅니다. 따라서 메서드는 **AdaptiveCardBuilder** 클래스를 통해 공개되는 **CreateAdaptiveCardFromJson** 메서드를 사용하여 카드를 다시 만듭니다. 메서드는 사용자 활동을 만든 후 활동을 저장하고 새 세션을 만듭니다.

사용자가 타임라인에서 활동을 클릭하면 **contosoexpenses://** 프로토콜이 활성화되고, URL에는 앱이 선택한 경비를 검색하는 데 필요한 정보가 포함됩니다. 선택적 작업으로, 프로토콜 활성화를 구현하여 사용자가 타임라인을 사용할 때 애플리케이션이 올바르게 반응하도록 할 수 있습니다.

### <a name="integrate-the-application-with-timeline"></a>타임라인과 애플리케이션 통합

이제 타임라인과 상호 작용하는 클래스를 만들었으므로, 이 클래스를 사용하여 애플리케이션 환경을 향상할 수 있습니다. **TimelineService** 클래스를 통해 공개되는 **AddToTimeline** 메서드를 사용하기에 가장 적합한 위치는 사용자가 경비 세부 정보 페이지를 열 때입니다.

1. **ContosoExpenses.Core** 프로젝트에서 **ViewModels** 폴더를 펼치고 **ExpenseDetailViewModel.cs** 파일을 엽니다. 이 파일은 경비 세부 정보 창을 지원하는 ViewModel입니다.

2. **ExpenseDetailViewModel** 클래스의 public 생성자를 찾은 후에 다음 코드를 생성자 끝에 추가합니다. 경비 창이 열릴 때마다 메서드는 **AddToTimeline** 메서드를 호출하고 현재 경비를 전달합니다. **TimelineService** 클래스는 이 정보를 통해 경비 정보를 사용하여 사용자 활동을 만듭니다.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    완료된 생성자는 다음과 같이 표시됩니다.

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

3. F5 키를 눌러 디버거에서 앱을 빌드하고 실행합니다. 목록에서 직원을 선택하고 경비를 선택합니다. 세부 정보 페이지에서 경비 설명, 날짜 및 금액을 확인합니다.

4. **시작+Tab**을 눌러 타임라인을 엽니다.

5. 현재 열려 있는 애플리케이션 목록에서 **Earlier today** 섹션이 표시될 때까지 아래로 스크롤합니다. 이 섹션에는 가장 최근 사용자 활동 중 일부가 표시됩니다. **Earlier today** 제목 옆에 있는 **모든 활동 표시** 링크를 클릭합니다.

6. 애플리케이션에서 방금 선택한 경비에 대한 정보가 포함된 새 카드가 표시되는지 확인합니다.

    ![Contoso Expenses 타임라인](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. 이제 다른 경비를 열면 새 카드가 사용자 활동으로 추가되는 것을 확인할 수 있습니다. 코드는 각 활동에 다른 ID를 사용하므로 앱에서 여는 경비마다 해당 카드가 생성됩니다.

8. 앱을 종료합니다.

## <a name="add-a-notification"></a>알림 추가

Contoso 개발 팀이 추가하려는 두 번째 기능은 새 경비를 데이터베이스에 저장할 때마다 사용자에게 표시되는 알림입니다. 이 작업을 위해 WinRT API를 통해 개발자에게 공개되는 Windows 10의 기본 제공 알림 시스템을 활용할 수 있습니다. 이 알림 시스템에는 다음과 같은 여러 장점이 있습니다.

- 알림이 OS의 다른 부분과 일치합니다.
- 실행 가능합니다.
- 알림 센터에 저장되므로 나중에 검토할 수 있습니다.

앱에 알림을 추가하려면 다음을 수행합니다.

1. **솔루션 탐색기**에서 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 -> 클래스**를 선택합니다. 클래스 이름을 **NotificationService.cs**로 지정하고 **확인**을 클릭합니다.

2. **NotificationService.cs** 파일에서 다음 문을 파일 맨 위에 추가합니다.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. 파일에서 선언된 네임스페이스를 `ContosoExpenses.Core`에서 `ContosoExpenses`로 변경합니다.

4. 다음 메서드를 `NotificationService` 클래스에 추가합니다.

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

    알림 메시지는 텍스트, 이미지, 작업 등을 포함할 수 있는 XML 페이로드로 표시됩니다. 지원되는 모든 요소는 [여기](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema)에서 확인할 수 있습니다. 이 코드는 제목과 본문이라는 두 줄의 텍스트를 포함하는 매우 간단한 스키마를 사용합니다. 코드는 XML 페이로드를 정의하고 **XmlDocument** 개체에 로드한 후 XML을 **ToastNotification** 개체에 래핑하고 **ToastNotificationManager** 클래스를 사용하여 표시합니다.

5. **ContosoExpenses.Core** 프로젝트에서 **ViewModels** 폴더를 펼치고 **AddNewExpenseViewModel.cs** 파일을 엽니다. 

6. 사용자가 단추를 눌러 새 경비를 저장할 때 트리거되는 `SaveExpenseCommand` 메서드를 찾습니다. 이 메서드에서 `SaveExpense` 메서드 호출 바로 뒤에 다음 코드를 추가합니다.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    완료된 `SaveExpenseCommand` 메서드는 다음과 같이 표시됩니다.

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

7. F5 키를 눌러 디버거에서 앱을 빌드하고 실행합니다. 목록에서 직원을 선택하고 **새 경비 추가** 단추를 클릭합니다. 양식의 모든 필드를 작성하고 **저장**을 누릅니다.

8. 다음 예외가 표시됩니다.

    ![알림 메시지 오류](images/wpf-modernize-tutorial/ToastNotificationError.png)

이 예외는 Contoso Expenses 앱에 패키지 ID가 아직 없기 때문에 발생합니다. 알림 API를 비롯한 일부 WinRT API는 패키지 ID가 있어야만 앱에서 사용할 수 있습니다. UWP 앱은 MSIX 패키지를 통해서만 배포할 수 있으므로 기본적으로 패키지 ID를 받습니다. 패키지 ID를 얻기 위해 WPF 앱을 비롯한 다른 유형의 Windows 앱도 MSIX 패키지를 통해 배포할 수 있습니다. 이 자습서의 다음 부분에서 이 작업을 수행하는 방법을 살펴보겠습니다.

## <a name="next-steps"></a>다음 단계

지금까지 자습서를 진행했다면 Windows 타임라인과 통합된 앱에 사용자 활동을 추가했으며, 사용자가 새 경비를 만들 때 트리거되는 알림도 앱에 추가했습니다. 그러나 알림 API를 사용하려면 앱에 패키지 ID가 필요하기 때문에 알림은 아직 작동하지 않습니다. 패키지 ID와 기타 배포 혜택을 얻기 위해 앱의 MSIX 패키지를 빌드하는 방법에 대한 자세한 내용은 [5부: MSIX로 패키징 및 배포](modernize-wpf-tutorial-5.md)를 참조하세요.
