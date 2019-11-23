---
title: Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자
description: Windows 런타임 구성 요소에 대 한 .NET 지원을 사용 하면 유니버설 Windows 플랫폼 (UWP) 이벤트 패턴과 .NET 이벤트 패턴 간의 차이점을 숨겨서 이벤트 구성 요소를 쉽게 선언할 수 있습니다.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1891a89d83ea8a193d5c0fcedf939101323981e6
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340538"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자

Windows 런타임 구성 요소에 대 한 .NET 지원을 사용 하면 유니버설 Windows 플랫폼 (UWP) 이벤트 패턴과 .NET 이벤트 패턴 간의 차이점을 숨겨서 이벤트 구성 요소를 쉽게 선언할 수 있습니다. 그러나 Windows 런타임 구성 요소에서 사용자 지정 이벤트 접근자를 선언 하는 경우 UWP에서 사용 되는 패턴을 따라야 합니다.

## <a name="registering-events"></a>이벤트 등록

UWP에서 이벤트를 처리하도록 등록하면 추가 접근자에서 토큰을 반환합니다. 등록을 해제하려면 이 토큰을 제거 접근자로 전달합니다. 즉, UWP 이벤트의 추가 및 제거 접근자의 서명과 사용된 접근자의 서명은 다릅니다.

다행히 Visual Basic 및 C# 컴파일러는이 프로세스를 간소화 합니다. Windows 런타임 구성 요소에서 사용자 지정 접근자를 사용 하 여 이벤트를 선언 하면 컴파일러에서 자동으로 UWP 패턴을 사용 합니다. 예를 들어 추가 접근자에서 토큰을 반환하지 않으면 컴파일러 오류가 발생합니다. .NET에서는 구현을 지원 하기 위해 두 가지 형식을 제공 합니다.

