---
연습: C++로 기본적인 Windows 런타임 구성 요소를 만들고 JavaScript 또는 C에서 호출#
이 연습에서는 JavaScript, C# 또는 Visual Basic에서 호출할 수 있는 기본 Windows 런타임 구성 요소 DLL을 만드는 방법을 보여 줍니다.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
---

# 연습: C++로 기본적인 Windows 런타임 구성 요소를 만들고 JavaScript 또는 C에서 호출#


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

이 연습에서는 JavaScript, C# 또는 Visual Basic에서 호출할 수 있는 기본 Windows 런타임 구성 요소 DLL을 만드는 방법을 보여 줍니다. 이 연습을 시작하기 전에 쉽게 ref 클래스를 사용할 수 있게 해주는 ABI(추상 이진 인터페이스), ref 클래스, Visual C++ 구성 요소 확장 등의 개념을 이해해야 합니다. 자세한 내용은 [C++로 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-cpp.md) 및 [Visual C++ 언어 참조(C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)를 참조하세요.

## C++ 구성 요소 DLL 만들기


이 예제에서는 구성 요소 프로젝트를 먼저 만들지만 JavaScript 프로젝트를 먼저 만들 수도 있습니다. 순서는 중요하지 않습니다.

구성 요소의 기본 클래스에는 속성 및 메서드 정의의 예와 이벤트 선언이 포함되어 있습니다. 이러한 항목은 작업 방식을 보여 주기 위해서만 제공됩니다. 필수는 아니며, 이 예제에서는 생성된 코드를 고유한 코드로 바꿀 것입니다.

## **C++ 구성 요소 프로젝트를 만들려면**

1.  Visual Studio 메뉴 모음에서 **파일, 새로 만들기, 프로젝트**를 선택합니다.
2.  **새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C++**를 확장하고 유니버설 Windows 앱에 대한 노드를 선택합니다.
3.  가운데 창에서 **Windows 런타임 구성 요소**를 선택하고 프로젝트 이름을 WinRT\_CPP로 지정합니다.
4.  **확인** 단추를 선택합니다.

## **구성 요소에 활성화 가능 클래스를 추가하려면**

