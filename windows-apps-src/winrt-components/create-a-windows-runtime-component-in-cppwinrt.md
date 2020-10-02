---
title: C++/WinRT를 사용한 Windows 런타임 구성 요소
description: 이 항목에서는 [c + +/Winrt](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 를 사용 하 여 &mdash; Windows 런타임 언어를 사용 하 여 빌드된 유니버설 Windows 앱에서 호출할 수 있는 구성 요소를 Windows 런타임 구성 요소를 만들고 사용 하는 방법을 보여 줍니다.
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10, uwp, windows, 런타임, 구성 요소, 구성 요소, Windows 런타임 구성 요소, WRC, c + +/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 12fa7c499dd0203a489b5657f1e3e2634bdad8b1
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646296"
---
# <a name="windows-runtime-components-with-cwinrt"></a>C++/WinRT를 사용한 Windows 런타임 구성 요소

이 항목에서는 [c + +/Winrt](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 를 사용 하 여 &mdash; Windows 런타임 언어를 사용 하 여 빌드된 유니버설 Windows 앱에서 호출할 수 있는 구성 요소를 Windows 런타임 구성 요소를 만들고 사용 하는 방법을 보여 줍니다.

C + +/WinRT.에서 Windows 런타임 구성 요소를 작성 하는 데는 몇 가지 이유가 있습니다.
- 복잡 하거나 계산 집약적인 작업에서 c + +의 성능 이점을 누릴 수 있습니다.
- 이미 작성 되 고 테스트 된 표준 c + + 코드를 다시 사용 합니다.
- 로 작성 된 UWP (유니버설 Windows 플랫폼) 앱에 Win32 기능을 노출 하려면 (예: c #)

일반적으로 c + +/WinRT 구성 요소를 제작할 때 다른 패키지의 코드에서 데이터를 전달 하는 ABI (응용 프로그램 이진 인터페이스) 경계를 제외 하 고 표준 c + + 라이브러리의 형식 및 기본 제공 형식을 사용할 수 있습니다 `.winmd` . ABI에서 Windows 런타임 형식을 사용 합니다. 또한 c + +/WinRT 코드에서 대리자 및 이벤트와 같은 형식을 사용 하 여 구성 요소에서 발생 하 고 다른 언어로 처리 될 수 있는 이벤트를 구현 합니다. C + +/Winrtd에 대 한 자세한 내용은 [c + +/vb](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 를 참조 하세요.

이 항목의 나머지 부분에서는 c + +/WinRT에서 Windows 런타임 구성 요소를 작성 한 다음 응용 프로그램에서 사용 하는 방법을 안내 합니다.

이 항목에서 빌드할 Windows 런타임 구성 요소는 온도계를 나타내는 런타임 클래스를 포함 합니다. 또한이 항목에서는 온도계 런타임 클래스를 사용 하는 핵심 앱을 보여 주고 온도를 조정 하는 함수를 호출 합니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](../cpp-and-winrt-apis/consume-apis.md)과 [C++/WinRT를 통한 API 작성](../cpp-and-winrt-apis/author-apis.md)을 참조하세요.

## <a name="create-a-windows-runtime-component-thermometerwrc"></a>Windows 런타임 구성 요소 만들기 (ThermometerWRC)

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Windows 런타임 구성 요소 (c + +/WinRT)** 프로젝트를 만들고 이름을 *ThermometerWRC* ("온도계 Windows 런타임 구성 요소"의 경우)로 만듭니다. **솔루션 및 프로젝트를 같은 디렉터리에 배치**를 선택하지 않아야 합니다. 일반적으로 사용 가능한 최신(미리 보기 아님) 버전의 Windows SDK를 대상으로 합니다. *ThermometerWRC* 프로젝트 이름을 지정 하면이 항목의 나머지 단계를 가장 쉽게 수행할 수 있습니다. 

아직 프로젝트를 빌드하지 마세요.

새로 만든 프로젝트에는 `Class.idl`이라는 이름의 파일이 포함되어 있습니다. 솔루션 탐색기에서 `Thermometer.idl` 파일의 이름을 바꿉니다(`.idl` 파일의 이름을 바꾸면 자동으로 종속 `.h` 및 `.cpp` 파일의 이름도 바뀜). `Thermometer.idl`의 콘텐츠를 아래 목록으로 바꿉니다.

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
    };
}
```

파일을 저장합니다. 프로젝트는 현재 완료 될 때까지 빌드되지 않지만 이제는 **온도계** 런타임 클래스를 구현 하는 소스 코드 파일을 생성 하기 때문에 빌드를 수행 하는 것이 좋습니다. 따라서 계속해서 지금 빌드합니다(이 단계에서 표시될 것으로 예상되는 빌드 오류는 발견되는 `Class.h` 및 `Class.g.h`로 처리해야 함).

빌드 프로세스에서 `midl.exe` 도구가 실행되어 구성 요소의 Windows 런타임 메타데이터 파일(`\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd`)을 생성합니다. 그런 다음, `cppwinrt.exe` 도구가 실행되어(`-component` 옵션과 함께) 구성 요소를 작성하도록 지원하는 원본 코드 파일을 생성합니다. 이러한 파일에는 IDL에 선언 된 **온도계** 런타임 클래스 구현을 시작 하기 위한 스텁이 포함 되어 있습니다. 이 스텁이 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h`와 `Thermometer.cpp`입니다.

프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **파일 탐색기에서 폴더 열기**를 클릭합니다. 파일 탐색기에서 프로젝트 폴더가 열립니다. 여기에서 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\` 폴더에서 스텁 파일 `Thermometer.h` 및 `Thermometer.cpp`를 복사하여 프로젝트 파일을 포함하는 폴더인 `\ThermometerWRC\ThermometerWRC\`에 붙여넣고 대상에서 파일을 바꿉니다. 이제 `Thermometer.h`와 `Thermometer.cpp`를 열고 런타임 클래스를 구현합니다. 에서 `Thermometer.h` **온도계**의 구현 (팩터리 구현이*아님* )에 새 개인 멤버를 추가 합니다.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

    private:
        float m_temperatureFahrenheit { 0.f };
    };
}
...
```

에서 `Thermometer.cpp` 아래 목록에 표시 된 것 처럼 **AdjustTemperature** 메서드를 구현 합니다.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
    }
}
```

또한 두 파일에서 `static_assert`를 삭제해야 합니다.

경고 메시지로 인해 빌드가 중단되는 경우에는 경고를 해결하거나 프로젝트 속성 **C/C++**  > **일반** > **경고를 오류로 처리**를 **아니요(/WX-)** 로 설정한 후 프로젝트를 다시 빌드합니다.

## <a name="create-a-core-app-thermometercoreapp-to-test-the-windows-runtime-component"></a>핵심 앱 (ThermometerCoreApp)을 만들어 Windows 런타임 구성 요소 테스트

이제 *ThermometerWRC* 솔루션 또는 새 프로젝트에서 새 프로젝트를 만듭니다. **핵심 앱 (c + +/WinRT)** 프로젝트를 만들고 이름을 *ThermometerCoreApp*로 합니다. 두 프로젝트가 동일한 솔루션에 있는 경우 *ThermometerCoreApp* 를 시작 프로젝트로 설정 합니다.

> [!NOTE]
> 앞에서 설명한 것 처럼 Windows 런타임 구성 요소에 대 한 Windows 런타임 메타 데이터 파일 (이름이 *ThermometerWRC*인 프로젝트)이 폴더에 만들어집니다 `\ThermometerWRC\Debug\ThermometerWRC\` . 해당 경로의 첫 번째 세그먼트는 솔루션 파일이 포함된 폴더의 이름입니다. 다음 세그먼트는 `Debug`라는 하위 디렉터리입니다. 마지막 세그먼트는 Windows 런타임 구성 요소에 대해 이름이 지정된 하위 디렉터리입니다. 프로젝트 이름을 *ThermometerWRC*로 지정한 경우 메타 데이터 파일은 폴더에 `\<YourProjectName>\Debug\<YourProjectName>\` 있습니다.

이제 핵심 앱 프로젝트 (*ThermometerCoreApp*)에서 참조를 추가 하 고, `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` 두 프로젝트가 동일한 솔루션에 있는 경우 프로젝트 간 참조를 탐색 하거나 추가 합니다. **추가**, **확인**을 차례로 클릭합니다. 이제 *ThermometerCoreApp*을 빌드합니다. 페이로드 파일이 없다는 오류가 표시 되는 경우 `readme.txt` Windows 런타임 구성 요소 프로젝트에서 해당 파일을 제외 하 고 다시 빌드한 다음 *ThermometerCoreApp*를 다시 빌드합니다.

빌드 프로세스에서 `cppwinrt.exe` 도구가 실행되면서 참조된 `.winmd` 파일을 원본 코드 파일로 처리합니다. 이 원본 코드 파일에는 구성 요소를 사용할 때 지원할 목적으로 프로젝션된 형식이 포함되어 있습니다. 구성 요소의 런타임 클래스에 맞게 프로젝션된 형식의 헤더(`ThermometerWRC.h`)가 `\ThermometerCoreApp\ThermometerCoreApp\Generated Files\winrt\` 폴더로 생성됩니다.

이 헤더를 `App.cpp`에 추가합니다.

```cppwinrt
// App.cpp
...
#include <winrt/ThermometerWRC.h>
...
```

또한에서 `App.cpp` 다음 코드를 추가 하 여, 프로젝션 된 형식의 기본 생성자를 사용 하 여 **온도계** 개체를 인스턴스화하고 온도계 개체에서 메서드를 호출 합니다.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(1.f);
        ...
    }
    ...
};
```

