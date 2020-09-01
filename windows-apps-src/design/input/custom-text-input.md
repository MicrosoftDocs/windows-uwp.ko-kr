---
Description: Windows 앱은 windows 장치에서 지원 되는 모든 텍스트 서비스에서 텍스트 입력을 받을 수 있도록 하는 핵심 텍스트 Api를 제공 합니다.
title: 사용자 지정 텍스트 입력 개요
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: 키보드, 텍스트, 핵심 텍스트, 사용자 지정 텍스트, 텍스트 서비스 프레임 워크, 입력, 사용자 상호 작용
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8cde343a77c41a9bb833a7484610e1c324916f8c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160147"
---
# <a name="custom-text-input"></a>사용자 지정 텍스트 입력



[**Windows 앱**](/uwp/api/Windows.UI.Text.Core) 은 windows 장치에서 지원 되는 모든 텍스트 서비스에서 텍스트 입력을 받을 수 있도록 하는 핵심 텍스트 api를 제공 합니다. Api는 텍스트 서비스에 대 한 자세한 정보를 앱에 필요 하지 않은에서 [텍스트 서비스 프레임 워크](/windows/desktop/TSF/text-services-framework) api와 비슷합니다. 이를 통해 앱은 키보드, 음성 또는 펜 같은 모든 입력 형식에서 모든 언어와 텍스트를 받을 수 있습니다.

> **중요 한 api**: [**Windows**](/uwp/api/Windows.UI.Text.Core). uia. [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)

## <a name="why-use-core-text-apis"></a>핵심 텍스트 Api를 사용 하는 이유


