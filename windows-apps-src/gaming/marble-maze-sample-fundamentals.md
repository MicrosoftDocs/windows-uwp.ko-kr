---
title: Marble Maze 샘플 기본 사항
description: 이 문서에서는 프로젝트가 Windows 런타임 환경에서 Visual C++를 사용하는 방법, 프로젝트를 만들고 구성하는 방법, 프로젝트 빌드 방법 등 Marble Maze 프로젝트의 기본 특성에 대해 설명합니다.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 샘플, directx, 기본 사항
ms.localizationpriority: medium
ms.openlocfilehash: f04c17609976e8bd8f6c1c6143ed7b992b0bb3c5
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820614"
---
# <a name="marble-maze-sample-fundamentals"></a>Marble Maze 샘플 기본 사항




이 항목에서는 프로젝트가 Windows 런타임 환경에서 Visual C++를 사용하는 방법, 프로젝트를 만들고 구성하는 방법, 프로젝트 빌드 방법 등 Marble Maze 프로젝트의 기본 특성에 대해 설명합니다. 또한 코드에 사용되는 여러 규칙에 대해서도 설명합니다.

> [!NOTE]
> 이 문서에 해당하는 샘플 코드는 [DirectX Marble Maze 게임 샘플](https://go.microsoft.com/fwlink/?LinkId=624011)에 있습니다.

이 문서에서 UWP(유니버설 Windows 플랫폼) 게임을 계획하고 개발하는 경우에 대해 논의하는 주요 사항은 다음과 같습니다.

-   사용 된 **DirectX 11 앱 (유니버설 Windows- C++/CX)** DirectX UWP 게임을 만들려면 Visual Studio에서 템플릿.
-   Windows 런타임은 보다 현대적이고 개체 지향적인 방식으로 UWP 앱을 개발할 수 있도록 클래스와 인터페이스를 제공합니다.
-   Hat (^) 기호를 사용 하 여 개체 참조를 사용 하 여 Windows 런타임 변수의 수명을 관리 하 [Microsoft::WRL::ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) COM 개체의 수명을 관리 하 고 [std::shared\_ptr](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class) 또는 [std::unique\_ptr](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class) 다른 모든 힙 할당의 수명을 관리 하려면 C++ 개체입니다.
-   대부분의 경우 결과 코드 대신 예외 처리를 사용하여 예기치 않은 오류를 처리합니다.
-   앱에서 오류를 찾는 데 도움이 되도록 [SAL 주석](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects)을 코드 분석 도구와 함께 사용합니다.

## <a name="creating-the-visual-studio-project"></a>Visual Studio 프로젝트 만들기


다운로드 하 고 샘플을 열 수 있습니다는 **MarbleMaze_VS2017.sln** 파일 (에 **C++** 폴더) Visual Studio에서 코드를 해야 합니다.

Marble Maze에 대한 Visual Studio 프로젝트를 만들 때는 기존 프로젝트에서 시작했습니다. 그러나 DirectX UWP 게임에 필요한 기본 기능을 제공 하는 기존 프로젝트를 아직 없는 경우 좋습니다 Visual Studio를 기반으로 프로젝트를 만들어야 **DirectX 11 앱 (유니버설 Windows- C++/CX)** 서식 파일을 기본 작업 3D 응용 프로그램을 제공 하기 때문입니다. 이렇게 하려면 다음 단계를 수행합니다.

1. Visual Studio 2019 선택 **파일 > 새로 만들기 > 프로젝트...**

2. 에 **새 프로젝트를 만듭니다** 창에서 **DirectX 11 앱 (유니버설 Windows- C++/CX)** 합니다. 설치 필수 구성 요소 없을 경우이 옵션에 보이지&mdash;참조 [워크 로드 및 구성 요소 추가 또는 제거 하 여 Visual Studio 2019 수정](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) 추가 구성 요소를 설치 하는 방법에 대 한 내용은 .

![새 프로젝트](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. 선택 **다음**, 한 다음 enter를 **프로젝트 이름**, **위치** 저장 될 파일에 대 한 및 **솔루션 이름**를 선택한 다음  **만들**합니다.



한 가지 중요 한 프로젝트 설정은 합니다 **DirectX 11 앱 (유니버설 Windows- C++/CX)** 템플릿은 합니다 **/ZW** Windows 런타임 언어 확장을 사용 하 여 프로그램을 사용 하도록 설정 하는 옵션. 이 옵션은 Visual Studio 템플릿을 사용할 때 기본적으로 사용됩니다. Visual Studio에서 컴파일러 옵션을 설정하는 방법에 대한 자세한 내용은 [컴파일러 옵션 설정](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options)을 참조하세요.

> **주의**    는 **/ZW** 와 같은 옵션 옵션과 호환 되지 않습니다 **/clr**합니다. **/clr**의 경우 동일한 Visual C++ 프로젝트에서 .NET Framework 및 Windows 런타임을 둘 다 대상으로 지정할 수 없다는 의미입니다.

 

Microsoft Store 다운로드 하는 모든 UWP 앱에서 앱 패키지의 형태로 제공 됩니다. 앱 패키지에는 앱 정보가 포함된 패키지 매니페스트가 있습니다. 예를 들어 앱의 기능(즉, 보호된 시스템 리소스 또는 사용자 데이터에 대해 필요한 액세스)을 지정할 수 있습니다. 앱이 특정 기능을 사용하도록 결정하면 패키지 매니페스트를 사용하여 필요한 기능을 선언합니다. 매니페스트를 사용하여 지원되는 장치 회전, 타일 이미지 및 시작 화면과 같은 프로젝트 속성을 지정할 수도 있습니다. 프로젝트에서 **Package.appxmanifest**를 열어 매니페스트를 편집할 수 있습니다. 앱 패키지에 대한 자세한 내용은 [앱 패키징](https://docs.microsoft.com/windows/uwp/packaging/index)을 참조하세요.

##  <a name="building-deploying-and-running-the-game"></a>게임 빌드, 배포 및 실행

Visual Studio 맨 위에 있는 드롭다운 메뉴의 녹색 재생 단추 왼쪽에서 배포 구성을 선택합니다. 장치의 아키텍처(32비트는 **x86**, 64비트는 **x64**) 및 **로컬 컴퓨터**를 대상으로 하는 **디버그**로 설정하는 것이 좋습니다. **원격 컴퓨터** 또는 USB를 통해 연결된 **장치**에서 테스트할 수도 있습니다. 그런 다음 녹색 재생 단추를 클릭하여 장치를 빌드하고 배포합니다.

![디버그, x64, 로컬 컴퓨터](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>게임 제어

터치, 가속도계, Xbox One 컨트롤러 또는 마우스를 사용하여 Marble Maze를 제어할 수 있습니다.

-   컨트롤러의 방향 패드를 사용하여 활성 메뉴 항목을 변경합니다.
-   터치, 또는 컨트롤러의 A 단추 또는 시작 단추를 사용하여 메뉴 항목을 선택합니다.
-   터치, 가속도계, 왼쪽 엄지스틱 또는 마우스를 사용하여 미로를 기울입니다.
-   터치, 또는 컨트롤러의 A 단추 또는 시작 단추 또는 마우스를 사용하여 최고 점수 테이블 등의 메뉴를 닫습니다.
-   컨트롤러의 시작 단추 또는 키보드의 P 키를 사용하여 게임을 중지하거나 다시 시작합니다.
-   컨트롤러의 뒤로 단추 또는 키보드의 Home 키를 사용하여 게임을 다시 시작합니다.
-   최소 점수 테이블이 표시될 때 컨트롤러의 뒤로 단추 또는 키보드의 Home 키를 사용하여 모든 점수를 지웁니다.

##  <a name="code-conventions"></a>코드 규칙


Windows 런타임은 특수 응용 프로그램 환경에서만 실행되는 UWP 앱을 만드는 데 사용할 수 있는 프로그래밍 인터페이스입니다. 이러한 앱 권한이 부여 된 함수, 데이터 형식 및 장치를 사용 하 고 Microsoft Store 배포 됩니다. 최하위 수준에서 Windows 런타임은 ABI(응용 프로그램 이진 인터페이스)로 구성됩니다. ABI는 JavaScript, .NET 언어, Visual C++ 등 여러 프로그래밍 언어가 Windows 런타임 API에 액세스할 수 있게 하는 하위 수준 이진 계약입니다.

JavaScript와 .NET에서 Windows 런타임 API를 호출하려면 해당 언어에 각 언어 환경과 관련된 프로젝션이 필요합니다. JavaScript 또는 .NET에서 Windows 런타임 API를 호출하는 경우 프로젝션을 호출하는 것이며, 프로젝션이 기본 ABI 함수를 호출합니다. C++에서 직접 ABI 함수를 호출할 수도 있지만 C++에 대한 프로젝션도 제공됩니다. 이러한 프로젝션을 사용할 경우 고성능을 유지하는 동시에 Windows 런타임 API를 훨씬 더 간단하게 사용할 수 있기 때문입니다. Microsoft는 Windows 런타임 프로젝션을 구체적으로 지원하는 언어 확장도 Visual C++에 제공합니다. 이러한 언어 확장은 대부분 C++/CLI 언어의 구문과 비슷합니다. 그러나 CLR(공용 언어 런타임)을 대상으로 하는 대신 기본 앱은 이 구문을 사용하여 Windows 런타임을 대상으로 합니다. 개체 참조 또는 캐럿 (^) 한정자는 참조 개수를 계산하여 런타임 개체를 자동으로 삭제할 수 있도록 하기 때문에 이 새로운 구문의 중요 부분입니다. [AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref), [Release](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) 등의 메서드를 호출하여 Windows 런타임 개체의 수명을 관리하는 대신, 런타임은 개체가 범위를 벗어나거나 모든 참조를 **nullptr**로 설정하는 경우와 같이 개체를 참조하는 다른 구성 요소가 없는 경우 개체를 삭제합니다. Visual C++를 사용하여 UWP 앱을 만드는 경우의 다른 중요 부분은 **ref new** 키워드입니다. **new** 대신 **ref new**를 사용하여 참조 개수가 계산되는 Windows 런타임 개체를 만듭니다. 자세한 내용은 [형식 시스템(C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)을 참조하세요.

> [!IMPORTANT]
> Windows 런타임 개체를 만들거나 Windows 런타임 구성 요소를 만드는 경우 **^** 및 **ref new**만 사용하면 됩니다. Windows 런타임을 사용하지 않는 핵심 응용 프로그램 코드를 작성하는 경우 표준 C++ 구문을 사용할 수 있습니다.

Marble Maze는 **^** 을 **Microsoft::WRL::ComPtr**과 함께 사용하여 힙 할당 개체를 관리하고 메모리 누수를 최소화합니다. 사용 하는 것이 좋습니다 ^ Windows 런타임 변수의 수명을 관리 하 **ComPtr** (예: 사용 하는 경우 DirectX), COM 변수의 수명을 관리 하 고 **std::shared\_ptr** 또는 **std::unique\_ptr** 다른 모든 힙 할당의 수명을 관리 하려면 C++ 개체입니다.

 

C++ UWP 앱에서 사용할 수 있는 언어 확장에 대한 자세한 내용은 [Visual C++ 언어 참조(C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)를 참조하세요.

###  <a name="error-handling"></a>오류 처리

Marble Maze는 예기치 않은 오류를 처리하는 주요 방법으로 예외 처리를 사용합니다. 일반적으로 게임 코드는 **HRESULT** 값과 같은 로깅 또는 오류 코드를 사용하여 오류를 나타내지만 예외 처리에는 두 가지 주요 장점이 있습니다. 먼저, 코드를 읽고 유지 관리하기 쉽게 만들 수 있습니다. 코드 관점에서 예외 처리는 오류를 처리할 수 있는 루틴에 오류를 전파하는 보다 효율적인 방법입니다. 오류 코드를 사용할 경우 일반적으로 각 함수에서 명시적으로 오류를 전파해야 합니다. 두 번째 장점은 오류 위치와 컨텍스트에서 즉시 중지할 수 있도록 예외가 발생할 때 중단되도록 Visual Studio 디버거를 구성할 수 있다는 것입니다. 또한 Windows 런타임은 예외 처리를 광범위하게 사용합니다. 따라서 코드에 예외 처리를 사용하여 모든 오류 처리를 하나의 모델로 결합할 수 있습니다.

오류 처리 모델에는 다음 규칙을 사용하는 것이 좋습니다.

-   예외를 사용하여 예기치 않은 오류를 전달합니다.
-   코드 흐름을 제어하는 데 예외를 사용하지 않습니다.
-   안전하게 처리하고 복구할 수 있는 예외만 catch합니다. 그렇지 않으면 예외를 catch하지 말고 앱이 종료될 수 있게 허용합니다.
-   **HRESULT**를 반환하는 DirectX 루틴을 호출하는 경우 **DX::ThrowIfFailed** 함수를 사용합니다. 이 함수는 [DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h)에 정의되어 있습니다. 제공된 **HRESULT**가 오류 코드인 경우 **ThrowIfFailed**에서 예외가 발생합니다. 예를 들어 **E\_포인터** 사용 하면 **ThrowIfFailed** 시키려면 [platform:: nullreferenceexception](https://docs.microsoft.com/cpp/cppcx/platform-nullreferenceexception-class)합니다.

    **ThrowIfFailed**를 사용하는 경우 다음 예제와 같이 코드를 읽기 쉽도록 DirectX 호출을 개별 줄에 배치합니다.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   예기치 않은 오류에는 **HRESULT**를 사용하지 않는 것이 좋지만 코드 흐름을 제어하는 데 예외 처리를 사용하지 않는 것이 더 중요합니다. 따라서 필요한 경우 **HRESULT** 반환 값을 사용하여 코드 흐름을 제어하는 것이 좋습니다.

###  <a name="sal-annotations"></a>SAL 주석

앱에서 오류를 찾는 데 도움이 되도록 SAL 주석을 코드 분석 도구와 함께 사용합니다.

Microsoft SAL(소스 코드 주석 언어)을 사용하여 함수가 해당 매개 변수를 사용하는 방법을 설명하거나 주석을 달 수 있습니다. SAL 주석은 반환 값에 대해서도 설명합니다. SAL 주석은 C/C++ 코드 분석 도구와 함께 작동하여 C 및 C++ 소스 코드에서 가능한 결함을 검색합니다. 이 도구를 통해 보고되는 일반적인 코딩 오류에는 버퍼 오버런, 초기화되지 않은 메모리, null 포인터 역참조, 메모리 및 리소스 누수 등이 있습니다.

[BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h)에 선언되어 있는 **BasicLoader::LoadMesh** 메서드를 고려합니다. 이 메서드는 `_In_`을 사용하여 *filename*을 입력 매개 변수(따라서 읽기만 수행됨)로 지정하고, `_Out_`을 사용하여 *vertexBuffer* 및 *indexBuffer*를 출력 매개 변수(따라서 쓰기만 수행됨)로 지정하고, `_Out_opt_`를 사용하여 *vertexCount* 및 *indexCount*를 선택적 출력 매개 변수(쓰기도 가능함)로 지정합니다. *vertexCount* 및 *indexCount*는 선택적 출력 매개 변수이므로 **nullptr**이 될 수 있습니다. C/C++ 코드 분석 도구는 이 메서드 호출을 검사하여 전달되는 매개 변수가 이러한 조건을 충족하는지 확인합니다.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

앱에서 코드 분석을 수행하려면 메뉴 모음에서 **빌드 > 솔루션에서 코드 분석 실행**을 선택합니다. 코드 분석에 대한 자세한 내용은 [코드 분석을 사용하여 C/C++ 코드 품질 분석](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis)을 참조하세요.

사용 가능한 주석의 전체 목록은 sal.h에 정의되어 있습니다. 자세한 내용은 [SAL 주석](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations)을 참조하세요.

## <a name="next-steps"></a>다음 단계


Marble Maze 응용 프로그램 코드가 구성된 방식 및 DirectX UWP 앱의 구조와 일반적인 데스크톱 응용 프로그램 구조 간의 차이점에 대한 자세한 내용은 [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)를 참조하세요.

## <a name="related-topics"></a>관련 항목


* [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)
* [UWP 게임 Marble Maze 개발 C++ 와 DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




