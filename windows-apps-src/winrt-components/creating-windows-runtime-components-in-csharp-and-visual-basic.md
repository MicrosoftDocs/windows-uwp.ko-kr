---
title: C# 및 Visual Basic으로 Windows Runtime 구성 요소 만들기
description: .NET Framework 4.5부터 관리 코드를 사용하여 Windows 런타임 구성 요소에 패키지된 Windows 런타임 형식을 직접 만들 수 있습니다.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a20f8e85927015d6aa69dbbf4381cb2d7daafc36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372037"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>C# 및 Visual Basic으로 Windows Runtime 구성 요소 만들기
.NET Framework 4.5부터, Windows 런타임 형식을 사용자를 만들고 Windows 런타임 구성 요소를 패키지 하거나 관리 되는 코드를 사용할 수 있습니다. 구성 요소를 사용 하 여 작성 된 유니버설 Windows 플랫폼 (UWP) 앱에서 C++, JavaScript, Visual Basic 또는 C#합니다. 이 항목에서는 구성 요소 만들기 규칙에 대해 간략히 설명하고, Windows 런타임에 대한 .NET Framework 지원의 몇 가지 측면에 대해 설명합니다. 일반적으로 이 지원은 .NET Framework 프로그래머에게 투명하게 디자인되었습니다. 그러나 JavaScript 또는 C++를 사용하는 구성 요소를 만들 때는 이러한 언어와 Windows 런타임을 지원하는 방식의 차이점을 알아야 합니다.

Visual Basic에서 작성 된 UWP 앱 에서만에서 사용에 대 한 구성 요소를 만드는 경우 또는 C#, UWP 컨트롤을 사용 하는 것을 고려 하세요 구성 요소가 포함 되어 있지 않습니다 합니다 **클래스 라이브러리** 템플릿 대신 합니다 **Windows 런타임 구성 요소** Microsoft Visual Studio 프로젝트 템플릿. 간단한 클래스 라이브러리일수록 제한이 적습니다.

## <a name="declaring-types-in-windows-runtime-components"></a>Windows 런타임 구성 요소에서 형식 선언

내부적으로 Windows 런타임 형식 구성 요소에서 UWP 앱에서 허용 되는 모든.NET Framework 기능을 사용할 수 있습니다. 자세한 내용은 참조 하세요. [UWP 앱 용.NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)합니다.

외부적으로 형식의 멤버는 해당 매개 변수에 대 한 Windows 런타임 형식에만 노출 하 고 값을 반환할 수 있습니다. 다음은 Windows 런타임 구성 요소에서 노출 하는.NET Framework 형식에 제한 사항을 설명 합니다.

