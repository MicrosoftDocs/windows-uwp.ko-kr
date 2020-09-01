---
title: Windows 런타임 구성 요소 오류 조건 진단
description: 이 문서에서는 관리 코드로 작성 된 Windows 런타임 구성 요소에 대 한 제한 사항에 대 한 추가 정보를 제공 합니다.
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 301a0a650f65d33b3ea3e4f091fbb623b9ebe53d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174267"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>Windows 런타임 구성 요소 오류 조건 진단

이 문서에서는 관리 코드로 작성 된 Windows 런타임 구성 요소에 대 한 제한 사항에 대 한 추가 정보를 제공 합니다. Winmdexp.exe의 오류 메시지에 제공 되는 정보 [ (Windows 런타임 메타 데이터 내보내기 도구)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)를 확장 하 고, [c # 및 Visual Basic를 사용 하 여 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에 제공 되는 제한 사항에 대 한 정보를 보완 합니다.

이 문서는 모든 오류에 대해 설명하지 않습니다. 여기서 설명하는 오류는 일반적인 범주로 묶여 있으며 각 범주에는 관련 오류 메시지 목록이 포함되어 있습니다. 자리 표시자로 사용되는 특정 값을 생략하고 메시지 텍스트로 검색하거나 메시지 번호로 검색하세요. 여기에 필요한 정보가 없으면 문서를 개선할 수 있도록 문서 맨 끝에 있는 피드백 단추를 사용해 주십시오. 피드백을 보낼 때는 오류 메시지를 포함해 주세요. 또는 Microsoft Connect 웹 사이트에서 버그를 제출할 수 있습니다.

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>비동기 인터페이스 구현에 대한 오류 메시지에서 잘못된 형식 제공

관리 되는 Windows 런타임 구성 요소는 비동기 작업 또는 작업을 나타내는 UWP (유니버설 Windows 플랫폼) 인터페이스를 구현할 수 없습니다 ([iasyncaction](/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction), [iasyncactionwithprogress &lt; &gt; ](/previous-versions/br205784(v=vs.85)), [iasyncoperation<tresult> &lt; &gt; TResult](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)또는 [IAsyncOperationWithProgress &lt; TResult, &gt; tprogress](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)). 대신 .NET은 Windows 런타임 구성 요소에서 비동기 작업을 생성 하기 위한 [system.runtime.interopservices.windowsruntime.asyncinfo](/dotnet/api/system.runtime.interopservices.windowsruntime) 클래스를 제공 합니다. 비동기 인터페이스를 구현할 때 Winmdexp.exe 표시 되는 오류 메시지는이 클래스를 이전 이름인 AsyncInfoFactory로 잘못 참조 합니다. .NET에는 더 이상 AsyncInfoFactory 클래스가 포함 되지 않습니다.

| 오류 번호 | 메시지 텍스트|       
|--------------|-------------|
| WME1084      | ' {0} ' 형식은 비동기 인터페이스 ' ' Windows 런타임 {1} 를 구현 합니다. Windows Runtime 형식은 비동기 인터페이스를 구현할 수 없습니다. System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory 클래스를 사용하여 Windows Runtime으로 내보내기 위한 비동기 작업을 생성하세요. |

> **참고**   Windows 런타임를 참조 하는 오류 메시지는 이전 용어를 사용 합니다. 이제이를 UWP (유니버설 Windows 플랫폼) 라고 합니다. 예를 들어 Windows 런타임 형식은 이제 UWP 형식 이라고 합니다.

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>mscorlib.dll 또는 System.Runtime.dll에 대한 참조 누락

이 문제는 명령줄에서 Winmdexp.exe를 사용하는 경우에만 발생합니다. /Reference 옵션을 사용 하 여 mscorlib.dll에 대 한 참조 및 System.Runtime.dll "% ProgramFiles (x86)% \\ 참조 어셈블리 Microsoft Framework에 있는 .NET Framework core 참조 어셈블리의를 포함 하는 것이 좋습니다 \\ \\ \\ . NETCore \\ v 4.5 "("% ProgramFiles% \\ ... ") 32 비트 컴퓨터의 경우).

