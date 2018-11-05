---
author: Jwmsft
Description: You create the UI for your app by using controls such as buttons, text boxes, and combo boxes to display data and get user input. Here, we show you how to add controls to your app.
title: 컨트롤 및 패턴 소개
ms.assetid: 64740BF2-CAA1-419E-85D1-42EE7E15F1A5
label: Intro to controls and patterns
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 290c06a7bac75217c8249821821f081da619c7f3
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6027733"
---
# <a name="intro-to-controls-and-patterns"></a>컨트롤 및 패턴 소개

UWP 앱 개발에서 *컨트롤*은 콘텐츠를 표시하거나 조작을 가능하게 하는 UI 요소입니다. 단추, 입력란, 콤보 상자 등의 컨트롤을 사용하여 데이터를 표시하고 사용자 입력을 받기 위한 앱의 UI를 만듭니다.

> **중요 API**: [Windows.UI.Xaml.Controls 네임스페이스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)

*패턴*은 컨트롤을 수정하거나 새로운 것을 만들기 위해 여러 컨트롤을 조합하는 방법입니다. 예를 들어, [마스터/세부 정보](master-details.md) 패턴은 앱 탐색을 위해 [SplitView](split-view.md) 컨트롤을 사용할 수 있는 하는 방법. 마찬가지로, 탭 패턴을 구현 하는 [NavigationView](navigationview.md) 컨트롤의 템플릿을 사용자 지정할 수 있습니다.

대부분의 경우 컨트롤은 있는 그대로 사용할 수 있습니다. 하지만 XAML 컨트롤은 구조 및 모양과 기능을 분리하기 때문에 다양한 수준의 수정을 통해 요구에 맞게 수정할 수 있습니다. [스타일](../style/index.md) 섹션에서 [XAML 스타일](xaml-styles.md) 및 [컨트롤 템플릿](control-templates.md)을 사용하여 컨트롤을 수정하는 방법을 배울 수 있습니다.

이 섹션에서는 앱 UI를 빌드하는 데 사용할 수 있는 각 XAML 컨트롤에 대한 지침을 제공합니다. 시작을 위해 이 문서는 앱에 컨트롤을 추가하는 방법을 보여 줍니다. 앱에 컨트롤을 사용하기 위한 주요 3단계가 있습니다.

- 컨트롤을 앱 UI에 추가합니다.
- 컨트롤의 속성(예: 너비, 높이, 전경색 등)을 설정합니다.
- 컨트롤 이벤트 처리기에 특정 작업을 수행하는 코드를 추가합니다. 

## <a name="add-a-control"></a>컨트롤 추가
다음과 같은 여러 가지 방법으로 앱에 컨트롤을 추가할 수 있습니다.
 
- Blend for Visual Studio 또는 Microsoft Visual Studio XAML(Extensible Application Markup Language) 디자이너와 같은 디자인 도구를 사용합니다. 
- Visual Studio XAML 편집기에서 XAML 태그에 컨트롤을 추가합니다. 
- 코드에 컨트롤을 추가합니다. 코드에 추가한 컨트롤은 앱이 실행되면 표시되지만 Visual Studio XAML 디자이너에서는 표시되지 않습니다.

Visual Studio에서 앱에 컨트롤을 추가하고 조작할 경우 도구 상자, XAML 디자이너, XAML 편집기 및 속성 창 등 프로그램의 다양한 기능을 사용할 수 있습니다. 

Visual Studio 도구 상자에는 앱에 사용할 수 있는 다양한 컨트롤이 표시됩니다. 앱에 컨트롤을 추가하려면 도구 상자에서 원하는 컨트롤을 두 번 클릭합니다. 예를 들어 TextBox 컨트롤을 두 번 클릭하면 XAML 보기에 이 XAML이 추가됩니다. 

```xaml
<TextBox HorizontalAlignment="Left" Text="TextBox" VerticalAlignment="Top"/>
```

