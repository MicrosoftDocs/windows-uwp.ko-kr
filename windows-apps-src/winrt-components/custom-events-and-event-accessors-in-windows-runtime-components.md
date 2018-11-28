---
title: Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자
description: Windows 런타임 구성 요소에 대한 .NET Framework 지원을 사용하면 UWP(유니버설 Windows 플랫폼) 이벤트 패턴과 .NET Framework 이벤트 패턴 간의 차이점을 숨겨 이벤트 구성 요소를 쉽게 선언할 수 있습니다.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b8c4777e1c34bca36200bf6e8a96c35d6a0b1079
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7836547"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자



Windows 런타임 구성 요소에 대한 .NET Framework 지원을 사용하면 UWP(유니버설 Windows 플랫폼) 이벤트 패턴과 .NET Framework 이벤트 패턴 간의 차이점을 숨겨 이벤트 구성 요소를 쉽게 선언할 수 있습니다. 그러나 Windows 런타임 구성 요소에서 사용자 지정 이벤트 접근자를 선언할 때는 UWP에서 사용되는 패턴을 따라야 합니다.

## <a name="registering-events"></a>이벤트 등록


UWP에서 이벤트를 처리하도록 등록하면 추가 접근자에서 토큰을 반환합니다. 등록을 해제하려면 이 토큰을 제거 접근자로 전달합니다. 즉, UWP 이벤트의 추가 및 제거 접근자의 서명과 사용된 접근자의 서명은 다릅니다.

Visual Basic 및 C# 컴파일러를 사용하면 이 프로세스를 간단히 수행할 수 있습니다. Windows 런타임 구성 요소에서 사용자 지정 접근자로 이벤트를 선언할 때 컴파일러에서 자동으로 UWP 패턴을 사용합니다. 예를 들어 추가 접근자에서 토큰을 반환하지 않으면 컴파일러 오류가 발생합니다. .NET Framework에서는 구현을 지원하는 두 가지 형식을 제공합니다.

-   [EventRegistrationToken](https://msdn.microsoft.com/library/windows/apps/windows.foundation.eventregistrationtoken.aspx) 구조는 토큰을 나타냅니다.
-   [EventRegistrationTokenTable&lt;T&gt;](https://msdn.microsoft.com/library/hh138412.aspx) 클래스는 토큰을 만들고 토큰과 이벤트 처리기 간의 매핑을 유지합니다. 제네릭 형식 인수는 이벤트 인수 형식입니다. 각 이벤트에 대해 이 클래스의 인스턴스를 만들면 처음에는 이벤트 처리기가 해당 이벤트에 대해 등록됩니다.

NumberChanged 이벤트에 대한 다음 코드는 UWP 이벤트의 기본 패턴을 보여 줍니다. 이 예제에서는 이벤트 인수 개체 NumberChangedEventArgs의 생성자가 변경된 숫자 값을 나타내는 단일 정수 매개 변수를 사용합니다.

> **참고**이 Windows 런타임 구성 요소에서 선언 된 일반 이벤트에 대해 컴파일러를 사용할 동일한 패턴입니다.

 
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

> **중요 한**스레드 보안을 위해 EventRegistrationTokenTable 이벤트의 인스턴스를 포함 하는 필드를&lt;T&gt; 는 클래스 수준 필드 여야 합니다. 클래스 수준 필드인 경우 GetOrCreateEventRegistrationTokenTable 메서드를 사용하면 여러 스레드에서 토큰 테이블을 만들려고 할 때 모든 스레드에서 동일한 테이블 인스턴스를 가져올 수 있습니다. 지정된 이벤트의 경우 GetOrCreateEventRegistrationTokenTable 메서드로의 모든 호출에서 동일한 클래스 수준 필드를 사용해야 합니다.

제거 접근자 및 [RaiseEvent](https://msdn.microsoft.com/library/fwd3bwed.aspx) 메서드(C#의 OnRaiseEvent 메서드)에서 GetOrCreateEventRegistrationTokenTable 메서드를 호출하면 이벤트 처리기 대리자가 추가되기 전에 이러한 메서드가 호출되는 경우 예외가 발생하지 않습니다.

그 밖에 UWP 이벤트 패턴에 사용되는 EventRegistrationTokenTable&lt;T&gt; 클래스의 멤버는 다음과 같습니다.

-   [AddEventHandler](https://msdn.microsoft.com/library/hh138458.aspx) 메서드는 이벤트 처리기 대리자에 대한 토큰을 생성하고, 대리자를 테이블에 저장하고, 호출 목록에 추가하고, 토큰을 반환합니다.
-   [RemoveEventHandler(EventRegistrationToken)](https://msdn.microsoft.com/library/hh138425.aspx) 메서드 오버로드는 테이블과 호출 목록에서 대리자를 제거합니다.

    >**참고**AddEventHandler 및 removeeventhandler (eventregistrationtoken) 메서드는 스레드 보안을 위해 테이블을 잠급니다.

-   [InvocationList](https://msdn.microsoft.com/library/hh138465.aspx) 속성은 현재 이벤트를 처리하도록 등록된 모든 이벤트 처리기를 포함하는 대리자를 반환합니다. 이 대리자를 사용하여 이벤트를 발생시키거나 Delegate 클래스의 메서드를 사용하여 처리기를 개별적으로 호출합니다.

    >**참고**이 문서의 앞부분에서 제공 되는 예제에 표시 된 패턴에 따라 호출 하기 전에 대리자를 임시 변수로 복사 하는 것이 좋습니다. 이렇게 하면 하나의 스레드가 마지막 처리기를 제거하여 다른 스레드가 대리자를 호출하려고 하기 전에 대리자가 null이 되는 경합 상태를 방지할 수 있습니다. 대리자는 변경할 수 없으므로 복사는 유효합니다.

직접 작성한 코드를 접근자에 적절히 배치합니다. 스레드 보안이 중요한 경우 코드에 대한 잠금을 직접 제공해야 합니다.

C# 사용자: UWP 이벤트 패턴에 사용자 지정 이벤트 접근자를 작성하는 경우 컴파일러에서는 일반적인 구문 바로 가기를 제공하지 않습니다. 코드에서 이벤트 이름을 사용하는 경우 오류가 발생합니다.

Visual Basic 사용자: .NET Framework에서 이벤트는 등록된 모든 이벤트 처리기를 나타내는 멀티캐스트 대리자입니다. 이벤트를 발생시킨다는 것은 대리자를 호출한다는 것을 의미합니다. 스레드 보안에 대한 참고 사항에서 설명한 것처럼 일반적으로 Visual Basic 구문은 대리자와의 상호 작용을 숨기고 컴파일러는 호출 전 대리자를 복사합니다. Windows 런타임 구성 요소에서 사용자 지정 이벤트를 만들 때 대리자를 직접 처리해야 합니다. 이는 예를 들어 처리기를 개별적으로 호출하려는 경우 [MulticastDelegate.GetInvocationList](https://msdn.microsoft.com/library/system.multicastdelegate.getinvocationlist.aspx) 메서드를 사용하여 각 이벤트 처리기에 대한 개별 대리자가 포함된 배열을 가져올 수 있다는 의미이기도 합니다.

## <a name="related-topics"></a>관련 항목

* [이벤트(Visual Basic)](https://msdn.microsoft.com/library/ms172877.aspx)
* [이벤트(C# 프로그래밍 가이드)](https://msdn.microsoft.com/library/awbftdfh.aspx)
* [.NET UWP 앱 개요](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [UWP 앱용 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [연습: 단순한 Windows 런타임 구성 요소를 만들고 JavaScript에서 이를 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