| 오류 번호 | 메시지 텍스트                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | mscorlib.dll을 참조하지 않았습니다. 올바르게 내보내려면 이 메타데이터 파일에 대한 참조가 필요합니다.                               |
| WME1090      | 핵심 참조 어셈블리를 확인할 수 없습니다. mscorlib.dll과 System.Runtime.dll이 /reference 스위치를 사용하여 참조되는지 확인하세요. |

## <a name="operator-overloading-is-not-allowed"></a>연산자 오버로딩은 허용되지 않음

관리 코드로 작성 된 Windows 런타임 구성 요소에서는 공용 형식에 오버 로드 된 연산자를 노출할 수 없습니다.

> **참고**   오류 메시지에서 연산자는 op \_ 덧셈, op \_ 곱하기, op \_ ExclusiveOr, op \_ 암시적 (암시적 변환) 등의 메타 데이터 이름으로 식별 됩니다.

| 오류 번호 | 메시지 텍스트                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | ' {0} '은 연산자 오버 로드입니다. 관리 되는 형식은 Windows 런타임 연산자 오버 로드를 노출할 수 없습니다. |

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>클래스 생성자의 매개 변수 수가 같음

UWP에서 클래스는 지정 된 수의 매개 변수를 가진 생성자를 하나만 포함할 수 있습니다. 예를 들어 **String** 형식의 단일 매개 변수를 포함 하는 생성자와 **int** 형식의 단일 매개 변수 (Visual Basic의**정수** )가 있는 생성자를 둘 수 없습니다. 이 문제를 해결하는 방법은 생성자마다 다른 개수의 매개 변수를 사용하는 방법 뿐입니다.

| 오류 번호 | 메시지 텍스트                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | ' ' 형식 {0} 에 ' ' 인수를 사용 하는 생성자가 여러 개 있습니다 {1} . Windows 런타임 형식에는 동일한 수의 인수를 사용 하는 생성자가 여러 개 있을 수 없습니다. |

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>매개 변수 수가 같은 오버로드의 경우 기본값을 지정해야 함

