---
title: Windows 런타임 구성 요소에 배열 전달
description: UWP (Windows 유니버설 플랫폼)에서 매개 변수는 입력 또는 출력에 대 한 매개 변수 이며 둘 다 사용할 수는 없습니다. 즉, 메서드에 전달되는 배열의 콘텐츠와 배열 자체는 입력 또는 출력의 하나에 사용됩니다.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f38538d1a77bdeb6a497eda5802f712f23002b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155187"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Windows 런타임 구성 요소에 배열 전달




UWP (Windows 유니버설 플랫폼)에서 매개 변수는 입력 또는 출력에 대 한 매개 변수 이며 둘 다 사용할 수는 없습니다. 즉, 메서드에 전달되는 배열의 콘텐츠와 배열 자체는 입력 또는 출력의 하나에 사용됩니다. 배열 콘텐츠가 입력에 사용되면 메서드는 배열에서 읽지만 배열에 쓰지 않습니다. 배열 콘텐츠가 출력에 사용되면 메서드는 배열에 쓰지만 배열에서 읽지 않습니다. .NET의 배열은 참조 형식이 기 때문에 배열 매개 변수에 문제가 발생 하며 배열 참조가 값으로 전달 되는 경우에도 배열의 내용이 변경 될 수 있습니다 (Visual Basic의**ByVal** ). [Windows 런타임 Metadata Export Tool (Winmdexp.exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) 에서는 ReadOnlyArrayAttribute 특성 또는 WriteOnlyArrayAttribute 특성을 매개 변수에 적용 하 여 컨텍스트에서 명확 하지 않은 경우 배열의 용도를 지정 해야 합니다. 배열 사용법은 다음과 같이 결정됩니다.

-   반환 값 또는 out 매개 변수의 경우 (Visual Basic의 [OutAttribute](/dotnet/api/system.runtime.interopservices.outattribute) 특성이 있는 **ByRef** 매개 변수) 배열은 항상 출력 전용입니다. ReadOnlyArrayAttribute 특성을 적용 하지 마세요. WriteOnlyArrayAttribute 특성은 output 매개 변수에서 허용 되지만 중복 됩니다.

    > **주의**    Visual Basic 컴파일러는 출력 전용 규칙을 적용 하지 않습니다. 출력 매개 변수를 읽을 수 없습니다. **아무 것도**포함 하지 않을 수 있습니다. 항상 새 배열을 할당하세요.
 
-   **Ref** 한정자 (Visual Basic에서는**ByRef** )를 포함 하는 매개 변수는 허용 되지 않습니다. Winmdexp.exe에서 오류를 생성 합니다.
-   값으로 전달 되는 매개 변수의 경우 [ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute) 특성 또는 [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute) 특성을 적용 하 여 입력 또는 출력에 대 한 배열 콘텐츠 인지 여부를 지정 해야 합니다. 두 특성을 모두 지정하면 오류가 발생합니다.

메서드가 입력용 배열을 허용해야 할 경우 배열 콘텐츠를 수정하고 배열을 호출자로 반환하고, 입력에 읽기 전용 매개 변수를 사용하고 출력에 쓰기 전용 매개 변수(또는 반환 값)를 사용합니다. 다음 코드에서는 이 패턴을 구현하는 한 가지 방법을 보여 줍니다.

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

입력 배열을 바로 복사하여 복사본으로 작업하는 것이 좋습니다. 이렇게 하면 .NET 코드에서 구성 요소를 호출 하는지 여부에 관계 없이 메서드가 동일 하 게 동작 합니다.

## <a name="using-components-from-managed-and-unmanaged-code"></a>관리 코드 및 비관리 코드의 구성 요소 사용


ReadOnlyArrayAttribute 특성이 나 WriteOnlyArrayAttribute 특성이 있는 매개 변수는 호출자가 네이티브 코드 또는 관리 코드로 작성 되었는지 여부에 따라 다르게 동작 합니다. 호출자가 네이티브 코드 (JavaScript 또는 Visual C++ 구성 요소 확장) 인 경우 배열 내용은 다음과 같이 처리 됩니다.

-   ReadOnlyArrayAttribute: 호출이 ABI (응용 프로그램 이진 인터페이스) 경계를 교차할 때 배열이 복사 됩니다. 필요한 경우 요소를 변환 합니다. 따라서 메서드가 입력 전용 배열에 대해 수행 하는 실수로 인 한 변경 내용은 호출자에 게 표시 되지 않습니다.
-   WriteOnlyArrayAttribute: 호출 된 메서드는 원래 배열의 내용에 대 한 가정을 만들 수 없습니다. 예를 들어 메서드가 받는 배열이 초기화 되지 않았거나 기본값을 포함할 수 있습니다. 메서드는 배열에 있는 모든 요소의 값을 설정 합니다.

호출자가 관리 코드 이면 호출 된 메서드에서 원래 배열을 사용할 수 있습니다 .이는 .NET의 모든 메서드 호출에 있기 때문입니다. .NET 코드에서 배열 콘텐츠를 변경할 수 있으므로 메서드가 배열에 대해 수행 하는 모든 변경 내용이 호출자에 게 표시 됩니다. 이는 Windows 런타임 구성 요소에 대해 작성 된 단위 테스트에 영향을 주므로 기억해 야 합니다. 테스트를 관리 코드로 작성 하는 경우 테스트 중에 배열의 내용이 변경 가능 하 게 표시 됩니다.

## <a name="related-topics"></a>관련 항목

* [ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute)
* [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute)
* [C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)