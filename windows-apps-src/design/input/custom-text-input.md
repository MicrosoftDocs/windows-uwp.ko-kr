---
Description: The core text APIs in the Windows.UI.Text.Core namespace enable a Universal Windows Platform (UWP) app to receive text input from any text service supported on Windows devices.
title: 사용자 지정 텍스트 입력 개요
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: 키보드, 텍스트, 코어 텍스트, 사용자 지정 텍스트, 텍스트 서비스 프레임워크, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 161278dc5fe0bb8c7d4c790def6a9f7ba88b83d2
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8946579"
---
# <a name="custom-text-input"></a>사용자 지정 텍스트 입력



[**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) 네임스페이스의 코어 텍스트 API를 통해 UWP(유니버설 Windows 플랫폼) 앱은 Windows 장치에서 지원되는 모든 텍스트 서비스에서 텍스트 입력을 받을 수 있습니다. 이 API는 앱이 텍스트 서비스의 자세한 정보를 알 필요가 없다는 점에서 [텍스트 서비스 프레임워크](https://msdn.microsoft.com/library/windows/desktop/ms629032) API와 유사합니다. 이 API를 통해 앱은 모든 언어로 된 텍스트를 키보드, 음성 또는 펜과 같은 모든 입력 유형에서 받을 수 있습니다.

> **중요 API**: [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238), [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)

## <a name="why-use-core-text-apis"></a>코어 텍스트 API를 사용하는 이유


많은 앱의 경우 XAML 또는 HTML 텍스트 상자 컨트롤만으로도 텍스트 입력 및 편집에는 충분합니다. 그러나 앱이 워드 프로세싱 앱과 같은 복잡한 텍스트 시나리오를 처리하는 경우 사용자 지정 텍스트 편집 컨트롤의 유연성이 필요할 수 있습니다. [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 키보드 API를 사용하여 텍스트 편집 컨트롤을 만들 수 있지만 이렇게 하면 동아시아 언어를 지원하는 데 필요한 컴퍼지션 기반 텍스트 입력을 받을 방법이 없습니다.

대신 사용자 지정 텍스트 편집 컨트롤을 만들어야 할 경우 [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) API를 사용하세요. 이러한 API는 모든 언어로 된 텍스트 입력 처리 시 많은 유연성을 제공하도록 디자인되어 앱에 가장 적합한 텍스트 환경을 제공할 수 있도록 합니다. 코어 텍스트 API로 작성된 텍스트 입력 및 편집 컨트롤은 Windows 장치의 모든 기존 텍스트 입력 방법, [텍스트 서비스 프레임워크](https://msdn.microsoft.com/library/windows/desktop/ms629032) 기반 IME(입력기), PC의 필기, 모바일 장치의 WordFlow 키보드(자동 수정, 예측 및 받아쓰기를 제공함)에서 텍스트 입력을 받을 수 있습니다.

## <a name="architecture"></a>아키텍처


다음은 텍스트 입력 시스템을 간단하게 표현한 것입니다.

-   "응용 프로그램"은 코어 텍스트 API를 사용하여 작성된 사용자 지정 편집 컨트롤을 호스트하는 UWP 앱을 나타냅니다.
-   [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) API는 Windows를 통해 텍스트 서비스와 쉽게 통신할 수 있도록 합니다. 텍스트 편집 컨트롤과 텍스트 서비스 간의 통신은 기본적으로 쉽게 통신할 수 있도록 하는 메서드 및 이벤트를 제공하는 [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) 개체를 통해 처리됩니다.

![코어 텍스트 아키텍처 다이어그램](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>텍스트 범위 및 선택


편집 컨트롤은 텍스트 입력을 위한 공간을 제공하고 사용자는 이 공간 내 어디서나 텍스트를 편집할 수 있기를 기대합니다. 여기서는 코어 텍스트 API에서 사용되는 텍스트 위치 지정 시스템과 이 시스템에서 범위 및 선택이 표현되는 방식을 설명합니다.

### <a name="application-caret-position"></a>응용 프로그램 캐럿 위치

코어 텍스트 API에서 사용되는 텍스트 범위는 캐럿 위치로 표현됩니다. "ACP(응용 프로그램 캐럿 위치)"는 다음과 같이 캐럿 바로 앞의 텍스트 스트림 시작 부분에서부터 문자 수를 나타내는 0부터 시작하는 숫자입니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>텍스트 범위 및 선택

텍스트 범위 및 선택은 다음과 같은 두 개의 필드가 포함된 [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201) 구조로 표현됩니다.

| 필드                  | 데이터 형식                                                                 | 설명                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | 범위의 시작 위치는 첫 번째 문자 바로 앞의 ACP입니다. |
| **EndCaretPosition**   | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | 범위의 끝 위치는 마지막 문자 바로 뒤의 ACP입니다.     |

 

예를 들어 앞에서 표시된 텍스트 범위에서 범위 [0, 5]는 단어 "Hello"를 지정합니다. **StartCaretPosition**은 항상 **EndCaretPosition**보다 작거나 같아야 합니다. 범위 [5, 0]은 잘못되었습니다.

### <a name="insertion-point"></a>삽입 지점

흔히 삽입 지점이라고 하는 현재 캐럿 위치는 **StartCaretPosition**이 **EndCaretPosition**과 같도록 설정하여 표현됩니다.

### <a name="noncontiguous-selection"></a>비연속 선택

일부 편집 컨트롤은 비연속 선택을 지원합니다. 예를 들어 Microsoft Office 앱은 다중 임의 선택을 지원하며 많은 소스 코드 편집기에서는 열 선택을 지원합니다. 그러나 코어 텍스트 API는 비연속 선택을 지원하지 않습니다. 편집 컨트롤은 단일 연속 선택만 보고해야 하며, 대개 비연속 선택의 활성 하위 범위를 보고해야 합니다.

다음 텍스트 스트림을 예로 들어보겠습니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-2.png) 두 가지 선택 \[0, 1\]과 \[6, 11\]이 있습니다. 편집 컨트롤은 \[0, 1\]과 \[6, 11\] 중 하나만 보고해야 합니다.

## <a name="working-with-text"></a>텍스트 작업


[**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) 클래스를 사용하면 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 이벤트, [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 이벤트 및 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 메서드를 통한 Windows와 편집 컨트롤 간의 텍스트 흐름이 가능합니다.

편집 컨트롤은 사용자가 키보드, 음성 또는 IME와 같은 텍스트 입력 방법을 조작할 때 생성된 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 이벤트를 통해 텍스트를 받습니다.

편집 컨트롤에서 텍스트를 변경하는 경우(예: 컨트롤에 텍스트를 붙여넣음) [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)를 호출하여 Windows에 알려야 합니다.

텍스트 서비스에 새 텍스트가 필요한 경우 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 이벤트가 발생합니다. **TextRequested** 이벤트 처리기에 새 텍스트를 제공해야 합니다.

### <a name="accepting-text-updates"></a>텍스트 업데이트 수락

일반적으로 편집 컨트롤은 텍스트 업데이트 요청이 사용자가 입력하려는 텍스트를 나타내므로 텍스트 업데이트 요청을 수락해야 합니다. [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 이벤트 처리기에서 편집 컨트롤에 대해 다음과 같은 작업이 예상됩니다.

1.  [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236)에 지정된 텍스트를 [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234)에 지정된 위치에 삽입합니다.
2.  [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233)에 지정된 위치에 선택 항목을 배치합니다.
3.  [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235)를 [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237)로 설정하여 업데이트 성공을 시스템에 알립니다.

예를 들어 다음은 사용자가 "d"를 입력하기 전의 편집 컨트롤 상태입니다. 삽입 지점은 \[10, 10\]에 있습니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-3.png) 사용자가 "d"를 입력하면 다음 [**CoreTextTextUpdatingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958176) 데이터가 포함된 상태로 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958229) 이벤트가 발생합니다.

-   [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [**Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) = "d"
-   [**NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

편집 컨트롤에서 지정된 변경 내용을 적용하고 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235)를 **Succeeded**로 설정합니다. 다음은 변경 내용이 적용된 후 컨트롤의 상태입니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>텍스트 업데이트 거부

요청된 범위가 변경되어서는 안 되는 편집 컨트롤의 범위에 있어 텍스트 업데이트를 적용할 수 없는 경우가 있습니다. 이 경우 변경 내용을 적용하면 안 됩니다. 대신 [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235)를 [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237)로 설정하여 업데이트 실패를 시스템에 알립니다.

메일 주소만 수락하는 편집 컨트롤을 예로 들어보겠습니다. 메일 주소는 공백을 포함할 수 없으므로 공백은 거부되어야 합니다. 따라서 스페이스 키에 대해 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 이벤트가 발생하는 경우 편집 컨트롤에서 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235)를 **Failed**로 설정해야 합니다.

### <a name="notifying-text-changes"></a>텍스트 변경 내용 알림

텍스트를 붙여넣거나 텍스트가 자동 수정되는 등의 경우에 편집 컨트롤은 텍스트를 변경합니다. 이러한 경우 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 메서드를 호출하여 텍스트 서비스에 이러한 변경 내용을 알려야 합니다.

예를 들어 다음은 사용자가 "World"를 붙여넣기 전의 편집 컨트롤 상태입니다. 삽입 지점은 \[6, 6\]에 있습니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-5.png) 사용자가 붙여넣기 작업을 수행하면 편집 컨트롤에는 다음과 같은 텍스트가 남습니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-4.png) 이 경우 다음 인수를 사용하여 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)를 호출해야 합니다.

