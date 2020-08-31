---
title: 마우스 모드를 사용하지 않도록 설정하는 방법
description: 'HTML 및 UWP (XAML/c # 유니버설 Windows 플랫폼) 응용 프로그램에서 기본 마우스 모드를 해제 하는 방법에 대해 알아봅니다.'
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
ms.localizationpriority: medium
ms.openlocfilehash: 16b2df2d84c0064c2549c75d867123d90e663314
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094600"
---
# <a name="how-to-disable-mouse-mode"></a>마우스 모드를 사용하지 않도록 설정하는 방법
기본적으로 모든 응용 프로그램에 마우스 모드가 설정 되어 있습니다. 즉, 옵트아웃 되지 않은 모든 응용 프로그램은 마우스 포인터를 받습니다 (콘솔의 가장자리 브라우저에 있는 것과 유사). 이 기능을 해제 하 고 방향 컨트롤러 탐색을 최적화 하는 것이 좋습니다.   
   
## <a name="html"></a>HTML   
UWP (JavaScript 유니버설 Windows 플랫폼) 앱에서 방향 컨트롤러 탐색을 켜려면 [Tvhelpers 방향 탐색](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) JavaScript 라이브러리를 사용 합니다. 앱 패키지에 방향성 탐색 JavaScript 파일을 포함 하 고, 방향 컨트롤러 탐색이 필요한 모든 HTML 페이지에 해당 파일에 대 한 참조를 추가 합니다.

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
자세한 내용은 [방향 탐색 wiki](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation)를 참조 하세요.

대신 마우스 모드를 끄고 DOM 또는 WinRT 게임 패드 Api를 직접 사용 하려는 경우 필요한 모든 페이지에 대해 다음을 실행 합니다. 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   이 속성은 기본적으로로 설정 `mouse` 되며, 마우스 모드를 사용 합니다. 이를 설정 하면 `keyboard` 마우스 모드가 꺼집니다. 대신 게임 패드 입력은 DOM 키보드 이벤트를 생성 합니다. 이를 설정 하면 `gamepad` 마우스 모드가 꺼지고 dom 키보드 이벤트가 생성 되지 않으며 dom 또는 WinRT 게임 패드 api만 사용할 수 있습니다.

## <a name="xaml"></a>XAML    
마우스 모드를 해제 하려면 앱에 대 한 생성자에 다음을 추가 합니다.   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C + +/DirectX   
C + +/Va 앱을 작성 하는 경우에는 아무 작업도 수행 하지 않습니다. 마우스 모드는 HTML 및 XAML 응용 프로그램에만 적용 됩니다.

## <a name="see-also"></a>참고 항목
- [Xbox에 대 한 모범 사례](tailoring-for-xbox.md)
- [Xbox One의 UWP](index.md)

