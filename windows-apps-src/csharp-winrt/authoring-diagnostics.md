---
description: '작성 한 구성 요소에 대 한 c #/Winrt 진단 오류 메시지'
title: 'C #/Winrt 구성 요소 오류 진단'
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d1e5eda34a8cbce70edc812ba27437090393ca0
ms.sourcegitcommit: 457239a6907198bc2948322a07c484dd8a170b33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230021"
---
# <a name="diagnose-cwinrt-component-errors"></a>C #/Winrt 구성 요소 오류 진단

이 문서에서는 c #/Winrt.로 작성 된 Windows 런타임 구성 요소의 제한 사항에 대 한 추가 정보를 제공 합니다. 작성자가 구성 요소를 빌드할 때 c #/Winrt의 오류 메시지에 제공 되는 정보를 확장 합니다. 기존 UWP .NET 네이티브 관리 되는 구성 요소에 대 한 c # WinRT 구성 요소에 대 한 메타 데이터는 .NET 도구인 [Winmdexp.exe](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)를 사용 하 여 생성 됩니다. 이제는 Windows 런타임 지원이 .NET에서 분리 되었으므로 c #/Winrt는 구성 요소에서 winmd 파일을 생성 하는 도구를 제공 합니다. Windows 런타임는 c # 클래스 라이브러리 보다 코드에 대 한 제약 조건을 더 많이 가지 며 c #/Winrt 진단 스캐너는 winmd 파일을 생성 하기 전에이에 대 한 경고를 생성 합니다.

이 문서에서는 c #/Winrt.에서 빌드에 보고 된 오류에 대해 설명 합니다. 이 문서에서는 Winmdexp.exe 도구를 사용 하는 [기존 UWP .NET 네이티브 관리 되는 구성 요소](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 에 대 한 제한 사항 정보를 업데이트 된 버전으로 사용 합니다.