-   *modifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *newSelection* = \[11, 11\]

하나 이상의 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 이벤트가 뒤이어 발생하며, 텍스트 서비스에서 작업 중인 텍스트를 업데이트하기 위해 이러한 이벤트를 처리합니다.

### <a name="overriding-text-updates"></a>텍스트 업데이트 재정의

편집 컨트롤에서 자동 수정 기능을 제공하기 위해 텍스트 업데이트를 재정의할 수 있습니다.

축약형을 형식화하는 수정 기능을 제공하는 편집 컨트롤을 예로 들어보겠습니다. 다음은 사용자가 스페이스 키를 입력하여 수정을 트리거하기 전의 편집 컨트롤 상태입니다. 삽입 지점은 \[3, 3\]에 있습니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-6.png) 사용자가 스페이스 키를 누르면 해당 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 이벤트가 발생합니다. 편집 컨트롤에서 텍스트 업데이트를 수락합니다. 다음은 수정이 완료되기 전 잠깐의 편집 컨트롤 상태입니다. 삽입 지점은 \[4, 4\]에 있습니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-7.png) [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 이벤트 처리기 외부에서 편집 컨트롤은 다음과 같이 수정합니다. 다음은 수정이 완료된 후 편집 컨트롤 상태입니다. 삽입 지점은 \[5, 5\]에 있습니다.

![텍스트 스트림 다이어그램의 예](images/coretext/stream-8.png) 이 경우 다음 인수를 사용하여 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)를 호출해야 합니다.

