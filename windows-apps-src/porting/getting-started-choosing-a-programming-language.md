---
author: stevewhims
title: 프로그래밍 언어 선택
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: 프로그래밍 언어 선택
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 24b374a007bf562b2a1c8ba0afe42e75e04bc63e
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5928276"
---
# <a name="getting-started-choosing-a-programming-language"></a>시작: 프로그래밍 언어 선택


## <a name="choosing-a-programming-language"></a>프로그래밍 언어 선택

이제 UWP(유니버설 Windows 플랫폼) 앱을 개발할 때 선택할 수 있는 프로그래밍 언어에 대해 알아야 합니다. 이 문서의 연습에서는 C#을 사용하지만 개발자는 하나 이상의 프로그래밍 언어를 사용하여 UWP 앱을 개발할 수 있습니다([언어, 도구 및 프레임워크](https://msdn.microsoft.com/library/windows/apps/dn465799) 참조).

C++, C#, Microsoft Visual Basic 및 JavaScript를 사용하여 개발할 수 있습니다. JavaScript에서는 UI 레이아웃에 HTML5 태그를 사용하고 다른 언어에서는 *XAML(Extensible Application Markup Language)* 이라는 태그 언어를 사용하여 해당 UI를 설명합니다.

이 도움말에서는 C#을 기반으로 하지만 언어마다 살펴볼 수 있는 고유한 이점이 있습니다. 예를 들어 앱의 성능이 주요 고려 사항인 경우, 특히 그래픽이 많이 나오는 경우 C++를 선택하는 것이 좋습니다. Visual Basic의 Microsoft .NET 버전은 Visual Basic 앱 개발자에게 유용합니다. HTML5로 작성한 JavaScript는 웹 개발 경험이 있는 개발자에게 유용합니다. 자세한 내용은 다음 중 하나를 참조하세요.

-   [C + +를 사용 하 여 첫 UWP 앱 만들기](../get-started/create-a-basic-windows-10-app-in-cpp.md)
-   [C# 또는 Visual Basic을 사용 하 여 첫 UWP 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [JavaScript를 사용 하 여 첫 UWP 앱 만들기](../get-started/create-a-hello-world-app-js-uwp.md)

**참고**을 3D 그래픽을 사용 하는 앱에 대 한 OpenGL 및 OpenGL ES 표준을 기본적으로 UWP 앱에 사용할 수 있습니다. OpenGL ES 코드를 Microsoft DirectX에 다시 작성하지 않으려는 경우 **Angle**이 유용할 수 있습니다. Angle은 OpenGL API 호출을 DirectX API 호출로 변환하여 OpenGL을 DirectX로 변환하도록 디자인된 진행 중인 프로젝트입니다. 자세한 내용은 다음을 참조하세요.
-   [Angle](https://code.google.com/p/angleproject/)
-   [DirectX를 사용 하 여 첫 UWP 앱 만들기](https://msdn.microsoft.com/library/windows/apps/br229580)
-   [DirectX를 사용 하는 UWP 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=263603)
-   [DirectX SDK 위치](https://msdn.microsoft.com/library/windows/desktop/ee663275)

## <a name="giving-c-a-go"></a>C#

iOS 개발자는 Objective-C 및 Swift에 익숙합니다. 두 언어 모두에 가장 가까운 Microsoft 프로그래밍 언어는 C#입니다. 대부분의 개발자와 대부분의 앱의 경우 가장 쉽고 짧은 기간 안에 배워서 사용할 수 있는 언어는 C#이므로 이 문서의 정보와 연습에서는 이 언어를 중심으로 설명합니다. C#에 대한 자세한 내용은 다음을 참조하세요.

-   [C# 또는 Visual Basic을 사용 하 여 첫 UWP 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [C#을 사용 하는 UWP 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=263453)
-   [Visual C#](http://go.microsoft.com/fwlink/p/?LinkId=263450)

다음은 Objective-C와 C#으로 작성된 클래스입니다. Objective-C 버전이 먼저 표시되고 다음에 C# 버전이 표시됩니다.

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

현재는 C# 버전입니다. Swift처럼 헤더와 구현이 동일한 파일에 있습니다.

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

C#은 간편한 언어이며 .NET을 구성하는 많은 지원 클래스 및 프레임워크와 함께 제공합니다. 대괄호 없이도 코드를 빠르고 쉽게 작성할 수 있습니다.

## <a name="next-step"></a>다음 단계

[시작: Visual Studio 둘러보기](getting-started-getting-around-in-visual-studio.md)
