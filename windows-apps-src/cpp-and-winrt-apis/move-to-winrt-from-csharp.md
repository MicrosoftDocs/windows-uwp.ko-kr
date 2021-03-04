---
description: 이 항목에서는 [C#](/visualstudio/get-started/csharp) 프로젝트의 소스 코드를 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)의 해당 소스 코드로 이식하는 데 관련된 기술 세부 정보를 포괄적으로 분류합니다.
title: C#에서 C++/WinRT로 이동
ms.date: 07/15/2019
ms.topic: article
keywords: Windows 10, UWP, 표준, C++, cpp, WinRT, 프로젝션, 이식, 마이그레이션, C#
ms.localizationpriority: medium
ms.openlocfilehash: f4dbffbee1ecabf89d316fe0d497162c0b1f6312
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824407"
---
# <a name="move-to-cwinrt-from-c"></a>C#에서 C++/WinRT로 이동

> [!TIP]
> 이전에 이 항목을 읽었으며 특정 작업을 염두에 두고 다시 읽는 경우 이 항목의 [수행하는 작업을 기준으로 콘텐츠 찾기](#find-content-based-on-the-task-youre-performing) 섹션으로 건너뛸 수 있습니다.

이 항목에서는 [C#](/visualstudio/get-started/csharp) 프로젝트의 소스 코드를 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)의 해당 소스 코드로 이식하는 데 관련된 기술 세부 정보를 포괄적으로 분류합니다.

UWP(유니버설 Windows 플랫폼) 앱 샘플 중 하나를 이식하는 방법에 대한 사례 연구는 [C#에서 C++/WinRT로 클립보드 샘플 이식](./clipboard-to-winrt-from-csharp.md) 관련 문서를 참조하세요. 이 연습을 통해 진행하면서 샘플을 직접 이식하여 이식 실습과 경험을 쌓을 수 있습니다.

## <a name="how-to-prepare-and-what-to-expect"></a>준비 방법 및 필요한 항목

[C#에서 C++/WinRT로 클립보드 샘플 이식](./clipboard-to-winrt-from-csharp.md) 사례 연구에서는 프로젝트를 C++/WinRT로 이식하는 동안 수행하는 다양한 소프트웨어 설계 결정의 예제를 보여 줍니다. 따라서 기존 코드의 작동 방식을 확실하게 이해하여 이식을 준비하는 것이 좋습니다. 이렇게 하면 앱 기능 및 코드 구조에 대해 개략적으로 확실하게 파악할 수 있습니다. 그리고 나서 결정을 내리면 항상 올바른 방향으로 나아가게 됩니다.

필요한 이식 변경의 종류에 따라 4가지 범주로 그룹화할 수 있습니다.

- [**언어 프로젝션 이식**](#changes-that-involve-the-language-projection). WinRT(Windows 런타임)는 다양한 프로그래밍 언어로 *프로젝션* 됩니다. 이러한 언어 프로젝션 각각은 문제의 프로그래밍 언어에 자연스러운 느낌을 주도록 설계되었습니다. C#의 경우 일부 Windows 런타임 형식이 .NET 형식으로 프로젝션됩니다. 예를 들어 [**System.Collections.Generic.IReadOnlyList\<T\>**](/dotnet/api/system.collections.generic.ireadonlylist-1)를 [**Windows.Foundation.Collections.IVectorView\<T\>**](/uwp/api/windows.foundation.collections.ivectorview-1)로 다시 변환할 수 있습니다. 또한 C#에서 일부 Windows 런타임 작업은 편리한 C# 언어 기능으로 프로젝션됩니다. 예를 들어 C#에서 `+=` 연산자 구문을 사용하여 이벤트 처리 대리자를 등록합니다. 이에 따라 이러한 언어 기능을 수행되는 기본 작업(이 예에서는 이벤트 등록)으로 다시 변환할 수 있습니다.
- [**언어 구문 이식**](#changes-that-involve-the-language-syntax). 이러한 변경 중 대부분은 한 기호를 다른 기호로 바꾸는 간단한 기계적 변환입니다. 예를 들어 점(`.`)을 이중 콜론(`::`)으로 변경합니다.
- [**언어 프로시저 이식**](#changes-that-involve-procedures-within-the-language). 이러한 변경 중 일부는 단순하고 반복적인 변경일 수 있습니다(예: `myObject.MyProperty`에서 `myObject.MyProperty()`로). 다른 변경은 더 심층적으로 변경해야 합니다(예: **System.Text.StringBuilder** 사용과 관련된 프로시저를 **std::wostringstream** 사용과 관련된 프로시저로 이식).
- [**C++/WinRT와 관련된 이식 관련 작업**](#porting-related-tasks-that-are-specific-to-cwinrt). Windows 런타임의 특정 세부 정보는 C# 내부에서 암시적으로 처리됩니다. 이러한 세부 정보는 C++/WinRT에서 명시적으로 수행됩니다. 예를 들어 `.idl` 파일을 사용하여 런타임 클래스를 정의합니다.

이 항목에서 다음에 나오는 작업 기반 인덱스 이후의 나머지 섹션은 위의 분류에 따라 구조화됩니다.

## <a name="find-content-based-on-the-task-youre-performing"></a>수행하는 작업을 기준으로 콘텐츠 찾기

| 작업 | Content |
| - | - |
|Windows 런타임 구성 요소(WRC) 작성|특정 기능(또는 특정 API)은 C++에서만 사용할 수 있습니다. 해당 기능을 C++/WinRT WRC의 한 요소로 포함시킨 다음, C# 앱 등에서 WRC를 사용할 수 있습니다. [C++/WinRT를 사용한 Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) 및 [Windows 런타임 구성 요소에서 런타임 클래스를 작성하는 경우](./author-apis.md#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)를 참조하세요.|
|비동기 메서드 포팅|C++/WinRT 런타임 클래스에서 비동기 메서드의 첫 번째 줄을 `auto lifetime = get_strong();`으로 지정하는 것이 좋습니다([클래스 멤버 코루틴의 *이* 포인터에 안전하게 액세스](./weak-references.md#safely-accessing-the-this-pointer-in-a-class-member-coroutine) 참조).<br><br>`Task`에서 포팅. <a href="#id_async_action">비동기 작업</a>을 참조하세요.<br>`Task<T>`에서 포팅. <a href="#id_async_operation">비동기 연산</a>을 참조하세요.<br>`async void`에서 포팅. <a href="#id_fire_and_forget">Fire-and-forget 메서드</a>를 참조하세요.|
|클래스 포팅|먼저 클래스가 런타임 클래스여야 하는지, 아니면 일반 클래스여도 되는지 결정해야 합니다. 이를 결정하는 데 도움이 되는 [C++/WinRT를 통한 API 작성](./author-apis.md)의 맨 첫 부분을 참조하세요. 그런 다음, 아래의 세 행을 참조하세요.|
|런타임 클래스 포팅|C++ 앱 외부의 기능을 공유하는 클래스 또는 XAML 데이터 바인딩에서 사용되는 클래스를 공유하는 클래스. [Windows 런타임 구성 요소에서 런타임 클래스를 작성하는 경우](./author-apis.md#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component) 또는 [XAML UI에서 참조할 런타임 클래스를 작성하는 경우](./author-apis.md#if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui)를 참조하세요.<br><br>이러한 링크에는 자세한 설명이 나와 있지만 런타임 클래스는 IDL로 선언되어야 합니다. 프로젝트에 이미 IDL 파일이 포함되어 있는 경우(예: `Project.idl`) 해당 파일에 새 런타임 클래스를 선언하는 것이 좋습니다. IDL에서는 앱 외부에서 사용되거나 XAML에서 사용되는 모든 메서드 및 데이터 멤버를 선언합니다. IDL 파일을 업데이트한 후 다시 빌드하고 프로젝트의 `Generated Files` 폴더에 생성된 스텁 파일(`.h` 및 `.cpp`)을 확인합니다(**솔루션 탐색기** 에서는 프로젝트 노드를 선택한 상태에서 **모든 파일 표시** 를 활성화합니다). 스텁 파일을 프로젝트에 이미 있는 파일과 비교하여 파일을 추가하거나 필요한 경우 함수 시그니처를 추가/업데이트합니다. 스텁 파일 구문은 항상 올바르지만 빌드 오류를 최소화하기 위해 사용하는 것이 좋습니다. 프로젝트의 스텁이 스텁 파일의 스텁과 일치하면 C# 코드를 포팅하여 계속해서 구현할 수 있습니다. |
|일반 클래스 포팅|런타임 클래스를 작성하지 [않는 경우](./author-apis.md#if-youre-not-authoring-a-runtime-class)를 참조하세요.|
|작성자 IDL|[Microsoft 인터페이스 정의 언어 3.0 소개](/uwp/midl-3/intro)<br>[XAML UI에서 참조할 런타임 클래스를 작성하는 경우](./author-apis.md#if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui)<br>[XAML 태그에서 개체 사용](./binding-property.md#consuming-objects-from-xaml-markup)<br>[IDL에서 런타임 클래스 정의](#define-your-runtime-classes-in-idl)|
|컬렉션 포팅|[C++/WinRT로 작성된 컬렉션](./collections.md)<br>[XAML 태그에서 데이터 원본을 사용할 수 있도록 만들기](#making-a-data-source-available-to-xaml-markup)<br><a href="#id_associative_container">결합형 컨테이너</a><br><a href="#id_vector_member_access">벡터 멤버 액세스</a>|
|이벤트 포팅|<a href="#id_event_handler_delegate_as_class_member">클래스 멤버인 이벤트 처리기 대리자</a><br><a href="#id_revoke_event_handler_delegate">해지 이벤트 처리기 대리자</a>|
|메서드 포팅|C#에서: `private async void SampleButton_Tapped(object sender, Windows.UI.Xaml.Input.TappedRoutedEventArgs e) { ... }`<br>C++/WinRT `.h` 파일로: `fire_and_forget SampleButton_Tapped(IInspectable const&, RoutedEventArgs const&);`<br>C++/WinRT `.cpp` 파일로: `fire_and_forget OcrFileImage::SampleButton_Tapped(IInspectable const&, RoutedEventArgs const&) {...}`<br>|
|문자열 포팅|[C++/WinRT의 문자열 처리](./strings.md)<br>[ToString](#tostring)<br>[문자열 작성](#string-building)<br>[문자열 boxing 및 unboxing](#boxing-and-unboxing-a-string)|
|형식 변환(형식 캐스팅)|C#: `o.ToString()`<br>C++/WinRT: `to_hstring(static_cast<int>(o))`<br>또한 [ToString](#tostring)도 참조하세요.<br><br>C#: `(Value)o`<br>C++/WinRT: `unbox_value<Value>(o)`<br>unboxing에 실패하는 경우 throw됩니다. 또한 [boxing 및 unboxing](./boxing.md)도 참조하세요.<br><br>C#: `o as Value? ?? fallback`<br>C++/WinRT: `unbox_value_or<Value>(o, fallback)`<br>unboxing에 실패하는 경우 fallback을 반환합니다. 또한 [boxing 및 unboxing](./boxing.md)도 참조하세요.<br><br>C#: `(Class)o`<br>C++/WinRT: `o.as<Class>()`<br>변환이 실패할 경우 throw됩니다.<br><br>C#: `o as Class`<br>C++/WinRT: `o.try_as<Class>()`<br>변환이 실패할 경우 null을 반환합니다.|

## <a name="changes-that-involve-the-language-projection"></a>언어 프로젝션과 관련된 변경 내용

| 범주 | C# | C++/WinRT | 참고 항목 |
| -------- | -- | --------- | -------- |
|형식화되지 않은 개체|`object` 또는 [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[**EnableClipboardContentChangedNotifications** 메서드 이식](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|프로젝션 네임스페이스|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|컬렉션 크기|`collection.Count`|`collection.Size()`|[**BuildClipboardFormatsOutputString** 메서드 이식](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|일반적인 컬렉션 형식|[**IList\<T\>**](/dotnet/api/system.collections.generic.ilist-1) 및 요소를 추가하는 **Add**.|[**IVector\<T\>**](/uwp/api/windows.foundation.collections.ivector-1) 및 요소를 추가하는 **Append**. 모든 곳에서 **std::vector** 를 사용하는 경우 **push_back** 을 사용하여 요소를 추가합니다.||
|읽기 전용 컬렉션 형식|[**IReadOnlyList\<T\>**](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>**](/uwp/api/windows.foundation.collections.ivectorview-1)|[**BuildClipboardFormatsOutputString** 메서드 이식](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|<a name="id_event_handler_delegate_as_class_member"></a>클래스 멤버인 이벤트 처리기 대리자|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[**EnableClipboardContentChangedNotifications** 메서드 이식](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|<a name="id_revoke_event_handler_delegate"></a>해지 이벤트 처리기 대리자|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[**EnableClipboardContentChangedNotifications** 메서드 이식](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|<a name="id_associative_container"></a>결합형 컨테이너|[**IDictionary\<K, V\>**](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>**](/uwp/api/windows.foundation.collections.imap-2)||
|<a name="id_vector_member_access"></a>벡터 멤버 액세스|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>이벤트 처리기 등록/해지

C++/WinRT에는 [C++/WinRT의 대리자를 사용한 이벤트 처리](./handle-events.md)에서 설명한 대로 이벤트 처리기 대리자를 등록/해지할 수 있는 몇 가지 구문 옵션이 있습니다. 또한 [**EnableClipboardContentChangedNotifications** 메서드 이식](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)도 참조하세요.

예를 들어 이벤트 수신자(이벤트를 처리하는 개체)가 소멸되려고 하는 경우 이벤트 원본(이벤트를 발생시키는 개체)이 소멸된 개체로 호출되지 않도록 이벤트 처리기를 해지하려고 할 수 있습니다. [등록된 대리자 철회](./handle-events.md#revoke-a-registered-delegate)를 참조하세요. 이러한 경우 이벤트 처리기에 대한 **event_token** 멤버 변수를 만듭니다. 예제는 [**EnableClipboardContentChangedNotifications** 메서드 이식](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)을 참조하세요.

또한 이벤트 처리기를 XAML 태그에 등록할 수도 있습니다.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

C#에서 **OpenButton_Click** 메서드는 private일 수 있으며, XAML은 여전히 *OpenButton* 에서 발생한 [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트에 연결할 수 있습니다.

C++/WinRT에서 *XAML 태그에 등록하려면* **OpenButton_Click** 메서드가 [구현 형식](./author-apis.md)에서 public이어야 합니다. 이벤트 처리기를 명령형 코드에만 등록하는 경우 이벤트 처리기는 public일 필요가 없습니다.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

또는 등록 XAML 페이지를 구현 형식의 friend로 만들고, **OpenButton_Click** 을 private으로 만들 수 있습니다.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

한 가지 마지막 시나리오는 *바인드* 를 태그에서 이벤트 처리기로 이식할 C# 프로젝트입니다(이 시나리오에 대한 자세한 배경은 [x:Bind의 함수](../data-binding/function-bindings.md) 참조).

```xaml
<Button x:Name="OpenButton" Click="{x:Bind OpenButton_Click}" />
```

해당 태그를 보다 단순한 `Click="OpenButton_Click"`으로 변경하면 됩니다. 또는 원하는 경우 해당 태그를 그대로 유지할 수 있습니다. 이를 지원하려면 IDL에서 이벤트 처리기를 선언하기만 하면 됩니다.

```idl
void OpenButton_Click(Object sender, Windows.UI.Xaml.RoutedEventArgs e);
```

> [!NOTE]
> 이를 [Fire and forget](./concurrency-2.md#fire-and-forget)으로 *구현* 하더라도 함수를 `void`로 선언합니다.

## <a name="changes-that-involve-the-language-syntax"></a>언어 구문과 관련된 변경 내용

| 범주 | C# | C++/WinRT | 참고 항목 |
| -------- | -- | --------- | -------- |
|액세스 한정자|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[**Button_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#button_click)|
|데이터 멤버 액세스|`this.variable`|`this->variable`||
|<a name="id_async_action"></a>비동기 작업|`async Task ...`|`IAsyncAction ...`| [**IAsyncAction** 인터페이스](/uwp/api/windows.foundation.iasyncaction), [C++/WinRT로 동시성 및 비동기 작업](./concurrency.md) |
|<a name="id_async_operation"></a>비동기 연산|`async Task<T> ...`|`IAsyncOperation<T> ...`| [**IAsyncOperation** 인터페이스](/uwp/api/windows.foundation.iasyncoperation), [C++/WinRT로 동시성 및 비동기 작업](./concurrency.md) |
|<a name="id_fire_and_forget"></a>fire-and-forget 메서드(비동기 함축)|`async void ...`|`winrt::fire_and_forget ...`|[**CopyButton_Click** 메서드 포팅](./clipboard-to-winrt-from-csharp.md#copybutton_click), [Fire and forget](./concurrency-2.md#fire-and-forget)|
|열거형 상수 액세스|`E.Value`|`E::Value`|[**DisplayChangedFormats** 메서드 이식](./clipboard-to-winrt-from-csharp.md#displaychangedformats)|
|협조적 대기|`await ...`|`co_await ...`|[**CopyButton_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|프로젝션된 형식 컬렉션(프라이빗 필드)|`private List<MyRuntimeClass> myRuntimeClasses = new List<MyRuntimeClass>();`|`std::vector`<br>`<MyNamespace::MyRuntimeClass>`<br>`m_myRuntimeClasses;`||
|GUID 생성|`private static readonly Guid myGuid = new Guid("C380465D-2271-428C-9B83-ECEA3B4A85C1");`|`winrt::guid myGuid{ 0xC380465D, 0x2271, 0x428C, { 0x9B, 0x83, 0xEC, 0xEA, 0x3B, 0x4A, 0x85, 0xC1} };`||
|네임스페이스 구분 기호|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[**UpdateStatus** 메서드 이식](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|형식 개체 가져오기|`typeof(MyType)`|`winrt::xaml_typename<MyType>()`|[**Scenarios** 속성 이식](./clipboard-to-winrt-from-csharp.md#scenarios)|
|메서드에 대한 매개 변수 선언|`MyType`|`MyType const&`|[매개 변수 전달](./concurrency.md#parameter-passing)|
|비동기 메서드에 대한 매개 변수 선언|`MyType`|`MyType`|[매개 변수 전달](./concurrency.md#parameter-passing)|
|정적 메서드 호출|`T.Method()`|`T::Method()`||
|문자열|`string` 또는 **System.String**|[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring)|[C++/WinRT의 문자열 처리](./strings.md)|
|문자열 리터럴|`"a string literal"`|`L"a string literal"`|[생성자, **Current** 및 **FEATURE_NAME** 이식](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|유추(또는 추론) 형식|`var`|`auto`|[**BuildClipboardFormatsOutputString** 메서드 이식](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|using 지시문|`using A.B.C;`|`using namespace A::B::C;`|[생성자, **Current** 및 **FEATURE_NAME** 이식](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|축자/원시 문자열 리터럴|`@"verbatim string literal"`|`LR"(raw string literal)"`|[**DisplayToast** 메서드 이식](./clipboard-to-winrt-from-csharp.md#displaytoast)|

> [!NOTE]
> 헤더 파일에 지정된 네임스페이스에 대한 `using namespace` 지시문이 없으면 해당 네임스페이스에 대한 모든 형식 이름을 정규화하거나 적어도 컴파일러에서 찾을 수 있을 만큼 충분히 한정해야 합니다. 예제는 [**DisplayToast** 메서드 이식](./clipboard-to-winrt-from-csharp.md#displaytoast)을 참조하세요.

### <a name="porting-classes-and-members"></a>클래스 및 멤버 이식

각 C# 형식에 대해 Windows 런타임 형식 또는 일반 C++ 클래스/구조체/열거형으로 이식할지 여부를 결정해야 합니다. 이러한 결정을 내리는 방법에 대한 자세한 내용과 예제는 [C#에서 C++/WinRT로 클립보드 샘플 이식](./clipboard-to-winrt-from-csharp.md)을 참조하세요.

C# 속성은 일반적으로 접근자 함수, 변경자(mutator) 함수 및 지원 데이터 멤버가 됩니다. 자세한 내용과 예제는 [**IsClipboardContentChangedEnabled** 속성 이식](./clipboard-to-winrt-from-csharp.md#isclipboardcontentchangedenabled)을 참조하세요.

비정적 필드의 경우 [구현 형식](./author-apis.md)의 데이터 멤버로 만듭니다.

C# 정적 필드는 C++/WinRT 정적 접근자 및/또는 변경자 함수가 됩니다. 자세한 내용과 예제는 [생성자, **Current** 및 **FEATURE_NAME** 이식](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)을 참조하세요.

멤버 함수의 경우 각 함수에 대해 IDL에 속하는지 여부 또는 구현 형식에 대한 public 또는 private 멤버 함수인지 여부를 결정해야 합니다. 자세한 내용 및 결정하는 방법에 대한 예제는 [**MainPage** 형식에 대한 IDL](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type)을 참조하세요.

### <a name="porting-xaml-markup-and-asset-files"></a>XAML 태그 및 자산 파일 이식

[C#에서 C++/WinRT로 클립보드 샘플 이식](./clipboard-to-winrt-from-csharp.md)에서는 C# 및 C++/WinRT 프로젝트에서 *동일한* XAML 태그(리소스 포함) 및 자산 파일을 사용할 수 있었습니다. 이를 위해 태그를 편집해야 하는 경우도 있습니다. [**MainPage** 이식을 완료하는 데 필요한 XAML 및 스타일 복사](./clipboard-to-winrt-from-csharp.md#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage)를 참조하세요.

## <a name="changes-that-involve-procedures-within-the-language"></a>언어 내의 프로시저와 관련된 변경 내용

| 범주 | C# | C++/WinRT | 참고 항목 |
| -------- | -- | --------- | -------- |
|비동기 메서드의 수명 관리|해당 없음|`auto lifetime{ get_strong() };` 또는<br>`auto lifetime = get_strong();`|[**CopyButton_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|삭제|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[**CopyImage** 메서드 이식](./clipboard-to-winrt-from-csharp.md#copyimage)|
|개체 생성|`new MyType(args)`|`MyType{ args }` 또는<br>`MyType(args)`|[**Scenarios** 속성 이식](./clipboard-to-winrt-from-csharp.md#scenarios)|
|초기화되지 않은 참조 만들기|`MyType myObject;`|`MyType myObject{ nullptr };` 또는<br>`MyType myObject = nullptr;`|[생성자, **Current** 및 **FEATURE_NAME** 이식](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|인수를 사용하여 개체를 변수로 생성|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` 또는 <br>`auto myObject{ MyType(args) };` 또는 <br>`auto myObject = MyType{ args };` 또는 <br>`auto myObject = MyType(args);` 또는 <br>`MyType myObject{ args };` 또는 <br>`MyType myObject(args);`|[**Footer_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#footer_click)|
|인수를 사용하지 않고 개체를 변수로 생성|`var myObject = new T();`|`MyType myObject;`|[**BuildClipboardFormatsOutputString** 메서드 이식](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|개체 초기화 줄임|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|대량 벡터 작업|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[**CopyButton_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|컬렉션 반복|`foreach (var v in c)`|`for (auto&& v : c)`|[**BuildClipboardFormatsOutputString** 메서드 이식](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|예외 catch|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[**PasteButton_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|예외 세부 정보|`ex.Message`|`ex.message()`|[**PasteButton_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|속성 값 가져오기|`myObject.MyProperty`|`myObject.MyProperty()`|[**NotifyUser** 메서드 이식](./clipboard-to-winrt-from-csharp.md#notifyuser)|
|속성 값 설정|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|속성 값 증가|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>문자열의 경우 작성기로 전환||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|언어 문자열을 Windows 런타임 문자열로 변환|해당 없음|`winrt::hstring{ s }`||
|문자열 작성|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[문자열 작성](#string-building)|
|문자열 보간|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) 및/또는 [**winrt::hstring::operator+**](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[**OnNavigatedTo** 메서드 이식](./clipboard-to-winrt-from-csharp.md#onnavigatedto)|
|비교할 빈 문자열|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[**UpdateStatus** 메서드 이식](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|빈 문자열 만들기|`var myEmptyString = String.Empty;`|`winrt::hstring myEmptyString{ L"" };`||
|사전 작업|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|형식 변환(실패 시 throw)|`(MyType)v`|`v.as<MyType>()`|[**Footer_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#footer_click)|
|형식 변환(실패 시 null)|`v as MyType`|`v.try_as<MyType>()`|[**PasteButton_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|X:Name이 있는 XAML 요소는 속성임|`MyNamedElement`|`MyNamedElement()`|[생성자, **Current** 및 **FEATURE_NAME** 이식](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|UI 스레드로 전환|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync** 또는 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[**NotifyUser** 메서드 이식](./clipboard-to-winrt-from-csharp.md#notifyuser) 및 [**HistoryAndRoaming** 메서드 이식](./clipboard-to-winrt-from-csharp.md#historyandroaming)|
|XAML 페이지의 명령형 코드에서 UI 요소 생성|[UI 요소 생성](#ui-element-construction) 참조|[UI 요소 생성](#ui-element-construction) 참조||

다음 섹션에서는 표의 일부 항목에 대해 자세히 설명합니다.

### <a name="ui-element-construction"></a>UI 요소 생성

다음 코드 예제는 XAML 페이지의 명령형 코드에서 UI 요소를 생성하는 방법을 보여줍니다.

```csharp
var myTextBlock = new TextBlock()
{
    Text = "Text",
    Style = (Windows.UI.Xaml.Style)this.Resources["MyTextBlockStyle"]
};
```

```cppwinrt
TextBlock myTextBlock;
myTextBlock.Text(L"Text");
myTextBlock.Style(
    winrt::unbox_value<Windows::UI::Xaml::Style>(
        Resources().Lookup(
            winrt::box_value(L"MyTextBlockStyle")
        )
    )
);
```

### <a name="tostring"></a>ToString()

C# 형식은 [Object.ToString](/dotnet/api/system.object.tostring) 메서드를 제공합니다.

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/WinRT는 이 기능을 직접 제공하지 않지만 대체 기능으로 전환할 수 있습니다.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

또한 C++/WinRT는 제한된 수의 형식에 대해 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)도 지원합니다. 문자열화하려는 추가 형식에 대한 오버로드를 추가해야 합니다.

| Language | 정수 문자열화 | 열거형 문자열화 |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

열거형을 문자열화하는 경우 **winrt::to_hstring** 의 구현을 제공해야 합니다.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

이러한 문자열화는 데이터 바인딩에서 암시적으로 사용되는 경우가 많습니다.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

이러한 바인딩은 bound 속성의 **winrt::to_hstring** 을 수행합니다. 두 번째 예제(**StatusEnum**)의 경우 **winrt::to_hstring** 에 대한 사용자 고유의 오버로드를 제공해야 합니다. 그렇지 않으면 컴파일러 오류가 발생합니다.

[**Footer_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#footer_click)도 참조하세요.

### <a name="string-building"></a>문자열 작성

문자열 작성의 경우 C#에는 기본 제공 [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 형식이 있습니다.

| 범주 | C# | C++/WinRT |
| -------- | -- | --------- |
| 문자열 작성 | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| Windows 런타임 문자열 추가, null 유지 | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| 줄 바꿈 추가 |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| 결과 액세스 | `s = builder.ToString();` | `ws = builder.str();` |

[**BuildClipboardFormatsOutputString** 메서드 이식](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring) 및 [**DisplayChangedFormats** 메서드 이식](./clipboard-to-winrt-from-csharp.md#displaychangedformats)도 참조하세요.

### <a name="running-code-on-the-main-ui-thread"></a>기본 UI 스레드에서 코드 실행 

이 예제는 [바코드 스캐너 샘플](/samples/microsoft/windows-universal-samples/barcodescanner/)에서 가져온 것입니다.

C# 프로젝트의 기본 UI 스레드에서 작업하려는 경우 일반적으로 다음과 같이 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 메서드를 사용합니다.

```csharp
private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Do work on the main UI thread here.
    });
}
```

C++/WinRT에서 표현하는 것이 훨씬 간단합니다. 첫 번째 일시 중단 지점(여기서는 `co_await`) 후에 액세스하려 한다는 전제 하에 값을 기준으로 매개 변수를 허용합니다. 자세한 내용은 [매개 변수 전달](./concurrency.md#parameter-passing)을 참조하세요.

```cppwinrt
winrt::fire_and_forget Watcher_Added(DeviceWatcher sender, winrt::DeviceInformation args)
{
    co_await Dispatcher();
    // Do work on the main UI thread here.
}
```

기본값이 아닌 우선 순위에 따라 작업을 수행해야 하는 경우에는 우선 순위를 사용하는 오버로드가 있는 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 함수를 참조하세요. **winrt::resume_foreground** 호출을 대기하는 방법을 보여주는 코드 예제는 [스레드 선호도를 고려한 프로그래밍](./concurrency-2.md#programming-with-thread-affinity-in-mind)을 참조하세요.

## <a name="porting-related-tasks-that-are-specific-to-cwinrt"></a>C++/WinRT와 관련된 이식 관련 작업

### <a name="define-your-runtime-classes-in-idl"></a>IDL에서 런타임 클래스 정의

[**MainPage** 형식에 대한 IDL](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type) 및 [`.idl` 파일 통합](./clipboard-to-winrt-from-csharp.md#consolidate-your-idl-files)을 참조하세요.

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>필요한 C++/WinRT Windows 네임스페이스 헤더 파일 포함

C++/WinRT에서 Windows 네임스페이스의 형식을 사용하려면 해당 C++/WinRT Windows 네임스페이스 헤더 파일을 포함해야 합니다. 예제는 [**NotifyUser** 메서드 이식](./clipboard-to-winrt-from-csharp.md#notifyuser)을 참조하세요.

### <a name="boxing-and-unboxing"></a>boxing 및 unboxing

C#은 자동으로 스칼라를 개체에 boxing합니다. C++/WinRT에서는 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 함수를 명시적으로 호출해야 합니다. 두 언어에서 모두 명시적으로 unboxing해야 합니다. [C++/WinRT를 사용하여 boxing 및 unboxing](./boxing.md)을 참조하세요.

다음 표에서는 이러한 정의를 사용합니다.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| 작업 | C# | C++/WinRT|
|-|-|-|
| boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX 및 C#에서는 값 형식에 대한 null 포인터를 unboxing하려고 하면 예외가 발생합니다. C++/WinRT는 이를 프로그래밍 오류로 간주하여 작동이 중단됩니다. C++/WinRT에서 개체가 예상한 형식이 아닌 경우를 처리하려면 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 함수를 사용합니다.

| 시나리오 | C# | C++/WinRT|
|-|-|-|
| 알려진 정수 unboxing |`i = (int)o;` | `i = unbox_value<int>(o);` |
| o가 null인 경우 | `System.NullReferenceException` | 작동 중단 |
| o가 boxing된 정수가 아닌 경우 | `System.InvalidCastException` | 작동 중단 |
| 정수 unboxing, null인 경우 대체 사용, 다른 항목이 있는 경우 작동 중단 | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 가능한 경우 정수 unboxing, 다른 항목이 있는 경우 대체 사용 | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

예제는 [**OnNavigatedTo** 메서드 이식](./clipboard-to-winrt-from-csharp.md#onnavigatedto) 및 [**Footer_Click** 메서드 이식](./clipboard-to-winrt-from-csharp.md#footer_click)을 참조하세요.

#### <a name="boxing-and-unboxing-a-string"></a>문자열 boxing 및 unboxing

문자열은 어떤 방식에서는 값 형식이고, 다른 방식에서는 참조 형식입니다. C# 및 C++/WinRT는 문자열을 다르게 처리합니다.

[**HSTRING**](/windows/win32/winrt/hstring) ABI 형식은 참조 횟수가 계산되는 문자열에 대한 포인터입니다. 그러나 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)에서 파생되지 않으므로 기술적으로는 *개체* 가 아닙니다. 또한 null **HSTRING** 은 빈 문자열을 나타냅니다. **IInspectable** 에서 파생되지 않은 것에 대한 boxing은 [**IReference\<T\>**](/uwp/api/windows.foundation.ireference_t_) 안에 래핑하여 수행되며, Windows 런타임에서 표준 구현을 [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) 개체 형식으로 제공합니다(사용자 지정 형식은 [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)으로 보고됨).

C#은 Windows 런타임 문자열을 참조 형식으로 나타내는 반면, C++/WinRT는 문자열을 값 형식으로 프로젝션합니다. 즉, boxing된 null 문자열은 해당 문자열을 가져온 방식에 따라 다르게 표현될 수 있습니다.

| 동작 | C# | C++/WinRT|
|-|-|-|
| 선언 | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| 문자열 형식 범주 | 참조 형식 | 값 유형 |
| null **HSTRING** 에서 프로젝션하는 형식 | `""` | `hstring{}` |
| null 및 `""`가 동일한가요? | 아니요 | 예 |
| null의 유효성 | `s = null;`<br>`s.Length`에서 NullReferenceException 발생 | `s = hstring{};`<br>`s.size() == 0`(유효) |
| 개체에 null 문자열을 할당하는 경우 | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| 개체에 `""`을(를) 할당하는 경우 | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

기본 boxing 및 unboxing.

| 작업 | C# | C++/WinRT|
|-|-|-|
| 문자열 boxing | `o = s;`<br>빈 문자열은 null이 아닌 개체가 됩니다. | `o = box_value(s);`<br>빈 문자열은 null이 아닌 개체가 됩니다. |
| 알려진 문자열 unboxing | `s = (string)o;`<br>Null 개체는 null 문자열이 됩니다.<br>문자열이 아닌 경우 InvalidCastException이 발생합니다. | `s = unbox_value<hstring>(o);`<br>Null 개체가 충돌합니다.<br>문자열이 아닌 경우 충돌이 발생합니다. |
| 가능한 문자열 Unbox | `s = o as string;`<br>Null 개체 또는 비문자열은 null 문자열이 됩니다.<br><br>또는<br><br>`s = o as string ?? fallback;`<br>Null 또는 비문자열이 대체됩니다.<br>빈 문자열이 유지됩니다. | `s = unbox_value_or<hstring>(o, fallback);`<br>Null 또는 비문자열이 대체됩니다.<br>빈 문자열이 유지됩니다. |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>{Binding} 태그 확장에서 사용할 수 있는 클래스 만들기

{Binding} 태그 확장을 사용하여 데이터를 데이터 형식에 바인딩하려면 [{Binding}을 사용하여 선언된 Binding 개체](../data-binding/data-binding-in-depth.md#binding-object-declared-using-binding)를 참조하세요.

### <a name="consuming-objects-from-xaml-markup"></a>XAML 태그에서 개체 사용

C# 프로젝트에서는 XAML 태그에서 프라이빗 멤버 및 명명된 요소를 사용할 수 있습니다. 그러나 C++/WinRT에서 XAML [ **{x:Bind} 태그 확장**](../xaml-platform/x-bind-markup-extension.md)을 통해 사용되는 모든 엔터티는 IDL에 공개적으로 노출되어야 합니다.

또한 부울에 바인딩하면 C#에서 `true` 또는 `false`가 표시되지만, C++/WinRT에서는 **Windows.Foundation.IReference`1\<Boolean\>** 이 표시됩니다.

자세한 내용과 코드 예제는 [태그에서 개체 사용](./binding-property.md#consuming-objects-from-xaml-markup)을 참조하세요.

### <a name="making-a-data-source-available-to-xaml-markup"></a>XAML 태그에서 데이터 원본을 사용할 수 있도록 만들기

C++/WinRT 버전 2.0.190530.8 이상에서 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)는 **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** 및 **IObservableVector\<IInspectable\>** 를 모두 지원하는 관찰 가능한 벡터를 만듭니다. 예제는 [**Scenarios** 속성 이식](./clipboard-to-winrt-from-csharp.md#scenarios)을 참조하세요.

다음과 같이 **Midl 파일(.idl)** 을 작성할 수 있습니다([런타임 클래스를 Midl 파일(.idl)로 팩터링](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)도 참조).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

그리고 다음과 같이 구현합니다.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

자세한 내용은 [XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩](./binding-collection.md) 및 [C++/WinRT로 작성된 컬렉션](./collections.md)을 참조하세요.

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>XAML 태그에서 데이터 원본을 사용할 수 있도록 만들기(C++/WinRT 2.0.190530.8 이전)

XAML 데이터 바인딩을 수행하려면 항목 원본에서 **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** 뿐만 아니라 다음 인터페이스 조합 중 하나도 구현해야 합니다.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** 및 **INotifyCollectionChanged**
- **IBindableVector** 및 **IBindableObservableVector**
- **IBindableVector** 자체(변경에 응답하지 않음)
- **IVector\<IInspectable\>**
- **IBindableIterable**(요소를 반복하여 프라이빗 컬렉션에 저장)

**IVector\<T\>** 와 같은 제네릭 인터페이스는 런타임에 검색할 수 없습니다. 각 **IVector\<T\>** 에는 **T** 의 함수인 다른 IID(인터페이스 식별자)가 있습니다. 모든 개발자는 임의로 **T** 집합을 확장할 수 있으므로 XAML 바인딩 코드는 쿼리할 전체 집합을 알 수 없습니다. **IEnumerable\<T\>** 를 구현하는 모든 CLR 개체에서 **IEnumerable** 을 자동으로 구현하므로 이 제한은 C#에서 문제가 되지 않습니다. ABI 수준에서는 **IObservableVector\<T\>** 를 구현하는 모든 개체에서 **IObservableVector\<IInspectable\>** 를 자동으로 구현한다는 것을 의미합니다.

C++/WinRT는 이러한 보장을 제공하지 않습니다. C++/WinRT 런타임 클래스에서 **IObservableVector\<T\>** 를 구현하는 경우 **IObservableVector\<IInspectable\>** 의 구현도 어떻게든 제공된다고 가정할 수 없습니다.

따라서 이전 예제를 조사해야 하는 방법은 다음과 같습니다.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

그리고 구현은 다음과 같습니다.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

*m_bookSkus* 의 개체에 액세스해야 하는 경우 해당 개체를 **Bookstore::BookSku** 로 다시 QI해야 합니다.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

### <a name="derived-classes"></a>파생 클래스

런타임 클래스에서 파생하려면 기본 클래스를 *구성* 할 수 있어야 합니다. C#에서는 클래스를 구성할 수 있게 하는 특별한 단계를 수행할 필요가 없지만, C++/WinRT는 이를 수행합니다. [unsealed 키워드](/uwp/midl-3/intro#base-classes)를 사용하여 클래스를 기본 클래스로 사용할 수 있도록 지정합니다.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

[구현 형식](./author-apis.md)에 대한 헤더 파일에서 먼저 기본 클래스 헤더 파일을 포함한 후에 파생 클래스에 대해 자동 생성된 헤더를 포함해야 합니다. 그렇지 않으면 "이 형식을 식으로 잘못 사용했습니다"와 같은 오류가 발생합니다.

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="important-apis"></a>중요 API
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C# 자습서](/visualstudio/get-started/csharp)
* [C++/WinRT](./index.md)
* [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md)