-   *modifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *newSelection* = \[5, 5\]

하나 이상의 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 이벤트가 뒤이어 발생하며, 텍스트 서비스에서 작업 중인 텍스트를 업데이트하기 위해 이러한 이벤트를 처리합니다.

### <a name="providing-requested-text"></a>요청한 텍스트 제공

자동 수정 또는 예측과 같은 기능을 제공하기 위해서는 텍스트 서비스에 올바른 텍스트가 있어야 하며, 특히 편집 컨트롤에 이미 있는 텍스트(예: 문서 로드 시) 또는 이전 섹션에서 설명한 대로 편집 컨트롤에 의해 삽입된 텍스트의 경우 그렇습니다. 따라서 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 이벤트가 발생할 때마다 지정된 범위에 현재 편집 컨트롤에 있는 텍스트를 제공해야 합니다.

[**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958227)의 [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958221)가 편집 컨트롤에서 있는 그대로 수용할 수 없는 범위를 지정하는 경우가 있습니다. 예를 들어 **Range**가 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 이벤트 발생 시 편집 컨트롤의 크기보다 크거나 **Range**의 끝이 범위를 벗어납니다. 이러한 경우 일반적으로 요청한 범위의 하위 집합인 타당한 범위를 반환해야 합니다.

## <a name="related-articles"></a>관련 문서

**샘플**
* [사용자 지정 편집 컨트롤 샘플](https://go.microsoft.com/fwlink/?linkid=831024) 
 **보관 샘플**
* [XAML 텍스트 편집 샘플](http://go.microsoft.com/fwlink/p/?LinkID=251417)