또한 도구 상자에서 XAML 디자이너로 컨트롤을 끌 수 있습니다.

## <a name="set-the-name-of-a-control"></a>컨트롤 이름 설정

코드에서 컨트롤 작업을 하려면 해당 [x:Name](../../xaml-platform/x-name-attribute.md) 특성을 설정하고 이름으로 컨트롤을 참조합니다. 컨트롤 이름은 Visual Studio 속성 창이나 XAML에서 설정할 수 있습니다. 속성 창의 맨 위에 있는 이름 입력란을 사용하여 현재 선택한 컨트롤의 이름을 설정하는 방법은 다음과 같습니다.

컨트롤의 이름을 지정하려면
1. 이름을 지정할 요소를 선택합니다.
2. 속성 패널의 Name 입력란에 이름을 입력합니다.
3. Enter 키를 눌러 이름을 커밋합니다.

![Visual Studio 디자이너의 이름 속성](images/add-controls-control-name-designer.png)

XAML 편집기에서 x:Name 특성을 변경하여 컨트롤 이름을 설정하는 방법은 다음과 같습니다.

```xaml
<Button x:Name="Button1" Content="Button"/>
```

## <a name="set-the-control-properties"></a>컨트롤 속성 설정 

속성을 사용하여 컨트롤의 모양, 콘텐츠 및 기타 특성을 지정합니다. 디자인 도구를 사용하여 컨트롤을 추가할 경우 크기, 위치 및 콘텐츠를 제어하는 일부 속성은 Visual Studio에서 사용자를 위해 설정할 수 있습니다. 디자인 보기에서 컨트롤을 선택하고 조작하여 Width, Height, Margin 등의 속성을 변경할 수 있습니다. 다음 그림에서는 디자인 보기에서 사용할 수 있는 몇 가지 크기 조정 도구를 보여 줍니다. 

![Visual Studio 디자이너의 크기 조정 도구](images/add-controls-resizing-designer.png)

컨트롤의 크기와 위치가 자동으로 조정되기를 원할 수 있습니다. 이 경우 Visual Studio에서 사용자를 위해 설정한 크기 및 위치 속성을 다시 설정할 수 있습니다.

속성을 다시 설정하려면
1. 속성 패널에서 속성 값 옆에 있는 속성 마커를 클릭합니다. 속성 메뉴가 열립니다.
2. 속성 메뉴에서 다시 설정을 클릭합니다.

![Visual Studio 속성 초기화 메뉴 옵션](images/add-controls-property-reset.png)

컨트롤 속성은 속성 창, XAML 또는 코드에서 설정할 수 있습니다. 예를 들어 Button의 전경색을 변경하려면 컨트롤의 Foreground 속성을 설정합니다. 다음 그림에서는 속성 창의 색 선택을 사용하여 Foreground 속성을 설정하는 방법을 보여 줍니다. 

![Visual Studio 디자이너의 색 선택](images/add-controls-foreground-designer.png)

XAML 편집기에서 Foreground 속성을 설정하는 방법은 다음과 같습니다. 여기에 표시된 Visual Studio IntelliSense 창은 구문 관련 도움말을 제공합니다. 

![XAML 1부의 Intellisense](images/add-controls-foreground-xaml.png)

![XAML 2부의 Intellisense](images/add-controls-foreground-xaml-2.png)

Foreground 속성을 설정하면 XAML은 다음과 같이 됩니다. 

```xaml
<Button x:Name="Button1" Content="Button" 
        HorizontalAlignment="Left" VerticalAlignment="Top"
        Foreground="Beige"/>
```

코드에서 Foreground 속성을 설정하는 방법은 다음과 같습니다. 

```csharp
Button1.Foreground = new SolidColorBrush(Windows.UI.Colors.Beige);
```

## <a name="create-an-event-handler"></a>이벤트 처리기 만들기 