- 구성 요소에 있는 모든 공용 형식 및 멤버의 필드, 매개 변수 및 반환 값은 Windows 런타임 형식이어야 합니다. 이 제한 및 Windows 런타임 자체에서 제공 되는 형식을 작성 하는 Windows 런타임 형식이 포함 됩니다. 또한 다양한 .NET Framework 형식도 포함됩니다. 이러한 형식 포함 일부인 지원의.NET Framework 관리 코드에서 Windows 런타임 자연스럽 게 사용할 수 있도록 제공&mdash;기본 Windows 런타임 형식 대신 익숙한.NET Framework 형식을 사용 하 여 코드가 나타납니다. 와 같은.NET Framework에 대 한 형식 정보를 사용할 수는 예를 들어 **Int32** 하 고 **Double**와 같은 특정 기본 형식 **DateTimeOffset** 고 **Uri** , 및 일반적으로 사용 되는 일부 제네릭 인터페이스 형식을 같은 **IEnumerable&lt;T&gt;**  (Visual Basic에서 IEnumerable (Of T)) 및 **IDictionary&lt; TKey, TValue&gt;** 합니다. 이러한 제네릭 형식의 형식 인수를 Windows 런타임 형식 이어야 하는 참고 합니다. 이 섹션에 설명 되어 [Windows 런타임 형식 전달 관리 코드로](#passing-windows-runtime-types-to-managed-code) 하 고 [관리 되는 Windows 런타임 형식 전달](#passing-managed-types-to-the-windows-runtime)이 항목의 뒷부분에 나오는.

- 공용 클래스와 인터페이스에는 메서드, 속성 및 이벤트가 포함될 수 있습니다. 이벤트에 대 한 대리자를 선언 하거나 사용할 수 있습니다 합니다 **EventHandler&lt;T&gt;**  위임 합니다. 공용 클래스 또는 인터페이스 수 없습니다.
    - 제네릭이 아니어야 합니다.
    - 그러나 Windows 런타임 인터페이스를 없는 인터페이스 구현 (만들고 수 있습니다 고유한 Windows 런타임 인터페이스 구현).
    - 와 같은 Windows 런타임에서 되지 않는 형식에서 파생 **System.Exception** 하 고 **System.EventArgs**합니다.

- 모든 공용 형식에는 어셈블리 이름과 일치하는 루트 네임스페이스가 있어야 하며 어셈블리 이름이 "Windows"로 시작해서는 안 됩니다.

    > **팁**합니다. 기본적으로 Visual Studio 프로젝트 어셈블리 이름과 일치 하는 네임 스페이스 이름을 있습니다. Visual Basic에서 이 기본 네임스페이스에 대한 Namespace 문은 코드에 표시되지 않습니다.

- 공용 구조에는 공용 필드 이외의 멤버를 포함할 수 없으며 이러한 필드는 값 형식 또는 문자열이어야 합니다.
- 공용 클래스는 **sealed**(Visual Basic의 **NotInheritable**)여야 합니다. 프로그래밍 모델에 다형성이 필요한 경우 그런 다음 수는 공용 인터페이스를 만들고 다형 해야 하는 클래스에서 해당 인터페이스를 구현 합니다.

## <a name="debugging-your-component"></a>구성 요소 디버깅

UWP 앱 및 구성 요소를 모두 관리 코드로 빌드된 됩니다을 하는 경우 다음 디버깅할 수 있습니다 둘 다 동시에.

사용 하 여 UWP 앱의 일부로 구성 요소를 테스트 하는 경우 C++를 동시에 관리 및 네이티브 코드를 디버깅할 수 있습니다. 기본적으로는 네이티브 코드만 디버그합니다.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>네이티브 C++ 코드와 관리 코드를 모두 디버그하려면
1.  Visual C++ 프로젝트에서 바로 가기 메뉴를 열고 **속성**을 선택합니다.
2.  속성 페이지의 **구성 속성**에서 **디버깅**을 클릭합니다.
3.  **디버거 유형**을 선택하고 드롭다운 목록 상자에서 **네이티브만**을 **혼합(관리 및 네이티브)** 로 변경합니다. **확인**을 선택합니다.
4.  네이티브 및 관리 코드에 중단점을 설정합니다.

JavaScript를 사용 하 여 UWP 앱의 일부로 구성 요소를 테스트할 때 기본적으로 솔루션은 JavaScript 디버깅 모드입니다. Visual Studio에서는 JavaScript와 관리 코드를 동시에 디버그할 수 없습니다.

## <a name="to-debug-managed-code-instead-of-javascript"></a>JavaScript 대신 관리 코드를 디버그하려면
1.  JavaScript 프로젝트에서 바로 가기 메뉴를 열고 **속성**을 선택합니다.
2.  속성 페이지의 **구성 속성**에서 **디버깅**을 클릭합니다.
3.  **디버거 유형**을 선택하고 드롭다운 목록 상자에서 **스크립트만**을 **관리만**으로 변경합니다. **확인**을 선택합니다.
4.  관리 코드에 중단점을 설정하고 일반적인 방법으로 디버그합니다.

## <a name="passing-windows-runtime-types-to-managed-code"></a>관리 코드에 Windows 런타임 형식 전달
섹션에서 설명한 것 처럼 [Windows 런타임 구성 요소에서 선언 형식](#declaring-types-in-windows-runtime-components)특정.NET Framework 형식은 공용 클래스 멤버의 시그니처에 나타날 수 있습니다. 이는 관리 코드에서 Windows 런타임을 자연스럽게 사용할 수 있도록 하기 위해 .NET Framework에서 제공하는 지원의 일부입니다. 여기에는 기본 형식 및 일부 클래스와 인터페이스가 포함됩니다. 또는 JavaScript에서 구성 요소가 사용 되는 경우 C++ 는.NET Framework 형식이 호출자에 게 표시 하는 방법을 알아야 합니다. 참조 [연습: 간단한 구성 요소를 만드는 C# 또는 Visual Basic 및 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) JavaScript 사용한 예입니다. 이 섹션에서는 자주 사용되는 형식에 대해 설명합니다.

.NET framework에서 기본 형식-는 **Int32** 와 같은 구조체는 여러 유용한 속성 및 메서드는 **TryParse** 메서드. 반면, Windows 런타임의 기본 형식과 구조에는 필드만 있습니다. 관리 코드로 이러한 형식을 전달하면 .NET Framework 형식으로 나타나므로 일반적인 방법으로 .NET Framework 형식의 속성 및 메서드를 사용할 수 있습니다. 다음 목록에는 IDE에서 자동으로 만들어지는 대체 항목이 요약되어 있습니다.

-   Windows 런타임 기본 형식에 대해 **Int32**, **Int64**를 **Single**, **Double**를 **부울**,  **문자열** (유니코드 문자를 변경할 수 없는 컬렉션), **열거형**, **UInt32**하십시오 **UInt64**, 및 **Guid**, System 네임 스페이스에서 이름이 동일한 형식을 사용 합니다.
-   에 대 한 **UInt8**를 사용 하 여 **System.Byte**합니다.
-   에 대 한 **Char16**를 사용 하 여 **System.Char**합니다.
-   에 대 한 합니다 **IInspectable** 인터페이스를 사용 하 여 **System.Object**합니다.

경우 C# 또는 이러한 형식에 대 한 언어 키워드를 제공 하는 Visual Basic을 대신 언어 키워드를 사용할 수 있습니다.

기본 형식 외에도 자주 사용되는 일부 기본 Windows 런타임 형식은 관리 코드에서 해당하는 .NET Framework 형식으로 나타납니다. 예를 들어 JavaScript 코드에서 사용 하 여 **Windows.Foundation.Uri** 클래스에 전달 하도록 설정 하려는 C# 또는 Visual Basic 메서드. 관리 되는 코드에서 해당 하는 형식은.NET Framework **System.Uri** 클래스, 메서드 매개 변수로 사용 하는 형식입니다. 관리 코드를 작성할 때 Visual Studio의 IntelliSense는 Windows 런타임 형식을 숨기고 해당하는 .NET Framework 형식을 표시하므로 Windows 런타임 형식이 언제 .NET Framework 형식으로 나타나는지 알 수 있습니다. 보통 두 형식은 이름이 같습니다. 그러나 합니다 **Windows.Foundation.DateTime** 구조는 관리 코드에 표시 됩니다 **System.DateTimeOffset** 아니라 **System.DateTime**.)

자주 사용하는 컬렉션 형식의 경우 Windows 런타임 형식으로 구현된 인터페이스와 해당 .NET Framework 형식으로 구현된 인터페이스가 매핑되어 있습니다. 위에서 설명한 형식과 마찬가지로, .NET Framework 형식을 사용하여 매개 변수 형식을 선언합니다. 그러면 형식 간의 일부 차이점이 숨겨지고 .NET Framework 코드 작성이 더 자연스러워집니다.

다음 표에서는 이러한 제네릭 인터페이스 형식 중 가장 일반적인 항목과 기타 일반 클래스 및 인터페이스 매핑을 나열합니다. .NET Framework에서 매핑되는 Windows 런타임 형식의 전체 목록은 참조 하세요 [Windows 런타임 형식의.NET Framework 매핑](net-framework-mappings-of-windows-runtime-types.md)합니다.

| Windows 런타임                                  | .NET Framework                                    |
|-|-|
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

하나의 형식이 두 개 이상의 인터페이스를 구현하는 경우 구현된 인터페이스 중 하나만 매개 변수 형식 또는 멤버의 반환 형식으로 사용할 수 있습니다. 전달 하거나 반환할 수 있습니다 예를 들어를 **사전&lt;int, string&gt;**  (**Dictionary (Of Integer, String)** Visual basic에서)으로 **IDictionary&lt;int, string&gt;** 하십시오 **IReadOnlyDictionary&lt;int, string&gt;** , 또는 **IEnumerable&lt; System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;&gt;** 합니다.

> [!IMPORTANT]
> JavaScript는 관리 되는 형식이 구현 하는 인터페이스 목록에서 가장 먼저 나타나는 인터페이스를 사용 합니다. 예를 들어, 반환 하는 경우 **사전&lt;int, string&gt;**  JavaScript 코드에 나올 **IDictionary&lt;int, string&gt;**  관계 없이 반환 형식으로 지정 하는 인터페이스입니다. 즉, 첫 번째 인터페이스가 나머지 인터페이스에 나타나는 멤버를 포함하고 있지 않은 경우 해당 멤버는 JavaScript에 표시되지 않습니다.

Windows 런타임에서 **IMap&lt;K, V&gt;**  하 고 **IMapView&lt;K, V&gt;**  IKeyValuePair를 사용 하 여 반복 됩니다. 으로 표시 되 고 관리 코드에 전달 하는 경우 **IDictionary&lt;TKey, TValue&gt;**  하 고 **IReadOnlyDictionary&lt;TKey, TValue&gt;** 이므로 사용 하는 자연스럽 게 **System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;**  를 열거 해야 합니다.

인터페이스가 관리 코드에 나타나는 방법은 이러한 인터페이스를 구현한 형식이 나타나는 방법에 영향을 줍니다. 예를 들어를 **PropertySet** 클래스 구현 **IMap&lt;K, V&gt;** 에 관리 코드에 나타나는 **IDictionary&lt;TKey, TValue&gt;** . **PropertySet** 구현한 것 처럼 나타나며 **IDictionary&lt;TKey, TValue&gt;**  of **IMap&lt;K, V&gt;** 에서 관리 에 게 표시 되는 코드는 **추가** 메서드처럼 동작 메서드를 **추가** .NET Framework 사전의 메서드. 할 나타나지는 **삽입** 메서드. 항목의이 예제를 볼 수 있습니다 [연습: 간단한 구성 요소를 만드는 C# 또는 Visual Basic 및 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)합니다.

## <a name="passing-managed-types-to-the-windows-runtime"></a>Windows 런타임에 관리 형식 전달

이전 섹션에서 설명한 것처럼 일부 Windows 런타임 형식은 구성 요소의 멤버 서명 또는 IDE에 사용된 Windows 런타임 멤버 서명에서 .NET Framework 형식으로 나타납니다. 이러한 멤버에 .NET Framework 형식을 전달하거나 구성 요소 멤버의 반환 값으로 사용할 경우 다른 쪽의 코드에서는 해당 Windows 런타임 형식으로 나타납니다. "관리 되는 반환 형식 구성 요소에서" 섹션을 참조 하는 구성 요소를 JavaScript에서 호출할 때 이러한 점이 효과 예가 [연습: 간단한 구성 요소를 만드는 C# 또는 Visual Basic 및 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)합니다.

## <a name="overloaded-methods"></a>오버로드된 메서드

Windows 런타임에서 메서드가 오버로드될 수 있습니다. 그러나 매개 변수 수가 같은 오버 로드를 여러 개를 선언 하는 경우 적용 해야 합니다 [ **Windows.Foundation.Metadata.DefaultOverloadAttribute** ](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) 특성을 해당 오버 로드 중 하나에 합니다. 해당 오버로드만 JavaScript에서 호출할 수 있습니다. 예를 들어 다음 코드에서 **int**(Visual Basic의 **Integer**)가 사용된 오버로드가 기본 오버로드입니다.

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

> [중요] JavaScript를 사용 하면 모든 값을 전달할 **OverloadExample**, 값을 매개 변수에 필요한 형식으로 강제 변환 하 고 있습니다. 호출할 수 있습니다 **OverloadExample** "40-2"를 사용 하 여 "42" 또는 42.3, 하지만 모든 해당 값은 기본 오버 로드에 전달 합니다. 이전 예제의 기본 오버 로드 각각 0, 42 및 42를 반환합니다.

적용할 수 없습니다는 **DefaultOverloadAttribut**e 특성 생성자입니다. 클래스의 모든 생성자는 매개 변수 수가 달라야 합니다.

## <a name="implementing-istringable"></a>IStringable 구현

Windows 8.1 Windows 런타임 포함을 **IStringable** 인 단일 메서드를 인터페이스 **IStringable.ToString**, 기본 서식 지정 지원을 제공 제공하는 **Object.ToString**합니다. 구현 하려는 경우 **IStringable** Windows 런타임 구성 요소에서 내보낸 공용 관리 되는 형식에서 다음과 같은 제한 사항이 적용 합니다.

-   정의할 수 있습니다 합니다 **IStringable** 인터페이스에서 다음 코드와 같은 "클래스 구현" 관계 에서만에서 C#:

    ```cs
    public class NewClass : IStringable
    ```

    또는 다음 Visual Basic 코드에서:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   구현할 수 없습니다 **IStringable** 인터페이스에서 합니다.
-   형식 매개 변수를 선언할 수 없습니다 **IStringable**합니다.
-   **IStringable** 반환 형식의 메서드, 속성 또는 필드 일 수 없습니다.
-   숨길 수 없습니다 하 **IStringable** 다음과 같은 메서드 정의 사용 하 여 기본 클래스에서 구현 합니다.

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    대신 합니다 **IStringable.ToString** 구현은 언제나 기본 클래스 구현을 재지정 해야 합니다. 숨길 수 있습니다는 **ToString** 강력한 형식의 클래스 인스턴스에서 호출 해야만 구현 합니다.

> [!NOTE]
> 다양 한 조건에서 호출 네이티브 코드에서 구현 하는 관리 되는 형식을 **IStringable** 표시 하거나 숨깁니다 해당 **ToString** 구현에 예기치 않은 동작이 발생할 수 있습니다.

## <a name="asynchronous-operations"></a>비동기 작업

구성 요소에서 비동기 메서드를 구현 하려면 메서드 이름의 끝에 "Async"를 추가 하 고 비동기 작업 또는 작업을 나타내는 Windows 런타임 인터페이스 중 하나를 반환 합니다. **IAsyncAction**, **IAsyncActionWithProgress&lt;TProgress&gt;** 하십시오 **IAsyncOperation&lt;TResult&gt;** , 또는 **IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** 합니다.

.NET Framework 작업을 사용할 수 있습니다 (합니다 [ **태스크** ](/dotnet/api/system.threading.tasks.task) 클래스 및 제네릭 [ **태스크&lt;TResult&gt;**  ](/dotnet/api/system.threading.tasks.task-1) 클래스)를 비동기 메서드를 구현 합니다. 작성 된 비동기 메서드에서 반환 되는 작업 등 진행 중인 작업을 나타내는 작업을 반환 해야 합니다 C# 에서 반환 되는 작업 또는 Visual Basic의 [ **Task.Run** ](/dotnet/api/system.threading.tasks.task.run) 메서드입니다. 생성자를 사용하여 작업을 만드는 경우 반환하기 전에 해당 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 메서드를 호출해야 합니다.

사용 하는 메서드 `await` (`Await` Visual Basic의) 필요 합니다 `async` 키워드 (`Async` Visual basic에서). Windows 런타임 구성 요소에서 이러한 메서드를 노출 하는 경우 적용 합니다 `async` 키워드에 전달 하는 대리자에는 **실행** 메서드.

취소 또는 진행 상황 보고를 지원하지 않는 비동기 작업의 경우 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) 또는 [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) 확장 메서드를 사용하여 작업을 적절한 인터페이스에 래핑할 수 있습니다. 다음 코드를 사용 하 여 비동기 메서드를 구현 하는 예를 들어 합니다 **Task.Run&lt;TResult&gt;**  메서드 작업을 시작 합니다. 합니다 **AsAsyncOperation&lt;TResult&gt;**  확장 메서드는 Windows 런타임 비동기 작업으로 작업을 반환 합니다.

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

다음 JavaScript 코드를 표시 하는 방법을 사용 하 여 메서드를 호출할 수 없습니다는 [ **WinJS.Promise** ](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10)) 개체입니다. 다음 메서드에 전달된 함수는 비동기 호출이 완료될 때 실행됩니다. StringList 매개 변수에서 반환 되는 문자열의 목록을 포함 합니다 **DownloadAsStringAsync** 메서드 및 함수는 필요한 모든 처리를 수행 합니다.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

비동기 작업 및 작업 취소를 지원 또는 진행률 보고에 대해 사용 합니다 [ **AsyncInfo** ](/dotnet/api/system.runtime.interopservices.windowsruntime) 클래스 시작된 작업을 생성 하 고 연결 및 진행률 보고 취소 및 진행률 보고 적절 한 Windows 런타임 인터페이스의 기능을 사용 하 여 태스크의 기능입니다. 취소와 진행률 보고를 지 원하는 예제를 참조 하세요. [연습: 간단한 구성 요소를 만드는 C# 또는 Visual Basic 및 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)합니다.

메서드를 사용할 수 있습니다 합니다 **AsyncInfo** 비동기 메서드가 취소를 지원 하지 않습니다 및 진행률 보고 하는 경우에 클래스입니다. Visual Basic 람다 함수를 사용 하는 경우 C# 무명 메서드는 토큰에 대 한 매개 변수를 제공 하지 않는 및 [ **IProgress&lt;T&gt;**  ](https://docs.microsoft.com/dotnet/api/system.iprogress-1?redirectedfrom=MSDN) 인터페이스입니다. C# 람다 함수를 사용하는 경우 토큰 매개 변수를 제공하되 무시합니다. AsAsyncOperation 사용 되는 이전 예제의&lt;TResult&gt; 사용 하는 경우 메서드를 다음과 같이 표시 됩니다는 [ **AsyncInfo.Run&lt;TResult&gt;(Func&lt; CancellationToken, Task&lt;TResult&gt;&gt;** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime?redirectedfrom=MSDN)) 메서드 오버 로드를 대신 합니다.

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

필요에 따라 취소 또는 진행률 보고를 지 원하는 비동기 메서드를 만드는 경우 취소 토큰에 대 한 매개 변수가 없는 오버 로드를 추가 하십시오 또는 **IProgress&lt;T&gt;**  인터페이스입니다.

## <a name="throwing-exceptions"></a>예외 발생

Windows 앱용 .NET에 포함된 모든 예외 형식을 발생시킬 수 있습니다. Windows 런타임 구성 요소에서 자체 공용 예외 형식을 선언할 수는 없지만 비공용 형식을 선언하고 발생시킬 수 있습니다.

구성 요소에서 예외를 처리하지 않으면 구성 요소를 호출한 코드에서 해당 예외가 발생합니다. 호출자에 예외가 나타나는 방식은 호출 언어가 Windows 런타임을 지원하는 방식에 따라 달라집니다.

-   JavaScript에서 예외는 예외 메시지가 스택 추적으로 대체된 개체로 나타납니다. Visual Studio에서 앱을 디버그할 때 디버거 예외 대화 상자에서 원래 메시지 텍스트가 "WinRT 정보"로 식별되어 표시되는 것을 볼 수 있습니다. JavaScript 코드에서 원래 메시지 텍스트에 액세스할 수는 없습니다.

    > **팁**합니다. 현재, 스택 추적에는 관리 되는 예외 형식에 포함 되어 있지만 예외 형식을 식별 하기 위해 추적을 구문 분석 하는 것이 좋습니다. 대신 이 섹션의 뒷부분에서 설명하는 HRESULT 값을 사용하세요.

-   C++에서 예외는 플랫폼 예외로 나타납니다. 관리 되는 예외의 HResult 속성이 특정 플랫폼 예외의 HRESULT에 매핑될 수, 특정 예외가 사용 됩니다. 그렇지 않은 경우는 [ **platform:: comexception** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class) 예외가 throw 됩니다. C++ 코드에서는 관리 예외의 메시지 텍스트를 사용할 수 없습니다. 특정 플랫폼 예외가 발생된 경우 해당 예외 형식에 대한 기본 메시지 텍스트가 나타납니다. 그렇지 않은 경우 메시지 텍스트가 나타나지 않습니다. [예외(C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx)를 참조하세요.
-   C# 또는 Visual Basic에서 예외는 일반적인 관리 예외입니다.

구성 요소에서 예외가 발생할 때 HResult 속성 값이 해당 구성 요소와 관련된 비공용 예외 형식을 발생시켜 JavaScript 또는 C++ 호출자에서 예외를 쉽게 처리하도록 할 수 있습니다. HRESULT 및 예외 개체의 숫자 속성을 통해 JavaScript 호출자에 게 사용할 수는 C++ 를 통해 호출자에 게는 [ **comexception:: Hresult** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult) 속성입니다.

> [!NOTE]
> HRESULT에 대 한 음수 값을 사용 합니다. 양수 값은 성공으로 해석되어 JavaScript 또는 C++ 호출자에서 예외가 발생되지 않습니다.

## <a name="declaring-and-raising-events"></a>이벤트 선언 및 발생

이벤트에 대한 데이터를 저장하는 형식을 선언하면 EventArgs 대신 Object에서 파생되는데 그 이유는 EventArgs가 Windows 런타임 형식이 아니기 때문입니다. 사용 하 여 [ **EventHandler&lt;TEventArgs&gt;**  ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1?redirectedfrom=MSDN) 형식의 이벤트 및 제네릭 형식 인수로 이벤트 인수 형식을 사용 합니다. 일반적인 .NET Framework 응용 프로그램에서와 마찬가지로 이벤트를 발생시킵니다.

Windows 런타임 구성 요소가 JavaScript 또는 C++에서 사용되는 경우 이벤트는 해당 언어에 필요한 Windows 런타임 이벤트 패턴을 따릅니다. C# 또는 Visual Basic의 구성 요소를 사용하면 이벤트가 일반 .NET Framework 이벤트로 나타납니다. 예제에서 [연습: 간단한 구성 요소를 만드는 C# 또는 Visual Basic 및 JavaScript에서 호출](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)합니다.

사용자 지정 이벤트 접근자를 구현한 경우(Visual Basic에서는 **Custom** 키워드로 이벤트 선언) 해당 구현의 Windows 런타임 이벤트 패턴을 따라야 합니다. [Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자](custom-events-and-event-accessors-in-windows-runtime-components.md)를 참조하세요. C# 또는 Visual Basic 코드에서 이벤트를 처리하는 경우 일반 .NET Framework 이벤트처럼 나타납니다.

## <a name="next-steps"></a>다음 단계

직접 사용하기 위해 Windows 런타임 구성 요소를 만든 경우 해당 기능이 다른 개발자에게도 유용할 수 있습니다. 다른 개발자에게 배포하기 위해 구성 요소를 패키지하는 방법에는 두 가지가 있습니다. [관리되는 Windows 런타임 구성 요소 배포](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140))를 참조하세요.

Visual Basic 및 C# 언어 기능과 Windows 런타임용 .NET Framework 지원에 대한 자세한 내용은 [Visual Basic 및 C# 언어 참조](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)를 참조하세요.

## <a name="related-topics"></a>관련 항목
* [UWP 앱 용.NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [연습: 간단한 Windows 런타임 구성 요소를 만들고 JavaScript에서 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
