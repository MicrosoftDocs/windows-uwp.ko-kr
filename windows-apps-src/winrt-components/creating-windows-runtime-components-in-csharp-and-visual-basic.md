---
title: C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기
description: .NET Framework 4.5부터 관리 코드를 사용하여 Windows 런타임 구성 요소에 패키지된 Windows 런타임 형식을 직접 만들 수 있습니다.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 52745a4fcd6b5a6b33982595f8c7c65c0bee3c32
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8331155"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기
.NET Framework 4.5부터 관리 코드를 사용하여 Windows 런타임 구성 요소에 패키지된 Windows 런타임 형식을 직접 만들 수 있습니다. UWP(유니버설 Windows 플랫폼) 앱의 구성 요소를 C++, JavaScript, Visual Basic 또는 C#과 함께 사용할 수 있습니다. 이 항목에서는 구성 요소를 만들기 위한 규칙을 간략히 설명 하 고 Windows 런타임에 대 한.NET Framework 지원의 일부 측면을 설명 합니다. 일반적으로 이 지원은 .NET Framework 프로그래머에게 투명하게 디자인되었습니다. 그러나 JavaScript 또는 C++를 사용하는 구성 요소를 만들 때는 이러한 언어와 Windows 런타임을 지원하는 방식의 차이점을 알아야 합니다.

Visual Basic 또는 C#을 사용하여 UWP 앱에서만 사용되는 구성 요소를 만드는데 해당 구성 요소에 UWP 컨트롤이 없는 경우 **Windows 런타임 구성 요소** 템플릿 대신 **클래스 라이브러리** 템플릿을 사용할 수 있습니다. 간단한 클래스 라이브러리일수록 제한이 적습니다.

이 항목에는 다음 섹션이 포함 됩니다.

## <a name="declaring-types-in-windows-runtime-components"></a>Windows 런타임 구성 요소에서 형식 선언
내부적으로, 구성 요소의 Windows 런타임 형식에서는 유니버설 Windows 앱에 허용되는 .NET Framework 기능을 사용할 수 있습니다. (자세한 내용은 [UWP 앱용 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx) 개요를 참조하세요.) 외부적으로 형식의 멤버는 해당 매개 변수 및 반환 값에 대해 Windows 런타임 형식만 표시할 수 있습니다. 다음 목록에서는 Windows 런타임 구성 요소에서 표시되는 .NET Framework 형식에 대한 제한을 설명합니다.

-   구성 요소에 있는 모든 공용 형식 및 멤버의 필드, 매개 변수 및 반환 값은 Windows 런타임 형식이어야 합니다.

    이 제한에는 Windows 런타임 자체에서 제공하는 형식뿐 아니라 직접 만든 Windows 런타임 형식도 포함됩니다. 또한 다양한 .NET Framework 형식도 포함됩니다. 이러한 형식의 포함은 관리 코드에서 Windows 런타임을 자연스럽게 사용할 수 있도록 하기 위해 .NET Framework에서 제공하는 지원의 일부입니다. 기본 Windows 런타임 형식 대신 친숙한 .NET Framework 형식을 사용하여 코드가 나타납니다. 예를 들어 Int32 및 Double 등의 .NET Framework 기본 형식, DateTimeOffset 및 Uri와 같은 특정한 기본 형식, IEnumerable&lt;T&gt;Visual Basic의 IEnumerable(Of T))과 IDictionary&lt;TKey,TValue&gt; 등 일반적으로 사용되는 일부 제네릭 인터페이스 형식을 사용할 수 있습니다. (참고 이러한 제네릭 형식의 형식 인수는 Windows 런타임 형식 이어야 합니다.) 이 섹션에서는 관리 코드에 전달 하 여 Windows 런타임 형식에 대해서는 설명 및 관리 전달이이 항목의 뒷부분에서 Windows 런타임 형식입니다.