각 컨트롤에는 사용자의 동작이나 기타 앱 변경 사항에 응답할 수 있는 이벤트가 포함되어 있습니다. 예를 들어 Button 컨트롤에는 사용자가 Button을 클릭하면 발생하는 Click 이벤트가 있습니다. 이벤트를 처리하려면 이벤트 처리기라고 하는 메서드를 만듭니다. XAML 또는 코드에서, 속성 창에서 컨트롤의 이벤트와 이벤트 처리기 메서드를 연결할 수 있습니다. 이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](../../xaml-platform/events-and-routed-events-overview.md)를 참조하세요.

이벤트 처리기를 만들려면 컨트롤을 선택하고 속성 창의 맨 위에 있는 이벤트 탭을 클릭합니다. 속성 창에는 해당 컨트롤에 사용할 수 있는 모든 이벤트가 나열됩니다. Button에 대한 몇 가지 이벤트는 다음과 같습니다.

![Visual Studio 이벤트 목록](images/add-controls-add-event-designer.png)

기본 이름으로 이벤트 처리기를 만들려면 속성 창에서 이벤트 이름 옆에 있는 입력란을 두 번 클릭합니다. 사용자 지정 이름으로 이벤트 처리기를 만들려면 입력란에 원하는 이름을 입력하고 &lt;Enter&gt; 키를 누릅니다. 이벤트 처리기가 만들어지고 코드 숨김 파일이 코드 편집기에서 열립니다. 이벤트 처리기 메서드에는 두 개의 매개 변수가 있습니다. 첫 번째 매개 변수는 처리기가 연결된 개체에 대한 참조인 `sender`입니다. `sender` 매개 변수는 **Object** 유형입니다. `sender` 개체 자체에 대한 상태를 확인하거나 변경하려는 경우 일반적으로 `sender`를 보다 정확한 유형으로 캐스팅합니다. 고유한 앱 디자인에 따라 처리기가 연결된 위치를 기준으로 `sender`를 캐스팅해도 괜찮은 형식을 기대할 수 있습니다. 두 번째 값은 일반적으로 `e` 또는 `args` 매개 변수로 서명에 나타나는 이벤트 데이터입니다.

`Button1`이라는 Button의 Click 이벤트를 처리하는 코드는 다음과 같습니다. 단추를 클릭하면 클릭한 Button의 Foreground 속성이 파란색으로 설정됩니다. 

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Button b = (Button)sender;
    b.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
}
```

또한 XAML에서 이벤트 처리기를 연결할 수 있습니다. XAML 편집기에서 처리할 이벤트의 이름을 입력합니다. 입력을 시작하면 Visual Studio에 IntelliSense 창이 표시됩니다. 이벤트를 지정한 후 IntelliSense 창에서 `<New Event Handler>`를 두 번 클릭하여 기본 이름으로 새 이벤트 처리기를 만들거나 목록에서 기존 이벤트 처리기를 선택할 수 있습니다. 

표시되는 IntelliSense 창은 다음과 같습니다. 새 이벤트 처리기를 만들거나 기존 이벤트 처리기를 선택하는 데 도움이 됩니다.

![Click 이벤트에 대한 Intellisense](images/add-controls-add-event-xaml.png)

이 예에서는 XAML에서 Click 이벤트와 Button_Click이라는 이벤트 처리기를 연결하는 방법을 보여 줍니다. 

```xaml
<Button Name="Button1" Content="Button" Click="Button_Click"/>
```

또한 코드 숨김 파일을 사용하여 이벤트를 해당 이벤트 처리기와 연결할 수도 있습니다. 코드에서 이벤트 처리기를 연결하는 방법은 다음과 같습니다.

```csharp
Button1.Click += new RoutedEventHandler(Button_Click);
```

## <a name="related-topics"></a>관련 항목

-   [기능별 컨트롤 인덱스](controls-by-function.md)
-   [Windows.UI.Xaml.Controls 네임스페이스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)
-   [레이아웃](../layout/index.md)
-   [스타일](../style/index.md)
-   [유용성](../usability/index.md)