많은 앱에서 XAML 또는 HTML 텍스트 상자 컨트롤은 텍스트 입력 및 편집에 충분 합니다. 그러나 앱이 워드 프로세싱 앱과 같은 복잡 한 텍스트 시나리오를 처리 하는 경우 사용자 지정 텍스트 편집 컨트롤의 유연성이 필요할 수 있습니다. [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 키보드 api를 사용 하 여 텍스트 편집 컨트롤을 만들 수 있지만 동아시아 언어를 지 원하는 데 필요한 컴퍼지션 기반 텍스트 입력을 수신 하는 방법은 제공 되지 않습니다.

대신 사용자 지정 텍스트 편집 컨트롤을 만들어야 하는 경우에는 [**Windows. u**](/uwp/api/Windows.UI.Text.Core) i >를 사용 합니다. 이러한 Api는 다양 한 언어로 텍스트 입력을 유연 하 게 처리할 수 있도록 설계 되었으며, 앱에 가장 적합 한 텍스트 환경을 제공할 수 있도록 합니다. 핵심 텍스트 Api를 사용 하 여 빌드된 텍스트 입력 및 편집 컨트롤은 모바일 장치에 대 한 텍스트 [서비스 프레임 워크](/windows/desktop/TSF/text-services-framework) ime (입력기) 및 pc의 필기에서 wordflow 키보드 (자동 수정, 예측 및 받아쓰기 제공)에 이르기까지 Windows 장치의 모든 기존 텍스트 입력 방법에서 텍스트 입력을 받을 수 있습니다.

## <a name="architecture"></a>Architecture


다음은 텍스트 입력 시스템을 간단 하 게 표현한 것입니다.

-   "응용 프로그램"은 핵심 텍스트 Api를 사용 하 여 작성 된 사용자 지정 편집 컨트롤을 호스트 하는 Windows 앱을 나타냅니다.
-   Windows [**를**](/uwp/api/Windows.UI.Text.Core) 통해 텍스트 서비스와의 통신을 용이 하 게 합니다. 텍스트 편집 컨트롤과 텍스트 서비스 간의 통신은 주로 통신을 용이 하 게 하는 메서드 및 이벤트를 제공 하는 [**Coretexteditcontext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) 개체를 통해 처리 됩니다.

![핵심 텍스트 아키텍처 다이어그램](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>텍스트 범위 및 선택


편집 컨트롤은 텍스트 입력을 위한 공간을 제공 하며, 사용자는이 공간의 모든 위치에서 텍스트를 편집할 것으로 간주 합니다. 여기서는 핵심 텍스트 Api에서 사용 하는 텍스트 위치 지정 시스템과 범위 및 선택이 시스템에 표시 되는 방식에 대해 설명 합니다.

### <a name="application-caret-position"></a>응용 프로그램 캐럿 위치

핵심 텍스트 Api와 함께 사용 되는 텍스트 범위는 캐럿 위치를 기준으로 표현 됩니다. "응용 프로그램 캐럿 위치 (ACP)"는 여기에 표시 된 것 처럼 캐럿 바로 앞에 있는 텍스트 스트림의 시작 부분에서 문자 수를 나타내는 0부터 시작 하는 숫자입니다.

![예제 텍스트 스트림 다이어그램](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>텍스트 범위 및 선택

텍스트 범위와 선택 항목은 두 필드를 포함 하는 [**CoreTextRange**](/uwp/api/Windows.UI.Text.Core.CoreTextRange) 구조로 표시 됩니다.

| 필드                  | 데이터 형식                                                                 | Description                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Number** \[ JavaScript\] | **System.object** \[ .net\] | **int32** \[ C\] | 범위의 시작 위치는 첫 번째 문자 바로 앞에 있는 ACP입니다. |
| **EndCaretPosition**   | **Number** \[ JavaScript\] | **System.object** \[ .net\] | **int32** \[ C\] | 범위의 끝 위치는 마지막 문자 바로 다음에 있는 ACP입니다.     |

 

예를 들어 앞에 표시 된 텍스트 범위에서 \[ 0, 5 범위는 \] "Hello" 라는 단어를 지정 합니다. **StartCaretPosition** 는 항상 **EndCaretPosition**보다 작거나 같아야 합니다. 범위 \[ 5 \] 가 잘못 되었습니다.

### <a name="insertion-point"></a>삽입 지점

현재 캐럿 위치 (삽입 지점 이라고 함)는 **StartCaretPosition** 을 **EndCaretPosition**로 설정 하 여 나타냅니다.

### <a name="noncontiguous-selection"></a>인접 하지 않은 선택 영역

일부 편집 컨트롤은 비연속 선택을 지원 합니다. 예를 들어 Microsoft Office apps는 여러 개의 선택 항목을 지원 하 고 많은 소스 코드 편집기에서 열 선택을 지원 합니다. 그러나 핵심 텍스트 Api는 연속 선택 항목을 지원 하지 않습니다. 편집 컨트롤은 연속 된 단일 선택 항목을 보고 해야 합니다. 대부분은 연속 되지 않은 선택 항목의 활성 하위 범위입니다.

예를 들어 다음 텍스트 스트림을 살펴보십시오.

![텍스트 스트림 다이어그램 예 ](images/coretext/stream-2.png) : \[ 0, 1, \] \[ 6, 11을 선택할 수 있습니다 \] . 편집 컨트롤은 그 중 하나만 보고 해야 합니다. \[0, 1 \] 또는 \[ 6, 11 중 하나 \] 입니다.

## <a name="working-with-text"></a>텍스트 작업


[**Coretexteditcontext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) 클래스는 [**Textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) 이벤트, [**Textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) 된 이벤트 및 [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) 메서드를 통해 창과 편집 컨트롤 사이에서 텍스트 흐름을 사용 하도록 설정 합니다.

편집 컨트롤은 사용자가 키보드, 음성 또는 Ime와 같은 텍스트 입력 메서드와 상호 작용할 때 생성 되는 [**Textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) 이벤트를 통해 텍스트를 받습니다.

예를 들어 컨트롤에 텍스트를 붙여 넣는 방식으로 편집 컨트롤의 텍스트를 변경 하는 경우 [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged)를 호출 하 여 창에 알려야 합니다.

텍스트 서비스에 새 텍스트가 필요한 경우 [**Textrequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) 이벤트가 발생 합니다. **Textrequested** 된 이벤트 처리기에서 새 텍스트를 제공 해야 합니다.

### <a name="accepting-text-updates"></a>텍스트 업데이트 허용

편집 컨트롤은 사용자가 입력 하려는 텍스트를 나타내므로 일반적으로 텍스트 업데이트 요청을 허용 해야 합니다. [**Textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) 이벤트 처리기에서 다음과 같은 작업이 편집 컨트롤에 필요 합니다.

1.  [**CoreTextTextUpdatingEventArgs**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) 에 지정 된 텍스트를 [**CoreTextTextUpdatingEventArgs**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range)에 지정 된 위치에 삽입 합니다.
2.  선택 영역을 [**CoreTextTextUpdatingEventArgs**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection)에 지정 된 위치에 배치 합니다.
3.  [**CoreTextTextUpdatingEventArgs**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) 를 [**CoreTextTextUpdatingResult**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult)로 설정 하 여 시스템에 업데이트가 성공 했음을 알립니다.

예를 들어 사용자가 "d"를 입력 하기 전의 편집 컨트롤 상태입니다. 삽입 지점은 10 \[ , 10 \] 입니다.

![예제 텍스트 스트림 다이어그램 ](images/coretext/stream-3.png) 사용자가 "d"를 입력 하면 다음 [**CoreTextTextUpdatingEventArgs**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingEventArgs) 데이터와 함께 [**textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) 이벤트가 발생 합니다.

-   [**Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range)  =  범위 \[ 10, 10\]
-   [**Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) = "d"
-   [**Newselection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection)  =  \[ 11, 11\]

편집 컨트롤에서 지정 된 변경 내용을 적용 하 고 [**결과**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) 를 **Succeeded**로 설정 합니다. 다음은 변경 내용이 적용 된 후의 컨트롤 상태입니다.

![예제 텍스트 스트림 다이어그램](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>텍스트 업데이트 거부

요청 된 범위가 편집 컨트롤에서 변경 하지 않아야 하는 영역에 있기 때문에 텍스트 업데이트를 적용할 수 없는 경우도 있습니다. 이 경우 변경 내용을 적용 하면 안 됩니다. 대신 [**CoreTextTextUpdatingEventArgs**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) 를 [**CoreTextTextUpdatingResult**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult)로 설정 하 여 업데이트가 실패 했음을 시스템에 알립니다.

예를 들어 전자 메일 주소만 허용 하는 edit 컨트롤이 있다고 가정해 봅니다. 전자 메일 주소에는 공백이 포함 될 수 없으므로 공백을 거부 해야 하므로 공백 키에 대해 [**Textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) 이벤트가 발생 하는 경우 편집 컨트롤에서 [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) 를 **Failed** 로 설정 하면 됩니다.

### <a name="notifying-text-changes"></a>텍스트 변경 알림

경우에 따라 편집 컨트롤은 텍스트를 붙여넣거나 자동으로 수정 하는 등의 텍스트를 변경 합니다. 이러한 경우에는 [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) 메서드를 호출 하 여 이러한 변경 내용을 텍스트 서비스에 알려야 합니다.

예를 들어 사용자가 "세계"를 붙여넣기 전의 편집 컨트롤 상태입니다. 삽입 지점은 6 \[ , 6 \] 입니다.

![예제 텍스트 스트림 다이어그램 ](images/coretext/stream-5.png) 사용자는 붙여넣기 작업을 수행 하 고 편집 컨트롤은 다음 텍스트와 함께 종료 됩니다.

![예제 텍스트 스트림 다이어그램 ](images/coretext/stream-4.png) 이 경우 다음 인수를 사용 하 여 [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) 를 호출 해야 합니다.

-   *modifiedRange*  =  modifiedRange \[ 6, 6\]
-   *새 길이* = 5
-   *Newselection*  =  \[ 11, 11\]

텍스트 서비스가 작업 중인 텍스트를 업데이트 하기 위해 처리 하는 하나 이상의 [**Textrequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) 된 이벤트가 수행 됩니다.

### <a name="overriding-text-updates"></a>텍스트 업데이트 재정의

편집 컨트롤에서 텍스트 업데이트를 재정의 하 여 자동 수정 기능을 제공 하는 것이 좋습니다.

예를 들어, 축약을 공식화 하는 수정 기능을 제공 하는 edit 컨트롤이 있다고 가정 합니다. 사용자가 수정 사항을 트리거하기 위해 space 키를 입력 하기 전의 편집 컨트롤 상태입니다. 삽입 지점은 3 \[ , 3 \] 입니다.

![텍스트 스트림 다이어그램 예 ](images/coretext/stream-6.png) 사용자가 space 키를 누르고 해당 [**textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) 이벤트가 발생 합니다. 편집 컨트롤은 텍스트 업데이트를 허용 합니다. 수정이 완료 되기 전에 잠시 동안 편집 컨트롤의 상태입니다. 삽입 지점은 4 \[ , 4 \] 입니다.

![텍스트 스트림 다이어그램 ](images/coretext/stream-7.png) [**textupdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) 이벤트 처리기 외부에서 편집 컨트롤을 사용 하면 다음과 같이 수정할 수 있습니다. 수정이 완료 된 후 편집 컨트롤의 상태입니다. 삽입 지점은 \[ 5, 5 \] 입니다.

![예제 텍스트 스트림 다이어그램 ](images/coretext/stream-8.png) 이 경우 다음 인수를 사용 하 여 [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) 를 호출 해야 합니다.

-   *modifiedRange*  =  modifiedRange \[ 1, 2\]
-   *새 길이* = 2
-   *Newselection*  =  \[ 5, 5\]

텍스트 서비스가 작업 중인 텍스트를 업데이트 하기 위해 처리 하는 하나 이상의 [**Textrequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) 된 이벤트가 수행 됩니다.

### <a name="providing-requested-text"></a>요청 된 텍스트 제공

특히 편집 컨트롤에 이미 있던 텍스트, 문서를 로드 하는 경우, 예를 들어 이전 섹션에서 설명한 대로 편집 컨트롤에서 삽입 한 텍스트 등에 대해 텍스트 서비스에 올바른 텍스트를 지정 하는 것이 중요 합니다. 따라서 [**Textrequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) 된 이벤트가 발생할 때마다 편집 컨트롤에 현재 지정 된 범위에 대 한 텍스트를 제공 해야 합니다.

[**Coretexttextrequest**](/uwp/api/Windows.UI.Text.Core.CoreTextTextRequest) 의 [**범위**](/uwp/api/windows.ui.text.core.coretexttextrequest.range) 에서 편집 컨트롤이 있는 그대로 처리할 수 없는 범위를 지정 하는 경우가 있습니다. 예를 들어, **범위** 는 [**textrequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) 된 이벤트 시 편집 컨트롤의 크기 보다 크거나 **범위의** 끝이 범위를 벗어납니다. 이러한 경우에는 의미가 있는 모든 범위를 반환 해야 합니다 .이는 일반적으로 요청 된 범위의 하위 집합입니다.

## <a name="related-articles"></a>관련된 문서

### <a name="samples"></a>샘플

- [사용자 지정 컨트롤 편집 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)

### <a name="archive-samples"></a>보관 샘플

- [XAML 텍스트 편집 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))