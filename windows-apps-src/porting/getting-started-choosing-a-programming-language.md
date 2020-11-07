---
title: 프로그래밍 언어 선택
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: UWP (유니버설 Windows 플랫폼) 앱을 개발할 때 선택할 수 있는 프로그래밍 언어에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2e4613b565545bfb7807c98fa74495b4b60b57bb
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339542"
---
# <a name="getting-started-choosing-a-programming-language"></a>시작: 프로그래밍 언어 선택

## <a name="choosing-a-programming-language"></a>프로그래밍 언어 선택

더 진행 하기 전에 UWP (유니버설 Windows 플랫폼) 앱을 개발할 때 선택할 수 있는 프로그래밍 언어에 대해 알고 있어야 합니다. 이 문서의 연습에서는 c #을 사용 하지만 하나 이상의 프로그래밍 언어를 사용 하 여 UWP 앱을 개발할 수 있습니다 ( [언어, 도구 및 프레임 워크](/previous-versions/windows/apps/dn465799(v=win.10))참조).

C + +, c #, Microsoft Visual Basic 및 JavaScript를 사용 하 여 개발할 수 있습니다. JavaScript는 UI 레이아웃에 HTML5 태그를 사용 하 고 다른 언어는 *XAML (Extensible Application Markup Language)* 이라는 태그 언어를 사용 하 여 ui를 설명 합니다.

이 도움말에서는 C#을 기반으로 하지만 언어마다 살펴볼 수 있는 고유한 이점이 있습니다. 예를 들어 응용 프로그램의 성능이 매우 중요 한 경우 특히 집약적 그래픽의 경우 c + +를 선택 하는 것이 적합 합니다. Microsoft .NET 버전의 Visual Basic는 Visual Basic 앱 개발자에 게 적합 합니다. JavaScript는 웹 개발 백그라운드에서 제공 되는 경우에 유용 합니다. 자세한 내용은 다음 중 하나를 참조 하세요.

-   [C # 또는 Visual Basic를 사용 하 여 "Hello, 세계!" 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [C++/WinRT를 사용하여 "Hello, World!" 앱 만들기](../get-started/create-a-basic-windows-10-app-in-cppwinrt.md)
-   [C + +/CX를 사용 하 여 "Hello, 세계!" 앱 만들기](../get-started/create-a-basic-windows-10-app-in-cpp.md)
-   [JavaScript를 사용 하 여 "Hello, 세계!" 앱 만들기](/windows/apps/get-started/)

**참고**  3D 그래픽을 사용 하는 앱의 경우 UWP 앱에서 OpenGL 및 OpenGL ES 표준을 기본적으로 사용할 수 있는 것은 아닙니다. Microsoft DirectX에 OpenGL ES 코드를 다시 작성 하지 않고 **각도** 에 대해 알고 싶을 수 있습니다. 각도는 OpenGL API 호출을 DirectX API 호출로 변환 하 여 OpenGL을 DirectX로 변환 하도록 디자인 된 진행 중인 프로젝트입니다. 자세한 알아보려면 다음을 참조하세요.
-   [각도](https://bugs.chromium.org/p/angleproject/)
-   [DirectX를 사용 하 여 첫 번째 UWP 앱 만들기](/previous-versions/windows/apps/br229580(v=win.10))
-   [DirectX를 사용 하는 UWP 앱 샘플](/samples/browse/?expanded=windows&products=windows-uwp&terms=directx)
-   [DirectX SDK 위치](/windows/desktop/directx-sdk--august-2009-)

## <a name="giving-c-a-go"></a>C # a go 제공

IOS 개발자는 목표-C 및 Swift에 익숙합니다. 가장 가까운 Microsoft 프로그래밍 언어는 c #입니다. 대부분의 개발자와 대부분의 앱에 대해 학습 하 고 사용할 수 있는 가장 쉽고 빠른 언어인 c # 이라고 생각 합니다. 따라서이 문서의 정보 및 연습은 해당 언어에 초점을 둡니다. C #에 대 한 자세한 내용은 다음을 참조 하세요.

-   [C # 또는 Visual Basic를 사용 하 여 첫 번째 UWP 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [C를 사용 하는 UWP 앱 샘플 #](/samples/browse/?expanded=windows&languages=csharp&products=windows-uwp)
-   [Visual C#](/dotnet/csharp/)

다음은 목표-C 및 c #으로 작성 된 클래스입니다. 목표-C 버전이 먼저 표시 된 다음 c # 버전이 표시 됩니다.

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

이제 c # 버전입니다. Swift와 같이 헤더와 구현은 별도의 파일에 있지 않은 것을 볼 수 있습니다.

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C #은 쉽게 선택할 수 있는 언어 이며 .NET을 구성 하는 여러 지원 클래스 및 프레임 워크를 제공 합니다. 지금은 괄호 없이 코드를 작성 하는 것이 좋습니다.

## <a name="next-step"></a>다음 단계

[시작: Visual Studio 둘러보기](getting-started-getting-around-in-visual-studio.md)