-   공용 클래스와 인터페이스에는 메서드, 속성 및 이벤트가 포함될 수 있습니다. 이벤트에 대한 대리자를 선언하거나 EventHandler&lt;T&gt; 대리자를 사용할 수 있습니다. 공용 클래스 또는 인터페이스는 다음과 같은 특징이 있습니다.

    -   제네릭이 아니어야 합니다.
    -   Windows 런타임 인터페이스가 아닌 인터페이스를 구현할 수 없습니다. 그러나 Windows 런타임 인터페이스를 직접 만들어 구현할 수는 있습니다.
    -   System.Exception 및 System.EventArgs 등 Windows 런타임이 아닌 형식에서 파생될 수 없습니다.
-   모든 공용 형식에는 어셈블리 이름과 일치하는 루트 네임스페이스가 있어야 하며 어셈블리 이름이 "Windows"로 시작해서는 안 됩니다.

    > **팁**기본적으로 Visual Studio 프로젝트에는 어셈블리 이름과 일치 하는 네임 스페이스 이름이 있습니다. Visual Basic에서 이 기본 네임스페이스에 대한 Namespace 문은 코드에 표시되지 않습니다.

-   공용 구조에는 공용 필드 이외의 멤버를 포함할 수 없으며 이러한 필드는 값 형식 또는 문자열이어야 합니다.
-   공용 클래스는 **sealed**(Visual Basic의 **NotInheritable**)여야 합니다. 프로그래밍 모델에 다형성이 필요한 경우 공용 인터페이스를 만들어 다형성이어야 하는 클래스에 해당 인터페이스를 구현할 수 있습니다.

## <a name="debugging-your-component"></a>구성 요소 디버깅
유니버설 Windows 앱 및 구성 요소가 모두 관리 코드로 작성된 경우 동시에 디버그할 수 있습니다.

C++를 사용하여 유니버설 Windows 앱의 일부인 구성 요소를 테스트할 때 관리 코드와 네이티브 코드를 동시에 디버그할 수 있습니다. 기본적으로는 네이티브 코드만 디버그합니다.

## **<a name="to-debug-both-native-c-code-and-managed-code"></a>네이티브 C++ 코드와 관리 코드를 모두 디버그하려면**
1.  Visual C++ 프로젝트에서 바로 가기 메뉴를 열고 **속성**을 선택합니다.
2.  속성 페이지의 **구성 속성**에서 **디버깅**을 클릭합니다.
3.  **디버거 유형**을 선택하고 드롭다운 목록 상자에서 **네이티브만**을 **혼합(관리 및 네이티브)** 로 변경합니다. **확인**을 선택합니다.
4.  네이티브 및 관리 코드에 중단점을 설정합니다.

JavaScript를 사용하여 유니버설 Windows 앱의 일부로 구성 요소를 테스트할 때 기본적으로 솔루션은 JavaScript 디버깅 모드로 설정됩니다. Visual Studio에서는 JavaScript와 관리 코드를 동시에 디버그할 수 없습니다.

## **<a name="to-debug-managed-code-instead-of-javascript"></a>JavaScript 대신 관리 코드를 디버그하려면**
1.  JavaScript 프로젝트에서 바로 가기 메뉴를 열고 **속성**을 선택합니다.
2.  속성 페이지의 **구성 속성**에서 **디버깅**을 클릭합니다.
3.  **디버거 유형**을 선택하고 드롭다운 목록 상자에서 **스크립트만**을 **관리만**으로 변경합니다. **확인**을 선택합니다.
4.  관리 코드에 중단점을 설정하고 일반적인 방법으로 디버그합니다.

