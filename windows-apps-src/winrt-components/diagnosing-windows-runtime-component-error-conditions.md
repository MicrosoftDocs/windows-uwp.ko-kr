---
title: Windows 런타임 구성 요소 오류 조건 진단
description: 이 문서는 관리 코드로 작성된 Windows 런타임 구성 요소의 제한에 대한 추가 정보를 제공합니다.
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4733edba06b7042c436918e882556f86dfa00071
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8327200"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>Windows 런타임 구성 요소 오류 조건 진단




이 문서는 관리 코드로 작성된 Windows 런타임 구성 요소의 제한에 대한 추가 정보를 제공합니다. [Winmdexp.exe(Windows 런타임 메타데이터 내보내기 도구)](https://msdn.microsoft.com/library/hh925576.aspx)의 오류 메시지에서 제공되는 정보에 대해 더 자세히 설명하고 [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에서 제공되는 제한 사항에 대한 정보를 보충합니다.

이 문서에 모든 오류가 다 설명되어 있지는 않습니다. 여기에서 설명하는 오류는 일반적인 범주별로 나뉘어 있으며 각 범주별로 관련 오류 메시지 표가 제공됩니다. 메시지 텍스트(자리 표시자에 대한 특정 값 생략) 또는 메시지 번호를 검색합니다. 여기에서 필요한 정보를 찾지 못한 경우 이 문서 마지막에 있는 피드백 단추를 사용하여 이 설명서를 개선할 수 있도록 도와주시기 바랍니다. 오류 메시지를 포함시켜 주세요. 또는 Microsoft Connect 웹 사이트에서 버그를 보고할 수도 있습니다.

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>비동기 인터페이스 구현에 대한 오류 메시지가 잘못된 형식 제공


관리되는 Windows 런타임 구성 요소는 비동기 작업을 나타내는 UWP(유니버설 Windows 플랫폼) 인터페이스를 구현할 수 없습니다([IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br206598.aspx) 또는 [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)). 대신 .NET Framework가 Windows 런타임 구성 요소에서 비동기 작업을 생성하기 위해 [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) 클래스를 제공합니다. 비동기 인터페이스를 잘못 구현하려고 할 때 Winmdexp.exe가 표시하는 오류 메시지가 이 클래스를 이전 이름인 AsyncInfoFactory로 나타냅니다. .NET Framework는 더 이상 AsyncInfoFactory 클래스를 포함하지 않습니다.

| 오류 번호 | 메시지 내용|       
|--------------|-------------|
| WME1084      | 형식 '{0}'구현 하는 Windows 런타임 비동기 인터페이스'{1}'. Windows 런타임 형식은 비동기 인터페이스를 구현할 수 없습니다. System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory 클래스를 사용하여 Windows 런타임으로 내보내기 위한 비동기 작업을 생성하세요. |

> **참고**Windows 런타임을 참조 하는 오류 메시지는 이전 용어를 사용 합니다. 지금은 이를 UWP(유니버설 Windows 플랫폼)라고 합니다. 예를 들어 Windows 런타임 형식은 이제 UWP 형식이라고 합니다.

 

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>mscorlib.dll 또는 System.Runtime.dll에 대해 누락된 참조


이 문제는 명령줄에서 Winmdexp.exe를 사용하는 경우에만 발생합니다. /reference 옵션을 사용하여 "%ProgramFiles(x86)%\\Reference Assemblies\\Microsoft\\Framework\\.NETCore\\v4.5"(32비트 컴퓨터에서는 "%ProgramFiles%\\...")에 있는 .NET Framework 핵심 참조 어셈블리에서 mscorlib.dll과 System.Runtime.dll에 대한 참조를 모두 포함시키는 것이 좋습니다.

| 오류 번호 | 메시지 내용                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | mscorlib.dll을 참조하지 않았습니다. 내보내기 작업이 제대로 이루어지려면 이 메타데이터 파일에 대한 참조가 필요합니다.                               |
| WME1090      | 핵심 참조 어셈블리를 확인할 수 없습니다. mscorlib.dll과 System.Runtime.dll이 /reference 스위치를 사용하여 참조되는지 확인하세요. |

 

## <a name="operator-overloading-is-not-allowed"></a>연산자 오버로드가 허용되지 않음


관리 코드로 작성된 Windows 런타임 구성 요소에서는 공용 형식에 오버로드된 연산자를 노출할 수 없습니다.

> **참고**오류 메시지에서 연산자는 op\_Addition, op\_Multiply, op\_ExclusiveOr, op\_Implicit (암시적 변환) 등과 같은 메타 데이터 이름으로 식별 됩니다.

 

| 오류 번호 | 메시지 내용                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | '{0}' 연산자 오버 로드입니다. Windows 런타임에서는 관리되는 형식이 연산자 오버로드를 노출할 수 없습니다. |

 

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>클래스의 생성자가 동일한 수의 매개 변수 사용


UWP에서 클래스 생성자의 매개 변수 수는 모두 달라야 합니다. 예를 들어 **String** 형식의 단일 매개 변수를 사용하는 생성자와 **int**(Visual Basic에서는 **Integer**) 형식의 단일 매개 변수를 사용하는 생성자를 동시에 가질 수 없습니다. 이 문제를 해결하는 방법은 생성자마다 다른 개수의 매개 변수를 사용하는 방법뿐입니다.

| 오류 번호 | 메시지 내용                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | 형식 '{0}'생성자가 여러 개 사용 하 여'{1}' 인수입니다. Windows 런타임 형식에는 동일한 수의 인수를 사용하는 생성자가 여러 개 있을 수 없습니다. |

 

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>동일한 수의 매개 변수를 사용하는 오버로드에 대해 기본값을 지정해야 함


UWP에서 오버로드된 메서드는 하나의 오버로드가 기본 오버로드로 지정된 경우에만 동일한 수의 매개 변수를 사용할 수 있습니다. [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에서 "오버로드 메서드"를 참조하세요.

| 오류 번호 | 메시지 내용                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | 여러 {0}-의 매개 변수 오버 로드 '{1}. {2}' Windows.Foundation.Metadata.DefaultOverloadAttribute로 데코레이팅 합니다.                                                            |
| WME1085      | {0}-의 매개 변수 오버 로드 {1}. {2} Windows.Foundation.Metadata.DefaultOverloadAttribute로 데코레이팅하여 기본 오버 로드로 지정 된 메서드가 정확히 하나만 있어야 합니다. |

 

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>출력 파일의 네임스페이스 오류 및 잘못된 이름


유니버설 Windows 플랫폼에서는 Windows 메타데이터(.winmd) 파일의 모든 공용 형식이 .winmd 파일 이름을 공유하는 네임스페이스에 있거나 해당 파일 이름의 하위 네임스페이스에 있어야 합니다. 예를 들어 Visual Studio 프로젝트의 이름이 A.B(즉, Windows 런타임 구성 요소가 A.B.winmd)인 경우 공용 클래스 A.B.Class1 및 A.B.C.Class2는 포함될 수 있지만 A.Class3(WME0006) 또는 D.Class4(WME1044)는 포함될 수 없습니다.

> **참고**이러한 제한 하지 구현에서 사용 하는 전용 형식에만 공용 형식에 적용 합니다.

 

A.Class3의 경우 Class3을 다른 네임스페이스로 이동하거나 Windows 런타임 구성 요소의 이름을 A.winmd로 변경합니다. WME0006은 경고이지만 오류로 처리해야 합니다. 이전 예제에서 A.B.winmd를 호출하는 코드는 A.Class3을 찾을 수 없습니다.

D.Class4의 경우 D.Class4와 A.B 네임스페이스의 클래스를 모두 포함할 수 있는 파일 이름은 없으므로 Windows 런타임 구성 요소의 이름을 변경하는 것이 불가능합니다. D.Class4를 다른 네임스페이스로 이동하거나 다른 Windows 런타임 구성 요소에 넣을 수 있습니다.

파일 시스템은 대/소문자를 구분할 수 없으므로 대/소문자만 다른 네임스페이스는 허용되지 않습니다(WME1067).

구성 요소에 **public sealed** 형식(Visual Basic에서는 **Public NotInheritable**)이 하나 이상 포함되어야 합니다. 그렇지 않으면 구성 요소가 전용 형식을 포함하는지 여부에 따라 WME1042 또는 WME1043가 발생합니다.

Windows 런타임 구성 요소의 형식은 네임스페이스와 동일한 이름을 사용할 수 없습니다(WME1068).

> **주의**Winmdexp.exe는 구성 요소의 모든 네임 스페이스를 포함 하는 이름을 생성 하려고 Winmdexp.exe를 직접 호출 하 고 Windows 런타임 구성 요소에 대 한 이름을 지정 하려면 /out 옵션을 사용 하지 마세요. 네임스페이스의 이름을 바꾸면 구성 요소의 이름도 변경될 수 있습니다.

 

| 오류 번호 | 메시지 내용                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | '{0}'이 어셈블리에 대 한 유효한 winmd 파일 이름이 아닙니다. Windows 메타데이터 파일 내의 모든 형식은 파일 이름이 암시하는 네임스페이스의 하위 네임스페이스에 있어야 합니다. 이러한 하위 네임스페이스에 존재하지 않는 형식은 런타임에 찾을 수 없습니다. 이 어셈블리에서 파일 이름으로 사용 될 수 있는 가장 작은 공통 네임 스페이스는 '{1}'. |
| WME1042      | 입력 모듈에는 네임스페이스 내에 있는 공용 형식이 하나 이상 포함되어야 합니다.                                                                                                                                                                                                                                                                   |
| WME1043      | 입력 모듈에는 네임스페이스 내에 있는 공용 형식이 하나 이상 포함되어야 합니다. 네임스페이스 내에서 발견된 유일한 형식은 전용 형식입니다.                                                                                                                                                                                                               |
| WME1044      | 공용 형식에 네임 스페이스 ('{1}') 다른 네임 스페이스를 사용 하 여 일반적인 접두사를 공유 하는 ('{0}'). Windows 메타데이터 파일 내의 모든 형식은 파일 이름이 암시하는 네임스페이스의 하위 네임스페이스에 있어야 합니다.                                                                                                                              |
| WME1067      | Namespace 이름 대/소문자만 다른 수 없습니다: '{0}','{1}'.                                                                                                                                                                                                                                                                                                |
| WME1068      | 형식 '{0}'네임 스페이스와 같은 이름을 가진 없습니다'{1}'.                                                                                                                                                                                                                                                                                                 |

 

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>유효한 유니버설 Windows 플랫폼 형식이 아닌 형식 내보내기


구성 요소의 공용 인터페이스는 UWP 형식만 노출해야 합니다. 그러나 .NET Framework는 일반적으로 사용되는 다양한 형식에 대한 매핑을 제공하며 이들은 .NET Framework와 UWP에서 거의 유사합니다. 따라서 .NET Framework 개발자는 새 형식을 학습할 필요 없이 친숙한 형식을 사용하여 작업할 수 있습니다. 구성 요소의 공용 인터페이스에서 이러한 매핑된 .NET Framework 형식을 사용할 수 있습니다. [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)의 "Windows 런타임 구성 요소에서 형식 선언" 및 "관리 코드에 유니버설 Windows 플랫폼 형식 전달"과 [Windows 런타임 형식의 .NET Framework 매핑](net-framework-mappings-of-windows-runtime-types.md)을 참조하세요.

이러한 매핑의 상당수는 인터페이스입니다. 예를 들어 [IList&lt;T&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx)는 UWP 인터페이스 [IVector&lt;T&gt;](https://msdn.microsoft.com/library/windows/apps/br206631.aspx)에 매핑됩니다. 매개 변수 형식으로 IList&lt;string&gt; 대신 List&lt;string&gt;(Visual Basic에서는 `List(Of String)`)을 사용하는 경우 Winmdexp.exe는 List&lt;T&gt;에서 구현되는 매핑된 모든 인터페이스가 포함된 대체 목록을 제공합니다. List&lt;Dictionary&lt;int, string&gt;&gt;(Visual Basic에서는 List(Of Dictionary(Of Integer, String))과 같이 중첩된 제네릭 형식을 사용하는 경우 Winmdexp.exe는 중첩 수준마다 선택 항목을 제공합니다. 이러한 목록은 상당히 길어질 수 있습니다.

일반적으로 최선의 선택은 해당 형식에 가장 가까운 인터페이스입니다. 예를 들어 Dictionary&lt;int, string&gt;의 경우 최선의 선택은 IDictionary&lt;int, string&gt;일 가능성이 높습니다.

> **중요 한**JavaScript는 관리 형식이 구현한 인터페이스 목록에서 처음 나타나는 인터페이스를 사용 합니다. 예를 들어 Dictionary&lt;int, string&gt;을 JavaScript 코드로 반환하는 경우 반환 형식으로 지정한 인터페이스에 관계없이 IDictionary&lt;int, string&gt;으로 나타납니다. 즉, 첫 번째 인터페이스가 나머지 인터페이스에 나타나는 멤버를 포함하고 있지 않은 경우 해당 멤버는 JavaScript에 표시되지 않습니다.

> **주의**JavaScript에서 구성 요소 사용 될 경우 제네릭이 아닌 [IList](https://msdn.microsoft.com/library/system.collections.ilist.aspx) 및 [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) 인터페이스를 사용 하지 마세요. 이러한 인터페이스는 [IBindableVector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindablevector.aspx) 및 [IBindableIterator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindableiterator.aspx)에 각각 매핑됩니다. 이들은 XAML 컨트롤에 대한 바인딩을 지원하므로 JavaScript에 표시되지 않습니다. JavaScript에서는 "'X' 함수에 잘못된 서명이 있으며 호출할 수 없습니다."라는 런타임 오류가 발생합니다.

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">오류 번호</th>
<th align="left">메시지 내용</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">메서드 '{0}'매개 변수가'{1}'형식의'{2}'. '{2}'은 올바른 Windows 런타임 매개 변수 형식이 아닙니다.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">메서드 '{0}'형식의 매개 변수가'{1}' 서명에 합니다. 이 형식은 올바른 Windows 런타임 형식이 아니지만 올바른 Windows 런타임 형식인 인터페이스를 구현합니다. 메서드 서명을 다음 형식 중 하나를 대신 사용 하도록 변경 하는 것이 좋습니다.: '{2}'.</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>메서드 '{0}'형식의 매개 변수가'{1}' 서명에 합니다. 이 제네릭 형식은 올바른 Windows 런타임 형식이 아니지만 해당 형식이나 형식의 제네릭 매개 변수가 올바른 Windows 런타임 형식인 인터페이스를 구현합니다. {2}</p>
> **참고**에 대 한 {2}, Winmdexp.exe와 같이 대체 목록을 추가 "변경 하세요 ' System.Collections.Generic.List&lt;T&gt;' 대신 메서드 서명을 다음 중 하나로 형식을: ' System.Collections.Generic.IList&lt;T&gt;, System.Collections.Generic.IReadOnlyList&lt;T&gt;, System.Collections.Generic.IEnumerable&lt;T&gt;'. "
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">메서드 '{0}'형식의 매개 변수가'{1}' 서명에 합니다. 관리되는 작업 형식을 사용하는 대신 Windows.Foundation.IAsyncAction, Windows.Foundation.IAsyncOperation 또는 다른 Windows 런타임 비동기 인터페이스 중 하나를 사용하세요. 이러한 인터페이스에도 표준 .NET await 패턴이 적용됩니다. 관리되는 작업 개체를 Windows 런타임 비동기 인터페이스로 변환하는 방법에 대한 자세한 내용은 System.Runtime.InteropServices.WindowsRuntime.AsyncInfo를 참조하세요.</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>허용되지 않는 형식의 필드가 포함된 구조


UWP에서 구조체는 필드만 포함할 수 있으며 필드는 구조체에만 포함될 수 있습니다. 이러한 필드는 공용이어야 합니다. 유효한 필드 형식에는 열거형, 구조체, 기본 형식 등이 있습니다.

| 오류 번호 | 메시지 내용                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | 구조 '{0}'필드에 '{1}'형식의'{2}'. '{2}'은 올바른 Windows 런타임 필드 형식이 아닙니다. Windows 런타임 구조체의 각 필드는 UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String 또는 Enum이거나 필드 자체가 구조체여야 합니다. |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>멤버 서명의 배열에 대한 제한


UWP에서 멤버 서명의 배열은 하한이 0인 1차원 배열이어야 합니다. `myArray[][]`(Visual Basic에서는 `myArray()()`)와 같은 중첩된 배열은 허용되지 않습니다.

> **참고**이 제한 사항은 구현에서 내부적으로 사용 하는 배열에 적용 되지 않습니다.

 

| 오류 번호 | 메시지 내용                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | 메서드 '{0}'형식의 배열이 '{1}' 0이 아닌 인 서명에 합니다. Windows 런타임 메서드 서명의 배열은 하한이 0이어야 합니다. |
| WME1035      | 메서드 '{0}'형식의 다차원 배열이 '{1}' 서명에 합니다. Windows 런타임 메서드 서명의 배열은 1차원이어야 합니다.                  |
| WME1036      | 메서드 '{0}'형식의 중첩된 배열이 '{1}' 서명에 합니다. Windows 런타임 메서드 서명의 배열은 중첩될 수 없습니다.                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>배열 매개 변수는 배열 내용의 읽기 가능 또는 쓰기 가능 여부를 지정해야 함


UWP에서 매개 변수는 읽기 전용이거나 쓰기 전용이어야 합니다. 매개 변수는 **ref**(Visual Basic에서는 [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx) 특성이 없는 **ByRef**)로 표시할 수 없습니다. 이는 배열의 내용에 적용되므로 배열 매개 변수는 배열 내용이 읽기 전용인지 쓰기 전용인지 여부를 나타내야 합니다. 방향은 **out** 매개 변수(Visual Basic에서는 OutAttribute 특성을 가진 **ByRef** 매개 변수)의 경우 분명하지만 값(Visual Basic에서는 ByVal)으로 전달되는 배열 매개 변수는 표시해야 합니다. [Windows 런타임 구성 요소에 배열 전달](passing-arrays-to-a-windows-runtime-component.md)을 참조하세요.

| 오류 번호 | 메시지 내용         |
|--------------|----------------------|
| WME1101      | 메서드 '{0}'매개 변수가'{1}' 배열, 이며가 모두 {2} 및 {3}. Windows 런타임에서 내용 배열 매개 변수는 읽기 가능이거나 쓰기 가능이어야 합니다. 두 특성 중 하나를 제거 하세요 '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | 메서드 '{0}'출력 매개 변수는 '{1}'가 이지만 배열에는 {2}합니다. Windows 런타임에서 출력 배열 내용이 쓰기 가능합니다. 특성을 제거 하세요 '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | 메서드 '{0}'매개 변수가'{1}' 배열, 이며 System.Runtime.InteropServices.InAttribute 또는 System.Runtime.InteropServices.OutAttribute를가지고 있는 합니다. Windows 런타임에서 배열 매개 변수 있어야 {2} 또는 {3}. 필요한 경우 이러한 특성을 제거하거나 적절한 Windows 런타임 특성으로 바꾸세요.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | 메서드 '{0}'매개 변수가'{1}' 배열에 상관 없는 하며 있는 중 하나는 {2} 또는 {3}. 비 배열 매개 변수를 사용 하 여 Windows 런타임 지원 하지 않습니다 {2} 또는 {3}.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | 메서드 '{0}'매개 변수가'{1}' System.Runtime.InteropServices.InAttribute 또는 System.Runtime.InteropServices.OutAttribute를 갖는 합니다. Windows 런타임에서는 매개 변수를 System.Runtime.InteropServices.InAttribute 또는 System.Runtime.InteropServices.OutAttribute로 표시할 수 없습니다. System.Runtime.InteropServices.InAttribute를 제거하고 대신 'out' 한정자를 가진 System.Runtime.InteropServices.OutAttribute로 바꾸세요. 메서드 '{0}'매개 변수가'{1}' System.Runtime.InteropServices.InAttribute 또는 System.Runtime.InteropServices.OutAttribute를 갖는 합니다. Windows 런타임에서는 ByRef 매개 변수를 System.Runtime.InteropServices.OutAttribute로 표시할 수만 있고 다른 특성 사용법은 지원되지 않습니다. |
| WME1106      | 메서드 '{0}'매개 변수가'{1}' 배열입니다. Windows 런타임에서 배열 매개 변수의 내용은 읽기 가능 또는 쓰기 가능이어야 합니다. 적용 하세요 {2} 또는 {3} 에 '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## <a name="member-with-a-parameter-named-value"></a>"value"라는 매개 변수가 있는 멤버


UWP에서 반환 값은 출력 매개 변수로 간주되고, 매개 변수 이름은 고유해야 합니다. 기본적으로 Winmdexp.exe는 반환 값에 "value"라는 이름을 제공합니다. 메서드에 "value"라는 매개 변수가 있으면 WME1092 오류가 발생합니다. 다음과 같은 두 가지 방법으로 이 문제를 해결할 수 있습니다.

-   매개 변수의 이름을 "value" 이외의 이름으로 지정합니다. 속성 접근자의 경우 "returnValue" 이외의 이름으로 지정합니다.
-   ReturnValueNameAttribute 특성을 사용하여 다음과 같이 반환 값의 이름을 변경합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > using System.Runtime.InteropServices;
    > using System.Runtime.InteropServices.WindowsRuntime;
    >
    > [return: ReturnValueName("average")]
    > public int GetAverage(out int lowValue, out int highValue)
    > ```
    > ```vb
    > Imports System.Runtime.InteropServices
    > Imports System.Runtime.InteropServices.WindowsRuntime
    >
    > Public Function GetAverage(<Out> ByRef lowValue As Integer, _
    > <Out> ByRef highValue As Integer) As <ReturnValueName("average")> String
    > ```

> **참고**반환 값의 이름을 변경 했는데 새 이름이 다른 매개 변수를 사용 하 여, 경우에 WME1091 오류가 발생 합니다.

JavaScript 코드는 반환 값을 포함하여 메서드의 출력 매개 변수를 이름으로 액세스할 수 있습니다. 예제를 보려면 [ReturnValueNameAttribute](https://msdn.microsoft.com/library/windows/apps/system.runtime.interopservices.windowsruntime.returnvaluenameattribute.aspx) 특성을 참조하세요.

| 오류 번호 | 메시지 내용 |
|--------------|--------------|
| WME1091 | 메서드를 ' \{0}' 라는 반환 값 ' \{1}' 매개 변수 이름으로 같습니다. Windows 런타임 메서드의 매개 변수와 반환 값에는 고유한 이름이 있어야 합니다. |
| WME1092 | 메서드를 ' \{0}' 라는 매개 변수가 ' \{1}' 값 이름을 반환 하는 경우와 동일 합니다. 매개 변수에 다른 이름을 사용해 보거나 System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute를 사용하여 반환 값의 이름을 명시적으로 지정하세요. |

**참고**기본 이름이 "returnValue" 속성 접근자의 및 다른 모든 방법에 대 한 "value"입니다.


## <a name="related-topics"></a>관련 항목

* [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp.exe(Windows 런타임 메타데이터 내보내기 도구)](https://msdn.microsoft.com/library/hh925576.aspx)