오류 메시지 텍스트를 검색 하거나 (자리 표시자에 대 한 특정 값 생략) 메시지 번호를 검색 합니다. 여기서 필요한 정보를 찾지 못한 경우이 문서의 끝에 있는 피드백 단추를 사용 하 여 설명서를 개선 하는 데 도움을 얻을 수 있습니다. 사용자 의견에 오류 메시지를 포함 하세요. 또는 [c #/Winrt](https://github.com/microsoft/CsWinRT)리포지토리에서 버그를 제출할 수 있습니다.

이 문서에서는 시나리오 별로 오류 메시지를 구성 합니다.

## <a name="implementing-interfaces-that-arent-valid-windows-runtime-interfaces"></a>유효한 Windows 런타임 인터페이스가 아닌 인터페이스 구현

C #/winrt 구성 요소는 비동기 작업 또는 작업 (**iasyncaction**, **\<TProgress\> iasyncactionwithprogress**, **iasyncoperation<tresult> \<TResult\>** 또는 **IAsyncOperationWithProgress \<TResult,TProgress\>**)을 나타내는 Windows 런타임 인터페이스와 같은 특정 Windows 런타임 인터페이스를 구현할 수 없습니다. 대신 Windows 런타임 구성 요소에서 비동기 작업을 생성 하는 데 **system.runtime.interopservices.windowsruntime.asyncinfo** 클래스를 사용 합니다. 참고: 이러한 인터페이스는 올바르지 않습니다. 예를 들어 클래스는 System.object를 구현할 수 없습니다 **.**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1008</p></td>
<td><p>{0} {1} 인터페이스가 올바른 Windows 런타임 인터페이스가 아니므로 Windows 런타임 구성 요소 형식에서 인터페이스를 구현할 수 없습니다.</p></td>
</tr>
</tbody>
</table>

## <a name="attribute-related-errors"></a>특성 관련 오류

Windows 런타임에서 오버 로드 된 메서드는 하나의 오버 로드가 기본 오버 로드로 지정 된 경우에만 동일한 수의 매개 변수를 가질 수 있습니다. **DefaultOverload** (CsWinRT1015, 1016) 특성을 사용 합니다.

배열이 함수나 속성 중 하나에서 입력 또는 출력으로 사용 되는 경우에는 읽기 전용 이거나 쓰기 전용 (CsWinRT 1025) 이어야 합니다. T e m 및 **WindowsRuntime** 특성을 사용할 수 있도록 제공 합니다. WindowsRuntime. WriteOnlyArray **..** 제공 된 특성은 배열 형식 (CsWinRT1026)의 매개 변수에만 사용할 수 있으며, 매개 변수 마다 하나만 적용 해야 합니다 (CsWinRT1023).

쓰기 전용으로 간주 되므로 **out** 으로 표시 된 배열 매개 변수에는 특성을 적용할 필요가 없습니다. 이 경우에는 읽기 전용으로 데코레이팅 (CsWinRT1024) 하는 경우 오류 메시지가 표시 됩니다.

**T e m** 및 **OutAttribute** 특성은 모든 형식의 매개 변수 (CsWinRT1021, 1022)에서 사용 하면 안 됩니다.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1015</p></td>
<td><p>클래스에서 {2} : {0} ' '의 여러 매개 변수 오버 로드가 {1} DefaultOverloadAttribute로 데코 레이트 됩니다. 특성은 메서드의 오버 로드 하나에만 적용할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1016</p></td>
<td><p>클래스에서 {2} : {0} 의 매개 변수 오버 로드에는 {1} 기본 오버 로드로 지정 된 메서드가 정확히 한 개만 있어야 합니다. DefaultOverloadAttribute를 특성으로 데코레이팅 합니다.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1021</p></td>
<td><p>' ' 메서드에 ' {0} ' 매개 변수가 있으며,이는 {1} T e m 또는 InAttribute 또는 t e m가 포함 된 배열입니다. Windows 런타임 배열 매개 변수에는 ReadOnlyArray 또는 WriteOnlyArray가 있어야 합니다. 필요한 경우 이러한 특성을 제거하거나 적절한 Windows Runtime 특성으로 바꾸세요.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1022</p></td>
<td><p>메서드 ' {0} '의 매개 변수 ' '에 InAttribute Windows 런타임 또는 t e m가 있습니다. t e m 또는 InAttribute를 사용 하 여 매개 변수를 표시 하는 {1} 것은 지원 되지 않습니다. 대신 System.Runtime.InteropServices.InAttribute를 제거하고 System.Runtime.InteropServices.OutAttribute는 'out' 한정자로 바꾸세요.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1023</p></td>
<td><p>' {0} ' 메서드에 {1} ReadOnlyArray 및 WriteOnlyArray가 모두 포함 된 배열인 ' ' 매개 변수가 있습니다. Windows Runtime에서 배열 매개 변수의 내용은 읽기 가능하거나 쓰기 가능해야 합니다. ' '에서 특성 중 하나를 제거 하세요 {1} .</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1024</p></td>
<td><p>' {0} ' 메서드에 ReadOnlyArray 특성이 있는 배열인 출력 매개 변수 ' '이 (가) 있습니다 {1} . Windows Runtime에서는 출력 배열의 내용이 쓰기 가능해야 합니다.  ' '에서 특성을 제거 하세요 {1} .</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1025</p></td>
<td><p>' {0} ' 메서드에 배열인 ' ' 매개 변수가 있습니다 {1} . Windows 런타임에서 배열 매개 변수의 내용은 읽기 가능 또는 쓰기 가능이어야 합니다. ReadOnlyArray 또는 WriteOnlyArray를 ' '에 적용 하세요 {1} .</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1026</p></td>
<td><p>' {0} ' 메서드에 {1} 배열이 아니고 ReadOnlyArray 특성 또는 WriteOnlyArray 특성이 있는 ' ' 매개 변수가 있습니다. Windows 런타임에서는 배열이 아닌 매개 변수를 ReadOnlyArray 또는 WriteOnlyArray로 표시할 수 없습니다. "</p></td>
</tr>
</tbody>
</table>

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>네임스페이스 오류 및 출력 파일 이름 오류

Windows 런타임에서 Windows 메타 데이터 (winmd) 파일의 모든 public 형식은 winmd 파일 이름 또는 파일 이름의 하위 네임 스페이스를 공유 하는 네임 스페이스에 있어야 합니다. 예를 들어, Visual Studio 프로젝트의 이름이 A. B 인 경우 (즉, Windows 런타임 구성 요소는 .Blaa) public 클래스 A. b. A.winmd 및 A. B. B. C. b. C. n A m a를 포함할 수 있지만 것 또는 D.

> [!NOTE]
> 이러한 제한 사항은 공용 형식에만 적용되며 구현에 사용되는 전용 형식에는 적용되지 않습니다.

A.winmd의 경우 A.winmd을 다른 네임 스페이스로 이동 하거나 Windows 런타임 구성 요소의 이름을 winmd로 변경할 수 있습니다. 앞의 예제에서 A.B.winmd를 호출하는 코드는 A.Class3을 찾을 수 없습니다.

것의 경우 파일 이름에 것와 A. B 네임 스페이스의 클래스를 모두 포함할 수 없습니다. 따라서 Windows 런타임 구성 요소의 이름을 변경 하는 것은 옵션이 아닙니다. 것를 다른 네임 스페이스로 이동 하거나 다른 Windows 런타임 구성 요소에 배치할 수 있습니다.

파일 시스템은 대/소문자를 구분할 수 없으므로 대/소문자만 다른 네임 스페이스는 허용 되지 않습니다 (CsWinRT1002).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1001</p></td>
<td><p>공용 형식에 {1} 다른 네임 스페이스 (' ')와 공통 접두사를 공유 하지 않는 네임 스페이스 (' ')가 있습니다 {0} . Windows 메타데이터 파일 내의 모든 형식은 파일 이름이 암시하는 네임스페이스의 하위 네임스페이스에 있어야 합니다.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1002</p></td>
<td><p>이름이 ' ' 인 네임 스페이스를 여러 개 찾았습니다. {0} 네임 스페이스 이름은 Windows 런타임 대/소문자만 다를 수 없습니다.</p></td>
</tr>
</tbody>
</table>

## <a name="exporting-types-that-arent-valid-windows-runtime-types"></a>유효한 Windows Runtime 형식이 아닌 형식 내보내기

구성 요소의 공용 인터페이스는 Windows 런타임 형식만 노출 해야 합니다. 그러나 .NET에서는 .NET 및 Windows 런타임에서 약간 다른 여러 가지 일반적으로 사용 되는 형식에 대 한 매핑을 제공 합니다. 이렇게 하면 .NET 개발자가 새로운 형식을 학습 하는 대신 익숙한 형식으로 작업할 수 있습니다. 이렇게 매핑된 .NET Framework 형식을 구성 요소의 공용 인터페이스에서 사용할 수 있습니다. 자세한 내용은 [Windows 런타임 구성 요소에서 형식 선언](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#declaring-types-in-windows-runtime-components), [관리 코드에 Windows 런타임 형식 전달](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#passing-windows-runtime-types-to-managed-code)및 [Windows 런타임 형식의 .net 매핑](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)을 참조 하세요.

이러한 매핑의 상당수는 인터페이스입니다. 예를 들어 IList \<T\> 는 Windows 런타임 인터페이스 IVector에 매핑됩니다 \<T\> . IList 대신 List를 \<string\> \<string\> 매개 변수 형식으로 사용 하는 경우 c #/Winrt는 목록으로 구현 된 모든 매핑된 인터페이스를 포함 하는 대체 목록을 제공 합니다 \<T\> . List와 같은 중첩 된 제네릭 형식을 사용 하는 경우 \<Dictionary\<int, string\> \> c #/winrt는 각 중첩 수준에 대해 선택 항목을 제공 합니다. 이러한 목록은 상당히 길어질 수 있습니다.

대개 최선의 선택은 해당 형식과 가장 가까운 인터페이스입니다. 예를 들어 사전의 경우 가장 \<int, string\> 좋은 선택은 IDictionary \<int, string\> 입니다.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1006</p></td>
<td><p>' ' 멤버의 {0} 시그니처에 ' ' 형식이 있습니다 {1} . ' ' 형식은 {1} 올바른 Windows 런타임 형식이 아닙니다. 그러나 형식 (또는 제네릭 매개 변수)은 유효한 Windows 런타임 형식인 인터페이스를 구현 합니다. 멤버 시그니처의 ' 형식 ' {1} 을 system.object의 다음 형식 중 하나로 변경 하십시오. 제네릭: {2} .</p></td>
</tr>
</tbody>
</table>

Windows 런타임에서 멤버 시그니처의 배열은 하 한이 0 인 1 차원 이어야 합니다. `myArray[][]`(CsWinRT1017) 및 (CsWinRT1018)와 같은 중첩 된 배열 형식은 `myArray[,]` 허용 되지 않습니다.

> [!NOTE]
> 이 제한 사항은 구현에서 내부적으로 사용하는 배열에는 적용되지 않습니다.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1017</p></td>
<td><p>메서드에 {0} 시그니처에 형식의 중첩 배열이 있습니다 {1} . Windows 런타임 메서드 시그니처의 배열은 중첩할 수 없습니다.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1018</p></td>
<td><p>' {0} ' 메서드의 시그니처에 ' ' 형식의 다차원 배열이 있습니다 {1} . Windows 런타임 메서드 시그니처의 배열은 1 차원 이어야 합니다.</p></td>
</tr>
</tbody>
</table>

## <a name="structures-that-contain-fields-of-disallowed-types"></a>허용되지 않는 형식의 필드가 포함된 구조체

Windows 런타임에서 구조는 필드만 포함할 수 있으며 구조만 필드를 포함할 수 있습니다. 해당 필드는 공용이어야 합니다. 유효한 필드 형식에는 열거형, 구조체 및 기본 형식 등이 있습니다.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1007</p></td>
<td><p>구조체 {0} 에 public 필드가 없습니다. Windows 런타임 구조체에는 하나 이상의 public 필드가 포함 되어야 합니다.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1011</p></td>
<td><p>구조체 {0} 에 public이 아닌 필드가 있습니다. Windows 런타임 구조체의 모든 필드는 public 이어야 합니다.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1012</p></td>
<td><p>구조체 {0} 에 const 필드가 있습니다. 상수는 Windows 런타임 열거형에만 나타날 수 있습니다.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1013</p></td>
<td><p>구조 {0} 에 형식의 필드가 있습니다 {1} . {1} 은 (는) 올바른 Windows 런타임 필드 형식이 아닙니다. Windows Runtime 구조체의 각 필드는 UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String 또는 Enum이거나 필드 자체가 구조체여야 합니다.</p></td>
</tr>
</tbody>
</table>

## <a name="parameter-name-conflicts-with-generated-code"></a>매개 변수 이름이 생성 된 코드와 충돌 합니다.

Windows 런타임에서 반환 값은 출력 매개 변수로 간주 되 고 매개 변수 이름은 고유 해야 합니다. 기본적으로 c #/Winrt는 반환 값에 이름을 제공 합니다 `__retval` . 메서드에 라는 매개 변수가 있는 경우 `__retval` 오류 CsWinRT1010를 가져옵니다. 이를 해결 하려면 매개 변수에 이외의 이름을 지정 `__retvalue` 합니다.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1010</p></td>
<td><p>메서드의 매개 변수 이름은 {1} {0} 생성 된 c #/winrt interop에서 사용 되는 반환 값 매개 변수 이름과 동일 합니다. 다른 매개 변수 이름을 사용 합니다.</p></td>
</tr>
</tbody>
</table>

## <a name="miscellaneous"></a>기타

C #/Winrt로 작성 된 구성 요소에는 다음과 같은 기타 제한 사항이 있습니다.

- 공용 형식에 오버 로드 된 연산자를 노출할 수 없습니다.
- 클래스와 인터페이스는 제네릭일 수 없습니다.
- 클래스는 sealed 여야 합니다.
- 매개 변수를 참조로 전달할 수 없습니다. 예를 들어 **ref** 키워드를 사용 합니다.
- 속성에는 public get 메서드가 있어야 합니다.
- 구성 요소 네임 스페이스에 하나 이상의 public 형식 (클래스 또는 인터페이스)이 있어야 합니다.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1014</p></td>
<td><p>' {0} '은 연산자 오버 로드입니다. 관리 되는 형식은 Windows 런타임 연산자 오버 로드를 노출할 수 없습니다.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1004</p></td>
<td><p>Generic 유형입니다 {0} . Windows 런타임 형식은 제네릭일 수 없습니다.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1005</p></td>
<td><p>CsWinRT에서는 봉인 되지 않은 유형의 내보내기를 지원 하지 않습니다. 형식을 {0} sealed로 표시 하십시오.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1020</p></td>
<td><p>' {0} ' 메서드의 매개 변수 ' '이 (가) {1} 로 표시 되었습니다 `ref` . Windows 런타임에서는 참조 매개 변수를 사용할 수 없습니다.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1000</p></td>
<td><p>' ' 속성에 {0} public getter 메서드가 없습니다. Windows 런타임는 setter 전용 속성을 지원 하지 않습니다.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1003</p></td>
<td><p>Windows 런타임 구성 요소에는 public 형식이 하나 이상 있어야 합니다.</p></td>
</tr>
</tbody>
</table>

Windows 런타임에서 클래스는 지정 된 수의 매개 변수를 가진 생성자를 하나만 포함할 수 있습니다. 예를 들어 **string** 형식의 단일 매개 변수를 포함 하는 생성자와 **int** 형식의 단일 매개 변수가 있는 다른 생성자를 사용할 수 없습니다. 유일한 해결 방법은 생성자 마다 다른 개수의 매개 변수를 사용 하는 것입니다.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>오류 번호</p></th>
<th><p>메시지 텍스트</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1009</p></td>
<td><p>클래스에는 Windows 런타임에 있는 동일한 인자 수의 생성자가 여러 개 있을 수 없습니다. 클래스에는 {0} 다중 {1} 인자 생성자가 있습니다.</p></td>
</tr>
</tbody>
</table>


