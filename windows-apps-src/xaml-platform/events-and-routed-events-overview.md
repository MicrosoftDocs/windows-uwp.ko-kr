---
author: jwmsft
description: 프로그래밍 언어로 C#, Visual Basic 또는 Visual C++ 구성 요소 확장(C++/CX)을 사용하고 UI 정의에 XAML을 사용하는 경우 Windows 런타임 앱의 이벤트 프로그래밍 개념에 대해 설명합니다.
title: 이벤트 및 라우트된 이벤트 개요
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.author: jimwalk
ms.date: 07/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ca58613a5874cde10d2bb5322c3f930e1fbce44
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4123238"
---
# <a name="events-and-routed-events-overview"></a>이벤트 및 라우트된 이벤트 개요

**중요 API**
-   [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)
-   [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)

프로그래밍 언어로 C#, Visual Basic 또는 Visual C++ 구성 요소 확장(C++/CX)을 사용하고 UI 정의에 XAML을 사용하는 경우 Windows 런타임 앱의 이벤트 프로그래밍 개념에 대해 설명합니다. 이벤트 처리기를 XAML에서 UI 요소 선언의 일부로 할당하거나 코드에서 처리기를 추가할 수 있습니다. Windows 런타임은 *라우트된 이벤트*를 지원합니다. 이 기능을 통해 특정 입력 이벤트와 데이터 이벤트가 이벤트를 발생시킨 개체가 아닌 다른 개체에 의해 처리될 수 있습니다. 라우트된 이벤트는 컨트롤 템플릿을 정의하거나 페이지 또는 레이아웃 컨테이너를 사용하는 경우 유용합니다.

## <a name="events-as-a-programming-concept"></a>프로그래밍 개념으로서의 이벤트

일반적으로 Windows 런타임 앱을 프로그래밍할 경우의 이벤트 개념은 가장 인기 있는 프로그래밍 언어의 이벤트 모델과 유사합니다. Microsoft .NET 또는 C++ 이벤트로 작업하는 방법을 이미 알고 있으면 편리하게 시작할 수 있습니다. 그러나 처리기 연결과 같은 기본 작업을 수행하기 위해 이벤트 모델 개념에 대해 자세히 알아야 할 필요는 없습니다.

프로그래밍 언어로 C#, Visual Basic 또는 C++/CX를 사용하는 경우 UI는 태그(XAML)로 정의됩니다. XAML 태그 구문에서는 태그 요소와 런타임 코드 엔터티 간에 이벤트를 연결하는 원리 중 일부는 ASP.NET, HTML5 등의 다른 웹 기술과 유사합니다.

**참고** XAML로 정의된 UI에 대한 런타임 논리를 제공하는 코드를 흔히 *코드 숨김* 또는 코드 숨김 파일이라고 합니다. Microsoft Visual Studio 솔루션 뷰에서 이 관계는 그래픽으로 표시됩니다. 여기에서 코드 숨김 파일은 자신이 참조하는 XAML 페이지에 대해 종속된 중첩 파일이 됩니다.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button.Click: 이벤트 및 XAML 소개

Windows 런타임 앱의 가장 일반적인 프로그래밍 작업 중 하나는 UI에 대한 사용자 입력을 캡처하는 것입니다. 예를 들어 사용자가 정보를 제출하거나 상태를 변경하기 위해 클릭해야 하는 단추가 UI에 있을 수 있습니다.

XAML을 생성하여 Windows 런타임 앱의 UI를 정의합니다. 이 XAML은 대개 Visual Studio의 디자인 화면에서 제공된 출력일 수 있습니다. XAML은 일반 텍스트 편집기나 타사 XAML 편집기에서도 작성될 수 있습니다. 이 XAML을 생성하는 동안 개별 UI 요소의 속성 값을 지정하는 다른 모든 XAML 특성을 정의하는 동시에 해당 UI 요소의 이벤트 처리기를 연결할 수 있습니다.