-   [EventRegistrationToken](https://docs.microsoft.com/uwp/api/windows.foundation.eventregistrationtoken) 구조는 토큰을 나타냅니다.
-   [EventRegistrationTokenTable&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1) 클래스는 토큰을 만들고 토큰과 이벤트 처리기 간의 매핑을 유지합니다. 제네릭 형식 인수는 이벤트 인수 형식입니다. 각 이벤트에 대해 이 클래스의 인스턴스를 만들면 처음에는 이벤트 처리기가 해당 이벤트에 대해 등록됩니다.

NumberChanged 이벤트에 대한 다음 코드는 UWP 이벤트의 기본 패턴을 보여 줍니다. 이 예제에서는 이벤트 인수 개체 NumberChangedEventArgs의 생성자가 변경된 숫자 값을 나타내는 단일 정수 매개 변수를 사용합니다.

> **참고**  이 패턴은 컴파일러에서 Windows 런타임 구성 요소에 선언 하는 일반적인 이벤트에 사용 하는 패턴입니다.

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

정적(Visual Basic의 Shared) GetOrCreateEventRegistrationTokenTable 메서드는 나중에 EventRegistrationTokenTable&lt;T&gt; 개체의 이벤트 인스턴스를 만듭니다. 토큰 테이블 인스턴스를 포함할 클래스 수준 필드를 이 메서드에 전달합니다. 필드가 비어 있으면 메서드가 테이블을 만들고 테이블에 대한 참조를 필드에 저장하고 테이블에 대한 참조를 반환합니다. 필드에 이미 토큰 테이블 참조가 있는 경우 메서드가 해당 참조를 반환하기만 합니다.

> **중요**  스레드 안전을 보장 하려면 이벤트의 EventRegistrationTokenTable&lt;t&gt; 인스턴스를 보유 하는 필드가 클래스 수준 필드 여야 합니다. 클래스 수준 필드인 경우 GetOrCreateEventRegistrationTokenTable 메서드를 사용하면 여러 스레드에서 토큰 테이블을 만들려고 할 때 모든 스레드에서 동일한 테이블 인스턴스를 가져올 수 있습니다. 지정된 이벤트의 경우 GetOrCreateEventRegistrationTokenTable 메서드로의 모든 호출에서 동일한 클래스 수준 필드를 사용해야 합니다.

제거 접근자 및 [RaiseEvent](https://docs.microsoft.com/dotnet/articles/visual-basic/language-reference/statements/raiseevent-statement) 메서드(C#의 OnRaiseEvent 메서드)에서 GetOrCreateEventRegistrationTokenTable 메서드를 호출하면 이벤트 처리기 대리자가 추가되기 전에 이러한 메서드가 호출되는 경우 예외가 발생하지 않습니다.

그 밖에 UWP 이벤트 패턴에 사용되는 EventRegistrationTokenTable&lt;T&gt; 클래스의 멤버는 다음과 같습니다.

-   [AddEventHandler](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.addeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_AddEventHandler__0_) 메서드는 이벤트 처리기 대리자에 대한 토큰을 생성하고, 대리자를 테이블에 저장하고, 호출 목록에 추가하고, 토큰을 반환합니다.
-   [RemoveEventHandler(EventRegistrationToken)](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.removeeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_RemoveEventHandler_System_Runtime_InteropServices_WindowsRuntime_EventRegistrationToken_) 메서드 오버로드는 테이블과 호출 목록에서 대리자를 제거합니다.

    >**참고**  AddEventHandler 및 removeeventhandler (EventRegistrationToken) 메서드는 스레드 안전을 보장 하기 위해 테이블을 잠급니다.

-   [InvocationList](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.invocationlist#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_InvocationList) 속성은 현재 이벤트를 처리하도록 등록된 모든 이벤트 처리기를 포함하는 대리자를 반환합니다. 이 대리자를 사용하여 이벤트를 발생시키거나 Delegate 클래스의 메서드를 사용하여 처리기를 개별적으로 호출합니다.

    >**참고**  이 문서 앞부분에서 제공 된 예제에 표시 된 패턴을 따르고 대리자를 호출 하기 전에 임시 변수에 복사 하는 것이 좋습니다. 이렇게 하면 하나의 스레드가 마지막 처리기를 제거하여 다른 스레드가 대리자를 호출하려고 하기 전에 대리자가 null이 되는 경합 상태를 방지할 수 있습니다. 대리자는 변경할 수 없으므로 복사는 유효합니다.

직접 작성한 코드를 접근자에 적절히 배치합니다. 스레드 보안이 중요한 경우 코드에 대한 잠금을 직접 제공해야 합니다.

C# 사용자: UWP 이벤트 패턴에 사용자 지정 이벤트 접근자를 작성하는 경우 컴파일러에서는 일반적인 구문 바로 가기를 제공하지 않습니다. 코드에서 이벤트 이름을 사용하는 경우 오류가 발생합니다.

Visual Basic 사용자: .NET에서 이벤트는 등록 된 모든 이벤트 처리기를 나타내는 멀티 캐스트 대리자입니다. 이벤트를 발생시킨다는 것은 대리자를 호출한다는 것을 의미합니다. 스레드 보안에 대한 참고 사항에서 설명한 것처럼 일반적으로 Visual Basic 구문은 대리자와의 상호 작용을 숨기고 컴파일러는 호출 전 대리자를 복사합니다. Windows 런타임 구성 요소에서 사용자 지정 이벤트를 만들 때 대리자를 직접 처리 해야 합니다. 이는 예를 들어 처리기를 개별적으로 호출하려는 경우 [MulticastDelegate.GetInvocationList](https://docs.microsoft.com/dotnet/api/system.multicastdelegate.getinvocationlist#System_MulticastDelegate_GetInvocationList) 메서드를 사용하여 각 이벤트 처리기에 대한 개별 대리자가 포함된 배열을 가져올 수 있다는 의미이기도 합니다.

## <a name="related-topics"></a>관련 항목

* [이벤트 (Visual Basic)](https://docs.microsoft.com/dotnet/articles/visual-basic/programming-guide/language-features/events/index)
* [이벤트 (C# 프로그래밍 가이드)](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/events/index)
* [UWP 앱 용 .NET 개요](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [UWP 앱 용 .NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [C# 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출하는 연습](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