## <a name="passing-windows-runtime-types-to-managed-code"></a>관리 코드에 Windows 런타임 형식 전달
앞의 Windows 런타임 구성 요소에서 형식 선언 섹션에서 설명했듯이 공용 클래스의 멤버 서명에 특정 .NET Framework 형식이 나타날 수 있습니다. 이는 관리 코드에서 Windows 런타임을 자연스럽게 사용할 수 있도록 하기 위해 .NET Framework에서 제공하는 지원의 일부입니다. 여기에는 기본 형식 및 일부 클래스와 인터페이스가 포함됩니다. JavaScript 또는 C++ 코드의 구성 요소가 사용되는 경우 .NET Framework 형식이 호출자에게 어떻게 표시되는지 알아야 합니다. JavaScript 예제는 [연습: C# 또는 Visual Basic에서 간단한 구성 요소를 만들고 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)을 참조하세요. 이 섹션에서는 자주 사용되는 형식에 대해 설명합니다.

.NET Framework에서 Int32 구조와 같은 기본 형식에는 TryParse 메서드 등 여러 가지 유용한 속성과 메서드가 있습니다. 반면, Windows 런타임의 기본 형식과 구조에는 필드만 있습니다. 관리 코드로 이러한 형식을 전달하면 .NET Framework 형식으로 나타나므로 일반적인 방법으로 .NET Framework 형식의 속성 및 메서드를 사용할 수 있습니다. 다음 목록에는 IDE에서 자동으로 만들어지는 대체 항목이 요약되어 있습니다.

-   Windows 런타임의 기본 형식인 Int32, Int64, Single, Double, Boolean, String(유니코드 문자의 변경 불가능한 컬렉션), Enum, UInt32, UInt64 및 Guid의 경우 System 네임스페이스에서 동일한 이름의 형식을 사용할 수 있습니다.
-   UInt8의 경우 System.Byte를 사용합니다.
-   Char16의 경우 System.Char를 사용합니다.
-   IInspectable 인터페이스의 경우 System.Object를 사용합니다.

C# 또는 Visual Basic에서 이러한 형식에 대한 언어 키워드를 제공하는 경우 대신 언어 키워드를 사용할 수 있습니다.

기본 형식 외에도 자주 사용되는 일부 기본 Windows 런타임 형식은 관리 코드에서 해당하는 .NET Framework 형식으로 나타납니다. 예를 들어 JavaScript 코드에서 사용된 Windows.Foundation.Uri 클래스를 C# 또는 Visual Basic 메서드에 전달하려고 한다고 가정합니다. 관리 코드에서 해당하는 형식은 .NET Framework System.Uri 클래스이며 이 형식이 메서드 매개 변수에 사용됩니다. 관리 코드를 작성할 때 Visual Studio의 IntelliSense는 Windows 런타임 형식을 숨기고 해당하는 .NET Framework 형식을 표시하므로 Windows 런타임 형식이 언제 .NET Framework 형식으로 나타나는지 알 수 있습니다. 보통 두 형식은 이름이 같습니다. 그러나 관리 코드에서는 Windows.Foundation.DateTime 구조가 System.DateTime이 아닌 System.DateTimeOffset으로 나타납니다.

자주 사용하는 컬렉션 형식의 경우 Windows 런타임 형식으로 구현된 인터페이스와 해당 .NET Framework 형식으로 구현된 인터페이스가 매핑되어 있습니다. 위에서 설명한 형식과 마찬가지로, .NET Framework 형식을 사용하여 매개 변수 형식을 선언합니다. 그러면 형식 간의 일부 차이점이 숨겨지고 .NET Framework 코드 작성이 더 자연스러워집니다. 다음 표에서는 이러한 제네릭 인터페이스 형식 중 가장 일반적인 항목과 기타 일반 클래스 및 인터페이스 매핑을 나열합니다. .NET Framework에서 매핑되는 Windows 런타임 형식의 전체 목록은 Windows 런타임 형식의 .NET Framework 매핑을 참조하세요.

| Windows 런타임                                  | .NET Framework                                    |
|--------------------------------------------------|---------------------------------------------------|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K, V&gt;                                 | IDictionary&lt;TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary&lt;TKey, TValue&gt;           |
| IKeyValuePair&lt;K, V&gt;                        | KeyValuePair&lt;TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

하나의 형식이 두 개 이상의 인터페이스를 구현하는 경우 구현된 인터페이스 중 하나만 매개 변수 형식 또는 멤버의 반환 형식으로 사용할 수 있습니다. 예를 들어 전달 하거나 사전을 반환&lt;int, string&gt; (Dictionary (Of Integer, String) Visual basic에서)을 IDictionary&lt;int, string&gt;, IReadOnlyDictionary&lt;int, string&gt;, 또는 IEnumerable&lt; System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;&gt;합니다.

**중요 한**JavaScript는 관리 형식이 구현한 인터페이스 목록에서 처음 나타나는 인터페이스를 사용 합니다. 예를 들어 Dictionary&lt;int, string&gt;을 JavaScript 코드로 반환하는 경우 반환 형식으로 지정한 인터페이스에 관계없이 IDictionary&lt;int, string&gt;으로 나타납니다. 즉, 첫 번째 인터페이스가 나머지 인터페이스에 나타나는 멤버를 포함하고 있지 않은 경우 해당 멤버는 JavaScript에 표시되지 않습니다.

Windows 런타임에서 IMap&lt;K, V&gt; 및 IMapView&lt;K, V&gt;는 IKeyValuePair를 사용하여 반복됩니다. 관리 코드로 전달할 경우 IDictionary&lt;TKey, TValue&gt; 및 IReadOnlyDictionary&lt;TKey, TValue&gt;로 나타나므로 자연스럽게 System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;를 사용하여 열거합니다.

인터페이스가 관리 코드에 나타나는 방법은 이러한 인터페이스를 구현한 형식이 나타나는 방법에 영향을 줍니다. 예를 들어 PropertySet 클래스가 구현한 IMap&lt;K, V&gt;는 관리 코드에서 IDictionary&lt;TKey, TValue&gt;로 나타납니다. PropertySet는 IMap&lt;K, V&gt; 대신 IDictionary&lt;TKey, TValue&gt;를 구현한 것처럼 나타나므로, 관리 코드에서 Add 메서드가 있는 것처럼 나타나고 .NET Framework 사전의 Add 메서드처럼 동작합니다. Insert 메서드가 있는 것처럼 나타나지는 않습니다. 이 예제에서 항목을 볼 수 있습니다 [연습: C# 또는 Visual Basic에서 간단한 구성 요소를 만들고 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)합니다.

## <a name="passing-managed-types-to-the-windows-runtime"></a>Windows 런타임에 관리 형식 전달
이전 섹션에서 설명한 것처럼 일부 Windows 런타임 형식은 구성 요소의 멤버 서명 또는 IDE에 사용된 Windows 런타임 멤버 서명에서 .NET Framework 형식으로 나타납니다. 이러한 멤버에 .NET Framework 형식을 전달하거나 구성 요소 멤버의 반환 값으로 사용할 경우 다른 쪽의 코드에서는 해당 Windows 런타임 형식으로 나타납니다. JavaScript에서 구성 요소를 호출할 때의 효과에 대한 예제는 [연습: C# 또는 Visual Basic에서 간단한 구성 요소를 만들고 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)의 "구성 요소에서 관리 형식 반환" 섹션을 참조하세요.

## <a name="overloaded-methods"></a>오버로드된 메서드
Windows 런타임에서 메서드가 오버로드될 수 있습니다. 그러나 동일한 수의 매개 변수를 사용하여 여러 오버로드를 선언한 경우 해당 오버로드 중 하나에만 [Windows.Foundation.Metadata.DefaultOverloadAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) 특성을 적용해야 합니다. 해당 오버로드만 JavaScript에서 호출할 수 있습니다. 예를 들어 다음 코드에서 **int**(Visual Basic의 **Integer**)가 사용된 오버로드가 기본 오버로드입니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public string OverloadExample(string s)
> {
>     return s;
> }
> [Windows.Foundation.Metadata.DefaultOverload()]
> public int OverloadExample(int x)
> {
>     return x;
> }
> ```
> ```vb
> Public Function OverloadExample(ByVal s As String) As String
>     Return s
> End Function
> <Windows.Foundation.Metadata.DefaultOverload> _
> Public Function OverloadExample(ByVal x As Integer) As Integer
>     Return x
> End Function
> ```

 **주의**JavaScript overloadexample을 모든 값을 전달할 수 있도록 하 고 값을 매개 변수에 필요한 형식으로 강제 변환 합니다. "forty-two", "42" 또는 42.3을 사용하여 OverloadExample을 호출할 수 있지만 이러한 모든 값은 기본 오버로드에 전달됩니다. 이전 예제에서 기본 오버로드는 각각 0, 42 및 42를 반환합니다.

생성자에는 DefaultOverloadAttribute 특성을 적용할 수 없습니다. 클래스의 모든 생성자는 매개 변수 수가 달라야 합니다.

## <a name="implementing-istringable"></a>IStringable 구현
Windows 8.1부터 Windows 런타임에는 IStringable 인터페이스가 포함되며 이 인터페이스의 단일 메서드인 IStringable.ToString은 Object.ToString에서 제공하는 것과 비슷한 기본 형식 지원을 제공합니다. Windows 런타임 구성 요소로 내보낸 공용 관리 형식에서 IStringable을 구현하도록 선택한 경우 다음 제한이 적용됩니다.

-   C#의 다음 코드와 같이 "클래스 구현" 관계로만 IStringable 인터페이스를 정의할 수 있습니다.

    ```cs
    public class NewClass : IStringable
    ```

    또는 다음 Visual Basic 코드에서:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   인터페이스에서 IStringable을 구현할 수 없습니다.
-   매개 변수를 IStringable 형식으로 선언할 수 없습니다.
-   IStringable은 메서드, 속성 또는 필드의 반환 형식이 될 수 없습니다.
-   다음과 같은 메서드 정의를 사용하여 기본 클래스에서 IStringable 구현을 숨길 수 없습니다.

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    대신 IStringable.ToString 구현은 항상 기본 클래스 구현을 재정의해야 합니다. 강력한 형식의 클래스 인스턴스에서 호출하는 방법으로만 ToString 구현을 숨길 수 있습니다.

다양한 조건에서 네이티브 코드에서 관리 형식으로 호출하여 IStringable을 구현하거나 해당 ToString 구현을 숨기면 예기치 않은 동작이 발생할 수 있습니다.

## <a name="asynchronous-operations"></a>비동기 작업
구성 요소에서 비동기 메서드를 구현하려면 메서드 이름 끝에 "Async"를 추가하고 비동기 작업을 나타내는 Windows 런타임 인터페이스인 IAsyncAction, IAsyncActionWithProgress&lt;TProgress&gt;, IAsyncOperation&lt;TResult&gt; 또는 IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 중 하나를 반환합니다.

.NET Framework 작업([Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) 클래스 및 제네릭 [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) 클래스)을 사용하여 비동기 메서드를 구현할 수 있습니다. C# 또는 Visual Basic으로 작성된 비동기 메서드에서 반환된 작업 또는 [Task.Run](https://msdn.microsoft.com/library/system.threading.tasks.task.run.aspx) 메서드에서 반환된 작업 등 진행 중인 작업을 반환해야 합니다. 생성자를 사용하여 작업을 만드는 경우 반환하기 전에 해당 [Task.Start](https://msdn.microsoft.com/library/system.threading.tasks.task.start.aspx) 메서드를 호출해야 합니다.

await를 사용하는 메서드(Visual Basic의 Await)에는 **async** 키워드(Visual Basic의 **Async**)가 필요합니다. Windows 런타임 구성 요소에서 이러한 메서드를 표시하는 경우 Run 메서드에 전달하는 대리자에 **async** 키워드를 적용합니다.

취소 또는 진행 상황 보고를 지원하지 않는 비동기 작업의 경우 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) 또는 [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) 확장 메서드를 사용하여 작업을 적절한 인터페이스에 래핑할 수 있습니다. 예를 들어 다음 코드는 Task.Run&lt;TResult&gt; 메서드를 사용하여 작업을 시작하는 비동기 메서드를 구현합니다. AsAsyncOperation&lt;TResult&gt; 확장 메서드는 작업을 Windows 런타임 비동기 작업으로 반환합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return Task.Run<IList<string>>(async () =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     }).AsAsyncOperation();
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>      As IAsyncOperation(Of IList(Of String))
>
>     Return Task.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function).AsAsyncOperation()
> End Function
> ```

다음 JavaScript 코드에서는 [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) 개체를 사용하여 메서드를 호출하는 방법을 보여 줍니다. 다음 메서드에 전달된 함수는 비동기 호출이 완료될 때 실행됩니다. StringList 매개 변수에는 DownloadAsStringAsync 메서드에서 반환된 문자열 목록이 포함되고 함수는 처리가 필요한 작업을 수행합니다.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

취소 또는 진행 상황 보고를 지원하는 비동기 작업의 경우 [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) 클래스를 사용하여 시작된 작업을 생성하고 작업의 취소 및 진행 상황 보고 기능을 적절한 Windows 런타임 인터페이스의 취소 및 진행 상황 보고 기능과 연결합니다. 취소 및 진행 상황 보고를 모두 지원하는 예제는 [연습: C# 또는 Visual Basic에서 간단한 구성 요소를 만들어 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)을 참조하세요.

비동기 메서드가 취소 또는 진행 상황 보고를 지원하지 않는 경우에도 AsyncInfo 클래스의 메서드를 사용할 수 있습니다. Visual Basic 람다 함수 또는 C# 무명 메서드를 사용하는 경우 토큰 및 [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx) 인터페이스에 대한 매개 변수를 제공하지 마세요. C# 람다 함수를 사용하는 경우 토큰 매개 변수를 제공하되 무시합니다. Asasyncoperation 이전 예제에서는&lt;TResult&gt; 메서드를 사용 하면 다음과 같이 됩니다 합니다 [AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken, Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)) 메서드 대신 오버 로드 합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return AsyncInfo.Run<IList<string>>(async (token) =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     });
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>     As IAsyncOperation(Of IList(Of String))
>
>     Return AsyncInfo.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function)
> End Function
> ```

취소 또는 진행 상황 보고를 선택적으로 지원하는 비동기 메서드를 만드는 경우 취소 토큰 또는 IProgress&lt;T&gt; 인터페이스에 대한 매개 변수가 없는 오버로드를 추가하는 것이 좋습니다.

## <a name="throwing-exceptions"></a>예외 발생
Windows 앱용 .NET에 포함된 모든 예외 형식을 발생시킬 수 있습니다. Windows 런타임 구성 요소에서 자체 공용 예외 형식을 선언할 수는 없지만 비공용 형식을 선언하고 발생시킬 수 있습니다.

구성 요소에서 예외를 처리하지 않으면 구성 요소를 호출한 코드에서 해당 예외가 발생합니다. 호출자에 예외가 나타나는 방식은 호출 언어가 Windows 런타임을 지원하는 방식에 따라 달라집니다.

-   JavaScript에서 예외는 예외 메시지가 스택 추적으로 대체된 개체로 나타납니다. Visual Studio에서 앱을 디버그할 때 디버거 예외 대화 상자에서 원래 메시지 텍스트가 "WinRT 정보"로 식별되어 표시되는 것을 볼 수 있습니다. JavaScript 코드에서 원래 메시지 텍스트에 액세스할 수는 없습니다.

    > **팁**현재 스택 추적에는 관리 예외 형식이 포함 되어 있지만 추적을 구문 분석 예외 형식을 식별 하는 것이 좋습니다. 대신 이 섹션의 뒷부분에서 설명하는 HRESULT 값을 사용하세요.

-   C++에서 예외는 플랫폼 예외로 나타납니다. 관리 예외의 HResult 속성을 특정 플랫폼 예외의 HRESULT에 매핑할 수 있는 경우 해당 예외가 사용됩니다. 그렇지 않은 경우 [Platform::COMException](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx) 예외가 발생됩니다. C++ 코드에서는 관리 예외의 메시지 텍스트를 사용할 수 없습니다. 특정 플랫폼 예외가 발생된 경우 해당 예외 형식에 대한 기본 메시지 텍스트가 나타납니다. 그렇지 않은 경우 메시지 텍스트가 나타나지 않습니다. [예외(C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx)를 참조하세요.
-   C# 또는 Visual Basic에서 예외는 일반적인 관리 예외입니다.

구성 요소에서 예외가 발생할 때 HResult 속성 값이 해당 구성 요소와 관련된 비공용 예외 형식을 발생시켜 JavaScript 또는 C++ 호출자에서 예외를 쉽게 처리하도록 할 수 있습니다. 예외 개체의 숫자 속성을 통해 HRESULT를 JavaScript 호출자에 사용하고 [COMException::HResult](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx) 속성을 통해 C++ 호출자에 사용할 수 있습니다.

> **참고**에 대 한 HRESULT에 음수 값을 사용 합니다. 양수 값은 성공으로 해석되어 JavaScript 또는 C++ 호출자에서 예외가 발생되지 않습니다.

## <a name="declaring-and-raising-events"></a>이벤트 선언 및 발생
이벤트에 대한 데이터를 저장하는 형식을 선언하면 EventArgs 대신 Object에서 파생되는데 그 이유는 EventArgs가 Windows 런타임 형식이 아니기 때문입니다. [EventHandler&lt;TEventArgs&gt;](https://msdn.microsoft.com/library/db0etb8x.aspx)를 이벤트 형식으로 사용하고 이벤트 인수 형식을 제네릭 형식 인수로 사용합니다. 일반적인 .NET Framework 응용 프로그램에서와 마찬가지로 이벤트를 발생시킵니다.

Windows 런타임 구성 요소가 JavaScript 또는 C++에서 사용되는 경우 이벤트는 해당 언어에 필요한 Windows 런타임 이벤트 패턴을 따릅니다. C# 또는 Visual Basic의 구성 요소를 사용하면 이벤트가 일반 .NET Framework 이벤트로 나타납니다. 예제는 [연습: C# 또는 Visual Basic에서 간단한 구성 요소를 만들어 JavaScript에서 호출]()을 참조하세요.

사용자 지정 이벤트 접근자를 구현한 경우(Visual Basic에서는 **Custom** 키워드로 이벤트 선언) 해당 구현의 Windows 런타임 이벤트 패턴을 따라야 합니다. [Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자](custom-events-and-event-accessors-in-windows-runtime-components.md)를 참조하세요. C# 또는 Visual Basic 코드에서 이벤트를 처리하는 경우 일반 .NET Framework 이벤트처럼 나타납니다.

## <a name="next-steps"></a>다음 단계
직접 사용하기 위해 Windows 런타임 구성 요소를 만든 경우 해당 기능이 다른 개발자에게도 유용할 수 있습니다. 다른 개발자에게 배포하기 위해 구성 요소를 패키지하는 방법에는 두 가지가 있습니다. [관리되는 Windows 런타임 구성 요소 배포](https://msdn.microsoft.com/library/jj614475.aspx)를 참조하세요.

Visual Basic 및 C# 언어 기능과 Windows 런타임용 .NET Framework 지원에 대한 자세한 내용은 [Visual Basic 및 C# 언어 참조](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx)를 참조하세요.

## <a name="related-topics"></a>관련 항목
* [.NET UWP 앱 개요](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [UWP 앱용 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [연습: 단순한 Windows 런타임 구성 요소를 만들고 JavaScript에서 이를 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
