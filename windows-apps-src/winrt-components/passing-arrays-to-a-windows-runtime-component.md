---
title: Windows 런타임 구성 요소에 배열 전달
description: UWP(유니버설 Windows 플랫폼)에서 매개 변수는 입력 또는 출력용이며 입력과 출력 모두의 매개 변수는 아닙니다. 다시 말해서, 메서드에 전달되는 배열의 콘텐츠 및 배열 자체는 입력 또는 출력용입니다.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49fb5ac5fbba5fad8123eb0167a2e00037725487
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340510"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Windows 런타임 구성 요소에 배열 전달




UWP(유니버설 Windows 플랫폼)에서 매개 변수는 입력 또는 출력용이며 입력과 출력 모두의 매개 변수는 아닙니다. 다시 말해서, 메서드에 전달되는 배열의 콘텐츠 및 배열 자체는 입력 또는 출력용입니다. 배열의 콘텐츠가 입력용인 경우 메서드는 배열에서 읽지만 배열에 쓰지는 않습니다. 배열의 콘텐츠가 출력용인 경우 메서드는 배열에 쓰지만 배열에서 읽지는 않습니다. .NET의 배열은 참조 형식이 기 때문에 배열 매개 변수에 문제가 발생 하며 배열 참조가 값으로 전달 되는 경우에도 배열의 내용이 변경 될 수 있습니다 (Visual Basic의**ByVal** ). [Windows 런타임 메타데이터 내보내기 도구(Winmdexp.exe)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)를 사용하려면 컨텍스트에서 명확하지 않은 경우 의도한 배열의 용도를 지정해야 합니다. ReadOnlyArrayAttribute 특성이나 WriteOnlyArrayAttribute 특성을 매개 변수에 적용하면 됩니다. 배열 사용은 다음과 같이 결정됩니다.

-   반환 값의 경우 또는 out 매개 변수(Visual Basic의 **OutAttribute** 특성이 적용된 [ByRef](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.outattribute) 매개 변수)의 경우 배열은 항상 출력용입니다. ReadOnlyArrayAttribute 특성은 적용되지 않습니다. WriteOnlyArrayAttribute 특성은 출력 매개 변수에 허용되지만 중복됩니다.

    > **주의**  Visual Basic 컴파일러는 출력 전용 규칙을 적용 하지 않습니다. 출력 매개 변수에서 읽어서는 안 됩니다. **Nothing**이 포함되어 있을 수 있습니다. 항상 새 배열을 할당하세요.
 
-   **ref** 한정자(Visual Basic의 **ByRef**)가 있는 매개 변수는 사용할 수 없습니다. Winmdexp.exe에서 오류가 발생합니다.
-   값으로 전달되는 매개 변수의 경우 [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute) 특성이나 [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute) 특성을 적용하여 배열 콘텐츠가 입력용인지 아니면 출력용인지 지정해야 합니다. 두 특성을 모두 지정하는 것은 오류입니다.

메서드가 입력용 배열을 허용하고 배열 콘텐츠를 수정하고 배열을 호출자에 반환해야 하는 경우에는 입력용으로 읽기 전용 매개 변수를 사용하고 출력용으로 쓰기 전용 매개 변수(또는 반환 값)를 사용합니다. 다음 코드는 이 패턴을 구현하는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

즉시 입력 배열의 복사본을 만들어 복사본을 조작하는 것이 좋습니다. 이렇게 하면 .NET 코드에서 구성 요소를 호출 하는지 여부에 관계 없이 메서드가 동일 하 게 동작 합니다.

## <a name="using-components-from-managed-and-unmanaged-code"></a>관리 코드 및 비관리 코드의 구성 요소 사용


ReadOnlyArrayAttribute 특성이나 WriteOnlyArrayAttribute 특성이 있는 매개 변수는 호출자가 네이티브 코드로 작성되었는지 아니면 관리 코드로 작성되었는지에 따라 다르게 동작합니다. 호출자가 네이티브 코드(JavaScript 또는 Visual C++ 구성 요소 확장)이면 배열 콘텐츠가 다음과 같이 처리됩니다.

-   ReadOnlyArrayAttribute: 호출이 ABI(응용 프로그램 이진 인터페이스) 경계를 넘을 때 배열이 복사됩니다. 요소는 필요한 경우에 변환됩니다. 따라서 메서드가 입력 전용 배열을 실수로 변경하더라도 변경 내용이 호출자에 표시되지 않습니다.
-   WriteOnlyArrayAttribute: 호출된 메서드가 원래 배열의 콘텐츠에 대해 가정할 수 없습니다. 예를 들어 메서드가 받는 배열이 초기화되어 있지 않을 수도 있고 기본값을 포함할 수도 있습니다. 메서드는 배열에서 모든 요소의 값을 설정해야 합니다.

호출자가 관리 코드 이면 호출 된 메서드에서 원래 배열을 사용할 수 있습니다 .이는 .NET의 모든 메서드 호출에 있기 때문입니다. .NET 코드에서 배열 콘텐츠를 변경할 수 있으므로 메서드가 배열에 대해 수행 하는 모든 변경 내용이 호출자에 게 표시 됩니다. Windows 런타임 구성 요소에 대해 작성된 단위 테스트에 영향을 주므로, 이 점을 기억해야 합니다. 테스트가 관리 코드로 작성되면 배열 콘텐츠가 테스트 중에 mutable로 표시됩니다.

## <a name="related-topics"></a>관련 항목

* [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute)
* [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute)
* [C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
