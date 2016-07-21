---
author: payzer
title: "마우스 모드를 사용하지 않도록 설정하는 방법"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 6f4719c98d490cdcac8c799c4c68af55b217cbc5
ms.openlocfilehash: d1ee946693b9f9714b8d570b8ae3718469d2c10d

---

# 마우스 모드를 사용하지 않도록 설정하는 방법
마우스 모드는 기본적으로 모든 응용 프로그램에 대해 설정되어 있습니다. 즉, 옵트아웃(opt out)하지 않은 모든 응용 프로그램은 콘솔의 Edge 브라우저에 있는 것과 유사한 마우스 포인터를 받게 됩니다. 마우스 모드를 끄고 방향 컨트롤러 탐색에 최적화하는 것이 좋습니다.   
   
## HTML   
JavaScript UWP 앱에서 방향 컨트롤러 탐색을 켜려면 [TVHelpers 방향 탐색](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) JavaScript 라이브러리를 사용합니다. 앱 패키지에 방향 탐색 JavaScript 파일을 포함하고 방향 컨트롤러 탐색이 필요한 모든 HTML 페이지에서 다음과 같이 참조를 추가합니다.
```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
자세한 내용은 [방향 탐색 wiki](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation)를 참조하세요.

마우스 모드를 끄는 대신 DOM 또는 WinRT 게임 패드 API를 직접 사용하려는 경우 게임 패드가 필요한 모든 페이지에서 다음을 실행합니다. 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

이 속성은 기본적으로 마우스 모드를 사용할 수 있는 ```'mouse'```로 설정됩니다. 이 속성을 ```'keyboard'```로 설정하면 마우스 모드가 꺼지고 대신에, 게임 패드 입력이 DOM 키보드 이벤트를 생성합니다. 이 속성을 ```'gamepad'```로 설정하면 마우스 모드가 꺼지고 DOM 키보드 이벤트가 생성되지 않으며, 사용자가 DOM 또는 WinRT 게임 패드 API를 사용할 수 있게 됩니다.

## XAML    
마우스 모드를 끄려면 앱의 생성자에 다음을 추가합니다.   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
C++/DirectX 앱을 작성할 경우에는 수행할 작업이 없습니다. 마우스 모드는 HTML 및 XAML 응용 프로그램에만 적용됩니다.



<!--HONumber=Jul16_HO1-->