1.  활성화 가능 클래스는 **new** 식(Visual Basic의 **New** 또는 C++의 **ref new**)을 사용하여 클라이언트 코드에서 만들 수 있는 클래스입니다. 구성 요소에서 **public ref class sealed**로 선언합니다. 실제로 Class1.h 및 .cpp 파일에는 이미 ref 클래스가 있습니다. 이름을 변경할 수 있지만 이 예제에서는 기본 이름인 Class1을 사용합니다. 필요한 경우 구성 요소에서 추가 ref 클래스 또는 일반 클래스를 정의할 수 있습니다. ref 클래스에 대한 자세한 내용은 [형식 시스템(C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx)을 참조하세요.

2.  Class1.h에 다음 \#include 지시문을 추가합니다.

    ```cpp
             private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
            {
                var nativeObject = new WinRT_CPP.Class1();

                StringBuilder sb = new StringBuilder();
                sb.Append("Primes found (unordered): ");
                PrimesUnOrderedResult.Text = sb.ToString();

                // primeFoundEvent is a user-defined event in nativeObject
                // It passes the results back to this thread as they are produced
                // and the event handler that we define here immediately displays them.
                nativeObject.primeFoundEvent += (n) =>
                {
                    sb.Append(n.ToString()).Append(" ");
                    PrimesUnOrderedResult.Text = sb.ToString();
                };

                // Call the async method.
                var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

                // Provide a handler for the Progress event that the asyncResult
                // object fires at regular intervals. This handler updates the progress bar.
                asyncResult.Progress += (asyncInfo, progress) =>
                    {
                        PrimesUnOrderedProgress.Value = progress;
                    };
            }

            private void Clear_Button_Click(object sender, RoutedEventArgs e)
            {
                PrimesOrderedProgress.Value = 0;
                PrimesUnOrderedProgress.Value = 0;
                PrimesUnOrderedResult.Text = "";
                PrimesOrderedResult.Text = "";
                Result1.Text = "";
            }
    ```

## 앱 실행


솔루션 탐색기에서 프로젝트 노드의 바로 가기 메뉴를 열고 **시작 프로젝트로 설정**을 선택하여 C# 프로젝트 또는 JavaScript 프로젝트를 시작 프로젝트로 선택합니다. 그런 다음 디버깅을 사용하여 실행하려면 F5 키를 누르고, 디버깅을 사용하지 않고 실행하려면 Ctrl+F5를 누릅니다.

## 개체 브라우저에서 구성 요소 검사(옵션)


개체 브라우저에서 .winmd 파일에 정의된 모든 Windows 런타임 형식을 검사할 수 있습니다. 여기에는 플랫폼 네임스페이스와 기본 네임스페이스의 형식이 포함됩니다. 그러나 Platform::Collections 네임스페이스의 형식은 winmd 파일이 아니라 헤더 파일 collections.h에 정의되어 있으므로 개체 브라우저에 표시되지 않습니다.

## **구성 요소를 검사하려면**

1.  메뉴 모음에서 **보기, 개체 브라우저**(Ctrl+Alt+J)를 선택합니다.
2.  개체 브라우저의 왼쪽 창에서 WinRT\_CPP 노드를 확장하여 구성 요소에 정의된 형식 및 메서드를 표시합니다.

## 디버깅 팁


디버깅 환경을 개선하려면 공용 Microsoft 기호 서버에서 디버깅 기호를 다운로드하세요.

## **디버깅 기호를 다운로드하려면**

1.  메뉴 모음에서 **도구, 옵션**을 선택합니다.
2.  **옵션** 대화 상자에서 **디버깅**을 확장하고 **기호**를 선택합니다.
3.  **Microsoft 기호 서버**를 선택한 다음 **확인** 단추를 선택합니다.

처음으로 기호를 다운로드하는 경우 시간이 걸릴 수도 있습니다. 다음에 F5 키를 누를 때 성능을 향상시키려면 기호를 캐시할 로컬 디렉터리를 지정합니다.

구성 요소 DLL이 있는 JavaScript 솔루션을 디버그하는 경우 구성 요소에서 네이티브 코드를 단계별로 실행하거나 스크립트를 단계별로 실행하도록 디버거를 설정할 수 있지만 둘 다 동시에 실행하도록 설정할 수는 없습니다. 설정을 변경하려면 솔루션 탐색기에서 JavaScript 프로젝트 노드의 바로 가기 메뉴를 열고 **속성, 디버깅, 디버거 형식**을 선택합니다.

패키지 디자이너에서 적절한 기능을 선택해야 합니다. Package.appxmanifest 파일을 열어 패키지 디자이너를 열 수 있습니다. 예를 들어 사진 폴더의 파일을 프로그래밍 방식으로 액세스하는 경우 패키지 디자이너의 **기능** 창에서 **사진 라이브러리** 확인란을 선택해야 합니다.

JavaScript 코드가 구성 요소의 public 속성 또는 메서드를 인식할 수 없는 경우 JavaScript에서 Camel Casing을 사용 중인지 확인합니다. 예를 들어 `ComputeResult` C++ 메서드는 JavaScript에서 `computeResult`로 참조해야 합니다.

솔루션에서 C++ Windows 런타임 구성 요소 프로젝트를 제거하는 경우 JavaScript 프로젝트에서 프로젝트 참조를 수동으로 제거해야 합니다. 이렇게 하지 않으면 이후 디버그 또는 빌드 작업을 수행할 수 없습니다. 필요한 경우 DLL에 대한 어셈블리 참조를 추가할 수 있습니다.

## 관련 항목

* [C++로 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=Mar16_HO1-->


