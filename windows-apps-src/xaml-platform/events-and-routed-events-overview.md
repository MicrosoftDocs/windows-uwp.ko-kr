---
description: 'C #, Visual Basic 또는 Visual C++ 구성 요소 확장 (c + +/CX)을 프로그래밍 언어로 사용 하 고 UI 정의를 위한 XAML을 사용 하는 경우 Windows 런타임 앱에서 이벤트의 프로그래밍 개념을 설명 합니다.'
title: 이벤트 및 라우트된 이벤트 개요
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67ee1f31e1480e0aa805e161433976d5e789d00c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155097"
---
# <a name="events-and-routed-events-overview"></a>이벤트 및 라우트된 이벤트 개요

**중요 API**
- [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)
- [**System.windows.routedeventargs.handled**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

C #, Visual Basic 또는 Visual C++ 구성 요소 확장 (c + +/CX)을 프로그래밍 언어로 사용 하 고 UI 정의를 위한 XAML을 사용 하는 경우 Windows 런타임 앱에서 이벤트의 프로그래밍 개념을 설명 합니다. XAML에서 UI 요소 선언의 일부로 이벤트 처리기를 할당 하거나 코드에 처리기를 추가할 수 있습니다. Windows 런타임는 *라우트된 이벤트*를 지원 합니다. 특정 입력 이벤트 및 데이터 이벤트는 이벤트를 발생 시킨 개체 이외의 개체에서 처리할 수 있습니다. 라우트된 이벤트는 컨트롤 템플릿을 정의 하거나 페이지 또는 레이아웃 컨테이너를 사용 하는 경우에 유용 합니다.

## <a name="events-as-a-programming-concept"></a>프로그래밍 개념인 이벤트

일반적으로 Windows 런타임 앱을 프로그래밍할 때 이벤트 개념은 가장 인기 있는 프로그래밍 언어의 이벤트 모델과 비슷합니다. Microsoft .NET 또는 c + + 이벤트를 이미 사용 하는 방법을 알고 있는 경우 시작 하는 것이 좋습니다. 그러나 처리기를 연결 하는 것과 같은 몇 가지 기본 작업을 수행 하는 이벤트 모델 개념에 대해 알 필요가 없습니다.

C #, Visual Basic 또는 c + +/CX를 프로그래밍 언어로 사용 하는 경우 UI는 태그 (XAML)로 정의 됩니다. XAML 태그 구문에서 태그 요소와 런타임 코드 엔터티 간에 이벤트를 연결 하는 몇 가지 원칙이 ASP.NET 또는 HTML5와 같은 다른 웹 기술과 비슷합니다.

**참고**    XAML 정의 UI에 대 한 런타임 논리를 제공 하는 코드를 종종 *코드 숨김이* 나 코드 숨김이 라고도 합니다. Microsoft Visual Studio 솔루션 뷰에서는이 관계가 그래픽으로 표시 되 고 코드 숨김이 참조 하는 XAML 페이지와 종속 파일 및 중첩 파일이 됩니다.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>단추를 클릭 합니다. Click: 이벤트 및 XAML 소개

Windows 런타임 앱에 대 한 가장 일반적인 프로그래밍 작업 중 하나는 UI에 대 한 사용자 입력을 캡처하는 것입니다. 예를 들어 사용자가 정보를 제출 하거나 상태를 변경 하기 위해 클릭 해야 하는 단추가 UI에 있을 수 있습니다.

XAML을 생성 하 여 Windows 런타임 앱에 대 한 UI를 정의 합니다. 이 XAML은 일반적으로 Visual Studio의 디자인 화면에서 출력 됩니다. 또한 일반 텍스트 편집기나 타사 XAML 편집기에서 XAML을 작성할 수도 있습니다. 해당 XAML을 생성 하는 동안 UI 요소의 속성 값을 설정 하는 다른 모든 XAML 특성을 정의 하는 동시에 개별 UI 요소에 대 한 이벤트 처리기를 연결할 수 있습니다.

XAML로 이벤트를 연결 하려면 이미 정의 했거나 나중에 코드 숨김으로 정의 하는 처리기 메서드의 문자열 형식 이름을 지정 합니다. 예를 들어이 XAML은 특성으로 할당 된 다른 속성 ([x:Name 특성](x-name-attribute.md), [**콘텐츠**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content))을 사용 하 여 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 개체를 정의 하 고 라는 메서드를 참조 하 여 단추의 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트에 대 한 처리기를 만듭니다 `ShowUpdatesButton_Click` .

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**팁**  *이벤트 와이어링* 은 프로그래밍 용어입니다. 이벤트 발생이 명명 된 처리기 메서드를 호출 해야 함을 나타내는 프로세스 또는 코드를 나타냅니다. 대부분의 절차 코드 모델에서 이벤트 연결은 이벤트와 메서드를 모두 포함 하는 암시적 또는 명시적 "AddHandler" 코드 이며 일반적으로 대상 개체 인스턴스와 관련이 있습니다. XAML에서 "AddHandler"는 암시적 이며 이벤트 와이어링은 전적으로 이벤트 이름을 개체 요소의 특성 이름으로 지정 하 고 해당 특성의 값으로 처리기 이름을 지정 합니다.

모든 앱의 코드 및 코드 숨김으로 사용 중인 프로그래밍 언어로 실제 처리기를 작성 합니다. 특성을 사용 하 여 `Click="ShowUpdatesButton_Click"` xaml이 태그 컴파일 및 구문 분석 되 면, IDE의 빌드 작업에서 xaml 마크업 컴파일 단계와 앱이 로드 될 때 최종 xaml 구문 분석 모두 `ShowUpdatesButton_Click` 응용 프로그램 코드의 일부로 이름이 지정 된 메서드를 찾을 수 있는 계약을 만들었습니다. `ShowUpdatesButton_Click` 는 [**클릭**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트 처리기에 대해 대리자를 기반으로 하는 호환 되는 메서드 시그니처를 구현 하는 메서드 여야 합니다. 예를 들어이 코드는 처리기를 정의 `ShowUpdatesButton_Click` 합니다.

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

이 예제에서 메서드는 `ShowUpdatesButton_Click` [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 대리자를 기반으로 합니다. 이 대리자는 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 메서드의 구문에 이라는 대리자가 표시 되기 때문에 사용할 대리자 임을 알 수 있습니다.

**팁**    Visual Studio는 XAML을 편집 하는 동안 이벤트 처리기의 이름을 지정 하 고 처리기 메서드를 정의 하는 편리한 방법을 제공 합니다. XAML 텍스트 편집기에서 이벤트의 특성 이름을 제공 하는 경우 Microsoft IntelliSense 목록에 표시 될 때까지 잠시 기다립니다. 목록에서 ** &lt; 새 이벤트 처리기 &gt; ** 를 클릭 하면 요소의 **x:Name** (또는 유형 이름), 이벤트 이름 및 숫자 접미사에 따라 메서드 이름을 제안 하는 Microsoft Visual Studio. 그런 다음 선택한 이벤트 처리기 이름을 마우스 오른쪽 단추로 클릭 하 고 **이벤트 처리기로 이동을**클릭 합니다. 이렇게 하면 XAML 페이지에 대 한 코드 숨겨진 파일의 코드 편집기 보기에 표시 된 대로 새로 삽입 된 이벤트 처리기 정의로 직접 이동 합니다. 이벤트 처리기에는 *보낸 사람* 매개 변수 및 이벤트에서 사용 하는 이벤트 데이터 클래스를 포함 하 여 올바른 서명이 이미 있습니다. 또한 올바른 시그니처가 있는 처리기 메서드가 코드 숨김으로 이미 존재 하는 경우 해당 메서드의 이름이 ** &lt; 새 이벤트 처리기 &gt; ** 옵션과 함께 자동 완성 드롭다운에 표시 됩니다. IntelliSense 목록 항목을 클릭 하는 대신 Tab 키를 바로 가기로 누를 수도 있습니다.

## <a name="defining-an-event-handler"></a>이벤트 처리기 정의

UI 요소이 고 XAML로 선언 된 개체의 경우, 이벤트 처리기 코드는 XAML 페이지의 코드 숨김으로 사용 되는 partial 클래스에서 정의 됩니다. 이벤트 처리기는 XAML과 연결 된 partial 클래스의 일부로 작성 하는 메서드입니다. 이러한 이벤트 처리기는 특정 이벤트에서 사용 하는 대리자를 기반으로 합니다. 이벤트 처리기 메서드는 public 또는 private 일 수 있습니다. XAML에서 만든 처리기와 인스턴스는 궁극적으로 코드 생성에 의해 조인 되기 때문에 개인 액세스가 작동 합니다. 일반적으로 클래스에서 이벤트 처리기 메서드를 전용으로 설정 하는 것이 좋습니다.

**참고**    C + +에 대 한 이벤트 처리기는 partial 클래스에서 정의 되지 않으며, 헤더에서 전용 클래스 멤버로 선언 됩니다. C + + 프로젝트에 대 한 빌드 작업은 c + +의 XAML 형식 시스템 및 코드를 사용 하는 모델을 지 원하는 코드를 생성 합니다.

### <a name="the-sender-parameter-and-event-data"></a>*발신자* 매개 변수 및 이벤트 데이터

이벤트에 대해 작성 하는 처리기는 처리기가 호출 되는 각 사례에 대 한 입력으로 사용할 수 있는 두 값에 액세스할 수 있습니다. 첫 번째 값은 처리기가 연결 된 개체에 대 한 참조 인 *sender*입니다. *Sender* 매개 변수는 기본 **개체** 형식으로 입력 됩니다. 일반적인 방법은 *발신자* 를 보다 정확한 형식으로 캐스팅 하는 것입니다. 이 기법은 *발신자* 개체 자체의 상태를 확인 하거나 변경 해야 하는 경우에 유용 합니다. 사용자 고유의 앱 디자인을 기반으로 하 여 처리기가 연결 된 위치 또는 기타 디자인 특성에 따라 *발신자* 를로 캐스팅할 수 있는 안전한 형식을 알고 있습니다.

두 번째 값은 일반적으로 구문 정의에 *e* 매개 변수로 표시 되는 이벤트 데이터입니다. 처리 중인 특정 이벤트에 할당 된 대리자의 *e* 매개 변수를 확인 하 고 Visual Studio에서 IntelliSense 또는 개체 브라우저를 사용 하 여 이벤트 데이터에 대 한 속성을 확인할 수 있습니다. 또는 Windows 런타임 참조 설명서를 사용할 수 있습니다.

일부 이벤트의 경우 이벤트 데이터의 특정 속성 값은 이벤트가 발생 한 것을 인식 하는 것 만큼 중요 합니다. 이는 특히 입력 이벤트의 경우에 해당 합니다. 포인터 이벤트의 경우 이벤트가 발생 했을 때 포인터의 위치는 중요할 수 있습니다. 키보드 이벤트의 경우 가능한 모든 키 누름은 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 및 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트를 발생 시킵니다. 사용자가 누른 키를 확인 하려면 이벤트 처리기에서 사용할 수 있는 [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) 에 액세스 해야 합니다. 입력 이벤트를 처리 하는 방법에 대 한 자세한 내용은 [키보드 상호 작용](../design/input/keyboard-interactions.md) 및 [포인터 입력 처리](../design/input/handle-pointer-input.md)를 참조 하세요. 입력 이벤트 및 입력 시나리오에는 포인터 이벤트에 대 한 포인터 캡처, 키보드 이벤트에 대 한 보조키 및 플랫폼 키 코드와 같이이 항목에서 다루지 않는 추가 고려 사항이 포함 되어 있는 경우가 많습니다.

### <a name="event-handlers-that-use-the-async-pattern"></a>**비동기** 패턴을 사용 하는 이벤트 처리기

경우에 따라 이벤트 처리기 내에서 **비동기** 패턴을 사용 하는 api를 사용 하는 것이 좋습니다. 예를 들어 [**Appbar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) 의 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 를 사용 하 여 파일 선택기를 표시 하 고 상호 작용할 수 있습니다. 그러나 대부분의 파일 선택 Api는 비동기입니다. **비동기**/awaitable 범위 내에서 호출 되어야 하며, 컴파일러에서이를 적용 합니다. 이렇게 할 수 있는 작업은 처리기가 이제 **비동기** **void**가 되도록 이벤트 처리기에 **async** 키워드를 추가 하는 것입니다. 이제 이벤트 처리기에서 **비동기**/awaitable 호출을 수행할 수 있습니다.

**비동기** 패턴을 사용 하 여 사용자 상호 작용 이벤트를 처리 하는 예제는 [파일 액세스 및 선택](/previous-versions/windows/apps/jj655411(v=win.10)) ([c # 또는 Visual Basic 시리즈를 사용 하 여 첫 번째 Windows 런타임 앱 만들기](/previous-versions/windows/apps/hh974581(v=win.10)) 의 일부)을 참조 하세요. [C에서 비동기 Api 호출)도 참조 하세요.

## <a name="adding-event-handlers-in-code"></a>코드에 이벤트 처리기 추가

XAML은 개체에 이벤트 처리기를 할당 하는 유일한 방법이 아닙니다. XAML에서 사용할 수 없는 개체를 비롯 하 여 코드에서 지정 된 개체에 이벤트 처리기를 추가 하려면 이벤트 처리기를 추가 하기 위한 언어별 구문을 사용할 수 있습니다.

C #에서 구문은 연산자를 사용 하는 것입니다 `+=` . 연산자의 우변에 있는 이벤트 처리기 메서드 이름을 참조 하 여 처리기를 등록 합니다.

코드를 사용 하 여 런타임 UI에 표시 되는 개체에 이벤트 처리기를 추가 하는 경우 일반적인 방법은 개체 수명 [**이벤트 또는 콜백에**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)대 한 응답에 이러한 처리기를 추가 하는 것입니다 .이는 관련 개체에 대 한 이벤트 처리기가 런타임에 사용자가 시작 하는 이벤트에 대해 준비 되도록 하 [**는 것입니다**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) . 이 예제에서는 페이지 구조의 XAML 개요를 표시 한 다음 개체에 이벤트 처리기를 추가 하기 위한 c # 언어 구문을 제공 합니다.

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**참고**    더 자세한 구문이 있습니다. 2005에서는 컴파일러가 새 대리자 인스턴스를 유추 하 고 보다 단순한 이전 구문을 사용할 수 있도록 하는 대리자 유추 라는 기능을 추가 했습니다. 자세한 구문은 이전 예제와 기능적으로 동일 하지만 새 대리자 인스턴스를 등록 하기 전에 명시적으로 만들어 대리자 유추를 활용 하지 않습니다. 이 명시적 구문은 일반적이 아니지만 일부 코드 예제에서 볼 수 있습니다.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 구문에는 두 가지 가능성이 있습니다. 한 가지는 c # 구문을 병렬 처리 하 고 인스턴스에 직접 처리기를 연결 하는 것입니다. 이렇게 하려면 **AddHandler** 키워드와 처리기 메서드 이름을 역참조 하는 **AddressOf** 연산자도 필요 합니다.

Visual Basic 구문에 대 한 다른 옵션은 이벤트 처리기에서 **Handles** 키워드를 사용 하는 것입니다. 이 기술은 처리기가 로드 시 개체에 존재 하 고 개체 수명 내내 지속 되는 경우에 적합 합니다. XAML에 정의 된 개체에 대해 **핸들** 을 사용 하려면 **이름을**  /  **x:Name**으로 지정 해야 합니다. 이 이름은 인스턴스를 **처리** 하는 데 필요한 인스턴스 한정자가 됩니다 *.* 이 경우 다른 이벤트 처리기의 연결을 시작 하는 개체 수명 기반 이벤트 처리기가 필요 하지 않습니다. **핸들** 연결은 XAML 페이지를 컴파일할 때 생성 됩니다.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**참고**    Visual Studio 및 XAML 디자인 화면은 일반적으로 **Handles** 키워드 대신 인스턴스 처리 기법을 승격 시킵니다. 이는 XAML로 이벤트 처리기를 설정 하는 것이 일반적인 디자이너-개발자 워크플로의 일부이 고 **Handles** 키워드 기술은 xaml에서 이벤트 처리기를 연결 하는 것과 호환 되지 않기 때문입니다.

C + +/CX에서는 **+=** 구문도 사용 하지만 기본 c # 형식과는 차이가 있습니다.

- 대리자 유추는 존재 하지 않으므로 대리자 인스턴스에 대해 **ref new** 를 사용 해야 합니다.
- Delegate 생성자에는 두 개의 매개 변수가 있으며,이 경우에는 대상 개체가 첫 번째 매개 변수로 필요 합니다. 일반적으로 **이**를 지정 합니다.
- Delegate 생성자에는 메서드 주소가 두 번째 매개 변수로 필요 하므로 **&** 참조 연산자가 메서드 이름 앞에와 야 합니다.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>코드에서 이벤트 처리기 제거

코드에서 이벤트 처리기를 추가한 경우에도 코드에서 이벤트 처리기를 제거 하는 것은 일반적으로 필요 하지 않습니다. 페이지 및 컨트롤과 같은 대부분의 Windows 런타임 개체에 대 한 개체 수명 동작은 개체를 주 [**창과**](/uwp/api/Windows.UI.Xaml.Window) 시각적 트리에서 분리 하는 경우 개체를 삭제 하 고 대리자 참조는 제거 합니다. .NET은 가비지 수집을 통해이를 수행 하 고 c + +/CX와 Windows 런타임는 기본적으로 약한 참조를 사용 합니다.

이벤트 처리기를 명시적으로 제거 하려는 드문 경우도 있습니다. 여기에는 다음이 포함됩니다.

- 정적 이벤트에 대해 추가한 처리기로, 기존 방식으로 가비지 수집을 가져올 수 없습니다. Windows 런타임 API의 정적 이벤트 예는 [**CompositionTarget**](/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) 및 [**클립보드**](/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard) 클래스의 이벤트입니다.
- 처리기 제거 타이밍을 즉시 적용할 테스트 코드 또는 런타임에 이벤트에 대 한 이전/새 이벤트 처리기를 교환할 코드를 테스트 합니다.
- 사용자 지정 **제거** 접근자의 구현입니다.
- 사용자 지정 정적 이벤트.
- 페이지 탐색 처리기

[**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement.unloaded) 또는 [**NavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) 는 상태 관리 및 개체 수명에서 적절 한 위치를 가진 이벤트 트리거로 서 다른 이벤트에 대 한 처리기를 제거 하는 데 사용할 수 있습니다.

예를 들어,이 코드를 사용 하 여 대상 개체 **textBlock1** 에서 **textBlock1 \_ pointerentered** 된 이벤트 처리기를 제거할 수 있습니다.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

XAML 특성을 통해 이벤트가 추가 된 사례에 대 한 처리기를 제거할 수도 있습니다. 즉, 처리기가 생성 된 코드에 추가 되었습니다. 처리기가 연결 된 요소에 대해 나중에 코드에 대 한 개체 참조를 제공 하기 때문에이 작업을 수행 **하는 것** 이 더 쉽습니다. 그러나 개체에 **이름이**없는 경우 필요한 개체 참조를 찾기 위해 개체 트리를 탐색할 수도 있습니다.

C + +/CX에서 이벤트 처리기를 제거 해야 하는 경우 이벤트 처리기 등록의 반환 값에서 받은 등록 토큰이 필요 합니다 `+=` . C + +/CX 구문에서 등록 취소의 오른쪽에 사용 하는 값 `-=` 이 메서드 이름이 아닌 토큰 이기 때문입니다. C + +/cx의 경우 c + +/CX 생성 코드에서 토큰을 저장 하지 않기 때문에 XAML 특성으로 추가 된 처리기를 제거할 수 없습니다.

## <a name="routed-events"></a>라우트된 이벤트

C #, Microsoft Visual Basic 또는 c + +/CX를 사용 하는 Windows 런타임는 대부분의 UI 요소에 있는 이벤트 집합에 대해 라우트된 이벤트의 개념을 지원 합니다. 이러한 이벤트는 입력 및 사용자 상호 작용 시나리오에 사용 되며 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 기본 클래스에서 구현 됩니다. 라우트된 이벤트의 입력 이벤트 목록은 다음과 같습니다.

- [**BringIntoViewRequested**](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**컨텍스트 취소 됨**](/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested 됨**](/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**System.windows.dragdrop.dragenter>**](/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**System.windows.dragdrop.dragleave>**](/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**System.windows.uielement.dragover>**](/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**그림자**](/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**Get이상 포커스**](/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**System.windows.uielement.gotfocus>**](/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Ctrl**](/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**System.windows.uielement.manipulationcompleted>**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**System.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**System.windows.uielement.manipulationinertiastarting>**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**System.windows.uielement.manipulationstarted>**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**System.windows.uielement.manipulationstarting>**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered 됨**](/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved 됨**](/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**System.windows.forms.control.previewkeydown>**](/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**오른쪽 탭**](/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**탭**](/uwp/api/windows.ui.xaml.uielement.tapped)

라우트된 이벤트는 자식 개체에서 개체 트리의 연속 된 각 부모 개체에 잠재적으로 (*라우트된*) 잠재적으로 전달 되는 이벤트입니다. UI의 XAML 구조는 XAML의 루트 요소가 되는 트리의 루트를 사용 하 여이 트리의 근사치를 계산 합니다. 개체 트리에는 속성 요소 태그와 같은 XAML 언어 기능이 포함 되지 않기 때문에 진정한 개체 트리는 XAML 요소 중첩과 약간 다를 수 있습니다. 라우트된 이벤트를이를 포함 하는 부모 개체 요소에 대해 이벤트를 발생 시키는 모든 XAML 개체 요소 자식 요소에서 *버블링* 으로 conceive 수 있습니다. 이벤트와 이벤트 데이터는 이벤트 경로를 따라 여러 개체에서 처리 될 수 있습니다. 요소에 처리기가 없는 경우 루트 요소에 도달할 때까지 경로는 잠재적으로 계속 진행 됩니다.

DHTML(동적 HTML) 또는 HTML5와 같은 웹 기술을 알고 있는 경우 이미 *버블링* 이벤트 개념에 익숙할 수 있습니다.

라우트된 이벤트가 해당 이벤트 경로를 통해 버블링 되 면 연결 된 이벤트 처리기는 모두 이벤트 데이터의 공유 인스턴스에 액세스 합니다. 따라서 처리기가 이벤트 데이터를 쓸 수 있는 경우에는 이벤트 데이터에 대 한 모든 변경 내용이 다음 처리기로 전달 되 고 이벤트의 원래 이벤트 데이터를 더 이상 나타내지 않을 수 있습니다. 이벤트에 라우트된 이벤트 동작이 있는 경우 참조 설명서에는 라우트된 동작에 대 한 설명 또는 다른 표기법이 포함 됩니다.

### <a name="the-originalsource-property-of-routedeventargs"></a>**System.windows.routedeventargs.handled** 의 **originalsource** 속성

이벤트가 이벤트 경로를 버블링 할 때 *발신자* 는 이벤트를 발생 시키는 개체와 더 이상 동일 하지 않습니다. 대신, *발신자* 는 호출 되는 처리기가 연결 된 개체입니다.

경우에 따라 *발신자* 는 관심이 없으며 포인터 이벤트가 발생 했을 때 포인터가 가리키는 가능한 자식 개체 또는 사용자가 키보드 키를 누를 때 포커스가 있는 큰 UI의 개체와 같은 정보에 관심이 있습니다. 이러한 경우 [**Originalsource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) 속성의 값을 사용할 수 있습니다. 경로에 있는 모든 지점에서 **Originalsource** 는 처리기가 연결 된 개체 대신 이벤트를 발생 시킨 원래 개체를 보고 합니다. 그러나 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 입력 이벤트의 경우 원래 개체는 종종 페이지 수준 UI 정의 XAML에 즉시 표시 되지 않는 개체입니다. 대신 원래 원본 개체는 컨트롤의 템플릿 부분이 될 수 있습니다. 예를 들어 사용자가 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button)에 대 한 포인터를 포인터로 가리키면 대부분의 포인터 이벤트에서 **originalsource** 는 **단추** 자체가 아니라 [**템플릿의**](/uwp/api/windows.ui.xaml.controls.control.template) [**테두리**](/uwp/api/Windows.UI.Xaml.Controls.Border) 템플릿 파트입니다.

**팁**    입력 이벤트 버블링은 템플릿 기반 컨트롤을 만드는 경우에 특히 유용 합니다. 템플릿이 있는 모든 컨트롤에는 소비자가 적용 한 새 템플릿이 있을 수 있습니다. 작업 템플릿을 다시 만들려고 하는 소비자는 실수로 기본 템플릿에서 선언 된 일부 이벤트 처리를 제거할 수 있습니다. 클래스 정의에서 [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) 재정의의 일부로 처리기를 연결 하 여 컨트롤 수준 이벤트 처리를 계속 제공할 수 있습니다. 그런 다음 인스턴스화할 때 컨트롤의 루트까지 버블링 되는 입력 이벤트를 catch 할 수 있습니다.

### <a name="the-handled-property"></a>**처리** 된 속성

특정 라우트된 이벤트에 대 한 여러 이벤트 데이터 클래스는 **처리**된 라는 속성을 포함 합니다. 예는 [**PointerRoutedEventArgs**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**KeyRoutedEventArgs**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled), [**drageventargs.**](/uwp/api/windows.ui.xaml.drageventargs.handled)를 참조 하십시오. 모든 경우에 **처리** 되는 부울 속성은 설정 가능 부울 속성입니다.

**처리** 된 속성을 **true** 로 설정 하면 이벤트 시스템 동작에 영향을 미칩니다. **처리** **된 경우**대부분의 이벤트 처리기에 대해 라우팅이 중지 됩니다. 이벤트는 경로를 따라 진행 하지 않으므로 특정 이벤트 사례의 다른 연결 된 처리기에 게 알립니다. "처리 됨"은 이벤트의 컨텍스트와 앱이 응답 하는 방식에 따라 결정 됩니다. 기본적으로 **처리** 된는 응용 프로그램 코드에서 어떤 컨테이너에도 버블링 하지 않아도 되도록 응용 프로그램 코드를 설정할 수 있는 간단한 프로토콜입니다. 앱 논리는 수행 해야 하는 작업을 처리 합니다. 반대로, 기본 제공 시스템이 나 제어 동작을 수행할 수 있도록 버블링 해야 하는 이벤트를 처리 하지 않도록 주의 해야 합니다. 예를 들어 선택 컨트롤의 요소 또는 항목 내에서 하위 수준 이벤트를 처리 하는 것은 좋지 않을 수 있습니다. 선택 항목이 변경 될 것 이라는 것을 확인 하기 위해 선택 컨트롤에서 입력 이벤트를 찾을 수 있습니다.

라우트된 이벤트 중 일부는 이러한 방식으로 경로를 취소할 수 없으며 **처리** 된 속성이 없기 때문에이를 알 수 있습니다. 예를 들어 [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) 와 [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) 는 거품형 이지만 항상 루트에 대 한 모든 방식으로 버블링 되며 해당 이벤트 데이터 클래스에는 해당 동작에 영향을 줄 수 있는 **처리** 된 속성이 없습니다.

##  <a name="input-event-handlers-in-controls"></a>컨트롤의 입력 이벤트 처리기

특정 Windows 런타임 컨트롤은 종종 입력 이벤트에 대해 **처리** 된 개념을 내부적으로 사용 합니다. 사용자 코드에서 처리할 수 없기 때문에이로 인해 입력 이벤트가 발생 하지 않는 것 처럼 보일 수 있습니다. 예를 들어 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 클래스에는 일반 입력 이벤트를 의도적으로 처리 하는 논리가 포함 [**되어 있습니다.**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) 단추가 포커스를 가질 때 단추를 호출할 수 있는 Enter 키와 같은 키를 처리 하는 등의 기타 입력 모드 뿐만 아니라 포인터 누름 입력으로 시작 되는 [**클릭**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트를 발생 하기 때문입니다. **단추의**클래스 디자인을 위해 원시 입력 이벤트는 개념적으로 처리 되며, 사용자 코드와 같은 클래스 소비자는 컨트롤 관련 **Click** 이벤트와 상호 작용할 수 있습니다. Windows 런타임 API 참조의 특정 컨트롤 클래스에 대 한 항목에서는 클래스에서 구현 하는 이벤트 처리 동작에 대해 자주 확인 합니다. 경우 **에**따라_이벤트_ 메서드를 재정의 하 여 동작을 변경할 수 있습니다. 예를 들어 [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown)를 재정의 하 여 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 파생 클래스가 키 입력에 반응 하는 방식을 변경할 수 있습니다.

##  <a name="registering-handlers-for-already-handled-routed-events"></a>이미 처리 되는 라우트된 이벤트에 대 한 처리기 등록

이전에는를 **true** 로 설정 하면 대부분의 **처리기가 호출** 되지 않습니다. 그러나 [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) 메서드는 경로 앞부분의 다른 처리기가 공유 이벤트 데이터에서 **true** **로 설정 된 경우에도** 항상 경로에 대해 호출 되는 처리기를 연결할 수 있는 기술을 제공 합니다. 이 기술은 사용 중인 컨트롤이 내부 합성에서 이벤트를 처리 하거나 컨트롤별 논리에 대해 이벤트를 처리 하는 경우에 유용 합니다. 그러나 컨트롤 인스턴스나 앱 UI에서 응답 하는 것도 좋습니다. 그러나이 기술은 **처리** 의 목적과 일치 하지 않을 수 있으므로 주의 해 서 사용 해야 합니다.

해당 라우트된 이벤트 식별자가 있는 라우트된 이벤트만 [**addhandler 이벤트 처리 기법을 사용할**](/uwp/api/windows.ui.xaml.uielement.addhandler) 수 있습니다. 식별자는 **addhandler** 메서드의 필수 입력 이기 때문입니다. 라우트된 이벤트 식별자를 사용할 수 있는 이벤트 목록은 [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) 에 대 한 참조 설명서를 참조 하세요. 대부분의 경우이는 앞에서 설명한 것과 동일한 라우트된 이벤트 목록입니다. 예외는 목록에서 [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) 와 [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) 의 마지막 두 가지에 라우트된 이벤트 식별자가 없으므로 해당 식별자에 대해 **AddHandler** 를 사용할 수 없다는 것입니다.

## <a name="routed-events-outside-the-visual-tree"></a>시각적 트리 외부의 라우트된 이벤트

특정 개체는 주 시각적 개체와의 관계에 참여 합니다. 기본 시각적 트리와 개념적으로 주 시각적 개체에 대 한 오버레이가 있는 것과 같습니다. 이러한 개체는 모든 트리 요소를 시각적 루트에 연결 하는 일반적인 부모-자식 관계의 일부가 아닙니다. 표시 되는 [**팝업**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) 또는 [**도구 설명**](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)의 경우입니다. **팝업** 또는 **도구 설명**에서 라우트된 이벤트를 처리 **하려면 Popup 또는** **tooltip 요소가 아니라** **popup** 또는 **tooltip** 내에 있는 특정 UI 요소에 처리기를 추가 합니다. **팝업** 또는 **도구 설명** 콘텐츠에 대해 수행 되는 모든 합성 내에서 라우팅을 사용 하지 마세요. 라우트된 이벤트에 대 한 이벤트 라우팅은 주 시각적 트리를 따라만 작동 하기 때문입니다. 팝업 **또는** **도구 설명은** 자회사 UI 요소의 부모로 간주 되지 않으며, **popup** 기본 배경과 같은 항목을 입력 이벤트의 캡처 영역으로 사용 하려고 하는 경우에도 라우트된 이벤트를 수신 하지 않습니다.

## <a name="hit-testing-and-input-events"></a>적중 횟수 테스트 및 입력 이벤트

UI에서 요소가 마우스, 터치 및 스타일러스 입력에 표시 되는지 여부를 확인 하는 것을 *적중 테스트*라고 합니다. 터치 동작의 경우 터치 작업으로 인해 발생 하는 상호 작용 별 또는 조작 이벤트의 경우 요소는 적중 테스트를 통해 이벤트 소스가 되 고 작업과 연결 된 이벤트를 발생 시켜야 합니다. 그렇지 않은 경우 작업은 해당 입력과 상호 작용할 수 있는 시각적 트리의 부모 요소나 기본 요소에 요소를 전달 합니다. 적중 횟수 테스트에 영향을 주는 여러 요인이 있지만 해당 [**Ishittestvisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible) 속성을 확인 하 여 지정 된 요소가 입력 이벤트를 발생 시킬 수 있는지 여부를 확인할 수 있습니다. 이 속성은 요소가 다음 조건을 충족 하는 경우에만 **true** 를 반환 합니다.

- 요소의 [**표시 유형**](/uwp/api/windows.ui.xaml.uielement.visibility) 속성 값이 [**표시**](/uwp/api/Windows.UI.Xaml.Visibility)됩니다.
- 요소의 **Background** 또는 **Fill** 속성 값이 **null**이 아닙니다. **Null** [**브러시**](/uwp/api/Windows.UI.Xaml.Media.Brush) 값은 투명도와 적중 테스트 표시 안 함을 발생 합니다. 요소를 투명 하 게 만들기 위해 적중 테스트를 수행 하려면 **null**대신 [**투명**](/uwp/api/windows.ui.colors.transparent) 한 브러시를 사용 합니다.

**참고**  **Background** 및 **Fill** 은 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)에 의해 정의 되지 않으며 대신 [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control) 및 [**모양과**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)같은 다른 파생 클래스에 의해 정의 됩니다. 그러나 속성을 구현 하는 하위 클래스에 관계 없이 전경 및 배경 속성에 대해 사용 하는 브러시의 의미는 적중 테스트 및 입력 이벤트에 대해 동일 합니다.

- 요소가 컨트롤이 면 해당 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) 속성 값이 **true**여야 합니다.
- 요소는 레이아웃에 실제 차원이 있어야 합니다. [**System.windows.frameworkelement.actualheight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) 및 [**system.windows.frameworkelement.actualwidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 가 0 인 요소는 입력 이벤트를 발생 시 지 않습니다.

일부 컨트롤에는 적중 테스트에 대 한 특별 한 규칙이 있습니다. 예를 들어 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 에는 **배경** 속성은 없지만 해당 차원의 전체 영역 내에서 테스트 가능한 적중 횟수는 있습니다. 표시 되는 미디어 원본 파일의 알파 채널과 같은 투명 한 내용에 관계 없이 [**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image) 및 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 컨트롤은 정의 된 사각형 차원에 대해 테스트를 수행할 수 있습니다. [**웹 보기**](/uwp/api/Windows.UI.Xaml.Controls.WebView) 컨트롤은 호스팅된 HTML 및 실행 스크립트 이벤트에서 입력을 처리할 수 있기 때문에 특별 한 적중 테스트 동작을 포함 합니다.

대부분의 [**패널**](/uwp/api/Windows.UI.Xaml.Controls.Panel) 클래스와 [**테두리**](/uwp/api/Windows.UI.Xaml.Controls.Border) 는 자체 백그라운드로 적중 테스트를 수행할 수 없지만, 포함 된 요소에서 라우팅되는 사용자 입력 이벤트를 처리할 수 있습니다.

요소가 적중 테스트 가능한 지 여부에 관계 없이 사용자 입력 이벤트와 동일한 위치에 있는 요소를 확인할 수 있습니다. 이렇게 하려면 [**Findelementsinhostcoordinates**](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) 메서드를 호출 합니다. 이름에서 알 수 있듯이이 메서드는 지정 된 호스트 요소를 기준으로 위치에서 요소를 찾습니다. 그러나 적용 된 변환 및 레이아웃 변경은 요소의 상대 좌표계를 조정할 수 있으므로 지정 된 위치에 있는 요소에 영향을 줍니다.

## <a name="commanding"></a>명령

적은 수의 UI 요소에서 *명령*을 지원 합니다. 명령에서는 기본 구현에서 입력 관련 라우트된 이벤트를 사용 하 고 단일 명령 처리기를 호출 하 여 관련 된 UI 입력 (특정 포인터 작업, 특정 액셀러레이터 키)을 처리할 수 있도록 합니다. UI 요소에 대해 명령을 사용할 수 있는 경우 불연속 입력 이벤트 대신 해당 명령 Api를 사용 하는 것이 좋습니다. 일반적으로 데이터의 뷰 모델을 정의 하는 클래스의 속성에 대 한 **바인딩** 참조를 사용 합니다. 속성은 언어별 **ICommand** 명령 패턴을 구현 하는 명명 된 명령을 보유 합니다. 자세한 내용은 [**Buttonbase. 명령**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)을 참조 하세요.

## <a name="custom-events-in-the-windows-runtime"></a>Windows 런타임의 사용자 지정 이벤트

사용자 지정 이벤트를 정의 하기 위해 이벤트를 추가 하는 방법 및 클래스 디자인에 사용 되는 것은 사용 중인 프로그래밍 언어에 따라 달라 집니다.

- C # 및 Visual Basic의 경우 CLR 이벤트를 정의 합니다. 사용자 지정 접근자를 사용 하지 않는 경우 (제거**추가**) 표준 .net 이벤트 패턴을 사용할 수 있습니다 / **remove**. 추가 팁:
    - 이벤트 처리기의 경우 Windows 런타임 제네릭 [**이벤트 <T> 대리자 이벤트**](/uwp/api/windows.foundation.eventhandler)처리기에 대 한 기본 제공 변환이 있으므로 [** <TEventArgs> system.object**](/dotnet/api/system.eventhandler-1) 를 사용 하는 것이 좋습니다.
    - Windows 런타임로 변환 되지 않으므로 [**system.object**](/dotnet/api/system.eventargs) 에서 이벤트 데이터 클래스를 기반으로 하지 마십시오. 기존 이벤트 데이터 클래스를 사용 하거나 기본 클래스를 사용 하지 마십시오.
    - 사용자 지정 접근자를 사용 하는 경우 [Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자](/previous-versions/windows/apps/hh972883(v=vs.140))를 참조 하세요.
    - 표준 .NET 이벤트 패턴을 명확 하 게 모르겠으면 [사용자 지정 Silverlight 클래스에 대 한 이벤트 정의](/previous-versions/windows/)를 참조 하세요. Microsoft Silverlight 용으로 작성 되었지만 표준 .NET 이벤트 패턴에 대 한 코드 및 개념의 올바른 합계입니다.
- C + +/CX의 경우 [이벤트 (c + +/cx)](/cpp/cppcx/events-c-cx)를 참조 하세요.
    - 사용자 지정 이벤트의 용도에도 명명 된 참조를 사용 합니다. 사용자 지정 이벤트에 람다를 사용 하지 마세요. 순환 참조를 만들 수 있습니다.

Windows 런타임에 대 한 사용자 지정 라우트된 이벤트를 선언할 수 없습니다. 라우트된 이벤트는 Windows 런타임에서 제공 되는 집합으로 제한 됩니다.

사용자 지정 이벤트 정의는 일반적으로 사용자 지정 컨트롤을 정의 하는 과정의 일부로 수행 됩니다. 속성 변경 콜백이 있는 종속성 속성을 포함 하 고, 일부 또는 모든 경우에서 종속성 속성 콜백에 의해 발생 하는 사용자 지정 이벤트를 정의 하는 것이 일반적인 패턴입니다. 컨트롤의 소비자는 사용자가 정의한 속성 변경 콜백에 액세스할 수 없지만 알림 이벤트를 사용할 수 있는 것은 다음에 가장 좋은 방법입니다. 자세한 내용은 [사용자 지정 종속성 속성](custom-dependency-properties.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [빠른 시작: 터치식 입력](/previous-versions/windows/apps/hh465387(v=win.10))
* [키보드 조작](../design/input/keyboard-interactions.md)
* [.NET 이벤트 및 대리자](/previous-versions/17sde2xt(v=vs.110))
* [Windows 런타임 구성 요소 만들기](/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler)