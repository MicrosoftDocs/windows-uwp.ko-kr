---
title: C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소
description: .NET 4.5부터 관리 코드를 사용 하 여 Windows 런타임 구성 요소에 패키지 된 고유한 Windows 런타임 형식을 만들 수 있습니다.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
ms.openlocfilehash: e78171fa182d44f1699bc35643265fddb87824f4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220296"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소

관리 코드를 사용 하 여 사용자 고유의 Windows 런타임 형식을 만들고 Windows 런타임 구성 요소에 패키지할 수 있습니다. C + +, JavaScript, Visual Basic 또는 c #으로 작성 된 유니버설 Windows 플랫폼 (UWP) 앱의 구성 요소를 사용할 수 있습니다. 이 항목에서는 구성 요소 만들기 규칙에 대해 간략히 설명하고, Windows 런타임에 대한 .NET 지원의 몇 가지 측면에 대해 설명합니다. 일반적으로 이 지원은 .NET 프로그래머에게 투명하게 디자인되었습니다. 그러나 JavaScript 또는 C++를 사용하는 구성 요소를 만들 때는 이러한 언어와 Windows 런타임을 지원하는 방식의 차이점을 알아야 합니다.

Visual Basic 또는 c #으로 작성 된 UWP 앱 에서만 사용 하도록 구성 요소를 만들고 해당 구성 요소에 UWP 컨트롤이 포함 되어 있지 않은 경우 Microsoft Visual Studio에서 **Windows 런타임 구성 요소** 프로젝트 템플릿 대신 **클래스 라이브러리** 템플릿을 사용 하는 것이 좋습니다. 간단한 클래스 라이브러리일수록 제한이 적습니다.

## <a name="declaring-types-in-windows-runtime-components"></a>Windows 런타임 구성 요소에서 형식 선언

내부적으로 구성 요소의 Windows 런타임 형식은 UWP 앱에서 허용 되는 모든 .NET 기능을 사용할 수 있습니다. 자세한 내용은 [UWP 앱 용 .net](/dotnet/api/index?view=dotnet-uwp-10.0)을 참조 하세요.

외부적으로 형식의 멤버는 매개 변수 및 반환 값에 대 한 Windows 런타임 형식만 노출할 수 있습니다. 다음 목록에서는 Windows 런타임 구성 요소에서 노출 되는 .NET 형식에 대 한 제한 사항을 설명 합니다.

- 구성 요소에 있는 모든 공용 형식 및 멤버의 필드, 매개 변수 및 반환 값은 Windows 런타임 형식이어야 합니다. 이 제한에는 사용자가 작성 하는 Windows 런타임 형식과 Windows 런타임 자체에서 제공 하는 형식이 포함 됩니다. 또한 많은 .NET 형식을 포함 합니다. 이러한 형식을 포함 하는 것은 관리 코드에서 Windows 런타임를 자연스럽 게 사용할 수 있도록 하기 위해 .NET에서 제공 하는 지원의 일부입니다 &mdash; . 코드는 기본 Windows 런타임 형식 대신 익숙한 .net 형식을 사용 하기 위해 표시 됩니다. 예를 들어, **Int32** 및 **Double**과 같은 .Net 기본 형식, **DateTimeOffset** 및 **Uri**와 같은 특정 기본 형식, 그리고 일반적으로 사용 되는 일부 제네릭 인터페이스 형식 (예: **Ienumerable &lt; t &gt; ** (Of T) Visual Basic) 및 **IDictionary &lt; TKey, TValue &gt; **등을 사용할 수 있습니다. 이러한 제네릭 형식의 형식 인수는 Windows 런타임 형식 이어야 합니다. 이 내용은이 항목의 뒷부분에 있는 [관리 코드에 Windows 런타임 형식 전달](#passing-windows-runtime-types-to-managed-code) 및 [Windows 런타임에 관리 되는 형식 전달](#passing-managed-types-to-the-windows-runtime)단원에서 설명 합니다.

- 공용 클래스와 인터페이스에는 메서드, 속성 및 이벤트가 포함될 수 있습니다. 이벤트에 대 한 대리자를 선언 하거나 **EventHandler &lt; T &gt; ** 대리자를 사용할 수 있습니다. 공용 클래스 또는 인터페이스에는 다음을 사용할 수 없습니다.
    - 제네릭이 아니어야 합니다.
    - Windows 런타임 인터페이스가 아닌 인터페이스를 구현 합니다. 그러나 고유한 Windows 런타임 인터페이스를 만들고 구현할 수 있습니다.
    - Windows 런타임에 없는 형식 (예: **system.object** 및 **system.object**)에서 파생 됩니다.

- 모든 공용 형식에는 어셈블리 이름과 일치하는 루트 네임스페이스가 있어야 하며 어셈블리 이름이 "Windows"로 시작해서는 안 됩니다.

    > **팁**. 기본적으로 Visual Studio 프로젝트는 어셈블리 이름과 일치하는 네임스페이스 이름을 가지고 있습니다. Visual Basic에서 이 기본 네임스페이스에 대한 Namespace 문은 코드에 표시되지 않습니다.

- 공용 구조에는 공용 필드 이외의 멤버를 포함할 수 없으며 이러한 필드는 값 형식 또는 문자열이어야 합니다.
- 공용 클래스는 **sealed**(Visual Basic의 **NotInheritable**)여야 합니다. 프로그래밍 모델에 다형성이 필요한 경우 공용 인터페이스를 만들고 다형성이 필요한 클래스에서 해당 인터페이스를 구현할 수 있습니다.

## <a name="debugging-your-component"></a>구성 요소 디버깅

UWP 앱과 구성 요소가 모두 관리 코드로 빌드된 경우 두 구성 요소를 동시에 디버그할 수 있습니다.

C + +를 사용 하 여 UWP 앱의 일부로 구성 요소를 테스트 하는 경우 관리 되는 코드와 네이티브 코드를 동시에 디버깅할 수 있습니다. 기본적으로는 네이티브 코드만 디버그합니다.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>네이티브 C++ 코드와 관리 코드를 모두 디버그하려면
1.  Visual C++ 프로젝트에서 바로 가기 메뉴를 열고 **속성**을 선택합니다.
2.  속성 페이지의 **구성 속성**에서 **디버깅**을 클릭합니다.
3.  **디버거 유형**을 선택하고 드롭다운 목록 상자에서 **네이티브만**을 **혼합(관리 및 네이티브)** 로 변경합니다. **확인**을 선택합니다.
4.  네이티브 및 관리 코드에 중단점을 설정합니다.

JavaScript를 사용 하 여 UWP 앱의 일부로 구성 요소를 테스트 하는 경우 기본적으로 솔루션은 JavaScript 디버깅 모드에 있습니다. Visual Studio에서는 JavaScript와 관리 코드를 동시에 디버그할 수 없습니다.

## <a name="to-debug-managed-code-instead-of-javascript"></a>JavaScript 대신 관리 코드를 디버그하려면
1.  JavaScript 프로젝트에서 바로 가기 메뉴를 열고 **속성**을 선택합니다.
2.  속성 페이지의 **구성 속성**에서 **디버깅**을 클릭합니다.
3.  **디버거 유형**을 선택하고 드롭다운 목록 상자에서 **스크립트만**을 **관리만**으로 변경합니다. **확인**을 선택합니다.
4.  관리 코드에 중단점을 설정하고 일반적인 방법으로 디버그합니다.

## <a name="passing-windows-runtime-types-to-managed-code"></a>관리 코드에 Windows 런타임 형식 전달
[Windows 런타임 구성 요소에서 형식 선언](#declaring-types-in-windows-runtime-components)단원에서 설명한 대로 특정 .net 형식은 공용 클래스 멤버의 서명에 나타날 수 있습니다. 이는 관리 코드에서 Windows 런타임를 자연스럽 게 사용할 수 있도록 .NET에서 제공 하는 지원의 일부입니다. 여기에는 기본 형식 및 일부 클래스와 인터페이스가 포함됩니다. 구성 요소를 JavaScript 또는 c + + 코드에서 사용 하는 경우에는 .NET 형식이 호출자에 게 표시 되는 방식을 알아야 합니다. [C # 또는 Visual Basic Windows 런타임 구성 요소를 만들고 javascript에서](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) javascript를 사용 하는 예제에 대해 호출 하는 연습을 참조 하세요. 이 섹션에서는 자주 사용되는 형식에 대해 설명합니다.

.NET에서 **Int32** 구조체와 같은 기본 형식에는 **TryParse** 메서드와 같은 많은 유용한 속성과 메서드가 있습니다. 반면, Windows 런타임의 기본 형식과 구조에는 필드만 있습니다. 이러한 형식을 관리 코드에 전달 하면 .NET 형식으로 표시 되며 일반적으로 .NET 형식의 속성 및 메서드를 사용할 수 있습니다. 다음 목록에는 IDE에서 자동으로 만들어지는 대체 항목이 요약되어 있습니다.

-   Windows 런타임 기본 형식 **Int32**, **Int64**, **Single**, **Double**, **Boolean**, **String** (변경할 수 없는 유니코드 문자 컬렉션), **Enum**, **UInt32**, **UInt64**및 **Guid**의 경우 시스템 네임 스페이스에 있는 동일한 이름의 형식을 사용 합니다.
-   **UInt8**의 경우 **system.object**를 사용 합니다.
-   **Char16**의 경우에는 **system.object**를 사용 합니다.
-   **IInspectable** 인터페이스의 경우 **system.object**를 사용 합니다.

C # 또는 Visual Basic에서 이러한 형식에 대 한 언어 키워드를 제공 하는 경우 language 키워드를 대신 사용할 수 있습니다.

기본 형식 외에도 일반적으로 사용 되는 일부 기본 Windows 런타임 형식은 관리 코드에 해당 .NET으로 표시 됩니다. 예를 들어 JavaScript 코드에서 **Windows 기반** 클래스를 사용 하 고 c # 또는 Visual Basic 메서드에 전달 하려고 한다고 가정 합니다. 관리 코드에서 해당 하는 형식은 .NET **system.uri** 클래스 이며 메서드 매개 변수에 사용할 형식입니다. Visual Studio의 IntelliSense는 관리 코드를 작성할 때 Windows 런타임 형식을 숨기고 동등한 .NET 형식을 제공 하므로 Windows 런타임 형식이 .NET 형식으로 표시 되는 경우를 알 수 있습니다. 보통 두 형식은 이름이 같습니다. 그러나 **Windows Foundation. datetime** 구조는 관리 코드 **에 system.object로 표시 되며,** 이는 **system.object**로 표시 되지 않습니다.

일반적으로 사용 되는 일부 컬렉션 형식의 경우 Windows 런타임 형식으로 구현 되는 인터페이스와 해당 .NET 형식에 의해 구현 되는 인터페이스 사이에 매핑이 있습니다. 위에서 언급 한 형식과 마찬가지로 .NET 형식을 사용 하 여 매개 변수 형식을 선언 합니다. 이렇게 하면 형식 간의 몇 가지 차이점을 숨기고 .NET 코드를 보다 자연스럽 게 작성할 수 있습니다.

다음 표에서는 이러한 제네릭 인터페이스 형식 중 가장 일반적인 항목과 기타 일반 클래스 및 인터페이스 매핑을 나열합니다. .NET에 매핑되는 Windows 런타임 형식의 전체 목록은 [Windows 런타임 형식의 .net 매핑](net-framework-mappings-of-windows-runtime-types.md)을 참조 하세요.

| Windows 런타임                                  | .NET                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable &lt; T&gt;                              |
| IVector&lt;T&gt;                                 | IList &lt; T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList &lt; T&gt;                            |
| IMap &lt; K, V&gt;                                 | IDictionary &lt; TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | Ireadonlydictionary<string &lt; TKey, TValue&gt;           |
| Inputiterator<ikeyvaluepair<k &lt; K, V&gt;                        | KeyValuePair &lt; TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

하나의 형식이 두 개 이상의 인터페이스를 구현하는 경우 구현된 인터페이스 중 하나만 매개 변수 형식 또는 멤버의 반환 형식으로 사용할 수 있습니다. 예를 들어 Visual Basic의 **사전 &lt; int, &gt; 문자열** (**사전 (정수, 문자열)** )을 **IDictionary &lt; int, string &gt; **, **ireadonlydictionary<string &lt; int, string &gt; **또는 **IEnumerable &lt; KeyValuePair &lt; TKey, TValue &gt; &gt; **로 전달 하거나 반환할 수 있습니다.

> [!IMPORTANT]
> JavaScript는 관리되는 형식이 구현하는 인터페이스 목록에 첫 번째로 표시되는 인터페이스를 사용합니다. 예를 들어, 문자열을 JavaScript 코드로 반환 하는 경우 ** &lt; 문자열 &gt; ** 을 반환 형식으로 지정 하는 인터페이스에 관계 없이 ** &lt; &gt; IDictionary int** 로 표시 됩니다. 즉, 첫 번째 인터페이스가 나머지 인터페이스에 나타나는 멤버를 포함하고 있지 않은 경우 해당 멤버는 JavaScript에 표시되지 않습니다.

Windows 런타임에서 **IMap &lt; K, &gt; v** 및 **IMapView &lt; K, v &gt; ** 는 inputiterator<ikeyvaluepair<k를 사용 하 여 반복 됩니다. 관리 코드에 전달 하는 경우 해당 코드는 **IDictionary &lt; Tkey, &gt; TValue** 및 **ireadonlydictionary<string &lt; tkey, TValue &gt; **으로 표시 되므로 자연스럽 게 **KeyValuePair &lt; tkey, TValue &gt; ** 를 사용 하 여이를 열거 합니다.

인터페이스가 관리 코드에 표시되는 방식은 이러한 인터페이스를 구현하는 형식이 표시되는 방식에 영향을 미칩니다. 예를 들어, **PropertySet** 클래스는 관리 코드에 **IDictionary &lt; TKey, &gt; TValue**로 표시 되는 **IMap &lt; K, V &gt; **를 구현 합니다. **PropertySet** 는 다른 **IDictionary &lt; TKey &gt; , TValue** ** &lt; &gt; 를 구현**하는 것 처럼 표시 됩니다. 따라서 관리 코드에서 **add** 메서드를 포함 하는 것 처럼 표시 됩니다 .이 메서드는 .net 사전에서 **add** 메서드 처럼 동작 합니다. **Insert** 메서드가 없는 것 같습니다. [C # 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)하는 연습 항목에서이 예제를 확인할 수 있습니다.

## <a name="passing-managed-types-to-the-windows-runtime"></a>Windows 런타임에 관리 형식 전달

이전 섹션에서 설명한 것 처럼 일부 Windows 런타임 형식은 구성 요소 멤버의 서명 또는 IDE에서 사용할 때 Windows 런타임 멤버의 시그니처에서 .NET 형식으로 나타날 수 있습니다. 이러한 멤버에 .NET 형식을 전달 하거나 구성 요소 멤버의 반환 값으로 사용 하는 경우 이러한 멤버는 다른 쪽에 있는 코드에 해당 Windows 런타임 형식으로 표시 됩니다. JavaScript에서 구성 요소를 호출할 때 발생할 수 있는 효과에 대 한 예제는 [c # 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)하는 연습에서 "구성 요소에서 관리 되는 형식 반환" 섹션을 참조 하세요.

## <a name="overloaded-methods"></a>오버로드된 메서드

Windows 런타임에서 메서드가 오버로드될 수 있습니다. 그러나 매개 변수 수가 같은 오버 로드를 여러 개 선언 하는 경우 이러한 오버 로드 중 하나에만 [**DefaultOverloadAttribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) 특성을 적용 해야 합니다. 해당 오버로드만 JavaScript에서 호출할 수 있습니다. 예를 들어 다음 코드에서 **int**(Visual Basic의 **Integer**)가 사용된 오버로드가 기본 오버로드입니다.

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> 중요 한 JavaScript를 사용 하면 **OverloadExample**에 값을 전달 하 고 값을 매개 변수에 필요한 형식으로 강제 변환할 수 있습니다. "42", "42" 또는 42.3을 사용 하 여 **OverloadExample** 를 호출할 수 있지만 이러한 모든 값은 기본 오버 로드로 전달 됩니다. 이전 예제의 기본 오버 로드는 각각 0, 42 및 42을 반환 합니다.

**DefaultOverloadAttribut**e 특성은 생성자에 적용할 수 없습니다. 클래스의 모든 생성자는 매개 변수 수가 달라야 합니다.

## <a name="implementing-istringable"></a>IStringable 구현

Windows 8.1부터 Windows 런타임에는 **IStringable** **에서 제공**하는 것과 비교할 수 있는 기본적인 서식 지정 지원을 제공 하는 **IStringable** 인터페이스가 포함 되어 있습니다. Windows 런타임 구성 요소에서 내보낸 관리 되는 공용 형식에서 **IStringable** 을 구현 하도록 선택 하는 경우 다음 제한 사항이 적용 됩니다.

-   C #의 다음 코드와 같이 "클래스 구현" 관계 에서만 **IStringable** 인터페이스를 정의할 수 있습니다.

    ```cs
    public class NewClass : IStringable
    ```

    또는 다음 Visual Basic 코드에서:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   인터페이스에 **IStringable** 를 구현할 수 없습니다.
-   매개 변수를 **IStringable**형식으로 선언할 수 없습니다.
-   **IStringable** 는 메서드, 속성 또는 필드의 반환 형식일 수 없습니다.
-   다음과 같은 메서드 정의를 사용 하 여 기본 클래스에서 **IStringable** 구현을 숨길 수 없습니다.

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    대신 합니다 **IStringable.ToString** 구현은 언제나 기본 클래스 구현을 재지정 해야 합니다. 강력한 형식의 클래스 인스턴스에서 호출 하는 경우에만 **ToString** 구현을 숨길 수 있습니다.

> [!NOTE]
> 다양 한 조건에서, **IStringable** 을 구현 하거나 해당 **ToString** 구현을 숨기는 관리 되는 형식에 대 한 네이티브 코드의 호출은 예기치 않은 동작을 생성할 수 있습니다.

## <a name="asynchronous-operations"></a>비동기 작업

구성 요소에서 비동기 메서드를 구현 하려면 메서드 이름의 끝에 "Async"를 추가 하 고 비동기 작업 또는 작업을 나타내는 **iasyncaction**, **iasyncactionwithprogress &lt; &gt; **, **iasyncoperation<tresult> &lt; &gt; TResult**또는 **IAsyncOperationWithProgress &lt; TResult, tprogress &gt; **Windows 런타임 중 하나를 반환 합니다.

.NET 작업 ( [**task**](/dotnet/api/system.threading.tasks.task) 클래스 및 제네릭 [**작업 &lt; TResult &gt; **](/dotnet/api/system.threading.tasks.task-1) 클래스)을 사용 하 여 비동기 메서드를 구현할 수 있습니다. C# 또는 Visual Basic 작성 된 비동기 메서드에서 반환된 작업이나 [**Task.Run**](/dotnet/api/system.threading.tasks.task.run) 작업(Task)에서 반환된 작업 등 진행 중인 작업을 나타내는 작업을 반환해야 합니다. 생성자를 사용하여 작업을 만드는 경우 반환하기 전에 해당 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 메서드를 호출해야 합니다.

(Visual Basic)를 사용 하는 메서드에는 `await` `Await` `async` 키워드 ( `Async` Visual Basic)가 필요 합니다. Windows 런타임 구성 요소에서 이러한 메서드를 노출 하는 경우 `async` **실행** 메서드에 전달 하는 대리자에 키워드를 적용 합니다.

취소 또는 진행 상황 보고를 지원하지 않는 비동기 작업의 경우 [WindowsRuntimeSystemExtensions.AsAsyncAction](/dotnet/api/system) 또는 [AsAsyncOperation&lt;TResult&gt;](/dotnet/api/system) 확장 메서드를 사용하여 작업을 적절한 인터페이스에 래핑할 수 있습니다. 예를 들어 다음 코드는 Task를 사용 하 여 비동기 메서드를 구현 합니다 **. &lt; TResult &gt; ** 메서드를 실행 하 여 작업을 시작 합니다. **AsAsyncOperation &lt; TResult &gt; ** 확장 메서드는 작업을 Windows 런타임 비동기 작업으로 반환 합니다.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

다음 JavaScript 코드는 [**WinJS**](/previous-versions/windows/apps/br211867(v=win.10)) 개체를 사용 하 여 메서드를 호출 하는 방법을 보여 줍니다. 다음 메서드에 전달된 함수는 비동기 호출이 완료될 때 실행됩니다. StringList 매개 변수는 **DownloadAsStringAsync** 메서드에서 반환 되는 문자열 목록을 포함 하며 함수는 필요한 모든 처리 작업을 수행 합니다.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

취소 또는 진행률 보고를 지 원하는 비동기 작업 및 작업의 경우 [**system.runtime.interopservices.windowsruntime.asyncinfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) 클래스를 사용 하 여 시작 된 작업을 생성 하 고, 작업의 취소 및 진행률 보고 기능을 해당 Windows 런타임 인터페이스의 취소 및 진행률 보고 기능에 연결 합니다. 취소 및 진행률 보고를 모두 지 원하는 예제는 [c # 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)하는 연습을 참조 하세요.

비동기 메서드가 취소 또는 진행률 보고를 지원 하지 않는 경우에도 **system.runtime.interopservices.windowsruntime.asyncinfo** 클래스의 메서드를 사용할 수 있습니다. Visual Basic 람다 함수 또는 c # 무명 메서드를 사용 하는 경우 token 및 [**Iprogress &lt; t &gt; **](/dotnet/api/system.iprogress-1) 인터페이스에 대 한 매개 변수를 제공 하지 마세요. C# 람다 함수를 사용하는 경우 토큰 매개 변수를 제공하되 무시합니다. AsAsyncOperation tresult 메서드를 사용 하는 이전 예제는 &lt; &gt; [**system.runtime.interopservices.windowsruntime.asyncinfo &lt; tresult &gt; (Func &lt; CancellationToken, Task &lt; &gt; &gt; TResult**](/dotnet/api/system.runtime.interopservices.windowsruntime)) 메서드 오버 로드를 대신 사용 하는 경우 다음과 같이 표시 됩니다.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

취소 또는 진행률 보고를 선택적으로 지 원하는 비동기 메서드를 만드는 경우 취소 토큰 또는 **Iprogress &lt; t &gt; ** 인터페이스에 대 한 매개 변수를 포함 하지 않는 오버 로드를 추가 하는 것이 좋습니다.

## <a name="throwing-exceptions"></a>예외 발생

Windows 앱용 .NET에 포함된 모든 예외 형식을 발생시킬 수 있습니다. Windows 런타임 구성 요소에서 자체 공용 예외 형식을 선언할 수는 없지만 비공용 형식을 선언하고 발생시킬 수 있습니다.

구성 요소에서 예외를 처리하지 않으면 구성 요소를 호출한 코드에서 해당 예외가 발생합니다. 호출자에 예외가 나타나는 방식은 호출 언어가 Windows 런타임을 지원하는 방식에 따라 달라집니다.

-   JavaScript에서 예외는 예외 메시지가 스택 추적으로 대체된 개체로 나타납니다. Visual Studio에서 앱을 디버그할 때 디버거 예외 대화 상자에서 원래 메시지 텍스트가 "WinRT 정보"로 식별되어 표시되는 것을 볼 수 있습니다. JavaScript 코드에서 원래 메시지 텍스트에 액세스할 수는 없습니다.

    > **팁**.현재, 스택 추적에는 관리되는 예외 형식이 포함되어 있지만 예외 형식을 식별하기 위해 추적을 구문 분석하지 않는 것이 좋습니다. 대신 이 섹션의 뒷부분에서 설명하는 HRESULT 값을 사용하세요.

-   C++에서 예외는 플랫폼 예외로 나타납니다. 관리 되는 예외의 HResult 속성을 특정 플랫폼 예외의 HRESULT에 매핑할 수 있으면 특정 예외가 사용 됩니다. 그렇지 않으면 [**Platform:: COMException**](/cpp/cppcx/platform-comexception-class) 예외가 throw 됩니다. C++ 코드에서는 관리 예외의 메시지 텍스트를 사용할 수 없습니다. 특정 플랫폼 예외가 발생된 경우 해당 예외 형식에 대한 기본 메시지 텍스트가 나타납니다. 그렇지 않은 경우 메시지 텍스트가 나타나지 않습니다. [예외(C++/CX)](/cpp/cppcx/exceptions-c-cx)를 참조하세요.
-   C# 또는 Visual Basic에서 예외는 일반적인 관리 예외입니다.

구성 요소에서 예외가 발생할 때 HResult 속성 값이 해당 구성 요소와 관련된 비공용 예외 형식을 발생시켜 JavaScript 또는 C++ 호출자에서 예외를 쉽게 처리하도록 할 수 있습니다. HRESULT는 예외 개체의 number 속성을 통해 JavaScript 호출자가 사용할 수 있고 [**COMException:: HRESULT**](/cpp/cppcx/platform-comexception-class#hresult) 속성을 통해 c + + 호출자가 사용할 수 있습니다.

> [!NOTE]
> HRESULT에 음수 값을 사용 합니다. 양수 값은 성공으로 해석되어 JavaScript 또는 C++ 호출자에서 예외가 발생되지 않습니다.

## <a name="declaring-and-raising-events"></a>이벤트 선언 및 발생

이벤트에 대한 데이터를 저장하는 형식을 선언하면 EventArgs 대신 Object에서 파생되는데 그 이유는 EventArgs가 Windows 런타임 형식이 아니기 때문입니다. 이벤트 형식으로 [**EventHandler &lt; teventargs &gt; **](/dotnet/api/system.eventhandler-1) 를 사용 하 고 이벤트 인수 형식을 제네릭 형식 인수로 사용 합니다. .NET 응용 프로그램에서와 마찬가지로 이벤트를 발생 시킵니다.

Windows 런타임 구성 요소가 JavaScript 또는 C++에서 사용되는 경우 이벤트는 해당 언어에 필요한 Windows 런타임 이벤트 패턴을 따릅니다. C # 또는 Visual Basic에서 구성 요소를 사용 하는 경우 이벤트는 일반적인 .NET 이벤트로 표시 됩니다. 예제는 [c # 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출](./walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)하는 연습에서 제공 됩니다.

사용자 지정 이벤트 접근자를 구현한 경우(Visual Basic에서는 **Custom** 키워드로 이벤트 선언) 해당 구현의 Windows 런타임 이벤트 패턴을 따라야 합니다. [Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자](custom-events-and-event-accessors-in-windows-runtime-components.md)를 참조 하세요. C # 또는 Visual Basic 코드에서 이벤트를 처리 하는 경우에도 일반적인 .NET 이벤트로 표시 됩니다.

## <a name="next-steps"></a>다음 단계

직접 사용하기 위해 Windows 런타임 구성 요소를 만든 경우 해당 기능이 다른 개발자에게도 유용할 수 있습니다. 다른 개발자에게 배포하기 위해 구성 요소를 패키지하는 방법에는 두 가지가 있습니다. [관리되는 Windows 런타임 구성 요소 배포](/previous-versions/windows/apps/jj614475(v=vs.140))를 참조하세요.

Visual Basic 및 c # 언어 기능 및 Windows 런타임에 대 한 .NET 지원에 대 한 자세한 내용은 [Visual Basic 및 c # 언어 참조](/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)를 참조 하세요.


## <a name="troubleshooting"></a>문제 해결

| 증상 | 해결책 |
|---------|--------|
|C + +/WinRT 앱에서 XAML을 사용 하는 [c # Windows 런타임 구성 요소]() 를 사용 하는 경우 컴파일러는 *' ' MyNamespace_XamlTypeInfo '의 멤버가 아닙니다. 여기서 MyNamespace는 ' WinRT:: MyNamespace '의 멤버가 아닙니다* &mdash; . 여기서 *MyNamespace* 는 Windows 런타임 구성 요소 네임 스페이스의 이름입니다. | `pch.h`C + +/vb 앱 사용에서 MyNamespace을 적절 하 게 `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` &mdash; 대체 합니다. *MyNamespace* |

## <a name="related-topics"></a>관련 항목
* [UWP 앱용 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)
* [C# 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출하는 연습](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)