XAML에서 이벤트를 연결하려면 이미 정의했거나 나중에 코드 숨김에서 정의할 처리기 메서드의 문자열 형태 이름을 지정합니다. 예를 들어 이 XAML은 특성으로 할당된 다른 속성([x:Name 특성](x-name-attribute.md), [**Content**](https://msdn.microsoft.com/library/windows/apps/br209366))을 사용하여 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 개체를 정의하고 `ShowUpdatesButton_Click`라는 메서드를 참조하여 단추의 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 이벤트에 대한 처리기를 연결합니다.

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**팁** *이벤트 연결*은 프로그래밍 용어입니다. 이벤트가 발생하면 명명된 처리기 메서드가 호출되어야 함을 나타내는 데 사용하는 프로세스 또는 코드를 나타냅니다. 대부분의 절차적 코드 모델에서 이벤트 연결은 이벤트와 메서드 둘 다를 명명하고 일반적으로 대상 개체 인스턴스와 관련된 암시적이거나 명시적인 "AddHandler" 코드입니다. XAML에서 "AddHandler"는 암시적이며, 이벤트 연결은 이벤트를 개체 요소의 속성 이름으로 명명하고 처리기를 해당 속성의 값으로 명명하는 작업으로만 구성됩니다.

실제 처리기는 모든 앱 코드 및 코드 숨김에 사용하는 프로그래밍 언어로 작성합니다. `Click="ShowUpdatesButton_Click"` 특성을 사용하여 XAML의 태그가 컴파일되고 구문 분석될 때 IDE의 빌드 작업에 포함된 XAML 태그 컴파일 단계와 앱이 로드될 때 이벤트 XAML 런타임 구문 분석 작업에서 `ShowUpdatesButton_Click`이라는 메서드를 앱 코드의 일부로 발견할 수 있다는 계약을 만들었습니다. `ShowUpdatesButton_Click` 은 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 이벤트의 모든 처리기에 대해 대리자를 기반으로 호환되는 메서드 서명을 구현하는 메서드여야 합니다. 예를 들어 이 코드에서는 `ShowUpdatesButton_Click` 처리기를 정의합니다.

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

이 예제에서 `ShowUpdatesButton_Click` 메서드는 [**RoutedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br208812) 대리자를 기반으로 합니다. MSDN 참조 페이지에서 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 메서드에 대한 구문에 이름이 지정된 대리자를 확인하게 되므로 이 대리자를 사용할 대리자로 알고 있는 것이 좋습니다.

**팁** Visual Studio는 XAML을 편집하는 동안 이벤트 처리기의 이름을 지정하고 처리기 메서드를 정의하는 편리한 방법을 제공합니다. XAML 텍스트 편집기에서 이벤트 특성 이름을 제공하는 경우 Microsoft IntelliSense 목록이 표시될 때까지 잠시 기다려 주세요. 목록에서 **&lt;새 이벤트 처리기&gt;** 를 클릭하면 Microsoft Visual Studio에서 요소의 **x:Name**(또는 형식 이름), 이벤트 이름 및 숫자 접미사를 기반으로 메서드 이름을 제안합니다. 그런 다음, 선택한 이벤트 처리기 이름을 마우스 오른쪽 단추로 클릭하고 **이벤트 처리기 탐색**을 클릭할 수 있습니다. 이렇게 하면 XAML 페이지에 대한 코드 숨김 파일의 코드 편집기 보기에 표시된 것처럼 새로 삽입된 이벤트 처리기 정의로 바로 이동합니다. 이벤트 처리기에는 이벤트에서 사용하는 *sender* 매개 변수와 이벤트 데이터 클래스를 비롯한 올바른 서명이 이미 있습니다. 올바른 서명이 포함된 처리기 메서드가 코드 숨김에 이미 있는 경우에도 해당 메서드의 이름이 자동 완성 드롭다운에 **&lt;새 이벤트 처리기&gt;** 옵션과 함께 표시됩니다. IntelliSense 목록 항목을 클릭하지 않고 바로 가기로 Tab 키를 누를 수도 있습니다.

## <a name="defining-an-event-handler"></a>이벤트 처리기 정의

UI 요소이고 XAML에서 선언되는 개체의 경우 이벤트 처리기 코드는 XAML 페이지에 대한 코드 숨김의 역할을 하는 partial 클래스에서 정의됩니다. 이벤트 처리기는 XAML과 연관된 partial 클래스의 일부로 작성하는 메서드입니다. 이러한 이벤트 처리기는 특정 이벤트에서 사용하는 대리자를 기반으로 합니다. 이벤트 처리기 메서드는 public일 수도 있고 private일 수도 있습니다. private 액세스는 XAML로 만든 처리기와 인스턴스가 결국 코드 생성에 의해 결합되기 때문에 작동합니다. 일반적으로 이벤트 처리기 메서드는 클래스를 private으로 하는 것이 좋습니다.

**참고** C++의 이벤트 처리기는 partial 클래스에서 정의되지 않으며 private 클래스 멤버로 헤더에서 선언됩니다. C++ 프로젝트에 대한 빌드 작업에는 C++에 대한 XAML 형식 시스템 및 코드 숨김 모델을 지원하는 코드를 생성하는 작업이 포함됩니다.

### <a name="the-sender-parameter-and-event-data"></a>*sender* 매개 변수 및 이벤트 데이터

이벤트에 대해 작성하는 처리기는 처리기가 호출되는 각 경우에 대한 입력으로 사용할 수 있는 두 값에 액세스할 수 있습니다. 첫 번째 값은 처리기가 연결되는 개체에 대한 참조인 *sender*입니다. *sender* 매개 변수는 기본 **Object** 형식으로 형식화됩니다. 일반적인 방법은 *sender*를 보다 정확한 형식으로 캐스팅하는 것입니다. 이 방법은 *sender* 개체 자체에 대한 상태를 확인하거나 변경하려는 경우에 유용합니다. 고유한 앱 디자인에 따라, 대개 처리기가 연결된 위치나 다른 디자인 사항을 기준으로 *sender*를 캐스팅해도 괜찮은 형식을 알고 있습니다.

두 번째 값은 일반적으로 *e* 매개 변수로 구문 정의에 나타나는 이벤트 데이터입니다. 처리하고 있는 특정 이벤트에 대해 할당된 대리자의 *e* 매개 변수를 확인한 다음 Visual Studio에서 IntelliSense 또는 개체 브라우저를 사용하여 사용 가능한 이벤트 데이터의 속성을 발견할 수 있습니다. 또는 Windows 런타임 참조 설명서를 사용할 수 있습니다.

일부 이벤트의 경우 이벤트 데이터의 특정 속성 값은 이벤트가 발생했음을 아는 것만큼 중요합니다. 이는 입력 이벤트에 특히 해당하는 사실입니다. 포인터 이벤트의 경우 이벤트가 발생했을 때 포인터의 위치가 중요할 수 있습니다. 키보드 이벤트의 경우 모든 가능한 키 누름에 의해 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 및 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트가 발생합니다. 사용자가 누른 키를 확인하려면 이벤트 처리기가 사용할 수 있는 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)에 액세스해야 합니다. 입력 이벤트 처리에 대한 자세한 내용은 [키보드 조작](https://msdn.microsoft.com/library/windows/apps/mt185607) 및 [포인터 입력 처리](https://msdn.microsoft.com/library/windows/apps/mt404610)를 참조하세요. 입력 이벤트와 입력 시나리오에는 포인터 이벤트에 대한 포인터 캡처, 키보드 이벤트에 대한 보조 키 및 플랫폼 키 코드 등, 이 항목에서 다루지 않은 추가 고려 사항이 있는 경우가 많습니다.

### <a name="event-handlers-that-use-the-async-pattern"></a>**async** 패턴을 사용하는 이벤트 처리기

**async** 패턴을 사용하는 API를 이벤트 처리기 내에서 사용하려는 경우가 있습니다. 예를 들어 [**AppBar**](https://msdn.microsoft.com/library/windows/apps/hh701927)에 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)을 사용하여 파일 선택기를 표시하고 조작할 수 있습니다. 파일 선택기 API 중 다수는 비동기입니다. 이러한 API는 **async**/awaitable 범위 내에서 호출해야 하며, 컴파일러가 이를 적용합니다. 따라서 사용자가 할 수 있는 일은 **async** 키워드를 이벤트 처리기에 추가하는 것입니다. 그러면 처리기는 **async** **void** 상태가 됩니다. 이제 이벤트 처리기에서 **async**/awaitable 호출을 수행할 수 있습니다.

**async** 패턴을 사용하는 사용자 조작 이벤트 처리의 예제는 [파일 액세스 및 선택기](https://msdn.microsoft.com/library/windows/apps/jj655411)([C# 또는 Visual Basic을 사용하여 첫 번째 Windows 런타임 앱 만들기](https://msdn.microsoft.com/library/windows/apps/hh974581) 시리즈의 일부)를 참조하세요. C에서 비동기 API 호출도 참조하세요.)

## <a name="adding-event-handlers-in-code"></a>코드에서 이벤트 처리기 추가

XAML이 이벤트 처리기를 개체에 할당하는 유일한 방법은 아닙니다. 코드에서 지정된 개체(XAML에서 사용할 수 없는 개체 포함)에 이벤트 처리기를 추가하기 위해 이벤트 처리기를 추가하기 위한 언어별 구문을 사용할 수 있습니다.

C#에서 이 구문은 `+=` 연산자를 사용하는 것입니다. 연산자 오른쪽에서 이벤트 처리기 메서드 이름을 참조하여 처리기를 등록합니다.

코드를 사용하여 런타임 UI에 나타나는 개체에 이벤트 처리기를 추가하는 경우 일반적인 방법은 관련 개체에 대한 이벤트 처리기가 런타임에 사용자가 시작한 이벤트에 대한 준비를 갖추도록 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 또는 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737)과 같은 개체 수명 이벤트 또는 콜백에 대한 응답으로 이러한 처리기를 추가하는 것입니다. 이 예제에서는 페이지 구조의 XAML 개요를 보여 주고 개체에 이벤트 처리기를 추가하기 위한 C# 언어 구문을 제공합니다.

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

**참고** 더 자세한 구문이 있습니다. 2005년에 C#에는 컴파일러에서 새 대리자 인스턴스를 유추할 수 있고 이전의 보다 간단한 구문을 사용할 수 있도록 하는 대리자 유추라는 기능이 추가되었습니다. 자세한 구문은 이전 예와 기능적으로 동일하지만 대리자 인스턴스를 등록하기 전에 새 대리자 인스턴스를 명시적으로 만들기 때문에 대리자 유추를 활용하지 않습니다. 이러한 명시적 구문은 덜 일반적으로 사용되나 일부 코드 예에서 여전히 볼 수 있습니다.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 구문에는 두 가지 가능성이 있습니다. 그중 하나는 C# 구문과 유사하며 처리기를 인스턴스에 직접 연결하는 것입니다. 이를 위해 **AddHandler** 키워드와 처리기 메서드 이름을 역참조하는 **AddressOf** 연산자가 필요합니다.

Visual Basic 구문의 다른 옵션은 이벤트 처리기에 대해 **Handles** 키워드를 사용하는 것입니다. 이 방법은 로드 시에 개체에 대한 처리기가 존재하고 개체 수명 내내 유지될 것으로 예상되는 경우에 적합합니다. 개체에 대해 XAML에서 정의된 **Handles**를 사용하려면 **Name** / **x:Name**을 제공해야 합니다. 이 이름은 **Handles** 구문의 *Instance.Event* 부분에 필요한 인스턴스 한정자가 됩니다. 이 경우에는 다른 이벤트 처리기 연결을 시작하기 위해 개체 수명 기반의 이벤트 처리기가 필요하지 않습니다. **Handles** 연결은 XAML 페이지를 컴파일할 때 만들어집니다.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**참고** 일반적으로 Visual Studio와 XAML 디자인 화면에서는 **Handles** 키워드 대신 인스턴스 처리 방법을 장려합니다. 이는 XAML에서 이벤트 처리기 연결을 설정하는 것이 일반적인 디자이너-개발자 워크플로의 일부이고 **Handles** 키워드 방법이 XAML에서 이벤트 처리기를 연결하는 것과 호환되지 않기 때문입니다.

C + + /CX를 사용 하 여 있습니다 합니다 **+=** 구문을 하지만 C# 기본 형식에서 차이가 있습니다.

-   대리자 유추가 없으므로 대리자 인스턴스에 대해 **ref new**를 사용해야 합니다.
-   대리 생성자에 두 매개 변수가 있고 대상 개체가 첫 번째 매개 변수로 필요합니다. 일반적으로 **this**를 지정합니다.
-   대리 생성자에는 두 번째 매개 변수로 메서드 주소가 필요하므로 **&** 참조 연산자가 메서드 이름보다 우선합니다.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>코드에서 이벤트 처리기 제거

이벤트 처리기를 코드에 추가했더라도 코드에서 제거하는 것인 일반적으로 필요 없습니다. 페이지 및 컨트롤과 같은 대부분의 Windows 런타임 개체의 경우, 개체와 기본 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 및 해당 시각적 트리 사이의 연결이 끊기면 개체의 수명 동작에 의해 개체가 제거되며, 대리자 참조도 제거됩니다. .NET은 가비지 수집을 통해 이를 수행하며, C++/CX를 사용한 Windows 런타임은 기본적으로 약한 참조를 사용합니다.

드물긴 하지만 명시적으로 이벤트 처리기를 제거하려는 몇 가지 경우가 있습니다. 다음이 포함됩니다.

-   정적 이벤트에 대해 추가되었으며 일반적인 방법으로 가비지 수집할 수 없는 처리기. Windows 런타임 API의 정적 이벤트 예로 [**CompositionTarget**](https://msdn.microsoft.com/library/windows/apps/br228126) 및 [**Clipboard**](https://msdn.microsoft.com/library/windows/apps/br205867) 클래스가 있습니다.
-   처리기를 즉시 제거하려는 테스트 코드 또는 런타임에 이벤트에 대한 이전/새 이벤트 처리기를 바꾸려는 코드
-   사용자 지정 **remove** 접근자 구현
-   사용자 지정 정적 이벤트
-   페이지 탐색 처리기

[**FrameworkElement.Unloaded**](https://msdn.microsoft.com/library/windows/apps/br208748) 또는 [**Page.NavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507)은 다른 이벤트 처리기를 제거하는 데 사용할 수 있도록 상태 관리 및 개체 수명에서 적절한 위치에 있는 가능한 이벤트 트리거입니다.

예를 들어 다음 코드를 사용하여 대상 개체 **textBlock1**에서 **textBlock1\_PointerEntered**라는 이벤트 처리기를 제거할 수 있습니다.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

XAML 특성을 통해 이벤트가 추가된 경우, 즉 생성된 코드에서 처리기가 추가된 경우의 처리기를 제거할 수도 있습니다. 처리기가 연결된 요소에 대해 **Name** 값을 제공한 경우 나중에 이 이름이 코드에 개체 참조를 제공하므로 이 작업이 더 용이합니다. 그러나 개체에 **Name**이 없으면 필요한 개체 참조를 찾기 위해 개체 트리를 탐색할 수도 있습니다.

C++/CX에서 이벤트 처리기를 제거해야 하는 경우 등록 토큰이 필요하며, 이 토큰은 `+=` 이벤트 처리기 등록에서 반환되는 값에서 받아야 합니다. 왜냐하면 C++/CX 구문에서 `-=` 등록 해제의 오른쪽에 사용하는 값은 메서드 이름이 아니라 토큰이기 때문입니다. C++/CX의 경우 XAML 특성으로 추가된 처리기는 제거할 수 없습니다. C++/CX를 사용하여 생성된 코드는 토큰을 저장하지 않기 때문입니다.

## <a name="routed-events"></a>라우트된 이벤트

C#, Microsoft Visual Basic 또는 C++/CX를 사용하는 Windows 런타임은 대부분의 UI 요소에 있는 이벤트 집합에 대해 라우트된 이벤트의 개념을 지원합니다. 이러한 이벤트는 입력 및 사용자 조작 시나리오를 위한 것이며, [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 기본 클래스에서 구현됩니다. 다음은 라우트된 이벤트인 입력 이벤트의 목록입니다.

-   [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922)
-   [**DragEnter**](https://msdn.microsoft.com/library/windows/apps/br208923)
-   [**DragLeave**](https://msdn.microsoft.com/library/windows/apps/br208924)
-   [**DragOver**](https://msdn.microsoft.com/library/windows/apps/br208925)
-   [**놓기**](https://msdn.microsoft.com/library/windows/apps/br208926)
-   [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)
-   [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945)
-   [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)
-   [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947)
-   [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)
-   [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951)
-   [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)
-   [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)
-   [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)
-   [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)
-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)
-   [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)
-   [**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984)
-   [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985)
-   [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927)
-   [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943)

라우트된 이벤트는 자식 개체에서 개체 트리의 연속적인 부모 개체 각각으로 전달(*라우트*)될 수 있는 이벤트입니다. UI의 XAML 구조는 이 트리와 유사하며 트리의 루트는 XAML의 루트 요소입니다. 실제 개체 트리에는 속성 요소 태그와 같은 XAML 언어 기능이 포함되지 않기 때문에 실제 개체 트리는 XAML 요소 중첩과 다소 다를 수도 있습니다. 라우트된 이벤트는 이벤트를 발생시키는 XAML 개체 요소 자식 요소에서 이러한 자식을 포함하는 부모 개체 요소로의 *버블링*으로 간주될 수 있습니다. 이벤트 및 이벤트 데이터는 이벤트 경로를 따라 여러 개체에서 처리될 수 있습니다. 처리기를 가지고 있는 요소가 없는 경우 루트 요소에 도달할 때까지 경로를 따라 계속 이동할 수 있습니다.

DHTML(동적 HTML) 또는 HTML5와 같은 웹 기술을 알고 있는 경우 이미 *버블링* 이벤트 개념에 익숙할 수 있습니다.

라우트된 이벤트가 해당 이벤트 경로에서 버블링하는 경우 연관된 이벤트 처리기가 모두 이벤트 데이터의 공유 인스턴스에 액세스합니다. 따라서 처리기가 이벤트 데이터를 작성할 수 있는 경우 이벤트 데이터에 대한 모든 변경 내용이 다음 처리기로 전달되며 이벤트의 원래 이벤트 데이터를 더 이상 나타낼 수 없습니다. 이벤트에 라우트된 이벤트 동작이 있는 경우 참조 설명서에 라우트된 동작에 대한 설명이 포함됩니다.

### <a name="the-originalsource-property-of-routedeventargs"></a>**RoutedEventArgs**의 **OriginalSource** 속성

이벤트가 이벤트 경로 위로 버블링할 때 *sender*는 이벤트를 발생시킨 개체와 더 이상 동일한 개체가 아닙니다. *sender*는 호출될 처리기가 있는 개체입니다.

경우에 따라 *sender*가 관심 대상이 아니고, 포인터 이벤트 발생 시 가능한 자식 개체 중 포인터가 가리키는 개체 또는 키보드 키를 눌렀을 때 포커스를 받는 더 큰 UI의 개체 등과 같은 정보에 관심이 있을 수 있습니다. 이런 경우, [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 속성 값을 사용할 수 있습니다. 경로의 모든 지점에서 **OriginalSource**는 처리기가 연결된 개체 대신 이벤트를 발생시킨 원래 개체를 보고합니다. 그러나 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 입력 이벤트의 경우 원래 개체는 종종 페이지 수준 UI 정의 XAML에서 즉시 표시되지 않는 개체입니다. 대신 이 원본 개체는 컨트롤의 템플릿 기반 부분일 수 있습니다. 예를 들어 사용자가 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)의 가장자리에 포인터를 놓으면 대부분의 포인터 이벤트의 경우 **OriginalSource**는 **Button** 자체가 아니라 [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465)의 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 템플릿 요소입니다.

**팁** 입력 이벤트 버블링을 사용하면 템플릿 기반 컨트롤을 만드는 경우 특히 유용합니다. 템플릿이 있는 컨트롤에는 소비자가 적용한 새 템플릿이 있을 수 있습니다. 소비자는 작업 템플릿을 다시 만들려고 하다가 기본 템플릿에서 선언된 일부 이벤트 처리를 자신도 모르게 제거할 수 있습니다. 이 경우 클래스 정의에 있는 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737) 재정의의 일부로서 처리기를 연결하여 여전히 컨트롤 수준의 이벤트 처리를 제공할 수 있습니다. 그런 다음 인스턴스화에서 컨트롤의 루트까지 버블링된 입력 이벤트를 catch할 수 있습니다.

### <a name="the-handled-property"></a>**Handled** 속성

특정 라우트된 이벤트에 대한 몇 가지 이벤트 데이터 클래스에는 **Handled**라는 속성이 포함되어 있습니다. 예를 들어 [**PointerRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943079), [**KeyRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073), [**DragEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/br242375)를 참조하세요. 어떤 경우든 **Handled**는 설정 가능한 부울 속성입니다.

**Handled** 속성을 **true**로 설정하면 이벤트 시스템 동작이 영향을 받습니다. **Handled**가 **true**이면 대부분의 이벤트 처리기에 대한 라우팅이 중지됩니다. 이벤트는 계속하여 경로를 따라 다른 연결된 처리기에 해당하는 특정 이벤트에 대해 알리지 않습니다. "Handled"가 이벤트의 컨텍스트에서 의미하는 것과 앱이 이에 응답하는 방법은 개발자에게 달려 있습니다. 기본적으로 **Handled**는 이벤트의 발생이 특정 컨테이너로 버블링될 필요가 없음을 앱 코드에 명시하도록 하는 간단한 프로토콜이며, 필요한 것은 앱 논리에서 처리합니다. 그러나 반대로, 기본 제공 시스템이나 제어 동작이 작동할 수 있도록 버블링해야 할 수도 있는 이벤트를 처리하지 않도록 주의해야 합니다. 예를 들어, 선택 컨트롤의 일부 또는 항목 내에 있는 하위 수준 이벤트 처리가 저하될 수 있습니다. 선택 컨트롤은 선택을 변경해야 하는지를 확인하기 위해 입력 이벤트를 찾아볼 수 있습니다.

라우팅된 이벤트가 모두 이런 방식으로 경로를 취소할 수 있는 것은 아닌데, 그 이유는 그러한 이벤트에 **Handled** 속성이 없기 때문일 수 있습니다. 예를 들어 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 및 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943)는 버블링되지만 항상 루트까지 계속 버블링되며, 해당 이벤트 데이터 클래스에는 이 동작에 영향을 미칠 수 있는 **Handled** 속성이 없습니다.

##  <a name="input-event-handlers-in-controls"></a>컨트롤의 입력 이벤트 처리기

특정 Windows 런타임 컨트롤은 입력 이벤트에 대해 **Handled** 개념을 내부적으로 사용하기도 합니다. 이렇게 하면 사용자 코드에서 입력 이벤트를 처리할 수 없으므로 입력 이벤트가 발생하지 않은 것처럼 만들 수 있습니다. 예를 들어 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 클래스에는 일반 입력 이벤트 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)를 의도적으로 처리하는 논리가 포함되어 있습니다. 그 이유는 단추가 포인터 누름 입력과 포커스가 있을 때 단추를 호출할 수 있는 키(예: Enter 키) 처리와 같은 기타 입력 모드로 시작된 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 이벤트를 발생시키기 때문입니다. **Button**의 클래스 디자인을 위해 원시 입력 이벤트는 개념적으로 처리되고 사용자 코드 같은 클래스 소비자는 컨트롤 관련 **Click** 이벤트를 조직할 수 있습니다. Windows 런타임 API 참조의 특정 컨트롤 클래스에 대한 항목에서 클래스가 구현하는 이벤트 처리 동작에 대해 자주 설명합니다. **On***Event* 메서드를 재정의하여 동작을 변경할 수 있는 경우도 있습니다. 예를 들어 [**Control.OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982)을 재정의하여 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 파생 클래스가 키 입력에 응답하는 방식을 변경할 수 있습니다.

##  <a name="registering-handlers-for-already-handled-routed-events"></a>이미 처리된 라우트된 이벤트에 대한 처리기 등록

앞에서 **Handled**를 **true**로 설정하면 대부분의 처리기가 호출되지 않는다고 설명했습니다. 그러나 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 메서드는 경로에서 이전에 있는 다른 처리기가 공유 이벤트 데이터에서 **Handled**를 **true**로 설정한 경우에도 경로에 대해 항상 호출되는 처리기를 연결할 수 있는 방법을 제공합니다. 사용 중인 컨트롤이 내부 구성에서 또는 컨트롤 전용 논리에 대해 이벤트를 처리했지만 여전히 컨트롤 인스턴스 또는 앱 UI에서 이에 대해 응답하고자 하는 경우 이 방법이 유용합니다. 그러나 이 방법은 **Handled**의 목적과 모순되고 컨트롤에서 의도하는 조작을 위반할 수 있기 때문에 주의해서 사용해야 합니다.

라우트된 이벤트 식별자가 **AddHandler** 메서드의 필수 입력이기 때문에 해당하는 라우트된 이벤트 식별자가 있는 라우트된 이벤트만 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 이벤트 처리 기술을 사용할 수 있습니다. 라우트된 이벤트 식별자를 사용할 수 있는 이벤트의 목록은 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399)에 대한 참조 설명서를 참조하세요. 대체로 이 목록은 앞에서 표시된 라우트된 이벤트 목록과 동일합니다. 예외는 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 및 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 목록의 마지막 두 항목에 라우트된 이벤트 식별자가 없으므로 해당 항목에는 **AddHandler**를 사용할 수 없다는 것입니다.

## <a name="routed-events-outside-the-visual-tree"></a>시각적 트리 외부의 라우트된 이벤트

특정 개체는 기본 시각적 트리와의 관계에 참여합니다. 이는 개념적으로 기본 시각적 항목 위에 오버레이가 있는 것과 같습니다. 이러한 개체는 모든 트리 요소를 시각적 루트에 연결하는 일반적인 부모-자식 관계의 일부가 아닙니다. 이는 표시된 [**Popup**](https://msdn.microsoft.com/library/windows/apps/br227842) 또는 [**ToolTip**](https://msdn.microsoft.com/library/windows/apps/br227608)에 해당합니다. **Popup** 또는 **ToolTip**에서 라우트된 이벤트를 처리하려면 **Popup** 또는 **ToolTip** 요소 자체가 아니라 **Popup** 또는 **ToolTip** 내에 있는 특정 UI 요소에 처리기를 배치합니다. **Popup** 또는 **ToolTip** 콘텐츠에 대해 수행되는 합치기 내에서 라우팅에 의존하지 마세요. 이는 라우트된 이벤트의 이벤트 라우팅이 기본 시각적 트리를 따라서만 작동하기 때문입니다. **Popup** 기본 배경과 같은 것을 입력 이벤트의 캡처 영역으로 사용하려고 하는 경우에도 **Popup** 또는 **ToolTip**은 보조 UI 요소의 부모로 간주되지 않으며 라우트된 이벤트를 받지 않습니다.

## <a name="hit-testing-and-input-events"></a>적중 횟수 테스트 및 입력 이벤트

UI에서 요소가 마우스, 터치 및 스타일러스 입력에 보이는지 여부와 그 위치를 결정하는 것을 *적중 테스트*라고 합니다. 터치 동작의 경우와 터치 동작의 결과인 조작 관련 또는 조작 이벤트의 경우에도 이벤트 원본이 되거나 터치 동작과 연관된 이벤트를 실행하려면 요소의 적중 횟수 테스트가 보여야 합니다. 그렇지 않으면 동작이 이 요소를 거쳐 해당 입력을 조작할 수 있는 시각적 트리의 기본 요소나 부모 요소에까지 적용됩니다. 적중 횟수 테스트에 영향을 미치는 요소에는 여러 가지가 있지만 [**IsHitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 속성을 확인하여 지정된 요소가 입력 이벤트를 발생시킬 수 있는지 여부를 확인할 수 있습니다. 이 속성은 요소가 다음 기준을 충족하는 경우에만 **true**를 반환합니다.

-   요소의 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 속성 값이 [**Visible**](https://msdn.microsoft.com/library/windows/apps/br209006)입니다.
-   요소의 **Background** 또는 **Fill** 속성 값이 **null**이 아닙니다. [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 값이 **null**이면 투명성이 설정되고 적중 횟수 테스트가 표시되지 않습니다. 요소가 투명하나 적중 횟수를 테스트할 수 있게 하려면 **null** 대신 [**Transparent**](https://msdn.microsoft.com/library/windows/apps/hh748061) 브러시를 사용하세요.

**참고** **Background** 및 **Fill**은 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)로 정의되지 않으며 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 및 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)와 같은 다른 파생 클래스로 정의됩니다. 그러나 전경 및 백그라운드 속성에 사용하는 브러시의 의미는 속성을 구현하는 서브클래스와 관계없이 적중 횟수 테스트 및 입력 이벤트의 경우와 동일합니다.

-   요소가 컨트롤인 경우 해당 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) 속성 값이 **true**여야 합니다.
-   요소의 레이아웃에는 실제 차원이 있어야 합니다. [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 또는 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)가 0인 요소는 입력 이벤트를 발생시키지 않습니다.

일부 컨트롤에는 적중 횟수 테스트를 위한 특수한 규칙이 있습니다. 예를 들어 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)에는 **Background** 속성이 없으나 해당 차원의 전체 영역 내에서 여전히 적중 횟수 테스트가 가능합니다. [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 및 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 컨트롤은 표시될 미디어 원본 파일의 알파 채널 같은 투명 콘텐츠와 관계없이 정의된 사각형 차원에 대해 적중 횟수 테스트가 가능합니다. 입력이 호스트된 HTML에 의해 처리될 수 있고 스크립트 이벤트를 발생시킬 수 있기 때문에 [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 컨트롤에는 특수 적중 횟수 테스트 동작이 있습니다.

대부분의 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 클래스 및 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)의 경우 고유 백그라운드에서는 적중 횟수 테스트가 가능하지 않지만 포함된 요소에서 라우트된 사용자 입력 이벤트는 계속 처리할 수 있습니다.

요소의 적중 횟수 테스트가 가능한지에 관계없이 사용자 입력 이벤트와 동일한 위치에 있는 요소를 확인할 수 있습니다. 이렇게 하려면 [**FindElementsInHostCoordinates**](https://msdn.microsoft.com/library/windows/apps/br243039) 메서드를 호출합니다. 이름에서 알 수 있는 것처럼 이 메서드는 지정된 호스트 요소에 상대적인 위치에서 요소를 찾습니다. 그러나 적용된 변형 및 레이아웃 변경은 요소의 상대적 좌표계를 조정할 수 있으며, 따라서 지정된 위치에서 발견되는 요소에도 영향을 미칠 수 있습니다.

## <a name="commanding"></a>명령

적은 수의 UI 요소는 *명령*을 지원합니다. 명령은 기본 구현에서 입력 관련 라우트된 이벤트를 사용하고 단일 명령 처리기를 호출하여 관련 UI 입력(특정 포인터 작업, 특정 액셀러레이터 키)을 처리할 수 있도록 합니다. UI 요소에 명령을 사용할 수 있는 경우 불연속 입력 이벤트 대신 명령 API를 사용하는 것이 좋습니다. 데이터에 대한 보기 모델을 정의하는 클래스의 속성에는 일반적으로 **Binding** 참조를 사용합니다. 이러한 속성에는 언어별 **ICommand** 명령 패턴을 구현하는 명명된 명령이 포함되어 있습니다. 자세한 내용은 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)를 참조하세요.

## <a name="custom-events-in-the-windows-runtime"></a>Windows 런타임의 사용자 지정 이벤트

사용자 지정 이벤트 정의와 관련하여 이벤트 추가 방법 및 클래스 디자인에 미치는 영향은 사용하는 프로그래밍 언어에 따라 달라집니다.

-   C# 및 Visual Basic의 경우 CLR 이벤트를 정의합니다. 사용자 지정 접근자(**add**/**remove**)를 사용하지 않는다면 표준 .NET 이벤트 패턴을 사용할 수 있습니다. 추가 팁:
    -   이벤트 처리기의 경우 Windows 런타임 일반 이벤트 대리자 [**EventHandler<T>**](https://msdn.microsoft.com/library/windows/apps/br206577)로의 변환이 기본 제공되는 [**System.EventHandler<TEventArgs>**](https://msdn.microsoft.com/library/windows/apps/xaml/db0etb8x.aspx)를 사용하는 것이 좋습니다.
    -   Windows 런타임으로 변환되지 않는 [**System.EventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/system.eventargs.aspx)를 이벤트 데이터 클래스의 기초로 사용하지 마세요. 기존 이벤트 데이터 클래스를 사용하거나, 기본 클래스를 사용하지 마세요.
    -   사용자 지정 접근자를 사용하는 경우 [Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자](https://msdn.microsoft.com/library/windows/apps/xaml/hh972883.aspx)를 참조하세요.
    -   표준 .NET 이벤트 패턴이 무엇인지 잘 모르겠으면 [사용자 지정 Silverlight 클래스에 대한 이벤트 정의](http://msdn.microsoft.com/library/dd833067.aspx)를 참조하세요. 이 문서는 Microsoft Silverlight용으로 작성되었지만 표준 .NET 이벤트 패턴에 대한 코드와 개념이 잘 요약되어 있습니다.
-   C++/CX의 경우 [이벤트(C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh755799.aspx)를 참조하세요.
    -   사용자 지정 이벤트를 고유하게 사용하는 경우에도 명명된 참조를 사용합니다. 사용자 지정 이벤트에 람다를 사용하지 마세요. 람다는 순환 참조를 만들 수 있습니다.

Windows 런타임에 대한 사용자 지정 라우트된 이벤트를 선언할 수 없습니다. 라우트된 이벤트는 Windows 런타임에서 가져오는 집합으로 제한됩니다.

사용자 지정 이벤트 정의는 대체로 사용자 지정 컨트롤을 정의하는 연습의 일부로 수행됩니다. 속성 변경 콜백이 있는 종속성 속성을 포함하고 일부 또는 모든 경우에 종속성 속성 콜백에서 발생하는 사용자 지정 이벤트도 정의하는 것이 일반적인 패턴입니다. 컨트롤 사용자는 정의된 속성 변경 콜백에 액세스할 수 없으며 알림 이벤트를 제공하는 것이 차선입니다. 자세한 내용은 [사용자 지정 종속성 속성](custom-dependency-properties.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [빠른 시작: 터치식 입력](https://msdn.microsoft.com/library/windows/apps/xaml/hh465387)
* [키보드 조작](https://msdn.microsoft.com/library/windows/apps/mt185607)
* [.NET 이벤트 및 대리자](http://go.microsoft.com/fwlink/p/?linkid=214364)
* [Windows 런타임 구성 요소 만들기](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399)
