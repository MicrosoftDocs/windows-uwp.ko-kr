---
title: Marble Maze 샘플 기본 사항
description: 이 문서에서는 대리석 인 대리석 프로젝트의 기본적인 특성에 대해 설명 합니다. 예를 들어 Windows 런타임 환경에서 Visual C++를 사용 하는 방법,이를 만들고 구조화 하는 방법 및 빌드하는 방법을 설명 합니다.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 샘플, directx, 기본 사항
ms.localizationpriority: medium
ms.openlocfilehash: 714641be3c5ae6e202f0d6b7da5a1a4c1fc93d86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165327"
---
# <a name="marble-maze-sample-fundamentals"></a>Marble Maze 샘플 기본 사항




이 항목 &mdash; 에서는 Windows 런타임 환경에서 Visual C++를 사용 하는 방법, 만들고 구조화 하는 방법 및 빌드하는 방법을 보여 주는 대리석의 기본 특징에 대해 설명 합니다. 또한이 항목에서는 코드에 사용 되는 몇 가지 규칙도 설명 합니다.

> [!NOTE]
> 이 문서에 해당 하는 샘플 코드는 [DirectX 대리석 미로 game 샘플](https://github.com/microsoft/Windows-appsample-marble-maze)에서 찾을 수 있습니다.

다음은 UWP (유니버설 Windows 플랫폼) 게임을 계획 하 고 개발할 때이 문서에서 설명 하는 몇 가지 주요 사항입니다.

-   Visual Studio의 **directx 11 앱 (유니버설 Windows-c + +/cx)** 템플릿을 사용 하 여 directx UWP 게임을 만듭니다.
-   Windows 런타임는 클래스와 인터페이스를 제공 하므로 더 현대적인 개체 지향 방식으로 UWP 앱을 개발할 수 있습니다.
-   Hat (^) 기호와 함께 개체 참조를 사용 하 여 Windows 런타임 변수의 수명을 관리 하 고, [Microsoft:: WRL:: ComPtr](/cpp/windows/comptr-class) 를 사용 하 여 COM 개체의 수명을 관리 하 고, [std:: shared \_ ptr](/cpp/standard-library/shared-ptr-class) 또는 [std:: unique \_ ptr](/cpp/standard-library/unique-ptr-class) 을 사용 하 여 다른 모든 힙 할당 c + + 개체의 수명을 관리 합니다.
-   대부분의 경우 결과 코드 대신 예외 처리를 사용하여 예상치 못한 오류를 처리합니다.
-   코드 분석 도구와 함께 [SAL 주석을](/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) 사용 하 여 앱에서 오류를 검색할 수 있습니다.

## <a name="creating-the-visual-studio-project"></a>Visual Studio 프로젝트 만들기


샘플을 다운로드 하 고 추출한 경우 Visual Studio에서 **MarbleMaze_VS2017 .sln** 파일 ( **c + +** 폴더에 있음)을 열 수 있습니다. 그러면 코드가 앞에 표시 됩니다.

Marble Maze에 대한 Visual Studio 프로젝트를 만들 때 기존 프로젝트로 시작했습니다. 그러나 DirectX UWP 게임에 필요한 기본 기능을 제공 하는 기존 프로젝트가 아직 없는 경우에는 기본적인 작업 3D 응용 프로그램을 제공 하기 때문에 Visual Studio **DirectX 11 앱 (유니버설 Windows-c + +/cx)** 템플릿을 기반으로 프로젝트를 만드는 것이 좋습니다. 이렇게 하려면 다음 단계를 수행하세요.

1. Visual Studio 2019에서 **파일 > 새 > 프로젝트** ...를 선택 합니다.

2. **새 프로젝트 만들기** 창에서 **DirectX 11 앱 (유니버설 Windows-c + +/cx)** 을 선택 합니다. 이 옵션이 표시 되지 않으면 필수 구성 요소를 설치 하지 않은 것 &mdash; 입니다. 추가 구성 요소를 설치 하는 방법에 대 한 자세한 내용은 [작업 및 구성 요소를 추가 하거나 제거 하 여 Visual Studio 2019 수정](/visualstudio/install/modify-visual-studio) 을 참조 하세요.

![새 프로젝트](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. **다음**을 선택 하 고 **프로젝트 이름**, 저장할 파일의 **위치** 및 **솔루션 이름을**입력 한 다음 **만들기**를 선택 합니다.



**DirectX 11 앱 (유니버설 Windows-c + +/cx)** 템플릿의 중요 한 프로젝트 설정은 프로그램에서 Windows 런타임 언어 확장을 사용할 수 있도록 하는 **/zw** 옵션입니다. Visual Studio 템플릿을 사용하면 이 옵션이 기본적으로 사용됩니다. Visual Studio에서 컴파일러 옵션을 설정 하는 방법에 대 한 자세한 내용은 [컴파일러 옵션 설정](/cpp/build/reference/setting-compiler-options) 을 참조 하세요.

> **주의**    **/Zw** 옵션은 **/clr**와 같은 옵션과 호환 되지 않습니다. **/Clr**의 경우 동일한 Visual C++ 프로젝트에서 .NET Framework와 Windows 런타임를 모두 대상으로 지정할 수 없습니다.

 

Microsoft Store에서 가져오는 모든 UWP 앱은 앱 패키지 형식으로 제공 됩니다. 앱 패키지에는 앱 정보가 포함된 패키지 매니페스트가 들어 있습니다. 예를 들어 앱의 기능(즉, 보호된 시스템 리소스 또는 사용자 데이터에 대한 필수 액세스 권한)을 지정할 수 있습니다. 앱에 특정 기능이 필요한 것을 확인하면 패키지 매니페스트를 사용하여 필수 기능을 선언합니다. 매니페스트를 사용하여 지원되는 장치 회전, 타일 이미지, 시작 화면 등의 프로젝트 속성을 지정할 수도 있습니다. 프로젝트에서 **appxmanifest.xml** 를 열어 매니페스트를 편집할 수 있습니다. 앱 패키지에 대 한 자세한 내용은 [앱](../packaging/index.md)패키지를 참조 하세요.

##  <a name="building-deploying-and-running-the-game"></a>게임 빌드, 배포 및 실행

Visual Studio 위쪽의 드롭다운 메뉴에서 녹색 재생 단추의 왼쪽에 있는 배포 구성을 선택 합니다. 장치 아키텍처를 대상으로 하는 **디버그** 로 설정 하는 것이 좋습니다 (32 비트의 경우**x86** , 64 비트의 경우 **x64** ) 및 **로컬 컴퓨터**로 설정 하는 것이 좋습니다. **원격 컴퓨터**또는 USB를 통해 연결 된 **장치** 를 테스트할 수도 있습니다. 그런 다음 녹색 재생 단추를 클릭 하 여 장치를 빌드하고 배포 합니다.

![디버그 (x64 로컬 컴퓨터](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>게임 제어

터치,가 속도계, Xbox One 컨트롤러 또는 마우스를 사용 하 여 대리석의 미로를 제어할 수 있습니다.

-   컨트롤러의 방향 패드를 사용하여 활성 메뉴 항목을 변경합니다.
-   컨트롤러에서 터치, A 또는 시작 단추 또는 마우스를 사용 하 여 메뉴 항목을 선택 합니다.
-   터치, 가속도계, 왼쪽 엄지스틱 또는 마우스를 사용하여 미로를 기울입니다.
-   터치, 컨트롤러의 A 또는 시작 단추 또는 마우스를 사용 하 여 높은 점수 표 같은 메뉴를 닫습니다.
-   컨트롤러의 시작 단추나 키보드의 P 키를 사용 하 여 게임을 일시 중지 하거나 다시 시작 합니다.
-   컨트롤러의 뒤로 단추 또는 키보드의 Home 키를 사용하여 게임을 다시 시작합니다.
-   고득점 테이블이 표시 되 면 컨트롤러의 뒤로 단추를 사용 하거나 키보드의 Home 키를 사용 하 여 모든 점수를 지웁니다.

##  <a name="code-conventions"></a>코드 규칙


Windows 런타임은 특수 한 응용 프로그램 환경 에서만 실행 되는 UWP 앱을 만드는 데 사용할 수 있는 프로그래밍 인터페이스입니다. 이러한 앱은 권한 있는 함수, 데이터 형식 및 장치를 사용 하며 Microsoft Store에서 배포 됩니다. 가장 낮은 수준에서 Windows 런타임은 ABI (응용 프로그램 이진 인터페이스)로 구성 됩니다. ABI는 JavaScript, .NET 언어 및 Visual C++ 같은 여러 프로그래밍 언어에서 Windows 런타임 Api에 액세스할 수 있도록 하는 하위 수준 이진 계약입니다.

JavaScript 및 .NET에서 Windows 런타임 Api를 호출 하기 위해 해당 언어에는 각 언어 환경과 관련 된 프로젝션이 필요 합니다. JavaScript 또는 .NET에서 Windows 런타임 API를 호출 하는 경우 프로젝션을 호출 하 고,이는 기본 ABI 함수를 호출 합니다. C + +에서 직접 ABI 함수를 호출할 수 있지만, Microsoft는 c + +에 대 한 예측도 제공 합니다 .이는 Windows 런타임 Api를 사용 하는 것이 훨씬 더 간단 하 고 높은 성능을 유지 합니다. Microsoft는 또한 Windows 런타임 프로젝션을 구체적으로 지 원하는 Visual C++에 대 한 언어 확장을 제공 합니다. 이러한 언어 확장은 대부분 C++/CLI 언어의 구문과 유사합니다. 하지만 네이티브 앱은 CLR (공용 언어 런타임)을 대상으로 지정 하는 대신이 구문을 사용 하 여 Windows 런타임을 대상으로 지정 합니다. 개체 참조 또는 hat (^) 한정자는 참조 횟수를 통해 런타임 개체를 자동으로 삭제할 수 있도록 하기 때문에이 새 구문의 중요 한 부분입니다. [AddRef](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) 및 [Release](/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) 와 같은 메서드를 호출 하 여 Windows 런타임 개체의 수명을 관리 하는 대신, 다른 구성 요소에서 참조 하는 경우 (예: 범위를 벗어나거나 **nullptr**에 대 한 모든 참조를 설정 하는 경우) 런타임에서 개체를 삭제 합니다. Visual C++를 사용 하 여 UWP 앱을 만드는 또 다른 중요 한 부분은 **ref new** 키워드입니다. **New** 대신 **ref new** 를 사용 하 여 참조 횟수가 계산 된 Windows 런타임 개체를 만듭니다. 자세한 내용은 [형식 시스템 (c + +/cx)](/cpp/cppcx/type-system-c-cx)을 참조 하세요.

> [!IMPORTANT]
> **^** Windows 런타임 개체를 만들거나 Windows 런타임 구성 요소를 만들 때만 및 **ref new** 를 사용 해야 합니다. Windows 런타임 사용 하지 않는 핵심 응용 프로그램 코드를 작성할 때는 표준 c + + 구문을 사용할 수 있습니다.

대리석 미로는 **^** **Microsoft:: WRL:: ComPtr** 과 함께를 사용 하 여 힙 할당 개체를 관리 하 고 메모리 누수를 최소화 합니다. ^를 사용 하 여 Windows 런타임 변수의 수명을 관리 하 고, COM 변수의 수명을 관리 하는 **ComPtr** (예: DirectX를 사용 하는 경우), **std:: shared \_ ptr** 또는 **std:: unique \_ ptr** 을 사용 하 여 다른 모든 힙 할당 c + + 개체의 수명을 관리 하는 것이 좋습니다.

 

C + + UWP 앱에서 사용할 수 있는 언어 확장에 대 한 자세한 내용은 [Visual C++ 언어 참조 (c + +/cx)](/cpp/cppcx/visual-c-language-reference-c-cx)를 참조 하세요.

###  <a name="error-handling"></a>오류 처리

Marble Maze는 예상치 못한 오류를 처리하는 기본 방법으로 예외 처리를 사용합니다. 일반적으로 게임 코드는 **HRESULT** 값과 같은 로깅 또는 오류 코드를 사용 하 여 오류를 표시 하지만 예외 처리에는 두 가지 주요 이점이 있습니다. 첫 번째 이점은 예외 처리를 사용하면 코드를 읽고 관리하기 쉽습니다. 코드 측면에서 예외 처리는 오류를 처리할 수 있는 루틴으로 오류를 전파할 수 있는 보다 효율적인 방법입니다. 일반적으로 오류 코드를 사용하려면 각 함수가 명시적으로 오류를 전파해야 합니다. 두 번째 이점은 오류 위치와 컨텍스트에서 즉시 중지할 수 있도록 예외가 발생할 때 중단되도록 Visual Studio 디버거를 구성할 수 있다는 것입니다. 또한 Windows 런타임는 예외 처리를 광범위 하 게 사용 합니다. 따라서 코드에 예외 처리를 사용하면 모든 오류 처리를 하나의 모델로 결합할 수 있습니다.

오류 처리 모델에는 다음 규칙을 사용하는 것이 좋습니다.

-   예외를 사용하여 예상치 못한 오류를 전달합니다.
-   예외를 사용하여 코드 흐름을 제어하지 마세요.
-   안전하게 처리하고 복구할 수 있는 예외만 catch합니다. 그렇지 않으면 예외를 catch하지 말고 앱이 종료될 수 있게 하십시오.
-   **HRESULT**를 반환 하는 DirectX 루틴을 호출 하는 경우에는 **DX:: ThrowIfFailed** 함수를 사용 합니다. 이 함수는 [Directxhelper. h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h)에 정의 되어 있습니다. **ThrowIfFailed** 는 제공 된 **HRESULT** 가 오류 코드 이면 예외를 throw 합니다. 예를 들어 **E \_ 포인터** 를 누르면 **ThrowIfFailed** 에서 [Platform:: NullReferenceException](/cpp/cppcx/platform-nullreferenceexception-class)를 throw 합니다.

    **ThrowIfFailed**를 사용 하는 경우 다음 예제와 같이 코드 가독성을 향상 시킬 수 있도록 DirectX 호출을 별도의 줄에 배치 합니다.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   예기치 않은 오류에 대해 **HRESULT** 를 사용 하지 않는 것이 좋지만 예외 처리를 사용 하 여 코드 흐름을 제어 하지 않는 것이 좋습니다. 따라서 코드 흐름을 제어 하는 데 필요한 경우 **HRESULT** 반환 값을 사용 하는 것이 좋습니다.

###  <a name="sal-annotations"></a>SAL 주석

앱의 검색 오류에 도움이 되도록 SAL 주석을 코드 분석 도구와 함께 사용합니다.

Microsoft SAL(소스 코드 주석 언어)을 사용하여 함수가 매개 변수를 사용하는 방법을 설명하거나 주석을 달 수 있습니다. SAL 주석은 반환 값도 설명합니다. SAL 주석은 C/C++ 코드 분석 도구와 함께 작동하여 C 및 C++ 소스 코드에서 가능한 결함을 검색합니다. 이 도구를 통해 보고되는 일반적인 코딩 오류에는 버퍼 오버런, 초기화되지 않은 메모리, null 포인터 역참조, 메모리 및 리소스 누수 등이 포함됩니다.

Basicloader [. h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h)에 선언 된 **Basicloader:: loadmesh** 메서드를 살펴보겠습니다. 이 메서드는를 사용 하 여 `_In_` *filename* 이 입력 매개 변수이 고, 따라서에서 읽기만 가능 하도록 지정 하 `_Out_` 고, *vertexBuffer* 및 *indexbuffer* 가 출력 매개 변수 (즉,에만 기록 됨)로 지정 하 고, `_Out_opt_` *vertexCount* 및 *indexbuffer* 가 선택적 출력 매개 변수 (로 작성 될 수 있음) 임을 지정 합니다. *VertexCount* 및 *indexcount* 는 선택적 출력 매개 변수 이므로 **nullptr**가 될 수 있습니다. C/C++ 코드 분석 도구는 이 메서드에 대한 호출을 검사하여 전달하는 매개 변수가 이러한 기준을 충족하는지 확인합니다.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

앱에 대 한 코드 분석을 수행 하려면 메뉴 모음에서 빌드를 선택 하 **> 솔루션에서 코드 분석 실행**을 선택 합니다. 코드 분석에 대 한 자세한 내용은 [코드 분석을 사용 하 여 c/c + + 코드 품질 분석](/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis)을 참조 하세요.

사용할 수 있는 주석의 전체 목록은 sal.h에 정의되어 있습니다. 자세한 내용은 [SAL 주석](/cpp/c-runtime-library/sal-annotations)을 참조 하세요.

## <a name="next-steps"></a>다음 단계


대리석 x 메 이즈 응용 프로그램 코드의 구성 방식 및 DirectX UWP 앱의 구조가 기존 데스크톱 응용 프로그램의 구조와 어떻게 다른 지에 대 한 자세한 내용은 [대리석 미로 응용 프로그램 구조](marble-maze-application-structure.md) 를 참조 하세요.

## <a name="related-topics"></a>관련 항목


* [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)
* [C + + 및 c + +의 UWP 게임, 대리석](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 