창을 클릭할 때마다 온도계 개체의 온도가 증가 합니다. 코드를 단계별로 실행 하 여 응용 프로그램이 Windows 런타임 구성 요소를 정말로 호출 하 고 있는지 확인 하려는 경우 중단점을 설정할 수 있습니다.

## <a name="next-steps"></a>다음 단계

C + +/WinRT Windows 런타임 구성 요소에 더 많은 기능 또는 새로운 Windows 런타임 형식을 추가 하려면 위에 표시 된 것과 동일한 패턴을 따를 수 있습니다. 먼저 IDL을 사용 하 여 노출 하려는 기능을 정의 합니다. 그런 다음 Visual Studio에서 프로젝트를 빌드하여 스텁 구현을 생성 합니다. 그런 다음 구현을 적절 하 게 완료 합니다. IDL에서 정의 하는 메서드, 속성 및 이벤트는 Windows 런타임 구성 요소를 사용 하는 응용 프로그램에 표시 됩니다. IDL에 대 한 자세한 내용은 [Microsoft 인터페이스 정의 언어 3.0 소개](/uwp/midl-3/intro)를 참조 하세요.

Windows 런타임 구성 요소에 이벤트를 추가 하는 방법에 대 한 예제는 [c + +/WinRT의 Author 이벤트](../cpp-and-winrt-apis/author-events.md)를 참조 하세요.

## <a name="troubleshooting"></a>문제 해결

| 증상 | 해결책 |
|---------|--------|
|C++/WinRT 앱에서 XAML을 사용하는 [C# Windows 런타임 구성 요소](./creating-windows-runtime-components-in-csharp-and-visual-basic.md)를 사용할 때 컴파일러는 " *'MyNamespace_XamlTypeInfo': is not a member of 'winrt::MyNamespace'* " 형식의 오류를 발생합니다. &mdash;여기서 *MyNamespace*는 Windows 런타임 구성 요소의 네임스페이스 이름입니다. | 사용하는 C++/WinRT 앱의 `pch.h`에서 *MyNamespace*를 적절하게 대체하는 `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>`&mdash;를 추가합니다. |