UWP에서 오버 로드 된 메서드는 하나의 오버 로드가 기본 오버 로드로 지정 된 경우에만 동일한 수의 매개 변수를 가질 수 있습니다. [C # 및 Visual Basic의 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에서 "오버 로드 된 메서드"를 참조 하세요.

| 오류 번호 | 메시지 텍스트                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | {0}'. '의 여러 매개 변수 오버 로드 {1} {2} 는 DefaultOverloadAttribute로 데코 레이트 됩니다.                                                            |
| WME1085      | {0}의 매개 변수 오버 로드 .의 매개 변수를 {1} {2} DefaultOverloadAttribute로 데코레이팅하 여 기본 오버 로드로 지정 된 메서드를 하나만 지정 해야 합니다. |

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>네임스페이스 오류 및 출력 파일 이름 오류

유니버설 Windows 플랫폼에서 Windows 메타 데이터 (winmd) 파일의 모든 public 형식은 winmd 파일 이름 또는 파일 이름의 하위 네임 스페이스를 공유 하는 네임 스페이스에 있어야 합니다. 예를 들어, Visual Studio 프로젝트의 이름이 A. B 인 경우 (즉, Windows 런타임 구성 요소는. b. winmd) public 클래스 A. B. Class1 및 A. b. A.winmd (WME0006) 또는 것 (WME1044)를 포함할 수 있습니다.

> **참고**  이러한 제한 사항은 공용 형식에만 적용 되며 구현에 사용 되는 전용 형식에는 적용 되지 않습니다.

A.winmd의 경우 A.winmd을 다른 네임 스페이스로 이동 하거나 Windows 런타임 구성 요소의 이름을 winmd로 변경할 수 있습니다. WME0006은 경고 메시지이지만 오류로 간주해야 합니다. 앞의 예제에서 A.B.winmd를 호출하는 코드는 A.Class3을 찾을 수 없습니다.

것의 경우 파일 이름에 것와 A. B 네임 스페이스의 클래스를 모두 포함할 수 없습니다. 따라서 Windows 런타임 구성 요소의 이름을 변경 하는 것은 옵션이 아닙니다. 것를 다른 네임 스페이스로 이동 하거나 다른 Windows 런타임 구성 요소에 배치할 수 있습니다.

파일 시스템은 대/소문자를 구분할 수 없으므로 대/소문자만 다른 네임스페이스는 허용되지 않습니다(WME1067).

구성 요소에는 하나 이상의 **public sealed** 형식이 포함 되어야 합니다 (Visual Basic**공용 NotInheritable** ). 그렇지 않으면 구성 요소에 전용 형식이 포함되어 있는지 여부에 따라 WME1042 또는 WME1043이 발생합니다.

Windows 런타임 구성 요소의 형식은 네임 스페이스 (WME1068)와 동일한 이름을 가질 수 없습니다.

> **주의**  Winmdexp.exe를 직접 호출 하 고/out 옵션을 사용 하 여 Windows 런타임 구성 요소의 이름을 지정 하지 않는 경우 Winmdexp.exe는 구성 요소의 모든 네임 스페이스를 포함 하는 이름을 생성 하려고 시도 합니다. 네임스페이스의 이름을 바꾸면 구성 요소의 이름도 변경될 수 있습니다.

 

| 오류 번호 | 메시지 텍스트                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | ' {0} '은 (는)이 어셈블리에 대 한 올바른 winmd 파일 이름이 아닙니다. Windows 메타데이터 파일 내의 모든 형식은 파일 이름이 암시하는 네임스페이스의 하위 네임스페이스에 있어야 합니다. 이러한 하위 네임스페이스에 없는 형식은 런타임에 찾을 수 없습니다. 이 어셈블리에서 파일 이름으로 사용할 수 있는 가장 작은 공통 네임 스페이스는 ' {1} '입니다. |
| WME1042      | 입력 모듈에는 네임스페이스 내에 있는 공용 형식이 하나 이상 포함되어야 합니다.                                                                                                                                                                                                                                                                   |
| WME1043      | 입력 모듈에는 네임스페이스 내에 있는 공용 형식이 하나 이상 포함되어야 합니다. 네임스페이스 내에서 발견된 유일한 형식은 전용 형식입니다.                                                                                                                                                                                                               |
| WME1044      | 공용 형식에 {1} 다른 네임 스페이스 (' ')와 공통 접두사를 공유 하지 않는 네임 스페이스 (' ')가 있습니다 {0} . Windows 메타데이터 파일 내의 모든 형식은 파일 이름이 암시하는 네임스페이스의 하위 네임스페이스에 있어야 합니다.                                                                                                                              |
| WME1067      | 대/소문자만 다른 네임 스페이스 이름을 사용할 수 없습니다. ' {0} ', ' {1} '.                                                                                                                                                                                                                                                                                                |
| WME1068      | ' ' 형식에 {0} 는 ' ' 네임 스페이스와 같은 이름을 사용할 수 없습니다 {1} .                                                                                                                                                                                                                                                                                                 |

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>유효한 유니버설 Windows 플랫폼 형식이 아닌 형식 내보내기

구성 요소의 공용 인터페이스는 UWP 형식만 노출 해야 합니다. 그러나 .NET은 .NET 및 UWP에서 약간씩 다른 여러 가지 일반적으로 사용 되는 형식에 대 한 매핑을 제공 합니다. 이렇게 하면 .NET 개발자가 새로운 형식을 학습 하는 대신 익숙한 형식으로 작업할 수 있습니다. 이러한 매핑된 .NET 형식을 구성 요소의 공용 인터페이스에서 사용할 수 있습니다. [C # 및 Visual Basic를 사용 하 여 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에서 "Windows 런타임 구성 요소에서 형식 선언" 및 "관리 코드에 유니버설 Windows 플랫폼 형식 전달", [Windows 런타임 형식의 .net 매핑](net-framework-mappings-of-windows-runtime-types.md)을 참조 하세요.

이러한 매핑의 상당수는 인터페이스입니다. 예를 들어 [IList &lt; t &gt; ](/dotnet/api/system.collections.generic.ilist-1) 는 UWP 인터페이스 [IVector &lt; T &gt; ](/uwp/api/Windows.Foundation.Collections.IVector_T_)에 매핑됩니다. &lt; &gt; IList 문자열 대신 목록 문자열 ( `List(Of String)` Visual Basic) &lt; &gt; 을 매개 변수 형식으로 사용 하는 경우 Winmdexp.exe은 목록 T에서 구현 하는 모든 매핑된 인터페이스를 포함 하는 대체 목록을 제공 &lt; &gt; 합니다. 목록 사전 int와 같은 중첩 된 제네릭 형식 (예: Visual Basic)을 사용 하는 경우 &lt; &lt; &gt; &gt; 각 중첩 수준에 대해 선택 항목을 제공 Winmdexp.exe. 이러한 목록은 상당히 길어질 수 있습니다.

대개 최선의 선택은 해당 형식과 가장 가까운 인터페이스입니다. 예를 들어 Dictionary &lt; int, string의 경우 가장 &gt; 좋은 선택은 IDictionary &lt; int, string &gt; 입니다.

> **중요**  JavaScript는 관리 되는 형식이 구현 하는 인터페이스 목록에 첫 번째로 표시 되는 인터페이스를 사용 합니다. 예를 들어, 문자열을 JavaScript 코드로 반환 하는 경우 &lt; 문자열을 &gt; &lt; &gt; 반환 형식으로 지정 하는 인터페이스에 관계 없이 IDictionary int로 표시 됩니다. 즉, 첫 번째 인터페이스가 나머지 인터페이스에 나타나는 멤버를 포함하고 있지 않은 경우 해당 멤버는 JavaScript에 표시되지 않습니다.

> **주의**  JavaScript에서 구성 요소를 사용 하는 경우 제네릭이 아닌 [IList](/dotnet/api/system.collections.ilist) 및 [IEnumerable](/dotnet/api/system.collections.ienumerable) 인터페이스를 사용 하지 마십시오. 이러한 인터페이스는 각각 [IBindableVector](/uwp/api/windows.ui.xaml.interop.ibindablevector) 및 [Ibindableiterator](/uwp/api/windows.ui.xaml.interop.ibindableiterator)에 매핑됩니다. 이들은 XAML 컨트롤에 대한 바인딩을 지원하며 JavaScript에 표시되지 않습니다. JavaScript에서는 "'X' 함수의 시그니처가 잘못되었으므로 호출할 수 없습니다."라는 런타임 오류가 발생합니다.

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">오류 번호</th>
<th align="left">메시지 텍스트</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">' ' 메서드에 ' ' {0} 형식의 ' ' 매개 변수가 {1} {2} 있습니다. ' {2} '은 (는) 올바른 Windows 런타임 매개 변수 형식이 아닙니다.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">' {0} ' 메서드의 시그니처에 ' ' 형식의 매개 변수가 있습니다 {1} . 이 형식은 올바른 Windows Runtime 형식이 아니지만 올바른 Windows Runtime 형식인 인터페이스를 구현합니다. 대신 다음 형식 중 하나를 사용 하도록 메서드 시그니처를 변경 하십시오. ' {2} '.</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>' {0} ' 메서드의 시그니처에 ' ' 형식의 매개 변수가 있습니다 {1} . 이 제네릭 형식은 올바른 Windows Runtime 형식이 아니지만 해당 형식이나 형식의 제네릭 매개 변수가 올바른 Windows Runtime 형식인 인터페이스를 구현합니다. {2}</p>
> **참고**  Winmdexp.exe의 경우 {2} "메서드 시그니처의 ' IReadOnlyList t ' 형식을 다음 형식 중 하나로 변경 하는 것이 좋습니다 (예 &lt; &gt; : ' t, &lt; &gt; &lt; t &gt; , collections. IEnumerable t &lt; &gt; '."). "와 같은 대체 목록을 추가 하는 것이 좋습니다.
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">' {0} ' 메서드의 시그니처에 ' ' 형식의 매개 변수가 있습니다 {1} . 관리 되는 작업 형식을 사용 하는 대신 Iasyncoperation<tresult>를 사용 하거나 다른 Windows 런타임 비동기 인터페이스 중 하나를 사용 합니다. 또한 이러한 인터페이스에는 표준 .NET await 패턴이 적용됩니다. 관리 되는 작업 개체를 Windows 런타임 비동기 인터페이스로 변환 하는 방법에 대 한 자세한 내용은 T e m를 참조 하세요.</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>허용되지 않는 형식의 필드가 포함된 구조체


UWP에서 구조체는 필드만 포함할 수 있으며 구조만 필드를 포함할 수 있습니다. 해당 필드는 공용이어야 합니다. 유효한 필드 형식에는 열거형, 구조체 및 기본 형식 등이 있습니다.

| 오류 번호 | 메시지 텍스트                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | ' ' 구조체 {0} 에 ' ' {1} 형식의 ' ' 필드가 {2} 있습니다. ' {2} '은 (는) 올바른 Windows 런타임 필드 형식이 아닙니다. Windows Runtime 구조체의 각 필드는 UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String 또는 Enum이거나 필드 자체가 구조체여야 합니다. |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>멤버 시그니처에 있는 배열에 대한 제한 사항


UWP에서 멤버 서명의 배열은 하한이 0인 1차원 배열이어야 합니다. (Visual Basic)와 같은 중첩 된 배열 형식은 `myArray[][]` `myArray()()` 허용 되지 않습니다.

> **참고**   이 제한 사항은 구현에서 내부적으로 사용 하는 배열에는 적용 되지 않습니다.

 

| 오류 번호 | 메시지 텍스트                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | ' {0} ' 메서드의 {1} 시그니처에 하 한이 0이 아닌 ' ' 형식의 배열이 있습니다. Windows 런타임 메서드 시그니처의 배열은 하 한이 0 이어야 합니다. |
| WME1035      | ' {0} ' 메서드의 시그니처에 ' ' 형식의 다차원 배열이 있습니다 {1} . Windows 런타임 메서드 시그니처의 배열은 1 차원 이어야 합니다.                  |
| WME1036      | ' {0} ' 메서드의 시그니처에 ' ' 형식의 중첩 배열이 있습니다 {1} . Windows 런타임 메서드 시그니처의 배열은 중첩할 수 없습니다.                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>배열 매개 변수는 배열 내용이 읽기 가능인지 또는 쓰기 가능인지 지정해야 함


UWP에서 매개 변수는 읽기 전용 이거나 쓰기 전용 이어야 합니다. 매개 변수는 **ref** (Visual Basic의 [OutAttribute](/dotnet/api/system.runtime.interopservices.outattribute) 특성이 없는**ByRef** )로 표시할 수 없습니다. 이 제한 사항은 배열의 내용에 적용되므로 배열 매개 변수는 배열 내용이 읽기 전용인지 쓰기 전용인지 표시해야 합니다. **아웃** 매개 변수 (Visual Basic의 OutAttribute 특성이 있는**ByRef** 매개 변수)의 방향은 명확 하지만 값으로 전달 되는 배열 매개 변수 (Visual Basic의 경우)는로 표시 되어야 합니다. [Windows 런타임 구성 요소에 배열 전달을](passing-arrays-to-a-windows-runtime-component.md)참조 하세요.

| 오류 번호 | 메시지 텍스트         |
|--------------|----------------------|
| WME1101      | ' {0} ' 메서드에 {1} 및가 모두 포함 된 배열인 ' ' 매개 변수가 있습니다 {2} {3} . Windows Runtime에서 배열 매개 변수의 내용은 읽기 가능하거나 쓰기 가능해야 합니다. ' '에서 특성 중 하나를 제거 하세요 {1} .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | ' {0} ' 메서드에 배열이 인 출력 매개 변수 ' {1} '이 (가) 있습니다 {2} . Windows Runtime에서는 출력 배열의 내용이 쓰기 가능해야 합니다. ' '에서 특성을 제거 하세요 {1} .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | ' ' 메서드에 ' {0} ' 매개 변수가 있으며,이는 {1} T e m 또는 InAttribute 또는 t e m가 포함 된 배열입니다. Windows 런타임 배열 매개 변수에는 또는가 있어야 합니다 {2} {3} . 필요한 경우 이러한 특성을 제거하거나 적절한 Windows Runtime 특성으로 바꾸세요.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | ' {0} ' 메서드에 {1} 배열이 아니고 또는이 있는 ' ' 매개 변수가 있습니다 {2} {3} . Windows 런타임에서는 배열이 아닌 매개 변수를 또는로 표시할 수 없습니다 {2} {3} .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | ' ' 메서드의 ' {0} ' 매개 변수 {1} 는 t e m InAttribute 또는 T e m. OutAttribute입니다. Windows Runtime에서는 System.Runtime.InteropServices.InAttribute 또는 System.Runtime.InteropServices.OutAttribute로 매개 변수를 표시할 수 없습니다. 대신 System.Runtime.InteropServices.InAttribute를 제거하고 System.Runtime.InteropServices.OutAttribute는 'out' 한정자로 바꾸세요. ' ' 메서드의 ' {0} ' 매개 변수 {1} 는 t e m InAttribute 또는 T e m. OutAttribute입니다. Windows Runtime에서는 ByRef 매개 변수를 System.Runtime.InteropServices.OutAttribute로 표시할 수만 있고 다른 특성 사용법은 지원되지 않습니다. |
| WME1106      | ' {0} ' 메서드에 배열인 ' ' 매개 변수가 있습니다 {1} . Windows 런타임에서 배열 매개 변수의 내용은 읽기 가능 또는 쓰기 가능이어야 합니다. 또는 중 하나 {2} {3} 를 ' '에 적용 하세요 {1} .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## <a name="member-with-a-parameter-named-value"></a>매개 변수 이름이 "value"인 멤버


UWP에서 반환 값은 출력 매개 변수로 간주 되 고 매개 변수 이름은 고유 해야 합니다. 기본적으로 Winmdexp.exe는 반환 값의 이름을 "value"로 지정합니다. 메서드의 매개 변수 이름이 "value"인 경우 WME1092 오류가 발생합니다. 다음과 같은 두 가지 방법으로 이 문제를 해결할 수 있습니다.

-   매개 변수의 이름을 "value" 이외의 이름으로 지정합니다. 속성 접근자의 경우 "returnValue" 이외의 이름으로 지정합니다.
-   ReturnValueNameAttribute 특성을 사용 하 여 다음과 같이 반환 값의 이름을 변경 합니다.

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

> **참고**  반환 값의 이름을 변경 하 고 새 이름이 다른 매개 변수 이름과 충돌 하는 경우 오류 WME1091를 가져옵니다.

JavaScript 코드는 반환 값을 포함하여 메서드의 출력 매개 변수를 이름으로 액세스할 수 있습니다. 예는 [ReturnValueNameAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.returnvaluenameattribute) 특성을 참조 하세요.

| 오류 번호 | 메시지 텍스트 |
|--------------|--------------|
| WME1091 | ' \{ 0} ' 메서드에 \{ 매개 변수 이름과 동일한 ' 1} ' 이라는 반환 값이 있습니다. Windows Runtime 메서드의 매개 변수와 반환 값에는 고유한 이름이 있어야 합니다. |
| WME1092 | ' \{ 0} ' 메서드에 \{ 기본 반환 값 이름과 동일한 ' 1} ' 매개 변수가 있습니다. 매개 변수에 다른 이름을 사용하는 것을 고려하거나 System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute를 사용하여 반환 값의 이름을 명시적으로 지정하세요. |

**참고**  기본 이름은 속성 접근자의 경우 "returnValue"이 고 다른 모든 메서드의 경우 "value"입니다.

## <a name="related-topics"></a>관련 항목

* [C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp.exe(Windows 런타임 메타데이터 내보내기 도구)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)